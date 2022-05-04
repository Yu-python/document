# 一、**str**

- 打印时触发

```python
class Foo:
    pass


obj = Foo()
print(obj)
<__main__.Foo object at 0x10d2b8f98>

dic = {'a': 1}  # d = dict({'x':1})
print(dic)
{'a': 1}
```

- obj和dic都是实例化的对象，但是obj打印的是内存地址，而dic打印的是有用的信息，很明显dic的打印是非常好的

```python
class Foo:
    def __init__(self, name, age):
        """对象实例化的时候自动触发"""
        self.name = name
        self.age = age

    def __str__(self):
        print('打印的时候自动触发，但是其实不需要print即可打印')
        return f'{self.name}:{self.age}'  # 如果不返回字符串类型，则会报错


obj = Foo('nick', 18)
print(obj)  # obj.__str__() # 打印的时候就是在打印返回值
打印的时候自动触发，但是其实不需要print即可打印
nick:18
obj2 = Foo('tank', 30)
print(obj2)
打印的时候自动触发，但是其实不需要print即可打印
tank:30
```

# 二、**repr**

- str函数或者print函数—>obj.**str**()
- repr或者交互式解释器—>obj.**repr**()
- 如果**str**没有被定义，那么就会使用**repr**来代替输出
- 注意：这俩方法的返回值必须是字符串，否则抛出异常

```python
class School:
    def __init__(self, name, addr, type):
        self.name = name
        self.addr = addr
        self.type = type

    def __repr__(self):
        return 'School(%s,%s)' % (self.name, self.addr)

    def __str__(self):
        return '(%s,%s)' % (self.name, self.addr)


s1 = School('oldboy1', '北京', '私立')
print('from repr: ', repr(s1))
from repr:  School(oldboy1,北京)
print('from str: ', str(s1))
from str:  (oldboy1,北京)
print(s1)
(oldboy1,北京)
s1  # jupyter属于交互式
School(oldboy1,北京)
```