# 重新审视 React 组件生命周期在异步渲染中的应用

> 原文：<https://www.freecodecamp.org/news/how-to-safely-use-reacts-life-cycles-with-fiber-s-async-rendering-fd4469ebbd8f/>

亚历克斯·布朗

# 重新审视 React 组件生命周期挂钩在异步渲染中的应用

![1*zE7ymidBZ9BffwrT4MbfHw](img/94b4686a17c6bb58584c69c73efc9e5f.png)

如果你已经浏览了[文档](https://reactjs.org/docs/react-component.html#constructor)，或者留意了[核心反应团队](https://twitter.com/dan_abramov/status/790581793397305345?lang=en)的建议，你可能已经读到你不应该在`constructor`或`componentWillMount`中处理订阅或副作用。

虽然建议很明确，但这些指令背后的推理并没有被详细阐述，尽管[并非没有理由](https://github.com/reactjs/reactjs.org/issues/302#issuecomment-345445888)。简单的解释是，激发这些指令的纤程异步渲染的实现细节还没有完全解决。

因为 Fiber 的异步渲染还没有启用，所以忽略一些关于生命周期使用的智慧可能还不会伤害到你。在未来，这可能会改变，这就是我们在本文中要探讨的。

### 澄清:光纤准备好了吗？

如果 Fiber 的异步渲染还没有准备好，你可能想知道这个团队是否卖给了你一个伪造的倒计时器。请放心，事实并非如此。Fiber 的新引擎，或者更具体地说是协调过程，已经与 React v16 一起投入运行。也就是说，我们[还不能将](https://reactjs.org/docs/codebase-overview.html#fiber-reconciler)从同步渲染切换到优先渲染。

### 使用生命周期会受到怎样的影响？

最后，在异步渲染确定之前，我们还不知道。否则 React 团队也会这么说。但是我们可以得出一些关于处理订阅和副作用的安全结论。这就是我们将要探索的。

为了简单起见，这里有一个在构造函数中订阅[媒体查询列表](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList)的例子，这个**目前**不会给我们带来问题:

在启用异步渲染之前，我们没有任何问题，因为我们可以对组件做出以下保证:

1.  如果我们选择使用的话，`constructor`之后会同步跟随`componentWillMount`，然后是`render`。**重要的是，渲染前我们不会被打断。**正因为如此，我们可以进一步保证…
2.  如果将来卸载该组件，`componentWillUnmount`将预先清理事件监听器(订阅)。这意味着`window`不会通过媒体查询列表保留对组件的`handleMediaEvent`方法的引用，因此允许卸载的组件被垃圾收集，从而避免内存泄漏。一次清理失败没什么大不了的，但是组件的重新安装和添加更多的监听器可能会导致应用程序在整个生命周期中出现问题。

> 有一个警告:错误边界。我一会儿会谈到这一点。

#### 那么异步渲染会带来什么变化呢？

开门见山地说:你的类组件的许多生命周期方法可以触发多次。这是因为纤程的协调过程允许 React 产生它正在做的工作。允许主线程处理像动画一样急需显示的东西。这可能涉及丢弃已经完成的工作，可能包括对`constructor`、`componentWillMount`、`render`、`componentWillUpdate`和`componentWillReceiveProps`的调用。

但是，`componentDidUpdate`和`componentDidMount`只有在 React 刷新了对其主机环境的更改后才会被调用。从而避免这些问题。在`componentWillUnmount`中的清理或‘拆除’应该镜像`componentDidMount.`中的设置，帮助确保调用这个钩子失败不会有问题。

因此，我们需要在`componentDidMount`中处理订阅和副作用。在`constructor`和`componentWillMount`中发生的副作用通常包括网络请求。当它们导致我们应用程序的后端数据存储发生变化时，多次调用它们尤其麻烦。

最后一点。

和我一样，你可能认为 React 的第一次渲染保证总是同步的。但是，不一定是这样的！

Brian Vaughn(React 核心团队的成员)告诉我，目前的意图是第一次渲染默认为同步，可选的异步是可选的。他补充说，例如，如果 React 的主机容器尚未准备好，低优先级的第一次渲染可能是有价值的。显然，这更适用于 HTML 主体由多个用于 React 渲染的`div`组成的情况。

关于什么是安全执行以及在哪里执行的直观清单，参见 Brian 的[要点](https://gist.github.com/bvaughn/923dffb2cd9504ee440791fade8db5f9)。

### `componentWillMount`有什么用途？

用例非常狭窄。开发人员经常引用他们的两个可取的特征:

1.  `setState`可以从`componentWillMount`中调用，不像`constructor`。
2.  与`componentDidMount`不同的是，`componentWillMount`中的一个`setState`如果在`render`之前同步发生，就不会导致两次渲染。

同样，最初将`componentWillMount`保留在代码库中的原因，正如 Sebastian markb ge[在一份反对`componentWillMount`的提案中向](https://github.com/facebook/react/issues/7671)解释的那样，是为了处理一种副作用，这种副作用可能是同步的(如果本地缓存保存所需的数据),也可能是异步的。今天，正如他的演示代码块所传达的，`getInitialState`，es6 类构造器和 es7 属性初始化器迎合了这个目的。

综上所述，从`componentWillMount`发起的只读 GET 请求可能是有用的。在一个渲染速度慢的设备上，比如一个普通的移动设备，通过在这里而不是`componentDidMount`发起请求可以节省几百毫秒。当然，这样的请求应该是幂等的/只读的，因为它可能触发不止一次。

> 在服务器上渲染时，`componentWillMount`仍然是除了`constructor`之外唯一被调用的生命周期方法，所以这里可能有一些用例。由于我自己没有尝试过服务器端渲染，所以我不能详细阐述这个主题。

### 那么，这些警告是不是只有在异步渲染开始时才有意义呢？

正如布莱恩向我指出的，不完全是。React v16 推出的错误边界也会导致调用`componentWillMount`和`componentWillUpdate`而没有相应的`componentDidMount`和`componentDidUpdate`！

### 还有其他需要警惕的变化吗？

React 最近启动了一个 [RFC(征求意见)](https://reactjs.org/blog/2017/12/07/introducing-the-react-rfc-process.html)流程，使更广泛的社区能够讨论想法。第一批 RFC 中有两个来自 React 核心团队的成员，他们正在讨论重大的潜在变化。

1.  Andrew Clark 提交了一份关于上下文 API 变更的 RFC。这有望减轻在试图沿着组件树传播状态时绕过`shouldComponentUpdate`的困难。这里的 RFC 是。
2.  Brian 提交了一份异步安全、静态生命周期挂钩的 RFC。这主要包括逐渐贬低`componentWillMount`、`componentWillUpdate`和`componentWillReceieveProps`。提出了两种新的静钩:`prefetch`和`deriveStateFromProps`。你可以在这里阅读提案[，在这里](https://github.com/bvaughn/rfcs/blob/static-lifecycle-methods/text/0000-static-lifecycle-methods.md)阅读 RFC [。希望这篇文章为您提供了一些关于为什么提出这些变化的好的见解:)。](https://github.com/reactjs/rfcs/pull/6)
3.  在前面提到的提议中，Brian 还戏弄了一个即将到来的 RFC，要求一个新的 SSR 钩子:`componentDidServerRender`，取代服务器上的`componentWillMount`。

请记住，这些都是早期的建议！

### 关于作者

我是一名住在阿德莱德的澳大利亚开发人员，对 JavaScript 的前端和后端开发都充满热情！我最近发布了我的第一个 React 开源库:[React-MQL-管理器](https://github.com/AWebOfBrown/React-MQL-Manager)。这里有一个简单的演示，集成了 React 路由器 v4 用于[响应路由！](https://codesandbox.io/s/lo3p1wkjz)

我目前正在寻找我的第一个前端或全栈职位。你可以通过[电子邮件](mailto:ajcbrown820@gmail.com)联系我，或者在推特上向我问好:[@ awebobbrown](https://twitter.com/awebofbrown)。

### 特别感谢

非常感谢 React 核心团队的布莱恩·沃恩(Brian Vaughn)花时间阅读文章草稿并提出修改建议。除了在 React 上工作，Brian 还编写了一些很棒的开源库，如 [React-Virtualized](https://github.com/bvaughn/react-virtualized) 和 [JS-Search](https://github.com/bvaughn/js-search) ，并在 StackOverflow 等论坛上帮助回答社区问题。