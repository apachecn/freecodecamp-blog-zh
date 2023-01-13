# java 中的异步 IO 与异步请求处理

> 原文：<https://www.freecodecamp.org/news/java-async-io-async-request-processing-in-http-request-1a04f395d8c7/>

在本文中，我试图解释 Java 世界中 HTTP 请求的异步 IO 和异步请求处理之间的区别。

在 Java ***1.4*** 之前的世界里，Java 提供了通过网络套接字发送/接收数据的 API。JVM 的最初作者将这种 API 行为映射到 OS socket API，几乎是一对一的。

那么，什么是操作系统套接字行为呢？OS 提供 [Socket 编程 api](https://www.csd.uoc.gr/~hy556/material/tutorials/cs556-3rd-tutorial.pdf) ，有发送/接收**阻塞调用**。由于 java 只是一个运行在 linux(OS)之上的进程，因此这个 java 程序必须使用 OS 提供的这个阻塞 api。

世界很高兴，java 开发人员开始使用 API 来发送/接收数据。但是他们必须为每个套接字(客户机)保留一个 java 线程。

每个人都在编写自己风格的 HTTP 服务器。代码共享变得越来越困难，java 世界需要标准化。
**进入 java servlet 规范。**

在继续之前，让我们定义几个术语:

**Java 服务器开发者**:正在使用 java socket api，实现类似 tomcat 的 http 协议的人。

java 应用程序开发人员:在 tomcat 上构建商业应用程序的人。

现在回来

一旦 java servlet 规范问世，它就说:

*亲爱的 java 服务器开发人员，*请提供如下方法:

```
doGet(inputReq, OutPutRes)
```

这样 *java 应用程序开发人员*就可以实现`doGet`并编写他们的业务逻辑。一旦*【应用开发者】*想要发送`response`，他可以调用`OutPutRes.write().`

**需要注意的一点:**由于 socket api 是阻塞的，因此 OutPutRes.write()也是阻塞的。另外，附加的限制是响应对象是在 **doGet 方法**退出时提交的。

由于这些限制，人们不得不使用一个线程来处理一个请求。

时间流逝，互联网接管了世界。 [*每个请求一个线程*](https://stackoverflow.com/questions/15217524/what-is-the-difference-between-thread-per-connection-vs-thread-per-request) 开始显示出局限性。

### 问题 1:

当在每个请求的处理过程中有长时间的暂停时,[每请求线程](https://stackoverflow.com/questions/15217524/what-is-the-difference-between-thread-per-connection-vs-thread-per-request)模型就会失败。

> 例如:从子服务获取数据需要很长时间。

在这种情况下，线程大部分处于空闲状态，JVM 很容易耗尽线程。

### 问题二:

对于 http1.1 持久连接，情况变得更糟。与持久连接一样，底层的 TCP 连接将保持活动状态，服务器必须为每个连接阻塞*一个线程。*

> *但是为什么服务器必须为每个连接阻塞*一个线程？
> *由于 OS 提供了阻塞套接字 Recv api，jvm 必须调用 OS 阻塞 Recv 方法，以便监听来自客户端的同一 tcp 连接上的更多请求。*

### 世界需要一个解决方案！

第一个解决方案来自 JVM 的创建者。他们推出了 [NIO( **ASYNC-IO** )](https://en.wikipedia.org/wiki/Non-blocking_I/O_(Java)) 。Nio 是通过套接字发送/接收数据的非阻塞 API。

**一些背景:****OS 连同阻塞套接字 api 还提供了非阻塞版本的套接字 api。**

**但是操作系统是如何做到这一点的..它是否在内部分叉了一个线程，而那个线程被阻塞了？？？**

**答案是否定的……当有数据要读取或写入时，操作系统会指示硬件中断。**

**NIO 允许 *" **java 服务器开发者"*** 解决**问题 2** 阻塞*每个 TCP 连接*一个线程。由于 NIO 是一个 HTTP 持久连接，线程不需要它阻塞 *recv* 调用。相反，它现在只能在有数据要处理时才处理它。这允许一个线程监控/处理大量的持久连接。**

**第二个解决方案来自 servlet 规范。Servlet 规范得到了升级，他们引入了 [**异步支持**](https://docs.oracle.com/javaee/7/tutorial/servlets012.htm) **(异步请求处理)。****

```
`AsyncContext acontext = req.startAsync();`
```

> *****重要:*** *本次升级移除了 **doGet** 方法完成时提交响应对象的限制。***

**这使得“***”Java 应用程序开发人员“*** ”通过将工作卸载到后台线程来处理**问题 1、**。现在，线程可以用来处理其他请求，而不是在长暂停期间让线程等待。**

### **结论:**

***Java 中的 Async-IO* 基本都是使用 OS socket API 上的非阻塞版本。**

***异步请求处理*基本上是如何用一个线程处理更多请求的 servlet 规范标准化。**

### **参考资料:**

**[https://www.scottklement.com/rpg/socktut/tutorial.pd](https://www.scottklement.com/rpg/socktut/tutorial.pdf)f
https://stack overflow . com/questions/15217524/每连接线程与每请求线程的区别是什么**

> ***文章动机:团队学习/知识分享***