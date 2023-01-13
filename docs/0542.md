# 为 Python 列出 indexOf？如何获取列表中项的索引？索引()

> 原文：<https://www.freecodecamp.org/news/list-indexof-for-python-how-to-get-the-index-of-an-item-in-a-list-with-index/>

在 Python 中，我们可以使用各种技术来获取列表项的索引，比如`enumerate()`函数、`for`循环和`index()`方法。

在本文中，我们将重点讨论如何使用`index()`方法获取列表中某项的索引。

我们将从查看`index()`方法的语法开始，然后看一些例子来帮助你理解如何在你的代码中使用它。

## Python 中的`index()`方法的语法是怎样的？

`index()`方法接受其索引将作为参数返回的项目。但是这不是在`index()`方法中可以使用的唯一参数。

下面是语法的样子:

```
list.index(item, start, end)
```

以下是上述参数的明细:

*   `item`表示要搜索其索引的项目。
*   `start`，可选参数，表示项目搜索开始的起点。当您的项目有重复项时，这很有用。
*   `end`表示对项目索引的搜索应该停止/结束的索引。该参数也是可选的。

## 如何用`.index()`获取列表中某项的索引

在这一节中，您将看到如何使用`index()`方法来获取列表中某项的索引。您还将看到如何使用所有参数。

下面是第一个例子:

```
listOfNames = ['John', 'Jane', 'Doe', 'Ihechikara']

print(listOfNames.index('Jane'))
# 1
```

在上面的代码中，我们创建了一个名字列表:`listOfNames = ['John', 'Jane', 'Doe', 'Ihechikara']`。

使用`index()`方法，我们得到了列表中“Jane”的索引:`listOfNames.index('Jane')`

当打印到控制台时，1 被打印出来。

如果您不明白为什么返回 1，那么请注意列表是零索引的——所以第一项是 0，第二项是 1，依此类推。那就是:

约翰' = >指数 0
'简' = >指数 1
'多伊' = >指数 2
'伊赫奇卡拉' = >指数 3

### 如何在 Python 的`index()`方法中使用`start`和`end`参数

在本节中，您将看到如何在`index()`方法中使用`start`和`end`参数。

```
listOfNames = ['John', 'Jane', 'Doe', 'Ihechikara', 'John', 'Jane', 'Doe', 'Ihechikara']

print(listOfNames.index('Jane', 2))
# 5
```

在上面的列表中，我们有重复值的名称:`['John', 'Jane', 'Doe', 'Ihechikara', 'John', 'Jane', 'Doe', 'Ihechikara']`。

但是我们想得到第二个“简”项的索引。知道第一个“Jane”项的索引是 1，我们就可以在该项之后开始搜索。

因此，为了从第一个“Jane”项之后的索引开始搜索，我们向`index()`方法添加了另一个参数:`listOfNames.index("Jane", 2)`。现在，搜索值为“Jane”的项目的索引将从索引 2 开始。

我们返回了 5，因为这是第二个“Jane”项的索引。如果不指定起始索引，`index()`方法将返回指定项的第一个索引。

这里有第二个例子让你理解如何使用`end()`参数:

```
listOfNames = ['John', 'Jane', 'Doe', 'Ihechikara', 'John', 'Jane', 'Doe', 'Ihechikara']

print(listOfNames.index("Jane", 2,4))
# ValueError: 'Jane' is not in list
```

在上面的例子中，我们指定索引 2 为`start`索引，索引 4 为`end`索引。我们正在指定范围内搜索“Jane”的索引(索引 2 和 4)。

我们返回了一个错误:`ValueError: 'Jane' is not in list`。这是因为“Jane”不在指定的范围内。

回想一下，我们从索引 2 开始，所以:

指数 2 ( `start`指数)= >【多伊】
指数 3 = >【伊赫奇卡拉】
指数 4 ( `end`指数)= >【约翰】

从上面的索引中，您可以看到“Jane”不在该范围内，因此返回了一个错误。

在以下情况下，列表中会出现 ValueError:

*   列表中不存在要搜索的项目。
*   正在搜索的项目不在指定的搜索(开始和结束)范围内。

## 摘要

在本文中，我们讨论了 Python 中的`index()`方法。您可以用它来查找列表中某项的索引。

我们看到了一些展示如何使用`index()`方法及其`start`和`end`参数的例子。

编码快乐！