用 PyTorch 编写 train.py
        ↓
训练数据放到 S3
        ↓
SageMaker 创建 GPU 训练实例
        ↓
在 Docker 容器里运行 train.py
        ↓
PyTorch 做前向传播和反向传播
        ↓
得到训练后的参数
        ↓
保存成 model.pth
        ↓
SageMaker 将模型产物上传到 S3
        ↓
再创建 SageMaker Endpoint
        ↓
客户端通过 API 调用模型