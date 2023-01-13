# 我最喜欢的 Java 调试技术

> 原文：<https://www.freecodecamp.org/news/how-to-debug-java-code-4a28442e0959/>

这篇文章是关于我用来调试各种代码库的技术，比如:

1.  具有高并发性的代码库。
2.  包含许多专有(不支持)库的代码库。
3.  包含大量不推荐使用/不需要的代码的基本代码。
4.  内存泄漏的代码库。
5.  每个 JVM 都可以与其他 JVM 对话的代码库。

所以我们一个一个来看。

#### **具有高并发性的代码库。**

为了服务一个请求，JVM 可能会使用许多线程，例如:

```
Req -> tomcatThread-1 -> executorThread-2 -> BizThread-3->…`
```

比方说，我们发现 BizThread-3 中出现了异常。现在作为一个调试器，我们想要理解请求流。但是 stacktrace 将无法提供完整的请求流(例如在 **executorThread-2** 中发生了什么，在 **tomcatThread-1** 中发生了什么等等)。

**技巧 1.1:** 编写一个 [**自定义 java 代理**](https://www.baeldung.com/java-instrumentation) ，它将被用来有效地将`log.debug()`添加到某些 java 包的每个方法的开始和结束。这将让我们对所有被称为什么有一些了解。

**技巧 1.2:** 在某些框架中，如果支持，使用 [**AOP**](https://www.journaldev.com/2583/spring-aop-example-tutorial-aspect-advice-pointcut-joinpoint-annotations) 代理所有方法并有效添加`log.debug()`。

#### 拥有大量专有(不受支持)库的代码库。

有时我们发现自己处于这样一种情况，经过几个小时的调试，我们终于解决了这个问题:XYZ-gov-secret 库行为不正常，而且这个库现在不受支持。

**技巧 2.1:** 卷起袖子安装[**eclipse-decompiler**](https://marketplace.eclipse.org/content/enhanced-class-decompiler)**潜入代码库。**

#### ****包含大量不推荐使用/不需要的代码的代码库。****

**这是一个经典的例子:我们有时发现自己处在一个有 500 多行代码的方法中，其中包含大量不推荐使用的 if-else。现在，我们如何弄清楚特定调用的代码流是什么，if-else 将使用哪些代码，以及哪些是死代码？**

****手法 3.1:** 我们可以用一个叫 [**jacoco 代理**](https://www.eclemma.org/jacoco/trunk/doc/agent.html) 的工具。它在运行时收集执行细节，并可以在 eclipse 中对代码进行颜色编码。基本上，它是同一个工具，通常用于 JUnit Test 分析代码覆盖率。**

#### ****内存泄漏的代码库。****

**每个开发人员都有这样的一天，在他们的本地系统中，所有的事情都很顺利，没有内存:(**

*****技术 4.1:*** JVM 提供了在内存不足的情况下捕获堆转储的技术。**

**在启动 JVM
[*-XX:+heapdumponotemoryerror*](https://docs.oracle.com/javase/7/docs/webnotes/tsg/TSG-VM/html/clopts.html)*时添加以下内容作为参数。这将捕获堆转储并将其放入一个文件中，该文件可用于分析是什么在消耗内存。***

****技巧 4.2:** 你也可以使用[jProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html)/[jvvisualvm](https://visualvm.github.io/)来获取正在运行的 JVM 的堆转储/线程转储。**

#### **每个 JVM 都可以与其他 JVM 对话的代码库。**

**当您被扔进一个意大利面条式的分布式环境时，跟踪请求流就变得很困难。**

****技巧 5.1:** 可以使用 [**Wireshark**](https://www.wireshark.org/) 这样的工具。Wireshark 捕获网络数据，并在一个漂亮的 UI 中显示出来。然后，您可以查看流经系统的 HTTP 请求/响应**

#### ****优秀奖****

****技巧 6.1:** 在单线程环境下，为了快速了解 stacktrace，有意插入
`try catch`。**

```
`try {
	throw new RuntimeException(); 
} catch(Exception e){
  e.printStackTrace();
}`
```

****技巧 6.2:** 使用 eclipse 断点或者使用条件断点。**

****手法 6.3:**[https://en.wikipedia.org/wiki/Rubber_duck_debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging)**

> **文章动机:团队学习/知识共享。**