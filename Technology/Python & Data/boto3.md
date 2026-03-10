---
created: 2025-07-24
tags:
---
## 我啥时候遇到的
- 在[[20250724-localstack|用localstack跑模型]]的时候，需要连接到S3服务。
- `boto3` 是 **AWS提供的官方 Python SDK**，可以让你用 Python 代码直接操作各种 AWS 服务，比如 S3。
## 用法
1. 建立S3连接
```python
import boto3

s3_client = boto3.client(
    's3',
    endpoint_url=...,            # 比如 http://localhost:4566
    aws_access_key_id=...,       # AWS 访问密钥
    aws_secret_access_key=...,   # AWS 密钥
    region_name=...              # 区域，一般用 us-east-1
)
```
- 返回 `s3_client` 对象，之后就能用它下载、上传、列出 bucket 等操作。

2. 下载文件
```python
s3_client.download_file(bucket, key, filename)
```
- `bucket` (str): The name of the bucket to download from.
- `key` (str): The name of the key to download from.
- `filename`(str): The **path** to the file to download to.

3. 上传文件
```python
s3_client.upload_fileobj(file_obj, bucket, key)
```
- `file_obj`(a file-like object): A file-like object to upload.
- `bucket` (str): The name of the bucket to upload to.
- `key` (str): The name of the key to upload to.

4. 测试连接
```python
s3_client.list_buckets()
```
- 不需要参数
- 如果成功返回，说明连接没问题，会以dict形式返回所有的 bucket 列表

`boto3`S3客户端官方API文档：https://boto3.amazonaws.com/v1/documentation/api/1.9.42/reference/services/s3.html?utm_source=chatgpt.com#client 

