# Python 中科学计算的 NumPy 包的最终指南

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-the-numpy-scientific-computing-library-for-python/>

NumPy(读作“麻木派”)是当你开始学习 Python 时需要掌握的最重要的包之一。

众所周知，这个包有一个非常有用的数据结构，叫做 NumPy 数组。NumPy 还允许 Python 开发人员快速执行各种数值计算。

本教程将教你 NumPy 的基础知识，你可以用它来构建数字 Python 应用程序。

## **目录**

您可以使用下面的目录跳到本 NumPy 教程的特定部分:

*   [NumPy 简介](#introduction-to-numpy)
*   [NumPy 数组](#numpy-arrays)
*   [NumPy 方法和操作](#numpy-methods-and-operations)
*   [数字索引和分配](#numpy-indexing-and-assignment)
*   [最终想法&特价](#final-thoughts-special-offer)

## **NumPy 简介**

在这一节中，我们将介绍 Python 中的 [NumPy 库](https://nickmccullum.com/advanced-python/numpy/)。

### **NumPy 是什么？**

NumPy 是用于科学计算的 Python 库。NumPy 代表数字 Python。以下是官方对该图书馆的描述，来自其网站:

**“NumPy 是使用 Python 进行科学计算的基础包。其中包含了**

*   **一个强大的 N 维数组对象**
*   **精密(广播)功能**
*   **集成 C/C++和 Fortran 代码的工具**
*   **有用的线性代数、傅立叶变换和随机数功能**

除了其明显的科学用途，NumPy 还可以用作通用数据的高效多维容器。可以定义任意的数据类型。这使得 NumPy 可以无缝、快速地与各种数据库集成。

**NumPy 在 [BSD 许可](https://numpy.org/license.html#license)下被许可，使得重用几乎没有限制。”**

NumPy 是一个如此重要的 Python 库，以至于还有其他库(包括 pandas)完全构建在 NumPy 之上。

### **NumPy 的主要优势**

NumPy 的主要好处是它允许极快的数据生成和处理。NumPy 有自己的内置数据结构，称为`array`，类似于普通的 Python `list`，但是可以更有效地存储和操作数据。

### **我们将了解 NumPy**

高级 Python 实践者将花更多的时间与熊猫打交道，而不是与 NumPy 打交道。尽管如此，考虑到 pandas 是基于 NumPy 构建的，理解 NumPy 库最重要的方面是很重要的。

在接下来的几节中，我们将介绍 NumPy 库的以下信息:

*   NumPy 数组
*   数字索引和分配
*   NumPy 方法和操作

### **继续前进**

让我们继续学习 NumPy 数组，这是每个 NumPy 从业者都必须熟悉的核心数据结构。

## **NumPy 数组**

在本节中，我们将学习 [NumPy 数组](https://nickmccullum.com/advanced-python/numpy-arrays/)。

### **什么是 NumPy 数组？**

NumPy 数组是使用 NumPy 库存储数据的主要方式。它们类似于 Python 中的普通列表，但是具有更快的速度和更多内置方法的优势。

NumPy 数组是通过调用 NumPy 库中的`array()`方法创建的。在方法中，应该传入一个列表。

下面显示了一个基本 NumPy 数组的示例。注意，虽然我在这个代码块的开头运行了`import numpy as np`语句，但为了简洁起见，它将被排除在本节的其他代码块之外。

```
import numpy as np

sample_list = [1, 2, 3]

np.array(sample_list) 
```

该代码块的最后一行将产生如下所示的输出。

```
array([1,2,3]) 
```

`array()`包装器表明这不再是一个普通的 Python 列表。相反，它是一个 NumPy 数组。

### **两种不同类型的 NumPy 数组**

有两种不同类型的 NumPy 数组:向量和矩阵。

向量是一维 NumPy 数组，如下所示:

```
my_vector = np.array(['this', 'is', 'a', 'vector']) 
```

矩阵是二维数组，通过将一组列表传递给`np.array()`方法来创建。下面是一个例子。

```
my_matrix = [[1, 2, 3],[4, 5, 6],[7, 8, 9]]

np.array(my_matrix) 
```

您还可以扩展 NumPy 数组来处理三维、四维、五维、六维或更高维的数组，但这种情况很少见，而且很大程度上超出了本课程的范围(毕竟这是一门关于 Python 编程的课程，而不是线性代数)。

### **NumPy 数组:内置方法**

NumPy 数组带有许多有用的内置方法。我们将在本节的剩余部分详细讨论这些方法。

#### **如何使用 NumPy 在 Python 中获得一系列数字**

NumPy 有一个很有用的方法叫做`arange`，它接受两个数字并给出一个大于或等于第一个数字(`>=`)小于第二个数字(`<`)的整数数组。

下面是一个`arange`方法的例子。

```
np.arange(0,5)

#Returns array([0, 1, 2, 3, 4]) 
```

您还可以在`arange`方法中包含第三个变量，为函数返回提供步长。作为第三个变量传入的`2`将返回该范围内的第二个数字，作为第三个变量传入的`5`将返回该范围内的第五个数字，依此类推。

下面是在`arange`方法中使用第三个变量的例子。

```
np.arange(1,11,2)

#Returns array([1, 3, 5, 7, 9]) 
```

### **如何使用 NumPy 在 Python 中生成 1 和 0**

在编程时，你会不时地需要创建 1 或 0 的数组。NumPy 有内置的方法，允许你做这些。

我们可以使用 NumPy 的`zeros`方法创建零数组。你把你想要创建的整数个数作为函数的参数传入。下面是一个例子。

```
np.zeros(4)

#Returns array([0, 0, 0, 0]) 
```

你也可以用三维数组做类似的事情。例如，`np.zeros(5, 5)`创建一个包含全零的 5x5 矩阵。

我们可以使用类似的方法`ones`创建一个数组。下面是一个例子。

```
np.ones(5)

#Returns array([1, 1, 1, 1, 1]) 
```

#### **如何使用 NumPy 在 Python 中均匀划分一系列数字**

在许多情况下，你有一个数值范围，你想把这个数值范围平均分成几个区间。NumPy 的`linspace`方法就是为了解决这个问题而设计的。`linspace`接受三个参数:

1.  间隔的开始
2.  幕间休息结束
3.  您希望将时间间隔分成的子时间间隔的数量

下面是一个`linspace`方法的例子。

```
np.linspace(0, 1, 10)

#Returns array([0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]) 
```

#### **如何使用 NumPy 在 Python 中创建单位矩阵**

任何学过线性代数的人都会熟悉一个‘单位矩阵’的概念，它是一个对角值都是`1`的方阵。NumPy 有一个内置函数，它接受一个参数来构建单位矩阵。功能是`eye`。

例子如下:

```
np.eye(1)

#Returns a 1x1 identity matrix

np.eye(2) 

#Returns a 2x2 identity matrix

np.eye(50)

#Returns a 50x50 identity matrix 
```

#### **如何使用 NumPy 在 Python 中创建随机数**

NumPy 内置了许多方法，允许您创建随机数数组。这些方法都是以`random`开头的。下面是几个例子:

```
np.random.rand(sample_size)

#Returns a sample of random numbers between 0 and 1.

#Sample size can either be one integer (for a one-dimensional array) or two integers separated by commas (for a two-dimensional array).

np.random.randn(sample_size)

#Returns a sample of random numbers between 0 and 1, following the normal distribution.

#Sample size can either be one integer (for a one-dimensional array) or two integers separated by commas (for a two-dimensional array).

np.random.randint(low, high, sample_size)

#Returns a sample of integers that are greater than or equal to 'low' and less than 'high' 
```

#### **如何重塑 NumPy 数组**

取一个具有特定维度的数组并将它转换成不同的形状是很常见的。例如，您可能有一个包含 10 个元素的一维数组，并希望将其切换为 2×5 的二维数组。

下面是一个例子:

```
arr = np.array([0,1,2,3,4,5])

arr.reshape(2,3) 
```

该操作的输出是:

```
array([[0, 1, 2],

       [3, 4, 5]]) 
```

注意，为了使用`reshape`方法，原始数组的元素数量必须与您试图重新整形的数组的元素数量相同。

如果您对 NumPy 数组的当前形状感兴趣，可以使用 NumPy 的`shape`属性来确定它的形状。使用我们之前的`arr`变量结构，下面是一个如何调用`shape`属性的例子:

```
arr = np.array([0,1,2,3,4,5])

arr.shape

#Returns (6,) - note that there is no second element since it is a one-dimensional array

arr = arr.reshape(2,3)

arr.shape

#Returns (2,3) 
```

您还可以在一行中组合使用`reshape`方法和`shape`属性，如下所示:

```
arr.reshape(2,3).shape

#Returns (2,3) 
```

#### **如何求 NumPy 数组的最大值和最小值**

在结束本节之前，让我们学习四种用于识别 NumPy 数组中最大值和最小值的有用方法。我们将使用这个数组:

```
simple_array = [1, 2, 3, 4] 
```

我们可以使用`max`方法找到 NumPy 数组的最大值。下面是一个例子。

```
simple_array.max()

#Returns 4 
```

我们还可以使用`argmax`方法来查找 NumPy 数组中最大值的索引。当您想要找到最大值的位置，但不一定关心它的值是多少时，这很有用。

下面是一个例子。

```
simple_array.argmax()

#Returns 3 
```

类似地，我们可以使用`min`和`argmin`方法来查找 NumPy 数组中最小值的值和索引。

```
simple_array.min()

#Returns 1

simple_array.argmin()

#Returns 0 
```

### **继续前进**

在本节中，我们讨论了 NumPy 数组的各种属性和方法。在下一节中，我们将继续解决一些 NumPy 数组练习问题。

## **NumPy 方法和操作**

在本节中，我们将学习 NumPy 库中包含的[各种操作。](https://nickmccullum.com/advanced-python/numpy-methods-operations/)

在本节中，我们将假设已经运行了`import numpy as np`命令。

### **本节使用的数组**

在本节中，我将使用所有示例中使用`np.arange`创建的长度为 4 的数组。

如果您想将我的数组与本节中使用的输出进行比较，下面是我如何创建和打印数组的:

```
arr = np.arange(4)

arr 
```

数组值如下。

```
array([0, 1, 2, 3]) 
```

### **如何使用数字**在 Python 中执行算术运算

NumPy 使得用数组执行算术变得非常容易。您可以使用数组和单个数字执行运算，也可以在两个 NumPy 数组之间执行运算。

我们在下面探索每一个主要的数学运算。

#### **加法**

向 NumPy 数组添加单个数字时，该数字会添加到数组中的每个元素。下面是一个例子:

```
2 + arr

#Returns array([2, 3, 4, 5]) 
```

您可以使用`+`操作符添加两个 NumPy 数组。数组是逐个元素添加的(意味着第一个元素加在一起，第二个元素加在一起，依此类推)。

下面是一个例子。

```
arr + arr

#Returns array([0, 2, 4, 6]) 
```

#### **减法**

像加法一样，减法是在 NumPy 数组的基础上逐个元素执行的。您可以在下面找到单个数字和另一个 NumPy 数组的示例。

```
arr - 10

#Returns array([-10,  -9,  -8,  -7])

arr - arr

#Returns array([0, 0, 0, 0]) 
```

#### **乘法运算**

对于单个数字和 NumPy 数组，也是逐个元素地执行乘法。

下面是两个例子。

```
6 * arr

#Returns array([ 0,  6, 12, 18])

arr * arr

#Returns array([0, 1, 4, 9]) 
```

#### **分部**

至此，您可能不会对 NumPy 数组上执行的除法是逐个元素执行的感到惊讶。将`arr`除以一个数字的例子如下:

```
arr / 2

#Returns array([0\. , 0.5, 1\. , 1.5]) 
```

与我们在本节中看到的其他数学运算相比，除法确实有一个明显的例外。因为我们不能被零除，所以这样做会导致相应的字段被一个`nan`值填充，这是 Python 对“不是数字”的简写。Jupyter 笔记本还会打印一条类似这样的警告:

```
RuntimeWarning: invalid value encountered in true_divide 
```

下面是一个用 NumPy 数组除以零的例子。

```
arr / arr

#Returns array([nan,  1.,  1.,  1.]) 
```

我们将在本课程的后面更详细地学习如何处理`nan`值。

### **NumPy 数组中的复杂运算**

许多操作不能简单地通过对 NumPy 数组应用普通语法来执行。在这一节中，我们将探索几个在 NumPy 库中有内置方法的数学运算。

#### **如何使用 NumPy 计算平方根**

您可以使用`np.sqrt`方法计算数组中每个元素的平方根:

```
np.sqrt(arr)

#Returns array([0., 1., 1.41421356, 1.73205081]) 
```

下面是许多其他的例子(注意，我们不会对这些例子进行测试，但是看看 NumPy 的功能还是很有用的):

```
np.exp(arr)

#Returns e^element for every element in the array

np.sin(arr)

#Calculate the trigonometric sine of every value in the array

np.cos(arr)

#Calculate the trigonometric cosine of every value in the array

np.log(arr)

#Calculate the base-ten logarithm of every value in the array 
```

### **继续前进**

在本节中，我们探索了 NumPy Python 库中可用的各种方法和操作。在接下来的练习题中，我们会把你对这些概念的了解用文字表达出来。

## **数字索引和分配**

在这一节中，我们将探索 NumPy 数组中的[索引和赋值。](https://nickmccullum.com/advanced-python/numpy-indexing-assignment/)

### **我将在本节中使用的数组**

和以前一样，我将在本节中使用一个特定的数组。这次将使用`np.random.rand`方法生成。我是这样生成数组的:

```
arr = np.random.rand(5) 
```

下面是实际的数组:

```
array([0.69292946, 0.9365295 , 0.65682359, 0.72770856, 0.83268616]) 
```

为了让这个数组更容易看，我将使用 NumPy 的`round`方法将数组的每个元素四舍五入到两位小数:

```
arr = np.round(arr, 2) 
```

这是新的数组:

```
array([0.69, 0.94, 0.66, 0.73, 0.83]) 
```

### **如何从 NumPy 数组中返回特定元素**

我们可以从 NumPy 数组中选择(并返回)一个特定的元素，就像使用普通的 Python 列表一样:使用方括号。

下面是一个例子:

```
arr[0]

#Returns 0.69 
```

我们还可以使用冒号操作符引用 NumPy 数组的多个元素。例如，索引`[2:]`选择从索引 2 开始的每个元素。索引`[:3]`选择索引 3 以下的所有元素。索引`[2:4]`返回从索引 2 到索引 4 的所有元素，不包括索引 4。较高的端点总是被排除。

下面是一些使用冒号操作符进行索引的例子。

```
arr[:]

#Returns the entire array: array([0.69, 0.94, 0.66, 0.73, 0.83])

arr[1:]

#Returns array([0.94, 0.66, 0.73, 0.83])

arr[1:4] 

#Returns array([0.94, 0.66, 0.73]) 
```

### **NumPy 数组中的元素赋值**

我们可以使用`=`操作符给 NumPy 数组的元素赋值，就像普通的 python 列表一样。下面是几个例子(注意，这是一个代码块，这意味着元素分配是从一个步骤延续到另一个步骤的)。

```
array([0.12, 0.94, 0.66, 0.73, 0.83])

arr

#Returns array([0.12, 0.94, 0.66, 0.73, 0.83])

arr[:] = 0

arr

#Returns array([0., 0., 0., 0., 0.])

arr[2:5] = 0.5

arr

#Returns array([0\. , 0\. , 0.5, 0.5, 0.5]) 
```

### **NumPy 中的数组引用**

NumPy 使用了一个名为“数组引用”的概念，对于刚接触这个库的人来说，这是一个非常常见的混淆来源。

为了理解数组引用，让我们首先考虑一个例子:

```
 new_array = np.array([6, 7, 8, 9])

second_new_array = new_array[0:2]

second_new_array

#Returns array([6, 7])

second_new_array[1] = 4

second_new_array 

#Returns array([6, 4]), as expected

new_array 

#Returns array([6, 4, 8, 9]) 

#which is DIFFERENT from its original value of array([6, 7, 8, 9])

#What the heck? 
```

如你所见，修改`second_new_array`也改变了`new_array`的值。

这是为什么呢？

默认情况下，当您使用`=`赋值操作符引用原始数组变量时，NumPy 不会创建数组的副本。相反，它只是将新变量指向旧变量，这允许第二个变量对原始变量进行修改——即使这不是您的意图。

这可能看起来很奇怪，但它确实有一个合乎逻辑的解释。数组引用的目的是节省计算能力。当处理大型数据集时，如果每次想要处理数组的一部分时都创建一个新数组，那么 RAM 很快就会用完。

幸运的是，有一个解决数组引用的方法。您可以使用`copy`方法显式复制一个 NumPy 数组。

这方面的一个例子如下。

```
array_to_copy = np.array([1, 2, 3])

copied_array = array_to_copy.copy()

array_to_copy

#Returns array([1, 2, 3])

copied_array

#Returns array([1, 2, 3]) 
```

正如您在下面看到的，对复制的数组进行修改不会改变原始数组。

```
copied_array[0] = 9

copied_array

#Returns array([9, 2, 3])

array_to_copy

#Returns array([1, 2, 3]) 
```

到目前为止，我们只探讨了如何引用一维 NumPy 数组。我们现在将探索二维数组的索引。

### **索引二维数组**

首先，让我们创建一个名为`mat`的二维 NumPy 数组:

```
mat = np.array([[5, 10, 15],[20, 25, 30],[35, 40, 45]])

mat

"""

Returns:

array([[ 5, 10, 15],

       [20, 25, 30],

       [35, 40, 45]])

""" 
```

有两种方法可以索引二维 NumPy 数组:

*   `mat[row, col]`
*   `mat[row][col]`

我个人更喜欢使用`mat[row][col]`命名法进行索引，因为它更容易以一步一步的方式可视化。例如:

```
#First, let's get the first row:

mat[0]

#Next, let's get the last element of the first row:

mat[0][-1] 
```

您还可以使用以下符号从二维 NumPy 数组生成子矩阵:

```
mat[1:][:2]

"""

Returns:

array([[20, 25, 30],

       [35, 40, 45]])

""" 
```

数组引用也适用于 NumPy 中的二维数组，所以如果您想避免在将原始数组的一部分保存到新的变量名中后无意中修改它，请务必使用`copy`方法。

### **使用 NumPy 数组的条件选择**

NumPy 数组支持一个名为`conditional selection`的特性，它允许您生成一个新的布尔值数组，这些值表示数组中的每个元素是否满足特定的`if`语句。

下面是一个这样的例子(我还重新创建了我们最初的`arr`变量，因为我们已经有一段时间没有看到它了):

```
arr = np.array([0.69, 0.94, 0.66, 0.73, 0.83])

arr > 0.7

#Returns array([False,  True, False,  True,  True]) 
```

您还可以通过将条件传递到方括号中来生成满足该条件的新的值数组(就像我们对索引所做的那样)。

这方面的一个例子如下:

```
arr[arr > 0.7]

#Returns array([0.94, 0.73, 0.83]) 
```

条件选择会变得比这复杂得多。我们将在本节的相关练习题中探讨更多的例子。

### **继续前进**

在这一节中，我们详细探讨了 NumPy 数组索引和赋值。我们将通过下一节中的一批练习题来进一步巩固您对这些概念的了解。

## 最终想法和特别优惠

感谢您阅读这篇关于 NumPy 的文章，它是我最喜欢的 Python 包之一，也是每个 Python 开发人员必须了解的库。

**本教程摘自我的课程** **[金融与数据科学](https://courses.nickmccullum.com/courses/enroll/python-for-finance/)Python。如果您对学习更多核心 Python 技能感兴趣，该课程对注册的前 50 名 freeCodeCamp 读者提供 50%的折扣- [单击此处立即获得折扣课程](https://courses.nickmccullum.com/courses/enroll/python-for-finance/)！**