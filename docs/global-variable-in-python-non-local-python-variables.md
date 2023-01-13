# Python 中的全局变量–非局部 Python 变量

> 原文：<https://www.freecodecamp.org/news/global-variable-in-python-non-local-python-variables/>

在 Python 和大多数编程语言中，在函数外部声明的变量被称为全局变量。您可以在函数内部和外部访问这些变量，因为它们具有全局范围。

下面是一个全局变量的例子:

```
x = 10 

def showX():
    print("The value of x is", x)

showX()
# The value of x is 10
```

上面代码中的变量`x`是在函数`x = 10`外声明的。

使用`showX()`函数，我们仍然能够访问`x`,因为它是在全局范围内声明的。

让我们来看另一个例子，它展示了当我们在一个函数中声明一个变量并试图在其他地方访问它时会发生什么。

```
def X():
    x = 10 

X()

def showX():
    print("The value of x is", x)

showX()
NameError: name 'x' is not defined
```

在上面的例子中，我们在一个函数中声明了`x`,并试图在另一个函数中访问它。这导致了一个名称错误，因为`x`不是全局定义的。

函数内部定义的变量称为局部变量。它们的值只能在声明它们的函数中使用。

您可以使用关键字`global`改变局部变量的范围——我们将在下一节中讨论。

## Python 中的`global`关键字是用来做什么的？

使用 global 关键字主要有两个原因:

*   修改全局变量的值。
*   使局部变量可以在局部范围之外访问。

让我们看看每个场景的一些示例，以帮助您更好地理解。

### 示例 1——使用`global`关键字修改全局变量

在上一节中，我们声明了一个全局变量，我们没有试图改变变量的值。我们所做的只是在一个函数中访问并打印它的值。

让我们试着改变一个全局变量的值，看看会发生什么:

```
x = 10 

def showX():
    x = x + 2
    print("The value of x is", x)

showX()
# local variable 'x' referenced before assignment
```

正如你在上面看到的，当我们试图给`x`的值加 2 时，我们得到了一个错误。这是因为我们只能访问而不能修改`x`。

为了解决这个问题，我们使用了`global`变量。方法如下:

```
x = 10 

def showX():
    global x
    x = x + 2
    print("The value of x is", x)

showX()
# The value of x is 12
```

使用上面代码中的关键字`global`,我们能够修改`x`,并在它的初始值上加 2。

### 示例 2——如何使用`global`关键字使局部变量在局部范围之外可访问

当我们在一个函数中创建一个变量时，不可能在另一个函数中使用它的值，因为编译器不能识别这个变量。

下面是我们如何使用`global`关键字来解决这个问题:

```
def X():
    global x
    x = 10 

X()

def showX():
    print("The value of x is", x)

showX()
# The value of x is 10
```

为了使`x`能够在其本地范围之外被访问，我们使用`global`关键字:`global x`来声明它。

之后，我们给`x`赋值。然后我们调用我们用来声明它的函数:`X()`

当我们调用打印在`X()`函数中声明的`x`的值的`showX()`函数时，我们没有得到一个错误，因为`x`有一个全局范围。

## 摘要

在本文中，我们讨论了 Python 中的全局和局部变量。

这些例子展示了如何声明全局和局部变量。

我们还讨论了`global`关键字，它允许你修改一个全局变量的值，或者使一个局部变量在其作用域之外可访问。

编码快乐！