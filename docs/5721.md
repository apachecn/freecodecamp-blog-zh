# 我从对 Node.js Core 的第一次贡献中学到了什么

> 原文：<https://www.freecodecamp.org/news/what-i-learned-from-my-first-contribution-to-node-js-core-9cdc0e0d5efc/>

作者:耶尔·赫尔蒙

# 我从对 Node.js Core 的第一次贡献中学到了什么

![1*TMcOrpatfD5sTKioEqsMKg](img/a288c7f57908e84836e4291a6c11a68e.png)

drain

几周前，我的第一个 Node.js 核心公关被合并了！几天后，我决定在推特上发布这件事，并分享这一经历是多么积极，希望鼓励其他人也做出贡献。

后来， [Uri Shaked](https://www.freecodecamp.org/news/what-i-learned-from-my-first-contribution-to-node-js-core-9cdc0e0d5efc/undefined) 建议我在一篇简短的博文中分享我的经历。尤里总是有好主意。谢谢你，尤里！

#### 我怎么会有这个 PR？

很高兴你问了。我先说一点背景。我喜欢 Node.js，为它做贡献实际上在我的遗愿清单上已经有很长时间了。我从来没有抽出时间去做这件事，因为我总是告诉自己，我没有时间做这件事，或者我可能不够资格，或者其他蹩脚的借口。

剧情转折发生在我正在为一个 JavaScript-Israel 会议做关于 [V8 垃圾收集器](https://docs.google.com/presentation/d/14CVuylg19RUnNLz525ecSHTyN7upVdF196WNtzhqdoA/edit?usp=sharing)的演讲时。 [Benjamin Gruenbaum](https://github.com/benjamingr) ，Node.js 的核心合作者，问我是否愿意让他帮我联系 V8 的工程师来检查我的幻灯片。嗯… *显然，我会的。*

他照做了，结果非常棒。Benji 还问我是否有兴趣为 Node.js core 做贡献。再说一次，我答应了。别再找借口了。事情变得真实了。

#### 设置环境

我首先必须在我的机器上构建 Node.js。由于 Node.js 有[棒的 docs](https://github.com/nodejs/node/blob/master/BUILDING.md) ,这出奇的容易。接下来，我决定到处玩，并开始调试它。

如果我说从一开始就一帆风顺，那我是在撒谎。我最后一次使用 C++是在 2012 年。我生疏了。此外，那时我的环境与现在完全不同。我有一台装有 Visual Studio 的 Windows PC，而现在我在 Mac 上运行 VSCode。

我喜欢 VSCode，所以我也想为这个项目设置 VSCode。我很快就找到了一个[扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)，并配置好工作。对于我的调试配置，我最终设置了一个节点调试器和一个 lldb 调试器来附加到节点进程。效果很好。

#### 解决实际问题！

于是 Benji 给我联系了 [Anna](https://github.com/addaleax) ，她是 Node.js 的核心合作者，实现了“ [worker_threads](https://nodejs.org/api/worker_threads.html) ”。石磊还把这个[问题](https://github.com/nodejs/node/issues/24636)指向了我。我研究了这个问题，并试图用尽可能少的代码再现它，只是为了消除噪音。

我努力创建一个测试用例来重现这个问题，因为它是由竞争条件引起的。在 Node.js 内部运行时失败的代码在我的测试环境中不会失败。最终，我发现了一些失败的东西。虽然它可能不会在每台机器上或每次都失败，但安娜确认它已经足够好了。接下来，我开始调试它，看看那里到底发生了什么。

如果你从来没有听说过“工作者线程”，那可能是因为他们是相当新的，目前正处于实验状态。Workers 允许您创建在独立线程上运行的多个环境。它们对于执行 CPU 密集型 JavaScript 操作很有用，不会阻塞主线程。

主线程和工作线程可以通过它们之间的消息通道相互通信。除了这个消息通道之外，还有另一个消息通道用于发送内部消息，比如 worker 的 stdout。当您进入 worker 时，它通过这个内部消息通道到达主线程，主线程通过将它推送到它的 stdout 流来处理它。

问题是，我们在等待主线程通过内部消息端口处理来自 worker 的 stdio 的所有消息之前，调用了 JS worker 类中的`kDispose`函数。因此，当 worker 线程完成时，我们丢失了对父端 stdio 流的引用，之后可能会有一条消息到达父线程。

起初，我尝试了许多不同的方法来解决这个问题，包括设置一个承诺，在消息端口完成时进行解析，在释放之前等待它，以及将 JS 回调传递给 C++层。

与 Anna 聊天时，我了解到 MessagePort 有一个 *drain* 方法，它同步发出所有传入的消息。所以最终，来自它的所有消息都会被处理。事实上，已经为外部消息端口调用了 *drain* 。我怎么一直没看到这个函数呢？？我在内部消息端口上添加了一个对 d *rain* 的呼叫。修复就是这么简单。

要记住的一件重要事情是——一路上尝试奇怪的方法完全没问题。这就是你学习的方式。在调试了大量 worker_threads 代码之后，我可以说我现在已经非常了解它的一些代码库了:)

班吉和安娜从一开始就很热情。这是一次很棒的经历。我从安娜和代码中学到了很多，这非常具有挑战性。这绝对不是我日常生活中经常处理的事情。

我等不及要写下一期了！