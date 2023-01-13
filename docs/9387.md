# Browserify 与 Webpack

> 原文：<https://www.freecodecamp.org/news/browserify-vs-webpack-b3d7ca08a0a9/>

#### 如果你需要一个小屋，为什么从一堆木头开始
*？*

> 在 JavaScript 领域，没有人能长久称王。

就在去年， [Grunt](http://www.gruntjs.com) 实际上被 [Gulp](http://gulpjs.com/) 赶下了台。现在，就在 Gulp 和 [Browserify](http://browserify.org/) 终于达到临界质量的时候， [Webpack](http://webpack.github.io/) 威胁要把他们俩赶下台。是否真的有令人信服的理由再次改变您的前端构建流程*？*

*让我们考虑一下每种方法的优点。*

### *Browserify(和朋友……)*

*Browserify 在概念上很简单。嘿，看到 [npm](http://npmjs.org) 上这些很酷的包了吗？让我为你把它们打包，这样你就可以在浏览器中使用它们了。方便。谢谢 Browserify！然而这种简单性也是它的致命弱点。你可能有一长串需要完成的事情，比如缩小、捆绑、林挺、运行测试等等。*

*当然，Browserify 有一个丰富的转型生态系统来帮助完成工作。但是要把这些连接起来，你得靠自己。所以你需要一个 JavaScript 构建自动化工具，比如 Grunt 或 Gulp。这些工具工作得很好，但是正确配置它们是一个耗时的过程。在你长时间使用 Grunt 或 Gulp 之后，你开始意识到你为每个项目做了一长串事情。从一个更强大、更固执的工具开始，对你的构建过程做更多的假设，不是很好吗？*

*如果您将您的构建过程想象成一个独特的小木屋，那么使用 Gulp/Grunt 浏览就像从这里开始:*

*![1*E_cgh1eE6GAtWWMkQRc-3Q](img/5e22a1f676b130d7cbe0107c6dbc6715.png)

Some assembly required.* 

### *网络包*

*Webpack 从根本上以一种更加集成和固执己见的方式来解决构建问题。在 Browserify 中，你使用 Gulp/Grunt 和一长串的转换和插件来完成工作。 **Webpack 开箱即可提供足够的电力，您通常根本不需要咕哝或吞咽**。是的，抱歉，你刚刚掌握的闪亮新技能已经几乎没用了。呃…耶？叹气。欢迎来到前端生活。*

*但是，嘿，这就是进步的代价。使用 Webpack，您可以声明一个简单的配置文件来定义您的构建过程。哇，一个配置文件？是的，如果你因为更喜欢代码而不是配置，就从 Grunt 迁移到 Gulp，这听起来可能是一种倒退。但是配置不一定是坏事。*

> *如果基于配置的系统对你的目标做了正确的假设，那么它们比命令式系统更好。*

*我发现 Webpack 就是这样做的。Webpack 假设您需要将文件从源目录移动到目标目录。它假设您希望使用各种模块格式的 JavaScript 库，如 CommonJS 和 AMD。它假设您想要使用一个长长的加载器列表来编译各种格式。当然，你可以通过 Browserify 和 Gulp 完成所有这些，但你必须自己做更多的布线工作。你手动将两种完全不同的技术连接在一起。*

*当你考虑到与图像和 CSS 等其他资产一起工作的故事时，Webpack 的集成特性真的大放异彩。Webpack 足够智能，可以在有意义的时候动态内嵌你的 [CSS](http://webpack.github.io/docs/stylesheets.html) 和[图像](https://github.com/webpack/url-loader)。您可以像现在使用 JavaScript 一样简单地包含这些资产。想用 CSS？简单:*

```
*`require("./stylesheet.css");`*
```

*啊？为什么不简单地用老方法引用 CSS？嗯，Webpack 会考虑这个文件的大小。如果它很小，它会嵌入样式表！如果文件很长，它会缩小文件，并给它一个唯一的名称，用于缓存破坏。同样的故事也适用于使用 url loader 插件的[图片](https://github.com/webpack/url-loader)。*

```
*`img.src = require(‘./glyph.png’);`*
```

*如果图像足够小，Webpack 将对其进行 base64 编码。滑头！*

*由于 Webpack 完全了解您的构建过程，它可以[智能地将您的资源分成包](http://webpack.github.io/docs/code-splitting.html)。这对较大的水疗中心来说很方便。您的用户将只获得给定页面所需的文件，而不是一个单一的 JavaScript 文件。*

*最后，如果您想在不刷新浏览器的情况下享受快速应用程序开发，Webpack 提供了[热模块替换](http://webpack.github.io/docs/hot-module-replacement.html)。如果你碰巧在 React 工作，你可以利用 [React 热加载器](https://github.com/gaearon/react-hot-loader)。是的， [Browserify 有等价的](https://github.com/milankinen/livereactload)，但是以我的经验来看， [@dan_abromov](http://www.twitter.com/dan_abramov) 实现的 Webpack 更胜一筹。相信我，一旦你经历了这样的快速反馈应用程序开发，你就再也不会回头了:*

 *[https://www.youtube.com/embed/xsSnOQynTHs?feature=oembed](https://www.youtube.com/embed/xsSnOQynTHs?feature=oembed)* 

*Webpack 假设你想建一个小木屋。所以你从一个真正的小屋开始，然后它给你所需的工具来调整它以满足你的需求。*

*![1*YW70wmIr2R3tb5Xtec2giw](img/754252a4a28f57f6e446281f5efdf15c.png)

Why not start here and tweak a few minor things that make you unique?* 

### *底线*

*如果你喜欢从头开始明确地配置小型的单一用途工具，那么使用 Gulp/Grunt 的 Browserify 将更符合你的风格。Browserify 最初更容易理解，因为概念表面积非常小——假设你已经知道 Gulp 或 Grunt。在 Browserify 中使用 Gulp 时，生成的构建过程可能比等效的 Webpack 构建更容易理解。它通常更明确地表达意图。Browserify 丰富的插件生态系统意味着你可以通过足够的连接完成任何事情。啊，电线。这是最大的缺点。如此明确需要花费大量的时间和调试才能正确。我最近为即将到来的关于 React 和 Flux 的 Pluralsight 课程制作了一个[初学者工具包](https://github.com/coryhouse/react-flux-starter-kit)。它引入了一些 React 特定的技术，但如果您只是在寻找一种快速使用 Browserify 和 Gulp 的方法，那么移除这些依赖关系是微不足道的。*

*但是，如果您喜欢通过一个更加固执己见的工具来使用一点“魔法”,那么 Webpack 可以减少建立一个健壮的构建过程所需的时间。我发现我的 Webpack 配置通常只有同等的 Gulp 文件的一半大小。好处不仅仅是减少了输入——这也意味着减少了调试配置花费的时间。*

*大量单调的文件让许多人对配置失去了兴趣。但是 Grunt 的失败不在于配置。这是缺乏见解。**配置并不邪恶。**当假设正确时，这是一个强大的时间节省器。*

> *构建工具不应该要求从头开始定制构建。它应该提供定制点，允许您处理一些让您真正独一无二的事情。*

### *准备好深入挖掘了吗？*

*更新后的 Webpack 文档很棒，但我建议阅读 T2 的皮特·亨特的精彩介绍。然后，在 Pluralsight ( [免费试用](https://billing.pluralsight.com/individual/checkout/account-details?sku=IND-Y-PLUS-FT))上查看“[创建 JavaScript 开发环境](https://app.pluralsight.com/library/courses/javascript-development-environment)”，查看使用 Webpack 从头构建的完整开发环境。*

*使用 React？查看“[在 ES6](https://app.pluralsight.com/library/courses/react-redux-react-router-es6) 中使用 React 和 Redux 构建应用程序”。*

*在[黑客新闻](https://news.ycombinator.com/item?id=9954973)或 [Reddit](https://www.reddit.com/r/javascript/comments/3erss1/browserify_vs_webpack/) 上插话*

*[Cory House](https://twitter.com/housecor) 是关于 JavaScript、React、clean code、[多门课程的作者。NET，以及 Pluralsight](http://pluralsight.com/author/cory-house) 上的更多内容。他是 reactjsconsulting.com[公司的首席顾问，微软 MVP 公司 VinSolutions 的软件架构师，在国际上培训软件开发人员，如前端开发和干净编码。Cory 在 Twitter 上以](http://www.reactjsconsulting.com) [@housecor](http://www.twitter.com/housecor) 的身份发关于 JavaScript 和前端开发的推文。*