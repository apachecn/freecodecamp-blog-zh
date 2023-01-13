# Python 字符串到数组——如何将文本转换成列表

> 原文：<https://www.freecodecamp.org/news/python-string-to-array-how-to-convert-text-to-a-list/>

有时您需要将一种数据类型转换成另一种数据类型。

不用担心，因为 Python 提供了各种不同的方法来帮助您做到这一点。

在本文中，您将看到几种将字符串转换为列表的方法。

以下是我们将要介绍的内容:

1.  [字符串和列表概述](#intro)
    1.  [如何检查对象的数据类型](#type)
2.  [将字符串转换成单个字符的列表](#characters)
3.  [将字符串转换成单词列表](#words)
    1.  [`split()`法 suntax 细目](#syntax)
    2.  [使用带分隔符](#sep)的`split()`
    3.  [使用`split()`和`maxsplit`参数](#maxsplit)
4.  [将一串数字转换成一列数字](#numbers)

## Python 中的字符串和列表是什么？

一个**字符串**是一个有序的字符序列。它是一系列字符，一个字符接一个字符。

字符串用单引号或双引号括起来:

```
# all the following are strings

# a string enclosed in single quotes
first_name = 'John'

#a string enclosed in double quotes
last_name = "Doe" 
```

如果要创建跨多行的字符串，或者称为多行字符串，请使用三重引号来开始和结束它:

```
# a multiline string enclosed in triple quotes

phrase = '''I am learning Python
and I really enjoy learning the language!
''' 
```

字符串是*不可变的*。这意味着一旦它们被创建，就不能改变。组成字符串的单个字符不能改变。

例如，如果您试图将一个单词的第一个字母从小写改为大写，您将在代码中得到一个错误:

```
#try and change lowercase 'p' to uppercase 'P'
fave_language = "python"
fave_language[0] = "P"

print(fave_language)

#the output will be an error message
#fave_language[0] = "P"
#TypeError: 'str' object does not support item assignment 
```

但是，您可以通过更新变量来重新分配不同的字符串，如下所示:

```
fave_language = "python"
fave_language = "Python"

print(fave_language)

#output
#Python 
```

一个**列表**是数据的有序集合。

多个(通常相关的)项目一起存储在同一个变量下。

您可以通过在方括号`[]`中包含零个或多个项目来创建列表，每个项目用逗号分隔。

列表可以包含 Python 的任何内置数据类型。

```
# a list of numbers
my_numbers_list = [10,20,30,40,50]

print(my_numbers_list)

#output
# [10, 20, 30, 40, 50] 
```

列表是可变的。

创建列表后，您可以更改列表项目。这意味着您可以在程序生命周期中的任何时候修改现有项目、添加新项目或删除项目。

```
programming_languages = ["Javascript", "Python", "Java"]

#update the 1st item in the list
programming_languages[0] = "JavaScript"

print(programming_languages)

#output
#['JavaScript', 'Python', 'Java'] 
```

### Python 中如何确定对象的数据类型

要在 Python 中查找对象的数据类型，请使用内置的`type()`函数，其语法如下:

```
type(object)

#where object is the object you need to find the data type of 
```

`type()`函数将返回作为参数传递给函数的对象的类型。

这通常用于调试目的。

让我们看看如何在下面的例子中对字符串和列表使用`type()`:

```
my_name = "John Doe"
my_lucky_numbers = [7,14,33]

print(type(my_name))
print(type(my_lucky_numbers))

#output
#<class 'str'>
#<class 'list'> 
```

## 如何将字符串转换成单个字符的列表

你可以把一个单词变成一个列表。

组成这个单词的每个字符都成为列表中一个单独的元素。

例如，我们以文本“Python”为例。

您可以将其转换为字符列表，其中每个列表项都是组成字符串“Python”的每个字符。

这意味着`P`字符是一个列表项，`y`字符是另一个列表项，`t`字符是另一个列表项，依此类推。

最直接的方法是将字符串类型转换成一个列表。

Tyepcasting 意味着直接从一种数据类型转换到另一种数据类型——在本例中是从字符串数据类型转换到列表数据类型。

您可以通过使用内置的`list()`函数并将给定的字符串作为参数传递给该函数来实现。

```
programming_language = "Python"

programming_language_list = list(programming_language)

print(programming_language_list)

#output
#['P', 'y', 't', 'h', 'o', 'n'] 
```

让我们看另一个例子:

```
current_routine = " Learning Python ! "

current_routine_list = list(current_routine)

print(current_routine_list)

#output
#[' ', 'L', 'e', 'a', 'r', 'n', 'i', 'n', 'g', ' ', 'P', 'y', 't', 'h', 'o', 'n', ' ', '!', ' '] 
```

文本`" Learning Python ! "`的开头和结尾都有空格，单词“Learning”和“Python”之间有空格，单词“Python”和感叹号之间也有空格。

当字符串被转换成一个字符列表时，每个空格都被视为一个单独的字符，这就是为什么你会看到空格，`' '`，作为列表项。

要删除字符串开头和结尾的空白，使用`strip()`方法。

```
current_routine = " Learning Python ! "

#the leading and trailing whitespace will no longer be separate elements in the list
current_routine_list = list(current_routine.strip())

print(current_routine_list)

#output
#['L', 'e', 'a', 'r', 'n', 'i', 'n', 'g', ' ', 'P', 'y', 't', 'h', 'o', 'n', ' ', '!'] 
```

要删除*所有的*，而不仅仅是开头和结尾的空白，并使新列表中不包含空白字符，请使用`replace()`方法:

```
current_routine = " Learning Python ! "

#replace every instance of whitespace with no space
current_routine_list = list(current_routine.replace(" ", ""))

print(current_routine_list)

#output
#['L', 'e', 'a', 'r', 'n', 'i', 'n', 'g', 'P', 'y', 't', 'h', 'o', 'n', '!'] 
```

## 如何将字符串转换成单词列表

另一种将字符串转换成列表的方法是使用`split()` Python 方法。

`split()`方法将一个字符串分割成一个列表，其中每个列表项是组成字符串的每个单词。

每个单词将是一个单独的列表项。

### Python 中`split()`方法的语法分解

`split()`方法的一般语法如下:

```
string.split(separator=None, maxsplit=-1) 
```

让我们来分解一下:

*   `string`是要转换成列表的给定字符串。
*   方法把一个字符串变成一个列表。它需要两个可选的参数。
*   `separator`是第一个可选参数，它决定了字符串的拆分位置。默认情况下，分隔符是空白，字符串将在任何有空白的地方拆分。
*   `maxsplit`是第二个可选参数。它指定了要执行的最大拆分次数。默认值`-1`意味着它会在整个字符串中进行拆分，并且拆分没有限制。

让我们来看一个例子。

```
phrase = "I am learning Python !"

print(type(phrase))

#output
#<class 'str'> 
```

在上面的字符串中，组成字符串的每个单词都由空格分隔。

要将该字符串转换成单词列表，请使用`split()`方法。

您不需要指定分隔符或`maxsplit`参数，因为我们希望将所有单词分隔开，只要它们之间有空格。

```
phrase = "I am learning Python !"

phrase_to_list = phrase.split()

print(phrase_to_list)
print(type(phrase_to_list))

#output
#['I', 'am', 'learning', 'Python', '!']
#<class 'list'> 
```

字符串根据有空白的地方被分割，组成字符串的每个单词变成一个单独的列表项。

### 如何使用带分隔符的`split()`方法

您也可以使用分隔符和`split()`方法将字符串转换成列表。分隔符可以是您指定的任何字符。

字符串将根据您提供的分隔符进行分隔。

例如，您可以使用逗号`,`作为分隔符。

只要有逗号，字符串就会变成一个列表，从左边开始。

逗号分隔的项目将是单独的列表项目。

让我们看看下面的字符串:

```
phrase = "Hello world,I am learning Python!" 
```

有一个逗号将`Hello world`和`I am learning Python!`分开。

如果我们想要使用逗号作为分隔符来创建两个单独的列表项，我们将执行以下操作:

```
phrase = "Hello world,I am learning Python!"

phrase_to_list = phrase.split(",")

print(phrase_to_list)

#output
#['Hello world', 'I am learning Python!'] 
```

创建了两个单独的项目作为列表项目，并且在有逗号的地方出现了分隔。

另一个例子是，只要有一个点，`.`，就可以分隔一个域名。

```
domain_name = "www.freecodecamp.org"

domain_name_list = domain_name.split(".")

print(domain_name_list)

#output
#['www', 'freecodecamp', 'org'] 
```

每出现一个点，列表中就会增加一个新的列表项。

### 如何使用带有`maxsplit`参数的`split()`方法

如前所述，`maxsplit`是`split()`方法的可选参数。

它定义了列表中有多少元素将被拆分并变成单独的列表项。默认情况下，它被设置为`-1`，这意味着组成字符串的所有元素都将被拆分。

但是我们可以将该值更改为一个特定的数字。

为了只拆分两个单词而不是每个单词，我们将`maxsplit`设置为 2:

```
current_routine = "I enjoy learning Python everyday"

current_routine_list = current_routine.split(maxsplit=2)

print(current_routine_list)

#output
#['I', 'enjoy', 'learning Python everyday'] 
```

`maxsplit`被设置为`2`，这意味着最多只有两个单词将被空格分割，并将形成两个单独的列表项。第三个列表项是组成初始字符串的剩余单词。

使用上一节中的另一个例子，您可以将分隔符与`maxsplit`结合起来，进行从字符串到列表的目标转换:

```
domain_name = "www.freecodecamp.org"

domain_name_list = domain_name.split(".", maxsplit=1)

print(domain_name_list)

#output
#['www', 'freecodecamp.org'] 
```

在本例中，分隔符是一个点，只有第一个元素被拆分。

## 如何将一个整数串转换成一个整数列表

用单引号或双引号括起来的数字被视为字符串。

假设您将自己的出生日期存储为字符串，如下所示:

```
birthdate = "19/10/1993"

print(type(birthdate))

#output
#<class 'str'> 
```

若要删除斜杠并将与出生日期、月份和年份相关联的数字存储为单独的列表项，请执行以下操作:

```
birthdate = "19/10/1993"

birthdate_list = birthdate.split("/")

print(birthdate_list)
print(type(birthdate_list))

#output
#['19', '10', '1993']
#<class 'list'> 
```

在这个例子中，分隔符是斜线`/`，只要有斜线，就会创建一个新的列表项。

如果仔细观察输出，您会发现列表项仍然是字符串，因为它们被单引号括起来，并且没有类型转换。

要将每个列表项从字符串转换为整数，请使用`map`函数。

`map`函数有两个参数:

*   一个功能。在这种情况下，该函数将是`int`函数。
*   iterable 是一个项目序列或集合。在这种情况下，iterable 是我们创建的列表。

```
birthdate = "19/10/1993"

birthdate_list = birthdate.split("/")

str_to_int = (map(int, birthdate_list))

print(str_to_int)

#output
#<map object at 0x10e289960> 
```

这不完全是我们想要的结果。当我们检查数据类型时，我们看到不再有列表:

```
print(type(str_to_int))

#output
#<class 'map'> 
```

为了纠正这一点，我们需要返回并在转换之前添加`list()`函数:

```
birthdate = "19/10/1993"

birthdate_list = birthdate.split("/")

str_to_int = list(map(int, birthdate_list))

print(type(str_to_int))
print(str_to_int)

#output
#<class 'list'>
#[19, 10, 1993] 
```

## 结论

现在你知道了！您现在知道了在 Python 中将字符串转换为列表的一些方法。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！