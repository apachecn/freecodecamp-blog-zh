# 我们进入微服务世界的旅程，以及从中我们学到了什么。

> 原文：<https://www.freecodecamp.org/news/our-journey-into-the-world-of-microservices-and-what-we-learned-from-it-d255b9a2a654/>

伊格纳西奥·萨拉扎·威廉姆斯

# 我们进入微服务世界的旅程，以及从中我们学到了什么。

![QTmUvRWczec44xHTrgf7UoeVK47ivPHRNaaf](img/06ab108075aa2368c7805a282a4cbfd2.png)

“An overhead shot of a compass laid out on an old map” by [Himesh Kumar Behera](https://unsplash.com/@whoishimesh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我知道，我知道大家都在说微服务。那些谈论数字转型的人说，这是一种将引领我们走向建筑未来的模式。对于其他人来说，它是“巨石的破坏者”，是解决我们所有建筑问题的银弹。

我给你讲讲微服务吧。他们真的很了不起，但不仅仅是像**泡芙** * *一些魔法* * * *仙尘* *这是我们所有问题的解决方案。我**不会**告诉你更多关于他们的模式，而是我会尽我所能讲述这个故事(我的故事)。我将讨论这个概念，这个模式，是如何在现实中，在某些情况下发展起来的。我称之为**微型大决战**。

![kHXZp0ke9-N0aBMf6LeqPV5RrjZaDIKjCtOP](img/5e0ce28ba14dae00d0a8b034f6bba18b.png)

Source from [GIPHY](https://giphy.com/gifs/oscars-awards-academy-Ho2mVZ5dvsW7S)

在我的团队中，每天都有超出我们能力范围的事情发生，并导致问题。但这只是一个看到全局的问题，并以不断改进我们的组件的心态工作，直到我们达到我们作为一个团队所拥有的质量标准。

所以，请跟随我踏上这条有好有坏，有欢笑有泪水，还有很多“我们当初到底为什么要这么做？”

> *TL；博士？*
> *我知道这看起来很多，但是让我告诉你一些事情。如果你想从别人关于微服务的错误中吸取教训，我强烈推荐你完整地阅读这篇文章。如果没有，你可以直接跳到迷因——至少它会让你发笑！*

### 一点背景知识

![HFuGKP76J88kmX5hEOYDvZQnocRWHcqehIql](img/ca15e4ff39b67c14c7b65017fb86b402.png)

Source From [Innoview](http://innoview.hu)

让我们从基础开始。有我(嗨！)，一个刚毕业的计算机专业的学生，刚刚被一些咨询公司录用(乔布斯的狂野西部)。我为我们的一个客户(在他们的办公室)分配了这个项目，在这个项目中，我们的团队负责将数字化转型应用到他们的业务中。所以，微服务就参与进来了。(现在我在这个领域更有经验了，我经常听到这两个概念在一起。)

我们使用的是[节点。js 作为后端编程语言(ohhhh yeeees)，这意味着我们也使用](https://nodejs.org/en/) [Express](http://expressjs.com/) 作为默认框架来公开 API。此外，重要的是，我们使用了敏捷方法学 [Scrum](https://medium.com/chingu/a-short-introduction-to-the-scrum-methodology-7a23431b9f17) (你很快就会明白我为什么提出这一点)。

#### 各队

![AyGe9CCxGMuw6dKE2txFxzMYtm-Dg511LMTF](img/cbbbffb8870df1ed6f855eb245ca9b0e.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们被分成两大组:第一组，也是我所在的一组，是**架构团队。**我们负责为团队定位，并宣传微服务。第二个团队，**开发团队，**负责开发业务所需的产品。有多个团队同时在开发不同的产品，同时也在开发微服务。

**架构团队**看起来像这样:

*   1 名高级经理(我们的)
*   2 名经理(1 名我们的，1 名他们的)
*   10 名建筑师(6 名我们的，4 名他们的)
*   2 负责大数据
*   3 主管 CI/CD
*   3 负责安全
*   2 负责后端/前端开发架构

每个**开发团队**看起来都像这样:

*   1 名 Scrum 大师(另一名顾问)
*   1 个产品所有者(客户端)
*   4 名开发人员(2 名我们的，2 名他们的)
*   1 质量保证
*   1 UX
*   1 名架构师(客户端)

> *我知道这听起来很糟糕，把这一切都搞混了，但是不要担心，对我们来说，这是一个让事情变好的机会……*

#### 开发人员背景技能

![jmm-Fjij7bveSXWDVpeKMQVaeHMijUZyp8oG](img/645e79c08d1c892d9b52a52bce98a176.png)

> 我们都不是天生的熟练开发者，我们都像一只试图编译一个基本的“Hello World”的猴子。费利佩·拉佐

我们的团队成员有各种各样的背景，从那些几乎不知道他们自己的计算机如何工作的人，到那些可能来自 NASA 的人。一些人曾经使用过 COBOL、Java、JavaScript、C、Python 等语言，而另一些人从未使用过任何语言。

因此，如果一些团队成员不特别擅长开发良好的代码和结构，这很容易理解，因为他们中的许多人以前没有这方面的背景。同样，还有其他人有一些经验。因此，拥有所有这些不同的配置文件是非常好的，但如何充分利用它们取决于我们。我们不认为这是一个弱点，而是我们团队的一个机会(特别是当你在一个敏捷的环境中工作的时候)。

### 目标

在这里，我们的目标是将微服务作为后端解决方案来实现，以集成我们客户的遗留组件。我们计划将它们公开为简单的 API，以便团队可以将它们集成到他们的应用程序中。

以下是我们微服务的初始要求:

*   他们必须使用 SOAP 服务，并将结果作为 JSON 返回。我知道对你们大多数人(包括我)来说，这听起来很糟糕。但它必须是这样的，因为微服务没有被授权直接连接到数据层，所以它们必须通过 SOAP[客户端初始要求]。
*   它必须将产生微服务的所有数据记录到全新的数据湖中。
*   基本身份验证。
*   让它们尽可能防错。

除了这些要求，我们还必须加上:

*   我们通过单元测试所期望的质量(包括我们雄心勃勃的 90%的覆盖率标准)
*   静态代码分析
*   特性试验
*   还有某种安全检查。

所有这些都必须在本地手动检查，然后通过严格的管道(CI/CD)进行检查。我说严格，但它不是一个封闭的。即使其中一个任务失败，它仍然允许团队部署微服务。但是**千万不要这么做，或者至少知道后果**。

到目前为止，我们根本没有遇到什么问题。对于开发微服务的基本设置来说，这听起来相当不错。我们有 DevOps，我们都在同一个地方，我们有我们的方法，我们有我们的模式，我们有一个奇妙的运行时(Node.js ),这将允许我们一步一步地建立和遵循规则，使这个项目成为一个杰作。嗯，至少我们是这么认为的…

### 哦，孩子，错误已经犯了

![NI49nxrMg1e5yTrvyk9QU6RZrVo5qONSrong](img/f5dc55b788f15e0fd94f7173cdd80536.png)

Accurate picture of the team trying to save Microservices

看看这张相当准确的图片，图中的架构团队试图将微服务从厄运中拯救出来。你可能会问，为什么会这样？当你给多个团队在敏捷环境中开发他们自己的微服务的自由时，这可能会发生。当你对微服务的实际情况、它们的作用、它们的目的、我们如何管理它们，以及最重要的，它们必须有多大，没有给出任何其他解释时，麻烦就来了。

最重要的是，在项目开始时，除了 Subversion，我们没有任何可靠的版本控制软件。与此同时，我们正在等待 Git 在本地安装。

我们在许多不成熟的团队中经常看到的一个问题是，他们并没有试图扑灭这场大火，而是通过复制 Microservices 并开始在它们的基础上进行构建，将火进一步蔓延。这使得他们甚至更大，他们包含无用和重复的内容。

*   微服务*客户端*(A 队、B 队、C 队在上面工作)
    —B 队厌倦了所有的合并，所有的谁负责什么的争斗，再加上 it 的部署。
*   微服务*贷款-客户*(团队 B)
    -团队 B 复制他们在*客户*微服务中工作的确切状态。这在实际有用的端点之上暴露和维护了越来越多无用的端点。

所以我们在这里。我们如何解决所有这些问题？这就是我们所做的。

### 症状

![A2j6VKJ699KC42Umz9HLnuNLJhkGTLKc8z8A](img/9656745bed4b8cb14bf7f2f982b8c9da.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

很明显，我们不能再忍受这些混乱了，所以我们穿上了医生的白大褂，对房间进行了消毒，并对我们所拥有的进行了解剖。我们确定了不可避免的厄运的征兆，优先考虑最重要的征兆，并试图赢得一些小的胜利来控制局面。

> 小胜利不仅让你证明你了解这个主题，也让团队知道可以做些什么来改善他们的日常工作。

#### 微型整体建筑又名宏观服务

等等什么？我们在谈论微服务…

![S1UIv5TkaDGJjNGUjDudUMTPXhuyKITVkeZV](img/cd2716980b9f9ccfbdaa69fa8ba1983f.png)

我打赌你已经读过很多次关于 [**神盾局**的报道了。](https://hackernoon.com/solid-principles-made-easy-67b1246bcdf)原则，关于拥有智能紧凑型产品，即著名的单一责任原则。

我们拥有的不是那样的。这就是为什么在我看到正在发生的事情后，我称它们为**宏服务、**。

想象一下:在一个简单的领域中，姑且称之为*用户*，在同一个*** *咳嗽**** “微服务”中有大约 15 个**帖子**操作在同一个域下，每一个都有不同的用途，并为每一个使用定制的库。另外，我们将所有的单元和性能测试分散在那里。所以这是故意伤害。差不多是这样的:

```
.
├── app --The whole MS is in here
│   ├── controllers --All the controllers of the domain
│   │   ├── dummies
│   │   │  └── ** All the dummies for each controller **
│   │   ├── xsl
│   │   │   └── ** All xsl configuration for each controller **
│   │   ├── Controller1.js
│   │   ├── Controller2.js
│   │   ├── Controller3.js
│   │   ├── Controller4.js
│   │   ├── Controller5.js
│   │   └── **Literally 20 more controllers**
│   ├── functions --All the functions of the MS
│   │   ├── function1.js
│   │   ├── function2.js
│   │   ├── function3.js
│   │   └── function4.js
│   ├── properties --All the properties of the MS
│   │   ├── propertie1.js
│   │   └── propertie2.js
│   ├── routes --All the routes of the MS
│   │   ├── routes_useSecurity.js
│   │   └── routes_withoutSecurity.js
│   ├── services --Extra services that were consumed
│   │   ├── service1.js
│   │   └── service2.js
│   └── xsl
│      └── **A bunch of XSL to do transformations**
├── config --"Global" configurations
│   ├── configSOAP.js
│   ├── configMS.js
│   ├── environments.js
│   ├── logging.js
│   ├── userForBussinessA.js
│   └── userForBussinessB.js
├── package.json
├── README.md
├── test--All the tests
│   ├── UnitTesting
│   │   └── Controllers
│   │       └── ** All the 25 tests in theory **
│   └── PerformanceTest
│       ├── csv_development.txt
│       ├── csv_QA.txt
│       ├── csv_production.txt
│       ├── performance1.jmx
│       └── performance2.jmx
├── server.js --Express Server
├── serverKey.keytab
├── sonarlint.json
├── encryptor
├── ** Around 10 more useless files **
└── Dockerfile
```

![9omI01k5ttMHeMAnmTT3xHEgG9fFvclEkeLo](img/7233b324b040d358299849a859f37d68.png)

This was pretty much me

首先，这必须停止，因为这是无法治理的。团队在战斗，因为为了在开发环境中测试一些东西(他们经常这样做)，他们必须通过 CI/CD 管道。到项目进行的那个时候，它肯定还不完美。

因此，如果**团队 A** 修改了**控制器 1，**他们必须通过管道，很有可能失败(部署也会失败)。他们会一遍又一遍，直到成功。因此，所有的车队都努力比赛，这样他们就不会是最后一个出发的。因为如果部署失败了，就会被指责。这意味着团队做错了什么，打破了它。

> *好玩吧？所有开发人员的健康环境。谁不想在那里…嗯，不是我！*

### 是时候重新开始了

![w6qPgt5B6Wy2mmnYRsbSNmY34FSk5IqKiDP3](img/38688c386c10790915596eb8ca740cd9.png)

我们需要重新开始，把事情做好。控制谁在做什么，并让他们负责。但是我们必须公平:我们不会让一个团队负责包含 15 个操作、测试、部署等等的整个领域。没人想这样。

> 你知道，我们是敏捷的，敏捷的人做敏捷的事情。我们不需要把宝贵的时间浪费在这些谁拥有什么、举起阻挡器、指指点点** **翻白眼** **的争斗上。

#### 步骤 1:确定微服务的规模

我要做一个**大胆断言**，说任何微服务**的最大运算次数都必须**遵循 [**CRUD**](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 标准。忘记考虑微服务应该有多大。

遵循这个规则会让你晚上安心，知道在任何给定的时间，你最多只需要在任何子域中有 4 个操作。仅此而已。

这意味着:

#### 后期-创建

创建程序是作为微服务终结的新数据的插入。

#### 获取—读取

读取程序读取客户端所需的数据。

#### 上传—更新

更新过程修改记录而不覆盖它们。

#### 删除—删除

删除指定的程序。

![3SKmmU2hZXeEdU2Owfg9z-ihdt-NS6Bq-8-k](img/1b580846ed8ca75433b6d91869778286.png)

利用这一规则，我们可以制造出更紧凑、更智能、更标准的微服务。例如，在划分微服务的时候，它会让我们占上风。

假设我在一个银行领域拥有我的*客户*微服务，突然我发现我不仅需要我们的*信贷客户*，还需要我们的*贷款人。*嗯，那就简单了。我只是将我们的域客户分成两个子域:*信贷客户*和*贷款客户*，从那里你可以看到一切是如何开始就位的。

完美！我们现在有了合适的微服务。现在由客户和团队来开发一种方法，知道如何分割域名，并知道它们的子域。

> *要是有办法就好了……* *咳** [**多曼驱动设计**](https://medium.com/withbetterco/what-is-domain-driven-design-bcf81fc4fdc1) 。*

#### 第二步:必须有人拥有它

哇哦，我们解决了一个问题，但是等等——现在我有了一堆更小的部分，每个人都在处理它们。如果它坏了，我不会负责的。

我要说的是:“**如果你编码了它，你就拥有了它”**。有了这种强大的智慧，你可能会说:“我知道，每个人都知道。”但是，不是每个人都知道，这是一个常见的错误。所以聪明点，更进一步，把它变成一个规则。

![gKCZrqk4UPKn1dD2rJ7n-QJnrCHKoBELGhYN](img/4bec01ceaeecac5967e09b6ae1cc1caf.png)

Source From D. Keith Robinson Article [Learn to love Git](https://medium.com/designing-atlassian/learn-to-love-git-part-one-the-basics-90429f456ace)

Git 允许你平静地开发(如果应用得好的话——查看上面 d .基斯·罗宾逊关于[学会爱上 Git](https://medium.com/designing-atlassian/learn-to-love-git-part-one-the-basics-90429f456ace) 的链接),知道你的代码总是最新的。如果其他人想改进它，建议改变，或者如果他们只是需要更新，所有这些都必须经过所有者。为了这个例子，我们将说所有者是开发它的开发团队的架构师。这在敏捷环境中非常有效。

#### 步骤 3: API 端点(命名)和版本控制

命名 API 的方式可以节省所有开发人员大量的时间和精力。命名 API 这不是游戏。它可以拯救生命。

通过正确命名来为您的微服务增值非常重要。如果你不知道给它们取什么名字，问问业务部门，并和你的团队讨论一下。设计驱动的开发可能会有所帮助。

点击这里查看 [RESTful API 设计指南——最佳实践](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)了解更多信息。我可以引用整页的内容。

#### 第四步:让我们重组我们所拥有的

![D1OTtJsvT32jf-aLthKXqbYlnWQwbOkJpYSq](img/21a02e9cf9b08bd1a3c83c0fa2cefa3a.png)

“A child playing with a Jenga block tower” by [Michał Parzuchowski](https://unsplash.com/@mparzuchowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 有正确的概念是一回事，但它在实践中看起来如何？

我要展示的下一个文件树回到了我的想法，我是微服务概念的追随者。我遵循服务之间的**松耦合**和**高内聚**的概念:

```
.
├── config
│   ├── artillery.js
│   ├── config.js
│   ├── develpment.csv
│   ├── processorArtillery.js
│   ├── production.csv
│   └── qa.csv
├── index.js
├── package.json
├── package-lock.json
├── README.md
├── service
│   ├── getLoans --The operation
│   │   ├── getLoans.config.json --Configuration of the resource
│   │   ├── getLoans.contract.js --Contract test
│   │   ├── getLoans.controller.js --Controller
│   │   ├── getLoans.performance.json --Performance test config
│   │   ├── getLoans.scheme.js --Scheme validator
│   │   ├── getLoans.spec.js --Unit Tests
│   │   └── Util --Local functions
│   │       ├── trimmer.js
│   │       └── requestHandler.js
│   ├── postLoans
│   │   ├── postLoans.config.json
│   │   ├── postLoans.contract.js
│   │   ├── postLoans.controller.js
│   │   ├── postLoans.performance.json
│   │   ├── postLoans.scheme.js
│   │   └── postLoans.spec.js
│   └── notFound
│       ├── notFound.js
│       ├── notFound.performance.json
│       └── notFound.spec.js
├── Util --Global functions
│   ├── headerValidator.js
│   ├── bodyValidator.js
│   ├── DBConnector.js
│   └── BrokerConnector.js
├── sonarlint.json
└── sonar-project.properties
```

不仅在 **DDD** 过程中使它们在域/子域概念中可替换或可分割的概念是可能的，而且在目录/文件方式中也是可能的。出于这个例子的目的，我在 Node.js 中使用了一个项目。

我们微服务的每个操作都具有满足其开发、配置、单元测试、性能测试、[合同测试](https://martinfowler.com/bliki/ContractTest.html)、方案验证和控制器要求的所有组件。因此，将运营作为一个整体来对待，可以让我们在微服务增长过多而不得不被分割时拥有控制权。因此，我们几乎必须将整个文件夹移动到其对应的新微服务中。但就是这样——不需要试图找到正确的组件，也不需要试图改变它们以使其再次工作。

**注意**:我们动态地生成了 API 路由，所以每个操作都足够自描述，连同项目的 *package.json* ，来构建我们公开的路由。这给了我们想要的灵活性:不再需要手动编辑路线(这里经常会犯很多错误，所以我们想避免它们)。例如:

*   动词/{ {工件名称} }/{ {域} }/{ {版本} }/{ {子域}}/
    — N **工件名称:**你暴露的是什么类型的工件(微服务， [BFF](https://samnewman.io/patterns/architectural/bff/) ，或者其他)？
    — **域:**自说明，操作所属的域。
    — **版本:**我们资源中可用的当前主要版本。
    — **子域:**我们的微服务将要执行的操作 **CRUD** 。
*   GET/Microservice/Client/v1/loan/—获取所有客户端已经完成的所有贷款。

这听起来真的很神奇，但是我强烈推荐它。您将会看到，在组织微服务时，您遇到的大多数问题将会大大减少。

#### 第五步:文件

![exGoVYJp-DUx9jiFKEqFceDZNDu81jww0Slj](img/b81c013298308a1c63a4aeeb906cbff0.png)

A brilliant comic by Dilbert, Source [Here](http://dilbert.com/strip/2007-11-26)

呃，我不得不说，我真的打了个寒战。我可以想象你们所有的敏捷实践者，尖叫着喊出你们的 scrum 灵魂。但别担心，我会保护你的。

我将提出两个概念:首先也是最重要的，因为我们正在公开 API，所以让我们都来试试这个 **API 优先开发。**

> API 优先开发是一种策略，其中首要任务是开发一个应用程序界面，让目标开发者感兴趣，然后在此基础上构建产品，无论是网站、移动应用还是 SaaS 软件。通过以开发人员为中心构建 API，您和您的开发人员可以节省大量工作，同时为其他人的构建奠定基础。([rest case 的一种 API 优先的开发方法](https://blog.restcase.com/an-api-first-development-approach/))。

你可能会问，我们该如何建造它？下面是我们要用到的第二个概念:[](https://swagger.io/)****，**构建 API 的众多工具之一。这个工具将打开大门，以一种干净和有组织的方式设计和建模 API。**

**没有比这更好的了。我们不仅解决了通常在敏捷文档中遇到的问题，还改进了团队开发微服务的方式。它给了他们正确的工具来相互交互，并消除了另一个团队可能会说类似这样的话的可能性:“我的团队需要这个作为输出，具有这个 API 的这些特性，但是我们没有得到这样的东西。”然后，您可以有把握地说:“这是我们 API 的文档，由架构师设计和批准，满足了业务需求”。麦克风下降。因此，任何进一步的迭代都将围绕记录良好的 API 进行。**

#### **第六步:培训**

**正如我前面说过的，这取决于我们如何充分利用我们的开发人员和团队。慢慢来，找准弱点，改进！**

**![I1erhqLk-0Kgh8G-4z7xkxTfKHmfAkVFYYQR](img/c63bdec34ae34754da9f8f769fc76b17.png)**

**我知道每个人在训练他们的团队时都有不同的偏好，但当涉及到敏捷性和优化您团队的时间时，我强烈推荐 [**编码道场**](http://codingdojo.org/) 。这种培训技术允许我们培训我们所有的团队，这样他们在每个主题(Node.js、微服务、单元测试、性能测试等等)上都有相同的专业水平。).它还改善了信息传递给团队的方式——我们都玩过电话游戏，我们知道大多数情况下它是如何结束的。没有人有时间阅读多年的文件。我们甚至可以将团队的反馈应用到日常生活中。所以大家都赢了！**

### **经验教训和总结**

**![1Dbzf84WYhYQC0StYhfnrNvrlwXLADAq6X2R](img/871389ab3dc50010158c19c454ec3d61.png)

Photo by [Hello I'm Nik](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)** 

**对我来说，这是关于了解你的生态系统的所有部分是如何相互作用的。而是学会如何应对它们，因为我可以向你保证，总有一天，你会想到解决问题的办法。但是你可能会为了适应需求而做一些完全不同的事情。这就是微服务的妙处。它们允许你灵活，不管它看起来有多可怕，如果你遵循**可替换部分、松散耦合、**和**高内聚**的概念，相信我，一切都会好的。**

**微服务实施是一个勇敢者的旅程，他们愿意每天都保持进步。这是给那些意识到哪些事情他们可以做得更好，看到大局并把事情做好的人。**

**我之前说过，我开始的时候不是专家，错误是会犯的。但这并没有阻止我做正确的事情。对于所有那些正在为自己的宏观服务、微型整体服务、微观服务而奋斗的人，我可以告诉你:停下来，深呼吸，做自己的诊断并改进。把事情做对还不晚。**