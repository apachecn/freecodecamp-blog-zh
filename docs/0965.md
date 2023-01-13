# 如何在 Python 中用 Datetime 获取当前时间

> 原文：<https://www.freecodecamp.org/news/how-to-get-the-current-time-in-python-with-datetime/>

在 Python 应用程序中，您可能希望使用时间来添加时间戳、检查用户活动时间等功能。

在 Python 中帮助您处理日期和时间的模块之一是`datetime`。

使用`datetime`模块，您可以获得当前日期和时间，或者特定时区的当前日期和时间。

在本文中，我将向您展示如何用 Python 中的`datetime`模块获取当前时间。我还将向您展示如何获得世界上任何时区的当前时间。

## 目录

*   [如何用`datetime`模块](#howtogetthecurrenttimewiththedatetimemodule)获取当前时间
*   [功能`datetime.now()`的属性](#attributesofthedatetimenowfunction)
*   [如何用`datetime`](#howtogetthecurrenttimeofatimezonewithdatetime) 获取一个时区的当前时间
*   [结论](#conclusion)

## 如何用`datetime`模块获取当前时间

您必须做的第一件事是像这样导入`datetime`模块:

```
from datetime import datetime 
```

下一件可以快速获得当前日期和时间的事情是使用`datetime`模块中的`datetime.now()`函数:

```
from datetime import datetime
currentDateAndTime = datetime.now()

print("The current date and time is", currentDateAndTime)
# Output: The current date and time is 2022-03-19 10:05:39.482383 
```

为了获得当前时间，您可以使用`strftime()`方法并向其中传递表示小时、分钟和秒的字符串`”%H:%M:%S”`。

这将为您提供 24 小时格式的当前时间:

```
from datetime import datetime
currentDateAndTime = datetime.now()

print("The current date and time is", currentDateAndTime)
# Output: The current date and time is 2022-03-19 10:05:39.482383

currentTime = currentDateAndTime.strftime("%H:%M:%S")
print("The current time is", currentTime)
# The current time is 10:06:55 
```

## `datetime.now()`功能的属性

`datetime.now`函数有几个属性，可以用来获取当前日期的年、月、周、日、小时、分钟和秒。

下面的代码片段将所有属性值打印到终端:

```
from datetime import datetime
currentDateAndTime = datetime.now()

print("The current year is ", currentDateAndTime.year) # Output: The current year is  2022
print("The current month is ", currentDateAndTime.month) # Output: The current month is  3 
print("The current day is ", currentDateAndTime.day) # Output: The current day is  19
print("The current hour is ", currentDateAndTime.hour) # Output: The current hour is  10 
print("The current minute is ", currentDateAndTime.minute) # Output: The current minute is  49
print("The current second is ", currentDateAndTime.second) # Output: The current second is  18 
```

## 如何用`datetime`获得一个时区的当前时间

您可以通过将`datetime`模块与另一个名为`pytz`的模块一起使用来获得特定时区的当前时间。

你可以像这样从 pip 安装`pytz`模块:
`pip install pytz`

您必须做的第一件事是导入`datetime`和`pytz`模块:

```
from datetime import datetime
import pytz 
```

然后，您可以使用下面的代码片段检查所有可用的时区:

```
from datetime import datetime
import pytz

zones = pytz.all_timezones

print(zones) 
# Output: all timezones of the world. Massive! 
```

在下面的代码片段中，我能够获得纽约的时间:

```
from datetime import datetime
import pytz

newYorkTz = pytz.timezone("America/New_York") 
timeInNewYork = datetime.now(newYorkTz)
currentTimeInNewYork = timeInNewYork.strftime("%H:%M:%S")

print("The current time in New York is:", currentTimeInNewYork)
# Output: The current time in New York is: 05:36:59 
```

我怎么能知道纽约现在的时间呢？

*   我引入了`pytz`模块的`pytztimezone()`方法，将纽约的准确时区作为一个字符串传递给它，并将其赋给一个名为`newYorkTz`(代表纽约时区)的变量
*   为了获得纽约的当前时间，我使用了来自`datetime`模块的`datetime.now()`函数，并将我创建的用于存储纽约时区的变量传递给它
*   为了最终获得 24 小时格式的纽约当前时间，我对`timeInNewYork`变量使用了`strftime()`方法，并将它存储在名为`currentTimeInNewYork`的变量中，这样我就可以将它打印到终端上

## 结论

如本文所示，`datetime`模块非常方便地处理时间和日期，并随后获得当地的当前时间。

当与可以从 pip 安装的`pytz`模块结合使用时，还可以用它来获取世界上任何时区的当前时间。

感谢您的阅读。如果你觉得这篇文章有帮助，你可以分享给你的朋友和家人。