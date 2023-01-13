# TypeError:只有大小为 1 的数组可以转换为 Python 标量

> 原文：<https://www.freecodecamp.org/news/how-to-fix-typeerror-only-size-1-arrays-can-be-converted-to-python-scalars/>

在 Python 中，当处理数组和某些数学概念如矩阵和线性代数时，可以使用`numpy`库。

但是就像学习和使用编程语言的其他方面一样，错误是不可避免的。

在本文中，您将了解如何修复“TypeError:只有大小为 1 的数组才能转换为 Python 标量”错误，这种错误通常发生在使用`numpy`库时。

## Python 中的`TypeError: only size-1 arrays can be converted to Python scalars`错误是什么原因造成的？

当我们将一个数组传递给一个只接受一个参数的方法时，就会出现“type error:only size-1 arrays can be convert to Python scalar”错误。

这里有一个例子:

```
import numpy as np

y = np.array([1, 2, 3, 4])
x = np.int(y)

print(x)

# TypeError: only size-1 arrays can be converted to Python scalars 
```

上面的代码抛出了“TypeError:只有大小为 1 的数组才能转换成 Python 标量”错误，因为我们将`y`数组传递给了 NumPy `int()`方法。该方法只能接受一个参数。

在下一节中，您将看到这个错误的一些解决方案。

## 如何修复 Python 中的`TypeError: only size-1 arrays can be converted to Python scalars`错误

有两种解决“TypeError:只有大小为 1 的数组可以转换为 Python 标量”错误的通用方法。

### 解决方案# 1–使用`np.vectorize()`功能

`np.vectorize()`函数可以接受一个序列/数组作为它的参数。当打印出来时，它返回一个数组。

这里有一个例子:

```
import numpy as np

vector = np.vectorize(np.int_)
y = np.array([2, 4, 6, 8])
x = vector(y)

print(x)
# [2, 4, 6, 8]
```

在上面的例子中，我们创建了一个`vector`变量，它将“矢量化”传递给它的任何参数:`np.vectorize(np.int_)`。

然后，我们创建了一个数组，并将其存储在`y`变量中:`np.array([2, 4, 6, 8])`。

使用我们最初创建的`vector`变量，我们将`y`数组作为参数传递:`x = vector(y)`。

当打印出来时，我们得到了数组— `[2, 4, 6, 8]`。

### 解决方案# 2–使用`map()`功能

在这种情况下,`map()`函数接受两个参数 NumPy 方法和数组。

```
import numpy as np

y = np.array([2, 4, 6, 8])
x = np.array(list(map(np.int_, y)))

print(x)
# [2, 4, 6, 8]
```

在上面的例子中，我们将`map()`函数嵌套在一个`list()`方法中，这样我们就可以将数组作为一个列表而不是一个 map 对象返回。

### 解决方案# 3–使用`astype()`方法

我们可以使用`astype()`方法将 NumPy 数组转换成整数。这将防止引发“TypeError:只有大小为 1 的数组才能转换为 Python 标量”错误。

方法如下:

```
import numpy as np

vector = np.vectorize(np.int_)
y = np.array([2, 4, 6, 8])
x = y.astype(int)

print(x)
# [2 4 6 8]
```

## 摘要

在本文中，我们讨论了 Python 中的“TypeError:只有大小为 1 的数组才能转换为 Python 标量”错误。

当我们将一个数组作为参数传递给一个只接受一个参数的`numpy`方法时，就会引发这个问题。

为了修复这个错误，我们使用了不同的方法，比如`np.vectorize()`函数、`map()`函数和`astype()`方法。

编码快乐！