# 最好的 Linux 例子

> 原文：<https://www.freecodecamp.org/news/linux-example-bash-command-line/>

Linux 是一个强大的操作系统，支持大多数服务器和大多数移动设备。在本指南中，我们将举例说明如何使用它的一些最强大的功能。这涉及到使用 Bash 命令行。

## 12 个简单而有用的 Linux 命令

这里列出的命令是基本的，可以帮助您快速入门。但是它们也很强大，并且随着您的 Linux 专业知识的扩展，它们将继续有用。

1.  命令:它接收你给它的文本，并把它发送到某个地方——返回到屏幕、文件或另一个命令。示例:`echo "hello!"`
2.  `cat`命令:要显示文本文件的内容，只需键入`cat myfile`。
3.  `find`命令:它言出必行，而且很擅长。使用它来查找文件的路径，大小，日期，所有者和一堆其他有用的过滤器。例子:`find . -type f -mtime -1h # List files in this directory modified in the past hour`。
4.  命令:当你想知道时间时，只需键入日期。例子:`date "+It's %l:%m%p on %A"`。在脚本中使用它根据当前日期命名文件。
5.  命令:这个目录里有什么？将`ls`与一些有用的标志结合起来，按照日期和大小显示和排序目录内容。它还为您提供了许多格式化输出的选项。
6.  命令:我在哪里？Linux 是不宽容的，尤其是当你删除一些东西的时候。在发布命令之前，请确保您知道自己在哪里。
7.  Linux 的邮件程序并不好看，但是它真的很有用。您可以在一个命令中创建邮件并添加文本、收件人和附件。示例:`echo "We're having a great time." | mail -s "Wish you were here!" -A postcard.png -t mom@example.com`
8.  `cut`命令:当你有一个带分隔符的字符串时，使用`cut`过滤掉某些字段。例子:`echo "this, that, and the other" | cut -d, -f2 # "that"`
9.  命令:要查找包含特定字符串的文本行，使用 grep。示例:`grep 'root' /etc/passwd # root:x:0:0:root:/root:/bin/bash`
10.  `sed`命令:使用`sed`在一段文本中查找并改变一个子串。例子:`echo "this, that, and the other" | sed 's/that/those/' # "this, those, and the other"`
11.  `shutdown`命令:使用该命令关闭系统并关闭电源。示例:`shutdown -h now`立即关闭系统。`shutdown -h +5`五分钟后关闭系统。
12.  `wget`命令:使用`wget`从命令行下载文件。
    例如:`wget [http://ftp.gnu.org/gnu/wget/wget-1.5.3.tar.gz](http://ftp.gnu.org/gnu/wget/wget-1.5.3.tar.gz)`

在脚本中和命令行中使用这些命令。它们都是非常强大的命令，Linux 的主页上有关于每一个命令的更多信息。

## Linux 用户管理命令示例

此外，用于系统管理员的重要命令如下:

1.  命令:在 Linux 中，uptime 命令显示您的系统已经运行了多长时间，以及当前登录的用户数量。它还显示 1、5 和 15 分钟间隔的平均负载。
2.  命令:这将显示当前登录的用户和他们的进程，以及平均负载。还显示登录名、tty 名称、远程主机、登录时间、空闲时间、JCPU、PCPU、命令和进程。
3.  `users`命令:显示当前登录的用户。该命令除了帮助和版本之外没有其他参数。
4.  命令:who 命令只返回用户名、日期、时间和主机信息。who 命令类似于 w 命令。不像 w 命令，who 不打印用户在做什么。
5.  命令:whoami 命令打印当前用户的名字。您也可以使用“我是谁”来显示当前用户。如果您使用 sudo 命令以 root 用户身份登录，“whoami”将 root 用户作为当前用户返回。如果您想知道登录的确切用户，请使用“我是谁”。
6.  命令:ls 以人类可读的格式显示文件列表。
7.  `crontab`命令:使用 crontab 命令和-l 选项列出当前用户的计划作业。
8.  命令:less 命令允许你快速查看一个文件。你可以上下翻页。按“q”退出 less 窗口。
9.  命令:更多命令允许你快速查看一个文件，并以百分比显示细节。你可以上下翻页。按“q”退出更多窗口。
10.  命令:将文件从源位置复制到目标位置，同时保持相同的模式。

## **如何创建用户**

#### **使用`adduser`或`useradd`命令向您的系统添加一个新用户。**

```
$ sudo adduser username
```

确保用您想要创建的用户替换`username`。

#### **使用`passwd`命令更新新用户的密码。**

```
$ sudo passwd username
```

强烈建议使用强密码！

## **如何创建 Sudo 用户**

要创建一个`sudo`用户，首先需要使用上面的命令创建一个普通用户，然后使用`usermod`命令将这个用户添加到`sudoers`组中。

##### **在 Debian 系统(Ubuntu/LinuxMint/ElementryOS)上，`sudo`组的成员拥有 sudo 特权。**

```
$ sudo usermod -aG sudo username
```

##### **在 RHEL 的系统(Fedora/CentOs)上，`wheel`组的成员拥有 sudo 特权。**

```
$ sudo usermod -aG wheel username
```

## **如何删除用户**

##### **对于 Debian (Ubuntu)**

```
$ sudo deluser username
```

##### **RHEL(Fedora/CentOS)**

```
$ sudo userdel username
```

##### **创建群组和添加用户**

```
$ sudo groupadd editorial
$ sudo usermod -a -G editorial username
```

#### **注意:在`root`模式**下，上述所有命令都可以在没有 sudo 的情况下执行

要切换到 ubuntu 上的 root，运行`su -i`命令，后跟登录用户的密码。提示更改为`#`而不是`$`。

##### **在 Debian 系统(Ubuntu/LinuxMint/ElementryOS)上，`sudo`组的成员拥有 sudo 特权。**

```
$ sudo usermod -aG sudo username
```

## **如何创建群组**

要创建一个组，使用命令`groupadd`

```
$ sudo groupadd groupname
```

## **如何删除群组**

要删除一个组，请使用命令“groupdel”

```
$ sudo groupdel grouname
```

## Linux Find 命令示例

### 使用查找命令

Linux find 命令是一个强大的工具，可以帮助您定位服务器上的文件和目录。只要稍加练习，您就可以根据名称、类型、大小或日期(创建或最后更新的时间)轻松地跟踪事物。

把 find 想成你热切的帮手:

你:“我在我的服务器上找东西。”

发现:“我能帮忙！关于这件事你能告诉我什么？”

你:“这是一个文件，大于 2GB，在我的主目录下，在过去的 48 小时内更新。”

Find: “Tada!”

Find 是一个程序，所以你必须告诉它。

以下是一些使用查找的命令示例:

*   `find ~ -type d # Show me all the subdirectories inside my home directory`
*   `find / -type f -name 'todo.txt' # Show me files named 'todo.txt' anywhere under the root directory (i.e. anywhere)`

第一个参数总是命名我们要查找的目录。在上面的例子中，它们是~(当前用户的主目录)和/(文件系统的根目录)。

其他参数是可选的，可以按照您认为有用的任何方式组合:

*   type 参数允许您限制仅搜索文件(f)、仅搜索目录(d)或符号链接(l)。如果省略 type 参数，您将搜索所有这些类型。
*   name 参数允许您指定要通过名称查找的内容，可以使用文字字符串(' filename.txt ')或通配符(' file？。*').

`man find`将向您展示更多参数，值得回顾。Find 可以通过名称、用户、创建日期、大小等等来查找文件。下次你要找什么东西，就去找吧！

## **Linux dd 命令示例**

“dd”命令可用于创建特定大小的文件。如果您想测试下载速度或任何其他测试，并且需要特定大小的文件，这是非常有用的。

```
dd if=/dev/zero of=file_name.txt bs=1024k count=10
```

这将创建一个 1MB 的文件，名为 file_name.txt。

bs 是你的字节大小，count 代表块数。一个简单的方法是 1024K X 10。

下面是创建 1MB 文件的更简单的方法:

```
dd if=/dev/zero of=file_name.txt bs=1MB count=1
```

## 如何编写 Linux Bash 脚本的示例

### 编写 Bash 脚本

通过在 Linux 命令行上键入命令，您可以向服务器发出指令来完成一些简单的任务。shell 脚本是一种将一系列指令放在一起的方式，使这变得更容易。当您添加像`if`和`while`这样的逻辑来自动控制它们在环境变化时的行为时，Shell 脚本变得更加强大。

### 巴什是什么？

Bash 是一个命令行解释程序的名字，这个程序解释您在命令提示符下或在脚本中输入的 Linux 命令。

### 剧本里有什么？

脚本只是一个文件。一个基本的脚本由一个介绍性的行和一个或多个要执行的指令组成，该行告诉服务器如何使用它。这里有一个例子:

```
#!/bin/bash
echo "Hi. I’m your new favorite bash script."
```

第一行有特殊的含义，我们将在下面讨论。第二行只是一个 Linux 命令，可以在命令行中输入。

### 什么是评论？

注释是您添加到脚本中的文本，您希望 bash 忽略它。注释以井号开头，用于注释代码，以便您和其他用户能够理解它。

要添加评论，请键入`#`字符，后跟任何对您有帮助的文本。Bash 将忽略`#`和其后的所有内容。

****注:**** 剧本的第一行不是注释。这一行总是第一，总是以`#!`开头，对 bash 有特殊的意义。

这是之前的剧本，评论道:

```
#!/bin/bash # Designates the path to the bash program. Must start with '#!' (but isn't a comment).
echo "Hi. I’m your new favorite bash script." # 'echo' is a program that sends a string to the screen.
```

### 执行脚本

您可以打开一个文本编辑器，粘贴示例代码并保存文件，这样您就有了一个脚本。脚本通常以'结尾。因此您可以将该代码保存为 myscript.sh。

脚本不会执行，直到我们做了两件事:

****首先，使其可执行。**** (我们只需要这样做一次。)Linux 广泛依赖于文件权限。它们在很大程度上决定了服务器的行为。关于权限有很多需要知道的，但是现在我们只需要知道这个:在你给自己执行权限之前，你不能运行你的脚本。为此，请键入:

`chmod +x my script.sh`

****其次，跑吧。**** 我们从命令行执行脚本，就像其他命令一样，比如`ls`或`date`。脚本名是命令，您需要在它前面加一个'./'当你调用它时:

`./myscript.sh # Outputs "Hi. I'm your new favorite bash script." (This part is a comment!)`

### 条件式

有时，您希望您的脚本只在其他情况为真时才执行某些操作。例如，仅当某个值低于某个限制时才打印消息。这里有一个使用`if`来做这件事的例子:

```
#!/bin/bash

count=1 # Create a variable named count and set it to 1

if [[ $count -lt 11 ]]; then # This is an if block (or conditional). Test to see if $count is 10 or less. If it is, execute the instructions inside the block.
    echo "$count is 10 or less" # This will print, because count = 1.
fi # Every if ends with fi
```

类似地，我们可以安排脚本，使它只在某个条件为真时才执行指令。我们将更改代码，使 count 变量的值发生变化:

```
#!/bin/bash

count=1 # Create a variable named count and set it to 1

while [[ $count -lt 11 ]]; do # This is an if block (or conditional). Test to see if $count is 10 or less. If it is, execute the instructions inside the block.
    echo "$count is 10 or less" # This will print as long as count <= 10.
    count=$((count+1)) # Increment count
done # Every while ends with done
```

这个版本的 myscript.sh 的输出如下所示:

```
"1 is 10 or less"
"2 is 10 or less"
"3 is 10 or less"
"4 is 10 or less"
"5 is 10 or less"
"6 is 10 or less"
"7 is 10 or less"
"8 is 10 or less"
"9 is 10 or less"
"10 is 10 or less"
```

## **真实世界脚本**

这些例子并不十分有用，但是原则很有用。使用`while`、`if`和任何其他手动输入的命令，您可以创建有价值的脚本。