---
tags:
  - backend
---
## 什么是 ORM？

传统后端操作数据库需要手写 SQL，有以下缺点：
- 代码难以维护
- 容易写错（空行、引号等）
- 容易遭受 SQL Injection 攻击

ORM（对象关系映射）用 OOP 概念操作数据库，不需要手写 SQL：

```sql
-- SQL 写法
SELECT * FROM TABLE
```

```typescript
// ORM 写法
this.prisma.table.findMany();
```

> 缺点：需要在背后大量转换 SQL，性能比直接操作 SQL 稍慢。

## 什么是 Prisma？

Prisma 是用于 Node.js 和 TypeScript 的**开源 ORM**（Object Relational Mapper），可替代手写 SQL 或其他数据库工具（如 knex.js、TypeORM、Sequelize）。

支持数据库：PostgreSQL、MySQL、SQL Server、SQLite、MongoDB、CockroachDB

## 安装

```bash
npm install prisma @prisma/client
npx prisma init
```

运行完会生成两个东西：

- `prisma/schema.prisma` — 在这里定义数据结构
- `.env` — 在这里填数据库连接地址

## 配置数据库连接

打开 `.env`，填入你的数据库地址：

```env
DATABASE_URL="postgresql://用户名:密码@localhost:5432/数据库名"
```

## 定义数据结构（Schema）

在 `prisma/schema.prisma` 里定义你的表长什么样：

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
  dreams    Dream[]  // 一个用户有多个梦境
}

model Dream {
  id        String   @id @default(uuid())
  content   String
  userId    String
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now())
}
```

## 同步到数据库

定义好 schema 之后，跑这个命令让数据库实际创建这些表：

```bash
npx prisma migrate dev --name init
```

每次改了 schema 就跑一次，Prisma 会自动生成 SQL 并执行。

## 生成客户端代码

```bash
npx prisma generate
```

这一步让 Prisma 根据你的 schema 生成 TypeScript 类型，这样写代码时有自动补全。

## 在代码里使用

### 初始化

一般单独建一个文件 `prisma/prisma.service.ts`（NestJS 项目）或者 `lib/prisma.ts`（普通项目）：

```ts
// lib/prisma.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();
export default prisma;
```

### 查询

```ts
// 查单个（找不到返回 null）
const user = await prisma.user.findUnique({
  where: { id: '123' }
});

// 查单个（找不到直接报错）
const user = await prisma.user.findUniqueOrThrow({
  where: { id: '123' }
});

// 查多个
const users = await prisma.user.findMany();

// 带条件查多个
const dreams = await prisma.dream.findMany({
  where: { userId: '123' }
});

// 关联查询（查用户同时带出他的梦境）
const user = await prisma.user.findUnique({
  where: { id: '123' },
  include: { dreams: true }
});
```

### 创建

```ts
const user = await prisma.user.create({
  data: {
    email: 'clem@cmu.edu',
    name: 'Clem',
  }
});
```

### 更新

```ts
const user = await prisma.user.update({
  where: { id: '123' },
  data: { name: '新名字' }
});
```

### 删除

```ts
await prisma.user.delete({
  where: { id: '123' }
});
```

### 计数

```ts
const count = await prisma.dream.count({
  where: { userId: '123' }
});
```

## 在 NestJS 里的标准用法

NestJS 项目里一般把 Prisma 封装成 Service：

```ts
// prisma/prisma.service.ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  async onModuleInit() {
    await this.$connect(); // 启动时连接数据库
  }
}
```

然后在其他 Service 里直接注入使用：

```ts
@Injectable()
export class AnalysisService {
  constructor(private readonly prisma: PrismaService) {}

  async getDream(id: string) {
    return this.prisma.dream.findUnique({ where: { id } });
  }
}
```

## 常用命令速查

|命令|干啥的|
|---|---|
|`npx prisma init`|初始化，生成 schema 文件|
|`npx prisma migrate dev`|同步 schema 到数据库（开发用）|
|`npx prisma generate`|生成 TypeScript 类型|
|`npx prisma studio`|打开可视化数据库管理界面|
|`npx prisma db push`|直接推送 schema，不生成迁移文件（快速原型用）|

## 没有 Prisma 的替代方案

|工具|特点|
|---|---|
|手写 SQL|最原始，完全控制，但麻烦|
|TypeORM|NestJS 最常见替代品，功能全|
|Drizzle|最近很火，轻量，类型安全|
|Sequelize|老牌 ORM，生态成熟|

