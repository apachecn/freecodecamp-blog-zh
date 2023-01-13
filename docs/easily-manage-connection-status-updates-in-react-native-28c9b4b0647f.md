# 如何在 React Native 中轻松管理连接状态更新

> 原文：<https://www.freecodecamp.org/news/easily-manage-connection-status-updates-in-react-native-28c9b4b0647f/>

地球的乔丹

# 如何在 React Native 中轻松管理连接状态更新

作为一名自由前端开发人员，我喜欢不时地创造和探索我在日常生活中没有时间去做的技术。最近，我在探索 React Native，并尝试一些新的工具和 API。

但是构建一个本地应用程序和构建一个 web 应用程序有一点不同。我最近遇到一种情况，用户没有活跃的互联网连接。

那么，我们如何告知用户我们的应用程序功能有限呢？

![1*Sp2YoeR7ngda2gFD5MkBKw](img/e0eb839c3806f774b42566b044da49ac.png)

Photo by [Galen Crout](https://unsplash.com/photos/ZYecenZy7o4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当您构建一个需要网络连接的应用程序时，您需要正确处理失败的请求。例如，当用户的互联网连接决定玩捉迷藏。因此，我们的应用程序可以通知用户请求失败的原因，甚至阻止请求启动。更好的是:向我们的用户显示一条有用的消息，解释正在发生的事情，这样他们就可以采取行动了。

换句话说，我们可以给我们的用户一些*上下文*来解释为什么应用程序不能执行某个请求。

#### Redux 与上下文 API

React Native 社区提供了一个 [NetInfo](https://github.com/react-native-community/react-native-netinfo) 模块来公开关于用户网络连接的信息，比如它是在线还是离线。我们需要这些数据在我们的应用程序中全球可用。

一个普遍的想法是使用 Redux。我的 App 已经用 Redux 了，为什么不用 Redux 来做呢？

当然，我们可以。但是，如果我们想要使用这个连接信息，就需要将每个组件都连接到 Redux 存储中。挂钩 Redux 会产生额外的开销和更多的代码行，并可能使我们的应用程序变得比需要的更复杂。

让我们探索其他的可能性…

React 的上下文 API 提供了一种**更简单的**、**更干净的**方式来通过我们的组件共享类似状态的数据:

> Context 旨在共享可以被视为 React 组件树的“全局”数据，例如当前经过身份验证的用户、主题或首选语言。— [来源](https://reactjs.org/docs/context.html#when-to-use-context)

看起来我们有一个完美的用例来使用 React 的新上下文 API！

### 让我们开始吧

首先，我们必须安装所需的包，因为在 React Native 0.59 中，`NetInfo`模块在一个单独的包中。React 16.6 或更高版本也是必需的，因为它允许上下文在渲染方法之外可用。非常有用，因为这给了我们在使用这个上下文时更大的灵活性。

我不会打扰你设置一个 React 本地应用程序，只是假设你已经有一个。

让我们安装`NetInfo`包:

```
npm install @react-native-community/netinfo --save
```

安装完成后，我们就可以创建我们的组件了。

**创建上下文提供者**
让我们设置一下`<NetworkProvide` r >组件。该组件将我们的连接状态传递给所有子组件:

如上所示，我们只是监听`connectionChange`事件。当有活动的互联网连接时，该事件返回`true`,当用户没有活动的互联网连接时，该事件返回`false`。当连接状态改变时，我们更新状态。

一旦我们更新了状态，组件树中的上下文就会改变。因此每个组件都可以访问更新后的`isConnected`值。类似于 Redux，但样板代码更少。

**包装上下文提供者**
为了让 React 的上下文 API 工作，我们需要将我们刚刚创建的这个`<NetworkProvide` r >组件包装在我们的其他组件周围，就像这样:

通过这样做，我们使`context`在`<NetworkProvid` er >的每个组件中都可用。

最后一步是在组件中使用上下文。我们现在使用一个`<ExampleCompone` nt >:

现在我们的组件使用了上下文 API 并且`this.context.isConnected`可供我们使用。

现在，当用户的互联网连接在线或离线时，我们可以在`<ExampleCompone` nt >中向用户显示一条消息。

在 React 的早期版本中，`context`在 render 方法之外不可用。[从 React 16.6.0](https://reactjs.org/blog/2018/10/23/react-v-16-6.html#static-contexttype) 开始，使用`static contextType`就可以了，如上例所示。像这样使用它给了我们更大的灵活性，让我们可以在组件中使用上下文。

#### 最后一点

因此，我们已经展示了上下文 API 非常适合在这个用例中设置和使用这些全局值。连接状态在我们的整个应用程序中都很重要，因此我们可以在需要活动互联网连接的操作失败时通知用户。

我们可以用 Redux 做同样的事情，但是这需要更多的代码。让我们尽可能使用本机 React API，因为它限制了依赖性！

完整的要点可以在我的 GitHub 上找到。

### 感谢阅读！

我已经使用 Medium 很多年了，但通常只是阅读和学习其他人的内容。这些年来，像这样的教程对我帮助很大。所以，写我自己的是我回馈这个了不起的开发者社区的方式！

这个教程对你有帮助吗？在评论里让我知道？

**收到了对我改进文章的反馈？我渴望提高和分享我的知识。**