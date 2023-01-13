# 如何使用 CSS 将图像垂直和水平居中

> 原文：<https://www.freecodecamp.org/news/how-to-center-an-image-in-css/>

许多开发人员在处理图像时会遇到困难。处理[响应](https://www.freecodecamp.org/news/css-responsive-image-tutorial/)和对齐是特别困难的，尤其是将图像放在页面中间。

所以在这篇文章中，我将展示一些最常用的方法，使用不同的 CSS 属性来垂直和水平地居中图片。

我在上一篇文章中已经讨论过 CSS [位置](https://www.freecodecamp.org/news/how-to-use-the-position-property-in-css-to-align-elements-d8f49c403a26/)和[显示](https://www.youtube.com/watch?v=hgoFi0fCv3w)属性。如果您不熟悉这些属性，我建议在阅读本文之前查看一下这些帖子。

如果你想看的话，这里有一个视频版本:

[https://www.youtube.com/embed/mwVNVxpkly0?feature=oembed](https://www.youtube.com/embed/mwVNVxpkly0?feature=oembed)

## 将图像水平居中

让我们从使用 3 个不同的 CSS 属性来水平居中图像开始。

### 文本对齐

将图像水平居中的第一种方法是使用`text-align`属性。然而，这种方法只有在图像位于块级容器(如`<div>`)中时才有效:

```
<style>
  div {
    text-align: center;
  }
</style>

<div>
  <img src="your-image.jpg">
</div>
```

### 边距:自动

另一种使图像居中的方法是使用`margin: auto`属性(用于左边距和右边距)。

然而，单独使用`margin: auto`对图像不起作用。如果您需要使用`margin: auto`，还有 2 个额外的属性必须使用。

自动边距属性对行内级别的元素没有任何影响。由于`<img>`标签是一个内联元素，我们需要首先将它转换成一个块级元素:

```
img {
  margin: auto;
  display: block;
}
```

其次，我们还需要定义一个宽度。因此，左边距和右边距可以占据剩余的空白空间并自动对齐，这样就成功了(除非我们给它 100%的宽度):

```
img {
  width: 60%;
  margin: auto;
  display: block;
}
```

### 显示器:Flex

第三种将图像水平居中的方法是使用`display: flex`。就像我们对容器使用了`text-align`属性一样，我们也对容器使用了`display: flex`。

然而，仅仅使用`display: flex`是不够的。容器还必须有一个名为`justify-content`的附加属性:

```
div {
  display: flex;
  justify-content: center;
}

img {
  width: 60%;
}
```

`justify-content`属性与`display: flex`一起工作，我们可以用它来水平居中图像。

最后，图像的宽度必须小于容器的宽度，否则，它会占用 100%的空间，然后我们无法将其居中。

**重要提示:**旧版本的浏览器不支持`display: flex`属性。[点击这里](https://caniuse.com/#search=display%20flex)了解更多详情。

## 将图像垂直居中

### 显示器:Flex

对于垂直对齐，使用`display: flex`也非常有用。

考虑这样一种情况，我们的容器有 800 像素的高度，但是图像的高度只有 500 像素:

```
div {
  display: flex;
  justify-content: center;
  height: 800px;
}

img {
  width: 60%;
  height: 500px;
}
```

现在，在这种情况下，向容器中添加一行代码`align-items: center`，就能达到目的:

```
div {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 800px;
}
```

如果与`display: flex`一起使用，`align-items`属性可以垂直定位元素。

### 位置:绝对和变换属性

另一种垂直对齐的方法是同时使用`position`和`transform`属性。这个有点复杂，我们一步一步来。

### 步骤 1:定义绝对位置

首先，我们将图像的定位行为从`static`更改为`absolute`:

```
div {
  height: 800px;
  position: relative;
  background: red;
}

img {
  width: 80%;
  position: absolute;
}
```

而且，它应该在一个相对定位的容器中，所以我们将`position: relative`添加到它的容器 div 中。

### 步骤 2:定义左上属性

其次，我们为图像定义顶部和左侧属性，并将其设置为 50%。这将把图像的起点(左上)移动到容器的中心:

```
img {
  width: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
}
```

### 步骤 3:定义转换属性

但是第二步将图像部分移出了它的容器。所以我们需要把它带回来。

定义一个`transform`属性并给它的 X 和 Y 轴加-50%就可以了:

```
img {
  width: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### 

还有其他方法可以水平和垂直居中，但我已经解释了最常见的方法。我希望这篇文章能帮助你理解如何在页面中央对齐你的图片。

如果你想了解更多关于网络开发的知识，欢迎访问我的 Youtube 频道。

感谢您的阅读！