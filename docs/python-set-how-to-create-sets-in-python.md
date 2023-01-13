# Python 集合——如何在 Python 中创建集合

> 原文：<https://www.freecodecamp.org/news/python-set-how-to-create-sets-in-python/>

您可以使用 Python 中的集合在单个变量中存储数据集合。

Python 中的每个内置数据结构，如列表、字典和元组，都有其独特的特性。

以下是 Python 中集合的一些特性:

*   不允许重复的项目。如果项目出现多次，则只有一个项目会在集合中被识别。
*   器械包中的物品是无序的。每次使用时，集合的顺序都会改变。
*   一旦创建了集合，就不能修改/更改集合中项目的值。

在本文中，您将学习如何创建集合。您还将学习如何在 Python 中访问、添加和移除集合中的项目。

最后，我们将讨论 Python 编程和数学中集合的一些用例。

## 如何在 Python 中创建集合

我们使用花括号来存储集合中的项目。以下是器械包的外观:

```
nameSet = {"John", "Jane", "Doe"}

print(nameSet)
# {'Jane', 'Doe', 'John'}
```

在上面的代码中，我们创建了一个名为`nameSet`的集合。

您会注意到，当集合被打印出来时，值以不同的顺序出现。这是我上面提到的 Python 中集合的特性之一。

这是另一个有一个重复项目的例子:

```
nameSet = {"John", "Jane", "Doe", "Jane"}

print(nameSet)
# {'Jane', 'Doe', 'John'}
```

上面的例子中忽略了重复的“Jane”。这是因为不允许重复的项目。

## 如何在 Python 中访问集合中的项目

您可以使用循环来访问和打印集合中的项目。您不能使用项目的索引来访问它们，因为顺序总是在变化——没有项目保留其索引。

这里有一个例子:

```
nameSet = {"John", "Jane", "Doe"}

for names in nameSet:
    print(names)
    # John
    # Doe
    # Jane
```

我们使用一个`for`循环来打印`nameSet`中的项目

在下一节中，您将看到如何向集合中添加项目。

## 如何在 Python 中向集合中添加项目

在 Python 中，可以通过使用`add()`方法将一个条目添加到集合中，并将要添加的新条目作为参数传递。

这里有一个例子:

```
nameSet = {"John", "Jane", "Doe"}

nameSet.add("Ihechikara")

print(nameSet)
# {'John', 'Ihechikara', 'Doe', 'Jane'}
```

我们增加了一个新的项目——“Ihechikara”——到集合:`nameSet.add("Ihechikara")`。

您还可以使用`update()`方法将另一个集合或其他数据结构(列表、字典和元组)中的项目添加到集合中。

这里有一个例子:

```
nameSet = {"John", "Jane", "Doe"}

nameSet2 = {"Jade", "Jay"}

nameSet.update(nameSet2)

print(nameSet)
# {'Doe', 'Jay', 'Jane', 'Jade', 'John'}
```

为了添加从`nameSet2`到`nameSet`的名字，我们将`nameSet2`作为参数传递给`update()`方法:`nameSet.update(nameSet2)`。

## 如何在 Python 中移除集合中的项目

您可以使用不同的方法来移除器械包中的物品。现在让我们来看看它们。

### 如何在 Python 中使用`discard()`方法移除集合中的项目

您可以使用`discard()`方法移除指定的项目。这里有一个例子:

```
nameSet = {"John", "Jane", "Doe"}

nameSet.discard("John")

print(nameSet)
# {'Doe', 'Jane'}
```

在上面的例子中，“John”作为一个参数传递给了`discard()`方法，所以它被从集合中删除了。

### 如何在 Python 中使用`remove()`方法移除集合中的项目

`remove()`方法的工作方式与`discard()`方法相同。

```
nameSet = {"John", "Jane", "Doe"}

nameSet.remove("Jane")

print(nameSet)
# {'John', 'Doe'}
```

### 如何在 Python 中使用`clear()`方法清空集合

为了清除集合中的所有项目，我们使用了`clear()`方法。

这里有一个例子:

```
nameSet = {"John", "Jane", "Doe"}

nameSet.clear()

print(nameSet)
# set()
```

## Python 中何时使用集合

在这一节中，我们将讨论 Python 中集合的两个重要用例。

我们可以使用集合来删除其他数据结构中的重复项。

我们还可以执行一些很酷的数学运算，比如获取两个或更多集合的并集、交集、差集和对称差集。

### 如何使用集合删除其他数据结构中的重复项

我们可以使用集合来消除列表和元组等其他数据结构中的重复项。

当您处理一个非常大的数据集，只需要返回单个单元的项目，而不是项目的出现次数时，这是非常有用的。

这里有一个例子:

```
numberList = [2, 2, 4, 8, 9, 10, 8, 2, 5, 7, 3, 4, 7, 9]

numberSet = set(numberList)

print(numberSet)
# {2, 3, 4, 5, 7, 8, 9, 10}
```

在上面的代码中，我们创建了一个名为`numberList`的数字列表，其中的一些数字多次出现:`[2, 2, 4, 8, 9, 10, 8, 2, 5, 7, 3, 4, 7, 9]`。

使用`set()`方法，我们将列表`numberList`转换为集合:`numberSet = set(numberList)`。

当新的集合被打印出来时，所有重复的数字都被删除了——我们只得到一个返回的数字:`{2, 3, 4, 5, 7, 8, 9, 10}`。

### 如何在 Python 中使用集合执行数学运算

Python 中的集合类似于数学中的集合，我们可以根据集合之间存在的关系得到各种结果。

在本节中，您将看到如何在 Python 中获得集合之间的并、交、差和对称差。

您可以使用两套以上的设备来执行本节中的所有操作。为了尽可能简单，我们将在每个例子中只使用两个集合。

#### 如何在 Python 中获得集合的并集

两个集合的并集是存在于两个集合中的所有单个项目的集合。在联合中，重复项被忽略。

这里有一个例子:

```
firstSet = {2, 3, 4, 5}

secondSet = {1, 3, 5, 7}

print(firstSet | secondSet)
# {1, 2, 3, 4, 5, 7}
```

在上面的例子中，我们有两个集合——`firstSet = {2, 3, 4, 5}`和`secondSet = {1, 3, 5, 7}`。

使用竖线操作符(`|`)，我们能够得到两个集合的并集:`firstSet | secondSet`。

集合的并集如下:{1，2，3，4，5，7}。您可以看到这两个集合已经合并为一个集合，没有任何重复。

#### Python 中如何求集合的交集

两个集合的交集是两个集合共有的项的集合。在我们的例子中，它是同时出现在`firstSet`和`secondSet`中的项目集。

这里有一个例子:

```
firstSet = {2, 3, 4, 5}

secondSet = {1, 3, 5, 7}

print(firstSet & secondSet)
# {3, 5}
```

在这个例子中，我们使用了`&`操作符来获取`firstSet`和`secondSet` : `firstSet & secondSet`的交集。

我们返回了{3，5}，因为 3 和 5 同时出现在两个集合中。

#### 如何在 Python 中获取集合之间的差异

两个集合之间的区别在于一个集合中存在而另一个集合中不存在的所有项目的集合。

这里有一个例子:

```
firstSet = {2, 3, 4, 5}

secondSet = {1, 3, 5, 7}

print(firstSet - secondSet)
# {2, 4}
```

在上面的例子中，我们得到了一组存在于`firstSet`中而不存在于`secondSet`中的项目。

我们使用了`-`操作符来实现这个功能:`firstSet - secondSet`。

运算的结果是 2 和 4。

#### Python 中如何求集合的对称差

两个集合的对称差是指存在于两个集合中任何一个而不是两个集合中的所有项目的集合。

在上一节中，我们看到了只存在于一个集合中而不存在于另一个集合中的项目的结果。对称差异是由存在于每个集合中但不存在于两个集合中的项目造成的。

```
firstSet = {2, 3, 4, 5}

secondSet = {1, 3, 5, 7}

print(firstSet ^ secondSet)
# {1, 2, 4, 7}
```

我们使用了`^`运算符来获得两个集合的对称差:`firstSet ^ secondSet`。

结果是 1，2，4，7。这些项目不会同时出现在两个集合中。

## 摘要

在本文中，我们讨论了集合以及如何用 Python 创建它们。

集合不允许重复的项目，它们是无序的，并且不能修改其中存储的项目。

我们还了解了如何使用不同的方法来访问、添加和删除集合中的项目。

最后，我们讨论了在 Python 中何时使用集合。我们看到了 Python 中集合的一些应用及其在数学运算中的使用。

编码快乐！