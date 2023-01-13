# ScyllaDB 比 Cassandra 好，原因如下。

> 原文：<https://www.freecodecamp.org/news/scylladb-its-cassandra-but-better-76e3d83a4f81/>

作者:卡蒂克·哈雷

# ScyllaDB 比 Cassandra 好，原因如下。

![yNPhqEY4hcp3olLmtXnTDkiadKXCd7Bh2S2C](img/425e1c3b8520bd366aeff6130e3016f9.png)

Database by [Nick Youngson](http://www.nyphotographic.com/) [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/) [Alpha Stock Images](http://alphastockimages.com/)

ScyllaDB 是最新的 NoSQL 数据库之一，它以亚毫秒级延迟提供真正高的吞吐量。重要的一点是，它以现代 NoSQL 数据库的一小部分成本实现了这一点。

ScyllaDB 几乎实现了 C++中 [Cassandra](http://cassandra.apache.org/) 的所有特性。但是说它仅仅是一个 C++端口是一种保守的说法。Scylla 的开发人员做了很多用户看不到的改动，但这些改动带来了巨大的性能提升。

### 你在开玩笑，对吗？

不，[我不是](https://www.scylladb.com/product/benchmarks/aws-c3-2xlarge-benchmark/)。

如您所见(如果您访问了该链接)，在大多数情况下，Scylla 99.9%的延迟比 Cassandra 快 5-10 倍。

此外，在本文提到的基准测试中，一个标准的 3 节点 Scylla 集群提供了与 30 节点 Cassandra 集群几乎相同的性能(导致成本降低 10 倍)。

### 这怎么可能？

最重要的一点是，Scylla 是用 C++14 写的。因此，预计它会比纯粹运行在 JVM 上的 Cassandra 更快。

然而，在 Scylla 中有许多重要的低级优化，这使它比它的竞争对手更好。

### 无共享方法

Cassandra 依靠线程实现并行。问题是线程需要上下文切换，这很慢。

此外，对于线程间的通信，您需要锁定共享内存，这又会导致处理时间的浪费。

ScyllaDB 使用 [seastar](http://www.seastar-project.org/) 框架来分割每个内核上的请求。应用程序每个内核只有一个线程。这样，如果内核 1 正在处理一个会话，并且该会话的查询请求到达内核 2，它将被定向到内核 1 进行处理。之后，任何内核都可以处理响应。

无共享方法的优点是每个线程都有自己的内存、cpu 和 NIC 缓冲队列。

在无法避免内核间通信的情况下，Seastar 提供了高度可扩展的异步无锁内核间通信。这些无锁原语包括 Futures 和 Promises，它们在编程中非常常用，因此对开发人员非常友好。

### 避免内核

当在表中找到一行时，需要通过网络将它发送给客户机。这包括将数据从用户空间复制到内核空间。

然而，Linux 内核通常执行不可扩展的多线程锁定操作。

ScyllaDB 通过使用 Seastar 的网络堆栈来处理这个问题。

Seastar 的网络堆栈运行在用户空间中，并利用 [DPDK](https://dpdk.org/) 进行更快的数据包处理。DPDK 绕过内核将数据直接复制到 NIC 缓冲区，并在 80 个 CPU 周期内处理一个数据包。(来源: [DPDK 网站](https://dpdk.org/))

### 不要依赖页面缓存

当您有顺序 I/O 并且数据以有线格式存储在磁盘中时，页面缓存非常有用。

然而，在 Scylla/Cassandra 中，我们有表格形式的数据。页面缓存以相同的格式存储数据，这对于小数据来说占用了很大一块内存，当你要传输它的时候需要序列化/反序列化。

ScyllaDB 不依赖页面缓存，而是将其大部分内存分配给行缓存。

行缓存以优化的内存格式存储数据，占用的空间更少，并且不需要序列化/反序列化

使用行缓存的另一个优点是，在页面缓存被反复使用的情况下进行压缩时，行缓存不会被删除。

这些是 ScyllaDB 中的主要优化，使它比 Cassandra 更快、更可靠、更便宜。Scylla 还有许多其他优化，可以在这里找到。

***如果你对以上更多的设计感兴趣或者想与我取得联系，请在 [LinkedIn](http://www.linkedin.com/in/kartik-khare) 或[脸书](https://www.facebook.com/KK.corps)上联系我，或者发邮件到[kharekartik@gmail.com](mailto:kharekartik@gmail.com)***