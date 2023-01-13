# Python 获取当前时间

> 原文：<https://www.freecodecamp.org/news/python-get-current-time/>

在你的网站和应用程序中，你可能想要添加一些功能，比如时间戳或者检查用户活动的时间。

每种编程语言都有处理时间的模块或方法，Python 也不例外。

使用 Python 的`datetime`和`time`模块，可以获得当前日期和时间，或者特定时区的日期和时间。

在本文中，我将向您展示如何用 Python 中的`datetime`和`time`模块获取当前时间。

## 如何用日期时间模块获取当前时间

快速获取当前日期和时间的第一件事是使用 datetime 模块中的`datetime.now()`函数:

```
from datetime import datetime
current_date_and_time = datetime.now()

print("The current date and time is", current_date_and_time)

# The current date and time is 2022-07-12 10:22:00.776664 
```

这不仅显示了时间，还显示了日期。

要提取时间，您可以使用`strftime()`函数并传入`("%H:%M:%S")`

*   %H 得到小时
*   %M 获取分钟
*   %S 获得秒数

```
from datetime import datetime
time_now = datetime.now()
current_time = time_now.strftime("%H:%M:%S")

print("The current date and time is", current_time)

# The current date and time is 10:27:45 
```

您也可以像这样重写代码:

```
from datetime import datetime
time_now = datetime.now().strftime("%H:%M:%S")

print("The current date and time is", time_now)

# The current date and time is 10:30:37 
```

## 如何用时间模块获取当前时间

除了`datetime()`模块之外，`time`模块是 Python 中另一种获取当前时间的内置方式。

像往常一样，首先要导入时间模块，然后可以使用`ctime()`方法获取当前日期和时间。

```
import time

current_time = time.ctime()
print(current_time)

# Tue Jul 12 10:37:46 2022 
```

要提取当前时间，还必须使用`strftime()`函数:

```
import time

current_time = time.strftime("%H:%M:%S")
print("The current time is", current_time)

# The current time is 10:42:32 
```

## 最后的想法

本文向您展示了使用 Python 获取当前时间的两种方法。

如果您想知道在`time`和`datetime`模块之间使用哪个，这取决于您想要什么:

*   `time`比`datetime`更精确
*   如果你不希望夏令时(DST)不明确，使用`time`
*   `datetime`有更多您可以使用的内置对象，但对时区的支持有限。

如果你想处理时区，你应该考虑使用`pytz`模块。

为了学习如何获得特定时区的时间，我在这里写了关于[的`pytz`模块](https://www.freecodecamp.org/news/how-to-get-the-current-time-in-python-with-datetime/#howtogetthecurrenttimeofatimezonewithdatetime)。

继续编码:)