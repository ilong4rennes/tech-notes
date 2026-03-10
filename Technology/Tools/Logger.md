---
created: 2025-07-25
tags:
---
## 我啥时候遇到的

在[[20250724-localstack|解决这个问题]]的时候，需要debug，在vibe code的时候我让cursor加一些debug statement，它用了这个library。

## 它解决了什么问题

为什么不用print？因为不像 `print()`，它是为生产环境设计的，更灵活、专业、结构化。

## 我啥时候用得上



## 一句话用法

### 1. 初始化 logger

在文件开头加：

```python
import logging

logging.basicConfig(level=logging.DEBUG)  # 设置默认日志等级
logger = logging.getLogger(__name__)      # 获取当前模块名的 logger
```

### 2. 打日志

| 等级                       | 用法           | 用途             |
| ------------------------ | ------------ | -------------- |
| `logger.debug("...")`    | 详细调试信息       | 开发时查看内部变量、流程   |
| `logger.info("...")`     | 一般运行信息       | 正常行为的记录（如启动成功） |
| `logger.warning("...")`  | 非致命的警告       | 可能出现问题         |
| `logger.error("...")`    | 错误信息（但程序还能跑） | 捕获的异常、数据不合法    |
| `logger.critical("...")` | 严重错误（系统快挂了）  | 比如数据库炸了        |

### 3. 示例

```python
# ❌ 不推荐
# print("No file provided")

# ✅ 推荐
logger.error("No file provided")
```

### 4. 打印异常 + 堆栈（你也用到了）

```python
except Exception as e:
    logger.error(f"Error: {str(e)}", exc_info=True)
```

- `exc_info=True` 会自动把 traceback 打出来
    
- 非常适合你在 except 块中调试问题时用
    

### 总结

```python
logger.debug("变量值：", x)
logger.error("模型下载失败", exc_info=True)
```

比 `print()` 高级一百倍！要不要我写个日志输出到文件的版本？