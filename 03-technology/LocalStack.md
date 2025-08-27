---
created: 2025-07-15
tags:
---
## 我啥时候遇到的

I encountered the need for LocalStack when developing the computer vision model simulator project that relies on AWS services (S3 and SQS). During development, I needed to test AWS interactions locally without incurring costs or requiring internet connectivity, and without risking accidental interactions with production AWS resources.

## 用法

想象你在开发一个需要用 AWS 的项目，但是：
1. AWS 要花钱
2. 测试的时候可能会搞坏线上环境
3. 没网的时候没法开发

所以我们用 LocalStack，它就是个"假 AWS"：

**整个系统是这样工作的：**
1. 你在本地电脑上用 Docker 跑一个 LocalStack
   - 相当于在你电脑上开了个"迷你 AWS"
   - 完全免费，随便你怎么折腾

2. 这个"迷你 AWS"提供了两个服务：
   - S3：就是个网盘，存文件用的
   - SQS：就是个消息队列，类似一个待办事项清单

3. 我们的代码流程是这样的：
   ```
   用户上传文件 → 存到S3 → 发个消息到SQS说"有新文件要处理" 
   → 后面的服务看到消息 → 处理文件 → 结果再存回S3
   ```

4. 最妙的是：
   - 开发的时候，所有文件和消息都存在你自己电脑上
   - 上线的时候，只要改个配置，同样的代码就能连真实的 AWS
   - 完全不用改代码，开发和生产环境无缝切换

**具体怎么用：**

1. 首先启动这个"迷你 AWS"：
   ```bash
   docker-compose up -d
   ```
   这时候你电脑上就有了个假的 AWS 服务

2. 然后初始化一下：
   ```bash
   python init_localstack.py
   ```
   这步相当于在你的"迷你 AWS"里创建需要的东西（S3桶和消息队列）

3. 最后启动你的 Flask 应用：
   ```bash
   export FLASK_DEBUG=1  # 这个告诉程序"用本地的假AWS"
   flask run
   ```

**代码里最重要的改动：**
- 加了个判断：如果是开发模式，就连本地的 LocalStack
- 如果是生产环境，就连真实的 AWS
- 其他代码完全不用改，该怎么写怎么写

这样你就可以：
- 在本地随便测试，不用担心费用
- 不用网络也能开发
- 不用担心搞坏线上环境
- 代码写好了，直接就能用在真实环境

简单来说，LocalStack 就是让你在本地开发的时候可以假装有个 AWS，等代码写好了，直接换个配置就能用真的 AWS，特别方便测试和开发。

**How to run?**
```bash
# 1. 先进入backend目录
cd backend

# 2. 激活虚拟环境
source venv/bin/activate

# 3. 设置debug模式
export FLASK_DEBUG=1

# 4. 现在再运行flask
flask run
```

   1. Start LocalStack (docker-compose up)
   2. Run init script (creates bucket & queue)
   3. Start Flask app
   4. Upload file → goes to local S3
   5. Message added to local SQS
   6. Check status → looks in local S3
