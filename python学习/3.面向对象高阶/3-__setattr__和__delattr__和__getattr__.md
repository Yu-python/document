```
class Foo:
    x = 2

    def __init__(self, y):
        self.y = y

    def __getattr__(self, item):
        print('----> from getattr:你找的属性不存在')

    def __setattr__(self, key, value):
        print('----> from setattr')
        # self.key = value  # 这就无限递归了,你好好想想
        # self.__dict__[key] = value  # 应该使用它

    def __delattr__(self, item):
        print('----> from delattr')
        # del self.item  # 无限递归了
        self.__dict__.pop(item)


f1 = Foo(10)
```

# 一、\_\_setattr\_\_

- 添加/修改属性会触发它的执行

```python
print(f1.__dict__
      )  # 因为你重写了__setattr__，凡是赋值操作都会触发它的运行，你啥都没写，就是根本没赋值，除非你直接操作属性字典，否则永远无法赋值
f1.z = 3
print(f1.__dict__)
```

# 二、\_\_delattr\_\_

- 删除属性的时候会触发

```python
f1.__dict__['aa'] = 3  # 我们可以直接修改属性字典，来完成添加/修改属性的操作
del f1.aa
print(f1.__dict__)

----> from delattr
{}
```

# 三、 \_\_getattr\_\_

- 只有在使用点调用属性且属性不存在的时候才会触发

```
f1.xx
----> from getattr:你找的属性不存在
```





*备注*：如果是有\_\_getattribute\_\_优先调用此方法。。。。不过容易递归报错