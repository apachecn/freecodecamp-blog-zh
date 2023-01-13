# Python 多行注释——如何在 Python 中注释掉多行

> 原文：<https://www.freecodecamp.org/news/python-multiline-comment-how-to-comment-out-multiple-lines-in-python/>

注释是每种编程语言不可或缺的一部分。通过注释，您可以更好地理解自己的代码，使其更具可读性，并且可以帮助团队成员理解它是如何工作的。

注释被编译器和解释器忽略，所以它们不会运行。

除了让你的代码更具可读性之外，注释还可以在你调试的时候有所帮助——如果你有两行代码，你可以注释掉其中一行以防止它运行。

就像其他编程语言一样，Python 支持注释。

问题是 Python 没有内置的多行注释机制。

因此，在本文中，我将不仅仅向您展示如何在 Python 中进行单行注释——我还将向您展示进行多行注释的变通方法。

## 如何在 Python 中创建单行注释

要在 Python 中进行单行注释，需要在每一行前面加上一个散列(`#`)。

```
# print("Hello world")

print("Hello campers") 
```

输出:

```
Hello campers 
```

如您所见，输出中没有打印注释行。

## 如何在 Python 中进行多行注释

与使用`/*...*/`进行多行注释的 JavaScript、Java 和 C++等其他编程语言不同，Python 中没有内置的多行注释机制。

要在 Python 中注释掉多行，可以在每行前面加上一个散列(`#`)。

```
# print("Hello world")
# print("Hello universe")
# print("Hello everyone")

print("Hello campers") 
```

输出:

```
Hello campers 
```

使用这种方法，从技术上讲，您是在做多个单行注释。

在 Python 中进行多行注释的真正解决方法是使用**文档字符串**。

如果在 Python 中使用 docstring 注释掉多行代码，代码块将被忽略，只有 docstring 之外代码行将运行。

```
"""
This is a multi-line comment with docstrings

print("Hello world")
print("Hello universe")
print("Hello everyone")
"""

print("Hello campers") 
```

输出:

```
Hello campers 
```

**NB:** 需要注意的一点是，在使用 doctsrings 进行注释时，缩进仍然很重要。如果您使用 4 个空格(或一个制表符)进行缩进，您将得到一个缩进错误。

例如，这将起作用:

```
def addNumbers(num1, num2, num3):
    """
    A function that returns the sum of
    3 numbers
    """
    return num1 + num2 + num3
print(addNumbers(2, 3, 4))

# Output: 9 
```

但这是行不通的:

```
def addNumbers(num1, num2, num3):
"""
A function that returns the sum of
3 numbers
"""
    return num1 + num2 + num3
print(addNumbers(2, 3, 4)) 
```

所以你的 IDE 会抛出错误“`IndentationError: expected an indented block`”。

## 结论

因为 Python 中没有对多行注释的内置支持，所以本文演示了如何使用 docstrings 作为一种变通方法。

尽管如此，您通常应该坚持使用使用散列(`#`)的常规 Python 注释，即使您必须在多行中使用它。这是因为文档字符串意味着文档，而不是注释掉代码。

如果你觉得这篇文章有帮助，可以考虑与你的朋友和家人分享。

感谢您的阅读。