# python String to Int–转换字符串示例

> 原文：<https://www.freecodecamp.org/news/python-string-to-int-convert-a-string-example/>

创建程序时，您可能需要从用户那里获得数字输入，并对该值执行各种数学运算。

同样，有些情况下，您可能希望对字符串值执行数学运算。

在这两种情况下，返回的值都是字符串，所以我们不能对它们执行数学运算，因为这样会抛出一个错误。

在本文中，我们将通过一些例子来看看如何在 Python 中将字符串转换成整数。

## 如何在 Python 中将字符串转换成 Int

在 Python 中，我们可以使用内置的`int()`函数将字符串转换成整数。下面是语法的样子:

```
int(string_value)
```

所以我们把要转换的字符串作为参数传递给`int()`函数。就是这样！

这里有一个例子可以帮助你理解:

```
userAge = "10"

print(userAge + 8)

# TypeError: can only concatenate str (not "int") to str
```

在上面的例子中，我们在字符串变量`userAge`上加了 8——但是这显示了一个错误，因为解释器假设我们试图添加(连接)两个字符串。

现在，让我们将变量转换为整数，并执行相同的操作:

```
userAge = "10"

convertUserAge = int(userAge)

print(convertUserAge + 8)

# 18
```

我们转换了`userAge`变量并将其存储在一个名为`convertUserAge`的变量中，然后再次执行我们的操作以获得预期的结果。

在下一个例子中，与上一个类似，我们将获得用户的输入并执行一些计算来显示他们的年龄。

```
from datetime import date

currentDate = date.today()
currentYear = currentDate.year

userBirthYear = input("What is your birth year?")

convertUserBirthYear = int(userBirthYear)

userAge = currentYear - convertUserBirthYear

print(userAge) 
```

在上面的代码中，我们首先从`datetime`模块导入了`date`类。有了它，我们就能够获取当前年份并将其存储在一个变量中。

然后我们请求用户的出生年份:`userBirthYear = input("What is your birth year?")`

之后，我们使用`int()`函数将用户的出生年份(作为字符串返回给我们)转换成整数。有了这个整数值，我们就能够从当前年份中减去用户的出生年份，从而获得并打印出他们的实际年龄。

您可以复制代码并对其进行修改。

## 结论

在本文中，我们学习了如何在 Python 中将字符串转换成整数。我们首先看到一个例子，在这个例子中，我们必须使用`int()`函数将字符串转换为整数，并对该值执行简单的操作。

在第二个例子中，我们从用户那里得到一个输入，将其转换成一个整数，然后执行我们的数学运算来打印他们的当前年龄。

感谢您的阅读和快乐编码！