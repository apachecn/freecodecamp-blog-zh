# z-index 属性的工作原理

> 原文：<https://www.freecodecamp.org/news/how-the-z-index-property-works-under-the-hood/>

z-index 属性可能看起来很简单，但它不仅仅是使用正整数或负整数。

在引擎盖下还有更多的东西，当你使用它时，一些怪癖会引起问题。

在本文中，我们将深入 z-index 属性并了解它的实际工作原理。

所以，拿起一杯咖啡或茶，准备好纸和笔😉

z-index 控制堆叠顺序并创建堆叠上下文。首先让我们了解这些术语的含义。

## 堆叠顺序

当浏览器沿着 z 轴呈现网页元素时，它必须选择首先在画布上绘制哪个元素。

这些元素的顺序叫做堆叠顺序。

在我们进一步讨论 z-index 属性的堆叠顺序之前，让我们先了解一下默认堆叠是什么样子的:

1.  根元素的背景和边框。
2.  未定位的块元素(按出现的顺序)
3.  非定位浮动元素(按外观顺序)
4.  内嵌元素(按外观顺序)
5.  已定位的元素(按外观顺序)

根元素背景和边框表示堆栈的最低层。定位的元素表示堆栈的最高级别。

> 注意:定位元素是相对的、固定的、绝对的或粘性的元素——除了静态的任何元素。

[https://codepen.io/ali6nx404/embed/gOGdJEY?default-tab=result&theme-id=dark](https://codepen.io/ali6nx404/embed/gOGdJEY?default-tab=result&theme-id=dark)

See the Pen [Untitled](https://codepen.io/ali6nx404/pen/gOGdJEY) by Mahesh Patidar ([@ali6nx404](https://codepen.io/ali6nx404)) on [CodePen](https://codepen.io).

非定位元素位于行内元素和定位元素之下。如果我们更改代码，并在第二个 div 中添加相对位置，那么它将隐藏内联元素。

## z-index 属性的工作原理

z-index 属性用于更改元素的默认堆叠顺序。

z 索引可以取 3 个值中的一个:

1.  **auto** :该元素的堆栈顺序与其父元素相同。这是默认值。
2.  **整数**:可以是正数，也可以是负数。
3.  **继承**:将值设置为与父元素的值相同。

> z-index 仅适用于定位的元素。(嗯，这并不完全正确，因为也有一些边缘情况——但我们将在下面讨论这些情况。)

最高的正 z 索引值表示离用户最近的一个，最低的负 z 索引值表示离用户最远的一个。

[https://codepen.io/ali6nx404/embed/GRMXVwd?default-tab=result&theme-id=dark](https://codepen.io/ali6nx404/embed/GRMXVwd?default-tab=result&theme-id=dark)

See the Pen [Negative z-index](https://codepen.io/ali6nx404/pen/GRMXVwd) by Mahesh Patidar ([@ali6nx404](https://codepen.io/ali6nx404)) on [CodePen](https://codepen.io).

上面的 codepen 示例具有与默认堆叠顺序示例相同的代码，但是我只是在第 3 个元素代码中更改了一行。

`z-index: -1;`

我添加了一个负的 z 索引，所以它隐藏了非定位元素下面的定位元素。

当我们在定位的元素上指定 z 索引时，它会创建一个堆叠上下文。

随着堆叠上下文的引入，简单的 z-index 属性变得有点复杂。

## 堆叠上下文

在堆叠顺序中向前或向后移动的具有共同父元素的元素组构成了所谓的堆叠上下文。(官方定义来自[w3.org](https://drafts.csswg.org/css2/#propdef-z-index)。)

> 根元素(HTML)形成了根堆栈上下文。其他堆叠上下文由任何定位的元素(包括相对定位的元素)生成，该元素具有除“auto”之外的计算值“z-index”。堆叠上下文不一定与包含块相关。

希望这个定义有道理。

HTML 标签形成了根堆叠上下文，默认情况下，所有元素都属于这个堆叠上下文。

但是通过使用定位元素和 z 索引而不是“auto”值，我们可以创建一个本地堆栈上下文。该元素在本地堆栈上下文中被称为根元素。

父元素的子元素属于本地堆叠上下文，它们在堆叠顺序中一起向前和向后移动。

### 为什么我们需要了解堆叠环境

因此，如果您可以简单地通过给出正值或负值来使用 z-index，那么您为什么要学习所有这些东西呢？

让我给你一个简单的问题:

[https://codepen.io/ali6nx404/embed/gOGBovo?default-tab=result&theme-id=dark](https://codepen.io/ali6nx404/embed/gOGBovo?default-tab=result&theme-id=dark)

See the Pen [stacking context example](https://codepen.io/ali6nx404/pen/gOGBovo) by Mahesh Patidar ([@ali6nx404](https://codepen.io/ali6nx404)) on [CodePen](https://codepen.io).

我们有 3 个 div，它们的 z 索引值分别为 1、999 和 2。但是令人困惑的部分是第二个 div，它的 z-index 值为 999(最高值)——它仍然不能出现在第三个 div 之前。

这就是我们的理解派上用场的地方:这一切都是因为堆叠环境而发生的。

让我们找出原因。

魏看了于飞一眼:

```
 <main>
      <div class="first">1</div>
      <div class="second">999</div>
   </main>
   <div class="third">2</div> 
```

我们有三个 div，它们的类名分别是第一、第二和第三。第一个和第二个 div 被包装到主标记中。

这是 CSS:

```
.first {
  z-index: 1;
}

.second {
  z-index: 999;
}

.third {
  z-index: 2;
}
```

您可以看到第二个 div 具有最高的 z 索引值。但是，它不能出现在第三个 div 之前。

这一切都是因为这行代码而发生的👇

```
main {
  position: relative;
  z-index: 1;
} 
```

主要元素是创建一个本地堆栈上下文，第一个和第二个 div 属于同一个本地堆栈上下文，但是第三个 div 不属于。

这就是为什么第二个 div 能够出现在第一个之前:因为它的 z 索引值为 999(高于第一个 div)。

主标记和第三个 div 是根元素的子元素，都具有 z 索引值。

主元素的 z 索引值为 1，第三个元素的 z 索引值为 2。第三个具有较高的 z-index 值，这就是为什么第三个 div 能够出现在主标记之前。

我希望您现在理解了堆叠上下文是如何工作的，以及为什么您需要了解它。

## 如何创建堆叠上下文

我们已经看到了如何将定位(尤其是相对和绝对)与 z 索引相结合来创建堆叠上下文，但这不是唯一的方法！以下是其他一些例子:

1.  使用位置固定或粘性(这些值不需要 z 索引！)
2.  向 display: flex 或 display: grid 容器内的子容器添加 z 索引值
3.  添加小于 1 的不透明度值

这是一些常用的方法，但是你可以在这里查看完整的列表。

我之前告诉过你，z-index 实际上不仅仅适用于定位元素——有一些边缘情况。

z-index 属性适用于 grid 和 flex 容器子级，无需指定位置。

查看下面的示例和代码:

[https://codepen.io/ali6nx404/embed/JjrmvNd?default-tab=result&theme-id=dark](https://codepen.io/ali6nx404/embed/JjrmvNd?default-tab=result&theme-id=dark)

See the Pen [z-index with grid](https://codepen.io/ali6nx404/pen/JjrmvNd) by Mahesh Patidar ([@ali6nx404](https://codepen.io/ali6nx404)) on [CodePen](https://codepen.io).

这是因为向 grid 或 flex 容器中的子容器添加 z 索引值会形成一个堆叠上下文。

## 结论

我知道这些事情令人困惑，有点复杂。

我希望在阅读了这篇文章并使用 z-index 属性进行了实践之后，您能够理解事情是如何在幕后工作的。

但是如果你仍然困惑，你可以联系我[这里](https://twitter.com/Ali6nX404)。