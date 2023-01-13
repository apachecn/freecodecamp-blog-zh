# 什么时候应该使用 TypeScript？

> 原文：<https://www.freecodecamp.org/news/when-should-i-use-typescript-311cb5fe801b/>

去年夏天，我们不得不将庞大的代码库(18，000 多行代码)从 JavaScript 转换成 TypeScript。我学到了很多关于每一种的优点和缺点，以及什么时候使用一种比另一种更有意义。

*这篇文章现在有[日文](http://postd.cc/when-should-i-use-typescript/)和[中文](http://www.luxingyun.xyz/2016/08/17/%E4%BD%95%E6%97%B6%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8typescript/)两种版本。*

### 当使用 TypeScript 有意义时

#### 当你有一个大的代码库时

当你的代码库很大，并且不止一个人在项目中工作时，类型系统可以帮助你避免很多常见的错误。对于单页应用程序来说尤其如此。

任何时候一个开发人员可能引入突破性的变化，拥有某种安全机制通常是好的。

TypeScript [transpiler](https://www.stevefenton.co.uk/2012/11/compiling-vs-transpiling/) 揭示了最明显的错误——尽管它不会神奇地消除调试的需要。

如果您的代码库不是那么大，那么通过添加类型注释来扩大代码库可能没有意义。我已经将 180 多个文件从 JavaScript 转换为 TypeScript，在大多数情况下，它增加了大约 30%的总代码量。

#### 当你团队的开发人员已经习惯了静态类型的语言时

如果您或团队中的大多数人来自 C#或 Java 这样的强类型语言，并且不想完全依赖 JavaScript，那么 TypeScript 是一个不错的选择。

尽管我建议彻底学习 JavaScript，但没有什么能阻止你在不了解 Javascript 的情况下使用 TypeScript。事实上，TypeScript 是由制作 C#的那个人[创建的，所以语法是相似的。](https://en.wikipedia.org/wiki/Anders_Hejlsberg)

在我的公司，我们有一个 C#开发团队，他们用 C#和 [WPF](https://en.wikipedia.org/wiki/Windows_Presentation_Foundation) (基本上是桌面世界的前端开发工具)编写复杂的桌面应用程序。然后，他们被要求作为全栈开发人员加入一个 web 项目。因此，很快，他们就能够学习前端的 TypeScript，然后利用他们的 C#知识进行后端。

#### TypeScript 可以作为巴别塔的替代品

过去的微软使用标准工具——例如 Java 并在其中加入专有的非标准特性——在这种情况下产生了 J++。然后他们会试图强迫开发者在两者之间做出选择。

TypeScript 是完全相同的方法——这次是 JavaScript。顺便说一下，这不是微软的第一个 JavaScript 分支。1996 年，他们派生出 JavaScript，创建了 [JScript](https://en.wikipedia.org/wiki/JScript) 。

尽管这是一个不太常见的用例，但从技术上讲，使用 TypeScript transpiler 将 ES6 代码转换成 ES5 是可能的。这是可能的，因为 ES6 本质上是 TypeScript 的子集，TypeScript [transpiler](https://www.stevefenton.co.uk/2012/11/compiling-vs-transpiling/) 生成 ES5 代码。

Typescript 的 transpiler 生成可读性很好的 Javascript (EcmaScript 5)代码作为输出。这也是 Angular 2 团队选择 TypeScript 而不是 Google 自己的 Dart 语言的原因之一。

此外，TypeScript 还有一些 ES6 中没有的很酷的特性，比如枚举和在构造函数中初始化成员变量的能力。我不太喜欢继承，但是我发现在类中使用 *public、private、protected 和 abstract* 关键字很有用。TypeScript 有，ES6 没有。

我们的 C#开发人员认为能够编写一个 lambda 函数作为方法的主体是非常令人惊奇的——这消除了与关键字 *this* 相关的令人头痛的问题。

#### **当一个库或框架推荐 TypeScript 时**

如果您正在使用 Angular 2 或另一个推荐 TypeScript 的库，请使用它。看看[这些开发者在使用 Angular 2 半年后有什么说的](http://m12.io/blog/we-launched-angular-2-project)。

只需知道——即使 TypeScript 可以使用所有现成的 JavaScript 库——如果您想要良好的语法错误，您将需要从外部添加这些库的类型定义。幸运的是，[的好人已经用工具建立了一个社区驱动的回购协议。但这仍然是你设置项目时的一个额外步骤](http://definitelytyped.org/)

(顺便说一句:对于所有 JSX 的粉丝来说，看看 TSX 。)

#### **当你真的觉得需要速度的时候**

这可能会让您感到震惊，但是在某些情况下，TypeScript 代码可以比 JavaScript 执行得更好。让我解释一下。

在我们的 JavaScript 代码中，我们有很多类型检查。这是一个医疗技术应用程序，所以如果处理不当，即使是一个小错误也可能是致命的。所以很多函数都有这样的语句:

```
if(typeof name !== ‘string) throw ‘Name should be string’
```

有了 TypeScript，我们可以一起消除大量的类型检查。

这在我们之前遇到性能瓶颈的代码部分尤其明显，因为我们能够跳过许多不必要的运行时类型检查。

### 那么什么时候没有 Typescript 会更好呢？

#### **当你负担不起额外的营业税时**

目前还没有计划在浏览器中本地支持 TypeScript。Chrome [做了一些实验](https://developers.google.com/v8/experiments#soundscript)，但是后来[取消了](https://groups.google.com/forum/embed/?place=forum/strengthen-js#!topic/strengthen-js/ojj3TDxbHpQ)支持。我怀疑这与不必要的运行时开销有关。

如果有人想要辅助轮，可以安装。但是自行车不应该带有永久性的训练轮。这意味着，在浏览器中运行您的 TypeScript 代码之前，您总是必须转换它。

对于标准的 ES6，这是一个完全不同的故事。当 [ES6 被大多数浏览器](https://kangax.github.io/compat-table/es6/)支持时，现在的 ES6 到 ES5 的转换就变得没有必要了(更新:[的确是的](https://medium.freecodecamp.org/you-might-not-need-to-transpile-your-javascript-4d5e0a438ca)！).

ES6 是 JavaScript 语言最大的变化，我相信大多数程序员都会接受它。但是那些勇敢的少数人想要尝试下一版本 JavaScript 的实验性特性，或者还没有在所有浏览器上实现的特性——他们无论如何都需要传输文件。

没有 transpilation，你只需修改文件和刷新你的浏览器。就是这样。不需要*观看、* *按需传输、*或*构建系统*。

如果您选择 TypeScript，您将最终为您的 Javascript 库和框架的类型定义做一些额外的簿记工作(通过使用 DefinitelyTyped 或编写您自己的类型注释)。对于纯 JavaScript 项目来说，这是不需要做的事情。

#### **当你想避免怪异的调试边缘情况时**

Sourcemaps 让调试 Typescript 变得更加容易，但现状并不完美。真的有很烦很乱的边缘案例。

此外，调试“this”关键字和附加到它的属性也有一些问题(提示:“this”在大多数情况下都有效)。这是因为 Sourcemaps 目前对变量没有很好的支持——尽管这在将来可能会改变。

#### **当您想要避免潜在的性能损失时**

在我们的项目中，我们有 9000 多行优秀的旧 ES5 JavaScript，它们为 3D WebGL 画布提供了纯粹的马力。我们一直保持这种方式。

TypeScript transpiler(就像 Babel)具有需要生成额外代码的特性(继承、枚举、泛型、异步/等待等)。你的 transpiler 再好，也无法超越一个优秀程序员的优化。所以我们决定将它放在普通的 ES5 中，以便于调试和部署(无论如何不要进行任何转换)。

也就是说，对于大多数项目来说，与类型系统和更现代的语法的好处相比，性能损失可能可以忽略不计。但是在有些情况下，毫秒甚至微秒都很重要，在这些情况下，不推荐任何类型的 transpilation(即使是 Babel、CoffeeScript、Dart 等。).

注意，Typescript 没有为运行时类型检查添加任何额外的代码。所有的类型检查都发生在转换时，类型注释从生成的 Javascript 代码中删除。

#### **当您希望最大限度地提高团队的敏捷性时**

用 JavaScript 设置会更快。类型系统的缺乏使得改变东西变得敏捷和容易。这也让打碎东西变得更容易，所以确保你知道你在做什么。

Javascript 更加灵活。记住，类型系统的一个主要用例是使破坏东西变得困难。如果 Typescript 是 Windows，Javascript 就是 Linux。

在 JavaScript 领域，你没有类型系统的训练轮，计算机假定你知道你在做什么，但是允许你骑得更快，操作更容易。

如果您仍处于原型阶段，这一点尤为重要。如果是这样，就不要在 TypeScript 上浪费时间了。JavaScript 更加灵活。

记住，TypeScript 是 JavaScript 的超集。这意味着如果需要，以后可以很容易地将 JavaScript 转换成 TypeScript。

#### 我对 JavaScript 和 TypeScript 的偏好

总的来说，没有一种最好的语言。但是对于每个单独的项目，可能都有一个客观上最好的语言、库、框架、数据库和操作系统……你会明白的。

对于我们的项目，使用 TypeScript 是有意义的。我试图将我的一些爱好项目重构为 TypeScript，但这不值得。

我个人喜欢关于 TypeScript 的 5 点:

1.  完全兼容 ES6。很高兴看到微软与其他浏览器公平竞争。我们的生态系统可以受益于谷歌、Mozilla 和苹果的强劲对手。微软在这方面投入了大量精力——比如在所有平台中，使用谷歌 Chrome 上的 TypeScript 从头开始编写 Visual Studio 代码。
2.  类型系统是可选的。来自 C 和 Java 背景，我发现 JavaScript 中缺少类型系统是一种解放。但是当我在运行时遇到愚蠢的错误时，我讨厌浪费时间。TypeScript 允许我避免许多常见的错误，这样我就可以把时间集中在修复真正棘手的问题上。这是一个很好的平衡。我喜欢。这是我的口味。我尽可能地使用类型，因为它让我内心平静。但那是我。如果我使用 TypeScript，我不想局限于它的 ES6 特性。
3.  从 TypeScript transpiler 出来的 JavaScript 代码可读性很强。我不是 Sourcemaps 的粉丝，所以我的大部分调试工作都是在生成的 JavaScript 上进行的。绝对牛逼。我完全可以理解 Angular 2 [为什么选择 TypeScript 而不是 Dart](https://jaxenter.com/angular-typescript-dart-115426.html) 。
4.  TypeScript 的开发体验棒极了。 [VS Code](https://code.visualstudio.com/) 在处理 JavaScript 时非常聪明(有些人可能会争论它是最聪明的 JS IDE)。但是 TypeScript 将这种限制推到了一个全新的高度。VSCode 中的自动完成和重构功能更加准确，这并不是因为 IDE 非常智能。这都要感谢 TypeScript。
5.  类型注释就像一个基本级别的文档。它们声明每个函数期望的数据类型、它们的结果和各种其他提示，如`readonly`、`public`或`private`属性。根据我的经验，尝试将 JavaScript 代码重构为 TypeScript 时，我通常会得到结构更好、更干净的代码。事实上，编写 TypeScript 改进了我编写普通 JavaScript 的方式。

Typescript 不是解决所有问题的答案。你仍然可以[在里面写可怕的代码](https://medium.com/@alexewerlof/what-is-shitty-code-handwriting-ae7c00708b)。

讨厌打字稿的人会讨厌，要么是因为害怕改变，要么是因为他们知道有人知道有人害怕改变。生活在继续，TypeScript 向[的社区](https://github.com/Microsoft/TypeScript)介绍[新功能](https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript)。

但是像 React 一样，TypeScript 是推动我们 web 开发边界的有影响力的技术之一。

无论您是否使用 TypeScript，为了发展您自己对它的看法，尝试一下也无妨。它有一个学习曲线，但如果你已经知道 JavaScript，这将是一个平稳的曲线。

这里有一个[在线实时 TS transpiler](http://www.typescriptlang.org/Playground) 和一些例子，可以让你比较 TypeScript 代码和它的等价 JavaScript 代码。

这里有一个快速[教程](http://www.typescriptlang.org/Tutorial)，和一个[非常好的指南](http://www.typescriptlang.org/Handbook)，但我更像是一个[语言参考](https://github.com/Microsoft/TypeScript/blob/master/doc/spec.md)类型的人。如果你喜欢视频，这里有一个来自 Udemy 的课程。

约翰爸爸有一篇关于 ES5 和打字稿的短文。

有一项有趣的研究表明，在同等条件下，一个类型系统可以减少 15%的错误。

哦，如果你想兼职，读读[为什么编程是有史以来最好的工作](https://medium.com/@alexewerlof/what-s-cool-about-being-a-programmer-5a1e58efeee6)。