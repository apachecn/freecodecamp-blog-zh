# 终极 Python 初学者手册

> 原文：<https://www.freecodecamp.org/news/the-python-guide-for-beginners/>

在过去的几年中，Python 已经成为发展最快的编程语言之一。

它不仅被广泛使用，如果你想进入编程领域，它也是一门很棒的语言。

这本 Python 初学者指南可以让你在几个小时内学会这门语言的核心，而不是几个星期。

快速信息:[你可以为初学者下载这个 Python 指南的 PDF 版本](https://renanmf.com/python-guide-beginners/)。

准备好开始了吗？

# 目录

1.  [Python 简介](#introductiontopython)
2.  [安装 Python 3](#installingpython3)
3.  [运行代码](#runningcode)
4.  [语法](#syntax)
5.  [评论](#comments)
6.  [变量](#variables)
7.  [类型](#types)
8.  [类型转换](#typecasting)
9.  [用户输入](#userinput)
10.  [操作员](#operators)
11.  [条件句](#conditionals)
12.  [列表](#lists)
13.  [元组](#tuples)
14.  [设置](#sets)
15.  [字典](#dictionaries)
16.  [while 循环](#whileloops)
17.  [用于循环](#forloops)
18.  [功能](#functions)
19.  [范围](#scope)
20.  [列出理解](#listcomprehensions)
21.  [λ函数](#lambdafunctions)
22.  [模块](#modules)
23.  [若**名** == ' **主** '](#if__name____main__)
24.  [文件](#files)
25.  [类和对象](#classesandobjects)
26.  [继承](#inheritance)
27.  [异常情况](#exceptions)
28.  [结论](#conclusion)

# Python 简介

Python 于 1990 年由荷兰的吉多·范·罗苏姆创建。

这种语言的目标之一是非程序员也能使用。

Python 还被设计成程序员学习的第二语言，因为它的学习曲线低且易于使用。

Python 可以在 Mac、Linux、Windows 和许多其他平台上运行。

Python 是:

*   解释的:它可以在运行时执行，程序中的变化可以立即被察觉。说的很技术一点，Python 有编译器。与 Java 或 C++的区别在于它的透明性和自动化程度。使用 Python，我们不必担心编译步骤，因为它是实时完成的。代价是解释语言通常比编译语言慢。

*   语义动态:您不必为变量指定类型，也没有什么让您这样做。

*   面向对象:Python 中的一切都是对象。但是您可以选择以面向对象、过程化甚至函数式的方式编写代码。

*   高级:你不用处理低级的机器细节。

Python 最近发展很快，部分原因是因为它在以下领域有很多用途:

*   系统脚本:这是一个自动化日常重复任务的伟大工具。

*   数据分析:这是一种很好的实验语言，有大量的库和工具来处理数据、创建模型、可视化结果，甚至部署解决方案。这被用于金融、电子商务和研究等领域。

*   Web 开发:像 Django 和 Flask 这样的框架允许开发 web 应用程序、API 和网站。

*   机器学习:Tensorflow 和 Pytorch 是一些允许科学家和行业在图像识别、健康、自动驾驶汽车和许多其他领域开发和部署人工智能解决方案的库。

您可以轻松地将代码组织成模块，并重用它们或与其他人共享它们。

最后，我们必须记住 Python 在版本 2 和版本 3 之间有突破性的变化。由于 Python 2 的支持将于 2020 年结束，本文仅基于 Python 3。

所以让我们开始吧。

# 安装 Python 3

如果你使用 Mac 或 Linux，你已经安装了 Python。但是 Windows 并没有默认安装 Python。

你也可能有 Python 2，我们将使用 Python 3。所以你应该先看看你有没有 Python 3。

在您的终端中键入以下内容。

```
python3 -V 
```

注意大写的`V`。

如果您的结果类似于‘Python 3 . x . y’，例如，`Python 3.8.1`，那么您就可以开始了。

如果没有，请根据您的操作系统遵循以下说明。

## 在 Windows 上安装 Python 3

去 https://www.python.org/downloads/。

下载最新版本。

下载完成后，双击安装程序。

在第一个屏幕上，选中指示“将 Python 3.x 添加到路径”的框，然后单击“立即安装”。

等待安装过程完成，直到下一个屏幕显示消息“安装成功”。

点击“关闭”。

## 在 Mac 上安装 Python 3

从 App Store 安装 [XCode](https://itunes.apple.com/br/app/xcode/id497799835) 。

通过在终端中运行以下命令来安装命令行工具。

```
xcode-select --install 
```

我推荐用自制的。转到[https://brew.sh/](https://brew.sh/)并按照第一页上的说明进行安装。

安装完 Homebrew 之后，运行下面的`brew`命令来安装 Python 3。

```
brew update
brew install python3 
```

Homebrew 已经把 Python 3 加到路径里了，所以你不用做别的。

## 在 Linux 上安装 Python 3

要使用 Ubuntu 和 Debian 中可用的`apt`进行安装，请输入以下内容:

```
sudo apt install python3 
```

要使用 RedHat 和 CentOS 中的`yum`进行安装，请输入以下内容:

```
sudo yum install python3 
```

# 运行代码

您可以在终端中作为命令直接运行 Python 代码，也可以将代码保存在扩展名为`.py`的文件中并运行 Python 文件。

## 末端的

当您想运行一些简单的东西时，建议直接在终端中运行命令。

打开命令行，键入`python3`

```
renan@mypc:~$ python3 
```

您应该在您的终端中看到类似这样的内容，指示版本(在我的例子中是 Python 3.6.9)、操作系统(我使用的是 Linux)和一些帮助您的基本命令。

`>>>`告诉我们我们在 Python 控制台中。

```
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

让我们通过运行第一个程序来执行基本的数学运算并添加两个数来测试它。

```
>>> 2 + 2 
```

输出是:

```
4 
```

要退出 Python 控制台，只需输入`exit()`。

```
>>> exit() 
```

## 运行`.py`文件

如果你有一个复杂的程序，有许多行代码，Python 控制台不是最好的选择。

另一种方法是打开一个文本编辑器，输入代码，然后用扩展名`.py`保存文件。

让我们这样做，用下面的内容创建一个名为`second_program.py`的文件。

```
print('Second Program') 
```

`print()`功能在屏幕上打印信息。

消息用单引号或双引号括起来，两者的作用是一样的。

要运行该程序，请在您的终端上执行以下操作:

```
renan@mypc:~$ python3 second_program.py 
```

输出是:

```
Second Program 
```

# 句法

Python 以其简洁的语法而闻名。

这种语言避免使用不必要的字符来表示某种特殊性。

## 分号

Python 不使用分号来结束行。新的一行足以告诉解释器新的命令开始了。

方法会显示一些东西。

在本例中，我们有两个命令显示单引号内的消息。

```
print('First command')
print('Second command') 
```

输出:

```
First command
Second command 
```

但是下面是**错**由于分号在最后:

```
print('First command');
print('Second command'); 
```

## 刻痕

许多语言使用花括号来定义范围。

Python 的解释器只使用缩进来定义一个作用域何时结束，另一个作用域何时开始。

这意味着你必须注意每行开头的空格——它们有意义，如果放错了位置，可能会破坏你的代码。

函数的定义是这样的:

```
def my_function():
    print('First command') 
```

这个**不起作用**因为第二行的缩进丢失了，会抛出一个错误:

```
def my_function():
print('First command') 
```

## 区分大小写和变量

Python 是区分大小写的。所以变量`name`和`Name`不是同一个东西，存储不同的值。

```
name = 'Renan'
Name = 'Moura' 
```

如您所见，只需使用`=`符号给变量赋值，就可以轻松创建变量。

这意味着`name`商店为“雷南”,`Name`商店为“莫拉”。

## 评论

最后，要注释代码中的某些内容，请使用散列符号`#`。

注释部分不影响程序流程。

```
# this function prints something
def my_function():
    print('First command') 
```

这只是一个概述。在下一章的例子和更广泛的解释中，每一个的细节将变得更加清晰。

# 评论

注释的目的是解释代码中发生了什么。

注释与代码一起编写，但不影响程序流程。

你一个人工作的时候，可能评论感觉不是你该写的东西。毕竟，此时此刻，你知道每一行代码的原因。

但是如果一年后新人加入你的项目，项目有 3 个模块，每个模块有 10，000 行代码，那该怎么办？

想想那些对你的应用一无所知的人，他们突然不得不维护它，修复它，或者添加新功能。

记住，对于一个给定的问题，没有单一的解决方案。你解决问题的方式是你的，而且只属于你。如果你让 10 个人解决同一个问题，他们会想出 10 种不同的解决方案。

如果你想让别人完全理解你的推理，好的代码设计是必须的，但是注释是任何代码库不可或缺的一部分。

## 如何用 Python 写注释

Python 中注释的语法相当简单:只需在想要成为注释的文本前使用散列符号`#`。

```
#This is a comment and it won't influence my program flow 
```

您可以使用注释来解释某段代码的作用。

```
#calculates the sum of any given two numbers
a + b 
```

## 多行注释

也许你想评论一些非常复杂的东西或者描述一些过程如何在你的代码中工作。

在这些情况下，您可以使用多行注释。

为此，只需对每一行使用一个散列标记`#`。

```
#Everything after the hash mark # is a comment
#This is a comment and it won't influence my program flow

#Calculates the cost of the project given variables a and b
#a is the time in months it will take until the project is finished
#b is how much money it will cost per month
a + b * 10 
```

# 变量

在任何程序中，您都需要存储和操作数据来创建流程或一些特定的逻辑。

这就是变量的作用。

你可以用一个变量存储一个名字，用另一个变量存储一个人的年龄，甚至用一个更复杂的类型像字典一样一次性存储所有这些。

## 创建，也称为声明

在 Python 中，声明变量是一个基本而简单的操作

只需选择一个名称，并使用`=`符号赋予它一个值。

```
name='Bob'

age=32 
```

您可以使用`print()`函数来显示变量的值。

```
print(name)

print(age) 
```

```
Bob

32 
```

注意在 Python 中没有特殊的词来声明变量。

当你赋值的时候，变量就在内存中创建了。

Python 还有动态类型，这意味着你不必告诉它你的变量是文本还是数字。

解释器根据赋值来推断类型。

如果你需要，你也可以通过改变变量的值来重新声明它。

```
#declaring name as a string
name='Bob'
#re-declaring name as an int
name = 32 
```

但是请记住，不建议这样做，因为变量必须有意义和上下文。

如果我有一个名为`name`的变量，我不希望它存储一个数字。

## 命名规格

让我们从上一节继续，当我谈到意义和语境的时候。

不要使用像`x`或`y`这样的随机变量名。

说你要存储一个聚会的时间，就叫`party_time`。

哦，你注意到下划线`_`了吗？

按照惯例，如果您想使用由两个或更多单词组成的变量名，可以用下划线将它们分隔开。这叫蛇案。

另一种选择是使用 CamelCase，如`partyTime`所示。这在其他语言中很常见，但在 Python 中并不像前面所说的那样是惯例。

变量是区分大小写的，所以`party_time`和`Party_time`是不同的。此外，请记住，约定告诉我们总是使用小写。

请记住，使用您可以在程序中轻松回忆起来的名称。糟糕的命名会耗费你大量的时间，并导致恼人的错误。

总之，变量名:

*   区分大小写:`time`和`TIME`不相同
*   必须以下划线`_`或字母开头(不要以数字开头)
*   只能包含数字、字母和下划线。没有特殊字符，如:#、$、&、@等。

比如这个，就是**不**允许:`party#time`，`10partytime`。

# 类型

为了在 Python 中存储数据，你需要使用一个变量。每个变量都有自己的类型，这取决于所存储数据的值。

Python 具有动态类型，这意味着您不必显式声明变量的类型——但是如果您愿意，您可以这样做。

列表、元组、集合和字典都是数据类型，稍后会有专门的章节提供更多的细节，但是我们在这里将简要地看一下它们。

这样，我可以在各自的部分向您展示每一个最重要的方面和操作，同时保持这一部分更加简洁，重点是让您对 Python 中的主要数据类型有一个大致的了解。

## 确定类型

首先，我们来学习如何确定数据类型。

只需使用`type()`函数并将您选择的变量作为参数传递，如下例所示。

```
print(type(my_variable)) 
```

## 布尔代数学体系的

布尔类型是最基本的编程类型之一。

布尔型变量只能代表*真*或*假*。

```
my_bool = True
print(type(my_bool))

my_bool = bool(1024)
print(type(my_bool)) 
```

```
<class 'bool'>
<class 'bool'> 
```

## 数字

有三种类型的数值类型:int、float 和 complex。

### 整数

```
my_int = 32
print(type(my_int))

my_int = int(32)
print(type(my_int)) 
```

```
<class 'int'>
<class 'int'> 
```

### 浮动

```
my_float = 32.85
print(type(my_float))

my_float = float(32.85)
print(type(my_float)) 
```

```
<class 'float'>
<class 'float'> 
```

### 复杂的

```
my_complex_number = 32+4j
print(type(my_complex_number))

my_complex_number = complex(32+4j)
print(type(my_complex_number)) 
```

```
<class 'complex'>
<class 'complex'> 
```

## 线

文本类型是最常见的类型之一，通常被称为*字符串*，或者在 Python 中，简称为`str`。

```
my_city = "New York"
print(type(my_city))

#Single quotes have exactly
#the same use as double quotes
my_city = 'New York'
print(type(my_city))

#Setting the variable type explicitly
my_city = str("New York")
print(type(my_city)) 
```

```
<class 'str'>
<class 'str'>
<class 'str'> 
```

您可以使用`+`操作符来连接字符串。

串联是指当你有两个或更多的字符串，你想把它们连接成一个。

```
word1 = 'New '
word2 = 'York'

print(word1 + word2) 
```

```
New York 
```

string 类型有许多内置的方法，让我们可以操作它们。我将演示其中一些方法是如何工作的。

`len()`函数返回一个字符串的长度。

```
print(len('New York')) 
```

```
8 
```

`replace()`方法用另一部分替换字符串的一部分。举个例子，让我们用“新”代替“旧”。

```
print('New York'.replace('New', 'Old')) 
```

```
Old York 
```

`upper()`方法将以大写形式返回所有字符。

```
print('New York'.upper()) 
```

```
NEW YORK 
```

`lower()`方法正好相反，以小写形式返回所有字符。

```
print('New York'.lower()) 
```

```
new york 
```

## 列表

列表中的项目是有序的，您可以根据需要多次添加相同的项目。一个重要的细节是列表是可变的。

可变性意味着您可以在列表创建后通过添加、删除或者仅仅改变它们的值来改变列表。这些操作将在后面的列表部分演示。

```
my_list = ["bmw", "ferrari", "maclaren"]
print(type(my_list))

my_list = list(("bmw", "ferrari", "maclaren"))
print(type(my_list)) 
```

```
<class 'list'>
<class 'list'> 
```

## 元组

元组就像一个列表:有序的，并允许项目重复。

只有一点不同:元组是不可变的。

不变性意味着元组创建后不能更改。例如，如果您试图添加或更新一个项目，Python 解释器会显示一个错误。我将在后面专门讨论元组的部分说明这些错误。

```
my_tuple = ("bmw", "ferrari", "maclaren")
print(type(my_tuple))

my_tuple = tuple(("bmw", "ferrari", "maclaren"))
print(type(my_tuple)) 
```

```
<class 'tuple'>
<class 'tuple'> 
```

## 设置

集合不保证项目的顺序，也不会被编入索引。

使用集合时的一个关键点是:它们不允许一个项目重复。

```
my_set = {"bmw", "ferrari", "maclaren"}
print(type(my_set))

my_set = set(("bmw", "ferrari", "maclaren"))
print(type(my_set)) 
```

```
<class 'set'>
<class 'set'> 
```

## 字典

字典不保证元素的顺序，并且是可变的。

字典的一个重要特点是，您可以为每个元素设置自己的访问键。

```
my_dict = {"country" : "France", "worldcups" : 2}
print(type(my_dict))

my_dict = dict(country="France", worldcups=2)
print(type(my_dict)) 
```

```
<class 'dict'>
<class 'dict'> 
```

# 铅字铸造

类型转换允许您在不同类型之间转换。

举例来说，这样你就可以把一个`int`变成一个`str`，或者把一个`float`变成一个`int`。

## 显式转换

要将变量转换成字符串，只需使用`str()`函数。

```
# this is just a regular explicit intialization
my_str = str('32') 
print(my_str)

# int to str
my_str = str(32) 
print(my_str)

# float to str
my_str = str(32.0)
print(my_str) 
```

```
32
32
32.0 
```

要将变量转换成整数，只需使用`int()`函数。

```
# this is just a regular explicit intialization
my_int = int(32) 
print(my_int)

# float to int: rounds down to 3
my_int = int(3.2) 
print(my_int)

# str to int
my_int = int('32') 
print(my_int) 
```

```
32
3
32 
```

要将变量转换成浮点数，只需使用`float()`函数。

```
# this is an explicit intialization
my_float = float(3.2)   
print(my_float)

# int to float
my_float = float(32)     
print(my_float)

# str to float
my_float = float('32')  
print(my_float) 
```

```
3.2
32.0
32.0 
```

我上面做的叫做*显式*类型转换。

在某些情况下，您不需要显式地进行转换，因为 Python 可以自己完成转换。

## 隐式转换

下面的例子显示了添加一个`int`和一个`float`时的隐式转换。

注意`my_sum`是`float`。Python 使用`float`来避免数据丢失，因为`int`类型不能表示十进制数字。

```
my_int = 32
my_float = 3.2

my_sum = my_int + my_float

print(my_sum)

print(type(my_sum)) 
```

```
35.2
<class 'float'> 
```

另一方面，在这个例子中，当你添加一个`int`和一个`str`时，Python 将无法进行隐式转换，显式类型转换是必要的。

```
my_int = 32
my_str = '32'

# explicit conversion works
my_sum = my_int + int(my_str)
print(my_sum)

#implicit conversion throws an error
my_sum = my_int + my_str 
```

```
64

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str' 
```

当试图在不进行显式转换的情况下添加`float`和`str`类型时，会抛出相同的错误。

```
my_float = 3.2
my_str = '32'

# explicit conversion works
my_sum = my_float + float(my_str)
print(my_sum)

#implicit conversion throws an error
my_sum = my_float + my_str 
```

```
35.2

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'float' and 'str' 
```

# 用户输入

如果在命令行中运行程序时需要与用户交互(例如，询问一条信息)，可以使用`input()`函数。

```
country = input("What is your country? ") #user enters 'Brazil'

print(country) 
```

```
Brazil 
```

捕获的值总是`string`。请记住，您可能需要使用类型转换来转换它。

```
age = input("How old are you? ") #user enters '29'

print(age)

print(type(age))

age = int(age)

print(type(age)) 
```

每个`print()`的输出为:

```
29

<class 'str'>

<class 'int'> 
```

请注意，年龄 29 被捕获为`string`，然后显式转换为`int`。

# 经营者

在编程语言中，运算符是可以应用于变量和值的特殊符号，以便执行算术/数学和比较等操作。

Python 有很多可以应用于变量的操作符，我将展示最常用的操作符。

## 算术运算符

算术运算符是最常见的运算符，也是最容易识别的运算符。

它们允许你进行简单的数学运算。

它们是:

*   `+`:添加
*   `-`:减法
*   `*`:乘法
*   `/`:分部
*   `**`:求幂运算
*   `//`:地板除法，将除法结果向下舍入
*   `%`:模数，给出除法的余数

让我们来看一个程序，展示它们是如何使用的。

```
print('Addition:', 5 + 2)
print('Subtraction:', 5 - 2)
print('Multiplication:', 5 * 2)
print('Division:', 5 / 2)
print('Floor Division:', 5 // 2)
print('Exponentiation:', 5 ** 2)
print('Modulus:', 5 % 2) 
```

```
Addition: 7

Subtraction: 3

Multiplication: 10

Division: 2.5

Floor Division: 2

Exponentiation: 25

Modulus: 1 
```

### 串联

串联是指当你有两个或更多的字符串，你想把它们连接成一个。

当你有多个变量的信息并且想要组合它们时，这是有用的。

例如，在下一个例子中，我将分别包含我的名和姓的两个变量组合起来，得到我的全名。

`+`操作符也用于连接。

```
first_name = 'Renan '
last_name = 'Moura'

print(first_name + last_name) 
```

```
Renan Moura 
```

因为串联应用于字符串，所以要将字符串与其他类型串联，必须使用`str()`进行显式类型转换。

我必须用`str()`将`int`值 30 类型转换为字符串，以将其与字符串的其余部分连接起来。

```
age = 'I have ' + str(30) + ' years old'

print(age) 
```

```
I have 30 years old 
```

## 比较运算符

使用比较运算符比较两个值。

这些运算符返回`True`或`False`。

它们是:

*   `==`:相等
*   `!=`:不相等
*   `>`:大于
*   `<`:小于
*   `>=`:大于或等于
*   `<=`:小于或等于

让我们来看一个程序，展示它们是如何使用的。

```
print('Equal:', 5 == 2)
print('Not equal:', 5 != 2)
print('Greater than:', 5 > 2)
print('Less than:', 5 < 2)
print('Greater than or equal to:', 5 >= 2)
print('Less than or equal to:', 5 <= 2) 
```

```
Equal: False

Not equal: True

Greater than: True

Less than: False

Greater than or equal to: True

Less than or equal to: False 
```

## 赋值运算符

顾名思义，这些运算符用于给变量赋值。

第一个例子中的`x = 7`是一个直接赋值，将数字`7`存储在变量`x`中。

赋值操作将右边的值赋给左边的变量。

其他运算符是算术运算符的简单缩写。

在第二个例子中，`x`以`7`开始，`x += 2`是`x = x + 2`的另一种写法。这意味着`x`的先前值被`2`相加，并被重新分配给现在等于`9`的`x`。

*   `=`:简单赋值

```
x = 7
print(x) 
```

```
7 
```

*   `+=`:添加和分配

```
x = 7
x += 2
print(x) 
```

```
9 
```

*   `-=`:减法和赋值

```
x = 7
x -= 2
print(x) 
```

```
5 
```

*   `*=`:乘法和赋值

```
x = 7
x *= 2
print(x) 
```

```
14 
```

*   `/=`:划分和分配

```
x = 7
x /= 2
print(x) 
```

```
3.5 
```

*   `%=`:模数和赋值

```
x = 7
x %= 2
print(x) 
```

```
1 
```

*   `//=`:楼层划分和分配

```
x = 7
x //= 2
print(x) 
```

```
3 
```

*   `**=`:取幂和赋值

```
x = 7
x **= 2
print(x) 
```

```
49 
```

## 逻辑运算符

逻辑运算符用于结合应用布尔代数的语句。

它们是:

*   `and` : `True`仅当两个陈述都为真时
*   `or` : `False`仅当 x 和 y 都为假时
*   `not`:操作符`not`简单地反转输入，`True`变成`False`，反之亦然。

让我们看一个程序，展示每一个是如何使用的。

```
x = 5
y = 2

print(x == 5 and y > 3) 

print(x == 5 or y > 3) 

print(not (x == 5)) 
```

```
False

True

False 
```

## 成员运算符

这些操作符提供了一种简单的方法来检查某个对象是否按顺序出现:`string`、`list`、`tuple`、`set`和`dictionary`。

它们是:

*   `in`:如果对象存在，返回`True`
*   `not in`:如果对象不存在，返回`True`

让我们看一个程序，展示每一个是如何使用的。

```
number_list = [1, 2, 4, 5, 6]

print( 1 in number_list)

print( 5 not in number_list)

print( 3 not in number_list) 
```

```
True

False

True 
```

# 条件式

条件是任何编程语言的基石之一。

它们允许您根据您可以检查的特定条件来控制程序流。

## `if`声明

实现条件的方法是通过`if`语句。

`if`语句的一般形式是:

```
if expression:
    statement 
```

`expression`包含一些返回布尔值的逻辑，只有当返回的是`True`时才会执行`statement`。

一个简单的例子:

```
bob_age = 32
sarah_age = 29

if bob_age > sarah_age:
    print('Bob is older than Sarah') 
```

```
Bob is older than Sarah 
```

我们有两个变量表示 Bob 和 Sarah 的年龄。该条件用简单的英语说就是“如果鲍勃的年龄大于莎拉的年龄，则打印短语‘鲍勃比莎拉大’”。

由于条件返回`True`，该短语将被打印在控制台上。

## `if else`和`elif`语句

在我们的最后一个例子中，程序只在条件返回`True`时做一些事情。

但是我们也希望它在返回`False`时做一些事情，或者甚至在第一个条件不满足时检查第二个或第三个条件。

在这个例子中，我们交换了鲍勃和莎拉的年龄。第一个条件将返回`False`，因为 Sarah 现在长大了，然后程序将在`else`后打印短语。

```
bob_age = 29
sarah_age = 32

if bob_age > sarah_age:
    print('Bob is older than Sarah')
else:
    print('Bob is younger than Sarah') 
```

```
Bob is younger than Sarah 
```

现在，考虑下面带有`elif`的例子。

```
bob_age = 32
sarah_age = 32

if bob_age > sarah_age:
    print('Bob is older than Sarah')
elif bob_age == sarah_age:
    print('Bob and Sarah have the same age')
else:
    print('Bob is younger than Sarah') 
```

```
Bob and Sarah have the same age 
```

`elif`的目的是在执行`else`之前提供一个要检查的新条件。

我们又一次改变了他们的年龄，现在他们都是 32 岁。

因此，满足了`elif`中的条件。由于两人的年龄相同，程序将打印“Bob 和 Sarah 的年龄相同”。

请注意，您可以拥有任意多的`elif`,只需按顺序排列即可。

```
bob_age = 32
sarah_age = 32

if bob_age > sarah_age:
    print('Bob is older than Sarah')
elif bob_age < sarah_age:
    print('Bob is younger than Sarah')
elif bob_age == sarah_age:
    print('Bob and Sarah have the same age')
else:
    print('This one is never executed') 
```

```
Bob and Sarah have the same age 
```

在这个例子中，`else`永远不会被执行，因为所有的可能性都包含在前面的条件中，因此可以被删除。

## 嵌套条件句

您可能需要检查一个以上的条件才能发生某些事情。

在这种情况下，您可以嵌套您的`if`语句。

例如，只有当两个`if`都通过时，才会打印第二个短语“Bob 是最老的”。

```
bob_age = 32
sarah_age = 28
mary_age = 25

if bob_age > sarah_age:
    print('Bob is older than Sarah')
    if bob_age > mary_age:
        print('Bob is the oldest') 
```

```
Bob is older than Sarah
Bob is the oldest 
```

或者，根据逻辑，用布尔代数使它更简单。

这样，你的代码更小，可读性更好，也更容易维护。

```
bob_age = 32
sarah_age = 28
mary_age = 25

if bob_age > sarah_age and bob_age > mary_age:
    print('Bob is the oldest') 
```

```
Bob is the oldest 
```

## 三元运算符

三元运算符是一行`if`语句。

对于简单条件来说非常方便。

看起来是这样的:

```
<expression> if <condition> else <expression> 
```

考虑以下 Python 代码:

```
a = 25
b = 50
x = 0
y = 1

result = x if a > b else y

print(result) 
```

```
1 
```

这里我们使用四个变量，`a`和`b`代表条件，`x`和`y`代表表达式。

`a`和`b`是我们为了评估某个条件而相互核对的值。在这种情况下，我们检查`a`是否大于`b`。

如果表达式成立，即`a`大于`b`，那么`x`的值将归于等于 0 的`result`。

然而，如果`a`小于`b`，那么我们将`y`的值赋给`result`，`result`将保存值`1`。

由于`a`小于`b`，25 < 50，`result`将从`y`得到`1`作为最终值。

记住病情如何评估的简单方法是用简单的英语阅读。

我们的例子是这样的:如果`a`大于`b`，则`result`将是`x`，否则为`y`。

# 列表

正如在 [Types](#types) 一节中所承诺的，这一节和接下来的三节关于元组、集合和字典将对它们中的每一个进行更深入的解释，因为它们是 Python 中组织和处理数据的非常重要和广泛使用的结构。

列表中的项目是有序的，您可以根据需要多次添加相同的项目。

一个重要的细节是列表是可变的。

可变性意味着您可以在列表创建后通过添加、删除或者仅仅改变它们的值来改变列表。

### 初始化

#### 空列表

```
people = [] 
```

#### 带有初始值的列表

```
people = ['Bob', 'Mary'] 
```

### 在列表中添加

要在列表末尾添加项目，使用`append()`。

```
people = ['Bob', 'Mary']
people.append('Sarah')

print(people) 
```

```
['Bob', 'Mary', 'Sarah'] 
```

要指定新项目的位置，使用`insert()`方法。

```
people = ['Bob', 'Mary']
people.insert(0, 'Sarah')

print(people) 
```

```
['Sarah', 'Bob', 'Mary'] 
```

### 在列表中更新

指定要更新的项目的位置，并设置新值

```
people = ['Bob', 'Mary']
people[1] = 'Sarah'
print(people) 
```

```
['Bob', 'Sarah'] 
```

### 在列表中删除

使用`remove()`方法删除作为参数给出的项目。

```
people = ['Bob', 'Mary']
people.remove('Bob')
print(people) 
```

```
['Mary'] 
```

要删除所有人，使用`clear()`方法:

```
people = ['Bob', 'Mary']
people.clear() 
```

### 在列表中检索

使用索引来引用项目。

请记住，索引从 0 开始。

因此，要访问第二项，请使用索引 1。

```
people = ['Bob', 'Mary']
print(people[1]) 
```

```
Mary 
```

### 检查给定项目是否已经存在于列表中

```
people = ['Bob', 'Mary']

if 'Bob' in people:
  print('Bob exists!')
else:
  print('There is no Bob!') 
```

# 元组

元组类似于列表:它是有序的，并且允许项目重复。

只有一点不同:元组是不可变的。

如果你还记得的话，不变性意味着你不能在元组创建后改变它。例如，如果您试图添加或更新一个项目，Python 解释器会显示一个错误。

### 初始化

#### 空元组

```
people = () 
```

#### 具有初始值的元组

```
people = ('Bob', 'Mary') 
```

### 在元组中添加

元组是不可变的。这意味着如果你试图添加一个项目，你会看到一个错误。

```
people = ('Bob', 'Mary')
people[2] = 'Sarah' 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment 
```

### 元组中的更新

更新项目也会返回错误。

但是有一个窍门:你可以转换成一个列表，改变项，然后再转换回一个元组。

```
people = ('Bob', 'Mary')
people_list = list(people)
people_list[1] = 'Sarah'
people = tuple(people_list)
print(people) 
```

```
('Bob', 'Sarah') 
```

### 删除元组中的

出于同样的原因，你不能添加一个项目，你也不能删除一个项目，因为他们是不可变的。

### 在元组中检索

使用索引来引用项目。

```
people = ('Bob', 'Mary')
print(people[1]) 
```

```
Mary 
```

### 检查给定的项是否已经存在于元组中

```
people = ('Bob', 'Mary')

if 'Bob' in people:
  print('Bob exists!')
else:
  print('There is no Bob!') 
```

# 设置

集合不保证项目的顺序，也不被索引。

使用集合时的一个关键点是:它们不允许一个项目重复。

### 初始化

#### 空集

```
people = set() 
```

#### 用初始值设置

```
people = {'Bob', 'Mary'} 
```

### 添加到集合中

使用`add()`方法添加一个项目。

```
people.add('Sarah') 
```

使用`update()`方法一次添加多个项目。

```
people.update(['Carol', 'Susan']) 
```

请记住，集合不允许重复，所以如果您再次添加“Mary ”,什么都不会改变。

```
people = {'Bob', 'Mary'}

people.add('Mary')

print(people) 
```

```
{'Bob', 'Mary'} 
```

### 在集合中更新

集合中的项目是不可变的。您必须添加或删除一个项目。

### 在集合中删除

要从字典中删除 Bob:

```
people = {'Bob', 'Mary'}
people.remove('Bob')
print(people) 
```

```
{'Mary'} 
```

要删除所有人:

```
people.clear() 
```

### 检查给定项目是否已经存在于集合中

```
people = {'Bob', 'Mary'}

if 'Bob' in people:
  print('Bob exists!')
else:
  print('There is no Bob!') 
```

# 字典

字典不保证元素的顺序，而且它是可变的。

字典的一个重要特征是，您可以为每个元素设置自定义的访问键。

### 字典的初始化

#### 空字典

```
people = {} 
```

#### 具有初始值的字典

```
people = {'Bob':30, 'Mary':25} 
```

### 在字典中添加

如果这个键还不存在，它将被追加到字典中。

```
people['Sarah']=32 
```

### 更新字典

如果该键已经存在，则只更新值。

```
#Bob's age is 28 now
people['Bob']=28 
```

请注意，代码几乎是一样的。

### 在字典中删除

要从字典中删除 Bob:

```
people.pop('Bob') 
```

要删除所有人:

```
people.clear() 
```

### 在字典中检索

```
bob_age = people['Bob']
print(bob_age) 
```

```
30 
```

### 检查给定的键是否已经存在于字典中

```
if 'Bob' in people:
  print('Bob exists!')
else:
  print('There is no Bob!') 
```

# `while`循环

当您需要将一个代码块重复一定次数，或者对集合中的每一项应用相同的逻辑时，可以使用循环。

循环有两种类型:`for`和`while`。

您将在下一节学习`for`循环。

## 基本语法

`while`循环的基本语法如下。

```
while condition:
    statement 
```

当条件为`True`时，循环将继续*。*

## 一个数的平方是

下面的例子取`number`的每个值，并计算其平方值。

```
number = 1
while number <= 5:
    print(number, 'squared is', number**2)
    number = number + 1 
```

```
1 squared is 1
2 squared is 4
3 squared is 9
4 squared is 16
5 squared is 25 
```

您可以使用任何变量名，但是我选择了`number`,因为它在上下文中有意义。一个常见的通用选择是简单的`i`。

循环将继续，直到`number`(初始化为 1)小于或等于 5。

注意在`print()`命令之后，变量`number`增加 1 以获取下一个值。

如果你不做增量，你将有一个无限循环，因为`number`永远不会达到一个大于 5 的值。这是一个非常重要的细节！

## `else`块

当条件返回`False`时，将调用`else`块。

```
number = 1
while number <= 5:
    print(number, 'squared is', number**2)
    number = number + 1
else:
    print('No numbers left!') 
```

```
1 squared is 1
2 squared is 4
3 squared is 9
4 squared is 16
5 squared is 25
No numbers left! 
```

注意短语“没有数字了！”在循环结束后打印，即在条件`number <= 5`评估为`False`后。

## 如何在 Python 中跳出一个`while`循环

只需使用`break`关键字，循环就会停止执行。

```
number = 1
while number <= 5:
    print(number, 'squared is', number**2)
    number = number + 1
    if number == 4:
        break 
```

```
1 squared is 1
2 squared is 4
3 squared is 9 
```

循环正常运行，当`number`达到 4 时，`if`语句计算为`True`，调用`break`命令。这在计算数字 4 和 5 的平方值之前结束了循环。

## 如何在`while`循环中跳过一个项目

`continue`会为你做的。

我必须颠倒`if`语句和`print()`语句的顺序来展示它是如何正常工作的。

```
number = 0
while number < 5:
    number = number + 1
    if number == 4:
        continue
    print(number, 'squared is', number**2) 
```

```
1 squared is 1
2 squared is 4
3 squared is 9
5 squared is 25 
```

程序总是检查 4 是否是`number`的当前值。如果是，则不会计算 4 的平方，并且当`number`的值为 5 时，`continue`将跳到下一次迭代。
*

# `for`循环

`for`循环与`while`循环相似，都是用来重复代码块的。

最重要的区别是，您可以轻松地迭代顺序类型。

## 基本语法

`for`循环的基本语法如下。

```
for item in collection:
    statement 
```

## 循环遍历列表

要遍历列表或任何其他集合，只需按照下面的示例进行操作。

```
cars = ['BMW', 'Ferrari', 'McLaren']
for car in cars:
    print(car) 
```

```
BMW
Ferrari
McLaren 
```

`cars`列表包含三项。`for`循环将遍历列表并将每一项存储在`car`变量中，然后执行一条语句，在本例中是`print(car)`，在控制台中打印每辆汽车。

## `range()`功能

range 函数在 for 循环中广泛使用，因为它提供了一种简单的方法来列出数字。

这段代码将遍历数字 0 到 5，并打印出每一个数字。

```
for number in range(5):
    print(number) 
```

```
0
1
2
3
4 
```

相比之下，如果没有`range()`函数，我们会这样做。

```
numbers = [0, 1, 2, 3, 4]
for number in numbers:
    print(number) 
```

```
0
1
2
3
4 
```

您也可以使用`range()`定义一个`start`和`stop`。

这里我们 5 分钟后开始，10 分钟后停止。您设置停止的号码不包括在内。

```
for number in range(5, 10):
    print(number) 
```

```
5
6
7
8
9 
```

最后，也可以设置一个步骤。

这里我们从 10 开始，递增 2 直到 20，因为 20 是`stop`，所以不包括它。

```
for number in range(10, 20, 2):
    print(number) 
```

```
10
12
14
16
18 
```

## `else`块

当列表中的项目结束时，将调用`else`块。

```
cars = ['BMW', 'Ferrari', 'McLaren']
for car in cars:
    print(car)
else:
    print('No cars left!') 
```

```
BMW
Ferrari
McLaren
No cars left! 
```

## 如何在 Python 中跳出 for 循环

只需使用`break`关键字，循环就会停止执行。

```
cars = ['BMW', 'Ferrari', 'McLaren']
for car in cars:
    print(car)
    if car == 'Ferrari':
        break 
```

```
BMW
Ferrari 
```

该循环将遍历列表并打印每辆汽车。

在这种情况下，循环到达'法拉利'后，调用`break`，不会打印'迈凯轮'。

## 如何跳过 for 循环中的一项

在本节中，我们将了解`continue`如何为您做到这一点。

我必须颠倒`if`语句和`continue`语句的顺序来展示它是如何正常工作的。

请注意，我总是检查“法拉利”是否是当前项目。如果是，则不会打印“法拉利”，并且`continue`会跳到下一项“迈凯轮”。

```
cars = ['BMW', 'Ferrari', 'McLaren']
for car in cars:
    if car == 'Ferrari':
        continue
    print(car) 
```

```
BMW
McLaren 
```

## 循环上的循环:嵌套循环

有时候你有更复杂的集合，比如一个列表的列表。

要遍历这些列表，需要嵌套的`for`循环。

在这种情况下，我有三个列表:一个是宝马车型，另一个是法拉利车型，最后一个是迈凯轮车型。

第一个循环遍历每个品牌的列表，第二个循环遍历每个品牌的模型。

```
car_models = [ 
['BMW I8', 'BMW X3', 
'BMW X1'], 
['Ferrari 812', 'Ferrari F8', 
'Ferrari GTC4'], 
['McLaren 570S', 'McLaren 570GT', 
'McLaren 720S']
]

for brand in car_models:
    for model in brand:
        print(model) 
```

```
BMW I8
BMW X3
BMW X1
Ferrari 812
Ferrari F8
Ferrari GTC4
McLaren 570S
McLaren 570GT
McLaren 720S 
```

## 在其他数据结构上循环

在`list`上应用`for`循环的相同逻辑可以扩展到其他数据结构:`tuple`、`set`和`dictionary`。

我将简要演示如何对它们中的每一个进行基本循环。

### 在元组上循环

```
people = ('Bob', 'Mary')

for person in people:
  print(person) 
```

```
Bob
Mary 
```

### 在集合上循环

```
people = {'Bob', 'Mary'}

for person in people:
  print(person) 
```

```
Bob
Mary 
```

### 循环查阅字典

要打印密钥:

```
people = {'Bob':30, 'Mary':25}

for person in people:
  print(person) 
```

```
Bob
Mary 
```

要打印这些值:

```
people = {'Bob':30, 'Mary':25}

for person in people:
  print(people[person]) 
```

```
30
25 
```

# 功能

随着代码的增长，复杂性也在增长。和函数帮助组织代码。

函数是创建可以重用的代码块的便捷方式。

## 定义和召唤

在 Python 中使用`def`关键字来定义函数。

给它一个名称，并用括号来表示 0 个或多个参数。

在声明开始后的行中，记住缩进代码块。

下面是一个名为`print_first_function()`的函数的例子，它只打印一个短语‘我的第一个函数！’。

要调用该函数，只需使用其定义的名称。

```
def print_first_function():
    print('My first function!')

print_first_function() 
```

```
My first function! 
```

## `return`一个值

使用`return`关键字从函数中返回值。

在这个例子中，函数`second_function()`返回字符串“我的第二个函数！”。

注意`print()`是一个内置函数，我们的函数是从内部调用的。

由`second_function()`返回的字符串作为参数传递给`print()`函数。

```
def second_function():
    return 'My second function!'

print(second_function()) 
```

```
My second function! 
```

## `return`多个值

函数也可以一次返回多个值。

`return_numbers()`同时返回两个值。

```
def return_numbers():
    return 10, 2

print(return_numbers()) 
```

```
(10, 2) 
```

## 争论

您可以在括号之间定义参数。

当调用带参数的函数时，你必须根据定义的参数传递参数。

过去的例子没有参数，所以不需要参数。当函数被调用时，括号保持为空。

### 一个论点

要指定一个参数，只需在括号内定义它。

在这个例子中，函数`my_number`期望一个数字作为由参数`num`定义的自变量。

然后，可以在要使用的函数内部访问参数的值。

```
def my_number(num):
    return 'My number is: ' + str(num)

print(my_number(10)) 
```

```
My number is: 10 
```

### 两个或更多参数

要定义更多的参数，只需用逗号分隔即可。

这里我们有一个将两个数相加的函数叫做`add`。它需要由`first_num`和`second_num`定义的两个参数。

参数由`+`操作符添加，然后结果由`return`返回。

```
def add(first_num, second_num):
    return first_num + second_num

print(add(10,2)) 
```

```
12 
```

这个例子和上一个很像。唯一的区别是我们有 3 个参数，而不是 2 个。

```
def add(first_num, second_num, third_num):
    return first_num + second_num + third_num

print(add(10,2,3)) 
```

```
15 
```

这种定义参数和传递参数的逻辑对于任意数量的参数都是相同的。

需要指出的是，参数的传递顺序必须与参数的定义顺序相同。

### 默认值。

如果没有给定参数，可以使用`=`操作符和一个选择值来设置参数的默认值。

在这个函数中，如果没有给定参数，默认情况下，数字 30 被假定为期望值。

```
def my_number(my_number = 30):
    return 'My number is: ' + str(my_number)

print(my_number(10))
print(my_number()) 
```

```
My number is: 10
My number is: 30 
```

### 关键字或命名参数

调用函数时，参数的顺序必须与形参的顺序相匹配。

另一种方法是使用关键字或命名参数。

使用参数名和`=`操作符直接将参数设置为各自的参数。

这个例子翻转了参数，但是函数像预期的那样工作，因为我通过名字告诉它哪个值属于哪个参数。

```
def my_numbers(first_number, second_number):
    return 'The numbers are: ' + str(first_number) + ' and ' + str(second_number)

print(my_numbers(second_number=30, first_number=10)) 
```

```
The numbers are: 10 and 30 
```

### 任意数量的参数:`*args`

如果不想指定参数的个数，只需在参数名前使用`*`即可。那么函数将根据需要接受尽可能多的参数。

参数名可以是类似于`*numbers`的任何东西，但是 Python 中有一个约定，使用`*args`来定义数量可变的参数。

```
def my_numbers(*args):
    for arg in args:
        print(number)

my_numbers(10,2,3) 
```

```
10
2
3 
```

### 任意数量的关键字/命名参数:`**kwargs`

类似于`*args`，我们可以使用`**kwargs`传递任意多的关键字参数，只要我们使用`**`。

同样，这个名字可以是类似于`**numbers`的任何东西，但是`**kwargs`是一个约定。

```
def my_numbers(**kwargs):
    for key, value in kwargs.items():
        print(key)
        print(value)

my_numbers(first_number=30, second_number=10) 
```

```
first_number
30
second_number
10 
```

### 作为参数的其他类型

过去的例子主要使用数字，但是你可以传递任何类型作为参数，它们在函数中也会被这样处理。

此示例将字符串作为参数。

```
def my_sport(sport):
    print('I like ' + sport)

my_sport('football')
my_sport('swimming') 
```

```
I like football
I like swimming 
```

这个函数接受一个列表作为参数。

```
def my_numbers(numbers):
    for number in numbers:
        print(number)

my_numbers([30, 10, 64, 92, 105]) 
```

```
30
10
64
92
105 
```

# 范围

变量的创建位置定义了它是否可以被代码的其余部分访问和操作。这就是所谓的**范围**。

有两种类型的作用域:局部和全局。

## 全球范围

全局范围允许你在程序的任何地方使用变量。

如果您的变量在函数之外，默认情况下它具有全局范围。

```
name = 'Bob'

def print_name():
  print('My name is ' + name)

print_name() 
```

```
My name is Bob 
```

注意，该函数可以使用变量`name`并打印`My name is Bob`。

## 局部范围

当你在函数内部声明一个变量时，它只存在于函数内部，不能从外部访问。

```
def print_name():
	name = "Bob"
	print('My name is ' + name)

print_name() 
```

```
My name is Bob 
```

变量`name`是在函数内部声明的，所以输出和以前一样。

但是这将抛出一个错误:

```
def print_name():
	name = 'Bob'
	print('My name is ' + name)

print(name) 
```

上面代码的输出是:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'name' is not defined 
```

我试图从函数外部打印变量`name`，但是该变量的作用域是局部的，在全局范围内找不到。

## 混合范围

如果在函数内部和外部使用相同的变量名称，函数将使用其作用域内的名称。

所以当你调用`print_name()`时，`name='Bob'`用来打印短语。

另一方面，当在函数作用域外调用`print()`时，`name="Sarah"`因其全局作用域而被使用。

```
name = "Sarah"

def print_name():
	name = 'Bob'
	print('My name is ' + name)

print_name()

print(name) 
```

上面代码的输出是:

```
My name is Bob

Sarah 
```

# 列出理解

有时我们想对列表中的项目执行一些非常简单的操作。

列表理解为我们提供了一种简洁的方式来处理列表，作为其他迭代方法的替代，比如`for`循环。

## 基本语法

要使用列表理解来替换常规的 for 循环，我们可以:

```
[expression for item in list] 
```

这相当于:

```
for item in list:
    expression 
```

如果我们要一些有条件的表达式来应用，我们有:

```
[expression for item in list if conditional ] 
```

这相当于:

```
for item in list:
    if conditional:
        expression 
```

## 示例 1:计算数字的立方

**常规方式**

```
numbers = [1, 2, 3, 4, 5]
new_list = []

for n in numbers:
    new_list.append(n**3)

print(new_list) 
```

```
[1, 8, 27, 64, 125] 
```

**使用列表理解**

```
numbers = [1, 2, 3, 4, 5]
new_list = []

new_list = [n**3 for n in numbers]

print(new_list) 
```

```
 [1, 8, 27, 64, 125] 
```

## 示例 2:仅计算大于 3 的数字的立方

使用条件，我们可以只过滤我们希望表达式应用到的值。

**常规方式**

```
numbers = [1, 2, 3, 4, 5]
new_list = []

for n in numbers:
    if(n > 3):
        new_list.append(n**3)

print(new_list) 
```

```
[64, 125] 
```

**使用列表理解**

```
numbers = [1, 2, 3, 4, 5]
new_list = []

new_list = [n**3 for n in numbers if n > 3]

print(new_list) 
```

```
[64, 125] 
```

## 示例 3:用列表理解调用函数

我们也可以使用列表理解语法调用函数:

```
numbers = [1, 2, 3, 4, 5]
new_list = []

def cube(number):
    return number**3

new_list = [cube(n) for n in numbers if n > 3]

print(new_list) 
```

```
[64, 125] 
```

# λ函数

Python lambda 函数只能有一个表达式，不能有多行。

它应该使在一行中创建一些小逻辑变得更容易，而不是像通常那样创建一个完整的函数。

Lambda 函数也是匿名的，这意味着不需要给它们命名。

## 基本语法

基本语法非常简单:只需使用`lambda`关键字，定义所需的参数，并使用“:”将参数与表达式分开。

一般的形式是:

```
lambda arguments : expression 
```

### 一个参数示例

看看这个只使用一个参数的例子

```
cubic = lambda number : number**3
print(cubic(2)) 
```

```
8 
```

### 多参数示例

如果你愿意，你也可以有多个参数。

```
exponential = lambda multiplier, number, exponent : multiplier * number**exponent
print(exponential(2, 2, 3)) 
```

```
16 
```

### 直接调用 Lambda 函数

你不需要像我们之前那样使用变量。相反，您可以在 lambda 函数周围使用括号，在参数周围使用另一对括号。

函数的声明和执行将发生在同一行。

```
(lambda multiplier, number, exponent : multiplier * number**exponent)(2, 2, 3) 
```

```
16 
```

## 使用 lambda 函数和其他内置函数的示例

### 地图

Map 函数将表达式应用于列表中的每个项目。

让我们计算列表中每个数字的立方。

```
numbers = [2, 5, 10]
cubics = list(map(lambda number : number**3, numbers))
print(cubics) 
```

```
[8, 125, 1000] 
```

### 过滤器

Filter 函数将根据表达式过滤列表。

让我们筛选出大于 5 的数字。

```
numbers = [2, 5, 10]
filtered_list = list(filter(lambda number : number > 5, numbers))
print(filtered_list) 
```

```
[10] 
```

# 模块

一段时间后，你的代码开始变得更加复杂，包含了大量的函数和变量。

为了更容易组织代码，我们使用模块。

一个设计良好的模块还具有可重用的优势，因此您只需编写一次代码，就可以在任何地方重用它。

你可以写一个包含所有数学运算的模块，其他人可以使用它。

而且，如果你需要，你可以使用别人的模块来简化你的代码，加速你的项目。

在其他编程语言中，这些也称为库。

## 使用模块

要使用一个模块，我们使用`import`关键字。

顾名思义，我们必须告诉程序要导入什么模块。

之后，我们可以使用该模块中的任何可用函数。

让我们看一个使用`math`模块的例子。

首先，让我们看看如何获得一个常数，欧拉数。

```
import math

math.e 
```

```
2.718281828459045 
```

在第二个例子中，我们将使用一个函数来计算一个数的平方根。

也可以使用`as`关键字创建别名。

```
import math as m

m.sqrt(121)

m.sqrt(729) 
```

```
11
27 
```

最后，使用`from`关键字，我们可以精确地指定导入什么而不是整个模块，并直接使用函数而不用模块名。

这个例子使用了`floor()`函数，该函数返回小于或等于给定数字的最大整数。

```
from math import floor

floor(9.8923) 
```

```
9 
```

## 创建模块

现在我们知道了如何使用模块，让我们看看如何创建一个模块。

这将是一个具有基本数学运算`add`、`subtract`、`multiply`、`divide`的模块，它将被称为`basic_operations`。

用四个函数创建`basic_operations.py`文件。

```
def add(first_num, second_num):
    return first_num + second_num

def subtract(first_num, second_num):
    return first_num - second_num

def multiply(first_num, second_num):
    return first_num * second_num

def divide(first_num, second_num):
    return first_num / second_num 
```

然后，只需导入`basic_operations`模块并使用函数。

```
import basic_operations

basic_operations.add(10,2)
basic_operations.subtract(10,2)
basic_operations.multiply(10,2)
basic_operations.divide(10,2) 
```

```
12
8
20
5.0 
```

# `if __name__ == '__main__'`

您正在使用保存在`basic_operations.py`文件中名为`basic_operations`的基本数学运算`add`、`subtract`、`multiply`和`divide`构建一个模块。

为了保证一切正常，你可以做一些测试。

```
def add(first_num, second_num):
    return first_num + second_num

def subtract(first_num, second_num):
    return first_num - second_num

def multiply(first_num, second_num):
    return first_num * second_num

def divide(first_num, second_num):
    return first_num / second_num

print(add(10, 2)) 
print(subtract(10,2))
print(multiply(10,2))
print(divide(10,2)) 
```

运行代码后:

```
renan@pro-home:~$ python3 basic_operations.py 
```

输出是:

```
12
8
20
5.0 
```

这些测试的输出是我们所期望的。

我们的代码是正确的，并准备分享。

让我们导入新模块，并在 Python 控制台中运行它。

```
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import basic_operations
12
8
20
5.0
>>> 
```

当模块被导入时，我们的测试显示在屏幕上，即使我们除了导入`basic_operations`之外没有做任何事情。

为了解决这个问题，我们像这样在`basic_operations.py`文件中使用`if __name__ == '__main__'`:

```
def add(first_num, second_num):
    return first_num + second_num

def subtract(first_num, second_num):
    return first_num - second_num

def multiply(first_num, second_num):
    return first_num * second_num

def divide(first_num, second_num):
    return first_num / second_num

if __name__ == '__main__':
    print(add(10, 2)) 
    print(subtract(10,2))
    print(multiply(10,2))
    print(divide(10,2)) 
```

直接在终端上运行代码将继续显示测试。但是当你像导入一个模块一样导入它时，测试不会显示出来，你可以按照你想要的方式使用这些函数。

```
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import basic_operations
>>> basic_operations.multiply(10,2)
20
>>> 
```

现在你知道如何使用`if __name__ == '__main__'`，让我们了解它是如何工作的。

条件告诉解释器何时将代码视为一个模块或一个直接执行的程序本身。

Python 有一个名为`__name__`的本地变量。

该变量代表模块的名称，即`.py`文件的名称。

用下面的代码创建一个文件`my_program.py`并执行它。

```
print(__name__) 
```

输出将是:

```
__main__ 
```

这告诉我们，当一个程序被直接执行时，变量`__name__`被定义为`__main__`。

但是当它作为模块导入时，`__name__`的值就是模块的名称。

```
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import my_program
my_program
>>> 
```

这就是 Python 解释器如何区分导入的模块和直接在终端上执行的程序的行为。

# 文件

创建、删除、读取和许多其他应用于文件的功能是许多程序不可或缺的一部分。

因此，知道如何直接从代码中组织和处理文件非常重要。

让我们看看如何在 Python 中处理文件。

## 文件创建

首先，创造！

我们将使用`open()`函数。

这个函数打开一个文件并返回它对应的对象。

第一个参数是我们正在处理的文件的名称，第二个参数是我们正在使用的操作。

下面的代码创建了文件“people.txt ”,当我们只想创建该文件时使用了`x`参数。如果同名文件已经存在，它将抛出一个异常。

```
people_file = open("people.txt", "x") 
```

您也可以使用`w`模式创建文件。与`x`模式不同，它不会抛出异常，因为该模式表示*写*模式。我们正在打开一个文件向其中写入数据，如果文件不存在，就会创建一个。

```
people_file = open("people.txt", "w") 
```

最后一个是`a`模式，代表*追加*。顾名思义，您可以向文件追加更多的数据，而`w`模式只是简单地覆盖任何现有的数据。

追加时，如果文件不存在，它也会创建它。

```
people_file = open("people.txt", "a") 
```

## 文件写入

要将数据写入文件，只需用`w`模式打开文件。

然后，为了添加数据，使用由`open()`函数返回的对象。在这种情况下，对象被称为`people_file`。然后调用 *write()* 函数，将数据作为参数传递。

```
people_file = open("people.txt", "w")
people_file.write("Bob\n")
people_file.write("Mary\n")
people_file.write("Sarah\n")
people_file.close() 
```

我们在末尾使用`\n`断行，否则文件中的内容会和“BobMarySarah”留在同一行。

还有一个细节就是*关闭()*文件。这不仅是一个好的做法，而且还能确保您的更改被应用到文件中。

记住，当使用`w`模式时，文件中已经存在的数据将被新数据覆盖。为了在不丢失现有数据的情况下添加新数据，我们必须使用 append 模式。

## 文件追加

`a`模式将新数据添加到文件中，保留现有数据。

在本例中，在使用`w`模式进行第一次写入后，我们使用`a`模式进行追加。结果是每个名字都会在文件“people.txt”中出现两次。

```
#first write
people_file = open("people.txt", "w")
people_file.write("Bob\n")
people_file.write("Mary\n")
people_file.write("Sarah\n")
people_file.close()

#appending more data
#keeping the existing data
people_file = open("people.txt", "a")
people_file.write("Bob\n")
people_file.write("Mary\n")
people_file.write("Sarah\n")
people_file.close() 
```

## 文件读取

读取文件也非常简单:只需像这样使用`r`模式。

如果您阅读上一个示例中创建的“people.txt”文件，您应该在输出中看到 6 个名字。

```
people_file = open("people.txt", "r")
print(people_file.read()) 
```

```
Bob
Mary
Sarah
Bob
Mary
Sarah 
```

`read()`函数一次读取整个文件。如果使用`readline()`函数，可以逐行读取文件。

```
people_file = open("people.txt", "r")
print(people_file.readline())
print(people_file.readline())
print(people_file.readline()) 
```

```
Bob
Mary
Sarah 
```

您也可以像下面的例子一样循环读取这些行。

```
people_file = open("people.txt", "r")
for person in people_file:
  print(person) 
```

```
Bob
Mary
Sarah
Bob
Mary
Sarah 
```

## 删除文件

要删除一个文件，还需要`os`模块。

使用`remove()`方法。

```
import os

os.remove('my_file.txt') 
```

## 检查文件是否存在

使用`os.path.exists()`方法检查文件是否存在。

```
import os

if os.path.exists('my_file.txt'):
  os.remove('my_file.txt')
else:
  print('There is no such file!') 
```

## 复制文件

对于这一个，我喜欢使用来自`shutil`模块的`copyfile()`方法。

```
from shutil import copyfile

copyfile('my_file.txt','another_file.txt') 
```

复制一个文件有几个选项，但是`copyfile()`是最快的一个。

## 重命名并移动文件

如果需要移动或重命名文件，可以使用`shutil`模块中的`move()`方法。

```
from shutil import move

move('my_file.txt','another_file.txt') 
```

# 类别和对象

类和对象是面向对象编程的基本概念。

在 Python 中，**一切都是对象**！

变量(对象)只是其类型(类)的一个实例。

这就是为什么当你检查一个变量的类型时，你可以在它的类型(类)旁边看到`class`关键字。

这段代码表明`my_city`是一个对象，并且是类`str`的一个实例。

```
my_city = "New York"
print(type(my_city)) 
```

```
<class 'str'> 
```

## 区分 x 类对象

类为您提供了创建对象的标准方法。一个类就像一个基础项目。

假设你是一名在波音公司工作的工程师。

你的新任务是为公司制造新产品，一种叫做 747-Space 的新型号。这种飞机比其他商业机型飞得更高。

波音公司需要制造几十架这样的飞机卖给世界各地的航空公司，而且飞机必须完全一样。

为了保证飞行器(对象)遵循相同的标准，你需要一个可复制的项目(类)。

类是一个项目，一个对象的蓝图。

这样，您只需制作一次项目，就可以多次重复使用。

在我们之前的代码示例中，考虑每个字符串都有相同的行为和相同的属性。所以只有用类`str`来定义字符串才有意义。

## 属性和方法

对象有一些由属性和方法赋予的行为。

简单地说，在对象的上下文中，属性是变量，方法是附属于对象的函数。

例如，一个字符串有许多我们可以使用的内置方法。

它们像函数一样工作，你只需要用一个`.`把它们从对象中分离出来。

在这段代码中，我从字符串变量`my_city`调用了`replace()`方法，该变量是一个对象，也是类`str`的一个实例。

`replace()`方法将字符串的一部分替换为另一部分，并返回一个有变化的新字符串。原始字符串保持不变。

让我们用“New”代替“New York”中的“Old”。

```
my_city = 'New York'
print(my_city.replace('New', 'Old'))
print(my_city) 
```

```
Old York
New York 
```

## 创建一个类

我们使用了许多对象(类的实例),如字符串、整数、列表和字典。它们都是 Python 中预定义类的实例。

为了创建我们自己的类，我们使用了`class`关键字。

按照惯例，类的名称与`.py`文件和模块的名称相匹配。组织代码也是一个很好的实践。

用下面的类`Vehicle`创建一个文件`vehicle.py`。

```
class Vehicle:
    def __init__(self, year, model, plate_number, current_speed = 0):
        self.year = year
        self.model = model
        self.plate_number = plate_number
        self.current_speed = current_speed

    def move(self):
        self.current_speed += 1

    def accelerate(self, value):
        self.current_speed += value

    def stop(self):
        self.current_speed = 0

    def vehicle_details(self):
        return self.model + ', ' + str(self.year) + ', ' + self.plate_number 
```

让我们把课程分成几部分来解释。

`class`关键字用于指定类`Vehicle`的名称。

`__init__`函数是所有类都有的内置函数。它在创建对象时被调用，通常用于初始化属性，给它们赋值，类似于对变量所做的。

`__init__`函数中的第一个参数`self`是对对象(实例)本身的引用。我们习惯上称它为`self`，它必须是每个实例方法的第一个参数，正如你在其他方法定义`def move(self)`、`def accelerate(self, value)`、`def stop(self)`和`def vehicle_details(self)`中看到的。

`Vehicle`有 5 个属性(包括自身):`year`、`model`、`plate_number`、`current_speed`。

在`__init__`中，它们中的每一个都用对象实例化时给出的参数初始化。

注意，默认情况下，`current_speed`用`0`初始化，这意味着如果没有给定值，当对象第一次被实例化时，`current_speed`将等于 0。

最后，我们有三种方法来控制我们的车辆的速度:`def move(self)`、`def accelerate(self, value)`和`def stop(self)`。

以及一种返回车辆信息的方法:`def vehicle_details(self)`。

方法内部的实现与函数中的实现方式相同。你也可以用一个`return`在方法结束时返回一些值，如`def vehicle_details(self)`所示。

## 使用类

要在您的终端中使用该类，请从`vehicle`模块中导入`Vehicle`类。

创建一个名为`my_car`的实例，用 2009 初始化`year`，用‘F8’初始化`model`，用 ABC1234 初始化`plate_number`，用 100 初始化`current_speed`。

调用方法时不考虑`self`参数。Python 解释器自动将其值推断为当前对象/实例，因此我们只需在实例化和调用方法时传递其他参数。

现在使用这些方法来`move()`汽车，它将它的`current_speed`增加 1，`accelerate(10)`将它的`current_speed`增加参数中给定的值，`stop()`将`current_speed`设置为 0。

记住在每个命令中打印出`current_speed`的值来查看变化。

要完成测试，请调用`vehicle_details()`打印我们车辆的信息。

```
>>> from vehicle import Vehicle
>>>
>>> my_car = Vehicle(2009, 'F8', 'ABC1234', 100)
>>> print(my_car.current_speed)
100
>>> my_car.move()
>>> print(my_car.current_speed)
101
>>> my_car.accelerate(10)
>>> print(my_car.current_speed)
111
>>> my_car.stop()
>>> print(my_car.current_speed)
0
>>> print(my_car.vehicle_details())
F8, 2009, ABC1234 
```

如果我们不为`current_speed`设置初始值，它将默认为零，如前所述，并将在下一个示例中演示。

```
>>> from vehicle import Vehicle
>>>
>>> my_car = Vehicle(2009, 'F8', 'ABC1234')
>>> print(my_car.current_speed)
0
>>> my_car.move()
>>> print(my_car.current_speed)
1
>>> my_car.accelerate(10)
>>> print(my_car.current_speed)
11
>>> my_car.stop()
>>> print(my_car.current_speed)
0
>>> print(my_car.vehicle_details())
F8, 2009, ABC1234 
```

# 遗产

让我们定义一个通用的`Vehicle`类，并将其保存在`vehicle.py`文件中。

```
class Vehicle:
    def __init__(self, year, model, plate_number, current_speed):
        self.year = year
        self.model = model
        self.plate_number = plate_number
        self.current_speed = current_speed

    def move(self):
        self.current_speed += 1

    def accelerate(self, value):
        self.current_speed += value

    def stop(self):
        self.current_speed = 0

    def vehicle_details(self):
        return self.model + ', ' + str(self.year) + ', ' + self.plate_number 
```

一辆车有属性`year`、`model`、`plate_number`和`current_speed`。

例如，`Vehicle`中的车辆定义非常通用，可能不适合卡车，因为它应该包含一个`cargo`属性。

另一方面，货物属性对于像摩托车这样的小型车辆没有太大的意义。

为了解决这个问题，我们可以使用*继承*。

当一个类(子类)继承另一个类(父类)时，父类的所有属性和方法都会被子类继承。

## 父母和孩子

在我们的例子中，我们希望一个新的`Truck`类继承`Vehicle`类的所有内容。然后，我们希望它添加自己的特定属性`current_cargo`来控制卡车货物的添加和移除。

`Truck`类被称为*子*类，它继承自其*父*类`Vehicle`。

一个*父*类也被称为*超类*，而一个*子*类也被称为*子类*。

创建类`Truck`并将其保存在`truck.py`文件中。

```
from vehicle import Vehicle

class Truck(Vehicle):
    def __init__(self, year, model, plate_number, current_speed, current_cargo):
        super().__init__(year, model, plate_number, current_speed)
        self.current_cargo = current_cargo

    def add_cargo(self, cargo):
        self.current_cargo += cargo

    def remove_cargo(self, cargo):
        self.current_cargo -= cargo 
```

让我们把课程分成几部分来解释。

定义类`Truck`时括号内的类`Vehicle`表示*父* `Vehicle`正被其子`Truck`继承。

像往常一样，`__init__`方法的第一个参数是`self`。

参数`year`、`model`、`plate_number`和`current_speed`与`Vehicle`类中的参数相匹配。

我们添加了一个适合于`Truck`类的新参数`current_cargo`。

在`Truck`类的`__init__`方法的第一行，我们必须调用`Vehicle`类的`__init__`方法。

为此，我们使用`super()`来引用*的超类* `Vehicle`，因此当`super().__init__(year, model, plate_number, current_speed)`被调用时，我们避免代码重复。

之后我们就可以正常赋值`current_cargo`的值了。

最后，我们有两种方法来处理`current_cargo` : `def add_cargo(self, cargo):`和`def remove_cargo(self, cargo):`。

记住`Truck`从`Vehicle`继承属性和方法，所以我们也可以隐式访问控制速度的方法:`def move(self)`、`def accelerate(self, value)`和`def stop(self)`。

## 使用`Truck`类

要在您的终端中使用该类，请从`truck`模块中导入`Truck`类。

创建一个名为`my_truck`的实例，用 2015 初始化`year`，用‘V8’初始化`model`，用‘XYZ 1234’初始化`plate_number`，用 0 初始化`current_speed`，用 0 初始化`current_cargo`。

使用`add_cargo(10)`将`current_cargo`增加 10，`remove_cargo(4)`，将`current_cargo`减少 4。

记住在每个命令中打印出`current_cargo`的值来查看变化。

通过继承，我们可以使用从`Vehicle`类到`move()`卡车的方法，将`current_speed`增加 1，`accelerate(10)`将`current_speed`增加参数中给定的值，以及`stop()`将`current_speed`设置为 0。

记住在每次交互时打印`current_speed`的值，以查看变化。

为了完成测试，调用从`Vehicle`类继承的`vehicle_details()`来打印关于我们卡车的信息。

```
>>> from truck import Truck
>>> 
>>> my_truck = Truck(2015, 'V8', 'XYZ1234', 0, 0)
>>> print(my_truck.current_cargo)
0
>>> my_truck.add_cargo(10)
>>> print(my_truck.current_cargo)
10
>>> my_truck.remove_cargo(4)
>>> print(my_truck.current_cargo)
6
>>> print(my_truck.current_speed)
0
>>> my_truck.accelerate(10)
>>> print(my_truck.current_speed)
10
>>> my_truck.stop()
>>> print(my_truck.current_speed)
0
>>> print(my_truck.vehicle_details())
V8, 2015, XYZ1234 
```

# 例外

错误是每个程序员生活的一部分，知道如何处理它们本身就是一种技能。

Python 处理错误的方式被称为“异常处理”。

如果某段代码出错，Python 解释器会*抛出*异常。

## 例外的类型

让我们试着故意抛出一些异常，看看它们产生的错误。

*   `TypeError`

首先，尝试添加一个字符串和一个整数

```
'I am a string' + 32 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: must be str, not int 
```

*   `IndexError`

现在，尝试访问一个不存在于列表中的索引。

一个常见的错误是忘记序列的索引是 0，这意味着第一项的索引是 0，而不是 1。

在这个例子中，列表`car_brands`在索引 2 处结束。

```
car_brands = ['ford', 'ferrari', 'bmw']
print(car_brands[3]) 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range 
```

*   `NameError`

如果我们试图打印一个不存在的变量:

```
print(my_variable) 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'my_variable' is not defined 
```

*   `ZeroDivisionError`

数学不允许被零除，所以尝试这样做将会引发错误，这是意料之中的。

```
32/0 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero 
```

这只是您在日常工作中可能会看到的各种异常的一个示例，以及导致这些异常的原因。

## 异常处理

现在我们知道了如何引起错误，使我们的代码崩溃，并向我们显示一些消息说有什么地方出错了。

要处理这些异常，只需使用`try/except`语句。

```
try:
  32/0
except:
  print('Dividing by zero!') 
```

```
Dividing by zero! 
```

上面的例子显示了`try`语句的用法。

将可能导致异常的代码块放在`try`范围内。如果一切运行正常，就不会调用`except`块。但是，如果出现异常，就会执行`except`中的代码块。

这样程序就不会崩溃，如果你在异常后有一些代码，如果你希望它继续运行，它会继续运行。

## 特定异常处理

在最后一个例子中，`except`块是通用的，这意味着它可以捕捉任何东西。

很好的做法是指定我们试图捕捉的异常的类型，这在以后调试代码时很有帮助。

如果你知道一个代码块可以抛出一个`IndexError`，在`except`中指定它:

```
try:
  car_brands = ['ford', 'ferrari', 'bmw']
  print(car_brands[3])
except IndexError:
  print('There is no such index!') 
```

```
There is no such index! 
```

您可以使用 tuple 在单个`except`中指定任意多的异常类型:

```
try:
  print('My code!')
except(IndexError, ZeroDivisionError, TypeError):
  print('My Excepetion!') 
```

## `else`

可以在`try/except`的末尾添加一个`else`命令。只有在没有异常的情况下，它才会运行。

```
my_variable = 'My variable'
try:
  print(my_variable)
except NameError:
  print('NameError caught!')
else:
  print('No NameError') 
```

```
My variable
No NameError 
```

## 引发异常

`raise`命令允许您手动引发异常。

如果您想捕获一个异常并对它做一些事情，比如以某种个性化的方式记录错误，比如将它重定向到一个日志聚集器，或者结束代码的执行，因为错误不应该允许程序进行。

```
try:
  raise IndexError('This index is not allowed')
except:
  print('Doing something with the exception!')
  raise 
```

```
Doing something with the exception!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
IndexError: This index is not allowed 
```

## `finally`

无论是否出现异常，都会执行`finally`块。

它们通常允许程序清理文件、内存、网络连接等资源。

```
try:
  print(my_variable)
except NameError:
  print('Except block')
finally:
  print('Finally block') 
```

```
Except block
Finally block 
```

# 结论

就是这样！

祝贺你到达终点。

我想感谢你阅读这篇文章。

如果你想了解更多，请查看我的博客[renanmf.com](https://renanmf.com)。

记得[下载这个 Python 初学者指南的 PDF 版本](https://renanmf.com/python-guide-beginners/)。

也可以在推特上找我: [@renanmouraf](https://twitter.com/renanmouraf) 。