# 什么是 YAML？YML 文件格式

> 原文：<https://www.freecodecamp.org/news/what-is-yaml-the-yml-file-format/>

YAML 是编写配置文件最流行的语言之一。

在本文中，您将了解 YAML 与 XML 和 JSON 的比较——这两种语言也用于创建配置文件。

您还将学习该语言的一些规则和特性，以及它的基本语法。

以下是我们将要介绍的内容:

1.  什么是 YAML？
2.  XML VS JSON VS YAML——有什么不同？
    1.  [XML](#xml)
    2.  [JSON](#json)
    3.  [YAML](#yaml)
3.  [YAML 的特色和基本规则](#features-and-rules)
    1.  [如何创建 YAML 文件](#create-file)
    2.  [YAML 的多文档支持](#multi-document)
    3.  [YAML 的缩进](#indentation)
    4.  [YAML 的选项卡](#tabs)
    5.  [YAML 的空白](#whitespace)
    6.  [YAML 的显式数据类型](#explicit-data-types)
4.  [YAML 语法简介](#syntax)
    1.  [标量](#scalars)
    2.  [收藏](#collections)

## 什么是 YAML？

YAML 在 t **M** arkup **L** 语言中代表**YAML**T2A，但它原本代表**Y**et**A**other**M**arkup**L**语言。

YAML 是一种人类可读的数据序列化语言，就像 XML 和 JSON 一样。

序列化是一个过程，在这个过程中，一个具有不同数据结构并使用不同技术编写的应用程序或服务可以使用标准格式将数据传输到另一个应用程序。

换句话说，序列化就是将数据结构翻译、转换和包装成另一种格式。

新格式的数据可以存储在文件中，也可以通过网络传输到另一个应用程序或服务。

YAML 是一种广泛使用的格式，用于为不同的 DevOps 工具、程序和应用程序编写配置文件，因为它具有人类可读和直观的语法。

## XML VS JSON VS YAML——有什么区别？

XML、JSON 和 YAML 都用于创建配置文件和在应用程序之间传输数据。

每种语言都有其优点和缺点。

现在，让我们来看看这三种语言的一些特征。您还将看到一个示例，展示如何用每种语言编写相同的代码，以展示它们在语法上的高级差异。

### XML

XML 是可扩展标记语言的缩写，于 1996 年首次推出，是为通用目的而设计的。

XML 是一种通用的标记语言。它提供了结构化但灵活的语法和定义的文档模式。当处理需要结构化格式和对模式验证的更好控制的复杂配置时，这是一个很好的选择，以确保配置总是具有正确的格式。

也就是说，与其他序列化语言相比，XML 的语法可能冗长、多余，并且更难阅读。

```
<Employees>
    <Employee> 
        <name> John Doe </name>
        <department> Engineering </department>
        <country> USA </country>
    </Employee>
     <Employee> 
        <name> Kate Kateson </name>
        <department> IT Support </department>
        <country> United Kingdom </country>
    </Employee>
</Employees> 
```

### JSON的作用

JSON 代表 JavaScript 对象表示法，自 21 世纪初就已经存在。

JSON 最初受到 JavaScript 编程语言的启发，但它并不局限于一种语言。相反，它是一种独立于语言的格式。

大多数现代编程语言都有用于解析和生成 JSON 数据的库。

与 XML 相比，JSON 提供了可读性更好、更友好、更简洁的语法。它为通过网络在 web 应用程序和服务器之间存储和传输信息提供了一种很好的格式。

也就是说，它可能无法为复杂的配置提供最好的支持。

```
{
	"Employees": [
    {
			"name": "John Doe",
			"department": "Engineering",
			"country": "USA"
		},

		{
			"name": "Kate Kateson",
			"department": "IT support",
			"country": "United Kingdom"
		}
	]
} 
```

### YAML

YAML，最初被称为另一种标记语言，创建于 2001 年，但现在代表 YAML 不是标记语言。

YAML 是 JSON 的官方严格超集，尽管看起来与 JSON 非常不同。

YAML 能做 JSON 能做的一切，甚至更多。一个有效的 YAML 文件可以包含 JSON，并且 JSON 可以转换成 YAML。

与 XML 和 JSON 相比，YAML 拥有最易于阅读、最直观、最简洁的语法来定义配置。

YAML 使用缩进来定义文件中的结构，如果您习惯于编写 Python 代码并且熟悉该语言使用的缩进样式，这将很有帮助。

也就是说，如果你没有得到正确的缩进和格式，可能会导致验证错误，使它对初学者来说不是最友好的。

```
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom 
```

## YAML 的特点和基本规则

现在，让我们回顾一下这种语言的一些基本规则和特征。

### 如何创建 YAML 文件

要创建 YAML 文件，请使用文件扩展名`.yaml`或`.yml`。

### YAML 的多文档支持

在编写任何 YAML 代码之前，您可以在文件开头添加三个破折号(`---`):

```
---
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom 
```

YAML 允许你在一个 YAML 文件中有多个 YAML 文档，使文件组织更加容易。

用三个破折号(`---`)分隔每个文档:

```
---
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom
---
Fruit:
 - Oranges
 - Pears
 - Apples 
```

您也可以使用三个点(`...`)来标记文档的结尾:

```
---
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom
... 
```

### YAML 的缩进

在 YAML，强调缩进和行分隔来表示数据的层次和结构。缩进系统与 Python 使用的系统非常相似。

YAML 不使用符号，如花括号，方括号，或开始或结束标签-只是缩进。

### YAML 的选项卡

YAML 不允许你在创建缩进时使用制表符，而是使用空格。

### YAML 的空白

只要子元素在父元素中缩进，空白就无关紧要。

### 如何在 YAML 写评论

要添加注释来注释掉一行代码，请使用`#`字符:

```
---
# Employees in my company
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom 
```

### YAML 的显式数据类型

尽管 YAML 会自动检测文件中的数据类型，但您可以指定要使用的数据类型。

要明确指定数据类型，请在值前使用`!!`符号和数据类型名称:

```
# parse this value as a string
date: !!str 2022-11-11

## parse this value as a float (it will be 1.0 instead of 1)
fave_number: !!float 1 
```

## YAML 语法简介

### 标量

YAML 的标量是页面上的数据——字符串、数字、布尔值和空值。

让我们看一些如何使用每一个的例子。

在 YAML，字符串在某些情况下可以不加引号，但是您也可以用单引号(`' '`)或双引号(`" "`)将它们括起来:

```
A string in YAML!

'A string in YAML!'

"A string in YAML!" 
```

如果您想要编写跨越多行的字符串，并且想要保留换行符，请使用管道符号(`|`):

```
|
 I am message that spans multiple lines
 I go on and on across lines
 and lines
 and more lines 
```

确保消息是缩进的！

或者，如果您在 YAML 文件中有一个跨越多行的字符串，但是您希望解析器将它解释为单行字符串，那么您可以使用`>`字符，它将用一个空格替换每个换行符:

```
>
 I am message that spans
 multiple lines
 but I will be parsed
 on one line 
```

同样，确保你没有忘记缩进信息！

数字表示数字数据，在 YAML，数字包括整数(整数)、浮点数(带小数点的数字)、指数、八进制和十六进制:

```
# integer
19

# float 
8.7

# exponential
4.5e+13

# octal 
0o23

# hexadecimal
0xFF 
```

在 YAML 和其他编程语言中，布尔值有两种状态，用`true`或`false`表示。

像`true`和`false`这样的词在 YAML 是关键词，所以如果你想让它们被解释为布尔值，就不要用引号把它们括起来。

最后，空值用关键字`null`或波浪号字符`~`表示。

### 收藏

通常情况下，你不会在你的 YAML 文件中编写简单的标量——而是使用集合。

YAML 的收藏可以是:

*   序列(列表/数组)
*   映射(字典/哈希)

要编写序列，请使用破折号(`-`)后跟一个空格:

```
- HTML
- CSS
- JavaScript 
```

序列(列表)中的每一项都放在单独的一行上，值前面有一个破折号。

列表中的每一项都在同一级别上。

也就是说，您可以创建一个嵌套序列(记住，使用空格而不是制表符来创建缩进层次):

```
- HTML
- CSS
- JavaScript
 - React
 - Angular
 - Vue 
```

在上面的顺序中，`React, Angular and Vue`是项目`JavaScript`的子项。

映射允许您列出带有值的键。键/值对是 YAML 文档的构造块。

使用冒号(`:`)后跟一个空格来创建键/值对:

```
Employees:
 name: John Doe
 age: 23
 country: USA 
```

在上面的例子中，一个名字被赋予了一个特定的值。

值`John Doe`被映射(或分配)到`name`键，值`23`被映射到`age`键，值`USA`被映射到`country`键。总之，这些创建了一个对象。

您也可以将映射与序列一起使用。

例如，以前面的示例序列为例，下面是如何构建一个`frontend_languages`列表:

```
frontend_languages:
 - HTML
 - CSS
 - JavaScript
  - React
  - Angular
  - Vue 
```

在上面的例子中，我创建了一个`frontend_languages`列表，其中同一个键`frontend_languages`下有多个值。

同样，您可以创建一个对象列表:

```
Employees:
- name: John Doe
  department: Engineering
  country: USA
- name: Kate Kateson
  department: IT support
  country: United Kingdom 
```

## 结论

希望这篇文章对您有所帮助，让您了解什么是 YAML，这种语言的语法是什么，以及它与 XML 和 JSON 有什么不同。

感谢您的阅读，祝您编码愉快！