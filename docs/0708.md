# TypeError:不能将序列与“float”类型的非 int 相乘[已解决]

> 原文：<https://www.freecodecamp.org/news/typeerror-cant-multiply-sequence-by-non-int-of-type-float-solved/>

编码时，你可能会遇到错误。在大多数情况下，错误消息有助于您理解错误是关于什么的。理解错误信息是解决错误的步骤之一。

在本文中，我们将讨论 Python 中的一个错误——“type error:不能将序列乘以‘float’类型的非整数”错误。

我们将了解这是什么类型的错误，为什么会发生，以及如何用不同的解决方案和代码示例来修复它。

## 为什么会出现错误“TypeError:无法将序列乘以' float '类型的非 int”？

好吧，为了理解这个错误，让我们看一下错误消息，看看我们能从中抓住什么。第一个词说 **TypeError** 。

那么什么是类型错误呢？当操作中涉及的数据类型与所述操作不兼容时，Python 中会出现 TypeError。因此，当在操作中看到意外值时，就会发生这种情况。通过一些代码示例，您会更好地理解这一点。

错误消息的其余部分强调了单词“float”。让我们找出原因。

```
print("John " * 2)
# John John 
```

在上面的代码中，我们将一个字符串乘以一个整数(2)。这导致字符串被复制。

类似地，我们将一个元组乘以一个整数，如下所示:

```
names = ("John ", "Jane ")

print(names * 2)
# ('John ', 'Jane ', 'John ', 'Jane ')
```

我们在乘法运算后复制了元组中的值。元组被乘以 2。这是可能的，因为我们要乘以一个整数。

下面是我们使用浮点数做同样的事情时会发生的情况:

```
print("John " * 2.0)
# TypeError: can't multiply sequence by non-int of type 'float' 
```

```
names = ("John ", "Jane ")

print(names * 2.0)
# TypeError: can't multiply sequence by non-int of type 'float'
```

“TypeError:不能将序列乘以‘float’类型的非整数”错误被抛出。这是因为我们不能将一个字符串和一个浮点数相乘，或者将一个元组和一个浮点数相乘。

通常，当我们对不应乘以浮点数的数据类型执行操作时，会出现此错误。

在下一节中，我们将看看解决这个错误的一些方法。

## 如何解决 TypeError:无法将序列乘以“float”类型的非 int 错误

本节将被分成几个小节，因为有各种方法可以解决这个错误。每个小节都有一个代码示例。

### 解决方案 1–将浮点数转换为整数

要解决“TypeError:不能将序列乘以‘float’类型的非整数”错误，我们可以将 float 转换为整数。

这里有一个例子:

```
names = ("John ", "Jane ")

print(names * int(2.0))
# ('John ', 'Jane ', 'John ', 'Jane ')
```

现在我们得到了预期的结果——元组被复制了。

我们将浮点数传入一个`int()`函数，以便将它从浮点数转换成整数。这就消除了错误。

### 解决方案 2 -将字符串转换为整数或浮点数

当字符串是数值时，这种解决方案很重要。我们可能试图获取用户输入，然后对其执行乘法运算。

```
userId = "10"

print(float(userId) * 2.0)
# 20.0 
```

为了在不出错的情况下执行操作，我们使用`float()`函数将字符串转换为浮点数据类型。

我们同样可以使用`int()`函数将其转换为整数，以获得相同的结果。那就是:

```
userId = "10"

print(int(userId) * 2.0)
# 20.0 
```

### 解决方案 3–将用户输入转换为整数或浮点数

这种解决方案适用于我们从用户那里获得输入后执行操作的情况。

看看下面的例子:

```
userId = input("Enter user ID: ")
print(userId * 2.0)
# TypeError: can't multiply sequence by non-int of type 'float'
```

这将在输入一个数字作为用户 ID 后引发一个错误。

为了解决这个问题，我们将在执行操作之前转换用户输入的结果。

我们可以这样做:

```
userId = int(input("Enter user ID: "))
print(userId * 2.0) 
```

现在，当用户键入一个数字时，程序应该可以完美地运行，因为它将被转换为一个整数:`int(input("Enter user ID: "))`。

我们可以使用`float()`函数做同样的事情，该函数将用户输入转换成浮点数。

这里有一个例子:

```
userId = float(input("Enter user ID: "))
print(userId * 2.0) 
```

## 结论

在本文中，我们讨论了 Python 中的“TypeError:不能将序列乘以‘float’类型的非整数”错误。

我们首先通过定义 TypeError 来理解错误消息的含义。

然后我们看到了一些例子，展示了为什么 float 数据类型会导致这个错误。

为了修复该错误，我们查看了三种不同的解决方案，并提供了与可能发生该错误的不同情况相关的示例。

编码快乐！