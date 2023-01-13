# 为什么测试驱动开发有用？

> 原文：<https://www.freecodecamp.org/news/why-test-driven-development-4fb92d56487c/>

#### 关于如何更有效地应用 TDD 的提示，以及为什么它是一种有价值的技术

当我们使用 TDD 开始一个项目时，我们遵循一个共同的模式。我们以特殊测试的形式描述我们期望系统做什么的规范。这种“特殊测试”可以是与前端的端到端测试，也可以是执行 HTTP 请求来测试后端的集成测试。

这是我们写的第一个测试。我们在编写一行代码之前就这样做了。这个特殊的测试将作为一个指导方针，确保我们不会破坏任何阻止正常流程工作的东西。如果我们不这样做，仅仅依靠单元测试，最终，我们可能会通过所有的测试，但是服务器不会启动，或者用户不能在屏幕上做任何事情。

> 当使用 TDD 开始一个项目时，有一个通用的模式来创建一个特殊的测试，以确保我们不会破坏任何阻止常规流程工作的东西。

在我们用一个简单的实现通过了那个特殊的测试之后(或者如果我们使用 [ATDD](https://www.agilealliance.org/glossary/atdd/) 来驱动应用内部的话，我们可以让它失败)，我们开始在微观层面上使用一个相似的模式来构建系统的单元，从不破坏我们之前创建的任何测试。我们通过一个失败的测试来描述系统的每个单元，并首先通过一个简单的实现。然后，我们识别[气味](https://medium.com/@fagnerbrack/code-smell-92ebb99a62d0)，如果有必要的话[重构](https://medium.com/@fagnerbrack/how-to-refactor-a-public-interface-317ed18d38a3)，这样我们就可以一遍又一遍地循环。

这就是所谓的 TDD 的[红/绿/重构周期。](http://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)

这个周期将驱使我们以足够的信心构建我们应用程序的所有部分，它将是健壮的和可维护的。如果我们由于对 API 应该如何运行的错误假设而停滞不前，它还会在早期暴露问题。

有一件重要的事情我们应该小心:我们应该**避免[重构](https://medium.com/@fagnerbrack/how-to-refactor-a-public-interface-317ed18d38a3)代码或者在另一个测试失败的时候增加一个新的测试。**如果我们这样做，我们很有可能会因为担心我们已经讨论过的另一条规则而陷入不必要的认知负担。为了防止这种情况，我们需要在开始任何其他事情之前修复失败的测试。

> 在 TDD 中，我们应该**避免[重构](https://medium.com/@fagnerbrack/how-to-refactor-a-public-interface-317ed18d38a3)代码或者在另一个测试失败时增加一个新的测试。**

有些情况下，人们更喜欢在编写代码之后编写测试。然而，这种方法也有一些负面影响:

*   我们可能会错过重要的功能，因为很难知道覆盖率是否符合我们的期望。
*   它会[产生假阳性](https://medium.com/@fagnerbrack/mocking-can-lean-to-nondeterministic-tests-4ba8aef977a0),因为我们不会首先看到失败的测试。
*   这可能会让我们[过度设计](https://hackernoon.com/how-to-accept-over-engineering-for-what-it-really-is-6fca9a919263)架构，因为我们没有任何指导方针来强迫我们编写最少的代码来满足我们最基本的需求。
*   很难验证失败测试的消息是否清晰，是否指出了失败的原因。

要记住的一件事是，TDD 可以作为一个规程，但是[没有办法在产品代码](http://blog.cleancoder.com/uncle-bob/2017/03/07/SymmetryBreaking.html)之后创建一个编写测试的规程。

有[种情况，应用 TDD 或自动化测试](https://8thlight.com/blog/uncle-bob/2014/04/30/When-tdd-does-not-work.html)根本没有价值。当我们测试一些 IO 层、测试的支持函数，或者使用声明性语言(如 HTML 或 CSS)构建的东西时(我们可以测试 CSS 中的视觉，但不能测试 CSS 代码)。然而，测试是确保一个复杂的功能满足一组期望的过程的基本部分。仅此一点就让我们有足够的信心，系统的每个部分都按预期工作。

> 有些情况下，应用 TDD 或者自动化测试根本没有价值，比如测试 IO 层、测试的支持函数或者用声明性语言编写的代码。

有一个概念叫做[转型优先前提](https://8thlight.com/blog/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html)。[TL；DR](https://en.wikipedia.org/wiki/TL;DR) 是当在 TDD 周期的“绿色”阶段使代码更通用时，我们可以应用一些转换。

“重构”是指我们改变代码的结构，而不改变它的行为。这些转换没有被称为“[重构](https://medium.com/@fagnerbrack/how-to-refactor-a-public-interface-317ed18d38a3)，因为它们改变了代码的结构 ****和行为**** 以使其更加通用。

使用转换优先级的一个例子是，当我们进行一个测试，强制我们从返回一个单一的常量到返回一个包含多个值的参数。在这种情况下，它的 ****常数- >标量**** 优先变换。

> 那么这些转变是什么呢？或许我们可以把它们列成一个清单:
> 
> ******({ }–>nil)****完全没有代码- >代码采用 nil*
> 
> ******(nil->常数)*****
> 
> ******(常数- >常数+)**** a*
> 
> ******(无条件- > if)**** 拆分执行路径*
> ******(标量- >数组)*****
> 
> ******(数组- >容器)*****
> 用函数或算法替换表达式
> 
> ******(变量- >赋值)**** 替换变量的值。*
> 
> *还有可能是别人。*
> 
> *—摘自[转型优先前提](https://8thlight.com/blog/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html)篇*

> 在 TDD 中，转换优先的前提可以给我们一个“绿色”阶段的指导方针。

编写正确的软件很难。TDD 是一种常见的模式，在这种模式下，我们使用测试来帮助驱动我们系统的实现，同时保留大部分的测试覆盖率。然而，这不是[的银弹](https://medium.com/@fagnerbrack/how-to-reject-the-belief-on-the-silver-bullet-1d86b686acbb)。

如果我们使用 TDD，我们应该避免在测试失败时重构代码。为了让它通过“绿色”阶段，我们使用转换优先级前提来指导我们在[重构](https://medium.com/@fagnerbrack/how-to-refactor-a-public-interface-317ed18d38a3)之前可以采取的最简单的实现方法。

与其他编写测试的方式相比，TDD 在开始时会花费更多的时间。然而，正如每一项新技术一样，通过足够的实践，我们将到达一个平台，应用 TDD 所花费的时间将与用传统方式编写测试所花费的时间没有什么不同。

现在的不同之处在于，你的软件不太可能以你意想不到的方式运行。

对于所有实际的手段来说，这与 100%的测试覆盖率没有什么不同。

感谢阅读。如果您有任何反馈，请通过 [Twitter](https://twitter.com/FagnerBrack) 、[脸书](https://www.facebook.com/fagner.brack)或 [Github](http://github.com/FagnerMartinsBrack) 联系我。