# django models方法的区别(get、filter、all、values)

## get

1. 代码：`clock_log_models.Clock.objects.get(clock_id=1)`
2. 返回值：`Clock object (1)`
3. 返回类型：`<class 'clock_log.models.Clock'>`
4. 结果：返回一个对象，如果所查询记录不存在或多于两条就会报错
5. 转换json方式

```python
from django.forms.models import model_to_dict
data = model_to_dict(data)
```

## filter

1. 代码：`clock_log_models.Clock.objects.filter(clock_id=1)`
2. 返回值：`<QuerySet [<Clock: Clock object (1)>]>`
3. 返回类型：`<class 'django.db.models.query.QuerySet'>`
4. 结果：返回一个对象列表，如果记录不存在就会返回```<QuerySet []>```
5. 转换json方式

```python
data = list(data.values())
```

## all

1. 代码：`clock_log_models.Clock.objects.all()`
2. 返回值：`<QuerySet [<Clock: Clock object (1)>]>`
3. 返回类型：`<class 'django.db.models.query.QuerySet'>`
4. 结果：返回对象的所有实例
5. 转换json方式

```python
data = list(data.values())
```

## values

1. 代码：`clock_log_models.Clock.objects.values(clock_id=1)`
2. 返回值：`<QuerySet [{'clock_id': 1, 'clock_time': datetime.date(2019, 9, 24), 'on_duty_time': datetime.datetime(2019, 9, 24, 9, 18, 23, 659317, tzinfo=<UTC>), 'on_duty_status': True, 'off_duty_time': datetime.datetime(2019, 9, 24, 9, 18, 23, 659317, tzinfo=<UTC>), 'off_duty_status': True, 'user_info_id': '1234'}]>`
3. 返回类型：`<class 'django.db.models.query.QuerySet'>`
4. 结果：可以指定查询的字段
