# 轻松创建约曼发电机与约曼-轻松

> 原文：<https://www.freecodecamp.org/news/creating-yeoman-generators-easily-with-yeoman-easily-cf552aef0d2f/>

作者:克里斯特·旺苏帕萨瓦特

# 轻松创建约曼发电机与约曼-轻松

![1*80vnFUltjdnV38Owrjr95g](img/487bf0aff881145889dbe53f1b3ec764.png)

我已经用约曼开始了我的许多项目。这是一个了不起的网络搭建工具。

但是在创建了几次自己的生成器之后，我看到了重复的任务，有些冗长的代码，以及每次都让我困惑的部分生成器代码。

有一次，我结束了对一个小实用程序的黑客攻击，我不停地从一个项目复制到另一个项目。我花了一个周末来组织它，并添加了几个新功能来处理重复的任务。

### 约曼很容易就出生了。

[约曼-轻松](https://github.com/kristw/yeoman-easily)在用约曼创建发电机时，帮助完成以下任务:

#### 优势 1:确认

通常，您会在继续之前要求用户确认。下面的第一个代码块展示了如何用普通的 Yeoman 编写这个代码。第二个代码块展示了如何在 yeoman 的帮助下轻松地编写它。

有了 yeoman-很容易，你可以在进入下一行之前要求确认。`easily.confirmBeforeStart(message)`然后`easily.checkForConfirmation()` 返回结果。

#### **优势#2:提示**

处理来自提示符的结果，然后选择显示哪个提示符过去很复杂。

*   `this.prompt()`返回一个需要处理的承诺，以获得答案并存储它们。答案一般存储在`this.props`中。这段代码必须一遍又一遍地编写。
*   父生成器通常通过选项将参数传递给子生成器。据我所见，许多生成器会隐藏选项中出现的字段提示。(是的，您必须编写代码来检查这一点。)然后把提示和选项的答案组合成`this.props`。

为方便起见，约曼-易:

*   处理将提示中的用户答案存储到`this.props`中。就叫`easily.prompt(prompts)`而不是`this.prompt(prompts)`
*   如果存在同名的选项，可以自动跳过提示。它还会将现有`this.options[field]`的值复制到`this.props[field]`中。
*   可以通过`easily.learnPrompts(prompts)`注册常用提示，并允许在调用`easily.prompt()`时按名称查找提示。如果创建多个询问类似问题的生成器，这可以节省大量时间。

#### **优势三:作曲**

Yeoman generator 可以从另一个包或本地子生成器中调用(`composeWith`)另一个生成器，但是目前这样做的语法有点长。我仍然不确定*本地*字段是什么意思。

yeoman-轻松地将语法简化为`easily.composeWithLocal(name, namespace, options)` 和`easily.composeWithExternal(package, namespace, options)`。

#### **优势#4:文件处理**

Yeoman 为文件处理提供了灵活的 API，覆盖了许多场景。但是执行一些常见的任务需要几行代码，比如将文件从模板目录复制到目标目录。也有批量复制的功能，但是不鼓励使用。

为了解决上述问题，约曼-易:

*   提供包装`this.fs.xxx`的 I/O 函数，并为常见情况解析*模板*和*目的地*目录(从模板到目的地)。这些功能包括`read`、`write`、`writeJSON`、`extendJSON`、`exists`、`copy`和`copyTemplate`。我在我的 [API 文档](https://github.com/kristw/yeoman-easily/blob/master/docs/api.md)中有一个完整的列表。
*   提供基于 glob 模式批量复制静态和动态文件的功能。参见下面例子中的`easily.copyFiles(…)`。

#### 优势 5:方法链接

yeoman-easy 是在考虑到链接和支持流畅编码的方法链接的情况下创建的。

### 把所有的放在一起

这里有一个示例，展示了所有这些优势在一个生成器中的体现:

约曼-轻松包现在可以在 npm 上获得。访问 [github repo](https://github.com/kristw/yeoman-easily) 获取更多细节、 [API 文档](https://github.com/kristw/yeoman-easily/blob/master/docs/api.md)和示例。我欢迎你的请求和错误报告。