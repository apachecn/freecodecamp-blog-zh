# 组件制作:如何创建一个带有链接输入的滑块

> 原文：<https://www.freecodecamp.org/news/component-crafting-how-to-create-a-slider-with-a-linked-input-600d3438a050/>

罗宾·桑德伯格

# 组件制作:如何创建一个带有链接输入的滑块

![1*4xqdv8O0r3mXLL13R0Gk3A](img/6e0a385264422da9e5ac86e12806a226.png)

Photo by [Hermes Rivera](https://unsplash.com/photos/qbf59TU077Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 Stacc 这里，我们是 React 和渲染道具模式的超级粉丝。当需要创建滑块输入时，我们求助于我们最喜欢的资源之一——贾里德·帕尔默斯[真棒-反应-渲染-道具](https://github.com/jaredpalmer/awesome-react-render-props)。在这里我们发现了[反应复合滑块](https://github.com/sghall/react-compound-slider)。

我们的挑战是将滑块与链接输入结合起来。用户可以选择是通过键盘输入还是滑块来输入他们的数据。

滑块和输入就像你典型的兄弟姐妹，不断争吵。当输入请求一个滑块域外的值或者一个不正好落在滑块某个台阶上的值时，顽固的滑块不仅拒绝听取输入——它还会强迫输入改变它的值。结果是令人沮丧的用户体验。

我已经搜索过了，但没有找到能帮我解决这个问题的人。所以，本着分享的精神，这是我的解决方案。如果你有任何想法或建议可以做得更好，请分享！毕竟，与其说我是开发人员，不如说我是设计师。

### 目标

看起来很简单，对吧？所以让我们想想我们需要做什么。

1.  将共享值放在滑块和输入都可以访问的地方。在这种情况下，我们将创建一个组件来包装它们，并将值放在那里。
2.  当输入和滑块中的任何一个发生变化时，管理发送给它们的值。
3.  避免滑块域(最小和最大)和步长的严格规则干扰用户在输入中键入值的能力。

我们稍后将回到包装组件。首先，让我们动手实现滑块。例子下面的解释。

这里我实现了 *getDerivedStateFromProps。*滑块需要从滑块的 props 提供的值中更新其内部状态。我还为 onUpdate、onChange 和 onSlideStart 添加了一些道具。我们可以在包装组件中处理这些事件。除了这些细节，这非常接近于 [react-compound-slider 文档](https://sghall.github.io/react-compound-slider/#/slider-demos/horizontal)中使用的代码。

我挣扎的部分是处理输入和滑块的链接。键入时，该值经常超出滑块允许的最小值和最大值。不能保证用户会键入与滑块中允许的任何步长完全匹配的值。

如果我们不处理这些情况，用户将永远不允许清空输入。如果她在任何步骤之外键入一个值，它会将该值设置为最近的步骤。基本上，我们输入的任何变化都会导致滑块移动到它认为应该在的地方，从而用它的值更新我们的状态，覆盖用户刚刚输入的值。

为了处理这些情况，我需要一些逻辑。我能想到的最佳解决方案是这样的:

1.  当输入获得焦点时，将滑块的步长设置为 1，这样就可以将它设置为用户键入的任何值
2.  如果输入值发生变化，并且新值在允许的范围内，请设置滑块的值。否则，什么都不做。
3.  当用户开始拖动滑块时，再次将 step prop 设置为应该的值，并更新输入的值。

你可以在 [CodeSandbox](https://codesandbox.io/s/wj9wv6nyw) 上看到完整的实现和更多的注释。

感谢您的阅读！