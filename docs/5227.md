# 当今最快的 Java 框架 Vert.x 简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-vert-x-the-fastest-java-framework-today-27d8661ceb14/>

马丁·布迪

# 当今最快的 Java 框架 Vert.x 简介

![1*RovqxSyUULpHDMxwYGXQPA](img/33a6fb5a796c1a73e715c14be55ca04e.png)

如果你最近在谷歌上搜索了“最佳 web 框架”,你可能会偶然发现 Techempower 基准测试，其中有超过 300 个框架被排名。在那里，你可能已经注意到 Vert.x 是排名最高的之一，如果不是按照某些标准排名第一的话。

所以就来说说吧。

Vert.x 是一个多语言的 web 框架，在其支持的语言 Java、Kotlin、Scala、Ruby 和 Javascript 之间共享通用功能。不管什么语言，Vert.x 都是在 Java 虚拟机(JVM)上运行的。由于模块化和轻量级，它面向微服务开发。

Techempower 基准测试测量从数据库更新、获取和交付数据的性能。每秒处理的请求越多越好。在这种几乎不涉及计算的 IO 场景中，任何非阻塞框架都有优势。近年来，这样的范式几乎与 Node.js 密不可分，node . js 以其单线程事件循环推广了它。

Vert.x 和 Node 一样，运行单个事件循环。但是 Vert.x 也利用了 JVM。Node 运行在一个内核上，而 Vert.x 维护一个线程池，其大小可以匹配可用内核的数量。凭借更强的并发支持，Vert.x 不仅适用于 IO，还适用于需要并行计算的 CPU 密集型进程。

然而，事件循环只是故事的一半。另一半和 Vert.x 关系不大。

要连接到数据库，客户端需要连接器驱动程序。在 Java 领域，最常见的 Sql 驱动程序是 JDBC。问题是，这个司机挡住了。它在套接字级别阻塞。一个线程将一直卡在那里，直到它返回一个响应。

不用说，驱动程序一直是实现完全非阻塞应用程序的瓶颈。幸运的是，在[上已经有了一些进展(尽管是非官方的),一个带有几个活动分支的异步驱动](https://github.com/mauricio/postgresql-async),其中包括:

*   [https://github.com/jasync-sql/jasync-sql](https://github.com/jasync-sql/jasync-sql)(针对 Postgres 和 MySql)
*   https://github.com/reactiverse/reactive-pg-client(Postgres)

#### **黄金法则**

使用 Vert.x 非常简单，只需几行代码就可以启动 http 服务器。