# strftime() Python – Datetime Format Tutorial

> 原文：<https://www.freecodecamp.org/news/strftime-in-python/>

在 Python 中，可以使用`strftime`函数格式化日期对象并以可读的形式呈现它们。在本文中，我将向您展示如何实现。

## Python 中的`strftime()`是什么？

`strftime()`是一个 Python 日期方法，可以用来将日期转换成字符串。它不仅仅转换成字符串，还允许你以一种可读的方式格式化你的日期。

如果你熟悉 JavaScript，可以把这个方法想象成`date-fns`库的`format`函数，它有不同的字符用于格式化日期。

## 如何在 Python 中使用`strftime()`

`strftime`方法的语法是:

```
date.strftime(format) 
```

`format`参数可以是字符串最终输出的不同字符的组合。让我们来看看其中的一些:

```
from datetime import datetime

current_date = datetime.now()
print(current_date)
# 2022-07-14 23:37:38.578835

string_date = current_date.strftime("%Y")
print(string_date)
# 2022 
```

`datetime.now`返回当前日期。使用`strftime`方法和“%Y”字符，日期被转换为显示年份的字符串。

这是另一个例子:

```
from datetime import datetime

date = datetime.fromisoformat("2022-07-15 00:15:14.643725")

string_date = current_date.strftime("%Y-%b")
print(string_date)
# 2022-Jul 
```

使用`datetime`对象的`fromisoformat`,可以传递一个完整的日期字符串，这样就可以获得该字符串的日期对象。

`%Y`表示全年(2022 年)`%b`表示月份的简称(7 月)。

`strftime`保留了连字符“-”，但用正确的日期表示法替换了其他字符。

下面是另一个将时间格式化为日期的示例:

```
from datetime import datetime

date = datetime.now()

string_time = date.strftime("%X")
print(string_time)
# 00:54:20 
```

`%X`字符通过在`hours:minutes:seconds`中显示时间表示来格式化日期字符串。

## 包裹

在本教程中，我们看到了如何使用不同的字符作为参数传递给`strftime` date 方法来格式化日期字符串。

我们使用了:

*   整整一年
*   `%b`为缩写的月份名
*   `%X`用于时间表示

还有许多其他字符表示完整的月份名称、缩写的年份名称和时间。查看 [Python strftime cheatsheet](https://strftime.org/) 以了解更多可以用来表示日期的字符。