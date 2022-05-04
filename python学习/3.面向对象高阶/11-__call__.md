# 一、**call**

- 对象后面加括号时，触发执行。
- 注：构造方法的执行是由创建对象触发的，即：对象 = 类名() ；而对于 \_\_call\_\_方法的执行是由对象后加括号触发的，即：对象() 或者 类()()

```python
class Foo:
    def __init__(self):
        print('__init__触发了')

    def __call__(self, *args, **kwargs):

        print('__call__触发了')


obj = Foo()  # 执行 __init__
__init__触发了
obj()  # 执行 __call__
__call__
```