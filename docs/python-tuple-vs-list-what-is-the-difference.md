# python Tuple VS List–有什么区别？

> 原文：<https://www.freecodecamp.org/news/python-tuple-vs-list-what-is-the-difference/>

元组和列表是四种可用的内置数据类型中的两种，可用于在 Python 中存储数据。

它们都很有用，乍看起来可能很相似。但是它们有很大的不同，每一种都适用于不同的情况。

本文将向您概述元组和列表是如何工作的。我们将讨论它们各自的特征和独特的用例，我将一路展示它们的相似之处和不同之处。

您可以使用交互式 Python shell 来尝试本文中显示的代码示例，这是在您的计算机上安装 Python 时获得的。

我们开始吧！

## Python 中的元组和列表是什么？

元组和列表都是 Python 中内置的数据结构。

它们是容器，通过允许您存储一个或多个项目的有序集合，让您组织您的数据。

一个元组有一个' tuple '类，`<class 'tuple'>`，一个列表有一个' list '类，`<class 'list'>`。

您总是可以使用`type()`内置函数，并将对象作为想要测试的参数传递。这让您可以检查它是一个元组还是一个列表。

假设您创建了一个名为`my_tuple`的元组。您可以像这样检查它的类型:

```
>>>type(my_tuple)

#output
<class 'tuple'> 
```

这对于调试特别有用。

现在让我们看看元组和列表之间的其他一些相似之处。

## Python 中元组和列表的相似性

正如我之前提到的，元组和列表确实是相似的，它们共享一些我们现在将要讨论的特性。

### 元组和列表都可以在单个变量下存储多个项目

元组和列表可以是空的，也可以在单个变量下包含一个甚至多个项。

唯一的区别是语法上的:创建元组是通过用左圆括号和右圆括号`()`将元组中的项括起来，而列表是通过左方括号和右方括号`[]`来表示和定义的。

要创建一个*空的*元组，要么单独使用圆括号`()`，要么使用`tuple()`构造函数方法。

```
>>>type(())
<class 'tuple'>

>>>my_tuple = ()

>>>type(my_tuple)
<class 'tuple'>

#or..

>>>my_tuple = tuple()

>>>type(my_tuple)
<class 'tuple'> 
```

要创建一个*空的*列表，你可以只使用两个空的方括号或者调用`list()`构造函数方法。

```
>>>type([])
<class 'list'>

>>>my_list = []

#or..

>>>my_list = list() 
```

当创建一个有*一个条目*的元组时，不要忘记在末尾加一个逗号。

```
>>>age = (28,) 
```

如果使用`tuple()`方法创建元组，不要忘记它需要双括号。

```
>>>age = tuple((28,))

>>>type(age)
<class 'tuple'> 
```

如果不添加结尾逗号，Python 不会将其视为元组。

```
>>>age = (28)

>>>type(age)
<class 'int'> 
```

当创建一个包含一个条目的列表时，你不必担心添加逗号。

```
>>> age = [28]

>>> type(age)
<class 'list'> 
```

存储的项目通常在性质上是相似的，并且以某种方式彼此相关。

您可以创建一个元组或列表，其中只包含一个字符串序列、一个整数序列或一个布尔值序列，序列中的每一项都用逗号分隔。

您还可以创建一个包含不同数据类型混合的元组或列表。

```
>>>my_information = ["Dionysia",27,True,"Lemonaki",7,"Python",False]

#or..

>>>my_information = list(("Dionysia",27,True,"Lemonaki",7,"Python",False))

print(my_information)
['Dionysia', 27, True, 'Lemonaki', 7, 'Python', False] 
```

列表和元组可以包含重复项，值可以重复出现多次。

```
>>>information = ("Jimmy",50,True,"Kate",50)

>>>print(information)
('Jimmy', 50, True, 'Kate', 50)

or..

>>>my_information = ["Dionysia",27,True,"Lemonaki",7,"Python",False,27,"Python",27] 
```

如果你忘记了逗号，你会得到一个错误:

```
>>>information = ("Jimmy" 50,True,"Kate",50)
File "<stdin>", line 1
    >>>information = ("Jimmy" 50,True,"Kate",50)
    ^
SyntaxError: invalid syntax 
```

```
>>>my_information = ["Dionysia" 28,True,"Lemonaki",7,"Python",False]
 File "<stdin>", line 1
    my_information = ["Dionysia" 28,True,"Lemonaki",7,"Python",False]
                                 ^
SyntaxError: invalid syntax 
```

要检查长度并确定一个元组或一个列表中有多少项，可以使用`len()`方法。

```
>>>my_information = ["Dionysia",27,True,"Lemonaki",7,"Python",False,27,"Python",27]

>>>len(my_information)
7 
```

### Python 中的元组和列表都支持解包

本质上，当创建一个元组或列表时，许多值被“打包”到一个变量中，正如我前面提到的。

```
>>>front_end = ("html","css","javascript") 
```

这些值可以“解包”并分配给单个变量。

```
>>>front_end = ("html","css","javascript")

>>>content,styling,interactivity = front_end

>>>content
'html'

>>>styling
'css'

>>>interactivity
'javascript' 
```

确保您创建的变量与 tuple/list 中的值数量完全相同，否则 Python 会向您抛出一个错误:

```
>>>front_end = ("html","css","javascript")

>>>content,styling = front_end
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)

#or..

>>>front_end = ("html","css","javascript")

>>>content,styling,interactivity,data =  front_end
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: not enough values to unpack (expected 4, got 3) 
```

### 在 Python 中，可以通过元组和列表中的索引来访问项目

如前所述，元组和列表都是一个**有序的**项目集合。

顺序是固定的，不可改变的，并且在程序的整个生命周期中保持不变。

指定项目的顺序将始终保持与创建时相同。

元组和列表中的每个值都有一个唯一的标识符，也称为索引。

因此，元组和列表中的每一项都可以通过引用该索引来访问。

Python(以及大多数编程语言和计算机科学)中的索引从`0`开始。

因此，第一项的索引为`0`，第二项的索引为`1`，依此类推。

你写下元组或列表的名称，然后在方括号中写下索引的名称。

```
>>>names = ("Jimmy","Timmy","John","Kate")

>>>names[2]
'John' 
```

或者像这样:

```
>>>programming_languages = ["Python","JavaScript","Java","C"]

>>>programming_languages[0]
'Python'

>>>programming_languages[1]
'JavaScript' 
```

好了，现在我们已经看到了它们是如何相似的，现在让我们看看元组和列表的不同之处。

## Python 中元组和列表的区别

### 元组是不可变的，而列表在 Python 中是可变的

在 Python 中，元组是不可变的，这意味着一旦你创建了一个元组，其中的条目就不能改变。

元组不能连续改变。

如果您试图更改其中一项的值，将会得到一个错误:

```
>>>names = ("Jimmy","Timmy","John","Kate")

>>>names[2] = "Kelly"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment 
```

您不能添加、替换、重新分配或移除任何项目，因为元组不能更改。

这也意味着元组的长度是固定的。它们的长度在程序的整个生命周期中不会改变。

#### 何时使用元组

如果您希望您的集合中的数据是只读的，永远不会改变，并且始终保持不变，那么元组是非常有用的。

由于这种能力和数据永不改变的保证，元组可以用在字典和集合中，这要求其中包含的元素是不可变的类型。

#### 何时使用列表

另一方面，您可以很容易地改变和修改列表，因为列表是可变的。

这意味着列表是可变的——你可以在列表中添加项目，从列表中删除项目，移动项目，并在列表中轻松切换它们。

当您希望数据具有灵活性，或者不总是保持不变，并在需要时进行修改时，列表非常有用。

列表支持各种内置的 Python 方法，这些方法在列表上执行某些不能在元组上使用的操作。

这意味着列表的长度和大小会随着程序的生命周期而增长和收缩。

现在让我们来看看改变列表的一些简单方法。

## 如何在 Python 中更新列表

因为列表是可变的，你需要知道一些更新列表中数据的基本方法。

### 如何在 Python 中更新列表中的项目

若要更新列表中的单个特定项目，请在方括号中引用其索引号，然后为其分配一个新值。

```
#general syntax
>>>list_name[index] = new_value

>>>programming_languages = ["Python","JavaScript","Java","C"]
>>>print(programming_languages)
['Python', 'JavaScript', 'Java', 'C']

>>>programming_languages[2] = "C++"
>>>print(programming_languages)
['Python', 'JavaScript', 'C++', 'C'] 
```

### 如何在 Python 中向列表添加项目

Python 中有一些用于向列表添加项目的内置方法。

`.append()`方法向列表的*末端*添加一个新项目。

```
#general syntax
>>>list_name.append(item)

>>>programming_languages = ["Python","JavaScript","Java","C"]
>>>print(programming_languages)
['Python', 'JavaScript', 'Java', 'C']

>>>programming_languages.append("C++")

>>>print(programming_languages)
['Python', 'JavaScript', 'Java', 'C', 'C++'] 
```

要在特定位置添加一个项目，可以使用`.insert()`方法。

这将在列表中的给定位置插入一个项目。列表中您要添加的项后面的其余元素都向右移动一个位置。

```
#general syntax
>>>list_name.insert(index,item)

>>>names = ["Cody","Dillan","James","Nick"]
>>>print(names)
['Cody', 'Dillan', 'James', 'Nick']

>>>names.insert(0,"Stephanie")

>>>print(names)
['Stephanie', 'Cody', 'Dillan', 'James', 'Nick'] 
```

如果你想在你的列表中添加一个以上的条目，你可以使用`.extend()`方法。

这将在列表的*末端*添加一个 iterable。例如，您可以在现有列表的末尾添加一个新列表。

```
#general syntax
>>>list_name.extend(iterable)

>>>programming_languages = ["Python","JavaScript"]
>>>more_programming_languages = ["Java","C"]

#add more_programming_languages to programming_languages
>>>programming_languages.extend(more_programming_languages) 

>>>print(programming_languages)
['Python', 'JavaScript', 'Java', 'C'] 
```

### 如何在 Python 中从列表中删除项目

在 Python 中，有两种从列表中删除项目的内置方法。

一种是`.remove()`法。这将移除您指定的项目的第一个实例。

```
#general syntax
>>>list_name.remove(item)

>>>programming_languages = ["Python", "JavaScript", "Java", "C"]
>>>print(programming_languages)
['Python', 'JavaScript', 'Java', 'C']

>>>programming_languages.remove("Java")
>>>print(programming_languages)
['Python', 'JavaScript', 'C']

#deletes only first occurence
>>>programming_languages = ["Python", "JavaScript", "Java", "C","Python"]
>>>programming_languages.remove("Python")
>>>print(programming_languages)
['JavaScript', 'Java', 'C', 'Python'] 
```

另一种方法是使用`.pop()`方法。

如果不传递参数，它将删除列表中的最后一项。

您可以将想要移除的特定项的索引作为参数传入。

在这两种情况下，移除的值都会返回，这很有用。如果您愿意，可以将它存储在一个变量中以备后用。

```
>>>programming_languages = ["Python", "JavaScript", "Java", "C"]

>>>programming_languages.pop()
'C'

>>>print(programming_languages)
['Python', 'JavaScript', 'Java']

#store returned value in a variable
>>>programming_languages = ["Python", "JavaScript", "Java", "C"]

>>>fave_language = programming_languages.pop(0)
>>>print(fave_language)
Python 
```

## 结论

这标志着我们对元组和列表如何工作以及它们如何被普遍使用的介绍结束了。

概括地说，元组和列表之间的**相似性**是:

*   它们都被认为是 Python 中的对象。
*   它们是容器，用来存储数据。该数据可以是任何类型的。
*   它们都是有序的，并且一直保持着这种秩序。项目的顺序一旦确定，就不会改变。
*   在元组和列表中，您可以通过索引访问单个项目。

元组和列表之间的**差异**是:

*   元组是**不可变的**。当您确信您的数据在程序的生命周期中不会改变，或者当您想要保证您的数据将始终保持不变时，请使用它们。
*   列表是可变的。您可以添加和删除项目。列表在程序的整个生命周期中会增长和收缩。当您的数据需要更改时，请使用它们。

如果你想深入学习 Python，freeCodeCamp 提供免费的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你从绝对的基础开始，并推进到更复杂的主题，如数据结构和关系数据库。最后有五个练习项目来巩固你的知识。

感谢阅读，快乐学习！