# Python 中的嵌套函数

> 原文：<https://www.freecodecamp.org/news/nested-functions-in-python/>

嵌套函数只是另一个函数中的一个函数，有时被称为“内部函数”。使用嵌套函数有很多原因，我们将在本文中讨论最常见的原因。

### 如何定义嵌套函数

要定义嵌套函数，只需使用`def`关键字初始化一个函数中的另一个函数:

```
def greeting(first, last):
  # nested helper function
  def getFullName():
    return first + " " + last

  print("Hi, " + getFullName() + "!")

greeting('Quincy', 'Larson')
```

**输出:**

```
Hi, Quincy Larson!
```

如您所见，嵌套的`getFullName`函数可以访问外部`greeting`函数的参数`first`和`last`。这是嵌套函数的一个常见用例——作为一个更复杂的外部函数的辅助函数。

## 使用嵌套函数的原因

虽然使用嵌套函数有很多合理的理由，但最常见的是封装和闭包/工厂函数。

### 数据封装

有时候，您希望防止代码的其他部分访问某个函数或它可以访问的数据，这样您就可以*将它封装在另一个函数中。*

当你像这样嵌套一个函数时，它在全局范围内是隐藏的。由于这种行为，数据封装有时被称为**数据隐藏**或**数据隐私**。例如:

```
def outer():
  print("I'm the outer function.")
  def inner():
    print("And I'm the inner function.")
  inner()

inner()
```

**输出:**

```
Traceback (most recent call last):
  File "main.py", line 16, in <module>
    inner()
NameError: name 'inner' is not defined
```

在上面的代码中，`inner`函数只能在函数`outer`中使用。如果你试图从函数外部调用`inner`，你会得到上面的错误。

相反，您必须像这样调用`outer`函数:

```
def outer():
  print("I'm the outer function.")
  def inner():
    print("And I'm the inner function.")
  inner()

outer()
```

**输出:**

```
I'm the outer function.
And I'm the inner function.
```

### 关闭

但是如果外部函数返回内部函数本身，而不是像上面的例子那样调用它，会发生什么呢？在这种情况下，你会有所谓的关闭。

以下是在 Python 中创建闭合所需满足的条件:

以下是在 Python 中创建闭包所需的条件:

> 1.必须有嵌套函数
> 
> 2。内部函数必须引用封闭范围
> 3 中定义的值。封闭函数必须返回嵌套函数
> T5——来源:[https://stackabuse.com/python-nested-functions/](https://stackabuse.com/python-nested-functions/)

下面是一个简单的结束示例:

```
def num1(x):
  def num2(y):
    return x + y
  return num2

print(num1(10)(5))
```

**输出:**

```
15
```

闭包使得将数据传递给内部函数成为可能，而不需要像本文开头的`greeting`例子那样，先用参数将数据传递给外部函数。它们还使得从封装外部函数的外部调用内部函数成为可能。所有这些都有前面提到的数据封装/隐藏的好处。

既然您已经理解了如何以及为什么要在 Python 中嵌套函数，那么就去用最好的方法来嵌套它们吧。