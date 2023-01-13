# CSS 字体大小教程-如何改变 HTML 中的文本大小

> 原文：<https://www.freecodecamp.org/news/css-font-size-tutorial-how-to-change-text-size-in-html/>

使用 CSS **`font-size`** 属性来决定文本的大小。

```
.container {
    font-size: 33px;
} 
```

该属性采用几种类型的值:

*   关键词(绝对大小和相对大小的关键词)，
*   长度(如像素(px)和 em 单位)，以及
*   百分比。

```
.container {
    font-size: xx-large;
} 
```

问题是:你应该选择哪种类型的价值，为什么？

这就是本文要解决的问题。它将向您展示如何使用`font-size`属性以及它的许多值之间的差异。所以下一次你需要改变你的文本的字体大小时，你就知道该用哪个值了。

## 关键词值

字体大小有两种关键字值:`absolute-size`和`relative-size`关键字。先说绝对。

### 绝对大小的关键字

下面的代码使用绝对大小关键字 **`small`** 来调整 HTML 文本的大小。

```
.childElement {
    font-size: small;
} 
```

有更多绝对大小的关键字选项可供选择:

*   xx-小型
*   x-small
*   小的
*   媒介
*   大的
*   超大号
*   xx-大
*   XXX-大号

绝对大小关键字的值由浏览器的默认字体大小决定。通常，大小是中等。

## 相对大小的关键字

相对大小的关键字是另一个要考虑的关键字选项。你有两个选择: **`smaller`** 和 **`larger`** 。

```
.parentElement {
    font-size: smaller;
} 
```

带有 relative-size 关键字的元素的字体大小是相对于其父元素字体大小的*相对值*-更大或更小。

换句话说，父元素的字体大小影响子元素的字体大小，如下所示。

[https://codepen.io/amymhaddad/embed/preview/eYZLNGN?height=300&slug-hash=eYZLNGN&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/eYZLNGN?height=300&slug-hash=eYZLNGN&default-tabs=html,result&host=https://codepen.io)

在本例中，相对大小关键字 **`smaller`** 应用于子元素，绝对大小值 **`large`** 应用于父元素。

# 长度值

`font-size`接受几个不同的长度值。我们将探讨其中的三种:像素(px)和 em 和 rem 单位。不管您如何选择，您使用的长度值必须是正数。

```
.parentElement {
    font-size: 60px;
} 
```

### 像素

像素是一个[绝对长度单位](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)。这意味着，它们是*而不是*受其他元素的影响，如父元素或窗口大小。

因此，像素是精确的:您定义了一个元素上所需的像素的精确数量，这通常就是您所得到的。但是，不同浏览器之间可能会略有不同。

[https://codepen.io/amymhaddad/embed/preview/KKzRbMb?height=300&slug-hash=KKzRbMb&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/KKzRbMb?height=300&slug-hash=KKzRbMb&default-tabs=html,result&host=https://codepen.io)

注意，在上面的代码示例中，子元素使用了`pixels`并具有相同的字体大小。

## 邮政快递

虽然你可以把像素看作是固定的，但是把 em 值看作是可变的。

那是因为 em 值是一个[相对长度单位](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)。使用 em 值的元素的字体大小是相对于其父元素字体大小的*。*

然而，假设您还没有为父元素设置字体大小。你也没有在更高层的地方设置一个。在这种情况下，em 值是使用浏览器默认的(通常为 16px)来计算的。

让我们把这个想法具体化。

假设父元素设置为 30px，子元素设置为 2em。然后，子元素的呈现字体大小为 60px (2 x 30px = 60px)。您可以在下面的代码中看到这个场景。

[https://codepen.io/amymhaddad/embed/preview/zYqJrOJ?height=300&slug-hash=zYqJrOJ&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/zYqJrOJ?height=300&slug-hash=zYqJrOJ&default-tabs=html,result&host=https://codepen.io)

由于 em 值的复合效应，它们可能会有问题，这在下面的示例中进行了演示。

[https://codepen.io/amymhaddad/embed/preview/LYNBjOp?height=300&slug-hash=LYNBjOp&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/LYNBjOp?height=300&slug-hash=LYNBjOp&default-tabs=html,result&host=https://codepen.io)

当有多个使用 em 值的元素相互嵌套时，字体大小值会复合:它们会变得越来越大。这就是复合效应在起作用。

## REMs

输入 rem 值，这是为了处理 ems 的复利问题而创建的。

回想一下，em 值是相对于父元素的。相反，rem 值与根(html)元素的字体大小相关。

这意味着您可以将 rem 值应用到一个元素，并且它不会受到父元素字体大小的影响。这避免了我们上面经历的复合效应。

这个例子使用了带有 rem 值的`font-size`属性。

[https://codepen.io/amymhaddad/embed/preview/JjXByvd?height=300&slug-hash=JjXByvd&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/JjXByvd?height=300&slug-hash=JjXByvd&default-tabs=html,result&host=https://codepen.io)

上面的例子有一个关键点:父元素的字体大小不会影响子元素的字体大小。

# 百分率

百分比提供了另一种方式来设置字体大小*相对于父元素的字体大小。*

带有百分比的元素引用其父元素来确定其字体大小。百分比值必须是正数。

这里有一个例子。

[https://codepen.io/amymhaddad/embed/preview/mdPjMjw?height=300&slug-hash=mdPjMjw&default-tabs=html,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/mdPjMjw?height=300&slug-hash=mdPjMjw&default-tabs=html,result&host=https://codepen.io)

正如你所看到的，谈到字体大小，你有很多选择来满足你的需要。

****我在【amymhaddad.com】的[上写了关于学习编程以及如何去做的最好方法。](http://amymhaddad.com/)** 我*也*关于编程、学习、生产力的推文: **[@amymhaddad](https://twitter.com/amymhaddad) 。****