# 关于分布式系统的一个注记

> 原文：<https://www.freecodecamp.org/news/a-note-on-distributed-systems-3c796f1eb0a0/>

这篇文章摘录了 Jim Waldo 和其他人在 1994 年发表的题为“[分布式系统笔记](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628)”的论文中的内容。

本文介绍了在面向对象编程环境中本地计算和分布式计算的区别。它解释了为什么同样对待它们是不正确的，并导致应用程序不健壮或不可靠。

#### 介绍

该论文开篇指出，当前分布式系统中的工作是围绕对象建模的——更具体地说，是对象的统一视图。对象由它们支持的接口和它们支持的操作定义。

自然，这可以扩展到意味着在同一地址空间中的对象，或者在同一机器上的不同地址空间中的对象，或者在不同机器上的对象，都以相似的方式运行。它们的位置是实现细节。

让我们来定义本文中最常见的术语:

#### 本地计算

它处理的程序仅限于单个地址空间。

#### 分布式计算

它处理可以在同一台机器或不同机器上调用不同地址空间中的对象的程序。

#### 统一对象的愿景

这一愿景隐含的意思是，系统将是“一路向下的对象”这意味着所有当前的调用，或者对系统服务的调用，最终都将被转换成对驻留在其他机器上的对象的调用。无论对象的位置如何，都有一个单一的对象使用和通信范例。

这指的是假设所有对象都是根据它们的接口来定义的。它们的实现还包括对象的位置，并且独立于它们的接口，对程序员来说是隐藏的。

就程序员而言，他们为每个对象编写相同类型的调用，无论是本地对象还是远程对象。系统通过找出编写应用程序的程序员看不到的底层机制来负责发送消息。

分布式计算中的困难问题不是如何让事情上线和下线的问题。

这篇文章接着定义了构建分布式系统的最大挑战:

1.  潜伏
2.  存储访问
3.  部分故障和并发

在处理上述所有问题的同时确保合理的性能并不会让分布式系统工程师的生活变得更轻松。缺乏任何中央资源或状态管理器增加了各种挑战。让我们逐一观察这些。

#### 潜伏

这是本地和分布式对象调用之间的根本区别。

这篇论文声称远程电话比本地电话慢 4 到 5 倍。如果一个系统的设计没有认识到这一基本的区别，它必然会遭受严重的性能问题。尤其是如果它依赖于远程通信。

您需要对正在设计的应用程序有一个透彻的了解，这样您才能决定哪些对象应该放在一起，哪些可以放在远程。

如果目标是统一延迟差异，那么我们有两个选择:

*   依靠硬件随着时间的推移变得更快，以消除效率差异
*   开发工具，使我们能够可视化不同对象之间的通信模式，并根据需要移动它们。由于位置是一个实现细节，这应该不难实现

#### 记忆

与分布式系统设计密切相关的另一个差异是本地和远程对象之间的内存访问模式。本地地址空间中的指针在远程地址空间中无效。

我们只有两个选择:

*   开发人员必须意识到访问模式之间的差异
*   为了统一本地访问和远程访问之间的差异，我们需要让系统处理内存访问的所有方面。

有几种方法可以做到这一点:

*   分布式共享存储器
*   使用 [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) (面向对象编程)范例，组成一个完全由对象组成的系统——一个只处理对象引用的系统。
    地址空间之间的数据传输可以通过底层的数据编组和解组来处理。然而，这种方法使得地址空间相关指针的使用变得过时。

危险在于宣扬“远程访问和本地访问完全一样”的神话我们不应该强化这个神话。一种不统一所有内存访问的底层机制，同时仍然宣扬这一神话，这既误导人又容易出错。

让程序员知道访问本地和远程对象之间的各种不同是很重要的。我们不想让他们因为不知道幕后发生了什么而受到伤害。

#### 部分故障和并发

部分失败是分布式计算的核心现实。

该论文认为本地和分布式系统都容易发生故障。但是在分布式系统的情况下，更难发现哪里出了问题。

对于本地系统，要么一切都关闭，要么有某个中央机构可以检测出哪里出了问题(例如，操作系统)。

然而，在分布式系统的情况下，没有全局状态或资源管理器可用来跟踪系统内和系统间发生的一切。因此，没有办法通知其他可能正常工作的组件哪些组件出现了故障。分布式系统中的组件独立发生故障。

分布式计算中的一个中心问题是确保整个系统的状态在这样的故障之后是一致的。这是在本地计算中根本不会出现的问题。

对于一个经受住部分失败的系统，重要的是它处理不确定性，并且对象以一致的方式对它做出反应。如果可能的话，接口必须能够陈述失败的原因。然后在原因无法确定的情况下，允许重建一个“合理的状态”。

问题不是“您能让远程方法调用看起来像本地方法调用吗”，而是“让远程方法调用等同于本地方法调用的代价是什么？”

我想到了两种方法:

1.  将所有接口和对象视为本地。这种方法的问题是，它没有考虑到与分布式系统相关的故障模型。因此，它本质上是不确定的。
2.  将所有接口和对象视为远程。这种方法的缺点是它使本地计算过于复杂。它为那些从不被远程访问的对象增加了大量的工作。

更好的方法是接受本地计算和分布式计算之间存在不可调和的差异，并在分布式应用程序的设计和实现的所有阶段意识到这些差异。

附:如果你已经做到了这一步，并且想在我发表这些帖子的时候收到邮件，请在这里注册。