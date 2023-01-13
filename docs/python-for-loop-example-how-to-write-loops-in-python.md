# Python For 循环示例——如何用 Python 编写循环

> 原文：<https://www.freecodecamp.org/news/python-for-loop-example-how-to-write-loops-in-python/>

如果您刚刚开始学习 Python，for 循环是您应该学习如何使用的基础知识之一。

在 Python 编程语言中，for 循环也称为“确定循环”，因为它们执行指令一定次数。

这与 while 循环或无限循环相反，while 循环执行一个动作，直到满足一个条件并被告知停止。

当您希望对给定序列中的每一项执行相同的代码时，For 循环非常有用。使用 for 循环，可以迭代任何可迭代的数据，比如列表、集合、元组、字典、范围甚至字符串。

在本文中，我将向您展示 Python 中 for 循环的工作原理。您还将了解在 Python 中编写循环时可以使用的关键字。

## Python 中 For 循环的基本语法

Python 中 for 循环的基本语法或公式如下所示:

```
for i in data:
    do something 
```

*   `i`代表迭代器。你可以用任何你想要的东西来代替它
*   `data`代表任何可迭代的对象，如列表、元组、字符串和字典
*   下一步你应该做的是键入一个冒号，然后缩进。您可以使用 tab 键或按空格键 4 次来完成此操作。

## Python For 循环示例

正如我上面提到的，可以用 for 循环迭代任何可迭代的数据。

### 如何用 For 循环迭代字符串

您可以迭代字符串，如下所示:

```
name = "freeCodeCamp"

for letter in name:
    print(letter) 
```

这将单独打印字符串中的所有字母:

```
# Output: 
# f
# r
# e
# e
# C
# o
# d
# e
# C
# a
# m
# p 
```

如果你想把字母打印成一行呢？

您可以通过将空白传递给`print()`语句中的`end`参数来实现这一点。这样，您告诉 Python 您希望控制台中有空白而不是新的一行。

```
name = "freeCodeCamp"

for letter in name:
    print(letter, end=" ")

# Output: f r e e C o d e C a m p 
```

### 如何用 For 循环遍历列表

要使用 for 循环遍历列表，请将列表定义为单独的数据，然后编写 for 循环，如下所示:

```
lang_list = ["Python", "JavaScript", "PHP", "Rust", "Solidity", "Assembly"]

for lang in lang_list:
    print(lang)

# Output: 
# Python
# JavaScript
# PHP       
# Rust      
# Solidity  
# Assembly 
```

不要忘记，您可以使用 end 关键字在一行中打印所有项目:

```
lang_list = ["Python", "JavaScript", "PHP", "Rust", "Solidity", "Assembly"]

for lang in lang_list:
    print(lang, end=" ")

# Output: Python JavaScript PHP Rust Solidity Assembly 
```

### 如何用 For 循环迭代元组

tuple 是 Python 中的一种可迭代数据类型，因此您可以编写一个 for 循环来打印其中的项。

```
footballers_tuple = ("Ronaldo", "Mendy", "Lukaku", "Lampard", "Messi", "Pogba")

for footballer in footballers_tuple:
    print(footballer, end=" ")

# Output: Ronaldo Mendy Lukaku Lampard Messi Pogba 
```

让人们知道元组中的名字代表一些活跃的足球运动员，你可以变得更有创意一些:

```
footballers_tuple = ("Ronaldo", "Mendy", "Lukaku", "Lampard", "Messi", "Pogba")

for footballer in footballers_tuple:
    print(footballer, "is an active footballer")

# Output: 
# Ronaldo is an active footballer
# Mendy is an active footballer  
# Lukaku is an active footballer 
# Lampard is an active footballer
# Messi is an active footballer  
# Pogba is an active footballer 
```

### 如何使用 For 循环迭代集合

您可以使用 for 循环打印集合中的单个项目，如下所示:

```
soc_set = {"Twitter", "Facebook", "Instagram", "Quora"}

for platform in soc_set:
    print(platform, end=" ")

# Output: Twitter Facebook Instagram Quora 
```

你也可以用这个变得更有创造力。在下面的例子中，借助 if 语句，我能够打印出即将被 Elon Musk 收购的平台:

```
soc_set = {"Twitter", "Facebook", "Instagram", "Quora"}

for platform in soc_set:
    if(platform == "Twitter"):
        print(platform, "is about to be bought by Elon Musk.")

# Output: Twitter is about to be bought by Elon Musk. 
```

### 如何用 For 循环迭代字典

字典是键值对形式的数据集合。字典可能是使用 for 循环能做的最多的数据类型。

例如，您可以通过遍历字典来获取字典中的键:

```
fcc_dict = {"name": "freeCodeCamp",
           "type": "non-profit", 
           "mode": "remote", 
           "paid": "no"}

for key in fcc_dict:
    print(key, end=" ")

# Output: name type mode paid 
```

您也可以通过 for 循环获得这些值:

```
for values in fcc_dict.values():
    print(values , end=" ")

# Output: freeCodeCamp non-profit remote no 
```

您可以使用 for 循环获取字典中的键和值:

```
fcc_dict = {"name": "freeCodeCamp",
           "type": "non-profit", 
           "mode": "remote", 
           "paid": "no"}

for key, value in fcc_dict.items():
    print(key, value)

# Output: 
# name freeCodeCamp
# type non-profit
# mode remote
# paid no 
```

我不知道任何其他编程语言能够以如此优雅和干净的方式做到这一点！

你甚至可以将`key, value`替换成任何你想要的东西，它仍然会像预期的那样工作:

```
fcc_dict = {"name": "freeCodeCamp",
           "type": "non-profit", 
           "mode": "remote", 
           "paid": "no"}

for a, b in fcc_dict.items():
    print(a, b)

# Output: 
# name freeCodeCamp
# type non-profit
# mode remote
# paid no 
```

当迭代到达某个键时，也可以执行特定的指令。在下面的例子中，当键等于`type`时，我将“freeCodeCamp 是一个非营利组织”打印到控制台:

```
fcc_dict = {"name": "freeCodeCamp",
           "type": "non-profit", 
           "mode": "remote", 
           "paid": "no"}

for a, b in fcc_dict.items():
    # print(a, b)
    if a == "type":
        print("freeCodeCamp is a non-profit organization")

# Output: freeCodeCamp is a non-profit organization 
```

### 如何使用`range()`函数通过 For 循环迭代数字

遍历一个整数会抛出 Python 中常见的`int object not iterable`错误。但是您可以通过使用`range()`函数来指定您想要遍历两个特定数字之间的数字，从而解决这个问题。

range `()`函数接受两个参数，因此您可以遍历这两个参数中的数字。下面的例子:

```
for i in range(1, 10):
    print(i, end="")

# Output: 123456789 
```

您可以将范围提取到一个变量，它仍然可以工作:

```
my_num = range(1, 10)

for i in my_num:
    print(i, end="")

# Output: 123456789 
```

请注意，结果包含第一个数字，但不包含第二个数字。

## 如何在 Python 中使用 Break 关键字

您可以使用`break`关键字在循环结束前停止它。

在下面的例子中，由于我在`lang`等于 Rust 时退出了循环，所以执行没有到达 Solidity 和 Assembly:

```
lang_list = ["Python", "JavaScript", "PHP", "Rust", "Solidity", "Assembly"]

for lang in lang_list:
    if lang == "Rust":
        break
    print(lang, end=" ")
# Output: Python JavaScript PHP 
```

## 如何在 Python 中使用 Continue 关键字

您可以使用`continue`关键字跳过当前的迭代，并继续其余的迭代。

在下面的例子中，使用 continue 关键字，我让循环跳过 PHP 并在其后继续循环:

```
lang_list = ["Python", "JavaScript", "PHP", "Rust", "Solidity", "Assembly"]

for lang in lang_list:
    if lang == "PHP":
        continue
    print(lang, end=" ")

# Output: Python JavaScript Rust Solidity Assembly 
```

## 如何在 Python 中使用 Else 关键字

您可以使用`else`关键字来指定循环完成后应该运行的代码块:

```
for i in range(10):
    print(i)
else:
    print("Do + ne = Done")

# Output: 
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# Do + ne = Done 
```

## 结论

Python 中的 for 循环看起来不像在许多其他编程语言中那样复杂。但是它的实现在运行时仍然很强大。

For 循环是 Python 的一个非常强大的特性，用它你可以做很多事情。

感谢您的阅读。如果你觉得这篇文章有帮助，分享给你的朋友和家人吧！