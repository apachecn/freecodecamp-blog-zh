# 如何在春天设置 Vertx

> 原文：<https://www.freecodecamp.org/news/vertx-in-spring-39c2dd7bc2a9/>

作者里克·李

# 如何在春天设置 Vertx

![1*zOLXCXhrX_NgyWJf3KGmrQ](img/af30b406c27a8efed6f8818893674e0f.png)

Spring 可能是 Java 领域最流行的框架。我们都喜欢它的依赖注入和所有自动连线/配置的魔力。这使得单元测试变得轻而易举。

另一方面， [Vertx.io](https://vertx.io/) 是一个较新的工具包/框架，近年来越来越受欢迎。它是轻量级的，支持通过事件循环(如 Node.js)和 eventbus 消息(如 Akka)进行完全异步的编程。此外，社区开发了相当多的异步工具/数据库客户端，如 [Async MySQL / PostgreSQL 客户端](https://vertx.io/docs/vertx-mysql-postgresql-client/java/)，这使得它成为除 Spring 之外的另一个流行选择。

对于新项目来说，似乎很难在 Vertx 和 Spring 之间做出选择，但好消息是它们确实不是互斥的！下面是一个简单的例子来说明设置。

这个示例项目是关于在 Springboot 应用程序中部署一个 Vertical。Vertical 提供了一个使用异步 MySQL 客户端查询 MySQL 的函数。该函数可以直接调用，也可以通过 vertx.eventbus 调用。

首先，创建一个简单的 maven Springboot 应用程序。你可以通过 [Spring Initializer](https://start.spring.io/) 来创建。然后将以下内容添加到 pom.xml 中:

由于我们将使用[异步 mysql / PostgreSQL 客户端](https://vertx.io/docs/vertx-mysql-postgresql-client/java/)查询 MySQL，因此创建了一个非常原始的 MysqlClient.java，并将 MySQL 配置放在 application.yaml 上

创建一个包含两个字段的虚拟用户表，并插入一些数据:

或者，创建一个用于访问用户表的存储库类:

现在我们可以创建 vertical，它有一个处理 MySQL 查询的方法。

最后，创建 Spring 应用程序并添加一个带有@PostConstruct 注释的 deployVerticle 方法。

如果您运行 Spring 应用程序，您将看到下面的系统打印输出“dbVerticle deployed ”,这意味着 Verticle 正在 Spring 应用程序上运行。

```
2019-02-11 08:56:27.110  INFO 29444 --- [ntloop-thread-0] i.v.ext.asyncsql.impl.MYSQLClientImpl    : Creating configuration for localhost:33062019-02-11 08:56:27.442  INFO 29444 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'dbVerticle deployed2019-02-11 08:56:27.848  INFO 29444 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''2019-02-11 08:56:27.853  INFO 29444 --- [           main] n.r.s.SpringVertxExampleApplication      : Started SpringVertxExampleApplication in 5.393 seconds (JVM running for 6.671)
```

为了测试它，我们可以简单地在部署 Verticle 之后添加一个 db 查询请求。

控制台打印出以下内容:

```
dbVerticle deployedsuccess[{"id":10466,"username":"ricklee"}][{"id":10468,"username":"maryjohnson"}]
```

这个例子展示了如何通过一个简单的设置享受 Spring 和 Vertx 世界的设施。

这里的源代码:[https://github.com/rickcodetalk/spring-vertx-example](https://github.com/rickcodetalk/spring-vertx-example)