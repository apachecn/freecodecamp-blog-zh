# Linux AWK 命令–Linux 和 Unix 用法语法示例

> 原文：<https://www.freecodecamp.org/news/the-linux-awk-command-linux-and-unix-usage-syntax-examples/>

在这本对初学者友好的指南中，你将学习到`awk`命令的最基本知识。您还将看到在处理文本时使用它的一些方法。

我们开始吧！

## 什么是`awk`命令？

`awk`是一种脚本语言，在命令行工作时很有帮助。这也是一个广泛用于文本处理的命令。

使用`awk`时，您可以根据您提供的模式选择数据——一段或多段单独的文本。

例如，你可以用`awk`做的一些操作是在一段给定的文本中搜索一个特定的单词或模式，或者甚至选择你提供的文件中的某一行或某一列。

### `awk`命令的基本语法

最简单的形式是,`awk`命令后跟一组单引号和一组花括号，最后是您想要搜索的文件的名称。

它看起来像这样:

```
awk '{action}' your_file_name.txt 
```

当您想要搜索具有特定模式的文本或在文本中查找特定的单词时，该命令类似于:

```
awk '/regex pattern/{action}' your_file_name.txt 
```

### 如何创建示例文件

要在命令行中创建文件，可以使用`touch`命令。
例如:`touch filename.txt`其中`filename`，是你的文件名。

然后，您可以使用`open`命令(`open filename.txt`)，像“文本编辑”这样的文字处理程序将会打开，您可以在其中添加文件内容。

因此，假设您有一个文本文件`information.txt`，它包含被分成不同列的数据。

文件内容可能如下所示:

```
fristName       lastName        age     city       ID

Thomas          Shelby          30      Rio        400
Omega           Night           45      Ontario    600
Wood            Tinker          54      Lisbon     N/A
Giorgos         Georgiou        35      London     300
Timmy           Turner          32      Berlin     N/A 
```

在我的例子中，有一列分别对应于`firstName`、`lastName`、`age`、`city`和`ID`。

在任何时候，您都可以通过键入`cat text_file`来查看文件内容的输出，其中`text_file`是您的文件的名称。

### 如何使用`awk`打印文件的所有内容

要打印*所有*文件的内容，您在花括号内指定的动作是`print $0`。

这与前面提到的`cat`命令的工作方式完全相同。

```
awk '{print $0}' information.txt 
```

输出:

```
fristName       lastName        age     city       ID

Thomas          Shelby          30      Rio        400
Omega           Night           45      Ontario    600
Wood            Tinker          54      Lisbon     N/A
Giorgos         Georgiou        35      London     300
Timmy           Turner          32      Berlin     N/A 
```

如果您希望每一行都有一个行号计数，您可以使用`NR`内置变量:

```
awk '{print NR,$0}' information.txt 
```

```
1 fristName     lastName        age     city       ID
2 
3 Thomas        Shelby          30      Rio        400
4 Omega         Night           45      Ontario    600
5 Wood          Tinker          54      Lisbon     N/A
6 Giorgos       Georgiou        35      London     300
7 Timmy         Turner          32      Berlin     N/A 
```

### 如何使用`awk`打印特定列

使用`awk`时，您可以指定想要打印的特定列。

要打印第一列，可以使用命令:

```
awk '{print $1}' information.txt 
```

输出:

```
Thomas
Omega
Wood
Giorgos
Timmy 
```

`$1`代表第一个字段，在本例中是第一列。

要打印第二列，可以使用`$2`:

```
awk '{print $2}' information.txt 
```

输出:

```
lastName

Shelby
Night
Tinker
Georgiou
Turner 
```

默认情况下，`awk`用空格来确定每一列的开始和结束位置。

要打印多列，例如第一列和第四列，您需要:

```
awk '{print $1, $4}' information.txt 
```

输出:

```
fristName city

Thomas    Rio
Omega     Ontario
Wood      Lisbon
Giorgos   London
Timmy     Berlin 
```

`$1`代表第一个输入字段(第一列)，而`$4`代表第四个。用逗号`$1,$4`将它们分开，这样输出就有了一个空格，可读性更好。

要打印最后一个字段(最后一列)，也可以使用代表记录中最后一个字段的`$NF`:

```
awk '{print $NF}' information.txt 
```

输出:

```
ID

400
600
N/A
300
N/A 
```

### 如何打印列的特定行

您还可以从所选列中指定要打印的行:

```
awk '{print $1}' information.txt | head -1 
```

输出:

```
FirstName 
```

让我们分解这个命令。打印第一列。然后，该命令的输出(您在前面已经看到了)通过管道符号`|`被*传输到 head 命令，在那里它的`-1`参数选择列的第一行。*

如果你想打印两行，你会这样做:

```
awk '{print $1}' information.txt | head -2 
```

输出:

```
FirstName
Dionysia 
```

### 如何在`awk`中打印出特定图案的线条

您可以打印一行以特定字母开始的文字。

例如:

```
awk '/^O/' information.txt 
```

输出:

```
Omega           Night           45      Ontario    600 
```

该命令选择任何文本行，该文本行的*以`O`开始*。

首先使用向上箭头符号(`^`)，它表示一行的开始，然后是您希望一行开始的字母。

您还可以打印一行以特定的模式结束**的行:**

```
awk '/0$/' information.txt 
```

输出:

```
Thomas          Shelby          30      Rio        400
Omega           Night           45      Ontario    600
Giorgos         Georgiou        35      London     300 
```

这将打印出以`0`结尾的行——`$`符号用在字符后，表示一行如何结束。

该命令也可以更改为:

```
awk '! /0$/' information.txt 
```

`!`被用作`NOT`，所以在这种情况下，它选择不以`0`结尾的行。

```
fristName       lastName        age     city       ID

Wood            Tinker          54      Lisbon     N/A
Timmy           Turner          32      Berlin     N/A 
```

#### 如何在`awk`中使用正则表达式

要输出包含某些字母的单词并打印出与您指定的模式匹配的单词，您需要再次使用前面显示的斜线`//`。

如果你想查找包含`on`的单词，你可以这样做:

```
awk ' /io/{print $0}' information.txt 
```

输出:

```
Thomas          Shelby          30      Rio        400
Omega           Night           45      Ontario    600
Giorgos         Georgiou        35      London     300 
```

这将匹配所有包含`io`的条目。

假设你有一个额外的专栏——`department`专栏:

```
fristName       lastName        age     city       ID   department

Thomas          Shelby          30      Rio        400  IT
Omega           Night           45      Ontario    600  Design
Wood            Tinker          54      Lisbon     N/A  IT
Giorgos         Georgiou        35      London     300  Data
Timmy           Turner          32      Berlin     N/A  Engineering 
```

要找到在`IT`工作的人的所有信息，您需要在斜线之间指定您要搜索的字符串，`//`:

```
awk '/IT/' information.txt 
```

输出:

```
Thomas          Shelby          30      Rio        400  IT
Wood            Tinker          54      Lisbon     N/A  IT 
```

如果您只想看到在`IT`工作的人的名字和姓氏，该怎么办？

您可以像这样指定列:

```
awk '/IT/{print $1, $2}' information.txt 
```

输出:

```
Thomas Shelby
Wood   Tinker 
```

这将只显示出现`IT`的第一和第二列，而不是显示所有字段。

当搜索具有特定模式的单词时，有时可能需要使用转义字符，如下所示:

```
awk '/N\/A$/' information.txt 
```

输出:

```
Wood            Tinker          54      Lisbon     N/A
Timmy           Turner          32      Berlin     N/A 
```

我想找到以模式`N/A`结尾的行。

因此，当在目前显示的`' // '`之间搜索时，我必须在`N/A`之间使用转义符(`\`)，否则我会得到一个错误。

### 如何在`awk`中使用比较运算符

例如，如果您想要查找年龄在`40`以下的雇员的所有信息，您可以像这样使用`<`比较运算符:

```
awk '$3 <  40 { print $0 }' information.txt 
```

输出:

```
Thomas          Shelby          30      Rio        400
Giorgos         Georgiou        35      London     300
Timmy           Turner          32      Berlin     N/A 
```

输出只显示 40 岁以下的人的信息。

## 结论

现在你知道了！现在你已经知道了使用`awk`操作文本数据的绝对基础。

为了学习更多关于 Linux 的知识，freeCodeCamp 提供了各种各样的学习材料。

这里有几个例子可以帮你入门:

*   [Linux 基础知识-实践研讨会](https://www.youtube.com/watch?v=0Qnwqe2P3eY)
*   [Linux for Ethical Hackers(Kali Linux 教程)](https://www.youtube.com/watch?v=lZAoFs75_cs&t=77s)
*   [Linux 命令手册](https://www.freecodecamp.org/news/the-linux-commands-handbook/)

感谢阅读和快乐学习😊