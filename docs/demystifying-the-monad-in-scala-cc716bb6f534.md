# 揭开 Scala 中单子的神秘面纱

> 原文：<https://www.freecodecamp.org/news/demystifying-the-monad-in-scala-cc716bb6f534/>

在我之前的文章中，我提到了我是如何决定写方差的，尽管已经有几十篇关于这个主题的文章了。这也是类似的情况。我知道这只是沧海一粟。我觉得海洋被涉及范畴理论的复杂数学解释污染了。枯燥的解释，一个定义接一个定义，却不提供事情的要点。

我不会讲技术，我说的技术是指数学。斯蒂芬·霍金说，他被警告说，他放入《时间简史》(顺便说一句，是本伟大的书)的每一个等式都会使销量减半。我将遵循同样的原则。但是我也会尽量不要太简单，简单的意思是模糊的。我们确实需要把手弄脏。最后，但同样重要的是，我将提供一些例子作为解释的主要手段。例子更多的是作为主要解释的补充。

#### 介绍

那么，什么是单子呢？

你可以把单子想象成**包装器**。你只要拿一个物体，用单子把它包起来。就像包礼物一样；你拿一条围巾、一本好书或一块漂亮的手表，用一些闪亮的纸和一条红丝带把它包起来。我们用闪亮的纸包装礼物，因为它们看起来很漂亮。我们用单子包装对象，因为单子为我们提供了以下两种操作:

*   **身份**(Haskell 中的*返回*，Scala 中的*单元*)
*   **绑定**(*>*)；> =在 Scala 中有*凯尔，f* latMap)

Scala 不像 Haskell 那样有内置的单子类型，所以我们将自己建模单子。如果你看一看一些很酷的函数式编程库，比如 [Scalaz](https://github.com/scalaz/scalaz) ，你会在那里找到单子。你还会发现范畴理论家族的其他成员(函子、应用子、幺半群等等)。在普通的 Scala 中，没有现成的东西。

我们将用提供方法 unit()和 flatMap()的一般特征来建模 monad。我们可以称它为 M 而不是 Monad，这样更简洁。这是:

```
trait M[A] {
  def flatMap[B](f: A => M[B]): M[B]
}

def unit[A](x: A): M[A]
```

看，它提供了前面提到的两个功能，*单位*和*平面图*。不要担心他们的签名，他们现在做什么或者为什么 unit()方法在 trait 主体之外。我们一会儿就会谈到这一点。

现在，请注意这里非常重要的一点——有一个单子的**概念(半分钟前我们在 Scala 中建模了它)。有**血肉之躯的具体单子**实现那两个功能并实际做一些事情(例如 IO 单子)。人们有时指的是一件事，有时指的是另一件事，所以要小心。在本文的后面，我们将会遇到一些具体的单子。你可以把一个通用的单子看作一个接口，把具体的单子看作实现。**

还有一件事你应该注意——单子接受一个类型参数。我们不只是写 M；我们写了 M[A]。类型参数就像我们礼品包装上的标签贴纸，说明我们里面有什么样的对象(在现实生活中，这可能会破坏惊喜，但在编程中，我们不喜欢惊喜)。因此，如果我们想用 monad 包装器包装某个对象，我们必须用底层对象的类型参数化 monad，例如 M[Int]，M[String]，M[MyClass]等。

现在让我们仔细看看这两个函数。

#### 单子函数

万一你刚加入我们，我再说一遍，单子来了两个操作，*单位*(也叫*身份*或*返回*)和*平面图*(也叫*绑定*)。顺便说一下，文献和网上资源丰富，有各种各样的命名。我只是提到了最常见的。等等，我忘了一个——unit 也常被称为*纯*。但是不要担心，无论使用哪种命名，引用哪个函数总是非常简单的。哦，他们有时也用*零*表示单位。好吧，我们继续。

单元执行包装部分。因此，在我们的礼品包装类比的情况下，我们可以将一本书传递给 unit()。它会把我们的书包在这个超级酷的闪亮的彩色包装纸里，上面有一个“书”的标签。在单子特征的情况下，如果我们给它一个 A 类型的值，它将返回一个 M[A]类型的值。这是一种单子构造函数，如果你愿意的话。

在这一点上，我们应该清楚为什么在 trait 主体之外定义方法单元()。因为我们不想在现有的单子对象上调用它(例如 myMonad.unit("myBook "))。那没什么意义。我们希望它是一个独立的静态方法(例如 unit("myBook "))。通过将我们的书传递给 unit()，我们将它包装在单子中返回。

现在，关于平面地图。你可能已经很熟悉了；这和你在 Scala 的其他地方遇到的平面图完全一样，比如集合。这是它的签名:

```
def flatMap[B](f: (A) => U[B]): U[B]
```

假设 U 是一个列表。它适用于各种其他类型，但是在这个例子中我们将使用 List。现在，flatMap 所做的是，它采用一个签名为 A *→* List[B]的函数，并使用该函数将 A 类型的底层对象转换为 List[B]。这个操作被称为*地图*。因为我们把底层的 A 转换成了 List[B]，这就留给我们一个 List[List[B]]。但是我们没有使用普通的 map() —我们使用了 flatMap()。这意味着工作还没有完成；flatMap 现在会将我们的 List[List[B]]“展平”成 List[B]。

我们来看一个具体的例子。如果 A 是 Int，而我们的函数 *f* 是

```
val f = (i: Int) => List(i - 1, i, i + 1)
```

然后我们可以用 *f* 来平面映射一个整数列表，如下所示:

```
val list = List(5, 6, 7)
println(list.flatMap(f))
// prints List(4, 5, 6, 5, 6, 7, 6, 7, 8)
```

请注意，常规 map()将获取每个数字 *i* 并将其扩展为一个迷你列表*(前任，原数字，继任者)*，这将为我们提供以下迷你列表列表:List((4，5，6)，(5，6，7)，(6，7，8))。FlatMap 更进一步；它把它展平成一个长列表，产生了
列表(4，5，6，5，6，7，6，7，8)。

现在假设我们有一个 M 类，它的底层类型是 A，写为 M[A]。如果我们想[平面映射那个 sh*t](http://1.bp.blogspot.com/-f5T38j9evCk/VZ54cIBwUeI/AAAAAAAABLY/3bvMZaQ4HCY/s640/nzfiq.jpg) (我没有发明这个短语；尝试谷歌一下)我们需要提供一个函数 A *→* M[B]。然后 FlatMap 会用这个函数把我们的底层 A 转换成 M[B]，产生一个 M[M[B]]，然后它会把整个东西扁平化成 M[B]。

```
 map with A => M[B]                  flatten
M[A]  ------------------------->  M[M[B]]  -----------> M[B]
```

注意，flatMap 不需要函数 A *→* M[A]，而是需要一个更灵活的函数，A *→* M[B]。所以如果 M 是一个 List，A 是一个 Int，我们可以用 Int *→* List[String]，Int *→* List[MyClass]等等函数来馈给 flatMap。不一定非得是 Int*→*List【Int】。例如，我们可以将 *f* 定义为:

```
val f = (i: Int) => List("pred=" + (i - 1), "succ=" + (i + 1))
```

然后我们可以像这样平面映射一个整数列表:

```
val list = List(5, 6, 7)
println(list.flatMap(f))
// prints List(pred=4, succ=6, pred=5, succ=7, pred=6, succ=8)
```

平面地图比地图更强大。它使我们能够将操作链接在一起，您将在示例部分看到这一点。地图功能只是平面地图功能的一个子集。如果想让 map()在你的 monad 中可用，可以用 monad 现有的方法 flatMap()和 unit()来表达，就像这样(注意 *g* 是某个函数 *Int → Something* ，不是*Int→List【Something】*):

```
 m map g = flatMap(x => unit(g(x)))
```

如果我们只有地图和单位，我们就不能定义平面地图，因为他们都不知道什么是扁平化。Flatten(在 Haskell 中也称为 *join* )是我们的 monad 机器中非常重要的一部分。如果我们有一个地图，但是没有展平的能力(因此没有平面地图)，那么我们将会以范畴理论中所谓的*函子*而告终。函子也很酷，但单子是我们节目的明星，所以我们不要跑题。

顺便说一下，如果我们将 *flatMap* 分解成 *map* ( *fmap* )和 *flatten* ( *join* )我们将创建一个[完全等价的](https://en.wikipedia.org/wiki/Monad_%28functional_programming%29#fmap_and_join)和有效的单子定义。这个定义应该是这样的:

*   定义单位:A → F[A]
*   def 映射:F[A] → (A → B) → F[B]
*   def flatten: F[F[A]] → F[A]

注意，我在这里将 *map 的*签名写成了一个完全独立的“中立”函数。这意味着它是在没有任何附加上下文的情况下定义的，而不是作为 F[A]上的一个方法。

这样的*映射*首先先取 F[A]的一个实例(比如 List(42))，然后取一个函数 A → B(比如 a → a + 1)然后给我们回一个 F[B]的实例(在我们的情况下 List(43))。我这样写只是为了能够写出相应的*扁平*签名。如果我把 map 简单地写成 A → B，假设它被定义为 F[A]上的一个方法，那么我将很难写出 *flatten 的签名。它必须是一个函数单元→ F[A]，只定义在 F[F[A]]的实例上，这使得事情变得笨拙。通过写下“中性”签名，事情应该是清楚的。*

无论如何，我们将坚持使用 flatMap 版本，我们将继续把 *Map 的*签名视为 A → B，而不是 F[A] → A → B，这意味着 F[A]在调用时是已知的(它是我们调用方法 map()的实例)。

#### 暴露单子

我之前说过，Scala 中没有 monad 类型类这种东西。但是这并不意味着 Scala 中没有单子。单子不是一个类或一个特征；单子是一个概念。每一个为我们提供两个心爱的操作的“包装器”，即*单元、*和*平面图*，本质上都是一个单子(好吧，仅仅提供这些名字的方法是不够的，它们当然必须遵循一定的规则，但是我们将会谈到这一点)。

我想我们现在终于可以把两个和两个放在一起，并意识到**列表是一个单子**！让它深入人心。多曲折的情节啊。就像在那部电影里，当你意识到一直都是他。但是等等——还有其他人！设置？单子。选项？单子。未来？单子！(*)

好吧，这是一部大众阴谋电影。单子无处不在！

未来是否真的是单子还有一点争议。由于这是一个初学者友好的文本，我就只说这是一个单子，并呼吁一天。

如果来自“未来不是真的”阵营的人正在读这篇文章，我相信你会同意这不是争论的地方。也许有一天我也会写一篇关于这个的文章。

我同意他们违反了一些基本的函数式编程原则，比如引用透明性，这一点不应该被忽视。但是让我们把那个留到其他时间。

当你近距离观察它们时，你会发现每一个都有一个 flatMap 方法。单位呢？请记住，unit 是一个从 A 类型的对象创建 monad M[A]的操作，这意味着一个简单的 apply()可以作为一个完美的单元。因此，如果我们有一个名为 x 的对象，下面是各种单子中的单元操作(回想一下，有一个语法糖允许我们调用 List.apply(3) as List(3)):

```
List:          Set:          Option:                 Future:
------------------------------------------------------------------
List(x)        Set(x)        Some(x) or Option(x)    Future(x)
```

此外，正如我前面所说，在普通 Scala 中没有实际的 monad 类型类。列表、选项、未来等结构。不要扩展任何特殊的单子性状(不存在)。这意味着他们没有义务向我们提供名为*单元*和*平面图*的方法。通过观察它们，看到它们具有正确签名和行为的方法 unit()和 flatMap()，我们可以推断它们实际上是单子。

我能听到你说“但是如果 Scala 中没有实际的 unit()方法(既然我们说 monad 的伴生对象中 apply()充当 unit)，那你说 Haskell 中的“ *return* ，Scala 中的 *unit* 是什么意思？什么，*单元*是一个不存在的函数的名字？"

这是一个很好的观察，是的，你是非常正确的。“Unit”只是 Scala 中引用 monad 的 identity 操作的约定。如果你愿意，你可以创建一个非常好的自定义单子，并调用它的方法*enving*和 *dropTheBass* ，而不是 *unit* 和 *flatMap* 。只要他们有正确的签名，并做他们应该做的事情，这在概念上就是一个单子。但是约定是个好东西，Scala 社区接受了绑定操作的术语 *flatMap* (如 List，Option，Future 所示)和标识操作的术语 *unit* (按照约定实现为 apply())。

我说，“只要他们有合适的签名，做他们应该做的事情”。好的，我们讨论了签名部分，但是我们从来没有真正指定这些方法到底要做什么。我的意思是，我们*讨论了*他们应该如何表现，但这并不足以更具体地定义他们的需求。这就是**单子定律**发挥作用的地方。如果我们的单子真的是一个真正的单子，那么*单元*和*平面图*必须遵守这些法则。

因为这是一篇初学者友好的文章，所以我将不会详细介绍这些定律背后的理论，甚至不会证明它们的正确性。现在重要的是你知道他们的存在。如果你推迟练习它们，直到你得到一些练习，这是可以的。

所以，如果我们有一些基本值 *x* ，一个单子实例 *m* (保存一些值)和函数 *f* 和*g*Int 类型 *Int → M[Int]* ，我们可以写如下的法则:

*   **左同一律** :
    *单位(x)。平面地图(f) == f(x)*
*   **右同一律** :
    *m.flatMap(单位)== m*
*   **结合律** :
    *m.flatMap(f)。flat map(g)= = m . flat map(x f(x)。平面地图(g))*

好吧。到目前为止，我们已经实现了我在本文中提出的三个目标中的两个。我们解释了单子的概念，并在 Scala 中画了一个类似于现实生活中单子的例子。顺便说一下，在开始我只提到 IO-monad 作为一个具体 monad 的例子。我想推迟提及其他人，直到你有了一个大致的概念。

如果你想知道 IO-monad 到底是什么，它是一个非常复杂的小东西，用于 Haskell 等纯函数式语言中的 IO 操作。现在不是深入探讨这个问题的时间和地点。

第三个目标的时间到了——单子为什么有用？

#### 实践中的单子:选项

在这一节，我将展示两个单子，期权和期货。

我们从期权开始。您可能知道，Option 是一个构造，它允许我们在 Scala 中避免空指针(在 Haskell 中它被称为 Maybe)。我们用它来描述可能有也可能没有定义值的事物。如果定义了一个值，option 等于 Some(value)，如果没有定义，它等于 None。

假设我们有一群用户存储在某个数据库中。我们还有一个服务，可以使用 loadUser()方法从数据库中加载用户。它接受一个名称，并为我们提供一个选项[User],因为具有该名称的用户可能存在，也可能不存在。

每个用户可能有也可能没有孩子(为了这个例子，我们假设这个州有一条法律，允许最多有一个孩子)。请注意，该子元素也属于 User 类型，因此它也可以有一个子元素。

最后但同样重要的是，我们有一个简单的函数 *getChild* ，它返回给定用户的孩子。

```
trait User {
  val child: Option[User]
}

object UserService {
  def loadUser(name: String): Option[User] = { /** get user **/ }
}

val getChild = (user: User) => user.child
```

现在，假设我们想从数据库加载一个用户，如果他们存在，我们想看看他们是否有孙子。我们需要调用这三个函数:

string*→*Option[User]//load from db
User*→*Option[User]//get child
User*→*Option[User]//get child 的 child

这是代码。

```
val result = UserService.loadUser("mike")
  .flatMap(getChild)
  .flatMap(getChild)
```

如果你不知道如何平面映射一个单子，你可能会写几个嵌套的 if-then-else 分支，检查选项是否被定义。这没有错，但是这要优雅、简洁得多，并且符合函数式编程的精神。

好了，让我们仔细看看我们的“可选用户”monad。记住我们之前学的内容。这里有个类比。

```
generic monad:
--------------
unit:     A => M[A]           
flatMap: (A => M[B]) => M[B]

our monad:
-------------- 
unit:     User => Option[User]
flatMap: (User => Option[User]) => Option[User]
```

顺便说一下，你也可以把那些函数写成就地的 lambda 函数，而不是先验地定义它们。然后代码变成这样:

```
val result = UserService.loadUser("mike")
  .flatMap(user => user.child)
  .flatMap(user => user.child)
```

或者更简洁地说:

```
val result = UserService.loadUser("mike")
  .flatMap(_.child)
  .flatMap(_.child)
```

您还可以使用 for-comprehension，它基本上是用于映射、平面映射和过滤的语法糖。不想跑题太多所以这里就不解释了，你可以去查；我只给你看代码。

```
val result = for {
  user             <- UserService.loadUser("mike)
  usersChild       <- user.child
  usersGrandChild  <- usersChild.child
} yield usersGrandChild
```

如果你觉得这一切有点混乱，摆弄自己的代码会有很大帮助。您可以创建一些虚拟用户，向 UserService.loadUser()添加一个基本实现，以便它返回其中一个用户。让他们抚养一大堆孩子和孙子，然后把他们的生活搞得一团糟。

#### 实践中的单子:未来

Future 是一些异步操作的包装器。一旦未来已经完成，你可以做任何你需要做的事情。

使用期货有两种主要方式:

*   使用 future.onComplete()定义一个回调，它将处理未来的结果(不太酷)
*   使用 future.flatMap()简单地说明在 future 完成后应该对结果执行哪些操作(更清晰、更强大，因为您可以返回上一次操作的结果)

继续我们的例子。我们有一个在线商店，客户下了成千上万的订单。对于每个客户，我们现在必须获得他/她的订单，检查订单是针对哪个项目的，从数据库中获得相应的项目，进行实际的购买，并将购买操作的结果写入日志。让我们看看代码。

```
// needed for Futures to work
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

trait Order
trait Item
trait PurchaseResult
trait LogResult
object OrderService {  
  def loadOrder(username: String): Future[Order] 
}

object ItemService {  
  def loadItem(order: Order): Future[Item] 
}

object PurchasingService { 
  def purchaseItem(item: Item): Future[PurchaseResult]
  def logPurchase(purchaseResult: PurchaseResult): Future[LogResult] 
}
```

顺便说一下，不要介意从函数中引用全局对象。我知道这不是最佳做法。但这完全离题了。另外，请注意，上面的代码无法编译，因为为了清楚起见，我省略了服务方法的实现。同样，如果您想自己玩这个例子(我推荐)，您可以自己创建模拟实现。例如 def load Item(Order:Order)= Future(new Item { })。

现在，与选项示例类似，我们将使用几个函数。它们非常简单，因为每一个都只是从相应的服务中调用一个方法。

```
val loadItem: Order => Future[Item] = {
  order => ItemService.loadItem(order)
}

val purchaseItem: Item => Future[PurchaseResult] = {
  item => PurchasingService.purchaseItem(item)
}

val logPurchase: PurchaseResult => Future[LogResult] = {
  purchaseResult => PurchasingService.logPurchase(purchaseResult)
}
```

我们需要加载给定客户的订单，获取有问题的商品，购买该商品并记录结果。这很简单，因为:

```
val result = 
  OrderService.loadOrder("customerUsername")
  .flatMap(loadItem)
  .flatMap(purchaseItem)
  .flatMap(logPurchase)
```

下面是同样易于理解的替代方法，使用直接服务方法调用代替上面使用的函数:

```
val result =
  for {
    loadedOrder    <- orderService.loadOrder(“customerUsername”)
    loadedItem     <- itemService.loadItem(loadedOrder)
    purchaseResult <- purchasingService.purchaseItem(loadedItem)
    logResult      <- purchasingService.logPurchase(purchaseResult)
  } yield logResult
```

就是这样。我希望我能对单子的神秘有所了解。

#### 结论

Monad 拥有两种武器，单位和平面图，是一个非常强大的家伙。当然，它们不能解决你所有的问题。但是以这种方式思考(使用 map、flatMap、filter 等链接操作和操纵数据。伴随着其他函数式编程结构，比如模式匹配)确实提高了您对代码的推理能力，并减少了代码中的错误数量。

考虑到代码被阅读的次数比被编写的次数多得多，这些代码的可读性和清晰性是一个很大的优势。例如，下面是我今天工作时写的代码的摘录(我改了名字):

```
itemService.loadItems(order).flatMap {

  case Success(items) =>
    val updateResults = items.map { item =>
      itemService.purchase(item, order.owner)
    }
    Future.sequence(updateResults)
      .map(toPurchaseResults(_))
      .map(mergeResults(_))

  case RepositoryFailure(failure) =>
    Future(Failure(Json.obj(Failed -> FailureLoadingItems)))

}
```

忘记纠缠的 if 分支，嵌套循环和它们的一个错误以及[回调地狱](http://callbackhell.com/)。这段代码更简单、更漂亮，而且在 Scala 编译器的帮助下，*在你第一次运行它的时候就能工作*。

你可以在这里瞥见一些其他的范畴理论结构[，或者在这里](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)阅读更详细的内容[。](http://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)

关于函数式编程，如果你喜欢 Scala，你可以从这里的[开始。](https://www.manning.com/books/functional-programming-in-scala)

也有一些很好的库提供了函数式编程构造，比如 [Scalaz](https://github.com/scalaz/scalaz) (前面已经提到过)和[Cats](https://github.com/non/cats)(block 上更小的孩子)，所以试试用它们吧；你可以在这里找到一些教程。

如果你想认真地参与函数式编程，你迟早会不得不[学习 Haskell](http://learnyouahaskell.com/chapters) (以防你还不知道)。

这是我的邮箱:sinisalouc[at]gmail[dot]com。如果您发现任何错误，认为某些特定部分需要改进或只是想取得联系，请随时与我联系。