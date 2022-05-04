# 一、反射在类中的使用

反射就是通过字符串来操作类或者对象的属性

- 反射本质就是在使用内置函数，其中反射有以下四个内置函数：
  - hasattr：判断一个属性是否存在与这个类中
  2. getattr：根据字符串去获取obj对象里的对应的属性的内存地址，如果是可执行对象加"()"括号即可执行
  3. setattr：通过setattr将外部的一个属性绑定到实例中
4. delattr：删除一个实例或者类中的属性

```python

class People:
    country = 'China'

    def __init__(self, name):
        self.name = name

    def eat(self):
        print('%s is eating' % self.name)


p1 = People('llnb')
print(hasattr(p1, 'eat'))  # p1.eat
True
print(getattr(p1, 'eat'))  # p1.eat
<bound method People.eat of <__main__.People object at 0x1043b9f98>>
print(getattr(p1, 'xxxxx', None))
None
setattr(p1, 'age', 18)  # p1.age=18
print(p1.age)
18
print(p1.__dict__)
{'name': 'egon', 'age': 18}
delattr(p1, 'name')  # del p1.name
print(p1.__dict__)
{'age': 18}
```

## 1.1 应用

需求：通过用户输入命令启动功能

```python
class Ftp:
    def __init__(self, ip, port):
        self.ip = ip
        self.port = port

    def get(self):
        print('GET function')

    def put(self):
        print('PUT function')

    def run(self):
        while True:
            choice = input('>>>: ').strip()

            if choice == 'q':
                print('break')
                break


#             print(choice, type(choice))
#             if hasattr(self, choice):
#                 method = getattr(self, choice)
#                 method()
#             else:
#                 print('输入的命令不存在')

            method = getattr(self, choice, None)

            if method is None:
                print('输入的命令不存在')
            else:
                method()
conn = Ftp('1.1.1.1', 23)
conn.run()
>>>: time
输入的命令不存在
>>>: time
输入的命令不存在
>>>: q
break
```

# 二、反射在模块中的使用

## 2.1 前言，记得测试时在同一目录下，不同文件中。。。

```python
# test.py
def f1():
    print('f1')


def f2():
    print('f2')


def f3():
    print('f3')


def f4():
    print('f4')


a = 1
import test as ss

ss.f1()
ss.f2()
print(ss.a)
```

我们要导入另外一个模块,可以使用import.现在有这样的需求,我动态输入一个模块名，可以随时访问到导入模块中的方法或者变量，怎么做呢？

```python
imp = input(“请输入你想导入的模块名:”)
CC = __import__(imp) 這种方式就是通过输入字符串导入你所想导入的模块 
CC.f1()  # 执行模块中的f1方法
```

上面我们实现了动态输入模块名，从而使我们能够输入模块名并且执行里面的函数。但是上面有一个缺点，那就是执行的函数被固定了。那么，我们能不能改进一下，动态输入函数名，并且来执行呢？

```python
# dynamic.py
imp = input("请输入模块:")
dd = __import__(imp)
# 等价于import imp
inp_func = input("请输入要执行的函数：")

f = getattr(dd, inp_func,
            None)  # 作用:从导入模块中找到你需要调用的函数inp_func,然后返回一个该函数的引用.没有找到就烦会None

f()  # 执行该函数
请输入模块:time
请输入要执行的函数：time

1560959528.6127071
```

上面我们就实现了，动态导入一个模块，并且动态输入函数名然后执行相应功能。

当然，上面还存在一点点小问题:那就是我的模块名有可能不是在本级目录中存放着。有可能是如下图存放方式：

```python
|- learn
    |- lib
        |- common.py
        
    |-test.py
```

 

那么这种方式我们该如何搞定呢?看下面代码:

```python
dd = __import__("lib.commons")  # 这样仅仅导入了lib模块
dd = __import__("lib.commons", fromlist=True)  # 改用这种方式就能导入成功
# 等价于import config
inp_func = input("请输入要执行的函数：")
f = getattr(dd, inp_func)
f()
```

## 2.2 反射机制

上面说了那么多，到底什么是反射机制呢?

其实，反射就是通过字符串的形式，导入模块；通过字符串的形式，去模块寻找指定函数，并执行。利用字符串的形式去对象（模块）中操作（查找/获取/删除/添加）成员，一种基于字符串的事件驱动！

先来介绍四个内置函数:

### 2.2.1 getattr()

getattr()函数是Python自省的核心函数，具体使用大体如下：

```python
class A:
    def __init__(self):
        self.name = 'llnb'
        # self.age='18'

    def method(self):
        print("method print")


a = A()

print(getattr(a, 'name',
              'not find'))  # 如果a 对象中有属性name则打印self.name的值，否则打印'not find'
print(getattr(a, 'age',
              'not find'))  # 如果a 对象中有属性age则打印self.age的值，否则打印'not find'
print(getattr(a, 'method', 'default'))  # 如果有方法method，否则打印其地址，否则打印default
print(getattr(a, 'method', 'default')())  # 如果有方法method，运行函数并打印None否则打印default
```

### 2.2.2 hasattr(object, name)

说明：判断对象object是否包含名为name的特性（hasattr是通过调用getattr(ojbect, name)是否抛出异常来实现的）

### 2.2.3 setattr(object, name, value)

这是相对应的getattr()。参数是一个对象,一个字符串和一个任意值。字符串可能会列出一个现有的属性或一个新的属性。这个函数将值赋给属性的。该对象允许它提供。例如,setattr(x,“foobar”,123)相当于x.foobar = 123。

### 2.2.4 delattr(object, name)

与setattr()相关的一组函数。参数是由一个对象(记住python中一切皆是对象)和一个字符串组成的。string参数必须是对象属性名之一。该函数删除该obj的一个由string指定的属性。delattr(x, ‘foobar’)=del x.foobar

我们可以利用上述的四个函数,来对模块进行一系列操作.

```python
r = hasattr(commons, xxx)  # 判断某个属性是否存在
print(r)

setattr(commons, 'age', 18)  # 给commons模块增加一个全局变量age = 18，创建成功返回none

setattr(commons, 'age', lambda a: a + 1)  # 给模块添加一个函数

delattr(commons, 'age')  # 删除模块中某个变量或者函数
```

注意：**getattr,hasattr,setattr,delattr对模块的修改都在内存中进行，并不会影响文件中真实内容。**

## 2.3 应用

基于反射机制模拟web框架路由

需求：比如我们输入<[www.xxx.com/commons/f2>](http://www.xxx.com/commons/f2>) ，返回f1的结果。

```python
# 动态导入模块，并执行其中函数
url = input("url: ")

target_module, target_func = url.split('/')
m = __import__('lib.' + target_module, fromlist=True)

inp = url.split("/")[-1]  # 分割url,并取出url最后一个字符串
if hasattr(m, target_func):  # 判断在commons模块中是否存在inp这个字符串
    target_func = getattr(m, target_func)  # 获取inp的引用
    target_func()  # 执行
else:
    print("404")
```

[← python/常用模块/8-logging模块](http://liuqingzheng.top/python/常用模块/8-logging模块/)