# Makefiles 101:如何使用 make 作为任务自动化工具

> 原文：<https://www.freecodecamp.org/news/makefiles-101-how-to-use-make-as-a-task-automation-tool-69d2ccc3f25e/>

亚历克斯·纳达林

# Makefiles 101:如何使用 make 作为任务自动化工具

![TcD0rnzbcNYghR240XeuA429p4gdt93FhJ1o](img/dbd7e35315bb55cecc27b8a605dc7000.png)

Photo by [Agto Nugroho](https://unsplash.com/photos/1mnXGDl3iRY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/factory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

开发人员似乎害怕使用`make`，因为他们将它与从零开始编译的痛苦经历联系在一起——可怕的`./configure && make && make install`。

这种担心部分是由于对 [make(1)](https://linux.die.net/man/1/make) 所做事情的描述:

> make 实用程序的目的是自动确定一个大程序的哪些部分需要重新编译，并发出重新编译它们的命令。

> ***自由软件基金会****[Linux 手册页](https://linux.die.net/man/1/make)*

不是每个人都知道 make 可以很容易地用于管理项目中的任务。在这篇文章中，我想简单介绍一下[makefile 如何帮助我在日常活动中自动化一些任务](https://github.com/odino/mssqldump/blob/master/Makefile)。这篇简短的指南将重点介绍如何使用 make 作为任务的自动化工具，而不是编译代码的工具。

### 执行任务

让我们首先简单地创建一个`Makefile`，并定义一个要运行的任务:

```
task:  date
```

如果你运行`make task`你会碰到以下错误:

```
/tmp ᐅ make taskMakefile:2: *** missing separator.  Stop.
```

这是因为 Makefiles 使用制表符来缩进代码。让我们通过使用制表符而不是空格来更新我们的例子。

```
/tmp ᐅ make taskdateFri Jun 15 08:34:15 +04 2018
```

这是什么巫术？嗯，`make`知道您想要运行 makefile 的部分`task`，并在 shell 中运行该部分中的代码(`date`)，输出命令及其输出。如果您想跳过输出正在执行的命令，您可以简单地在它前面加上一个`@`符号:

```
task:  @date
```

再次运行 make 命令:

```
/tmp ᐅ make taskFri Jun 15 08:34:15 +04 2018
```

`Makefile`中的第一个任务是**默认的**任务，这意味着我们可以不带任何参数地运行`make`:

```
/tmp ᐅ make       Fri Jun 15 08:37:11 +04 2018
```

### 运行附加任务

您可以在您的`Makefile`中添加额外的任务，并用`make $TASK`调用它们:

```
task:  @datesome:  sleep 1  echo "Slept"thing:  cal
```

再次运行 make 命令:

```
/tmp ᐅ make thingcal     June 2018        Su Mo Tu We Th Fr Sa                  1  2   3  4  5  6  7  8  9  10 11 12 13 14 15 16  17 18 19 20 21 22 23  24 25 26 27 28 29 30
```

### 以特定顺序运行任务

很多时候你会想在当前任务之前执行一个任务。把它想象成自动化测试中的`before`或`after`钩子。这可以通过在任务名称后指定任务列表来实现:

```
task: thing some  @date...
```

再次运行 make 命令:

```
/tmp ᐅ make taskcal     June 2018        Su Mo Tu We Th Fr Sa                  1  2   3  4  5  6  7  8  9  10 11 12 13 14 15 16  17 18 19 20 21 22 23  24 25 26 27 28 29 30
```

```
sleep 1echo "Slept"SleptFri Jun 15 08:40:23 +04 2018
```

### 在 make 中使用变量

定义和使用变量相当简单:

```
VAR=123
```

```
print_var:        echo ${VAR}...
```

再次运行 make 命令:

```
/tmp ᐅ make print_var    echo 123123
```

但是要小心，因为您的 shell 变量不能开箱即用:

```
print_user:        echo $USER
```

再次运行 make 命令:

```
/tmp ᐅ make print_user   echo SERSER
```

你需要用`${VAR}`或`$$VAR`来逃离它们。

传递标志也与您可能习惯的方式有所不同。它们被定位为标志，但使用与环境变量相同的语法:

```
print_foo:  echo $FOO
```

再次运行 make 命令:

```
/tmp ᐅ make print_fooecho $FOO
```

```
/tmp ᐅ make print_foo FOO=barecho $FOObar
```

### 制造和外壳

```
5.3.1 Choosing the Shell------------------------
```

```
The program used as the shell is taken from the variable `SHELL'.  If this variable is not set in your makefile, the program `/bin/sh' is used as the shell.
```

Make 将使用`sh`来执行任务中的代码。这意味着有些东西可能无法工作，因为您可能使用了一些特定于 bash 的语法。为了进行切换，您可以简单地指定`SHELL`变量。在我们的例子中，我们希望使用`SHELL:=/bin/bash`。

如前所述，有时您需要使用一种古怪的定制语法来让常规的 shell 命令在`make`中工作。就像变量需要用`$$`或`${...}`转义一样，当使用[命令替换](http://tldp.org/LDP/abs/html/commandsub.html)时，您将需要使用`shell`:

```
subshell:  echo $(shell echo ${USER})
```

再次运行 make 命令:

```
/tmp ᐅ make subshellecho alexalex
```

不相信我？尝试删除`shell`指令。这是你将要得到的:

```
/tmp ᐅ make subshellecho
```

### 结论

有这么多的`make`可以做，有这么多古怪的事情你可能需要找出减少 WPS(每秒 WTF)时使用它。？

这并不能否认一个事实，即`make`是一个非常有用的工具，它允许我们通过用一堆 shell 命令编写制表符分隔的行来轻松地自动化工作流(而不必设置非常复杂的管道)。

*最初发表于[odino.org](https://odino.org/makefile-101/)(2018 年 6 月 15 日)。*
*你可以在[推特](https://twitter.com/_odino_)上关注我——欢迎吐槽！*？