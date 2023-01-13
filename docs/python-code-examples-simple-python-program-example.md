# Python 代码示例手册——面向初学者的简单 Python 程序示例

> 原文：<https://www.freecodecamp.org/news/python-code-examples-simple-python-program-example/>

Python 是一种高级的通用解释编程语言。众所周知，它非常容易学习，但功能强大，在许多不同的领域有许多用途。

如果你是一个试图开始学习 Python 的人，很容易迷失在互联网上所有优秀的学习资源中。

这篇文章并不想成为那群人中的另一个。相反，在这里我将向您介绍 Python 的基础知识，并为您指出正确的方向。

在本文中，我将借助大量代码示例介绍 Python 编程语言的基础。我将非常详细地解释它们，并包括进一步研究的链接。

一旦我向你介绍了这门语言的基础知识，我会向你推荐一些优秀的学习资源，并解释如何充分利用它们。

虽然我将彻底解释代码示例，但我假设您熟悉常见的编程概念，如表达式、语句、变量、函数等等。因此，我不会花时间详细解释这些编程概念——而是将重点放在 Python 实现/使用它们的方式上。

事不宜迟，让我们开始吧！

## 目录

*   [Python 的高级概述](#high-level-overview-of-python)
*   [怎么写你好，世界！在 Python 中](#how-to-write-hello-world-in-python)
*   [Python 中的变量](#variables-in-python)
*   [Python 中的数据类型](#data-types-in-python)
*   [如何用 Python 写注释](#how-to-write-comments-in-python)
*   [Python 中的字符串](#strings-in-python)
*   [Python 中的数字](#numbers-in-python)
*   [如何在 Python 中处理用户输入](#how-to-handle-user-input-in-python)
*   [Python 中的 if-else-elif](#if-else-elif-in-python)
*   [Python 中的匹配用例](#match-case-in-python)
*   [Python 中的列表和元组](#lists-and-tuples-in-python)
*   [Python 中的循环](#loops-in-python)
*   [Python 中的字典](#dictionaries-in-python)
*   [Python 中的函数](#functions-in-python)
*   [额外的 Python 学习资源](#additional-python-learning-resources)
*   [结论](#conclusion)

## Python 的高级概述

在我开始编码之前，您需要安装 Python，并准备好在您的系统上运行。根据您运行的系统——Windows、macOS 或 Linux——安装过程会有所不同。

如果你在 Windows 上，我的 freeCodeCamp 作者同事 [Md. Fahim Bin Amin](https://www.freecodecamp.org/news/author/fahimbinamin/) 写了一篇关于[如何在 Windows 上安装 Python 的极好的指南](https://www.freecodecamp.org/news/how-to-install-python-in-windows-operating-system/)。另一位作者 [Dillion Megida](https://www.freecodecamp.org/news/author/dillionmegida/) 写了另一篇关于[如何在 Mac](https://www.freecodecamp.org/news/how-to-install-python-3-on-mac-and-update-the-python-version-macos-homebrew-command-guide/) 上安装 Python 3 的优秀文章。

一些平台，比如一些现代的 Linux 发行版，预装了最新版本的 Python。因此，如果您在 Linux 上，执行以下命令来检查您的 Python 版本:

```
python3 --version
```

在您的系统上安装任何版本的 Python 3 就足够了。除了 Python，您还需要一个非常适合编写 Python 代码的代码编辑器或 IDE。

在我的[Python IDE——Python 的最佳 IDE 和编辑器](https://www.freecodecamp.org/news/python-ide-best-ides-and-editors-for-python/)文章中，我列出了三个可以用来编写 Python 代码的最佳代码编辑器和 IDE。

因此，如果您已经准备好 Python 和代码编辑器或 IDE，让我们继续编写您的第一段 Python 代码。

### 如何写你好，世界！在 Python 中

在您的计算机中，创建一个名为`program.py`的新文件，并将以下代码放入其中:

```
print('Hello, World!')
```

要运行这段代码，请在放置`program.py`文件的目录中打开您的终端，并执行以下命令:

```
# on Windows and macOS
python program.py

# on Linux
python3 program.py
```

代码的输出将是您作为`print()`函数的参数传递的内容，在这个代码片段中是:

```
Hello, World!
```

您可能已经猜到了，`print()`是一个内置的 Python 函数，它可以打印您在控制台上给它的任何内容。该函数可以打印字符串、数字、表达式——或多或少地打印任何你能处理的东西。

```
print('Hello, World!')
print(100)
print(5 + 5)
```

第一条语句像前面一样打印出字符串`Hello, World!`。第二个输出一个数字，第三个输出`5 + 5`表达式的结果:

```
Hello, World!
100
10
```

您可能已经注意到或者没有注意到的一件事是，这三个 print 语句已经在三个单独的行中输出。而在其他语言中，比如 C/C++/C#/Java，你必须显式地添加一个换行符。

事实证明，`print()`默认情况下起换行符的作用，您可以像下面这样覆盖这个默认行为:

```
print('Hello, World!', end=' | ')
print(100, end=' | ')
print(5 + 5)
```

现在程序的输出将是:

```
Hello, World! | 100 | 10
```

这意味着您作为参数`end`的值传递的任何字符串都将被用作打印行的终止字符。

在这里，我使用`|`作为前两条语句的终止字符。然而，我使用了默认的换行符作为最后一条语句的终止符。

您可以通过试用或阅读关于该功能的[官方文档](https://docs.python.org/3/library/functions.html#print)来了解关于`print()`功能的更多信息。

### Python 中的变量

要在 Python 中声明一个变量，首先要写出变量的名称，然后是等号，最后是变量的值:

```
name = 'Farhan'

print('My name is ' + name)
```

该代码的输出将是:

```
My name is Farhan
```

如您所见，声明变量没有特殊的关键字。Python 足够聪明，可以从你赋予的值中获取变量的类型。

在上面的例子中，`name`变量包含了`Farhan`字符串。由于单词`Farhan`在引号内，Python 会把这个变量当作一个字符串。

在 Python 中，可以使用加号连接两个字符串。这就是我们在上面的`print()`语句中所做的。但是如果您按如下方式更改代码:

```
name = 'Farhan'
age = 27

print('My name is ' + name)
print('I am ' + age + 'years old')
```

并且试着运行这个程序，你会面临以下问题:

```
My name is Farhan
Traceback (most recent call last):
  File "C:\Users\shovi\repos\python-playground\hello-world.py", line 5, in <module>
    print('I am ' + age + 'years old')
TypeError: can only concatenate str (not "int") to str
```

如你所见，字符串只能与字符串连接，并且`age`变量是一个整数。在字符串语句中嵌入变量有一种更好的方法。

```
name = 'Farhan'
age = 27

print(f'My name is {name}')
print(f'I am {age} years old')
```

我希望您已经注意到了`print()`语句中字符串开头的`f`。这个`f`把琴弦变成 f 弦。这些字符串是在运行时计算的，所以在 f-string 中，您可以将任何有效的 Python 语句放在花括号中。这使得在字符串中嵌入变量甚至简单的逻辑变得非常容易。

你可以在程序的任何地方重新声明你的变量。如果你愿意，你甚至可以改变它们的类型。

```
a = 'this is a string'

a = 10

print(a)
```

这是一个完全有效的程序，`a`的值将被打印为`10`,因为你已经覆盖了第二行的初始值。

### Python 中的数据类型

在 Python 中，您需要了解四种主要的文字类型:

| 类型 | 例子 |
| --- | --- |
| 整数 | one |
| 浮点 | Two |
| 布尔代数学体系的 | 真实的 |
| 线 | 自由代码营 |

整数和浮点是不言自明的。布尔值可以是`true`或`false`，Python 中的字符串可以用单引号或双引号括起来。我更喜欢用单引号。你可以使用你喜欢的一个，但是不要把两种类型的引用混在一起。

### Python 中的注释

Python 中的注释以散列符号开始:

```
# this is a comment
```

使用哈希编写的注释只能是单行的。如果你想用 Python 写多行注释，你必须使用引号，如下所示:

```
'''this is a comment
 that goes on
 and on
 and on...'''
```

根据需要对代码进行注释是记录代码的好方法。但是要确保不要在代码看起来容易理解的地方添加注释。

### Python 中的字符串

Python 中的字符串是 Unicode 字符的有序集合。运行时不能修改字符串。你已经看到了如何声明一个字符串。在本节中，您将了解常见的字符串操作。

在字符串中，每个字符都有一个索引。和数组一样，字符串索引也是从零开始的。

```
name = 'Farhan'

# F -> 0
# a -> 1
# r -> 2
# h -> 3
# a -> 4
# n -> 5
```

可以使用这些索引来访问这些字符，如下所示:

```
name = 'Farhan'

print(name[0])
print(name[1])
print(name[2])
print(name[3])
print(name[4])
print(name[5])
```

该程序的输出如下:

```
F
a
r
h
a
n
```

使用这些索引可以做的另一件有趣的事情是切片。假设你想从一个字符串中取出一部分。

```
name = 'Farhan'

print(name[0:3])
```

该程序的输出将是:

```
Far
```

在本例中，`name[0:3]`表示从索引`0`到索引`3`开始打印。现在你可能会认为`h`位于索引`3`处，这一点你是对的。但是切片的问题是，它不包括结束索引处的字符。

如果你想学习更多关于切片的知识，有一篇名为[如何在 Python](https://www.freecodecamp.org/news/how-to-substring-a-string-in-python/) 中对字符串进行子串化的文章，你可能会觉得有用。

您可以使用`len()`函数来计算字符串的长度，如下所示:

```
name = 'Farhan'

print(len(name))
```

这个程序的输出将是`6`，因为字符串中有六个字符。

Python 有大量的字符串方法，但是在这里不可能一一演示，所以我将演示一些最常见的方法。

第一种方法是`capitalize()`。此方法返回给定字符串的副本，其中第一个字符大写，其余字符小写。

```
print('python is awesome'.capitalize())
```

该代码的输出将是`Python is awesome`。如果想把整个句子转换成大写，有`upper()`方法:

```
print('python is awesome'.upper())
```

该代码的输出将是`PYTHON IS AWESOME`。您可以使用`lower()`方法进行相反的操作:

```
print('PYTHON IS AWESOME'.lower())
```

这段代码的输出将是`python is awesome`。有`isupper()`和`islower()`方法来检查字符串是大写还是小写。

```
print('PYTHON IS AWESOME'.islower())
print('PYTHON IS AWESOME'.isupper())
```

该代码的输出如下所示:

```
False
True
```

如果想替换一个字符串中所有出现的子字符串，可以使用`replace()`方法:

```
print('python is awesome'.replace('python', 'freeCodeCamp'))
```

这段代码将把给定字符串中所有出现的`python`替换为`freeCodeCamp`。

最后，还有`split()`和`join()`方法。第一个将一个字符串拆分成一个列表:

```
print('python is awesome'.split(' '))
```

方法使用分隔符来拆分字符串。这里，我使用空格作为分隔符。该代码的输出将是`['python', 'is', 'awesome']`。这是一个列表。我们还没有覆盖列表，但我们很快会。现在，要理解它们就像数组。

您可以使用 iterable 的元素生成一个新的字符串，也就是一个列表，使用`join()`方法:

```
print(' '.join(['python', 'is', 'awesome']))
```

我在空格上调用了`join()`方法，所以这段代码的结果将是一个使用空格连接的字符串，如下所示:

```
python is awesome
```

如果你想学习更多关于 Python 中所有字符串方法的知识，请随意查阅官方文档。

### Python 中的数字

Python 中的数字可以是整数、浮点和复杂类型。在本文中，我将只讨论与实数相关的操作——即整数和浮点。

像在任何其他编程语言中一样，您可以使用整数和浮点数执行加、减、乘和除运算:

```
a = 10
b = 5

print(a+b)
print(a-b)
print(a*b)
print(a/b)
```

这段代码的输出如下:

```
15
5
50
2.0
```

需要记住的一点是，即使在两个整数之间执行除法运算，结果也始终是浮点。如果您希望结果为整数，可以按如下方式进行:

```
a = 10
b = 5

print(a//b)
```

这一次结果将是一个整数。注意，如果小数点后有任何数字，它们将被截掉。

如果你想学习更多关于 Python 中数字类型的知识，请随意查阅官方文档。

### 如何在 Python 中处理用户输入

为了接受用户的输入，有一个`input()`函数。

```
name = input('What is your name? ')

print(f'Your name is {name}')
```

该程序的输出如下:

```
What is your name? Farhan
Your name is Farhan
```

`input()`函数将用户输入保存为字符串，即使用户输入的是数字。因此，如果您从用户那里接受一个数字作为输入，一定要将其转换成适当的数据类型。

### Python 中的 if-elif-else

像任何其他编程语言一样，Python 有常见的`if-elif-else`语句。

```
a = float(input('First: '))
b = float(input('Second: '))
op = input('Operation (sum/sub/mul/div): ')

if op == 'sum':
    print(f'a + b = {a+b}')
elif op == 'sub':
    print(f'a - b = {a-b}')
elif op == 'mul':
    print(f'a * b = {a*b}')
elif op == 'div':
    print(f'a / b = {a/b}')
else:
    print('Invalid Operation!') 
```

这是一个非常简单的计算器程序。根据您选择的操作，计算器将执行上述操作之一。

在 Python 中，`if`块、`elif`块或`else`块等代码块以关键字和冒号开头。

缩进在 Python 中至关重要，如果在代码块中不恰当地缩进代码，代码将无法运行。

### Python 中的大小写匹配

在 Python 中，`match-case`相当于其他编程语言中的`switch-case`语句。上述计算器程序可以使用`match-case`改写如下:

```
a = float(input('First: '))
b = float(input('Second: '))
op = input('Operation (sum/sub/mul/div): ')

match op:
    case 'sum':
        print(f'a + b = {a+b}')
    case 'sub':
        print(f'a - b = {a-b}')
    case 'mul':
        print(f'a * b = {a*b}')
    case 'div':
        print(f'a / b = {a/b}')
    case _:
        print('Invalid Operation!') 
```

同样，根据`op`的值，将执行其中一种情况。如果用户的输入不匹配任何一种情况，那么通配符`_`操作将会发生。

请记住，`match-case`仅在 Python 3.10 和更高版本上可用。所以如果你用的是旧版本，你可能没有这个声明。

### Python 中的列表和元组

Python 中的列表是一个值序列。您可以在运行时修改列表。您可以按如下方式创建列表:

```
vowels = ['a', 'e', 'i', 'o', 'u']

print(vowels)
```

该程序的输出将是`['a', 'e', 'i', 'o', 'u']`。像字符串一样，Python 列表中的每个元素都有一个索引，这些索引从零开始。

```
vowels = ['a', 'e', 'i', 'o', 'u']

print(vowels[0])
print(vowels[1])
print(vowels[2])
print(vowels[3])
print(vowels[4])
```

像字符串一样，你也可以对列表进行切片，对列表进行切片的语法与字符串相同。

Python 中的列表有很多有用的方法。向列表中添加新项目有`append()`、`extend()`和`insert()`方法。

`append()`方法向列表中添加一个新项目，而`extend()`方法添加多个项目:

```
vowels = ['a', 'e']

vowels.append('i')
vowels.extend(['o', 'u'])

print(vowels)
```

另一方面，`insert()`方法在列表中的给定索引处插入一个项目:

```
vowels = ['a', 'i', 'o', 'u']

vowels.insert(1, 'e')

print(vowels)
```

方法从列表中弹出最后一个元素:

```
vowels = ['a', 'e', 'i', 'o', 'u']

popped_item = vowels.pop()

print(popped_item)
print(vowels)
```

这段代码的输出将是:

```
u
['a', 'e', 'i', 'o']
```

方法可以从列表中删除一个给定的元素:

```
vowels = ['a', 'e', 'i', 'o', 'u']

vowels.remove('e')

print(vowels)
```

这将从元音列表中删除`e`。

最后是从列表中删除所有元素的`clear()`方法。

还有`sort()`法:

```
vowels = ['u', 'e', 'a', 'o', 'i']

vowels.sort()

print(vowels)
```

`sort()`方法按升序对列表进行排序。该方法就地对列表进行排序。这意味着它不会返回一个新列表，而是对原始列表进行排序。

如果您想反转列表，有一个`reverse()`方法:

```
vowels = ['u', 'e', 'a', 'o', 'i']

vowels.reverse()

print(vowels)
```

它也是一个像 sort 一样的就地方法。这只是排序方法的反向操作(没有双关的意思)。你可以从[官方文档](https://docs.python.org/3/tutorial/datastructures.html)中了解更多关于列表的信息。

Python 中还有一种不可变的序列类型叫做 tuple。元组非常类似于列表，但是您不能修改元组。

```
vowels = ('a', 'e', 'i', 'o', 'u')

print(vowels)
```

这段代码的输出将是`('a', 'e', 'i', 'o', 'u')`。元组的方法不多。如果你想了解更多关于元组的知识，请查阅官方文档。

### Python 中的循环

您可以在 Python 中使用循环来迭代类似列表的序列类型。

```
vowels = ['a', 'e', 'i', 'o', 'u']

for letter in vowels:
    print(letter.upper())
```

还有`while`循环，但是因为`for`循环是你最常用的，我不会花时间解释`while`循环。

### Python 中的字典

让我们假设我给了你一句台词“敏捷的棕色狐狸跳过了懒惰的狗”，并告诉你数一数每个字母出现的次数。使用 hashmap 可以很容易地做到这一点。

hashmap 是键值对的集合。

```
{
    key_1: value_1,
    key_2: value_2,
    key_3: value_3,
    key_4: value_4,
    key_5: value_5,
}
```

要完成我之前交给您的任务，您可以编写以下代码:

```
sentence = 'the quick brown fox jumped over the lazy dog'

record = {}

for letter in sentence:
    if letter in record:
        record[letter] += 1
    else:
        record[letter] = 1

print(record)
```

这段代码的输出如下:

```
{'t': 2, 'h': 2, 'e': 4, ' ': 8, 'q': 1, 'u': 2, 'i': 1, 'c': 1, 'k': 1, 'b': 1, 'r': 2, 'o': 4, 'w': 1, 'n': 1, 'f': 1, 'x': 1, 'j': 1, 'm': 1, 'p': 1, 'd': 2, 'v': 1, 'l': 1, 'a': 1, 'z': 1, 'y': 1, 'g': 1}
```

这是一本字典。每个字母都是一个键，它们的出现次数就是值。在代码片段中，您在第二行声明了一个字典。Estefania Cassingena Navone 写了一篇文章，名为 [Python 字典 101:详细的可视化介绍](https://www.freecodecamp.org/news/python-dictionaries-detailed-visual-introduction/)，你可以参考这篇文章了解更多关于字典的知识。

### Python 中的函数

我要讨论的最后一个概念是函数。编程中的函数是执行特定任务的代码块。

在 Python 中，可以使用`def`关键字后跟函数签名来声明函数:

```
def sum(a, b):
    return a + b

def sub(a, b):
    return a - b

def mul(a, b):
    return a * b

def div(a, b):
    return a / b

a = float(input('First: '))
b = float(input('Second: '))
op = input('Operation (sum/sub/mul/div): ')

if op == 'sum':
    print(f'a + b = {sum(a, b)}')
elif op == 'sub':
    print(f'a - b = {sub(a, b)}')
elif op == 'mul':
    print(f'a * b = {mul(a, b)}')
elif op == 'div':
    print(f'a / b = {div(a, b)}')
else:
    print('Invalid Operation!') 
```

这是和以前一样的计算器程序，但是现在运算是在单独的函数中编写的。

要了解更多关于函数的知识，你可以阅读这篇文章:[Python 中的函数——由](https://www.freecodecamp.org/news/functions-in-python-a-beginners-guide/) [Bala Priya C](https://www.freecodecamp.org/news/author/bala-priya/) 用代码示例解释。

如果你想成为一名伟大的 Python 程序员，还有很多其他的概念需要学习。这就是下一节的内容。

## 其他 Python 学习资源

现在您已经对 Python 编程语言有了基本的了解，我将推荐一些高质量的学习资源来继续您的学习之旅。

### 4 小时学会 Python

[https://www.youtube.com/embed/rfscVS0vtbw?feature=oembed](https://www.youtube.com/embed/rfscVS0vtbw?feature=oembed)

列表中的第一个资源是 Girrafe Academy 创建的 freeCodeCamp YouTube 频道上的视频。

该讲师在该频道上创建了多个课程，并以制作简洁的视频而闻名。

该视频在 4 个小时内涵盖了大多数重要的 Python 概念。指导者也在过程中制作简单的项目。

### 面向所有人的 Python

[https://www.youtube.com/embed/8DvywoWv6fI?feature=oembed](https://www.youtube.com/embed/8DvywoWv6fI?feature=oembed)

另一个对初学者友好的 Python 课程是人人 Python。这门课程的特别之处在于，它不仅针对 Python 初学者，也针对那些试图开始一般编程的人。

该课程时长 13 小时多一点，由 Charles R. Severance(又名 Chuck 博士)教授。他在互联网上创作了一些最令人惊叹的课程。

如果你有足够的耐心坐完 13 个小时的课程，面向所有人的 Python 是最好的在线 Python 课程之一。

### 12 个初级 Python 项目

[https://www.youtube.com/embed/8ext9G7xspg?feature=oembed](https://www.youtube.com/embed/8ext9G7xspg?feature=oembed)

如果你喜欢以项目为基础的方法，那么强烈推荐 Kylie Ying 的这个 3 小时课程。

Kylie 是 freeCodeCamp 团队成员，知道自己在做什么。在本课程中，你将学习制作 12 个初学者友好的 Python 项目，项目的复杂程度逐渐增加。

### 通过构建 5 个游戏来学习 Python

[https://www.youtube.com/embed/XGf2GcyHPhc?feature=oembed](https://www.youtube.com/embed/XGf2GcyHPhc?feature=oembed)

如果你对游戏感兴趣，并且想通过构建经典游戏来学习 Python，那么这个课程非常适合你。

本课程的讲师是 Christian Thompson，一位经验丰富的 Python 程序员。如果你熟悉另一种编程语言，或者你通过投入项目学习得很好，那就投入进去吧。

### 中级 Python

[https://www.youtube.com/embed/HGOBQPFzWKo?feature=oembed](https://www.youtube.com/embed/HGOBQPFzWKo?feature=oembed)

如果你已经完成了你的第一门 Python 课程，并学习了所有的基本概念，现在你正在寻找下一个逻辑步骤，那么不要再看了。

Patrick Loeber 制作了这个 6 小时的中级 Python 课程，在这个课程中，你将学到初级 Python 课程中通常找不到的大量概念。

### 用 Python 进行面向对象编程

[https://www.youtube.com/embed/Ej_02ICOIgs?feature=oembed](https://www.youtube.com/embed/Ej_02ICOIgs?feature=oembed)

如果你正在努力理解面向对象编程，本课程将在 2 小时内教你用 Python 进行面向对象编程。

本课程包括大量的代码示例，涵盖了所有关于 Python 面向对象编程的重要概念。我建议你在完成基础课程后马上上这门课。

### 用于数据科学的 Python

[https://www.youtube.com/embed/LHBE6Q9XlzI?feature=oembed](https://www.youtube.com/embed/LHBE6Q9XlzI?feature=oembed)

这是一门专业课。如果你正在考虑进入数据科学领域，并且想学习数据科学所需的所有必要的 Python 知识，那么这个课程将会给你很大的帮助。

不要认为这是一门速成课程。在 12 个多小时的运行时间里，本课程将向您详细介绍 Python 编程概念和一系列数据科学必备的工具。

### Python 中的数据结构和算法

[https://www.youtube.com/embed/pkYVOmU3MgA?feature=oembed](https://www.youtube.com/embed/pkYVOmU3MgA?feature=oembed)

理解数据结构和算法对于成为一名高效的软件开发人员至关重要。

在 Jovian 的这个 12.5 小时的课程中，您将通过代码示例详细了解重要的数据结构和算法。不管你以后打算用 Python 做什么，我都强烈推荐这门课。

## 结论

我衷心感谢您花时间阅读这篇文章。

尽管我已经尽可能多地列出了好的资源，但是免费代码营 YouTube 频道却充满了优秀的 Python 学习资源。

我也有一个个人博客，在那里我写一些随机的技术东西，所以如果你对类似的东西感兴趣，请查看 https://farhan.dev 。如果你有任何问题或对任何事情感到困惑——或者只是想联系——我可以在 [Twitter](https://twitter.com/frhnhsin) 和 [LinkedIn](https://www.linkedin.com/in/farhanhasin/) 上找到我。