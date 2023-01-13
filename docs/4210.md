# 你的下一个游戏将使用什么 2D 游戏引擎

> 原文：<https://www.freecodecamp.org/news/what-2d-game-engine-to-use-for-your-next-game/>

几周前，我[发表了我的经历](https://www.freecodecamp.org/news/how-i-made-a-2d-prototype-in-different-game-engines/)试图在一堆不同的 2D 游戏引擎/框架中制作一个原型，以了解是什么让它们运转起来。

如果你正在为你的下一款 2D 游戏选购引擎，这篇文章将提供一些可以帮助你辨别的东西。

请注意，我不是试图涵盖每一个 2D 游戏引擎；我也没有将一个引擎或框架放在另一个之上。这些建议来自我使用不同引擎和框架进行原型开发的个人经验。

如果你更喜欢看而不是读，我制作了这篇文章的视频版本(26 分钟观看):

[https://www.youtube.com/embed/gtKEkuhsWOs?feature=oembed](https://www.youtube.com/embed/gtKEkuhsWOs?feature=oembed)

## 反应

乍一看，你可能会想，“ [React](https://reactjs.org/) 是一个制作交互式网站的前端框架。又不是游戏引擎！”你基本上是对的。

React 不提供对游戏开发基础的本地支持，例如 2D 物理学，但是它*非常好地处理了状态。如果你已经是一名 JavaScript 开发人员，并且愿意用类似 [boardgame.io](https://boardgame.io/) 的东西来制作一个简单的 2D 游戏，你可能会很快得到一个原型并运行起来。*

对于所有其他类型的 2D 游戏，你会想看看别处。

## 一致

Unity 已经在 2D 和 3D 游戏开发领域无处不在。我把它定位为一个优秀的 3D 游戏引擎，一个可以使用的 2D 引擎。

Unity 编辑器相当复杂，有许多嵌套菜单，需要一些时间才能理解(查看本文了解其 2D 特性)。如果你还没有 Unity 用于编写脚本的 C#背景知识，你会想在学习 Unity 之前先温习一下，因为这样会让你的整个学习过程变得轻松。

在 2D 游戏开发方面，Unity 也做了很多“艰难的事情”,与其他游戏引擎相比，这并不让人觉得是本地的。例如，在 Unity 中创建一个 2D 游戏世界，感觉就像你把一架 2D 飞机硬塞进一个巨大的 3D 空间，动画和像素完美之类的东西比其他 2D 专用引擎更笨拙。

如果你愿意与编辑器和潜在的 3D 特质搏斗，你可以用 Unity 制作任何类型的 2D 游戏。它有广泛的社区支持，你会发现使用 C#是一种享受。此外，Unity 的资产商店有各种各样的艺术品和模板供你下载和购买，但买家要注意:你可能会花很多时间重写别人的代码来适应你的项目，就像你刚刚从头开始一样。

一般来说，Unity 是免费使用的，但是如果你想使用*它所提供的一切*，定价就变得更加复杂了(更多细节见[本页](https://store.unity.com/compare-plans))。

## 戈多

Godot 是一个免费开源的 2D 和 3D 游戏引擎，它支持 GDScript、C#，甚至 C++和 Python，如果你愿意做很多繁重的工作来使它们工作的话。它支持节点风格的工作流，并且是超级轻量级的。

如果你 a)愿意投资学习 GDScript，或者 b)已经非常擅长 C#、C++或 Python，你可能会在 Godot 中做得很好，特别是如果你喜欢使用开源软件的话。如果没有，你可能会很容易感到沮丧，因为对 C#或其他语言的支持远不如对 GDScript 的支持。尽管如此，Godot 仍然是一个令人愉快的工作引擎，尽管它可能没有 unity 这样的血统和社区支持，但如果你是一个自我启动者，你可能会感觉很好。

## 构造 3

如果你只是想制作 2D 游戏，而不在乎编程语言或订阅费用，你会发现 [Construct 3](https://www.construct.net/en) 有你需要的一切来快速启动和运行演示。您的所有工作都将在浏览器中完成，使用拖放工具(如果您需要，还可以使用定制的 JavaScript 支持)。

但是，不要指望免费使用 Construct 3 会有有意义的生产体验。你可以尝试一个简单的演示，但是用 Construct 3 进行有影响力的游戏开发被锁在付费墙后面，并且需要订阅。

## 游戏制作者工作室 2

Game Maker Studio 2 有一个用户友好的编辑器，支持一种专有语言，恰当地说，叫做 Game Maker Language (GML)，以及可视化脚本。它也有很多教程，强大的社区支持，和一个资产商店(上面有和 unity 一样的警告)。

Game Maker Studio 2 的一般工作流程以及制作精灵动画、设置游戏世界等操作都非常简单直观。如果你来自另一种更广泛使用的编程语言，GML 可能不适合你，我不会推荐你把它作为学习如何编码的入门读物。它采用了一些编程的基本概念，但没有重要的细节，如编码最佳实践或如何编写干净的代码。

此外，您可以免费试用 Game Maker Studio 2 天，但在此之后需要付费才能继续使用。

## 相位器 3

如果你想编写所有的代码，并在编写过程中学习更多关于 JavaScript 生态系统的知识，看看《T2》的《相位器 3》(或者等待《相位器 4》，它是《T4》的《T5》)。

Phaser 是一个轻量级的强大的 JavaScript 框架，用于制作 2D 游戏。Phaser 2 有很好的文档记录，并且有很好的社区支持，而 Phaser 3 则完全相反。有很好的官方文档和一堆例子(必须说，它们周围没有太多的上下文)，以及数量少得可怜的教程。

期望自己构建所有东西，但是如果你正在寻找 ES6 或 TypeScript 支持，或者如果你真的想提高你作为 JavaScript 开发人员的技能，你将能够用 Phaser 3 走很长的路。

为了公平起见，我应该提到另外两个 2D 游戏引擎，自从我开始写这个主题以来，它们一直向我推荐:[勒夫·2D](https://love2d.org/)，它使用 Lua，和[一夫一妻制](http://www.monogame.net/)，它支持 C#。我没有用过这两款游戏(或者其他的，比如 [PyGame](https://www.pygame.org/) )，也不能说它们有用，但是它们可能值得一试。

让我知道你最终使用哪一个 2D 游戏引擎，为什么！

如果你喜欢这篇文章，请考虑[查看我的游戏和书籍](https://www.nightpathpub.com/)，[订阅我的 YouTube 频道](https://www.youtube.com/msfarzan?sub_confirmation=1)，或者[加入 *Entromancy* Discord](https://discord.gg/RF6k3nB) 。

****M. S. Farzan，**** 博士，曾为电子艺界、完美世界娱乐、Modus Games、MMORPG.com 等知名视频游戏公司和编辑网站撰稿和工作，并担任过 ****地下城&龙无冬**** 和 ****质量效应:仙女座**** 等游戏的社区经理。他是****[Entromancy:一款赛博朋克奇幻 RPG](https://www.entromancy.com/rpg)**** 的创意总监兼首席游戏设计师，也是 *[黑夜之路三部曲](http://nightpathpub.com/books)* 的作者。在 Twitter 上找到 m . s . Farzan[@ sominator](http://www.twitter.com/sominator)。