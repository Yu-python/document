# 一、简单示例

- 死循环

```python
class Foo:
    def __init__(self, x):
        self.x = x

    def __iter__(self):
        return self

    def __next__(self):
        self.x += 1
        return self.x


f = Foo(3)
for i in f:
    print(i)
```

# 二、StopIteration异常版

- 加上StopIteration异常

```python
class Foo:
    def __init__(self, start, stop):
        self.num = start
        self.stop = stop

    def __iter__(self):
        return self

    def __next__(self):
        if self.num >= self.stop:
            raise StopIteration
        n = self.num
        self.num += 1
        return n


f = Foo(1, 5)
from collections import Iterable, Iterator
print(isinstance(f, Iterator))
True
for i in Foo(1, 5):
    print(i)
1
2
3
4
```

# 三、模拟range

```python
class Range:
    def __init__(self, n, stop, step):
        self.n = n
        self.stop = stop
        self.step = step

    def __next__(self):
        if self.n >= self.stop:
            raise StopIteration
        x = self.n
        self.n += self.step
        return x

    def __iter__(self):
        return self


for i in Range(1, 7, 3):
    print(i)
1
4
```

# 四、斐波那契数列

```python
class Fib:
    def __init__(self):
        self._a = 0
        self._b = 1

    def __iter__(self):
        return self

    def __next__(self):
        self._a, self._b = self._b, self._a + self._b
        return self._a


f1 = Fib()
for i in f1:
    if i > 100:
        break
    print('%s ' % i, end='')
    
1 1 2 3 5 8 13 21 34 55 89
```

