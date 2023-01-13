# Python 反向列表——如何反转一个范围或数组

> 原文：<https://www.freecodecamp.org/news/python-reverse-list-how-to-reverse-a-range-or-array/>

在本教程中，您将学习在 Python 中反转列表和列表范围的一些不同方法。我们将在此过程中查看一些编码示例。

我们开始吧！

## 如何在 Python 中创建范围

在 Python 中创建一系列数字的有效方法是使用内置的`range()`函数。

要创建一个包含一系列数字的列表，可以使用`list()`构造函数，并在其中使用`range()`函数。

range 函数最多接受三个参数——`start, stop, and step`参数，基本语法如下所示:

`range(start, stop, step)`。

`start`参数是开始计数的数字。

`stop`参数是一直到(但不包括)计数停止的数字。

`step`参数是决定数字递增方式的数字。

在这三个参数中，只有`stop`是必需的，其余都是可选的。

如果您只包含`stop`参数，请记住，默认情况下，计数从 0 开始，然后计数在您指定的数字之前一个数字结束。

例如:

```
#creates a list of numbers that 
#start at 0 and end at 3, not 4.

my_range = list(range(4))

print(my_range)
#output
#[0, 1, 2, 3] 
```

因此，下面是如何将所有这些放在一起，以创建一个数字范围的列表:

```
#creates a list of range of numbers:
#starting from 0
#up to, but not including, 10
# and the counting is incremented by 1

my_range = list(range(0, 10, 1))

#print to the console
print(my_range)

#output
#[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

#check type using the built-in type() method
print(type(my_range))

#output
#<class 'list'> 
```

## 如何在 Python 中反转范围

要使用`range()`函数在 Python 中反转一系列数字，可以使用负步长，比如`-1`。

下面的示例创建了一个数字范围的列表，从 9 开始，但不包括-1(因此计数在 0 处停止)，并且序列的计数每次都减 1:

```
my_range = list(range(9, -1, -1))

print(my_range)
#output
#[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

print(type(my_range))
#<class 'list'> 
```

当*反转*列表时，需要包含`start`和`step`参数。

## 如何在 Python 中反转数组

编程中的数组是项目的有序集合，所有项目都具有相同的数据类型。

集合中的每一项都有自己的索引号。

然而，与其他编程语言不同，数组不是 Python 中内置的数据结构。

相反，您使用列表，Python 提供了一些反转列表的方法。

## 如何使用`.reverse()`方法在 Python 中反转列表

使用这个内置的 Python 方法，列表会在位置处改变*。这意味着列表的原始顺序受到影响。*

项目的初始顺序被更新和改变。

例如，假设您有以下列表:

```
#initial list
my_list = [10,20,30,40,50]

print("My initial list is: ",my_list)

#output
#My initial list is:  [10, 20, 30, 40, 50] 
```

要将`my_list`的项目更改为`50, 40, 30, 20,10`的顺序，您需要:

```
#initial list
my_list = [10,20,30,40,50]

#reverse order of items
my_list.reverse()

print("This is what the list looks like now: ", my_list)

#output
#This is what the list looks like now:  [50, 40, 30, 20, 10] 
```

您会看到列表的初始顺序已经改变，列表中的元素已经颠倒。

## 如何在 Python 中使用切片操作符反转列表

切片操作符的工作方式类似于您之前看到的`range()`函数。

它也接受`start`、`stop`、`step`的说法。

语法是这样的:`start:end:step`。

例如:

```
my_list = [10,20,30,40,50]

my_list2 = my_list[1:3:1]

print(my_list2)

#output
#[20, 30] 
```

在上面的例子中，我们希望从索引 1 开始获取项目，但不包括索引 3 的项目，一次增加一个数字。

Python 中的索引从 0 开始，因此第一个元素的索引为 0，第二个元素的索引为 1，依此类推。

当您想要打印所有项目时，您可以使用以下两种方式之一:

```
my_list = [10,20,30,40,50]

my_list2 = my_list[:]

#or..

my_list2 = my_list[::]

#print to the console
print(my_list2)

#output
#10, 20, 30, 40, 50] 
```

因此，您可以使用一个或两个冒号来输出列表中包含的所有项目。

现在，要使用切片操作符反转列表中的所有项目，您必须包含步骤。

在这种情况下，您使用两个冒号来表示参数`start`和`end`，并使用一个负步长来表示递减:

```
my_list = [10,20,30,40,50]

my_list2 = my_list[::-1]

print(my_list2)

#output
#[50, 40, 30, 20, 10] 
```

在这种情况下，将创建一个新列表，列表的原始顺序不受影响。

## 如何使用`reversed()`函数在 Python 中反转列表

不要把这个和`.reverse()`方法混淆了！内置的`reversed()`函数颠倒了列表的顺序，允许您一次访问一个单独的项目。

```
my_list = [10,20,30,40,50]

for num in reversed(my_list): 
    print(num)

#output
#50
#40
#30
#20
#10 
```

`reversed()`函数接受一个列表作为参数，并返回列表中包含的项目的可迭代的反转版本。

如果您想存储来自`reversed()`函数的返回值以备后用，您必须将它包含在`list()`构造函数中，并将新列表赋给一个变量，如下所示:

```
#initial list
my_list = [10,20,30,40,50]

#use reversed() function to reverse order of my_list
#store new list that gets created to the variable my_new_list
my_new_list = list(reversed(my_list))

print(my_new_list)
#output
#[50, 40, 30, 20, 10] 
```

## 结论

现在，您已经知道了 Python 中反转列表的基本原理！

如果你想了解更多关于 Python 的知识，freeCodeCamp 提供了一个 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)

在这个基于项目的课程中，你将从头开始。您将学习编程基础，并进一步学习更复杂的主题。最后，您将构建 5 个项目来实践您的新技能。

感谢阅读和快乐编码！