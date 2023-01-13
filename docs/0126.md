# TypeError:模块对象不可调用[Python 错误已解决]

> 原文：<https://www.freecodecamp.org/news/typeerror-module-object-is-not-callable-python-error-solved/>

在本文中，我们将讨论 Python 中的“类型错误:‘模块’对象不可调用”错误。

我们将从定义错误消息中的一些关键字开始— `module`和`callable`。

然后，您将看到一些引发错误的示例，以及如何修复它。

如果你已经知道什么是模块，以及调用一个函数或方法意味着什么，你可以随意跳过接下来的两节。

## 什么是编程中的模块？

在模块化编程中，模块只是包含执行某项任务所需的类似功能的文件。

模块帮助我们根据功能对代码进行分离和分组。例如，你可以有一个名为`math-ops.py`的模块，它只包含与数学运算相关的代码。

这使得阅读、重用和理解代码变得更加容易。要在代码库的不同部分使用一个模块，您必须导入它才能访问模块中定义的所有功能。

在 Python 中，有很多内置的模块，像`os`、`math`、`time`等等。

下面的例子展示了如何使用`math`模块:

```
import math

print(math.sqrt(25))
//5.0
```

正如你在上面看到的，我们在使用`math`模块之前做的第一件事就是导入它:`import math`。

然后我们使用模块的`sqrt`方法，该方法返回一个数的平方根:`math.sqrt(25)`。

我们得到 25 的平方根只需要两行代码。实际上，[`math`模块有超过 3500 行代码](https://github.com/python/cpython/blob/main/Modules/mathmodule.c)。

这应该有助于您理解什么是模块以及它是如何工作的。您也可以创建自己的模块(我们将在本文的后面完成)。

## “类型错误:模块对象不可调用”错误中的`callable`是什么意思？

在 Python 和大多数编程语言中，动词“call”与执行函数或方法中编写的代码相关联。

在同一动作中经常使用的其他类似术语是“调用”和“发射”。

这里有一个 Python 函数，它输出“微笑！”到控制台:

```
def smile():
    print("Smile!")
```

如果您运行上面的代码，将不会向控制台打印任何内容，因为函数`smile`尚未被调用、调用或触发。

为了执行函数，我们必须写出函数名，后面跟着括号。那就是:

```
def smile():
    print("Smile!")

smile()
# Smile!
```

如果没有括号，函数将不会执行。

现在你应该明白术语`callable`在错误消息中的意思了:“TypeError: 'module '对象不可调用”。

## Python 中的“TypeError: module 对象不可调用”错误是什么意思？

最后两节帮助我们理解了“type error:' module ' object is not callable”错误消息中的一些关键字。

简单来说，“TypeError: 'module '对象不可调用”错误是指模块不能像函数或方法一样被调用。

## 如何修复 Python 中的“类型错误:模块对象不可调用”错误

通常有两种方法可以引发“type error:' module ' object is not callable”错误:调用内置或第三方模块，以及调用模块代替函数。

#### 错误示例#1

```
import math

print(math(25))
# TypeError: 'module' object is not callable 
```

在上面的例子中，我们调用了`math`模块(通过使用括号`()`)并传递了 25 作为参数，希望执行特定的数学运算:`math(25)`。但是我们得到了错误。

为了解决这个问题，我们可以利用`math`模块提供的任何数学方法。我们将使用`sqrt`方法:

```
import math

print(math.sqrt(25))
# 5.0
```

#### 错误示例 2

对于这个例子，我将创建一个模块来计算两个数字:

```
# add.py

def add(a,b):
    print(a+b) 
```

上面模块的名字是`add`，可以从文件名 **add.py** 中导出。

让我们从另一个文件中的`add`模块导入`add()`函数:

```
# main.py
import add

add(2,3)
# TypeError: 'module' object is not callable
```

您一定想知道为什么即使我们导入了这个模块，我们还是会得到这个错误。

嗯，我们导入的是模块，不是函数。这就是原因。

要解决这个问题，您必须在导入模块时指定函数的名称:

```
from add import add

add(2,3)
# 5
```

我们具体来说:`from add import add`。这和说“从 add.py 模块导入`add`函数”是一样的。

你现在可以使用 **main.py** 文件中的`add()`函数。

## 摘要

在本文中，我们讨论了 Python 中的“type error:' module ' object is not callable”错误。

这个错误主要发生在我们调用一个模块的时候，就好像它是一个函数或者方法一样。

我们首先讨论了什么是编程中的模块，以及调用函数意味着什么——这有助于我们理解是什么导致了错误。

然后，我们给出了一些例子，展示了出现的错误以及如何修复它。

编码快乐！