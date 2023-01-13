# Python 在列表中查找–如何在列表中查找项目或元素的索引

> 原文：<https://www.freecodecamp.org/news/python-find-in-list-how-to-find-the-index-of-an-item-or-element-in-a-list/>

在本文中，您将学习如何在 Python 编程语言中找到包含在列表中的元素的索引。

有几种方法可以实现这一点，在本文中，您将学习三种用于在 Python 中查找列表元素索引的不同技术。

使用的三种技术是:

*   使用`index()`列表方法查找索引，
*   使用`for-loop`，
*   最后，使用列表理解和`enumerate()`函数。

具体来说，以下是我们将深入探讨的内容:

1.  [Python 中的列表概述](#intro)
    1.  [索引如何工作](#index-intro)
2.  [使用`index()`方法找到一个项目的索引](#index)
    1。[通过`index()`方法](#params)使用可选参数
3.  [获取列表中一个项目所有出现的索引](#all)
    1.  [使用一个`for-loop`来获得列表中一个项目的所有出现的索引](#for)
    2.  [使用 list comprehension 和`enumerate()`函数获取列表中某个项目所有出现的索引](#enumerate)

## Python 中的列表是什么？

列表是 Python 中内置的数据类型，也是最强大的数据结构之一。

它们充当容器，在同一个变量名下存储多个通常相关的项。

项目被放在方括号内`[]`。方括号内的每一项都用逗号分隔，`,`。

```
# a list called 'my_information' that contains strings and numbers
my_information = ["John Doe", 34, "London", 1.76] 
```

从上面的例子中，您可以看到列表可以包含任何数据类型的项目，这意味着列表元素可以是异构的。

与只存储相同类型的项的数组不同，列表具有更大的灵活性。

列表也是*可变的*，这意味着它们是可变的和动态的。列表项可以更新，新的项可以添加到列表中，任何项都可以在程序的整个生命周期中随时删除。

### Python 中的索引概述

如前所述，列表是项目的集合。具体来说，它们是有序的项目集合，并且在很大程度上保留了集合和定义的顺序。

列表中的每个元素都有一个唯一的位置来标识它。

该位置被称为元素的索引。

在 Python 和所有编程语言中，索引从`0`和**开始，而不是从**和`1`开始。

让我们看一下上一节中使用的列表:

```
my_information = ["John Doe", 34, "London", 1.76] 
```

列表是零索引的，计数从`0`开始。

第一个列表元素`"John Doe"`的索引为`0`。
第二个列表元素`34`的索引为`1`。
第三个列表元素`"London"`的索引为`2`。
第四个列表元素`1.76`的索引为`3`。

索引对于访问位置(索引)已知的特定列表项非常有用。

因此，您可以通过使用它的索引来获取任何想要的列表元素。

要访问一个项目，首先包括列表的名称，然后在方括号中包括对应于您要访问的项目的索引的整数。

下面是使用索引访问每个项目的方法:

```
my_information = ["John Doe", 34, "London", 1.76]

print(my_information[0])
print(my_information[1])
print(my_information[2])
print(my_information[3])

#output

#John Doe
#34
#London
#1.76 
```

但是*在 Python 中找到*一个列表项的索引呢？

在接下来的部分中，您将看到一些查找列表元素索引的方法。

## 使用 Python 中的 List `index()`方法查找项目的索引

到目前为止，您已经看到了如何通过引用索引号来访问一个值。

但是，当您不知道索引号并且正在处理一个大型列表时，会发生什么情况呢？

你可以给一个值，找到它的索引，然后检查它在列表中的位置。

为此，Python 内置的`index()`方法被用作搜索工具。

`index()`方法的语法如下所示:

```
my_list.index(item, start, end) 
```

让我们来分解一下:

*   `my_list`是您正在搜索的列表的名称。
*   `.index()`是采用三个参数的搜索方法。一个参数是必需的，另外两个是可选的。
*   `item`是必需的参数。它是您要搜索其索引的元素。
*   `start`是第一个可选参数。这是您开始搜索的索引。
*   `end`第二个可选参数。这是您将结束搜索的索引。

让我们看一个仅使用必需参数的示例:

```
programming_languages = ["JavaScript","Python","Java","C++"]

print(programming_languages.index("Python"))

#output
#1 
```

在上面的例子中，`index()`方法只接受一个参数，也就是您要寻找的索引的元素。

请记住，您传递的参数*区分大小写*。这意味着，如果您传递的是“python”，而不是“python”，您会收到一个错误，因为带有小写“p”的“Python”不在列表中。

返回值是一个整数，它是作为参数传递给方法的列表项的索引号。

让我们看另一个例子:

```
programming_languages = ["JavaScript","Python","Java","C++"]

print(programming_languages.index("React"))

#output
#line 3, in <module>
#    print(programming_languages.index("React"))
#ValueError: 'React' is not in list 
```

如果您尝试搜索一个项目，但是在您搜索的列表中没有匹配项，Python 将抛出一个错误作为返回值——特别是它将返回一个`ValueError`。

这意味着您要搜索的项目不在列表中。

防止这种情况发生的一种方法是将对`index()`方法的调用包装在`try/except`块中。

如果该值不存在，控制台会收到一条消息，说明它没有存储在列表中，因此不存在。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

try:
    print(programming_languages.index("React"))
except ValueError:
    print("That item does not exist")

#output
#That item does not exist 
```

另一种方法是在查找索引号之前，首先检查该项是否在列表中。输出将是一个布尔值——它要么是真，要么是假。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

print("React" in programming_languages)

#output
#False 
```

### 如何在`index()`方法中使用可选参数

让我们看看下面的例子:

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

print(programming_languages.index("Python"))

#output
#1 
```

在列表`programming_languages`中，有三个正在被搜索的“Python”字符串的实例。

作为一种测试方法，您可以逆向工作，因为在这种情况下列表很小。

您可以计算出它们的索引号，然后像您在前面章节中看到的那样引用它们:

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

print(programming_languages[1])
print(programming_languages[3])
print(programming_languages[5])

#output
#Python
#Python
#Python 
```

一个在位置`1`，另一个在位置`3`，最后一个在位置`5`。

当使用`index()`方法时，为什么它们没有显示在输出中？

当使用`index()`方法时，返回值只是列表中项的第**次出现**。其余的事件不会被返回。

`index()`方法只返回该项在第**次**出现的位置的索引。

您可以尝试将可选的`start`和`end`参数传递给`index()`方法。

您已经知道第一次出现从索引`1`开始，所以这可能是参数`start`的值。

对于`end`参数，您可以首先找到列表的长度。

要找到长度，使用`len()`功能:

```
print(len(programming_languages)) 

#output is 6 
```

然后，`end`参数的值将是列表的长度减 1。列表中最后一项的索引总是比列表的长度小一。

因此，将所有这些放在一起，您可以尝试获得该项目的所有三个实例:

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

print(programming_languages.index("Python",1,5))

#output
#1 
```

输出仍然只返回第一个实例！

虽然`start`和`end`参数为您的搜索提供了一个位置范围，但是使用`index()`方法时返回的值仍然只是列表中第一次出现的条目。

## 如何获取列表中某个项目所有出现的索引

### 使用一个`for-loop`来获得列表中一个项目的所有出现的索引

让我们举一个到目前为止我们用过的例子。

该列表中出现了三次字符串“Python”。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"] 
```

首先，创建一个新的空列表。

这将是存储“Python”的所有索引的列表。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

python_indices = [] 
```

接下来，使用一个`for-loop`。这是一种迭代(或循环)列表的方法，并获取原始列表中的每一项。具体来说，我们循环每个项目的索引号。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

python_indices = []

for programming_language in range(len(programming_languages)): 
```

你首先使用`for`关键字。

然后创建一个变量，在本例中是`programming_language`，在迭代过程中，它将作为原始列表中每个条目位置的占位符。

接下来，您需要指定`for-loop`应该执行的迭代次数。

在这种情况下，循环将从头到尾遍历列表的整个长度。语法`range(len(programming_languages))`是一种访问列表`programming_languages`中所有条目的方法。

`range()`函数接受一个数字序列，该序列指定它应该从哪个数字开始计数，以及应该从哪个数字结束计数。

`len()`函数计算列表的长度，所以在这种情况下，计数将从`0`开始，到——但不包括——`6`结束，这是列表的结尾。

最后，您需要设置一个逻辑条件。

本质上，你想说:“如果在迭代过程中，给定位置的值等于‘Python’，将那个位置添加到我之前创建的新列表中”。

使用`append()`方法向列表中添加元素。

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

python_indices = []

for programming_language in range(len(programming_languages)):
    if programming_languages[programming_language] == "Python":
      python_indices.append(programming_language)

print(python_indices)

#output

#[1, 3, 5] 
```

### 使用 List Comprehension 和`enumerate()`函数获取列表中某个项目的所有出现的索引

另一种查找某一特定项目所有出现的索引的方法是使用列表理解。

列表理解是一种基于现有列表创建新列表的方法。

下面是如何使用列表理解获得字符串“Python”每次出现的所有索引:

```
programming_languages = ["JavaScript","Python","Java","Python","C++","Python"]

python_indices  = [index for (index, item) in enumerate(programming_languages) if item == "Python"]

print(python_indices)

#[1, 3, 5] 
```

使用`enumerate()`功能，您可以存储符合您设置的条件的项目的索引。

它首先为列表(`programming_languages`)中的每个元素提供一对(`index, item`)，作为参数传递给函数。

`index`是列表项的索引号，`item`是列表项本身。

然后，它作为一个计数器，从`0`开始计数，并在每次满足您设置的条件时递增，选择并移动符合您的标准的项目的索引。

与 list comprehension 配对，创建一个包含字符串“Python”的所有索引的新列表。

## 结论

现在你知道了！现在，您已经知道了一些在 Python 中查找条目索引的方法，以及查找条目在列表中多次出现的索引的方法。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！