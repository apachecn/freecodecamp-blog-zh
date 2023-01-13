# Python 中的字典理解——解释字典理解

> 原文：<https://www.freecodecamp.org/news/dictionary-comprehension-in-python-dict-comprehensions-explained/>

您可以使用 Python 中的[字典](https://www.freecodecamp.org/news/python-add-to-dictionary-adding-an-item-to-a-dict/)以键和值对的形式存储数据。

您可以使用字典理解功能从现有的字典中创建一个新字典。

使用字典理解创建新字典时，可以使用表达式执行各种操作，以确定将存储在新字典中的数据(键和/或值)。

在本文中，您将学习如何使用字典理解在 Python 中创建字典。您还将看到各种表达式，您可以用它们来修改要存储在新字典中的数据。

我们开始吧！

## 如何在 Python 中使用字典理解

在我们看一些例子之前，下面是字典理解的语法:

```
new_dictionary = {key: value for (key,value) in iterable}
```

在上面的语法中，`key`和`value`代表初始字典中的键和值。无论您对它们执行什么操作，都会决定新字典中的键/值——通过一些例子，您很快就会更好地理解这一点。

括号中的`key`和`value`—`(key, value)`—仍然和我们上面提到的一样。在这种情况下，它们用于`for`循环。

因此，无论您在`key`和`value`上执行什么操作，循环都会将该操作应用于初始字典中与该操作相关联的每一项。

语法中的`iterable`表示可迭代对象。在我们的例子中，它表示最初的字典。

让我们看一些例子来帮助你理解上面的解释。

```
items_in_cm = {'pen': 40, 'book': 50, 'keyboard': 60}
```

在上面的代码中，我们创建了一个包含三个条目的字典。关键字是项目的名称，而值是以厘米为单位的项目长度。

使用字典理解，我们将创建一个新的字典，每个条目的长度以米为单位。

它是这样工作的:

```
items_in_cm = {'pen': 40, 'book': 50, 'keyboard': 60}

items_in_meters = {key: value/100 for (key, value) in items_in_cm.items()}

print(items_in_meters)
# {'pen': 0.4, 'book': 0.5, 'keyboard': 0.6}
```

要注意的是上面的第二行:`items_in_meters = {key: value/100 for (key, value) in items_in_cm.items()}`

我们来分解一下。

第一部分有这个:`key: value/100`。这意味着字典中的每个`value`都要除以 100。

但是字典理解还没有理解在哪里应用上面的命令。

这就引出了第二部分:`for (key, value) in items_in_cm.items()`。这部分代码有一个带两个参数的`for`循环— `key`和`value`。

所以这将遍历`items_in_cm`字典中的每个键和值，并将每个值除以 100。

我们能够使用`items()`方法访问字典中的条目，该方法返回字典中的每个键和值对。

打印新字典(`items_in_meters`)，我们有这个:`{'pen': 0.4, 'book': 0.5, 'keyboard': 0.6}`。新词典中的每个值都被除以 100。

您会注意到我们只改变了字典值中的数据。您也可以修改密钥。这里有一个例子:

```
items_in_cm = {'pen': 40, 'book': 50, 'keyboard': 60}

items_in_meters = {key + ' in meters': value/100 for (key, value) in items_in_cm.items()}

print(items_in_meters)
# {'pen in meters': 0.4, 'book in meters': 0.5, 'keyboard in meters': 0.6} 
```

在上面的例子中，我们为字典理解中的每个键添加了一个字符串:`key + ' in meters'`。

## Python 中如何在词典理解中使用条件语句

在这一节和下一节中，您将学习其他表达式，这些表达式可用于修改存储在使用字典理解创建的字典中的条目。

我们将从条件语句开始。

```
developers = {'Jane': 'Python', 'Jade': 'JavaScript', 'John': 'Python', 'Doe': 'JavaScript'}

python_developers = {key: value for (key, value) in developers.items() if value == 'Python'}

print(python_developers)
# {'Jane': 'Python', 'John': 'Python'} 
```

在上面的例子中，我们创建了一个名为`developers`的字典，它存储了一个开发人员列表以及他们最喜欢的语言:`{'Jane': 'Python', 'Jade': 'JavaScript', 'John': 'Python', 'Doe': 'JavaScript'}`。

在字典理解中，我们在末尾加了一个表达:`if value == 'Python'`。这将扫描并返回所有`value`的值为‘Python’的项目。

当打印出来时，我们在控制台中看到了这个:`{'Jane': 'Python', 'John': 'Python'}`。

我们也可以在字典理解中使用`if/else`语句。这里有一个例子:

```
random_items = {'monitor': 100, 'pen': 40, 'keyboard': 60, 'pencil': 30}

items_length_check = {key: ('above 50' if value > 50 else 'below 50') for (key, value) in random_items.items()}

print(items_length_check)
# {'monitor': 'above 50', 'pen': 'below 50', 'keyboard': 'above 50', 'pencil': 'below 50'}
```

使用上面的`if/else`语句，我们修改了新字典中的值。如果初始字典中的值大于 50，则每个项目的`value`将为“大于 50”，如果小于 50，则为“小于 50”。

字典理解中的代码开始显得笨重。我们将在下一节使用一个函数来清理它。

## Python 中如何在词典理解中使用函数

当使用字典理解时，您可以使用函数来替换/提取应该放入字典理解的花括号中的逻辑。这有助于保持代码的整洁和可读性。

```
random_items = {'monitor': 100, 'pen': 40, 'keyboard': 60, 'pencil': 30}

def check_length(value):
    if value >50:
        return 'above 50'
    else:
        return 'below 50'

items_length_check = {key: check_length(value) for (key, value) in random_items.items()}

print(items_length_check)
# {'monitor': 'above 50', 'pen': 'below 50', 'keyboard': 'above 50', 'pencil': 'below 50'} 
```

上面的例子类似于上一节中的例子。

我们做了一个主要的改变——提取字典理解中的逻辑，并把它放在一个函数中。那就是:

```
def check_length(value):
    if value >50:
        return 'above 50'
    else:
        return 'below 50'
```

然后我们把函数名放在字典里理解:`items_length_check = {key: check_length(value) for (key, value) in random_items.items()}`。

当字典理解中的代码运行时，该函数被触发。

## 摘要

在本文中，我们讨论了 Python 中的字典理解。您可以使用它从现有的字典创建新的字典。

我们看到了语法和如何使用字典理解。

我们还看到了一些例子，展示了在进行字典理解时如何使用条件语句和函数。

编码快乐！