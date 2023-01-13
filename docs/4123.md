# 终极 Linux 命令行指南——完整的 Bash 教程

> 原文：<https://www.freecodecamp.org/news/linux-command-line-bash-tutorial/>

欢迎来到我们的 Linux 命令行终极指南。本教程将向您展示一些关键的 Linux 命令行技术，并向您介绍 Bash 脚本语言。

## **什么是 Bash？**

(Bourne Again SHell 的简称)是一个 Unix shell，也是一个命令语言解释器。shell 只是一个执行命令的宏处理器。它是大多数 Linux 发行版默认打包的最广泛使用的 shell，是 Korn shell (ksh)和 C shell (csh)的继承者。

Linux 操作系统可以做的很多事情，都可以通过命令行来完成。一些例子是…

*   编辑文件
*   调整操作系统的音量
*   从互联网获取网页
*   自动化您每天做的工作

你可以通过 [GNU 文档](https://www.gnu.org/software/bash/manual/html_node/index.html#SEC_Contents)和 [tldp 指南](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html#toc10)在这里阅读更多关于 bash [的内容。](https://www.gnu.org/software/bash/)

## **在命令行上使用 bash(Linux，OS X)**

通过打开终端，您可以在大多数 Linux 和 OS X 操作系统上开始使用 bash。让我们考虑一个简单的 hello world 示例。打开您的终端，写下下面一行(所有内容都在＄符号之后):

```
zach@marigold:~$ echo "Hello world!"
Hello world!
```

如您所见，我们使用 echo 命令打印字符串“Hello world！”到终点站。

## **编写 bash 脚本**

您还可以将所有 bash 命令放入一个. sh 文件中，并从命令行运行它们。假设您有一个包含以下内容的 bash 脚本:

```
#!/bin/bash
echo "Hello world!"
```

值得注意的是，脚本的第一行以`#!`开始。这是一个特殊的指令，Unix 对此区别对待。

#### **我们为什么要用#！脚本文件开头的/bin/bash？**

这是因为让交互式 shell 知道为后面的程序运行哪种解释器是一种约定。第一行告诉 Unix 该文件将由/bin/bash 执行。这是几乎所有 Unix 系统上 Bourne shell 的标准位置。添加#！/bin/bash 作为脚本的第一行，告诉操作系统调用指定的 shell 来执行脚本中的命令。`#!`常被称为“哈希邦”、“舍邦”或“沙邦”。尽管只有当您将脚本作为可执行文件运行时，它才会被执行。例如，当您键入`./scriptname.extension`时，它会查看最上面一行来找出解释器，而以`bash scriptname.sh`的身份运行脚本时，第一行会被忽略。

然后你可以像这样运行脚本:为了使文件可执行，你应该在 sudo chmod+x“filename”下调用这个命令。

```
zach@marigold:~$ ./myBashScript.sh
Hello world!
```

剧本只有两行。第一个参数指示使用什么解释器来运行文件(在本例中是 bash)。第二行是我们想要使用的命令 echo，后面是我们想要打印的内容“Hello World”。

有时候脚本不会被执行，上面的命令会返回一个错误。这是由于文件上设置的权限。为了避免使用:

```
zach@marigold:~$ chmod u+x myBashScript.sh
```

然后执行脚本。

## **Linux 命令行:Bash Cat**

Cat 是 Unix 操作系统中最常用的命令之一。

Cat 用于顺序读取文件，并将其打印到标准输出。这个名字来源于它对 con ****cat**** enate 文件的功能。

### **用途**

```
cat [options] [file_names]
```

最常用的选项:

*   `-b`，非空白输出行数
*   `-n`，对所有输出行进行编号
*   `-s`，挤压多个相邻的空行
*   `-v`，显示非打印字符，除了制表符和行尾字符

### **例子**

在终端中打印 file.txt 的内容:

```
cat file.txt
```

连接两个文件的内容，并在终端中显示结果:

```
cat file1.txt file2.txt
```

## Linux 命令行:Bash cd

****将目录**** 改为指定的路径，例如`cd projects`。

有几个非常有用的论据来支持这一点:

*   `.`指当前目录，如`./projects`
*   `..`可以用来上移一个文件夹，使用`cd ..`，可以组合起来上移多级`../../my_folder`
*   `/`是你系统到达核心文件夹的根目录，如`system`、`users`等。
*   `~`是主目录，通常是路径`/users/username`。通过将其包含在路径的开头，移回相对于该路径引用的文件夹，例如`~/projects`。

## Linux 命令行:Bash head

Head 用于打印一个或多个文件的前十行(默认情况下)或任何其他指定量。Cat 用于顺序读取文件，并将其打印到标准输出。ie 打印出整个文件的全部内容。-这并不总是必要的，也许你只是想检查一个文件的内容，看看它是否是正确的，或者检查它确实不是空的。head 命令允许您查看文件的前 N 行。

如果调用了不止一个文件，则显示每个文件的前十行，除非指定了具体的行数。使用下面的选项可以选择显示文件头

### **用途**

```
head [options] [file_name(s)]
```

最常用的选项:

*   `-n N`，打印出文件的前 N 行
*   `-q`，不打印文件头
*   `-v`，总是打印出文件头

### **例子**

```
head file.txt
```

在终端中打印 file.txt 的前十行(默认)

```
head -n 7 file.txt
```

在终端中打印 file.txt 的前七行

```
head -q -n 5 file1.txt file2.txt
```

在终端中打印 file1.txt 的前 5 行，然后是 file2.txt 的前 5 行

Linux 命令行:Bash ls

`ls`是 Unix 类操作系统上的一个命令，用于列出目录的内容，例如文件夹和文件名。

### **用途**

```
cat [options] [file_names]
```

最常用的选项:

*   `-a`，所有文件和文件夹，包括隐藏的和以`.`开头的
*   `-l`，长格式列表
*   `-G`，启用彩色输出。

### **举例:**

列出`freeCodeCamp/guide/`中的文件

```
ls                                                                ⚬ master
CODE_OF_CONDUCT.md bin                package.json       utils
CONTRIBUTING.md    gatsby-browser.js  plugins            yarn.lock
LICENSE.md         gatsby-config.js   src
README.md          gatsby-node.js     static
assets             gatsby-ssr.js      translations
```

## Linux 命令行:Bash man

man****man**ual 的缩写，是一个 bash 命令，用于显示给定命令的在线参考手册。**

Man 显示给定命令的相关手册页(简称****man**ual**page****)。

### **用途**

```
man [options] [command]
```

最常用的选项:

*   `-f`，打印给定命令的简短描述
*   `-a`，连续显示手册中包含的所有可用介绍手册页面

### **例子**

显示 ls 的手册页:

```
man ls
```

## Linux 命令行:Bash mv

****移动文件和文件夹。****

```
mv source target
mv source ... directory
```

第一个参数是要移动的文件，第二个参数是要移动到的位置。

常用选项:

*   不经用户同意，强行移动并覆盖文件。
*   覆盖文件前提示确认。

仅此而已。向前迈进，使用 Linux。