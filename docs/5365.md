# 如何写出你将来会喜欢的代码

> 原文：<https://www.freecodecamp.org/news/how-to-write-code-you-will-love-in-the-future-ee5decae5ce4/>

本叔叔曾经告诉彼得·帕克(Peter Parker)，*“权力越大，责任越大”*。这句话适用于参与构建项目的程序员同事。在这个行业呆了 5 年多，让我反思了自己迄今为止的经历，现在是我回报社区的时候了。

### 开始

我在一家跨国公司开始了我的职业生涯，但我很快意识到我想从事更具挑战性、责任更大的工作。因此，一年后，我加入了一家初创公司。

这只是一个五人开发团队。它改变了我的发展观。我幸运地找到了一个伟大的导师和了不起的队友，他们帮助我成长。但话说回来，我们是一家快节奏的公司。为了确保按时交付，我们经常牺牲代码质量。我们经常认为我们最终会解决这个问题。所以当我们建造这艘船的时候，我们留下了漏洞。这导致了科技债务，这并不是一件坏事。

### 永远不要在代码质量上妥协

一段时间后，我们逐渐意识到，我们将无法再扩大规模。所以我们决定重写整个代码库，这反过来需要更多的时间。但是这最终导致了一个好的代码库，它是可扩展的，并且工作起来很有趣。我仍然记得我们曾经有一个'**羞耻文件夹** ' ，以防任何开发人员决定编写他们知道以后会产生额外工作的代码。

通过这种方式，我们对保持质量负责。但我得到的教训是:

> 即使需要多一点时间来完成，我们也应该利用这段时间为未来编写高质量的代码。由于我们付出了额外的努力，我们节省了大量的时间和金钱。

我们解决了架构问题，但接下来是有趣的部分:**性能*。*** 当我们构建项目时，为了快速开发，我们使用了很多库。我们觉得我们的项目增加了一些重量。它需要大量的练习。为了减轻这些额外的负担，我们做了一些分析，并意识到我们有许多不必要的库。我们本可以自己建造这些图书馆。所以我们收集了这些库，建立了我们自己的库。太好了！！由于较小的包大小，我们的页面速度更快。

但是对表演的渴望并没有结束。当你从零开始建立了一个项目，那种斯巴达人的感觉就慢慢消耗了你。故事还不能结束。我们可以更快。然后我们恍然大悟，我们还在用库。如果我们什么都没用呢？写我们自己的激动消耗了我们，所以我们做了。我们在几乎没有库的情况下成功构建了一个项目。

### 始终记录和编写代码注释

然后我们的故事发生了转折:这家初创公司最终被收购了。我被调到了一个新的团队。新成员对市场上现有的图书馆更加熟悉。突然间我们的代码库对他们来说是陌生的。我们确实编写了我们的库，但是我们没有足够的时间来记录它们。这造成了巨大的差距。我学到了文档和代码注释的重要一课。

> 我意识到代码不仅仅是关于你自己。作为一个作者，为大众写作是你的职责。

所以寓意是，写自己的库并没有错。但是如果你这样做了，那么文档和代码注释是必须的。任何人只要阅读你的文档就应该很容易理解你的图书馆。我怎么强调都不为过，不为自己写！作为代码审查者和维护者，确保这一点是您的责任。

### 不要重新发明轮子，除非你确保它是可维护的

随着时间的流逝，我意识到重新发明整个轮子是没有意义的。除非我们有大量的时间来开发和记录相同的内容，以便所有人都能理解。如果有一个库可以解决你的问题，那么为这个特定的项目做贡献是一个很好的主意！这里有一个陷阱，我想引用菲尔·沃顿在 T2 博客中的话:

> 重新发明轮子对生意不好，但对学习有好处。您可能很想从 npm 获取 typeahead 小部件或事件委托库，但是想象一下，如果您自己尝试构建这些东西，您会学到多少东西。

所以明智地做出你的选择吧，^_-

### 总是测试你的代码库

我怎么强调这一点都不为过。感谢像 [Jest](https://github.com/facebook/jest) 和 [React testing library](https://github.com/kentcdodds/react-testing-library) 这样的库，以及许多其他库，测试代码从未如此简单。通常，当代码库变得很大时，即使是一行更改也会导致应用程序崩溃。如果我们的测试是自动化的，我们可以对我们推动的变化充满信心。

### 不断学习

我希望我的前端开发快速高效。我最终决定学习 React，主要是因为我的背景。我发现它是不独立的，编写它非常接近于编写普通的 JavaScript。它让我的生活变得更好。

像 React、Vue、Angular 和其他各种库(尤其是 Redux)不只是告诉你如何构建一个快速的 UI。它们也打开了通向其他概念的大门，如函数式编程、不变性和许多其他概念，这实际上有助于你更好地掌握自己的技能。学习 React 和 Redux 增强了我已经知道的东西。

### 结论

随着经验的积累，我最终加入了另一家初创公司，在那里，我的任务是从头开始构建产品，并最终奠定基础。但是这一次我用我所有的经历和错误武装了自己。我很高兴地说，我为我到目前为止所取得的成就感到骄傲，我确信我还有很长的路要走。追求完美是一条永无止境的道路，但我们可以一直努力走在正确的道路上。

我所提到的所有经验并不意味着是法律的话。它们非常具体地反映了我在这个行业的经历。但是我希望这能帮助你成为一名更好的开发人员，我总是感谢社区，是他们帮助我成长。

*在 **[twitter](https://twitter.com/daslusan)** 上关注我，获取更多关于新文章的更新，并了解最新的前端开发。此外，在 twitter 上分享这篇文章，以帮助其他人了解它。分享就是关爱 **^_^.***

### 一些有用的资源

1.  [https://Philip walton . com/articles/how-to-be-a-great-front-end-engineer/](https://philipwalton.com/articles/how-to-become-a-great-front-end-engineer/)
2.  [https://jestjs.io/](https://jestjs.io/)
3.  [https://blog . kentcdodds . com/introducing-the-react-testing-library-e3a 274307 e 65](https://blog.kentcdodds.com/introducing-the-react-testing-library-e3a274307e65)
4.  [https://en.wikipedia.org/wiki/Technical_debt](https://en.wikipedia.org/wiki/Technical_debt)
5.  [https://en.wikipedia.org/wiki/Software_entropy](https://en.wikipedia.org/wiki/Software_entropy)

### 我以前的文章

1.  [https://medium . freecodecamp . org/the-best-way-to-architect-your-redux-app-ad 9 BD 16 c 8 e 2d](https://medium.freecodecamp.org/the-best-way-to-architect-your-redux-app-ad9bd16c8e2d)
2.  [https://medium . freecodecamp . org/how-to-use-redux-persist-when-migrating-your-States-a5 dee 16 b5 EAD](https://medium.freecodecamp.org/how-to-use-redux-persist-when-migrating-your-states-a5dee16b5ead)
3.  [https://code burst . io/redux-observable-to-the-rescue-b27f 07406 cf2](https://codeburst.io/redux-observable-to-the-rescue-b27f07406cf2)
4.  [https://code burst . io/building-web app-for-the-future-68d 69054 cbbd](https://codeburst.io/building-webapp-for-the-future-68d69054cbbd)
5.  [https://code burst . io/CORS-story-of-requested-twice-85219 da 7172d](https://codeburst.io/cors-story-of-requesting-twice-85219da7172d)
6.  [https://blog . use journal . com/what-I-learn-from-react foo-2018-E4 E1 a4 c6a 705](https://blog.usejournal.com/what-i-learnt-from-reactfoo-2018-e4e1a4c6a705)