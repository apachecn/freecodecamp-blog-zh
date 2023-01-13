# 如何设置 CSS 媒体查询的宽度范围

> 原文：<https://www.freecodecamp.org/news/media-queries-width-ranges/>

媒体查询是 CSS 的一个特性。它允许您创建和实现适应您所使用的设备属性的布局。这些属性包括页面的高度和宽度。

在这个简短的指南中，您将看到如何在介质查询规则中设置多个宽度属性。现在，让我们先看看基本面。

## CSS 中媒体查询的工作方式

现在您对什么是媒体查询有了一个基本的概念，让我们看看 CSS 的这个特殊特性实际上是如何工作的。

一个基本的媒体查询如下所示:

```
@media only screen and (max-width: 576px) {
    // do something
}

@media only screen and (min-width: 576px) {
    // do something again
}
```

这意味着在上面的媒体规则中编写的样式将只在上面指定的宽度属性下工作或有效。

从字面上看，你是说在这个特定的宽度(也就是说，`576px`的`max-width`属性)，CSS 请应用我将在这里写的样式到这个宽度，从初始宽度`0px`。

## 最大宽度和最小宽度属性

当您为不同的屏幕尺寸创建媒体查询时，需要记住两件事:`max-width`和`min-width`属性。

当一个`max-width`属性被传入媒体查询时，CSS 将其解释为从零开始的宽度——也就是说，如果还没有设置最小宽度属性的话。我们很快就会谈到这一点。

一旦`max-width`属性被赋予一个值，该特定媒体查询中的所有样式都将被应用于屏幕大小从`0px`到指定最大宽度的任何设备。

`min-width`属性，另一方面，接受您赋予它的初始值，并应用媒体规则中的样式，直到下一个`max-width`被提供。比如说:

```
@media only screen and (min-width: 576px) {
    // apply the styles here from this minimum width of 
    // 576px (medium screen sized devices)
}
```

将在上述媒体查询中写入的样式仅适用于在指定的最小宽度及以上范围内的设备。

## 如何设置媒体查询的宽度范围

我们刚刚讨论的通过应用一个`width`属性来创建媒体查询的方法只解决了一个问题。

通过设置“宽度范围”，您可以在创建布局时拥有一定的灵活性，从而在所有设备宽度范围内做出响应。

设置特定的“宽度范围”与创建媒体查询的方式没有任何不同。唯一的区别是增加了更多的媒体特征表达(即屏幕宽度尺寸)。

看一看:

```
@media only screen and (min-width: 360px) and (max-width: 768px) {
	// do something in this width range.
}
```

上面的媒体查询只适用于上面提供的特征表达式(您正在为其编写样式的移动设备的屏幕大小)。

将提供的第一个宽度表达式作为初始值，第二个作为最终值。

还记得我们在上面看到的`max-width`和`min-width`属性的区别吗？我们只是告诉浏览器将我们在规则中编写的 CSS 样式应用到屏幕尺寸从`360px`到`768px`的移动设备上。

从根本上说，我们正在构建从小型设备宽度到中型设备宽度的布局，如平板电脑或移动设备。

你可以看看在 [Bootstrap 的文档](https://getbootstrap.com/docs/5.0/layout/breakpoints/)中提供的一些媒体断点。通过设置您喜欢的屏幕尺寸范围来尝试使用它们。

## 使用 Flexbox 尝试媒体查询

您已经看到了如何创建一个接受多个屏幕尺寸表达式的基本媒体查询。您已经看到了`max-width`和`min-width`属性之间的区别以及如何应用它们。

在这一节中，我们将看看如何创建一个简单的布局，它的外观在不同的媒体断点(屏幕大小)发生变化。我们将从创建保存布局的标记开始。

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Media query example</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="container">
      <div class="boxes box1">
        <h1>First Box</h1>
        <p>
		Information in the first box
        </p>
      </div>
      <div class="boxes box2">
        <h1>Second Box</h1>
        <p>
          Information in the second box
        </p>
      </div>
  </body>
</html> 
```

当应用样式时，上面的代码片段将显示两个框以及它们各自的信息。如果你愿意，你可以在这里查看完整的代码示例。

```
.container {
  display: flex;
  flex-wrap: wrap;
  width: 980px;
  margin: 0 auto;
  margin-top: 40px;
}

@media only screen and (min-width: 320px) and (max-width: 576px) {
  .container {
    width: 100%;
    padding-left: 23px;
    flex-direction: column-reverse;
  }
  .boxes {
    width: 100%;
  }
}
```

看一下 CSS 文件，您会注意到媒体查询的最小宽度为`320px`，最大宽度为`576px`。这只是意味着，所有的风格，将进入这一规则，将只适用于设备与超小和小宽度。

盒子的布局也在这个特定的宽度范围内变化。这是因为`.container`选择器的`flex-direction`属性从`row`更改为`column-reverse`。

您可以决定尝试其他可以分配给`flex-direction`属性的值。点击这里查看它们。

## 结论

如果不设置宽度范围，上方的片段[的 CSS 样式将应用于最小屏幕尺寸为`576px`及以上的所有设备。](#max-width-and-min-width)

当你设置一个宽度范围时，作为一个开发者你可以得到更好的控制。您将能够指定哪些样式应该应用于具有特定屏幕大小的设备，从而使您的应用程序具有更好的响应能力。

感谢您阅读这篇文章。如果它有助于你理解为什么在创建媒体查询时应该设置宽度范围，请分享它。