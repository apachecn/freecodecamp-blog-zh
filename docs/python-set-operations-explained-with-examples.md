# Python 集合–操作和示例

> 原文：<https://www.freecodecamp.org/news/python-set-operations-explained-with-examples/>

如果你是 Python 的初学者，你可能已经遇到过列表。但是你听说过 Python 中的集合吗？

在本教程中，我们将探索什么是集合，如何创建它们，以及可以对它们使用的不同操作。

# Python 中的集合是什么？

在 Python 中，集合和列表完全一样，除了它们的元素是*不可变的*(这意味着一旦声明，就不能更改/改变集合的元素)。但是，您可以在集合中添加/移除元素。

如果这令人困惑，让我试着总结一下:

> 集合是可变的、无序的元素组，其中元素本身是不可变的。

集合的另一个特征是它可以包含不同类型的元素。这意味着您可以拥有一组数字、字符串，甚至元组，所有这些都在同一个集合中！

# 如何创建集合

在 Python 中创建集合最常见的方式是使用内置的`set()`函数。

```
>>> first_set = set(("Connor", 32, (1, 2, 3)))
>>> first_set
{32, 'Connor', (1, 2, 3)}
>>> 
>>> second_set = set("Connor")
>>> second_set
{'n', 'C', 'r', 'o'}
```

您也可以使用花括号`{}`语法创建集合:

```
>>> third_set = {"Apples", ("Bananas", "Oranges")}
>>> type(third_set)
<class 'set'>
```

`set()`函数接受一个*可迭代对象*，并产生一个将被插入集合的对象列表。`{}`语法将对象本身放入集合中。

您可能已经意识到，无论是使用`set()`函数还是`{}`来创建集合，每个元素都需要是不可变的对象。因此，如果您将一个列表(可变对象)添加到一个集合中，您将遇到一个错误:

```
>>> incorrect_set = {"Apples", ["Bananas", "Oranges"]}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list' 
```

# 如何添加或删除集合中的元素

我们已经知道集合是可变的。这意味着您可以添加/删除集合中的元素。

下面是一个使用`update()`函数向集合中添加元素的例子。

```
>>> add_set = set((1, 2, 3, 4))
>>> add_set
{1, 2, 3, 4}
>>>
>>> add_set.update((1,))
>>> add_set
{1, 2, 3, 4}
>>> add_set.update(("cello", "violin"))
>>> add_set
{1, 2, 3, 4, 'violin', 'cello'}
```

但是请注意，当我们再次尝试将“cello”添加到布景中时，没有任何变化:

```
>>> add_set.update(("cello",))
>>> add_Set
{1, 2, 3, 4, 'violin', 'cello'}
```

这是因为 Python 中的集合*不能*包含重复。因此，当我们试图再次将`"cello"`添加到集合中时，Python 识别出我们试图添加一个重复的元素，并且没有更新集合。这是区别集合和列表的一个注意事项。

以下是从集合中移除元素的方法:

```
>>> sub_set = add_set
>>> sub_set.remove("violin")
>>> sub_set
{1, 2, 3, 4, 'cello'}
```

函数的作用是:从集合中删除元素。如果`x`不是集合的一部分，它返回一个`KeyError`:

```
>>> sub_set.remove("guitar")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'guitar'
```

从集合中移除元素还有两种其他方法:

*   `discard(x)`方法将`x`从集合中移除，但是如果`x`不在集合中，*不会让*产生任何错误。
*   方法从集合中移除并返回一个随机元素。
*   方法从集合中移除所有元素

这里有一些例子来说明:

```
>>> m_set = set((1, 2, 3, 4))
>>> 
>>> m_set.discard(5) # no error raised even though '5' is not present in the set
>>>
>>> m_set.pop()
4
>>> m_set
{1, 2, 3}
>>>
>>> m_set.clear()
>>> m_set
set()
```

# Python set()操作

如果你还记得你的基础高中数学，你可能会回忆起数学集合运算，如*并*、*交*、*差*和*对称差*。嗯，你可以用 Python 集合实现同样的事情。

## 1.集合联合

两个集合的并集是两个集合的所有元素的*的集合，没有重复。您可以使用`union()`方法或`|`语法来查找 Python 集合的并集。*

```
>>> first_set = {1, 2, 3}
>>> second_set = {3, 4, 5}
>>> first_set.union(second_set)
{1, 2, 3, 4, 5}
>>>
>>> first_set | second_set     # using the `|` operator
{1, 2, 3, 4, 5} 
```

## 2.设置交集

两个集合的交集是两个集合的所有公共元素的集合*。您可以使用`&`操作符的`intersection()`方法来查找 Python 集合的交集。*

```
>>> first_set = {1, 2, 3, 4, 5, 6}
>>> second_set = {4, 5, 6, 7, 8, 9}
>>> first_set.intersection(second_set)
{4, 5, 6}
>>>
>>> first_set & second_set     # using the `&` operator
{4, 5, 6}
```

## 3.集合差异

两个集合之间的区别是第一个集合中的所有元素的集合，这些元素中的*不存在于第二个集合中。在 Python 中，您可以使用`difference()`方法或`-`操作符来实现这一点。*

```
>>> first_set = {1, 2, 3, 4, 5, 6}
>>> second_set = {4, 5, 6, 7, 8, 9}
>>> first_set.difference(second_set)
{1, 2, 3}
>>>
>>> first_set - second_set     # using the `-` operator
{1, 2, 3}
>>>
>>> second_set - first_set
{8, 9, 7}
```

## 4.设置对称差

两组之间的对称差是第一组*的*或第二组*的*中的*所有元素的集合，而不是两个*中的所有元素的集合。

在 Python 中，您可以选择使用`symmetric_difference()`方法或`^`操作符来完成这项工作。

```
>>> first_set = {1, 2, 3, 4, 5, 6}
>>> second_set = {4, 5, 6, 7, 8, 9}
>>> first_set.symmetric_difference(second_set)
{1, 2, 3, 7, 8, 9}
>>>
>>> first_set ^ second_set     # using the `^` operator
{1, 2, 3, 7, 8, 9}
```

# 如何通过操作修改集合

我们上面讨论的每一个`set()`操作都可以用来*修改*一个现有的 Python 集合。类似于你如何使用一个扩充的赋值语法，比如`+=`或`*=`来更新一个变量，你可以对集合做同样的事情:

```
>>> a = {1, 2, 3, 4, 5, 6}
>>> b = {4, 5, 6, 7, 8, 9}
>>>
>>> a.update(b)          # a "union" operation
>>> a
{1, 2, 3, 4, 5, 6, 7, 8, 9}
>>>
>>> a &= b               # the "intersection" operation
>>> a
{4, 5, 6, 7, 8, 9}
>>>
>>> a -= set((7, 8, 9))  # the "difference" operation
>>> a
{4, 5, 6}
>>>
>>> a ^= b               # the "symmetric difference" operation
>>> a
{7, 8, 9}
```

# Python 中的其他集合操作

这些并不常见，但它们有助于了解集合与其他集合之间的关系。

*   如果`a`是`b`的*子集*，则`a.issubset(b)`方法或`<=`操作符返回 true
*   如果`a`是`b`的*超集*，则`a.issuperset(b)`方法或`>=`操作符返回 true
*   如果集合`a`和`b`之间*没有公共元素*，则`a.isdisjoint(b)`方法返回 true

# Python 中的冻结集

因为集合是可变的，所以它们是不可变的——这意味着不能将它们用作字典键。

Python 允许您通过使用`frozenset`来解决这个问题。它拥有一个集合的所有属性，除了它是*不可变的*(这意味着你不能从 frozenset 中添加/移除元素)。它也是可散列的，所以它可以用作字典的键。

`frozenset`数据类型拥有一个集合的所有方法(比如`difference()`、`symmetric_difference`和`union`，但是因为它是不可变的，所以它没有添加/移除元素的方法。

```
>>> a = frozenset((1, 2, 3, 4))
>>> b = frozenset((3, 4, 5, 6))
>>>
>>> a.issubset(b)
False
>>> a.update(b)    # raises an error
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'update' 
```

而使用`frozenset` s 作为字典键就像 1，2，3 一样简单:

```
>>> a = frozenset((1, 2, 3, 4))
>>> b = frozenset(("w", "x", "y", "z"))
>>>
>>> d = {a: "hello", b: "world"}
>>> d
{frozenset({1, 2, 3, 4}): 'hello', frozenset({'w', 'x', 'y', 'z'}): 'world'}
```

# 包扎

就是这样！您已经学习了什么是集合，如何创建和使用集合，以及可以对它们使用的不同操作。

设置完成后，您现在应该对大多数 Python 内置函数都很熟悉了。你现在需要做的就是练习。祝你好运！

请务必在 Twitter 上[关注我，了解更多更新。祝你愉快！](http://twitter.com/jasmcaus)