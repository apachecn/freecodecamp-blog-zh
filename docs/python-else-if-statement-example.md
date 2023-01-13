# Python Else-If 语句示例

> 原文：<https://www.freecodecamp.org/news/python-else-if-statement-example/>

条件语句有助于决策，是所有编程语言中的核心概念。

在本文中，您将学习如何用 Python 编写条件语句。

具体来说，您将学习如何用 Python 编写`if`、`if else`和`elif`(也称为`else if`)语句。

以下是我们将要介绍的内容:

1.  [什么是`if`语句？](#if)
    1.  [一条`if`语句的语法](#if-syntax)
    2.  [一个`if`语句的例子](#if-example)
2.  [什么是`if else`语句？](#if-else)
    1.  [一个`if else`语句的例子](#if-else-example)
3.  [什么是`elif`语句？](#elif)
    1.  [一个`elif`语句的例子](#elif-example)

## Python 中的`if`语句是什么？

一个`if`语句也被称为一个**条件语句**，条件语句是决策的主要部分。

条件语句基于检查或比较采取特定的操作。

总而言之，`if`语句是基于一个条件做出决定的。

该条件是一个布尔表达式。布尔表达式只能是两个值之一-`True`或`False`。

因此，本质上，一个`if`语句说:“如果仅运行下面的代码一次*，并且如果*该条件评估为`True`，仅运行*。如果它*不*，那么就根本不要运行这段代码。直接忽略它，完全跳过它”。*

### 如何在 Python 中创建一个`if`语句——一个语法分解

Python 中的`if`语句的一般语法如下:

```
if expression:
   #run this code if expression evaluates to True
   code statement(s) 
```

让我们来分解一下:

*   使用关键字`if`开始`if`语句。
*   你留下一个空格，然后添加一个布尔值。布尔值是一个计算结果为`True`或`False`的表达式。
*   然后你加一个冒号，`:`。
*   在新的一行上，添加一级缩进。许多代码编辑器会自动为您完成这项工作。例如，当使用带有 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)的 [Visual Studio 代码编辑器](https://code.visualstudio.com/)时，在您写完上一步的冒号并点击`Enter`后，它会自动以正确的缩进级别缩进您的代码。这种级别的缩进是 Python 知道您将要编写的代码语句与`if`语句相关联的方式。
*   最后，编写任何一行代码语句。当且仅当表达式计算结果为`True`时，这些行将运行。如果表达式的计算结果为`False`，它们将不会运行。

### Python 中的`if`语句的例子是什么？

接下来，让我们看一个`if`语句的例子。

我想提示用户输入他们喜欢的编程语言，并将他们的答案存储在一个名为`language`的变量中。

```
language = input("Please enter your favorite programming language: ") 
```

然后，我会设定一个条件。

如果用户输入`Python`作为他们最喜欢的语言，那么并且只有在那时，我想向控制台打印一条消息，告诉它这是正确的答案。

因此，该条件将检查存储在变量`language`中的值是否等于`Python`。

为此，您使用等号运算符(`==`)来检查存储在变量`language`中的值是否等于字符串`Python`。

```
language = input("Please enter your favorite programming language: ")

if language == "Python":
    print("Correct! Of course it is Python!") 
```

我运行我的代码，当提示“请输入你最喜欢的编程语言:”出现时，我输入`Python`。

然后，我得到以下输出:

```
# output

# Please enter your favorite programming language: Python
# Correct! Of course it is Python! 
```

条件(`language == "Python"`)是`True`，因此执行`if`语句中的代码。

如果我重新运行我的程序并输入不同的编程语言，将不会有输出，因为条件将是`False`。

`if`语句中的代码将**而不是**运行，并且`if`语句将被完全跳过:

```
#output 

# Please enter your favorite programming language: Java 
```

在这一点上，还值得一提的是，您应该确保缩进`if`语句内部的代码。如果您忘记缩进打印语句，您将得到以下缩进错误:

```
language = input("Please enter your favorite programming language: ")

if language == "Python":
# Don't do this!
print("Correct! Of course it is Python!")

#output

# print("Correct! Of course it is Python!")
# ^
# IndentationError: expected an indented block after 'if' statement on line 3 
```

## Python 中的`if else`语句是什么？

单独写陈述，尤其是多个陈述，并没有多大帮助。当程序变得越来越大时，这也不被认为是最佳实践。这就是为什么一个`if`语句通常伴随着一个`else`语句。

`if else`语句实质上是说:“`if`这个条件为真做下面这件事，`else`做这件事”。

当且仅当您在`if`语句中设置的条件评估为`False`时，`else`语句中的代码才是您想要运行的代码。

如果您的`if`语句中的条件评估为`True`，`else`语句中的代码将永远不会运行。

`else`关键字是当`if`条件为假并且`if`块内的代码不运行时的解决方案。它提供了另一种选择。

Python 中的`if else`语句的一般语法如下:

```
if condition:
    #run this code if condition is True
    code statement(s)
else:
    # if the condition above is False run this code
    code statement(s) 
```

### Python 中的`if else`语句的例子是什么？

让我们回顾一下前面的例子:

```
language = input("Please enter your favorite programming language: ")

if language == "Python":
    print("Correct! Of course it is Python!") 
```

正如您之前看到的，当我输入字符串`Python`时，`print()`函数中的代码会运行，因为条件评估为`True`。

然而，当用户输入与字符串`Python`相等的**而不是**时，没有其他选择。

这就是`else`语句派上用场的地方，它被添加到`if`语句中:

```
language = input("Please enter your favorite programming language: ")

if language == "Python":
    print("Correct! Of course it is Python!")
else:
    print("Hmm..Are you sure that it is not Python??") 
```

如果条件为`False`，则跳过并忽略`if`语句中的代码。相反，`else`语句中的代码运行如下:

```
# output

# Please enter your favorite programming language: Java
# Hmm..Are you sure that it is not Python?? 
```

此时需要注意的一点是，您不能在`if else`语句之间编写任何额外的代码:

```
language = input("Please enter your favorite programming language: ")

if language == "Python":
    print("Correct! Of course it is Python!")
# Don't do this!!
print("Hello world")
else:
    print("Hmm..Are you sure that it is not Python??")

# output
# else:
    ^^^^
# SyntaxError: invalid syntax 
```

## Python 中的`elif`语句是什么？

`elif`的意思是`else if`。

当你想设置更多的条件，而不是只有`if`和`else`语句可供选择时，你可以引入`elif`语句。

如果`if`语句为`False`，Python 将继续执行`elif`语句，并尝试检查该块中设置的条件。

您也可以编写多个`elif`块，这取决于您想要的各种选项。

一个`elif`语句本质上的意思是:“如果这个条件为真，执行以下操作。如果不是，请尝试这样做。但是，如果以上都不是真的，其他都失败了，最后做这个。”

`elif`语句的一般语法如下:

```
if condition:
    #if condition is True run this code
    code statement(s)
elif:
    #if the above condition was False and this condition is True,
   # run the code in this block
    code statement(s)
else:
    #if the two above conditions are False run this code
    code statement 
```

代码按照编写的顺序从上到下进行计算。

当 Python 找到一个评估为`True`的条件时，它将运行该块中的代码，并忽略其余部分。

因此，如果`if`块中的代码是`True`，其他块都不会运行。它们将被跳过和忽略。

如果`if`块中的代码是`False`，它将移动到`elif`块。

如果是`True`，那么其余的块被忽略。

如果是`False`，Python 会移动到其他`elif`块(如果有的话)。

最后，如果所有的条件都是`False`，那么并且只有在那时`else`块中的代码才会运行。`else`块本质上意味着“当所有其他方法都失败时，运行这段代码”。

### Python 中的`elif`语句的例子是什么？

让我们看一个关于`elif`语句如何工作的例子。

让我们举下面的例子:

```
age = int(input("Please enter your age: "))

if age < 18:
    print("You need to be over 18 years old to continue")
elif age < 21:
    print("You need to be over 21 years old")
else:
    print("You are over 18 and 21 so you can continue") 
```

如果`if`语句是`True`，剩余的代码被跳过:

```
# output

# Please enter your age: 14
# You need to be over 18 years old to continue 
```

当`if`语句为`False`时，Python 会移动到`elif`块并检查该条件。如果`elif`语句是`True`，剩余的代码被跳过:

如果是`True`，Python 将运行`elif`块中的代码，忽略其余代码:

```
# output

# Please enter your age: 19
# You need to be over 21 years old 
```

如果前面两个条件都是`False`，那么最后一招就是`else`块:

```
# output

# Please enter your age: 45
# You are over 18 and 21 so you can continue 
```

## 结论

现在你知道了！您现在知道如何用 Python 编写`if`、`if else`和`elif`语句了。

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

编码快乐！