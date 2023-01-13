# Python 将字符串转换为整数——如何在 Python 中转换字符串

> 原文：<https://www.freecodecamp.org/news/python-convert-string-to-int-how-to-cast-a-string-in-python/>

当你编程时，你经常需要在数据类型之间切换。

在处理信息时，将一种数据类型转换为另一种数据类型的能力为您提供了极大的灵活性。

Python 编程语言中有不同的内置方法来转换或转换类型。

在本文中，您将学习如何将字符串转换为整数。

我们开始吧！

## Python 中的数据类型

Python 支持多种数据类型。

数据类型用于指定、表示和分类计算机程序中存在和使用的不同种类的数据。

此外，不同的操作可用于不同类型的数据——在一种数据类型中可用的操作在另一种数据类型中通常不可用。

数据类型的一个例子是字符串。

字符串是用于传递文本信息的字符序列。

它们用单引号或双引号括起来，就像这样:

```
fave_phrase = "Hello world!"

#Hello world! is a string,enclosed in double quotation marks 
```

整数是整数。

它们用于表示数字数据，在处理整数时，您可以进行任何数学运算(如加、减、乘、除)。

整数是用单引号或双引号括起来的*而不是*。

```
fave_number = 7

#7 is an int
#"7" would not be an int but a string, despite it being a number. 
#This is because of the quotation marks surrounding it 
```

### 数据类型转换

有时候，当您存储数据时，或者当您接收到来自用户的某种类型的输入时，您将需要对该数据进行操作和执行不同种类的操作。

由于每种数据类型都可以用不同的方式操作，这通常意味着您需要转换它。

将一种数据类型转换为另一种数据类型也称为类型转换。许多语言提供了内置的强制转换操作符来完成这一任务——显式地将一种类型转换成另一种类型。

## 如何在 Python 中将字符串转换成整数

要在 Python 中将字符串转换或造型为整数，可以使用内置函数`int()`。

该函数将您要转换的初始字符串作为参数，并返回您传递的值的整数等效值。

一般的语法大概是这样的:`int("str")`。

让我们以下面的例子为例，这里有一个数字的字符串版本:

```
#string version of the number 7
print("7")

#check the data type with type() method
print(type("7"))

#output

#7
#<class 'str'> 
```

要将数字的字符串版本转换为等效的整数，可以使用`int()`函数，如下所示:

```
#convert string to int data type
print(int("7"))

#check the data type with type() method
print(type(int("7")))

#output

#7
#<class 'int'> 
```

### 一个将字符串转换成整数的实际例子

假设你想计算一个用户的年龄。你通过接收他们的输入来做到这一点。该输入将总是字符串格式。

因此，即使他们键入一个数字，这个数字也将是`<class 'str'>`。

如果您想对该输入执行数学运算，例如从另一个数字中减去该输入，您将得到一个错误，因为您不能对字符串执行数学运算。

看看下面的例子，看看这是怎么回事:

```
current_year = 2021

#ask user to input their year of birth
user_birth_year_input = input("What year were you born? ")

#subtract the year the user filled in from the current year 
user_age = current_year - user_birth_year_input

print(user_age)

#output

#What year were you born? 1993
#Traceback (most recent call last):
#  File "demo.py", line 9, in <module>
#    user_age = current_year - user_birth_year_input
#TypeError: unsupported operand type(s) for -: 'int' and 'str' 
```

错误提到不能在 int 和 string 之间执行减法。

您可以使用`type()`方法检查输入的数据类型:

```
current_year = 2021

#ask user to input their year of birth
user_birth_year_input = input("What year were you born? ")

print(type(user_birth_year_input))

#output

#What year were you born? 1993
#<class 'str'> 
```

解决这一问题并避免错误的方法是将用户输入转换为整数，并将其存储在一个新变量中:

```
current_year = 2021

#ask user to input their year of birth
user_birth_year_input = input("What year were you born? ")

#convert the raw user input to an int using the int() function and store in new variable
user_birth_year = int(user_birth_year_input)

#subtract the converted user input from the current year
user_age = current_year - user_birth_year

print(user_age)

#output

#What year were you born? 1993
#28 
```

## 结论

现在，您已经知道如何在 Python 中将字符串转换成整数了！

如果你想学习更多关于 Python 编程语言的知识，freeCodeCamp 有一个 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)可以帮你入门。

您将从基础知识开始，逐步学习更高级的主题，如数据结构和关系数据库。最后，您将构建五个项目来实践您所学到的内容。

感谢阅读和快乐编码！