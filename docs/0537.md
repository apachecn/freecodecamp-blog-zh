# Python List . Append()–如何在 Python 中追加到列表中

> 原文：<https://www.freecodecamp.org/news/python-list-append-how-to-append-to-a-list-in-python/>

如何在 Python 中向已经创建的列表追加(或添加)新值？我将在本文中向您展示如何做到这一点。

但是首先要做的是...

## Python 中的 a `List`是什么？

`List`是一种数据类型，允许你在一个变量中存储相同或不同类型的多个值。

看看下面的例子:

```
age = 50
name = "Python"
isRunning = False 
```

在该代码中，`age`、`name`和`isRunning`分别只保存`number`、`string`和`boolean`数据类型中的一个值。

假设您想使用这种方法存储您在市场上购买的所有物品:

```
item1 = "banana"
item2 = "apple"
item3 = "orange" 
```

为相关项目创建三个独立的变量可能不是最佳方法。

使用列表，您可以创建一个保存多个值的变量。方法如下:

```
numbers = [1, 2, 3]

strings = ["list", "dillion", "python"]

mixed = [10, "python", False, [40, "yellow"]] 
```

`numbers`变量是一个包含三个数值的列表。

`strings`变量是一个包含三个字符串值的列表。

`mixed`变量是一个包含数字、字符串、布尔值甚至另一个列表的列表。

所以对于你在市场上买的东西，你可以这样储存它们:

```
items = ["banana", "apple", "orange"] 
```

您可以使用列表中的索引位置来访问每个条目，从 0 开始(因为 Python 中的列表是零索引的):

```
print(items[0], items[1], items[2])
# banana apple orange 
```

## 如何在 Python 中向列表追加数据

我们已经简要地了解了什么是列表。那么，如何用新值更新列表呢？使用`List.append()`方法。

`append`方法接收一个参数，这是您想要添加到列表末尾的值。

下面是使用这种方法的方法:

```
mixed = [10, "python", False]

mixed.append(40)

print(mixed)
# [10, 'python', False, 40] 
```

使用`append`方法，您已经将`40`添加到了`mixed`列表的末尾。

您可以添加任何想要的数据类型，包括其他列表:

```
mixed = [10, "python", False]

mixed.append([True, "hello"])

print(mixed)

# [10, 'python', False, [True, 'hello']] 
```

## 包扎

列表对于创建包含多个值的变量非常有用(尤其是当这些值相关时)

Python 中的列表有许多方法可以用来修改、扩展或缩减列表。在本文中，我们已经看到了将数据添加到列表末尾的`append`方法。