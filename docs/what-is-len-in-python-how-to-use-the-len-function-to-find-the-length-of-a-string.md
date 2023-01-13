# Python 中的 len()是什么？如何使用 len()函数查找字符串的长度

> 原文：<https://www.freecodecamp.org/news/what-is-len-in-python-how-to-use-the-len-function-to-find-the-length-of-a-string/>

在编程语言中，获取特定数据类型的长度是一种常见的做法。

Python 也不例外，因为您可以使用内置的`len()`函数来获取字符串、元组、列表、字典或任何其他数据类型的长度。

在这篇文章中，我将向你展示如何用`len()`函数获得一个字符串的长度。

## Python 中`len()`的基本语法

要使用`len()`函数获得数据类型的长度，将数据类型赋给一个变量，然后将变量名传递给`len()`函数。

像这样:

```
len(variableName) 
```

## 如何用`len()`函数求字符串的长度

当您使用`len()`函数获取字符串的长度时，它会返回字符串中的字符数——包括空格。

这里有三个例子向你展示它是如何工作的:

```
name = "freeCodeCamp"
print(len(name))

# Output: 12 
```

这意味着字符串中有 12 个字符。

```
founder = "Quincy Larson"
print(len(founder))

# Output: 13 
```

这意味着字符串中有 13 个字符。

```
description = "freeCodeCamp is a platform for learning how to code for free"
print(len(description))

# Output: 60 
```

这意味着字符串中有 60 个字符。

## 如何在 Python 中使用其他数据类型

您可能想知道`len()`函数如何处理其他数据类型，比如列表和元组。

当您在 tuple 或 list 等数据类型上使用`len()`函数时，它返回 tuple 或 list 中的项目数，而不是字符数。

例如，下面的元组的长度返回 3，而不是其中单词的字符数。

```
langs = ("Python", "JavaScript", "Golang")
print(len(langs))

# Output: 3 
```

所以这取决于你正在处理的数据类型。

## 结论

在本文中，您学习了如何获得字符串的长度——字符数。

感谢您的阅读。