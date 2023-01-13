# 类型错误:str 对象不可调用–如何在 Python 中修复

> 原文：<https://www.freecodecamp.org/news/typeerror-str-object-is-not-callable-how-to-fix-in-python/>

每种编程语言都有特定的关键字，这些关键字具有特定的、预先构建的功能和含义。

用这些关键字命名变量或函数很可能会引发错误。我们将在本文中讨论其中一种情况 Python 中的`TypeError: 'str' object is not callable`错误。

`TypeError: 'str' object is not callable`错误主要发生在以下情况:

*   您将名为`str`的变量作为参数传递给`str()`函数。
*   当你像调用函数一样调用一个字符串时。

在接下来的小节中，您将看到引发`TypeError: 'str' object is not callable`错误的代码示例，以及如何修复它们。

## 例 1——如果在 Python 中使用`str`作为变量名会发生什么？

在本节中，您将看到当您使用名为`str`的变量作为`str()`函数的参数时会发生什么。

`str()`函数用于将某些值转换成字符串。`str(10)`将整数`10`转换为字符串。

下面是第一个代码示例:

```
str = "Hello World"

print(str(str))
# TypeError: 'str' object is not callable
```

在上面的代码中，我们创建了一个值为“Hello World”的变量`str`。我们将变量作为参数传递给了`str()`函数。

结果是`TypeError: 'str' object is not callable`错误。发生这种情况是因为我们使用了编译器已经识别为不同的变量名。

要解决这个问题，可以将变量重命名为 Python 中未预定义的关键字。

这里有一个快速解决问题的方法:

```
greetings = "Hello World"

print(str(greetings))
# Hello World
```

现在代码可以完美地工作了。

## 例 2——如果你像 Python 中的函数一样调用一个字符串，会发生什么？

调用一个字符串，就好像它是 Python 中的一个函数一样，会引发`TypeError: 'str' object is not callable`错误。

这里有一个例子:

```
greetings = "Hello World"

print(greetings())
# TypeError: 'str' object is not callable
```

在上面的例子中，我们创建了一个名为`greetings`的变量。

在将它打印到控制台时，我们在变量名后使用了括号——这是调用函数时使用的语法:`greetings()`。

这导致编译器抛出`TypeError: 'str' object is not callable`错误。

您可以通过删除括号来轻松解决这个问题。

这对于不是函数的其他数据类型都是一样的。给它们加上括号也会产生同样的错误。

所以我们的代码应该这样工作:

```
greetings = "Hello World"

print(greetings)
# Hello World
```

## 摘要

在本文中，我们讨论了 Python 中的`TypeError: 'str' object is not callable`错误。

我们讨论了为什么会出现这个错误以及如何修复它。

为了避免在代码中出现此错误，您应该:

*   避免用 Python 内置的关键字来命名变量。
*   永远不要通过给变量加括号来像函数一样调用它们。

编码快乐！