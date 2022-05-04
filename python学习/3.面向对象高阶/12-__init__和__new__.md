曾经我幼稚的以为认识了python的\_\_init\_\_()方法就相当于认识了类构造器，结果，\_\_new\_\_()方法突然出现在我眼前，让我突然认识到原来\_\_new\_\_才是老大。为什么这么说呢？

我们首先得从\_\_new\_\_(cls[,…])的参数说说起，\_\_new\_\_方法的第一个参数是这个类，而其余的参数会在调用成功后全部传递给\_\_init\_\_方法初始化，这一下子就看出了谁是老子谁是小子的关系。

所以，\_\_new\_\_方法（第一个执行）先于\_\_init\_\_方法执行：

```python
class A:
    pass


class B(A):
    def __new__(cls):
        print("__new__方法被执行")
        return super().__new__(cls)

    def __init__(self):
        print("__init__方法被执行")


b = B()
__new__方法被执行
__init__方法被执行
```

我们比较两个方法的参数，可以发现\_\_new\_\_方法是传入类(cls)，而\_\_init\_\_方法传入类的实例化对象(self)，而有意思的是，\_\_new\_\_方法返回的值就是一个实例化对象（ps:如果\_\_new\_\_方法返回None，则\_\_init\_\_方法不会被执行，并且返回值只能调用父类中的\_\_new\_\_方法，而不能调用毫无关系的类的\_\_new\_\_方法）。我们可以这么理解它们之间的关系，\_\_new\_\_是开辟疆域的大将军，而\_\_init\_\_是在这片疆域上辛勤劳作的小老百姓，只有\_\_new\_\_执行完后，开辟好疆域后，\_\_init\_\_才能工作。

绝大多数情况下，我们都不需要自己重写\_\_new\_\_方法，但在当继承一个不可变的类型（例如str类,int类等）时，它的特性就尤显重要了。我们举下面这个例子：

```python
class CapStr(str):
    def __init__(self, string):
        string = string.upper()


a = CapStr("I love China!")
print(a)
I love China!
class CapStr(str):
    def __new__(cls, string):
        string = string.upper()
        return super().__new__(cls, string)


a = CapStr("I love China!")
print(a)
I LOVE CHINA!
```

我们可以根据上面的理论可以这样分析，我们知道字符串是不可改变的，所以第一个例子中，传入的字符串相当于已经被打下的疆域，而这块疆域除了将军其他谁也无法改变，\_\_init\_\_只能在这块领地上干瞪眼，此时这块疆域就是”I love China!“。而第二个例子中，\_\_new\_\_大将军重新去开辟了一块疆域，所以疆域上的内容也发生了变化，此时这块疆域变成了”I LOVE CHINA!“。

# 一、小结

小结：\_\_new\_\_和\_\_init\_\_想配合才是python中真正的类构造器。