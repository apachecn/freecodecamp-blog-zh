# 什么是 GitOps？原则、最佳实践和 Kubernetes 工作流程

> 原文：<https://www.freecodecamp.org/news/gitops-principles-kubernetes-workflow/>

在这次演讲中，CTO Cornelia Davis 将教你什么是 GitOps，它的四个主要原则是什么。

## 什么是 GitOps？

你需要知道的第一件事是，GitOps 是一套部署和管理云原生基础设施和应用的现代最佳实践。

如果您以前从未使用过集群管理或应用程序交付，这可能是一件很难理解的事情。但是谢天谢地，科妮莉亚在这个 30 分钟的演讲中做了很好的解释。

给它一个表，然后你可以找到下面的摘要。

[https://www.youtube.com/embed/wdoLEA7U8_M?feature=oembed](https://www.youtube.com/embed/wdoLEA7U8_M?feature=oembed)

既然我们已经介绍了什么是 GitOps 的基础知识，下面就来回顾一下它的 4 个主要原则。希望您可以使用它们开始用 GitOps 工作流管理您自己的集群。

## GitOps 的原则

### 声明性地描述

“声明性”的意思是，我们将配置作为一组事实直接写在 Git 的源代码中。这是现在我们唯一的“真理之源”。

例如，我可以声明我的环境，如“测试环境”，或“试运行环境”或“生产环境”等，以及驻留在该环境中的应用程序版本。

### 确保状态已版本化

随着我们的声明现在存储在一个版本控制系统中，并作为我们的“真理的来源”，我们现在有了一个单一的地方，一切都是从那里派生出来的。我们可以很容易地启动以前版本的应用程序，或者在需要时执行回滚。

### 自动化变更批准

我们还需要允许对我们声明的状态的任何更改自动应用到我们的系统中。这一点值得一提，因为我们现在在隔离的环境中工作，我们不再需要集群凭证来更改我们的系统。

### 警惕差异

所以现在我们已经声明了系统的状态并对其进行了版本控制，我们可以使用代理来检查一切是否正常工作。这被认为是一个“反馈和控制回路”。如果有些东西“看起来”不一样，不正确，我们会得到警告。

想要更深入地了解这四条原则，你可以看看上面科妮莉亚·戴维斯的演讲。

这篇文章是 Ania Kubow 为支持 Cornelia Davis 的会议发言而写的。

[Code with Ania KubówHello everyone. This channel is run by Ania Kubow. In this channel, I will be teaching you JavaScript,React, HTML, CSS, React-native, Node.js and so much more! A little bit about me:My background is in the financial markets, where I worked as a derivates broker our of University. After starting m…![favicon_144](img/3c00d8e1cae0b4d3ddd1219afa7413dc.png)YouTube![AAUvwnjSRt8sIbeM7P--pHoUDh67sDhaNTCMF_XiNOCvUw=s900-c-k-c0x00ffffff-no-rj](img/419b78372ca0ca37fd9d01d8ade672c6.png)](https://www.youtube.com/channel/UC5DNytAJ6_FISueUfzZCVsw)