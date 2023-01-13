# 如何在 Python 中使用内置循环函数

> 原文：<https://www.freecodecamp.org/news/python-looping-functions/>

当你在 Python 中循环遍历一个序列时，比如一个列表、元组、字符串或字典，你有没有觉得你的代码很乱或者你想从中删除一些变量？

幸运的是，Python 有一些有用的`inbuilt`函数，可以让你的代码更加简洁，可读性更好。

在本教程中，我们将通过简单的例子来了解各种`inbuilt`函数，从而理解它们是如何工作的。

## 如何使用 Python 中的 enumerate()函数遍历序列

Python 的`enumerate()`函数在一个序列(列表、元组、字符串或字典)上循环，同时跟踪一个单独变量中的索引值。

让我们看看语法:

`enumerate(iterable, start)`

它由两个参数组成:

*   **可迭代**–可迭代对象或序列，即可以循环的对象。
*   **开始**–计数的索引值或开始值。默认情况下，该值从 0 开始。

下面是您得到的输出:

```
Output
[(0, item_1), (1, item_1), (2, item_2), . . ., (n, item_n)] 
```

如你所见，我们得到了 iterable 中的元素以及它们各自的索引。

让我们举一个没有索引的例子:

```
colour = ["Black", "Purple", "Brown", "Yellow", "Blue"]
list(enumerate(colour)) 
```

```
Output:
[(0, 'Black'), (1, 'Purple'), (2, 'Brown'), (3, 'Yellow'), (4, 'Blue')] 
```

如你所见，索引从 0 开始。我们没有使用第二个论点。默认情况下，索引的值从 0 开始。

让我们再举一个有指数的例子:

```
colour = ["Black", "Purple", "Brown", "Yellow", "Blue"]
list(enumerate(colour, 10)) 
```

```
Output:
[(10, 'Black'), (11, 'Purple'), (12, 'Brown'), (13, 'Yellow'), (14, 'Blue')] 
```

因此，这里我们的索引从 10 开始，因为我们将 start 参数设置为 10，从 10 开始计数。

你必须指定一个列表或元组来获得输出，否则它只会给你这个结果:

```
colour = ["Black", "Purple", "Brown", "Yellow", "Blue"]
enumerate(colour) 
```

```
Output:
<enumerate object at 0x7f6a97ad7c40> 
```

让我们举一个字典的例子:

```
colour = {"Black": 0, 
          "Purple": 2, 
          "Brown": 4, 
          "Yellow": 9, 
          "Blue": 1}
list(enumerate(colour)) 
```

```
Output:
[(0, 'Black'), (1, 'Purple'), (2, 'Brown'), (3, 'Yellow'), (4, 'Blue')] 
```

当我们迭代一个字典时，我们只得到关键字而不是字典的值。

为了遍历字典并获取值，Python 有两个内置函数:

*   这个函数帮助我们从字典中获取键值对。
*   `values()`–该函数帮助我们只从字典中获取值，而不使用键。

让我们看看这些函数的例子:

```
colour = {"Black": 0, 
          "Purple": 2, 
          "Brown": 4, 
          "Yellow": 9, 
          "Blue": 1}
list(enumerate(colour.items())) 
```

```
Output:
[(0, ('Black', 0)), (1, ('Purple', 2)), (2, ('Brown', 4)), (3, ('Yellow', 9)), (4, ('Blue', 1))] 
```

您也可以将它与 for 循环一起使用，如下所示:

```
colour = {"Black": 0, 
          "Purple": 2, 
          "Brown": 4, 
          "Yellow": 9, 
          "Blue": 1}
for ind, (keys, value) in enumerate(colour.items()):
	print(ind, keys, value) 
```

```
Output:
0 Black 0
1 Purple 2
2 Brown 4
3 Yellow 9
4 Blue 1 
```

在这个例子中，我们得到了键值对。现在我们将使用另一个函数，

```
colour = {"Black": 0, 
          "Purple": 2, 
          "Brown": 4, 
          "Yellow": 9, 
          "Blue": 1}
list(enumerate(colour.values())) 
```

```
Output:
[(0, 0), (1, 2), (2, 4), (3, 9), (4, 1)] 
```

对于 for 循环:

```
colour = {"Black": 0, 
          "Purple": 2, 
          "Brown": 4, 
          "Yellow": 9, 
          "Blue": 1}
for ind, value in enumerate(colour.values()):
	print(ind, value) 
```

```
Output:
0 0
1 2
2 4
3 9
4 1 
```

这里，在遍历字典时，您只获得值，而不是键。

## 如何使用 Python 中的 zip()函数遍历序列

`zip()`函数接受多个具有相同索引值的 iterable，并将它们组合起来并返回一个迭代器。迭代器可以是元组、列表或字典。

让我们看看语法:

```
zip(*iterables) 
```

或者

```
zip(iterator1, iterator2, ... so on) 
```

zip()函数的参数:

*   **Iterables** 可以是列表、元组、字符串、集合或字典。

让我们举个例子:

```
color = ["Blue", "Orange", "Brown", "Red"]
code = [20, 10, 56, 84]
list(zip(color, code)) 
```

```
Output:
[('Blue', 20), ('Orange', 10), ('Brown', 56), ('Red', 84)] 
```

这里，两个列表被组合或压缩在一起，我们得到一个迭代器。

如果可迭代项的长度不同，那么当最短的可迭代项用尽时，迭代器停止产生输出。

让我们举个例子:

```
color = ["Blue", "Orange", "Brown"]
code = [20, 10, 56, 84]
list(zip(color, code)) 
```

```
Output:
[('Blue', 20), ('Orange', 10), ('Brown', 56)] 
```

这里，最短的长度是颜色，我们得到的输出多达 3 种颜色和代码。所以第 4 个代码被拒绝。

在 **Python 3.10** 中，有一个检查元素长度的新参数`strict`。如果元素的长度不匹配，它会给我们一个错误。让我们举个例子:

```
color = ("Blue", "Orange", "Brown", "Purple")
code = (20, 10, 56)
for col, cod in zip(color, code, strict=True):
    print(col, cod) 
```

```
Output:
Blue 20
Orange 10
Brown 56
Traceback (most recent call last):
  File "<pyshell#14>", line 1, in <module>
    for col, cod in zip(color, code, strict=True):
ValueError: zip() argument 2 is shorter than argument 1 
```

你不需要带两个迭代器，你可以带任意数量的迭代器。让我们以三个迭代器为例:

```
Abbreviation = ['Bl', 'Or', 'Br', 'Gn']
Color = ['Blue', 'Orange', 'Brown', 'Green']
Code = [20, 10, 56, 88]
for ab, col, cod in zip(Abbreviation, Color, Code):
	print(ab, col, cod) 
```

```
Output:
Bl Blue 20
Or Orange 10
Br Brown 56
Gn Green 88 
```

您也可以使用`dict`功能创建词典。让我们举个例子:

```
Color = ['Blue', 'Orange', 'Brown', 'Green']
Code = [20, 10, 56, 88]
dict(zip(Color, Code)) 
```

```
Output:
{'Blue': 20, 'Orange': 10, 'Brown': 56, 'Green': 88} 
```

## 如何使用 Python 中的 sorted()函数遍历序列

函数从一个 iterable 对象中返回排序后的元素。

让我们看看语法:

```
sorted(iterable, key=key, reverse=reverse)
```

它由三个参数组成:

*   **iterable**–可以是列表、元组、字符串等的序列。
*   **键**–可选。这是一个可以用来定制排序顺序的函数。默认选项是无。
*   **反转**–也是可选的。它是一个布尔值。这里，默认值为**假**，按照*升序*排列。**真**将是*降序*的顺序。

让我们举个例子:

```
Color = ['Blue', 'Orange', 'Brown', 'Green']
sorted(Color) 
```

```
Output:
['Blue', 'Brown', 'Green', 'Orange'] 
```

默认情况下，您的输出将是一个列表，并且按升序排列。如果是字符串，将按字母顺序排序，如果是数字，将按数字顺序排序。

上面的输出显示了一个按升序排序的列表。如果你想让它按降序排列，你可以使用`reverse`参数。让我们举个例子:

```
Color = ('Blue', 'Orange', 'Brown', 'Green')
sorted(Color, reverse=True) 
```

```
Output:
['Orange', 'Green', 'Brown', 'Blue'] 
```

原始值保持不变，因为`sorted()`函数不会改变原始值——它只会产生输出。输出将是一个有序的列表。

`key`函数可以是内置的，也可以是用户自定义的，您可以使用它来操纵输出的顺序。

让我们先以一个内置函数为例:

```
Word = ('TO', 'is', 'apple', 'PEAR', 'LIKE')
sorted(Word, key=str.upper) 
```

```
Output:
['apple', 'is', 'LIKE', 'PEAR', 'TO'] 
```

默认情况下，输出将按升序排列。你必须像这样反转论点:

```
Word = ('TO', 'is', 'apple', 'PEAR', 'LIKE')
sorted(Word, key=str.upper, reverse=True) 
```

```
Output:
['TO', 'PEAR', 'LIKE', 'is', 'apple'] 
```

使用键参数时，参数的数量应为 1。让我们以用户定义的函数为例:

```
numb = (22, 10, 5, 34, 29)
sorted(numb, key=lambda x: x%5) 
```

```
Output:
[10, 5, 22, 34, 29] 
```

为了简单起见，我使用了 lambda 函数，但是如果您愿意，也可以使用传统的方法来定义函数。

## 如何使用 Python 中的 reversed()函数遍历序列

`reversed`函数从 iterable 对象中以相反的顺序返回元素。

让我们看看语法:

```
reversed(iterable) 
```

反函数的自变量:

*   **iterable【T1—**它是一个类似链表、元组、集合等的序列。****

**让我们举个例子:**

```
`numb = (22, 10, 5, 34, 29)
list(reversed(numb))` 
```

```
Output:
[29, 34, 5, 10, 22] 
```

您必须指定一个列表或元组，否则它只会给您一个地址，如下例所示:

```
numb = (22, 10, 5, 34, 29)
reversed(numb) 
```

```
Output:
<reversed object at 0x000001C1E7D9E110> 
```

## 结论

内置函数帮助您以清晰简洁的方式编写 Python 函数。他们会帮助你执行你的功能而不会变得混乱。

在本教程中，您已经了解了 Python 中不同的内置函数。您已经看到了各种示例，现在您可以按照自己的顺序进行练习。希望这篇教程对你有用。

在 [Twitter](https://twitter.com/Shweta_go) 上关注我。编码快乐！**