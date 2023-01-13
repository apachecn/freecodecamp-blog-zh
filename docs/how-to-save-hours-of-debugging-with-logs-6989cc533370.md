# 如何用日志节省调试时间

> 原文：<https://www.freecodecamp.org/news/how-to-save-hours-of-debugging-with-logs-6989cc533370/>

作者:玛雅·吉拉德

# 如何用日志节省调试时间

![PvGBkHEkjiplPxRBeopPD9Euiem89qyAbdnQ](img/b8a4ad250f38788eb79b282e6a5d31b6.png)

一个好的日志机制可以在我们需要的时候帮助我们。

当我们处理生产故障或试图理解意外响应时，日志可能是我们最好的朋友，也可能是我们最大的敌人。

它们对我们处理失败的能力非常重要。在日常工作中，当我们设计新的生产服务/功能时，我们有时会忽略它们的重要性。我们忽视了对他们的适当关注。

当我开始开发时，我犯了一些日志错误，让我度过了许多不眠之夜。现在，我更清楚了，我可以和你分享几年来我学到的一些做法。

### 磁盘空间不足

当在本地机器上开发时，我们通常不介意使用文件处理程序来记录日志。我们的本地磁盘非常大，而写入的日志条目非常少。

在我们的生产机器中，情况并非如此。他们的本地磁盘通常只有有限的可用磁盘空间。最终，磁盘空间将无法存储生产服务的日志条目。因此，使用文件处理程序最终会导致所有新的日志条目丢失。

如果你想让你的日志在服务的本地磁盘上可用，不要忘记使用一个旋转文件处理器。这个可以限制你的日志将消耗的最大空间。循环文件处理程序将覆盖旧的日志条目，为新的日志条目腾出空间。

### Eeny，meeny，miny，moe

![Pt2k019xulsxiiGcOhkWJ6R4Nm3QpN-THYid](img/dfcc28859c4ae91c1c312cb120812e63.png)

我们的生产服务通常分布在多台机器上。搜索特定的日志条目需要调查所有条目。当我们急着修复我们的服务时，没有时间浪费在试图找出错误到底发生在哪里。

不要将日志保存在本地磁盘上，而是将它们传输到一个集中的日志记录系统中。这使您可以同时搜索所有内容。

如果你使用 AWS 或 GCP，你可以使用他们的日志代理。代理将负责将日志传输到他们的日志搜索引擎中。

### 记录还是不记录？这是个问题…

![ZU2WKnvPTiAv6QIh8TUUQcK92TU5mSXgHjL3](img/4de681daee6e67726facb306eab4dc90.png)

太少和太多的日志之间只有一线之隔。在我看来，日志条目应该是有意义的，并且只用于调查我们生产环境中的问题。当您准备添加一个新的日志条目时，您应该考虑将来如何使用它。试着回答这个问题:**日志消息给会阅读它的开发者提供了什么信息？**

我多次看到日志被用于用户分析。是的，在日志条目中写入“用户 watermelon2018 点击了按钮”要比开发新的事件基础设施容易得多。这不是日志的意义所在(解析日志条目也不好玩，所以提取洞察力需要时间)。

### 落进草垛里的一根针(指：几乎不可能找到的东西)

在下面的屏幕截图中，我们看到我们的服务处理了三个请求。

处理第二个请求花了多长时间？是 1ms，4ms 还是 6ms？

```
2018-10-21 22:39:07,051 - simple_example - INFO - entered request 2018-10-21 
22:39:07,053 - simple_example - INFO - entered request 2018-10-21 
22:39:07,054 - simple_example - INFO - ended request 2018-10-21 
22:39:07,056 - simple_example - INFO - entered request 2018-10-21 
22:39:07,057 - simple_example - INFO - ended request 2018-10-21 
22:39:07,059 - simple_example - INFO - ended request
```

因为我们没有关于每个日志条目的任何附加信息，所以我们不能确定哪个是正确答案。在每个日志条目中包含请求 id 可以将可能的答案数量减少到一个。此外，**在每个日志条目中包含元数据可以帮助我们过滤日志**并关注相关条目。

让我们向日志条目添加一些元数据:

```
2018-10-21 23:17:09,139 - INFO - entered request 1 - simple_example
2018-10-21 23:17:09,141 - INFO - entered request 2 - simple_example
2018-10-21 23:17:09,142 - INFO - ended request id 2 - simple_example
2018-10-21 23:17:09,143 - INFO - req 1 invalid request structure - simple_example
2018-10-21 23:17:09,144 - INFO - entered request 3 - simple_example
2018-10-21 23:17:09,145 - INFO - ended request id 1 - simple_example
2018-10-21 23:17:09,147 - INFO - ended request id 3 - simple_example
```

元数据作为条目的自由文本部分的一部分。因此，每个开发人员都可以执行自己的标准和风格。这将导致复杂的搜索。

我们的元数据应该被定义为条目固定结构的一部分。

```
2018-10-21 22:45:38,325 - simple_example - INFO - user/create - req 1 - entered request
2018-10-21 22:45:38,328 - simple_example - INFO - user/login - req 2 - entered request
2018-10-21 22:45:38,329 - simple_example - INFO - user/login - req 2 - ended request
2018-10-21 22:45:38,331 - simple_example - INFO - user/create - req 3 - entered request
2018-10-21 22:45:38,333 - simple_example - INFO - user/create - req 1 - ended request
2018-10-21 22:45:38,335 - simple_example - INFO - user/create - req 3 - ended request
```

日志中的每条消息都被我们的元数据推到了一边。因为我们从左向右阅读，我们应该把信息放在尽可能靠近行首的地方。此外，将信息放在开头会“破坏”行的结构。这有助于我们更快地识别消息。

```
2018-10-21 23:10:02,097 - INFO - entered request [user/create] [req: 1] - simple_example
2018-10-21 23:10:02,099 - INFO - entered request [user/login] [req: 2] - simple_example
2018-10-21 23:10:02,101 - INFO - ended request [user/login] [req: 2] - simple_example
2018-10-21 23:10:02,102 - INFO - entered request [user/create] [req: 3] - simple_example
2018-10-21 23:10:02,104 - INFO - ended request [user/create [req: 1] - simple_example
2018-10-21 23:10:02,107 - INFO - ended request [user/create] [req: 3] - simple_example
```

将时间戳和日志级别放在消息之前有助于我们理解事件的流程。其余的元数据主要用于过滤。在这个阶段，它不再是必需的，可以放在生产线的末端。

INFO 下记录的错误将在所有正常日志条目之间丢失。**使用整个范围的日志级别(错误、调试等)。)可以大幅减少搜索时间**。如果你想了解更多关于日志级别的内容，你可以在这里继续阅读。

```
2018-10-21 23:12:39,497 - INFO - entered request [user/create] [req: 1] - simple_example
2018-10-21 23:12:39,500 - INFO - entered request [user/login] [req: 2] - simple_example
2018-10-21 23:12:39,502 - INFO - ended request [user/login] [req: 2] - simple_example
2018-10-21 23:12:39,504 - ERROR - invalid request structure [user/login] [req: 1] - simple_example
2018-10-21 23:12:39,506 - INFO - entered request [user/create] [req: 3] - simple_example
2018-10-21 23:12:39,507 - INFO - ended request [user/create [req: 1] - simple_example
2018-10-21 23:12:39,509 - INFO - ended request [user/create] [req: 3] - simple_example
```

### 日志分析

在文件中搜索日志条目是一个漫长而令人沮丧的过程。它通常需要我们处理非常大的文件，有时甚至需要使用正则表达式。

如今，我们可以**利用快速搜索引擎**如弹性搜索，并在其中索引我们的日志条目。使用 ELK stack 还将为您提供分析日志和回答以下问题的能力:

1.  错误是否局限于一台机器？还是发生在所有的环境中？
2.  错误是什么时候开始的？错误的发生率是多少？

能够对日志条目执行聚合可以为可能的失败原因提供提示，而仅仅通过读取几个日志条目是不会注意到这些原因的。

总之，不要认为日志记录是理所当然的。在你开发的每个新功能上，想想未来的自己，哪些日志条目会对你有帮助，哪些只会让你分心。

记住:只有在你允许的情况下，你的日志才能帮助你解决生产问题。