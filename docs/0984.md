# Python 添加到字典——向字典添加一个条目

> 原文：<https://www.freecodecamp.org/news/python-add-to-dictionary-adding-an-item-to-a-dict/>

数据结构帮助我们组织和存储数据集合。Python 有内置的数据结构，如列表、集合、元组和字典。

每种结构都有自己的语法和方法来与存储的数据进行交互。

在这篇文章中，我们将讨论字典，它们的特性，以及如何向它们添加条目。

## 如何用 Python 创建字典

字典由嵌套在花括号中的键和值对组成。这里有一个字典的例子:

```
devBio = {
  "name": "Ihechikara",
  "age": 120,
  "language": "JavaScript"
}
print(devBio)
# {'name': 'Ihechikara', 'age': 120, 'language': 'JavaScript'}
```

在上面的代码中，我们创建了一个名为`devBio`的字典，其中包含了一个开发人员的信息——这个开发人员的年龄非常大。

字典中的每个键——`name`、`age`和`language`——都有相应的值。逗号分隔每个键和值对。省略逗号会给你带来错误。

在我们深入研究如何向字典添加条目之前，让我们先来看看字典的一些特性。这将帮助您轻松地将它们与 Python 中的其他数据结构区分开来。

## 词典的特点

以下是 Python 中字典的一些特性:

### 不允许重复的密钥

如果我们创建一个字典，其中有两个或多个相同的键，其中最后一个键将覆盖其余的键。这里有一个例子:

```
devBio = {
  "name": "Ihechikara",
  "name": "Vincent",
  "name": "Chikara",
  "age": 120,
  "language": "JavaScript"
}
print(devBio)
# {'name': 'Chikara', 'age': 120, 'language': 'JavaScript'}
```

我们创建了三个键名相同的键`name`。当我们将字典打印到控制台时，最后一个值为“Chikara”的键覆盖了其余的键。

让我们看看下一个特征。

### 字典中的条目是可变的

将项目分配给字典后，您可以将其值更改为不同的值。

```
devBio = {
  "name": "Ihechikara",
  "age": 120,
  "language": "JavaScript"
}

devBio["age"] = 1

print(devBio)

# {'name': 'Chikara', 'age': 120, 'language': 'JavaScript'}
```

在上面的例子中，我们给`age`重新分配了一个新值。这将覆盖我们在创建字典时分配的初始值。

我们还可以使用`update()`方法来改变字典中条目的值。我们可以通过使用`update()`方法——即:`devBio.update({"age": 1})`,在最后一个例子中获得相同的结果。

### 字典中的条目是有序的

通过排序，这意味着字典中的条目保持它们被创建或添加的顺序。这一秩序不能改变。

在 Python 3.7 之前，Python 中的字典是无序的。

在下一节中，我们将看到如何向字典中添加条目。

## 如何向词典中添加条目

向字典添加条目的语法与我们在更新条目时使用的语法相同。这里唯一的区别是索引键将包含要创建的新键的名称及其对应的值。

语法是这样的:`devBio[**newKey**] = **newValue**` **。**

我们还可以使用`update()`方法向字典中添加新条目。下面是它的样子:`devBio.**update**({"**newKey**": **newValue**})`。

让我们看一些例子。

```
devBio = {
  "name": "Ihechikara",
  "age": 120,
  "language": "JavaScript"
}

devBio["role"] = "Developer"

print(devBio)

# {'name': 'Ihechikara', 'age': 120, 'language': 'JavaScript', 'role': 'Developer'}
```

上面，使用索引键`devBio["role"]`，我们创建了一个值为`Developer`的新键。

在下一个例子中，我们将使用`update()`方法。

```
devBio = {
  "name": "Ihechikara",
  "age": 120,
  "language": "JavaScript"
}

devBio.update({"role": "Developer"})

print(devBio)

# {'name': 'Ihechikara', 'age': 120, 'language': 'JavaScript', 'role': 'Developer'}
```

上面，我们通过将新的键及其值传递到`update()`方法(即:`devBio.update({"role": "Developer"})`)中，获得了与上一个示例相同的结果。

## 结论

在本文中，我们学习了 Python 中的字典，如何创建它们，以及它们的一些特性。然后，我们看到了两种向字典添加条目的方法。

编码快乐！