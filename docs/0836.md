# strftime – Python DateTime Timestamp String

> 原文：<https://www.freecodecamp.org/news/strftime-python-datetime-timestamp-string/>

对象让我们在 Python 中处理日期和时间。但是当我们使用这个对象时，返回值是数据类型`datatime`而不是`string`。

下面是一个将当前年、月、日和时间打印到控制台的示例:

```
from datetime import datetime

current_day = datetime.now() 

print(current_day)
# 2022-04-18 21:50:24.524022

print(type(current_day))
# <class 'datetime.datetime'>
```

在上面的代码中，我们首先导入了`datetime`对象。`datetime.now()`为我们提供了当天的所有信息，我们将这些信息存储在一个名为`current_day`的变量中。

当我们打印`current_day`时，我们得到了这个:`2022-04-18 21:50:24.524022`。这向我们显示年、月、日和时间。当我们将该类型打印到控制台时，我们得到了`datetime`。

在本文中，我们将讨论由`datetime`对象提供的`strftime()`方法。这个方法让我们将 Python 中的日期和时间对象转换成它们的`string`格式。

## `Strptime`在 Python 中是做什么的？

`strftime()`方法接受一个参数(一个格式代码),指定我们想要返回什么。

这里有一个例子:

```
from datetime import datetime

current_day = datetime.now()

year = current_day.strftime("%Y")

print("current year:", year)
# current year: 2022

print(type(year))
# <class 'str'>
```

上面的代码类似于上一个例子，除了我们创建了一个名为`year`的新变量。在这个变量中，我们将`strftime()`方法附加到当前年份:`current_day.strftime("%Y")`。

您会注意到我们向方法传递了一个参数-" % Y "--它表示年份。这被称为格式代码。在下一个例子中，我们将看到其他格式代码。

在使用了`strftime()`方法之后，数据类型现在可以被看作是一个`string`。

这是另一个例子:

```
from datetime import datetime

current_day = datetime.now()

year = current_day.strftime("%Y")
print("current year:", year)
# current year: 2022

month = current_day.strftime("%B")
print("current month:", month)
# current month: April

day = current_day.strftime("%A")
print("current day:", day)
# current day: Monday

time = current_day.strftime("%-I %p")
print("current time:", time)
# current time: 9 PM
```

在上面的例子中，我们在`strftime()`方法中传递了不同的格式代码，以返回`month`、`day`和`time`变量的`string`值。

这些不是唯一存在的格式代码。点击[此处](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)查看更多你可以尝试的格式代码，看看它们能做什么。

## 结论

在本文中，我们讨论了帮助我们将日期和时间对象转换为`strings`的`strftime()`方法。

我们看到了各种格式代码，它们可以作为参数传入，以返回不同的日期和时间对象值。

编码快乐！