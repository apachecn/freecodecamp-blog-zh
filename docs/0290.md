# python datetime . now()–如何获取今天的日期和时间

> 原文：<https://www.freecodecamp.org/news/python-datetime-now-how-to-get-todays-date-and-time/>

您可以使用 Python 中的`datetime`模块来检索关于日期和时间的数据。

在本文中，您将学习如何使用来自`datetime`模块的`datetime`对象来获取当前的日期和时间属性。

您还将学习如何使用`datetime.now()`函数和`pytz`模块获得世界各地的日期和时间。

## 如何在 Python 中使用`datetime`对象

为了使用`datetime`对象，你必须首先导入它。方法如下:

```
from datetime import datetime
```

在下一个例子中，您将看到如何使用`datetime`对象。

```
from datetime import datetime

current_dateTime = datetime.now()

print(current_dateTime)
# 2022-09-20 10:27:21.240752
```

在上面的代码中，我们将`datetime`赋给了一个名为`current_dateTime`的变量。

当打印到控制台时，我们得到了当前的年、月、日和时间:`2022-09-19 17:44:17.858167`。

注意，我们可以使用`now()`方法:`datetime.now()`来访问上面的信息。

## 如何使用`datetime.now()`属性

在上一节中，我们检索了关于当前日期和时间的信息，包括当时的年、月、日和时间。

但是`datetime.now()`函数为我们提供了提取单个数据的额外属性。

例如，要获得当前年份，您应该这样做:

```
from datetime import datetime

current_dateTime = datetime.now()

print(current_dateTime.year)
# 2022
```

在上面的例子中，我们将`datetime.now()`函数赋给了一个名为`current_dateTime`的变量。

使用点符号，我们将`year`属性附加到上面声明的变量:`current_dateTime.year`。当打印到控制台时，我们得到 2022。

`datetime.now()`函数具有以下属性:

*   `year`
*   `month`
*   `day`
*   `hour`
*   `minute`
*   `second`
*   `microsecond`

下面是上面列出的属性的使用示例:

```
from datetime import datetime

current_dateTime = datetime.now()

print(current_dateTime.year) # 2022

print(current_dateTime.month) # 9

print(current_dateTime.day) # 20

print(current_dateTime.hour) # 11

print(current_dateTime.minute) # 27

print(current_dateTime.second) # 46

print(current_dateTime.microsecond) # 582035
```

## 如何使用`datetime.now()`和`pytz`在 Python 中获取特定时区

要在 Python 中获得世界各地不同时区的信息，您可以利用`datetime.now()`函数和`pytz`模块。

下面的示例显示了如何获取尼日利亚拉各斯的当前日期和时间:

```
from datetime import datetime
import pytz

datetime_in_Lagos = datetime.now(pytz.timezone('Africa/Lagos'))

print(datetime_in_Lagos)
# 2022-09-20 12:53:27.225570+01:00
```

在上面的代码中，我们首先导入了模块:

```
from datetime import datetime
import pytz 
```

然后我们将`pytz`对象作为参数传递给`datetime.now()`函数:

```
datetime_in_Lagos = datetime.now(pytz.timezone('Africa/Lagos'))
```

`pytz`对象有一个`timezone`属性，它接受您正在寻找的特定时区的信息/参数:`pytz.timezone('Africa/Lagos')`。

使用`all_timezones`属性，您可以获得`pytz`库中所有可能时区的列表，这些时区可以作为参数传递给`timezone`属性——就像我们在上一个示例中所做的那样。那就是:

```
from datetime import datetime
import pytz

all_timezones = pytz.all_timezones

print(all_timezones)
# [List of all timezones...]
```

## 摘要

在本文中，我们讨论了使用 Python 中的`datetime.now()`函数获取今天的日期和时间。

我们看到了一些展示如何使用`datetime.now()`函数及其属性的例子。

最后，我们看到了如何使用`datetime.now()`函数和`pytz`模块获取世界上特定位置的日期和时间。

编码快乐！