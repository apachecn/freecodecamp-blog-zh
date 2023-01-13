# 如何使用渲染道具跟踪 React 中的页面可见性

> 原文：<https://www.freecodecamp.org/news/how-to-track-page-visibility-in-react-using-render-props-b895537d62f7/>

作者:Soumyajit Pathak

# 如何使用渲染道具跟踪 React 中的页面可见性

![1*WsI4NrSVmdM-xjYDPa-Wgw](img/0e606deb25fe85188e862f954165dcf9.png)

Photo by [Simon Abrams](https://unsplash.com/photos/eMnddgd3pjQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/visible?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我们将创建一个简单的可重用 React 组件来跟踪“页面可见性状态”

创建 web 应用程序时，您可能会遇到需要跟踪应用程序当前可见性状态的情况。您可能需要播放/暂停视频或动画效果，抑制一些性能密集型工作，或者只是根据浏览器选项卡是否处于活动状态来跟踪用户行为以进行分析。

现在，这个特性看起来很容易实现，直到你第一次真正尝试实现它。事实证明，跟踪用户是否主动与 web 应用程序交互是相当棘手的。

有一个页面可见性 API 在大多数情况下都能很好地工作，但是它不能处理所有可能的浏览器标签不活动的情况。 [**页面可见性 API**](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API) 发送一个`visibilitychange`事件，让听者知道页面的可见状态已经改变，或者有一些不规则。在某些情况下，即使窗口或相关的浏览器选项卡不在视线内或不在焦点上，它也不会触发该事件。

为了处理这些边缘情况，我们需要在`document`和`window`元素上结合使用`focus`和`blur`事件监听器。你可以在这里找到关于它的详细讨论[。](https://stereologics.wordpress.com/2015/04/02/about-page-visibility-api-hidden-visibilitychange-visibilitystate/)

我们将在一个小的 React 应用程序中实现上述教程中描述的变通逻辑。不要担心，你可以稍后再看——我们将解释我们将使用的逻辑的每个方面。

[**code sandbox**](https://codesandbox.io/embed/81x9n78qmj)
[*code sandbox 是一款为 web 应用量身定制的在线编辑器。* codesandbox.io](https://codesandbox.io/embed/81x9n78qmj)

如果你想深入研究代码或观看演示，可以在 [**Codesandbox**](https://codesandbox.io/s/81x9n78qmj) 以及 [**Github**](https://github.com/drenther/react-page-visibility-example) 上找到。

### 入门指南

![1*SZoc6eC41JBxJSQ_xODPrA](img/5c5174c58aedd11cf1f544910b6d4b91.png)

The video pauses when it is in the background

我们将使用 Codesandbox 来引导 React 应用程序(您也可以使用 **create-react-app** )。我们将创建一个小应用程序，它将有一个 HTML5 视频元素，只有当浏览器选项卡处于焦点或活动状态时才会播放，否则它会自动暂停。我们使用视频，因为这将使测试我们的应用程序的功能变得容易。

让我们从创建最简单的部分开始，即`Video`组件。这将是一个简单的组件，将接收一个名为`active`的布尔道具和一个名为`src`的字符串道具，将持有视频的 URL。如果`active`道具是真的，那我们就播放视频。否则我们将暂停它。

我们将创建一个简单的 React 类组件。我们将呈现一个简单的视频元素，其源设置为使用`src` props 传递的 URL，我们将使用 React 的新`ref` API 在视频 DOM 节点上附加一个`ref`。我们将视频设置为自动播放，假设当我们启动应用程序的页面将是活跃的。

这里需要注意的一点是，Safari 现在不允许在没有用户交互的情况下自动播放媒体元素。当一个组件的属性改变时，生命周期方法在处理副作用方面非常方便。因此，这里我们将使用这种生命周期方法，根据`this.props.active`的当前值来播放和暂停视频。

在使用某些 API 时，浏览器供应商前缀差异是非常令人讨厌的，页面可见性 API 当然是其中之一。因此，我们将创建一个简单的实用函数来处理这些差异，并以统一的方式返回基于用户浏览器的兼容 API。我们将从 **src** 目录下的 **pageVisibilityUtils.js** 中创建并导出这个小函数。

在这个函数中，我们将利用简单的基于 if-else 语句的控制流来返回特定于浏览器的 API。你可以看到我们为 Internet Explorer 添加了 **ms** 前缀，为 webkit 浏览器添加了 **webkit** 前缀。我们将正确的 API 存储在`hidden`和`visibilityChange`字符串变量中，并以简单对象的形式从函数中返回。最后，我们将导出函数。

接下来，我们进入主要组件。我们将利用**渲染道具**模式，将所有页面可见性跟踪逻辑封装在一个可重用的 React 类组件中。我们将创建一个名为`VisibilityManager`的类组件。该组件将处理所有基于 DOM 的事件侦听器的添加和删除。

我们将从导入我们之前创建的实用函数开始，并调用它来获得正确的浏览器特定 API。然后，我们将创建 React 组件，并用设置为`true`的单个字段`isVisible`初始化其状态。这个布尔状态字段将负责反映我们的页面可见性状态。

在组件的`componentDidMount`生命周期方法中，我们将在文档上为`visibilitychange`事件附加一个事件监听器，用`this.handleVisibilityChange`方法作为其处理程序。我们还将为文档上的焦点和模糊事件以及窗口元素附加事件侦听器。这一次我们将设置`this.forceVisibilityTrue`和`this.forceVisibilityFalse`分别作为焦点和模糊事件的处理程序。

接下来，我们将创建接受单个参数`forcedFlag`的`handleVisibilityChange`方法。这个`forceFlag`参数将用于确定该方法是因为`visibilitychange`事件还是焦点或模糊事件而被调用。这是因为`forceVisibilityTrue`和`forceVisibilityFalse`方法什么都不做，只是用参数`forcedFlag`的 true 和 false 值调用`handleVisibilityChange`方法。

在`handleVisibilityChange`方法内部，我们首先检查`forcedFlag`参数值是否为布尔值(这是因为如果从`visibilitychange`事件处理程序调用它，那么传递的参数将是一个 [**合成事件**](https://reactjs.org/docs/events.html) 对象)。

如果它是一个布尔值，那么我们检查它是真还是假。当它为真时，我们用 true 调用`setVisibility`方法，否则我们用 false 作为参数调用方法。`setVisibility`方法利用`this.setState`方法来更新组件状态中`isVisible`字段的值。

但是，如果`forcedFlag`不是布尔值，那么我们检查文档中隐藏的属性值，并相应地调用`setVisibility`方法。这就完成了我们的页面可见性状态跟踪逻辑。

为了使组件在本质上可重用，我们使用 Render Props 模式。也就是说，我们调用`this.props.children`作为具有`this.state.isVisible`的函数，而不是呈现来自`render`方法的组件。

最后，我们将 React 应用程序挂载到我们的 **index.js** 文件中的 DOM。我们导入两个 React 组件`VisibilityManager`和`Video`，并通过组合它们创建一个小的功能性 React 组件`App`。我们传递一个函数作为`VisibilityManager`组件的子组件，它接受`isVisible`作为参数，并在其返回语句中将其传递给`Video`组件。我们还将一个视频 URL 作为`src`道具传递给`Video`组件。这就是我们如何使用基于渲染道具的`VisiblityManager`组件。最后，我们使用`ReactDOM.render`方法在 id 为“root”的 DOM 节点上呈现我们的 React 应用程序。

### 结论

现代浏览器 API 变得越来越强大，可以用来做惊人的事情。这些 API 中的大多数本质上都是命令式的，在 React 的声明性范例中有时会很难使用。使用像 Render Props 这样的强大模式将这些 API 包装到它们自己的可重用 React 组件中会非常有用。

*最初发表于 [able.bio](https://able.bio/drenther/track-page-visibility-in-react-using-render-props--78o9yw5) 。*