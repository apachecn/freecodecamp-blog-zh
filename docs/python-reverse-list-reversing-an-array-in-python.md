# Python 反向列表——在 Python 中反转一个数组

> 原文：<https://www.freecodecamp.org/news/python-reverse-list-reversing-an-array-in-python/>

在本文中，您将学习如何在 Python 中反转列表。

首先，我将向您介绍 Python 中的列表，您将学习如何编写它们。之后，您将看到三种不同的方法来反转您创建的任何列表中的列表项。

以下是我们将要介绍的内容:

1.  [什么是 Python 中的列表以及如何创建列表](#introduction)
2.  [如何使用`.reverse()`方法反转列表中的项目](#reverse-method)
3.  [如何使用`reversed()`函数](#reversed-function)反转列表中的项目
4.  [如何使用切片反转列表中的项目](#slicing)

## Python 中的列表是什么？如何在 Python 例子中写列表

列表是 Python 中内置的数据类型，也是最强大的数据结构之一。

您可以将它们视为在同一个变量名下存储多个(通常是相关的)项目集合的容器。

要在 Python 中创建列表，您需要:

*   给列表命名。
*   使用赋值运算符`=`。
*   在方括号中包含`0`或更多列表项，`[]`–并确保用逗号分隔每个列表项。

列表项可以是同类的，这意味着它们属于同一类型。

例如，您可以创建仅包含数字的列表或仅包含字符串(或文本)的列表。

下面是创建姓名列表的方法:

```
names = ["Johny", "Lenny", "Jimmy", "Timmy"]

# print list contents to the console
print(names)

# check data type for names using the type() function
print(type(names))

# output
# ['Johny', 'Lenny', 'Jimmy', 'Timmy']
# <class 'list'> 
```

上面的例子创建了一个包含四个值的名字列表:`Johny`、`Lenny`、`Jimmy`和`Timmy`。

您也可以只创建整数(或整数)列表:

```
my_numbers = [1, 2, 3, 4, 5]

# print list contents to the console
print(my_numbers)

# check the data type for my_numbers using the type() function
print(type(my_numbers))

# output
# [1, 2, 3, 4, 5]
# <class 'list'> 
```

列表项也可以是异构的，这意味着它们可以是不同的数据类型。

```
# a list containing strings, integers, and floats (or numbers with a decimal point)

my_information = ["John", "Doe", 34, "London", 1.76]

print(my_information)

# output
# ['John', 'Doe', 34, 'London', 1.76] 
```

这就是列表区别于数组的地方。

与只存储相同类型的项的数组不同，列表具有更大的灵活性。

数组要求项目只能是相同的数据类型，而列表则不要求。

列表是可变的，这意味着它们是可变的和动态的——在程序的整个生命周期中，您可以随时更新、删除和添加新的列表项。

## 如何使用`.reverse()`方法反转列表中的项目

Python 中的`.reverse()`方法是一个内置方法，它将列表*反转到适当的位置*。

在适当的位置反转列表*意味着该方法修改和改变原始列表。它*不会*创建一个新列表，它是原始列表的副本，但是列表项的顺序相反。*

当您不关心保留原始列表的顺序时，此方法很有用。

`.reverse()`方法的一般语法如下所示:

```
list_name.reverse() 
```

`.reverse()`方法不接受任何参数，也没有返回值——它只更新现有的列表。

下面是如何使用`.reverse()`方法来反转一个整数列表:

```
#original list
my_numbers = [1, 2, 3, 4, 5]

# reverse the order of items in my_numbers
my_numbers.reverse()

# print contents of my_numbers to the console
print(my_numbers)

# output
# [5, 4, 3, 2, 1] 
```

在上面的例子中，原始列表中列表项的顺序被颠倒，原始列表被修改和更新。

下面是如何在姓名列表中使用该方法:

```
names = ["Johny", "Lenny", "Jimmy", "Timmy"]

# reverse the order of items in names
names.reverse()

# print contents of names to the console
print(names)

# output
# ['Timmy', 'Jimmy', 'Lenny', 'Johny'] 
```

## 如何使用`reversed()`函数反转列表中的项目

当您想以相反的顺序访问单个列表元素时，这个函数很有用。

`reversed()`函数的一般语法如下所示:

```
reversed(list_name) 
```

Python 中的`reversed()`内置函数返回一个反向迭代器——一个**可迭代对象**,然后用于以逆序检索和遍历所有列表元素。

返回一个 iterable 对象意味着该函数以相反的顺序返回列表项。它不会在适当的位置反转列表。这意味着它不会修改原始列表，也不会创建一个新列表，该列表是原始列表的副本，但列表项的顺序相反。

```
my_numbers = [1, 2, 3, 4, 5]

my_numbers_reversed = reversed(my_numbers)

# print original list
print(my_numbers)

# check the data type of my_numbers_reversed using the type() function
print(type(my_numbers_reversed))

# output
# [1, 2, 3, 4, 5]
# <class 'list_reverseiterator'> 
```

使用带有一个`for`循环的`reversed()`函数，以逆序遍历列表项。(如果你需要重温 Python 中的`for`循环，通读一下[这篇文章](https://www.freecodecamp.org/news/python-for-loop-example-and-tutorial/)

```
my_numbers = [1, 2, 3, 4, 5]

for number in reversed(my_numbers):
  print(number)

# output
# 5
# 4
# 3
# 2
# 1 
```

如果您随后将原始列表的内容打印到控制台，您将看到项目的顺序被保留，并且原始列表没有被修改:

```
my_numbers = [1, 2, 3, 4, 5]

for number in reversed(my_numbers):
  print(number)

print(my_numbers)

# output
# 5
# 4
# 3
# 2
# 1
# [1, 2, 3, 4, 5] 
```

这是因为`reversed()`函数将一个列表作为参数，并以相反的顺序返回一个迭代器。

它不会对现有列表进行任何更改，也不会创建新列表。

该函数不会永久反转列表，只是在对原始列表执行`for`循环期间暂时反转。

但是，如果您想创建一个新列表，它是原始列表的副本，但是使用`reversed()`函数将项目按相反的顺序排列，该怎么办呢？

您可以将`reversed()`操作的结果作为参数传递给`list()`函数，该函数会将迭代器转换为一个列表，并将最终结果存储在一个变量中:

```
my_numbers = [1, 2, 3, 4, 5]

my_numbers_reversed = list(reversed(my_numbers))

# original list
print(my_numbers)

# new list, which is a copy of the original one with list items in reverse order
print(my_numbers_reversed)

# output
# [1, 2, 3, 4, 5]
# [5, 4, 3, 2, 1] 
```

## 如何使用切片反转列表中的项目

Python 中反转列表的另一种方式是使用切片。

切片是 Python 中反转列表最快的方法之一，它提供了简洁的语法。也就是说，与更加直观和自我描述的`.reverse()`方法相比，它更像是一个中级到高级的特性，而不是初学者友好的。

让我们快速回顾一下切片是如何工作的。

一般语法如下所示:

```
list_name[start:stop:step] 
```

让我们来分解一下:

`start`是切片的起始索引。

Python 和计算机科学中的索引从`0`开始。

`start`的默认值是`0`，它从列表的开头抓取第一个项目。

如果您想从列表的末尾获取第一个项目，那么值应该是`-1`。

```
my_numbers = [1,2,3,4,5,]

slicing_my_numbers = my_numbers[-1:]

print(slicing_my_numbers)

# output
# [5] 
```

`stop`是结束索引位置和您希望切片停止的位置，不包含-它不包括位于您指定的索引处的元素。

例如，如果您想从头开始分割列表，直到索引为`3`的项目，您应该这样做:

```
# the item at index 3 is the integer 4
my_numbers = [1,2,3,4,5,]

slicing_my_numbers = my_numbers[:3]

# the integer 4 is not included in the result
print(slicing_my_numbers)

# output
# [1, 2, 3] 
```

`step`为增量值，默认值为`1`。

现在，当使用切片来反转列表时，您需要使用反向切片操作符`[::-1]`语法。这将步骤设置为`-1`,并以相反的顺序获取列表中的所有项目。

```
# original list
my_numbers = [1,2,3,4,5]

# reversing original list
my_numbers_reversed = my_numbers[::-1]

# print original list
print(my_numbers)

# print new list with the items from the original list in reverse order
print(my_numbers_reversed)

# output
# [1, 2, 3, 4, 5]
# [5, 4, 3, 2, 1] 
```

切片操作符不修改原始列表。相反，它返回一个新的列表，该列表是原始列表中项目的逆序副本。

## 结论

现在你知道了！您现在知道如何在 Python 中反转任何列表。

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢您的阅读和快乐编码！