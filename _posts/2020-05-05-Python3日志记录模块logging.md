# Python3日志记录模块logging

## 日志级别
|日志等级|等级描述|
|-|-|
|DEBUG|最详细的日志信息，典型应用场景是问题诊断|
|INFO|信息详细程度仅次于DEBUG，通常只记录关键节点信息，用于确认一切都是按照我们预期的那样进行工作|
|WARNING|当某些不期望的事情发生时记录的信息（如，磁盘可用空间较低），但是此时应用程序还是正常运行的|
|ERROR|由于一个更严重的问题导致某些功能不能正常运行时记录的信息|
|CRITICAL|当发生严重错误，导致应用程序不能继续运行时记录的信息|

## 日志等级排名
DEBUG < INFO < WARNING < ERROR < CRITICAL

## 日志常用方式
```python
logging.debug(msg, *args, **kwargs)
logging.info(msg, *args, **kwargs)
logging.warning(msg, *args, **kwargs)
logging.error(msg, *args, **kwargs)
logging.critical(msg, *args, **kwargs)
```

## logging.basicconfig() 函数

> 用来修改日志的输出格式和方式。

### 可选参数

|可选参数|参数意义|
|-|-|
|filename|指定日志文件名
|filemode|指定日志文件打开的模式，w 或 a|
|level|指定日志级别，默认 logging.WARNING|
|format|指定输出的格式和内容，format 的参考信息如下|
|datefmt|使用指定的时间格式，format 参数中有 asctime 的话，需要使用 datefmt 指定格式|

### 可选参数format格式
|格式|意义|
|-|-|
|%(levelno)s|打印日志级别的数值|
|%(levelname)s|打印日志级别名称|
|%(pathname)s|打印当前执行程序的路径，其实就是sys.argv[0]|
|%(filename)s|打印当前执行程序名|
|%(funcName)s|打印日志的当前函数|
|%(lineno)d|打印日志的当前行号|
|%(asctime)s|打印日志的时间|
|%(thread)d|打印线程ID|
|%(threadName)s|打印线程名称|
|%(process)d|打印进程ID|
|%(message)s|打印日志信息|

