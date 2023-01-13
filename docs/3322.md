# JavaScript setTimeout 教程——如何使用睡眠、等待、延迟和暂停的 JS 等价物

> 原文：<https://www.freecodecamp.org/news/javascript-sleep-wait-delay/>

JavaScript 是网络语言。而且自从 ES5 发布后就不一样了。越来越多的想法和特性正从不同的语言移植过来，并集成到 JavaScript 中。

其中一个特性是承诺，这可能是 ES5 发布后 JavaScript 中使用最广泛的特性。

但是 JavaScript 遗漏了一件事，那就是“暂停”执行一段时间，稍后再继续执行。在这篇文章中，我将讨论如何实现这一点，以及 JavaScript 中“暂停”或“睡眠”的真正含义。

剧透:JavaScript 从未真正“暂停”。

## TL；速度三角形定位法(dead reckoning)

下面是完成这项工作的 copy-pasta 代码:

```
/**
 * 
 * @param duration Enter duration in seconds
 */
function sleep(duration) {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve()
		}, duration * 1000)
	})
}
```

但是这里到底发生了什么？

## 暂停和虚假承诺

让我们用上面的代码片段看一个简单的例子(我们将在后面讨论其中发生的事情):

```
async function performBatchActions() {
	// perform an API call
	await performAPIRequest()

	// sleep for 5 seconds
	await sleep(5)

	// perform an API call again
	await performAPIRequest()
}
```

这个函数`performBatchActions`，当被调用时，简单地执行`performAPIRequest`函数，等待**大约 5 秒**，然后再次调用相同的函数。注意我是如何写的**大约 5 秒**，而不是 5 秒。

需要强调的是:上面的代码并不能保证完美的睡眠。这意味着，如果你指定持续时间，比如说，1 秒，JavaScript **不能保证**会在 1 秒后开始运行代码。

为什么不呢？你可能会问。不幸的是，这是因为定时器在 JavaScript 中工作，一般来说，事件循环。然而，JavaScript 绝对保证睡眠后的代码段永远不会在指定时间之前执行**。**

所以我们没有完全不确定的情况，只是部分不确定。在大多数情况下，误差只有几毫秒。

## JavaScript 是单线程的

单线程意味着一个 JavaScript 进程根本不可能真正脱离正轨。它必须在同一个主线程上做所有的事情——从事件监听器到 HTTP 回调。当一件事正在执行时，另一件事不能执行。

考虑一个网页，其中有多个按钮，你运行上面的代码来模拟睡眠，比如说，10 秒钟。你认为会发生什么？

什么都没有。你的网页会工作得很好，你的按钮会有反应，一旦 10 秒睡眠结束，旁边的代码就会执行。所以很明显，JavaScript 并没有真正阻塞整个主线程，因为如果它这样做了，你的网页会冻结，按钮会变得不可点击。

那么 JavaScript 实际上是如何暂停一个线程，而不是真正暂停它呢？

## 遇见事件循环

与其他语言不同，JavaScript 不只是从上到下以线性方式执行代码。它是一种异步的事件驱动语言，以事件循环的形式提供了大量的魔法。

事件循环将您的代码分成同步的和特定的事件，比如计时器和 HTTP 请求。准确地说，有两个队列——任务队列和微任务队列。

每当运行 JS 时，如果有异步事件(比如鼠标点击事件，或者一个承诺)，JavaScript 会将它放入任务队列(或者微任务队列)并继续执行。当它完成一个“单滴答”时，它检查任务队列和微任务队列是否有一些工作要做。如果是，那么它将执行回调/执行一个动作。

我强烈建议任何对事件循环的详细工作方式感兴趣的人观看此视频:

[https://www.youtube.com/embed/8aGhZQkoFbQ?feature=oembed](https://www.youtube.com/embed/8aGhZQkoFbQ?feature=oembed)

## 结论

您来到这里是为了一个简单的 JavaScript 睡眠指令，并最终了解了 JavaScript 的核心内容之一——事件循环！很神奇，不是吗？

好吧，如果你喜欢这篇文章，请查看一下[code damn](https://codedamn.com)——我为像你这样的开发者和学习者搭建的平台。同样，让我们在社交媒体上联系一下——[推特](https://twitter.com/mehulmpt)和 [Instagram](https://instagram.com/mehulmpt) 。回头见！

和平