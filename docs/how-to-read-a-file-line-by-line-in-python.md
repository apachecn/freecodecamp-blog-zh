# 如何在 Python 中逐行读取文件

> 原文：<https://www.freecodecamp.org/news/how-to-read-a-file-line-by-line-in-python/>

用 Python 编码时，有时可能需要打开并读取文本文件的内容。

幸运的是，在 Python 中有几种方法可以做到这一点。

这种语言有许多内置的函数、方法和关键字，可以用来创建、写入、读取和删除文本文件。

在本文中，您将学习读取文件的最常见方法。借助编码示例，您将知道如何逐行读取文本文件。

以下是我们将要介绍的内容:

1.  [如何使用`open()`功能](#open-function)打开文本文件
2.  [如何使用`read()`方法读取文本文件](#read-method)
3.  [如何使用`readline()`方法读取文本文件](#readline-method)
4.  [如何使用`readlines()`方法读取文本文件](#readlines-method)
5.  [如何使用`for`循环读取文本文件](#for-loop)

让我们开始吧！

## 如何使用 Python 中的`open()`函数打开文本文件

在开始阅读 Python 中的文本文件之前，首先需要打开它。

要打开一个文本文件，使用内置的`open()`函数。

`open()`函数的一般语法如下:

```
open("filename", "mode") 
```

`open()`函数接受多个参数，但是在这个例子中，我只关注两个:`filename`和`mode`。

我们来分解一下语法。

`open()`函数接受的第一个必需参数是`filename`，它代表您想要打开的文件名的完整路径。

当指定要打开的文件的路径时，您需要知道该文件在文件夹结构中的位置。

例如，如果您想要打开的文本文件和您当前带有 Python 代码的文件在同一个文件夹中，您只需要引用它的名称和扩展名。

假设您有一个名为`projects`的文件夹。

在其中，您有两个文件，`main.py`，这是您编写 Python 代码的文件，和`example.txt`，这是您想要打开的文件。该文件包含以下内容:

```
I absolutely love coding!
I am learning to code for free with freeCodeCamp! 
```

这两个文件在文件夹中处于同一层，所以下面是使用`open()`函数时如何引用`example.txt`:

```
open("example.txt") 
```

`open()`函数接受的第二个可选参数是`mode`。它指定您是想读(`"r"`)、写(`"w"`)还是追加(`"a"`)到`filename`。

默认模式是读取(`"r"`)模式。

因此，要打开并读取`example.txt`，您可以选择使用`"r"`来表示您想要使用的模式:

```
open("example.txt", mode="r") 
```

说了这么多，不需要写关键词`mode`。

相反，您可以省略它，只使用字母`"r"`——它仍然会有相同的结果:

```
open("example.txt","r") 
```

最后，您可以完全省略字母`"r"`，因为这是默认模式:

```
open("example.txt") 
```

当您运行上面示例中的代码时，它不会做任何事情。

您已经完成了第一步，即打开文本文件，但是您还没有阅读和看到它的内容。

## 如何使用 Python 中的`read()`方法读取文本文件

为了读取`example.txt`的内容，让我们首先将我们在上一节中编写的代码存储在一个名为`file`的变量中:

```
file = open("example.txt") 
```

然后，让我们调用`file`上的`read()`方法，并将结果打印到控制台:

```
file = open("example.txt")

print(file.read())

# output

# I absolutely love coding!
# I am learning to code for free with freeCodeCamp! 
```

现在，你可以阅读`example.txt`的内容了！

`read()`方法将所有内容作为单个字符串读取，这在处理文本文件中没有很多内容的较小文件时很有用。

也就是说，上面的代码缺少了一些东西。

一旦你读完了文本文件，你需要关闭它。为此，使用`close()`方法。确保不要跳过这一步，因为忘记关闭文件可能会在代码中引入错误！

```
file = open("example.txt")

print(file.read())

# close file
file.close() 
```

现在，关闭文本文件是一个很好的实践，但是这是您很容易忘记做的事情——您可能不总是记得调用文件上的`close()`方法。

有一个替代方案。

关键字`with`确保文件在代码执行时自动关闭。

与`open()`函数一起使用时，`with`关键字的一般语法如下:

```
with open("filename") as variable:
    # do something with variable 
```

因此，下面是如何使用`with`关键字而不是`close()`方法重写前面示例中的代码:

```
with open("example.txt") as file:
  print(file.read())

# output

# I absolutely love coding!
# I am learning to code for free with freeCodeCamp! 
```

## 如何使用 Python 中的`readline()`方法读取文本文件

如果你想从一个文本文件中只读取一行，使用`readline()`方法:

```
with open("example.txt") as file:
  print(file.readline())

# output

# I absolutely love coding! 
```

文本文件`example.txt`里面有两行，但是`readline()`方法只从文件中读取一行并返回它。

`readline()`方法还在字符串末尾添加了一个尾随换行符。

您可以选择向`readline()`方法传递一个`size`参数，该参数指定返回行的长度和它将读取的最大字节数。

```
with open("example.txt") as file:
  print(file.readline(10))

# output

# I absolute 
```

## 如何使用 Python 中的`readlines()`方法读取文本文件

`readlines()`方法从文件中读取所有的行，一行一行地检查文件。

然后，它返回一个字符串列表:

```
with open("example.txt") as file:
  print(file.readlines())

# output

# ['I absolutely love coding!\n', 'I am learning to code for free with freeCodeCamp!'] 
```

`readlines()`方法一次性读取所有行，并将文本文件中的每一行作为一个列表中的单个列表项存储。`readlines()`方法还在每行末尾添加了一个换行符`\n`。

## 如何在 Python 中使用`for`循环读取文本文件

Python 中逐行读取文件的另一种方法是使用`for`循环，这是读取文件最 Python 化的方法:

```
with open("example.txt") as file:
  for item in file:
    print(item)

# output

# I absolutely love coding!

# I am learning to code for free with freeCodeCamp! 
```

`open()`函数返回一个 iterable 对象。

`for`循环与`in`关键字成对出现——它们遍历返回的 iterable file 对象并读取其中的每一行。

## 结论

希望本文能帮助您理解如何使用`read()`、`readline()`和`readlines()`方法以及`for`循环在 Python 中逐行读取文件。

感谢您的阅读，祝您编码愉快！