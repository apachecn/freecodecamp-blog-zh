# Bash 命令行提示帮助您更快地工作

> 原文：<https://www.freecodecamp.org/news/bash-command-line-tips-to-help-you-work-faster/>

学习命令行对于任何有抱负的开发人员来说都是必不可少的。

要在命令行上执行命令，您需要一个 shell。

Bash shell 在像 Mac 和 Linux 这样的类 Unix 操作系统中很流行。事实上，在大多数 Linux 发行版中，Bash 是默认的 shell。

还可以通过 WSL(Linux 的 Windows 子系统)在 Windows 中使用 Bash。

在学习了一些基本的 Bash 命令之后，是时候加快速度了。

本教程不适合绝对的初学者，但我希望新手和高级用户都能从中有所收获。

这里有 10 个 Bash 命令，可以帮助你更快地使用你的终端。

## 1.使用 ctrl+L 清除屏幕，使用 ctrl+D 退出

要清除终端屏幕，请在命令行中键入`clear`。

要退出，请键入`exit`。

更好的是，按 Ctrl + l ( ⌘ + l)清除屏幕，Ctrl + d (⌘ + d)关闭终端。

## 2.使用`nohup`命令来产生不随终端会话结束的进程

一旦程序被装入内存，它们就被称为进程。

有时候，我从命令行打开火狐:
`firefox https://freecodecamp.org`。

但是我一关闭终端，Firefox 也崩溃了。

为了防止这种情况，使用`nohup`(不挂断)命令:`nohup firefox https://freecodecamp.org`。

现在，当我关闭终端时，Firefox 不会崩溃，但我的标签会崩溃。

解决方法:通过添加`&`符号使 Firefox 成为后台进程。

`nohup firefox https://freecodecamp.org &`

现在，即使我退出终端，我的标签都是完整的。

## 3.使用`pkill`通过只输入名称的一部分来终止进程

通过使用`killall`命令，我们可以通过它的名字来终止一个进程:

`killall firefox`

更好的方法是，使用`pkill`通过只输入名称的一部分来终止。

`pkill fire*`

## 4.在前面加上`time`命令，以了解执行的速度

您想知道在 shell 中执行一个东西需要多长时间吗？

只需在命令前面加上`time`:`time gcc -g *.c`。

## 5.在 Linux 上，使用`cat /etc/*rel*`查看发行版名称

键入`uname -a`显示系统信息。

想要仔细检查你正在运行的发行版吗？

只需在 shell 上键入`cat /etc/*rel*`并按回车键。

## 6.在文本文件中使用`sed`命令查找和替换

是否要替换文本文件中多次出现的单词？

使用`sed`命令。

`sed s'/apples/oranges/g' myfile.txt`

在这里，所有出现的“苹果”都变成了“橘子”。

如果只需要替换每一行中的第一个出现的，只需要去掉末尾的' g '后缀:`sed s'/apples/oranges/' myfile.txt`。

“g”代表*全球。*

正斜杠`/`是分隔符。事实上，您可以使用任何分隔符。

让我们使用一个下划线`_`作为分隔符:`sed s'_apples_oranges_'g ` myfile.txt`。

简单地使用`sed`仅替换标准输出。原始文件保持不变。

要‘就地’更改文件，使用`-i`标志:`sed -i s'_apples_oranges_g' myfile.txt`。

## 7.使用`curl`了解您电脑的公共 IP 地址

有两种类型的 IP 地址:私有和公有。

系统分配内部 IP 地址，可以使用`ifconfig`命令检查这些地址。

但是你想知道你计算机的公共 IP 地址吗 ISP 分配给你接口的 IP 地址？

在线时，只需在命令行上使用`curl ifconfig.me ; echo`或`curl ifconfig.co ; echo`。

## 8.使用 Ctrl + R (⌘ + R)进行反向搜索

按“向上”箭头键会显示您键入的最后一个命令。

键入`history`显示您键入的所有命令，这些命令存储在 bash 历史中。

更好的方法是，在 shell 上键入 Ctrl + r (⌘ + r ),然后开始输入命令。

当您键入时，shell 会根据历史自动完成。一找到匹配项就按“回车”。

如果你只记得本教程中的一件事，请记住这个组合键:Ctrl + r (⌘ + r)。

它会节省你很多时间，保证！

## 9.使用外壳做数学

对于不输入或输出分数的简单计算，您可以简单地使用:

```
:~$ echo $((19*34))
:~$ 646 
```

对于涉及分数的计算，只需`echo`表达式并将其输入到`bc`命令中。

```
:~$ echo "scale=2; 9*3/((2*2)+1)" | bc
:~$ 5.40 
```

在这里，“规模”是我们需要的精度。我已经指定它是两个小数点。

## 10.使用大括号扩展来批量创建文件

我们如何在一个文件夹中创建 100 个文件？

文件 1.txt，文件 2.txt，文件 3.txt...file100.txt

通过使用大括号展开:`touch file{1..100}.txt`。

我们需要为我们的项目创建三个文件:app.html、app.css 和 app.js

我们可以简单地使用大括号展开来一次创建所有内容，而不是一个一个地创建。

```
:~$ touch app.{html,css,js}
:~$ ls
app.html app.css app.js
:~$ 
```

或者在项目文件夹中，我们需要创建五个目录:图像、css、src、模板和脚本。

我们可以使用:

```
:~$ mkdir {images,css,src,templates,scripts}
:~$ ls
images css src templates scripts
:~$ 
```

这里只有一个警告:只要确保括号内的单词之间没有空格。

## 包扎

我列出了 10 个 Bash 命令行提示，通过它们您可以在终端上加快速度。

学习这些 Bash 命令，它将对您的编程之旅大有裨益。

编码快乐！