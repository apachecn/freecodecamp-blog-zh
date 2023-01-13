# Python 数组教程–定义、索引、方法

> 原文：<https://www.freecodecamp.org/news/python-array-tutorial-define-index-methods/>

在本文中，您将学习如何使用 Python 数组。您将看到如何定义它们以及通常用于对它们执行操作的不同方法。

文章涵盖了通过导入`array module`创建的数组。我们不会在这里讨论 NumPy 数组。

## 目录

1.  [阵列简介](#introduction)
    1.  [列表和数组的区别](#differences)
    2.  [何时使用数组](#usage1)
2.  [如何使用数组](#usage2)
    1.  [定义数组](#define)
    2.  [求数组的长度](#length)
    3.  [数组索引](#indexing)
    4.  [搜索数组](#search)
    5.  [遍历数组](#loop)
    6.  [对数组进行切片](#slice)
3.  [执行运算的数组方法](#methods)
    1.  [改变现有值](#change)
    2.  [添加新值](#addition)
    3.  [删除一个值](#remove)
4.  [结论](#conclusion)

我们开始吧！

## 什么是 Python 数组？

数组是一种基本的数据结构，也是大多数编程语言的重要组成部分。在 Python 中，它们是能够同时存储多个项目的容器。

具体来说，它们是元素的有序集合，每个值都是相同的数据类型。这是关于 Python 数组要记住的最重要的事情——事实上，它们只能保存同一类型的多个项目的序列。

### Python 列表和 Python 数组有什么区别？

列表是 Python 中最常见的数据结构之一，也是该语言的核心部分。

列表和数组的行为类似。

就像数组一样，列表是元素的有序序列。

它们也是可变的，大小不固定，这意味着它们可以在程序的整个生命周期中增长和收缩。可以添加和删除项目，使它们的使用非常灵活。

然而，列表和数组不是一回事。

**列出**存储各种**数据类型**的项目。这意味着一个列表可以同时包含整数、浮点数、字符串或任何其他 Python 数据类型。数组就不是这样了。

正如上一节提到的，**数组**只存储属于**相同单一数据类型**的项目。有些数组只包含整数，或者只包含浮点数，或者只包含您想要使用的任何其他 Python 数据类型。

### 何时使用 Python 数组

Python 编程语言内置了列表，而数组没有。数组不是内置的数据结构，因此需要通过`array module`导入才能使用。

`array module`的数组是 C 数组的一个薄薄的包装，当你想处理同类数据时很有用。

它们也更紧凑，占用更少的内存和空间，这使得它们比列表更有效。

如果您想要执行数学计算，那么您应该通过导入 NumPy 包来使用 NumPy 数组。除此之外，你应该在真正需要的时候才使用 Python 数组，因为列表的工作方式与此类似，并且更加灵活。

## 如何在 Python 中使用数组

为了创建 Python 数组，首先必须导入包含所有必要函数的`array module`。

有三种方法可以导入`array module`:

1.  通过使用文件顶部的`import array`。这包括模块`array`。然后你可以使用`array.array()`创建一个数组。

```
import array

#how you would create an array
array.array() 
```

2.  你可以在文件的顶部使用`import array as arr`,而不是只使用`import array`,而不是一直键入`array.array()`。然后您可以通过键入`arr.array()`来创建一个数组。`arr`充当别名，数组构造函数紧随其后。

```
import array as arr

#how you would create an array
arr.array() 
```

3.  最后，您还可以使用`from array import *`，用`*`导入所有可用的功能。然后你可以通过单独编写`array()`构造函数来创建一个数组。

```
from array import *

#how you would create an array
array() 
```

### 如何在 Python 中定义数组

一旦导入了`array module`，就可以继续定义 Python 数组。

创建数组的一般语法如下所示:

```
variable_name = array(typecode,[elements]) 
```

让我们来分解一下:

*   `variable_name`将是数组的名称。
*   `typecode`指定什么样的元素将被存储在数组中。无论是整数数组、浮点数组还是任何其他 Python 数据类型的数组。请记住，所有元素都应该是相同的数据类型。
*   在方括号中，您提到了将存储在数组中的`elements`，每个元素用逗号分隔。你也可以只写`variable_name = array(typecode)`来创建一个*空的*数组，不用任何元素。

下面是一个类型代码表，其中包含定义 Python 数组时可用于不同数据类型的不同类型代码:

| 类型代码 | c 型 | Python 类型 | 大小 |
| --- | --- | --- | --- |
| ' b ' | 有符号字符 | （同 Internationalorganizations）国际组织 | one |
| ' b ' | 无符号字符 | （同 Internationalorganizations）国际组织 | one |
| 你好 | wchar_t | Unicode 字符 | Two |
| ' h ' | 带符号的短 | （同 Internationalorganizations）国际组织 | Two |
| ' h ' | 无符号短整型 | （同 Internationalorganizations）国际组织 | Two |
| 我 | 有符号整数 | （同 Internationalorganizations）国际组织 | Two |
| 我 | 无符号整数 | （同 Internationalorganizations）国际组织 | Two |
| 我 | 长签名 | （同 Internationalorganizations）国际组织 | four |
| 我 | 无符号长整型 | （同 Internationalorganizations）国际组织 | four |
| 问 | 署名龙龙 | （同 Internationalorganizations）国际组织 | eight |
| 问 | 无符号长整型 | （同 Internationalorganizations）国际组织 | eight |
| f ' | 漂浮物 | 漂浮物 | four |
| 迪 | 两倍 | 漂浮物 | eight |

将所有的事情联系在一起，下面是一个如何在 Python 中定义数组的例子:

```
import array as arr 

numbers = arr.array('i',[10,20,30])

print(numbers)

#output

#array('i', [10, 20, 30]) 
```

让我们来分解一下:

*   首先我们包括了数组模块，在这个例子中是用`import array as arr` 表示的。
*   然后，我们创建了一个`numbers`数组。
*   我们用`arr.array()`是因为`import array as arr` 。
*   在`array()`构造函数中，我们首先包含了`i`，表示有符号整数。有符号整数意味着数组可以包含正的*和负的*。无符号整数，例如`H`，意味着不允许有负值。
*   最后，我们将存储在数组中的值放在方括号中。

请记住，如果您试图包含不属于`i`类型代码的值，这意味着它们不是整数值，您将会得到一个错误:

```
import array as arr 

numbers = arr.array('i',[10.0,20,30])

print(numbers)

#output

#Traceback (most recent call last):
# File "/Users/dionysialemonaki/python_articles/demo.py", line 14, in <module>
#   numbers = arr.array('i',[10.0,20,30])
#TypeError: 'float' object cannot be interpreted as an integer 
```

在上面的例子中，我试图在数组中包含一个浮点数。我得到了一个错误，因为这应该是一个整数数组。

创建数组的另一种方法如下:

```
from array import *

#an array of floating point values
numbers = array('d',[10.0,20.0,30.0])

print(numbers)

#output

#array('d', [10.0, 20.0, 30.0]) 
```

上面的例子通过`from array import *`导入了`array module`，并创建了一个浮点数据类型的数组`numbers`。这意味着它只保存浮点数，这是用`'d'` typecode 指定的。

### 如何在 Python 中求数组的长度

要找出数组中包含的元素的确切数量，请使用内置的`len()`方法。

它将返回一个整数，该整数等于您指定的数组中的元素总数。

```
import array as arr 

numbers = arr.array('i',[10,20,30])

print(len(numbers))

#output
# 3 
```

在上面的例子中，数组包含三个元素——`10, 20, 30`——所以`numbers`的长度是`3`。

### Python 中的数组索引和如何访问数组中的单个项目

数组中的每一项都有一个特定的地址。通过引用它们的*索引号*来访问各个项目。

Python 以及所有编程语言和一般计算中的索引都是从`0`开始的。重要的是要记住计数从`0`和**开始，而不是从**的`1`开始。

要访问一个元素，首先要写数组名，后跟方括号。在方括号内，您包括该项目的索引号。

一般语法如下所示:

```
array_name[index_value_of_item] 
```

下面是访问数组中每个元素的方法:

```
import array as arr 

numbers = arr.array('i',[10,20,30])

print(numbers[0]) # gets the 1st element
print(numbers[1]) # gets the 2nd element
print(numbers[2]) # gets the 3rd element

#output

#10
#20
#30 
```

请记住，数组最后一个元素的索引值总是比数组长度小 1。其中`n`是数组的长度，`n - 1`将是最后一项的索引值。

请注意，您还可以使用负索引来访问每个单独的元素。

对于负索引，最后一个元素的索引为`-1`，倒数第二个元素的索引为`-2`，依此类推。

下面是如何使用该方法获取数组中的每一项:

```
import array as arr 

numbers = arr.array('i',[10,20,30])

print(numbers[-1]) #gets last item
print(numbers[-2]) #gets second to last item
print(numbers[-3]) #gets first item

#output

#30
#20
#10 
```

### 如何在 Python 中搜索数组

您可以使用`index()`方法找出一个元素的索引号。

您将正在搜索的元素的值作为参数传递给方法，然后返回该元素的索引号。

```
import array as arr 

numbers = arr.array('i',[10,20,30])

#search for the index of the value 10
print(numbers.index(10))

#output

#0 
```

如果有多个元素具有相同的值，将返回该值的第一个实例的索引:

```
import array as arr 

numbers = arr.array('i',[10,20,30,10,20,30])

#search for the index of the value 10
#will return the index number of the first instance of the value 10
print(numbers.index(10))

#output

#0 
```

### 如何在 Python 中循环遍历数组

您已经看到了如何访问数组中的每个单独的元素并单独打印出来。

您还看到了如何使用`print()`方法打印数组。该方法给出了以下结果:

```
import array as arr 

numbers = arr.array('i',[10,20,30])

print(numbers)

#output

#array('i', [10, 20, 30]) 
```

如果要逐个打印每个值呢？

这就是循环派上用场的地方。您可以遍历数组，并在每次循环迭代中逐一打印出每个值。

为此，您可以使用一个简单的`for`循环:

```
import array as arr 

numbers = arr.array('i',[10,20,30])

for number in numbers:
    print(number)

#output
#10
#20
#30 
```

您也可以使用`range()`函数，并将`len()`方法作为其参数传递。这将给出与上面相同的结果:

```
import array as arr  

values = arr.array('i',[10,20,30])

#prints each individual value in the array
for value in range(len(values)):
    print(values[value])

#output

#10
#20
#30 
```

### 如何在 Python 中分割数组

要访问数组中特定范围的值，可以使用切片操作符，它是一个冒号`:`。

当使用切片运算符并且只包含一个值时，默认情况下从`0`开始计数。它获取第一项，并向上移动，但不包括您指定的索引号。

```
 import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#get the values 10 and 20 only
print(numbers[:2])  #first to second position

#output

#array('i', [10, 20]) 
```

当您传递两个数字作为参数时，您指定了一个数字范围。在这种情况下，计数从范围内第一个数字的位置开始，直到但不包括第二个数字:

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#get the values 20 and 30 only
print(numbers[1:3]) #second to third position

#output

#rray('i', [20, 30]) 
```

## Python 中对数组执行操作的方法

数组是可变的，这意味着它们是可变的。您可以更改不同项目的值，添加新的项目，或者删除程序中不再需要的项目。

让我们看看一些最常用的方法，这些方法用于对数组执行操作。

### 如何改变数组中某一项的值

您可以通过指定特定元素的位置并为其分配新值来更改其值:

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#change the first element
#change it from having a value of 10 to having a value of 40
numbers[0] = 40

print(numbers)

#output

#array('i', [40, 20, 30]) 
```

### 如何向数组添加新值

要在数组末尾添加一个值，使用`append()`方法:

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#add the integer 40 to the end of numbers
numbers.append(40)

print(numbers)

#output

#array('i', [10, 20, 30, 40]) 
```

请注意，您添加的新项需要与数组中的其余项具有相同的数据类型。

看看当我试图向一个整数数组添加一个浮点数时会发生什么:

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#add the integer 40 to the end of numbers
numbers.append(40.0)

print(numbers)

#output

#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 19, in <module>
#   numbers.append(40.0)
#TypeError: 'float' object cannot be interpreted as an integer 
```

但是如果你想在数组末尾添加多个值呢？

使用`extend()`方法，该方法将一个 iterable(比如一个条目列表)作为参数。同样，确保新项目都是相同的数据类型。

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#add the integers 40,50,60 to the end of numbers
#The numbers need to be enclosed in square brackets

numbers.extend([40,50,60])

print(numbers)

#output

#array('i', [10, 20, 30, 40, 50, 60]) 
```

如果你不想在数组的末尾添加一个元素呢？使用`insert()`方法，在特定位置添加项目。

`insert()`函数有两个参数:新元素将被插入的位置的索引号，以及新元素的值。

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

#add the integer 40 in the first position
#remember indexing starts at 0

numbers.insert(0,40)

print(numbers)

#output

#array('i', [40, 10, 20, 30]) 
```

### 如何从数组中删除一个值

要从数组中删除一个元素，使用`remove()`方法并将值作为参数包含到该方法中。

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30])

numbers.remove(10)

print(numbers)

#output

#array('i', [20, 30]) 
```

使用`remove()`，只有作为参数传递的值的第一个实例会被删除。

看看当有多个相同的值时会发生什么:

```
 import array as arr 

#original array
numbers = arr.array('i',[10,20,30,10,20])

numbers.remove(10)

print(numbers)

#output

#array('i', [20, 30, 10, 20]) 
```

仅删除第一次出现的`10`。

您也可以使用`pop()`方法，并指定要移除的元素的位置:

```
import array as arr 

#original array
numbers = arr.array('i',[10,20,30,10,20])

#remove the first instance of 10
numbers.pop(0)

print(numbers)

#output

#array('i', [20, 30, 10, 20]) 
```

## 结论

现在，您已经知道了如何使用`array module`在 Python 中创建数组的基本知识。希望这个指南对你有所帮助。

要了解更多关于 Python 的知识，请查看 [freeCodeCamp 的具有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！

参考资料: [Python 文档](https://docs.python.org/3/library/array.html)