# 一种测试 Kotlin 中令人沮丧的静态方法调用的无压力方法

> 原文：<https://www.freecodecamp.org/news/a-stress-free-way-to-test-frustrating-static-method-calls-in-kotlin-81db43e7ed82/>

作者奥莱克西·费多罗夫

# 一种测试 Kotlin 中令人沮丧的静态方法调用的无压力方法

让我大胆猜测一下…你在 Kotlin 中遇到过一些代码正在使用某个第三方库。该库提供的 API 是一个或几个静态方法。你想用这些静态方法测试一些代码。这是痛苦的。

你不确定如何处理这个问题。

也许你会问自己，“第三方库作者什么时候会停止使用静态方法？”

不管怎样，我有什么资格告诉你如何在 Kotlin 中测试静态方法调用？

在过去的五年里，我是一个测试和测试驱动开发的狂热拥护者——他们称我为 [TDD 研究员](http://tddfellow.com/)是有原因的。在写这篇文章的时候，我已经和 Kotlin 一起工作两年了。

前进！

这就是我看到如此糟糕的 API 时的感受:

![VqR1mKMGNy9Q80UjC7KRGAB3I4WVgDA428t6](img/d09a9927a9b100c3ecfb4747300b27f7.png)

*(source: pexels.com)*

让我用我最近处理的一个粗略的例子来说明我的意思。图书馆是一个客户。为了使用它，我必须调用某个类的静态方法。如果简化，它看起来像这样:

```
NewRelicClient.addAttributesToCurrentRequest(“orderId”, order.id)
```

我需要改变我们正在发送的内容，我必须添加更多的属性。因为我想确信我的改变不会破坏任何东西，并且完全按照我的想法去做，所以我需要写一个测试。这个代码还没有测试。

如果你还在阅读，我想你也是同样的情况。或者你曾经在过去。

我同意这是一个痛苦的情况。

我该如何在测试中模拟这些电话？

我知道，令人沮丧的是大多数模仿库不能模仿静态方法调用。即使是在 Java 中运行的也不一定能在 Kotlin 中运行。

有一些库可以做到这一点，例如`powermock,`。但是你知道吗？也许，你已经在使用`mockito`或其他库了。向项目中添加另一个嘲讽工具将会使事情变得更加混乱和令人沮丧。

我知道在同一个代码库中为同一个工作使用多个工具是多么令人讨厌。这对每个人都造成了极大的困惑。

这个问题在大约二十年前就已经解决了！

感兴趣吗？来兜兜风。

### 面向卑微对象的重构

让我们来看看我们正在处理的代码:

```
class FulfilOrderService {

    fun fulfil(order: Order) {

        // .. do various things ..

        NewRelicClient.addAttributesToCurrentRequest(
                "orderId", order.id)
        NewRelicClient.addAttributesToCurrentRequest(
                "orderAmount", order.amount.toString())

    }

}
```

它对订单执行各种操作，然后为当前的`newrelic`请求分配一些属性。

这里我们要一起做的第一件事是提取方法`addAttributesToRequest`。我们还想用`key`和`value`参数对其进行参数化。您可以手动完成，或者，如果您有幸使用 IntelliJ IDEA，您可以自动完成这样的重构。

以下是方法:

1.  选择`”orderId”`并提取一个本地变量。命名为`key`。
2.  选择`order.id`并提取一个本地变量。命名为`value`。
3.  选择`NewRelicClient.addAttributesToCurrentRequest(key, value)`并提取一个方法。命名为`addAttributesToRequest`。
4.  IntelliJ 将把对`NewRelicClient`的第二次调用突出显示为重复的，并告诉您可以用对新的私有方法的调用来替换它。IntelliJ 将询问您是否要这样做。动手吧。
5.  内嵌变量`key`和`value`。
6.  最后，让方法`protected`代替`private`。我稍后将向您展示为什么该方法必须受到保护。
7.  您会注意到 IntelliJ 用一个警告突出显示了`protected`。这是因为 Kotlin 中的所有类默认都是`final`。由于 final 类是不可扩展的，`protected`是无用的。IntelliJ 提供的解决方案之一是创建类`open`。动手吧。方法`addAttributesToRequest`也应该变得开放。

下面是你最终应该得到的:

```
open class FulfilOrderService {

    fun fulfil(order: Order) {

        // .. do various things ..

        addAttributesToRequest("orderId", order.id)
        addAttributesToRequest("orderAmount",
                               order.amount.toString())

    }

    protected open fun addAttributesToRequest(key: String,
                                              value: String) {

        NewRelicClient.addAttributesToCurrentRequest(key, value)

    }

}
```

请注意，所有这些重构都是完全自动的，因此执行起来是安全的。我们不需要测试来做这些。将该方法作为受保护的方法将使我们有机会编写一个测试:

```
private val attributesAdded = mutableListOf<Pair<String, String>>()

private val subject = FulfilOrderService()

@Test
fun `adds order id to the current request within newrelic`() {

    val order = Order(id = "some-id", amount = 142)

    subject.fulfil(order)

    val expectedAttributes = listOf(
            Pair("orderId", "some-id"),
            Pair("orderAmount", "142"))
    assertEquals(expectedAttributes, attributesAdded)

}
```

说到测试和重构…

你想学习如何用 Kotlin 写验收测试吗？也许，如何利用 IntelliJ IDEA 的力量为你所用？

也许，你想学习如何用 Kotlin 很好地构建应用程序？—是命令行、web 还是 android 应用？

我无意中写了一本关于 Kotlin 入门的终极教程电子书。350 页的实践教程，你可以跟随。

你会觉得好像我正和你坐在一起，我们在享受我们的时光，同时构建一个成熟的命令行应用程序。

感兴趣吗？

在这里下载[终极教程](https://iwillteachyoukotlin.com/)。顺便说一句，它是免费的，而且永远是免费的！

回到我们的测试。

这一切看起来都是正确的，但是它不起作用，因为没有人向列表`attributesAdded`添加任何元素。既然我们有了那个最小的受保护的方法，我们就可以“侵入它”:

```
private val subject: FulfilOrderService = object :
                                          FulfilOrderService() {

    override fun addAttributesToRequest(key: String,
                                        value: String) {

        attributesAdded.add(Pair(key, value))

    }

}
```

如果你运行测试，它通过。您可以更改测试或生产代码中的值来查看失败，并确保它确实在测试您认为它在做的事情。

让我们看看完整的测试代码:

```
import org.junit.Assert.*
import org.junit.Test

@Suppress("FunctionName")
class FulfilOrderServiceTest {

    private val attributesAdded = 
            mutableListOf<Pair<String, String>>()

    private val subject: FulfilOrderService = object :
                                      FulfilOrderService() {

        override fun addAttributesToRequest(key: String,
                                            value: String) {

            attributesAdded.add(Pair(key, value))

        }

    }

    @Test
    fun `adds order id to the current request within newrelic`() {

        val order = Order(id = "some-id", amount = 142)

        subject.fulfil(order)

        val expectedAttributes = listOf(
                Pair("orderId", "some-id"),
                Pair("orderAmount", "142"))
        assertEquals(expectedAttributes, attributesAdded)

    }

}
```

这里刚刚发生了什么？

看，我做了一个稍微不同版本的`FulfilOrderService`类——一个可测试的。这种测试方法的唯一缺点是，如果有人搞砸了`addAttributesToRequest`函数，没有测试会中断。

另一方面，该函数将永远不会包含一行以上的简单代码，并且可能不会经常更改。只有当我们使用的第三方库的作者打算对这个方法进行突破性的改变时，这种情况才会发生。

这不太可能。大概每隔几年都会发生。

你知道吗？

即使您确实测试了比我在这里提供的更“黑盒子”的东西，当这样的突破性变化出现时，您仍然必须重新访问所有的用法并修复它们。很可能，你也需要扔掉或重写所有相关的测试。

哦，如果出现这样的突破性变化，我仍然建议至少手动测试一次，看看你是否正确理解了新的 API，以及它是否以你认为应该的方式与第三方系统交互。

考虑到所有这些信息，我想应该可以不测试那一行。

但是，如果这种变化在街区附近出现，你是否必须寻找所有我们需要去的地方？

简而言之，是的。

长回答:在目前的设计中——是的。但是你认为我们已经谈完了吗？

没有。

现在的设计很糟糕。让我们通过提取不起眼的物体来解决这个问题。一旦我们这样做了，在整个代码库中将只有一个地方需要改变——那个不起眼的对象。

不幸的是，IntelliJ 还不支持 Kotlin 的`Move method`或`Extract method object`重构，所以我们必须手动执行这个。

但是你知道吗？—没关系，因为我们已经有相关的测试支持我们了！

为了进行`Extract method object`重构，我们需要用对象创建来替换方法内部的实现，并使用与重构方法相同的参数立即调用该对象的方法:

```
protected open fun addAttributesToRequest(key: String,
                                          value: String) {

//   NewRelicClient.addAttributesToCurrentRequest(key, value)
    NewRelicHumbleObject().addAttributesToRequest(key, value)

}
```

然后我们需要创建这个类，并在其上创建方法。最后，我们将把重构方法的内容，也就是我们注释掉的内容，放到新创建的方法中；不要忘记删除评论，因为我们不再需要它:

```
class NewRelicHumbleObject {

    fun addAttributesToRequest(key: String, value: String) {

        NewRelicClient.addAttributesToCurrentRequest(key, value)

    }

}
```

我们已经完成了这一步的重构，现在应该运行我们的测试了。如果我们没有犯任何错误，它们都应该通过——它们确实犯了错误！

这个重构的下一步是将简单对象的创建转移到这个领域。在这里，我们可以执行自动重构，从表达式`NewRelicHumbleObject()`中提取字段。这就是重构后应该得到的结果:

```
private val newRelicHumbleObject = NewRelicHumbleObject()

protected open fun addAttributesToRequest(key: String,
                                          value: String) {

    newRelicHumbleObject.addAttributesToRequest(key, value)

}
```

现在，因为我们在字段中有了那个值，我们可以把它移到构造函数中。这也有一个自动化的重构！它叫做`Move to constructor`。您应该会得到以下结果:

```
open class FulfilOrderService(
        private val newRelicHumbleObject: NewRelicHumbleObject =
                                          NewRelicHumbleObject()) {

    fun fulfil(order: Order) {

        // .. do various things ..

        addAttributesToRequest("orderId", order.id)
        addAttributesToRequest("orderAmount",
                               order.amount.toString())

    }

    protected open fun addAttributesToRequest(key: String,
                                              value: String) {

        newRelicHumbleObject.addAttributesToRequest(key, value)

    }

}
```

这将使得从测试中注入依赖性变得非常简单。注意，它是一个普通的对象，有一个非静态的方法。

你知道这意味着什么吗？

是啊！你可以用你最喜欢的嘲笑工具来嘲笑它。让我们现在就做吧。在这个例子中，我将使用`mockito`。

首先，我们需要在测试中创建模拟:

```
private val newRelicHumbleObject =
        Mockito.mock(NewRelicHumbleObject::class.java)
```

为了能够模仿我们不起眼的对象，我们必须将它的类`open`和方法`addAttributesToRequest`也开放:

```
open class NewRelicHumbleObject {

    open fun addAttributesToRequest(key: String, value: String) {
        // ...

    }

}
```

然后我们需要将这个模拟作为参数提供给`FulfilOrderService`的构造函数:

```
private val subject = FulfilOrderService(newRelicHumbleObject)
```

最后，我们想用`mockito`的验证来代替我们的断言:

```
Mockito.verify(newRelicHumbleObject)
        .addAttributesToRequest("orderId", "some-id")
Mockito.verify(newRelicHumbleObject)
        .addAttributesToRequest("orderAmount", "142")
Mockito.verifyNoMoreInteractions(newRelicHumbleObject)
```

在这里，我们验证了我们卑微的对象的方法`addAttributesToRequest`已经被调用了两次，调用时只使用了适当的参数，没有使用任何其他参数。我们不再需要`attributesAdded`字段，所以让我们去掉它。

下面是您现在应该得到的内容:

```
class FulfilOrderServiceTest {

    private val newRelicHumbleObject =
            Mockito.mock(NewRelicHumbleObject::class.java)

    private val subject = FulfilOrderService(newRelicHumbleObject)

    @Test
    fun `adds order id to the current request within newrelic`() {

        val order = Order(id = "some-id", amount = 142)

        subject.fulfil(order)

        Mockito.verify(newRelicHumbleObject)
                .addAttributesToRequest("orderId", "some-id")
        Mockito.verify(newRelicHumbleObject)
                .addAttributesToRequest("orderAmount", "142")
        Mockito.verifyNoMoreInteractions(newRelicHumbleObject)

    }

}
```

既然我们不再重写受保护的方法，我们可以内联它。对了，课不用再`open`了。我们的`FulfilOrderService`类现在准备好接受我们想要做的更改，因为它现在是可测试的(至少在`newrelic`请求属性方面):

```
class FulfilOrderService(
        private val newRelicHumbleObject: NewRelicHumbleObject = 
                                          NewRelicHumbleObject()) {

    fun fulfil(order: Order) {

        // .. do various things ..

        newRelicHumbleObject.addAttributesToRequest(
                "orderId", order.id)
        newRelicHumbleObject.addAttributesToRequest(
                "orderAmount", order.amount.toString())

    }

}
```

为了准确起见，让我们再做一次所有的测试吧！—都过去了。

太好了，我想我们到此为止了。

### 分享你对卑微物体的看法！

感谢您的阅读！

如果你能在评论中分享你对这种重构的看法，我会很高兴的。你知道更简单的重构方法吗？—分享！

此外，如果你喜欢你所看到的，可以考虑在 Medium 上给我鼓掌，并在社交媒体上分享这篇文章。

如果你对学习科特林感兴趣，并且喜欢我的写作风格，[请阅读我的入门教程。](https://iwillteachyoukotlin.com/)

### 我的相关文章

Kotlin 的“@Deprecated”如何减轻巨大重构的痛苦？
[*我要给你讲一个我们如何节省大量时间的真实故事。科特林的@弃用重构的力量……*](https://hackernoon.com/how-kotlins-deprecated-relieves-pain-of-colossal-refactoring-8577545aaed)
[hackernoon.com](https://hackernoon.com/how-kotlins-deprecated-relieves-pain-of-colossal-refactoring-8577545aaed)

[**kot Lin 灾难如何像闪电一样吞噬你的 Java 应用？**](https://hackernoon.com/how-kotlin-calamity-devours-your-java-apps-like-cancer-f3ce9500a028)
[*我听到你在说什么了。有传言称 Android 积极采用 Kotlin 作为主要编程…*](https://hackernoon.com/how-kotlin-calamity-devours-your-java-apps-like-cancer-f3ce9500a028)
[hackernoon.com](https://hackernoon.com/how-kotlin-calamity-devours-your-java-apps-like-cancer-f3ce9500a028)

[**并行变更重构**](https://medium.com/@oleksii_f/parallel-change-refactoring-b83a2993ef26)
[*并行变更是一种重构技术，允许在一个安全的……*](https://medium.com/@oleksii_f/parallel-change-refactoring-b83a2993ef26)
[medium.com](https://medium.com/@oleksii_f/parallel-change-refactoring-b83a2993ef26)