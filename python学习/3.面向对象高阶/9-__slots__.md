# 一、什么是\_\_slots\_\_

- **slots**是一个类变量，变量值可以是列表，元祖，或者可迭代对象，也可以是一个字符串(意味着所有实例只有一个数据属性)
- 使用点来访问属性本质就是在访问类或者对象的**dict**属性字典(类的字典是共享的，而每个实例的是独立的)

![107-slots-内存.jpg](9-__slots__.assets/0081Kckwgy1gm2qsa5w7hj30f807zt8r.jpg)

# 二、为什么用\_\_slots\_\_

- 字典会占用大量内存，如果你有一个属性很少的类，但是有很多实例，为了节省内存可以使用\_\_slots\_\_取代实例的**dict**

- 当你定义\_\_slots\_\_后，\_\_slots\_\_就会为实例使用一种更加紧凑的内部表示。实例通过一个很小的固定大小的数组来构建，而不是为每个实例定义一个字典，这跟元组或列表很类似。在\_\_slots\_\_中列出的属性名在内部被映射到这个数组的指定小标上。使用\_\_slots\_\_一个不好的地方就是我们不能再给实例添加新的属性了，只能使用在\_\_slots\_\_中定义的那些属性名。

  ```
  class Foo:
      __slots__='x'
  
  
  f1=Foo()
  f1.x=1
  f1.y=2#报错
  print(f1.__slots__) #f1不再有__dict__
  
  class Bar:
      __slots__=['x','y']
      
  n=Bar()
  n.x,n.y=1,2
  n.z=3#报错
  ```

- 注意：\_\_slots\_\_的很多特性都依赖于普通的基于字典的实现。另外，定义了\_\_slots\_\_后的类不再 支持一些普通类特性了，比如多继承。大多数情况下，你应该只在那些经常被使用到 的用作数据结构的类上定义\_\_slots\_\_比如在程序中需要创建某个类的几百万个实例对象 。

- 关于\_\_slots\_\_的一个常见误区是它可以作为一个封装工具来防止用户给实例增加新的属性。尽管使用\_\_slots\_\_可以达到这样的目的，但是这个并不是它的初衷。它更多的是用来作为一个内存优化工具。

# 三、刨根问底

![107-slots-刨根问底.jpg](9-__slots__.assets/0081Kckwgy1gm2qsqvvbaj30dv0bbq33.jpg)

```
class Foo:
    __slots__=['name','age']

f1=Foo()
f1.name='alex'
f1.age=18
print(f1.__slots__)
['name', 'age']

f2=Foo()
f2.name='egon'
f2.age=19
print(f2.__slots__)
['name', 'age']
```

- f1与f2都没有属性字典**dict**了，统一归\_\_slots\_\_管，节省内存

```
print(Foo.__dict__)
{'__module__': '__main__', '__slots__': ['name', 'age'], 'age': <member 'age' of 'Foo' objects>, 'name': <member 'name' of 'Foo' objects>, '__doc__': None}
```

