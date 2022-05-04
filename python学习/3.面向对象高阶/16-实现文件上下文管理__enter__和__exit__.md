- 我们知道在操作文件对象的时候可以这么写

```python
with open('a.txt') as f:
    '代码块'
```

- 上述叫做上下文管理协议，即with语句，为了让一个对象兼容with语句，必须在这个对象的类中声明\_\_enter\_\_和_\_exit\_\_方法

# 一、上下文管理协议

```python
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')
        # return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')


with Open('a.txt') as f:
    print('=====>执行代码块')
    # print(f,f.name)
出现with语句,对象的__enter__被触发,有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
```

- \_\_exit\_\_()中的三个参数分别代表异常类型，异常值和追溯信息,with语句中代码块出现异常，则with后的代码都无法执行

```python
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')
        print(exc_type)
        print(exc_val)
        print(exc_tb)


try:
    with Open('a.txt') as f:
        print('=====>执行代码块')
        raise AttributeError('***着火啦，救火啊***')
except Exception as e:
    print(e)
出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
<class 'AttributeError'>
***着火啦，救火啊***
<traceback object at 0x1065f1f88>
***着火啦，救火啊***
```

- 如果\_\_exit\_\_()返回值为True,那么异常会被清空，就好像啥都没发生一样，with后的语句正常执行

```python
class Open:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('with中代码块执行完毕时执行我啊')
        print(exc_type)
        print(exc_val)
        print(exc_tb)
        return True


with Open('a.txt') as f:
    print('=====>执行代码块')
    raise AttributeError('***着火啦，救火啊***')
print('0' * 100)  #------------------------------->会执行
出现with语句，对象的__enter__被触发，有返回值则赋值给as声明的变量
=====>执行代码块
with中代码块执行完毕时执行我啊
<class 'AttributeError'>
***着火啦，救火啊***
<traceback object at 0x1062ab048>
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

# 二、模拟open

![113-enter,exit-open.jpg](16-实现文件上下文管理__enter__和__exit__.assets/0081Kckwgy1gm2qwgesmbj30ft0830tg.jpg)

```python
class Open:
    def __init__(self, filepath, mode='r', encoding='utf-8'):
        self.filepath = filepath
        self.mode = mode
        self.encoding = encoding

    def __enter__(self):
        # print('enter')
        self.f = open(self.filepath, mode=self.mode, encoding=self.encoding)
        return self.f

    def __exit__(self, exc_type, exc_val, exc_tb):
        # print('exit')
        self.f.close()
        return True

    def __getattr__(self, item):
        return getattr(self.f, item)


with Open('a.txt', 'w') as f:
    print(f)
    f.write('aaaaaa')
    f.wasdf  #抛出异常，交给__exit__处理
<_io.TextIOWrapper name='a.txt' mode='w' encoding='utf-8'>
```

# 三、优点

1. 使用with语句的目的就是把代码块放入with中执行，with结束后，自动完成清理工作，无须手动干预
2. 在需要管理一些资源比如文件，网络连接和锁的编程环境中，可以在\_\_exit\_\_中定制自动释放资源的机制，你无须再去关系这个问题，这将大有用处