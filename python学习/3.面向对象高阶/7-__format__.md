# 一、**format**

- 自定制格式化字符串

```python
date_dic = {
    'ymd': '{0.year}:{0.month}:{0.day}',
    'dmy': '{0.day}/{0.month}/{0.year}',
    'mdy': '{0.month}-{0.day}-{0.year}',
}


class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    def __format__(self, format_spec):
        # 默认打印ymd的{0.year}:{0.month}:{0.day}格式
        if not format_spec or format_spec not in date_dic:
            format_spec = 'ymd'
        fmt = date_dic[format_spec]
        return fmt.format(self)


d1 = Date(2016, 12, 29)
print(format(d1))
2016:12:29
print('{:mdy}'.format(d1))
12-29-2016
```

