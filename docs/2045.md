# Python 代码示例——面向初学者的示例脚本编码教程

> 原文：<https://www.freecodecamp.org/news/python-code-examples-sample-script-coding-tutorial-for-beginners/>

嗨！欢迎光临。如果你正在学习 Python，那么这篇文章适合你。您会发现 Python 语法的全面描述和大量代码示例，在您的编码之旅中为您提供指导。

### 我们将涵盖的内容:

*   [Python 中的变量定义](#-variable-definitions-in-python)
*   你好，世界！Python 中的程序
*   [Python 中的数据类型和内置数据结构](#-data-types-and-built-in-data-structures-in-python)
*   [Python 运算符](#-python-operators)
*   [Python 中的条件语句](#-conditionals-in-python)
*   [对于 Python 中的循环](#-for-loops-in-python)
*   [Python 中的 While 循环](#-while-loops-in-python)
*   [Python 中的嵌套循环](#-nested-loops-in-python)
*   [Python 中的函数](#-functions-in-python)
*   [Python 中的递归](#-recursion-in-python)
*   [Python 中的异常处理](#-exception-handling-in-python)
*   [Python 中的面向对象编程](#-object-oriented-programming-in-python)
*   [如何在 Python 中处理文件](#-how-to-work-with-files-in-python)
*   [在 Python 中导入语句](#-import-statements-in-python)
*   [Python 中的列表和字典理解](#-list-and-dictionary-comprehension-in-python)
*   更多...

你准备好了吗？我们开始吧！🔅

💡**提示:**在整篇文章中，我将使用`<>`来表示这部分语法将被文本所描述的元素替换。例如，`<var>`意味着在我们编写代码时，这将被一个变量所替代。

## 🔹Python 中的变量定义

任何编程语言最基本的组成部分是变量的概念，一个名字和我们为一个值保留的内存位置。

在 Python 中，我们使用以下语法创建一个变量并为该变量赋值:

```
<var_name> = <value>
```

例如:

```
age = 56
```

```
name = "Nora"
```

```
color = "Blue"
```

```
grades = [67, 100, 87, 56]
```

如果变量名有多个单词，那么 Python 代码的[风格指南建议用下划线分隔单词“以提高可读性”](https://www.python.org/dev/peps/pep-0008/)

例如:

```
my_list = [1, 2, 3, 4, 5]
```

💡**提示:**Python 代码的风格指南(PEP 8)有很好的建议，你应该遵循这些建议来编写干净的 Python 代码。

## 🔸你好，世界！Python 程序

在我们开始钻研 Python 中可以使用的数据类型和数据结构之前，让我们看看如何编写第一个 Python 程序。

你只需要调用`print()`函数并在括号内写上`"Hello, World!"`:

```
print("Hello, World!")
```

运行程序后，您会看到以下消息:

```
"Hello, World!"
```

💡**提示:**编写`"Hello, World!"`程序是开发者社区的传统。大多数开发人员通过编写这个程序开始学习如何编码。

太好了。你刚刚写了你的第一个 Python 程序。现在让我们开始学习可以在 Python 中使用的数据类型和内置数据结构。

## 🔹Python 中的数据类型和内置数据结构

我们有几种基本的数据类型和内置的数据结构，可以在程序中使用。每一种都有其特定的应用。下面我们来详细看看。

### Python 中的数字数据类型:整数、浮点数和复数

以下是您可以在 Python 中使用的数值类型:

#### 整数

整数是没有小数的数字。可以用`type()`函数检查一个数是否是整数。如果输出是`<class 'int'>`，那么数字是整数。

例如:

```
>>> type(1)
<class 'int'>

>>> type(15)
<class 'int'>

>>> type(0)
<class 'int'>

>>> type(-46)
<class 'int'>
```

#### 漂浮物

浮点数是带小数的数字。你可以通过定位小数点来直观地发现它们。如果我们调用`type()`来检查这些值的数据类型，我们将看到如下输出:

```
<class 'float'>
```

这里我们有一些例子:

```
>>> type(4.5)
<class 'float'>

>>> type(5.8)
<class 'float'>

>>> type(2342423424.3)
<class 'float'>

>>> type(4.0)
<class 'float'>

>>> type(0.0)
<class 'float'>

>>> type(-23.5)
<class 'float'>
```

#### 复杂的

复数有一个实部和一个虚部，用`j`表示。可以用`complex()`在 Python 中创建复数。第一个参数是实部，第二个参数是虚部。

以下是一些例子:

```
>>> complex(4, 5)
(4+5j)

>>> complex(6, 8)
(6+8j)

>>> complex(3.4, 3.4)
(3.4+3.4j)

>>> complex(0, 0)
0j

>>> complex(5)
(5+0j)

>>> complex(0, 4)
4j
```

### Python 中的字符串

字符串在 Python 中非常有用。它们包含一系列字符，通常用于表示代码中的文本。

例如:

```
"Hello, World!"
```

```
'Hello, World!'
```

我们可以使用单引号`''`或双引号`""`来定义一个字符串。它们都是有效的和等价的，但是你应该选择其中的一个，并在整个程序中一致地使用它。

**💡提示:**是的！你写`"Hello, World!"`程序的时候用了一个字符串。在 Python 中，只要看到用单引号或双引号括起来的值，那就是字符串。

字符串可以包含我们可以在键盘上输入的任何字符，包括数字、符号和其他特殊字符。

例如:

```
"45678"
```

```
"my_email@email.com"
```

```
"#IlovePython"
```

**💡提示:**空格也算作字符串中的字符。

#### 字符串中的引号

如果我们用双引号`""`定义一个字符串，那么我们可以在字符串中使用单引号。例如:

```
"I'm 20 years old"
```

如果我们用单引号`''`定义一个字符串，那么我们可以在字符串中使用双引号。例如:

```
'My favorite book is "Sense and Sensibility"'
```

#### 字符串索引

我们可以在 Python 程序中使用索引来访问字符串的字符。索引是表示字符串中特定位置的整数。它们与该位置的角色相关联。

这是字符串`"Hello"`的示意图:

```
String:  H e l l o
Index:   0 1 2 3 4
```

**💡提示:**索引从`0`开始，每向右一个字符就增加`1`。

例如:

```
>>> my_string = "Hello"

>>> my_string[0]
'H'

>>> my_string[1]
'e'

>>> my_string[2]
'l'

>>> my_string[3]
'l'

>>> my_string[4]
'o'
```

我们也可以使用负索引来访问这些字符:

```
>>> my_string = "Hello"

>>> my_string[-1]
'o'

>>> my_string[-2]
'l'

>>> my_string[-3]
'l'

>>> my_string[-4]
'e'

>>> my_string[-5]
'H'
```

**💡提示:**我们通常使用`-1`来访问字符串的最后一个字符。

#### 字符串切片

我们可能还需要获取字符串的一部分或其字符的子集。我们可以用字符串切片来实现。

这是一般语法:

```
<string_variable>[start:stop:step]
```

`start`是将包含在切片中的第一个字符的索引。默认是`0`。

*   `stop`是切片中最后一个字符的索引(该字符将**而非**包括在内)。默认情况下，它是字符串中的最后一个字符(如果我们省略这个值，最后一个字符也将包括在内)。
*   `step`是我们要在当前指数上加多少才能达到下一个指数。

我们可以指定两个参数来使用`step`的默认值，也就是`1`。这将包括从`start`到`stop`之间的所有字符(不含):

```
<string_variable>[start:stop]
```

例如:

```
>>> freecodecamp = "freeCodeCamp"

>>> freecodecamp[2:8]
'eeCode'

>>> freecodecamp[0:3]
'fre'

>>> freecodecamp[0:4]
'free'

>>> freecodecamp[4:7]
'Cod'

>>> freecodecamp[4:8]
'Code'

>>> freecodecamp[8:11]
'Cam'

>>> freecodecamp[8:12]
'Camp'

>>> freecodecamp[8:13]
'Camp'
```

**💡提示:**注意，如果一个参数的值超出了索引的有效范围，切片仍然会被显示。这就是 Python 的创造者如何实现这种字符串切片特性的。

如果我们自定义`step`，我们将根据这个值从一个索引“跳到”下一个。

例如:

```
>>> freecodecamp = "freeCodeCamp"

>>> freecodecamp[0:9:2]
'feCdC'

>>> freecodecamp[2:10:3]
'eoC'

>>> freecodecamp[1:12:4]
'roa'

>>> freecodecamp[4:8:2]
'Cd'

>>> freecodecamp[3:9:2]
'eoe'

>>> freecodecamp[1:10:5]
'rd'
```

我们也可以使用一个**负**步从右向左移动:

```
>>> freecodecamp = "freeCodeCamp"

>>> freecodecamp[10:2:-1]
'maCedoCe'

>>> freecodecamp[11:4:-2]
'paeo'

>>> freecodecamp[5:2:-4]
'o'
```

我们可以省略一个参数来使用它的默认值。如果我们省略了`start`、`stop`，或者两者都省略，我们只需要包含相应的冒号(`:`):

```
>>> freecodecamp = "freeCodeCamp"

# Default start and step
>>> freecodecamp[:8]
'freeCode'

# Default end and step
>>> freecodecamp[4:]
'CodeCamp'

# Default start
>>> freecodecamp[:8:2]
'feCd'

# Default stop
>>> freecodecamp[4::3]
'Cem'

# Default start and stop
>>> freecodecamp[::-2]
'paeoer'

# Default start and stop
>>> freecodecamp[::-1]
'pmaCedoCeerf'
```

**💡提示:**最后一个例子是反转字符串最常见的方法之一。

#### f 弦

在 Python 3.6 和更高版本中，我们可以使用一种叫做 f-string 的字符串类型，它可以帮助我们更容易地格式化我们的字符串。

要定义 f 字符串，我们只需在单引号或双引号前添加一个`f`。然后，在字符串中，我们用花括号`{}`将变量或表达式括起来。当我们运行程序时，这将替换它们在字符串中的值。

例如:

```
first_name = "Nora"
favorite_language = "Python"

print(f"Hi, I'm {first_name}. I'm learning {favorite_language}.") 
```

输出是:

```
Hi, I'm Nora. I'm learning Python.
```

这里有一个例子，我们计算表达式的值并替换字符串中的结果:

```
value = 5

print(f"{value} multiplied by 2 is: {value * 2}")
```

输出中的值被替换:

```
5 multiplied by 2 is: 10
```

我们还可以调用花括号中的方法，当我们运行程序时，返回值将被替换为字符串:

```
freecodecamp = "FREECODECAMP"

print(f"{freecodecamp.lower()}")
```

输出是:

```
freecodecamp
```

#### 字符串方法

字符串有方法，这些方法代表了 Python 开发人员已经实现的常见功能，所以我们可以在程序中直接使用它们。它们对执行常见操作非常有帮助。

这是调用字符串方法的一般语法:

```
<string_variable>.<method_name>(<arguments>)
```

例如:

```
>>> freecodecamp = "freeCodeCamp"

>>> freecodecamp.capitalize()
'Freecodecamp'

>>> freecodecamp.count("C")
2

>>> freecodecamp.find("e")
2

>>> freecodecamp.index("p")
11

>>> freecodecamp.isalnum()
True

>>> freecodecamp.isalpha()
True

>>> freecodecamp.isdecimal()
False

>>> freecodecamp.isdigit()
False

>>> freecodecamp.isidentifier()
True

>>> freecodecamp.islower()
False

>>> freecodecamp.isnumeric()
False

>>> freecodecamp.isprintable()
True

>>> freecodecamp.isspace()
False

>>> freecodecamp.istitle()
False

>>> freecodecamp.isupper()
False

>>> freecodecamp.lower()
'freecodecamp'

>>> freecodecamp.lstrip("f")
'reeCodeCamp'

>>> freecodecamp.rstrip("p")
'freeCodeCam'

>>> freecodecamp.replace("e", "a")
'fraaCodaCamp'

>>> freecodecamp.split("C")
['free', 'ode', 'amp']

>>> freecodecamp.swapcase()
'FREEcODEcAMP'

>>> freecodecamp.title()
'Freecodecamp'

>>> freecodecamp.upper()
'FREECODECAMP'
```

要了解更多关于 Python 方法的知识，我推荐阅读 Python 文档中的这篇文章。

💡**提示:**所有的字符串方法都返回字符串的副本。他们不修改字符串，因为字符串在 Python 中是不可变的。

### Python 中的布尔值

布尔值在 Python 中是`True`和`False`。它们必须以大写字母开头，才能被识别为布尔值。

例如:

```
>>> type(True)
<class 'bool'>

>>> type(False)
<class 'bool'>
```

如果我们用小写写它们，我们会得到一个错误:

```
>>> type(true)
Traceback (most recent call last):
  File "<pyshell#92>", line 1, in <module>
    type(true)
NameError: name 'true' is not defined

>>> type(false)
Traceback (most recent call last):
  File "<pyshell#93>", line 1, in <module>
    type(false)
NameError: name 'false' is not defined
```

### Python 中的列表

既然我们已经介绍了 Python 中的基本数据类型，让我们开始介绍内置数据结构。首先，我们有列表。

为了定义一个列表，我们使用方括号`[]`和逗号分隔的元素。

**💡提示:**建议在每个逗号后加一个空格，使代码可读性更好。

例如，下面是列表的示例:

```
[1, 2, 3, 4, 5]
```

```
["a", "b", "c", "d"]
```

```
[3.4, 2.4, 2.6, 3.5]
```

列表可以包含不同数据类型的值，因此这将是 Python 中的有效列表:

```
[1, "Emily", 3.4]
```

我们也可以给变量分配一个列表:

```
my_list = [1, 2, 3, 4, 5]
```

```
letters = ["a", "b", "c", "d"]
```

#### 嵌套列表

列表可以包含任何数据类型的值，甚至是其他列表。这些内部列表被称为**嵌套列表**。

```
[[1, 2, 3], [4, 5, 6]]
```

在这个例子中，`[1, 2, 3]`和`[4, 5, 6]`是嵌套列表。

这里我们有其他有效的例子:

```
[["a", "b", "c"], ["d", "e", "f"], ["g", "h", "i"]]
```

```
[1, [2, 3, 4], [5, 6, 7], 3.4]
```

我们可以使用相应的索引来访问嵌套列表:

```
>>> my_list = [[1, 2, 3], [4, 5, 6]]

>>> my_list[0]
[1, 2, 3]

>>> my_list[1]
[4, 5, 6]
```

例如，嵌套列表可以用来表示简单的 2D 游戏棋盘的结构，其中每个数字可以表示不同的元素或牌:

```
# Sample Board where: 
# 0 = Empty tile
# 1 = Coin
# 2 = Enemy
# 3 = Goal
board = [[0, 0, 1],
         [0, 2, 0],
         [1, 0, 3]]
```

#### 列表长度

我们可以使用`len()`函数来获得一个列表的长度(它包含的元素的数量)。

例如:

```
>>> my_list = [1, 2, 3, 4]

>>> len(my_list)
4
```

#### 更新列表中的值

我们可以使用以下语法更新特定索引处的值:

```
<list_variable>[<index>] = <value>
```

例如:

```
>>> letters = ["a", "b", "c", "d"]

>>> letters[0] = "z"

>>> letters
['z', 'b', 'c', 'd']
```

#### 向列表中添加值

我们可以用`.append()`方法在列表末尾添加一个新值。

例如:

```
>>> my_list = [1, 2, 3, 4]

>>> my_list.append(5)

>>> my_list
[1, 2, 3, 4, 5]
```

#### 从列表中删除值

我们可以用`.remove()`方法从列表中删除一个值。

例如:

```
>>> my_list = [1, 2, 3, 4]

>>> my_list.remove(3)

>>> my_list
[1, 2, 4]
```

💡**提示:**这只会删除第一次出现的元素。例如，如果我们试图从包含两个数字 3 的列表中删除数字 3，第二个数字不会被删除:

```
>>> my_list = [1, 2, 3, 3, 4]

>>> my_list.remove(3)

>>> my_list
[1, 2, 3, 4]
```

#### 列表索引

我们可以像索引字符串一样索引列表，索引从`0`开始:

```
>>> letters = ["a", "b", "c", "d"]

>>> letters[0]
'a'

>>> letters[1]
'b'

>>> letters[2]
'c'

>>> letters[3]
'd'
```

#### 列表切片

我们还可以使用与字符串相同的语法来获取列表的一部分，并且我们可以省略参数来使用它们的默认值。现在，我们将添加列表中的元素，而不是向切片中添加字符。

```
<list_variable>[start:stop:step]
```

例如:

```
>>> my_list = ["a", "b", "c", "d", "e", "f", "g", "h", "i"]

>>> my_list[2:6:2]
['c', 'e']

>>> my_list[2:8]
['c', 'd', 'e', 'f', 'g', 'h']

>>> my_list[1:10]
['b', 'c', 'd', 'e', 'f', 'g', 'h', 'i']

>>> my_list[4:8:2]
['e', 'g']

>>> my_list[::-1]
['i', 'h', 'g', 'f', 'e', 'd', 'c', 'b', 'a']

>>> my_list[::-2]
['i', 'g', 'e', 'c', 'a']

>>> my_list[8:1:-1]
['i', 'h', 'g', 'f', 'e', 'd', 'c']
```

#### 列出方法

Python 也已经实现了列表方法来帮助我们执行常见的列表操作。以下是一些最常用的列表方法的示例:

```
>>> my_list = [1, 2, 3, 3, 4]

>>> my_list.append(5)
>>> my_list
[1, 2, 3, 3, 4, 5]

>>> my_list.extend([6, 7, 8])
>>> my_list
[1, 2, 3, 3, 4, 5, 6, 7, 8]

>>> my_list.insert(2, 15)
>>> my_list
[1, 2, 15, 3, 3, 4, 5, 6, 7, 8, 2, 2]

>>> my_list.remove(2)
>>> my_list
[1, 15, 3, 3, 4, 5, 6, 7, 8, 2, 2]

>>> my_list.pop()
2

>>> my_list.index(6)
6

>>> my_list.count(2)
1

>>> my_list.sort()
>>> my_list
[1, 2, 3, 3, 4, 5, 6, 7, 8, 15]

>>> my_list.reverse()
>>> my_list
[15, 8, 7, 6, 5, 4, 3, 3, 2, 1]

>>> my_list.clear()
>>> my_list
[]
```

要了解更多关于列表方法的信息，我推荐阅读 Python 文档中的这篇文章。

### Python 中的元组

为了在 Python 中定义元组，我们使用括号`()`并用逗号分隔元素。建议在每个逗号后添加一个空格，使代码可读性更好。

```
(1, 2, 3, 4, 5)
```

```
("a", "b", "c", "d")
```

```
(3.4, 2.4, 2.6, 3.5)
```

我们可以将元组分配给变量:

```
my_tuple = (1, 2, 3, 4, 5)
```

#### 元组索引

我们可以访问元组的每个元素及其对应的索引:

```
>>> my_tuple = (1, 2, 3, 4)

>>> my_tuple[0]
1

>>> my_tuple[1]
2

>>> my_tuple[2]
3

>>> my_tuple[3]
4
```

我们也可以使用负指数:

```
>>> my_tuple = (1, 2, 3, 4)

>>> my_tuple[-1]
4

>>> my_tuple[-2]
3

>>> my_tuple[-3]
2

>>> my_tuple[-4]
1
```

#### 元组长度

为了找到元组的长度，我们使用`len()`函数，将元组作为参数传递:

```
>>> my_tuple = (1, 2, 3, 4)

>>> len(my_tuple)
4
```

#### 嵌套元组

元组可以包含任何数据类型的值，甚至列表和其他元组。这些内部元组称为**嵌套元组**。

```
([1, 2, 3], (4, 5, 6))
```

在这个例子中，我们有一个嵌套的元组`(4, 5, 6)`和一个列表。您可以使用相应的索引来访问这些嵌套的数据结构。

例如:

```
>>> my_tuple = ([1, 2, 3], (4, 5, 6))

>>> my_tuple[0]
[1, 2, 3]

>>> my_tuple[1]
(4, 5, 6)
```

#### 元组切片

我们可以像分割列表和字符串一样分割一个元组。同样的原则和规则也适用。

这是一般语法:

```
<tuple_variable>[start:stop:step]
```

例如:

```
>>> my_tuple = (4, 5, 6, 7, 8, 9, 10)

>>> my_tuple[3:8]
(7, 8, 9, 10)

>>> my_tuple[2:9:2]
(6, 8, 10)

>>> my_tuple[:8]
(4, 5, 6, 7, 8, 9, 10)

>>> my_tuple[:6]
(4, 5, 6, 7, 8, 9)

>>> my_tuple[:4]
(4, 5, 6, 7)

>>> my_tuple[3:]
(7, 8, 9, 10)

>>> my_tuple[2:5:2]
(6, 8)

>>> my_tuple[::2]
(4, 6, 8, 10)

>>> my_tuple[::-1]
(10, 9, 8, 7, 6, 5, 4)

>>> my_tuple[4:1:-1]
(8, 7, 6)
```

#### 元组方法

Python 中有两种内置的元组方法:

```
>>> my_tuple = (4, 4, 5, 6, 6, 7, 8, 9, 10)

>>> my_tuple.count(6)
2

>>> my_tuple.index(7)
5
```

💡**提示:**元组是不可变的。它们不能被修改，所以我们不能添加、更新或删除元组中的元素。如果我们需要这样做，那么我们需要创建一个新的元组副本。

#### 元组分配

在 Python 中，我们有一个非常酷的特性，叫做元组赋值。通过这种类型的赋值，我们可以在同一行上给多个变量赋值。

这些值按照出现的顺序分配给相应的变量。例如，在`a, b = 1, 2`中，值`1`被分配给变量`a`，值`2`被分配给变量`b`。

例如:

```
# Tuple Assignment
>>> a, b = 1, 2

>>> a
1

>>> b
2
```

**💡提示:**元组赋值常用于交换两个变量的值:

```
>>> a = 1

>>> b = 2

# Swap the values
>>> a, b = b, a

>>> a
2

>>> b
1
```

### Python 中的字典

现在让我们开始钻研字典。这个内置的数据结构允许我们创建成对的值，其中一个值与另一个值相关联。

为了在 Python 中定义一个字典，我们使用花括号`{}`和逗号分隔的键值对。

键用冒号`:`与值分开，如下所示:

```
{"a": 1, "b": 2, "c"; 3}
```

您可以将字典分配给变量:

```
my_dict = {"a": 1, "b": 2, "c"; 3}
```

字典的键必须是不可变的数据类型。例如，它们可以是字符串、数字或元组，但不是列表，因为列表是可变的。

*   字符串:`{"City 1": 456, "City 2": 577, "City 3": 678}`
*   数字:`{1: "Move Left", 2: "Move Right", 3: "Move Up", 4: "Move Down"}`
*   元组:`{(0, 0): "Start", (2, 4): "Goal"}`

字典的值可以是任何数据类型，因此我们可以将字符串、数字、列表、元组、集合甚至其他字典指定为值。这里我们有一些例子:

```
{"product_id": 4556, "ingredients": ["tomato", "cheese", "mushrooms"], "price": 10.67}
```

```
{"product_id": 4556, "ingredients": ("tomato", "cheese", "mushrooms"), "price": 10.67}
```

```
{"id": 567, "name": "Emily", "grades": {"Mathematics": 80, "Biology": 74, "English": 97}}
```

#### 字典长度

为了获得键值对的数量，我们使用了`len()`函数:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

>>> len(my_dict)
4
```

#### 在字典中获取值

要在字典中获取一个值，我们使用它的键，语法如下:

```
<variable_with_dictionary>[<key>]
```

该表达式将被对应于该键的值替换。

例如:

```
my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

print(my_dict["a"])
```

输出是与`"a"`相关的值:

```
1
```

#### 更新字典中的值

为了更新与现有键关联的值，我们使用相同的语法，但是现在我们添加了一个赋值运算符和值:

```
<variable_with_dictionary>[<key>] = <value>
```

例如:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

>>> my_dict["b"] = 6
```

现在字典是:

```
{'a': 1, 'b': 6, 'c': 3, 'd': 4}
```

#### 将键值对添加到字典中

字典的键必须是唯一的。要添加新的键-值对，我们使用与更新值相同的语法，但是现在键必须是新的。

```
<variable_with_dictionary>[<new_key>] = <value>
```

例如:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

>>> my_dict["e"] = 5
```

现在字典有了一个新的键值对:

```
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

#### 删除字典中的键值对

要删除一个键值对，我们使用`del`语句:

```
del <dictionary_variable>[<key>] 
```

例如:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

>>> del my_dict["c"]
```

现在字典是:

```
{'a': 1, 'b': 2, 'd': 4}
```

#### 字典方法

以下是一些最常用的字典方法的示例:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3, "d": 4}

>>> my_dict.get("c")
3

>>> my_dict.items()
dict_items([('a', 1), ('b', 2), ('c', 3), ('d', 4)])

>>> my_dict.keys()
dict_keys(['a', 'b', 'c', 'd'])

>>> my_dict.pop("d")
4

>>> my_dict.popitem()
('c', 3)

>>> my_dict.setdefault("a", 15)
1

>>> my_dict
{'a': 1, 'b': 2}

>>> my_dict.setdefault("f", 25)
25

>>> my_dict
{'a': 1, 'b': 2, 'f': 25}

>>> my_dict.update({"c": 3, "d": 4, "e": 5})

>>> my_dict.values()
dict_values([1, 2, 25, 3, 4, 5])

>>> my_dict.clear()

>>> my_dict
{}
```

要了解更多关于字典方法的知识，我推荐[阅读文档中的这篇文章](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)。

## 🔸Python 运算符

太好了。现在您已经知道了 Python 中基本数据类型和内置数据结构的语法，所以让我们开始研究 Python 中的运算符。它们对于执行运算和形成表达式是必不可少的。

### Python 中的算术运算符

这些运算符是:

#### 加法:+

```
>>> 5 + 6
11

>>> 0 + 6
6

>>> 3.4 + 5.7
9.1

>>> "Hello" + ", " + "World"
'Hello, World'

>>> True + False
1
```

💡**提示:**最后两个例子很好奇吧？该运算符根据操作数的数据类型表现不同。

当它们是字符串时，该操作符连接字符串，当它们是布尔值时，它执行特定的操作。

在 Python 中，`True`相当于`1`,`False`相当于`0`。这就是为什么结果是`1 + 0 = 1`

#### 减法:-

```
>>> 5 - 6
-1

>>> 10 - 3
7

>>> 5 - 6
-1

>>> 4.5 - 5.6 - 2.3
-3.3999999999999995

>>> 4.5 - 7
-2.5

>>> - 7.8 - 6.2
-14.0
```

#### 乘法:*

```
>>> 5 * 6
30

>>> 6 * 7
42

>>> 10 * 100
1000

>>> 4 * 0
0

>>> 3.4 *6.8
23.119999999999997

>>> 4 * (-6)
-24

>>> (-6) * (-8)
48

>>> "Hello" * 4
'HelloHelloHelloHello'

>>> "Hello" * 0
''

>>> "Hello" * -1
''
```

**💡提示:**你可以将一个字符串“乘以”一个整数，以给定的次数重复该字符串。

#### 幂运算:**

```
>>> 6 ** 8
1679616

>>> 5 ** 2
25

>>> 4 ** 0
1

>>> 16 ** (1/2)
4.0

>>> 16 ** (0.5)
4.0

>>> 125 ** (1/3)
4.999999999999999

>>> 4.5 ** 2.3
31.7971929089206

>>> 3 ** (-1)
0.3333333333333333
```

#### 分部:/

```
>>> 25 / 5
5.0

>>> 3 / 6
0.5

>>> 0 / 5
0.0

>>> 2467 / 4673
0.5279263856195163

>>> 1 / 2
0.5

>>> 4.5 / 3.5
1.2857142857142858

>>> 6 / 7
0.8571428571428571

>>> -3 / -4
0.75

>>> 3 / -4
-0.75

>>> -3 / 4
-0.75
```

💡**提示:**这个操作符返回一个`float`作为结果，即使小数部分是`.0`

如果你试着除以`0`，你会得到一个`ZeroDivisionError`:

```
>>> 5 / 0
Traceback (most recent call last):
  File "<pyshell#109>", line 1, in <module>
    5 / 0
ZeroDivisionError: division by zero
```

#### 整数除法://

如果操作数是整数，则该运算符返回整数。如果它们是浮点数，结果将是一个小数部分为`.0`的浮点数，因为它截断了小数部分。

```
>>> 5 // 6
0

>>> 8 // 2
4

>>> -4 // -5
0

>>> -5 // 8
-1

>>> 0 // 5
0

>>> 156773 // 356
440
```

#### 模数:%

```
>>> 1 % 5
1

>>> 2 % 5
2

>>> 3 % 5
3

>>> 4 % 5
4

>>> 5 % 5
0

>>> 5 % 8
5

>>> 3 % 1
0

>>> 15 % 3
0

>>> 17 % 8
1

>>> 2568 % 4
0

>>> 245 % 15
5

>>> 0 % 6
0

>>> 3.5 % 2.4
1.1

>>> 6.7 % -7.8
-1.0999999999999996

>>> 2.3 % 7.5
2.3
```

#### 比较运算符

这些运算符是:

*   大于:`>`
*   大于或等于:`>=`
*   小于:`<`
*   小于或等于:`<=`
*   等于:`==`
*   不等于:`!=`

这些比较运算符生成计算结果为`True`或`False`的表达式。这里我们有一些例子:

```
>>> 5 > 6
False

>>> 10 > 8
True

>>> 8 > 8
False

>>> 8 >= 5
True

>>> 8 >= 8
True

>>> 5 < 6
True

>>> 10 < 8
False

>>> 8 < 8
False

>>> 8 <= 5
False

>>> 8 <= 8
True

>>> 8 <= 10
True

>>> 56 == 56
True

>>> 56 == 78
False

>>> 34 != 59
True

>>> 67 != 67
False
```

我们还可以使用它们根据字母顺序比较字符串:

```
>>> "Hello" > "World"
False
>>> "Hello" >= "World"
False
>>> "Hello" < "World"
True
>>> "Hello" <= "World"
True
>>> "Hello" == "World"
False
>>> "Hello" != "World"
True
```

我们通常使用它们来比较两个或更多变量的值:

```
>>> a = 1
>>> b = 2

>>> a < b
True

>>> a <= b
True

>>> a > b
False

>>> a >= b
False

>>> a == b
False

>>> a != b
True
```

💡**提示:**注意比较运算符是`==`，赋值运算符是`=`。它们的效果是不同的。`==`返回`True`或`False`，而`=`给变量赋值。

#### 比较运算符链接

在 Python 中，我们可以使用一种叫做“比较操作符链接”的东西，在这种东西中，我们链接比较操作符，以便更简洁地进行多个比较。

例如，检查`a`是否小于`b`以及`b`是否小于`c`:

```
a < b < c
```

这里我们有一些例子:

```
>>> a = 1
>>> b = 2
>>> c = 3

>>> a < b < c
True

>>> a > b > c
False

>>> a <= b <= c
True

>>> a >= b >= c
False

>>> a >= b > c
False

>>> a <= b < c
True
```

#### 逻辑运算符

Python 中有三种逻辑运算符:`and`、`or`和`not`。这些操作符中的每一个都有自己的真值表，它们对于处理条件语句是必不可少的。

`and`操作员:

```
>>> True and True
True

>>> True and False
False

>>> False and True
False

>>> False and False
False
```

`or`操作员:

```
>>> True or True
True

>>> True or False
True

>>> False or True
True

>>> False or False
False
```

`not`操作员:

```
>>> not True
False

>>> not False
True
```

这些运算符用于形成更复杂的表达式，这些表达式组合了不同的运算符和变量。

例如:

```
>>> a = 6
>>> b = 3

>>> a < 6 or b > 2
True

>>> a >= 3 and b >= 1
True

>>> (a + b) == 9 and b > 1
True

>>> ((a % 3) < 2) and ((a + b) == 3)
False
```

#### 赋值运算符

赋值运算符用于给变量赋值。

分别是:`=`、`+=`、`-=`、`*=`、`%=`、`/=`、`//=`、`**=`

*   `=`操作符将值赋给变量。
*   其他运算符使用变量的当前值和新值执行运算，并将结果赋给同一个变量。

例如:

```
>>> x = 3
>>> x
3

>>> x += 15
>>> x
18

>>> x -= 2
>>> x
16

>>> x *= 2
>>> x
32

>>> x %= 5
>>> x
2

>>> x /= 1
>>> x
2.0

>>> x //= 2
>>> x
1.0

>>> x **= 5
>>> x
1.0
```

💡**提示:**这些运算符在将结果赋给变量之前先进行位运算:`&=`、`|=`、`^=`、`>>=`、`<<=`。

#### 成员运算符

你可以用操作符`in`和`not in`检查一个元素是否在一个序列中。结果不是`True`就是`False`。

例如:

```
>>> 5 in [1, 2, 3, 4, 5]
True

>>> 8 in [1, 2, 3, 4, 5]
False

>>> 5 in (1, 2, 3, 4, 5)
True

>>> 8 in (1, 2, 3, 4, 5)
False

>>> "a" in {"a": 1, "b": 2}
True

>>> "c" in {"a": 1, "b": 2}
False

>>> "h" in "Hello"
False

>>> "H" in "Hello"
True

>>> 5 not in [1, 2, 3, 4, 5]
False

>>> 8 not in (1, 2, 3, 4, 5)
True

>>> "a" not in {"a": 1, "b": 2}
False

>>> "c" not in {"a": 1, "b": 2}
True

>>> "h" not in "Hello"
True

>>> "H" not in "Hello"
False
```

我们通常将它们与存储序列的变量一起使用，如下例所示:

```
>>> message = "Hello, World!"

>>> "e" in message
True
```

## 🔹Python 中的条件句

现在让我们看看如何根据条件是`True`还是`False`来编写条件，使代码的某些部分运行(或不运行)。

### `if`Python 中的语句

这是一个基本`if`语句的语法:

```
if <condition>:
    <code>
```

如果条件为`True`，代码将运行。否则，如果是`False`，代码就不会运行。

**💡提示:**第一行末尾有一个冒号(`:`)，代码缩进。这在 Python 中是必不可少的，以使代码属于条件。

这里我们有一些例子:

#### 虚假条件

```
x = 5

if x > 9:
    print("Hello, World!")
```

条件为`x > 9`，代码为`print("Hello, World!")`。

这种情况下，条件是`False`，所以没有输出。

#### 真实条件

这里我们有另一个例子。现在条件是`True`:

```
color = "Blue"

if color == "Blue":
    print("This is my favorite color")
```

输出是:

```
"This is my favorite color"
```

#### 条件后的代码

这里我们有一个在条件完成后运行的代码的例子。注意，最后一行没有缩进，这意味着它不属于条件行。

```
x = 5

if x > 9:
    print("Hello!")

print("End")
```

在这个例子中，条件`x > 9`是`False`，所以第一个打印语句不运行，但是最后一个打印语句运行，因为它不是条件的一部分，所以输出是:

```
End
```

但是，如果条件是`True`，如本例所示:

```
x = 15

if x > 9:
    print("Hello!")

print("End")
```

输出将是:

```
Hello!
End
```

#### 条件句的例子

这是条件句的另一个例子:

```
favorite_season = "Summer"

if favorite_season == "Summer":
    print("That is my favorite season too!")
```

在这种情况下，输出将是:

```
That is my favorite season too!
```

但是如果我们改变`favorite_season`的值:

```
favorite_season = "Winter"

if favorite_season == "Summer":
    print("That is my favorite season too!")
```

将不会有输出，因为条件将是`False`。

### `if/else`Python 中的语句

如果我们需要指定当条件为`False`时应该发生什么，我们可以在条件中添加一个`else`子句。

这是一般语法:

```
if <condition>:
    <code>
else:
    <code>
```

**💡提示:**注意这两个代码块是缩进的(`if`和`else`)。这对于 Python 能够区分属于主程序的代码和属于条件程序的代码是至关重要的。

让我们看一个关于`else`子句的例子:

#### 真实条件

```
x = 15

if x > 9:
    print("Hello!")
else:
    print("Bye!")

print("End")
```

输出是:

```
Hello!
End
```

当`if`子句的条件为`True`时，该子句运行。`else`条款不运行。

#### 虚假条件

现在`else`子句运行，因为条件是`False`。

```
x = 5

if x > 9:
    print("Hello!")
else:
    print("Bye!")

print("End")
```

现在的输出是:

```
Bye!
End
```

### `if/elif/else`Python 中的语句

为了进一步定制我们的条件，我们可以添加一个或多个`elif`子句来检查和处理多个条件。只有评估为`True`的第一个条件的代码才会运行。

**💡提示:** `elif`必须写在`if`之后`else`之前。

#### 第一个条件为真

```
x = 5

if x < 9:
    print("Hello!")
elif x < 15:
    print("It's great to see you")
else:
    print("Bye!")

print("End")
```

我们有两个条件`x < 9`和`x < 15`。只有从第一个条件开始的代码块，即从上到下的`True`，才会被执行。

在这种情况下，输出是:

```
Hello!
End
```

因为第一个条件是`True` : `x < 9`。

#### 第二个条件为真

如果第一个条件是`False`，那么将检查第二个条件。

在这个例子中，第一个条件`x < 9`是`False`，但是第二个条件`x < 15`是`True`，所以属于这个子句的代码将会运行。

```
x = 13

if x < 9:
    print("Hello!")
elif x < 15:
    print("It's great to see you")
else:
    print("Bye!")

print("End")
```

输出是:

```
It's great to see you
End
```

#### 所有条件都是假的

如果所有条件都是`False`，那么`else`子句将运行:

```
x = 25

if x < 9:
    print("Hello!")
elif x < 15:
    print("It's great to see you")
else:
    print("Bye!")

print("End")
```

输出将是:

```
Bye!
End
```

#### 多重 elif 子句

我们可以根据需要添加任意多的`elif`子句。这是一个带有两个`elif`子句的条件句示例:

```
if favorite_season == "Winter":
    print("That is my favorite season too")
elif favorite_season == "Summer":
    print("Summer is amazing")
elif favorite_season == "Spring":
    print("I love spring")
else:
    print("Fall is my mom's favorite season")
```

将检查每个条件，并且仅运行评估为`True`的第一个条件的代码块。如果它们都不是`True`，那么`else`子句将会运行。

## 🔸Python 中的 For 循环

现在你知道如何用 Python 写条件语句了，所以让我们开始进入循环。For 循环是令人惊叹的编程结构，您可以使用它将代码块重复特定的次数。

以下是用 Python 编写 for 循环的基本语法:

```
for <loop_variable> in <iterable>:
    <code>
```

iterable 可以是列表、元组、字典、字符串、range 返回的序列、文件或 Python 中任何其他类型的 iterable。我们从`range()`开始。

### Python 中的`range()`函数

这个函数返回一个整数序列，我们可以用它来确定循环的迭代次数。循环将对每个整数完成一次迭代。

**💡提示:**每个整数被分配给循环变量，每次迭代一个。

这是用`range()`编写 for 循环的一般语法:

```
for <loop_variable> in range(<start>, <stop>, <step>):
    <code>
```

如您所见，范围函数有三个参数:

*   `start`:整数序列开始的地方。默认是`0`。
*   `stop`:整数序列将停止的位置(不包括该值)。
*   `step`:将被加到每个元素上以获得序列中下一个元素的值。默认是`1`。

您可以向`range()`传递 1、2 或 3 个参数:

*   有 1 个参数时，该值分配给`stop`参数，并使用其他两个参数的默认值。
*   有两个参数时，这些值被分配给`start`和`stop`参数，并使用`step`的默认值。
*   有 3 个参数时，这些值被分配给`start`、`stop`和`step`参数(按顺序)。

这里我们有一些带有**一个参数**的例子:

```
for i in range(5):
    print(i)
```

输出:

```
0
1
2
3
4
```

💡**提示:**循环变量自动更新。

```
>>> for j in range(15):
    print(j * 2)
```

输出:

```
0
2
4
6
8
10
12
14
16
18
20
22
24
26
28
```

在下面的例子中，我们按照循环变量的值所指示的次数重复一个字符串:

```
>>> for num in range(8):
	print("Hello" * num)
```

输出:

```
Hello
HelloHello
HelloHelloHello
HelloHelloHelloHello
HelloHelloHelloHelloHello
HelloHelloHelloHelloHelloHello
HelloHelloHelloHelloHelloHelloHello
```

我们还可以使用带有内置数据结构(如列表)的 for 循环:

```
>>> my_list = ["a", "b", "c", "d"]

>>> for i in range(len(my_list)):
	print(my_list[i]) 
```

输出:

```
a
b
c
d
```

💡**提示:**当你使用`range(len(<seq>))`时，你会得到一个从`0`到`len(<seq>)-1`的数字序列。这表示有效索引的序列。

以下是一些带有两个参数的例子:

```
>>> for i in range(2, 10):
	print(i)
```

输出:

```
2
3
4
5
6
7
8
9
```

**代码:**

```
>>> for j in range(2, 5):
	print("Python" * j)
```

输出:

```
PythonPython
PythonPythonPython
PythonPythonPythonPython
```

**代码:**

```
>>> my_list = ["a", "b", "c", "d"]

>>> for i in range(2, len(my_list)):
	print(my_list[i])
```

输出:

```
c
d
```

**代码:**

```
>>> my_list = ["a", "b", "c", "d"]

>>> for i in range(2, len(my_list)-1):
	my_list[i] *= i
```

现在的名单是:`['a', 'b', 'cc', 'd']`

以下是一些带有三个参数的例子:

```
>>> for i in range(3, 16, 2):
	print(i)
```

输出:

```
3
5
7
9
11
13
15
```

**代码:**

```
>>> for j in range(10, 5, -1):
	print(j)
```

输出:

```
10
9
8
7
6
```

**代码:**

```
>>> my_list = ["a", "b", "c", "d", "e", "f", "g"]

>>> for i in range(len(my_list)-1, 2, -1):
	print(my_list[i])
```

输出:

```
g
f
e
d
```

### 如何在 Python 中迭代 Iterables

我们可以使用 for 循环直接迭代列表、元组、字典、字符串和文件等可迭代对象。我们将在每次迭代中一次一个地获取它们的每一个元素。这对于直接与他们合作非常有帮助。

让我们看一些例子:

#### 迭代一个字符串

如果我们迭代一个字符串，它的字符会被一个一个地赋给循环变量(包括空格和符号)。

```
>>> message = "Hello, World!"

>>> for char in message:
	print(char)

H
e
l
l
o
,

W
o
r
l
d
!
```

我们还可以通过调用一个 string 方法迭代修改后的字符串副本，在 for 循环中指定 iterable。这将把字符串的副本指定为 iterable，用于迭代，如下所示:

```
>>> word = "Hello"

>>> for char in word.lower(): # calling the string method
	print(char)

h
e
l
l
o
```

```
>>> word = "Hello"

>>> for char in word.upper(): # calling the string method
	print(char)

H
E
L
L
O
```

#### 遍历列表和元组

```
>>> my_list = [2, 3, 4, 5]

>>> for num in my_list:
	print(num)
```

输出是:

```
2
3
4
5
```

**代码:**

```
>>> my_list = (2, 3, 4, 5)

>>> for num in my_list:
	if num % 2 == 0:
		print("Even")
	else:
		print("Odd")
```

输出:

```
Even
Odd
Even
Odd
```

### 迭代字典的键、值和键值对

我们可以通过调用特定的字典方法来迭代字典的键、值和键值对。让我们看看怎么做。

为了**迭代** **键**，我们编写:

```
for <var> in <dictionary_variable>:
    <code>
```

我们只需写出将字典存储为 iterable 的变量的名称。

**💡提示:**你也可以写`<dictionary_variable>.keys()`，但是直接写变量的名字更简洁，效果也完全一样。

例如:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3}

>>> for key in my_dict:
	print(key)

a
b
c
```

**💡提示:**您可以为循环变量指定任何有效的名称。

为了**迭代** **值**，我们使用:

```
for <var> in <dictionary_variable>.values():
    <code>
```

例如:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3}

>>> for value in my_dict.values():
	print(value)

1
2
3
```

为了**迭代** **键值对**，我们使用:

```
for <key>, <value> in <dictionary_variable>.items():
    <code>
```

💡**提示:**我们正在定义两个循环变量，因为我们想将键和值赋给可以在循环中使用的变量。

```
>>> my_dict = {"a": 1, "b": 2, "c": 3}

>>> for key, value in my_dict.items():
	print(key, value)

a 1
b 2
c 3
```

如果我们只定义一个循环变量，这个变量将包含一个具有键值对的元组:

```
>>> my_dict = {"a": 1, "b": 2, "c": 3}
>>> for pair in my_dict.items():
	print(pair)

('a', 1)
('b', 2)
('c', 3)
```

### Python 中的中断并继续

现在你知道如何在 Python 中迭代序列了。我们还有循环控制语句来定制循环运行时发生的事情:`break`和`continue`。

#### 中断语句

`break`语句用于立即停止循环。

当发现一个`break`语句时，循环停止，程序返回到循环之外的正常执行。

在下面的例子中，当发现一个偶数元素时，我们停止循环。

```
>>> my_list = [1, 2, 3, 4, 5]

>>> for elem in my_list:
	if elem % 2 == 0:
		print("Even:", elem)
		print("break")
		break
	else:
		print("Odd:", elem)

Odd: 1
Even: 2
break
```

#### Continue 语句

`continue`语句用于跳过当前迭代的剩余部分。

当在循环执行期间找到它时，当前迭代停止，新的迭代从循环变量的更新值开始。

在下面的例子中，如果元素是偶数，我们跳过当前迭代，如果元素是奇数，我们只打印值:

```
>>> my_list = [1, 2, 3, 4, 5]

>>> for elem in my_list:
	if elem % 2 == 0:
		print("continue")
		continue
	print("Odd:", elem)

Odd: 1
continue
Odd: 3
continue
Odd: 5
```

### Python 中的 zip()函数

`zip()`是一个令人惊叹的内置函数，我们可以在 Python 中使用它来一次迭代多个序列，在每次迭代中获得它们对应的元素。

我们只需要将序列作为参数传递给`zip()`函数，并在循环中使用这个结果。

例如:

```
>>> my_list1 = [1, 2, 3, 4]
>>> my_list2 = [5, 6, 7, 8]

>>> for elem1, elem2 in zip(my_list1, my_list2):
	print(elem1, elem2)

1 5
2 6
3 7
4 8
```

### Python 中的 enumerate()函数

您还可以在循环运行时使用`enum()`函数跟踪计数器。它通常用于迭代序列并获取相应的索引。

**💡提示:**默认情况下，计数器从`0`开始计数。

例如:

```
>>> my_list = [5, 6, 7, 8]

>>> for i, elem in enumerate(my_list):
	print(i, elem)

0 5
1 6
2 7
3 8
```

```
>>> word = "Hello"

>>> for i, char in enumerate(word):
	print(i, char)

0 H
1 e
2 l
3 l
4 o
```

如果您从`0`开始计数器，您可以在同一次迭代中使用索引和当前值来修改序列:

```
>>> my_list = [5, 6, 7, 8]

>>> for index, num in enumerate(my_list):
	my_list[index] = num * 3

>>> my_list
[15, 18, 21, 24]
```

您可以通过向`enumerate()`传递第二个参数来从不同的数字开始计数:

```
>>> word = "Hello"

>>> for i, char in enumerate(word, 2):
	print(i, char)

2 H
3 e
4 l
5 l
6 o
```

#### else 子句

For 循环也有一个`else`子句。如果您希望在循环完成所有迭代而没有找到`break`语句时运行特定的代码块，那么您可以将该子句添加到循环中。

**💡提示:**如果找到了`break`，则`else`子句不运行，如果没有找到`break`，则`else`子句运行。

在下面的例子中，我们试图在列表中找到一个大于 6 的元素。没有找到那个元素，所以`break`不运行，而`else`子句运行。

```
my_list = [1, 2, 3, 4, 5]

for elem in my_list:
    if elem > 6:
        print("Found")
        break
else:
    print("Not Found")
```

输出是:

```
Not Found
```

但是，如果`break`语句运行，那么`else`子句不会运行。我们可以在下面的例子中看到这一点:

```
my_list = [1, 2, 3, 4, 5, 8] # Now the list has the value 8

for elem in my_list:
    if elem > 6:
        print("Found")
        break
else:
    print("Not Found")
```

输出是:

```
Found
```

## 🔹Python 中的 While 循环

While 循环类似于 for 循环，因为它们让我们重复一段代码。不同之处在于，while 循环在条件为`True`时运行。

在 while 循环中，我们定义条件，而不是迭代次数。当条件为`False`时，循环停止。

这是 while 循环的一般语法:

```
while <condition>:
    <code>
```

💡**提示:**在 while 循环中，您必须更新作为条件一部分的变量，以确保条件最终成为`False`。

例如:

```
>>> x = 6

>>> while x < 15:
	print(x)
	x += 1

6
7
8
9
10
11
12
13
14
```

```
>>> x = 4

>>> while x >= 0:
	print("Hello" * x)
	x -= 1

HelloHelloHelloHello
HelloHelloHello
HelloHello
Hello
```

```
>>> num = 5

>>> while num >= 1:
	print("*" * num)
	num -= 2

*****
***
*
```

#### 中断并继续

我们也可以在 while 循环中使用`break`和`continue`，它们的工作方式完全相同:

*   `break`立即停止 while 循环。
*   `continue`停止当前迭代并开始下一次迭代。

例如:

```
>>> x = 5

>>> while x < 15:
	if x % 2 == 0:
		print("Even:", x)
		break
	print(x)
	x += 1

5
Even: 6
```

```
>>> x = 5

>>> while x < 15:
	if x % 2 == 0:
		x += 1
		continue
	print("Odd:", x)
	x += 1

Odd: 5
Odd: 7
Odd: 9
Odd: 11
Odd: 13
```

#### `else`条款

我们还可以在 while 循环中添加一个`else`子句。如果找到了`break`，则不运行`else`子句，但是如果没有找到`break`语句，则运行`else`子句。

在下面的示例中，找不到`break`语句，因为在条件变为`False`之前没有一个数字是偶数，所以运行`else`子句。

```
x = 5

while x < 15:
	if x % 2 == 0:
		print("Even number found")
		break
	print(x)
	x += 2
else:
	print("All numbers were odd")
```

这是输出:

```
5
7
9
11
13
All numbers were odd
```

但是在这个版本的示例中，找到了`break`语句，而`else`子句没有运行:

```
x = 5

while x < 15:
	if x % 2 == 0:
		print("Even number found")
		break
	print(x)
	x += 1 # Now we are incrementing the value by 1
else:
	print("All numbers were odd")
```

输出是:

```
5
Even number found
```

#### 无限 While 循环

当我们编写和使用 while 循环时，我们可能会遇到一个“无限循环”如果条件不为`False`，循环将不会在没有外部干预的情况下停止。

当条件中的变量在循环执行期间没有正确更新时，通常会发生这种情况。

**💡提示:**您必须对这些变量进行必要的更新，以确保条件最终评估为`False`。

例如:

```
>>> x = 5

>>> while x > 2:
	print(x)

5
5
5
5
5
5
5
5
5
.
.
.
# The output continues indefinitely
```

💡**提示:**要停止这个进程，键入`CTRL + C`。您应该会看到一条`KeyboardInterrupt`消息。

## 🔸Python 中的嵌套循环

我们可以在 for 循环中编写 for 循环，在 while 循环中编写 while 循环。这些内部循环称为嵌套循环。

💡**提示:**内循环为外循环的每次迭代运行。

### Python 中嵌套的 For 循环

```
>>> for i in range(3):
	for j in range(2):
		print(i, j)

0 0
0 1
1 0
1 1
2 0
2 1
```

如果我们添加打印报表，我们可以看到幕后发生的事情:

```
>>> for i in range(3):
	print("===> Outer Loop")
	print(f"i = {i}")
	for j in range(2):
		print("Inner Loop")
		print(f"j = {j}")

===> Outer Loop
i = 0
Inner Loop
j = 0
Inner Loop
j = 1
===> Outer Loop
i = 1
Inner Loop
j = 0
Inner Loop
j = 1
===> Outer Loop
i = 2
Inner Loop
j = 0
Inner Loop
j = 1
```

内循环在外循环的每次迭代中完成两次迭代。当新的迭代开始时，循环变量被更新。

这是另一个例子:

```
>>> num_rows = 5

>>> for i in range(5):
	for num_cols in range(num_rows-i):
		print("*", end="")
	print()

*****
****
***
**
*
```

### Python 中嵌套的 While 循环

这里我们有一个嵌套 while 循环的例子。在这种情况下，我们必须更新作为每个条件一部分的变量，以保证循环停止。

```
>>> i = 5

>>> while i > 0:
	j = 0
	while j < 2:
		print(i, j)
		j += 1
	i -= 1

5 0
5 1
4 0
4 1
3 0
3 1
2 0
2 1
1 0
1 1
```

💡提示:我们也可以在 while 循环中使用 for 循环，在 for 循环中使用 while 循环。

## 🔹Python 中的函数

在 Python 中，我们可以定义函数来使我们的代码可重用、更可读、更有组织。这是 Python 函数的基本语法:

```
def <function_name>(<param1>, <param2>, ...):
    <code>
```

**💡提示:**一个函数可以有零个、一个或多个参数。

### Python 中不带参数的函数

在函数定义中，不带参数的函数在其名称后有一对空括号。例如:

```
def print_pattern():
    size = 4
    for i in range(size):
        print("*" * size)
```

这是我们调用函数时的输出:

```
>>> print_pattern()
****
****
****
****
```

**💡提示:**你必须在函数名后面写一对空括号来调用它。

### Python 中的单参数函数

在函数定义中，带有一个或多个参数的函数在其名称后有一个用括号括起来的参数列表:

```
def welcome_student(name):
    print(f"Hi, {name}! Welcome to class.")
```

当我们调用函数时，我们只需要传递一个值作为参数，该值将在我们使用函数定义中的参数时被替换:

```
>>> welcome_student("Nora")
Hi, Nora! Welcome to class.
```

这里我们有另一个例子——一个打印带有星号的图案的函数。您必须指定要打印的行数:

```
def print_pattern(num_rows):
    for i in range(num_rows):
        for num_cols in range(num_rows-i):
            print("*", end="")
        print()
```

您可以看到`num_rows`不同值的不同输出:

```
>>> print_pattern(3)
***
**
*

>>> print_pattern(5)
*****
****
***
**
*

>>> print_pattern(8)
********
*******
******
*****
****
***
**
*
```

### Python 中带有两个或更多参数的函数

要定义两个或多个参数，我们只需用逗号分隔它们:

```
def print_sum(a, b):
    print(a + b) 
```

当我们调用这个函数时，我们必须传递两个参数:

```
>>> print_sum(4, 5)
9

>>> print_sum(8, 9)
17

>>> print_sum(0, 0)
0

>>> print_sum(3, 5)
8
```

我们可以将刚刚看到的带有一个参数的函数修改为带有两个参数，并打印带有自定义字符的图案:

```
def print_pattern(num_rows, char):
	for i in range(num_rows):
		for num_cols in range(num_rows-i):
			print(char, end="")
		print()
```

您可以看到带有自定义字符的输出是我们调用传递两个参数的函数:

```
>>> print_pattern(5, "A")
AAAAA
AAAA
AAA
AA
A

>>> print_pattern(8, "%")
%%%%%%%%
%%%%%%%
%%%%%%
%%%%%
%%%%
%%%
%%
%

>>> print_pattern(10, "#")
##########
#########
########
#######
######
#####
####
###
##
#
```

### 如何在 Python 中返回值

太棒了。现在你知道了如何定义一个函数，那么让我们看看如何使用 return 语句。

我们经常需要从函数中返回值。我们可以用 Python 中的`return`语句做到这一点。我们只需要把这个写在函数定义里:

```
return <value_to_return>
```

**💡提示:**找到`return`并返回值后，函数立即停止。

这里我们有一个例子:

```
def get_rectangle_area(length, width):
    return length * width
```

现在我们可以调用函数并将结果赋给一个变量，因为结果是由函数返回的:

```
>>> area = get_rectangle_area(4, 5)
>>> area
20
```

我们也可以将`return`与条件一起使用，根据条件是`True`还是`False`来返回值。

在此示例中，函数返回序列中找到的第一个偶数元素:

```
def get_first_even(seq):
    for elem in seq:
        if elem % 2 == 0:
            return elem
    else:
        return None
```

如果我们调用该函数，我们可以看到预期的结果:

```
>>> value1 = get_first_even([2, 3, 4, 5])
>>> value1
2
```

```
>>> value2 = get_first_even([3, 5, 7, 9])
>>> print(value2)
None
```

💡**提示:**如果一个函数没有`return`语句或者在执行过程中找不到，默认返回`None`。

Python 代码的[风格指南建议一致地使用 return 语句。它提到我们应该:](https://www.python.org/dev/peps/pep-0008/#programming-recommendations)

> 在返回声明中保持一致。要么函数中的所有 return 语句都应该返回表达式，要么都不应该返回表达式。如果任何 return 语句返回一个表达式，任何没有返回值的 return 语句都应该显式地声明这是 return None，并且一个显式的 return 语句应该出现在函数的末尾(如果可以到达的话)

### Python 中的默认参数

我们可以为函数的参数指定默认参数。为此，我们只需在参数列表中写入`<parameter>=<value>`。

**💡提示:**Python 代码的[风格指南](https://www.python.org/dev/peps/pep-0008/#other-recommendations)提到我们不应该“在=符号周围使用空格来表示关键字参数”

在本例中，我们将默认值 5 赋给参数`b`。如果我们在调用函数时省略了这个值，将使用默认值。

```
def print_product(a, b=5):
    print(a * b)
```

如果我们调用不带此参数的函数，您可以看到输出:

```
>>> print_product(4)
20
```

我们确认在操作中使用了默认参数 5。

但是我们也可以通过传递第二个参数为`b`指定一个自定义值:

```
>>> print_product(3, 4)
12
```

💡**提示:**带有默认参数的参数必须在参数列表的末尾定义。否则，你会看到这个错误:`SyntaxError: non-default argument follows default argument`。

这里我们有另一个例子，用我们写的函数来打印一个图案。我们将默认值`"*"`分配给`char`参数。

```
def print_pattern(num_rows, char="*"):
	for i in range(num_rows):
		for num_cols in range(num_rows-i):
			print(char, end="")
		print()
```

现在，我们可以选择使用默认值或自定义它:

```
>>> print_pattern(5)
*****
****
***
**
*

>>> print_pattern(6, "&")
&&&&&&
&&&&&
&&&&
&&&
&&
&
```

## 🔸Python 中的递归

递归函数是调用自身的函数。这些函数有一个停止递归过程的基本用例，以及一个通过进行另一个递归调用来继续递归过程的递归用例。

这里我们有一些 Python 中的例子:

```
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n-1)
```

Recursive Factorial Function

```
def fibonacci(n):
    if n == 0 or n == 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```

The Fibonacci Function

```
def find_power(a, b):
    if b == 0:
        return 1
    else:
        return a * find_power(a, b-1)
```

Find a Power Recursively

## 🔹Python 中的异常处理

程序运行时发生的错误或意外事件称为**异常**。感谢我们稍后将看到的元素，我们可以避免在这种情况发生时突然终止程序。

让我们看看 Python 中异常的类型以及如何处理它们。

### Python 中的常见异常

以下是 Python 中常见异常及其发生原因的列表:

*   **ZeroDivisionError:** 当除法或模运算的第二个参数为零时引发。

```
>>> 5 / 0
Traceback (most recent call last):
  File "<pyshell#0>", line 1, in <module>
    5 / 0
ZeroDivisionError: division by zero

>>> 7 // 0
Traceback (most recent call last):
  File "<pyshell#1>", line 1, in <module>
    7 // 0
ZeroDivisionError: integer division or modulo by zero

>>> 8 % 0
Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    8 % 0
ZeroDivisionError: integer division or modulo by zero
```

*   **IndexError:** 当我们试图使用无效的索引来访问序列的元素时引发。

```
>>> my_list = [3, 4, 5, 6]

>>> my_list[15]
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    my_list[15]
IndexError: list index out of range
```

*   **KeyError:** 当我们试图访问一个因为键不在字典中而不存在的键-值对时引发。

```
>>> my_dict = {"a": 1, "b": 2, "c": 3}

>>> my_dict["d"]
Traceback (most recent call last):
  File "<pyshell#6>", line 1, in <module>
    my_dict["d"]
KeyError: 'd'
```

*   **NameError:** 当我们使用一个之前没有定义的变量时引发。

```
>>> b
Traceback (most recent call last):
  File "<pyshell#8>", line 1, in <module>
    b
NameError: name 'b' is not defined 
```

*   **RecursionError:** 当解释器检测到超过最大递归深度时引发。这通常发生在流程从未到达基本案例时。

在下面的例子中，我们将得到一个`RecursionError`。`factorial`函数是递归实现的，但是传递给递归调用的参数是`n`而不是`n-1`。除非值已经是`0`或`1`，否则将不会到达基本情况，因为参数没有递减，所以过程将继续，我们将得到这个错误。

```
>>> def factorial(n):
	if n == 0 or n == 1:
		return 1
	else:
		return n * factorial(n)

>>> factorial(5)
Traceback (most recent call last):
  File "<pyshell#6>", line 1, in <module>
    factorial(5)
  File "<pyshell#5>", line 5, in factorial
    return n * factorial(n)
  File "<pyshell#5>", line 5, in factorial
    return n * factorial(n)
  File "<pyshell#5>", line 5, in factorial
    return n * factorial(n)
  [Previous line repeated 1021 more times]
  File "<pyshell#5>", line 2, in factorial
    if n == 0 or n == 1:
RecursionError: maximum recursion depth exceeded in comparison
```

💡**提示:**要了解关于这些异常的更多信息，我推荐阅读文档中的[这篇文章](https://docs.python.org/3/library/exceptions.html)。

### Python 中的`try` / `except`

我们可以在 Python 中使用 try/except 来捕捉出现的异常，并适当地处理它们。这样，程序可以适当地终止，甚至从异常中恢复。

这是基本语法:

```
try:
    <code_that_may_raise_an_exception>
except:
    <code_to_handle_the_exception_if_it_occurs> 
```

例如，如果我们接受用户输入来访问列表中的元素，输入可能不是有效的索引，因此可能会引发异常:

```
index = int(input("Enter the index: "))

try:
    my_list = [1, 2, 3, 4]
    print(my_list[index])
except:
    print("Please enter a valid index.")
```

如果我们输入一个像 15 这样的无效值，输出将是:

```
Please enter a valid index.
```

因为`except`子句运行。然而，如果该值有效，那么`try`中的代码将按预期运行。

这里我们有另一个例子:

```
a = int(input("Enter a: "))
b = int(input("Enter b: "))

try:
    division = a / b
    print(division)
except:
    print("Please enter valid values.")
```

输出是:

```
Enter a: 5
Enter b: 0

Please enter valid values.
```

### 如何在 Python 中捕捉特定类型的异常

我们可以捕捉并处理特定类型的异常，而不是捕捉并处理所有可能出现在`try`子句中的异常。我们只需要在关键字`except`后指定异常的类型:

```
try:
    <code_that_may_raise_an_exception>
except <exception_type>:
    <code_to_handle_an_exception_if_it_occurs> 
```

例如:

```
index = int(input("Enter the index: "))

try:
    my_list = [1, 2, 3, 4]
    print(my_list[index])
except IndexError: # specify the type
    print("Please enter a valid index.")
```

```
a = int(input("Enter a: "))
b = int(input("Enter b: "))

try:
    division = a / b
    print(division)
except ZeroDivisionError: # specify the type
    print("Please enter valid values.")
```

### 如何在 Python 中给异常对象赋值

我们可以为异常对象指定一个名称，方法是将它赋给一个可以在`except`子句中使用的变量。这将让我们访问它的描述和属性。

我们只需要加上`as <name>`，像这样:

```
try:
    <code_that_may_raise_an_exception>
except <exception_type> as <name>:
    <code_to_handle_an_exception_if_it_occurs> 
```

例如:

```
index = int(input("Enter the index: "))

try:
    my_list = [1, 2, 3, 4]
    print(my_list[index])
except IndexError as e:
    print("Exception raised:", e)
```

如果我们输入`15`作为索引，这就是输出:

```
Enter the index: 15
Exception raised: list index out of range
```

这是另一个例子:

```
a = int(input("Enter a: "))
b = int(input("Enter b: "))

try:
    division = a / b
    print(division)
except ZeroDivisionError as err:
    print("Please enter valid values.", err) 
```

如果我们为`b`输入值`0`，这就是输出:

```
Please enter valid values. division by zero
```

### Python 中的`try` / `except` / `else`

如果我们想选择在执行`try`子句期间没有异常发生时会发生什么，我们可以在这个结构的`except`之后添加一个`else`子句:

```
try:
    <code_that_may_raise_an_exception>
except:
    <code_to_handle_an_exception_if_it_occurs>
else:
    <code_that_only_runs_if_no_exception_in_try> 
```

例如:

```
a = int(input("Enter a: "))
b = int(input("Enter b: "))

try:
    division = a / b
    print(division)
except ZeroDivisionError as err:
    print("Please enter valid values.", err)
else:
    print("Both values were valid.")
```

如果我们分别为`a`和`b`输入值`5`和`0`，则输出为:

```
Please enter valid values. division by zero
```

但是如果两个值都有效，例如`a`和`b`的`5`和`4`，则`else`子句在`try`完成后运行，我们看到:

```
1.25
Both values were valid.
```

### Python 中的`try` / `except` / `else` / `finally`

如果我们需要运行应该一直运行的代码，即使在`try`中出现异常，我们也可以添加一个`finally`子句。

例如:

```
a = int(input("Enter a: "))
b = int(input("Enter b: "))

try:
    division = a / b
    print(division)
except ZeroDivisionError as err:
    print("Please enter valid values.", err)
else:
    print("Both values were valid.")
finally:
    print("Finally!")
```

如果两个值都有效，则输出是除法的结果，并且:

```
Both values were valid.
Finally!
```

如果因为`b`是`0`而引发异常，我们会看到:

```
Please enter valid values. division by zero
Finally!
```

`finally`子句总是运行。

**💡提示:**这个子句可以用来关闭文件，即使代码抛出异常。

## 🔸Python 中的面向对象编程

在面向对象编程(OOP)中，我们定义了充当蓝图的类，用属性和方法(与对象相关的功能)在 Python 中创建对象。

这是定义类的一般语法:

```
class <ClassName>:

    <class_attribute_name> = <value>

    def __init__(self,<param1>, <param2>, ...):
        self.<attr1> = <param1>
        self.<attr2> = <param2>
        .
        .
        .
        # As many attributes as needed

   def <method_name>(self, <param1>, ...):
       <code>

   # As many methods as needed
```

**💡提示:** `self`指的是类的一个实例(用类蓝图创建的一个对象)。

如您所见，一个类可以有许多不同的元素，所以让我们详细分析它们:

### 类别标题

类定义的第一行有`class`关键字和类名:

```
class Dog:
```

```
class House: 
```

```
class Ball:
```

**💡提示:**如果这个类继承了另一个类的属性和方法，我们会看到这个类的名字在括号中:

```
class Poodle(Dog):
```

```
class Truck(Vehicle):
```

```
class Mom(FamilyMember):
```

在 Python 中，我们用大写 Camel Case(也称为 Pascal Case)写类名，其中每个单词都以大写字母开头。例如:`FamilyMember`

### `__init__`和实例属性

我们将使用该类在 Python 中创建对象，就像我们根据蓝图建造真正的房子一样。

对象将拥有我们在类中定义的属性。通常，我们在`__init__`中初始化这些属性。这是一个在我们创建类的实例时运行的方法。

这是一般语法:

```
def __init__(self, <parameter1>, <parameter2>, ...):
        self.<attribute1> = <parameter1>  # Instance attribute
        self.<attribute2> = <parameter2>  # Instance attribute
        .
        .
        .
        # As many instance attributes as needed
```

我们根据需要指定尽可能多的参数来定制将要创建的对象的属性值。

下面是一个使用这个方法的`Dog`类的例子:

```
class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

💡**提示:**注意名字`__init__`中的双前导和尾部下划线。

### 如何创建实例

为了创建一个`Dog`的实例，我们需要指定 dog 实例的名称和年龄，以将这些值赋给属性:

```
my_dog = Dog("Nora", 10)
```

太好了。现在我们已经准备好在程序中使用我们的实例。

有些类不需要任何参数来创建实例。在这种情况下，我们只写空括号。例如:

```
class Circle:

    def __init__(self):
        self.radius = 1
```

要创建实例，请执行以下操作:

```
>>> my_circle = Circle()
```

💡**提示:** `self`就像一个“在幕后”活动的参数，所以即使你在方法定义中看到它，你也不应该在传递实参时考虑它。

### 默认参数

我们还可以为属性分配默认值，如果用户想要自定义值，我们可以为他们提供选项。

在这种情况下，我们将在参数列表中写入`<attribute>=<value>`。

这是一个例子:

```
class Circle:

    def __init__(self, radius=1):
        self.radius = radius
```

现在，我们可以通过省略值来创建一个具有半径默认值的`Circle`实例，或者通过传递一个值来定制它:

```
# Default value
>>> my_circle1 = Circle()

# Customized value
>>> my_circle2 = Circle(5)
```

### 如何获取实例属性

要访问实例属性，我们使用以下语法:

```
<object_variable>.<attribute>
```

例如:

```
# Class definition
>>> class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age

# Create instance        
>>> my_dog = Dog("Nora", 10)

# Get attributes
>>> my_dog.name
'Nora'

>>> my_dog.age
10
```

### 如何更新实例属性

要更新实例属性，我们使用以下语法:

```
<object_variable>.<attribute> = <new_value>
```

例如:

```
>>> class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> my_dog = Dog("Nora", 10)

>>> my_dog.name
'Nora'

# Update the attribute
>>> my_dog.name = "Norita"

>>> my_dog.name
'Norita'
```

### 如何删除实例属性

要删除实例属性，我们使用以下语法:

```
del <object_variable>.<attribute>
```

例如:

```
>>> class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> my_dog = Dog("Nora", 10)

>>> my_dog.name
'Nora'

# Delete this attribute
>>> del my_dog.name

>>> my_dog.name
Traceback (most recent call last):
  File "<pyshell#77>", line 1, in <module>
    my_dog.name
AttributeError: 'Dog' object has no attribute 'name'
```

### 如何删除实例

类似地，我们可以使用`del`删除一个实例:

```
>>> class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> my_dog = Dog("Nora", 10)

>>> my_dog.name
'Nora'

# Delete the instance
>>> del my_dog

>>> my_dog
Traceback (most recent call last):
  File "<pyshell#79>", line 1, in <module>
    my_dog
NameError: name 'my_dog' is not defined
```

### Python 中的公共和非公共属性

在 Python 中，我们没有访问修饰符来从功能上限制对实例属性的访问，所以我们依靠命名约定来指定这一点。

例如，通过添加前导下划线，我们可以向其他开发人员发出信号，表明某个属性应该是非公共的。

例如:

```
class Dog:

    def __init__(self, name, age):
        self.name = name  # Public attribute
        self._age = age   # Non-Public attribute
```

Python 文档提到:

> 仅对非公共方法和实例变量使用一个前导下划线。
> 
> 总是决定一个类的方法和实例变量(统称为:“属性”)应该是公共的还是非公共的。如有疑问，选择非公；稍后将其公开比将其公开更容易。
> 
> 非公共属性是不打算让第三方使用的属性；您不能保证非公共属性不会改变，甚至不会被删除。——[来源](https://www.python.org/dev/peps/pep-0008/#designing-for-inheritance)

然而，正如文件中也提到的:

> 我们在这里不使用术语“私有”，因为在 Python 中没有属性是真正私有的(没有不必要的工作)。- [来源](https://www.python.org/dev/peps/pep-0008/#designing-for-inheritance)

**💡技巧:**从技术上讲，如果我们在属性名中添加前导下划线，我们仍然可以访问和修改该属性，但是我们不应该这样做。

### Python 中的类属性

类属性由该类的所有实例共享。他们都可以访问该属性，并且也会受到对这些属性所做的任何更改的影响。

```
class Dog:

    # Class attributes
    kingdom = "Animalia"
    species = "Canis lupus"

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

**💡提示:**通常，它们是在`__init__`方法之前写的。

### 如何获得一个类属性

要获得类属性的值，我们使用以下语法:

```
<class_name>.<attribute>
```

例如:

```
>>> class Dog:

    kingdom = "Animalia"

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> Dog.kingdom
'Animalia'
```

**💡提示:**您也可以在类中使用这个语法。

### 如何更新类属性

要更新类属性，我们使用以下语法:

```
<class_name>.<attribute> = <value>
```

例如:

```
>>> class Dog:

    kingdom = "Animalia"

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> Dog.kingdom
'Animalia'

>>> Dog.kingdom = "New Kingdom"

>>> Dog.kingdom
'New Kingdom'
```

### 如何删除类属性

我们使用`del`来删除一个类属性。例如:

```
>>> class Dog:

    kingdom = "Animalia"

    def __init__(self, name, age):
        self.name = name
        self.age = age

>>> Dog.kingdom
'Animalia'

# Delete class attribute
>>> del Dog.kingdom

>>> Dog.kingdom
Traceback (most recent call last):
  File "<pyshell#88>", line 1, in <module>
    Dog.kingdom
AttributeError: type object 'Dog' has no attribute 'kingdom'
```

### 如何定义方法

方法表示该类实例的功能。

**💡提示:**如果我们在方法定义中写下`self.<attribute>`，实例方法可以使用调用方法的实例的属性。

这是类中方法的基本语法。它们通常位于`__init__`下方:

```
class <ClassName>:

    # Class attributes

    # __init__

    def <method_name>(self, <param1>, ...):
        <code>
```

如果需要，它们可以有零个、一个或多个参数(就像函数一样！)但是实例方法必须总是将`self`作为第一个参数。

例如，下面是一个没有参数的`bark`方法(除了`self`):

```
class Dog:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        print(f"woof-woof. I'm {self.name}")
```

要调用此方法，我们使用以下语法:

```
<object_variable>.<method>(<arguments>)
```

例如:

```
# Create the instance
>>> my_dog = Dog("Nora", 10)

# Call the method
>>> my_dog.bark()
woof-woof. I'm Nora
```

这里我们有一个带一个参数的`increment_speed`方法的`Player`类:

```
class Player:

    def __init__(self, name):
        self.name = name
        self.speed = 50

    def increment_speed(self, value):
        self.speed += value
```

要调用方法:

```
# Create instance        
>>> my_player = Player("Nora")

# Check initial speed to see the change
>>> my_player.speed
50

# Increment the speed
>>> my_player.increment_speed(5)

# Confirm the change
>>> my_player.speed
55
```

💡**提示:**要添加更多参数，只需用逗号分隔即可。建议在逗号后面加一个空格。

### Python 中的属性、Getters 和 Setters

Getters 和 setters 是我们可以分别定义来获取和设置实例属性值的方法。它们作为中介来“保护”属性不被直接改变。

在 Python 中，我们通常使用属性而不是 getters 和 setters。让我们看看如何使用它们。

要定义一个属性，我们用以下语法编写一个方法:

```
@property
def <property_name>(self):
    return self.<attribute>
```

这个方法将作为一个 getter，所以当我们试图访问属性的值时，它将被调用。

现在，我们可能还想定义一个 setter:

```
@<property_name>.setter
def <property_name>(self, <param>):
    self.<attribute> = <param>
```

和删除属性的删除器:

```
@<property_name>.deleter
def <property_name>(self):
    del self.<attribute>
```

**💡提示:**您可以在这些方法中编写任何您需要的代码来获取、设置和删除一个属性。建议尽可能保持简单。

这是一个例子:

```
class Dog:

    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, new_name):
        self._name = new_name

    @name.deleter
    def name(self):
        del self._name
```

如果我们添加描述性的打印语句，我们可以看到当我们执行它们的操作时它们被调用:

```
>>> class Dog:

    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        print("Calling getter")
        return self._name

    @name.setter
    def name(self, new_name):
        print("Calling setter")
        self._name = new_name

    @name.deleter
    def name(self):
        print("Calling deleter")
        del self._name

>>> my_dog = Dog("Nora")

>>> my_dog.name
Calling getter
'Nora'

>>> my_dog.name = "Norita"
Calling setter

>>> my_dog.name
Calling getter
'Norita'

>>> del my_dog.name
Calling deleter
```

## 🔹如何在 Python 中处理文件

处理文件对于创建强大的程序非常重要。让我们看看如何在 Python 中实现这一点。

### 如何在 Python 中读取文件

在 Python 中，推荐使用一个`with`语句来处理文件，因为它只在我们需要的时候打开它们，并且在过程完成后自动关闭它们。

要读取文件，我们使用以下语法:

```
with open("<file_path>") as <file_var>:
    <code>
```

我们还可以用`"r"`指定我们想要以读取模式打开文件:

```
with open("<file_path>", "r") as <file_var>:
    <code>
```

但是这已经是打开文件的默认模式，所以我们可以像第一个例子一样省略它。

这是一个例子:

```
with open("famous_quotes.txt") as file:
    for line in file:
        print(line)
```

或者...

```
with open("famous_quotes.txt", "r") as file:
    for line in file:
        print(line)
```

**💡提示:**没错！我们可以使用 for 循环遍历文件中的行。文件路径可以是我们正在运行的 Python 脚本的相对路径，也可以是绝对路径。

### 如何用 Python 写文件

有两种方法可以写入文件。您可以在添加新内容之前替换文件的全部内容，也可以追加到现有内容中。

```
with open("<file_path>", "w") as <file_var>:
    <code>
```

为了完全替换内容，我们使用了`"w"`模式，所以我们将这个字符串作为第二个参数传递给`open()`。我们调用 file 对象上的`.write()`方法，将我们想要写入的内容作为参数传递。

例如:

```
words = ["Amazing", "Green", "Python", "Code"]

with open("famous_quotes.txt", "w") as file:
    for word in words:
        file.write(word + "\n")
```

当你运行程序时，如果在我们指定的路径中不存在一个新文件，它将被创建。

这将是文件的内容:

```
Amazing
Green
Python
Code
```

### 如何在 Python 中向文件追加内容

但是，如果您想要追加内容，那么您需要使用`"a"`模式:

```
with open("<file_path>", "a") as <file_var>:
    <code> 
```

例如:

```
words = ["Amazing", "Green", "Python", "Code"]

with open("famous_quotes.txt", "a") as file:
    for word in words:
        file.write(word + "\n")
```

这一小小的改动将保留文件的现有内容，并将新内容添加到文件末尾。

如果我们再次运行该程序，这些字符串将被添加到文件的末尾:

```
Amazing
Green
Python
Code
Amazing
Green
Python
Code 
```

### 如何在 Python 中删除文件

要用我们的脚本删除一个文件，我们可以使用`os`模块。建议在从该模块调用`remove()`函数之前，用条件检查该文件是否存在:

```
import os

if os.path.exists("<file_path>"):
  os.remove("<file_path>")
else:
  <code>
```

例如:

```
import os

if os.path.exists("famous_quotes.txt"):
  os.remove("famous_quotes.txt")
else:
  print("This file doesn't exist")
```

你可能已经注意到第一行写着`import os`。这是一个重要声明。让我们来看看它们为什么有用，以及如何使用它们。

## 🔸Python 中的导入语句

随着程序规模和复杂性的增长，将代码组织成多个文件是一种很好的做法。但是我们需要找到一种方法来组合这些文件，以使程序正确工作，而这正是 import 语句所做的。

通过编写 import 语句，我们可以将模块(包含 Python 定义和语句的文件)导入到另一个文件中。

以下是 import 语句的各种备选方案:

### 第一种选择:

```
import <module_name>
```

例如:

```
import math
```

💡**提示:** `math`是一个内置的 Python 模块。

如果我们使用这个 import 语句，我们将需要在代码中引用的函数或元素的名称前添加模块的名称:

```
>>> import math
>>> math.sqrt(25)
5.0
```

我们在代码中明确提到元素所属的模块。

### 第二种选择:

```
import <module> as <new_name>
```

例如:

```
import math as m
```

在我们的代码中，我们可以使用我们分配的新名称来代替模块的原始名称:

```
>>> import math as m
>>> m.sqrt(25)
5.0
```

### 第三种选择:

```
from <module_name> import <element>
```

例如:

```
from math import sqrt
```

使用这个 import 语句，我们可以直接调用函数，而无需指定模块的名称:

```
>>> from math import sqrt
>>> sqrt(25)
5.0
```

### 第四种选择:

```
from <module_name> import <element> as <new_name>
```

例如:

```
from math import sqrt as square_root
```

使用这个 import 语句，我们可以为从模块导入的元素指定一个新名称:

```
>>> from math import sqrt as square_root
>>> square_root(25)
5.0
```

### 第五种选择:

```
from <module_name> import *
```

该语句导入模块的所有元素，您可以通过它们的名称直接引用它们，而无需指定模块的名称。

例如:

```
>>> from math import *

>>> sqrt(25)
5.0

>>> factorial(5)
120

>>> floor(4.6)
4

>>> gcd(5, 8)
1
```

💡提示:这种类型的导入语句会使我们更难知道哪些元素属于哪个模块，特别是当我们从多个模块导入元素时。

根据 Python 代码的[样式指南:](https://www.python.org/dev/peps/pep-0008/#imports)

> 应该避免通配符导入(来自<模块> import * ),因为它们会使命名空间中出现的名字变得不清楚，混淆读者和许多自动化工具。

## 🔹Python 中的列表和字典理解

你应该知道的 Python 的一个非常好的特性是列表和字典理解。这只是更简洁地创建列表和字典的一种方式。

### Python 中的列表理解

用于定义列表理解的语法通常遵循以下四种模式之一:

```
[<value_to_include> for <var> in <sequence>]
```

```
[<value_to_include> for <var1> in <sequence1> for <var2> in <sequence2>]
```

```
[<value_to_include> for <var> in <sequence> if <condition>]
```

```
[<value> for <var1> in <sequence1> for <var2> in <sequence2> if <condition>]
```

**💡提示:**只有当它们不会使你的代码更难阅读和理解时，你才应该使用它们。

这里我们有一些例子:

```
>>> [i for i in range(4, 15)]
[4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]

>>> [chr(i) for i in range(67, 80)]
['C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O']

>>> [i**3 for i in range(2, 5)]
[8, 27, 64]

>>> [i + j for i in range(5, 8) for j in range(3, 6)]
[8, 9, 10, 9, 10, 11, 10, 11, 12]

>>> [k for k in range(3, 35) if k % 2 == 0]
[4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34]

>>> [i * j for i in range(2, 6) for j in range(3, 7) if i % j == 0]
[9, 16, 25]
```

### Python 中的列表理解与生成器表达式

列表理解用方括号`[]`定义。这不同于用括号`()`定义的生成器表达式。他们看起来很相似，但却大不相同。我们来看看为什么。

*   **列表理解**立即生成整个序列，并将其存储在内存中。
*   **生成器表达式**在请求时一次产生一个元素。

我们可以用`sys`模块检查这一点。在下面的示例中，您可以看到它们在内存中的大小非常不同:

```
>>> import sys
>>> sys.getsizeof([i for i in range(500)])
2132
>>> sys.getsizeof((i for i in range(500)))
56
```

我们可以使用生成器表达式在 for 循环中迭代，一次获取一个元素。但是如果我们需要在一个列表中存储元素，那么我们应该使用列表理解。

### Python 中的词典理解

现在让我们深入了解字典理解。我们需要用来定义字典理解的基本语法是:

```
{<key_value>: <value> for <var> in <sequence>}
```

```
{<key_value>: <value> for <var> in <sequence> if <condition>}
```

这里我们有一些字典理解的例子:

```
>>> {num: num**3 for num in range(3, 15)}
{3: 27, 4: 64, 5: 125, 6: 216, 7: 343, 8: 512, 9: 729, 10: 1000, 11: 1331, 12: 1728, 13: 2197, 14: 2744}

>>> {x: x + y for x in range(4, 8) for y in range(3, 7)}
{4: 10, 5: 11, 6: 12, 7: 13}
```

这是一个有条件的示例，我们利用现有的字典创建一个新字典，其中只包含及格分数大于或等于 60 的学生:

```
>>> grades = {"Nora": 78, "Gino": 100, "Talina": 56, "Elizabeth": 45, "Lulu": 67}

>>> approved_students = {student: grade for (student, grade) in grades.items() if grade >= 60}

>>> approved_students
{'Nora': 78, 'Gino': 100, 'Lulu': 67}
```

****我**真的**希望你喜欢这篇文章，并发现它很有帮助。**** 现在你知道如何编写和使用 Python 最重要的元素了。

⭐ [订阅我的 YouTube 频道](https://www.youtube.com/channel/UCng0h8WiHLmT57JJ8At4LfQ)并在 [Twitter](https://twitter.com/EstefaniaCassN) 上关注我，以找到更多的编码教程和技巧。查看我的在线课程 [Python 初学者练习:解决 100 多个编码挑战](https://www.udemy.com/course/python-exercises-for-beginners-solve-coding-challenges/?referralCode=804D1EFAF779D07914D2)