# Python 集合——如何用 Python 创建集合

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-set-in-python/>

集合被定义为对象的集合。它们是数学和编程中的一个重要概念。

编程语言提供了不同的对象分组方式。列表、字典和数组是对对象进行分组的一些例子。

Python 有自己的创建对象集合的方法。在 Python 中，创建集合是我们将项目组合在一起的方式之一。

在本教程中，我们将学习在 Python 中创建集合的不同方法，并发现它们的特征。

## Python 中集合的特征

器械包具有以下特征:

*   集合是无序的。这意味着它们不会保留创建时的原始顺序。

```
>>> x = {'a','b','c'}
>>> print(x)
>>> x
{'a', 'c', 'b'} 
```

*   集合元素必须是唯一的，因为不允许重复。如果添加了重复值，它将只显示一次。

```
>>> x = {'a','b','c','c'}
>>> print(x)
{'a', 'c', 'b'}
>>> 
```

*   集合中的元素必须是不可变的类型。但是集合本身可以通过并集、交集等操作进行修改。

## 如何在 Python 中定义集合

创建集合有两种主要方法。一种是使用`set`函数，另一种是使用花括号并单独添加对象。

首先，可以在内置的`set`函数中传递一个 iterable。

*语法:*

```
sample_set = set(<iterable>)
```

这里，`<iterable>`可以是任何可迭代的对象，例如列表、字符串或元组。

**通过`list`作为可迭代:**

```
>>> sample_set = set(['100', 'Days', 'Of', 'Code'])
>>> print(sample_set)
{'Of', '100', 'Days', 'Code'}
>>>
```

**通过`tuple`作为可迭代:**

```
>>> sample_set = set(('Tuple', 'as', 'an', 'iterable'))
>>> print(sample_set)
{'as', 'iterable', 'an', 'Tuple'}
```

**通过`string`作为可迭代:**

```
>>> s = 'Alpha'
>>> set(s)
{'p', 'l', 'a', 'h', 'A'}
```

您也可以定义一个空集。您可以像这样定义空集:

```
>>> s = set()
>>> type(s)
<class 'set'>
```

您还可以通过将 objects .单独放在花括号中来定义集合。

```
>>> s = {'I', 'Like', 'Python'}
>>> type(s)
<class 'set'>
```

关于集合，有趣的一点是元素可以是不同的数据类型:

```
>>> s = {1947, 'cat', 1.179, None, 'w'}
>>> print(s)
{1.179, 'w', 1947, 'cat', None}
>>> 
```

但是记住，集合元素必须是不可变的。由于元组是不可变的，我们可以将它们包含在集合中:

```
>>> s = {42, 'foo', ('T','U','P','L','E'), 3.14159, None}
>>> type(s)
<class 'set'>
>>> print(s)
{3.14159, ('T', 'U', 'P', 'L', 'E'), 42, 'foo', None} 
```

**一些例外:**

但是我们不能在集合中包含列表和字典，因为它们是可变的。

```
>>> a = ['This', 'is', 'a', 'list']
>>> {a}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list' 
```

Error in setting lists as a set element

```
>>> dictionary = {'month': 1, 'day': 12}
>>> {dictionary}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'dict' 
```

## 如何在 Python 中确定集合大小

我们可以使用`len()`来检查集合的长度。

```
>>> sample_set = set(['100', 'Days', 'Of', 'Code'])
>>> len(sample_set)
4
```

## 如何确定集合成员

我们可以使用`in`和`not in`操作符来确认元素的成员资格。

```
>>> sample_set = set(['100', 'Days', 'Of', 'Code'])
>>> '100' in sample_set
True
>>> '100' not in sample_set
False
>>> 'red' in sample_set
False
```

# 包扎

在本教程中，我们学习了在 Python 中创建集合的不同方法。我希望现在你对创建布景感到舒服了。

我希望这篇教程对你有所帮助。谢谢你一直读到最后。

你从这个教程中学到的最喜欢的东西是什么？在 [Twitter](https://twitter.com/hira_zaira) 上告诉我！

你也可以在这里阅读我的其他帖子[。](https://www.freecodecamp.org/news/author/zaira/)

[横幅致谢:jcomp-www.freepik.com 创建的工业革命载体]