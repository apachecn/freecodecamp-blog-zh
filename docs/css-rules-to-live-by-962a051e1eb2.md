# 让你的生活更轻松的 CSS 规则

> 原文：<https://www.freecodecamp.org/news/css-rules-to-live-by-962a051e1eb2/>

尼克·加德

# 让你的生活更轻松的 CSS 规则

![kYaVc2rjxrZ5WlkxpVl-JIOHQBiCDBEM6BKE](img/86a6fcc917cb69bb1818c475347a0713.png)

Photo by [Sven Mieke](https://unsplash.com/@sxoxm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

经过多年编写和维护几个非常大的 web 项目和许多较小的项目，我已经开发了一些编写可维护 CSS 的启发式方法。我使用 BEM、SMACSS 和 CSS 模块进行命名，尽管本文本质上并不是关于命名的。(我倾向于混合使用原子类和模糊命名。)这篇文章更多的是关于我使用或避免的属性和值。

> 我的 StyleLint 配置:[https://github . com/nick gard/CSS-utils/blob/master/StyleLint . config . JSON](https://github.com/NickGard/css-utils/blob/master/stylelint.config.json)

#### 颜色；色彩；色调

我最讨厌的是 web 项目中过多的颜色值。几年前，我参与了一个大型的长期项目，在 40 多个 CSS 文件中声明了 300 多种独特的颜色。其中三分之一是灰色阴影。品牌颜色重复出现，色调略有不同。这些颜色中的许多颜色都有细微的差别，比如`#3426D1`和`#3426D2`。这个问题的解决方案是使用原子颜色类或变量(在 SCSS 或 CSS 中)作为公认的品牌颜色。

限制可接受颜色的数量还有一个好处，就是可以简单地确保背景色和前景色符合 WCAG2.0 颜色对比度准则。

另一个容易出错的实践是使用 alpha 通道颜色，通常通过用`rgba()`或`hsla()`函数来声明颜色。用这种方法创建的颜色的 alpha 通道值不是`1`，它是半透明的。感知的颜色现在根据*背景*中的内容而变化。通常，所需的颜色是白色背景上的颜色，所以可以使用十六进制值。一些预处理器函数，比如 SASS 的`lighten()`，会生成半透明的颜色，所以坚持使用硬编码的值或变量。

#### 排印

所有影响字体或受字体影响的属性都应该一起声明*一次*。在声明任何`@font-face`规则之后，我喜欢为字体添加原子类，这些原子类改变`font-size`(通过`rem`)并包括`line-height`、`letter-spacing`和`word-spacing`，它们适合字体和大小的组合。之后，任何规则集中都不应该使用任何`font-*`或`text-*`(除了`text-overflow`)属性。

将这些属性与字体一起声明一次，可以确保站点上的副本看起来总是正确的。调整`line-height`而不是`padding`或`margin`会在文本换行时产生错误。与字体声明分开调整`font-weight`有创建[假粗体字体](https://alistapart.com/article/say-no-to-faux-bold/)的风险。在不支持的字体上更改`font-style`会产生一个假斜线。

最后，避免用除了`rem`单位之外的任何单位设置字体大小。使用`em`会导致嵌套元素时出现问题，因为`em`是当前*的标量倍数*。使用`px`(或任何其他“固定”测量)有产生难以阅读的副本的风险**和** [用户无法调整](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size#Pixels)。允许用户(或用户的浏览器)通过不在`body`或`html`元素上声明`font-size`而只使用`rem`来设置`font-size`为他们所需要的。

#### 间隔

在一个内容第一的网站上，间距应该与内容互补。任何静态测量，像`padding: 4px`，在*某*字号看起来都不对。响应字体大小的动态测量，如`padding: .5em`，在每种字体大小下看起来都是正确的。

将`em`用于间距属性。

#### 格子

[CSS 网格](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)是[非常很支持](https://caniuse.com/#search=grid)(回 IE10！)并允许在二维空间中排列内容，而无需添加容器元素，如 Bootstrap 的`row`或`col`网格元素。设计师经常在 12 列网格中工作，CSS 框架倾向于效仿，但是网格，像所有的间距一样，应该补充拷贝，而不是限制它。网格应该是临时编写的，而不是没有上下文的预定格式。不要用“网格框架”来膨胀你的 CSS

#### 文本对齐

`text-align`常用来对齐文字以外的东西。这不是这项工作的合适工具。使用 flexbox 进行这种对齐。使用值`left` 和`right`并不总是适用于从右到左或垂直的语言(一些浏览器将这些值映射到流相关的`start`和`end`，但不是全部)。在文本上使用值`justify`会导致一些有符号的语言出现问题，而[会给有阅读障碍的人带来问题](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align#Accessibility_concerns)。flexbox 可以更好地解决`text-align`的每一个用例，所以请使用 flexbox。一直都是。

#### 概述

聚焦元素上的轮廓是浏览器传达哪个元素正在接收输入的方式。默认轮廓通常足够突出，对每个用户都有用，包括那些需要高对比度的用户。默认大纲通常会被覆盖(或删除)，因为它不适合网站的设计。除非你用其他突出的和可访问的焦点指示器*替换*聚焦的`outline`样式，否则**不要删除或取消轮廓属性**。

#### 聚焦和悬停

如上所述，改变`:focus`样式时要小心，因为它充当了当前哪个元素正在接收输入的指示器。向`:hover`上的元素添加样式通常是一种不错的方式，但是不要使用伪选择器来显示额外的副本，除非你对`:focus`做了同样的事情(当然，如果元素是*可聚焦的*)。对同一个规则集同时使用`:hover`和`:focus`伪选择器通常是个好主意，但并不总是这样。(将`:focus`选择器添加到按钮的悬停样式会导致被按下的按钮看起来“卡”在上面。)

#### 不透明

将元素的`opacity`设置为`0`实际上并不会对辅助工具隐藏它。该元素仍然在文档流中占据空间，并且它的副本仍然被屏幕阅读器读取。仅有的两个合理调用`opacity`属性的用例是将一个元素转换到视图中(从`0`快速转换到`1`)和样式化一个对话框覆盖(因此下面的内容多少是可见的)。当心“堆积”的半透明覆盖物。不透明度是倍增的，因此两个分别带有`opacity: 50%`的覆盖图下面的内容显示为好像在带有`opacity: 25%`的单个元素下面。

#### 选择器

坚持使用类和类选择器。使用 id、type 和 universal 选择器会带来一些麻烦。在 CSS 特异性中，id 选择器总是胜过任何其他选择器，但是`[id](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)` [属性应该是唯一的](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)(每页)，所以它们对于应用可重用样式没有用。

现代浏览器中的选择器性能是一个可以忽略的问题，所以尽管你可能听说过通用选择器(`*`)不是高性能的，但我真正关心的是它的使用对于几乎所有用例来说都太通用了。使用类似于`.my-class >`的选择器；*最终会导致选择掉一些子元素，所以您也可以将类添加到您希望设计样式的元素中，并直接将它们作为目标。

对于不使用类型选择器也有类似的说法，比如`div`、`main`。他们倾向于匹配太多的元素，通常需要更多有用的细节，比如`div.some-class`。像这样的复合选择器比单一的类选择器具有更高的专一性，这是一个产生 bug 的问题，下面将会讨论。

坚持使用类(`.class`)、属性(`[attribute]`)和伪类(`:focus`)选择器。它们都具有相同水平的特异性。

#### 特征

与过于一般化的选择器(比如使用`*`)相对的是过于具体化的选择器。这两种情况都会产生问题。过于特定的选择器会滋生更多特定的选择器或可怕的`!important`声明。当进行样式更改时，每个连续的选择器都成为需要克服的新障碍，沿着这条路走下去会导致我们都害怕使用的不断增长的脆弱样式表。

CSS 有一个自然增加的特殊性——规则集的顺序。这是级联样式表中[级联的一部分。考虑到这一点，我们可以按照“重要性”的升序来编写规则集，而不会增加选择器的特异性级别。例如:](https://medium.com/@ntgard/cascades-in-css-e79f8c0f4df2)

```
.btn {  color: black;}.btn--primary {  color: green;}.btn--primary--light {  color: white;}
```

在这个例子中，每个单个类选择器都比它的前身更加具体，消除了为`.btn.btn--primary`或`.btn.btn--primary--light`声明规则集的需要。

解决办法是尽可能坚持使用单个类选择器，按照“重要性”递增的顺序编写，并避免使用`!important`声明。

#### 文本转换

对于支持英语以外语言的网站，使用`text-transform`可能会导致问题。[在几种情况下，浏览器会将一个字符替换为不正确的大小写转换版本](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform#Browser_compatibility)。解决办法是永远不要使用`text-transform`,而是依靠一个准确大写的副本。

#### z 指数

如果任何一个`z-index`规则包含在一个样式表中，那么最终会有另外两个规则声明`z-index: 9999;`和`z-index: 99999;`。试图使用原子类或变量来限制可接受的 z 索引的数量不仅无法阻止开发人员使用`calc()`和 SCSS 数学来修改他们用例的值，而且会因为堆栈上下文的工作方式而完全错过目标。

根据我的经验，大多数(如果不是全部的话)对`z-index`的使用可以通过重新构造 HTML 来使用自然堆叠上下文(页面上较低的元素在上下文中较高)或者通过[向元素或其父元素添加属性来强制一个新的堆叠上下文](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)来代替。

不惜一切代价避免`z-index`。

#### 伪元素

使用伪元素`::before`和`::after`不仅有用，而且通常很有趣！许多风格技巧依赖于这两个伪元素的使用，只要它们中没有副本(通过它们的`content`属性)，它们就被认为是语义的。将副本放入这些元素的问题是，它们是否被可访问性设备读取因浏览器和设备而异。最好不要通过避免在其中放置副本来处理这种差异。

伪元素`::first-letter`和`::first-line`并不像您可能认为的那样工作。它们只针对块级元素中的第一个字母/行。还有一个问题是`::first-line`选择器错误地将双字节字符(如日语假名)和[双字母](https://developer.mozilla.org/en-US/docs/Web/CSS/::first-letter#Browser_compatibility)作为目标。

分别通过`::selection`和`::placeholder`操作所选文本或占位符文本的样式通常会带来麻烦。对于`::placeholder`，问题很简单:[你不应该使用占位符](https://www.nngroup.com/articles/form-design-placeholders/)。对于任何重要的东西，比如输入标签或提示，尤其如此。通过包含`::placeholder`样式，你鼓励开发者、设计者和作者使用它们，这让你的用户很沮丧。

修改选择样式，通常是`color`和`background-color`，会导致更加微妙但阴险的错误。虽然[默认选择颜色](https://stackoverflow.com/a/16094931)在不同的浏览器和设备上并不一致，并且它们并不总是提供与你的网站文本颜色的可接受的对比，但用户有时会出于可访问性的原因而覆盖它们。在这种情况下，改变颜色要么不起作用(因为用户的可访问性 CSS 胜过你的)，要么会干扰他们的样式表(如果你使用`!important`)。使用这种伪元素来试图保证一个可访问的对比，最终可能会破坏你希望帮助的人的体验。

(虽然我已经忘记了这个 bug 的很多细节，但几年前我遇到了一个问题，Chrome 的自动翻译文本变得不可见，因为它依赖于我修改过的`::selection`样式。)

#### 过渡和动画

除了`opacity`和`transform`之外的属性的转换或动画会导致浏览器重新绘制或重排页面。在高端开发人员的机器上，这似乎不是一个问题，但在低端笔记本电脑和手机上，这将导致口吃。烂动画比没动画还烂。

#### 喜欢减少运动

编写有用的、漂亮的、安全的*动画不是一件简单的事情。随着媒体查询的出现，我们可以让我们的页面对有前庭障碍的人来说更安全，对我们其他人来说不那么烦人。虽然添加这个媒体查询不是灵丹妙药，但它很有帮助。我已经编写了嵌套规则来选择退出，这意味着所有的**CSS 动画都会停止，除非作者在元素中包含了类`safe-animation`。***

```
/* https://github.com/mozdevs/cssremedy/issues/11#issuecomment-462867630 */@media (prefers-reduced-motion: reduce) {  *:not(.safe-animation),  *:not(.safe-animation)::before,  *:not(.safe-animation)::after {    animation-duration: 0.01s !important;    animation-iteration-count: 1 !important;    transition-duration: 0s !important;    scroll-behavior: auto !important;  }}
```

#### 重置扩展

我要去的 CSS 复位是一个修改形式的[迈耶斯复位](https://meyerweb.com/eric/tools/css/reset/)。不过，我从重置中删除了一些规则。我不喜欢从`ol`和`ul`元素中移除列表图标。我发现这样做会鼓励开发人员以非语义的方式使用这些元素，比如将物理上接近但本体上不接近的项目分组。我还删除了将`body`上的`line-height`设置为`1`的规则。设置影响字体*或被字体*影响的属性和设置字体*是一个迟早会发生的错误。*

下面是我对重置文件的一些补充。我不喜欢在我的 CSS 中包含一个`.hidden`原子类，因为有一个更好的选项，即使 CSS 不加载也能工作——`hidden`属性。在隐藏元素上设置`display: none`的默认浏览器行为可能会被覆盖，甚至是意外地，所以我加入了一个规则来强制执行。

```
body {  /* more intuitive sizing */  box-sizing: border-box;}*, ::before, ::after {  box-sizing: inherit;}i, cite, em, var, dfn, address {  /* prevent faux italic */  font-style: normal;}b, h1, h2, h3, h4, h5, h6, strong, th {  /* prevent faux bold */  font-weight: normal;}[hidden] {  /* enforce accessible semantics */  display: none !important;}
```

> 我的重置:[https://github.com/NickGard/css-utils/blob/master/reset.css](https://github.com/NickGard/css-utils/blob/master/reset.css)

我经常发现的另一个必要的实用程序是一个`visually-hidden`类。虽然我更多地使用`aria-label`来处理不可见的屏幕可读文本，但我通常会在某处包含以下规则:

```
/* https://a11yproject.com/posts/how-to-hide-content/ */.visually-hidden {  position: absolute !important;  height: 1px;  width: 1px;  overflow: hidden;  clip: rect(1px, 1px, 1px, 1px);}
```

#### 模糊命名

在结束这篇文章之前，我至少要对命名约定发表一点意见。我喜欢 BEM 命名，因为它读起来很好。`<button class="btn--primary"` / >告诉我到底是哪种按钮。我与官方 BEM 方法论的一个不同之处是，我喜欢在一个元素上使用一个类(原子类可能是个例外)。这冒犯了我的感受，因为第二个类已经告诉我样式是从基本 btn 规则集扩展而来的。这也为李做出改变创造了两个理由，这是一个危险信号。

在我的 CSS 中，这看起来像这样:

```
.btn, .btn--primary {  /* base button styles */}.btn--primary {  /* primary button overrides */  /* has naturally higher specificity */}
```

在 SCSS，你可以用`@extend`达到同样的效果。

#### 结论

这些是我几年来的经验法则，帮助我维护有许多贡献者的大型代码库。它并不完美，我一直在调整它(`prefers-reduced-motion`是新的)，但我希望通过分享它，它将帮助他人。