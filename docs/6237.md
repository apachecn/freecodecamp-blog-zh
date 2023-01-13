# 如何通过 React 中的样式使您的应用程序变得漂亮

> 原文：<https://www.freecodecamp.org/news/styling-in-react-101-2536b696219b/>

作者 Vinh Le

# 如何通过 React 中的样式使您的应用程序变得漂亮

![1SXKuDDFwlYF4KGzddj1K6eBuutY4YacH71h](img/dd098a8900d6e944b7d75a0d07058339.png)

当谈到 React 中的样式时，有太多的方法和技术选择来美化您的 web 应用程序。尽管如此，根据我的个人经验，决定使用哪一个确实取决于你的应用程序的 UI 需求。

### 是不是太容易了？

如果你在这里停下来说:“这很容易！只需谷歌查找顶级的 React UI 库，选择其中一个，你就可以开始了”，那么你可能没有经历过在这些库中配置预样式组件的痛苦经历。

你在某件事情上花的时间越多，你就越熟悉，你需要花在故障诊断上的时间就越少。React 中的样式也不例外。然而，它确实需要相当多的时间，至少对我这个自学成才的开发人员来说，才能做出明智的选择。

因此，我在这篇文章中的主要目的是快速介绍最佳的 React 样式选择，更重要的是，尝试进一步向您阐述何时应该使用哪种样式。

### 为什么造型？

![p247brEV3QCLUH97LtMsI0w7YhhekP5ZyfTd](img/ffd5fd7bdeca08c1c05b4f4e4e26d323.png)

Aesthetics ? Functionalities ? Or both?

又是一个死简单的问题，不是吗？好吧，如果我稍微改变一下这个问题:“为什么在你开始学习反应的时候就学习正确的造型？”它可能会激活你的思维流。

如果你是 React 生态系统的新手，你的第一个教程可能会教你如何开始一个项目，告诉你如何管理状态和处理道具。在线课程的第一部分仅仅提到了样式，因此在你的第一个应用程序中很少用到。

在我让你听起来过于严肃之前，让我告诉你“这完全没问题。”你的表现绝对没有问题。另一方面，如果你一开始就把样式放在一边，更多地关注应用程序的逻辑和功能，那就更好了。

然而，如果你从一开始就关心你的作品的美学，那么你可能对你的功能性但风格简约的应用程序不完全满意。

好了，说够了。让我总结一下应用程序样式化的好处，无论是从你的第一个“Hello world”还是你正在进行的项目:

*   **开始时漂亮的用户界面** —还记得 React 为什么存在吗？来帮助我们创建动态用户界面。更完美的用户界面有助于更好的用户体验。因此，如果我们设身处地为用户着想，我们就会意识到吸引人的设计是用户友好的应用程序必不可少的一部分。
*   良好的开发环境，尤其是当你在做你的副业时。如果一个好的设计让你喜欢使用它，你也许会有更多的灵感来用设计优先的方法开发一个应用程序。同样，这来自事物的美学方面。如果你只是想让它工作，这可能不适合你。
*   **避免造型压倒后来的** —想象一下，当你在一个项目上工作了一段时间，突然回头想想你需要做多少造型。如果它只是一个沙盒项目，那么它可能是好的。但是如果你的应用程序需要多层容器和元素，并且响应是必须的，那么这将是一个相当大的工作量。

**那我应该用什么来让我的 React app 好看呢？**

![S28rjd2vX8bKPhxNO1m1JpYO9QvorQtJ-3lK](img/7548156e665841ecd0e58da0ac755712.png)

You already got an idea, let’s now consider alternatives

### 内嵌样式

这种方法是最简单的开始方式，因为它不需要配置，您可以立即看到结果。然而，即使您熟悉 CSS，也要注意错别字，因为它们可能会让您头疼:

```
<div style={{ width: 50, height: 50, background: '#000'}}>    I am a square box with black background</div>
```

#### **外卖**

*   内联样式是由任何 DOM 元素中的**样式属性**完成的，它有一个**对象**类型，其中**键**是普通的 CSS 属性，**值**是等价的 CSS 值。
*   因为没有像许多 CSS 属性那样的破折号，所以您应该注意到大写，例如:**`backgroundColor``backgroundImage``textAlign`和`flexDirection`。**
*   **当您定义一个存储所有样式逻辑的独特变量时，它会更有条理:**

```
`const styles = {  alertMessage: { color: 'red' },  ... // other styling rule};...`
```

```
`<span style={styles.alertMessage}>Unknown error</span>`
```

*   **可以做条件造型。例如:**

```
`<span    style={{ color: this.state.isWarning ? ‘red’ : ‘black’  }}>   Let’s see if I am a warning </span>. </span>`
```

#### **赞成的意见**

*   **易于应用，非配置。**

#### **骗局**

*   **当你的项目变得更复杂时，你的 JavaScript 文件会变得更长更乱。一种方法是在外部 JavaScript 文件中定义样式变量，然后将它们导入回来。然而，这确实需要一个额外的步骤，并且与下面的造型方法相比变得更加难以使用。**
*   **app 重载导致的开发速度降低。如果你使用像 create-react-app 这样的工具，你的应用会在你每次做改动的时候热重装。否则，您必须手动重新加载页面才能看到更改。因此，根据你的应用程序的复杂程度，仅仅是重新渲染就要花费逐渐变长的时间。**

#### **什么时候用？**

**当你第一次开始学习 React 的时候，是选择这种方法最合适的时候。此外，如果你的项目很小，或者你只是想在你的应用程序上应用一些小的样式。例如，响应能力并不重要。那么内联造型就可以了。**

### **半铸钢ˌ钢性铸铁(Cast Semi-Steel)**

**好了，不再有奇怪的 CSS-in-JS 了。这是你喜欢的原始 CSS:)，开始前只需简单配置:**

*   **创建您的 CSS 文件并将其导入 JavaScript 文件:**

```
`import ‘./App.css’;`
```

*   **将 className 添加到要应用样式的元素中:**

```
`<p className=’warning-message’>Warning</p>`
```

**请注意，现在它是 **className** ，而不是普通的 **class** — 只是一个典型的 React 东西。**

*   **遵循您的造型规则:**

```
`App.css.warning-message { color: red;}`
```

*   **通过设置等效类名的条件样式:**

```
`<p   className={this.state.warning ? ’warning-message’ : ‘normal-message’}>Warning</p>`
```

#### **赞成的意见**

*   **编写你熟悉的 CSS 规则，减少打字错误的风险。**
*   **受益于 CSS 特性，如变量和媒体查询。**
*   **组织良好的项目结构。**
*   ****更高的开发速度**——这可能是我在开发过程中经历的最愉快的提升。当您在 CSS 文件中进行更改时，您的应用程序将不会重新呈现。因此，您的更新需要一秒钟才能显示在屏幕上。你的应用程序越大越复杂，节省那些不必要的重新加载时间就越有快感。**

#### **骗局**

*   **与 SCSS 相比，它缺少一些特征，我将在这之后深入探讨。**

#### **什么时候用？**

**你可以从一开始就使用 CSS，不管你的应用程序有多大。因为它几乎是零配置的，而且 CSS 可能对很多都很熟悉，所以很容易快速上手。**

**如果你的应用变得越来越大，有一个更加复杂的设计系统，考虑一下 SCSS 相对于 CSS 的优势。**

**尽管如此，请记住，您完全可以使用纯 CSS。SCSS 并不是一个真正的游戏规则改变者，它能提供你从 CSS 中得不到的所有好处。最近，像变量这样的全新特性开始缩小 CSS 和它的前置处理器之间的差距。此外，如果你以前没有使用过 SCSS，这将需要一些时间来熟悉它。**

### **SCSS**

**这可能是我反应造型的首选。它包含了 CSS 的所有熟悉之处和优点，再加上一些非常有用的特性，可以制作一个很棒的包。如果你不熟悉 SCSS，他们有很棒的文件供你查阅。**

**为了在 React 应用程序中使用 SCSS，需要做一些配置。如果你正在使用 create-react-app，这个[指南](https://medium.com/@oreofeolurin/configuring-scss-with-react-create-react-app-1f563f862724)可能对你有帮助。**

**接下来，让我向您展示 SCSS 的优势，与普通 CSS 相比，它是一个更好的选择。**

#### **嵌套**

**当你的项目越来越大时，你的 CSS 文件很有可能充满了类名。当你的设计由嵌套的块、容器和元素组成时，事情变得更加令人生畏。因此，在数百行的文件中查找某个特定的类名既累人又耗时。这就是嵌套派上用场的地方:**

```
`App.scss.intro-container {  h1 { font-size: 20px };  .nested-child {    display: block;    p {      margin: 0;    }  }}`
```

**例如，在这个结构中，您希望改变 intro 容器中子元素的样式。您现在需要做的就是找到它的类名，在本例中是`intro-container`。那么它的孩子的所有风格都可以在里面找到。容易多了，不是吗？**

#### **混合蛋白**

**mixins 给表带来的最大好处之一是在媒体查询中定义断点。让我们来看看这个例子:**

```
`_mixins.scss`
```

```
`// define breakpoint for mobile device  @mixin bp-mobile {    @media only screen and (min-device-width: 320px) and (max-device-width: 480px) {      @content;    }  }`
```

**回到主 SCSS 文件:**

```
`App.scss`
```

```
`body {  width: 60%; margin: 0 auto;  @include bp-mobile {    width: 90%;  }}`
```

**相比于:**

```
`App.css`
```

```
`// set width of the body to 90% only in mobile devicesbody {  width: 60%; margin: 0 auto;}...@media only screen and (min-device-width: 320px) and (max-device-width: 480px) {  body {    @include bp-mobile {      width: 90%;    }  }}`
```

**我相信在 SCSS 这要自然和容易得多。如同对一个元素应用样式规则一样，您可以清楚地看到它在其他视口中的外观。因此，改变和调整是直接进行的，而没有在 CSS 中向上滚动的负担，因为人们通常在 CSS 文件的末尾定义媒体查询。**

#### **遗产**

**这对于让你的代码[干燥](https://en.wikipedia.org/wiki/Don't_repeat_yourself)非常有帮助。假设您想要为两个按钮应用相似的背景和边框，只是其中一个按钮的文本颜色为红色，另一个按钮的文本颜色为蓝色:**

```
`// define common rules%button-common {  background: #fff;  border: 1px solid gray;  border-radius: 3px;}`
```

```
`.button-red {  @extend %button-common; color: ‘red’;}`
```

```
`.button-blue {  @extend %button-common; color: ‘blue’;}`
```

**让我们总结一下 SCSS 的利弊:**

#### **赞成的意见**

*   **基本上所有来自 CSS 的专业加上鲜明的特点，使人们热爱 SCSS。**

#### **骗局**

*   **需要配置才能使用。**
*   **对于不熟悉 SCSS 的人来说，确实需要一定的时间来学习。**

### **React UI 库**

**由于开源社区的蓬勃发展，有了非常棒的 React UI 库，您可以在项目中使用它们。优秀的资源有 [MaterialUI](https://github.com/mui-org/material-ui) 、[React-Bootstrap](https://github.com/react-bootstrap/react-bootstrap)……不一而足。**

**让我们以最流行的库之一 MaterialUI 为例:**

#### **装置**

```
`npm install @material-ui/core`
```

**为了使用它，你必须非常依赖它的文档，这些文档起草得很好，而且是按照谷歌材料的方式设计的。然而，如果你看一个代码样本的组件，这是一种令人生畏的。我的方法是只导入你想要使用的组件，注意一些重要的道具，然后再定制它。**

**假设我们想要创建一个按钮:**

```
`App.js`
```

```
`import { Button } from ‘@material-ui/core’;...`
```

```
`<Button  color=’primary’  onClick={() => console.log(‘clicked’)}  fullWidth> View </Button>`
```

**嘣！一个标签为“View”的蓝色按钮，其宽度值等于其父容器的宽度值，很好地出现在屏幕上。**

**就像这样，您几乎可以使用库中的所有组件。使用它的优势是一个明显的抛光和现代的材料设计。如果你想自己创建一个组件，可能需要做大量的工作，而且你的最终结果看起来可能还不如预先设计好的组件。**

#### **为什么我们不都用这个呢？**

**首先，如果让它出现在屏幕上看起来超级容易，那么定制并让它完美融入你的设计体系绝对不是一件容易的事情。**

**定制这些组件的一种方法是覆盖它的样式，大多数时候是通过 style prop。另一种方法是给它一个类名，用 CSS 写出自己的风格。在这种情况下，如果你的 CSS 完全有效，但是你的组件没有任何变化，记得在你的规则后面加上`!important`。**

#### **那么你什么时候会使用 React UI 库呢？**

**如果你正在做一个小的副业项目，像 MaterialUI 这样的库很好用。因为你只需要专注于 JavaScript 方面的事情，仍然有一个非常好看的应用程序。**

**然而，当你计划制作一个复杂的 UI 嵌套层的应用程序时，一定要注意，有时候创建你自己的可重用组件可能比试图定制预先设计好的组件更快。以我个人的经验，如果你真的需要特殊的组件，而这些组件很难按照你的意愿来设计或运行，那么你就使用它们。否则，最好创建您自己的，并充分利用您对它们的控制。**

**所以，在这里，他们是流行的反应方式。当然，仍然有许多其他伟大的图书馆和黑客。请在评论区分享你的观点。随着 React 社区越来越大，我们可以期待更多的“明日之星”。**

**此外，当前开源库的优秀维护者和开发者只会试图让他们的解决方案更好、更完善、更易于使用。这些都预示着光明的未来:)**

#### **感谢阅读！**

#### **在社交媒体上打招呼:[脸书](https://www.facebook.com/VinhLee95)、[推特](https://mobile.twitter.com/vinhle95)、[领英](https://www.linkedin.com/in/vinhlee95/)，或者我的[个人网站](http://vinhlee.com/)。**

#### **敬请关注即将到来的科技博客？？？**

#### **回头见！**