# ABS 1.2:后台命令&导入文件的能力

> 原文：<https://www.freecodecamp.org/news/abs-1-2-background-commands-the-ability-to-import-files-e5d1e046cb35/>

亚历克斯·纳达林

# ABS 1.2:后台命令&导入文件的能力

ABS 是一种编程语言，它将 bash 脚本的高效性与 Python 或 Ruby 等高级语言的优雅结合在一起。它允许您通过简单地将系统命令放在反斜杠中来发出它们(非常类似于您在 Bash 中的做法),并允许您使用清晰简洁的语法来使用它们的输出。

例如，这是一个试图发出一个`curl`命令的脚本，如果在`example.org`的服务器没有在 10 秒钟内回复，它就退出:

![dSVYyySoZGbfUcDXgDVHZ674fUKRRG5GGL8n](img/6ffb9d8b09b2c2d892d9b8dada4865ec.png)

几周前， [ABS 团队](https://github.com/abs-lang/abs)设法整合了该语言的一个新的次要版本， [1.2.0](https://github.com/abs-lang/abs/releases/tag/1.2.0) ，其中包括许多有趣的特性——让我们来看看吧！

### ~/.absrc

ABS 现在会在每次运行脚本时寻找一个默认的`~/.absrc`文件进行预加载。如果您希望将可能跨脚本重用的“基本”函数转储到一个公共位置，这将特别有用。您的`.absrc`可能看起来像:

```
tenth = f(x) {   return x / 10 }
```

因此在任何其他 abs 脚本中，您都可以`tenth(x)`。

### ~/.abs _ 历史记录

我们还引入了一个历史文件，以便在使用 ABS repl 时能够容易地重复命令。默认情况下，它位于`~/.abs_history`处，并且在每次关闭复制会话时进行同步:

```
$ absHello alex, welcome to the ABS (1.2.0) programming language!Type 'quit' when you're done, 'help' if you get lost!⧐  `sleep 1`
```

```
⧐  quitAdios!$ tail ~/.abs_history`sleep 1`
```

### 要求(文件)

这里有一个大问题:现在**可以通过`require(path/to/file.abs )`请求外部文件**。

这是一个垫脚石，允许创建可以跨 ABS 脚本重用的基础库，并更好地组织 ABS 代码。

### 后台命令

这里的另一个重要特性是:你现在可以发出不会阻塞你的 ABS 脚本的“后台”命令(这些命令在一个 [Goroutine](https://tour.golang.org/concurrency/1) 内执行)。

后台命令不同于常规命令，只是因为它在命令本身的末尾使用了一个`&`——让我们来看看它们的运行情况:

```
`sleep 10`echo("Hello world!") # This will be printed after 10s
```

```
`sleep 10 &`echo("Hello world!") # This will be printed immediately
```

您可以使用`.done`属性检查后台命令是否完成:

```
cmd = `sleep 10 &`cmd.done # falsewait(10000)cmd.done # true
```

我们还添加了`wait()`函数，如果您需要阻塞，直到命令完成:

```
cmd = `sleep 10 &`cmd.wait() # The script will be blocked for 10secho("Hello world!")
```

### 混杂的

这个版本中增加了一些新功能:

*   `floor`、`round`、`ceil`等数字功能
*   `cd()`，切换脚本的`cwd`
*   您可以通过设置环境变量`ABS_PROMPT_LIVE_PREFIX=true`和`ABS_PROMPT_PREFIX=templated_string`来调整您的提示。模板化的字符串可以使用`{dir}`、`{user}`、`{host}`，它们将被动态替换。欲了解更多信息，请查看样品[。absrc](https://github.com/abs-lang/abs/blob/d1e92899ed0d6b3abb7a0a3fc6ec18d13dbe3ff2/tests/test-absrc.abs) 文件

### 错误修复

像往常一样，我们设法解决了一些小问题:

*   修正了一些在没有足够参数的情况下调用内置函数时的随机混乱
*   windows 命令现在使用 cmd.exe 而不是 bash，因为 bash 可能在系统上不可用( [#180](https://github.com/abs-lang/abs/pull/180) )
*   解析“无效”数字时更好的错误消息( [#182](https://github.com/abs-lang/abs/pull/182) )
*   ABS 安装程序不支持 wget 1.20.1 ( [#178](https://github.com/abs-lang/abs/pull/178) )
*   ABS 解析器现在支持科学记数法中的数字(例如 8.366100560806463e-7， [#174](https://github.com/abs-lang/abs/pull/174) )
*   内置函数上的错误不会报告正确的错误行号/列号( [#168](https://github.com/abs-lang/abs/pull/168) )

### 现在怎么办？

用简单的一行程序安装 ABS:

```
bash <(curl https://www.abs-lang.org/installer.sh)
```

…开始编写脚本，就像现在是 2019 年一样！

PS:再次感谢埃里希，随着时间的推移，他扮演了更重要的角色。没有他，1.2 中的许多东西都不可能实现！

PPS: [1.3.0 已经在进行中](https://github.com/abs-lang/abs/milestone/10)——预计在四月份的某个时候。我们将推出非常有趣的功能，如取消后台命令的能力，所以这将是一个令人兴奋的版本！

*最初发表于[odino.org](https://odino.org/abs-1-dot-2-background-commands-and-the-ability-to-import-files/)(2019 年 3 月 21 日)。*
*你可以在[推特](https://twitter.com/_odino_)上关注我——欢迎吐槽！*？