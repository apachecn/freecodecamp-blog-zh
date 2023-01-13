# Bash 命令:Bash ls、Bash head、Bash mv 和 Bash cat 举例说明

> 原文：<https://www.freecodecamp.org/news/bash-commands-bash-ls-bash-head-bash-mv-and-bash-cat-explained-with-examples/>

## **Bash ls**

`ls`是 Unix 类操作系统上的一个命令，用于列出目录的内容，例如文件夹和文件名。

### **用途**

```
cat [options] [file_names]
```

最常用的选项:

*   `-a`，所有文件和文件夹，包括隐藏的和以`.`开头的
*   `-l`，以长格式列出所有文件
*   `-G`，启用彩色输出

### **举例:**

列出`freeCodeCamp/guide/`中的文件

在克隆了主 [freeCodeCamp](https://github.com/freeCodeCamp/freeCodeCamp) repo 之后，下面是在`freeCodeCamp`目录中运行`ls`之后的输出:

```
api-server                 docker-compose.yml  public
change_volumes_owner.sh    Dockerfile.tests    README.md
client                     docs                sample.env
CODE_OF_CONDUCT.md         HoF.md              search-indexing
config                     lerna.json          SECURITY.md
CONTRIBUTING.md            LICENSE.md          server
curriculum                 node_modules        tools
docker-compose-shared.yml  package.json        utils
docker-compose.tests.yml   package-lock.json
```

## 更多 bash 命令

### 重击头部

`head`用于打印一个文件的前十行(缺省)或任何其它指定的数量。另一方面，`cat`用于顺序读取文件并将其打印到标准输出(即打印出文件的全部内容)。

不过，这并不总是必要的——也许您只是想检查文件的内容，看看它是否是正确的，或者检查它是否真的不为空。`head`命令允许您查看文件的前 N 行。

如果调用了多个 on file，则显示每个文件的前十行，除非指定了特定的行数。使用下面的选项可以选择显示文件头。

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

打印 file.txt 的前十行(默认)

```
head -n 7 file.txt
```

打印 file.txt 的前七行

```
head -q -n 5 file1.txt file2.txt
```

打印 file1.txt 的前 5 行，然后是 file2.txt 的前 5 行

## **Bash mv**

这个 bash 命令移动文件和文件夹。

```
mv source target
mv source ... directory
```

第一个参数是要移动的文件，第二个参数是要移动到的位置。

常用选项:

*   不经用户同意，强行移动并覆盖文件。
*   覆盖文件前提示确认。

## **痛击猫**

`cat`是 Unix 操作系统中最常用的命令之一。

`cat`用于顺序读取文件并打印到标准输出。这个名字来源于它能控制 T2 文件的方式。

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

打印 file.txt 的内容:

```
cat file.txt
```

连接两个文件的内容，并在终端中显示结果:

```
cat file1.txt file2.txt
```

## 关于 Bash 的更多信息:

### 什么是 Bash？

(Bourne Again SHell 的简称)是一个 Unix shell，也是一个命令语言解释器。shell 只是一个执行命令的宏处理器。它是大多数 Linux 发行版默认打包的最广泛使用的 shell，是 Korn shell (ksh)和 C shell (csh)的继承者。

许多可以在 Linux 操作系统的 GUI 中完成的事情都可以通过命令行来完成。一些例子是:

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

这个脚本只有两行。第一个参数指示使用什么解释器来运行文件(在本例中是 bash)。第二行是我们想要使用的命令，`echo`，后面是我们想要打印的内容，这里是“Hello world！”

值得注意的是，脚本的第一行以`#!`开始。这是一个特殊的指令，Unix 对此区别对待。

#### **我们为什么要用#！脚本文件开头的/bin/bash？**

这是因为让交互式 shell 知道为后面的程序运行哪种解释器是一种约定。

第一行告诉操作系统文件应该由程序在`/bin/bash`执行，这是几乎所有 Unix 或类 Unix 系统上 Bourne shell 的标准位置。通过在脚本的开头添加`#!/bin/bash`，它告诉操作系统使用该特定路径的 shell 来执行脚本中的所有后续命令。

有许多名字，如“大麻棒”、“大麻棒”、“大麻棒”或“嘎吱棒”。请注意，只有当脚本是可执行文件时，才会考虑第一行。

例如，如果`myBashScript.sh`是可执行的，命令`./myBashScript.sh`将导致操作系统查看第一行，以确定使用哪个解释器。在这种情况下，应该是`#!/bin/bash`。

另一方面，如果您运行`bash myBashScript.sh`，那么第一行会被忽略，因为操作系统已经知道使用 bash。

要使`myBashScript.sh`可执行，只需运行`sudo chmod +x myBashScript.sh`。然后运行以下命令来执行脚本:

```
zach@marigold:~$ ./myBashScript.sh
Hello world!
```

有时候脚本不会被执行，上面的命令会返回一个错误。这是由于文件上设置的权限。要避免这种情况，请使用:

```
zach@marigold:~$ chmod u+x myBashScript.sh
```

然后执行脚本。