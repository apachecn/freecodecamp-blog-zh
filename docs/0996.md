# 如何在 CSS 中使用 Isolation 属性创建新的堆栈上下文

> 原文：<https://www.freecodecamp.org/news/the-css-isolation-property/>

## CSS 隔离属性是什么？

在 CSS 中，可以使用`isolation`属性创建一个新的堆栈上下文。看起来是这样的:

```
.container-for-new-stacking-context {
  isolation: isolate;
}
```

`isolation`的缺省值是`auto`，这稍微有点微妙，因为可以创建堆栈上下文*——但是这取决于元素的属性以及它们是否需要它。*

*您也可以将该值设置为`inherit`、`initial`、`revert`或`unset`。*

*使用`isolation: isolate;`是创建一个新的堆栈上下文的决定性方法。*

## *什么是堆叠上下文？*

*![doll-g9145bb1e2_1280](img/fcbe4336124b2296addd12cd1759e0a8.png)*

*在 CSS 中，堆叠上下文允许 HTML 元素根据提供上下文的基本元素的起始位置进行堆叠。*

*元素沿着具有 x 轴和 y 轴的虚拟矩阵放置。还有一个 z 轴，其中元素可以前后放置。属性通常用于沿 z 轴放置元素。*

*请记住，当呈现根 HTML 元素时，它伴随着根/全局堆栈上下文。*

*有许多方法可以在全局堆叠上下文中创建堆叠上下文。一种常见的方法是将`position: relative`与`z-index`一起使用。*

*使用`position: sticky`或`fixed`是可行的，但是会将元素从流布局中取出，并且需要额外的属性来放置。*

*也可以使用`transform`、`clip-path`或`filter`来方便堆叠。要查看堆栈上下文的所有形成方式，请阅读更多关于 [MDN Web 文档](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)的信息。*

*堆叠上下文可以包含后续的内部堆叠上下文，并继续进一步嵌套。这可能有点太像概念化的 *Inception* 了，所以让我们来看看为什么这是有用的。*

## *Z 指数黑洞*

*![wormhole-g9c2a60580_1280](img/e00d5b42c92e2015a21fd3006b7cb314.png)*

*使用`z-index`可能很难维护。你必须非常小心你在哪里使用它，以及你提供给它什么样的价值。*

*设计系统可以帮助解决与此相关的问题。创建一组可重用的值并记录在什么情况下应该使用它们会非常有帮助。例如，将最大的变量值留给模态和其他项目，它们总是会占据整个页面。*

*但大多数时候，我们真的只是想让我们的风格以我们想要的方式出现。这可能意味着规定任意的`z-index`值，并继续提高这些值，直到它们起作用。*

*我遇到过很多次臭名昭著的`z-index: 999999;`。追踪这些随机值并创建新的秩序可能会变得很困难。这可能会导致难以调试的问题。*

*你开始使用的数字越大，你就会越来越深地进入黑洞，以后就越难再出来。*

*![zindex](img/05a5a307bcd0f2061f0737612c85ce0f.png)*

### *用隔离来保持简单*

*将 isolation 属性设置为 isolate 是一个简单的一行程序，它可以创建一个新的堆栈上下文，而无需使用`z-index`将元素放置在彼此的前面。*

*您可以在静态定位的元素上使用隔离，它不会影响子元素。这是创建一个包含子元素的独立基础的好方法。隔离属性也得到广泛的支持。*

*![isolation-3](img/150c30fd17f03e278671057d8d95ced0.png)*

*提醒一下，下面是它的语法:*

```
*`.container-for-new-stacking-context {
  isolation: isolate;
}`*
```

## *总而言之:*

*设置`isolation: isolate;`将在应用到顶级元素时为子元素创建新的堆栈上下文。*

*如果你觉得这篇文章有帮助，请随时联系 Twitter [@ui_natalie](https://twitter.com/ui_natalie) 。快乐堆叠！*

### *资源*

*   *[MDN 网络文档-隔离](https://developer.mozilla.org/en-US/docs/Web/CSS/isolation)*
*   *[MDN Web Docs -堆叠上下文](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)*
*   *什么鬼东西，z-index？？*