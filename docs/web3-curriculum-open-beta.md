# freeCodeCamp 的 Web3 课程公开测试版——以及如何运行它

> 原文：<https://www.freecodecamp.org/news/web3-curriculum-open-beta/>

在过去的 11 个月里，我们在 Web3 课程方面取得了长足的进步。今天，我很高兴地宣布，这个课程的一部分现在处于公开测试阶段。你今天可以试试。

在我们进入细节之前，我想感谢 KaijuKingz 社区，他们向 freeCodeCamp 捐款，使得这些课程的开发成为可能。你可以[在这里](https://www.freecodecamp.org/news/carbon-neutral-web3-curriculum-plans/)阅读更多关于他们给 freeCodeCamp 社区的礼物。

## 如何学习这些 Web3 课程

作为本课程的先决条件，我们建议首先学习全栈 web 开发。您可以通过完成前 7 个 [freeCodeCamp 认证](https://www.freecodecamp.org/learn/)并构建他们的项目来做到这一点:

1.  [响应式网页设计](https://www.freecodecamp.org/learn/2022/responsive-web-design/)
2.  [JavaScript 算法和数据结构](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)
3.  [前端开发库](https://www.freecodecamp.org/learn/front-end-development-libraries/)
4.  [数据可视化](https://www.freecodecamp.org/learn/data-visualization/)
5.  [关系数据库](https://www.freecodecamp.org/learn/relational-database/)
6.  [后端开发和 API](https://www.freecodecamp.org/learn/back-end-development-and-apis/)
7.  [质量保证](https://www.freecodecamp.org/learn/quality-assurance/)

我们还建议您了解一些基本的区块链发展概念。freeCodeCamp 有一个涵盖这一点的 32 小时深入课程，由开发人员兼讲师 Patrick Collins 教授。

我们也推荐学习一些 Rust，你可以使用 [freeCodeCamp 的 Rust 课程](https://www.freecodecamp.org/news/rust-in-replit/)进行交互学习。

同样，这些先决条件只是我们的建议。如果你认为合适的话，可以随意地开始并返回这些资源。

现在，我们已经设计了五个集成的 Web3 项目供您完成:

1.  建立一个视频游戏市场区块链
2.  建立筹资智能合同
3.  建立一个对等网络
4.  为您的 dApp 构建一个 Web3 客户端包
5.  在 Rust 中建立智能合同

这些项目中的每一个都有自己的一套说明，其中包含需要您完成的任务，以及确保您正确实施项目的测试。完成所有的任务，并获得所有的测试通过，以完成每个项目。

## 这 5 个项目仅仅是开始

我们还在开发 10 个交互式 Web3 实践项目。

这些将带你了解构建我们今天发布的这 5 个集成项目所需的所有 Web3 概念。

为什么我们首先发布困难的部分(5 个集成项目)？对于那些不介意观看 [Patrick 的课程](https://www.freecodecamp.org/news/learn-blockchain-solidity-full-stack-javascript-development/)，阅读官方文档，以及参考其他免费的 Web3 教程的铁杆 Web3 爱好者来说。

很快，任何人学习这些工具和概念都将变得更加容易。但是我们想先为铁杆粉丝们做点什么。

## Web3 课程处于公开测试阶段。我们欢迎您的反馈和错误报告。

请注意，这些都处于公开测试阶段，这意味着我们将根据您的反馈继续完善它们。

你可以通过加入我们新的 [Web3 课程不和谐服务器](https://discord.gg/9KngwWzvd4)，介绍你自己，并帮助其他那些试图建立这 5 个综合项目的人。

你也可以注册更新，这将使建立这 5 个综合项目变得容易得多，所以在某种程度上，你实际上在做最难的，最模糊的部分[注册更新下面的](#sign-up)当新的课程发布时。

## 它将如何工作？

课程将使用 VS 代码和 [freeCodeCamp 课程扩展在 docker 容器中运行。](https://marketplace.visualstudio.com/items?itemName=freeCodeCamp.freecodecamp-courses)

### 这是一个样本

[https://www.youtube.com/embed/EAidlZ6FZwE?feature=oembed](https://www.youtube.com/embed/EAidlZ6FZwE?feature=oembed)

## 如何开办课程

按照以下步骤运行课程

### 开发人员环境先决条件

在开始之前，请确保您的计算机上安装了这些软件:

1.  [Docker 引擎](https://docs.docker.com/engine/)
2.  [VS 代码](https://code.visualstudio.com/download)和[开发容器](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)扩展
3.  饭桶

### 如何在 Docker 中运行课程

按照以下说明克隆存储库并运行课程:

1.  打开一个终端，用以下命令克隆[web 3-课程表](https://github.com/freeCodeCamp/web3-curriculum) repo:

```
git clone https://github.com/freeCodeCamp/web3-curriculum.git 
```

2.  导航到`web3-curriculum`目录，并在 VSCode 工作区中打开它:

```
code . 
```

3.  按`Ctrl / Cmd + Shift + P`打开命令面板，运行`Dev Containers: Rebuild Container and Reopen in Container`。VS 代码将构建运行项目的容器，第一次需要几分钟。
4.  完成后，再次按下`Ctrl / Cmd + Shift + P`并运行`freeCodeCamp: Run Course`开始课程。这也需要一点时间。
5.  完成后，简单的浏览器将会打开。如果是空白的白色页面，请使用“刷新”按钮进行更新，并查看课程主页。
6.  单击一个可用的项目来启动一个项目。
7.  按照说明完成项目。
8.  玩得开心！

如果您想切换项目，请单击顶部的 freeCodeCamp 徽标返回主页。

## 注册获取更新

填写[这个谷歌表单](https://docs.google.com/forms/d/e/1FAIpQLSdaKRd34e36eGVA7ne1g1x3kLPjTbLF0YoNqLWH6L7P2AmpxA/viewform?usp=sf_link)以便在新课程发布时接收更新。

## 其他课程

我们也在围绕索拉纳和 NEAR 协议创建课程。

查看 [Solana 公告文章。](https://www.freecodecamp.org/news/solana-curriculum/)
查看[附近的公告文章。](https://www.freecodecamp.org/news/near-curriculum/)

或者，查看我们展示所有课程的[web3.freecodecamp.org](https://web3.freecodecamp.org/)域名。