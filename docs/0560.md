# 如何在 Python 中创建列表——在 Python 示例中声明列表

> 原文：<https://www.freecodecamp.org/news/how-to-make-a-list-in-python-declare-lists-in-python-example/>

您可以使用 Python 中的一些内置数据结构来组织和存储变量集合。

这些数据结构包括列表、集合、元组和字典。它们都有自己的语法和功能。

在本文中，我们将关注列表。您将看到 Python 中列表的一些特性，如何创建它们，以及如何添加、访问、更改和删除列表中的项目。

## Python 中列表的特性

以下是 Python 中列表的一些特性:

*   列表中的项目可以有重复的。这意味着您可以添加两个或更多同名的项目。
*   列表中的项目是按顺序排列的。当您在列表中创建和添加项目时，它们保持相同的顺序。
*   列表中的项目是**可变的**。这意味着您可以修改列表中某个项目的值。您将在本文的示例中看到如何做到这一点。

## 如何在 Python 中创建列表

为了在 Python 中创建一个列表，我们使用方括号(`[]`)。列表看起来是这样的:

```
ListName = [ListItem, ListItem1, ListItem2, ListItem3, ...]
```

注意，列表可以有/存储不同的数据类型。您可以存储特定的数据类型，也可以混合使用。

在下一节中，您将看到如何向列表中添加项目。

## 如何在 Python 中向列表添加项目

在我们开始向列表添加项目之前，让我们创建它。这将帮助您理解最后一节中的语法。

```
names = ["Jane", "John", "Jade", "Joe"] 
```

在上面的代码中，我们创建了一个名为`names`的列表，其中有四个条目——`Jane`、`John`、`Jade`和`Joe`。

我们可以使用两种方法向列表中添加条目——`append()`和`insert()`方法。

### 如何使用`append()`方法在 Python 中向列表添加条目

使用点符号，我们可以将`append()`方法附加到一个列表中，以将一个项目添加到列表中。

要添加的新项目将作为参数传递给`append()`方法。

```
names = ["Jane", "John", "Jade", "Joe"]
names.append("Doe")

print(names)
# ['Jane', 'John', 'Jade', 'Joe', 'Doe']
```

在上面的代码中，我们使用`append()`方法将“Doe”添加到列表中:`names.append("Doe")`。

### 如何使用`insert()`方法在 Python 中向列表添加条目

在上一节中看到的`append()`方法在列表的最后一个索引处添加了一个条目。

当使用`insert()`方法将项目添加到列表中时，您需要指定它应该被放置的索引。

这里有一个例子:

```
names = ["Jane", "John", "Jade", "Joe"]
names.insert(2, "Doe")

print(names)
# ['Jane', 'John', 'Doe', 'Jade', 'Joe']
```

在我们的示例中，我们在第二个索引处添加了“Doe”。列表是零索引的，所以第一项是 0，第二项是 1，第三项是 2，依此类推。

## 如何在 Python 中访问列表中的项目

您可以使用项目的索引来访问列表中的项目。

```
names = ["Jane", "John", "Jade", "Joe"]

print(names[0])
# Jane
```

在上例中，我们打印了索引为 0 的项目:`print(names[0])`。打印出来的项目是“Jane ”,因为它是列表中的第一个项目。

指数 0 =简
指数 1 =约翰
指数 2 =杰德
指数 3 =乔

使用负索引，我们可以从数组的末尾开始访问项。这里有一个例子:

```
names = ["Jane", "John", "Jade", "Joe"]

print(names[-1])
# Joe
```

Index -1 =乔
Index -2 =杰德
Index -3 =约翰
Index -4 =简

## 如何更改 Python 列表中项目的值

要改变列表中项的值，你必须引用该项的索引，然后给它赋一个新值。

这里有一个例子:

```
names = ["Jane", "John", "Jade", "Joe"]
names[0] = "Doe"

print(names)
# ['Doe', 'John', 'Jade', 'Joe']
```

在上面的代码中，我们使用项目的索引:`names[0] = "Doe"`将第一个项目的值从“Jane”更改为“Doe”。

## 如何在 Python 中移除列表中的项目

我们可以使用以下方法从列表中删除一个项目:

*   `remove()`法。
*   `pop()`法。
*   `del`关键字。

### 如何使用`remove()`方法在 Python 中移除列表中的项目

```
names = ["Jane", "John", "Jade", "Joe"]

names.remove("John")

print(names)
# ['Jane', 'Jade', 'Joe']
```

正如您在上面的例子中看到的，我们在`remove()`方法中传递了要移除的项目作为参数:`names.remove("John")`。

### 如何使用`pop()`方法在 Python 中移除列表中的项目

```
names = ["Jane", "John", "Jade", "Joe"]

names.pop()

print(names)
# ['Jane', 'John', 'Jade']
```

方法删除列表中的最后一项。

您还可以使用索引来指定要删除的特定项目。这里有一个例子:

```
names = ["Jane", "John", "Jade", "Joe"]

names.pop(2)

print(names)
# ['Jane', 'John', 'Joe']
```

### 如何在 Python 中使用`del`关键字移除列表中的项目

```
names = ["Jane", "John", "Jade", "Joe"]

del names[1]

print(names)
# ['Jane', 'Jade', 'Joe']
```

在上面的代码中，我们通过指定项目的索引:`del names[1]`使用`del`关键字删除了第二个项目。

如果在使用`del`关键字时没有指定任何索引，整个列表将被删除。那就是:

```
names = ["Jane", "John", "Jade", "Joe"]

del names

print(names)
# name 'names' is not defined
```

像我们上面做的那样删除列表后试图访问它会抛出一个错误，说列表没有定义。

如果你想清空一个列表并且仍然有一个对它的引用而不出错，那么你可以使用`clear()`方法。这里有一个例子:

```
names = ["Jane", "John", "Jade", "Joe"]

names.clear()

print(names)
# []
```

方法清空列表。当你试图访问这个列表时，你得到了返回的`[]`,因为所有的条目都已经被“清除”了。

## 摘要

在本文中，我们讨论了 Python 中的列表。

我们看到了 Python 中 list 的一些特性以及如何创建 list。

我们还看到了如何在 Python 中添加、访问、更改和删除列表中的项目。

编码快乐！