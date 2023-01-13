# 如何使用 React 和 React-Sentinel 来制作响应性的哑组件

> 原文：<https://www.freecodecamp.org/news/how-to-use-react-and-react-sentinel-to-make-responsive-dumb-components-51a04c6279a3/>

瑞安·尤尔卡宁

# 如何使用 React 和 React-Sentinel 来制作响应性的哑组件

![0*-M7kIz-f-VmOfAy6](img/f39b928c53165baf91f51354980594ee.png)

Sometimes you just need a friend to stand guard! Photo by [Aldo De La Paz](https://unsplash.com/@aldodlp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**tldr；媒体询问并不总是足够的。元素查询是惊人的，你可以用渲染道具的组合和观察你的元素的东西来黑箱化它们！**

#### 处理媒体询问

如果你不得不重新创建一个响应性设计，那么你就知道媒体查询有多棒——但也有多麻烦。

媒体查询允许仅在大小相对于视口改变时应用的 CSS。

不幸的是，如果您想要制作一个可重复使用且响应迅速的卡组件，媒体查询就不太理想了:

1.  您需要找出响应卡的高度和宽度与视窗的高度和宽度之间的关系。
2.  如果你的卡片是一个更复杂的布局(如 flex 布局)，那么你需要弄清楚窗口大小将如何改变 flex 布局，以及它将如何影响卡片。？
3.  可能会有 JavaScript 来切换以编程方式改变卡片大小的条件，所以您还必须考虑到这一点，并将其传达给样式表。

![1*vIDJ7ghnI_MUu0kfVUD9DA](img/2048f3fc6d302ce2ffe23724c20ad62f.png)

… and now we are in CSS calc() hell. ?

在这一点上，我开始质疑我当初为什么要从事开发工作

我想要的只是一种基于该元素的高度和宽度来设计卡片样式的方法。许多人都想拥有这种能力。一个提议是元素查询，而且[现在有几种方法可以在 CSS 中使用它们！](https://elementqueries.com/)？

但是，如果你像我一样，你不仅希望能够将它引入 JavaScript 生态系统，还希望能够将其引入 React 生态系统。我们能制造一个智能的、黑盒的、反应灵敏的容器和一个愚蠢的视觉组件吗？

是的，我们可以。

#### 解决方案

这个组件的美妙之处在于，它不知道为什么会是现在的大小。这取决于使用它的人，这意味着这个组件可以在许多布局中重用。我们的目标是**保持这种方式**，同时让它变得令人敬畏。

让我们看看如何通过引入`react-sentinel`来做到这一点，并用它创建一个智能响应容器！？

那么**实际上**是怎么回事呢？

`react-sentinel`的工作原理是采用一个函数，即`observe`道具，并在一个执行`requestAnimationFrame`或`requestIdleCallback`循环中反复调用它。

`requestAnimationFrame`以浏览器决定的速度循环。如果有人在旧手机上浏览，这种循环就不会经常发生。这给了浏览器更好的控制，带来更流畅的体验！

如果你想了解更多关于`requestAnimationFrame`的知识，我建议阅读本杰明·德·科克的《获得动作超能力与请求动画帧 》！？

`Sentinel`从这些函数中获取返回值，如果它与之前的返回值不同，就将其设置为`Sentinel`组件的本地状态。如果没有不同，那么我们就停在那里不更新，这样我们就不会不断地重新渲染！？

#### 使用渲染道具

现在，你可能会问设置`Sentinel`的本地状态有什么好处？我们要怎么得到它？？

我的首选方法是使用渲染道具。

大多数人都知道可以向组件传递子组件，并使用`this.props.children`访问它们，但是也可以传递函数！

```
<MightHaveSecrets>  {() => <WantsSecrets />}</MightHaveSecrets>
```

好吧，那是一件事。为什么会有人想这么做？？

因为现在，**有秘密**可以把它的内部秘密作为参数传递给那个函数！它不知道你实际上将如何使用这些秘密，这使得它被超级封装。

```
<MightHaveSecrets>  {secret => <WantsSecrets emoji={ secret ? ? : ? } />}</MightHaveSecrets> 
```

所有的`<Sentinel` / >组件关心的是轮询无限期地寻找更新自己。Render Props 允许任何 UI 块在它们认为合适的时候解释那些更新。同样，这些价值的来源也更加明显。？

如果你想了解更多关于渲染道具的知识，我建议你看一看 React 文档或者读一读这篇文章，作者是第一个让我对它们感兴趣的人。

现在，我们有了一个智能组件，它将元素的大小转换成简单的道具，`<DumbCard` / >可以消化。重构和交换值超级容易，你不用担心它在什么布局中，或者在它的作用域之外发生了什么。

#### 包扎

想象一下，为一张用户可以调整大小的卡片编写 CSS 有多困难。现在，我们对任何改变元素大小的事物做出反应。

关于`react-sentinel`很酷的一点是，它不仅仅解决了元素查询的问题。我还用它创建了一个智能动画组件，因为它使用了`requestAnimationframe`？

[在这里](https://github.com/YurkaninRyan/react-sentinel)你可以查看`react-sentinel`的代码，以及一些替代的解决方案！

如果您有任何问题，或者您希望看到更深入讨论的任何主题，请随时联系我！感谢阅读和快乐编码！？