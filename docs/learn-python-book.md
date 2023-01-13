# 学习 Python 编程——你需要知道的一切(免费书籍)

> 原文：<https://www.freecodecamp.org/news/learn-python-book/>

Python 是当今世界上最流行的编程语言之一。它是由吉多·范·罗苏姆在 1991 年创立的。

根据 van Rossum 的说法，Python 是一种:

> 高级编程语言，它的核心设计理念是关于代码可读性和语法，它允许程序员用几行代码表达概念

## 学习 Python 的好处

Python 是最容易学习和使用的语言之一。你可以用它来进行软件开发、web 开发的服务器端、机器学习、数学以及你能想到的任何类型的脚本编写。

Python 的好处是它被许多公司和学术机构广泛使用和接受。这使得它成为一个非常好的选择，尤其是如果你刚刚开始你的编码之旅。

Python 也有一个使用它的大型开发人员社区，如果你遇到困难或有问题，他们愿意帮助你。这个社区已经发布了许多开源库，您可以开始使用。他们也积极地不断改进。

Python 的语法与英语的语法非常相似，这使得你可以非常直观地理解和使用它。

Python 运行在解释器系统上，这意味着您不必等待编译器编译代码然后执行它。相反，您可以快速构建原型。

它还可以在不同的平台上工作，比如 Windows、Linux、Mac、Rapsberry Pi 等等。

所以你看——不管你是刚开始还是想为你的工具箱增加一个有用的工具，它都是一个很棒的编程语言。

以下是我们将在本书中涉及的内容:

## 目录

*   [Python 基础知识](#python-basics)
*   [Python 中的运算符](#operators-in-python)
*   [Python 中的数据类型](#data-types-in-python)
*   [Python 中的元组](#tuples-in-python)
*   [Python 中的字典](#dictionaries-in-python-key-value-data-structures)
*   [在 Python 中设置](#sets-in-python)
*   [Python 中的类型转换](#type-conversions-in-python)
*   [Python 中的控制流](#control-flow-in-python)
*   [Python 中的函数](#functions-in-python)
*   [Python 中的面向对象编程](#object-oriented-programming-in-python)
*   [在 Python 中导入](#importing-in-python)
*   [如何在 Python 中处理异常](#how-to-handle-exceptions-in-python)
*   [Python 中的用户输入](#user-input-in-python)
*   [获取该书的 PDF 版本](#get-the-book-as-a-pdf)

# Python 基础

## Python 中的缩进

与其他语言相比，Python 特别重视缩进。

在其他编程语言中，空格和缩进只是用来让代码更易读、更漂亮。但是在 Python 中，它们表示代码的子块。

下面的代码不起作用，因为第二行不正确:

```
if 100 > 10:
  print("100 is greater than 10") 
```

为使其正常工作，缩进应该如下所示:

```
if 100 > 10:
    print("100 is greater than 10") 
```

这可能看起来很难理解，但是您可以使用代码编辑器非常生动地突出这种语法错误。此外，您编写的 Python 代码越多，就越容易将这种缩进视为第二天性。

默认情况下，Python 使用四个空格进行适当的缩进。

## Python 中的注释

我们使用注释来指定程序中被 Python 解释器忽略和不执行的部分。这意味着写在评论中的任何东西都不会被考虑在内，你可以用任何你想用的语言写，包括你自己的母语。

在 Python 中，您可以在行前用符号 *#* 开始注释:

```
# This is a comment in Python 
```

您还可以使用三重引号在多行中包含更多扩展注释:

```
"""
This is a comment in Python.
Here we are typing in another line and we are still inside the same comment block.

In the previous line we have a space, because it's allowed to have spaces inside comments.

Let's end this comment right here.
""" 
```

## Python 中的 print()方法

我们将使用方法`print()`,因为它有助于我们在控制台中看到结果。

你不需要知道它在幕后是如何工作的，甚至不需要知道现在的方法是什么。就把它想象成我们在控制台中显示代码结果的一种方式。

# Python 中的运算符

## Python 中的算术运算符

即使你可以从口袋里掏出手机做一些计算，你也应该习惯用 Python 实现一些算术运算符。

当我们想将两个数字相加时，我们使用加号，就像数学中一样:

```
print(50 + 4)  # 54 
```

同样，对于减法，我们使用减号:

```
print(50 - 4)  # 46 
```

对于乘法，我们使用星号或星号:

```
print(50 * 4)  # 200 
```

做除法时，我们使用正斜杠符号:

```
print(50 / 4)  # 12.5
print(8 / 4)  # 2.0 
```

这导致浮点数。如果我们想在做除法时只得到整数(也称为简单地做地板除法)，我们应该使用双分数符号:

```
print(50 // 4)  # 12
print(8 / 4)  # 2 
```

我们也可以用百分号`%`找到一个数除以另一个数的余数。

```
print(50 % 4)  # 2 
```

这个操作非常有用，尤其是当我们想检查一个数字是奇数还是偶数的时候。如果一个数除以 2，余数是 1，那么这个数就是奇数。否则就是扯平了。

下面是一个例子:

```
print(50 % 2)  # 0
# Since the remainder is 0, this number is even

print(51 % 2)  # 1
#Since the remainder is 1, this number is odd 
```

当我们想把一个数提高到特定的幂时，我们应该使用双星号:

```
print(2 ** 3)  # 8
# This is a short way of writing 2 * 2 * 2

print(5 ** 4)  # 625
# This is a short way of writing 5 * 5 * 5 * 5 
```

## Python 中的赋值运算符

您可以使用这些运算符为变量赋值。

当我们声明一个变量时，我们使用等号:

```
name = "Fatos"
age = 28 
```

我们也可以在同一行中声明多个变量:

```
name, age, location = "Fatos", 28, "Europe" 
```

我们可以用它来交换变量之间的值。例如，假设我们有两个变量`a`和`b`，我们想要切换它们的值。

一种合乎逻辑的方法是引入第三个变量作为临时变量:

```
a, b = 1, 2

print(a)  # 1
print(b)  # 2

c = a
a = b
b = c

print(a)  # 2
print(b)  # 1 
```

我们可以用下面的方法在一行中完成:

```
a, b = 1, 2

print(a)  # 1
print(b)  # 2

b, a = a, b

print(a)  # 2
print(b)  # 1 
```

我们也可以合并赋值操作符和算术操作符。

让我们先看看如何做加法。

让我们假设我们有以下变量:

```
total_sum = 20

current_sum = 10 
```

现在我们想把`current_sum`的值加到`total_sum`上。为此，我们应该编写以下代码:

```
total_sum = total_sum + current_sum

print(total_sum)  # 30 
```

这可能看起来不准确，因为右手边不等于左手边。然而，在这种情况下，我们只是做一个任务，而不是等式两边的比较。

为了快速做到这一点，我们可以使用以下形式:

```
total_sum += current_sum

print(total_sum)  # 30 
```

这相当于前面的说法。

类似地，我们可以对其他算术运算符进行同样的操作:

减法:

```
result = 3
number = 4

result -= number  # This is equal to result = result - number

print(result)  # -1 
```

乘法:

```
product = 3
number = 4

product *= number  # This is equal to product = product * number

print(product)  # 12 
```

部门:

```
result = 8
number = 4

result /= number  # This is equal to result = result / number

print(result)  # 2.0 
```

模数运算符:

```
result = 8
number = 4

result %= number  # This is equal to result = result % number

print(result)  # 0 
```

超级操作员:

```
result = 2
number = 4

result **= number  # This is equal to result = result ** number

print(result)  # 16 
```

## Python 中的比较运算符

你可能在学校学过如何进行基本的数字比较，比如检查一个特定的数字是否大于另一个数字，或者它们是否相等。

我们可以在 Python 中使用几乎相同的操作符来进行这样的比较。

让我们看看他们的行动。

### 等式运算符

您可以使用`==`运算符检查两个数字是否相等:

```
print(2 == 3)  # False 
```

由于`2`不等于`3`，最后一个表达式的计算结果为假。

还有另一个运算符可以用来检查两个数是否不相等。这是一个运算符，你可能在你的数学课上没有看到过，它是这样写的。这是操作员`!=`。

我们来做一个`2`是否不等于`3`的比较:

```
print(2 != 3)  # True 
```

因为`2`确实不等于`3`，所以该表达式评估为真。

### 不等式算子

现在，我们将了解如何检查一个数字是否大于另一个数字:

```
print(2 > 3)  # False 
```

这是你在数学课上应该已经知道的东西。

当试图检查一个数是否大于或等于另一个数时，我们需要使用这个运算符`>=`:

```
print(2 >= 3)  # False 
```

同样，为了检查一个数是否小于或等于另一个数，我们有:

```
print(2 < 3)  # True
print(2 <= 3)  # True 
```

### 逻辑运算符

高中数学，你可能学过`and`、`or`等逻辑运算符。

简而言之，对于使用`and`时计算为真的表达式，两个语句都应该为真。在 Python 中，我们使用`and`来实现它:

```
print(5 > 0 and 3 < 5)  # True 
```

由于`5`大于`0`，其计算结果为真，而`3`小于`5`，其计算结果也为真，因此该示例的计算结果为真。由此我们得到真和真，它评估为真。

让我们举一个例子，当一个`and`表达式将要计算为`False`时:

```
print(2 > 5 and 0 > -1)  # False 
```

`2`不大于`5`，所以左边的语句将被评估为假。无论右边是什么，我们都将得到等于`False`的整个表达式，因为 False 和任何其他值(如 True)都将导致 False。

当您的语句中至少有一个应该为真时，那么我们应该使用`or`操作符:

```
print(2 > 5 or 0 > -1)  # True 
```

这将计算为真，因为右边的语句计算为真，所以至少其中一个为真。

如果两个语句都为假，那么`or`最终导致`False`:

```
print(2 < 0 or 0 > 1)  # False 
```

这是假的，因为 0 不大于 2，并且 0 也不大于 1。所以整个表情都是假的。

# Python 中的数据类型

## Python 中的变量

你可以把变量看作是你可能编写的任何计算机程序的组成部分。

您可以使用变量来存储值，然后根据需要多次重用它们。当你想改变一个值的时候，你可以只在一个地方改变它，你刚刚改变的新值将会在使用该变量的任何地方得到反映。

Python 中的每个变量都是一个对象。

变量是在用值初始化时创建的。

以下是 Python 变量的一般规则:

*   变量名必须以字母或下划线字符开头。它不能以数字开头。
*   变量名只能包含字母数字字符和下划线(A-z、0-9 和 _)。
*   变量名区分大小写，这意味着`height`、`Height`和`HEIGHT`都是不同的变量。

让我们定义第一个变量:

```
age = 28 
```

在这个例子中，我们初始化一个名为 *age* 的变量，并给它赋值 28。

我们可以定义其他变量，比如:

```
age = 28
salary = 10000 
```

我们几乎可以使用任何我们想要的名字，但是最好使用你和其他同事都能理解的名字。

Python 中还有其他变量类型，比如浮点数、字符串和布尔值。我们甚至可以创建自己的自定义类型。

让我们看一个保存浮点数的变量的例子:

```
height = 3.5 
```

如您所见，这种初始化与我们使用整数时的初始化非常相似。这里我们只改变右边的值。Python 解释器足够聪明，知道我们正在处理另一种类型的变量，即浮点类型的变量。

让我们看一个字符串的例子:

```
reader = "Fatos" 
```

我们将字符串值放在双引号或单引号中，以指定要存储在字符串变量中的值。

当我们要存储布尔值时，需要使用保留字，即*真*和*假*。

```
text_visibile = False 
```

当表达式的结果为布尔值时，我们也可以存储布尔值，例如，当我们将一个数字与另一个数字进行比较时，例如:

```
is_greater = 5 > 6 
```

这个变量将被初始化为值 *False* ，因为 5 小于 6。

## Python 中的数字

Python 中有三种数字类型:整数、浮点数和复数。

### 整数

整数表示整数，既可以是正数，也可以是负数，不包含任何小数部分。

这里有几个整数的例子:`1`、`3000`、`-31234`等等。

当两个整数相加、相减或相乘时，我们最终得到一个整数作为结果。

```
print(3 + 5)  # 8
print(3 - 5)  # -2
print(3 * 5)  # 15 
```

这些都是整数。

这也包括我们将一个数提升到幂的情况:

```
result = 3 ** 4  # This is similar to multiplying 3 * 3 * 3 * 3 in which case, we are multiplying integers together
print(result)  # 81 
```

当我们想除以两个整数时，我们会得到一个浮点数。

### 布尔运算

布尔类型表示真值，真和假。您已经在 *Numbers* 一节中了解了这种类型的解释，因为布尔值确实是整数类型的子类型。

更具体地说，几乎总是假值可以被认为是`0`，而真值可以被认为是`1`。

同样，我们也可以用它们做算术运算:

```
print(True * 5)  # 5
print(False * 500)  # 0, since False is equal to 0 
```

布尔值的这种整数表示的例外是当这些值是诸如“假”和“真”的字符串时。

### 漂浮物

浮点数是包含小数部分的数字，如`-3.14`、`12.031`、`9.3124`等。

我们也可以使用`float()`函数转换成浮点数:

```
ten = float(10)

print(ten)  # 10.0 
```

当两个浮点数或者一个浮点数和一个整数相加、相减或相除时，我们最终得到一个浮点数作为结果:

```
print(3.4 * 2)  # 6.8
print(3.4 + 2)  # 5.4
print(3.4 - 2)  # 1.4
print(2.1 * 3.4)  # 7.14 
```

### 复数

复数既有实数部分又有虚数部分，我们可以这样写:

```
complex_number = 1 + 5j

print(complex_number)  # (1+5j) 
```

## Python 中的字符串

字符串表示用单引号或双引号括起来的字符。他们受到同样的待遇:

```
name = "Fatos"  # Double quotes

name = 'Fatos'  # Single quotes 
```

如果我们想在一个字符串中包含一个引号，我们需要让 Python 知道它不应该关闭这个字符串，而是简单地转义这个引号:

```
greeting = 'Hello. I\'m fine.'  # We escaped the apostrophe in I'm.

double_quote_greeting = "Hello. I'm fine."  # When using double quotes, we do not need to escape the apostrophe
```

当我们想要在字符串中包含一个新行时，我们可以包含特殊字符`\n`:

```
my_string = "I want to continue \n in the next line"

print(my_string) 
# I want to continue 
# in the next line 
```

由于字符串是字符数组，我们可以使用索引来索引特定的字符。索引从 0 开始，一直到字符串长度减 1。

我们排除了等于字符串长度的索引，因为我们从 0 而不是从 1 开始索引。

这里有一个例子:

```
string = "Word" 
```

在本例中，如果我们要选择字符串中的单个字符，它们将展开如下:

```
string = "Word"

print(string[0])  # W
print(string[1])  # o
print(string[2])  # r
print(string[3])  # d 
```

我们也可以使用负索引，这意味着我们从字符串末尾的`-1`开始。我们不能使用`0`从字符串的末尾开始索引，因为`-0 = 0`:

```
print(string[-1])  # d
print(string[-2])  # r
print(string[-3])  # o
print(string[-4])  # W 
```

我们也可以进行切片，只包含字符串的一部分，而不是整个字符串。比如我们想得到从某个特定索引开始一直到某个特定索引的字符，我们应该这样写:`string[start_index:end_index]`，不包括索引`end_index`处的字符:

```
string = "Word"

print(string[0:3])  # Wor 
```

如果我们想从一个特定的索引开始，并继续获取字符串的所有剩余字符，直到结束，我们可以省略指定结束索引，如下所示:

```
string = "Word"

print(string[2:])  # rd 
```

如果我们想从 0 开始，直到一个特定的索引，我们可以简单地指定结束索引:

```
string = "Word"

print(string[:2])  # Wo 
```

这意味着`string`的值等于`string[:2]`(不包括位置 2 的字符)+ `string[2:]`。

注意 Python 中的字符串是不可变的。这意味着一旦创建了字符串对象，就不能对其进行修改，例如试图改变字符串中的字符。

举例来说，假设我们想要更改字符串中的第一个字符，即以如下方式将`W`与`A`进行切换:

```
string = "Word"
string[0] = "A" 
```

现在，如果我们试图打印`string`，我们会得到如下错误:

```
# TypeError: 'str' object does not support item assignment 
```

如果我们需要一个新的字符串，我们应该简单地创建一个新的。

### 字符串运算符

我们可以使用`+`操作符连接字符串:

```
first = "First"
second = "Second"

concantenated_version = first + " " + second

print(concatenated_version)  # First Second 
```

我们可以对字符串和数字使用乘法运算符`*`。

你可以用这个来重复这个字符串很多次。例如，如果我们想将一个字符串重复 5 次，但不想手动书写 5 次，我们可以简单地将它乘以数字 5:

```
string = "Abc"

repeated_version = string * 5

print(repeated_version)  # AbcAbcAbcAbcAbc 
```

### 字符串内置方法

我们可以使用一些字符串的内置方法，使我们更容易操作它们。

#### `len()`方法

`len()`是一种我们可以用来获得字符串长度的方法:

```
sentence = "I am fine."

print(len(sentence))  # 10 
```

#### `replace()`方法

我们可以使用`replace()`用另一个字符或子串替换字符串中的一个字符或子串:

```
string = "Abc"

modified_version = string.replace("A", "Z")

print(modified_version)  # Zbc 
```

#### `strip()`方法

`strip()`删除字符串开头或结尾的空格:

```
string = " Hi there "

print(string.strip())  # Hi there 
```

#### `split()`方法

我们可以使用`split()`将一个字符串转换成一个基于特定模式的子字符串数组，该模式被称为分隔符。

例如，让我们假设我们想将一个句子中的所有单词保存在一个单词数组中。这些单词由空格分隔，因此我们需要在此基础上进行拆分:

```
sentence = "This is a sentence that is being declared here"

print(string.split(" ")) 
# ['This', 'is', 'a', 'sentence', 'that', 'is', 'being', 'declared', 'here'] 
```

#### `join()`方法

`join()`与`split()`相反:从字符串数组中，返回一个字符串。串联过程应该在数组的每个元素之间使用指定的分隔符，最终得到一个串联的字符串:

```
words = ["cat", "dog", "rabbit"]

print(" - ".join(words))  # cat - dog - rabbit 
```

#### `count()`方法

我们可以使用`count()`来计算一个字符或一个子串在一个字符串中出现的次数:

```
string = "Hi there"

print(string.count("h"))  # 1, since, it is case sensitive and 'h' is not equal to 'H'

print(string.count("e"))  # 2

print(string.count("Hi"))  # 1

print(string.count("Hi there"))  # 1 
```

#### `find()`方法

让我们在字符串中找到一个字符或子串，并返回它的索引。如果没有找到，它将简单地返回`-1`:

```
string = "Hi there"

print(string.find("3"))  # -1

print(string.find("e"))  # 5

print(string.find("Hi"))  # 0

print(string.find("Hi there"))  # 0 
```

#### `lower()`

`lower()`将字符串中的所有字符转换成小写:

```
string = "Hi there"

print(string.lower())  # hi there 
```

#### `upper()`

`upper()`将字符串中的所有字符转换成大写:

```
string = "Hi there"

print(string.upper())  # HI THERE 
```

#### `capitalize()`方法

我们可以使用`capitalize()`将字符串的第一个字符转换成大写:

```
string = "hi there"

print(string.capitalize())  # Hi there 
```

#### `title()`方法

`title()`将字符串中每个单词(由空格分隔的序列)的起始字符转换为大写:

```
string = "hi there"

print(string.title())  # Hi There 
```

#### `isupper()`方法

`isupper()`是一种我们可以用来检查字符串中所有字符是否都是大写的方法:

```
string = "are you HERE"
another_string = "YES"

print(string.isupper())  # False
print(another_string.isupper())  # True 
```

#### `islower()`方法

`islower()`同样检查所有字符是否都是小写:

```
string = "are you HERE"
another_string = "no"

print(string.islower())  # False
print(another_string.islower())  # True 
```

#### `isalpha()`方法

`isalpha()`如果字符串中的所有字符都是字母表中的字母，则返回 True:

```
string = "A1"
another_string = "aA"

print(string.isalpha())  # False, since it contains 1
print(another_string.isalpha())  # True since both `a` and `A` are letters of the alphabet 
```

#### `isdecimal()`方法

如果字符串中的所有字符都是数字，则返回 True:

```
string = "A1"
another_string = "3.31"
yet_another_string = "3431"

print(string.isdecimal())  # False, since it contains 'A'
print(another_string.isdecimal())  # False, since it contains '.'
print(yet_another_string.isdecimal())  # True, since it contains only numbers 
```

### 字符串格式

格式化字符串可能非常有用，因为无论您正在处理的项目或脚本的类型如何，您都可能会非常频繁地使用它。

让我们首先说明为什么我们需要格式化和包含字符串插值。

想象一下，我想开发一个软件，在人们进来的那一刻就打招呼，比如:

```
greeting = "Good morning Fatos." 
```

这现在看起来很棒，但我不是唯一使用它的人，对吗？

我只是使用它的人之一。

现在如果有人来签到，我就得用他们自己的名字，比如:

```
greeting = "Good morning Besart." 
```

这只是我第一个注册的真正用户。我不算我自己。

现在让我们假设我很幸运，第二个用户也在星期五早上出现，我们的应用程序应该显示:

```
greeting = "Good morning Betim." 
```

如您所见，从业务角度来看，我们正在取得进展，因为刚刚出现了两个新用户，但这不是一个可扩展的实现。我们写的是一个非常静态的`greeting`表达式。

你们应该已经记得，我们将要提到一些我在开始时介绍过的东西。

是的，我们需要使用变量并在字符串旁边包含一个变量，如下所示:

```
greeting = "Good morning " + first_name 
```

这就灵活多了。

这是一种方法。

另一种方法是使用名为`format()`的方法。

我们可以使用花括号来指定放置动态值的位置，如下所示:

```
greeting = "Good morning {}. Today is {}.".format("Fatos", "Friday") 
```

这将把方法的第一个参数`format()`放在第一个花括号中，在我们的例子中是`Fatos`。然后，在第二次出现的花括号中，它将放入方法的第二个参数`format()`。

如果我们试图打印字符串的值，我们应该得到如下结果:

```
print(greeting)
# Good morning Fatos. Today is Friday. 
```

我们可以用大括号内的索引来指定参数，如下所示:

```
greeting = "Today is {1}. Have a nice day {0}".format("Fatos", "Friday")

print(greeting)
# greeting = "Today is {1}. Have a nice day {0}.".format("Fatos", "Friday") 
```

我们还可以在`format()`方法中指定参数，并使用花括号中的特定单词作为参考:

```
greeting = "Today is {day_of_the_week}. Have a nice day {first_name}.".format(first_name="Fatos",
                                                                              day_of_the_week="Friday")
print(greeting)  # Today is Friday. Have a nice day Fatos. 
```

我们可以在一个示例中结合这两种类型的参数，如下所示:

```
short_bio = 'My name is {name}. My last name is {0}. I love {passion}. I like playing {1}.'.format(
    'Morina',
    'Basketball',
    name='Fatos',
    passion='Programming'
)

print(short_bio)
# My name is Fatos. My last name is Morina. I love Programming. I like playing Basketball. 
```

如您所见，使用命名参数而不是位置参数更不容易出错，因为它们在`format()`方法中的顺序并不重要。

我们还可以使用另一种格式化字符串的方式，即在开始引号或三引号之前以`f`或`F`开始字符串，并在末尾包含我们希望包含的变量的名称:

```
first_name = "Fatos"
day_of_the_week = "Friday"
continent = "Europe"

greeting = f'Good morning {first_name}. Today is {day_of_the_week}'

print(greeting)  # Good morning Fatos. Today is Friday. 
```

这里是另一个例子，我们在`F`后面使用了三重商:

```
continent = "Europe"

i_am_here = F'''I am in {continent}'''

print(i_am_here)  # I am in Europe 
```

## Python 中的列表

如果你看一看书架，你会发现书被叠放在一起。你可以看到有很多收集和以某种方式构建元素的例子。

这在计算机编程中也是相当重要的。我们不能继续声明无数的变量，并轻松地管理它们。

假设我们有一个班级的学生，想要保存他们的名字。我们可以根据他们在教室中的位置开始保存他们的名字:

```
first = "Albert"
second = "Besart"
third = "Fisnik"
fourth = "Festim"
fifth = "Gazmend" 
```

这份名单还会继续下去，这将使我们很难跟踪所有的人。

幸运的是，有一种更简单的方法可以让我们将它们放入 Python 中的一个名为 list 的集合中。

让我们创建一个名为 *students* 的列表，并将前面代码块中声明的所有名字存储在该列表中:

```
students = ["Albert", "Besart", "Fisnik", "Festim", "Gazmend"] 
```

这个更漂亮，对吧？

此外，通过这种方式，我们更容易管理和操作列表中的元素。

你可能会想，“对我来说，先调用*然后获取存储在那里的值更容易。现在不可能从这个名为*学生*的新列表中得到一个值。*

如果我们不能读取和使用那些我们刚刚存储在列表中的元素，那将会使它变得不那么有用。

幸运的是，列表有索引，索引从 0 开始。这意味着，如果我们想得到一个列表中的第一个元素，我们需要使用索引 0，而不是你可能认为的索引 1。

在上面的示例中，列表项具有以下相应的索引:

```
students = ["Albert", "Besart", "Fisnik", "Festim", "Gazmend"]
# Indexes 0, 1, 2, 3, 4 
```

现在，如果我们想得到第一个元素，我们只需写:

```
students[0] 
```

如果我们想得到第二个元素，我们只需写:

```
students[1] 
```

正如你可能看到的，我们只需要写列表的名称，以及我们想要在方括号中得到的元素的相应索引。

当然，这个列表不是静态的。我们可以加入一些元素，比如当一个新学生加入班级的时候。

让我们在列表*学生*中添加一个新元素，其值为*贝斯福特*:

```
students.append("Besfort") 
```

我们还可以改变现有元素的值。为此，我们只需用一个新值重新初始化列表中的特定元素。

例如，让我们更改第一个学生的姓名:

```
students[0] = "Besim" 
```

列表可以包含不同类型的变量，例如，我们可以有一个包含整数、浮点数和字符串的字符串:

```
combined_list = [3.14, "An element", 1, "Another element here"] 
```

### 限幅

与字符串类似，列表也可以被切片，结果返回一个新的列表。这意味着原始列表保持不变。

让我们看看如何使用切片获得列表的前三个元素:

```
my_list = [1, 2, 3, 4, 5]

print(my_list[0:3])  # [1, 2, 3] 
```

如您所见，我们将 0 指定为起始索引，将 3 指定为切片应该停止的索引，不包括结束索引处的元素。

如果我们只想从一个索引开始，并获取列表中所有剩余的元素，这意味着 end_index 应该是最后一个索引，那么我们可以省略，根本不必写入最后一个索引:

```
my_list = [1, 2, 3, 4, 5]

print(my_list[3:])  # [4, 5] 
```

类似地，如果我们想从列表的开头开始切片，直到一个特定的索引，那么我们可以完全省略 0 索引的编写，因为 Python 足够聪明，可以推断出:

```
my_list = [1, 2, 3, 4, 5]

print(my_list[:3])  # [1, 2, 3] 
```

Python 中的字符串是不可变的，而列表是可变的，这意味着我们可以在声明列表后修改它们的内容。

举例来说，假设我们想要更改字符串中的第一个字符，即以如下方式将`S`与`B`进行切换:

```
string = "String"
string[0] = "B" 
```

现在，如果我们试图打印`string`，我们会得到如下错误:

```
# TypeError: 'str' object does not support item assignment 
```

现在，如果我们有一个列表，并希望修改它的第一个元素，那么我们可以成功地这样做:

```
my_list = ["a", "b", "c", "d", "e"]

my_list[0] = 50

print(my_list)  # [50, 'b', 'c', 'd', 'e'] 
```

我们可以通过使用`+`操作符将一个列表与另一个列表连接起来来扩展它:

```
first_list = [1, 2, 3]

second_list = [4, 5]

first_list = first_list + second_list 

print(first_list)  # [1, 2, 3, 4, 5] 
```

### 如何将一个列表嵌套在另一个列表中

我们可以将一个列表嵌套在另一个列表中，如下所示:

```
math_points = [30, "Math"]

physics_points = [53, "Phyiscs"]

subjects = [math_points, physics_points]

print(subjects)  # [[30, 'Math'], [53, 'Phyiscs']] 
```

这些列表甚至不需要具有相同的长度。

为了访问列表中的元素，我们需要使用双重索引。

让我们看看如何访问列表`subjects`中的元素`math_points`。由于`math_points`是位于索引 0 的`subjects`列表中的一个元素，我们只需要做以下事情:

```
print(subjects[0])  # [30, 'Math'] 
```

现在让我们假设我们想要访问`subjects`列表中的`Math`。由于`Math`位于索引`1`，我们将需要使用以下双索引:

```
print(subjects[0][1])  # 'Math' 
```

### 列出方法

`len()`是一种可以用来查找列表长度的方法:

```
my_list = ["a", "b", 1, 3]

print(len(my_list))  # 4 
```

#### 如何向列表中添加元素

我们也可以通过添加新元素来扩展列表，或者我们也可以删除元素。

我们可以使用`append()`方法在列表末尾添加新元素:

```
my_list = ["a", "b", "c"]

my_list.append("New element")

my_list.append("Yet another new element")

print(my_list)  
# ['a', 'b', 'c', 'New element', 'Yet another new element'] 
```

如果我们想在列表的特定索引处添加元素，我们可以使用`insert()`方法。我们在第一个参数中指定索引，并在第二个参数中指定要添加到列表中的元素:

```
my_list = ["a", "b"]

my_list.insert(1, "z")

print(my_list)  # ['a', 'z', 'b'] 
```

#### 如何从列表中删除元素

我们可以使用`pop()`方法从列表中删除元素，该方法删除列表中的最后一个元素:

```
my_list = [1, 2, 3, 4, 5]

my_list.pop()  # removes 5 from the list

print(my_list)  # [1, 2, 3, 4]

my_list.pop()  # removes 4 from the list

print(my_list)  # [1, 2, 3] 
```

我们还可以指定列表中某个元素的索引，以指示我们应该删除列表中的哪个元素:

```
my_list = [1, 2, 3, 4, 5]

my_list.pop(0)  # Delete the element at index 0

print(my_list)  # [2, 3, 4, 5] 
```

我们还可以使用`del`语句从列表中删除元素，然后指定我们想要删除的元素的值:

```
my_list = [1, 2, 3, 4, 1]

del my_list[0]  # Delete element my_list[0]

print(my_list)  # [2, 3, 4, 5] 
```

我们也可以使用`del`删除列表的片段:

```
my_list = [1, 2, 3, 4, 1]

del my_list[0:3]  # Delete elements: my_list[0], my_list[1], my_list[2]

print(my_list)  # [4, 1] 
```

我们可以使用`remove()`来做到这一点:

```
my_list = [1, 2, 3, 4]

my_list.remove(3) 

print(my_list)  # [1, 2, 4] 
```

让我们反转列表中的元素。这非常简单明了:

```
my_list = [1, 2, 3, 4]

my_list.reverse()

print(my_list)  # [4, 3, 2, 1] 
```

#### 索引搜索

使用索引获取列表元素很简单。查找列表元素的索引也很容易。我们只需要使用方法`index()`并提及我们想要在列表中查找的元素:

```
my_list = ["Fatos", "Morina", "Python", "Software"]

print(my_list.index("Python"))  # 2 
```

### 成员资格

这很直观，而且与现实生活相关:我们可以问自己某样东西是不是某样东西的一部分。

我的手机在口袋里还是包里？

我同事的电子邮件是否包含在抄送中？

我的朋友在这家咖啡店吗？

在 Python 中，如果我们想检查一个值是否是某个东西的一部分，那我们可以使用运算符`in`:

```
my_list = [1, 2, 3]  # This is a list

print(1 in my_list)  # True 
```

因为数组`[1, 2, 3]`中包含 1，所以表达式的计算结果为 True。

我们不仅可以将它用于数字数组，也可以用于字符数组:

```
vowels = ['a', 'i', 'o', 'u']
print('y' in vowels)  # False 
```

由于`y`不是元音字母，也没有包含在声明的数组中，所以前面代码片段第二行中的表达式将导致 False。

类似地，我们也可以使用`not in`来检查是否有东西没有被包括在内:

```
odd_numbers = [1, 3, 5, 7]
print(2 not in odd_numbers)  # True 
```

因为数组中不包含 2，所以表达式的计算结果为 True。

### 如何对列表中的元素排序

对列表中的元素进行排序可能是你经常需要做的事情。`sort()`是一个内置的方法，可以让你按照字母或数字的升序对列表中的元素进行排序:

```
my_list = [3, 1, 2, 4, 5, 0]

my_list.sort()

print(my_list)  # [0, 1, 2, 3, 4, 5]

alphabetical_list = ['a', 'c', 'b', 'z', 'e', 'd']

alphabetical_list.sort()

print(alphabetical_list)  # ['a', 'b', 'c', 'd', 'e', 'z'] 
```

还有其他方法的列表，我们没有包括在这里。

### 列表理解

列表理解代表了一种简洁的方法，在这种方法中，我们使用一个`for`循环从一个现有列表创建一个新列表。结果最后总是一个新的列表。

让我们从一个例子开始，在这个例子中，我们想用 10 乘以一个列表中的每个数字，并将结果保存在一个新的列表中。首先，让我们在不使用列表理解的情况下这样做:

```
numbers = [2, 4, 6, 8]  # Complete list

numbers_tenfold = []  # Empty list

for number in numbers:
    number = number * 10  # Multiply each number with 10
    numbers_tenfold.append(number)  # Add that new number in the new list

print(numbers_tenfold)  # [20, 40, 60, 80] 
```

我们可以通过以下方式使用列表理解来实现:

```
numbers = [2, 4, 6, 8]  # Complete list

numbers_tenfold = [number * 10 for number in numbers]  # List comprehension

print(numbers_tenfold)  # [20, 40, 60, 80] 
```

在做这些列表理解时，我们也可以包括条件。

假设我们想要保存一个正数列表。

在我们编写使用 list comprehension 实现这一点的方法之前，让我们编写一种方法，在这种方法中，我们将创建一个只包含另一个列表中大于 0 的数字的列表，并将这些正数增加 100:

```
positive_numbers = []  # Empty list

numbers = [-1, 0, 1, -2, -3, -4, 3, 2]  # Complete list

for number in numbers:
    if number > 0:  # If the current number is greater than 0
        positive_numbers.append(number + 100)  # add that number inside the list positive_numbers

print(positive_numbers)  # [101, 103, 102] 
```

我们可以用列表理解来做同样的事情:

```
numbers = [-1, 0, 1, -2, -3, -4, 3, 2]  # Compelete list

positive_numbers = [number + 100 for number in numbers if number > 0]  # List comprehension

print(positive_numbers)  # [101, 103, 102] 
```

正如你所看到的，这是非常短的，应该花更少的时间来写。

我们也可以对多个列表使用列表理解。

让我们举一个例子，我们想要将一个列表中的每个元素与另一个列表中的每个元素相加:

```
first_list = [1, 2, 3]
second_list = [50]

double_lists = [first_element +
                second_element for first_element in first_list for second_element in second_list]

print(double_lists)  # [51, 52, 53] 
```

最后，我们将得到一个结果列表，它与长度最长的列表具有相同数量的元素。

## Python 中的元组

元组是有序且不可变的集合，这意味着它们的内容不能改变。它们是有序的，我们可以使用索引来访问它们的元素。

让我们从第一个元组开始:

```
vehicles = ("Computer", "Smartphone", "Smart watch", "Tablet")

print(vehicles)

# ('Computer', 'Smartphone', 'Smart watch', 'Tablet') 
```

我们在列表部分看到的所有索引和切片操作也适用于元组:

```
print(len(vehicles))  # 4

print(vehicles[3])  # Tablet

print(vehicles[:3])  # ('Computer', 'Smartphone', 'Smart watch') 
```

您可以使用`index()`方法找到元组中元素的索引:

```
print(vehicles.index('tablet'))  # 3 
```

我们还可以使用`+`操作符连接或合并两个元组:

```
natural_sciences = ('Chemistry', 'Astronomy',
                    'Earth science', 'Physics', 'Biology')

social_sciences = ('Anthropology', 'Archaeology', 'Economics', 'Geography',
                   'History', 'Law', 'Linguistics', 'Politics', 'Psychology', 'Sociology')

sciences = natural_sciences + social_sciences

print(sciences)
# ('Chemistry', 'Astronomy', 'Earth science', 'Physics', 'Biology', 'Anthropology', 'Archaeology', 'Economics', 'Geography', 'History', 'Law', 'Linguistics', 'Politics', 'Psychology', 'Sociology') 
```

### 成员资格检查

我们可以使用操作符`in`和`not in`检查一个元素是否是元组的一部分，就像列表一样:

```
vehicles = ('Car', 'Bike', 'Airplane')

print('Motorcycle' in vehicles)  # False, since Motorcycle is not included in vehicles

print('Train' not in vehicles)  # True, since Train is not included in vehicles 
```

### 如何嵌套二元组

除了合并，我们还可以通过使用我们想要嵌套在括号内的元组来将元组嵌套到单个元组中:

```
natural_sciences = ('Chemistry', 'Astronomy',
                    'Earth science', 'Physics', 'Biology')

social_sciences = ('Anthropology', 'Archaeology', 'Economics', 'Geography',
                   'History', 'Law', 'Linguistics', 'Politics', 'Psychology', 'Sociology')

sciences = (natural_sciences, social_sciences)

print(sciences)
# (('Chemistry', 'Astronomy', 'Earth science', 'Physics', 'Biology'), ('Anthropology', 'Archaeology', 'Economics', 'Geography', 'History', 'Law', 'Linguistics', 'Politics', 'Psychology', 'Sociology')) 
```

### 不变

由于元组是不可变的，所以我们在创建之后就不能改变它们。这意味着我们不能在其中添加或删除元素，或者将一个元组追加到另一个元组。

我们甚至不能修改元组中的现有元素。如果我们试图修改元组中的元素，我们将面临如下问题:

```
vehicles = ('Car', 'Bike', 'Airplane')

vehicles[0] = 'Truck'

print(vehicles)
# TypeError: 'tuple' object does not support item assignment 
```

## Python 中的字典——键值数据结构

正如我们之前看到的，列表中的元素与我们可以用来引用这些元素的索引相关联。

Python 中还有另一种数据结构，它允许我们指定自己的自定义索引，而不仅仅是数字。这些被称为字典，它们类似于我们用来查找我们不理解的单词的意思的字典。

让我们假设你正在努力学习德语，有一个你之前没有机会学习的新词，你刚刚在市场上看到: *Wasser* 。

现在，你可以拿起手机，使用谷歌翻译或任何其他你选择的应用程序来检查其相应的英语意思。但是如果你要使用一本实体词典，你就需要找到这个单词，进入那个特定的页面，并检查它旁边的意思。这个词的含义的参考或关键是术语 *Wasser* 。

现在，如果我们想在 Python 中实现这一点，我们不应该使用只有数字索引的列表。我们应该用字典来代替。

对于字典，我们使用花括号，每个元素有两部分:键和值。

在我们之前的示例中，关键字是德语单词，而值是其英语翻译，如下例所示:

```
german_to_english_dictionary = {
    "Wasser": "Water",
    "Brot": "Bread",
    "Milch": "Milk"
} 
```

现在，当我们想要访问字典中的特定元素时，我们只需使用键。例如，让我们假设我们想要获得英文单词 *Brot* 的意思。要做到这一点，我们只需使用该键引用该元素:

```
brot_translation = german_to_english_dictionary["Brot"]
print(brot_translation)  # Bread 
```

当我们打印得到的值时，我们将得到英文翻译。

类似地，我们可以通过获取以 *Milch* 为关键字的元素的值来获得单词 *Milch* 的英文翻译:

```
milch_translation = german_to_english_dictionary["Milch"]
print(milch_translation)  # Milk 
```

我们还可以使用`get()`并指定我们想要获取的项目的键来获取字典中元素的值:

```
german_to_english_dictionary.get("Wasser") 
```

键和值可以是任何数据类型。

字典可以有重复的值，但是所有的键应该是唯一的。看一下这个例子就明白我的意思了:

```
my_dictionary = dict([
  ('a', 1),
  ('b', 1),
  ('c', 2)
])
```

我们可以使用`dict()`创建字典:

```
words = dict([
    ('abandon', 'to give up to someone or something on the ground'),
    ('abase', 'to lower in rank, office, or esteem'),
    ('abash', 'to destroy the self-possession or self-confidence of')
])

print(words)
# {'abandon': 'to give up to someone or something on the ground', 'abase': 'to lower in rank, office, or esteem', 'abash': 'to destroy the self-possession or self-confidence of'} 
```

### 如何给字典添加新的值

我们可以通过指定一个新的键和一个相应的值在字典中添加新的值。然后 Python 将在字典中创建一个新元素:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

words['g'] = 'gama'

print(words)
# {'a': 'alfa', 'b': 'beta', 'd': 'delta', 'g': 'gama'} 
```

如果我们指定一个已经是字典一部分的元素的键，那么这个元素将被修改:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

words['b'] = 'bravo'

print(words)
# {'a': 'alfa', 'b': 'bravo', 'd': 'delta'} 
```

### 如何从字典中删除元素

如果我们想要从字典中删除元素，我们可以使用方法`pop()`并指定我们想要删除的元素的键:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

words.pop('a')

print(words)  # {'b': 'beta', 'd': 'delta'} 
```

从 Python 3.7 开始，我们还可以使用`popitem()`删除值，它删除最后插入的键值对。在早期版本中，它会删除一个随机对:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

words['g'] = 'gamma'

words.popitem()

print(words)  
# {'a': 'alfa', 'b': 'beta', 'd': 'delta'} 
```

还有另一种方法我们可以删除元素，即通过使用`del`语句:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

del words['b']

print(words)  # {'a': 'alfa', 'd': 'delta'} 
```

### 如何得到字典的长度

我们可以使用`len()`获得字典的长度，就像使用列表和元组一样:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

print(len(words))  # 3 
```

### 成员资格

如果我们想检查一个键是否已经是字典的一部分，以避免覆盖它，我们可以使用操作符`in`和`not in`，就像列表和元组一样:

```
words = {
    'a': 'alfa',
    'b': 'beta',
    'd': 'delta',
}

print('a' in words)  # True
print('z' not in words)  # True 
```

### 理解

我们可以像使用列表一样使用理解来快速创建字典。

为了帮助我们做到这一点，我们将需要使用一个名为`items()`的方法，将一个字典转换成一个元组列表。索引 0 中的元素是一个键，而在索引 1 的位置，我们有一个值。

让我们先来看看方法`items()`的实际应用:

```
points = {
    'Festim': 50,
    'Zgjim': 89,
    'Durim': 73
}

elements = points.items()

print(elements) # dict_items([('Festim', 50), ('Zgjim', 89), ('Durim', 73)]) 
```

现在让我们使用理解从这个现有的字典`points`创建一个新的字典。

我们可以假设一个教授心情很好，慷慨到给每个学生奖励`10`分的奖金。我们希望通过将这些新要点保存在新词典中，为每个学生添加这些新要点:

```
points = {
    'Festim': 50,
    'Zgjim': 89,
    'Durim': 73
}

elements = points.items()

points_modified = {key: value + 10 for (key, value) in elements}

print(points_modified)  # {'Festim': 60, 'Zgjim': 99, 'Durim': 83} 
```

## Python 中的集合

集合是无序和无索引的数据集合。由于集合中的元素是无序的，我们不能使用索引或方法`get()`来访问元素。

我们可以添加元组，但不能在集合中添加字典或列表。

我们不能在集合中添加重复的元素。这意味着当我们想要从另一种类型的集合中删除重复的元素时，我们可以在集合中利用这种唯一性。

让我们开始使用花括号创建第一个集合，如下所示:

```
first_set = {1, 2, 3} 
```

我们也可以使用`set()`构造函数创建集合:

```
empty_set = set()  # Empty set

first_set = set((1, 2, 3))  # We are converting a tuple into a set 
```

像所有的数据结构一样，我们可以使用方法`len()`找到一个集合的长度:

```
print(len(first_set))  # 3 
```

### 如何向集合中添加元素

我们可以使用方法`add()`在集合中添加一个元素:

```
my_set = {1, 2, 3}

my_set.add(4)

print(my_set)  # {1, 2, 3, 4} 
```

如果我们想要添加多个元素，那么我们需要使用方法`update()`。我们使用列表、元组、字符串或其他集合作为该方法的输入:

```
my_set = {1, 2, 3}

my_set.update([4, 5, 6])

print(my_set)  # {1, 2, 3, 4, 5, 6}

my_set.update("ABC")

print(my_set)  # {1, 2, 3, 4, 5, 6, 'A', 'C', 'B'} 
```

### 如何从集合中删除元素

如果我们想从集合中删除元素，我们可以使用方法`discard()`或`remove()`:

```
my_set = {1, 2, 3}

my_set.remove(2)

print(my_set)  # {1, 3} 
```

如果我们试图使用`remove()`删除一个不属于集合的元素，那么我们将得到一个错误:

```
my_set = {1, 2, 3}

my_set.remove(4)

print(my_set)  # KeyError: 4 
```

为了避免从集合中删除元素时出现这样的错误，我们可以使用方法`discard()`:

```
my_set = {1, 2, 3}

my_set.discard(4)

print(my_set)  # {1, 2, 3} 
```

### 集合论运算

如果你还记得高中的数学课，你应该已经知道并、交以及两组元素之间的区别。Python 中的集合也支持这些操作。

#### 联盟

Union 表示两个集合中所有唯一元素的集合。我们可以使用管道运算符`|`或`union()`方法找到两个集合的并集:

```
first_set = {1, 2}
second_set = {2, 3, 4}

union_set = first_set.union(second_set)

print(union_set)  # {1, 2, 3, 4} 
```

#### 交集

交集表示包含两个集合中的元素的集合。我们可以使用运算符`&`或`intersection()`方法找到它:

```
first_set = {1, 2}
second_set = {2, 3, 4}

intersection_set = first_set.intersection(second_set)

print(union_set)  # {2} 
```

#### 差异

两个集合之间的差异表示集合只包含第一个集合中的元素，而不包含第二个集合中的元素。我们可以使用`-`运算符或`difference()`方法找到两个集合之间的差异

```
first_set = {1, 2}
second_set = {2, 3, 4}

difference_set = first_set.difference(second_set)

print(difference_set)  # {1} 
```

你可能还记得高中的时候，当我们发现两个集合的区别时，集合的排序是很重要的，但并集和交集不是这样。

这类似于算术，其中`3 - 4`不等于`4 - 3`:

```
first_set = {1, 2}
second_set = {2, 3, 4}

first_difference_set = first_set.difference(second_set)

print(first_difference_set)  # {1}

second_difference_set = second_set.difference(first_set)

print(second_difference_set)  # {3, 4} 
```

# Python 中的类型转换

## 原始类型之间的转换

Python 是一种面向对象的编程语言。这就是为什么它使用类的构造函数来完成从一种类型到另一种类型的转换。

### `int()`方法

`int()`是一种用于转换整数文字、浮点文字(将其舍入到之前的整数，即 3.1 到 3)或字符串文字(条件是字符串表示 int 或 float 文字)的方法:

```
three = int(3)  # converting an integer literal into an integer
print(three)  # 3

four = int(4.8)  # converting a float number into its previous closest integer
print(four)  # 4

five = int('5')  # converting a string into an integer
print(five)  # 5 
```

### `float()`方法

类似地，`float()`用于从整数、浮点数或字符串文字创建浮点数(条件是字符串表示一个`int`或`float` 文字):

```
int_literal = float(5)
print(int_literal)  # 5.0

float_literal = float(1.618)
print(float_literal)  # 1.618

string_int = float("40")
print(string_int)  # 40.0

string_float = float("37.2")
print(string_float)  # 37.2 
```

### `str()`方法

我们可以使用`str()`从字符串、整数文字、浮点文字和许多其他数据类型创建字符串:

```
int_to_string = str(3)
print(int_to_string)  # '3'

float_to_string = str(3.14)
print(float_to_string)  # '3.14'

string_to_string = str('hello')
print(string_to_string)  # 'hello' 
```

## 其他转换

为了从一种类型的数据结构转换到另一种类型，我们执行以下操作:

```
destination_type(input_type) 
```

让我们从具体类型开始，这样就清楚多了。

### 转换为列表

我们可以使用`list()`构造函数将集合、元组或字典转换成列表。

```
books_tuple = ('Book 1', 'Book 2', 'Book 3')
tuple_to_list = list(books_tuple)  # Converting tuple to list
print(tuple_to_list)  # ['Book 1', 'Book 2', 'Book 3']

books_set = {'Book 1', 'Book 2', 'Book 3'}
set_to_list = list(books_set)  # Converting set to list
print(set_to_list)  # ['Book 1', 'Book 2', 'Book 3'] 
```

当把一个字典转换成一个列表时，只有它的键会使它成为一个列表:

```
books_dict = {'1': 'Book 1', '2': 'Book 2', '3': 'Book 3'}
dict_to_list = list(books_dict)  # Converting dict to list
print(dict_to_list)  # ['1', '2', '3'] 
```

如果我们希望保留字典的键和值，我们需要使用方法`items()`首先将它转换成一个元组列表，其中每个元组是一个键和值:

```
books_dict = {'1': 'Book 1', '2': 'Book 2', '3': 'Book 3'}

dict_to_list = list(books_dict.items())  # Converting dict to list

print(dict_to_list)
# [('1', 'Book 1'), ('2', 'Book 2'), ('3', 'Book 3')] 
```

### 向元组的转换

所有数据结构都可以使用`tuple()`构造函数方法转换成一个元组，包括字典，在这种情况下我们得到一个包含字典键的元组:

```
books_list = ['Book 1', 'Book 2', 'Book 3']
list_to_tuple = tuple(books_list)  # Converting tuple to tuple
print(list_to_tuple)  # ('Book 1', 'Book 2', 'Book 3')

books_set = {'Book 1', 'Book 2', 'Book 3'}
set_to_tuple = tuple(books_set)  # Converting set to tuple
print(set_to_tuple)  # ('Book 1', 'Book 2', 'Book 3')

books_dict = {'1': 'Book 1', '2': 'Book 2', '3': 'Book 3'}
dict_to_tuple = tuple(books_dict)  # Converting dict to tuple
print(dict_to_tuple)  # ('1', '2', '3') 
```

### 转换为集合

类似地，所有数据结构都可以使用`set()`构造函数方法转换成一个集合，包括一个字典。在这种情况下，我们得到一个包含字典关键字的集合:

```
books_list = ['Book 1', 'Book 2', 'Book 3']
list_to_set = set(books_list)  # Converting list to set
print(list_to_set)  # {'Book 2', 'Book 3', 'Book 1'}

books_tuple = ('Book 1', 'Book 2', 'Book 3')
tuple_to_set = set(books_tuple)  # Converting tuple to set
print(tuple_to_set)  # {'Book 2', 'Book 3', 'Book 1'}

books_dict = {'1': 'Book 1', '2': 'Book 2', '3': 'Book 3'}
dict_to_set = set(books_dict)  # Converting dict to set
print(dict_to_set)  # {'1', '3', '2'} 
```

### 向词典的转换

任何类型的集合、列表或元组都不能转换为字典，因为字典表示的数据结构中每个元素都包含一个键和值。

如果列表中的每个元素也是具有两个元素的列表或具有两个元素的元组，则可以将列表或元组转换成字典。

```
books_tuple_list = [(1, 'Book 1'), (2, 'Book 2'), (3, 'Book 3')]
tuple_list_to_dictionary = dict(books_tuple_list)  # Converting list to dict
print(tuple_list_to_dictionary)  # {1: 'Book 1', 2: 'Book 2', 3: 'Book 3'}

books_list_list = [[1, 'Book 1'], [2, 'Book 2'], [3, 'Book 3']]
tuple_list_to_dictionary = dict(books_list_list)  # Converting list to dict
print(tuple_list_to_dictionary)  # {1: 'Book 1', 2: 'Book 2', 3: 'Book 3'}

books_tuple_list = ([1, 'Book 1'], [2, 'Book 2'], [3, 'Book 3'])
tuple_list_to_set = dict(books_tuple_list)  # Converting tuple to set
print(tuple_list_to_set)  # {'Book 2', 'Book 3', 'Book 1'}

books_list_list = ([1, 'Book 1'], [2, 'Book 2'], [3, 'Book 3'])
list_list_to_set = dict(books_list_list)  # Converting list to set
print(list_list_to_set)  # {'Book 2', 'Book 3', 'Book 1'} 
```

如果我们想将一个集合转换成一个字典，我们需要将每个元素作为一个长度为 2 的元组。

```
books_tuple_set = {('1', 'Book 1'), ('2', 'Book 2'), ('3', 'Book 3')}
tuple_set_to_dict = dict(books_tuple_set)  # Converting dict to set
print(tuple_set_to_dict)  # {'1', '3', '2'} 
```

如果我们试图将每个元素都是长度为 2 的列表的集合转换成字典，我们将会得到一个错误:

```
books_list_set = {['1', 'Book 1'], ['2', 'Book 2'], ['3', 'Book 3']}
list_set_to_dict = dict(books_list_set)  # Converting dict to set
print(list_set_to_dict)  # {'1', '3', '2'} 
```

在我们运行最后一个代码块之后，我们将得到一个错误:

```
TypeError: unhashable type: 'list' 
```

## 包装数据类型

总之，Python 有多种数据类型可以用来存储数据。了解这些数据类型非常重要，这样您就可以根据需要选择正确的数据类型。

确保为您面前的任务使用正确的数据类型，以避免错误并优化性能。

# Python 中的控制流

### 条件语句

当你思考我们思考和相互交流的方式时，你可能会得到这样的印象:我们确实总是在使用条件。

*   如果是早上 8 点，我坐公交车去上班。
*   如果我饿了，我就吃。
*   如果这个东西便宜，我买得起。

这也是你在编程中可以做到的。我们可以使用条件来控制执行的流程。

为此，我们使用保留术语 *if* 和一个计算为真或假值的表达式。然后我们还可以使用一个 *else* 语句，在不满足 *if* 条件的情况下，我们希望流程继续。

为了更容易理解，让我们假设我们有一个例子，我们想检查一个数字是否是正数:

```
if number > 0:
    print("The given number is positive")
else:
    print("The given number is not positive") 
```

如果我们有 *number = 2* :我们将进入 *if* 分支并执行用于在控制台中打印以下文本的命令:

```
The given number is positive 
```

如果我们有另一个数字，例如-1，我们将在控制台中看到以下打印的消息:

```
The given number is not positive 
```

我们还可以添加额外的条件——而不仅仅是上面的两个——通过使用 *elif* ,当 *if* 表达式未被求值时，将对其求值。

让我们看一个例子，让你更容易理解:

```
if number > 0:
    print("The given number is positive")
elif number == 0:
    print("The given number is 0")
else:
    print("The given number is negative") 
```

现在，如果我们让 *number = 0* ，第一个条件将不被满足，因为该值不大于 0。正如您所猜测的，由于给定的数字等于 0，我们将在控制台中看到以下消息:

```
The given number is 0 
```

如果值为负，我们的程序将通过前两个条件，因为它们不满足，然后跳转到 *else* 分支，并在控制台中打印以下消息:

```
The given number is negative 
```

### 循环/迭代器

循环表示程序反复执行一组指令直到满足某个条件的能力。我们可以用*同时用*和*来做*。

先来看用*的迭代。*

#### Python 中的 for 循环

这种循环简单明了。您所要做的就是指定一个起始状态，并指出它应该迭代的范围，如下例所示:

```
for number in range(1, 7):
    print(number) 
```

在这个例子中，我们从 1 到 7 进行迭代，并在控制台中打印每个数字(从 1 到 7，不包括 7)。

我们可以根据需要更改范围中的起始和结束数字。通过这种方式，我们可以根据我们的具体情况非常灵活。

#### Python 中的 while 循环

现在让我们用 *while* 来描述迭代。这也是进行迭代的另一种方式，也是非常直接和直观的。

这里我们需要在 *while* 块之前指定一个开始条件，并相应地更新条件。

**而**循环需要一个**循环条件。**“如果保持为真，则继续迭代。在这个例子中，当`num`为`11`时，**循环条件**等于`False`。

```
number = 1

while number < 7:
    print(number)
    number += 1  # This part is necessary for us to add so that the iteration does not last forever 
```

这个 *while* 块将打印与我们对块的*使用的代码相同的语句。*

#### 迭代:遍历数据结构

既然我们已经讨论了迭代和列表，我们可以跳到遍历列表的方法。

我们不仅仅是把东西存储在数据结构中，然后把它们放在那里很久。我们应该能够在不同的场景中使用这些元素。

让我们看看之前的学生名单:

```
students = ["Albert", "Besart", "Fisnik", "Festim", "Gazmend"] 
```

现在，要遍历列表，我们只需输入:

```
for student in students:
    print(student) 
```

是的，就这么简单。我们遍历列表中的每个元素并打印它们的值。

我们也可以对字典这样做。但是由于字典中的元素有两个部分(键和值)，我们需要指定键和值，如下所示:

```
german_to_english_dictionary = {
    "Wasser": "Water",
    "Brot": "Bread",
    "Milch": "Milk"
}

for key, value in german_to_english_dictionary:
    print("The German word " + key + " means " + value + " in English") 
```

我们也只能从字典的柠檬中得到关键字:

```
for key in german_to_english_dictionary:
    print(key) 
```

注意，*键*和*值*只是我们选择用来说明迭代的变量名。但是我们可以为变量使用任何我们想要的名称，例如下面的例子:

```
for german_word, english_translation in german_to_english_dictionary:
    print("The German word " + german_word + " means " + english_translation + " in English") 
```

这次迭代将在控制台中打印与上一次迭代之前的代码块相同的内容。

我们也可以嵌套 for 循环。例如，假设我们想要遍历一个数字列表，并找到列表中每个元素与其他元素的和。我们可以使用嵌套的`for`循环来实现:

```
numbers = [1, 2, 3]
sum_of_numbers = []  # Empty list

for first_number in numbers:
    for second_number in numbers:  # Loop through the list and add the numbers
        current_sum = first_number + second_number
        # add current first_number from the first_list to the second_number from the second_list
        sum_of_numbers.append(current_sum)

print(sum_of_numbers)
# [2, 3, 4, 3, 4, 5, 4, 5, 6] 
```

#### 如何停止 for 循环

有时我们可能需要在循环结束前退出`for`循环。当条件已经满足或者我们已经找到了我们正在寻找的东西并且没有必要再继续下去时，就可能出现这种情况。

在这些情况下，我们可以使用`break`来停止`for`循环的任何其他迭代。

假设我们要检查一个列表中是否有负数。万一我们找到那个号码，我们就停止搜索。

让我们使用`break`来实现它:

```
my_list = [1, 2, -3, 4, 0]

for element in my_list:
    print("Current number: ", element)
    if element < 0:
        print("We just found a negative number")
        break

# Current number:  1
# Current number:  2
# Current number:  -3
# We just found a negative number 
```

正如我们所见，当我们到达-3 时，我们从`for`循环中脱离并停止。

#### 如何跳过迭代

也可能有这样的情况，当我们想要跳过某些迭代时，因为我们对它们不感兴趣，而且它们也没那么重要。我们可以使用`continue`来做到这一点，它会阻止代码块中它下面的代码执行，并将执行过程移向下一次迭代:

```
my_sum = 0
my_list = [1, 2, -3, 4, 0]

for element in my_list:
    if element < 0:  # Do not include negative numbers in the sum
        continue
    my_sum += element

print(my_sum)  # 7 
```

`pass`是一个语句，当我们要实现一个方法或其他东西，但我们还没有完成，不想出错时，我们可以用它来帮助我们。

即使部分代码丢失，它也能帮助我们执行程序:

```
my_list = [1, 2, 3]

for element in my_list:
    pass  # Do nothing 
```

## 条件语句总结

总之，Python 提供了条件语句来帮助您控制程序的流程。

`if`语句让您仅在满足特定条件时运行代码块。只有在满足另一个条件时，`elif`语句才允许您运行一段代码。并且`else`语句让你只在不满足其他条件的情况下运行一段代码。

这些语句对于控制程序的流程非常有用。

# Python 中的函数

在很多情况下，我们需要反复使用同一个代码块。我们的第一个猜测是想写多少遍就写多少遍。

客观地说，它确实有效，但事实是，这是一种非常糟糕的做法。我们正在做重复性的工作，这可能会很无聊，而且容易出现更多我们可能会忽略的错误。

这就是为什么我们需要开始使用可以定义一次的代码块，然后在其他地方使用相同的代码。

想想现实生活中的这种情况:你看到一个 YouTube 视频，已经被录制并上传到 YouTube 一次。接下来会有很多人观看，但是视频仍然是最初上传的那个。

换句话说，我们使用方法作为一组编码指令的代表，这些指令可以在代码中的任何地方被调用，我们不必重复编写。

如果我们想修改这个方法，我们只需在它第一次被声明的地方修改它，而在其他调用它的地方不需要做任何事情。

为了在 Python 中定义一个方法，我们首先使用`def`关键字，然后是函数名，最后是我们期望使用的参数列表。之后，我们需要在缩进后的新行中开始编写方法体。

```
def add(first_number, second_number):
    our_sum = first_number + second_number
    return our_sum 
```

从语法突出显示中可以看出，`def`和`return`都是 Python 中的关键字，不能用来命名变量。

现在，无论我们想在哪里调用这个`add`()函数，我们都可以在那里调用它，而不必担心如何完全实现它。

既然我们已经定义了这个方法，我们可以用下面的方式调用它:

```
result = add(1, 5)

print(result)  # 6 
```

你可能会认为这是一个如此简单的方法，并开始问，为什么我们还要为它写一个方法呢？

你是对的。这是一个非常简单的方法，只是为了向你介绍我们实现函数的方式。

让我们编写一个函数来计算两个指定数字之间的和:

```
def sum_in_range(starting_number, ending_number):
    result = 0

    while starting_number < ending_number:
        result = result + starting_number
        starting_number = starting_number + 1

    return result 
```

现在这是一组指令，你可以在其他地方调用，而不必全部重写。

```
result = sum_in_range(1, 5)

print(result)  # 10 
```

请注意，函数定义了一个作用域，这意味着在该作用域中定义的变量在外部是不可访问的。

例如，我们不能在函数范围之外访问名为`product`的变量:

```
def multiply_in_range(starting_number, ending_number):
    product = 1
    while starting_number < ending_number:
        product = product * starting_number
        starting_number = starting_number + 1
    return product 
```

`product`只能在这个方法的主体内部访问。

## 函数中的默认参数

当我们调用函数时，我们可以通过在函数的头部为它们写一个初始值来使一些参数可选。

让我们举一个例子，将用户的名字作为必需参数，将第二个参数作为可选参数。

```
def get_user(first_name, last_name=""):
    return f"Hi {first_name} {last_name}" 
```

我们现在用两个参数调用这个函数:

```
user = get_user("Durim", "Gashi")

print(user)  # Hi Durim Gashi 
```

即使没有指定第二个参数，我们现在也可以调用同一个函数:

```
user = get_user("Durim")

print(user)  # Hi Durim 
```

## 关键字参数列表

我们可以将函数的参数定义为关键字:

```
# The first argument is required. The other two are optional
def get_user(number, first_name='', last_name=''):
    return f"Hi {first_name} {last_name}" 
```

现在，我们可以通过将参数写成关键字来调用这个函数:

```
user = get_user(1, last_name="Gashi")

print(user)  # Hi  Gashi 
```

如您所见，我们可以省略`first_name`，因为它不是必需的。我们也可以在调用函数时改变参数的顺序，但它仍然会工作:

```
user = get_user(1, last_name="Gashi", first_name='Durim')

print(user)  # Hi Durim Gashi 
```

## 数据生命周期

在函数内部声明的变量不能在函数外部访问。他们被孤立了。

让我们看一个例子来说明这一点:

```
def counting():
    count = 0  # This is not accessible outside of the function.

counting()

print(count)  # This is going to throw an error when executing, since count is only declared inside the function and is not acessible outside that 
```

类似地，我们不能改变函数内部的变量，这些变量已经在函数外部声明，并且没有作为参数传递:

```
count = 3331

def counting():
    count = 0  # This is a new variable

counting()

print(count)  # 3331
# This is declared outside the function and has not been changed 
```

## 如何改变函数内部的数据

我们可以改变通过函数作为参数传递的可变数据。可变数据表示即使在声明之后我们也可以修改的数据。例如，列表是可变数据。

```
names = ["betim", "durim", "gezim"]

def capitalize_names(current_list):

    for i in range(len(current_list)):
        current_list[i] = current_list[i].capitalize()

    print("Inside the function:", current_list)

    return current_list

capitalize_names(names)  # Inside the function: ['Betim', 'Durim', 'Gezim']

print("Outside the function:", names)  # Outside the function: ['Betim', 'Durim', 'Gezim'] 
```

在不可变数据的情况下，我们只能修改函数内部的变量，但是函数外部的实际值将保持不变。不可变数据是字符串和数字:

```
name = "Betim"

def say_hello(current_param):
    current_param = current_param + " Gashi"
    name = current_param  # name is a local variable
    print("Value inside the function:", name)
    return current_param

say_hello(name)  # Value inside the function: Betim Gashi

print("Value outside the function:", name)  # Value outside the function: Betim 
```

如果我们真的想通过函数更新不可变变量，我们可以将函数的返回值赋给不可变变量:

```
name = "Betim"

def say_hello(current_param):
    current_param = current_param + " Gashi"
    name = current_param  # name is a local variable
    print("Value inside the function", name)
    return current_param

# Here we are assigning the value of name to the current_param that is returned from the function
name = say_hello(name)  # Value inside the function Betim Gashi

# Value outside the function: Betim Gashi
print("Value outside the function:", name) 
```

## λ函数

Lambda 函数是匿名函数，我们可以用它来返回输出。我们可以使用以下语法模式编写 lambda 函数:

```
lambda parameters: expression 
```

表达式只能写在一行中。

让我们用几个例子来说明这些匿名函数。

我们从一个将每个输入乘以 10 的函数开始:

```
tenfold = lambda number : number * 10

print(tenfold(10))  # 100 
```

让我们再写一个例子，在这个例子中，我们检查给定的参数是否为正:

```
is_positive =  lambda a : f'{a} is positive' if a > 0 else f'{a} is not positive'

print(is_positive(3))  # 3 is positive

print(is_positive(-1))  # -1 is not positive 
```

注意，如果 lambda 函数中没有`else`子句，我们就不能使用`if`子句。

此时，你可能会想，既然 lambda 函数看起来和其他函数几乎一样，为什么我们还需要使用它们呢？

我们可以在下一节中看到这一点。

### 作为函数参数的函数

到目前为止，我们已经看到了使用数字和字符串调用函数的方法。我们实际上可以用任何类型的 Python 对象调用函数。

我们甚至可以提供一个完整的函数作为函数的参数，这可以提供一个非常有用的抽象层次。

让我们看一个例子，我们想做一些从一个单位到另一个单位的转换:

```
def convert_to_meters(feet):
    return feet * 0.3048

def convert_to_feet(meters):
    return meters / 0.3048

def convert_to_miles(kilometers):
    return kilometers / 1.609344

def convert_to_kilometers(miles):
    return miles * 1.609344 
```

现在，我们可以创建一个通用函数，并将另一个函数作为参数传递:

```
def conversion(operation, argument):
    return operation(argument) 
```

我们现在可以这样称呼`conversion()`:

```
result = conversion(convert_to_miles, 10)

print(result)  # 6.2137119223733395 
```

如你所见，我们将`convert_to_miles`写成了函数`conversion()`的参数。我们可以使用其他已经定义的函数，比如:

```
result = conversion(convert_to_feet, 310)

print(result)  # 1017.0603674540682 
```

我们现在可以利用 lambdas，使这种类型的抽象更加简单。

我们可以简单地编写一个简洁的 lambda 函数，并在调用`conversion()`函数时将其用作参数，而不是编写所有这四个函数:

```
def conversion(operation, argument):
    return operation(argument)

result = conversion(lambda kilometers: kilometers / 1.609344, 10)

print(result)  # 6.2137119223733395 
```

这当然更简单。

让我们用其他几个内置函数的例子。

#### `map()`功能

map()是一个内置函数，它通过对现有列表的每个元素调用函数来获取结果，从而创建一个新的对象:

```
map(function_name, my_list) 
```

让我们看一个将 lambda 函数写成 map 函数的例子。

让我们使用列表理解将列表中的每个数字增加三倍:

```
my_list = [1, 2, 3, 4]

triple_list = [x * 3 for x in my_list]

print(triple_list)  # [3, 6, 9, 12] 
```

我们可以使用一个`map()`函数和一个 lambda 函数来实现:

```
my_list = [1, 2, 3, 4]

triple_list = map(lambda x: x * 3, my_list)

print(triple_list)  # [3, 6, 9, 12] 
```

这将创建一个新列表。旧列表不变。

#### `filter()`功能

这是另一个内置函数，我们可以用它来过滤满足条件的列表元素。

让我们首先使用列表理解从列表中过滤掉负面元素:

```
my_list = [3, -1, 2, 0, 14]

non_negative_list = [x for x in my_list if x >= 0]

print(non_negative_list)  # [3, 2, 0, 14] 
```

现在，我们将使用`filter()`和 lambda 函数过滤元素。这个函数返回一个对象，我们可以使用`list()`将它转换成一个列表:

```
my_list = [3, -1, 2, 0, 14]

non_negative_filter_object = filter(lambda x: x >= 0, my_list)

non_negative_list = list(non_negative_filter_object)

print(non_negative_list)  # [3, 2, 0, 14] 
```

现在你应该明白如何调用带有其他函数作为参数的函数，以及为什么 lambdas 是有用和重要的。

## Python 中的装饰者

装饰器代表一个接受另一个函数作为参数的函数。

我们可以把它看作是一种动态的方式来改变函数、方法或类的行为方式，而不必使用子类。

一旦一个函数作为参数传递给装饰器，它将被修改，然后作为一个新函数返回。

让我们从一个我们想要装饰的基本功能开始:

```
def reverse_list(input_list):
    return input_list[::-1] 
```

在这个例子中，我们只是返回一个反向列表。

我们还可以编写一个接受另一个函数作为参数的函数:

```
def reverse_list(input_list):
    return input_list[::-1]

def reverse_input_list(another_function, input_list):
    # we are delegating the execution to another_function() 
    return another_function(input_list)

result = reverse_input_list(reverse_list, [1, 2, 3])

print(result)  # [3, 2, 1] 
```

我们还可以将一个函数嵌套在另一个函数中:

```
def reverse_input_list(input_list):
    # reverse_list() is now a local function that is not accessible from the outside
    def reverse_list(another_list):
        return another_list[::-1]

    result = reverse_list(input_list)
    return result               # Return the result of the local function

result = reverse_input_list([1, 2, 3])
print(result)  # [3, 2, 1] 
```

在这个例子中，`reverse_list()`现在是一个局部函数，不能在`reverse_input_list()`函数的范围之外调用。

现在我们可以编写我们的第一个装饰器了:

```
def reverse_list_decorator(input_function):
    def function_wrapper():
        returned_result = input_function()
        reversed_list = returned_result[::-1]
        return reversed_list

    return function_wrapper 
```

`reverse_list_decorator()`是一个装饰函数，它将另一个函数作为输入。要调用它，我们需要编写另一个函数:

```
# Function that we want to decorate
def get_list():
    return [1, 2, 3, 4, 5] 
```

现在我们可以用我们的新函数作为参数调用装饰器:

```
decorator = reverse_list_decorator(get_list)  # This returns a reference to the function

result_from_decorator = decorator()  # Here we call the actual function using parenthesis

# We can now print the result in the console
print(result_from_decorator)  # [5, 4, 3, 2, 1] 
```

以下是完整的示例:

```
def reverse_list_decorator(input_function):
    def function_wrapper():
        returned_result = input_function()
        reversed_list = returned_result[::-1]
        return reversed_list

    return function_wrapper

# Function that we want to decorate
def get_list():
    return [1, 2, 3, 4, 5]

# This returns a reference to the function
decorator = reverse_list_decorator(get_list)

# Here we call the actual function using the parenthesis
result_from_decorator = decorator()

# We can now print the result in the console
print(result_from_decorator)  # [5, 4, 3, 2, 1] 
```

我们也可以使用注释调用装饰器。为此，我们在想要调用的装饰器名称前使用了`@`符号，并将其放在函数名称的正上方:

```
# Function that we want to decorate
@reverse_list_decorator  # The annotation of the decorator function
def get_list():
    return [1, 2, 3, 4, 5] 
```

现在，我们可以简单地调用函数`get_list()`，装饰器将被应用于其中:

```
result_from_decorator = get_list()

print(result_from_decorator)  # [5, 4, 3, 2, 1] 
```

### 如何堆叠装饰者

我们也可以对一个函数使用多个装饰器。它们的执行顺序是从上到下开始的，这意味着首先应用已定义的装饰器，然后是第二个，依此类推。

让我们做一个简单的实验，应用我们在前一节中定义的同一个装饰器两次。

首先让我们理解这是什么意思。

所以我们首先调用装饰器来反转一个列表:

`[1, 2, 3, 4, 5]`至`[5, 4, 3, 2, 1]`

然后我们再次应用它，但是现在使用的是装饰器先前调用的返回结果:

`[5, 4, 3, 2, 1]`=>= 

换句话说，反转一个列表，然后再次反转那个反转的列表，将会返回列表的原始顺序。

让我们用装饰者来看看这个:

```
@reverse_list_decorator
@reverse_list_decorator
def get_list():
    return [1, 2, 3, 4, 5]

result = get_list()

print(result)  # [1, 2, 3, 4, 5] 
```

我用另一个例子来解释这个。

让我们实现另一个装饰器，它只返回大于 1 的数字。然后，我们想用现有的装饰器反转返回的列表。

```
def positive_numbers_decorator(input_list):
    def function_wrapper():
        # Get only numbers larger than 0
        numbers = [number for number in input_list() if number > 0]
        return numbers

    return function_wrapper 
```

现在我们可以调用这个装饰器和我们已经实现的另一个装饰器:

```
@positive_numbers_decorator
@reverse_list_decorator
def get_list():
    return [1, -2, 3, -4, 5, -6, 7, -8, 9]

result = get_list()
print(result)  # [9, 7, 5, 3, 1] 
```

以下是完整的示例:

```
def reverse_list_decorator(input_function):
    def function_wrapper():
        returned_result = input_function()
        reversed_list = returned_result[::-1]  # Reverse the list
        return reversed_list

    return function_wrapper

# First decoorator
def positive_numbers_decorator(input_list):
    def function_wrapper():
        # Get only numbers larger than 0
        numbers = [number for number in input_list() if number > 0]
        return numbers

    return function_wrapper

# Function that we want to decorate

@positive_numbers_decorator
@reverse_list_decorator
def get_list():
    return [1, -2, 3, -4, 5, -6, 7, -8, 9]

result = get_list()
print(result)  # [9, 7, 5, 3, 1] 
```

### 如何向装饰函数传递参数

我们还可以向装饰函数传递参数:

```
def add_numbers_decorator(input_function):
    def function_wrapper(a, b):
        result = 'The sum of {} and {} is {}'.format(
            a, b, input_function(a, b))  # calling the input function with arguments
        return result
    return function_wrapper

@add_numbers_decorator
def add_numbers(a, b):
    return a + b

print(add_numbers(1, 2))  # The sum of 1 and 2 is 3 
```

#### 内置装饰器

Python 自带多个内置装饰器，比如`@classmethod`、`@staticmethod`、`@property`等等。我们将在下一章讨论这些。

## 功能总结

Python 是编写函数的优秀语言，因为它们很容易编写。

Lambda 函数是在 Python 中创建小而简洁的函数的好方法。当你不需要一个完整的功能时，或者当你只想测试一小段代码时，它们是完美的。

Python 装饰器是提高代码可读性和可维护性的好方法。它们允许你模块化你的代码，使它更有组织性。您还可以使用它们来执行各种任务，如日志记录、异常处理和测试。因此，如果您正在寻找一种清理 Python 代码的方法，可以考虑使用 decorators。

# Python 中的面向对象编程

如果你去当地的商店买饼干，你将会得到一个已经生产了许多其他版本的饼干。

工厂里有一台曲奇切片机，用来生产大量的曲奇饼干，然后分发到不同的商店，这些曲奇饼干再送到最终消费者手中。

我们可以把这种千篇一律的东西想象成一张蓝图，它被设计了一次，然后被多次使用。我们也在计算机编程中使用这种蓝图。

用于创建无数其他副本的蓝图被称为**类**。我们可以把一个类想象成叫做**饼干**、**工厂**、**建筑**、**书**、**铅笔**等等的类。我们可以使用 **cookie** 类作为蓝图来创建尽可能多的实例，我们称之为对象。

换句话说，蓝图是被用作*饼干切割器*的类，而在不同商店提供的饼干是*对象*。

**面向对象编程**代表了一种使用类和对象来组织程序的方式。我们使用类来创建对象。物体相互作用。

我们不会对所有的物体都使用完全相同的蓝图。有一个生产书的蓝图，另一个生产铅笔的蓝图，等等。我们需要根据属性和功能对它们进行分类。

从 Pencil 类创建的对象可以有颜色类型、制造商、特定的粗细等等。这些是**属性**。一个*铅笔*对象也可以*写出代表其功能的*，或者它的**方法**。

我们在不同的编程语言中使用类和对象，包括 Python。

让我们看看 Python 中一个非常基本的*自行车*类是怎样的:

```
class Bicycle:
    pass 
```

我们使用了关键字 *class* 来表示我们将要开始编写一个类，然后我们输入这个类的名字。

我们添加了`pass`,因为我们不希望 Python 解释器对我们大喊大叫，因为我们没有继续编写属于这个类的代码的剩余部分而抛出错误。

现在，如果我们想从这个类 *Bicycle* 中创建新的对象，我们可以简单地写下对象的名称(可以是您想要的任何变量名)并用用于创建新对象的构造函数方法 *Bicycle()* 初始化它:

```
favorite_bike = Bicycle() 
```

在这种情况下， *favorite_bike* 是从类*bike*创建的对象。它获得了类 Bicycle 的所有功能和属性。

我们可以丰富我们的*自行车*类，并包括额外的属性，这样我们就可以根据我们的需求定制*自行车*。

为此，我们可以定义一个名为 *init* 的构造函数方法，如下所示:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self.manufacturer = manufacturer
        self.color = color
        self.is_mountain_bike = is_mountain_bike 
```

注意方法名称`init`前后下划线的用法。它们表示 Python 解释器将该方法视为特殊方法的指示符。

这是一个不返回任何内容的方法。将它定义为该类的第一个方法是一个很好的实践，这样其他开发人员也可以看到它位于特定的行。

现在，如果我们想用自行车的蓝图创造新的物体，我们可以简单地写:

```
bike = Bicycle("Connondale", "grey", True) 
```

我们已经为这辆自行车提供了自定义参数，并将它们传递给构造函数方法。然后，作为回报，我们得到一辆具有这些特定属性的新自行车。你可能已经知道，我们正在设计一辆品牌为`Connondale`的灰色山地车。

我们还可以使用可选参数从类中创建对象，如下所示:

```
class Bicycle:
    # All the following attributes are optional
    def __init__(self, manufacturer=None, color='grey', is_mountain_bike=False):
        self.manufacturer = manufacturer
        self.color = color
        self.is_mountain_bike = is_mountain_bike 
```

现在，我们刚刚创建了具有这些属性的对象，这些属性当前在类的范围之外是不可访问的。

这意味着我们已经从 *Bicycle* 类创建了这个新对象，但是它对应的属性是不可访问的。为了访问它们，我们可以实现帮助我们访问它们的方法。

为此，我们将定义`getters`和`setters`，它们代表我们用来获取和设置对象属性值的方法。我们将使用一个名为`@property`的注释来帮助我们。

让我们用代码来看看:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self._manufacturer = manufacturer
        self._color = color
        self._is_mountain_bike = is_mountain_bike

    @property
    def manufacturer(self):
        return self._manufacturer

    @manufacturer.setter
    def manufacturer(self, manufacturer):
        self._manufacturer = manufacturer

bike = Bicycle("Connondale", "Grey", True)

print(bike.manufacturer)  # Connondale 
```

我们可以为类的所有属性编写 getters 和 setters:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self._manufacturer = manufacturer
        self._color = color
        self._is_mountain_bike = is_mountain_bike

    @property
    def manufacturer(self):
        return self._manufacturer

    @manufacturer.setter
    def manufacturer(self, manufacturer):
        self._manufacturer = manufacturer

    @property
    def color(self):
        return self._color

    @color.setter
    def color(self, color):
        self._color = color

    @property
    def is_mountain_bike(self):
        return self._is_mountain_bike

    @is_mountain_bike.setter
    def is_mountain_bike(self, is_mountain_bike):
        self.is_mountain_bike = is_mountain_bike

bike = Bicycle("Connondale", "Grey", True) 
```

既然我们已经定义了它们，我们可以将这些 getter 方法作为属性来调用:

```
print(bike.manufacturer)  # Connondale
print(bike.color)  # Grey
print(bike.is_mountain_bike)  # True 
```

我们还可以修改我们最初为任何属性使用的值，只需键入对象的名称和我们要更改内容的属性:

```
bike.is_mountain_bike = False
bike.color = "Blue"
bike.manufacturer = "Trek" 
```

我们的类也可以有其他方法，而不仅仅是 getters 和 setters。

让我们在类 Bicycle 中定义一个方法，然后我们可以从我们从该类创建的任何对象中调用该方法:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self._manufacturer = manufacturer
        self._color = color
        self._is_mountain_bike = is_mountain_bike

    def get_description(self):
        desc = "This is a " + self._color + " bike of the brand " + self._manufacturer
        return desc 
```

我们已经创建了一个非常简单的方法，在这个方法中，我们准备一个字符串作为我们正在创建的对象的属性的结果。然后我们可以像调用其他方法一样调用这个方法。

让我们来看看实际情况:

```
bike = Bicycle("Connondale", "Grey", True)

print(bike.get_description())  # This is a Grey bike of the brand Connondale 
```

## Python 中的方法

方法类似于函数，我们在上面已经讨论过了。

简而言之，我们将一些语句组合在一个名为 method 的代码块中。在那里，我们执行一些操作，这些操作我们希望不止一次地执行，并且不希望一次又一次地编写它们。最后，我们可能根本不会返回任何结果。

Python 中有三种类型的方法:

*   实例方法
*   类方法
*   静态方法

让我们简单地讨论一下方法的整体结构，然后再深入研究每种方法类型的细节。

#### 因素

方法的参数使我们有可能传递动态值，然后在执行方法内部的语句时考虑这些动态值。

`return`语句表示将是该方法中最后一个被执行的语句。它是 Python 解释器停止任何其他行的执行并返回值的指示符。

#### 自我论证

Python 中方法的第一个参数是`self`，这也是方法和函数的区别之一。它表示对其所属对象的引用。如果我们在声明时没有指定作为方法的第一个参数，那么第一个参数将被视为对象的引用。

我们只在声明方法时编写它，但是当我们使用对象作为调用者来调用特定方法时，我们不需要包含它。

我们并不要求将其命名为`self`，但这是一个被全世界编写 Python 代码的开发人员广泛实践的约定。

让我们在类`Bicycle`中定义一个实例方法，然后我们可以从我们从该类创建的任何对象中调用它:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self._manufacturer = manufacturer
        self._color = color
        self._is_mountain_bike = is_mountain_bike

    def get_description(self):
        desc = "This is a " + self._color + " bike of the brand " + self._manufacturer
        return desc 
```

我们已经创建了一个非常简单的方法，在这个方法中，我们准备一个字符串作为我们正在创建的对象的属性的结果。然后我们可以像调用其他方法一样调用这个方法:

```
bike = Bicycle("Connondale", "Grey", True)

print(bike.get_description())  # This is a Grey bike of the brand Connondale
# We are not passing any argument when calling the method get_description() since we do not need to include self at all 
```

### 类方法

到目前为止，我们已经讨论了实例方法。我们可以用对象调用这些方法。

类方法是我们可以使用类名调用的方法，并且我们根本不需要创建任何新对象就可以访问这些方法。

由于它是一种特定类型的方法，我们需要告诉 Python 解释器它实际上是不同的。我们通过改变语法做到这一点。

我们在类方法上使用注释`@classmethod`，在实例方法上使用类似于`self`用法的`cls`。`cls`只是引用调用该方法的类的一种常规方式——您不必使用这个名称。

让我们声明我们的第一个类方法:

```
class Article:
    blog = 'https://www.python.org/'

    # the init method is called when an instance of the class is created
    def __init__(self, title, content):
        self.title = title
        self.content = content

    @classmethod
    def get_blog(cls):
        return cls.blog 
```

现在让我们调用我们刚刚声明的这个类方法:

```
print(Article.get_blog())  # https://www.python.org/ 
```

注意，在调用`get_blog()`方法时，我们不必编写任何参数。另一方面，当我们声明方法和实例方法时，我们应该总是包含至少一个参数。

### 静态方法

这些方法与类变量或实例变量没有直接关系。你可以把它们想成是效用函数，用来帮助我们用调用它们时传递的参数做一些事情。

我们可以通过使用类名和由声明该方法的类创建的对象来调用它们。这意味着它们不需要让它们的第一个参数与调用它们的对象或类相关(就像实例方法使用参数`self`和类方法使用参数`cls`一样)。

我们可以用来调用它们的参数数量没有限制。

要创建它，我们需要使用`@staticmethod`注释。

让我们创建一个静态方法:

```
class Article:
    blog = 'https://www.python.org/'

    # the init method is called when an instance of the class is created
    def __init__(self, title, content):
        self.title = title
        self.content = content

    @classmethod
    def get_blog(cls):
        return cls.blog

    @staticmethod
    def print_creation_date(date):
        print(f'The blog was created on {date}')

article = Article('First Article', 'This is the first article')

# Calling the static method using the object
article.print_creation_date('2022-07-18')  # The blog was created on 2022-07-18

# Calling the static method using the class name
Article.print_creation_date('2022-07-21')  # The blog was created on 2022-07-21 
```

静态方法不能修改类或实例属性。它们应该像效用函数一样。

如果我们试图改变一个类，我们会得到错误:

```
class Article:
    blog = 'https://www.python.org/'

    # the init method is called when an instance of the class is created
    def __init__(self, title, content):
        self.title = title
        self.content = content

    @classmethod
    def get_blog(cls):
        return cls.blog

    @staticmethod
    def set_title(self, date):
        self.title = 'A random title' 
```

如果我们现在尝试调用这个静态方法，我们将会得到一个错误:

```
# Calling the static method using the class name
Article.set_title('2022-07-21') 
```

```
TypeError: set_title() missing 1 required positional argument: 'date' 
```

这是因为静态方法没有对`self`的任何引用，因为它们不直接与对象或类相关，所以它们不能修改属性。

### 访问修饰符

当创建类时，我们可以限制对某些属性和方法的访问，这样它们就不那么容易被访问了。

我们有`public`和`private`访问修饰符。

让我们看看这两者是如何工作的。

#### 公共属性

公共属性是可以从类内部和外部访问的属性。

默认情况下，Python 中的所有属性和方法都是公共的。如果我们希望它们是私有的，我们需要指定这一点。

让我们看一个公共属性的例子:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self.manufacturer = manufacturer
        self.color = color
        self.is_mountain_bike = is_mountain_bike

    def get_manufacturer(self):
        return self.manufacturer 
```

在前面的代码块中，`color`和`get_manufacturer()`都可以在类外访问，因为它们是`public`，在类内外都可以访问:

```
bike = Bicycle("Connondale", "Grey", True)

print(bike.color)  # Grey
print(bike.get_manufacturer())  # Connondale 
```

#### 私有属性

私有属性只能从类内部直接访问。

我们可以使用双下划线创建 properties 属性，如下例所示:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike, old):
        self.manufacturer = manufacturer
        self.color = color
        self.is_mountain_bike = is_mountain_bike
        self.__old = old  # This is a private property 
```

现在，如果我们试图访问`__old`，我们将得到一个错误:

```
bike = Bicycle("Connondale", "Grey", True, False)

print(bike.__old)  # AttributeError: 'Bicycle' object has no attribute '__old' 
```

现在让我们看一个例子，在这个例子中，我们使用双下划线在我们希望成为私有的方法名前面声明私有方法:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike, old):
        self.manufacturer = manufacturer
        self.color = color
        self.is_mountain_bike = is_mountain_bike
        self.__old = old  # This is a private property

    def __get_old(self):  # This is a private method
        return self.__old 
```

现在，如果我们想从类外部调用这个私有方法，将会抛出一个错误:

```
bike = Bicycle("Connondale", "Grey", True, False)

print(bike.__get_old())  # AttributeError: 'Bicycle' object has no attribute '__get_old' 
```

在 Python 中拥有私有变量并不是一种常见的做法。然而，开发人员可能会发现有必要限制访问，以便特定变量不会被随意访问和修改。

## 如何在 Python 中隐藏信息

当你出去使用咖啡机时，你不需要知道机器背后所有的工程细节。

你的车也是一样。当你坐在你的驾驶座上时，你没有分析和理解汽车每一部分的所有细节。你对它们有一些基本的概念，但除此之外，你只需要专注于驾驶。

这是一种对外界人员进入的限制，这样他们就不必担心内部发生的具体细节。

我们也可以用 Python 来实现。

到目前为止，我们已经看到了面向对象编程的基础模块，比如类和对象。

类是蓝图，用于创建称为对象的实例。我们可以使用不同类的对象相互交互，构建一个健壮的程序。

当我们开发自己的程序时，我们可能不需要让每个人都知道我们的类的所有细节。所以我们可以限制对它们的访问，这样某些属性就不太可能被无意中访问和错误地修改。

为了帮助我们做到这一点，我们隐藏了类的一些部分，简单地提供了一个接口，这个接口包含了关于类内部工作的更少的细节。

我们可以用两种方式隐藏数据:

1.  包装
2.  抽象

让我们从封装开始。

### 什么是封装？

封装不仅仅是 Python 特有的东西。其他编程语言也使用它。

简而言之，我们可以将其定义为在一个类中绑定数据和方法。然后我们使用这个类来创建对象。

我们通过使用`private`访问修饰符来封装类，然后可以限制对这些属性的直接访问。这可能会限制控制。

然后，我们应该编写可以提供对外部世界的访问的公共方法。

这些方法被称为`getters`和`setters`。

**getter** 方法是我们用来获取属性值的方法。

**setter** 是我们用来设置属性值的方法。

让我们先定义一个`getter`和一个`setter`方法，我们可以用它们来获取值:

```
class Smartphone:
    def __init__(self, type=None):  # defining initializer for case of no argument
        self.__type = type  # setting the type here in the beginning when the object is created

    def set_type(self, value):
        self.__type = value

    def get_type(self):
        return (self.__type) 
```

现在，让我们使用这个类来设置类型并获取类型:

```
smartphone = Smartphone('iPhone')  # we are setting the type using the constructor method

# getting the value of the type
print(smartphone.get_type())   # iPhone

# Changing the value of the type
smartphone.set_type('Samsung')  

# getting the new value of the type
print(smartphone.get_type())    # Samsung 
```

到目前为止，我们所做的是设置并读取从`Smartphone`类创建的对象的私有属性的值。

我们也可以使用`@property`注释来定义`getters`和`setters`。

让我们用代码来看看:

```
class Bicycle:
    def __init__(self, manufacturer, color):
        self._manufacturer = manufacturer
        self._color = color

    @property
    def manufacturer(self):
        return self._manufacturer

    @manufacturer.setter
    def manufacturer(self, manufacturer):
        self._manufacturer = manufacturer

    @property
    def color(self):
        return self._color

    @color.setter
    def color(self, color):
        self._color = color

bike = Bicycle("Connondale", "Grey") 
```

既然我们已经定义了它们，我们可以将这些 getter 方法作为属性来调用:

```
print(bike.manufacturer)  # Connondale
print(bike.color)  # Grey 
```

我们还可以修改最初用于任何属性的值，只需键入要修改的对象和属性的名称:

```
bike.is_mountain_bike = False
bike.color = "Blue" 
```

我们的类也可以有其他方法，而不仅仅是 getters 和 setters。

让我们在类 Bicycle 中定义一个方法，然后我们可以从我们从该类创建的任何对象中调用该方法:

```
class Bicycle:
    def __init__(self, manufacturer, color, is_mountain_bike):
        self._manufacturer = manufacturer
        self._color = color
        self._is_mountain_bike = is_mountain_bike

    def get_description(self):
        desc = "This is a " + self._color + " bike of the brand " + self._manufacturer
        return desc 
```

我们已经创建了一个非常简单的方法，在这个方法中，我们准备一个字符串作为我们正在创建的对象的属性的结果。然后我们可以像调用其他方法一样调用这个方法。

让我们来看看实际情况:

```
bike = Bicycle("Connondale", "Grey", True)

print(bike.get_description())  # This is a Grey bike of the brand Connondale 
```

#### 但是我们为什么需要封装呢？

这看起来很有前途，很有意思，但是你可能还没有完全明白。你可能需要一些额外的理由来解释为什么你需要这种类型的隐藏。

为了说明这一点，我们来看另一个类，其中有一个私有属性叫做`salary`。假设我们不关心封装，我们只是试图快速构建一个类，并在我们的会计客户项目中使用它。

假设我们有下面的类:

```
class Employee:
    def __init__(self, name=None, email=None, salary=None):
        self.name = name
        self.email = email
        self.salary = salary 
```

现在，让我们创建一个新的`employee`对象，并相应地初始化它的属性:

```
# We are creating an object
betim = Employee('Betim', 'betim@company.com', 5000)

print(betim.salary)  # 5000 
```

由于`salary`没有受到任何保护，我们可以毫无问题地为这个新对象设定一个新的薪水:

```
betim.salary = 25000

print(betim.salary)  # 25000 
```

正如我们所看到的，这个人没有经过任何评估或面试就拿到了之前的五倍薪水。事实上，这发生在几秒钟之内。那很可能会严重影响公司的预算。

我们不想那样做。我们希望限制对`salary`属性的访问，这样就不会从其他地方调用它。我们可以通过在属性名前使用双下划线来做到这一点，如下所示:

```
class Employee:
    def __init__(self, name=None, email=None, salary=None):
        self.__name = name
        self.__email = email
        self.__salary = salary 
```

让我们创建一个新对象:

```
# We are creating an object
betim = Employee('Betim', 'betim@company.com', 1000) 
```

现在，如果我们试图访问它的属性，我们不能这样做，因为它们是私有属性:

```
print(betim.salary)  # 1000 
```

试图访问任何属性都会出现一个错误:

```
AttributeError: 'Employee' object has no attribute 'salary' 
```

我们可以简单地实现一个返回属性的方法，但是我们没有为某人偷偷增加工资提供任何方法:

```
class Employee:
    def __init__(self, name=None, email=None, salary=None):
        self.__name = name
        self.__email = email
        self.__salary = salary

    def get_info(self):
        return self.__name, self.__email, self.__salary 
```

现在，我们可以访问由该类创建的对象的信息:

```
# We are creating an object
betim = Employee('Betim', 'betim@company.com', '5000')

print(betim.get_info())  # ('Betim', 'betim@company.com', '5000') 
```

总之，封装有助于我们保护对象的属性，并以可控的方式提供对它们的访问。

## Python 中的继承

在现实生活中，我们可以和其他人分享许多特征。

我们都需要吃东西、喝水、工作、睡觉、运动等等。这些和许多其他的行为和特征是全世界数十亿人共有的。

它们不是我们这一代人独有的东西。这些特征和人类一样早就存在了。

这也是将持续到未来几代人的事情。

我们还可以在对象和类之间拥有某些共享的特征，我们在计算机编程中使用**继承**来实现这些特征。这包括属性和方法。

假设我们有一个名为`Book`的类。它应该包含标题、作者、页数、类别、ISBN 等等。我们将保持我们的类简单，只使用两个属性:

```
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def get_short_book_paragraph(self):
        short_paragraph = "This is a short paragraph of the book."
        return short_paragraph 
```

现在，我们可以从这个类创建一个对象并访问它:

```
first_book = Book("Atomic Habits", "James Clear")

print(first_book.title)  # Atomic Habits
print(first_book.author)  # James Clear
print(first_book.get_short_book_paragraph())  # This is a short paragraph of the book. 
```

现在让我们创建一个类`Book`的子类，它继承了类`Book`的属性和方法，但是还有一个额外的方法叫做`get_book_description()`:

```
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def get_short_book_paragraph(self):
        short_paragraph = "This is a short paragraph of the book."
        return short_paragraph

class BookDetails(Book):
    def __init__(self, title, author):
        Book.__init__(self, title, author)
        # Here we are call the constructor of the parent class Book

    def get_book_details(self):
        description = "Title: " + self.title + ". "
        description += "Author: " + self.author
        return description 
```

注意我们告诉 Python`BookDetails`是类`Book`的子类的语法:

```
class BookDetails(Book): 
```

如果我们试图从类`Book`的对象中访问这个新方法，我们将得到一个错误:

```
first_book = Book("Atomic Habits", "James Clear")

print(first_book.get_book_details())
# AttributeError: 'Book' object has no attribute 'get_book_details' 
```

发生这种情况是因为这个方法`get_book_details()`只能从`BookDetails`的对象中访问:

```
first_book_details = BookDetails("Atomic Habits", "James Clear")

print(first_book_details.get_book_details())
# Title: Atomic Habits. Author: James Clear 
```

然而，我们可以访问父类中定义的任何方法，在我们的例子中是`Book`类:

```
first_book_details = BookDetails("Atomic Habits", "James Clear")

print(first_book_details.get_short_book_paragraph())
# This is a short paragraph of the book. 
```

在前面的类中，`Book`被认为是父类或超类，而`BookDetails`被认为是子类或子类。

### `super()`功能

有一个叫做`super()`的特殊函数，我们可以从一个子类中使用它来引用它的父类，而不用写出父类的确切名称。

我们在初始化器中使用它，或者在调用父类的属性或方法时使用它。

让我们用例子来说明这三个问题。

#### 如何将`super()`用于初始值设定项

我们可以在子类的构造函数方法中使用`super()`，甚至可以调用超类的构造函数:

```
class Animal():
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Cat(Animal):
    def __init__(self, name, age):
        super().__init__(name, age)  # calling the parent class constructor
        self.health = 100  # initializing a new attribute that is not in the parent class 
```

我们还可以用父类的名称替换`super()`，这将再次以同样的方式工作:

```
class Animal():
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Cat(Animal):
    def __init__(self, name, age):
        Animal.__init__(name, age)  # calling the parent class constructor
        self.health = 100  # initializing a new attribute that is not in the parent class 
```

即使改变子构造函数中的行的顺序也不会导致任何错误。

#### 如何对父类的类属性使用`super()`

我们可以使用`super()`来访问父类的类属性，这在父类和子类对一个属性使用相同的名称时尤其有用。

为了了解实际情况，让我们假设我们有一个名为`name`的类属性，它同时出现在父类和子类中。我们想从父类和子类中访问这个变量。

要做到这一点，我们只需要写下`super()`，然后是变量的名称:

```
class Producer:  # parent class
    name = 'Samsung'

class Seller(Producer):  # child class
    name = 'Amazon'

    def get_product_details(self):
        # Calling the variable from the parent class
        print("Producer:", super().name)

        # Calling the variable from the child class
        print("Seller:", self.name) 
```

现在，如果我们调用方法`get_product_details()`，我们将在控制台中打印以下内容:

```
seller = Seller()

seller.get_product_details()

# Producer: Samsung
# Seller: Amazon 
```

#### 如何对父类的方法使用`super()`

我们同样可以使用`super()`调用父类中的方法。

```
class Producer:  # parent class
    name = 'Samsung'

    def get_details(self):
        return f'Producer name: {self.name}'

class Seller(Producer):  # child class
    name = 'Amazon'

    def get_details(self):
        # Calling the method from the parent class
        print(super().get_details())

        # Calling the variable from the child class
        print(f'Seller name: {self.name}')

seller = Seller()
seller.get_details()

# Producer name: Amazon
# Seller name: Amazon 
```

这就是你需要知道的关于`super()`的一切。

### 继承的类型

基于父类和子类的关系，我们可以有不同类型的继承:

1.  单一的
2.  多层次
3.  等级体系的
4.  多重
5.  混合物

##### 1.单一遗传

我们可以有一个只从另一个类继承的类:

```
class Animal:
    def __init__(self):
        self.health = 100

    def get_health(self):
        return self.health

class Cat(Animal):
    def __init__(self, name):
        super().__init__()
        self.health = 150
        self.name = name

    def move(self):
        print("Cat is moving")

cat = Cat("Cat")

# Calling the method from the parent class
print(cat.get_health())  # 150

# Calling the method from the child class
cat.move()  # Cat is moving 
```

##### 2.多层次继承

这是另一种类型的继承，其中一个类从另一个类继承:类 A 从类 B 继承，类 B 从类 c 继承。

让我们用 Python 来实现它:

```
class Creature:
    def __init__(self, alive):
        self.alive = alive

    def is_it_alive(self):
        return self.alive

class Animal(Creature):
    def __init__(self):
        super().__init__(True)
        self.health = 100

    def get_health(self):
        return self.health

class Cat(Animal):
    def __init__(self, name):
        super().__init__()
        self.name = name

    def move(self):
        print("Cat is moving")

cat = Cat("Cat")

# Calling the method from the parent of the parent class
print(cat.is_it_alive())

# Calling the method from the parent class
print(cat.get_health())  # 150

# Calling the method from the child class
cat.move()  # Cat is moving 
```

##### 3.分层继承

当我们从同一个父类派生出多个子类时，我们就有了层次继承。这些子类继承自父类:

```
class Location:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def get_location(self):
        return self.x, self.y

class Continent(Location):
    pass

class Country(Location):
    pass

continent = Continent(0, 0)
print(continent.get_location())  # (0, 0)

country = Country(10, 30)
print(country.get_location())  # (10, 30) 
```

##### 4.多重遗传

我们可以有另一种类型的继承，即多重继承，它可以帮助我们同时继承多个类。

假设我们有一个名为`Date`的类和另一个名为`Time`的类。

然后我们可以实现另一个类，然后继承这两个类:

```
class Date:
    date = '2022-07-23'  # Hardcoded date

    def get_date(self):
        return self.date

class Time:
    time = '20:20:20'  # Hardcoded time

    def get_time(self):
        return self.time

class DateTime(Date, Time):  # Inheriting from both
    def get_date_time(self):
        return self.get_date() + ' ' + self.get_time()  # getting methods from its parent classes

date_time = DateTime()
print(date_time.get_date_time())  # 2022-07-23 20:20:20 
```

##### 5.混合遗传

混合继承是多重和多层次继承的组合:

```
class Vehicle:
    def print_vehicle(self):
        print('Vehicle')

class Car(Vehicle):
    def print_car(self):
        print('Car')

class Ferrari(Car):
    def print_ferrari(self):
        print('Ferrari')

class Driver(Ferrari, Car):
    def print_driver(self):
        print('Driver') 
```

现在，如果我们从类`Driver`创建一个对象，我们可以调用所有类中的所有方法:

```
driver = Driver()

# Calling all methods from the subclass
driver.print_vehicle()  # Vehicle
driver.print_car()  # Car
driver.print_ferrari()  # Ferrari
driver.print_driver()  # Driver 
```

## Python 中的多态性

这是另一个来自面向对象编程的重要概念，指的是一个对象表现为不同形式并调用不同行为的可能性。

使用多态性的内置函数的一个例子是方法`len()`，它既可以用于字符串，也可以用于列表:

```
print(len('Python'))  # 6

print(len([2, 3, -43]))  # 3 
```

我们可以举另一个名为`House`的类的例子。我们可以有不同的子类从那个超类继承方法和属性，也就是像`Condo`、`Apartment`、`SingleFamilyHouse`、`MultiFamilyHouse`这样的类。

让我们假设我们想要在`House`类中实现一个方法，该方法应该获得面积。

每种居住类型都有不同的大小，所以每个子类都应该有不同的实现。

现在我们可以将方法定义为子类，例如:

*   `getAreaOfCondo()`
*   `getAreaOfApartment()`
*   `getAreaOfSingleFamilyHouse()`
*   `getAreaOfMultiFamilyHouse()`

这将迫使我们记住每个子类的名称，这可能很繁琐，并且在调用它们时容易出错。

幸运的是，我们可以使用来自多态性的更简单的方法。

我们可以使用方法和继承来实现多态性。

让我们首先看看如何使用方法实现多态性。

### 多态性使用方法

假设我们有两个类，分别是`Condo`和`Apartment`。它们都有返回值的方法`get_area()`。

他们每个人都将有一个自定义的实现。

现在我们要调用的方法取决于对象的类类型:

```
class Condo:
    def __init__(self, area):
        self.area = area

    def get_area(self):
        return self.area

class Apartment:
    def __init__(self, area):
        self.area = area

    def get_area(self):
        return self.area 
```

让我们从这些类中创建两个对象:

```
condo = Condo(100)

apartment = Apartment(200) 
```

现在，我们可以将它们放在一个列表中，并对这两个对象调用相同的方法:

```
places_to_live = [condo, apartment]

for place in places_to_live:
    print(place.get_area())  # same method for both objects 
```

执行该操作后，我们将在控制台中看到以下内容:

```
# 100
# 200 
```

这就是用方法实现多态性的方式。

### 遗传多态性

我们不仅可以从超类中调用方法。我们也可以使用相同的名称，但是每个子类有不同的实现。

让我们首先定义一个超类:

```
class House:
    def __init__(self, area):
        self.area = area

    def get_price(self):
        pass 
```

然后我们将实现超类`House`的子类`Condo`和`Apartment`:

```
class House:
    def __init__(self, area):
        self.area = area

    def get_price(self):
        pass

class Condo(House):
    def __init__(self, area):
        self.area = area

    def get_price(self):
        return self.area * 100 

class Apartment(House):
    def __init__(self, area):
        self.area = area

    def get_price(self):
        return self.area * 300 
```

正如我们所见，两个子类都有方法`get_price()`，但是实现不同。

我们现在可以从子类中创建新的对象，并调用这个方法，它将根据调用它的对象对*进行变形:*

```
condo = Condo(100)

apartment = Apartment(200)

places_to_live = [condo, apartment]

for place in places_to_live:
    print(place.get_price()) 
```

执行该操作后，我们将在控制台中看到以下内容:

```
# 10000
# 60000 
```

这是多态的另一个例子，我们有一个同名方法的具体实现。

# 在 Python 中导入

使用 Python 这样的流行语言的一个主要好处是它有大量的库，您可以使用并从中受益。

世界各地的许多开发人员都慷慨地投入时间和知识，并发布了许多真正有用的库。这些图书馆可以为我们的专业工作节省大量时间，也可以为我们的业余项目节省大量时间。

以下是一些包含非常有用的方法的模块，您可以立即在项目中开始使用:

*   `time`:时间访问和转换
*   `csv` : CSV 文件读写
*   `math`:数学函数
*   `email`:创建、发送和处理电子邮件
*   `urllib`:使用 URL

要导入一个或多个模块，我们只需要写下`import`，然后是我们想要导入的模块的名称。

让我们导入第一个模块:

```
import os 
```

现在，让我们一次导入多个模块:

```
import os, numbers, math 
```

一旦我们导入了一个模块，我们就可以开始使用其中的方法了。

```
import math

print(math.sqrt(81))  # 9.0 
```

我们还可以为导入的模块使用新名称，方法是为它们指定一个别名`as alias`，其中`alias`是您想要的任何变量名:

```
import math as math_module_that_i_just_imported

result = math_module_that_i_just_imported.sqrt(4)

print(result)  # 2.0 
```

## 如何限制我们想要导入的内容

有时候我们不想导入一个包含所有方法的包。这是因为我们希望避免用我们希望自己实现的方法或变量覆盖模块中的方法或变量。

我们可以使用以下表格指定想要导入的零件:

```
from module import function 
```

让我们举一个从`math`模块只导入平方根函数的例子:

```
from math import sqrt

print(sqrt(100))  # 10.0 
```

## 从模块导入所有内容的问题

我们也可以从一个模块中导入所有东西，这可能会成为一个问题。让我们用一个例子来说明这一点。

假设我们想要导入包含在`math`模块中的所有内容。我们可以这样使用星号:

```
from math import *  # The asterisk is an indicator to include everything when importing 
```

现在，让我们假设我们想要声明一个名为`sqrt`的变量:

```
sqrt = 25 
```

当我们试图从数学模块调用函数`sqrt()`时，我们将会得到一个错误，因为解释器将调用我们刚刚在前面的代码块中声明的最新的`sqrt`变量:

```
print(sqrt(100)) 
```

```
TypeError: 'float' object is not callable 
```

# 如何在 Python 中处理异常

当我们实现 Python 脚本或者做任何类型的实现时，我们会得到很多错误，即使语法是正确的。

在执行过程中发生的这些类型的错误被称为异常。

我们确实不需要投降，也不需要对他们做任何事情。我们可以编写处理程序来做一些事情，这样程序的执行就不会停止。

## Python 中的常见异常

以下是 Python 中发生的一些最常见的异常，其定义来自于 [Python 文档](https://docs.python.org/3/library/exceptions.html):

*   异常(Exception)–这是一个类，是大多数其他发生的异常类型的超类。
*   **name error**–找不到本地或全局名称时引发。
*   **attribute error**–当属性引用或赋值失败时引发。
*   **语法错误**–当解析器遇到语法错误时引发。
*   **type error**–当一个操作或函数被应用到一个不合适类型的对象时引发。关联的值是一个字符串，给出关于类型不匹配的详细信息。
*   **ZeroDivisionError**–当除法或模运算的第二个参数为零时引发。
*   **io error**–当 I/O 操作(如 print 语句、内置 open()函数或 file 对象的方法)因 I/O 相关原因失败时引发，如“找不到文件”或“磁盘已满”。
*   **import error**–当 import 语句找不到模块定义或者当来自… import 的**找不到要导入的名称时引发。**
*   **index error**–当序列下标超出范围时引发。
*   **key error**–在现有键集中找不到映射(字典)键时引发。
*   **value error**–当内置操作或函数接收到类型正确但值不合适的参数，并且这种情况没有通过更精确的异常(如 IndexError)来描述时引发。

还有许多其他的错误类型，但是您现在真的不需要了解它们。你也不太可能一直看到各种类型的错误。

您可以在 [Python 文档](https://docs.python.org/3/library/exceptions.html)中看到更多类型的异常。

## 如何在 Python 中处理异常

让我们从一个非常简单的例子开始，写一个程序，故意抛出一个错误，然后我们可以修复它。

我们要做一个被零除的除法，你们可能在学校里见过:

```
print(5 / 0) 
```

如果我们尝试执行该命令，我们将在控制台中看到以下错误:

```
ZeroDivisionError: division by zero 
```

如果我们在任何类型的 Python 程序中出现这种情况，我们应该在一个`try/except`块中捕获并包装这个错误。

我们需要在`try`块中编写我们预计会抛出错误的那部分代码。然后，我们通过指定我们希望发生的错误类型来捕获`except`块中的错误类型。

我们来看第一个例子。

让我们看看如何处理该错误，以便我们也能得到发生此类错误的通知:

```
try:
    5 / 0
except ZeroDivisionError:
    print('You cannot divide by 0 mate!') 
```

如您所见，一旦我们到达发生被 0 除的部分，我们就在控制台中打印一条消息。

我们也可以完全省略`ZeroDivisionError`部分:

```
try:
    5 / 0
except:
    print('You cannot divide by 0 mate!') 
```

然而，不建议这样做，因为我们在一个单独的`except`块中捕捉所有类型的错误，并且我们不确定捕捉的是哪种类型的错误(这对我们很有用)。

让我们继续另一种类型的错误。

现在我们将尝试使用一个根本没有定义的变量:

```
name = 'User'

try:
    person = name + surname  # surname is not declared
except NameError:
    print('A variable is not defined') 
```

在前面的例子中，我们在声明变量`surname`之前使用了它，因此将抛出一个`NameError`。

让我们继续另一个很常见的例子。

当我们使用列表时，使用超出范围的索引可能是一个常见的错误。这意味着我们使用的索引大于或小于列表中元素的索引范围。

让我们用一个例子来说明这一点，这里将抛出一个`IndexError`:

```
my_list = [1, 2, 3, 4]

try:
	print(my_list[5])
    # This list only has 4 elements, so its indexes range from 0 to 3
except IndexError:
    print('You have used an index that is out of range') 
```

我们也可以使用带有多个`except`错误的单个`try`块:

```
my_list = [1, 2, 3, 4]

try:
    print(my_list[5])
    # This list only has 4 elements, so its indexes range from 0 to 3
except NameError:
    print('You have used an invalid value')
except ZeroDivisionError:
    print('You cannot divide by zero')
except IndexError:
    print('You have used an index that is out of range') 
```

在前面的例子中，我们试图首先捕捉是否有任何变量被使用但没有声明。如果这个错误发生，那么这个`except`块将接管执行流。这个执行流程将在这里停止。

然后，我们尝试检查我们是否被零除。如果这个错误被抛出，那么这个`except`块将接管执行，它里面的所有东西都将被执行。类似地，我们继续声明其余的错误。

我们也可以在括号中放入多个错误来捕捉多个异常。但是这对我们没有帮助，因为我们不知道具体抛出了什么错误。换句话说，以下方法确实有效，但不推荐使用:

```
my_list = [1, 2, 3, 4]

try:
    print(my_list[5])
    # This list only has 4 elements, so its indexes range from 0 to 3
except (NameError, ZeroDivisionError, IndexError):
    print('A NameError, ZeroDivisionError, or IndexError occurred') 
```

#### `finally`关键字

在通过了`try`和`except`之后，我们可以声明并执行另一个块。这个程序块以`finally`关键字开始，无论我们是否抛出错误，它都会被执行:

```
my_list = ['a', 'b']

try:
    print(my_list[0])
except IndexError:
    print('An IndexError occurred')
finally:
    print('The program is ending. This is going to be executed.') 
```

如果我们执行前面的代码块，我们将在控制台中看到以下内容:

```
The program is ending. This is going to be executed. 
```

我们通常在`finally`块中编写想要清理的代码。这包括关闭文件、停止与数据库的连接、完全退出程序等等。

#### 尝试，否则，除了

我们可以在`try`和`except`中编写语句，但是我们也可以使用一个`else`块，在这里我们可以编写代码，如果没有抛出错误，我们希望执行这些代码:

```
my_list = ['a', 'b']

try:
    print(my_list[0])
except IndexError:
    print('An IndexError occurred')
else:
    print('No error occurred. Congratulations!') 
```

如果我们执行上面的代码，我们将在控制台中打印出以下内容:

```
No error occurred. Congratulations! 
```

### 异常总结

希望您现在已经理解了异常以及处理它们的各种方法。如果你正确地处理它们，不应该有任何突然的中断导致你的程序意外失败。

# Python 中的用户输入

当你想开发一个交互式程序，并在命令行中获得用户输入时，你可以调用一个名为`input()`的函数。

这非常简单，您所要做的就是声明一个变量，您要在这个变量中保存用户键入的值:

```
user_input = input("Please type in your name.") 
```

然后我们可以使用这个值并打印出来:

```
print(f'Hello {user_input}. Nice to have you here') 
```

# 包扎

这本书代表了我试图让你更快更容易地学习 Python 的基础知识。关于 Python，还有很多我在本书中没有涉及到的东西，但是我们将把它留在这里。

希望这是对你有用的参考。

既然您已经有机会学习如何编写 Python，那就走出去，用您的代码行产生积极的影响。

## 获取这本书的 PDF 格式

你可以在这里下载这本书的 PDF 版本。