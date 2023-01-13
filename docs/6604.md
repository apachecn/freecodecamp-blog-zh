# 网络动画简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-web-animations-86f45de2a871/>

由 CodeDraken

# 网络动画简介

在这篇介绍 web 动画的文章中，我们将介绍使用**伪类**、**转换**和**转换**的基本 CSS 动画。

如果你不确定为什么应该使用 CSS 动画，那么看看这些 [文章](https://medium.com/bridge-collection/improve-the-payment-experience-with-animations-3d1b0a9b810e)[。](https://developers.google.com/web/fundamentals/design-and-ux/animations/css-vs-javascript)

本文的一些基本(目前来说非常难看)示例代码将放在 CodePen 上:

### 触发的

在我们开始一些动画之前，让我们想想它们将如何被激活。当页面加载时，我们的大多数动画不会立即运行。更常见的是*它们将由类变化(通过 JavaScript)或使用伪类来触发*。

#### 伪 Foo

下面是几个最常用于动画的[伪类](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)。

[**:悬停**](https://developer.mozilla.org/en-US/docs/Web/CSS/:hover)
当你用鼠标*悬停在目标*上时，就会触发悬停伪类。在这个例子中，我们设置了`<`；p >悬停时颜色变为蓝色。当鼠标从它上面移开后，它会恢复到原来的颜色。

```
<style> #hover-example:hover {  color: blue;  cursor: pointer; }</style>
```

```
<p id=”hover-example”>Hover example</p>
```

[**:焦点**](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus)

> " focus CSS 伪类表示获得焦点的元素(如表单输入). "— MDN

(嗯……那不就像用一个词来定义自己吗？？)

当输入和按钮被选择/激活时，焦点主要用于输入和按钮——也就是说，当你点击一个输入或跳转到它的时候。在本例中，单击或按 tab 键进入输入将导致其改变宽度和背景颜色。点击它会使它恢复到原来的大小(和颜色)。

```
<style> input:focus {  background-color: #f4f4f4;  width: 50vw; }</style>
```

```
<input type=”text”>
```

[**:激活**](https://developer.mozilla.org/en-US/docs/Web/CSS/:active)
激活看似类似于对焦，但通常只是瞬间触发*。首先想到的用例是锚(链接)。在这个例子中，当类`activate`被点击时(也就是说，当它处于活动状态时),我们对它进行任何修改。*

```
<style> .activate:active {  background-color: orange; }</style>
```

```
<p class=”activate”>Click me!</p><div class=”activate”>Activate me!</div><button class=”activate”>Hold me!</button>
```

### [变形金刚](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

> "转换 CSS 属性允许你旋转，缩放，倾斜或平移一个给定的元素. "— MDN

变换是你把你的 CSS 动画带到下一个层次的地方。大约有 21 个函数可以和 transform 一起使用，但是我们不会在本文中一一介绍。

#### Translate ( x, y )

翻译的意思是你在移动什么东西。在下面的例子中，我们将具有`translate`类的`10rem`移动到 X 轴上，将`5rem`移动到 Y 轴上。(如果你不熟悉 rem，你也可以用像素。)

这是一个组合了 X 和 Y 的简写函数，但是如果您愿意，也可以使用`translateX`或`translateY`来代替。

```
<style> .translate {  transform: translate(10rem, 5rem) }</style>
```

#### 缩放(x，y)

与`translate`类似，scale 也有一个`scaleX`、`scaleY`，和一个`scale`速记功能。用 scale 来*改变某物的大小*。在下面的例子中，`scale`类的元素被缩小了一半。

```
<style> .scale {  transform: scale(0.5); }</style>
```

#### [变换原点](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-origin) ( x，y，z)

在处理动画，尤其是旋转时，变换原点是一个重要的属性。这有点奇怪，很难用文字来解释，如果你还不熟悉《改变起源》(像在 Photoshop 中)，我强烈建议你看看 MDN 文档。文档说得好:

> “转换原点是应用转换的点”——MDN

想象一个轮子:

![1*Ez-Q3W-m0iDOFJyExQJMdw](img/6e0e6c1cc50eebd1fe500cc1adec7ec2.png)

ferris wheel from Unsplash

当轮子旋转时，它绕着中心点旋转。但是现在想象一下，中心点被移到了左上角。现在轮子绕着那个新的点旋转，因此提供了一种非常不同的体验。那个中心点类似于原点。参见[代码笔](https://codepen.io/CodeDraken/pen/OwOLrW)中的交互示例。

#### 旋转(角度)

Rotate 确实做了它所说的事情。你可以指定任何角度，负的，正的，任何数字，它会旋转那么多。有几个不同的值可以使用(deg，rad，grad) — [查看 MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/angle) 上的更多值类型。

### 让事情变得顺利

使用[过渡](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)我们可以平滑事物并控制动画的流动。

对于我们的动画来说，过渡就像补间动画或速度控制。它可以接受 4 个参数，我将详细讨论每个参数。

#### [过渡属性](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-property)

过渡属性是你正在制作动画的。这可能是颜色、大小、变换等等。你也可以说`all`来转换一切，但是你应该避免这样做，并且要更加具体。

你可以在 MDN 上制作一个[巨大的属性列表](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)。你应该避免制作任何不在列表中的动画。

`transition-property: all;`

#### [过渡持续时间](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration)

这是*动画需要多长时间完成*。使用秒或毫秒。

`transition-duration: 2s;`

#### [过渡-计时-功能](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function)

这就是事情变得更复杂的地方。过渡计时功能是一条*加速曲线，描述动画如何流动*。看看[这些](http://cubic-bezier.com/#.17,.67,.83,.67) [站点](https://easings.net/)，看看这条曲线是什么样子，以及它是如何影响动画的。

你可以让它慢慢进入然后慢慢出来，慢慢开始然后快速结束，或者一个更复杂的流程，包括一些慢的部分和一些快的部分。有很多方法可以让你的动画流畅。

幸运的是，我们可以使用一些预定义的值:

```
easeease-inease-outease-in-outlinearstep-startstep-end
```

你必须和他们玩一会儿，为你的动画找到一个。

有时候我们不得不使用`cubic-bezier`(或者[使用一个库](https://daneden.github.io/animate.css/))来制作自己的程序。为此，我建议你找一个在线工具(见上面的链接)或者使用你的浏览器开发工具来制作一个。

`transition-timing-function: cubic-bezier(.29, 1.01, 1, -0.68);`

#### [过渡延迟](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-delay)

这也许是最简单的价值。过渡延迟是在开始效果之前*等待的时间。使用秒或毫秒。*

`transition-delay: 500ms;`

#### [转换](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)(属性、持续时间、定时、延迟)

你可能已经猜到了，这是组合上述所有功能的*简写*。

这是一个过渡的样子:

`transition: background 1s ease-in-out 0.5s;`

对于 multiple，你需要把它们添加到同一个转换中，用逗号隔开。

```
transition: background 1s ease-in-out 0.5s,width 2s ease-in,height 1.5s;
```

### 最后

这就是你开始制作互动网站所需要的全部。去实践你所学的，一旦你掌握了这里所涉及的主题，你就可以继续学习更高级的动画了。

在下一篇(或两篇)文章中，我将讨论关键帧、3D 变换、性能、JavaScript 动画等等。

感谢阅读！如果你有任何问题、意见或批评，请在下面留言，我会尽快回复。