# 前端开发人员 GraphQL 指南

> 原文：<https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/>

作者:沙尔克·文特尔

**10 个月前， [Artsy](https://www.artsy.net/) 工程负责人， [Alan Johnson](http://artsy.github.io/author/alan/) 宣称[“我看到了未来，它看起来很像 GraphQL](http://artsy.github.io/blog/2018/05/08/is-graphql-the-future/) ”。**

**快进到我开始这篇文章的 13 天前， [Wired Magazine](https://www.wired.com) 发表了一篇题为 [*脸书如何改变计算*](https://www.wired.com/story/how-facebook-has-changed-computing/) 的故事，强调 [GraphQL](https://graphql.org/) 是“*在改变我们构建服务器的方式以及编写代码的方式方面发挥了巨大作用的技术之一*。**

这些都是大胆的主张！我没有资格反驳或赞同他们。然而，对我来说，以上表明了 GraphQL 生态系统在过去两年中增长的速度有多快——不管我们认为这是好是坏。

这种增长也反映在我作为开发人员的日常经历中。在 [OpenUp](https://openup.org.za) 内部，我们的团队已经在越来越多的产品中稳步采用 GraphQL，因为它解决了许多通常与 REST API 端点相关的[痛点。此外，我们并不孤单。GraphQL 被一系列国际科技团队广泛使用，从](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/) [Pinterest](https://pinterest.com) 、 [Twitter](https://twitter.com/) 、 [Yelp](https://www.yelp.com/developers/graphql/guides/intro) 、[纽约时报](https://www.nytimes.com/)、 [Paypal](https://www.paypal.com) 、 [Atlassian](https://www.atlassian.com/) 、[脸书](https://code.fb.com/core-data/graphql-a-data-query-language/)、 [Github](https://developer.github.com/v4/) ( [等等](https://graphql.org/users/))；还有南非的其他创业公司，如 [GetTruck](https://gettruck.co.za) 、 [Bettr](https://bettr.finance) 和 [Dine4Six](https://dine4six.com) 。

在阅读了上面的 Wired 文章后，我反思了自己在 GraphQL 中的经历(有时是混乱的),我认为编译一个(希望如此！)对开始学习 GraphQL 感兴趣(并避免常见陷阱)的其他前端开发人员的简单指南。

我将下面的内容分成了以下几个大的主题，所以如果您只对 GraphQL 的某个特定方面感兴趣，请随意跳过:

*   [我自己的学习历程图 QL](#9a35)
*   [那么 GraphQL 到底是什么？](#20c2)
*   [graph QL 试图解决什么问题？](#e61e)
*   相当于“Hello World！”的 GraphQL
*   [我应该按照什么顺序学习 GraphQL 概念？](#fbef)
*   后端呢？

### 我自己学习 GraphQL 的旅程

当我第一次听说 GraphQL 的时候，我一直在玩这个叫做[反应](https://reactjs.org/)的东西。我以前用过一点儿[骨干](http://backbonejs.org/)和[角骨](https://angularjs.org/)，但都没有真正坚持下来。

然而，React 的状态管理[功能方法](https://en.wikipedia.org/wiki/Functional_programming)和使用[虚拟 DOM](https://reactjs.org/docs/faq-internals.html) 来减少密集 DOM 操作的性能足迹让我非常兴奋。到目前为止，我已经基本上手工完成了我自己的 [HyperScript](https://github.com/hyperhype/hyperscript) 风格的助手函数来做上面的事情，并期待着摆脱后一个讨厌的函数，而支持`React.createClass()`和`React.createElement()`。

在生产环境中介绍上述内容之前，我认为询问一位资深团队成员对 React 的看法是明智的。然而，任何好的(或者谨慎的)行为都会受到惩罚:我受到了很多后端开发人员在那个时候(甚至有些人今天仍然如此)对 JavaScript 框架的鄙视。然而(也许是作为一种善意的姿态)，他确实提到 GraphQL 这个东西(React 背后的团队正在开发)看起来很有前景。

这使我得出以下结论:

*   **Googling GraphQL。**
*   阅读一些概述。
*   仍然不知道 GraphQL 是什么。
*   **假设它的目标用户可能是至少拥有一个(或多个)计算机科学学位的 web 开发人员(我猜我对 [Web Assembly](https://webassembly.org/) 和 [Houdini](https://github.com/w3c/css-houdini-drafts) )**
*   继续我的生活。

几年后，在被已经成为 React 生态系统的前端超新星的引力吸引之后，我开始摆弄另一个叫做 [Gatsby](https://www.gatsbyjs.org/) 的工具。我一直在努力让 [React 头盔](https://www.npmjs.com/package/react-helmet)与我自己的建立在[static-site-generator-web pack-plugin](https://github.com/markdalgleish/static-site-generator-webpack-plugin)之上的弗兰肯斯坦 React 静态站点生成器玩得很好。

实际上，我在 [ZA Tech](https://zatech.github.io/) 的`#react` [Slack](https://slack.com/) 频道上发现了一条消息，它很好地概括了我当时的挫败感:

> **2017 年 9 月 14 日**

> Schalk Venter 下午 4:02
> 我可能漏掉了一些明显的东西*？*

> 然而，要么是我误解了文档，要么是服务器端的渲染并不像文档中描述的那么简单。*？*

所以盖茨比这个东西似乎给了我我试图用别的方法拼凑出来的东西。然而，当我意识到盖茨比与我以前的宿敌 GraphQL(作为一种从 [Markdown](https://en.wikipedia.org/wiki/Markdown) 文件中查询前端内容和文本内容的手段)串通一气时，我遇到了进一步的挫折。然而，回到我的科学怪人的混乱状态看起来也不令人信服。

*原来我真的别无选择，只能在这个时候学习 GraphQL。*

这意味着我需要找到以下问题的答案:

*   [那么 GraphQL 到底是什么？](#20c2)
*   [graph QL 试图解决什么问题？](#e61e)
*   相当于“Hello World！”的 GraphQL
*   [我应该按照什么顺序学习 GraphQL 概念？](#fbef)
*   后端呢？

### 那么 GraphQL 到底是什么？

> “我已经看到了未来，它看起来很像 GraphQL。记住我的话:在 5 年内，新诞生的全栈应用开发者将不再争论 RESTfulness，因为 REST API 设计将会过时。[…]它允许您将服务器提供的资源和流程建模为特定于领域的语言(DSL)。客户端可以使用它将用 DSL 编写的脚本发送到服务器，以进行批处理和响应。”

> —[Alan Johnson](http://artsy.github.io/author/alan/)([graph QL 是未来吗？](http://artsy.github.io/blog/2018/05/08/is-graphql-the-future/))

天哪，这可能不是我(也不是你，亲爱的读者)想要的答案！

#### 域名是什么？

然而，尽管上面的定义看起来很可怕，但如果我们想知道 GraphQL 到底是什么，对它进行一点解释是很重要的。

*让我们从术语“特定领域语言”开始*:

1.  **首先也是最重要的一点，**特定领域语言(也称为*小型语言*)是一种编程语言，用于表达一种非常特定和预定义类型的数字信息(领域)。而像 [JavaScript](https://en.wikipedia.org/wiki/JavaScript) 这样的[通用语言](https://en.wikipedia.org/wiki/General-purpose_programming_language)可以(类似于瑞士军刀)用来表达一系列数字信息([，在某些情况下，甚至是其创造者在最初](http://www.jsfuck.com/)没有预料到的信息)。这包括从低级原语，如[对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)、[函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)、[字符串](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)、[符号](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)到通用编程模式，如 [HTTP 请求](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)、 [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) 操作和/或[在线数据存储](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)。与通用编程语言相比，特定于领域的语言在表达内容方面往往更加有限(这是有意为之的)。
2.  **其次，** DSL 经常被插入到其他 DSL 或通用语言中，以搭载现有的功能(由于它们有限的范围)。然而，这并不意味着 DSL 与特定的语言相关联(GraphQL 就是一个例子)。例如， [XForms](https://en.wikipedia.org/wiki/XForms) DSL 可以用在 [HTML](https://en.wikipedia.org/wiki/HTML) 中(它本身是在另一个叫做 [SGML](https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language) 的 DSL 之上的 DSL)，同时它也可以用在像 [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) 这样的通用语言中。

***这样是不是有点太书呆子气了？***

对，让我们把它带回来！你可能比你意识到的对 DSL 有更多的经验(不仅仅是通过 HTML！)，而是以下一项或多项:

*   [*CSS*](https://en.wikipedia.org/wiki/Cascading_Style_Sheets)
*   [*JSON*](https://en.wikipedia.org/wiki/JSON)
*   [YAML](https://en.wikipedia.org/wiki/YAML)
*   [*XML*](https://en.wikipedia.org/wiki/XML)

此外，您可能已经对它们狭窄的范围有了第一手的经验。如果我试图通过 HTML 管理数据库或者通过 CSS 控制宇航服，大多数人会翻白眼。

#### DSL 的真正价值

然而，由于它们的范围非常狭窄，与通用语言相比，DSL 往往具有极强的表达能力(换句话说，易于读写)。

作为一个例子，e:

我的一个朋友(也是前同事)， [Greg Kempe](https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/undefined) 在他名为[Open laws](https://openbylaws.org.za)的非盈利项目中使用了一个名为 [Akoma Ntoso](http://www.akomantoso.org/) (基于 [XML](https://en.wikipedia.org/wiki/XML) )的 DSL。Akoma Ntoso(在 [Akan](https://en.wikipedia.org/wiki/Akan_language) 中的意思是“*相连的心*”)是一种专用于以数字方式表达议会、立法和司法文件的 DSL。

例如，请参见下面的[开普敦市户外标牌法规](https://openbylaws.org.za/za-cpt/act/by-law/2001/outdoor-advertising-signage/eng/)(以 Akoma Ntoso 表示):

#### 将它带回前端开发

为了在前端开发的环境中说明上述内容，我们可以看一个常见的例子，其中我们通过 [JavaScript](https://en.wikipedia.org/wiki/JavaScript) 更新我们网站的[文档对象模型](https://en.wikipedia.org/wiki/Document_Object_Model) (DOM)(以便在用户登录时显示特定的消息):

您可以在下面的 Codepen 示例中看到它的运行:

尽管 JavaScript 很强大(即使使用了上面的 ES6 语法)，但在使用 DOM 时，它的表达能力不是很强。幸运的是，有一种 DSL 专门用来更好地表达浏览器 DOM 节点。你可能知道它是 HTML(或者全称:超文本标记语言)。

这意味着我们可以使用`innerHTML`属性(仅用[*Internet Explorer 4*](https://en.wikipedia.org/wiki/Internet_Explorer_4)添加到 JavaScript back)来重写上面的内容。`innerHTML`属性接受用 HTML DSL 编写的字符串:

如你所见，我们仍然得到完全相同的行为:

#### 最后一件事

最后，在我们进入 GraphQL DSL 本身之前，区分 DSL 和像 [TypeScript](https://en.wikipedia.org/wiki/TypeScript) 或 [Sass](https://en.wikipedia.org/wiki/Sass_(stylesheet_language)) 这样的[超集](https://en.wikipedia.org/wiki/Subset)语言可能是有价值的。虽然超集语言旨在扩展现有语言的语法，但 DSL 不需要遵循任何底层语言或环境。例如， [JSX](https://en.wikipedia.org/wiki/React_(JavaScript_library)#JSX) (构建在 [XML](https://en.wikipedia.org/wiki/XML) 之上)可以用来直接与浏览器 DOM 接口，或者以[移动应用](https://en.wikipedia.org/wiki/Mobile_app)的形式与[移动操作系统](https://en.wikipedia.org/wiki/Mobile_operating_system)接口(通过 [React Native](https://facebook.github.io/react-native/) )。

在你键入那个愤怒的回复之前停下来！我知道 DSL/通用语言和 DSL/超集语言之间的上述区别在很多情况下是模糊的，并且(很像网站和 webapps 之间有争议的区别)受制于 [Sorites 悖论](http://Sorites paradox)。然而，正如索里特斯悖论的所有情况一样，模糊的区别是存在的(尽管它们缺乏科学的严谨性)，特别是因为它们在解释日常经验的本质时提供了价值。所以我们姑且称上面的定义为本文中 DSL 的[工作定义](https://en.wiktionary.org/wiki/working_definition)。

### GraphQL 到底想解决什么问题？

与上面类似，GraphQL 是由脸书背后的[技术团队在 2012 年内部创建的，作为 DSL 在脸书移动应用程序中编写更具表现力(和强大)的数据查询(到远程数据 API)。](https://github.com/facebook)

![1*ikw1LeJXVlyPGxA7C0_svQ](img/a24a9ed56cbc0d1c26c3a988b93f8a3d.png)

**问题:常见的 [REST(或表述性状态转移)API](https://en.wikipedia.org/wiki/Representational_state_transfer) 方法大多依赖于固定的数据结构。这意味着在足够的迭代之后，大多数 REST API 最终需要一个 [Lernaean Hydra](https://en.wikipedia.org/wiki/Lernaean_Hydra) 查询来获得特定的数据。**

简而言之(正如 [Chimezie Enyinnaya](https://blog.pusher.com/author/mezie/) 所指出的，他是 [Pusher](https://pusher.com/) 的尼日利亚内容创建者——一种管理远程发布/订阅消息的服务):

> “使用 REST，我们可以用一个`/authors/:id`端点获取作者，然后用另一个`/authors/:id/posts`端点获取该作者的文章。最后，我们可以有一个`/authors/:id/posts/:id/comments`端点来获取帖子上的评论。[……]使用 REST 很容易获取比您需要的更多的数据，因为 REST API 中的每个端点都有一个固定的数据结构，每当被命中时，它都会返回该数据结构。”

> — [奇美齐·恩因那亚](https://blog.pusher.com/author/mezie/) ( [REST 对 GraphQL](https://blog.pusher.com/rest-versus-graphql/) )

事实上，这种情况非常普遍，GraphQL 只是为解决这一问题而设计的几种解决方案之一:

> “有趣的是，网飞或 Coursera 等其他公司也在研究类似的想法，以提高 API 交互的效率。Coursera 设想了一种类似的技术，让客户指定其数据需求，网飞甚至开源了他们名为 Falcor 的解决方案

> — [如何 GraphQL](https://www.howtographql.com) ( [GraphQL 基础:简介](https://www.howtographql.com/basics/0-introduction/))

然而，与上面的一些相反，GraphQL 在三年后以 [MIT 许可证](https://en.wikipedia.org/wiki/MIT_License)发布，今天形成了开源服务背后的主干，如 [Apollo](https://www.apollographql.com/) (使用 GraphQL 读取和/或更改本地和远程应用程序状态)，甚至是 [Gatsby](https://www.gatsbyjs.org/) (使用 GraphQL 从 markdown 文件中查询前端内容和文本内容)。

所以，废话少说，让我们进入这一部分的核心:一个真实的工作示例，它实际上说明了 GraphQL 如何解决上述问题。

为了便于讨论，假设我想知道在 Github 上关注我的用户数量[。我们可以通过本地 JavaScript fetch 方法(从](https://github.com/schalkventer?tab=followers) [Github REST API](https://developer.github.com/v3/) 中)轻松检索数据，然后通过无序列表在 DOM 中显示用户名列表(通过上面的`innerHTML`示例):

**出奇的直白对吧？**

然而，获得关注者列表并不能真正告诉我们太多。我们想知道这些用户在 Github 上有多突出(因为丹·阿布拉莫夫的关注应该与团队实习生约翰尼的关注有所不同)。为了做到这一点，我将使用一个极其不科学的概念，我此后称之为 *Github Equity* (类似于 [Link Equity](https://moz.com/learn/seo/what-is-link-equity) 的搜索引擎概念)。 *Github Equity* 将通过用户维护的总存储库和他们自己的追随者数量来计算。

简而言之:

`Total Repositories * Total Followers`

相当容易！然而，获取执行这个计算所需的数据有点棘手，因为它需要几个异步请求 REST 查询(通过[本地 JavaScript 承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)的方式需要一些编排):

随着数据查询的进行，上述内容仍然在合理的范围内。但是，所有需要的 REST API 调用(并行运行！)使得代码极难阅读和修改。这意味着，即使我们只从前 10 个关注者那里获得所需的信息，也需要总共 31 次 REST API 调用。这意味着我们很快就遇到了[默认的 Github API 速率限制](https://developer.github.com/apps/building-github-apps/understanding-rate-limits-for-github-apps/)(对 API 在一小时内接受来自特定 IP 的请求量的限制)

您可以在下面的 [Codepen](https://codepen.io/schalkventer/pen/ef4598c518037d83bb006529a2d7ad30) 中看到这种情况，如果在一个小时内运行几次(单击几次右下角的`rerun`按钮)，它应该会向 DOM 输出以下错误:`TypeError: response.map is not a function`。换句话说，API 没有返回所需的数组(因为`.map()`只能在 JavaScript 的 [iterables 上运行):](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

幸运的是(除了上面的 REST API)，Github 还通过下面的方式`[https://api.github/graphql](https://api.github/graphql.)` [公开了一个 GraphQL 端点](https://api.github/graphql.)。这意味着我们可以在 GraphQL DSL 中重写上述内容，如下所示:

如果你以前没有遇到过 GraphQL，上面的可能看起来有点令人困惑。然而，我们将在下一节中对语法进行一点解释。

作为题外话，出于说明的目的，我们正在做 GraphQL 的一个非常低级/手工的实现。当在野外遇到 GraphQL 时，你最有可能通过[阿波罗](https://www.apollographql.com/)(由[流星](https://www.meteor.com/)背后的团队开发)或[中继](https://facebook.github.io/relay/)(由[脸书](https://www.facebook.com/)背后的团队开发)的方式遇到。这些库本质上只是一些工具，使得在客户端使用 GraphQL 更加容易。

### 相当于“Hello World！”的 GraphQL

**查询、订阅和突变，天啊！**

对我个人来说，当我刚开始使用一门新的编程语言时(即使它是像 GraphQL 这样的 DSL ),经常会有(看起来)难以理解的信息量。为了让这个过程更容易管理，我总是从同一个问题开始:什么是“Hello World！”这种语言的等价物(后面经常跟什么是这种语言的待办事项 app 等价物)。

简而言之:

> 一句“你好，世界！”程序通常是输出或显示信息“你好，世界！”的计算机程序。因为它在大多数编程语言中非常简单，所以它经常被用来说明编程语言的基本语法，并且经常是那些学习编码的人编写的第一个程序。

> 一句“你好，世界！”program 传统上用于向编程新手介绍编程语言。

> ——[维基百科](https://en.wikipedia.org) ( [“你好，世界！”程序](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)

那么什么是“你好世界！”GraphQL 中的等价？

根据 Julian Mayorga 在他的书 [Fullstack GraphQL](https://www.graphql.college/fullstack-graphql/) 、*中的说法，GraphQL 最简单的形式就是询问对象的特定字段*。所谓的 c *客户端指定的查询在*最初的【2015 GraphQL 规范中:

> 这些查询是在字段级粒度指定的。在大多数不使用 GraphQL 编写的客户端-服务器应用程序中，服务器确定在其各种脚本化端点中返回的数据。另一方面，GraphQL 查询返回的正是客户要求的内容，仅此而已。

> — [GraphQL 工作草案(2015 年 7 月](https://facebook.github.io/graphql/July2015/)

然而，是什么分隔了一个 GraphQL“Hello World！”例如，我们需要将我们的查询与可查询的东西联系起来。

幸运的是，互联网上有几个公开的 GraphQL 端点。从允许一个[查询艾滋病毒耐药性数据的斯坦福大学端点](https://hivdb.stanford.edu/page/graphiql/)到存储按国家排列的[公共人口统计信息集合的端点](https://countries.trevorblades.com/)。

然而，让我们把目光投向人类技术成就的顶峰:一个提供*“你需要的所有神奇宝贝数据”的[神奇宝贝应用程序接口](https://graphql-pokemon.now.sh)。*

作为热身练习，让我们创建下面的查询([)如果您需要](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemons%20(first%3A%2020)%20%7B%0A%20%20%20%20name%0A%20%20%7D%0A%7D)，您可以跟着做:

```
{  pokemons (first: 20) {    name  }}
```

***咩！*** 这是一份令人印象深刻的名单:来自 [Pokedex](https://pokedex.org/) 的 20 个口袋妖怪。然而，让我们把它提高一个档次，找到更具体的东西(如果你没记错的话，这是 GraphQL 真正闪光的地方！).

为了让它变得有趣，假设我们想要确定[皮卡丘](https://pokedex.org/#/pokemon/25)最终进化的平均体重(剧透:是[雷丘](https://pokedex.org/#/pokemon/26))。我们可以使用下面的查询([链接到实例](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20evolutions%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20weight%20%7B%0A%20%20%20%20%20%20%20%20minimum%0A%20%20%20%20%20%20%20%20maximum%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)):

需要注意的是，上述查询实际上是以下内容的简写:

```
query GetPikachuEvolutionWeight {  pokemon(name: "Pikachu") {    evolutions {      weight {        minimum        maximum      }    }  }}
```

即使您没有指定动作，GraphQL 也会假设您将执行查询，这一事实显示了中央查询对 GraphQL 的重要性。

无论如何，无论我们使用上面的哪一个，我们都将从端点获得以下 JSON 响应:

这意味着我们可以对这个 JSON 响应运行我们的`parse()`函数(来自上面的例子),并将结果(29.5)输出到 DOM。

![1*ATRODaaXUt35esV1CCFvwg](img/9d5b858cd758966206cda185f9c9b15d.png)

**简而言之，可以说查询就是字符串形式的小地图([不，不是这种](https://www.lovelyetc.com/diy-map-string-art/)！)由 GraphQL 用来导航数据结构，以便在一次旅程中找到所有请求的项目。**

这意味着我们可以(为了好玩)在 GraphQL DSL 中编写一组类似的真实指令:

### 我应该按照什么顺序学习 GraphQL 概念？

到目前为止，我们只涉及了查询。然而(也许令人惊讶的是)，只要理解以上内容，您就可以在 GraphQL 上走得很远。但是，如果您想充分发挥 GraphQL 的潜力，您可能需要了解几个额外的概念:

GraphQL 查询使用的其他工具/概念:

1.  [**字段**](https://www.apollographql.com/docs/resources/graphql-glossary.html#field) :这是查询中的项目，表面上看起来类似于 JavaScript 对象中的键。比如上面例子中的`paper`、`post_office`和`travel`。
2.  [**参数**](https://www.apollographql.com/docs/resources/graphql-glossary.html#argument) :我们可以传递给字段的可选信息。例如上面例子中的`type: drive`和`amount: 12`。
3.  [**别名**](https://www.apollographql.com/docs/resources/graphql-glossary.html#alias) :一个自定义名称，应该用于字段解析到的 JavaScript 键。默认情况下，对象键与字段同名。例如上面的`post_office`将解析为`{ post_office: { ... } }`。但是，我们可以将它别名如下:`postOffice: post_office`，它将返回`{ postOffice: { ... } }`。这不仅在我们想让键更有语义或者改变大小写时有用，而且在我们两次使用同一个字段时(防止第二个`post_office`的默认键覆盖第一个`post_office`值)也有用。
4.  [**变量**](https://www.apollographql.com/docs/resources/graphql-glossary.html#variable) **:** 刚开始使用 GraphQL 的时候，我做了任何一个通情达理的开发者都会做的事情。我只是在查询中使用了动态插值(通过一个[模板文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)表示)。换句话说，我会为我们上面的口袋妖怪例子做以下事情:`pokemon (name: "${dynamicValue)")`。让 GraphQL 开发人员震惊的是(因为我实际上是在到处制造副作用)！事实证明，GraphQL 具有将外部值传递到查询中的内置功能。通过在查询根处声明变量。例如，使用`query GetPokemonEvolutionWeight($name: string)`和`pokemon (name: $name)`，我们可以在查询被调用时简单地将一个变量对象传递给查询(即`{ name: 'Pikachu' }`)。请注意，您需要将[核心 GraphQL 标量类型](https://www.apollographql.com/docs/apollo-server/schemas/types.html)中的一个分配给一个变量(例如我们上面使用的`string`)。可以创建自己的定制类型，但这超出了本文的范围。

此外，您可能还对探索更高级的 GraphQL 特性感兴趣，这些特性仅涉及过去的查询(本文中有所涉及):

*   [**突变**](https://graphql.org/graphql-js/mutations-and-input-types/) :到目前为止，我们只通过 GraphQL 获取数据。但是，也可以通过 GraphQL 发送数据。这是通过突变实现的，这是 GraphQL 中传统 REST API 端点中的 [POST](https://en.wikipedia.org/wiki/POST_(HTTP)) 的等价物。
*   [](https://www.graph.cool/docs/reference/graphql-api/subscription-api-aip7oojeiv/)**:这本质上是传统 GraphQL 查询的逆向。我们不是向服务器发送检索数据的请求，而是告诉服务器在数据发生变化时通知我们，并按照订阅中的定义将更新的数据发送给我们。**

### **后端呢？**

**简而言之，当我三年前第一次问自己这个问题时，答案是响亮的:**

**别担心这件事。**

**此外，我不确定我的反应是否有所改变。我仍然不知道盖茨比在幕后做了什么来将我的 markdown(通过[Gatsby-transformer-remark](https://www.gatsbyjs.org/packages/gatsby-transformer-remark/))和 JSON(通过 [gatsby-transformer-json](https://www.gatsbyjs.org/packages/gatsby-transformer-json/) )内容转换成 GraphQL 端点。**

**这不是因为冷漠，事实上恰恰相反！我是 Gatsby 的忠实粉丝，正忙于处理一个 [pull 请求，以编程方式触发页面预取](https://github.com/gatsbyjs/gatsby/issues/8122)。考虑到 GraphQL 是如何自文档化的，我还没有必要去窥探 Gatsby 是如何处理 GraphQL 的——不管我的查询或数据变得多么复杂。此外，我还使用了其他几个 GraphQL 服务(例如， [Hygraph](https://hygraph.com/) ，以前的 GraphCMS)，我可以说完全一样。**

****这是否意味着学习如何配置 GraphQL 服务器没有价值？****

**肯定不是！**

**就像任何事情一样，理解一些东西在表面下是如何工作的(无论是 JavaScript、CSS 还是浏览器本身)使得在它们出错时更容易调试。然而，我最近才开始深入研究 GraphQL 的后端，这一事实表明，在不了解端点如何在幕后创建的情况下，您可以走多远。**

**然而，如果你有兴趣学习如何建立一个定制的 GraphQL 服务器，你可以看看我的好朋友 [Shailen Naidoo](https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/undefined) 写的[graph QL for Back-end Developers](https://medium.com/@naidooshailen648/graphql-for-back-end-developers-b4f809417a99)。他是一个非凡的开发人员，这也是我为什么首先考虑 GraphQL 后端的原因。**

### **一锤定音**

**首先，如果你从上到下看完了整面墙的文字，那就做得很好。挺冗长的！我开始写它主要是为了应对缺乏对 GraphQL 的前端具体介绍。这意味着有相当多的地方需要讨论。**

**其次，我绝不是 GraphQL 方面的专家，所以如果有我错过的或者你认为会帮助前端开发人员开始使用 GraphQL 的资源/参考资料，请在评论中告诉我。我非常乐意补充。**

**最后，如果有任何不准确的地方，也可以在评论中告诉我！**

**如果您有兴趣了解有关 GraphQL 的更多信息，请查看以下资源:**

*   **[完整堆栈图 QL](https://www.graphql.college/fullstack-graphql/)**
*   ***[如何绘制 QL](https://www.howtographql.com/) (网站)***
*   ***[官方 GraphQL 文档](https://graphql.org/learn/) *(网站)****

***最后，感谢[沙伊伦·奈杜](https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/undefined)和[齐沙恩·莫德库斯](https://www.freecodecamp.org/news/graphql-for-front-end-developers-1f59808f4435/undefined)提供的反馈和意见。***

***[**在 Github**](https://github.com/schalkventer) **上跟随我，我总是很好奇科技领域的每个人都在做什么——所以我可能会跟随你回来！？*****

***[**【schalkventer】概述**](https://github.com/schalkventer)
[*？前端开发/？UI 设计/？hub.com*社会公益/ ❤️精神疾病去污名化](https://github.com/schalkventer)***