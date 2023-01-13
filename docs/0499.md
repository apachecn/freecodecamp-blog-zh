# //在 Python 中是什么意思？Python 中的运算符

> 原文：<https://www.freecodecamp.org/news/what-does-double-slash-mean-in-python/>

在 Python 中，使用双斜线`//`操作符来执行楼层划分。这个`//`操作符将第一个数字除以第二个数字，并将结果向下舍入到最接近的整数(或整数)。

在本文中，我将向您展示如何使用`//`运算符，并将其与常规除法进行比较，这样您就可以看到它是如何工作的。

然而，事情并没有就此结束——您还将学习一种 Python 数学方法，它与双斜线`//`运算符同义。

## 我们将涵盖的内容

*   [运算符`//`的基本语法](#thebasicsyntaxoftheoperator)
*   [楼层划分示例](#examplesoffloordivision)
*   [双斜线`//`运算符的工作原理类似于`math.floor()`](#thedoubleslashoperatorworkslikemathfloor)
*   [双斜线`//`运算符如何在幕后工作](#howthedoubleslashoperatorworksbehindthescenes)
*   [结论](#conclusion)

## `//`运算符的基本语法

使用双斜线`//`操作符，你可以像在常规除法中一样操作。唯一的区别是，你使用了双斜线`//`，而不是单斜线`/`:

```
firstNum // secondNum 
```

## 楼层划分示例

在下面的例子中，12 除以 5 得到 2:

```
num1 = 12
num2 = 5
num3 = num1 // num2

print("floor division of", num1, "by", num2, "=", num3)
# Output: floor division of 12 by 5 = 2 
```

然而，常规的 12 除以 5 等于 2.4。即 2 余数 4:

```
num2 = 5
num3 = num1 / num2

print("normal division of", num1, "by", num2, "=", num3)
# Output: normal division of 12 by 5 = 2.4 
```

这向您展示了`//`运算符将两个数相除的结果向下舍入到最接近的整数。

即使小数点是 9，`//`操作符仍然会将结果向下舍入到最接近的整数。

```
num1 = 29 
num2 = 10 
num3 = num1 / num2
num4 = num1 // num2

print("normal division of", num1, "by", num2, "=", num3)
print("but floor division of", num1, "by", num2, "=", num4)

"""
Output:
normal division of 29 by 10 = 2.9
but floor division of 29 by 10 = 2
""" 
```

如果你用负数进行除法运算，结果仍然会向下取整。

为了让你对结果有所准备，向下舍入一个负数意味着远离 0。所以，-12 除以 5 得到-3。不要混淆——尽管乍一看这个数字似乎变得“更大”,但它实际上变得更小了(离零更远/更大的负数)。

```
num1 = -12
num2 = 5
num3 = num1 // num2

print("floor division of", num1, "by", num2, "=", num3)

# floor division of -12 by 5 = -3 
```

## 双斜线`//`操作符的工作方式类似于`math.floor()`

在 Python 中，`math.floor()`将一个数字向下舍入到最接近的整数，就像双斜线`//`操作符一样。

所以，`math.floor()`是`//`操作符的替代，因为它们在幕后做同样的事情。

这里有一个例子:

```
import math

num1 = 12
num2 = 5
num3 = num1 // num2
num4 = math.floor(num1 / num2)

print("floor division of", num1, "by", num2, "=", num3)
print("math.floor of", num1, "divided by", num2, "=", num4)

"""
Output:
floor division of 12 by 5 = 2
math.floor of 12 divided by 5 = 2
""" 
```

你可以看到`math.floor()`和`//`操作符做同样的事情。

## 双斜线`//`操作符如何在幕后工作

当您使用`//`操作符将两个数相除时，在幕后调用的方法是`__floordiv__()`。

您也可以直接使用这个`__floordiv__()`方法来代替`//`操作符:

```
num1 = 12
num2 = 5
num3 = num1 // num2
num4 = num1.__floordiv__(num2)

print("floor division of", num1, "by", num2, "=", num3)
print("using the floordiv method gets us the same value of", num4)

"""
Output:
floor division of 12 by 5 = 2
using the floordiv method gets us the same value of 2
""" 
```

## 结论

在本文中，您了解了如何使用双斜线`//`操作符，以及它是如何在幕后工作的。

此外，您还了解了`//`操作符–`math.floor()`和`__floordiv__()`方法的两种替代方法。

不要对使用哪个感到困惑。这三种执行地板划分工作的方式相同。但是我建议你使用双斜线`//`操作符，因为用它你可以输入更少的内容。

感谢您的阅读。