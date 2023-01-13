# 盖茨比 vs 雨果，详细比较

> 原文：<https://www.freecodecamp.org/news/gatsby-vs-hugo-a-detailed-comparison-e78d94f640fc/>

在本文中，我比较了两个静态站点生成器，Gatsby 和 Hugo。我讨论了框架熟悉度、稳定性、安全性、工具、构建速度、性能以及围绕这两者的社区。所以让我们开始吧。

大约一年前，我把我的网站从 Wordpress 改成了 T2 的 Hugo，这是一个用 go 编写的静态网站生成器，使用 go 的模板库作为模板。我最近对 [Gatsby](https://www.gatsbyjs.org/) 做了一个可行性评估，这是另一个用 React 编写的静态站点生成器，它使用 React 作为模板。

在这篇文章中，我比较了 Hugo v0.42 和 Gatsby v1.93 的区别，为了比较，我用了[这个 Hugo 站点](https://learnitmyway-hugo.netlify.com/)和[这个 Gatsby 站点](http://learnitmyway-gatsby.netlify.com/)。每个网站的代码都可以在 Github 上找到，分别是[的 Hugo 网站](https://github.com/DeveloperDavo/learnitmyway/tree/gatsby-vs-hugo)和[的 Gatsby 网站](https://github.com/DeveloperDavo/learnitmyway-gatsby)。

#### 框架熟悉度

如果你不熟悉 React，也不打算学习 React，那么你可能应该选择 Hugo。如果你知道并且喜欢 React，你应该选择盖茨比吗？嗯，不一定。

我认为，如果你想使用盖茨比，你需要对 React 有一个相当好的理解。为了理解 React，你需要对 JavaScript 有一个相当好的理解(参见[用这些资源学习 JavaScript](https://learnitmyway.com/learn-javascript-with-these-resources/))。

尽管我已经使用 Hugo 快一年了，但我没有必要去理解 go。我也只需要学习一点点关于 Go 的模板库。然而，我确实发现，由于对 Hugo 不够熟悉，我不得不更多地参考文档。盖茨比要求对 React 的理解比雨果对 go 的期望更深。然而，如果熟悉框架是唯一的标准，我会选择 Gatsby，因为在我的网站上添加新功能时不必参考文档是很好的。

#### 稳定性

评估稳定性的一种方法是比较雨果在 GitHub 上的问题和 T2 在 GitHub 上的问题。你会看到盖茨比有更多的特点，(这很令人兴奋)但也有更多的 bug(这并不那么令人兴奋)。我最初并没有把稳定性作为一个标准，直到我发现了[这个 bug](https://github.com/gatsbyjs/gatsby/issues/6392) ，这让我意识到软件稳定性的重要性。由于我花费了大量的时间和精力来寻找这个 bug，我可能会把它当成是针对我个人的，但是我仍然认为 Hugo 比 Gatsby 更稳定。

#### 安全性

Gatsby 使用 JavaScript，JavaScript 应用程序因需要大量节点模块运行而臭名昭著。甚至有一个节点模块[发送热门的口袋推文](https://medium.com/@jdan/i-peeked-into-my-node-modules-directory-and-you-wont-believe-what-happened-next-b89f63d21558)和另一个[收集信用卡号码](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5) :D。静态网站本质上更安全，但我仍然认为值得一提的是，更多的依赖会导致更多你可能不信任的代码。

#### 工具作业

Gatsby 拥有 [JavaScript 工具链](https://www.npmjs.com/search?q=Gatsby)的所有优点和 [JavaScript 疲劳](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4)的所有缺点。最重要的是，盖茨比有一个非常好的[插件库](https://www.gatsbyjs.org/plugins/)。特别是， [gatsby-plugin-offline](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-offline) 让我可以轻松地给我的网站添加离线功能，这一点我和 Hugo 还没有搞清楚。

另一方面，有些需要 Gatsby 插件的东西，Hugo 正好开箱即用。例如，需要[Gatsby-plugin-react-helmet](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-plugin-react-helmet)来编辑 head 标签，而在 Hugo 中这可以用简单的 HTML 来完成。因为我喜欢使用盖茨比自带的工具，所以我把这个给盖茨比。

#### 构建速度

Hugo 能够在不到 100 毫秒的时间内建立我的网站，不需要任何额外的工具。Gatsby 能够在大约 15 秒内构建我的网站，但这确实包括许多额外的工具。将 [PostCSS](https://github.com/postcss/postcss) 和 [Imagemin](https://github.com/imagemin/imagemin) 添加到 Hugo 构建中，将构建时间提高了大约 5 秒。使用 Hugo 可以更快地观察开发过程中的变化。我认为雨果是这里的赢家。

#### 证明文件

盖茨比和雨果都有很好的文档。雨果有一个[快速入门](https://gohugo.io/getting-started/quick-start/)，盖茨比有一个[入门](https://www.gatsbyjs.org/docs/)部分。盖茨比也有一个非常好的[教程](https://www.gatsbyjs.org/tutorial/)，它使陡峭的学习曲线变得平坦。就我个人而言，我发现《盖茨比》更容易上手，但那是因为我已经理解了《反应》。我认为公平地说，雨果和盖茨比都有伟大的文献。

#### 表演

使用 [Lighthouse](https://developers.google.com/web/tools/lighthouse/) ，我在 Hugo 的站点的性能得分是 100，我在 Gatsby 的站点的性能得分是 95。3G 连接的第一个令人满意的描述是 Hugo 网站的 1 秒和 Gatsby 网站的 1.5 秒。使用[网页测试](https://www.webpagetest.org/)，2G 连接的加载时间在雨果中为 [8.7 秒，在盖茨比](https://www.webpagetest.org/result/180722_Y6_19710626850f2326f4610b156398dbf0/)中为 [11.7 秒。](https://www.webpagetest.org/result/180722_PJ_fa010df1c51f603586ee9c04f2abd558/)

然而，做一个简单的手动测试来看看哪个站点先加载，Gatsby 明显更快，所以我真的不明白 Lighthouse 或 Web 页面测试在测量什么。此外，由于 Gatsby 是一个单页应用程序，在网站内导航不需要来自服务器的请求。页面只是用 JavaScript 重新呈现。无论如何，我可以肯定地说，雨果和盖茨比都很快。我很想在下面的评论中听到你的想法。

#### 社区

盖茨比迅速走红，随之而来的是一个欣欣向荣的社区。这并不是说雨果的社区很无聊。如果 GitHub 明星有什么可借鉴的，雨果有超过 2.7 万，盖茨比有超过 2.3 万。在推特上，盖茨比似乎比雨果更活跃。

#### 最后的想法

那么你应该选择哪一个呢？盖茨比使用 React，我更熟悉它，它有更好的工具，它有一个蓬勃发展的社区。另一方面，Hugo 更稳定，花在构建上的时间更少。对于较大的网站，构建速度变得更加重要，有些人可能根本不关心 React。对我来说，稳定是最重要的标准，所以我决定继续使用 Hugo。我非常期待看到这个领域的未来。

* * *

**在你离开之前……**感谢你阅读这篇文章！我写的是我作为一名自学成才的软件开发人员的职业和教育经历，所以请查看[我的博客](https://www.learnitmyway.com/)或订阅[我的时事通讯](https://learnitmyway.com/newsletter)了解更多内容。

**您可能还会喜欢:**

*   [我如何更新我的个人网站](https://learnitmyway.com/how-i-release-updates-to-my-personal-website/)
*   [学习材料—软件开发](https://www.learnitmyway.com/2016/11/11/learning-material-software-development/)(学习资源列表，从计算机科学入门开始)
*   [全栈 web 开发值得学习吗？](https://learnitmyway.com/opinion-full-stack/)