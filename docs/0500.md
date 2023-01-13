# 类型错误:字符串索引必须是整数–如何在 Python 中修复

> 原文：<https://www.freecodecamp.org/news/typeerror-string-indices-must-be-integers-how-to-fix-in-python/>

在 Python 中，有一些特定的可迭代对象——列表、元组和字符串——它们的项目或字符可以使用它们的索引号来访问。

例如，要访问字符串中的第一个字符，您应该这样做:

```
greet = "Hello World!"

print(greet[0])
# H
```

为了访问上面的`greet`字符串中第一个字符的值，我们使用了它的索引号:`greet[0]`。

但是在某些情况下，当你试图访问一个字符串中的一个字符时，你会得到一个错误，说“类型错误:字符串索引必须是整数”。

在本文中，您将看到为什么会出现这个错误以及如何修复它。

## Python 中是什么原因导致“TypeError:字符串索引必须是整数”？

出现“type error:string indexes must be integers”错误有两个常见原因。

我们将在两个不同的小节中讨论这些原因及其解决方案。

### 如何修复 Python 中字符串的`TypeError: string indices must be integers`错误

正如我们在上一节中看到的，要访问字符串中的一个字符，需要使用字符的索引。

当我们试图使用字符串值而不是索引号来访问一个字符时，我们会得到“type error:string indexes must is integers”错误。

这里有一个例子可以帮助你理解:

```
greet = "Hello World!"

print(greet["H"])
# TypeError: string indices must be integers
```

正如你在上面的代码中看到的，我们得到了一个错误消息`TypeError: string indices must be integers`。

发生这种情况是因为我们试图使用它的值(“H”)而不是索引号来访问`H`。

也就是`greet["H"]`而不是`greet[0]`。就是这么修的。

这个问题的解决方案非常简单:

*   当处理需要在访问项目/字符时使用索引号(整数)的 iterable 对象时，不要使用字符串来访问项目/字符。

### 如何修复 Python 中切片字符串时的`TypeError: string indices must be integers`错误

在 Python 中对字符串进行切片时，会根据给定的参数(`start`和`end`参数)返回字符串中的一系列字符。

这里有一个例子:

```
greet = "Hello World!"

print(greet[0:6])
# Hello 
```

在上面的代码中，我们提供了两个参数-0 和 6。这将返回索引 0 和索引 6 内的所有字符。

当我们不正确地使用 slice 语法时，我们会得到“type error:string indexes must is integers”错误。

这里有一个例子来说明:

```
greet = "Hello World!"

print(greet[0,6])
# TypeError: string indices must be integers
```

代码中的错误很容易被忽略，因为我们使用了整数——但我们仍然会得到一个错误。在这种情况下，错误消息可能会产生误导。

我们得到这个错误是因为我们使用了错误的语法。在我们的例子中，我们使用逗号来分隔`start`和`end`参数:`[0,6]`。这就是我们出错的原因。

要解决这个问题，您可以将逗号改为冒号。

在 Python 中分割字符串时，需要使用冒号–`[0:6]`来分隔`start`和`end`参数。

## 摘要

在本文中，我们讨论了 Python 中的“类型错误:字符串索引必须是整数”错误。

使用 Python 字符串时会发生此错误，主要有两个原因——在访问字符串中的字符时使用字符串而不是索引号(整数),以及在 Python 中对字符串进行切片时使用错误的语法。

我们在两个小节中看到了引发这个错误的例子，并学习了如何修复它们。

编码快乐！