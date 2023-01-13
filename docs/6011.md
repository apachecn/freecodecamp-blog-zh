# 如何用 Kotlin 中的 Actor 实现对象池

> 原文：<https://www.freecodecamp.org/news/how-to-implement-an-object-pool-with-an-actor-in-kotlin-ed06d3ba6257/>

根据 osha1

# 如何用 Kotlin 中的 Actor 实现对象池

![1*DC-hZN28XBFzRHf3UhyeEQ](img/418e7b1640911ce98ca3a00075ed6426.png)

Photo by [David Lezcano](https://unsplash.com/photos/mNCFOaaLu5o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/pool?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们在 [jasync-sql](https://github.com/jasync-sql/jasync-sql) 中使用对象池来管理到数据库的连接。在这篇文章中，我将分享如何通过一个 Actor 使用 Kotlin 协程以一种高性能的、无锁的方式实现这一点。

对象池有一个非常简单的 API。它是一个对象池，有两个方法:`take()`和`return()`。

乍一看，这似乎是一个非常简单的问题。这里的主要问题是它必须是高性能的和线程安全的，这使得它的实现变得有趣和棘手。

### 但是嘿！我们为什么需要对象池呢？

jasync-sql 是一个用来访问关系数据库的库，比如 MySQL 和 PostgreSQL。数据库连接是需要对象池的一个很好的例子。对数据库的访问是通过从**连接池**中获得一个连接，使用它并将其返回到连接池来完成的。

与为每个 SQL 查询创建连接相比，使用连接池有几个优点:

*   *重用连接* —由于启动数据库连接的开销很高(握手等)，连接池允许保持连接活动，从而减少了开销。
*   *限制资源* —为每个用户请求创建一个数据库连接可能会让数据库不堪重负。使用池实际上增加了一个障碍，限制了最大并发连接数。

> 嗯，我被卖了，但是…

### 连接池不是 Java 世界已经解决的问题吗？

是的，如果你使用 [JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) ，这个问题就解决了。在这种情况下，根据我的经验， [HikariCP](https://brettwooldridge.github.io/HikariCP/) 是一个很好的选择，但是还有很多其他的选择。在 jasync-sql 的情况下，不可能使用 [HikariCP](https://brettwooldridge.github.io/HikariCP/) ，因为 [HikariCP](https://brettwooldridge.github.io/HikariCP/) 与 JDBC API 一起工作，并且 jasync-sql 驱动程序没有实现那个完全成熟的 API，只是它的一个子集。

> Java 世界的其他对象池呢？

有许多实现，但是事实证明，您通常会发现一些您正在使用的池没有实现的特定需求。

在我们的例子中，要求是非阻塞的。在我们的池中，所有操作都必须是非阻塞的，因为库是异步的。例如，大多数实现中的`take()`操作会立即返回一个对象，或者阻塞，直到对象准备好。我们的`take()`在>上返回一个`Future<Connecti`，当连接准备好可以使用时，它将被完成并继续。

我在野外没有见过这样的实现。

我很喜欢 Stack Exchange 的这个回答:

[**对象池是一种不推荐使用的技术吗？**](https://softwareengineering.stackexchange.com/questions/115163/is-object-pooling-a-deprecated-technique)
[*软件工程栈交流是一个为专业人士、学者和学生工作的问答网站……*softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/115163/is-object-pooling-a-deprecated-technique)

另一个很难找到替代方案的需求是需要尽可能地与我们现有的实现保持兼容。

如果你想看其他的实现，你可以点击这里:

java 中的 [**对象池——谷歌搜索**](https://www.google.co.il/search?q=object+pool+in+java)
[*对象池是一个特定对象的集合，应用程序将为这些对象创建并保存……*www.google.co.il](https://www.google.co.il/search?q=object+pool+in+java)

### 那么我们是如何实现对象池的呢？

在我们深入细节之前，让我们观察一下对象池的其他需求，为了清楚起见，上面省略了这些需求，但它们是必要的细节。

#### 接口

对象池界面如下所示:

```
interface AsyncObjectPool<T&gt; {  fun take(): CompletableFuture&lt;T>  fun giveBack(item: T): CompletableFuture<AsyncObjectPool<T>>  fun close(): CompletableFuture<AsyncObjectPool<T>>
```

```
}
```

此外，当一个池想要创建新的对象(连接)时，它将调用`ObjectFactory`。工厂有更多的方法来处理对象生命周期:

*   *validate* —检查对象是否仍然有效的方法。该方法应该很快，并且只检查内存中的构造。对于连接，我们通常检查最后一个查询没有抛出异常，也没有从 [netty](https://netty.io/) 得到终止消息。
*   *测试* —类似于验证，但是更彻底的检查。我们允许测试方法缓慢，并访问网络等。此方法用于检查空闲对象是否仍然有效。对于连接，这将类似于`select 0`。
*   *destroy* —当池不再使用对象时，调用它来清理对象。

完整的界面是:

```
interface ObjectFactory<T> {  fun create(): CompletableFuture<;out T>  fun destroy(item: T)  fun validate(item: T): Try<T>  fun test(item: T): CompletableFuture<T>
```

```
}
```

对于池配置，我们有以下属性:

*   `maxObjects` —我们允许的最大连接数。
*   `maxIdle` —我们在不使用的情况下保持连接打开的时间。在那之后，它将被回收。
*   `maxQueueSize` —当一个连接请求到达而没有可用的连接时，我们将该请求放在队列中等待。如果队列已满(其大小超过了`maxQueueSize`)，它将不会等待，而是返回一个错误。
*   `createTimeout` —等待创建新连接的最长时间。
*   `testTimeout` —等待空闲连接上的测试查询的最长时间。如果通过，我们将认为连接是错误的。
*   `validationInterval` —在此时间间隔内，我们将测试空闲连接是否处于活动状态，并释放通过`maxIdle`的连接。我们也将删除通过`testTimeout`的连接。

#### 原始实现

对象池的第一个实现是单线程的。所有操作都被发送到负责执行它们的工作线程。这种方法被称为[线程限制](https://www.javaspecialists.eu/archive/Issue218.html)。对象创建和测试操作是阻塞的，而查询执行本身是非阻塞的。

这种方法是有问题的，因为操作是一个接一个地进行的。除此之外，如上所述，还有一些操作被阻塞。在一些场景和用例中工作时，会出现各种高延迟的情况(比如这里的)。

作为一种变通方法，引入了`PartitionedPool`。这是用上述单线程方法解决*阻塞*问题的一种变通方法。分区池创建多个`SingleThreadedObjectPools`，每个都有自己的工作线程。当请求连接时，通过线程 id 上的模数来选择池。分区池实际上是一个池的池；-)

我提到这是一种变通方法，因为它有自己的问题:您可能仍然阻塞，但速率较低——而且它消耗更多的线程和资源。

#### 基于执行元的实现

参与者是拥有邮箱的实体。它将邮件接收到它的邮箱中，并逐个处理它们。邮箱是一种将事件从外部世界传递给参与者的渠道。

协程执行元使用无锁算法来快速高效地执行事件，而不需要锁和`synchronized`块。

![0*T1B40xs7Fsf-gnfZ](img/53ca787079aacd7952ed595992673e71.png)

“wall rack filled with paper document lot” by [Nong Vang](https://unsplash.com/@californong?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你可以在这里看到详细的解释[。](https://www.brianstorti.com/the-actor-model/)

在我们的例子中，这些事件将是`take`和`giveBack`。除此之外，我们还会有演员发送给自己的内部消息，如`objectCreated`等。这允许参与者拥有不受并发问题困扰的状态，因为它总是被限制在相同的顺序执行中。此外，传递这些事件的通道是一个使用无锁算法的队列，因此它非常高效，避免了争用，并且通常具有非常高的性能。

有一个很棒的视频解释了这是如何实现的(注意这是“沉重的”算法人员):

让我们回顾一下到目前为止我们所拥有的:

*   一个参与者接收消息并逐个处理它们。
*   通常消息会包含一个`CompletableFuture`,它应该在参与者处理它时完成。

消息将被立即完成或延迟(就像在我们等待连接被创建的情况下)。如果它被延迟，actor 将把`Future`放入一个队列中，并使用回调机制来通知自己最初的未来何时可以完成。

*   actor 中的消息处理不应该阻塞或延迟 actor。如果发生这种情况，它将延迟队列中等待处理的所有消息，并将降低整个 actor 操作的速度。

这就是为什么，如果我们在 actor 中有长时间运行的操作，我们使用回调机制。

#### 让我们看看用例的更多细节

`Take` —有人想要池子里的一个物体。它将向参与者发送一条带有回调的消息。演员将做以下事情之一:

*   如果对象是可用的——参与者将简单地返回它。
*   如果池没有超过已创建对象的限制，执行元将创建一个新对象，并在对象就绪时返回它。

在这种情况下，对象创建可能需要时间，因此 actor 会将对象创建的回调连接到原始的 take 请求回调。

*   会将请求放入一个可用对象的队列中(除非队列已满，在这种情况下只会返回一个错误)。

`GiveBack` —有人想把一个对象还给池(释放它)。这也是通过向演员发送消息来实现的。该演员将执行以下操作之一:

*   如果有人在等待队列中等待，它将借用对象。
*   在其他情况下，它只是将对象保留在池中以等待请求的到来，因此对象保持空闲。

`Test` —外部人员会定期通知演员测试连接:

*   actor 会释放长时间不用的空闲连接(可配置)。
*   参与者将使用`ObjectFactory`测试其他空闲对象。它将向工厂发送一个回调，并将这些对象标记为使用中的*，以防止在测试完成之前借用它们。*
*   *参与者将检查测试中的超时，并销毁超时的对象。*

*这些是主要的用例。*

#### *渗*

*![0*64dZ7F9trDtbSdWq](img/83c3594c578ad558796b0716c0aa259f.png)

“selective focus photography of brown faucet” by [Jouni Rajala](https://unsplash.com/@leipuri?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)* 

*对象池中可能存在各种各样的泄漏。有些是内部错误，我希望更容易发现和修复，其他是由于一些用户错误而被取走但没有归还的对象。在这种情况下，对象可能会永远留在“*使用中”*队列中。*

*为了避免这种情况，*“使用中”*地图使用 Java 的 [WeakHashMap](https://www.baeldung.com/java-weakhashmap) 。因此，如果一个用户失去了连接，当 Java 的垃圾收集器清理它时，它将自动从地图中删除。*

*此外，我们在这种情况下添加了一条日志消息，内容为:**“检测到泄漏”**。*

### *就是这样！*

*对象池的完整 Kotlin 源代码可从以下网址获得:*

*[**ja sync-SQL/ja sync-SQL**](https://github.com/jasync-sql/jasync-sql/blob/bacdd12243d89a5e2a46501bb5303815a9fd11e7/db-async-common/src/main/java/com/github/jasync/sql/db/pool/ActorBasedObjectPool.kt)
[*用 Kotlin - jasync-sql/jasync-sql 编写的 MySQL 和 PostgreSQL 的 Java 异步数据库驱动程序*github.com](https://github.com/jasync-sql/jasync-sql/blob/bacdd12243d89a5e2a46501bb5303815a9fd11e7/db-async-common/src/main/java/com/github/jasync/sql/db/pool/ActorBasedObjectPool.kt)*

*在接下来的文章中，我将比较不同实现的性能指标。*

*如果你想了解更多关于科特林的信息，这里有一个不错的介绍:*

*对于一般的协程，请查看此视频:*

*最后，如果您想了解更多关于在 Kotlin 中使用协程实现 Actors 的内容，请点击这里:*

*[**Kotlin/kot linx . coroutines**](https://github.com/Kotlin/kotlinx.coroutines/blob/master/docs/shared-mutable-state-and-concurrency.md)
[*库对 kot Lin 协同程序的支持。在…*github.com](https://github.com/Kotlin/kotlinx.coroutines/blob/master/docs/shared-mutable-state-and-concurrency.md)上创建一个帐户，为 Kotlin/kotlinx.coroutines 的开发做出贡献*

*感谢阅读！❤️*

*![0*0aDugHie8xlGjhOZ](img/04ee719097b7972e59e792532611edbe.png)

“aerial photography of woman on pink swimming floats” by [Tom Grimbert](https://unsplash.com/@tom_grimbert?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*