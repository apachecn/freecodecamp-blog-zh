# JUnit 规则简化了 AppEngine 单元测试

> 原文：<https://www.freecodecamp.org/news/appengine-unit-testing-made-easy-with-junit-rules-97c2127a161a/>

作者:拉梅什·林加帕

# JUnit 规则简化了 AppEngine 单元测试

![1*V2EDFPlZSdQQSVyKn6I68w](img/3763e408a7da7faf6bc004129b17db53.png)

Image Source - Code Affine

你好。如果您使用 AppEngine 托管您的应用程序，那么您将使用一个或多个他们的服务，如 Datastore、Memcache、TaskQueues、UserService 等。

您将需要编写单元测试，以确保您的应用程序功能与这些服务一起正常工作。为此，您需要设置一些配置，以便在您的本地环境中测试这些服务。

如果你是 AppEngine 服务测试的新手，Google 有很好的文档说明如何用示例代码设置这个配置。看看【Java 的本地单元测试。

例如，下面是 Google 提供的用于执行数据存储测试的示例代码:

这太棒了。大多数时候，我们的服务将利用多个 AppEngine 服务，如实体的 Memcache(Objectify)，在保存实体后对任务进行排队。当我们试图设置多个配置时，真正的难点就来了。

好吧，这就是大量的配置，但是你猜怎么着，还没完呢。如果您需要更改某些测试的特定设置，如*高复制*、*队列 XML 路径*或*当前用户*，该怎么办？那么你的设置将会更加复杂。并且您需要为您的每个单元测试类重复所有这些。

哦，顺便问一下，你忘了[物化](https://github.com/objectify/objectify)测试设置了吗？看看[对象化单元测试](https://stackoverflow.com/questions/32628124/how-to-use-objectifyservice-in-junit-testing)。

你可能会想，

> “让我们在父类中设置所有这些配置，每个单元测试类都将扩展这个配置”

我们可以这样做，但还有另一个问题:

创建这些服务的成本都很高。您需要只使用您需要的服务来配置环境*。*

因此不必要地配置所有服务可能会降低测试执行时间。

#### 好吧，有救援人员吗？

是的， [Junit Rules](https://github.com/junit-team/junit4/wiki/rules) 前来救援！

> 规则允许非常灵活地添加或重新定义测试类中每个测试方法的行为。测试人员可以重用或扩展下面提供的规则之一，或者编写他们自己的”

有很多关于 Junit 规则的好文章——请去看看。

因此，使用 JUnit 规则，我们能够以一种简单的方式在测试执行之前设置外部依赖。通过一些调整，我们可以按照我们想要的方式为每个类设置 AppEngine 相关的服务。以下是一个示例:

这里的 **AppEngineRule** 是一个简单的类，它扩展了 **ExternalResource** ，并且是使用构建器模式创建的。我们可以设置我们需要的服务，如果您在 ***SampleTestClass*** 中看到，我们可以在一行中完成所有设置。

```
@Rule    public final AppEngineRule rule = AppEngineRule.builder()             .withDatastore()             .withQueue()             .build();
```

并且 *AppEngineRule* 类覆盖`before`和`after`方法来设置 AppEngine 设置和拆卸功能。您还可以为每个测试类配置类似的服务。

#### 就这些吗？我们能做得更好吗？

我们当然可以！为了使这个设置简单，你需要用所有的样板设置代码编写类似于 *AppEngineRule* 类的东西。

这里有一个好消息:你不需要写任何。我做了一个小库，里面有所有服务的所有必要的设置实现，甚至还有更多可配置的选项。

[**Ramesh-dev/gae-test-util-Java**](https://github.com/ramesh-dev/gae-test-util-java)
[*Google app engine Java 实用程序库。创建一个帐户，为 ramesh-dev/gae-test-util-java 开发做贡献……*github.com](https://github.com/ramesh-dev/gae-test-util-java)

在您的构建脚本中，只需添加依赖项，例如在 gradle:

```
testCompile 'com.github.ramesh-dev:gae-test-util-java:0.3'
```

这样，我们就可以拥有如下灵活的配置:

![1*PEm-JvZu6NKFKq_AY7VoPA](img/9999814507c750d8eccda77b67a58191.png)

Image downloaded from the internet

### 结论

因此，Junit Rules 是一个方便的选项，可以轻松地配置我们的外部依赖项，即使是像 AppEngine 服务这样复杂的依赖项。您还可以拥有其他规则，并使用[规则链](https://github.com/junit-team/junit4/wiki/rules#rulechain)以特定的顺序将它们链接起来

感谢您的阅读和这段时间，并**测试愉快……**

#### 参考

*   [https://github.com/junit-team/junit4/wiki/rules](https://github.com/junit-team/junit4/wiki/rules)
*   [https://cloud . Google . com/app engine/docs/standard/Java/tools/localunittesting](https://cloud.google.com/appengine/docs/standard/java/tools/localunittesting)
*   [https://github.com/ramesh-dev/gae-test-util-java](https://github.com/ramesh-dev/gae-test-util-java)