---
created: 2025-07-09
tags:
---
## 我啥时候遇到的

- 在写[[20250707-Backend]]时，需要加unit test
- **Unit testing** is about testing the smallest “units” of your code, like a single function or endpoint, to make sure they behave correctly in isolation.
- We want to test:
	1. What happens if no file is uploaded?
	2. What if the file is uploaded but has no name?
	3. What happens when a valid image is sent?
## 用法

### How pytest Works With Flask

Flask provides a test client. It lets you:
- Call routes without running a real server.
- Simulate requests (GET, POST, etc.).
- Check responses.

Example:
`response = client.post('/run-detect')`

### fixture

```python
@pytest.fixture
%% 表示下面这个函数是个 fixture。pytest 会在测试里识别它。 %%

def client(): %% 定义了一个名字叫 `client` 的 fixture。 %%
    app = create_app()
    app.config['TESTING'] = True
    return app.test_client()
```

- 以上是固定写法，因为**Flask 的单元测试都要用 test_client()**，所以大家都用 fixture 来统一创建 `client`，省得每个测试都重复写。
- fixture: pytest 提供的一种**测试前准备**或**资源管理**的机制，可以：建立数据库连接，创建 Flask 测试客户端，准备测试数据
- fixture 写好后，在你的测试函数里**直接作为参数使用**。比如：
```python
def test_run_detect_no_file(client):
    ...
```
- 这里的 `client` 就是 pytest 自动注入你定义好的 fixture。

### 具体tests写法

```python
def test_run_detect_success(client):
	"""Test successful detection with a sample image"""
	TEST_DIR = os.path.dirname(os.path.abspath(__file__))
	test_img1 = os.path.join(TEST_DIR, 'test_img1.jpeg')
	  
	with open(test_img1, 'rb') as img:
		data = {'file': (img, 'test_img1.jpeg')}
		response = client.post('/run-detect', 
								data=data,
								content_type='multipart/form-data')
	
	assert response.status_code == 200
	data = json.loads(response.data)
	assert 'boxes' in data
	assert isinstance(data['boxes'], list)
	assert len(data['boxes']) > 0 # At least one detection
	
	box = data['boxes'][0]
	assert 'label' in box
	assert 'confidence' in box
	assert 'x' in box
	assert 'y' in box
	assert 'width' in box
	assert 'height' in box
```

在原来的file中需要加这一行：`os.environ['TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD'] = '1'`
### 如何run tests

1. 安装pytest
```bash
pip3 install pytest
```
或者写在你的 `requirements.txt` 里加上`pytest`，然后：
```bash
pip3 install -r requirements.txt
```

2. 运行pytest

重点：
- 测试文件在 `tests/` 下
- 测试文件名是 `test_xxx.py`（pytest 默认会识别）

```bash
python3 -m pytest tests/test_routes.py -v
```

