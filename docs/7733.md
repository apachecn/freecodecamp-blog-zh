# 超越 Android:kot Lin 如何在后端工作

> 原文：<https://www.freecodecamp.org/news/going-beyond-android-kotlin-on-the-backend-2a75eef2582b/>

亚当·阿罗德

# 超越 Android:kot Lin 如何在后端工作

![1*6K5vmzalJUxn44v3cm6wBw](img/514ce1fcb911035058543d9d42481bae.png)

本文是[系列](http://the-cogitator.com/2017/12/21/beyond-android-exploring-kotlin-areas-of-application.html)的一部分。

虽然大多数开发人员在 Android 上使用 Kotlin，但它在其他平台上也是一个可行的选择。在本文中，我们将看看它在后端是如何工作的。

正如我在之前所写的，我认为 Java 和 Kotlin 之间的互操作是非常无缝的。这也意味着在后端使用 Kotlin 代替 Java 相当容易。除了一些麻烦之外，您几乎可以开始在 Java 项目中用 Kotlin 编写新特性了。或者如果您只是想尝试一下，您可以从用它编写测试开始。

如果你环顾四周，似乎在后端市场占有很大份额的公司也有同样的想法:新版本的 Spring 有一些专门针对 Kotlin 的特性[，你甚至可以使用 Kotlin 通过](https://tech.io/playgrounds/8594/spring-5---dedicated-kotlin-features) [kotlin-dsl](https://github.com/gradle/kotlin-dsl) 来编写你的 Gradle 脚本。

这里值得注意的是**你不需要 Kotlin 对这些库**的支持，因为 Kotlin 的 Java 互操作特性非常好。

### 决定

当您开始在后端使用 Kotlin 时，您有几个选择。

如果您选择使用用 Kotlin 编写的库，您将获得一些主要优势:因为所有内容都是用 Kotlin 编写的，所以不会有与您使用的语言相关的兼容性问题。另一件值得一提的事情是，有些东西只存在于 Kotlin 中，比如协程和具体化的泛型。

权衡的结果是，这些库中的大部分都不是很老，可能会有新项目的典型问题:缺少文档和最终的 bug 或设计问题。

如果你需要一些久经考验的东西，使用像 [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/) 或 [Mockito](http://site.mockito.org/) 这样的工具是不会出错的。你得到的是，大多数问题都有很好的记录，你会很快得到你的问题的答案。然而，你失去的是 Kotlin 的一些优点:它们没有 [Kotlin DSL](https://kotlinlang.org/docs/reference/type-safe-builders.html) s，并且代码将相当像 Java。

另一个选择是选择一个对 Kotlin 有内置支持的库。RxKotlin 和 [vert.x](https://github.com/vert-x3/vertx-lang-kotlin) 就是很好的例子。从现在开始，我将把这些项目叫做*混合动力车*。有了它们，你可以两全其美:你通常会有一个好的 API，从 Kotlin 的角度来看，它是习惯性的，而在它的背后，会有一些众所周知的、久经沙场的东西。

让我们看看你可能想尝试的一些图书馆。

请注意，以下只是我的观点，因此它们是主观的。

#### Ktor

[**Ktor**](http://ktor.io/) 是用 Kotlin 编写的较新的 web 框架之一。它带有嵌入式 [Netty](http://netty.io/wiki/user-guide-for-5.x.html) 和一个漂亮的 DSL 启动。有趣的是，它利用了 Kotlin 的[协程](https://kotlinlang.org/docs/reference/coroutines.html)支持。实际情况是这样的:

当我尝试时，我担心文档很缺乏。当你碰到一个问题时，你更容易被卡住。它也[表现不好](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext)，与 Java 的互操作有点粗略。

#### 标枪

Javalin 是另一个 web 框架，它有一个非常简单、流畅和可读的 API。它还可以与 Netty 或 Undertow 等多个嵌入式 web 服务器一起工作。我最喜欢的是它在 [Spark](http://sparkjava.com/) (不要与 [Apache Spark](https://spark.apache.org/) 混淆)的极简主义方法和 [vert.x](http://vertx.io/) 的低级本质之间取得了平衡。[文档](https://javalin.io/documentation)也非常好，因此您可以立即开始使用:

我喜欢这个 API 比 Ktor 的更像 Java，所以如果你有 Java 背景，可能会更容易上手。

#### 六边形

对于编写 web 应用程序来说，六边形是一个有趣的选择。名称的选择不是随意的:它鼓励使用[六边形架构](http://alistair.cockburn.us/Hexagonal+architecture)(更普遍的说法是[干净架构](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html))，而且它也比 ktor 甚至 Spring 框架[更具性能](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext)！你得到的 DSL 也是相当描述性的:

#### 离开网络

值得注意的是，有很多用 Kotlin 编写的工具可以选择。Kotlin 有一个非常有用的[规范框架](https://github.com/spekframework/spek)。如果你不喜欢没有好的 Java HTTP 客户端这个事实，你现在可以利用 [Fuel](https://github.com/kittinunf/Fuel) ，它是一个非常方便的工具，可以与 REST 端点以及其他端点进行接口。你甚至有用 Kotlin 编写游戏的工具。

### Java 框架

#### 垂直 x

[vert.x](http://vertx.io/) 是一个类似于 [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/) 的多模块 web 框架。这里需要注意的是，有一个专门针对科特林的[文档部分](http://vertx.io/docs/vertx-core/kotlin/)。如果性能对你来说至关重要，那么 vert.x 可能是你编写 web 应用程序的选择:vert.x 有[非常好的基准分数](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext)。这并不奇怪，因为它实现了[多反应器模式](http://vertx.io/docs/vertx-core/java/#_reactor_and_multi_reactor)(反应器模式对您来说可能是熟悉的，来自 [node.js](https://www.packtpub.com/mapt/book/web_development/9781783287314/1/ch01lvl1sec09/the-reactor-pattern) )。

值得注意的是，vert.x 有非常好的 [Kotlin 示例](https://github.com/vert-x3/vertx-examples/tree/master/kotlin-examples)。您会需要它们，因为它有一些其他地方没有的概念，设置起来也有点复杂:

#### 春天

对于许多 Java 开发人员来说， [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/) 是编写 Java 应用程序的事实上的工具。好消息是自从 *5.0* Spring 有了[对 Kotlin 的内置支持](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0)。有大量的 Spring 项目供你选择。如果你感兴趣，有一个简单的[教程](https://kotlinlang.org/docs/tutorials/spring-boot-restful.html)，它将带你开始使用 Kotlin 的 Spring。以下是 Hello World 的外观:

#### Sparkjava

Sparkjava 是一个极简(微)web 框架。你可以几乎零时间投入就开始使用它，如果你来自 node.js，它也是一个非常好的选择。你也不能得到比这更少的了:

### 混合期权

虽然与 Java 工具的接口通常非常方便，但上面的一些工具具有为 Kotlin 编写的工具支持:

[**Spark-kotlin**](https://github.com/perwendel/spark-kotlin) 为 kotlin 用户提供了更加精简的体验，另外值得注意的是，它是由 Sparkjava 的原作者 [perwendel](https://github.com/perwendel) 撰写的。

[**vertx-lang-kotlin**](https://github.com/vert-x3/vertx-lang-kotlin)为 vert.x 提供了有用的 kot Lin 特定选项，如协程、一次性工作器或反应流。

虽然使用 Kotlin 的 Mockito 非常令人愉快，但[**mock ITO-kot Lin**](https://github.com/nhaarman/mockito-kotlin)通过给你一个漂亮的 DSL 并处理一些问题来改进这一点。

[**Hamkrest**](https://github.com/npryce/hamkrest) 是 [Hamcrest](http://hamcrest.org/) 的重新实现，具有语法糖、可扩展性等。

[**RxKotlin**](https://github.com/ReactiveX/RxKotlin) 为 [RxJava](https://github.com/ReactiveX/RxJava) 增加了方便的[扩展函数](https://kotlinlang.org/docs/reference/extensions.html)，SAM helpers 等。唯一的问题是文档在[付费墙](https://www.packtpub.com/application-development/learning-rxjava)后面。

### 一个工作实例

现在我们来看一个分步示例。我们将使用带有 [Spring Initializr](https://start.spring.io/) 的 Spring Boot。

本教程的源代码可以在[这里](https://github.com/AppCraft-Projects/spring-boot-kotlin-demo)找到。

### 入门指南

Spring Boot 附带了 [Spring Initializr](https://start.spring.io/) ，这是一个方便的工具，你可以用它快速启动你的项目。对于这个例子，我推荐选择带有 *Kotlin* 和 Spring Boot *2.0.0* 的 *Gradle 项目*，因为使用 **2.0.0** 你将获得 *Spring 5.0* 的特性。

如果你点击“切换到完整版本”,你也可以拼凑出一个精细的框架。有丰富的主题，你可以从中挑选工具，从 Web 到 AWS 等等。在这个练习中，我只选择了 Web。

设置好 Initializr 后，点击“生成项目”并在 IDE 中打开它。您可能会注意到，与一个简单的 Java 项目相比，您不必向特定于 Kotlin 的`build.gradle`添加太多内容。我在这个例子中提取了它们:

### 添加简单的控制器

如果您查看应用程序的入口点，它相当简单:

现在，对于一个工作的 Hello World，我们唯一需要做的就是向我们的项目添加一个`RestController`:

然后**砰！**大功告成！使用`./gradlew bootRun`启动后，您可以在`[http://localhost:8080/](http://localhost:8080/:)`查看结果:

你可以在本教程中了解更多关于这是如何工作的[。或者如果你想看看 Kotlin APIs 和函数方式是什么样子，这里有一个不错的教程](https://spring.io/guides/gs/spring-boot/)。

### 包扎

在本文中，我们探索了一些更广为人知的使用 Kotlin 进行后端开发的选项。我们也看到市场上的知名参与者已经*采用了 Kotlin，*与纯 Java 相比，后端开发可以简单得多。

在下一篇文章中，我们将探索如何在您的项目中使用 Kotlin 来代替 Javascript！

*感谢阅读！你可以在我的博客上阅读更多我的文章。*