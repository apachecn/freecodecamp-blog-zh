# Python 中的 While 循环–While True 循环语句示例

> 原文：<https://www.freecodecamp.org/news/while-loops-in-python-while-true-loop-statement-example/>

Python 有许多工具和特性可以帮助您自动化重复的任务。

其中一个特征是循环。

在所有现代编程语言中，循环都是一个有用且常用的特性。

当您想要自动执行一个特定的重复任务或者防止自己在程序中复制和粘贴相同的代码时，循环是很有帮助的。

计算机编程中的循环多次重复相同的代码块或相同的指令序列，直到满足某个条件或不再满足某个条件。

所以，总而言之，循环让你不用一遍又一遍地写同样的代码。

Python 内置了两种类型的循环:

*   `for`循环往复。
*   `while`循环往复。

在本文中，您将学习如何构造`while`循环。

以下是我们将要介绍的内容:

1.  [什么是`while`循环？](#definition)
    1.  [一个`while`循环的语法](#syntax)
    2.  [一个`while`循环的例子](#example)
2.  [什么是`while True`循环？](#while-true)

## Python 中的`while`循环是什么？初学者的定义

一个`while`循环将一段代码重复未知次数，直到不再满足某个条件。另一方面，循环将代码块重复固定的次数。

所以，当你不知道你想要一个代码块预先执行多少次时，`while`循环是有用的。

一个`while`循环根据给定的布尔条件重复代码块。

布尔条件是评估为`True`或`False`的条件。

一个`while`循环总是在运行前首先检查条件。如果条件评估为`True`，那么循环将运行循环体内的代码，并在条件保持`True`时继续运行代码。

它将继续执行所需的代码语句集，直到条件不再是`True`。

我们举个假设的例子。

你可以要求用户提交一个秘密关键字，这样他们就可以访问你的网站的特定部分。

比如说，要让他们能够查看某些内容，他们首先必须输入关键字“Python”。

为此，您可以要求他们输入关键字。也就是说，你不知道用户会输入多少次错误的关键词。

每次他们输入错误的关键字时，您都会继续提示他们输入正确的关键字。只要他们输入了错误的关键字，你就不会允许他们继续。

当他们最终输入关键字“Python”时，您将允许他们查看该内容，您将停止提示他们，并且该代码块将停止执行。

要做类似于这个例子的事情，您需要利用 Python 的`while`循环。

### 如何用 Python 写一个`while`循环——给初学者的语法分解

用 Python 编写`while`循环的一般语法如下:

```
while condition:
    body of while loop containing code that does something 
```

让我们来分解一下:

*   通过使用关键字`while`开始`while`循环。
*   然后，添加一个布尔表达式的条件。布尔表达式是计算结果为`True`或`False`的表达式。
*   条件后面跟一个冒号，`:`。
*   在新的一行上，添加一个缩进级别。许多代码编辑器会自动为您完成这项工作。例如，当使用带有 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)的 [Visual Studio 代码编辑器](https://code.visualstudio.com/)时，在您写完上一步的冒号并点击`Enter`后，它会自动以正确的缩进级别缩进您的代码。这种级别的缩进是 Python 知道您将要编写的代码语句与`while`语句相关联的方式。
*   然后，您想要运行的代码放在`while`语句的主体中。
*   当条件评估为`True`时，`while`循环体内的代码将会执行。主体内部的代码将继续执行，直到条件不再满足，并且计算结果为`False`。

### Python 中的`while`循环的例子是什么？

现在，让我们使用 Python while 循环编写我前面提到的例子。

首先，我将把秘密关键字`Python`存储在一个名为`secret_keyword`的变量中。

```
secret_keyword = "Python" 
```

然后，我会要求用户输入所需的秘密关键字，他们应该知道访问其余的内容。

为此，我将使用`input()`函数并将结果存储在名为`user_input`的变量中。

```
user_input = input("Please enter the secret keyword: ") 
```

这里需要注意的是，默认情况下用户输入是区分大小写的，这意味着如果用户输入“python”而不是“Python ”,他们仍然无法继续。

要解决这个问题，您可以使用一个字符串方法，如`.capitalize()`来大写用户输入的单词的第一个字母。

```
user_input = input("Please enter the secret keyword: ").capitalize() 
```

接下来，是时候构造`while`循环了。

我将检查变量`user_input`是否是*而不是*等于变量`secret_keyword`的内容。

本质上，我是在检查用户输入的内容是否不等于字符串‘Python’。

为了用 Python 编写这个条件，我将使用`!=`操作符，它检查不相等性。

```
secret_keyword = "Python"

user_input = input("Please enter the secret keyword: ").capitalize()

while user_input != secret_keyword: 
```

在`while`循环体中，我将再次提示用户输入 secret 关键字。

```
secret_keyword = "Python"

user_input = input("Please enter the secret keyword: ").capitalize()

while user_input != secret_keyword:
    user_input = input("Please enter the secret keyword: ").capitalize() 
```

其工作方式是，如果用户输入字符串“Python ”,循环将终止，程序将不再运行。但是，如果用户输入的字符串不等于“Python”，循环将继续。

因此，如果`user_input`是*而不是*等于`secret_keyword`，循环将继续执行。

并且没有设定运行和停止的次数，这意味着只要用户*没有*输入字符串‘Python’，循环`while`就会继续执行。这是因为我设置的条件继续求值到`True`。

```
Please enter the secret keyword: Hello
Please enter the secret keyword: Hi
Please enter the secret keyword: CSS
Please enter the secret keyword: css
Please enter the secret keyword: 
..
..
.. 
```

如果您想终止程序，键入`Control C`以退出无限循环。无限循环是指循环永远不会停止执行。

现在，如果我重新运行程序，并最终输入正确的 secret 关键字，循环将退出，代码将停止运行。

```
Please enter the secret keyword: Java
Please enter the secret keyword: Python 
```

如果我通过`capitalize()`方法输入‘python ’,就会发生这种情况:

```
Please enter the secret keyword: java
Please enter the secret keyword: python 
```

循环终止，因为条件不再评估为`True`。

## Python 中的`while True`循环是什么？

之前，您看到了什么是无限循环。

从本质上来说，`while True`循环是一个连续的`True`循环，因此会无休止地运行。除非你强迫它停止，否则它永远不会停止。

```
#this creates an infinite loop

while True:
    print("I am always true") 
```

正如您之前看到的，避免这种情况的方法是键入`Control C`。

另一种显式避免这种情况的方法是使用`break`语句。

由于`True`将总是评估为`True`并因此重复执行，`break`语句将在需要时强制循环停止。

让我们举下面的例子:

```
i = 0

# this creates an infinite loop

while True:
    print(i)
    i = i + 1 
```

在本例中，`i`将继续重复增加 1——没有条件阻止它增加，因为`True`将始终计算为`True`。

为了避免无限循环，我首先引入了一个`if`语句。

`if`语句检查`i`是否等于`5`。如果是，那么由于`if`语句中的`break`语句，循环将会结束，这实际上是告诉循环停止。

```
i = 0

while True:
    print(i)
    i = i + 1

    if i == 5:
        break 
```

## 结论

现在你知道了！您现在知道如何用 Python 编写`while`和`while True`循环了。

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢您的阅读和快乐编码！