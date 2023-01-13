# Python enumerate()–Python 中的 Enumerate 函数是什么？

> 原文：<https://www.freecodecamp.org/news/python-enumerate-what-is-the-enumerate-function-in-python/>

Python 中的`enumerate()`函数接受一个数据集合作为参数，并返回一个枚举对象。

枚举对象以键值对格式返回。键是每个项目对应的索引，值是项目。

在本文中，我们将看到一些如何在 Python 中使用`enumerate()`函数的例子。

我们将讨论语法和参数，如何使用`enumerate()`，以及如何遍历一个枚举对象。

## Python 中 enumerate()函数的语法是怎样的？

下面是`enumerate()`函数及其参数的语法:

```
enumerate(iterable, start)
```

`enumerate()`函数接受两个参数:`iterable`和`start`。

*   `iterable`是传入的要作为枚举对象返回的数据集合。
*   `start`是枚举对象的起始索引。默认值为 0，因此如果忽略此参数，0 将用作第一个索引。

## 如何使用 Python 中的`enumerate()`函数

在这一节中，我们将看一些例子来帮助我们理解上一节中显示的语法和参数。

请注意，我们必须指定枚举对象在返回时存储的数据格式类型(如列表、集合等)。在示例中您会更好地理解这一点。

### `enumerate()`Python 示例#1 中的函数

```
names = ["John", "Jane", "Doe"]
enumNames = enumerate(names)

print(list(enumNames))
# [(0, 'John'), (1, 'Jane'), (2, 'Doe')]
```

在上面的例子中，我们创建了一个名为`names`的列表。

然后，我们将`names`变量转换成一个枚举对象:`enumerate(names)`，并将其存储在一个名为`enumNames`的变量中。

我们希望枚举对象在返回时存储在一个列表中，所以我们这样做了:`list(enumNames)`。

当打印到控制台时，结果如下:`[(0, 'John'), (1, 'Jane'), (2, 'Doe')]`

正如您在结果中看到的，它们是键值对。第一个索引是 0，附加到`names`列表中的第一项，第二个索引是 1，附加到`names`列表中的第二项，依此类推。

在我们的例子中，我们只使用了第一个参数。

在下一个例子中，我们将利用这两个参数，这样您就可以理解第二个参数是如何工作的。

### `enumerate()`Python 示例#2 中的函数

```
names = ["John", "Jane", "Doe"]
enumNames = enumerate(names, 10)

print(list(enumNames))
# [(10, 'John'), (11, 'Jane'), (12, 'Doe')]
```

在上面的例子中，我们向`enumerate()`函数添加了第二个参数:`enumerate(names, 10)`。

第二个参数是 10。这将表示枚举对象中键(索引)的起始索引。

下面是我们的结果:`[(10, 'John'), (11, 'Jane'), (12, 'Doe')]`

从结果中，我们可以看到第一个索引是 10，第二个是 11，依此类推。

## 如何在 Python 中循环遍历枚举对象

下面是一个简单的例子，展示了我们如何在 Python 中遍历枚举对象:

```
names = ["John", "Jane", "Doe"]
enumNames = enumerate(names)

for item in enumNames:
    print(item)

# (0, 'John')
# (1, 'Jane')
# (2, 'Doe') 
```

使用一个`for`循环，我们遍历枚举对象:`for item in enumNames:`

当打印出来时，我们得到了对象中的条目，这些条目按照它们对应的键-值对顺序列出。

我们还可以像在上一节中那样利用第二个参数来更改键的初始值。

## 摘要

在本文中，我们讨论了 Python 中的`enumerate()`函数。

我们首先看一下函数的语法和参数。

然后，我们看到了一些帮助我们理解如何使用`enumerate()`函数及其参数的例子。

最后，我们看到了如何使用`for`循环遍历 Python 中的枚举对象。

编码快乐！