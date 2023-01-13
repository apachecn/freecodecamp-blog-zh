# 用 Python-Python 字典方法创建字典

> 原文：<https://www.freecodecamp.org/news/create-a-dictionary-in-python-python-dict-methods/>

在本文中，您将学习 Python 中字典的基础知识。

您将学习如何创建字典，如何访问字典中的元素，以及如何根据需要修改它们。

您还将学习一些字典上最常用的内置方法。

以下是我们将要介绍的内容:

1.  [定义字典](#define-intro)
    1.  [定义一个空字典](#define-empty)
    2.  [用条目定义字典](#define-items)
2.  [键和值的概述](#overview)
    1。[求一个字典中包含的`key-value`对的个数](#length)
    2。[查看所有`key-value`对](#key-value)
    3。[查看全部`keys`](#keys)
    4。[查看全部`values`](#values)
3.  [访问单个项目](#access)
4.  [修改字典](#modify)
    1.  [添加新项目](#add)
    2.  [更新项目](#update)
    3.  [删除项目](#delete)

## 如何用 Python 创建字典

Python 中的字典由键值对组成。

在接下来的两个部分中，您将看到创建字典的两种方法。

第一种方法是使用一组花括号`{}`，第二种方法是使用内置的`dict()`函数。

### 如何在 Python 中创建一个空字典

要创建一个空字典，首先创建一个变量名，它将是字典的名称。

然后，将变量赋给一组空的花括号，`{}`。

```
#create an empty dictionary
my_dictionary = {}

print(my_dictionary)

#to check the data type use the type() function
print(type(my_dictionary))

#output

#{}
#<class 'dict'> 
```

创建空字典的另一种方法是使用`dict()`函数，而不传递任何参数。

它充当构造函数并创建一个空字典:

```
#create an empty dictionary
my_dictionary = dict()

print(my_dictionary)

#to check the data type use the type() function
print(type(my_dictionary))

#output

#{}
#<class 'dict'> 
```

### 如何用 Python 创建条目字典

要创建一个包含条目的字典，需要在花括号中包含*键值*对。

一般语法如下:

```
dictionary_name = {key: value} 
```

让我们来分解一下:

*   `dictionary_name`是变量名。这是字典将拥有的名称。
*   `=`是将`key:value`对分配给`dictionary_name`的分配运算符。
*   你用一组花括号`{}`声明一个字典。
*   在花括号里有一个键值对。键用冒号`:`与它们的相关值分开。

让我们看一个用条目创建字典的例子:

```
#create a dictionary
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

print(my_information)

#check data type
print(type(my_information))

#output

#{'name': 'Dionysia', 'age': 28, 'location': 'Athens'}
#<class 'dict'> 
```

在上面的例子中，花括号中有一个元素序列。

具体来说，有三个键值对:`'name': 'Dionysia'`、`'age': 28`和`'location': 'Athens'`。

这些键是`name`、`age`和`location`。它们的关联值分别是`Dionysia`、`28`和`Athens`。

当一个字典中有多个键值对时，每个键值对用逗号`,`隔开。

让我们看另一个例子。

假设这次您想使用`dict()`函数创建一个包含条目的字典。

您可以通过使用`dict()`并将包含键值对序列的花括号作为参数传递给函数来实现这一点。

```
#create a dictionary with dict()
my_information = dict({'name': 'Dionysia' ,'age': 28,'location': 'Athens'})

print(my_information)

#check data type
print(type(my_information))

#output

#{'name': 'Dionysia', 'age': 28, 'location': 'Athens'}
#<class 'dict'> 
```

值得一提的是`fromkeys()`方法，这是创建字典的另一种方法。

它将一个预定义的项目序列作为参数，并返回一个新的字典，序列中的项目被设置为字典的指定键。

您可以*选择*为所有键设置一个值，但是默认情况下键的值将是`None`。

该方法的一般语法如下:

```
dictionary_name = dict.fromkeys(sequence,value) 
```

让我们看一个使用`fromkeys()`创建字典的例子，它没有为所有的键设置值:

```
#create sequence of strings
cities = ('Paris','Athens', 'Madrid')

#create the dictionary, `my_dictionary`, using the fromkeys() method
my_dictionary = dict.fromkeys(cities)

print(my_dictionary)

#{'Paris': None, 'Athens': None, 'Madrid': None} 
```

现在让我们看另一个例子，它为字典中的所有键设置一个相同的值:

```
#create a sequence of strings
cities = ('Paris','Athens', 'Madrid')

#create a single value
continent = 'Europe'

my_dictionary = dict.fromkeys(cities,continent)

print(my_dictionary)

#output

#{'Paris': 'Europe', 'Athens': 'Europe', 'Madrid': 'Europe'} 
```

## Python 中字典的键和值概述

Python 字典中的键**只能是不可变的类型**。

Python 中不可变的数据类型有`integers`、`strings`、`tuples`、`floating point numbers`、`Booleans`。

字典键**不能是**可变的类型，比如`sets`、`lists`或`dictionaries`。

所以，假设你有下面的字典:

```
my_dictionary = {True: "True",  1: 1,  1.1: 1.1, "one": 1, "languages": ["Python"]}

print(my_dictionary)

#output

#{True: 1, 1.1: 1.1, 'one': 1, 'languages': ['Python']} 
```

字典中的关键字是`Boolean`、`integer`、`floating point number`和`string`数据类型，这些都是可以接受的。

如果你试图创建一个可变类型的键，你会得到一个错误——特别是这个错误会是一个`TypeError`。

```
my_dictionary = {["Python"]: "languages"}

print(my_dictionary)

#output

#line 1, in <module>
#    my_dictionary = {["Python"]: "languages"}
#TypeError: unhashable type: 'list' 
```

在上面的例子中，我试图创建一个`list`类型的键(一种可变的数据类型)。这导致了一个`TypeError: unhashable type: 'list'`错误。

对于 Python 字典中的值，没有任何限制。值可以是任何数据类型——也就是说，它们可以是可变和不可变类型。

关于 Python 字典中键和值之间的区别，另一件要注意的事情是键是唯一的。这意味着一个键在字典中只能出现一次，但是可以有重复的值。

### 如何用 Python 求一个字典中包含的`key-value`对的个数

`len()`函数返回作为参数传递的对象的总长度。

当字典作为参数传递给函数时，它返回字典中包含的键-值对的总数。

这就是使用`len()`计算键值对数量的方法:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

print(len(my_information))

#output

#3 
```

### 如何用 Python 查看一个字典中包含的所有`key-value`对

要查看字典中的每个键值对，可以使用内置的`items()`方法:

```
year_of_creation = {'Python': 1993, 'JavaScript': 1995, 'HTML': 1993}

print(year_of_creation.items())

#output

#dict_items([('Python', 1993), ('JavaScript', 1995), ('HTML', 1993)]) 
```

`items()`方法返回一个元组列表，其中包含字典中的键值对。

### 如何用 Python 查看一个字典中包含的所有`keys`

要查看字典中的所有键，使用内置的`keys()`方法:

```
year_of_creation = {'Python': 1993, 'JavaScript': 1995, 'HTML': 1993}

print(year_of_creation.keys())

#output

#dict_keys(['Python', 'JavaScript', 'HTML']) 
```

`keys()`方法返回一个只包含字典中的键的列表。

### 如何用 Python 查看一个字典中包含的所有`values`

要查看字典中的所有值，使用内置的`values()`方法:

```
year_of_creation = {'Python': 1993, 'JavaScript': 1995, 'HTML': 1993}

print(year_of_creation.values())

#output

#dict_values([1993, 1995, 1993]) 
```

`values()`方法返回一个只包含字典中的值的列表。

## 如何用 Python 访问字典中的单个条目

使用列表时，通过提及列表名称并使用方括号符号来访问列表项。在方括号中，您可以指定项目的索引号(或位置)。

你不能对字典做完全相同的事情。

使用字典时，不能通过引用索引号来访问元素，因为字典包含键值对。

相反，您通过使用字典名和方括号符号来访问该项，但是这次在方括号中您指定了一个键。

每个键对应一个特定的值，所以您只需提到与您想要访问的值相关联的键。

这样做的一般语法如下:

```
dictionary_name[key] 
```

让我们来看看下面的例子，看看如何访问 Python 字典中的项目:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

#access the value associated with the 'age' key
print(my_information['age'])

#output

#28 
```

当你试图访问一个字典中不存在的键时会发生什么呢？

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

#try to access the value associated with the 'job' key
print(my_information['job'])

#output

#line 4, in <module>
#    print(my_information['job'])
#KeyError: 'job' 
```

它产生一个`KeyError`，因为字典中没有这样的键。

避免这种情况发生的一种方法是首先搜索，看看这个键是否在字典中。

您可以通过使用返回布尔值的关键字`in`来做到这一点。如果关键字在字典中，它返回`True`，如果不在，它返回`False`。

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

#search for the 'job' key
print('job' in my_information)

#output

#False 
```

另一种解决方法是使用`get()`方法访问字典中的条目。

您将所寻找的键作为参数传递，`get()`返回与该键对应的值。

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

#try to access the 'job' key using the get() method
print(my_information.get('job'))

#output

#None 
```

正如你注意到的，当你搜索一个不存在的键时，默认情况下`get()`返回`None`而不是`KeyError`。

如果您不想显示默认的`None`值，而是想在键不存在时显示不同的消息，您可以通过提供不同的值来自定义`get()`。

为此，您可以将新值作为第二个可选参数传递给`get()`方法:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

#try to access the 'job' key using the get() method
print(my_information.get('job', 'This value does not exist'))

#output

#This value does not exist 
```

现在，当您正在搜索一个不在字典中的键时，您将会在控制台上看到消息`This value does not exist`。

## 如何在 Python 中修改字典

字典是*可变的*，这意味着它们是可变的。

它们可以在程序的整个生命周期中增长和收缩。

可以添加新项目，可以用新值更新现有项目，也可以删除项目。

### 如何在 Python 中向字典添加新条目

要将键值对添加到字典中，请使用方括号符号。

这样做的一般语法如下:

```
dictionary_name[key] = value 
```

首先，指定字典的名称。然后，在方括号中，创建一个键并给它赋值。

假设你从一本空字典开始:

```
my_dictionary = {}

print(my_dictionary)

#output

#{} 
```

下面是如何向`my_dictionary`添加一个键值对:

```
my_dictionary = {}

#add a key-value pair to the empty dictionary
my_dictionary['name'] = "John Doe"

#print dictionary
print(my_list)

#output

#{'name': 'John Doe'} 
```

下面是如何添加另一个新的键值对:

```
my_dictionary = {}

#add a key-value pair to the empty dictionary
my_dictionary['name'] = "John Doe"

# add another  key-value pair
my_dictionary['age'] = 34

#print dictionary
print(my_dictionary)

#output

#{'name': 'John Doe', 'age': 34} 
```

请记住，如果您尝试添加的键已经存在于该字典中，并且您正在为它分配一个不同的值，则该键将最终被更新。

请记住，键需要是唯一的。

```
my_dictionary = {'name': "John Doe", 'age':34}

print(my_dictionary)

#try to create a an 'age' key and assign it a value
#the 'age' key already exists

my_dictionary['age'] = 46

#the value of 'age' will now be updated

print(my_dictionary)

#output

#{'name': 'John Doe', 'age': 34}
#{'name': 'John Doe', 'age': 46} 
```

如果您想防止意外更改已经存在的键的值，您可能想检查您试图添加的键是否已经在字典中。

您可以通过使用我们上面讨论过的关键字`in`来做到这一点:

```
my_dictionary = {'name': "John Doe", 'age':34}

#I want to add an `age` key. Before I do so, I check to see if it already exists
print('age' in my_dictionary)

#output

#True 
```

### 如何用 Python 更新字典中的条目

更新字典中的条目与向字典中添加条目的方式类似。

当您知道您想要更新一个现有键值时，请使用您在上一节中看到的以下通用语法:

```
dictionary_name[existing_key] = new_value 
```

```
my_dictionary = {'name': "John Doe", 'age':34}

my_dictionary['age'] = 46

print(my_dictionary)

#output

#{'name': 'John Doe', 'age': 46} 
```

要更新字典，也可以使用内置的`update()`方法。

当您想同时更新一个字典中的多个值时，这种方法特别有用。

假设您想要更新`my_dictionary`中的`name`和`age`键，并添加一个新键`occupation`:

```
my_dictionary = {'name': "John Doe", 'age':34}

my_dictionary.update(name= 'Mike Green', age = 46, occupation = "software developer")

print(my_dictionary)

#output

#{'name': 'Mike Green', 'age': 46, 'occupation': 'software developer'} 
```

`update()`方法接受一组键值对。

已经存在的键用分配的新值更新，并添加了一个新的键-值对。

当您想要将一个字典的内容添加到另一个字典中时,`update()`方法也很有用。

假设您有一个字典`numbers`，和第二个字典`more_numbers`。

如果你想合并`more_numbers`的内容和`numbers`的内容，使用`update()`方法。

包含在`more_numbers`中的所有键值对将被添加到`numbers`字典的末尾。

```
numbers = {'one': 1, 'two': 2, 'three': 3}
more_numbers = {'four': 4, 'five': 5, 'six': 6}

#update 'numbers' dictionary
#you update it by adding the contents of another dictionary, 'more_numbers',
#to the end of it
numbers.update(more_numbers)

print(numbers)

#output

#{'one': 1, 'two': 2, 'three': 3, 'four': 4, 'five': 5, 'six': 6} 
```

### 如何用 Python 从字典中删除条目

从字典中删除特定键及其相关值的方法之一是使用`del`关键字。

这样做的语法如下:

```
del dictionary_name[key] 
```

例如，从`my_information`字典中删除`location`键的方法如下:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

del my_information['location']

print(my_information)

#output

#{'name': 'Dionysia', 'age': 28} 
```

如果你想删除一个键，但是也想保存删除值，使用内置的`pop()`方法。

`pop()`方法移除并返回您指定的键。这样，您可以将移除的值存储在变量中，供以后使用或检索。

您将要移除的键作为参数传递给方法。

下面是实现这一点的一般语法:

```
dictionary_name.pop(key) 
```

要从上面的示例中删除`location`键，但这次使用`pop()`方法并将与键相关的值保存到一个变量中，请执行以下操作:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

city = my_information.pop('location')

print(my_information)
print(city)

#output

#{'name': 'Dionysia', 'age': 28}
#Athens 
```

如果您指定一个字典中不存在的键，您将得到一条`KeyError`错误消息:

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

my_information.pop('occupation')

print(my_information)

#output

#line 3, in <module>
#   my_information.pop('occupation')
#KeyError: 'occupation' 
```

解决这个问题的方法是向`pop()`方法传递第二个参数。

通过包含第二个参数，不会有错误。相反，如果键不存在，将会有一个无声的失败，字典将保持不变。

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

my_information.pop('occupation','Not found')

print(my_information)

#output

#{'name': 'Dionysia', 'age': 28, 'location': 'Athens'} 
```

`pop()`方法删除一个特定的键及其相关值——但是如果您只想从字典中删除最后一个键-值对呢？

为此，请使用内置的`popitem()`方法。

这是`popitem()`方法的一般语法:

```
dictionary_name.popitem() 
```

`popitem()`方法没有参数，但是从字典中移除并返回最后一个键值对。

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

popped_item = my_information.popitem()

print(my_information)
print(popped_item)

#output

#{'name': 'Dionysia', 'age': 28}
#('location', 'Athens') 
```

最后，如果想从字典中删除所有的键值对，可以使用内置的`clear()`方法。

```
my_information = {'name': 'Dionysia', 'age': 28, 'location': 'Athens'}

my_information.clear()

print(my_information)

#output

#{} 
```

使用这种方法会给你留下一个空字典。

## 结论

现在你知道了！现在，您已经了解了 Python 中字典的基础知识。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！