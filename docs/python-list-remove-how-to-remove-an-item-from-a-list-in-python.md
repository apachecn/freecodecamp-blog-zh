# Python 列表。remove() -如何在 Python 中从列表中移除一个项目

> 原文：<https://www.freecodecamp.org/news/python-list-remove-how-to-remove-an-item-from-a-list-in-python/>

在本文中，您将学习如何使用 Python 内置的`remove()` list 方法。

最后，您将知道如何使用`remove()`在 Python 中从列表中删除一个项目。

以下是我们将要介绍的内容:

1.  [`remove()`方法的语法](#syntax)
2.  [使用`remove()`](#demo-intro) 从列表中删除一个元素
3.  [`remove()`仅删除第一次出现的项目](#first-occurrence)
    1.  [如何删除项目的所有事件](#all-occurrences)

## `remove()`方法-语法概述

`remove()`方法是 Python 中从列表中移除元素的方法之一。

`remove()`方法通过项目的**值**而不是索引号从列表中删除项目。

`remove()`方法的一般语法如下所示:

```
list_name.remove(value) 
```

让我们来分解一下:

*   `list_name`是您正在处理的列表的名称。
*   `remove()`是 Python 内置的列表方法之一。
*   `remove()`需要一个**必需的**参数。如果你不提供，你会得到一个`TypeError`——特别是你会得到一个`TypeError: list.remove() takes exactly one argument (0 given)`错误。
*   `value`是要从`list_name`中删除的项目的具体值。

`remove()`方法不返回已经被移除的值，而是只返回`None`，这意味着没有返回值。

如果你需要通过索引号删除一个项目和/或由于某种原因你想返回(保存)你删除的值，使用 [`pop()`方法](https://www.freecodecamp.org/news/python-pop-how-to-pop-from-a-list-or-an-array-in-python/)代替。

## 如何使用 Python 中的`remove()`方法从列表中移除元素

要使用`remove()`方法从列表中删除元素，请指定该元素的值，并将其作为参数传递给该方法。

`remove()`将搜索列表找到它并删除它。

```
#original list
programming_languages = ["JavaScript", "Python", "Java", "C++"]

#print original list
print(programming_languages)

# remove the value 'JavaScript' from the list
programming_languages.remove("JavaScript")

#print updated list
print(programming_languages)

#output

#['JavaScript', 'Python', 'Java', 'C++']
#['Python', 'Java', 'C++'] 
```

如果您指定了一个不包含在列表中的值，那么您将会得到一个错误——特别是这个错误将会是一个`ValueError`:

```
programming_languages = ["JavaScript", "Python", "Java", "C++"]

#I want to remove the value 'React'
programming_languages.remove("React")

#print list
print(programming_languages)

#output

# line 5, in <module>
#programming_languages.remove("React")
#ValueError: list.remove(x): x not in list 
```

为了避免这种错误的发生，您可以首先使用`in`关键字检查您想要删除的值是否在列表中。

如果项目在列表中，它将返回一个布尔值-`True`,如果值不在列表中，则返回`False`。

```
programming_languages = ["JavaScript", "Python", "Java", "C++"]

#check if 'React' is in the 'programming_languages' list
print("React" in programming_languages)

#output
#False 
```

避免这种错误的另一种方法是创建一个条件，本质上是说，“如果这个值是列表的一部分，那么删除它。如果它不存在，则显示一条消息，说明它不包含在列表中”。

```
programming_languages = ["JavaScript", "Python", "Java", "C++"]

if "React" in programming_languages:
    programming_languages.remove("React")
else:
    print("This value does not exist")

#output
#This value does not exist 
```

现在，当您试图删除某个不存在的值时，您不会得到 Python 错误，而是得到一条返回的消息，表明您想要删除的项目不在您正在处理的列表中。

## 方法删除列表中第一个出现的条目

使用`remove()`方法时要记住的一点是，它将搜索并只删除项目的第一个**实例**。

这意味着，如果列表中有多个项目实例的值是作为参数传递给方法的，则只移除第一个出现的项目。

让我们看看下面的例子:

```
programming_languages = ["JavaScript", "Python", "Java", "Python", "C++", "Python"]

programming_languages.remove("Python")

print(programming_languages)

#output
#['JavaScript', 'Java', 'Python', 'C++', 'Python'] 
```

在上例中，值为`Python`的项目在列表中出现了三次。

当使用`remove()`时，只有第一个匹配的实例被删除——在`JavaScript`值之后和`Java`值之前的那个。

另外两次出现的`Python`仍保留在列表中。

当您想要删除一个项目的所有出现时，会发生什么呢？

单独使用`remove()`并不能实现这一点，并且您可能不希望仅仅删除您指定的项目的第一个实例。

### 如何在 Python 中移除列表中某项的所有实例

删除列表中所有条目的方法之一是使用列表理解。

列表理解从现有的列表中创建新的列表，或者创建所谓的子列表。

这不会修改您的原始列表，而是创建一个满足您设置的条件的新列表。

```
#original list
programming_languages = ["JavaScript", "Python", "Java", "Python", "C++", "Python"]

#sublist created with list comprehension
programming_languages_updated = [value for value in programming_languages if value != "Python"]

#print original list
print(programming_languages)

#print  new sublist that doesn't contain 'Python'
print(programming_languages_updated)

#output

#['JavaScript', 'Python', 'Java', 'Python', 'C++', 'Python']
#['JavaScript', 'Java', 'C++'] 
```

在上面的例子中，有一个原始的`programming_languages`列表。

然后，返回一个新的列表(或子列表)。

子列表中包含的项目必须满足一个条件。条件是，如果原始列表中的一个条目的值为`Python`，那么它就不是子列表的一部分。

现在，如果您不想创建一个新的列表，而是想就地修改已经存在的列表*，那么就使用结合了列表理解的切片赋值。*

 *使用切片分配，您可以修改和替换列表的某些部分(或切片)。

要替换整个列表，使用`[:]`切片语法，以及列表理解。

列表理解设置了一个条件，即任何具有值`Python`的项目都不再是列表的一部分。

```
programming_languages = ["JavaScript", "Python", "Java", "Python", "C++", "Python"]

programming_languages[:] = (value for value in programming_languages if value != "Python")

print(programming_languages)

#output

#['JavaScript', 'Java', 'C++'] 
```

## 结论

现在你知道了！您现在知道了如何使用`remove()`方法在 Python 中删除列表项。您还看到了在 Python 中删除列表中所有条目的一些方法。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码😊！*