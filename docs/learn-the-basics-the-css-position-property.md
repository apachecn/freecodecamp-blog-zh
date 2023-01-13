# 用例子解释 CSS 位置属性

> 原文：<https://www.freecodecamp.org/news/learn-the-basics-the-css-position-property/>

在你真正擅长 CSS 之前，你需要了解一些基础知识。你必须了解 CSS 属性和它们的值。

在本文中，我们将关注 CSS 位置属性。我们将学习 CSS 位置属性的各种值以及它们是如何工作的。我们开始吧！

## CSS 位置属性是什么？

CSS position 属性定义了元素在文档中的位置。该属性与 left、right、top、bottom 和 z-index 属性一起确定元素在页面上的最终位置。

位置属性可以有五个值。它们是:

1.  静电
2.  亲戚
3.  绝对的
4.  固定的；不变的
5.  粘的

让我们逐一讨论。

### 静态

这是元素的默认值。该元素根据文档的正常流向进行定位。左、右、上、下和 z-index 属性不影响带有`position: static`的元素。

让我们用一个例子来说明`position: static`对一个元素的位置没有影响。我们在一个父容器中放置了三个 div。我们将在整篇文章中使用这个例子。

```
<html>
	<body>
        <div class="parent-element">
            <div class="sibling-element">
                I'm the other sibling element.
            </div>

            <div class="main-element">
                All eyes on me. I am the main element.
            </div>

            <div class="sibling-element">
                I'm the other sibling element.
            </div>
        </div>
    </body>
<html>
```

让我们将`position: static`添加到带有类`main-element`的 div 中，并为其添加左顶值。我们还向其他 div 添加了一些样式，以区别于焦点中的元素。

```
.main-element {
    position: static;
    left: 10px;
    bottom: 10px;
    background-color: yellow;
    padding: 10px;
}

.sibling-element {
    padding: 10px;
    background-color: #f2f2f2;
}
```

这就是结果。

[//jsfiddle.net/sarahchima/4vsm3o1d/5/embedded/result,css/](//jsfiddle.net/sarahchima/4vsm3o1d/5/embedded/result,css/)

你注意到没有变化吗？这证实了 left 和 bottom 属性不影响带有`position: static`的元素。

让我们转到下一个值。

### 亲戚

带有`position: relative`的元素保留在文档的正常流程中。但是，与静态元素不同，left、right、top、bottom 和 z-index 属性会影响元素的位置。基于 left、right、top 和 bottom 属性的值的偏移量被应用于相对于自身的元素。

让我们在例子中用`position: relative`代替`position: static`。

```
.main-element {
    position: relative;
    left: 10px;
    bottom: 10px;
    background-color: yellow;
    padding: 10px;
}
```

[//jsfiddle.net/sarahchima/78tbavnu/14/embedded/result,css/](//jsfiddle.net/sarahchima/78tbavnu/14/embedded/result,css/)

请注意，left 和 bottom 属性现在会影响元素的位置。另请注意，元素保留在文档的正常流程中，并且相对于自身应用偏移。当我们转到其他值时，请注意这一点。

### 绝对的

带有`position: absolute`的元素相对于它们的父元素定位。在这种情况下，该元素将从普通文档流中删除。其他元素的行为就好像该元素不在文档中一样。页面布局中不会为元素创建任何空间。left、top、bottom 和 right 的值决定了元素的最终位置。

需要注意的一点是，带有`position: absolute`的元素是相对于其最近的祖先定位的。这意味着父元素必须有一个不同于`position: static`的位置值。

如果没有定位最近的父元素，则相对于定位的下一个父元素进行定位。如果没有定位的祖先元素，它相对于`<html>`元素定位。

让我们回到我们的例子。在这种情况下，我们将主元素的位置更改为`position: absolute`。我们还会给它的父元素一个相对位置，这样它就不会相对于`<html>`元素被定位。

```
.main-element {
    position: absolute;
    left: 10px;
    bottom: 10px;
    background-color: yellow;
    padding: 10px;
}

.parent-element {
    position: relative;
    height: 100px;
    padding: 10px;
    background-color: #81adc8;
}

.sibling-element {
	background: #f2f2f2;
	padding: 10px;
    border: 1px solid #81adc8;
} 
```

这是结果。

[//jsfiddle.net/sarahchima/5qyp4m2z/5/embedded/result,css/](//jsfiddle.net/sarahchima/5qyp4m2z/5/embedded/result,css/)

请注意，文档中没有为元素创建空格。该元素现在相对于父元素定位。当我们进入下一个值时，请注意这一点。

### 固定的；不变的

固定位置元素类似于绝对定位元素。它们也被从文档的正常流程中删除。但是与绝对定位的元素不同，它们总是相对于`<html>`元素定位。

需要注意的一点是，固定元素不受滚动的影响。它们总是停留在屏幕上的同一个位置。

```
.main-element {
    position: fixed;
    bottom: 10px;
    left: 10px;
    background-color: yellow;
    padding: 10px;
}

html {
    height: 800px;
}
```

[//jsfiddle.net/sarahchima/L4gsxwft/2/embedded/result,css/](//jsfiddle.net/sarahchima/L4gsxwft/2/embedded/result,css/)

在这种情况下，元素相对于`<html>`元素定位。尝试滚动以查看元素是否固定在屏幕上。

让我们来看看最终值。

### 粘的

`position: sticky`是`position: relative`和`position: fixed`的混合。它就像一个相对定位的元素，直到某个滚动点，然后它就像一个固定的元素。如果你不明白这是什么意思，不要害怕，这个例子会帮助你更好地理解它。

```
.main-element {
    position: sticky;
    top: 10px;
    background-color: yellow;
    padding: 10px;
}

.parent-element {
    position: relative;
    height: 800px;
    padding: 50px 10px;
    background-color: #81adc8;
}
```

[//jsfiddle.net/sarahchima/f3m0qxgn/8/embedded/result,css/](//jsfiddle.net/sarahchima/f3m0qxgn/8/embedded/result,css/)

滚动结果选项卡查看结果。你看到它作为一个相对元素，直到它到达屏幕上的某个点，然后它就像一个固定元素一样到达那里。

## 摘要

哎呦！那是一段相当长的旅程。但是我真的希望这篇文章能帮助你理解 CSS 位置属性以及它是如何工作的。

如果你不完全理解其中的任何一个，请随意摆弄小提琴。您可以更改这些值并注意到差异，或者更好的是，尝试使用 position 属性来处理个人项目。

记住，更好地掌握 CSS 需要不断的练习。所以坚持练习，你会变得更好。

**想在我发表新文章时得到通知吗？[点击这里](https://mailchi.mp/69ea601a3f64/join-sarahs-mailing-list)。**