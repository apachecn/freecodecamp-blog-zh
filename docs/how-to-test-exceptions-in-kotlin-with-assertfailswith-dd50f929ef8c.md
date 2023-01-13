# 如何用 assertFailsWith 测试 Kotlin 中的异常

> 原文：<https://www.freecodecamp.org/news/how-to-test-exceptions-in-kotlin-with-assertfailswith-dd50f929ef8c/>

丹尼尔·牛顿

# 如何用 assertFailsWith 测试 Kotlin 中的异常

![1*97MX1nM7-TWatCX9pbU-Kw](img/8421c8cf73facad58605635cb1850653.png)

[Photo](https://pixabay.com/en/late-autumn-snow-snowfall-lake-web-3841454/) by [Stefan Schweihofer](https://pixabay.com/en/users/stux-12364/)

我想写这篇短文来强调 Kotlin 可用的`assertFailsWith`功能。这个函数使得测试异常变得更加容易。测试异常对于 JVM 语言来说并不新奇(从现在开始，我将使用 Java 进行比较)。Kotlin 附带了一个很好的额外好处，即作为其标准库的一部分提供该功能。与 Java 相比，你可能会将 [AssertJ](http://joel-costigliola.github.io/assertj/) 带入组合中以获得类似的结果。

这篇文章的主要目的是让你了解`assertFailsWith`功能。我个人暂时不知道它的存在，默认依赖 AssertJ。也就是说，我并不反对 AssertJ。该库还提供了许多其他功能。对于这个特定的实例，也许可以删除它(假设您没有将它用于任何其他用途)。

一般来说`assertFailsWith`和 AssertJ 有什么好的地方？它提供了比 JUnit 提供的简单结构更好的异常测试。更准确地说，它允许您指定您希望在测试的哪个部分抛出异常，而不是声明异常将在代码的某个地方出现。这可能导致异常在不正确的点上被测试错误地吞下，并欺骗您认为它像您认为的那样工作。

现在我已经把要点说清楚了，让我们继续这篇文章的主要内容。下面是`assertFailsWith`在测试中的样子:

在这个例子中，`hereIsAnException`被放置在`assertFailsWith`的主体中，T1 检查是否抛出了一个`IllegalArgumentException`。如果没有引发，那么断言将失败。如果确实发生了，那么断言将通过，异常被捕获。

捕捉异常允许测试代码在需要时继续执行，也允许您对异常的状态做出进一步的断言。

例如，它是另一个异常的包装器吗(它的`cause`属性的类型是什么)？

消息是您所期望的吗(不是最可靠的检查)？

只有与`assertFailsWith`指定的类型或子类型相同的异常才会被捕获。任何其他错误都会导致测试失败。既然它捕捉子类型，请不要只指定`Exception`或`RuntimeException`就走来走去。尽量精确，让你的测试尽可能有用。

如前所述，`assertFailsWith`只会捕捉在函数体内抛出的异常。因此，如果改为这样写:

测试会失败。`hereIsAnException`抛出了一个异常，该异常未被捕获并导致测试失败。我相信这是这种函数相对于以前的方式最好的部分(例如，在`@Test`内部断言一个异常将会发生)。

我个人从未真正使用过断言的消息部分。也许你知道，所以，我想我至少应该让你知道。

在我总结这篇文章中的少量内容之前，让我们快速地看一下 AssertJ，这样我们可以对两者进行比较。同样，这只适用于捕捉异常的情况，这只是 AssertJ 提供的一小部分功能。

这比`assertFailsWith`版本略显“冗长”。但是，AssertJ 提供的大量函数弥补了这一点，使得对返回异常的进一步检查变得更加容易。更准确地说，当使用`assertFailsWith`时，我需要编写另一个断言来检查消息。在 AssertJ 中，这只是一个链接到上一次调用末尾的函数。

总之，`assertFailsWith`是一个很好的小函数，可以在测试中用来确保一段代码抛出特定类型的异常。它内置于 Kotlin 标准库中，这样就不需要在项目中引入额外的依赖项。也就是说，它是一个相对简单的函数，并不具备像 AssertJ 这样的库所具备的功能。这可能就足够了，直到您想要编写包含广泛范围或断言的测试，因为这是可能会变得混乱的地方。

如果你对[科特林文件感兴趣，可以在这里找到`assertFailsWith`的官方文件。](https://kotlinlang.org/api/latest/kotlin.test/kotlin.test/assert-fails-with.html)

如果你觉得这篇文章很有帮助，你可以在 Twitter 上关注我，地址是 [@LankyDanDev](https://twitter.com/LankyDanDev) 来了解我的新文章。

[查看丹·牛顿的所有帖子](https://lankydanblog.com/author/danknewton/)

*最初发布于 2019 年 1 月 26 日[lankydanblog.com](https://lankydanblog.com/2019/01/26/testing-exceptions-in-kotlin-with-assertfailswith/)。*