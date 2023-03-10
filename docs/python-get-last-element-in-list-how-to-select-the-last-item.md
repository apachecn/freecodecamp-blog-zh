# Python 获取列表中的最后一个元素–如何选择最后一个项目

> 原文：<https://www.freecodecamp.org/news/python-get-last-element-in-list-how-to-select-the-last-item/>

列表是 Python 中内置的数据类型之一。您可以使用它们在一个变量中存储多个元素。

列表使我们能够将相似的数据分组在一起，并且我们还可以同时对这些分组的元素执行操作。

在本文中，我们将讨论如何获取列表中的最后一项。我们将首先解释如何访问列表中的项目，然后看看我们可以选择最后一个项目的一些方法。

## 如何在 Python 中访问列表中的项目

在这一节中，我们将讨论如何使用索引来访问存储在列表中的数据。

列表看起来是这样的:

```
myList = ["yes", "no", "maybe"]
```

这些项目嵌套在方括号中，每个项目用逗号分隔。

列表中的项目在创建列表时或在我们将它们添加到列表中时都有分配给它们的索引号。这个索引号从零开始。因此，列表中的第一项的索引号为 0，第二项的索引号为 1，依此类推。

在上例中我们创建的列表中，`"yes"`的索引是 0，`"no"`的索引是 1，`"maybe"`的索引是 2。

```
myList = ["yes", "no", "maybe"]

print(myList[0]) # yes
print(myList[1]) # no
print(myList[2]) # maybe
```

现在，让我们来看一些可以用来选择列表中最后一项的方法。

## 如何使用负索引选择列表中的最后一项

就像我们在上一节中建立的那样，列表中的每一项都有一个从零开始的索引号。

在 Python 中，我们也可以对这些索引使用负值。正索引从 0 开始(表示第一个元素的位置)，负索引从-1 开始，这表示最后一个元素的位置。

这里有一个例子:

```
myList = ["yes", "no", "maybe"]

print(myList[-1]) # maybe 
```

我们将-1 作为索引传递，并获得了返回使用的最后一项。同样，如果我们使用-2 的索引，我们将返回`"no"`。如果我们使用-3 的索引，我们将返回`"yes"`。

```
myList = ["yes", "no", "maybe"]

print(myList[-1]) # maybe
print(myList[-2]) # no
print(myList[-3]) # yes 
```

使用不存在的索引会导致错误。

在下一节中，我们将看到如何使用`pop()`方法选择最后一项。

## 如何使用`pop()`方法选择列表中的最后一项

当`pop()`方法选择最后一项时，它也会删除它——所以只有当你真的想删除列表中的最后一项时，你才应该使用这个方法。

这里有一个例子:

```
myList = ["yes", "no", "maybe"]

print(myList.pop()) # maybe

print(myList) # ['yes', 'no'] 
```

在使用了`pop()`方法之后，我们得到了打印的最后一个元素的值。但是当我们将列表打印到控制台时，我们可以看到最后一项已经不存在了。

## 结论

在本文中，我们学习了如何使用负索引选择列表中的最后一项，以及如何使用 Python 中的`pop()`方法选择和删除最后一项。

编码快乐！