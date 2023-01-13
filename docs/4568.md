# 如何使用主视图类型缩放 Elm 视图

> 原文：<https://www.freecodecamp.org/news/scaling-elm-views-with-master-view-types/>

这个概念帮助 Elm 视图随着应用程序变得更大更复杂而扩展。

在 Elm 中，有很多很好的方法来缩放`Model`和`update`，但是围绕缩放`view`有更多的争议。很多争论都围绕着[可重用视图和](https://gist.github.com/rofrol/fd46e9570728193fddcc234094a0bd99#reusable-views-instead-of-nested-components)组件。组件不被推荐，但是很多人仍然支持它们。这篇文章提出了一个想法，希望加强可恢复的观点的论点。

几乎在所有情况下，缩放问题都归结为强制一致性，这通常意味着允许子视图对主视图进行一些调整，同时不允许子视图搞得一团糟。

我将使用 Richard Feldman 的优秀的[真实世界应用程序](https://github.com/rtfeldman/elm-spa-example)(专门编写来演示 Elm 中的伸缩性)作为示例，因为它包含了许多当前的最佳实践技术，这是众所周知的(2000+ stars 和 300+ forks)，并且 Richard 是一位著名的 Elm 专家。

我将对这段代码提出一些改进建议，所以我想在这一点上澄清一下，我没有不尊重的意思(我敢打赌，他做这件事只用了我十分之一的时间！).你也可以说这些问题很小，不值得解决。最终，这个决定是你的，但是在这篇文章的结尾，我希望说服你有问题，并且如果你认为值得的话，这些问题是可以解决的。

![ski-touring](img/662e85ae25f0c32e430aa02059d5bf81.png)

## 带条件的主视图函数

一种选择是定义一个主视图函数。这个函数负责共同关心的问题，比如标题栏和整体布局。然后，它根据当前视图调用子视图函数和/或具有控制子视图特定行为的参数。

这很有效，但很快会导致:

*   参数爆炸，潜在地迫使你的子视图返回很多他们不关心的东西。
*   主视图和子视图之间的责任混合。
*   额外的代码和重复。

在现实世界的应用程序中，一个类型为`Page`的参数被传递给主视图，这样它就可以将导航条链接呈现为活动的。有一个[大型 case 语句](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page.elm#L113)使用这个参数来确定哪个链接是活动的，对于孩子来说，指定这个参数会容易得多。

下方的[线显示主视图通过`Page.Home`，主视图必须与`Home.view home`匹配。这很容易出错，没有编译器或类型系统的帮助，实际上这是子视图的责任。](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Main.elm#L85)

`viewPage Page.Home GotHomeMsg (Home.view home)`

当[创建 NavBarLink Html](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page.elm#L62) 时有一些重复，并且`linkTo`函数将接受任何 Html，尽管只有非常特殊的 Html 是有效的。

## 惯例和信任

另一种可能性是由子视图根据约定和信任来负责保持共享元素的一致性。

可以说这也发生在现实世界的应用程序中。[首页](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page/Home.elm#L146)、[文章](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page/Article.elm#L119)和[简介](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page/Profile.elm#L197)视图都有横幅的概念。每个视图中的横幅是不同的，但应该是一致的、可识别的视觉元素(本质上，它是视图的标题)。这些视图没有共享这些横幅的任何代码，因此它们的大小和颜色也不一样。理论上，您可以尝试使用测试来执行约定，但是这很困难，而且可能不值得。

## 助手功能

另一种可能性是让子视图负责保持共享元素的一致性，但是要使用一些辅助函数。这绝对是一个进步，也可能是我在野外看到的最常见的解决方案。这些函数可以放在同一个文件中，并且彼此相邻。这样更容易看出它们是相关的，并且代表相同的视觉元素，也更容易使它们一致。

但是，还是有一些弊端。主要的一点是，子视图必须知道如何使用助手函数，但没有任何强制措施。当您只有一个共享元素和一个要调用的函数时，这没什么大不了的，但是随着应用程序越来越大，您最终会面临共享视觉元素差异的组合爆炸。大多数人通过为不同的差异提供一些小的、集中的功能来解决这个问题。然后，子视图必须知道所有这些函数，以及如何组合它们，并且不需要编译器的帮助。

同样，这可能发生在现实世界的应用程序中:例如在[profile . view 函数](https://github.com/rtfeldman/elm-spa-example/blob/b5064c6ef0fde3395a7299f238acf68f93e71d03/src/Page/Profile.elm#L211)的这一部分，它需要知道如何使用`viewTabs`、`Feed.viewArticles`和`Feed.viewPagination`助手函数，以及它们需要包含在什么 Html 中。

## 使用主视图类型缩放

为了克服这些问题，我建议使用一个`Type`来定义你的站点结构(我相当夸张地称之为“主视图类型”)。然后子视图返回这个类型，主视图将它作为参数并返回 html。

对于我们一直在看的真实世界应用程序示例，主视图类型如下(`Viewer`是在真实世界应用程序中查看页面的人)。根据您的域，您可以在这里使用更通用的横幅类型，比如 AvatarBanner，甚至 IconBanner(而不是 ViewerBanner)。

```
type alias Page =
    {   activeNavBarLink: NavBarLink
		, banner: Banner
        , body: Html Msg
    }

type Banner =
    TextBanner TextBannerProperties
    | ViewerBanner Viewer
    | ArticleBanner Viewer ArticlePreview

type NavBarLink =
    NavBarLink NavBarLinkProperties 
```

为了演示这一点，我创建了一个只有真实世界应用程序的[标题和横幅部分的存储库，然后在重构后创建了一个新的存储库，使用了](https://github.com/ceddlyburge/elm-without-master-view-types)[母版页类型](https://github.com/ceddlyburge/elm-master-view-types/blob/master/src/Page.elm)、[导航栏链接类型](https://github.com/ceddlyburge/elm-master-view-types/blob/master/src/NavBarLink.elm)和[横幅类型](https://github.com/ceddlyburge/elm-master-view-types/blob/master/src/Banner.elm)。您可以仔细阅读代码，感受一下它是如何工作的。

在我看来，使用母版页类型有以下好处:

*   编写主视图代码更加容易
*   编写子视图代码更容易
*   交流和理解得到了改善，因为 UI 概念现在有了名称
*   主题化/重新设计网站要容易得多
*   Elm 包可以提供 UI 模板

主视图可以精确地定义它将通过类型接受/支持什么，用[联合类型](https://guide.elm-lang.org/types/custom_types.html)和[不透明类型](https://medium.com/@ckoster22/advanced-types-in-elm-opaque-types-ec5ec3b84ed2)。不受支持的组合可以被设置为不可代表或不可创建。

在我的示例存储库中，[导航条链接类型是不透明的](https://github.com/ceddlyburge/elm-master-view-types/blob/master/src/NavBarLink.elm)，所以只可能创建受支持的导航条链接(`home`、`article`和`viewer`)。类似地， [Banner 是一个联合类型](https://github.com/ceddlyburge/elm-master-view-types/blob/master/src/Banner.elm)，这意味着只能表示受支持的变体。

程序员可以简单地修改这些文件，但是一个熟练的程序员会识别这些模式并遵循它们。如果这还不够，并且您感到多疑，那么您可以要求对此类文件进行更严格的代码审查，潜在地利用 GitHub 和 GitLab 上的 [CODEOWNERS](https://help.github.com/en/articles/about-code-owners) 功能。在极端情况下，您可以通过 elm 包提供模块，并限制对底层存储库的推送访问。

子视图不需要做任何事情，只需要创建一个类型的实例。helper 函数都返回类型，所以很容易看出哪些函数可以在特定的上下文中使用，并且不可能在错误的上下文中使用函数。例如，如果一个函数返回一个`HeaderBarLink`，就不可能错误地使用这个函数在`FooterBar`或者页面上的其他地方创建一个链接。子视图也可以将一些复杂性留给主视图。例如，子视图可以定义一个选项列表供选择，而主视图可以使用按钮、下拉列表或自动完成列表来呈现它，这取决于选项的数量。

母版页类型还提供了 UI 概念的名称，然后可以对其进行讨论。例如，一个设计师可以说“让我们把导航条移动到左边”，每个人都会知道他们的意思。产品负责人可能会说“让我们创建一个带有图标横幅的新页面，我们将为图标使用当前的天气 api”，同样，每个人都知道他们的意思。你可以看看这篇[优秀的 thoughtworks 文章](https://www.thoughtworks.com/insights/blog/ui-components-design)了解更多细节。

由于将主视图类型转换为 html 的责任都在同一个地方，所以很容易对网站的外观和感觉做出重大改变，并进行主题化。这些变化和主题可以改变 Css *和 Html* ，这是普通的主题技术做不到的。实际上，您的主视图类型通常会有一个`body: Html Msg`属性(允许子视图在页面的子特定部分上具有完全的灵活性),因此仍然会有一些庞大的代码需要修复，但这肯定会容易得多。

最后，它打开了提供现成的主题和网站布局包的可能性。这将允许你只需要做以下事情就可以得到一个工作的应用程序，包括布局和样式:

*   `create-elm-app`
*   `elm install elm-bootstrap-starter-template`
*   编写一些代码来创建母版页类型
*   `elm-app start`

公司可以创建这样的包来确保他们的应用程序具有一致的外观和感觉。开源设计和布局可能会出现并变得司空见惯，就像 Bootstrap 彻底改变了 html 和 css 设计一样。设计技能有限的开发人员(像我一样)可以专注于他们最擅长的部分(逻辑)，但仍然可以使用这些包制作优雅的网站。

为了演示这一点，我创建了一个[引导启动器主视图包](https://package.elm-lang.org/packages/ceddlyburge/elm-bootstrap-starter-master-view/latest/)。它模仿了[引导启动模板](https://getbootstrap.com/docs/4.0/examples/starter-template/)的布局和设计。然后，我在一个演示 elm 应用程序中使用了这个包。你可以[浏览演示应用程序](https://elm-bootstrap-starter.netlify.com/)看看它看起来怎么样，并且[查看源代码](https://github.com/ceddlyburge/elm-bootstrap-starter-demo)看看它是如何工作的。

所有这些优势的代价都很小，甚至是负的。新类型的代码要多一点，但是一些重复的代码被删除了。您可以查看真实世界应用程序库的源代码[之前的](https://github.com/ceddlyburge/elm-without-master-view-types)和重构后的[，以使用母版页类型](https://github.com/ceddlyburge/elm-master-view-types)了解全部细节。

## 结论

主视图类型带来了很多好处(视图代码更容易编写和维护，UI 概念被命名，UI 包也是可能的),而且成本很低或者没有成本。他们应该改进任何 Elm 应用程序的代码，这些应用程序在其视图代码中存在强制一致性(同时允许灵活性)的问题，根据我的经验，这是大多数大中型应用程序。