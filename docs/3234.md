# 我从 4000 名 Rust 开发者身上学到的 8 件事

> 原文：<https://www.freecodecamp.org/news/8-things-i-learned-from-4000-rust-developers/>

你知道大多数 Rust 程序员都在做 web 应用吗？？Rust 是具有挑战性的，但也是有益的和巨大的乐趣！以身作则学[锈](https://rust-by-example-ext.com/)，还是？打开[这个 GitHub repo](https://github.com/second-state/learn-rust-with-github-actions) 开始使用 VSCode。

铁锈是最热的？当今的编程语言。它是 StackOverflow 在过去 4 年里最受欢迎的编程语言。然而，它仍然享有编程语言的声誉。

据估计，全球有 60 万名 Rust 开发者，这是一个相当大的数字。但与数以千万计的 JavaScript、Java 和 Python 开发者相比，这仍然是小巫见大巫。

那些 Rust 开发者是谁？他们用铁锈做什么？他们为什么这么爱铁锈？最重要的是，你如何加入他们的行列，并亲眼看到为什么铁锈是如此受人喜爱？不要被落下。

为了回答这些问题，Rust 社区从 2016 年开始在 rust-lang.org 进行年度开发者调查。该网站最近发布了基于近 4000 名 Rust 开发者反馈的 2019 年[调查结果](https://blog.rust-lang.org/2020/04/17/Rust-survey-2019.html)。以下是我从调查中学到的 8 件事。

## ？？‍?Rust 是为专业程序员准备的

Rust 编程语言并没有被设计成“[容易上手](https://www.secondstate.io/articles/a-rusty-hello-world/)”。相反，它被设计成既强大又安全。它的目标是成为专业程序员的开发效率语言。这很有挑战性，很有趣，也很有收获。调查显示。

极少数受访者称自己是铁锈专家。大多数人对他们的 Rust 专业知识的评价是 7/10 或更低，尽管事实上超过 68%的人每周编写 Rust 代码。这显然是一门需要时间来掌握和超越的语言。

> 大约 37%的 Rust 用户在使用 Rust 不到一个月的时间里就觉得很有效率——这与去年的百分比(40%)没有太大的区别。超过 70%的人在第一年感到富有成效。不幸的是，像去年一样，用户中仍然存在一个问题——21%的用户表示他们还没有感到有效率。

同时，当被问及为什么不在一些项目中使用 Rust 时，学习曲线被认为是第二个最常见的原因。当然，首要原因是公司决定是否在项目中使用特定的编程语言。

## ？文档对于采用是至关重要的

开发者如何克服 Rust 的学习曲线并爱上它？不出所料，大多数开发人员将“更好的文档”作为采用的驱动力。

但对“专业程序员”来说，最受欢迎的 Rust 文档是帮助开发人员提高 Rust 技能和生产力的中级内容。

虽然调查偏向于已经了解 Rust 基础知识的开发人员，但这一人群似乎渴望知识和自我完善。

## ？开发者不想要大量的文本

传统的软件文档通常由整本书和网站组成。新一代开发人员想要更多更好的文档。作为一种“新”语言，Rust 已经引领了编程语言文档的革新。

例如，Rust 编译器是一个自文档工具。Rust 最独特和最受欢迎的特性之一是它积极的编译器，可以在程序运行前帮助你确保正确性和安全性。因此，Rust 开发人员可以编写高性能且安全的程序。

当您在 Rust 中遇到编译错误时，编译器会立即给出错误的解释，并根据程序的上下文给出如何修复错误的建议。

GitHub 中的这个入门项目让你开始使用 Rust 编译器和 Cargo 系统，而无需安装任何软件工具链。您可以将 VSCode online IDE 直接用于此项目。

Rust 文档网站如 [docs.rs](http://docs.rs) 和 [Rust by Example](https://doc.rust-lang.org/rust-by-example/) (及其[扩展版](https://rust-by-example-ext.com/))使用 [Rust Playground](https://play.rust-lang.org/) 直接从浏览器运行 Rust 示例代码。那些互动的书比简单的文字好多了。

然而，调查发现，开发商想要更多。例如，开发者渴望更多的视频内容。我们可以期待社区很快会有更多的编码视频和直播。

## ？️:大多数人用 Rust 做网络应用，真的！

作为一种旨在取代 C 和 C++的系统级语言，大多数人认为 Rust 将用于基础设施编程，如操作系统、本机库和运行时平台。

然而，调查清楚地表明，目前大多数 Rust 开发者都在开发 web 应用后端。难怪像 [hyper](https://docs.rs/hyper/0.13.5/hyper/) 、 [actix-web](https://github.com/actix/actix-web) 和 [Rocket](https://rocket.rs/) 这样的机箱最受 Rust 开发者的欢迎。

可以肯定的是，大多数软件开发人员都在开发 web 应用程序。毫不奇怪，随着 Rust 获得主流采用，Rust 项目将反映更大的软件行业。

然而，这确实为将 Rust 集成到流行的 web 应用程序运行时的项目和工具提供了机会。例如， [Rust + JavaScript 混合应用程序](https://www.secondstate.io/articles/getting-started-with-rust-function/)方法正在获得动力。

## ？区块链是一个生锈的温床

说到基础设施软件，Rust 作为区块链系统的编程语言确实大放异彩。

调查显示，在所有软件相关行业中，区块链在所有软件开发商中仅排名第 35 位，而在软件开发商中排名第 11 位。这在很大程度上是由于区块链的大型项目如[波尔卡多特/基板](https://www.parity.io/)、[绿洲](https://www.oasislabs.com/)、[索拉纳](https://solana.com/)和[第二状态](https://www.secondstate.io/)等积极采用 Rust。

在许多方面，区块链非常适合生锈。区块链代表了机构群体以分散的方式重建互联网基础设施的努力。他们需要高性能且非常安全的软件。如果你对区块链工程师的职业感兴趣，生锈是一项必备技能。

## 拉斯特网络组装

调查显示，WebAssembly 是 Rust 程序的流行运行时环境。Rust 和 WebAssembly 都是 Mozilla 发明的。

Rust 侧重于性能和内存安全，而 WebAssembly 侧重于性能和运行时安全。作为一个运行时容器，WebAssembly 也让 Rust 程序跨平台，更易管理。这两种技术之间确实有很多协同作用。

WebAssembly 最初是作为运行浏览器内应用程序的客户端虚拟机而发明的。但是就像之前的 Java 和 JavaScript 一样，WebAssembly 现在正在从客户端[向服务器端](https://www.secondstate.io/articles/why-webassembly-server/)迁移。

Rust-in-WebAssembly 预示着后端 web 应用程序加速采用 Rust 的趋势。您可以从这个 GitHub 库中的[starter 项目开始 Rust 和 WebAssembly 应用程序开发。](https://github.com/second-state/ssvm-nodejs-starter)

## ？异步编程正在兴起

近年来，两种新的编程语言在开发人员中获得了巨大的吸引力。一个是铁锈，一个是围棋。他们的成功很大一部分是因为他们对并发编程模型的出色支持。

事实上，Rust 的早期口号是“无畏并发”。它保证了开发人员在编写针对当今多核 CPU 架构优化的异步多线程程序时的生产力。正如 Node.js 所展示的，简单的异步编程对于语言或框架在服务器端的成功至关重要。

调查显示，10 个最重要的 Rust crates(即第三方库)中的 4 个、 [tokio](https://tokio.rs/) 、 [async](https://docs.rs/crate/async-std/1.4.0) 、 [futures](https://docs.rs/futures/0.3.4/futures/) 和 [hyper](https://hyper.rs/) ，都是异步多线程应用的框架。

## ？r、Python 和 JavaScript

随着 Rust 的普及，开发人员越来越需要将 Rust 程序与用其他语言编写的程序集成在一起。过去，C 和 C++是与 Rust“对话”的最常见语言，因为它们都用于基础设施软件项目。

随着 Rust 成长为应用软件项目，现在需要更多的语言级接口和桥梁。一个很好的例子是在 Node.js 应用程序中支持 [Rust 函数的](https://www.secondstate.io/articles/getting-started-with-rust-function/) [Rust JavaScript 桥](https://www.secondstate.io/articles/rust-functions-in-nodejs/)。

调查发现，除了 C/C++和 JavaScript，Rust 开发者对与 R 和 Python 的集成也很感兴趣。这表明了开发者对机器学习、大数据和人工智能(AI)应用的兴趣。事实上，很多 Python 和 R 机器学习和统计包都是在原生二进制模块中实现的。

Rust 是编写本机模块的最佳编程语言之一。[这个例子](https://github.com/second-state/rust-wasm-ai-demo)展示了如何使用 [Rust 在 Node.js](https://www.secondstate.io/articles/artificial-intelligence/) 应用中执行 Tensorflow 模型。将来，我们设想这样的 Rust 模块运行在高性能的托管容器中，比如 WebAssembly。

## 结论

2019 年是 Rust 增长和增量改进的一年。随着 Rust 成为主流编程语言，我们期待更多的文档、更多的工具、更多的生态系统支持、与其他语言更多的互操作性以及更温和的学习曲线。

最重要的是，我们渴望结交更多的朋友，享受世界上最受欢迎的编程语言带来的乐趣！

## 关于作者

Michael Yuan 博士是关于软件工程的 5 本书的作者。他的最新著作[构建区块链应用](https://www.buildingblockchainapps.com/)于 2019 年 12 月由 Addison-Wesley 出版。袁博士是第二状态(Second State)的联合创始人，这是一家由风投资助的初创公司，将 WebAssembly 和 Rust 技术引入云应用、人工智能应用。它使开发人员能够在 Node.js 上部署快速、安全、可移植、无服务器的 [Rust 函数。](https://www.secondstate.io/articles/getting-started-with-rust-function/)

[https://webassemblytoday.substack.com/embed](https://webassemblytoday.substack.com/embed)

在 Second State 之前，袁博士是 Red Hat、JBoss 和 Mozilla 的长期开源贡献者。在软件之外，袁博士是美国国立卫生研究院的首席研究员，在癌症和公共卫生研究方面获得了多项研究奖。他拥有奥斯汀德克萨斯大学的天体物理学博士学位。