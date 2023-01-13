# Akka HTTP 路由简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-akka-http-routing-697b00399cad/>

米格尔·洛佩兹

# Akka HTTP 路由简介

![cMwTDW6UpogFvWV8ay5FbcQkZfnj5AsXWn4d](img/5df172bc12993eb494b902c8e17b3b7e.png)

Akka HTTP 的路由 DSL 初看起来可能很复杂，但是一旦你掌握了它的窍门，你就会发现它有多强大。

在本教程中，我们将重点放在创建路线及其结构。我们不会讨论 JSON 的双向解析，我们有其他教程讨论这个主题。

### 什么是指令？

在学习服务器端 Akka HTTP(也有客户端库)时，我们会发现的第一个概念是*指令*。

那么，它们是什么？

你可以把它们想象成积木，或者乐高积木，你可以用它们来构建你的路线。它们是可组合的，这意味着我们可以在其他指令之上创建指令。

如果你想要更深入的阅读，可以随意查阅 [Akka HTTP 的官方文档](https://doc.akka.io/docs/akka-http/current/routing-dsl/directives/index.html)。

在继续之前，让我们讨论一下我们将构建什么。

### 类似博客的 API

我们将为博客创建一个面向公众的 API 示例，在这里我们将允许用户:

*   查询教程列表
*   按 ID 查询单个教程
*   查询教程中的注释列表
*   向教程添加注释

终点将是:

```
- List all tutorials GET /tutorials 
```

```
- Create a tutorial GET /tutorials/:id 
```

```
- Get all comments in a tutorial GET /tutorials/:id/comments 
```

```
- Add a comment to a tutorial POST /tutorials/:id/comments
```

我们将只实现端点，其中没有逻辑。通过这种方式，我们将了解如何创建这种结构以及从 Akka HTTP 开始时的常见陷阱。

### 项目设置

我们为这个教程创建了一个 [repo](https://github.com/Codemunity/akka-http-routing-primer) ，在其中你会发现每个需要编码的部分都有一个分支。随意克隆它，并将其用作基础项目，或者甚至只是在分支之间进行更改，以查看不同之处。

否则，创建一个新的 SBT 项目，然后在`build.sbt`文件中添加依赖项:

```
name := "akkahttp-routing-dsl" 
```

```
version := "0.1" 
```

```
scalaVersion := "2.12.7" 
```

```
val akkaVersion = "2.5.17" val akkaHttpVersion = "10.1.5" 
```

```
libraryDependencies ++= Seq(   "com.typesafe.akka" %% "akka-actor" % akkaVersion,   "com.typesafe.akka" %% "akka-testkit" % akkaVersion % Test,  "com.typesafe.akka" %% "akka-stream" % akkaVersion,   "com.typesafe.akka" %% "akka-stream-testkit" % akkaVersion % Test,   "com.typesafe.akka" %% "akka-http" % akkaHttpVersion,   "com.typesafe.akka" %% "akka-http-testkit" % akkaHttpVersion % Test,      "org.scalatest" %% "scalatest" % "3.0.5" % Test )
```

我们添加了 Akka HTTP 及其依赖项、Akka Actor 和 Streams。我们还将使用 Scalatest 进行测试。

### 列出所有教程

我们将采用 TDD 方法来构建我们的指令层次，首先创建测试，以确保在添加其他指令时不会中断我们的路径。从 Akka HTTP 开始时，采用这种方法非常有帮助。

让我们从列出所有教程的路线开始。在`src/test/scala`下创建一个名为`RouterSpec`的新文件(如果文件夹不存在，则创建它们):

```
import akka.http.scaladsl.testkit.ScalatestRouteTest import org.scalatest.{Matchers, WordSpec} 
```

```
class RouterSpec extends WordSpec with Matchers with ScalatestRouteTest { 
```

```
}
```

`WordSpec`和`Matchers`由 Scalatest 提供，我们将使用它们来构建我们的测试和断言。`ScalatestRouteTest`是 Akka HTTP 的测试工具包提供的一个特性，它将允许我们以一种方便的方式测试我们的路由。让我们看看如何实现这一点。

因为我们使用了 [Scalatest 的 WordSpec](http://www.scalatest.org/at_a_glance/WordSpec) ，我们将从为我们即将创建的`Router`对象创建一个作用域开始，第一个测试:

```
"A Router" should {   "list all tutorials" in {   } }
```

接下来，我们希望确保可以向路径`/tutorials`发送 GET 请求，并获得我们期望的响应，让我们看看如何实现这一点:

```
Get("/tutorials") ~> Router.route ~> check {   status shouldBe StatusCodes.OK   responseAs[String] shouldBe "all tutorials" }
```

它甚至不会编译，因为我们还没有创建我们的`Router`对象。让我们现在做那件事。

在`src/main/scala`下创建一个名为`Router`的新 Scala 对象。在其中，我们将创建一个返回`Route`的方法:

```
import akka.http.scaladsl.server.Route 
```

```
object Router {
```

```
 def route: Route = ???
```

```
}
```

不要太担心`???`，它只是一个占位符，暂时避免编译错误。然而，如果代码被执行，它将抛出一个`NotImplementedError`,我们很快就会看到。

既然我们的测试和项目已经编译好了，让我们运行测试(右键单击 spec 并“运行‘RouterSpec’”)。

测试失败，出现了我们预期的异常，我们还没有实现我们的路由。我们开始吧！

### 创建列表路径

通过查看[官方文档](https://doc.akka.io/docs/akka-http/current/routing-dsl/directives/index.html#composing-directives)，我们看到这条路线始于`path`指令。让我们模仿他们在做什么，并建立我们的路线:

```
import akka.http.scaladsl.server.{Directives, Route}
```

```
object Router extends Directives {
```

```
 def route: Route = path("tutorials") {    get {      complete("all tutorials")    }  }}
```

似乎合理，让我们运行我们的规格。它通过了，太好了！

作为参考，我们的整个`RouterSpec`现在看起来像:

```
import akka.http.scaladsl.model.StatusCodesimport akka.http.scaladsl.testkit.ScalatestRouteTestimport org.scalatest.{Matchers, WordSpec}class RouterSpec extends WordSpec with Matchers with ScalatestRouteTest {  "A Router" should {    "list all tutorials" in {      Get("/tutorials") ~> Router.route ~> check {        status shouldBe StatusCodes.OK        responseAs[String] shouldBe "all tutorials"      }    }  }}
```

### 通过 ID 获取单个教程

接下来，我们将允许用户检索单个教程。

让我们为新路线添加一个测试:

```
"return a single tutorial by id" in {  Get("/tutorials/hello-world") ~> Router.route ~> check {    status shouldBe StatusCodes.OK    responseAs[String] shouldBe "tutorial hello-world"  }}
```

我们希望得到一条包含教程 ID 的消息。

测试将失败，因为我们还没有创建我们的路线，让我们现在就做。

从我们之前使用的相同的[资源](https://doc.akka.io/docs/akka-http/current/routing-dsl/directives/index.html#composing-directives)中，我们可以看到如何使用`~`指令将多个指令放置在层次结构中的相同级别。

我们将不得不嵌套`path`指令，因为在教程 ID 的`/tutorials`路线之后需要另一个段。在文档中，他们使用`IntNumber`从路径中提取一个数字，但是我们将使用一个字符串，为此我们使用 can `Segment`来代替。

我们的路线看起来像:

```
def route: Route = path("tutorials") {  get {    complete("all tutorials")  } ~ path(Segment) { id =>    get {      complete(s"tutorial $id")    }  }}
```

我们来做测试。您应该会得到类似的错误:

```
Request was rejectedScalaTestFailureLocation: RouterSpec at (RouterSpec.scala:17)org.scalatest.exceptions.TestFailedException: Request was rejected
```

这是怎么回事？！

当一个请求不符合我们的指令层次结构时，它就会被拒绝。这是我开始时遇到的事情之一。

现在可能是研究这些指令如何在传入的请求通过层次结构时匹配它的好时机。

不同的指令将匹配一个传入请求的不同方面，我们已经看到了`path`和`get`，一个匹配请求的 URL，另一个匹配方法。如果一个请求与一个指令匹配，它将进入指令内部，如果不匹配，它将继续进入下一个指令。这也告诉我们，秩序很重要。如果与任何指令都不匹配，则请求将被拒绝。

现在我们已经知道我们的请求与我们的指令不匹配，让我们开始研究为什么。

如果我们在文档中查找`path`指令(Mac 上的 Cmd + Click ),我们会发现:

```
/** * Applies the given [[PathMatcher]] to the remaining unmatched path after consuming a leading slash. * The matcher has to match the remaining path completely. * If matched the value extracted by the [[PathMatcher]] is extracted on the directive level. * * @group path */
```

因此，`path`指令必须完全匹配路径，这意味着我们的第一个`path`指令将只匹配`/tutorials`，而不会匹配`/tutorials/:id`。

在包含`path`指令的同一个`PathDirectives`特征中，我们可以看到另一个名为`pathPrefix`的指令:

```
/** * Applies the given [[PathMatcher]] to a prefix of the remaining unmatched path after consuming a leading slash. * The matcher has to match a prefix of the remaining path. * If matched the value extracted by the PathMatcher is extracted on the directive level. * * @group path */
```

`pathPrefix`只匹配一个前缀并删除它。听起来这就是我们要找的，让我们更新我们的路线:

```
def route: Route = pathPrefix("tutorials") {  get {    complete("all tutorials")  } ~ path(Segment) { id =>    get {      complete(s"tutorial $id")    }  }}
```

运行测试，然后…我们得到另一个错误。？

```
"[all tutorials]" was not equal to "[tutorial hello-world]"ScalaTestFailureLocation: RouterSpec at (RouterSpec.scala:18)Expected :"[tutorial hello-world]"Actual   :"[all tutorials]"
```

看起来我们的请求与第一个`get`指令相匹配。它现在匹配了`pathPrefix`，因为它也是一个 GET 请求，所以它将匹配第一个`get`指令。秩序很重要。

我们可以做几件事。最简单的解决方案是将第一个`get`请求移动到层次结构的末尾，但是，我们必须记住这一点或者记录下来。不理想。

就我个人而言，我倾向于避免这种解决方案，而是通过代码来明确意图。如果我们查看前面的`PathDirectives`特征，我们会发现一个名为`pathEnd`的指令:

```
/** * Rejects the request if the unmatchedPath of the [[RequestContext]] is non-empty, * or said differently: only passes on the request to its inner route if the request path * has been matched completely. * * @group path */
```

这正是我们想要的，所以让我们用`pathEnd`包装我们的第一个`get`指令:

```
def route: Route = pathPrefix("tutorials") {  pathEnd {    get {      complete("all tutorials")    }  } ~ path(Segment) { id =>    get {      complete(s"tutorial $id")    }  }}
```

再次运行测试，最后，测试通过了！？

### 列出教程中的所有注释

让我们进一步将我们所学的关于嵌套路由的知识付诸实践。

首先是测试:

```
"list all comments of a given tutorial" in {  Get("/tutorials/hello-world/comments") ~> Router.route ~> check {    status shouldBe StatusCodes.OK    responseAs[String] shouldBe "comments for the hello-world tutorial"  }}
```

这与之前的情况类似:我们知道需要将一条路线放置在另一条路线旁边，这意味着我们需要:

*   将`path(Segmenter)`改为`pathPrefix(Segmenter)`
*   用`pathEnd`指令包装第一个`get`
*   将新路线放在`pathEnd`旁边

我们的路线最终看起来像:

```
def route: Route = pathPrefix("tutorials") {  pathEnd {    get {      complete("all tutorials")    }  } ~ pathPrefix(Segment) { id =>    pathEnd {      get {        complete(s"tutorial $id")      }    } ~ path("comments") {      get {        complete(s"comments for the $id tutorial")      }    }  }}
```

运行测试，他们应该会通过！？

### 向教程添加注释

我们的最后一个端点与前面的类似，但是它将匹配 POST 请求。我们将用这个例子来看看实现和测试 GET 请求和 POST 请求之间的区别。

测试:

```
"add comments to a tutorial" in {  Post("/tutorials/hello-world/comments", "new comment") ~> Router.route ~> check {    status shouldBe StatusCodes.OK    responseAs[String] shouldBe "added the comment 'new comment' to the hello-world tutorial"  }}
```

我们使用的是`Post`方法，而不是我们一直使用的`Get`，我们给它一个额外的参数，即请求体。剩下的我们现在都熟悉了。

为了实现我们的最后一条路线，我们可以参考[文档](https://doc.akka.io/docs/akka-http/current/routing-dsl/index.html#longer-example)，看看它通常是如何完成的。

我们有一个`post`指令，正如我们有一个`get`指令一样。为了提取请求体，我们需要两个指令，`entity`和`as`，我们向它们提供我们期望的类型。在我们的例子中，它是一个字符串。

让我们试一试:

```
post {  entity(as[String]) { comment =>    complete(s"added the comment '$comment' to the $id tutorial")  }}
```

看起来很合理。我们将请求体提取为字符串，并在我们的响应中使用它。让我们将它添加到我们之前工作的路线旁边的`route`方法中:

```
def route: Route = pathPrefix("tutorials") {  pathEnd {    get {      complete("all tutorials")    }  } ~ pathPrefix(Segment) { id =>    pathEnd {      get {        complete(s"tutorial $id")      }    } ~ path("comments") {      get {        complete(s"comments for the $id tutorial")      } ~ post {        entity(as[String]) { comment =>          complete(s"added the comment '$comment' to the $id tutorial")        }      }    }  }}
```

如果你想学习如何在 JSON 中解析 Scala 类[，我们也有相关的教程](https://www.codemunity.io/tutorials/akka-http-json-circe/)。

做测试，应该都能通过。

### 结论

Akka HTTP 的路由 DSL 起初可能看起来令人困惑，但在克服一些障碍后，它就能点击了。过一会儿，它会自然而然地出现，而且会变得非常强大。

我们学会了如何构建我们的路线，但更重要的是，我们学会了如何在测试的指导下创建这种结构，以确保我们不会在未来的某个时刻破坏它们。

尽管我们只在四个端点上工作，但我们最终得到了一个有点复杂和深刻的结构。敬请关注，我们将探索不同的方法来简化我们的路线，使它们更易于管理！

[通过这个循序渐进的免费课程，了解如何用 Scala 和 Akka HTTP 构建 REST APIs！](http://link.codemunity.io/website-akka-http-quickstart)

最初发布于[www . code unity . io](https://www.codemunity.io/tutorials/akka-http-routing-primer/)。