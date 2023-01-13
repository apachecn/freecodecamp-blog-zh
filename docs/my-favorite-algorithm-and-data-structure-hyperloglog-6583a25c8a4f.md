# 这就是为什么超对数算法是我的新宠

> 原文：<https://www.freecodecamp.org/news/my-favorite-algorithm-and-data-structure-hyperloglog-6583a25c8a4f/>

亚历克斯·纳达林

# 这就是为什么超对数算法是我的新宠

![1*ij4OThN8DISD0zwH-7UlWQ](img/7c4ba1a3996792885ce21a154dd38d8a.png)

Photo by [Nick Hillier](https://unsplash.com/photos/yD5rv8_WzxA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

时不时地，我会想到一个简单而强大的概念，我真希望自己发现了一个如此不可思议而又美丽的想法。

几年前，我发现了超级日志(HLL)，并在阅读了 T2 的 redis 如何决定增加一个 HLL 数据结构(T3)后爱上了它。

HLL 背后的想法极其简单，但却极其强大。这就是为什么它是如此广泛的算法，被互联网巨头如谷歌和 Reddit 所使用。

#### 收集电话号码

我和我的朋友汤米计划去参加一个会议。在前往拍摄地点的途中，我们决定打赌谁会遇到最多的新人。一旦我们到达那个地方，我们就开始四处交谈，并记录我们交谈的人数。

![0*oIVPnmV6cE4mAbaK](img/29d8759fb369f7277d22fa99463db93d.png)

活动结束时，汤米拿着他的数字来找我——比如说，17 个——我告诉他，我和 46 个人聊过。

很明显，我是赢家，但是汤米很沮丧，因为他认为我已经数了同样的人很多次了。他认为他只看到我总共和 15-20 个人说话。

所以，打赌取消了。我们决定，在下一个活动中，我们将记下名字，以确保我们统计的是独特的人，而不仅仅是对话的总数。

在接下来的会议结束时，我们带着一份很长的名单见面，你猜怎么着？汤米的遭遇比我还多。我们一笑置之，在讨论我们计算唯一性的方法时，汤米想到了一个好主意:

*“亚历克斯，你知道吗？我们不能拿着纸和笔到处走，然后追踪一个名单，这真的不切实际！今天我和 65 个不同的人交谈，在这张纸上数他们的名字真是一件痛苦的事。我数错了 3 次，不得不从头开始！”*

是的，我知道，但是我们还有别的选择吗

如果在我们的下一次会议上，我们不询问姓名，而是询问人们电话号码的最后 5 位数字，会怎么样？获胜者不是通过数他们的名字来获胜，而是与那些数字中前导零序列最长的人说话的那个人。”

“等等汤米，你开得太快了！慢一秒钟，给我举个例子……”

当然，只要问每个人最后 5 位数，好吗？让我们假设他们回复‘54701’。没有前导零，所以最长的前导零序列是 0。下一个和你说话的人说‘02561’——那是一个前导零！所以你的最长序列现在是 1。”

*“你开始让我明白了…”*

是的，所以如果我们只和几个人说话，最长的零序列很可能是 0。但如果我们和 10 个人交谈，我们就更有可能是 1。”

*“现在，想象你告诉我你的最长零序列是 5——你必须和成千上万的人交谈才能找到电话号码中有 00000 的人！”*

“老兄，你真是个天才！”

#### 朋友们，这就是超级日志的基本工作原理。

它允许我们通过记录大型数据集中最长的零序列来估计该数据集中的唯一项目。

与跟踪集合中的每一个元素相比，这最终创造了一个难以置信的优势。这是一种非常高效的计算唯一值的方法，而且精确度相对较高。

> *“超对数算法可以估计远超过 10⁹的基数，相对精度(标准误差)为 2%，同时仅使用 1.5kb 的内存”*

> ***杨—*** *[快速、廉价、98%正确:大数据基数估计](http://druid.io/blog/2012/05/04/fast-cheap-and-98-right-cardinality-estimation-for-big-data.html)*

由于我可能过于简单化，让我们来看看 HLL 更多的细节。

### 更多 HLL 细节

HLL 是一系列算法的一部分，这些算法旨在解决基数估计问题，也就是众所周知的“非重复计数问题”我们如何有效地计算数据集中唯一对象的数量？

这对于当今的许多 web 应用程序来说非常有用。例如，当你想计算一篇文章在你的网站上产生了多少独特的看法。

当 HLL 运行时，它获取您的输入数据并进行哈希处理，将其转换为一个位序列:

```
IP address of the viewer: 54.134.45.789
```

```
HLL hash: 010010101010101010111010...
```

现在，HLL 的一个重要部分是确保你的散列函数尽可能均匀地分配比特。您不希望使用弱函数，例如:

例如，如果访问者的[分布与特定的地理区域](https://stackoverflow.com/a/277537/934439)相关联，使用这种哈希函数的 HLL 将返回有偏差的结果。

[原文](http://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf)有更多关于好的散列函数对 HLL 意味着什么的细节:

> 所有已知的有效基数估计器依赖于随机化，这通过使用散列函数来确保。

> *要计数的元素属于某个数据域 D，我们假设给定一个哈希函数，h : D → {0，1 }∞；也就是说，我们将散列值同化为{0，1}∞的无限二进制串，或者等效为单位区间的实数。*

> *[…]*

> 我们假设散列函数已经以这样的方式被设计，即散列值非常类似于随机的统一模型，即，散列值的比特被假设为独立的，并且具有每个出现概率[0.5]。"

> ***Philippe Flajolet—****[hyperlog log:一种近优基数估计算法的分析](http://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf)*

现在，在我们选择了合适的散列函数之后，我们需要解决另一个陷阱:[方差](https://en.wikipedia.org/wiki/Variance)。

回到我们的例子，想象你在会议上交谈的第一个人告诉你他们的号码以`00004`结尾——jackpot！

你可能赢得了对汤米的赌注，但如果你在现实生活中使用这种方法，你的数据集中的特定数据可能会对估计产生负面影响。

不要再害怕了，因为这是 HLL 生来要解决的问题。

没有多少人知道 HLL 的智囊之一 Philippe Flajolet 已经参与基数估计问题很长时间了。时间长到足以在 1984 年提出 [Flajolet-Martin 算法](https://en.wikipedia.org/wiki/Flajolet%E2%80%93Martin_algorithm#Improving_accuracy)，在 2003 年提出[(超级)LogLog 算法](http://algo.inria.fr/flajolet/Publications/DuFl03-LNCS.pdf)。

这些算法已经通过将测量划分到桶中，并(在某种程度上)对桶中的值进行平均，解决了一些与外围散列值有关的问题。

如果你在这里迷路了，让我回到我们最初的例子。

我们不是只取电话号码的最后 5 位，而是取其中的 6 位。现在，我们将最长的前导零序列与第一个数字(桶)存储在一起。

这意味着我们的数据将看起来像:

```
Input:708942 --> in the 7th bucket, the longest sequence of 0s is 1518942 --> in the 5th bucket, the longest sequence of 0s is 0500973 --> in the 5th bucket, the longest sequence of 0s is now 2900000 --> in the 9th bucket, the longest sequence of 0s is 5900672 --> in the 9th bucket, the longest sequence of 0s stays 5
```

```
Buckets:0: 01: 02: 03: 04: 05: 26: 07: 18: 09: 5
```

```
Output:avg(buckets) = 0.8
```

如您所见，如果我们不使用桶，我们将使用 5 作为最长的零序列。这将对我们的估计产生负面影响。

尽管我简化了 buckets 背后的数学(这不仅仅是一个简单的平均值)，但是您完全可以看到这种方法是如何有意义的。

有趣的是看到 Flajolet 如何在他的作品中处理变化:

> “虽然我们已经有了一个相当不错的估计，但还有可能变得更好。Durand 和 Flajolet 观察到异常值大大降低了估计的准确性；通过在求平均值之前丢弃最大值，可以提高精确度。

> *具体来说，通过丢弃具有最大值的 30%的桶，并且仅对具有较小值的 70%的桶进行平均，准确度可以从 1.30/sqrt(m)提高到仅 1.05/sqrt(m)！这意味着我们之前的例子，有 640 字节的状态和 4%的平均误差，现在有大约 3.2%的平均误差，不需要额外增加空间。*

> *最后，Flajolet 等人在超对数论文中的主要贡献是使用了一种不同类型的平均，采用调和平均值代替我们刚刚应用的几何平均值。通过这样做，他们能够将误差降低到 1.04/sqrt(m)，同样不增加所需的状态"*

> ***尼克·约翰逊****——[提高准确度:超级日志和超级日志](http://blog.notdot.net/2012/09/Dam-Cool-Algorithms-Cardinality-Estimation)*

### 野外的 HLL

那么，在哪里可以找到 HLLs 的应用呢？两个伟大的网络规模的例子是:

*   [BigQuery](https://cloud.google.com/blog/big-data/2017/07/counting-uniques-faster-in-bigquery-with-hyperloglog) ，有效地计算表中的唯一性(`APPROX_COUNT_DISTINCT()`)
*   Reddit ，它被用来计算一个帖子聚集了多少独特的浏览量

具体来说，请看 HLL 如何影响 BigQuery 上的查询:

```
SELECT COUNT(DISTINCT actor.login) exact_cntFROM `githubarchive.year.2016`6,610,026 (4.1s elapsed, 3.39 GB processed, 320,825,029 rows scanned)
```

```
SELECT APPROX_COUNT_DISTINCT(actor.login) approx_cntFROM `githubarchive.year.2016`6,643,627 (2.6s elapsed, 3.39 GB processed, 320,825,029 rows scanned)
```

第二个结果是一个近似值(误差率约为 0.5%)，但需要一部分时间。

长话短说:**hyperlog 太神奇了！**

现在你知道它是什么，什么时候可以使用它，所以出去用它做不可思议的事情吧！

### 进一步阅读

*   [维基百科上的超级日志](https://en.wikipedia.org/wiki/HyperLogLog)
*   [原文](http://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf)
*   [HyperLogLog++，谷歌对 HLL 的改进实现](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/40671.pdf)
*   [Redis 新数据结构:HyperLogLog](http://antirez.com/news/75)
*   [该死的酷算法:基数估计](http://blog.notdot.net/2012/09/Dam-Cool-Algorithms-Cardinality-Estimation)
*   [Riak 中的 HLL 数据类型](https://github.com/basho/riak_kv/blob/develop/docs/hll/hll.pdf)
*   [超级日志和最小哈希](http://tech.adroll.com/blog/data/2013/07/10/hll-minhash.html)

*最初发表于[odino.org](http://odino.org/my-favorite-data-structure-hyperloglog/)(2018 年 1 月 13 日)。*