# python ITER tools——chain、isSlice 和 izip 举例说明

> 原文：<https://www.freecodecamp.org/news/python-itertools-chain-isslice-and-izip-explained-with-examples/>

Itertools 是返回生成器的函数的 Python 模块，生成器是仅在迭代时起作用的对象。

## 链条()

`chain()`函数接受几个迭代器作为参数。它遍历每个传递的 iterable 的每个元素，然后返回一个迭代器，其中包含所有传递的迭代器的内容。

```
import itertools
list(itertools.chain([1, 2], [3, 4]))

# Output
# [1, 2, 3, 4]
```

## 伊斯利斯()

`islice()`函数从传递的迭代器中返回特定的元素。

对于列表，它采用与`slice()`操作符相同的参数:start、stop 和 step。开始和停止是可选的。

```
import itertools
list(itertools.islice(count(), 5))

# Output
# [0, 1, 2, 3, 4]
```

## izip()

`izip()`返回一个迭代器，将传递的迭代器的元素组合成元组。

它的工作方式与`zip()`类似，但是返回一个迭代器而不是一个列表。

```
import itertools
list(izip([1, 2, 3], ['a', 'b', 'c']))

# Output
# [(1, 'a'),(2, 'b'),(3, 'c')]
```

## 更多信息:

*   [学习 Python 数据分析——4 小时免费课程](https://www.freecodecamp.org/news/learn-data-analysis-with-python-course/)
*   多线程 Python:突破 I/O 瓶颈？