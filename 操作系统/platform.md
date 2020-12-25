# python 标准库 platform

```python
import platform
#Windows will be : Windows
#Linux will be : Linux
platform.system()
# 代码示例
system = platform.system()
engine = None
if system == "Linux":
    # 这样的路径只适用于linux
    engine = create_engine(f'sqlite:////{dbpath}')
elif system == "Windows":
    engine = create_engine()
else:
    raise SystemError('仅支持Linux系统或Windows系统')
```
