# Scala 中任一单子的生存指南

> 原文：<https://www.freecodecamp.org/news/a-survival-guide-to-the-either-monad-in-scala-7293a680006/>

几个月前我开始使用 Scala。我最难理解的概念之一是`Either`单子。所以，我决定用它来玩，更好地了解它的力量。

在这个故事中，我分享了我所学到的东西，希望能帮助编码人员接近这种美丽的语言。

#### 要么是单子

`Either`是 Scala 中最有用的单子之一。[如果你想知道什么是单子](https://medium.com/@sinisalouc/demystifying-the-monad-in-scala-cc716bb6f534)，嗯…我不能在这里说细节，也许在未来的故事里！

想象一个包含计算的盒子。你在这个盒子里工作，直到你决定从中获得结果。

在这个具体的例子中，我们的`Either`框可以有两种“形式”。它可以是一个`Left`或者一个`Right`，这取决于它内部的计算结果。

我能听到你在问:“好吧，那它有什么用？”

通常的回答是:错误处理。

我们可以将一个计算放在`Either`中，并在出错时使其成为`Left`，或者在成功时成为包含结果的`Right`。使用`Left`表示错误，使用`Right`表示成功是一种惯例。我们用一些代码来理解这个吧！

[https://scalafiddle.io/html/sgAxvBI/0](https://scalafiddle.io/html/sgAxvBI/0)