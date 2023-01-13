# rx——如果操作员会说话就好了！

> 原文：<https://www.freecodecamp.org/news/rx-if-the-operators-could-speak-58567c4618f1/>

艾哈迈德·里兹万

![YxwemabyTeiiKMYmw6mStF2kTUHkSYP0UDIU](img/ad559f8510ef85315c8a8cb0ee59a774.png)

# rx——如果操作员会说话就好了！

如果操作员会说话，他们会怎么告诉我们他们在做什么？

为了充分利用 Rx，您需要清楚地了解 Rx 操作符是什么以及它们做什么。

当我们使用它们的时候，如果它们能说话，这就是操作者会告诉我们的。

对于本文，我假设您已经知道 Rx 是什么。如果没有，转到[读取此](https://medium.com/@ahmedrizwan/rxandroid-and-kotlin-part-1-f0382dc26ed8#.vundiz1fq)。或者只是简单地谷歌一下 Rx，你就会找到大量有用的文章、教程和视频。

### **创造运算符**

#### [创建](http://reactivex.io/documentation/operators/create.html)

> 我告诉你发出什么，什么时候终止，抛出什么错误。因为我是老板。

#### [延期](http://reactivex.io/documentation/operators/defer.html)

> 只有当有人订阅你时，你才能“创造”自己。每一次都是一个全新的你。

#### [清空](http://reactivex.io/documentation/operators/empty-never-throw.html)

> 嗯。不发射任何东西。然后死了，求求你。

#### [从不](http://reactivex.io/documentation/operators/empty-never-throw.html)

> 不发射任何东西。不要…永远不要…终止。

#### [投掷](http://reactivex.io/documentation/operators/empty-never-throw.html)

> 不发射任何东西，然后抛出一个错误，好吗？

#### [来自](http://reactivex.io/documentation/operators/from.html)

> 我会给你一些东西，然后你把它们发射回来给我。

#### [间隔](http://reactivex.io/documentation/operators/interval.html)

> 这个怎么样:你发射物品。但不是马上。在一定的“间隔”后，将它们一个接一个地送回去

#### [只是](http://reactivex.io/documentation/operators/just.html)

> 我只需要你给我一样东西。只有一个。

#### [范围](http://reactivex.io/documentation/operators/range.html)

> 我给你一个整数范围，然后你发出这个范围内的所有值。

#### [重复](http://reactivex.io/documentation/operators/repeat.html)

> 你重复发射同一个物体。

#### [开始](http://reactivex.io/documentation/operators/start.html)

> 好的。我有一个功能。当它回来时，你开始发射。但只有当它回来的时候。明白了吗？

#### [小时](http://reactivex.io/documentation/operators/timer.html)

> 所以你得到了一个项目。暂时不要发射。我会告诉你确切的发射时间。不要草率行事。

### 转换运算符

#### [缓冲器](http://reactivex.io/documentation/operators/buffer.html)

> 好吧，这样吧。不管你平时释放什么，我们都不会释放。相反，随着时间的推移，将物品收集成捆。而是发送捆绑包。因为我想要捆绑！

#### [平面图](http://reactivex.io/documentation/operators/flatmap.html)

> 所以，比如说，如果你有一个物品列表，而另一个可观察对象中充满了物品，你能不能把你自己和那个可观察对象“拉平”，这样你就可以发送物品了？

#### [地图](http://reactivex.io/documentation/operators/map.html)

> 将每个项目转换成另一个项目。

#### [扫描](http://reactivex.io/documentation/operators/scan.html)

> 将每个项目转换成另一个项目，就像您对地图所做的那样。但是当您着手进行转换时，也要包括“前一个”项目。

### 过滤运算符

#### [去抖](http://reactivex.io/documentation/operators/debounce.html)

> 仅在经过一定时间后发出。

#### [截然不同的](http://reactivex.io/documentation/operators/distinct.html)

> 仅发出不同的项目。好吗？

#### [ElementAt](http://reactivex.io/documentation/operators/elementat.html)

> 我告诉你指数。你在那个索引处发射物品。

#### [过滤器](http://reactivex.io/documentation/operators/filter.html)

> 我给你一个标准。你给我通过标准的项目。

#### [第一次](http://reactivex.io/documentation/operators/first.html)

> 把第一件东西还给我。

#### [IgnoreElements](http://reactivex.io/documentation/operators/ignoreelements.html)

> 不要，我再说一遍，不要发射任何东西。然后死去。

#### [最后一次](http://reactivex.io/documentation/operators/last.html)

> 把最后一件东西还给我。

#### [样品](http://reactivex.io/documentation/operators/sample.html)

> 我给你一个间隔。你只给我最近的项目。

#### [跳过](http://reactivex.io/documentation/operators/skip.html)

> 好吧，跳过前 n 项，好吗？

#### SkipLast

> 跳过最后 n 项。对，就是那些。

#### [乘](http://reactivex.io/documentation/operators/take.html)

> 仅发出前 n 个项目。

#### [TakeLast](http://reactivex.io/documentation/operators/takelast.html)

> 仅发出最后 n 个项目。

### 组合运算符

#### [合并](http://reactivex.io/documentation/operators/merge.html)

> 这里有两个值得注意的地方。让我们假设它们只是一个可观测的。

#### [从](http://reactivex.io/documentation/operators/startwith.html)开始

> 这里有两个值得注意的地方。但我会告诉你从哪一个开始。

#### [组合测试](http://reactivex.io/documentation/operators/combinelatest.html)

> 这里有两个值得注意的地方。在两者之间，将最新的物品配对。

#### [Zip](http://reactivex.io/documentation/operators/zip.html)

> 这里有两个值得注意的地方。但是我告诉你如何组合它们的项目(当然是通过一个函数)。

### 处理错误

#### [接住](http://reactivex.io/documentation/operators/catch.html)

> 抛出错误后，继续发出。

#### [重试](http://reactivex.io/documentation/operators/retry.html)

> 抛出错误后，从头开始重新启动。

### 效用

#### [延迟](http://reactivex.io/documentation/operators/delay.html)

> 开始发射前加个延时就行了，好吗？

#### 观察

> “观察”代码应该在这个特定的线程上运行。

#### [订阅](http://reactivex.io/documentation/operators/subscribeon.html)

> “订阅”代码应该在这个特定的线程上运行。

#### [订阅](http://reactivex.io/documentation/operators/subscribe.html)

> 你现在可以开始发射了。*音乐加强*

#### [时间间隔](http://reactivex.io/documentation/operators/timeinterval.html)

> 好的，那么 observables 送回物品，对吗？相反，我希望你把时间间隔发送回来。比如每次发射的时间差。

#### [超时](http://reactivex.io/documentation/operators/timeout.html)

> 设置每次发射的超时时间。如果一个项目没有在这段时间内发出，就抛出一个错误*？*

### 条件与布尔运算

#### [全部](http://reactivex.io/documentation/operators/all.html)

> 如果所有项目都满足特定条件，则返回 true。

#### [Amb](http://reactivex.io/documentation/operators/amb.html)

> 这里至少有两点值得注意。给我先开始发射的那个。

#### [包含](http://reactivex.io/documentation/operators/contains.html)

> 如果我要一样东西，你能告诉我你是否已经有了吗？

#### [default ifying](http://reactivex.io/documentation/operators/defaultorempty.html)

> 当你没有东西要发出时，这里有一个默认值，你可以发送回来。

#### [顺序相等](http://reactivex.io/documentation/operators/sequenceequal.html)

> 这里有两个值得注意的地方。如果它们的项目(及其顺序)相同，则返回 true。

#### [SkipUntil](http://reactivex.io/documentation/operators/skipuntil.html)

> 这里有两个值得注意的地方。跳过第一个的项目，直到第二个开始发射。

#### [一会儿跳过](http://reactivex.io/documentation/operators/skipwhile.html)

> 我给你一个条件。你发射物品，直到条件变为假。

#### [持续到](http://reactivex.io/documentation/operators/takeuntil.html)

> 这里有两个值得注意的地方。只给我第一个的项目，直到第二个开始发射。

### 数学运算符

#### [平均值](http://reactivex.io/documentation/operators/average.html)

> 给我你整数项的平均值。

#### [计数](http://reactivex.io/documentation/operators/count.html)

> 给我数一数你的物品。

#### [最大](http://reactivex.io/documentation/operators/max.html)

> 仅发出最大值的项目。

#### [分钟](http://reactivex.io/documentation/operators/min.html)

> 仅发出最小价值的项目。

#### [减少](http://reactivex.io/documentation/operators/reduce.html)

> 进行扫描，但只发出最终值。

#### [总和](http://reactivex.io/documentation/operators/sum.html)

> 返回所有项目的总和。

### 转换运算符

#### [至](http://reactivex.io/documentation/operators/to.html)

> 将一个可观察对象转换成列表、地图或数组，或者我告诉你的任何东西。

暂时就这样了。还有其他的操作符，你可以在这里找到[。你也可以看看](http://reactivex.io/documentation/operators.html) [RxMarbles](http://rxmarbles.com) ，它有很酷的图表来演示每个操作员。

无论如何，谢谢你的阅读。我希望这篇文章以有趣的方式帮助您更好地理解这些命令的作用。

编码快乐！