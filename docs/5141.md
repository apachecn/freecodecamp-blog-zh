# 如何用 React 和钩子检测外部点击

> 原文：<https://www.freecodecamp.org/news/how-to-detect-an-outside-click-with-react-and-hooks-25dbaa30abcd/>

安德烈·卡西欧

# 如何用 React 和钩子检测外部点击

![1*xJYzPKhZBDlYsR2nwkbvPQ](img/40d4cf7205edbc204ff9cc3cb239c8d2.png)

Photo by [Alex Block](https://unsplash.com/photos/hp74PknYyXE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/window?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### “外部点击”是什么意思？

你可以把它想象成“反按钮”。外部单击是了解用户是否单击了除特定组件之外的所有组件的一种方式。您可能在打开下拉菜单或下拉列表并单击其外部将其关闭时见过这种行为。

这种特性还有各种各样的其他用例:

*   关闭下拉列表时
*   关闭模式窗口时
*   当可编辑元素在编辑模式中来回转换时
*   结束的
*   还有更多…

现在，让我们看看如何编写一个通用的、可重用的 React 组件来实现这种行为。

### 它会是什么样子

快乐的流程应该是这样的:

### 组件结构

为了让这个组件工作，我们需要在文档本身附加一个 click 事件处理程序。这将有助于我们检测何时我们点击了页面上的任何地方。然后我们需要检查我们点击的目标是否不同于我们包装的元素。所以一个基本的结构会是这样的:

对于第一个例子，我们将开始使用 React 类风格编码，然后用新的 Hooks API 重构它。

我们实施了两个生命周期功能:

*   **componentidmount()**:将附加事件监听器
*   **componentWillUnmount()** :将在组件被销毁之前清理点击处理程序

然后我们渲染组件包装的任何东西。对于我们上面的第一个例子，它将呈现。

### “点击外部”条件

现在我们需要检查用户是否在包装好的孩子之外单击。一个简单的解决方案是比较目标元素(我们点击的元素)和我们孩子的节点。但是，只有当我们有一个简单(单级)节点作为子节点时，这才会起作用。如果我们包装的子节点有更多的子节点，那么这个解决方案将会失败。

幸好有个方法叫 [**。包含()**T3，它告诉我们一个节点是否是一个给定节点的子节点。下一步是进入我们孩子的节点。为此，我们将使用](https://developer.mozilla.org/en/docs/Web/API/Node/contains) [React Refs](https://reactjs.org/docs/refs-and-the-dom.html) 。

Refs 是 React 让我们访问原始节点对象的方式。我们还将使用 Reacts API 来处理 **this.props.children** 组件。我们需要这个 API，因为我们将把我们创建的 ref 注入到我们包装的子元素中。考虑到这一点，我们的组件看起来应该是这样的:

完美，这应该像预期的那样工作。至少对于我们开心流(一个被包裹的孩子)来说是这样的。如果我们打算包装多个节点，我们需要做一些调整:

*   我们需要一个 refs 数组(和我们包装的孩子一样多)
*   我们需要使用 **React。Children.map** 来克隆每个孩子，并从我们的私有引用数组中注入相关联的引用

这应该没问题。现在让我们用钩子来重构它！

### 钩住

React 16.8 引入了一个名为 [Hooks](https://reactjs.org/docs/hooks-intro.html) 的新 API。有了钩子，我们可以编写更少的代码，并在代码库中占据更小的空间。此外，钩子利用了 JavaScript 中的一级公民函数。如果您熟悉 React 中的[无状态功能组件](https://hackernoon.com/react-stateless-functional-components-nine-wins-you-might-have-overlooked-997b0d933dbc)，那么您已经成功了一半。我们最初的重构如下所示:

到目前为止，我们仍然使用“旧的”React API 来声明一个简单的无状态功能组件。然而，我们仍然需要那些生命周期函数来将我们的处理程序附加到**文档**节点上。

这里就是 [**效果挂钩**](https://reactjs.org/docs/hooks-effect.html) 的用武之地。效果挂钩将替换我们的“ **componentDidMount** ”和“ **componentWillUnmount** ”方法。组件渲染后，效果钩子将被调用，这样它将帮助我们按时附加我们想要的处理程序。同样对于清理部分，如果效果钩子返回一个函数，那么这个函数将在组件被卸载之前被调用。所以现在正是做一些清理工作的时候。在下一次重构中，事情会变得更加清晰。

这是我们使用效果钩子的功能组件的最终形式。如果你想看看这两个例子，你可以在下面运行它们。(*您可以默认导出类组件或功能组件，应用程序将表现相同。*)

### 结论

尽管点击外部行为是一个广泛使用的特性，但在 React 中实现它可能不那么简单。在这个例子中，我冒昧地用 React 钩子做了一点实验，并以两种方式构建了解决方案，以比较这两种方法。我是功能组件的忠实粉丝，现在在钩子的帮助下，我们可以让它们更上一层楼。