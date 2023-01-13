# Python 字典——如何在 Python 中对字典执行 CRUD 操作

> 原文：<https://www.freecodecamp.org/news/everything-you-need-to-know-about-python-dictionaries/>

Python 中最重要的复合数据类型之一是字典。这是一个用来成对存储数据的集合。

字典是有序的和可变的，它们不能存储重复的数据。请记住，在 Python 3.6 之前，字典是无序的。

好吧，让我们深入了解一下它们是如何工作的。

## 如何用 Python 创建字典

众所周知，字典由一组`{key:value}`对组成。冒号(`:`)将每个键与其关联的值分开。

我们可以通过用大括号(`{}`)括起一个逗号分隔的键值对列表来定义一个字典。

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}
```

在上面的例子中，`Name`、`Roll`和`Subjects`是字典`my_dict`的关键字。`Ashutosh Krishna`、`23`和`["OS", "CN", "DBMS"]`是它们各自的值。

我们还可以使用内置的`dict()`函数声明一个字典，如下所示:

```
my_dict = dict({
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}) 
```

元组列表很适合这种情况:

```
my_dict = dict([
    ("Name", "Ashutosh Krishna"), 
    ("Roll", 23),
    ("Subjects", ["OS", "CN", "DBMS"])
]) 
```

它们也可以被指定为关键字参数。

```
my_dict = dict(
    Name="Ashutosh Krishna", 
    Roll=23, 
    Subjects=["OS", "CN", "DBMS"]
) 
```

## 如何在 Python 中访问字典值

您不能使用索引来访问字典值。

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}

print(my_dict[1]) 
```

如果你尝试这样做，它会抛出一个 **KeyError** 如下:

```
Traceback (most recent call last):
  File "C:\Users\ashut\Desktop\Test\hello\test.py", line 7, in <module>
    print(my_dict[1])
KeyError: 1
```

请注意，该异常被称为 KeyError。这是否意味着可以使用键来访问字典值？是的，你答对了！

您可以通过在方括号(`[]`)中指定相应的键来从字典中获取值。

```
>>> my_dict['Name']          
'Ashutosh Krishna'
>>> my_dict['Subjects'] 
['OS', 'CN', 'DBMS']
```

如果一个键在字典中不存在，我们会得到一个 KeyError 异常，正如我们上面看到的。

```
>>> my_dict['College']  
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'College'
```

但是我们也可以使用`get()`函数来避免这个错误。

```
>>> my_dict.get('College')
>>> 
```

如果字典中存在该键，它将检索相应的值。但如果不存在，就不会抛出错误。

## 如何用 Python 更新字典

字典是可变的，这意味着它们可以被修改。我们可以添加新的`{key:value}`对或修改现有的对。

使用赋值操作符在字典中添加一个新条目非常容易，就像这样:

```
>>> my_dict['College'] = 'NSEC'
>>> my_dict                    
{'Name': 'Ashutosh Krishna', 'Roll': 23, 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC'}
```

如果字典中已经存在该键，它将更新该键的值。

```
>>> my_dict['Roll'] = 35
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 35, 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC'}
```

### 如何使用 update()方法更新字典

我们还可以使用内置的`update()`方法更新字典，如下所示:

```
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 35, 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC'}
>>> another_dict = {'Branch': 'IT'}
>>> my_dict.update(another_dict)
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 35, 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC', 'Branch': 'IT'}
>>>
```

`update()`方法要么接受字典，要么接受键/值对(通常是元组)的 iterable 对象。

```
>>> my_dict.update(Branch='CSE') 
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 35, 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC', 'Branch': 'CSE'}
```

## 如何在 Python 中从字典中移除元素

有几种方法可以从 Python 字典中移除元素。

### 如何用`pop()`方法从字典中删除元素

我们可以通过使用`pop()`方法删除字典中的特定条目。这个方法用提供的`key`移除一个项目，并返回`value`。

```
>>> roll = my_dict.pop('Roll') 
>>> roll
35
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC', 'Branch': 'CSE'}
```

### 如何用`popitem()`方法从字典中删除一个条目

`popitem()`删除最后一个键值对，并将其作为元组返回:

```
>>> my_dict.popitem()
('Branch', 'CSE')
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Subjects': ['OS', 'CN', 'DBMS'], 'College': 'NSEC'}
```

### 如何用关键字`del`从字典中删除一个条目

您可以使用`del`关键字来删除特定的`{key:value}`对，甚至是整个字典。

```
>>> del my_dict['Subjects'] 
>>> my_dict
{'Name': 'Ashutosh Krishna', 'College': 'NSEC'}
>>> del my_dict
>>> my_dict
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'my_dict' is not defined
```

### 如何用`clear()`方法从字典中删除一个条目

`clear`方法清除字典中所有的`{key:value}`对。

```
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 23, 'Subjects': ['OS', 'CN', 'DBMS']}
>>> my_dict.clear()
>>> my_dict
{}
```

注意，在调用了`clear()`方法之后，打印字典不会抛出错误，因为`clear()`方法不会删除字典。但是`del`关键字也删除了词典。这就是为什么我们在这种情况下得到一个名字错误。

## 字典运算符和内置函数

让我们讨论两个重要的操作符和内置函数，它们可以和字典一起使用。

### `len()`功能

`len()`函数返回字典中键值对的数量:

```
>>> my_dict
{'Name': 'Ashutosh Krishna', 'Roll': 23, 'Subjects': ['OS', 'CN', 'DBMS']}
>>> len(my_dict)
3
```

### `sorted()`功能

函数以特定的顺序(升序或降序)对给定 iterable 的元素进行排序，并以列表形式返回。

```
>>> sorted(my_dict)
['Name', 'Roll', 'Subjects']
```

### `in`操作员

您可以使用`in`操作符来检查字典中是否存在一个键。

```
>>> 'Roll' in my_dict
True
>>> 'College' in my_dict
False
```

## 内置字典方法

Python 字典中有各种内置方法。我们之前已经讨论过其中的几个，比如`clear()`、`pop()`和`popitem()`。让我们也看看其他的方法。

| 方法 | 描述 |
| 清除() | 从字典中删除所有项目。 |
| 复制() | 返回字典的浅层副本。 |
| fromkeys(序列[，v]) | 返回一个新字典，其关键字来自`seq`，值等于`v`(默认为`None`)。 |
| 获取(密钥[，d]) | 返回`key`的值。如果`key`不存在，返回`d`(默认为`None`)。 |
| 项目() | 以(key，value)格式返回字典条目的新对象。 |
| 按键() | 返回字典键的新对象。 |
| pop(键[，d]) | 移除带有`key`的项目，如果没有找到`key`，则返回其值或`d`。如果没有提供`d`并且没有找到`key`，则引发`KeyError`。 |
| popitem() | 移除并返回任意项(**键，值**)。如果字典为空，则引发`KeyError`。 |
| setdefault(key[,d]) | 如果`key`在字典中，则返回相应的值。如果没有，插入值为`d`的`key`并返回`d`(默认为`None`)。 |
| 更新([其他]) | 用来自`other`的键/值对更新字典，覆盖现有的键。 |
| 值() | 返回字典值的新对象 |

## 如何遍历字典

默认情况下，当我们使用 for 循环迭代一个字典时，我们得到键:

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}

for item in my_dict:
    print(item)
```

输出:

```
Name
Roll
Subjects
```

我们还可以通过以下方式迭代字典:

### 如何使用`items()`方法遍历字典

当我们使用`items()`方法迭代一个字典时，它在每次迭代中返回一个键和值的元组。因此，在这种情况下，我们可以直接获得键和值:

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}

for key, value in my_dict.items():
    print(key, value) 
```

输出:

```
Name Ashutosh Krishna
Roll 23
Subjects ['OS', 'CN', 'DBMS']
```

### 如何使用`keys()`遍历字典

在这种情况下，我们在每次迭代中获取密钥。

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}

for key in my_dict.keys():
    print(key) 
```

输出:

```
Name
Roll
Subjects
```

### 如何使用`values()`遍历字典

在这种情况下，我们直接获得字典的值。

```
my_dict = {
    "Name": "Ashutosh Krishna",
    "Roll": 23,
    "Subjects": ["OS", "CN", "DBMS"]
}

for value in my_dict.values():
    print(value) 
```

输出:

```
Ashutosh Krishna
23
['OS', 'CN', 'DBMS']
```

## 如何在 Python 中合并字典

我们经常需要在 Python 中合并字典。我已经就这个话题写了一篇单独的文章，你可以在这里阅读。

## 包扎

在本文中，我们学习了什么是 Python 字典以及如何对它们执行 CRUD 操作。我们还看到了一些与它们相关的方法和函数。

我希望你喜欢它——感谢你的阅读！

[Subscribe to my newsletter](https://newsletter.ashutoshkrris.tk)