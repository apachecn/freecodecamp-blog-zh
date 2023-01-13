# Firebase: 5 种太常见的误解

> 原文：<https://www.freecodecamp.org/news/firebase-5-way-too-common-misconceptions-93b843ee1b93/>

凯瑟琳·格里菲斯

# Firebase: 5 种太常见的误解

![1*zdrREji23Eu5dqVVJE1E7w](img/d65cc15b9d4b838d2eaabe70fbf9fe41.png)

最近在网上看了很多关于 Firebase 的评论。必须指出的是，大部分是由讨厌 Firebase 的开发人员编写的。

[“复杂的查询是不可能的！”](https://crisp.chat/blog/why-you-should-never-use-firebase-realtime-database/)说一个。

[“哑数据建模！”](https://medium.freecodecamp.com/firebase-the-great-the-meh-and-the-ugly-a07252fbcf15)另一个说。

"[一切都必须是客户端的！另一个抱怨道。](https://lugassy.net/why-im-dumping-firebase-for-web-cd64a78cb43e)

你几乎可以听到他们额头上的小青筋。

不过，我明白了。他们遇到的问题令人沮丧。但是很大一部分问题是缺乏对 Firebase 的了解，它能做什么(和不能做什么！)做。

过去几个月我一直在和 Firebase 合作。在 [FFunction](https://ffctn.com/?utm_source=medium&utm_medium=5-firebase-myths&utm_campaign=general-pr) 中，我们正在使用 Firebase 构建一个名为 Min 的[项目规划工具。对于这种后端即服务(BaaS)解决方案，存在一些严重的误解/谬论/误解。所以，我想在这里解开其中一些。](https://www.min.team/?utm_source=medium&utm_medium=5-firebase-myths&utm_campaign=early-access)

顺便说一下:我们和 Firebase 没有任何关系。我想对正在变得相当一边倒的讨论补充一点细微差别。

### 误解 1: Firebase 只是客户端的

直到最近，Firebase 还是一项专有的客户端技术。但是，它提供了存储和查询功能。但是大多数应用程序逻辑必须在客户端上存在和执行。对于许多开发商来说，这是一个完全的交易破坏者。毕竟，如今有多少 web 应用程序不需要后端？

Firebase 团队实际上听取了开发人员社区的要求。2017 年 3 月，他们为 Firebase 推出了[云功能。有了云功能，你可以在谷歌云上保存代码片段。该代码将运行以响应特定的 Firebase 事件和 HTTP 请求。例如，如果你想在保存数据到数据库时进行数据处理，](https://firebase.googleblog.com/2017/03/introducing-cloud-functions-for-firebase.html)[你可以这样做](https://firebase.google.com/docs/functions/use-cases#perform_database_sanitization_and_maintenance)。同样，如果你不希望你的应用程序的 API 键暴露在你的客户端代码中，[你也可以这么做](https://firebase.google.com/docs/functions/use-cases#integrate_with_third-party_services_and_apis)。

如果你有兴趣了解更多，我推荐你去看看 Firebase 文档的云功能(其中有一些很棒的例子)和[这些最近发布的教程](https://www.youtube.com/playlist?list=PLl-K7zZEsYLkPZHe41m4jfAxUi0JjLgSM)。

### 误解 2: Firebase 导致意大利面代码

这让我想起了那句老话:“拙匠怨工具！”但是让我们更深入地研究一下这个问题。

根据我迄今为止的经验，Firebase 不会产生意大利面条代码*。由于 Firebase 大部分是客户端的，你的大部分后端逻辑将会在客户端完成。如果你不小心，你可能会得到一堆混乱的、不可维护的代码。*

在开发 [Min](https://www.min.team/?utm_source=medium&utm_medium=5-firebase-myths&utm_campaign=early-access) 的早期阶段，我们花了很多时间来规划应用。我们计划如何建模我们的数据、我们的数据库结构以及与我们的数据交互的最佳方式。我们创建了一个与 Firebase 通信的连接器。它拥有进行 CRUD 操作和与 Firebase 交互的所有代码。我们构建了一个类集合来根据 Firebase 数据库结构处理对象的数据。

这种抽象帮助我们将与 Firebase 相关的逻辑和应用程序逻辑分开。我们的代码易于维护和调试。

### 误解 3:愚蠢的数据建模/太多的重复

正如 Firebase 团队所描述的，Firebase 数据库[只是一棵巨大的 JSON 树](https://firebase.google.com/docs/database/web/structure-data#how_data_is_structured_its_a_json_tree)。数据被存储为键值对的集合，并且可以具有您想要的任何宽度和深度。存储数据的方式有很大的灵活性，如果不小心的话，会给你带来很多麻烦。让我用一个例子来说明。

假设您正在构建一个基本的项目管理应用程序。您有用户和任务。可以给用户分配任务。您可能希望将所有任务信息存储在“任务”节点下的一个数据库位置:

```
tasks : {    "001" : {        name         : "Development Round 1"        description  : "Lorem ipsum dolor sit amet elit..."        startDate    : "20170101"        endDate      : "20170201"        loggedHours  : {            "20170101" : "1.66"            "20170102" : "7"            "20170103" : "5.5"        }        assignedStaff : "Cathryn"    }    "002" : {        name : "Development Round 2"        description : "Mauris quis turpis ut ante..."        startDate   : "20170206"        endDate     : "20170228"        loggedHours  : {            "20170206" : "3"            "20170207" : "1"            "20170208" : "4.75"        }        assignedStaff : "Sam"    }    "003" : {        name : "Browser Testing"        description : "Vivamus nec ligula et nulla blandit..."        startDate   : "20170301"        endDate     : "20170303"        loggedHours  : {            "20170301" : "1"            "20170301" : "3"        }        assignedStaff : "Cathryn"    }}
```

现在，假设您想要显示分配给 Cathryn 的所有任务的任务名称。为此，您可以查询数据库以返回所有“assignedStaff”值为“Cathryn”的任务。

```
firebase.database().ref(“tasks/”).orderByValue(“assignedStaff”).equalTo(“Cathryn”).once(“value”);
```

这个查询的问题是，它将返回分配给 Cathryn 的给定任务的任务信息，而不仅仅是任务名称。下载大量不必要的数据。

为了解决这个问题，Firebase 建议你[去规范化你的数据](https://firebase.googleblog.com/2013/04/denormalizing-your-data-is-normal.html) ( [这里有一个很棒的方法](https://www.youtube.com/watch?v=vKqXSZLLnHA&index=6&list=PLl-K7zZEsYLlP-k-RKFa7RyNPa9_wCH2s&t=1s))。反规范化是在整个数据库中存储冗余数据副本以提高读取性能的过程。在我们的示例中，我们可以通过向数据库中添加以下内容来反规范化我们的数据:

```
tasksByUser : {    "Cathryn" : {        "001" : "Development Round 1"        "003" : "Browser Testing"    }    "Sam" : {        "002" : "Development Round 2"    }}
```

现在，如果我们想要检索分配给 Cathryn 的所有任务的任务名称，我们只需要从一个数据库位置读取:

```
firebase.database().ref(“/tasksByUser/Cathryn”).once(“value”);
```

与前面的查询相比，这将返回分配给 Cathryn 的任务的名称。从长远来看，这将导致更快的读取操作和更好的性能。

对某些人来说，反规格化似乎是一种黑客行为。但是在为复杂的可伸缩的 web 应用程序设计 Firebase 数据库时，这是一个必要的策略。它要求你对你想要存储的数据以及你将如何使用它有深刻的理解。

在开始构建您的 Firebase 数据库之前。花点时间了解一下[反规范化](https://www.youtube.com/watch?v=vKqXSZLLnHA)、[如何组织你的数据](https://firebase.google.com/docs/database/web/structure-data)、[如何保持反规范化数据的一致性](https://firebase.googleblog.com/2015/09/introducing-multi-location-updates-and_86.html)。正如 Firebase 团队所说，“*考虑磁盘空间是廉价的，但用户的时间不是。*”

如果你的阅读速度很慢，那么你的用户很可能不会停留在你身边。

此外，返回不必要数据的低效查询可能会造成巨大的经济损失。根据你的 Firebase 定价计划，你可能需要支付高达每 GB 下载 1[美元的费用。](https://firebase.google.com/pricing/)

### 误解 4: Firebase 会导致数据不一致

如果你正在以正确的方式设计你的 Firebase 数据库。您的数据有可能在多个数据库位置被反规范化。如果您的数据存储在多个位置，那么您可能想知道…“我如何保持所有数据的一致性？”

在正常情况下，当向 Firebase 发送数据时，您需要指定一个数据库路径和要存储在那里的数据。让我们回到这个例子，为了更新一个任务名(在使用反规范化之前)，我会这样做:

```
firebase.database().ref(“tasks/001/name”).set(“Here’s the new name”);
```

现在，通过反规范化，我可以通过执行两个写操作来更新任务的名称:

```
firebase.database().ref(“tasks/001/name”).set(“Here’s the new name”);
```

```
firebase.database().ref(“tasksByUser/Cathryn/001”).set(“Here’s the new name”);
```

但是执行这两个写操作会导致数据不一致。如果其中一个写操作失败，而另一个没有失败，该怎么办？我需要的是一个原子写操作，允许我同时写入数据库路径。为此，Firebase 提供了[多路径更新](https://firebase.googleblog.com/2015/09/introducing-multi-location-updates-and_86.html)来解决这个问题。你可以在这里观看这个[的精彩操作。现在，要更新任务名称，我们只需执行以下操作:](https://www.youtube.com/watch?v=i1n9Kw3AORw&index=7&list=PLl-K7zZEsYLlP-k-RKFa7RyNPa9_wCH2s&t=4s)

```
firebase.database().ref().update({    “tasks/001/name” : “Here’s the new name”,    “tasksByUser/Cathryn/001” : “Here’s the new name”});
```

如果任何数据库路径的更新失败，整个更新都将失败。只要您使用多路径更新，您的数据应该总是一致的。

### 误解 5:非常有限的查询能力

Firebase 的查询能力有限。您可以按键或值对数据进行排序，也可以按等式或使用范围对数据进行筛选。

例如，使用任务和用户的例子。我可以通过查询来检索开始于 20170601、之前或之后的任务。我还可以进行查询，以检索分配给 Cathryn 的所有任务，或者 20170203 记录了小时数的任务。但是我*不能*做的是通过多个值或者键过滤。例如，我不能进行查询来检索分配给凯瑟琳*和*在 20170601 之后开始的任务。用于检索记录了 20170203 和 20170304 小时数的任务的查询也将不起作用。

对多个键或值的查询或过滤不起作用，开发人员喜欢抱怨这一点。但是如果他们做了研究，他们会意识到这其实有一个很好的理由。由于 Firebase 是一个实时数据库，并且是为速度而设计的， [Firebase 只支持它能保证快](https://firebase.googleblog.com/2013/04/denormalizing-your-data-is-normal.html)的操作。如果你想做一些复杂的查询，你必须相应地设计你的数据库。复杂的查询并非不可能，您必须提前为它们做好计划。

例如，如果我想查询从 20170201 开始分配给 Cathryn 的所有任务。我可以将“staff_startDate”属性添加到我的任务中，如下所示:

```
tasks : {    "001" : {        ...        startDate       : "20170101"        assignedStaff   : "Cathryn"        staff_startDate : “Cathryn_20170101”        ...    }    ...}
```

然后，我只需要这样查询它:

```
firebase.database().ref(“/tasks/”).orderByChild(“staff_startDate”).equalTo(“Cathryn_20170101”);
```

我强烈推荐你观看为 Firebase 数据库和 [Firebase 数据库查询 101](https://www.youtube.com/watch?v=3WTQZV5-roY) 转换的[通用 SQL 查询。了解如何构建和查询数据将允许您进行更高级的查询。这将省去你今后许多头疼的事。](https://www.youtube.com/watch?v=sKFLI5FOOHs&t=7s)

我决定遵循本文的 Firebase 最佳实践清单。如果你把你的电子邮件放在下面的盒子里，我会把它发给你。

同时，我想听听其他使用 Firebase 的开发人员的意见。

还有人喜欢 Firebase 吗？

没那么多？

用头撞屏幕？

告诉我，开发者们。评论开放。