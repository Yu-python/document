```
class Foo:
    def __init__(self, name):
        self.name = name

    def __getitem__(self, item):
        print('getitem执行', self.__dict__[item])

    def __setitem__(self, key, value):
        print('setitem执行')
        self.__dict__[key] = value
        

    def __delitem__(self, key):
        print('del obj[key]时，delitem执行')
        self.__dict__.pop(key)

    def __delattr__(self, item):
        print('del obj.key时，delattr执行')
        self.__dict__.pop(item)


f1 = Foo('sb')
```

# 一、**setitem**

- 中括号赋值时触发

```
f1['age'] = 18
f1['age1'] = 19
setitem执行
setitem执行
```

# 二、**getitem**

- 中括号取值时触发

```
f1['age']
getitem执行 18
f1['name'] = 'tank'
setitem执行
```

# 三、**delitem**与**delattr**

- **delitem**：中括号删除时触发
- **delattr**：.删除时触发

```
del f1.age1
del f1['age']
del obj.key时，delattr执行
del obj[key]时，delitem执行
print(f1.__dict__)
{'name': 'tank'}
```