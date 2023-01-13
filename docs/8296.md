# React shouldComponentUpdate 去神秘化

> 原文：<https://www.freecodecamp.org/news/react-shouldcomponentupdate-demystified-c5d323099ef6/>

在 React 中开发时，您是否想过组件的 [render](https://facebook.github.io/react/docs/react-component.html#render) ()方法何时以及为何运行？或者什么时候使用不太明显的生命周期方法 [shouldComponentUpdate](https://facebook.github.io/react/docs/react-component.html#shouldcomponentupdate) ()？

如果答案是肯定的，你的应用可能有性能问题。通读一遍，你将能够很容易地修复它们。

这完全取决于 React 在幕后的工作方式。React 的大承诺是，它在页面上呈现元素的速度非常快。

为此，React 在内存中保存了两个版本的 DOM:

*   当前显示的 DOM 版本
*   要显示的 DOM 的下一个版本

它将两者进行比较，并只更新显示的 DOM 中发生变化的部分。这个过程被称为[树协调](https://facebook.github.io/react/docs/reconciliation.html)。评估协调的树根是[道具](https://facebook.github.io/react/docs/components-and-props.html)已经改变的组件。

太好了。现在，无论您是否计划好了，您的 web 应用程序在某种程度上都遵循容器/表示组件的划分。定义见[此处](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)和[此处](https://medium.com/@learnreact/container-components-c0e67432e005)。这意味着你的应用程序中的每个复杂视图都是由一个容器组件组成的，这个容器组件包含逻辑，并且有许多[只显示组件](https://www.fullstackreact.com/30-days-of-react/day-11/)作为子组件。

这是一个非常好的模式。如果仔细观察，这意味着视图上的任何用户交互都将影响容器本身，并触发容器及其所有子容器的呈现。假设你有一个元素列表，里面有漂亮的文本、图像和一个类似“添加到收藏夹”的黄色星形按钮。列表元素的最小模型可以是:

```
product = { 
    imageUrl: '...', 
    title: '...', 
    isFavourite: false
}
```

收藏夹列表可以来自另一个数据源。不管怎样，您的组件组织可能看起来像这样:

```
<Container>
    <ListOfElements
        elements={this.props.elements} 
        onElementChanged={this.props.onElementChanged} 
    />
</Container>
```

该处理程序在用户点击时被调用，并保存信息服务器端(或保存在存储中或其他地方),并触发 this.props.elements 中的更改

单击的结果触发了容器和列表中所有行的呈现，只是为了更新一个复选框。

这就是 shouldComponentUpdate()发挥作用的地方。您可以告诉 React 不要呈现不需要使用该方法的行。

```
class ListItem extends Component {
    shouldComponentUpdate(nextProps, nextState) {
        return nextProps.isFavourite != this.props.isFavourite;
    }
    ...
}
```

这里有一个具体的案例:在一个市场应用项目中，我们有一个销售人员的产品管理视图。该列表有一个“当用户向下滚动时加载更多”模式和一个内联项目动作“显示/隐藏”来设置每个产品的可见性。当卖家在他们的仪表板中管理少于 100 个产品时，一切都很好。然后一个特定的卖家开始进入并宣传 300 多种产品…

在用户点击“启用/禁用”图标后，在用户界面更新之前有大约 600 毫秒的延迟。最终用户肯定能看到这种滞后。使用 [Chrome profiler](https://developers.google.com/web/tools/chrome-devtools/rendering-tools/) 我们看到渲染一行花了 React ~2ms。乘以 300 …我们达到了 600 毫秒。我们添加了 shouldComponentUpdate()来检查适当的条件。用户点击后的渲染时间不到 10 毫秒…

我做了一个小项目，可以在这里重现这个案例[。运行它并阅读代码注释，看看神奇的事情发生了。](https://github.com/jpdelima/react-should-component-update-demystified)

### Redux 用户警告

如果您使用 [Redux](https://github.com/reactjs/react-redux) 和 [reselect](https://github.com/reactjs/reselect) (或类似的“基于存储”的动作管道库)，上述问题可能会更经常发生。

使用 Redux 和 reselect，您可以将操作推送到存储区，并插入侦听器来存储更改，也就是选择器。选择器在应用程序中是全局可用的，在大型应用程序中，许多组件映射到同一个选择器是非常容易的。对商店的更改可能会触发道具更改，从而导致与某些组件完全无关的渲染。

这里有一个令人困惑的建议:**在这种情况下，不要使用** shouldComponentUpdate()来阻止渲染。shouldComponentUpdate 内部的逻辑应该只查看与组件相关的内容。它不应该预测组件使用的上下文。原因是你的代码会很快变得不可维护。

如果你有这种问题，这意味着你的商店结构是错误的，或者选择器不够具体。你需要参加新一轮的模特比赛。

我推荐[这个令人敬畏的样板](https://github.com/react-boilerplate/react-boilerplate)指南。它促进了每个高级容器的存储封装，为跨整个应用程序的关键数据结构提供了一个全局区域。这是避免商店建模错误的非常安全的方法。

**感谢阅读！如果你喜欢它，请点击下面的按钮。这有助于其他人了解这个故事。**