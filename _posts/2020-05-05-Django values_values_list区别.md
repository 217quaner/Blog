# Django values/values_list区别
## values(*fields)
返回QuerySet类型，迭代时返回字典。

### fields
**类型**
str

**输入**
1. 限制字段，类比于`SELECT name，age FROM table`，返回值的键为输入字段，值为记录的字典。
2. 空，类比于`SELECT * FROM table`，返回值的键为所有字段，值为记录的字典。

## values_list(*fields, flat=False)
返回QuerySet类型，迭代时返回的是元组。

### fields
**类型**
str

**输入**
1. 限制字段，类比于`SELECT name，age FROM table`，返回值的键为输入字段，值为记录的元组。
2. 空，类比于`SELECT * FROM table`，返回值的键为所有字段，值为记录的元组。

**flat作用**
默认为False，返回时为元组，当为True时，返回为列表。
