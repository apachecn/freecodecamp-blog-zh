# Python 注释块——如何用 Python 注释掉代码

> 原文：<https://www.freecodecamp.org/news/python-comment-block-how-to-comment-out-code-in-python/>

在本文中，我们将讨论 Python 中的注释，为什么它们很重要，以及如何在代码中有效地使用它们。

## 何时使用注释

在这一部分，我们将讨论一些注释的一般用例。这些不仅适用于 Python，也适用于大多数编程语言。

以下是注释代码的一些主要原因:

*   **阻止代码执行**:在某些情况下，你需要阻止你的部分代码运行。这可能是因为您目前还不需要这些代码，但是您还是想为将来的功能添加它们。
*   可读性:这非常重要——不仅对我们自己，对其他开发者也是如此。我们可以用注释来解释每个代码块的作用。当其他开发人员阅读我们的代码时，这很有用，因为这使得理解代码的每个部分是干什么的变得很容易。

## 如何用 Python 注释掉代码

每种编程语言的注释语法都不同。在这一节中，我们将看到如何使用 Python 添加注释。

Python 中的注释以`#`符号开始。这里有一个例子:

```
#The code below prints Hello World! to the console
print("Hello World!")
```

在上面的代码中，我使用了一个注释来解释代码的作用。当代码正在执行时，解释器将忽略注释并运行`print()`函数。

我们也可以注释掉实际的代码。那就是:

```
# print("Hello World!")
print("Happy coding!") 
```

当我们运行代码时，只有第二行会被解释。

您不必总是将注释放在它们所引用的代码行的上方。你也可以把它们放在同一行。那就是:

```
print("Hello world") # Prints Hello World
```

## 如何用 Python 写多行注释

在这一节中，我们将看到如何写跨多行的注释。

与大多数其他编程语言不同，Python 没有用于创建多行注释的内置语法。

幸运的是，有两种方法可以解决这个问题。这是第一个:

```
# When this code runs,
# you will see Hello World! 
# in the console. 
print("Hello world")
```

在上面，我们在每一行上放置了`#`符号来继续写我们的注释。

在下一个例子中，我们将使用多行字符串(以三个引号开始和结束)来嵌套我们的注释。

当您在 Python 中使用多行字符串而没有将字符串赋给变量时，这部分代码将被忽略。

这里有一个例子:

```
"""
When this code runs,
you will see Hello World! 
in the console.
"""
print("Hello World!")
```

## 结论

在本文中，我们了解了注释代码的重要性以及如何使用注释。我们还看到了如何创建多行注释。

编码快乐！