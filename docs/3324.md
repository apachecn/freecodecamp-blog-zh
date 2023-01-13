# CSS 背景图像大小教程——如何编码一整页背景图像

> 原文：<https://www.freecodecamp.org/news/css-full-page-background-image-tutorial/>

本教程将向你展示一个简单的方法来使用 CSS 编码一个完整的页面背景图片。您还将学习如何根据用户的屏幕尺寸制作图像。

使背景图像完全伸展以覆盖整个浏览器视窗是 web 设计中的一项常见任务。幸运的是，这个任务可以通过几行 CSS 来完成。

## 用图像覆盖视口

首先，我们要确保我们的页面实际上覆盖了整个视窗:

```
html {
   min-height: 100%;
}
```

在`html`中，我们将使用`background-image`属性来设置图像:

```
background-image: url(image.jpg); /*replace image.jpg with path to your image*/
```

## “背景大小”属性的魔力

神奇的事情发生在`background-size`属性上:

```
background-size: cover;
```

`cover`告诉浏览器确保图像总是覆盖整个容器，在本例中是`html`。浏览器将覆盖容器，即使它不得不拉伸图像或切掉一些边缘。

因为浏览器可能会拉伸图像，所以您应该使用分辨率足够高的背景图像。否则图像可能出现像素化。

如果你想让所有的图像都出现在背景中，那么你就要确保图像的长宽比相对于屏幕尺寸来说比较接近。

## 如何微调图像位置并避免平铺

浏览器还可以根据背景图像的大小选择将其显示为平铺图像。为了防止这种情况发生，使用`no-repeat`:

```
background-repeat: no-repeat;
```

为了保持美观，我们将始终保持我们的形象:

```
background-position: center center;
```

这将使图像始终水平和垂直居中。

我们现在已经制作了一个完全响应的图像，它将始终覆盖整个背景:

```
html {
   min-height: 100%;
   background-image: url(image.jpg);
   background-size: cover;
   background-repeat: no-repeat;
   background-position: center center;
} 
```

## 如何在滚动时固定图像

现在，万一您想要在背景图像上添加文本，并且该文本溢出了当前视窗，您可能希望确保当用户向下滚动以查看文本的其余部分时，您的图像留在背景中:

```
background-attachment: fixed;
```

您可以使用简短符号包括上述所有背景属性:

```
background: url(image.jpg) center center cover no-repeat fixed;
```

## 可选:如何为移动设备添加媒体查询

为了锦上添花，您可能希望为较小的屏幕添加媒体查询:

```
@media only screen and (max-width: 767px) {
  html {
     background-image: url(smaller-image.jpg);
  }
} 
```

您可以使用 Photoshop 或其他图像编辑软件缩小原始图像，以提高移动互联网连接的页面加载速度。

所以现在你知道了创建一个响应迅速的全页面图像背景的魔力，是时候去创建一些漂亮的网站了！

## 想看更多的网页开发技巧和知识？

*   订阅我的每周简讯
*   在 [1000 英里世界](https://1000mileworld.com/)访问我的博客
*   在 Twitter 上关注我