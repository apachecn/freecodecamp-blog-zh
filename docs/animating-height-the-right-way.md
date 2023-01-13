# 动画高度-正确的方式

> 原文：<https://www.freecodecamp.org/news/animating-height-the-right-way/>

实话实说吧。制作高度动画可能是一个巨大的痛苦。对我来说，这是一场持续的战斗，既想拥有漂亮的动画，又不愿意支付与动画高度相关的巨大性能成本。现在——一切都完成了。

这一切都是从我做兼职项目开始的——一个简历生成器，你可以在那里分享你简历的链接，这些链接只在某段时间内有效。

我希望简历中的所有部分都有一个漂亮的动画，我构建了一个 react 组件来执行动画。然而，我很快发现这个组件完全破坏了低端设备和一些浏览器的性能。见鬼，即使是我的高端 Macbook pro 也很难在动画上保持流畅的 fps。

这样做的原因当然是在 CSS 中激活 height 属性会导致浏览器在每一帧上执行昂贵的布局和绘制操作。在 [Google Web Fundamentals 上有一个关于渲染性能的精彩章节，我建议你去看看。](https://developers.google.com/web/fundamentals/performance/rendering/)

简而言之——每当你在 css 中改变一个几何属性时，浏览器必须调整并计算这种改变对页面布局的影响，然后它必须在一个称为 paint 的步骤中重新呈现页面。

### 我们为什么要关心性能呢？

忽视性能是很有诱惑力的。这并不明智，但却很诱人。从业务角度来看，您节省了大量时间，否则这些时间可以用来构建新功能。

然而，性能可以直接影响你的底线。如果没有人使用它们，那么构建大量的特性有什么好处呢？亚马逊和谷歌所做的多项研究表明这是事实。性能与应用程序使用和底线收入直接相关。

性能的另一面同样重要。作为开发者，我们有责任确保每个人都可以访问网络——我们这样做是因为这是正确的。因为互联网不仅仅是你和我的，它是所有人的。

从 Addy Osmani 的[优秀文章](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)中可以看出，与高端设备相比，低端设备解析和执行 javascript 的时间要长得多。

为了避免在互联网上造成阶级分化，我们需要坚持不懈地追求表现。对我来说，这意味着要有创造性，并找到另一种方式来实现我的动画而不牺牲性能。

### 以正确的方式制作高度动画

如果您不关心如何操作，而只想看一个真实的示例，请查看以下链接，了解演示站点、示例和 react 的 npm 包:

*   [利用该技术的演示网站](https://resummy.no)
*   [香草 JS 中的实例](https://codesandbox.io/s/3x2zjl0l7m)
*   [react 中的简单例子](https://codesandbox.io/s/983z7n92rw)
*   [NPM 包装和反应文件](https://fredrikoseberg.github.io/react-anim-kit-docs/#/)

我问自己的问题是，如何避免动画高度所带来的性能损失。简单的回答——你不能。

相反，我需要对其他不产生这些成本的 CSS 属性进行创新。即转换。

因为变换无法影响高度。我们不能简单地把一个简单的属性应用到一个元素上就完事了。我们需要比这更聪明。

我们将用来实现高度的表演动画的方式实际上是通过用 transform: scaleY 来伪装它。动画分几个步骤完成:

这里有一个例子:

```
<div class=”ah-outercontainer”>  
<div class=”ah-background” style=”${this.getBackgroundStyle()}”></div>
<div class=”ah-innercontainer”>${this.markup}</div></div>`
```

*   首先，我们需要获得元素容器的初始高度。然后，我们将外部容器高度设置为这个高度。这将导致任何更改的内容溢出容器，而不是扩展其父容器。
*   在外部容器中，我们有另一个 div，它的位置完全跨越了 div 的整个宽度和高度。这是我们的背景，一旦我们切换转换，将被缩放。
*   我们还有一个内部容器。内部容器包含标记，并将根据其包含的内容改变其高度。
*   **诀窍是:**一旦我们切换了一个改变标记的事件，我们就获取内部容器的新高度，并计算背景需要缩放的量，以适应新的高度。然后，我们将背景设置为 scaleY 的新数量。
*   在普通的 javascript 中，这意味着一些双重渲染的诡计。一次获取内容器的高度来计算刻度。然后再次将缩放应用到背景 div，以便它将执行转换。

你可以在 vanilla JS 中看到一个活生生的例子。

在这一点上，我们的背景已经被适当地缩放以产生高度的错觉。但是周围的内容呢？由于我们不再调整布局，周围的内容不会受到变化的影响。

让周围的内容动起来。我们需要使用 transformY 来调整内容。诀窍是获取内容扩展的数量，并使用它通过变换来移动周围的内容。参见上面的例子。

### 反应中的动画高度

正如我前面提到的，我在 React 中从事个人项目时开发了这个方法。这个演示站点在其所有的“高度动画”中专门使用了这种方法。点击此处查看演示网站。

成功实现后，我花时间将这个组件和一些支持组件添加到我在 React 中制作的一个小动画库中。如果你感兴趣，你可以在这里找到相关信息:

*   [点击此处查看 NPM 图书馆](https://www.npmjs.com/package/react-anim-kit)
*   文档可以在这里找到。

该库中最重要的组件是 AnimateHeight 和 AnimateHeightContainer。让我们来检查一下:

```
// Inside a React component 
// handleAnimateHeight is called inside AnimateHeight and is passed the 
// transitionAmount and optionally selectedId if you pass that as a prop to // AnimateHeight. This means that you can use the transitionAmount to
// transition your surrounding 

content.const handleAnimateHeight = (transitionAmount, selectedId) => {     this.setState({ transitionAmount, selectedId });
};

// Takes a style prop, a shouldchange prop and a callback. shouldChange 
// determines when the AnimateHeight component should trigger, which is 
// whenever the prop changes. The same prop is used to control which 
// content to show.

<AnimateHeight 
  style={{ backgroundColor: "#f2f2f2" }} 
  shouldChange={this.state.open}   
  callback={this.handleAnimateHeight}
>
  {this.state.open && <Component />}
  {!this.state.open && <AnotherComponent />}
</AnimateHeight>
```

*   [移动周围内容的示例](https://codesandbox.io/s/jn5rp5jvzy)

上面的例子向你展示了如何使用 AnimateHeight 并手动触发你周围的内容进行调整。但是如果你有很多内容，不想手动做这个过程呢？在这种情况下，您可以将 AnimateHeight 与 AnimateHeightContainer 结合使用。

要使用 AnimateHeightContainer，您需要为所有顶级子项提供一个名为 animateHeightId 的属性，该属性也需要传递给 AnimateHeightContainer 组件:

```
// Inside React Component
const handleAnimateHeight = (transitionAmount, selectedId) => {     
this.setState({ transitionAmount, selectedId });
};

// AnimateHeight receives the transitionAmount and the active id of the AnimateHeight that fired. 
<AnimateHeightContainer
  transitionAmount={this.state.transitionAmount}
  selectedId={this.state.selectedId}
>
  <div animateHeightId={1}}>
    <AnimateHeight 
      style={{ backgroundColor: "#f2f2f2" }}
      shouldChange={this.state.open}
      callback={this.handleAnimateHeight}
      animateHeightId={1}
    > 
    {this.state.open && <Component />
    {!this.state.open && <AnotherComponent />}  
    <AnimateHeight />

    // When AnimateHeight is triggered by state change
    // this content will move because the animateHeightId
    // is greater than the id of the AnimateHeight component above
    <div animateHeightId={2}>I will move</div>
    <div animateHeightId={3}>I will also move</div>
<AnimateHeightContainer />
```

从这个例子中可以看出，当 AnimateHeight 组件被更改状态触发时，AnimateHeight 接收到调整内容所需的信息。

一旦发生这种情况，AnimateHeight 组件将触发回调并设置 state 中的属性。在 AnimateHeight 内部，它看起来像这样(简化):

```
// Inside AnimateHeight
componentDidUpdate() {
  if (update) {
    doUpdate() 
    callback(transitionAmount, this.props.animateHeightId)
   } 
}

// Equivalent to calling this function: 
const handleAnimateHeight = (transitionAmount, selectedId) => {     
this.setState({ transitionAmount, selectedId });
};

handleAnimateHeight(transitionAmount, this.props.animateHeight)
```

现在，您知道了以像素为单位的内容转换量，以及被触发的 AnimateHeight 组件的 id。一旦您将这些值传递给 AnimateHeightContainer，它将接受这些值并转换其自身内的其他组件，前提是您在顶级子级上设置了递增的 animateHeightIds。

更多高级示例:

*   [使用 AnimateHeightContainer 移动周围内容](https://codesandbox.io/s/6zz62yp65w)
*   [手风琴示例](https://codesandbox.io/s/202w3n81q0)

注意:如果您使用这种方法来制作高度和移动周围内容的动画，您需要将过渡量添加到页面的高度。

### 结论

你可能在这篇文章中注意到了，我们实际上并没有制作高度动画——称之为高度动画是错误的。你当然是完全正确的。然而，我坚信我们怎么称呼它并不重要。重要的是，我们要以最低的性能成本达到预期的效果。

虽然我认为我找到了一种比直接制作高度属性动画更好的方法，但我并不声称发明或想到了以前没有发现的东西。我也不做判断。也许在你的场景中设置高度动画对你有用——没问题。

我想要的只是实现和简化我们都需要做的效果，但有时会产生难以承受的成本。至少，我想引发一场值得讨论的讨论。怎样才能为大家改善互联网？