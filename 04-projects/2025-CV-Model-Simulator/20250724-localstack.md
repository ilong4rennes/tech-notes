---
created: 2025-07-24
tags: 
org:
  - GC
goal: 用local stack跑cv模型
---
目前我收集到的信息有：

目标：实现用local stack跑model
1. 写一个能够跑player detection model的api
2. 需要完成如下功能：
	1. Get model
	2. Run model
	3. Return results
3. 一些具体的要求包括：
	1. 目前所有的model是存储在local stack的s3 bucket里面
	2. 我们需要用到ultralytics来跑model

  

讨论以下这种方案的可行性

1. 有一个s3 client文件，功能如下：
	1. 建立与s3 的连接
	2. Get到那个s3存model的路径
	3. 下载到local stack

2. 在route文件里
	1. 搞到从local stack的路径
	2. 加载ultralytics，跑model

讨论这种方案的可行性，提出可能的疑问

  

模型缓存问题：

- 每次请求都重新下载模型吗？
    

- 模型文件很大，频繁下载会影响性能
    

- 需要一个缓存策略
    

Docker环境问题：

- 模型下载到哪里？容器里还是宿主机？
    

- 容器重启后文件会丢失
    

- 需要考虑持久化存储
    

内存管理：

- YOLO模型加载后占用内存较大
    

- 多个请求同时来会怎样？
    

- 需要考虑模型的生命周期管理
    

错误处理：

- S3连接失败怎么办？
    

- 模型下载失败怎么办？
    

- 模型加载失败怎么办？
    

请从头到尾帮我理清思路，step by step


给了两个方案
1. dockerfile
2. sagemaker endpoint

那我先选第一个。。感觉容易一点hhh

要把flask+yolo代码放进一个docker容器，这个容器启动后就是一个http服务

部署到 AWS EC2 - 一台远程 Linux 虚拟机，就像你的服务器，你可以 ssh 上去，跑 Docker 容器。

- 一个 EC2 实例（Amazon Linux 或 Ubuntu）
- 安装 Docker
- `docker run` 跑你的容器

所以我现在的步骤就是，
先在本地写好file，然后打包上传到docker？
如果是这样的话，file下载到哪里，我还是没懂

现在的testing流程
1. Start the container: `docker-compose up -d`
2. Verify the container started successfully: `docker ps -a`
3. A screenshot of the container being healthy should show up:
[![image](https://private-user-images.githubusercontent.com/5545603/468790703-ce93aae3-4881-428d-b641-5dc4a9aff85c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM0MDk1MTMsIm5iZiI6MTc1MzQwOTIxMywicGF0aCI6Ii81NTQ1NjAzLzQ2ODc5MDcwMy1jZTkzYWFlMy00ODgxLTQyOGQtYjY0MS01ZGM0YTlhZmY4NWMucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDcyNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA3MjVUMDIwNjUzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9YTUyYWFkNWE2NDFhNmVkOTdmMmRhNmMzZjlkZTE4MGRmMzJlYzE0NGZlNjQxNDUwZmRmODUwYzhkM2YyZTIyNCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.yTN7sSZByvaSbCNMA9vKvquLLbc1WcyTTwKbAhH20-Q)](https://private-user-images.githubusercontent.com/5545603/468790703-ce93aae3-4881-428d-b641-5dc4a9aff85c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM0MDk1MTMsIm5iZiI6MTc1MzQwOTIxMywicGF0aCI6Ii81NTQ1NjAzLzQ2ODc5MDcwMy1jZTkzYWFlMy00ODgxLTQyOGQtYjY0MS01ZGM0YTlhZmY4NWMucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDcyNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA3MjVUMDIwNjUzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9YTUyYWFkNWE2NDFhNmVkOTdmMmRhNmMzZjlkZTE4MGRmMzJlYzE0NGZlNjQxNDUwZmRmODUwYzhkM2YyZTIyNCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.yTN7sSZByvaSbCNMA9vKvquLLbc1WcyTTwKbAhH20-Q)
4. Connect to the local docker container: `docker exec -it localstack bash`
5. Verify the bucket exists: `awslocal s3 ls s3://gc-computer-vision-models-qa/server-side/models/sport/baseball/clipping/event_clipping/ --recursive`
6. Start the application: `python run.py`
7. Hit the endpoint:  `curl http://127.0.0.1:5000/healthcheck` and verify the results below
```bash
{
	"s3_connection": "ok"
}
```

下载model是下到哪里去

> 是容器内部的 `/tmp/` 目录


如果我们现在需要写一个class，大概的structure，应该是啥样子的


- 你仍然可以用现在的 Flask API
- 把 app 打包进 Docker
- 镜像里预装好 `ultralytics` 和依赖
- S3 模型仍然从 LocalStack 下载到 `/tmp`，推理后删除
- 用 EC2、ECS 或远程服务器跑 Docker 容器