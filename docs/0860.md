# Python。zip()–Python 中的 Zip 函数

> 原文：<https://www.freecodecamp.org/news/python-zip-zip-function-in-python/>

当您在 Python 中使用`zip()`函数时，它获取两个或更多数据集并将它们“压缩”在一起。这将返回一个包含从数据集派生的项目对的对象。它按照索引的顺序对这些项目进行分组。

让我再细分一下:第一个数据集中的第一项与第二个数据集中的第一项配对，两个数据集中的第二项彼此配对，依此类推。

在本文中，我们将通过一些例子来看看如何在 Python 中使用`zip()`函数。

## 如何使用 Python 中的`zip()`函数

下面是 Python 中`zip()`函数的语法:

```
zip(dataSet1, dataSet2, ...)
```

这里有一个例子来演示它是如何工作的:

```
names = ("John", "Jane", "Jade")
ages = (2, 4, 6)

print(zip(names, ages))
# <zip object at 0x7f8d5915cc40>
```

在上面的代码中，我们创建了两个`tuples`–`names`和`ages`。

然后我们使用了`zip()`函数:`print(zip(names, ages))`。

但是我们实际上并没有得到返回给我们的配对数据。这是因为我们必须说明它将被压缩成什么样的数据结构。这里:

```
names = ("John", "Jane", "Jade")
ages = (2, 4, 6)

zipped = zip(names, ages)

print(tuple(zipped))
# (('John', 2), ('Jane', 4), ('Jade', 6))
```

我们将压缩后的数据存储在一个名为`zipped`的变量中，在打印时，我们将它嵌套在一个`tuple` : `print(tuple(zipped))`中。

我已经注释了代码的输出:`(('John', 2), ('Jane', 4), ('Jade', 6))`。正如您在上面看到的，给定索引中的每一项都与另一个数据集的同一索引中的另一项成对出现。

也可以返回嵌套在`list`中的数据。方法如下:

```
names = ("John", "Jane", "Jade")
ages = (2, 4, 6)

zipped = zip(names, ages)

print(list(zipped))
# [('John', 2), ('Jane', 4), ('Jade', 6)]
```

这与上一个例子相同，但是我们没有使用`tuple(zipped)`，而是使用了`list(zipped)`。

同样，我们也可以使用`dict`和`set`，但是当我们使用`set`时返回的数据很可能是无序的。

我们可以这样循环压缩数据:

```
names = ("John", "Jane", "Jade", "John")
ages = (2, 4, 6)

zipped = zip(names, ages)

for(x,y) in zipped:
    print(x,y)

# John 2
# Jane 4
# Jade 6
```

## 结论

在本文中，我们学习了什么是`zip()`函数以及它在 Python 中的作用。

我们看到了如何压缩两个数据集，并使用不同的数据结构返回它们的对。

最后，我们看到了如何循环并打印压缩的数据。

编码快乐！