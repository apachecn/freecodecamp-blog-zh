# NameError:未定义名称 plot _ cases _ simple–如何修复此 Python 错误

> 原文：<https://www.freecodecamp.org/news/nameerror-name-plot_cases_simple-is-not-defined-how-to-fix-this-python-error/>

修复编码错误的第一步是理解错误。尽管有些错误消息看起来令人困惑，但大多数都会帮助您修复错误。

在本文中，我们将讨论 Python 中属于 **NameError** 类别的一个错误。

您将看到什么是 **NameError** ，一些代码示例显示错误是如何/为什么发生的，以及如何修复它们。

## Python 中什么是 NameError？

在 Python 中，当你试图使用一个不存在或没有被有效使用的变量、函数或模块时，就会出现 **NameError** 。

导致此错误的一些常见错误有:

*   使用尚未定义的变量或函数名。
*   调用变量/函数时拼写错误。
*   使用 Python 模块而不导入模块，等等。

## 如何修复 Python 中的“名称错误:名称未定义”

在本节中，您将看到如何修复 Python 中的“NameError: Name is Not Defined”错误。

我将这一节分成了几个小节，以显示上面使用变量、函数和模块时的错误。

我们将从引发错误的代码块开始，然后看看如何修复它们。

### 例子#1 -变量名称没有在 Python 中定义

```
name = "John"

print(age)
# NameError: name 'age' is not defined
```

在上面的代码中，我们定义了一个`name`变量，但试图打印尚未定义的`age`。

我们得到一个错误消息:`NameError: name 'age' is not defined`表明`age`变量不存在。

要解决这个问题，我们可以创建变量，我们的代码将运行良好。方法如下:

```
name = "John"
age = 12

print(age)
# 12
```

现在`age`的值被打印出来。

同样，当我们拼错变量名时，也会出现同样的错误。这里有一个例子:

```
name = "John"

print(nam)
# NameError: name 'nam' is not defined
```

在上面的代码中，我们写的是`nam`而不是`name`。要修复这样的错误，您只需以正确的方式拼写变量名。

### 示例 2——函数名没有在 Python 中定义

```
def sayHello():
    print("Hello World!")

sayHelloo()
# NameError: name 'sayHelloo' is not defined
```

在上面的例子中，我们在调用函数时多加了一个 o—`sayHelloo()`而不是`sayHello()`。

我们得到的错误:`NameError: name 'sayHelloo' is not defined`。像这样的拼写错误很容易被忽略。错误消息通常有助于解决这个问题。

调用该函数的正确方法如下:

```
def sayHello():
    print("Hello World!")

sayHello()
# Hello World! 
```

正如我们在上一节中看到的，调用一个尚未定义的变量会引发一个错误。这同样适用于函数。

这里有一个例子:

```
def sayHello():
    print("Hello World!")

sayHello()
# Hello World!

addTWoNumbers()
# NameError: name 'addTWoNumbers' is not defined
```

在上面的代码中，我们调用了一个函数—`addTWoNumbers()`——这个函数还没有在程序中定义。要解决这个问题，您可以在需要时创建函数，或者在不相关时删除函数。

请注意，在创建函数之前调用它将会抛出同样的错误。那就是:

```
sayHello()

def sayHello():
    print("Hello World!")

# NameError: name 'sayHello' is not defined
```

所以你应该在调用函数之前定义它们。

### 示例 3——在 Python 中使用模块而不导入模块错误

```
x = 5.5

print(math.ceil(x))
# NameError: name 'math' is not defined
```

在上面的例子中，我们使用 Python `math.ceil`方法，而没有导入`math`模块。

产生的错误如下:`NameError: name 'math' is not defined`。这是因为解释器没有识别出关键字`math`。

和 Python 中的其他数学方法一样，我们必须首先导入`math`模块来使用它。

这里有一个解决方法:

```
import math

x = 5.5

print(math.ceil(x))
# 6
```

在代码的第一行，我们导入了`math`模块。现在，当您运行上面的代码时，应该会返回 6。

## 摘要

在本文中，我们讨论了 Python 中的“NameError: Name is Not Defined”错误。

我们首先在 Python 中定义了什么是名称错误。

然后，我们看到了一些在 Python 中使用变量、函数和模块时可能引发 **NameError** 的例子。每个例子都分为几个部分，展示了如何修复错误。

编码快乐！