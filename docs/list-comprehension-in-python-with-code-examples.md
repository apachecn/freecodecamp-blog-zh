# Python 中的列表理解

> 原文：<https://www.freecodecamp.org/news/list-comprehension-in-python-with-code-examples/>

列表是 Python 中一个有用且常用的特性。

list comprehension 为您提供了一种创建列表的方法，同时还能编写更优雅、易读的代码。

在这篇初学者友好的文章中，我将概述 Python 中列表理解的工作原理。我还将展示大量代码示例。

我们开始吧！

## 如何在 Python 中使用`for`循环创建列表

用 Python 创建列表的一种方法是使用`for`循环。

例如，您可以使用`range()`函数创建一个从 0 到 4 的数字列表。

```
#first create an empty list
my_list = []

#iterate over the numbers 0 - 4 using the range() function
#range(5) creates an iterable, starting from 0 up to (but not including) 5
#Use the .append() method to add the numbers 0 - 4 to my_list

for num in range(5):
    my_list.append(num)

#print my_list
print(my_list)

#output
#[0, 1, 2, 3, 4] 
```

如果您已经有了一个数字列表，但是想用它们的方块创建一个新列表，该怎么办？

你可以再次使用一个`for`循环，就像这样:

```
#initial list of numbers
numbers = [1,2,3,4,5,6]

#create a new,empty list to hold their squares
square_numbers = []

#iterate over initial list
#multiply each number by itself
#use .append() method, to add the square to the new list, square_numbers

for num in numbers: 
    square_numbers.append(num * num)

#print new list
print(square_numbers)

#output
#[1, 4, 9, 16, 25, 36] 
```

但是有一个更快更简洁的方法可以达到同样的效果——使用列表理解。

## Python 中的列表理解是什么？语法概述

当您在 Python 中分析和处理列表时，您经常需要同时对列表中的每一项进行操作、修改或计算。

您可能还需要从头开始创建新列表，或者基于现有列表的值创建新列表。

与其他迭代方法(如`for`循环)相比，列表理解是一种快速、简短且优雅的创建列表的方式。

列表理解的一般语法如下所示:

```
new_list = [expression for variable in iterable] 
```

让我们来分解一下:

*   列表理解以方括号开始和结束，`[]`。
*   然后是您想要对当前 iterable 中的每个值执行的`expression` or 操作。这些计算的结果进入新列表。
*   `expression`后面是一个`for`子句。
*   `variable`是当前列表中正在迭代的每一项的临时名称。
*   关键字`in`用于循环遍历 iterable。
*   `iterable`可以是任何 Python 对象，比如 list、tuple、string 等等。
*   从执行的迭代和迭代期间对每个项目进行的计算中，创建了新的值并保存到变量中，在本例中为`new_list`。**旧列表(或其他对象)将保持不变**。
*   可以有一个可选的`if`语句和附加的`for`子句。

## 如何在 Python 中使用列表理解

使用前面的同一个例子，下面是如何使用 list comprehension 在一行中用`range()`函数创建一个从 0 到 4 的新数字列表:

```
new_list = [num for num in range(5)]

print(new_list)

#output
#[0, 1, 2, 3, 4] 
```

这与`for`循环示例的输出相同，但是代码明显更少！

让我们来分解一下:

*   在这种情况下，iterable 是一个从 0 到 4 的数字序列，使用`range(5)`。`range()`构造一个数字列表。
*   您使用`in`关键字来迭代这些数字。
*   `for`子句后面的`num`是一个变量，是 iterable 中每个值的临时名称。因此`num`在第一次迭代中将等于`0`，然后`num`在下一次迭代中将等于`1`，依此类推，直到它达到并等于数字 4，此时迭代将停止。
*   `for`子句前的`num`是序列中每一项的表达式。
*   最后，创建的新列表(或其他可迭代列表)存储在变量`new_list`中。

您甚至可以对 iterable 中包含的项目执行数学运算，结果将被添加到新列表中:

```
new_list = [num * 2 for num in range(5)]

print(new_list)

#output
#[0, 2, 4, 6, 8] 
```

这里，`range(5)`中的每个数字将乘以 2，新值将存储在变量`new_list`中。

如果您有一个预先存在的列表，并希望在其中操作和修改每一项，会怎么样？这类似于前面的例子，我们创建了一个正方形列表。

同样，您只需一行代码，使用列表理解就可以实现这一点:

```
#initial list
numbers = [1,2,3,4,5,6]

#new list
#num * num is the operation that takes place to create the squares

square_numbers = [num * num for num in numbers]

print(square_numbers)

#output
[1, 4, 9, 16, 25, 36] 
```

### 如何在 Python 中使用列表理解条件句

可选地，您可以使用带有列表理解的`if`语句。

一般语法如下所示:

```
new_list = [expression for variable in iterable if condition == True] 
```

当创建一个新的列表时，条件就像一个过滤器，为额外的精确性和定制化增加了额外的检查。

这意味着表达式中的值必须满足特定的标准和特定的条件，才能出现在新的列表中。

```
new_list = [num for num in range(50) if num % 2 == 0]

print(new_list)

#output
#[0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48] 
```

在上面的例子中，只有条件`num % 2 == 0`被检查并且评估为真的值才会进入`new_list`。

模运算符用于从 0 开始到 49 结束的数字序列中的每一个数字。

如果数被 2 除后的余数是 0，那么且仅在那时它才进入列表。

所以在这种情况下，它创建了一个只有偶数的列表。

然后，您可以根据自己的需要将其具体化。

例如，您可以添加多个条件，如下所示:

```
new_list = [num for num in range(50) if  num > 20 and num % 2 == 0]

print(new_list)

#output
#[22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48] 
```

在这个例子中，有两个条件`num > 20`和`num % 2 == 0`。

`and`操作符表示为了将值添加到新列表中，必须满足两个中的*。*

不符合条件的值被排除在外，不会被相加。

### 如何在 Python 中对字符串使用列表理解

您可以使用给定字符串中包含的单个字符创建新列表。

```
fave_language_chars = [letter for letter in "Python"]

print(fave_language_chars)

#output
#['P', 'y', 't', 'h', 'o', 'n'] 
```

创建的新列表由字符串“Python”中包含的所有独立字母组成，该字符串充当 iterable。

就像数字一样，您可以对字符串中包含的字符执行操作，并根据您希望它们在您创建的新列表中的位置来自定义它们。

如果您希望所有字母都是大写的，您应该执行以下操作:

```
fave_language_chars_upper = [letter.upper() for letter in "Python"]

print(fave_language_chars_upper)

#output
#['P', 'Y', 'T', 'H', 'O', 'N'] 
```

这里您使用`.upper()`方法将“Python”中的每个字母转换成大写字母，并将它们添加到`fave_language_chars_upper`变量中。

如果您希望所有的字母都是小写的，情况也是如此——您可以使用`lower()`方法。

## 结论

现在你知道了！现在，您已经了解了 Python 中理解列表的基础知识。

它提供了一种优雅简洁的语法，用于基于现有列表或其他可重复项创建新列表。

如果你有兴趣了解更多关于 Python 的知识，freeCodeCamp 有一个 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

在这个基于项目的课程中，您将从学习编程基础开始，然后学习更复杂的科目，如数据结构。您还将构建五个项目来实践您所学到的内容。

感谢阅读和快乐编码！