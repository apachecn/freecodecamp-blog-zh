# 我拓展之旅的下一步:码头工人、大挑战和小胜利

> 原文：<https://www.freecodecamp.org/news/the-next-steps-on-my-outreachy-journey-docker-big-challenges-and-small-victories-2c3a2dd2277a/>

托尼·肖特斯维

# 我拓展之旅的下一步:码头工人、大挑战和小胜利

![O3tJSi7cOIVIdnGKvoQW5XnkBgh2-tJz6qDG](img/2e0dd28921e0351116a6644837f1a406.png)

这几周对我来说很有趣。因此，我稍微推迟了这篇文章的发表。

我最近被选中去外展实习。Outreachy 是一个为通常在技术领域代表性不足的人组织为期三个月的带薪实习和免费开源软件项目的计划。

作为一名外展实习生，我每隔几周就要写一篇我的经历。这对我来说是独一无二的，因为我习惯于编辑而不是写作。

在我的第一篇[文章](https://medium.freecodecamp.org/how-i-beat-the-odds-and-became-an-outreachy-intern-9a92f47cb44e)中，我刚刚被[外展](https://www.outreachy.org/)实习生接受，在[图书馆健康](http://librehealth.io/)工作。下一篇[文章](https://medium.freecodecamp.org/my-outreachy-internship-begins-today-heres-what-i-ve-done-and-learned-so-far-88fef9c18619)讨论了我被录取后开始实际实习的准备工作。今天，我将分享我从实习开始到现在的情况。

### 想要了解更多

我最近提交了我一直在做的文档的最新版本，我正在等待反馈。同时，我意识到我仍然没有足够的关于放射学模块和运行它的 LibreHealth 工具包的信息。

我的实习伙伴@阿黛尔把我带到了一个很棒的 T2 博客网站。Ivange Larry Ndumbe 是 GSOC 2017 年的实习生，从事图书馆健康放射学模块的工作。

他的几篇文章引导我找到了帮助建立工具包的正确方向。原来我需要下载 Docker。但只有一个问题:我不知道 Docker 是什么，也不知道它是做什么的。

### 有些曲折

但有时，时机就是一切。就在那个时候，我很幸运地为 freeCodeCamp 的媒体出版物编辑了[让我引导你与 Docker](https://medium.freecodecamp.org/let-me-guide-you-through-your-first-date-with-docker-f03f35567d95) 的第一次约会。这给了我一个很好的介绍 Docker 下载。除了 Docker Windows 下载只针对 Windows 10。

你还记得 Windows 95、Vista 和 XP 吗？我的电脑有。在 Windows 8 之前的每次新升级中，他们都是最好的朋友。我对这些年来我的大部分软件升级非常满意。但是我发现我的一些收藏在升级后已经不可用了。而且就我个人而言，我对 Windows 10 的印象还不足以让我放弃我最喜欢的套件。这是我在过去一届会议中焦虑的根源。

更多的研究让我找到了 Docker [工具箱](https://docs.docker.com/toolbox/overview/)。但是首先，Docker 文档说:

> 确保您的 Windows 系统支持硬件虚拟化技术，并且虚拟化已启用。

这太可怕了。

![pa-0s9ghXHuagCoD9VBrxNUSX2wLk2X9SlEv](img/181b57e92868ba30d89571af56b2bee8.png)

回来做更多的研究。我(在某种程度上)找到了一种策略来实现这里的虚拟化。

我必须在电脑启动时进入 BIOS，在它完全加载前修改代码。

它重启了很多次。我不得不玩建议的`F-keys`，同时在系统完全进入开启阶段之前足够快地抓住它。然后我用不同的`F-key`重复重启，直到我在正确的时间找到正确的那个。在经历了所有这些`F-keys`之后，我试了试`Delete`键。成功！

当然，我的开关藏在高级部分，我必须搜索才能找到正确的命令。但是下一次重启启用了我的虚拟化！

### 一次一个胜利

现在是时候下载 Docker 工具箱了。试了几次之后，我已经准备好运行`docker-compose`命令了。但是我忘记了以管理员身份运行该命令，我的访问被拒绝。

所以我又运行了一次，这次是以管理员的身份。我收到了一个`File Not Found`错误。

回到研究上来。freeCodeCamp[pair programming women](https://gitter.im/FreeCodeCamp/PairProgrammingWomen)聊天室通过私人信息和发送有用的链接与我联系非常好。我的技术导师在提供更多有用的链接和信息方面表现出色。

我终于设法让 Docker Compose 工作了！但现在我无法访问我的本地主机。我的导师会继续和我一起工作。这是一个过程。

### 走向

我将继续遵循我从 freeCodeCamp 聊天室得到的建议，使用阅读-搜索-提问的方法。我还将详细介绍所有这些链接，并继续创建一个可管理的放射设施。

当我们完成时，这将是一个很好的用户指南。感谢你们陪着我继续我的实习之旅。下次见！