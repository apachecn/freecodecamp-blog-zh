# Python List insert()–如何在 Python 中添加列表

> 原文：<https://www.freecodecamp.org/news/python-list-insert-how-to-add-to-a-list-in-python/>

列表数据类型是 Python 中内置的数据结构之一，还有集合、元组和字典。您使用列表来组织、分组和存储数据。

但是这些数据结构中的每一种都有区别于其他数据结构的独特特征。

在本文中，我们将看到如何用 Python 创建一个列表。我们还将看到如何使用`insert()`、`append()`和`extend()`方法向列表添加条目。

## 如何在 Python 中创建列表

为了在 Python 中创建列表，我们使用方括号。这里有一个例子:

```
myList = ['one', 'two', 'three']

print(myList)

# ['one', 'two', 'three']
```

在上面的代码中，我们创建了一个名为`myList`的列表，其中包含三个条目——“一”、“二”和“三”。正如您在上面看到的，项目在方括号中。

## 如何在 Python 中向列表添加项目？

在 Python 中向列表添加项目时，我们可以使用三种方法。分别是:`insert()`、`append()`、`extend()`。我们将把它们分成单独的小节。

### 如何使用`insert()`方法将项目添加到列表中

您可以使用`insert()`方法将一个项目插入到指定索引的列表中。列表中的每个项目都有一个索引。第一个项目的索引为零(0)，第二个项目的索引为一(1)，依此类推。

这里有一个使用`insert()`方法的例子:

```
myList = ['one', 'two', 'three']

myList.insert(0, 'zero')

print(myList)

# ['zero', 'one', 'two', 'three']
```

在上面的例子中，我们创建了一个包含三个条目的列表:`['one', 'two', 'three']`。

然后，我们使用`insert()`方法在索引 0(第一个索引)处插入一个新项目——“零”:`myList.insert(0, 'zero')`。

`insert()`方法接受两个参数——要插入的新项的索引和该项的值。

### 如何使用`append()`方法将项目添加到列表中

与允许我们指定在哪里插入项目的`insert()`方法不同，`append()`方法将项目添加到列表的末尾。新项目的值作为参数传入到`append()`方法中。

这里有一个例子:

```
myList = ['one', 'two', 'three']

myList.append('four')

print(myList)

# ['one', 'two', 'three', 'four']
```

新项目在上面的代码中作为参数传入:`myList.append('four')`。

当打印到控制台时，我们将该项目放在列表的最后一个索引处。

### 如何使用`extend()`方法将项目添加到列表中

您可以使用`extend()`方法将数据集合添加到列表中。我在这里使用“数据收集”,因为我们还可以将集合、元组等追加到列表中。

让我们看一些例子。

#### 如何使用`extend()`方法将列表追加到列表中

```
myList1 = ['one', 'two', 'three']
myList2 = ['four', 'five', 'six']
```

在上面的代码中，我们创建了两个列表—`myList1`和`myList2`。接下来，我们将把第二个列表中的项目追加到第一个列表中。

方法如下:

```
myList1.extend(myList2)
```

所以当我们打印`myList1`时，我们会得到这个:`['one', 'two', 'three', 'four', 'five', 'six']`。

以下是所有的信息:

```
myList1 = ['one', 'two', 'three']
myList2 = ['four', 'five', 'six']

myList1.extend(myList2)

print(myList1)
# ['one', 'two', 'three', 'four', 'five', 'six']
```

#### 如何使用`extend()`方法将一个元组追加到一个列表中

这里的过程与上一个例子相同，只是我们添加了一个元组。使用括号创建元组。

那就是:

```
myTuple = ('Hello', 'Hi')
```

让我们使用`extend()`方法将一个元组追加到一个列表中。

```
myList1 = ['one', 'two', 'three']
myList2 = ('four', 'five', 'six')

myList1.extend(myList2)

print(myList1)

# ['one', 'two', 'three', 'four', 'five', 'six']
```

我们得到与上一节相同的结果。

## 结论

在本文中，我们讨论了 Python 中的列表。

我们看到了如何创建列表以及向列表添加条目的各种方法。

我们使用`insert()`、`append()`和`extend()`方法将项目添加到列表中。

`insert()`方法在指定的索引中插入一个新项，`append()`方法在列表的最后一个索引处添加一个新项，而`extend()`方法向列表追加一个新的数据集合。

编码快乐！