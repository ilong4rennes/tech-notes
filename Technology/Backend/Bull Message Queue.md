---
tags:
  - backend
---
## 1. 为什么需要 Queue？

### 高并发处理
当 server 同时涌入大量 request，Queue 作为任务中转站，让 server 保持随时可响应的状态，有效减轻负担。

### 微服务架构
当前端送出一则 response ，后端会透过各个 server 之间互相沟通去完成这个流程，而沟通模式又可分为同步(Sync)与非同步(Async)以使用者送出订单为例

**同步（Sync）**：
![[Screenshot 2026-03-11 at 5.54.05 PM.png]]

**异步（Async）**：
![[Screenshot 2026-03-11 at 5.54.29 PM.png]]

## 2. 通信模式比较

| 模式 | 特性 | 适用场景 | 限制 |
|------|------|----------|------|
| **Request-Response** | 简单直接 | 需要即时响应的操作 | 不支持延迟结果，大数据量性能差 |
| **Pub-Sub** | 广播给多个订阅者 | 事件广播、通知 | 不适合需要追踪特定任务执行状态的场景 |
| **Producer-Consumer** | 异步队列处理 | 延迟计算、定时任务、大量数据处理 | 架构稍复杂 |

**结论**：当需求是「不需立即响应、支持定时、处理大量数据」时，**Producer-Consumer** 是最佳选择。

## 3. Bull Queue 核心架构

### 组成元素

```
Producer → Queue (Redis) → Consumer
                ↓
         Event Listener
```

| 元素                    | 说明                                                 |
| --------------------- | -------------------------------------------------- |
| **Redis**             | 存储 Queue 数据的 key-value 数据库，存放所有 metadata、job 数据与状态 |
| **Job**               | Queue 中的基本消息单位，可携带 JSON 等数据                        |
| **Queue**             | Redis 中存放 Job 的容器，支持 FIFO（先进先出）与 LIFO（后进先出）        |
| **Producer**          | 发送 Job 的 instance                                  |
| **Consumer / Worker** | 接收并执行 Job 的 instance                               |
| **Event Listener**    | 监听 Job 和 Queue 状态变化的 instance                      |
![[Screenshot 2026-03-11 at 6.21.13 PM.png]]
### 任务（Job）生命周期
![[Screenshot 2026-03-11 at 6.28.18 PM.png]]

- **Wait**：等待执行（需 Producer 主动设定）
- **Delayed**：延迟执行（需 Producer 主动设定）
- **Active**：正在执行中
- **Completed**：执行成功
- **Failed**：执行失败

## 4. NestJS + BullMQ + Redis 实现

### 4.1 安装依赖

```bash
npm install @nestjs/bullmq bullmq redis @bull-board/nestjs @nestjs/config
```

### 4.2 环境变量 `.env`

```env
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=yourpassword
```

### 4.3 Queue Module 配置

```typescript
import { Module } from '@nestjs/common';
import { BullModule } from '@nestjs/bullmq';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { ExpressAdapter } from '@bull-board/express';

@Module({
  imports: [
    ConfigModule.forRoot({ cache: true, isGlobal: true }),
    // 连接 Redis
    BullModule.forRootAsync({
      useFactory: async (configService: ConfigService) => ({
        connection: {
          host: configService.get('REDIS_HOST'),
          port: configService.get('REDIS_PORT'),
          password: configService.get('REDIS_PASSWORD'),
        },
      }),
      inject: [ConfigService],
    }),
    // 注册 Queue
    BullModule.registerQueue(...QueueList),
    // Bull Board 监控 UI
    BullBoardModule.forRoot({ route: '/queues', adapter: ExpressAdapter }),
    BullBoardModule.forFeature(...QueueListBoard),
  ],
  providers: [QueueProducerService, QueueConsumerService],
  exports: [QueueProducerService],
})
export class QueueModule {}
```

### 4.4 Producer Service

负责将 Job 加入 Queue：

```typescript
import { Injectable } from '@nestjs/common';
import { InjectQueue } from '@nestjs/bullmq';
import { Queue } from 'bullmq';

@Injectable()
export class QueueProducerService {
  constructor(@InjectQueue('exampleQueue') private readonly queue: Queue) {}

  async addJob(data: any) {
    await this.queue.add('exampleJob', data, {
      attempts: 3,   // 重试次数
      backoff: 5000, // 重试间隔（ms）
    });
  }
}
```

### 4.5 Consumer Service

负责从 Queue 取出 Job 并执行：

```typescript
import { Processor, WorkerHost } from '@nestjs/bullmq';
import { Job } from 'bullmq';

@Processor('exampleQueue')
export class QueueConsumerService extends WorkerHost {
  async process(job: Job<any>): Promise<any> {
    console.log(`Processing job ${job.id}:`, job.data);
    try {
      // 执行任务逻辑
    } catch (error) {
      throw error; // 抛出错误让 BullMQ 触发重试
    }
  }
}
```

### 4.6 Controller 触发添加 Job

```typescript
import { Controller, Get } from '@nestjs/common';
import { QueueProducerService } from './queue-producer.service';

@Controller('jobs')
export class JobsController {
  constructor(private readonly producerService: QueueProducerService) {}

  @Get('add')
  async addJob() {
    const data = { name: 'BullMQ job', priority: 'high' };
    await this.producerService.addJob(data);
    return 'Job added successfully!';
  }
}
```

### 4.7 控制并发数（Concurrency）

```typescript
import { Worker, Job } from 'bullmq';

const worker = new Worker(
  queueName,
  async (job: Job) => {
    // 处理 job
    return 'result';
  },
  { concurrency: 50 } // 同时处理 50 个 job
);
```

## 5. 设计决策：多 Queue vs 单一 Queue

当有多个独立的数据处理任务时，建议为每个任务建立**独立的 Queue**：

**优点：**
- **调试方便**：可隔离问题到特定 Queue，快速定位数据异常
- **失败监控**：各 Queue 的失败 Job 独立追踪，方便管理
- **弹性扩展**：可针对各 Queue 的负载独立调整资源与 concurrency

**示例**：供应商绩效评估系统有 8 个评估参数，各建一个 Queue，独立处理、独立监控。

## 6. 常见挑战与解法

| 挑战 | 解法 |
|------|------|
| Producer 产出速度 > Consumer 处理速度 | 调整 `concurrency` 参数、增加 worker 数量 |
| 服务耦合造成瓶颈 | 使用 Queue 解耦服务，各服务独立扩展 |
| Job 执行失败 | 设定 `attempts` 重试次数 + `backoff` 重试间隔 |
| Server 过载 | 通过 concurrency 限制同时处理量；后续可考虑水平扩展 |
| Redis 重启数据丢失 | 配置 Redis 持久化（AOF / RDB），确保数据不丢失 |

## 7. 监控：Bull Board UI

- 路径：`/queues`（依配置而定）
- 功能：
  - 查看所有 Queue 及其 Job 数量
  - 追踪 Job 状态（Active / Wait / Delayed / Completed / Failed）
  - 查看错误日志
  - 直接从 UI 重跑失败的 Job

## 参考资源

- [BullMQ 官方文档](https://docs.bullmq.io/)
- [NestJS BullMQ 集成](https://docs.nestjs.com/techniques/queues)
- [Bull Board GitHub](https://github.com/felixmosh/bull-board)
