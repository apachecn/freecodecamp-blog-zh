# Python 字典方法–Python 中的字典

> 原文：<https://www.freecodecamp.org/news/python-dictionary-methods-dictionaries-in-python/>

在 Python 中，字典是核心数据结构之一。它是由逗号分隔并用大括号括起来的键值对序列。

![keyvalue](img/88bc51113ba268e0736eae7129fce22e.png)

如果你熟悉 JavaScript，Python 字典就像 JavaScript 对象。

Python 提供了 10 多种使用字典的方法。

在本文中，我将向您展示如何用 Python 创建一个字典，并使用这些方法来使用它。

## 我们将涵盖的内容

*   [如何用 Python 创建字典](#how-to-create-a-dictionary-in-Python)
*   [使用 Python 字典的方法](#methods-forworking-with-python-dictionaries)
    *   [如何使用`get()`字典法](#how-to-use-the-get-dictionary-method)
    *   [如何使用`items()`字典法](#how-to-use-the-items-dictionary-method)
    *   [如何使用`keys()`字典法](#how-to-use-the-keys-dictionary-method)
    *   [如何使用`values()`字典法](#how-to-use-the-values-dictionary-method)
    *   [如何使用`pop()`字典法](#how-to-use-the-pop-dictionary-method)
    *   [如何使用`popitem()`字典法](#how-to-use-the-popitem-dictionary-method)
    *   [如何使用`update()`字典法](#how-to-use-the-update-dictionary-method)
    *   [如何使用`copy()`字典法](#how-to-use-the-copy-dictionary-method)
    *   [如何使用`clear()`字典法](#how-to-use-the-clear-dictionary-method)
*   [结论](#conclusion)

## 如何用 Python 创建字典

要创建一个字典，您需要打开一个花括号，并将数据放入一个用逗号分隔的键值对中。

字典的基本语法是这样的:

```
demo_dict = {
"key1": "value1",
"key2": "value2", 
"key3": "value3"
} 
```

请注意，值可以是任何数据类型，并且可以重复，但键不能重复。如果这些键是重复的，您将得到一个无效的语法错误。

## 使用 Python 字典的方法

我将使用下面的字典向您展示字典方法是如何工作的:

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
} 
```

### 如何使用`get()`字典法

get 方法返回指定键的值。

在下面的代码中，我能够通过在`get()`方法中传递`founder`键来获得 freeCodeCamp 的创建者:

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

founder = first_dict.get("founder")
print(founder)

# Output: Quincy Larson 
```

### 如何使用`items()`字典法

`items()`方法在一个列表中返回字典的所有条目。列表中有一个表示每个项目的元组。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

items = first_dict.items()
print(items)

# Output: dict_items([('name', 'freeCodeCamp'), ('founder', 'Quincy Larson'), ('type', 'charity'), ('age', 8), ('price', 'free'), ('work-style', 'remote')]) 
```

### 如何使用`keys()`字典法

`keys()`返回字典中的所有键。它返回元组中的键——另一种 Python 数据结构。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

dict_keys = first_dict.keys()
print(dict_keys)

# Output: dict_keys(['name', 'founder', 'type', 'age', 'price', 'work-style']) 
```

### 如何使用`values()`字典法

values 方法访问字典中的所有值。像`keys()`方法一样，它返回元组中的值。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

dict_values = first_dict.values()
print(dict_values)

# Output: dict_values(['freeCodeCamp', 'Quincy Larson', 'charity', 8, 'free', 'remote']) 
```

### 如何使用`pop()`字典法

`pop()`方法从字典中删除一个键值对。要使它工作，您需要在括号内指定密钥。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

first_dict.pop("work-style")
print(first_dict)

# Output: {'name': 'freeCodeCamp', 'founder': 'Quincy Larson', 'type': 'charity', 'age': 8, 'price': 'free'} 
```

您可以看到`work-style`键及其值已经从字典中删除。

### 如何使用`popitem()`字典法

`popitem()`方法的工作方式类似于`pop()`方法。不同之处在于它删除了字典中的最后一项。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

first_dict.popitem()
print(first_dict)

# Output: {'name': 'freeCodeCamp', 'founder': 'Quincy Larson', 'type': 'charity', 'age': 8, 'price': 'free'} 
```

可以看到最后一个键值对(“work-style”:“remote”)已经从字典中删除了。

### 如何使用`update()`字典法

方法将一个条目添加到字典中。您必须在大括号中指定键和值，并用花括号将其括起来。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

first_dict.update({"Editor": "Abbey Rennemeyer"})
print(first_dict)

# Output: {'name': 'freeCodeCamp', 'founder': 'Quincy Larson', 'type': 'charity', 'age': 8, 'price': 'free', 'work-style': 'remote', 'Editor': 'Abbey Rennemeyer'} 
```

新条目已被添加到词典中。

### 如何使用`copy()`字典法

`copy()`方法顾名思义——它将字典复制到指定的变量中。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

second_dict = first_dict.copy()
print(second_dict)

# Output: {'name': 'freeCodeCamp', 'founder': 'Quincy Larson', 'type': 'charity', 'age': 8, 'price': 'free', 'work-style': 'remote'} 
```

### 如何使用`clear()`字典法

clear 方法删除字典中的所有条目。

```
first_dict = {
    "name": "freeCodeCamp", 
    "founder": "Quincy Larson",
    "type": "charity", 
    "age": 8, 
    "price": "free", 
    "work-style": "remote",
}

first_dict.clear()
print(first_dict)

# Output: {} 
```

## 结论

在本文中，您学习了如何创建 Python 字典，以及如何使用 Python 提供的内置方法来使用它。

如果你觉得这篇文章有帮助，不要犹豫，与朋友和家人分享。

继续编码:)