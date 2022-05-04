# 一、**getattr**

- 不存在的属性访问，触发\_\_getattr\_\_

```python
class Foo:
    def __init__(self, x):
        self.x = x

    def __getattr__(self, item):
        print('执行的是我')
        # return self.__dict__[item]


f1 = Foo(10)
print(f1.x)
10
f1.xxxxxx
执行的是我
```

# 二、\_\_getattribute\_\_

- 查找属性无论是否存在，都会执行

![103-getattribute-霸道.jpg](4-__getattribute__.assets/0081Kckwgy1gm2qjnmektj30ku0jajsr.jpg)

- 你可真霸道呀！！！

```python
class Foo:
    def __init__(self, x):
        self.x = x

    def __getattribute__(self, item):
        print('不管是否存在，我都会执行')


f1 = Foo(10)
f1.x
不管是否存在，我都会执行
f1.xxxxxx
不管是否存在，我都会执行
```

# 三、**getattr**与**getattribute**

- 当**getattribute**与**getattr**同时存在，只会执行**getattrbute**，除非**getattribute**在执行过程中抛出异常AttributeError

```python
#_*_coding:utf-8_*_
__author__ = 'Linhaifeng'


class Foo:
    def __init__(self, x):
        self.x = x

    def __getattr__(self, item):
        print('执行的是我')
        # return self.__dict__[item]
    def __getattribute__(self, item):
        print('不管是否存在,我都会执行')
        raise AttributeError('哈哈')


f1 = Foo(10)
f1.x
不管是否存在,我都会执行
执行的是我
f1.xxxxxx
不管是否存在,我都会执行
执行的是我
```

