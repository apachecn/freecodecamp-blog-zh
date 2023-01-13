# JavaScript 的异步 API 定时器之间的区别

> 原文：<https://www.freecodecamp.org/news/the-differences-between-javascripts-asynchronous-api-timers-d916e0596716/>

作者:Rajika Imal

# JavaScript 的异步 API 定时器之间的区别

![1*iF8uCp-Dx8BfuCSgkbHvnQ](img/9c1924d703a3bd1052c73564aa825658.png)

Photo by [Szűcs László](https://unsplash.com/photos/0gdHUhYkXDc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/timer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 是一种单线程语言，它利用异步结构来并发处理任务。有趣的是，与 Java 和 C#等传统语言相比，它以不同的方式高效地处理并发任务。

#### 事件循环

无论是浏览器环境还是 Node.js，JavaScript 都是异步的，因为它使用了事件循环。在节点环境中，它是使用 libuv 库实现的。最初 libuv 是作为 libev 的包装器开发的。在节点版本 0.9.0 中，删除了对 libev 的依赖。

#### 事件循环中的阶段

```
 ┌───────────────────────────┐┌─>│           timers          ││  └─────────────┬─────────────┘│  ┌─────────────┴─────────────┐│  │     pending callbacks     ││  └─────────────┬─────────────┘│  ┌─────────────┴─────────────┐│  │       idle, prepare       ││  └─────────────┬─────────────┘      ┌───────────────┐│  ┌─────────────┴─────────────┐      │   incoming:   ││  │           poll            │<─────┤  connections, ││  └─────────────┬─────────────┘      │   data, etc.  ││  ┌─────────────┴─────────────┐      └───────────────┘│  │           check           ││  └─────────────┬─────────────┘│  ┌─────────────┴─────────────┐└──┤      close callbacks      │   └───────────────────────────┘
```

> 来源:[https://nodejs . org/en/docs/guides/event-loop-timers-and-next tick/](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

如上所述，事件循环可以分为几个阶段。每个阶段将在每次迭代中执行。这样的一次迭代称为事件循环中的一个节拍。每个阶段都有一个先进先出队列(FIFO ),用于登记不同的任务。为了理解 setTimeout、setImmediate 和 nextTick 是如何工作的，我们将经历相关的重要阶段。

#### 计时器阶段

由 *setTimeout* 和 *setInterval* 注册的回调将在本阶段执行。需要注意的是，回调不会立即执行，而是在超过某个时间阈值后执行。

#### 检查阶段

如果处理 I/O 回调的轮询阶段变得空闲或超过最大执行次数，它将转到检查阶段，在那里它将执行由 *setImmediate* 注册的回调。

#### 微任务队列和宏任务队列

这两个队列对于理解通过不同 API 执行的任务的顺序非常重要。宏任务在上图所示的每个阶段中执行。

setImmediate 是宏任务队列的一部分。微任务将一直执行到队列为空，然后继续下一次迭代或事件循环的节拍。

process.nextTick 回调将在微任务队列中注册，它们将一直执行到队列为空。因此，在 process.nextTick 中进行递归调用会使事件循环饥饿，阻止它进入下一个 Tick。宏任务不会让事件循环挨饿，因为一旦达到最大执行次数，它就会在下一个节拍移动。

让我们看几个例子，看看每个 API 在现实世界中是如何工作的，以便更好地理解。

在本文展示的其余示例中，Node.js 将用作执行环境。

#### settimeout vs setimmediate(第七季)

请注意，这些调用不在一个 I/O 周期内。因为这个事实，执行将取决于 CPU 的性能。因此，在这种情况下，日志将被随机打印出来。

在本例中，它们在一个 I/O 周期内。由于 macrotask 队列(检查阶段)将在 tick 之后执行，因此每次都会执行 setImmediate 回调。一旦超过阈值，将在计时器阶段调用 setTimeout。

#### setImmediate vs process.nextTick

*nextTick* 是微任务队列的一部分，它将在事件循环移动到下一个 Tick 之前执行。在下一个 Tick 中跟随 nextTick，setImmediate 将在检查阶段触发它在 macrotask 队列中的回调。

nextTick 执行递归函数，该函数将一直执行到进入基本条件(如果 num > 5)。只有在执行 nextTick 之后，setImmediate 才会触发它的回调。连续递归行为是由于 nextTick 是微任务队列的一部分，不允许事件循环继续到下一个 Tick。

#### set immediate vs setTimeout vs process . next tick

正如所料，首先调用 nextTick，然后是 setImmediate 和 setTimeout。需要注意的是，这些函数是在一个 I/O 周期中调用的。如果它们不在 I/O 周期内，输出将会不同，并且将取决于过程性能。

> 跟进资源

[**并发模型和事件循环**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
[*JavaScript 有一个基于“事件循环”的并发模型。该模型与其他模型有很大不同…*developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)[**Node.js 事件循环、计时器和 process . next tick()| node . js**](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
[*Edit 在 GitHub 上，事件循环允许 node . js 执行非阻塞 I/O 操作—*nodejs.org](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)[**任务、微任务、队列和调度**](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)