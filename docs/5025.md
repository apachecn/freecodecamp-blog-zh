# CSP vs RxJS:你不知道的。

> 原文：<https://www.freecodecamp.org/news/csp-vs-rxjs-what-you-dont-know-1542cd5dd100/>

凯文·加德亚尼

# CSP vs RxJS:你不知道的。

#### CSP 怎么了？

![0*ColTdidsgUyGVesk](img/68b0b84f161324fc8504dfeb9b2bfb64.png)

Photo by [James Pond](https://unsplash.com/@lamppidotco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您可能在点击这篇文章时会想“什么是 CSP？”这是**与顺序流程**的沟通。还是百思不得其解？

CSP 是一种使用共享通道在代码中的不同函数(生成器)之间进行通信的方法。

这到底是什么意思？让我直截了当地告诉你。有一个渠道的概念。把它想象成一个队列。你可以在上面放东西，也可以从上面拿东西。

所以有了两个功能，你可以有一个在频道上添加东西(生产者)和另一个拉东西做一些工作(消费者)。

一个典型的高级用例是多个生产者和一个消费者。这样，你可以控制你得到的数据，但你可以有多个东西给你。

与 RxJS 不同，这些通道是自动的。你不会不情愿地得到价值，你必须去要求它们。

### 使用 CSP

下面是一个使用超级简单(也是死的)库 [Channel4](https://www.npmjs.com/package/channel4) 的小型 CSP 示例:

CSP 通道异步运行。因此，一旦运行，同步的“完成”消息首先被记录。然后我们的通道接受者被依次处决。

我最感兴趣的是 CSP 的阻塞(但异步)特性。请注意，在将“C”放在通道上之前，我们是如何创建第三个`take`的。与前两个`take`函数不同的是，第三个函数什么都不带。一旦有什么东西进入通道，它就立刻把它拿走。

还要注意，消费者需要不断地从渠道中拿走东西，直到渠道关闭。这就是为什么“D”永远不会被记录。您需要定义另一个`take`来获取通道的下一个值。

有了可观测量，你就有了值，所以你不必担心手动获取它们。如果您想缓冲这些值，RxJS 为此提供了相当多的管道方法。不需要使用 CSP。

observables 背后的整个概念是，只要观察者一调用`next`，每个监听器就获得相同的数据。使用 CSP，就像 IxJS 方法一样，您可以分块处理数据。

### CSP 死了！？

您可以在 [Go](https://godoc.org/github.com/thomas11/csp) 和 [Closure](https://github.com/clojure/core.async) 中找到 CSP 实现。在 JavaScript 中，除了几个 CSP 库之外，其他的都死了，即使这样，它们的受众也很少？。

我是从[文森佐·基亚内塞的精彩演讲](https://www.youtube.com/watch?v=r7yWWxdP_nc)中知道 CSP 的。他推荐了这个叫做 [js-csp](https://github.com/ubolonton/js-csp) 的高端库。可悲的是，它不再被维护。

根据他在 2017 年的演讲中所说的，这似乎是一件大事。他谈到了传感器在几个月后将如何爆炸，以及 js-csp 已经如何支持它们。

看起来 CSP 可以从根本上改变用 JavaScript 开发异步应用程序的方式。但是这些都没有发生。传感器消失了；取而代之的是像 RxJS 这样的库，对 CSP 的大肆宣传也烟消云散了。

文森佐确实注意到 CSP 比承诺高了一个层次。他是对的。让多个函数异步交互所带来的力量是难以置信的。

承诺，就其热切的本性而言，甚至不在同一个范围内。他一点也不知道最后几个 CSP 库最终会支持幕后的承诺。。

### CSP 替代方案:Redux-Saga

如果您曾经使用过 Redux-Saga，那么关于 CSP 的想法和概念可能听起来很熟悉。那是因为他们是。实际上，Redux-Saga 是 CSP 在 JavaScript 中的一种实现；目前为止最受欢迎的。

Redux-Sagas 中甚至还有“渠道”的概念:
[https://github . com/Redux-saga/Redux-saga/blob/master/docs/advanced/channels . MD](https://github.com/redux-saga/redux-saga/blob/master/docs/advanced/Channels.md)

通道从外部事件接收信息，将动作缓冲到 Redux 存储，并在两个 sagas 之间进行通信。这与 CSP 中使用相同的`take`和`put`功能的方式相同。

看到 CSP 在 JavaScript 中的实际实现很酷，但奇怪的是很少有人注意到它。向你展示了小 CSP 死前是如何离开的。

### CSP 替代方案:Redux-可观察

你可能听说过一个叫 Redux-Observable 的东西。这是一个类似于 CSP 和 Redux-Saga 的概念，但它不是命令式的生成器，而是采用一种函数式的方法，并利用被称为“epics”的 RxJS 管道。

在 Redux-Observable 中，一切都是通过两个主体发生的:`action$`和`state$`。那些是你的频道。

您不是手动获取和放置，而是作为动作或状态通道的消费者来监听特定的动作。通过管道发送动作，每个史诗都有能力成为生产者。

如果您想像 CSP 一样在 Redux-Observable 中构建一个队列，这要稍微复杂一点，因为没有用于此目的的操作符，但是这是完全可能的。

我创建了一个 repl 来做这件事:

与我们之前的 CSP 示例相比，这是您可以预期看到的结果:

该示例只需要 RxJS，为了简单起见，所有内容都在一个文件中。如您所见，在 RxJS 中像在 CSP 中一样排列项目要困难得多。这是完全可能的，但是需要更多的代码。

就我个人而言，我很乐意看到 RxJS 添加一个像`bufferWhen`这样的操作符，它允许你划分单个项目，而不是整个缓冲区。然后，您可以更容易地在 Redux-Observable 中完成 CSP 风格的任务。

### 结论

CSP 是一个很酷的概念，但是它在 JavaScript 中已经死了。Redux-Saga 和 Redux-Observable 是有价值的选择。

即使能够与传感器库集成，RxJS 仍然有明显的优势。其庞大的教育工作者和生产应用程序社区使其难以竞争。

这就是为什么我认为 CSP 死在了 JavaScript 里。

### 更多阅读

如果你喜欢你所读的，请查看我关于类似的令人大开眼界的主题的其他文章:

*   [Redux-Observable 可以解决你的状态问题](https://medium.com/@Sawtaytoes/redux-observable-can-solve-your-state-problems-15b23a9649d7)
*   [Redux-可观察无 Redux](https://itnext.io/redux-observable-without-redux-ff4a2b5a4b39)
*   [试听:权威指南](https://itnext.io/the-definitive-guide-to-callbacks-in-javascript-44a39c065292)
*   [承诺:权威指南](https://itnext.io/promises-the-definitive-guide-6a49e0dbf3b7)