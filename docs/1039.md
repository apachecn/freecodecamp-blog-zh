# Python。Pop()–如何在 Python 中从列表或数组中弹出

> 原文：<https://www.freecodecamp.org/news/python-pop-how-to-pop-from-a-list-or-an-array-in-python/>

在本文中，您将学习如何使用 Python 内置的`pop()` list 方法。

最后，您将知道如何使用`pop()`在 Python 中从列表中删除一个项目。

以下是我们将要介绍的内容:

1.  [Python 中的列表概述](#intro)
2.  [如何使用`pop()`](#pop-intro) 删除列表项
    1.  [`pop()`方法的语法](#syntax)
    2.  [使用不带参数](#no-parameter)的`pop()`方法
    3.  [使用带有可选参数](#with-parameter)的`pop()`方法
    4.  [处理常见错误](#errors)

## Python 中的列表是什么以及如何创建它们

列表是 Python 中的一种内置数据类型。它们充当容器，存储数据集合。

使用方括号`[]`创建列表，如下所示:

```
#an empty list
my_list = []

print(my_list)
print(type(my_list))

#output

#[]
#<class 'list'> 
```

您也可以使用`list()`构造函数创建一个列表:

```
#an empty list
my_list = list()

print(my_list)
print(type(my_list))

#output

#[]
#<class 'list'> 
```

正如你在上面看到的，一个列表可以包含`0`项，在这种情况下，它被认为是一个空列表。

列表也可以包含项目或列表项目。列表项用方括号括起来，并用逗号分隔。

列表项可以是同类的，这意味着它们属于同一类型。

例如，您可以有一个只有数字的列表，或者一个只有文本的列表:

```
# a list of integers
my_numbers_list = [10,20,30,40,50]

# a list of strings
names = ["Josie", "Jordan","Joe"]

print(my_numbers_list)
print(names)

#output

#[10, 20, 30, 40, 50]
#['Josie', 'Jordan', 'Joe'] 
```

列表项也可以是异构的，这意味着它们可以是不同的数据类型。

这就是列表区别于数组的地方。数组要求项目只能是相同的数据类型，而列表则不要求。

```
#a list containing strings, integers and floating point numbers
my_information = ["John", "Doe", 34, "London", 1.76]

print(my_information)

#output

#['John', 'Doe', 34, 'London', 1.76] 
```

列表是可变的，这意味着它们是可变的。列表项可以更新，列表项可以删除，新的项可以添加到列表中。

## 如何使用 Python 中的`pop()`方法从列表中删除元素

在接下来的小节中，您将学习如何使用`pop()`方法从 Python 的列表中移除元素。

### `pop()`方法-语法概述

`pop()`方法的一般语法如下所示:

```
list_name.pop(index) 
```

让我们来分解一下:

*   `list_name`是您正在处理的列表的名称。
*   内置的`pop()` Python 方法只接受一个**可选的**参数。
*   可选参数是要删除的项目的索引。

### 如何使用不带参数的`pop()`方法

默认情况下，如果没有指定索引，`pop()`方法将删除列表中包含的最后一个项。

这意味着当`pop()`方法没有任何参数时，它将删除最后一个列表项。

所以，它的语法应该是这样的:

```
list_name.pop() 
```

让我们看一个例子:

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

#print initial list
print(programming_languages)

#remove last item, which is "JavaScript"
programming_languages.pop()

#print list again
print(programming_languages)

#output

#['Python', 'Java', 'JavaScript']
#['Python', 'Java'] 
```

除了删除项目，`pop()`还返回它。

如果您希望将该项保存并存储在变量中以备后用，这将很有帮助。

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

#print initial list
print(programming_languages)

#remove last item, which is "JavaScript", and store it in a variable
front_end_language = programming_languages.pop()

#print list again
print(programming_languages)

#print the item that was removed
print(front_end_language)

#output

#['Python', 'Java', 'JavaScript']
#['Python', 'Java']
#JavaScript 
```

### 如何使用带有可选参数的`pop()`方法

若要删除特定的列表项，您需要指定该项的索引号。具体来说，您将表示项目位置的索引作为参数传递给`pop()`方法。

Python 和所有编程语言的索引都是从零开始的。计数从`0`而不是`1`开始。

这意味着列表中的第一项有一个索引`0`。第二项的索引为`1`，以此类推。

因此，要删除列表中的第一项，需要指定索引`0`作为`pop()`方法的参数。

并且记住，`pop()`返回已经移除的项目。这使您能够将它存储在一个变量中，就像您在上一节中看到的那样。

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

#remove first item and store in a variable
programming_language = programming_languages.pop(0)

#print updated list
print(programming_languages)

#print the value that was removed from original list
print(programming_language)

#output

#['Java', 'JavaScript']
#Python 
```

让我们看另一个例子:

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

#remove "Java" from the list
#Java is the second item in the list which means it has an index of 1

programming_languages.pop(1)

#print list 
print(programming_languages)

#output
#['Python', 'JavaScript'] 
```

在上面的例子中，列表中有一个您想要删除的特定值。为了成功删除特定值，您需要知道它的位置。

### 使用`pop()`方法时出现的常见错误概述

请记住，如果您试图删除一个等于或大于列表长度的项目，您将会得到一个错误——特别是一个`IndexError`。

让我们看看下面的例子，它显示了如何找到一个列表的长度:

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

#find the length of the list
print(len(programming_languages))

#output
#3 
```

要查找列表的长度，可以使用`len()`函数，该函数返回列表中包含的项目总数。

如果我试图删除位置 3 处的一个项目，它等于列表的长度，我得到一个错误，说传递的索引超出范围:

```
#list of programming languages
programming_languages = ["Python", "Java", "JavaScript"]

programming_languages.pop(3)

#output

# line 4, in <module>
#    programming_languages.pop(3)
#IndexError: pop index out of range 
```

如果我试图移除位置 4 甚至更高的项目，也会引发同样的异常。

同样，如果在空列表上使用`pop()`方法，也会引发异常:

```
#empty list
programming_languages = []

#try to use pop() on empty list
programming_languages.pop()

#print updated list
print(programming_languages)

#output
#line 5, in <module>
#    programming_languages.pop()
#IndexError: pop from empty list 
```

## 结论

现在你知道了！您现在知道了如何使用`pop()`方法在 Python 中删除列表项。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！