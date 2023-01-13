# Python 将字符串小写–str . lower()示例

> 原文：<https://www.freecodecamp.org/news/python-to-lowercase-a-string-str-lower-example/>

字符串可以是不同的格式，如小写、大写和大写。在本文中，我将向您展示如何在 Python 中将字符串转换为小写。

小写字符串的所有字符都是小写字母。一个例子就是 **python** 。

一个大写的字符串，每个单词的第一个字母大写，其余字母小写。一个例子就是 **Python** 。

大写字符串的所有字符都是大写字母。 **PYTHON** 就是一个例子。

Python 有许多以不同方式修改字符串的字符串方法。要将字符串转换成小写，可以使用`lower`方法。

strings 的 lower 方法从旧字符串返回一个新字符串，其中所有字符都是小写的。数字，符号，所有其他非字母字符保持不变。

## Python 中的 str.lower()示例

下面是如何在 Python 中使用`lower`方法:

```
text = "HellO woRLd!"

lower = text.lower()

print(lower)
# hello world! 
```

`lower`方法的一个很好的用例是不管大小写，比较字符串来评估它们的相等性。

在 Python 中，“hello world”不等于“Hello World ”,因为相等是区分大小写的，如下面的代码所示:

```
text1 = "Hello World"
text2 = "hello world"

print(text1 == text2)
# False 
```

`text1`有一个上面的“H”和“W”，而`text2`有一个下面的“H”和“W”。因为这些字符的大小写不同，`text1`不等于`text2`，即使它们有相同的字符。

要比较两个字符串而忽略它们的大小写，可以使用下面的方法，如下所示:

```
text1 = "HeLLo worLD"
text2 = "HellO WORLd"

print(text1 == text2)
# False

print(text1.lower() == text2.lower())
# True 
```

通过将两个字符串转换为小写，您可以正确地检查它们是否具有相同的字符。

## 如何在 Python 中使用`upper`

小写的反义词是大写，Python 对此也有一个方法。您可能已经猜到，Python 中的`upper`方法返回一个新字符串，其中所有字符都是大写的。

你可以像这样使用`lower`方法:

```
text = "hello World!"

upper = text.upper()

print(upper)
// HELLO WORLD! 
```

您还可以使用此方法来检查两个字符串是否具有相同的字符集。但是在今天的大多数应用程序中，开发人员使用小写比较方法。

就是这样！现在您知道如何在 Python 中将字符串转换为全部小写或大写字母。