# 用 Kakapo.js 动态嘲讽

> 原文：<https://www.freecodecamp.org/news/dynamic-mocking-with-kakapo-js-bdbd3d7b58e2/>

by zzarcon

# 用 Kakapo.js 动态嘲讽

![1*AWc9hdd-JQUromU-wYWeDA](img/51f3ebdaeff8871969a0d38b93eeca79.png)

7 billion people on Earth. [Fewer than 150 Kakapos](http://kakaporecovery.org.nz).

第一次提交 3 个月后， [Kakapo.js](http://github.com/devlucky/Kakapo.js) 发布了第一个版本，我们自豪地宣布，现在它已经可以使用了。让我们给你介绍一下鸮鹦鹉。

> kakapo——Javascript 中的下一代模仿框架

Kakapo 只是一套工具，它试图让你构建网络应用的生活变得更简单，尤其是在创建**客户端模拟**时。它提供了组件和 API，让您可以轻松地在客户端复制后端逻辑和响应。

这并不是什么新鲜事，我很确定你对类似于 [jquery-mockjax](https://github.com/jakerella/jquery-mockjax) 、 [FakeXMLHttpRequest](https://github.com/pretenderjs/FakeXMLHttpRequest) 或 [fetch-mock](https://github.com/wheresrhys/fetch-mock) 的工具很熟悉，这些工具很棒，已经存在很长时间了，但在我看来，它们只是一个大问题解决方案的一部分。

你为什么要在意客户端的嘲笑呢？解决*后端瓶颈* **。**

![1*UJ2ClAEKWpWAM_QF5kj9Kg](img/78fa3cb7efa553bf495ad19135245884.png)

### 后端瓶颈

上次冲刺回顾，在连续三次冲刺错过了超过 50%的计划点后，我们开始问自己哪里出了问题。一些后端开发人员说:

*   *是的，我们认为这是一项简单的任务，但我们不得不花 1 周的时间来重构当前的功能，使其按预期工作……*
*   *太多没有计划的东西出来了，我们不得不处理生产中出现的问题……*
*   *临时服务器根本无法工作，我们不得不重新部署服务超过 5 次…*

另一方面，前端开发人员:

*   *我花了整整一个星期一试图弄清楚为什么端点返回 500 状态码，而不是得到预期的响应…*
*   *我们正在构建用户配置文件，但是没有记录创建端点，所以我们无法发布…*
*   *昨天，我不得不在不同的试运行环境中切换太多次，以至于我没有时间来开发这个功能……*

我对目前的情况感到非常沮丧，特别是，无法在预计的时间内发布一个小功能。我花了很长时间才意识到这不是后端或客户端的问题:这个问题是更深层次的，需要更多的时间和精力来解决。

> 如果不处理后端问题和暂存环境，而是基于事先与后端团队达成一致的 JSON 响应来构建特性，会怎么样？

让我们看一个基本的例子来了解它是如何工作的:

在上面的例子中，我们只是定义了几个端点和一个工厂，然后我们在请求处理程序中定义了一些业务逻辑，以便返回假的响应。为此，我们使用了鸮鹦鹉的三个关键元素:

*   [**路由器**](http://devlucky.github.io/kakapo-js#router) : Kakapo 的路由器识别 URL(路由)并将其分派给相关的处理程序。它还提供了一个**请求**对象作为参数，为您提供关于传入请求的有用信息。
*   [**数据库**](http://devlucky.github.io/kakapo-js#database) :这个类连同**工厂**和**关系**允许你定义你的实体应该如何被表示以及它们的行为。
*   [**服务器**](http://devlucky.github.io/kakapo-js#server) :连接所有其他组件，让你激活或停用；这个特性让你能够在多个数据库和路由器之间切换，我们称之为[场景](http://devlucky.github.io/kakapo-js#scenarios)。

#### 现实生活中的客户端嘲讽

通常模仿 API 是通过为每一个请求创建一个静态 JSON 并测试它来完成的。创建和维护 JSON 是一项重复的任务，并且容易出错。

相反，Kakapo 让你**动态模拟**你的响应，定义它们应该是什么样子，并自动将它们序列化到 JSON 中。

作为一个例子，让我们试着做一个 CRUD

这就是用 Kakapo 复制 CRUD 有多容易，你也可以看看[假数据](http://devlucky.github.io/kakapo-js#fake-data)和[序列化器](http://devlucky.github.io/kakapo-js#serializers)来看看 Kakapo 的一些好东西。

### 技术挑战

除了我们在建造图书馆的过程中学到的所有东西，我想指出一些我们必须面对的最具挑战性的事情:

#### 截击机

[拦截器](http://devlucky.github.io/kakapo-js#interceptors)的组件负责*拦截*用户请求，检查是否匹配任何路由并应用模拟，它们以可插拔的方式设计，用户可以自己定义。目前我们支持浏览器网络 API， [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) 和 [fetch](https://developer.mozilla.org/en/docs/Web/API/Fetch_API) 但是很快我们就会支持 Node.js？。

> 重新发明轮子并不是绝对的坏事。你可以从中学到很多。

我发现这个组件很复杂，因为您必须复制浏览器 API 提供的相同功能，只要您的行为稍有不同，应用程序就可能会崩溃，因为它依赖于本机行为。在直接使用本机 API 而不是使用 jQuery 这样的包装器来构建这些东西时，您可以学到很多东西，因为您将真正理解它的内部工作原理。

在实现拦截器时，我们必须确保不破坏流行的网络库，如 [jQuery](http://api.jquery.com/jquery.ajax/) 和[Superagent](http://visionmedia.github.io/superagent/)；我们创建了 [*集成测试*](https://github.com/devlucky/Kakapo.js/tree/master/test/specs/integration) 以确保 Kakapo 在引入新的变化后继续按预期工作。

#### 测试

开发软件时，测试是必须的，但在其他开发人员可能会使用的开源项目中，测试甚至更为关键。我们在创作鸮鹦鹉的时候一直都有这个想法，这是我第一次严格按照时间顺序完成的项目。我不得不承认，我开始时的感觉和现在完全不同。有时候我觉得写这么多测试会让我们慢下来，但是现在有了[的高代码覆盖率](https://codecov.io/github/devlucky/Kakapo.js?branch=master)，当我不得不重构一个关键组件或者向库中添加一个新特性时，我会感到非常自信。

这是你必须在工作流程中逐步引入的东西，并与团队一起定义。由于这是我参与过的最大的开源项目，我学会了如何与团队合作。有时事情需要讨论两次或更多次，以确保所有成员都在同一页上，但最终会得到解决。

### 文件的重要性

开发人员*讨厌*写文档。不幸的是，它和拥有一个好的库一样重要，并且将是你的用户和贡献者首先看到的。

这么想吧:你已经花了几个月的时间建立你的图书馆，现在终于准备好了，难道你不认为花几天时间[建立一个网站](https://pages.github.com/)并写一些好的例子是值得的吗？

这是来自 React Europe 2016 的一篇演讲，其中 [Christopher Chedeau](https://twitter.com/Vjeux) 解释了脸书如何应对开源库的传播。

#### 吉基尔博士

Jekyll 确实救了我们，它改进了我们写文档的方式和速度。在选择 Jekyll 之前，我曾经用一些 css 和 html 建立静态网站，然后将文档放在那里。然而，一些开发人员可能不熟练，错过了[降价](https://en.wikipedia.org/wiki/Markdown)的简单性。这就是为什么我们决定使用 Jekyll，它可以让你用 [Kramdown](http://kramdown.gettalong.org/) (用类固醇降价)写你的页面，并且与 Github 页面集成。

一旦我们对文档和示例的状态感到满意，我们也希望给新来者留下良好的第一印象。我们创建了一个[脚本](https://github.com/devlucky/Kakapo.js/blob/master/create-readme.js)，它从 [github 页面](https://github.com/devlucky/devlucky.github.io)获取 **md 文件**，添加一些内容并输出[自述文件](https://github.com/devlucky/Kakapo.js/blob/master/README.md)？

#### 演示应用

每个人都喜欢演示，它们展示了你的库做了什么以及它是如何做的。这听起来可能很奇怪，但是它也将帮助您学习如何使用您自己的库，以及查找 bug 或缺失的特性。

直到我们使用 Kakapo 构建了我们的第一个 [todo 应用](https://kakapo-todo-app.firebaseapp.com/)之前，我们没有意识到主要的痛点以及如何解决它们，这就是为什么我们后来构建了我们的第二个演示应用，一个 [github explorer](https://kakapo-github-explorer.firebaseapp.com/) 。

> 拥有一个没有文档的好库就像拥有一枚无人知晓如何使用的火箭。

### 路标

这个项目刚刚开始，但我们对它有雄心勃勃的计划；请随时查看[未解决的问题](https://github.com/devlucky/Kakapo.js/issues)或提出新问题，我们将不胜感激！以下是一些最重要的:

*   完整的 [**JSON API**](http://jsonapi.org) 序列化程序支持
*   **Node.js** 拦截器支持
*   **异步处理程序**支持

我们也在努力完成 Kakapo 的 Swift 版本(T1 ),它几乎已经准备好进入测试阶段，我们认为它将成为构建 iOS 应用程序的游戏规则改变者:敬请关注！？