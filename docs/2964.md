# CSS 框大小属性如何控制元素的大小

> 原文：<https://www.freecodecamp.org/news/how-to-use-the-css-box-sizing-property/>

CSS box-sizing 属性用于调整或控制任何接受`width`或`height`的元素的大小。它规定了如何计算该元素的总计`width`和`height`。

在本文中，我将解释如何使用 CSS `box-sizing`属性来控制元素的大小。

## 先决条件

*   基本的 CSS 知识。
*   代码编辑器。
*   网络浏览器。

## 如果没有 CSS 框大小属性

如果您查看下面的代码片段，您会注意到有两个`div`元素具有相同的`width`和`height`——但是第一个`div`看起来比第二个`div`大。

[https://codepen.io/edyasikpo/embed/preview/mdVoxrg?height=300&slug-hash=mdVoxrg&default-tabs=css,result&host=https://codepen.io](https://codepen.io/edyasikpo/embed/preview/mdVoxrg?height=300&slug-hash=mdVoxrg&default-tabs=css,result&host=https://codepen.io)

很奇怪，对吧？因为为什么有人会给两个`div`元素分配相同的宽度和高度，除非他们希望这些元素是相同的？

继续阅读，找出为什么两个`div`元素不一样？？‍?？？‍?。

默认情况下， [CSS 盒模型](https://kolosek.hashnode.dev/a-beginners-guide-to-box-model-in-css-ck6rnzojg03fidfs1g2kxl8ci)计算任何接受宽度或高度的元素，格式如下:

*   width + padding + border =元素框的呈现或显示宽度。
*   height + padding + border =元素框的呈现或显示高度。

这意味着每当你给元素添加`padding`或`border`时，元素的大小会比最初分配给它的大。这是因为该元素的内容包括`width`和`height`属性，但不包括`padding`或`border`属性。

还不明白吗？查看下面的代码片段，了解实际的计算。

```
.first-box {
  width: 200px;
  height: 100px;
  border: 8px solid blue;
  padding: 20px;
  background: yellow;
  /* Total width: 200px + (2 * 20px) + (2 * 8px) = 256px
     Total height: 100px + (2 * 20px) + (2 * 8px) = 156px */
}

.second-box {
  width: 200px;
  height: 100px;
  border: 8px solid blue;
  background: yellow;
  /* Total width: 200px + (2 * 8px) = 216px
     Total height: 100px +  (2 * 8px) = 116px */
} 
```

如代码片段所示，CSS 将`padding`和`border`添加到已经指定的`width`和`height`中。它将总值显示为元素的大小，从而忽略您分配给`div`的实际大小。

## 使用 CSS 框大小属性

使用`box-sizing`属性，可以改变上面解释的默认行为。

使用相同的代码，让我们添加`box-sizing`属性并将其设置为`border-box`，看看我们是否可以实际控制大小。

[https://codepen.io/edyasikpo/embed/preview/zYrbbwP?height=300&slug-hash=zYrbbwP&default-tabs=css,result&host=https://codepen.io](https://codepen.io/edyasikpo/embed/preview/zYrbbwP?height=300&slug-hash=zYrbbwP&default-tabs=css,result&host=https://codepen.io)

您一定已经注意到两个`div`元素现在有了相同的大小。

## 句法

```
box-sizing:content-box;
box-sizing:border-box; 
```

## 内容盒

这是框大小属性的默认行为。`content-box`不考虑为元素指定的`width`和`height`。

也就是说，如果你设置一个元素的`width`为 **200 像素**，然后设置边框为 **8 像素**，填充为 **20 像素**，那么`border`和`padding`的大小将被添加到最终的渲染宽度中。这使得元素比**宽 200 像素**。

```
div{
  box-sizing:content-box;
  width: 200px;
  border: 8px solid blue;
  padding: 20px;
  background: yellow;
  /* Total width: 200px + (2 * 20px) + (2 * 8px) = 256px*/
} 
```

如上面的代码片段所示，这个`div`元素的大小已经自动增加到 256px，即使它最初被设置为 200px。

## 边框

当您将`box-sizing`属性设置为`border-box`时，它会告诉浏览器考虑分配给元素宽度和高度的任何`border`和`padding`。

也就是说，如果您将元素的`width`设置为 **200 像素**，这 200 像素将包括您添加的任何边框或填充，并且内容框将收缩以吸收额外的宽度。

```
div{
  box-sizing:border-box;
  width: 200px;
  border: 8px solid blue;
  padding: 20px;
  background: yellow;
  /* Total width: 200px - (2 * 20px) - (2 * 8px) = 144px*/
} 
```

如上面的代码片段所示，这个`div`元素的大小已经自动减小到 144px，即使它最初被设置为 200px。

让我们合并两个代码片段，看看这个盒子用`content-box`和`border-box`会是什么样子。

[https://codepen.io/edyasikpo/embed/preview/xxZerWW?height=300&slug-hash=xxZerWW&default-tabs=css,result&host=https://codepen.io](https://codepen.io/edyasikpo/embed/preview/xxZerWW?height=300&slug-hash=xxZerWW&default-tabs=css,result&host=https://codepen.io)

## 结论

使用 CSS 框大小属性，您可以设置如何计算代码中元素的大小。

根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) ，当你布局元素时，设置`box-sizing`到`border-box`通常是有用的。这使得处理元素的大小变得容易得多，并且通常消除了您在布局内容时可能遇到的一些陷阱。

另一方面，当你使用`position: relative`或`position: absolute`时，使用`box-sizing: content-box`允许定位值相对于内容，并且独立于边框和填充大小的改变。有时候你可能想要这个。

那都是乡亲们！我希望这有所帮助。如果有，请分享这篇文章，并在 [Twitter](https://twitter.com/Didicodes) 上关注我。