# 如何在遍历字典时从字典中删除一个键

> 原文：<https://www.freecodecamp.org/news/how-to-remove-a-key-from-the-dictionary-while-iterating-over-it-definitive-guide/>

Python 字典允许您以`Key`和`Value`格式存储值。

您可以使用键访问这些值。您还可以遍历整个字典来访问每个元素。

有时在遍历字典时，您可能需要从字典中删除一个键。

本教程将教你如何在迭代字典时从字典**中移除一个键。**

要直接移除一个键而不迭代，请阅读 freeCodeCamp 教程[如何从 Python 字典中移除一个键](https://www.freecodecamp.org/news/how-to-remove-a-key-from-a-python-dictionary-delete-key-from-dict/)。

## 如何创建字典

让我们首先使用赋值操作符创建一个包含一些键值对的字典。

要使用不同的方法向字典添加键，请阅读[如何向字典添加键](https://www.stackvidhya.com/python-add-key-to-dictionary/)

创建字典后，可以使用`for`循环遍历字典并打印值，以检查字典是否创建成功。

代码如下:

```
yourdict = {
    "one": 1,
    "two": 2,
    "three": 3,
    "four": 4,
}

for key in yourdict.keys():
    print(key, yourdict[key]) 
```

您可以按如下方式打印字典中的值。

**输出:**

```
 one 1
    two 2
    three 3
    four 4 
```

要在不迭代的情况下检查字典中是否存在键，请阅读[如何在 Python 中检查字典中是否存在键](https://www.stackvidhya.com/check-if-key-exists-in-dictionary-python/)。

## 如何检查 Python 版本

当你试图在迭代时从字典中删除一个键时，Python 2 和 Python 3 的工作方式*不同*。

要检查您使用的 Python 版本，请使用下面的代码片段。

代码如下:

```
import sys
print(sys.version) 
```

**输出:**

```
3.8.2 (default, Sep  4 2020, 00:03:40) [MSC v.1916 32 bit (Intel)] 
```

你会看到你的任何版本。现在您知道您使用的是哪个版本的 Python 了。

您可以遵循下面解释的适当方法。

## 如何根据键值从字典中删除一个键——Python 3

本节将教您如何使用 Python 3 从字典中删除一个键。

您需要使用`list(dict.keys())`和[将`keys`转换为`list`，并使用`for`循环遍历字典](https://www.stackvidhya.com/iterate-through-dictionary-python/)。

在将`keys`转换为`list`的同时，Python 3 为列表中的键创建了一个新的*副本*。在迭代过程中不会引用字典。

如果你不把它转换成一个列表，那么 [keys](https://docs.python.org/3/library/stdtypes.html#dict.keys) 方法只是返回一个新的键视图，引用当前迭代的字典。

现在，在键列表的每次迭代中，您可以检查`key`是否等于您需要*删除的项目*。如果是`True`，可以用`del`语句删除`key`。

代码如下:

下面的代码演示了如何在使用 Python 3 迭代字典时从字典中删除一个键。

```
yourdict = {
    "one": 1,
    "two": 2,
    "three": 3,
    "four": 4,
}

for k in list(yourdict.keys()):
    if k == "four":
        del yourdict[k]

yourdict 
```

正如您在下面的代码中看到的，键 *four* 在遍历字典时被从字典中删除。

**输出:**

```
 {'one': 1, 'two': 2, 'three': 3} 
```

如果您使用`dict.keys()`方法迭代并发出一个`del`语句，您将在 Python 3 中看到下面的错误。

```
RuntimeError: dictionary changed size during iteration 
```

## 如何从基于值的字典中删除键——Python 3

这一节教你如何在遍历字典时根据键值从字典中删除一个键。

首先，您需要使用`list(dict.keys())`方法将字典键转换成一个`list`。

在每次迭代中，您可以检查一个键的*值是否等于期望值。如果是`True`，可以发出`del`语句删除键。*

```
yourdict = {
    "one": 1,
    "two": 2,
    "three": 3,
    "four": 4,
}

for k in list(yourdict.keys()):
    if yourdict [k] == 4:
        del yourdict[k]

print(yourdict) 
```

键*4*根据其值 *4* 被删除。

**输出:**

```
{‘three’: 3, 'two': 2, 'one': 1} 
```

## 如何从基于键的字典中删除键——Python 2

本节将教您如何在使用 Python 2 迭代字典时从字典中删除一个键。

您可以使用`dict.keys()`方法直接[遍历字典](https://www.stackvidhya.com/iterate-through-dictionary-python/)。在 Python 2 中，`dict.keys()`方法创建一个键的副本，并遍历`copy`，而不是直接遍历键。所以在迭代的时候不会有*直接引用*到字典。

现在，在每一次迭代中，您可以检查该项是否等于您想要删除的键。如果相等，就可以发出`del`语句。它将从字典中删除该键。

代码如下:

下面的代码演示了如何在使用 Python 2 迭代字典时从字典中删除一个键。

```
yourdict = {
    "one": 1,
    "two": 2,
    "three": 3,
    "four": 4,
}

for k in yourdict.keys():
    if k == "four":
        del yourdict[k]

yourdict 
```

键 *four* 被删除，只有其他条目在字典中可用。

**输出:**

```
 {'one': 1, 'two': 2, 'three': 3} 
```

这就是如何根据密钥移除密钥。

## 如何删除基于值的字典中的键——Python 2

这一节教你如何在遍历字典时根据键值从字典中删除一个键。

使用`dict.items()`方法迭代字典。它将在每次迭代中返回键值对。

然后你可以检查当前迭代*的`value`是否等于你想要移除的值*。然后发出`del`语句从字典中删除这个键。

```
yourdict = {
    "one": 1,
    "two": 2,
    "three": 3,
    "four": 4,
}

for key, val in yourdict.items():
    if val == 3:
        del yourdict[key]

print(yourdict) 
```

键*三*根据其值 *3* 被删除。所有其他项目都可以在字典中找到。

**输出:**

```
{'four': 4, 'two': 2, 'one': 1} 
```

## 为什么 Python 3 和 Python 2 的工作方式不同？

当使用`dict.keys()`方法时，Python 3 返回键的*视图*。这意味着在对字典进行迭代时，存在对字典的引用。

另一方面，Python 2 返回键的一个*副本*，这意味着在迭代时没有字典引用。这意味着删除将会成功，不会出现问题。

## 结论

在本文中，您了解了如何在不同版本的 Python(Python 2 和 Python 3)中迭代字典时从字典中删除一个键。

您还学习了如何基于键或键值删除键。

如果你喜欢这篇文章，请随意分享。

你可以在这里查看我的其他 Python 教程。