# Grep 命令教程——如何使用递归查找在 Linux 和 Unix 中搜索文件

> 原文：<https://www.freecodecamp.org/news/grep-command-tutorial-how-to-search-for-a-file-in-linux-and-unix/>

`grep`代表*全局搜索正则表达式并打印出*。它是 UNIX 和 Linux 系统中使用的命令行工具，用于在一个文件或一组文件中搜索指定的模式。

`grep`提供了许多选项，允许我们对文件执行各种与搜索相关的操作。在本文中，我们将看看如何使用带有可用选项的`grep`以及基本的正则表达式来搜索文件。

## 如何使用`grep`

不传递任何选项，`grep`可用于在一个文件或一组文件中搜索模式。语法是:

```
grep '<text-to-be-searched>' <file/files> 
```

**注意**如果文本不止一个单词，需要用单引号或双引号括起来。

您还可以使用**通配符(*)** 来选择目录中的所有文件。

这样做的结果是模式在文件中的出现(通过找到的行)。如果不匹配，则不会向终端打印输出。

例如，假设我们有以下文件(名为 grep.txt):

```
Hello, how are you
I am grep
Nice to meet you 
```

下面的`grep`命令将搜索所有出现的单词“you”:

```
grep you grep.txt 
```

其结果是:

```
Hello, how are you
Nice to meet you 
```

`you`预期具有与其他文本不同的颜色，以便于识别所搜索的内容。

但是`grep`提供了更多的选项，帮助我们在搜索过程中获得更多。在将它们应用到上面的例子时，让我们看看其中的九个。

### 与`grep`一起使用的选项

#### 1.`-n` ( -行号)-列出行号

这将打印出文本的匹配项以及行号。如果你看我们上面的结果，你会注意到没有行号，只有匹配。

```
grep you grep.txt -n 
```

结果:

```
1: Hello, how are you
3: Nice to meet you 
```

#### 2.`-c` ( - count) -打印匹配的行数

```
grep you grep.txt -c 
```

结果:

```
2 
```

**注意**如果第一行有另一个‘你’，选项`-c`仍然会打印 2。这是因为它关心匹配出现的行数，而不是匹配的数量。

#### 3.`-v` ( -反转匹配)-打印与指定模式不匹配的行

```
grep you grep.txt -v -n 
```

结果:

```
2\. I am grep 
```

注意，我们还使用了选项`-n`？是的，您可以在一个命令中应用多个选项。

#### 4.`-i` ( -忽略大小写)-用于不区分大小写

```
# command 1
grep You grep.txt
# command 2
grep YoU grep.txt -i 
```

结果:

```
# result 1
# no result
# result 2
Hello, how are you
Nice to meet you 
```

#### 5.`-l` ( -匹配文件)-打印与模式匹配的文件名

```
# command 1
grep you grep.txt -l
# command 2
grep You grep.txt -i -l 
```

结果:

```
# result 1
grep.txt
# result 2
# all files in the current directory that matches
# the text 'You' case insensitively 
```

```
#### 6\. `-w` (--word-regexp) - print matches of the whole word 
```

默认情况下，`grep`匹配包含指定模式的字符串。这意味着`grep yo grep.txt`将打印与`grep yo grep.txt`相同的结果，因为在你身上可以找到‘yo’。同理，‘欧’。

使用选项`-w`，`grep`确保匹配与指定的模式完全相同。示例:

```
grep yo grep.txt -w 
```

结果:

没有结果！

#### 7.`-o` ( -仅-匹配)-仅打印匹配的图案

默认情况下，`grep`打印找到匹配模式的那一行。使用选项`-o`，仅逐行打印匹配的图案。示例:

```
grep yo grep.txt -o 
```

结果:

```
yo 
```

#### 8.`-A` ( -后上下文)和`-B` ( -前上下文)-分别打印匹配图案前后的行

```
grep grep grep.txt -A 1 -B 1 
```

结果:

```
Hello, how are you
I am grep
Nice to meet you 
```

这个匹配的模式在第 2 行。`-A 1`表示匹配行之后的一行，`-B 1`表示匹配行之前的一行。

还有一个`-C` ( - context)选项等于`-A` + `-B`。传递给`-C`的值将用于`-A`和`-B`。

#### 9.`-R` ( -解引用-递归)-递归搜索

默认情况下，`grep`无法搜索目录。如果您尝试这样做，您会得到一个错误(“是一个目录”)。使用选项`-R`，可以在目录和子目录中搜索文件。示例:

```
grep you . 
```

结果:

```
# 'you' matches in a folders
# and files starting from the
# current directory 
```

### 模式的正则表达式

`grep`也允许使用基本的正则表达式来指定模式。其中两个是:

#### 1.`^pattern` -一行的开始

这种模式意味着`grep`将匹配其行以在`^`之后指定的字符串开始的字符串。示例:

```
grep ^I grep.txt -n 
```

结果:

```
2: I 
```

#### 2.`pattern$` -一行的结尾

与`^`相反，`$`指定了当行以`$`前的字符串结束时匹配的模式。示例:

```
grep you$ grep.txt 
```

结果:

```
1: Hello, how are you
3: Nice to meet you 
```

## 包裹

`grep`是在终端中搜索文件的强大工具。了解如何使用它可以让您通过终端轻松找到文件。

这个工具有更多的选项。用`man grep`可以找到。