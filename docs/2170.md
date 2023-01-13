# Python For Loop - For i in Range 示例

> 原文：<https://www.freecodecamp.org/news/python-for-loop-for-i-in-range-example/>

循环是任何编程语言的主要控制结构之一，Python 也不例外。

在本文中，我们将通过 Python 的`range()`函数来看几个使用`for`循环的例子。

## Python 中的 For 循环

`for`循环为一组值重复一部分代码。

正如在 [Python 的文档](https://docs.python.org/3/tutorial/controlflow.html#for-statements)中所讨论的，`for`循环的工作方式与在 JavaScript 或 c 语言中略有不同

一个`for`循环将迭代器变量设置为提供的列表、数组或字符串中的每个值，并为迭代器变量的每个值重复`for`循环体中的代码。

在下面的例子中，我们使用一个`for`循环来打印数组中的每个数字。

```
# Example for loop
for i in [1, 2, 3, 4]:
    print(i, end=", ") # prints: 1, 2, 3, 4,
```

我们也可以在`for`循环体中包含更复杂的逻辑。在这个例子中，我们打印了一个基于迭代器变量值的小计算的结果。

```
# More complex example
for i in [1, 3, 5, 7, 9]:
    x = i**2 - (i-1)*(i+1)
    print(x, end=", ") # prints 1, 1, 1, 1, 1, 
```

当我们的`for`循环的数组中的值是连续的时，我们可以使用 Python 的`range()`函数，而不是写出数组的内容。

## Python 中的 Range 函数

`range()`函数根据函数的参数提供一个整数序列。关于`range()`函数的更多信息可以在 [Python 的文档](https://docs.python.org/3/library/stdtypes.html#range)中找到。

```
range(stop)
range(start, stop[, step]) 
```

`start`参数是范围中的第一个值。如果调用`range()`时只有一个参数，那么 Python 会假设`start = 0`。

`stop`参数是范围的上限。重要的是要认识到，这个上限值不包括在范围内。

在下面的例子中，我们有一个范围，从默认值`0`开始，包括小于`5`的整数。

```
# Example with one argument
for i in range(5):
    print(i, end=", ") # prints: 0, 1, 2, 3, 4, 
```

在我们的下一个例子中，我们设置`start = -1`并再次包含小于`5`的整数。

```
# Example with two arguments
for i in range(-1, 5):
    print(i, end=", ") # prints: -1, 0, 1, 2, 3, 4, 
```

可选的`step`值控制范围内数值之间的增量。默认，`step = 1`。

在我们最后的例子中，我们使用从`-1`到`5`的整数范围，并设置`step = 2`。

```
# Example with three arguments
for i in range(-1, 5, 2):
    print(i, end=", ") # prints: -1, 1, 3, 
```

## 摘要

在本文中，我们看了 Python 中的`for`循环和`range()`函数。

`for`循环对列表、数组、字符串或`range()`中的所有值重复一段代码。

我们可以使用一个`range()`来简化编写一个`for`循环。必须指定`range()`的`stop`值，但是我们也可以修改`range()`中整数之间的`start` ing 值和`step`。