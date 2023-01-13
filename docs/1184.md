# 列表索引超出范围–Python 错误消息已解决

> 原文：<https://www.freecodecamp.org/news/list-index-out-of-range-python-error-message-solved/>

在本文中，您将看到导致`list index out of range` Python 错误的一些原因。

除了首先知道为什么会出现这个错误，您还将学习一些避免它的方法。

我们开始吧！

## 如何在 Python 中创建列表

要在 Python 中创建列表对象，您需要:

*   给列表起一个名字，
*   使用赋值运算符`=`，
*   并在方括号内包含 0 个或多个列表项，`[]`。每个列表项都需要用逗号分隔。

例如，要创建姓名列表，您需要执行以下操作:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"] 
```

上面的代码创建了一个名为`names`的列表，它有四个值:`Kelly, Nelly, Jimmy, Lenny`。

### 如何在 Python 中检查列表的长度

要在 Python 中检查列表的长度，使用 Python 的内置`len()`方法。

`len()`将返回一个整数，即存储在列表中的项目数。

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

#create a variable called name_length to store the length of the list
name_length = len(names)

#print value of variable to the console
print(name_length)

#output
#4 
```

列表中存储了四个条目，因此列表的长度为四。

### 如何在 Python 中访问单个列表项

列表中的每个项目都有自己的*索引号*。

Python 和大多数现代编程语言中的索引从 0 开始。

这意味着列表中第一项的索引为 0，第二项的索引为 1，依此类推。

您可以使用索引号来访问单个项目。

要使用索引号访问列表中的项目，首先要写列表的名称。然后，在方括号内，包含与项目的索引号相对应的整数。

以前面的例子为例，这是使用索引号访问列表中每个项目的方式:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

names[0] # Kelly
names[1] # Nelly
names[2] # Jimmy
names[3] # Lenny 
```

在 Python 中，还可以使用负索引来访问列表中的项目。

要访问最后一个项，可以使用索引值-1。要访问倒数第二项，可以使用索引值-2。

下面是如何使用负索引来访问列表中的每个项目:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

names[-4] # Kelly
names[-3]# Nelly
names[-2] # Jimmy
names[-1] # Lenny 
```

## Python 中为什么会出现`Indexerror: list index out of range`错误？

### 使用超出列表范围的索引号

当你试图使用一个超出列表索引范围的不存在的值访问一个项目时，你会得到一个`Indexerror: list index out of range`错误。

当你试图访问一个列表的最后一项时，或者当你使用负索引时访问第一项时，这是很常见的。

让我们回到目前为止使用的列表。

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"] 
```

假设我想访问最后一项“Lenny”，并尝试使用以下代码来实现这一目的:

```
print(names[4])

#output

#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 3, in <module>
#    print(names[4])
#IndexError: list index out of range 
```

一般一个列表的索引范围是`0 to n-1`，其中`n`是列表中值的总数。

以上列表的总值为`4`，索引范围为`0 to 3`。

现在，让我们尝试使用负索引来访问一个条目。

假设我想通过使用负索引来访问列表中的第一项“Kelly”。

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

print(names[-5])

#output

#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 3, in <module>
#    print(names[-5])
#IndexError: list index out of range 
```

使用负索引时，列表的索引范围是`-1 to -n`，其中`-n`是列表中包含的项目总数。

列表中的项目总数为`4`，索引范围为`-1 to -4`。

### 在 Python `for`循环的`range()`函数中使用了错误的值

当遍历一个列表并试图访问一个不存在的条目时，你会得到`Indexerror: list index out of range`错误。

发生这种情况的一个常见例子是在 Python 的`range()`函数中使用了错误的整数。

`range()`函数通常接受一个整数，表示计数将停止的位置。

例如，`range(5)`表示计数将从`0`开始，到`4`结束。

因此，默认情况下，计数从位置`0`开始，每次递增`1`，并且该数字一直到(但不包括)计数将停止的位置。

让我们举下面的例子:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

for name in range(5):
    print(names[name])

#output

#Kelly
#Nelly
#Jimmy
#Lenny
#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 7, in <module>
#   print(names[name])
#IndexError: list index out of range 
```

这里，列表`names`有四个值。

我想遍历列表并打印出每个值。

当我使用`range(5)`时，我告诉 Python 解释器打印位置`0 to 4`的值。

但是，位置 4 中没有项目。

您可以通过首先打印出位置的编号和*然后*该位置的值来看到这一点。

```
#0
#Kelly
#1
#Nelly
#2
#Jimmy
#3
#Lenny
#4
#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 8, in <module>
#    print(names[name])
#IndexError: list index out of range 
```

你看到在位置`0`是“凯利”，在位置`1`是“耐莉”，在位置`2`是“吉米”，在位置`3`是“莱尼”。

当到达位置 4 时，用指示位置`0 to 4`的`range(5)`指定，没有任何东西要打印出来，因此解释器抛出一个错误。

解决这个问题的一种方法是降低`range()`中的整数:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

for name in range(4):
    print(name)
    print(names[name])

#output

#0
#Kelly
#1
#Nelly
#2
#Jimmy
#3
#Lenny 
```

使用`for`循环时解决这个问题的另一种方法是将列表的长度作为参数传递给`range()`函数。您可以通过使用内置的 Python 函数`len()`来实现这一点，如前一节所示:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

for name in range(len(names)):
    print(names[name])

#output

#Kelly
#Nelly
#Jimmy
#Lenny 
```

当将`len()`作为参数传递给`range()`时，确保**不要让**犯以下错误:

```
names = ["Kelly", "Nelly", "Jimmy", "Lenny"]

for name in range(len(names) + 1):
    print(names[name]) 
```

运行代码后，您将再次得到一个`IndexError: list index out of range`错误:

```
#Kelly
#Nelly
#Jimmy
#Lenny
#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 4, in <module>
#    print(names[name])
#IndexError: list index out of range 
```

## 结论

希望这篇文章能让你了解为什么会出现`IndexError: list index out of range`错误，以及避免它的一些方法。

如果你想了解更多关于 Python 的知识，可以查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。你将开始以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！