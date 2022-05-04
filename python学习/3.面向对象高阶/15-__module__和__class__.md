```python
# flask/aa.py
class C:
    def __init__(self):
        self.name = 'DSB'

# index.py

from flask_app.aa import C

obj = C()
```

# 一、**module**

- **module** 表示当前操作的对象在那个模块

```python
print(obj.__module__)  # 输出 lib.aa，即：输出模块
```

# 二、**class**

- **class**表示当前操作的对象的类是什么

```python
print(obj.__class__)  # 输出 lib.aa.C，即：输出类
```