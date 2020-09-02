---
layout: post
---

# Django框架中choice使用
## 原理
choice接收一个元组（保证值不可变），同理每一个选项也是由一个元组（value,display_name）构成。

## 获取displayname
通过属性取value，通过 get_属性_display()方法取display_name。

```python
from django.db import models

SHIRT_SIZES = (
    ('S', 'Small'),
    ('M', 'Medium'),
    ('L', 'Large')
)

class Person(models.Model):
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1,choices=SHIRT_SIZES)
```

查询结果
```python
p = Person(name="Fred Flintstone", shirt_size="L")
p.save()
p.shirt_size  # 'L'
p.get_shirt_size_display()  # 'Large'
```
