# python lower case–如何使用字符串 lower()函数

> 原文：<https://www.freecodecamp.org/news/python-lowercase-how-to-use-the-string-lower-function/>

字符串是使用 Python 的基础部分。而`lower()`方法是众多可以用来处理字符串的集成方法之一。

在本文中，我们将看到如何用 Python 中的`lower()`方法使字符串小写。

## 什么是字符串？

字符串是一种可以包含许多不同字符的数据类型。字符串由单引号或双引号之间的一系列字符组成。

```
>>> example_string = 'I am a String!'
>>> example_string
'I am a String!'
```

## 什么是方法？

方法是可以在特定数据类型上使用的函数。方法可以带参数也可以不带参数。

有时你可能想知道一个方法是否存在。在 Python 中，您可以通过使用带有字符串参数的`dir()`函数来查看字符串方法的完整列表，如下所示:

```
>>> dir(example_string)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

在这些众多的字符串方法中，在本文中您将了解到`lower()`方法及其工作原理。

## `lower()`方法是如何工作的？

`lower()`方法是一个字符串方法，返回一个新的字符串，完全小写。如果原始字符串有大写字母，那么在新字符串中，这些字母将是小写的。任何小写字母或任何非字母字符不受影响。

```
>>> example_string.lower()
'i am a string!'

>>> 'FREECODECAMP'.lower()
'freecodecamp'
```

## 使用低位方法时要注意什么

`lower()`方法做了一件非常简单的事情:它创建了一个新的字符串，其中所有的大写字母都变成了小写字母。但是在使用它的时候，有一些事情需要记住。让我们来看看它们。

### 字符串是不可变的

字符串是不可变的数据类型，这意味着它们不能被改变。使用`lower()`方法后，原始字符串将保持不变。

在上面的例子中，`lower()`方法作用于`example_string`,但从未改变它。检查`example_string`的值仍然显示原始值。

```
>>> example_string
'I am a String!'

>>> example_string.lower()
'i am a string!'

>>> example_string
'I am a String!'
```

### `lower()`方法返回一个新字符串

`lower()`返回一个新的字符串。如果您想在代码中再次使用它，您需要将它保存在一个变量中。

```
>>> new_string = example_string.lower()

>>> new_string
'i am a string!'
```

### 字符串区分大小写

字符串区分大小写，因此小写字符串不同于大写字符串。

```
>>> 'freecodecamp' == 'FREECODECAMP'
False
```

当考虑到`lower()`方法的用途时，这是很有用的。在这个例子中，您将看到这个特性如何使`lower()`方法在构建处理字符串的脚本或程序时变得有用和必要。

## 方法示例:如何检查用户输入是否匹配

让我们编写一个小脚本，向用户提出一个问题并等待输入，并对用户的回答进行反馈。

```
answer = input("What color is the sun? ")
if answer == "yellow":
  print("Correct!")
else:
  print("That is not the correct color!")
```

the sun_color.py file

这个脚本问用户一个问题，“太阳是什么颜色？”，并等待回答。然后它检查答案是否是“黄色”，如果是，它打印“正确！”如果不是，它会打印“这不是正确的颜色！”。

但是这个脚本有一个问题。

运行这个脚本，您将在终端中被问到这个问题:

```
$ python sun_color.py
What color is the sun? 
```

如果你回答“黄色”，它说:

```
$ python sun_color.py
What color is the sun? Yellow
That is not the correct color!
```

为什么会这样？

记住字符串是区分大小写的。该脚本检查用户输入的带有大写字母“Y”的字符串`yellow`–`Yellow`是否是不同的字符串。

您可以通过使用`lower()`方法，并对`sun_color.py`文件做这个小小的修改来轻松解决这个问题:

```
answer = input("What color is the sun? ")
if answer.lower() == "yellow":
  print("Correct!")
else:
  print("That is not the correct color!")
```

the fixed sun_color.py file

现在，如果你再试一次...

```
>>> python sun_color.py
What color is the sun? Yellow
Correct!
```

什么变了？书写`answer.lower()`在与正确答案字符串“yellow”进行比较之前，你要确保被检查的字符串完全是小写字母。通过这种方式，用户写“黄色”或“黄色”或“黄色”都没关系——都被转换成小写。

感谢阅读！现在您知道了如何在 JavaScript 中使用`lower()`方法。