# 数据科学家该不该学 JavaScript？

> 原文：<https://www.freecodecamp.org/news/should-data-scientists-learn-javascript-e611d45804b8/>

#### 将 web 的#1 语言用于数据科学的利与弊

如果你近年来一直在关注科技领域，你可能会注意到至少两件事。

首先，你可能已经注意到 [JavaScript 是目前非常流行的语言](https://insights.stackoverflow.com/survey/2017#technology-programming-languages)。自从 [Node.js](https://nodejs.org/en/) 允许 JavaScript 开发者编写服务器端代码以来，它越来越受欢迎。

最近，像 [Electron](https://electronjs.org/) 、 [Cordova](https://cordova.apache.org/) 和 [React-Native](https://facebook.github.io/react-native/) 这样的框架已经使 JavaScript 开发者能够在广泛的平台上构建本地应用。

你可能也注意到了数据科学领域有很多令人兴奋的事情，尤其是机器学习。理论和技术的最新进展使得这个曾经深奥的领域更容易被开发者理解。

那么，你可能会问，它们是否是天生的一对？数据科学家应该考虑学习 JavaScript 吗？

大多数数据科学家都使用 Python、R 和 SQL 的某种组合。如果你是这个领域的新手，**这些是你应该首先掌握的语言**。

数据科学家也可能专攻另一种语言，如 Scala 或 Java。这些语言如此受欢迎有很多原因。

但是相对来说，很少有数据科学家专门研究 JavaScript。

然而，考虑到 JavaScript 的无处不在和数据科学的流行，数据科学家能从学习这种语言的基础知识中获益多少呢？而想探索数据科学的 JavaScript 开发者呢？

让我们从一些重要的反对意见开始，然后回顾一些支持的论点。

#### 反对

*   **功能** —与 R 和 Python 等语言相比，JavaScript 只是没有数据科学包的范围和内置功能。如果你不介意重新发明轮子，这可能不是一个问题。但是如果你需要运行更复杂的分析，你会很快用完所有的选项。
*   **生产力**—Python 和 R 的广泛生态系统的另一个优势是有许多指南和方法可用于几乎任何你想做的数据科学任务。对于 JavaScript，情况并非如此。与使用 Python 或 r 相比，用 JavaScript 解决数据科学问题可能需要更长的时间。
*   **多线程** —这通常有助于处理大型数据集或[并行运行模拟](https://medium.freecodecamp.org/solve-the-unsolvable-with-monte-carlo-methods-294de03c80cd)。但是，Node.js 不适合计算密集型、CPU 受限的任务。对于这样的任务，Python、Java 或 Scala 等语言比 JS 占了上风。但是，看看[微软的 Napa.js 项目](https://github.com/Microsoft/napajs#napajs)。它提供了一个多线程 JavaScript 运行时，可以补充 Node.js。
*   **机会成本** —也许数据科学家倾向于不学习 Python 和 R 之外的许多语言的主要原因是由于“[机会成本](https://en.wikipedia.org/wiki/Opportunity_cost)”。花在学习另一种语言上的每一个小时都可以用来学习新的 Python 框架或另一个 R 库。虽然这些语言在数据科学就业市场上占据主导地位，但学习它们的动力更大。因为数据科学是一个快速发展的领域，所以总会有新的东西需要学习。

#### 为

*   **可视化** — JavaScript 擅长数据可视化。诸如 [D3.js](https://d3js.org/) 、 [Chart.js](https://www.chartjs.org/) 、 [Plotly.js](https://plot.ly/javascript/) 和[许多其他](https://www.codewall.co.uk/the-best-javascript-data-visualization-charting-libraries/)的库使得强大的数据可视化和仪表板真的很容易构建。查看[一些伟大的 D3 例子](https://github.com/d3/d3/wiki/Gallery)！
*   **产品集成** —越来越多的公司使用基于节点堆栈的 web 技术来构建他们的核心产品或服务。如果你作为数据科学家的角色需要你与产品开发人员密切合作，那么“说”同一种语言不会有什么坏处。
*   数据处理管道通常用通用语言实现，比如 Python、Scala 或 Java。JavaScript 经常不被关注。然而，这可能是不公平的。Node 的文件系统模块‘fs’提供了一个很棒的 API，让你可以同步或者异步调用标准的文件系统操作。Node 还可以很好地与 [MongoDB](https://www.mongodb.com/) 和许多其他流行的数据库系统协同工作。 [Streams API 使得处理大型数据流](https://medium.freecodecamp.org/node-js-streams-everything-you-need-to-know-c9141306be93)变得非常容易——这是 ETL 的另一个潜在优势。如上所述，多线程和并行处理见[微软的 Napa.js 项目](https://github.com/Microsoft/napajs#napajs)。
*   **Tensorflow.js** —谁说 js 不能做很酷的机器学习的东西？2018 年早些时候， [Tensorflow.js 发布](https://medium.com/tensorflow/introducing-tensorflow-js-machine-learning-in-javascript-bf3eab376db)。这给 JavaScript 开发人员带来了机器学习——在浏览器和服务器端都是如此。 [Tensorflow](https://www.tensorflow.org/) 是一个流行的机器学习库，由谷歌开发，于 2015 年开源。手势识别、物体识别、音乐作曲……你说得出的，你大概都能有。你现在能做的最好的事情就是[看一些演示](https://github.com/tensorflow/tfjs/blob/master/GALLERY.md)。

### 结论

那么，数据科学家应该学习 JavaScript 吗？

学习 JavaScript 不会损害你的简历。但是不要学习它来代替其他语言。

作为第一种语言，最好的建议是学习 Python 或 r 中的一种。您还应该熟悉使用一些数据库语言，如 SQL 或 MongoDB。

然而，一旦你熟悉了基础知识，你可能会想进一步专业化。也许你想学习 Apache Spark 来处理巨大的分布式数据集。或者你更喜欢学习另一种语言，比如 Scala、MATLAB 或 Julia。

为什么不考虑 JavaScript？如果您想专攻数据可视化，或者如果您的角色要求您与使用 JavaScript 或相关技术构建的产品密切合作，这将证明是有价值的。

JavaScript 的机器学习能力正在快速进步。对于某些用例来说，它可能已经是常用数据科学语言的强大替代品。

最终，这个决定既是实际的，也是个人的。这取决于你对数据科学的哪些方面最感兴趣，哪些职业机会最让你兴奋。

但是从目前的趋势来看，有一件事是肯定的。在接下来的几年里，JavaScript 打开的门会比关闭的门多。