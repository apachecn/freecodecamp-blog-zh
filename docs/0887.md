# 列表转字符串 Python–join()语法示例

> 原文：<https://www.freecodecamp.org/news/python-list-to-string-join-example/>

有时你想把你的 Python 列表转换成字符串。本教程将向您展示如何做到这一点的例子。

但是首先，让我们探索一下 Python 列表数据结构和 Python 字符串数据结构之间的区别。

## Python 中的列表是什么？

在 Python 中，列表是元素的有序序列。列表中的每个元素都有一个相应的索引号，您可以使用它来访问它。

您可以使用方括号创建列表，并且可以包含任何数据类型的混合。

```
>>> exampleList = ['car', 'house', 'computer']
```

注意，我将展示来自 Python REPL 的代码。我正在输入的内容开头有`>>>`。输出的开头没有任何内容。你可以通过进入你的终端并输入`python`然后按回车键来启动这个 REPL。

一旦初始化了 Python 列表，就可以使用括号符号来访问它的元素。请记住，索引从零开始，而不是从 1 开始。以下是输入和输出的示例:

```
>>> exampleList[0]
'car'

>>> exampleList[1] 
'house'

>>> exampleList[2] 
'computer'
```

## Python 中的字符串是什么？

字符串只是一个或多个字符的序列。例如，`'car'`是一个字符串。

您可以像这样初始化它:

```
>>> exampleString = 'car'
```

然后您可以调用字符串数据结构来查看其内容:

```
>>> exampleString
'car' 
```

## 如何将 Python 列表转换成逗号分隔的字符串？

您可以使用。join string 方法将列表转换为字符串。

```
>>> exampleCombinedString = ','.join(exampleList)
```

然后，您可以访问新的字符串数据结构来查看其内容:

```
>>> exampleCombinedString
'car,house,computer'
```

所以语法还是`[seperator].join([list you want to convert into a string])`。

在本例中，我使用逗号作为分隔符，但是您可以使用任何想要的字符。

给你。让我们再次连接，但这一次，让我们在逗号后添加一个空格，这样得到的字符串将更容易阅读:

```
>>> exampleCombinedString = ', '.join(exampleList)
>>> exampleCombinedString
'car, house, computer' 
```

给你。

## 在 Python 中如何连接列表？

在 Python 中，有多种方式来连接列表。最常见的是使用`+`运算符:

```
>>> list1 = [1, 2, 3]
>>> list2 = [4, 5, 6]
>>> list1 + list2
[1, 2, 3, 4, 5, 6]
```

另一种选择是使用`extend()`方法:

```
>>> list1 = [1, 2, 3]
>>> list2 = [4, 5, 6]
>>> list1.extend(list2)
>>> list1
[1, 2, 3, 4, 5, 6]
```

最后，您可以使用`list()`构造函数:

```
>>> list1 = [1, 2, 3]
>>> list2 = [4, 5, 6]
>>> list3 = list1 + list2
>>> list3
[1, 2, 3, 4, 5, 6]
```

## 在 Python 中如何将列表转换成数组？

一种方法是使用 NumPy 库。

首次导入数量:

```
import numpy as np
```

然后运行 np.array()方法将列表转换为数组:

```
>>> a = np.array([1, 2, 3])
>>> print(a)
[1 2 3]
```

## 用 Python 可以把列表转换成字典吗？

绝对是。首先，让我们使用内置的`dict()`函数创建一个字典。示例`dict()`语法:

```
d = dict(name='John', age=27, country='USA') 
print(d)
```

它将输出:

```
{'age': 27, 'country': 'USA', 'name': 'John'}
```

在这个例子中，我们使用`dict()`函数创建了一个 dictionary 对象。`dict()`函数接受一个可迭代的对象。在这种情况下，我们使用一个元组。

### 如何从列表中创建字典

您也可以使用`dict()`功能从列表中创建一个字典。

```
d = dict(zip(['a', 'b', 'c'], [1, 2, 3]))
print(d) 
```

它将输出:

```
{'a': 1, 'b': 2, 'c': 3} 
```

注意，在这个例子中，我们使用了`zip()`函数来创建一个元组。

### 如何从另一个字典创建字典:

您可以从另一个字典创建一个字典。您可以使用 dict()函数或构造函数方法来实现这一点。

```
d = dict(a=1, b=2, c=3) print(d) 
```

它将输出:

```
{'a': 1, 'b': 2, 'c'} 
```

## 使用 Python 列表、数组、字典和字符串可以做很多令人惊叹的事情

我真的只是触及了表面。如果你想更深入，并在现实世界的项目中应用这些方法和技术，freeCodeCamp.org 可以帮助你。

如果你想学习更多的编程和技术知识，可以试试 [freeCodeCamp 的核心编码课程](https://www.freecodecamp.org/learn)。它是免费的。