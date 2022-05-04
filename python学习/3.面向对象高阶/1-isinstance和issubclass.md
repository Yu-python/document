# 一、isinstance与type

在游戏项目中，我们会在每个接口验证客户端传过来的参数类型，如果验证不通过，返回给客户端“参数错误”错误码。

这样做不但便于调试，而且增加健壮性。因为客户端是可以作弊的，不要轻易相信客户端传过来的参数。

验证类型用type函数，非常好用，比如

```python
print(type('foo') == str)
True
print(type(2.3) in (int, float))
True
```

既然有了type()来判断类型，为什么还有isinstance()呢？

一个明显的区别是在判断子类。

type()不会认为子类是一种父类类型；isinstance()会认为子类是一种父类类型。

千言不如一码。

```python
class Foo(object):
    pass
 
class Bar(Foo):
    pass
 
print(type(Foo()) == Foo)
True
print(type(Bar()) == Foo)
False
# isinstance参数为对象和类
print(isinstance(Bar(),Foo))
True
```

需要注意的是，旧式类跟新式类的type()结果是不一样的。旧式类都是<type ‘instance’>。

```python
# python2.+
class A:
    pass
 
class B:
    pass
 
class C(object):
    pass

print('old style class',type(A()))  # old style class <type 'instance'>

print('old style class',type(B()))  # old style class <type 'instance'>

print('new style class',type(C()))  # new style class <class '__main__.C'>

print(type(A()) == type(B()))  # True
```

注意：**不存在说isinstance比type更好。只有哪个更适合需求。**

# 二、issubclass

判断此类是不是其它类的子类

```python
class Parent:
    pass


class Sub(Parent):
    pass


print(issubclass(Sub, Parent))
True
print(issubclass(Parent, object))
True
```