# 运行 Python 脚本——如何在终端中执行 Python Shell 命令

> 原文：<https://www.freecodecamp.org/news/run-python-script-how-to-execute-python-shell-commands-in-terminal/>

当你开始学习一门新的编程语言时，你的第一个程序很可能是打印“hello world！”。

假设你想用 Python 来做这件事。有两种方法可以做到这一点:使用 Python shell 或将其编写为脚本并在终端中运行。

## 什么是贝壳？

操作系统是由一堆程序组成的。它们执行文件处理、内存管理和资源管理等任务，并帮助您的应用程序平稳运行。

我们在计算机上做的所有工作，像在 Excel 中分析数据或玩游戏，都是由操作系统促成的。

操作系统程序有两种，叫做**外壳**和**内核**程序。

内核程序是执行实际任务的程序，比如创建文件或发送中断。Shell 是另一个程序，它的工作是接受输入，决定并执行所需的内核程序来完成工作并显示输出。

shell 也被称为**命令处理器**。

## 什么是终端？

终端是与 shell 交互的程序，允许我们通过基于文本的命令与它通信。这就是它也被称为命令行的原因。

要在 Windows 上访问终端，请点击 Windows 徽标+ R，键入 cmd，然后按 Enter。

要在 Ubuntu 上访问终端，按 Ctrl + Alt + T。

## Python 外壳是什么？

Python 是一种解释型语言。这意味着 Python 解释器读取一行代码，执行该行代码，然后在没有错误的情况下重复这个过程。

Python Shell 为您提供了一个命令行界面，您可以使用该界面以交互方式直接向 Python 解释器指定命令。

你可以在[官方文档](https://docs.python.org/3/tutorial/interpreter.html#)中获得很多关于 Python shell 的详细信息。

## 如何使用 Python Shell

要启动 Python shell，只需在终端中键入`python`并按 Enter 键:

```
C:\Users\Suchandra Datta>python
Python 3.8.3 (tags/v3.8.3:6f8c832, May 13 2020, 22:37:02) [MSC v.1924 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>print("hello world!") 
```

交互式 shell 也称为 REPL，代表读取、评估、打印和循环。它将读取每个命令，评估并执行它，打印该命令的输出(如果有的话),并重复相同的过程，直到您退出 shell。

有不同的方法可以退出 shell:

*   您可以在 Windows 上按 Ctrl+Z，或者在 Unix 系统上按 Ctrl+D 来退出
*   使用 exit()命令
*   使用 quit()命令

```
C:\Users\Suchandra Datta>python
Python 3.8.3 (tags/v3.8.3:6f8c832, May 13 2020, 22:37:02) [MSC v.1924 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> print("HELLO WORLD")
HELLO WORLD
>>> quit()

C:\Users\Suchandra Datta>
```

```
C:\Users\Suchandra Datta>python
Python 3.8.3 (tags/v3.8.3:6f8c832, May 13 2020, 22:37:02) [MSC v.1924 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()

C:\Users\Suchandra Datta>
```

```
C:\Users\Suchandra Datta>python
Python 3.8.3 (tags/v3.8.3:6f8c832, May 13 2020, 22:37:02) [MSC v.1924 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> ^Z

C:\Users\Suchandra Datta>
```

## 在 Python Shell 中可以做什么？

您可以做 Python 语言允许的几乎所有事情，从使用变量、循环和条件到定义函数等等。

`>>>`是 shell 提示符，您可以在这里输入命令。如果您有跨几行的命令——例如当您定义循环时 shell 打印出表示一行继续的`...`字符。

让我们看一个例子:

```
>>>
>>> watch_list = ["stranger_things_s1", "stranger_things_s2", "stranger_things_s3","stranger_things_s4"]
>>>
>>>
```

这里，我们通过 Python shell 定义了一个包含一些电视节目名称的列表。

接下来，让我们定义一个接受节目列表并随机返回一个节目的函数:

```
>>> def weekend_party(show_list):
...     r = random.randint(0, len(show_list)-1)
...     return show_list[r]
...
```

注意这里 Python shell 的延续行(`...`)。

最后，要从 shell 中调用该函数，只需像在脚本中一样调用该函数:

```
>>> weekend_party(watch_list)
'stranger_things_s1'
>>>
>>>
>>> weekend_party(watch_list)
'stranger_things_s3'
>>>
>>>
>>> weekend_party(watch_list)
'stranger_things_s2'
>>>
>>>
>>> weekend_party(watch_list)
'stranger_things_s2'
>>>
>>>
>>> weekend_party(watch_list)
'stranger_things_s3'
>>> 
```

您可以从 shell 中检查 Python 模块，如下所示:

```
>>>
>>>
>>> import numpy
>>> numpy.__version__
'1.20.1'
>>>
```

您可以通过使用`dir()`方法来查看模块提供了哪些方法和属性:

```
>>>
>>> x = dir(numpy)
>>> len(x)
606
>>> x[0:3]
['ALLOW_THREADS', 'AxisError', 'BUFSIZE']
```

这里可以看到 Numpy 总共有 606 个方法和属性。

## 如何运行 Python 脚本

Python shell 对于执行简单程序或调试复杂程序的某些部分非常有用。

但是非常复杂的大型 Python 程序是用扩展名为. py 的文件编写的，通常称为 Python 脚本。然后使用`Python`命令从终端执行它们。

通常的语法是:

```
python filename.py
```

我们之前通过 shell 执行的所有命令，我们也可以写在脚本中并以这种方式运行。

## 结论

在这篇文章中，我们学习了外壳，终端，如何使用 Python 外壳。我们还看到了如何从命令行运行 Python 脚本。

我希望这篇文章能帮助你理解 Python shell 是什么，以及如何在日常生活中使用它。快乐学习！