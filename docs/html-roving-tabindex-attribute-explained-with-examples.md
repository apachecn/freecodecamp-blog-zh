# HTML 漫游 tabindex 属性用例子解释

> 原文：<https://www.freecodecamp.org/news/html-roving-tabindex-attribute-explained-with-examples/>

你用过 CSS `order`或者`direction`属性吗？您可能已经使用它们很多次了，但是您是否意识到这些属性会导致 DOM 中显示的内容和实际内容之间的脱节？

> 使用 order 属性将会在内容的可视表示和 DOM 顺序之间造成一种脱节。–MDN 文档

当我重新创建“Twitter 定制主题 UI”时，当我使用`dir="rtl`改变一个元素的方向时，我遇到了这个奇怪的不一致。

这是因为元素只是在视觉上改变了方向，而 HTML 保持不变。这导致我的键盘导航是从后面开始的。

[https://www.youtube.com/embed/sAC387rF7aA?feature=oembed](https://www.youtube.com/embed/sAC387rF7aA?feature=oembed)

我们可以用`tabindex`修复这个奇怪的断开。但是使用除了`0`之外的值来使用`tabindex`是非常不鼓励的，并且被许多开发人员(包括我)所反对。

所以我们将使用一种叫做**漫游 tabindex** 的技术来解决这个问题。但是在我们研究这项技术之前，让我们先回顾一下`tabindex` HTML 属性是做什么的。

### HTML Tabindex 属性

您使用 HTML `tabindex`属性来设置元素的制表符位置，它通常表示元素可以使用`Tab`键进行制表符处理。

`tabindex`只接受整数作为从`0`到`32767`的值。如果不指定值，它将采用默认值`0`。

`tabindex="0"`将把任何元素按自然 tab 键顺序排列:

`<span tabindex="0"> Now I'm in the natural tab order <span>`

`tabindex="-1"`将从自然 tab 顺序中删除一个元素:

`<button tabindex="0"> I'm no longer part of the natural tab order </button>`

大于`0`的`tabindex`会将一个元素移动到自然 tab 键顺序的前面。

如果你不熟悉`tabindex`属性，你可以在这里阅读更多关于它的信息[。](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)

## 什么是流动 Tabindex？

现在我们已经看了`tabindex`，什么是游动 tabindex 技术？

漫游 tabindex 基本上为元素的所有子元素设置`tabindex`到`-1`，除了当前被聚焦的元素。

```
<div class"btns js-btns"> 
    <button tabindex="0"> currently focused </button> 
    <button tabindex="-1"> next button </button> 
    ... 
</div>
```

然后使用一个`EventListener`我们可以确定哪个按钮当前有焦点。当事件触发时，将之前获得焦点的按钮`tabindex`设置为`-1`，然后将下一个孩子的(要获得焦点的)`tabindex`设置为`0`，并在其上调用`focus()`方法。重复这样做，直到你到达最后一个。

这里有一个更详细的关于 [**游动 tabindex**](https://developers.google.com/web/fundamentals/accessibility/focus/using-tabindex) [**方法**](https://developers.google.com/web/fundamentals/accessibility/focus/using-tabindex) **的指导。**

### 如何使用漫游选项卡索引

让我们将这个方法应用于在 HTML 中使用`dir="rtl"`属性时得到的奇怪的断开行为。

我们使用`dir="rtl"`属性(可视化地)反转下面 HTML 代码的`order`(相当于在 CSS 中使用`direction`属性)。

如果你不确定或者不熟悉 HTML 中的`dir`属性，你可以[在这里](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/dir)了解更多。

```
<div class="btns js-btns" dir="rtl"> 
    <button="btn js-btn" aria-label="button name"> button </button> 
    <button="btn js-btn" aria-label="button name"> button </button> 
    <button="btn js-btn" aria-label="button name"> button </button> 
    <button="btn js-btn" aria-label="button name"> button </button> 
    <button="btn js-btn" aria-label="button name"> button </button> 
</div>
```

首先，我们将`tabindex`属性添加到所有按钮，并将其值设置为`-1`:

```
let btns = document.querySelectorAll("button"); 
btns.forEach((btn, index) => { 
// set first button tabindex to 0 
// and set every other button tabindex to -1 
if (index == btns.length - 1) { 
btn.setAttribute("tabindex", 0); 
} else { 
btn.setAttribute("tabindex", -1); 
}
```

注意我们是如何先将最后一项`tabindex`设置为`0`(`if (index == btns.length - 1)`)。视觉上，这是用户看到的第一个元素，因为我们在 HTML 上设置了`dir="rtl"`属性:

```
<div class="btns js-btns" dir="rtl"> 
    //buttons 
</div>
```

然后我们添加一个`EventListener`，在这里我们将当前焦点按钮的`tabindex`设置为`-1`。我们不断重复设置下一个孩子的`tabindex`为`0`，直到它到达最后一个。然后它将焦点移回第一个，反之亦然。

```
// add an event listener when tab key is pressed 
btn.addEventListener("keydown", (e) => { 
if (e.keyCode == 9) { 
// prevent the default behaviour 
e.preventDefault(); 
// set current button tabindex to 0 
btn.setAttribute("tabindex", -1); 
// if not last button keep setting tabindex to 0 
if (btn.previousElementSibling != null) { 
let nextEl = btn.previousElementSibling; 
nextEl.setAttribute("tabindex", 0); 
nextEl.focus(); 
} else { 
// when we get to last element set first element to tabindex 0 
// and call focus method on it. 
// note the .lastElementChild the last element becomes our first 
// that's because of the direction we changed 
let firstEl = document.querySelector(".js-btns").lastElementChild; firstEl.setAttribute("tabindex", 0); firstEl.focus(); 
} 
} 
});
});
```

经过这一切，代码现在看起来像 this:‌

```
let btns = document.querySelectorAll("button"); 
btns.forEach((btn, index) => { 
// set first button tabindex to 0 
// and set every other button tabindex to -1 
if (index == btns.length - 1) { 
btn.setAttribute("tabindex", 0); 
} else { 
btn.setAttribute("tabindex", -1); 
} 
// add an event listener when tab key is pressed 
btn.addEventListener("keydown", (e) => { 
if (e.keyCode == 9) { 
// prevent the default behaviour 
e.preventDefault(); 
// set current button tabindex to 0 
btn.setAttribute("tabindex", -1); 
// if not last button keep setting tabindex to 0 
if (btn.previousElementSibling != null) { 
let nextEl = btn.previousElementSibling; 
nextEl.setAttribute("tabindex", 0); 
nextEl.focus(); 
} else { 
// when we get to last element set first element to tabindex 0 
// and call focus method on it. 
// note the .lastElementChild the last element becomes our first 
// that's because of the direction we changed 
let firstEl = document.querySelector(".js-btns").lastElementChild; firstEl.setAttribute("tabindex", 0); 
firstEl.focus(); 
} 
} 
});
}); 
```

这是 Codepen 上的一个工作版本(试着用你的键盘导航)。你也可以点击主题按钮，在 Spruce.com.ng[观看完整的 Twitter 定制 UI 的现场演示。](https://spruce.com.ng)

[https://codepen.io/Spruce_khalifa/embed/preview/BaQemGw?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaQemGw](https://codepen.io/Spruce_khalifa/embed/preview/BaQemGw?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaQemGw)

‌

但是等一下——在你决定使用这种技术之前，记住几乎每一个设计实现都是有代价的。因此，让我们来看看使用流动 tabindex 技术的利弊:

### 漫游 tabindex:‌的好处

1.  改善键盘导航体验问题
2.  修复了由方向 change‌导致的顺序断开问题

### 流动 tabindex 的缺点:

1.  完全依赖于 JavaScript——如果用户出于某种原因关闭了 JavaScript，键盘导航会再次产生奇怪的体验。
2.  不支持 users‌辅助技术

## 结论

没有特定或优选的方式来实现漫游 tabindex 技术。因此，无论您决定如何编写和实现您的代码，只要您遵循以下步骤，都是非常好的:

1.  将除第一个元素之外的所有元素的`tabindex`设置为`-1`
2.  添加一个键盘事件侦听器来确定哪个元素被聚焦
3.  将先前聚焦的子节点`tabindex`设置为`-1`
4.  然后将下一个孩子`tabindex`设置为`0`
5.  在它上面调用`focus()`方法

如果你觉得这个教程有用，请在 twitter 上关注我。