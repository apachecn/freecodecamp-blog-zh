# HTML 中的内联样式–CSS 内联样式

> 原文：<https://www.freecodecamp.org/news/inline-style-in-html/>

级联样式表(CSS)是一种确定网页外观的标记语言。它管理网站元素的颜色、字体和布局，并允许您向页面添加效果或动画。

我们可以用三种方式来设计 HTML 文件/页面的样式:外部样式、内部样式和内联样式。在本文中，我们将关注内联样式。

## 如何在 HTML 中使用内联样式

使用 style 属性，我们可以通过内联样式将样式应用于单个 HTML 标记中的 HTML。

```
<h1 style="...">...</h1>
```

style 属性的工作方式与任何其他 HTML 属性相同。我们使用`style`，后跟等号(=)，然后是一个引号，其中所有的样式值都将使用标准的 CSS 属性-值对- `"property: value;"`来存储。

**注意:**只要用分号(；).

值得注意的是,`style`属性通常用在开始的 HTML 标记中，因为这是 HTML 元素的一部分，可以包含文本、数据、图像或者什么都不包含。内嵌样式的一个示例如下:

```
<h1 style="color: red; font-size: 40px;">Hello World</h1>
```

这类似于这样:

```
h1 {
  color: red;
  font-size: 40px;
}
```

唯一的区别是内联样式只应用于它所应用的标签，而第二个代码示例影响 html 页面上的所有`p`标签。

### 何时使用内嵌样式

但是，使用内联样式并不被认为是最佳实践，因为它会导致大量的重复——因为样式不能在其他地方重用。

但是有时候内联样式是最好的(或者唯一的)选择，比如在设计 HTML 电子邮件、WordPress、Drupal 等 CMS 内容时。您还可以在设计动态内容样式时使用它们，动态内容是由 HTML 创建或由 JavaScript 更改的。

除了`!important`声明，内联样式有一个[高特异性/最高优先级](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#Understanding_the_cascade)，这意味着它们将覆盖内部和外部样式表中的大多数其他规则。

假设我们有两个段落文本，内联样式设置为`red`，内部样式设置为`green`:

```
<html>
  <head>
      <title>Hello World</title>
      <style>
       p {
           color: green;
       }
   </style>
  </head>

  <body>
     <p style="color: red;">Paragraph one is red.</p>
     <p style="color: red;">Paragraph two is also red.</p>
  </body>
</html>
```

我们内联样式的 CSS 将覆盖内部样式的 CSS，所以两个段落都是`red`。

## 内嵌样式的优点和缺点

到目前为止，我们已经了解了什么是内联样式以及如何在 HTML 标签中使用它。现在，让我们看看优缺点，看看什么时候应该使用内联样式，什么时候不应该。

### 内联 CSS 的优势:

*   内嵌优先于所有其他样式。内部和外部样式表中定义的任何样式都会被内联样式覆盖。
*   您可以快速轻松地将 CSS 规则插入 HTML 页面，这对于测试或预览更改以及在网站上执行快速修复非常有用。
*   不需要创建额外的文件。
*   要在 JavaScript 中应用样式，请使用`style`属性。

### 内联 CSS 的缺点:

*   向每个 HTML 元素添加 CSS 规则需要时间，并且会使 HTML 结构变得杂乱无章。很难保持、重用和扩展。
*   设计多个元素的样式会影响页面的大小和下载时间。
*   内联样式不能用于样式化伪元素和伪类。例如，您可以使用外部和内部样式表来设置锚标记的已访问、悬停、活动和链接颜色的样式。

## 结论

在本文中，我们学习了如何在 HTML 中使用内联样式，何时使用，以及这样做的一些好处和缺点。

因为内联样式优先于所有其他样式，所以使用它的最佳时机之一是在你的网站上测试或预览变化以及执行快速修复的时候。