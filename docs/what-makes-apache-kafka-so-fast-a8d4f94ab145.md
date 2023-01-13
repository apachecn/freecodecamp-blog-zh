# 这就是阿帕奇卡夫卡如此快速的原因

> 原文：<https://www.freecodecamp.org/news/what-makes-apache-kafka-so-fast-a8d4f94ab145/>

作者:卡蒂克·哈雷

# 这就是阿帕奇卡夫卡如此快速的原因

![u8GxQv4ndyG-qVyXgfD18hkDdqKi7rKI0ymG](img/42808d0be37e6157b2a7e5ee66ac60b4.png)

BigData By Camelia.boban (Own work) [CC BY-SA 3.0 ([https://creativecommons.org/licenses/by-sa/3.0](https://creativecommons.org/licenses/by-sa/3.0))], via Wikimedia Commons

#### 什么是阿帕奇卡夫卡？

[Apache Kafka](https://kafka.apache.org/) 是一个分布式流媒体平台，允许您:

*   发布和订阅记录流，类似于消息队列或企业消息传递系统。
*   以容错的持久方式存储记录流。
*   在记录流出现时处理它们。

如果你以前没有使用过 Kafka，你可以点击这里的[快速入门](https://kafka.apache.org/quickstart)，一旦你熟悉了这个用例，再回到本文。

Kafka 支持一个高吞吐量、高度分布式、容错的平台，具有低延迟的消息传递。这里，我们将重点关注低延迟交付方面。

#### I/O =文件系统的低延迟？

大多数传统数据系统使用随机存取存储器(RAM)作为数据存储，因为 RAM 提供极低的延迟。

虽然这种方法使它们更快，但 RAM 的成本比磁盘高得多。当您有数以百计的 GBPS 数据流经系统时，此类系统的运行成本通常会更高。

Kafka 依赖文件系统进行存储和缓存。问题是磁盘比内存慢。这是因为与实际读取数据所需的时间相比，通过磁盘的寻道时间很长。

但是，如果您可以避免寻道，那么在某些情况下，您可以实现与 RAM 一样低的延迟。这是卡夫卡通过**顺序 I/O** 完成的。

顺序 I/O 的一个优点是，无需在应用程序中为其编写任何逻辑就可以获得缓存。现代操作系统将大部分空闲内存分配给磁盘缓存。因此，如果您以有序的方式读取，操作系统总是可以预读，并在每次磁盘读取时将数据存储在缓存中。

这比在 JVM 应用程序中维护缓存好得多。这是因为 JVM 对象很“重”，会导致高垃圾收集，随着数据大小的增加，这种情况会变得更糟。

#### 不要用树

大多数现代数据库使用一种形式的[树数据结构](https://medium.freecodecamp.org/all-you-need-to-know-about-tree-data-structures-bceacb85490c)来进行持久数据存储。例如，MongoDB 使用 [BTree](https://en.wikipedia.org/wiki/B-tree) ，而 Cassandra 使用 [LSM 树](https://en.wikipedia.org/wiki/Log-structured_merge-tree)。

这些结构提供了 O(log N)的搜索性能。

对于需要同时执行许多读写操作的消息传递系统，使用树会导致大量随机 I/O。这会导致大量磁盘寻道，这对性能是灾难性的。

对于消息传递系统来说，[队列](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))是更好的数据结构。大多数情况下，数据被附加到系统中，读取很简单。所有这样的操作都是 O(1) —这是一个更高的性能。

#### 不要抄袭！

数据处理系统的主要低效之一是传输期间数据的[串行化和解串行化](https://en.wikipedia.org/wiki/Serialization)(翻译成适合存储和传输的格式)。

通过使用更好的二进制数据格式，如协议缓冲区或平面缓冲区，而不是 JSON，可以使速度更快。

但是如何才能完全避免序列化/反序列化呢？

卡夫卡用两种方式解决这个问题:

1.  为[生产者、经纪人和消费者](https://kafka.apache.org/documentation/#api)使用标准化的二进制数据格式(这样数据可以不加修改地传递)
2.  不要拷贝应用程序中的数据(“零拷贝”)

第一个是不言自明的。这是第二个需要注意的问题。

从文件到套接字的常见数据传输可能如下所示:

1.  操作系统将数据从磁盘读入内核空间的页面缓存
2.  应用程序将数据从内核空间读入用户空间缓冲区
3.  应用程序将数据写回到内核空间的套接字缓冲区中
4.  操作系统将数据从套接字缓冲区复制到网卡缓冲区，在那里通过网络发送数据

但是，如果我们有相同的标准化格式的数据，不需要修改，那么我们就不需要第 2 步(将数据从内核空间复制到用户空间)。

如果我们保持数据的格式与通过网络发送的格式相同，那么我们可以直接将数据从 pagecache 复制到 NIC 缓冲区。这可以通过操作系统[发送文件系统调用](http://man7.org/linux/man-pages/man2/sendfile.2.html)来完成。

关于零拷贝方法的更多细节可以在这篇文章中找到。

#### 还有什么？

除了上面提到的技术之外，Kafka 还使用了许多其他技术来提高系统的速度和效率:

1.  批处理数据以减少网络调用，并将大量随机写入转换为顺序写入。
2.  使用 [LZ4](https://en.wikipedia.org/wiki/LZ4_(compression_algorithm)) 、 [SNAPPY](https://en.wikipedia.org/wiki/Snappy_(compression)) 或 [GZIP](https://en.wikipedia.org/wiki/Gzip) 编解码器压缩批量(而非单个消息)。一个批处理中的许多消息数据是一致的(例如，消息字段和元数据信息)。这可以带来更好的压缩比。

想了解更多卡夫卡的设计，可以参考他们的[官方文章](https://kafka.apache.org/0102/documentation.html#majordesignelements)。

值得注意的是，上述所有技术都可以应用在大多数系统中，以实现低延迟。它们不涉及干预内核、调整垃圾收集、使用本机应用程序或使用极端的数据结构。

但是，有一个缺点是，这些技术中有一些是专门针对类似于消息传递平台的情况的。它们不适合更通用的分布式数据库。

***如果你对更多这样的设计感兴趣或者有任何机会，请在 [LinkedIn](http://www.linkedin.com/in/kartik-khare) 或[脸书](https://www.facebook.com/KK.corps)上联系我，或者发邮件到[kharekartik@gmail.com](mailto:kharekartik@gmail.com)***