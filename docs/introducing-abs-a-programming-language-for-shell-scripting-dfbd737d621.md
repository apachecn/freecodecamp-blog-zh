# 介绍 ABS，一种用于 shell 脚本的编程语言

> 原文：<https://www.freecodecamp.org/news/introducing-abs-a-programming-language-for-shell-scripting-dfbd737d621/>

亚历克斯·纳达林

# 介绍 ABS，一种用于 shell 脚本的编程语言

在过去的几天里，我花了一些时间做了一个我想了很久的项目，Bash 的脚本替代:让我向你介绍一下 [ABS 编程语言](https://www.abs-lang.org/)。

![SEQ45gplyveMOk8nKiL8d-w5azHRSEjtC7-8](img/b18250d85b430f3fdde3884acfabe528.png)

### 为什么

让我长话短说:我们都喜欢 shell 编程——无需太多努力就能自动化重复的任务。

我们可能会同意，shell 编程在语法方面也有点疯狂:

```
if [ -z $STRING ]; then    ...fi
```

比如，嗯，*什么鬼？菲。-z？括号？*

与 Bash 或通用 shell 编程语言的斗争有时会变得非常激烈。编写代码，例如:

```
if (this == that) {    parts = this.split("/").filter(...).map(...)}
```

如果你用贝壳会让你热泪盈眶。

现在，你可以用任何主流编程语言做类似的事情(上面的例子是 valid JavasScript):这些语言不擅长的是它们与底层系统的集成——从这个角度来看， [shell 只是更加精确/强大。](https://stackoverflow.com/questions/796319/strengths-of-shell-scripting-compared-to-python)

想象您可以运行如下代码:

```
host = $(hostname)
```

```
if (host == "johns_computer") {    ...}
```

好了，您不必再“想象”了:ABS 是一种结合了快速简单的系统命令和更优雅的语法的语言。

把它当成自《糖果》以来最好的东西，只记得这是 ABS 的作者给你的定义。但是说真的，这太方便了。

不相信我？请继续阅读！

### 例子

我是“*给我看看代码”的坚定信徒！*“口头禅，所以我们赶紧言归正传吧。在 ABS 中运行 shell 命令非常容易:

```
# Get the content of your hostfile$(cat /etc/hosts)
```

管道也可以工作:

```
# Check if a domain is in your hostfile$(cat /etc/hosts | grep domain.com | wc -l)
```

此时，我们可以捕获命令的输出，并在其上编写脚本:

```
# Check if a domain is in your hostfilematches = $(cat /etc/hosts | grep domain.com | wc -l)
```

```
# If so, print an awesome stringif matches.int() > 0 {  echo("We got ya!")}
```

这不会发生，但假设发生了错误:

```
# Check if a domain is in your hostfilematches = $(cat /etc/hosts | grep domain.com | wc -l)
```

```
if !matches.ok {    echo("How do you even...")}
```

```
# If so, print an awesome stringif matches.int() > 0 {  echo("We got ya!")}
```

我们可以把它变得更普遍一点:

```
$ cat script.abs# Usage $ abs script.abs domain.com# Check if a domain is in your hostfiledomain = arg(2)matches = $(cat /etc/hosts | grep $domain | wc -l)
```

```
if !matches.ok {    echo("How do you even...")}
```

```
# If so, print an awesome stringif matches.int() > 0 {  echo("We got %s!", domain)}
```

现在，弦乐相当无聊，所以我们可以尝试一些更有趣的东西:

```
# Say we're getting some JSON from a commandx = $(echo '{"some": {"dope": "json"}}')x.json().some.dope # "json"
```

```
# Arrays, you say?tz = $(cat /etc/timezone) # "Asia/Dubai"parts = tz.split("/") # ["Asia", "Dubai"]
```

```
# You better destructure the hell out of that![continent, city] = tz.split("/")
```

…等等。你可以用腹肌做很多“常规”的事情，所以我就不多讲了——让我给你看看更奇怪的部分:

```
# Avoiding the bug that happened because# we forgot to compare strings case-insensitively"HELLO" ~ "hello" # true
```

```
# Just range1..3 # [1, 2, 3]
```

```
# Combined comparison operator (thanks Ruby!)5 <=> 5 # 05 <=> 6 # -16 <=> 5 # 1
```

```
# Classic short-circuiting1 && 2 # 21 || 2 # 1
```

你可以在 15 分钟内浏览整个[文档](https://www.abs-lang.org/introduction/why-another-scripting-language): ABS 的目标不是成为一种通用的、功能丰富的语言，所以它的范围不是很广。此外，如果你使用过 JavaScript、Python 或 Ruby 之类的语言，你就不会有习惯 ABS 的问题。

### 现在会发生什么？

你可以去 ABS 的网站了解更多关于这种语言的知识。勇敢的人会去 [ABS 的 github repo](https://github.com/abs-lang/abs) 和[下载一个版本](https://github.com/abs-lang/abs/releases)在本地安装。

勇敢的人只会:

```
bash <(curl https://www.abs-lang.org/installer.sh)
```

*(你可能需要在那之前先做一个动作)*

你会是哪一个？

![964Hq52XCeztEs5UKsPayCvFgCTtLfD1pcRZ](img/4eaa6d6f1893fc41cc87984cc840df51.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/XMFZqrGyV-Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*最初发表于[odino.org](https://odino.org/introducing-abs-a-programming-language-for-shell-scripting/)(2018 年 12 月 25 日)。*
*你可以在[推特](https://twitter.com/_odino_)上关注我——欢迎吐槽！*？