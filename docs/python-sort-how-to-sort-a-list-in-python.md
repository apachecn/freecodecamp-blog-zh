# Python。Sort()–如何在 Python 中对列表进行排序

> 原文：<https://www.freecodecamp.org/news/python-sort-how-to-sort-a-list-in-python/>

在本文中，您将学习如何使用 Python 的`sort()` list 方法。

您还将学习使用`sorted()`函数在 Python 中执行排序的不同方式，这样您就可以看到它与`sort()`的不同之处。

最后，您将了解在 Python 中对列表进行排序的基础，并知道如何定制排序以满足您的需求。

以下是我们将要介绍的内容:

1.  [`sort`方法的语法](#syntax)
2.  [按升序排列列表项目](#ascending)
3.  [按降序排列列表项目](#descending)
4.  [使用`key`参数](#key)对列表项进行排序
5.  [`sort()`和`sorted()`T3 的区别】](#differences)
    1.  [什么时候用`sort()`和`sorted()`](#usage)

## `sort()`方法-语法概述

`sort()`方法是 Python 中对列表进行排序的方法之一。

当使用`sort()`时，你就地排序一个列表*。这意味着直接修改原始列表。具体来说，元素的原始顺序被改变了。*

 *`sort()`方法的一般语法如下所示:

```
list_name.sort(reverse=..., key=... ) 
```

让我们来分解一下:

*   `list_name`是您正在处理的列表的名称。
*   `sort()`是 Python 的列表方法之一，用于排序和改变列表。它按照从*升序*或者从*降序*的顺序对列表元素进行排序。
*   `sort()`接受两个**可选的**参数。
*   `reverse`是第一个可选参数。它指定列表是按升序还是降序排序。它接受一个布尔值，即值为真或假。默认值为 **False** ，表示列表按升序排序。将其设置为 True 将列表按降序向后排序。
*   `key`是第二个可选参数。它需要一个函数或方法来指定你可能有的任何详细的排序标准。

`sort()`方法返回`None`，这意味着没有返回值，因为它只是修改了原始列表。它不会**返回一个新的列表。**

## 如何使用`sort()`方法对列表项进行升序排序

如前所述，默认情况下，`sort()`按升序对列表项进行排序。

升序意味着项目从最低值到最高值排列。

最低值在左侧，最高值在右侧。

执行此操作的一般语法如下所示:

```
list_name.sort() 
```

让我们来看看下面的例子，它展示了如何对整数列表进行排序:

```
# a list of numbers
my_numbers = [10, 8, 3, 22, 33, 7, 11, 100, 54]

#sort list in-place in ascending order
my_numbers.sort()

#print modified list
print(my_numbers)

#output

#[3, 7, 8, 10, 11, 22, 33, 54, 100] 
```

在上面的例子中，数字从最小到最大排序。

在处理字符串列表时，也可以达到同样的效果:

```
# a list of strings
programming_languages = ["Python", "Swift","Java", "C++", "Go", "Rust"]

#sort list in-place in alphabetical order
programming_languages.sort()

#print modified list
print(programming_languages)

#output

#['C++', 'Go', 'Java', 'Python', 'Rust', 'Swift'] 
```

在这种情况下，列表中包含的每个字符串都按字母顺序排序。

正如您在两个示例中看到的，原始列表被直接更改了。

## 如何使用`sort()`方法对列表项进行降序排序

降序与升序相反，元素从最高值到最低值排列。

要对列表项进行降序排序，需要在`sort()`方法中使用可选的`reverse`参数，并将其值设置为`True`。

这样做的一般语法如下所示:

```
list_name.sort(reverse=True) 
```

让我们重复使用上一节中的同一个示例，但这次要让数字以相反的顺序排序:

```
# a list of numbers
my_numbers = [10, 8, 3, 22, 33, 7, 11, 100, 54]

#sort list in-place in descending order
my_numbers.sort(reverse=True)

#print modified list
print(my_numbers)

#output

#[100, 54, 33, 22, 11, 10, 8, 7, 3] 
```

现在所有的数字都反向排列，最大值在左边，最小值在右边。

在处理字符串列表时，也可以达到同样的效果。

```
# a list of strings
programming_languages = ["Python", "Swift","Java", "C++", "Go", "Rust"]

#sort list in-place in  reverse alphabetical order
programming_languages.sort(reverse=True)

#print modified list
print(programming_languages)

#output

#['Swift', 'Rust', 'Python', 'Java', 'Go', 'C++'] 
```

列表项现在按相反的字母顺序排列。

## 如何通过`sort()`方法使用`key`参数对列表项进行排序

您可以使用`key`参数来执行更多定制的排序操作。

分配给`key`参数的值需要是可调用的。

Callable 是可以被调用的东西，这意味着它可以被调用和引用。

可调用对象的一些例子是方法和函数。

分配给`key`的这个方法或函数将在任何排序发生之前应用于列表中的所有元素，并将指定排序标准的逻辑。

假设您想要根据字符串的长度对字符串列表进行排序。

为此，您将内置的`len()`函数分配给`key`参数。

`len()`函数将通过计算元素中包含的字符来计算存储在列表中的每个元素的长度。

```
programming_languages = ["Python", "Swift","Java", "C++", "Go", "Rust"]

programming_languages.sort(key=len)

print(programming_languages)

#output

#['Go', 'C++', 'Java', 'Rust', 'Swift', 'Python'] 
```

在上面的例子中，字符串以默认的升序排序，但是这次排序是基于它们的长度。

最短的绳子在左手边，最长的在右手边。

也可以组合使用`key`和`reverse`参数。

例如，您可以根据列表项的长度对其进行排序，但以降序排列。

```
programming_languages = ["Python", "Swift","Java", "C++", "Go", "Rust"]

programming_languages.sort(key=len, reverse=True)

print(programming_languages)

#output

#['Python', 'Swift', 'Java', 'Rust', 'C++', 'Go'] 
```

在上面的例子中，字符串从最长到最短。

另一件要注意的事情是，您可以创建自己的自定义排序函数，以创建更明确的排序标准。

例如，您可以创建一个特定的函数，然后根据该函数的返回值对列表进行排序。

假设您有一个包含编程语言和每种编程语言创建年份的字典列表。

```
programming_languages = [{'language':'Python','year':1991},
{'language':'Swift','year':2014},
{'language':'Java', 'year':1995},
{'language':'C++','year':1985},
{'language':'Go','year':2007},
{'language':'Rust','year':2010},
] 
```

您可以定义一个自定义函数，从字典中获取特定键的值。

💡请记住，字典键和`sort()`接受的`key`参数是两回事！

具体来说，该函数将获取并返回字典列表中的`year`键的值，该值指定了字典中每种语言的创建年份。

返回值将作为列表的排序标准。

```
programming_languages = [{'language':'Python','year':1991},
{'language':'Swift','year':2014},
{'language':'Java', 'year':1995},
{'language':'C++','year':1985},
{'language':'Go','year':2007},
{'language':'Rust','year':2010},
]

def get_year(element):
    return element['year'] 
```

然后，您可以根据您之前创建的函数的返回值进行排序，方法是将它分配给`key`参数，并按照默认的升序时间顺序进行排序:

```
programming_languages = [{'language':'Python','year':1991},
{'language':'Swift','year':2014},
{'language':'Java', 'year':1995},
{'language':'C++','year':1985},
{'language':'Go','year':2007},
{'language':'Rust','year':2010},
]

def get_year(element):
    return element['year']

programming_languages.sort(key=get_year)

print(programming_languages) 
```

输出:

```
[{'language': 'C++', 'year': 1985}, {'language': 'Python', 'year': 1991}, {'language': 'Java', 'year': 1995}, {'language': 'Go', 'year': 2007}, {'language': 'Rust', 'year': 2010}, {'language': 'Swift', 'year': 2014}] 
```

如果您想从最近创建的语言到最早的语言进行排序，或者以降序排序，那么您可以使用`reverse=True`参数:

```
programming_languages = [{'language':'Python','year':1991},
{'language':'Swift','year':2014},
{'language':'Java', 'year':1995},
{'language':'C++','year':1985},
{'language':'Go','year':2007},
{'language':'Rust','year':2010},
]

def get_year(element):
    return element['year']

programming_languages.sort(key=get_year, reverse=True)

print(programming_languages) 
```

输出:

```
[{'language': 'Swift', 'year': 2014}, {'language': 'Rust', 'year': 2010}, {'language': 'Go', 'year': 2007}, {'language': 'Java', 'year': 1995}, {'language': 'Python', 'year': 1991}, {'language': 'C++', 'year': 1985}] 
```

为了达到完全相同的结果，你可以创建一个 lambda 函数。

除了使用使用`def`关键字定义的常规自定义函数，您还可以:

*   创建一个简洁的单行表达式，
*   不要像对`def`函数那样定义函数名。Lambda 函数也被称为*匿名*函数。

```
programming_languages = [{'language':'Python','year':1991},
{'language':'Swift','year':2014},
{'language':'Java', 'year':1995},
{'language':'C++','year':1985},
{'language':'Go','year':2007},
{'language':'Rust','year':2010},
]

programming_languages.sort(key=lambda element: element['year'])

print(programming_languages) 
```

用行`key=lambda element: element['year']`指定的 lambda 函数将这些编程语言从最旧到最新排序。

## `sort()`和`sorted()`的区别

`sort()`方法的工作方式类似于`sorted()`函数。

`sorted()`函数的一般语法如下:

```
sorted(list_name,reverse=...,key=...) 
```

让我们来分解一下:

*   `sorted()`是一个接受 iterable 的内置函数。然后按升序或降序排序。
*   `sorted()`接受三个参数。一个参数是必需的，另外两个是可选的。
*   `list_name`是**必需的**参数。在这种情况下，参数是 list，但是`sorted()`接受任何其他的 iterable 对象。
*   `sorted()`也接受可选参数`reverse`和`key`，它们与`sort()`方法接受的可选参数相同。

`sort()`和`sorted()`的主要区别在于，`sorted()`函数接受一个列表，**返回它的一个新的排序副本**。

新副本包含按排序顺序排列的原始列表的元素。

原始列表中的元素不受影响，保持不变。

所以，总结一下不同之处:

*   `sort()`方法没有返回值，直接修改原始列表，改变其中包含的元素顺序。
*   另一方面，`sorted()`函数有一个返回值，它是原始列表的一个排序副本。该副本包含按排序顺序排列的原始列表的列表项。最后，原始列表保持不变。

让我们看看下面的例子，看看它是如何工作的:

```
#original list of numbers
my_numbers = [10, 8, 3, 22, 33, 7, 11, 100, 54]

#sort original list in default ascending order
my_numbers_sorted = sorted(my_numbers)

#print original list
print(my_numbers)

#print the copy of the original list that was created
print(my_numbers_sorted)

#output

#[10, 8, 3, 22, 33, 7, 11, 100, 54]
#[3, 7, 8, 10, 11, 22, 33, 54, 100] 
```

由于没有向`sorted()`提供额外的参数，它以默认的升序对原始列表的副本进行排序，从最小值到最大值。

当打印原始列表时，您会看到它保持不变，并且项目保持其原始顺序。

正如您在上面的例子中看到的，列表的副本被赋给了一个新变量`my_numbers_sorted`。

这样的事情用`sort()`是做不到的。

看看下面的例子，看看如果用`sort()`方法尝试会发生什么。

```
my_numbers = [10, 8, 3, 22, 33, 7, 11, 100, 54]

my_numbers_sorted = my_numbers.sort()

print(my_numbers)
print(my_numbers_sorted)

#output

#[3, 7, 8, 10, 11, 22, 33, 54, 100]
#None 
```

你看到`sort()`的返回值是`None`。

最后，另一件要注意的事情是，`sorted()`函数接受的`reverse`和`key`参数的工作方式与您在前面章节中看到的`sort()`方法完全相同。

### 何时使用`sort()`和`sorted()`

下面列出了一些你在决定是否应该使用`sort()`还是`sorted()`时可能要考虑的事情。

首先，考虑您正在处理的数据类型:

*   如果您从一开始就严格地使用列表，那么您将需要使用`sort()`方法，因为`sort()`只在列表上被调用。
*   另一方面，如果您想要更大的灵活性，并且还没有使用列表，那么您可以使用`sorted()`。`sorted()`函数接受并排序任何可迭代的对象(如字典、元组和集合),而不仅仅是列表。

接下来，另一件要考虑的事情是，保持您正在处理的列表的原始顺序是否重要:

*   调用`sort()`时，原列表会被更改，原订单会丢失。您将无法检索列表元素的原始位置。当你确定你想要改变你正在处理的列表并且确定你不想保留它的顺序时，使用`sort()`。
*   另一方面，`sorted()`在你想创建一个新列表但又想保留你正在使用的列表时很有用。`sorted()`函数将创建一个新的排序列表，列表元素按照期望的顺序排序。

最后，在处理大型数据集时，您可能需要考虑的另一件事是时间和内存效率:

*   `sort()`方法占用和消耗更少的内存，因为它只是就地对列表进行排序，不会创建你不需要的新列表。出于同样的原因，它也稍微快一些，因为它不创建副本。当您处理包含更多元素的大型列表时，这很有帮助。

## 结论

现在你知道了！您现在知道了如何使用`sort()`方法在 Python 中对列表进行排序。

您还了解了使用`sort()`和`sorted()`对列表进行排序的主要区别。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！*