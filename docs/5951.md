# 一个强大的流库:FS2 和函数式编程

> 原文：<https://www.freecodecamp.org/news/a-streaming-library-with-a-superpower-fs2-and-functional-programming-6f602079f70a/>

Scala 有一个非常特殊的流库，叫做 FS2(Scala 的函数流)。这个库体现了函数式编程(FP)的所有优点。通过理解它的设计目标，你会接触到让 FP 如此吸引人的核心思想。

FS2 有一个中心类型:`**Stream[Effect,Output]**`

你可能会从这个类型中得到它是一个`Stream`并且它发出类型`Output`的值。

这里明显的问题是什么是`Effect`？`Effect`和`Output`有什么联系？而 FS2 相对于其他流库有什么优势？

### 概观

我将从回顾 FS2 解决了什么问题开始。然后我用几个代码例子对比一下`List`和`Stream`。在那之后，我将重点介绍如何将`Stream`与 DB 或任何其他 IO 一起使用。这就是 FS2 的闪光点，也是使用`Effect`类型的地方。一旦你理解了什么是`Effect`,函数式编程的优势就显而易见了。

在这篇文章的最后，你会得到以下问题的答案:

*   用 FS2 可以解决什么问题？
*   我能用`Stream`做什么`List`做不到的事？
*   如何将数据从 API/文件/数据库输入到`Stream`？
*   这个`Effect`类型是什么，它与函数式编程有什么关系？

注意:代码是用 Scala 编写的，即使没有语法知识也应该可以理解。

### 用 FS2 可以解决什么问题？

1.  流式 I/O:增量加载不适合内存的大数据集，并在不破坏堆的情况下对其进行操作。
2.  控制流(未涉及):以良好的声明方式将数据从一个/几个数据库/文件/API 转移到其他数据库/文件/API。
3.  并发性(未涵盖):并行运行不同的流，并使它们一起通信。例如，从多个文件加载数据并同时处理它们，而不是顺序处理。你可以在这里做一些高级的东西。流可以在处理阶段一起通信**，而不仅仅是在结束时。**

### `List`对`Stream`

`List`是最广为人知和最常用的数据结构。为了感受它与 FS2 `Stream`的不同，我们将通过几个用例。我们将看到`Stream`如何解决`List`无法解决的问题。

### 您的数据太大，不适合内存

假设您有一个非常大的文件(40GB) `fahrenheit.txt`。该文件每行都有一个温度，您想将其转换为`celsius.txt`。

### 使用`List`加载大文件

```
import scala.io.Source
val list = Source.fromFile("testdata/fahrenheit.txt").getLines.toList
java.lang.OutOfMemoryError: Java heap space
  java.util.Arrays.copyOfRange(Arrays.java:3664)
  java.lang.String.<init>(String.java:207)
  java.io.BufferedReader.readLine(BufferedReader.java:356)
  java.io.BufferedReader.readLine(BufferedReader.java:389)
```

失败很惨，因为文件太大，内存放不下。如果你很好奇，你可以在这里用`Stream` [查看完整的解决方案——但是稍后再做，请继续阅读:)](https://functional-streams-for-scala.github.io/fs2/#about)

### 当列表不起作用时…流去救援！

假设我成功地读取了我的文件，我想把它写回来。我想保留线条结构。我需要在每个温度后插入一个换行符`\n`。

我可以使用`intersperse`组合子来做这件事

```
import fs2._
Stream(1,2,3,4).intersperse("\n").toList
```

另一个不错的是`zipWithNext`

```
scala> Stream(1,2,3,4).zipWithNext.toList
res1: List[(Int, Option[Int])] = List((1,Some(2)), (2,Some(3)), (3,Some(4)), (4,None))
```

它把连续的东西捆绑在一起，如果你想[删除连续的重复](https://gist.github.com/dsebban/bb34ea4671bda8d52e2f083e2b160778)非常有用。

这些只是许多非常有用的例子中的几个，这里是完整的列表。

显然`Stream`可以做很多`List`做不到的事情，但是最好的特性将在下一节出现，这是关于如何在真实世界中使用数据库/文件/API 的`Stream`...

### 如何将数据从 API/文件/数据库输入到`Stream`？

我们现在只能说这是我们的计划

```
scala> Stream(1,2,3)
res2: fs2.Stream[fs2.Pure,Int] = Stream(..)
```

这个`Pure`是什么意思？下面是 scaladoc 的源代码:

```
/**
    * Indicates that a stream evaluates no effects.
    *
    * A `Stream[Pure,O]` can be safely converted to a `Stream[F,O]` for all `F`.
*/
type Pure[A] <: Nothing
```

意思是没有效果，好吧…，但是**什么是效果？**更具体地说，我们的节目`Stream(1,2,3)`有什么效果？

这个项目实际上对世界没有任何影响。它唯一的作用就是让你的 CPU 工作并消耗一些能量！！它不会影响你周围的世界。

我所说的影响世界是指**消耗**任何有意义的资源，比如文件、数据库，或者 it **产生**任何东西，比如文件、上传数据到某个地方、写到你的终端等等。

### 我如何把一个`Pure`流变成有用的东西？

假设我想从一个数据库加载用户 Id，我得到了这个函数，它调用数据库并返回用户 id 作为一个`Long`。

```
import scala.concurrent.Future
def loadUserIdByName(userName: String): Future[Long] = ???
```

它返回一个`[Future](https://www.scala-lang.org/api/2.12.3/scala/concurrent/Future.html)`,表示该调用是异步的，并且该值将在将来的某个时间点可用。它包装数据库返回的值。

我有这条`Pure`小溪。

```
scala> val names = Stream("bob", "alice", "joe")
names: fs2.Stream[fs2.Pure,String] = Stream(..)
```

我如何获得 id 的`Stream`？

最简单的方法是使用`map`函数，它应该为`Stream`中的每个值运行该函数。

```
scala> userIdsFromDB.compile
res5: fs2.Stream.ToEffect[scala.concurrent.Future,Long] = fs2.Stream$ToEffect@fc0f18da
```

我还是拿回了一个`Pure`！我给了`Stream`一个*影响世界*的功能，我还是得到了一个`Pure`，不酷...如果 FS2 能够自动检测到`loadUserIdByName`函数对世界产生了*影响*，并返回给我一个不是`Pure`的东西，那就太好了，但它不是这样工作的。你得用一个特殊的组合子来代替`map`:你得用`evalMap`。

```
scala> userIdsFromDB.toList
<console>:18: error: value toList is not a member of fs2.Stream[scala.concurrent.Future,Long]
       userIdsFromDB.toList
                     ^
```

不再有`Pure`！我们得到了`Future`，耶！刚刚发生了什么？

花了:

*   `loadUserIdByName: Future[Long]`
*   `Stream[Pure, String]`

并将流的类型切换到

*   `Stream[Future, Long]`

它把`Future`分开了，隔离了！左边是`Effect`类型参数，现在是具体的`Future`类型。

巧妙的诡计，但它对我有什么帮助？

您刚刚见证了真正的**关注点分离。**你可以继续用所有好的`List`像组合子在流上操作，你不必担心数据库是否关闭、变慢或所有与网络(效果)相关的问题。

在我想使用`toList`取回值之前，一切都正常

```
scala> userIdsFromDB.toList
<console>:18: error: value toList is not a member of fs2.Stream[scala.concurrent.Future,Long]
       userIdsFromDB.toList
                     ^
```

什么？？？！！！我可以发誓我以前使用过`toList`并且有效，它怎么能说`toList`不再是`fs2.Stream[Future,String]`的成员呢？就好像在我开始使用 effect-ful stream 的时候，这个功能就被移除了一样，令人印象深刻！但是我如何找回我的价值观呢？

```
scala> userIdsFromDB.compile
res5: fs2.Stream.ToEffect[scala.concurrent.Future,Long] = fs2.Stream$ToEffect@fc0f18da
```

首先我们使用`compile`来告诉`Stream`将所有的效果合并成一个，实际上它将所有对`loadUserIdByName`的调用合并成一个大的`Future`。这是框架所需要的，很快就会明白为什么需要这一步。

现在`toList`应该工作了

```
scala> userIdsFromDB.compile.toList
<console>:18: error: could not find implicit value for parameter F: cats.effect.Sync[scala.concurrent.Future]
       userIdsFromDB.compile.toList
                             ^
```

什么？！编译器还在抱怨。这是因为`Future`不是一个好的`Effect`类型——它打破了关注点分离的哲学，这将在下一个非常重要的部分解释。

### 重要:从这篇文章中可以学到的一件事

这里的一个关键点是，此时还没有调用 DB。真的什么也没发生，整个程序什么也没产生。

```
def loadUserIdByName(userName: String): Future[Long] = ???
Stream("bob", "alice", "joe").evalMap(loadUserIdByName).compile
```

### 将程序描述与评估分开

是的，这可能令人惊讶，但 FP 的主题是将

*   你的程序的描述:一个很好的例子是我们刚刚写的程序，它是对问题的一个纯粹的描述“我给你名字和数据库，给我 id”

然后

*   你的程序的执行:运行实际的代码并请求它去数据库

再说一次，我们的程序除了让你的电脑变暖之外，对世界没有任何影响*，就像我们的`Pure`流一样。*

*没有效果的代码被称为**纯**，这就是所有函数式编程的意义:用纯**的函数编写程序。**太棒了，你现在知道 FP 是怎么回事了。*

*为什么要这样写代码呢？简单:实现 IO 部分和代码其余部分的关注点分离。*

*现在让我们修正我们的程序，解决这个`Future`问题。*

*正如我们所说的`Future`是一个不好的`Effect`类型，它违背了关注点分离原则。事实上，`Future`在 Scala 中非常渴望:当你创建一个线程时，它就开始在某个线程上执行，你无法控制执行，因此它会中断。FS2 很清楚这一点，不让你编译。为了解决这个问题，我们必须使用一个名为`IO`的类型来包装我们的坏`Future`。*

*这就把我们带到了最后一部分，这是什么类型的？我怎样才能最终找回我的列表呢？*

```
*`scala> import cats.effect.IO
import cats.effect.IO
scala> Stream("bob", "alice", "joe").evalMap(name => IO.fromFuture(IO(loadUserIdByName(name)))).compile.toList
res8: cats.effect.IO[List[Long]] = IO$2104439279`*
```

*现在它给了我们一个`List`但是我们仍然没有拿回我们的 id，所以最后一个东西一定是丢失了。*

*![ivbIHjwyNlEmckvLT6thLcPRcVRUkRGgw-dH](img/9f2b8f73e2ccb759b17e87a1bb5e89da.png)*

### *`IO`到底是什么意思？*

*`IO`来自[猫-效果库](https://typelevel.org/cats-effect/datatypes/io.html)。首先让我们完成我们的程序，最后从数据库中取出 id。*

```
*`scala> userIds.compile.toList.unsafeRunSync
<console>:18: error: not found: value userIds
       userIds.compile.toList.unsafeRunSync
       ^`*
```

*证明它正在做某事的证据是它正在失败的事实。*

```
*`loadUserIdByName(userName: String): Future[Long] = ???`*
```

*当`???`被调用时，你会得到这个异常，这意味着函数被执行了(与之前我们指出什么都没发生相反)。当我们实现这个函数时，它将进入数据库并加载 ids，它将对世界(网络/文件系统)产生一个**效应**。*

*`IO[Long]`是**的**描述如何获得类型为`Long`的值，它肯定涉及到一些 I/O 操作，例如访问网络、加载文件、...*

*是**如何**而不是**什么。**描述了如何从网络中获取价值。如果要执行这个描述，可以使用`unsafeRunSync`(或者其他函数前缀`unsafe`)。您可以猜到为什么这样称呼它们:事实上，对数据库的调用本质上是不安全的，因为它可能会失败，例如，如果您的互联网连接中断了。*

### *概述*

*让我们最后看一下`**Stream[Effect,Output]**` **。***

*`Output`是流发出的类型(可以是`String`、`Long`或您定义的任何类型的流)。*

*`Effect`是产生`Output`的方法(配方)(即去数据库给我一个`Long`类型的`id`)。*

*重要的是要明白，如果将这些类型分开来使事情变得更容易，那么将一个问题分解成子问题可以让你独立地思考子问题。然后，您可以解决它们并组合它们的解决方案。*

*这两种类型之间的联系如下:*

*为了让`Stream`发出类型的元素*

*   *`Output`*

*它需要评估一个类型*

*   *`Effect`*

*一种特殊类型，将有效动作编码为类型`IO`的值，这个`IO`值允许分离两个关注点:*

*   ***描述** : `IO`是一个简单的不可变值，通过做一些 IO(网络/文件系统/…)来获得类型`A`是一个诀窍*
*   ***执行**:为了让`IO`做某事，你需要*使用`io.unsafeRunSync`执行/运行它**

#### *把所有的放在一起*

*`Stream[IO,Long]`说:*

*这是一个发出类型为`Long`的值的`Stream`，为了这样做，它需要运行一个*有效的*函数，为每个值产生`IO[Long]`。*

*这种非常简短的字体包含了很多细节。你对事情发生的细节了解得越多，你犯的错误就越少。*

### *外卖食品*

*   *`Stream`是**超级充电**版`List`*
*   *`Stream(1,2,3)`是类型`Stream[Pure, Int]`，第二种类型`Int`是该流将发出的所有值的类型*
*   *`Pure`表示对世界没有*影响*。它只是让你的 CPU 工作，消耗一些电量，但除此之外并不影响你周围的世界。*
*   *当您想要将具有类似`loadUserIdByName`效果的函数应用到`Stream`时，请使用`evalMap`而不是`map`。*
*   *通过让您只处理值，而不用担心如何获得它们(从数据库中加载),分离了对什么和如何的关注。*
*   *将程序描述从评估中分离出来是 FP 的一个关键方面。*
*   *你用`Stream`写的所有程序在你用`unsafeRunSync`之前什么都不会做。在此之前，你的代码实际上是*纯的。**
*   *`IO[Long]`是一种效果类型，它告诉你:你将从 IO(可能是文件、网络、控制台)获得`Long`值...).这是描述而不是包装！r*
*   *`Future`不遵守这一原则，因此与 FS2 不兼容，您必须使用`IO`类型。*

### *FS2 视频*

*   *迈克尔·皮尔奎斯特截图:[https://www.youtube.com/watch?v=B1wb4fIdtn4](https://www.youtube.com/watch?v=B1wb4fIdtn4)*
*   *法比奥·拉贝拉的谈话[https://www.youtube.com/watch?v=x3GLwl1FxcA](https://www.youtube.com/watch?v=x3GLwl1FxcA)*