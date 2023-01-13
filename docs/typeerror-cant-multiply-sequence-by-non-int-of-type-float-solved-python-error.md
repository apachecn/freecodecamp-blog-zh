# TypeError:不能将序列与 float 类型的非 int 相乘[已解决 Python 错误]

> 原文：<https://www.freecodecamp.org/news/typeerror-cant-multiply-sequence-by-non-int-of-type-float-solved-python-error/>

在本文中，您将了解 Python 错误`TypeError: can't multiply sequence by non-int of type float`的含义。

首先，我将解释为什么会出现这个错误。您还将学习如何解决错误，以及如何在第一时间避免错误。

以下是我们将要介绍的内容:

1.  [Python 中的`TypeError: can't multiply sequence by non-int of type float`错误是什么？](#intro)
    1.  [Python 中`TypeError: can't multiply sequence by non-int of type float`错误是如何发生的？](#reason)
2.  [如何解决 Python 中的`TypeError: can't multiply sequence by non-int of type float`错误](#solution)

## Python 中的`TypeError: can't multiply sequence by non-int of type float`错误是什么？

Python 中有两种类型的数字:

*   整数–可以是正数、负数或零的整数。
*   浮点数–带小数点的正数或负数。

您可以将整数乘以字符串来创建重复的字符序列:

```
print("Python" * 3)

# output

# PythonPythonPython 
```

提醒一下，字符串是用单引号或双引号括起来的任何字符，包括数字:

```
print("3" * 3)

# output

# 333 
```

但是，看看当您尝试将一个字符串与一个浮点数相乘时会发生什么:

```
print("3" * 3.3)

# output

# Traceback (most recent call last):
#  File "main.py", line 1, in <module>
#    print("3" * 3.3)
# TypeError: can't multiply sequence by non-int of type 'float' 
```

出现了一个错误——特别是一个`TypeError`。

`TypeError`是 Python 中的一个异常，当您试图对不支持该特定操作的数据类型进行操作时会引发该异常。

错误消息告诉您，不能在字符串(或序列)和浮点数(或浮点数)之间执行乘法，因为 Python 不支持这两种数据类型之间的乘法。

### Python 中的`TypeError: can't multiply sequence by non-int of type float`错误是如何发生的？

当您使用接受用户输入的 Python `input()`函数时，通常会出现`TypeError: can't multiply sequence by non-int of type float`错误。这是因为，默认情况下，`input()`函数将用户输入作为字符串返回。

让我们举一个假设的例子。假设我让一个用户输入他们的年龄，并将他们的答案存储在一个名为`user_age`的变量中:

```
user_age = input("Please enter your age: ") 
```

我可以使用`type()`函数检查存储在变量`user_age`中的值的数据类型，然后将结果打印到控制台:

```
user_age = input("Please enter your age: ")

print(type(user_age))

# output

# Please enter your age: 29
# <class 'str'> 
```

从输出中，您可以看到即使我输入了一个整数，返回的数据类型也是 string 类型。

不管出于什么原因，如果我想用一个浮点数乘以用户的年龄，我会得到`TypeError: can't multiply sequence by non-int of type float`错误:

```
user_age = input("Please enter your age: ")

print(user_age * 0.5)

# output

# Please enter your age: 29
# Traceback (most recent call last):
#  File "main.py", line 3, in <module>
#    print(user_age * 0.5)
# TypeError: can't multiply sequence by non-int of type 'float' 
```

## 如何解决 Python 中的`TypeError: can't multiply sequence by non-int of type float`错误

要解决`TypeError: can't multiply sequence by non-int of type float`错误，先将字符串转换成浮点数，然后再乘以浮点数。

正如您之前看到的，下面的代码抛出了`TypeError: can't multiply sequence by non-int of type float`错误:

```
print("3" * 3.3)

# output

# Traceback (most recent call last):
#  File "main.py", line 1, in <module>
#    print("3" * 3.3)
# TypeError: can't multiply sequence by non-int of type 'float' 
```

如果在乘以浮点数`3.3`之前将字符串`"3"`转换为浮点数，将不会有错误。

要将字符串转换成浮点数，使用`float()`函数:

```
print(float("3") * 3.3)

# output

# 9.899999999999999 
```

当您使用`input()`功能时，也可以这样做。使用`float()`函数将用户输入的值转换成浮点数。

下面是如何使用前面的`input()`函数重写示例:

```
user_age = float(input("Please enter your age: "))

print(user_age * 0.5)

# output

# Please enter your age: 29
# 14.5 
```

`float()`函数将`input()`函数返回的字符串值转换成浮点数，您可以将该值乘以浮点数。

## 结论

现在你知道了！您现在知道如何解决 Python 中的`TypeError: can't multiply sequence by non-int of type float`错误了！

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢您的阅读，祝您编码愉快！