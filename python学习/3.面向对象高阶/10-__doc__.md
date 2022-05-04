# 一、**doc**

- 返回类的注释信息

```python
class Foo:
    '我是描述信息'
    pass


print(Foo.__doc__)
我是描述信息
```

- 该属性无法被继承

```python
class Foo:
    '我是描述信息'
    pass

class Bar(Foo):
    pass
print(Bar.__doc__) #该属性无法继承给子类
None
```