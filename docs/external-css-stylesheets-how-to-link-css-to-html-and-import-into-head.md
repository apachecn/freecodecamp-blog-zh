# 外部 CSS 样式表——如何将 CSS 链接到 HTML 并导入 Head

> 原文：<https://www.freecodecamp.org/news/external-css-stylesheets-how-to-link-css-to-html-and-import-into-head/>

将 CSS 样式表保存在外部文件中被认为是一种最佳实践。那么，如何将 CSS 链接到 HTML 文件呢？

链接到外部 CSS 文件是任何 [HTML 页面样板](https://www.freecodecamp.org/news/basic-html5-template-boilerplate-code-example/)的重要部分。在这篇文章中，我们将学习如何去做。

## **如何将 CSS 文件链接到 HTML 文件**

通过在 HTML 文件的`head`元素中添加一个`link`元素，可以将 CSS 文件链接到 HTML 文件，如下所示:

```
<!DOCTYPE html>
  <html>
    <head>
      <link rel="stylesheet" href="style.css">
    </head>
    <body>

    </body>
</html>
```

`link`元素有许多用途，指定正确的属性很重要，这样就可以用它来导入外部 CSS 样式表。我们现在来看看一些重要的属性。

## **`rel`属性**

两个不可或缺的属性中的第一个是`rel`属性。您将使用这个属性告诉浏览器它与导入的文件有什么关系。

您将编写`rel="stylesheet"`来告诉浏览器您正在导入一个样式表。

## **`href`属性**

第二个不可或缺的属性是`href`属性，它指定了要导入的文件。

一种常见的情况是 CSS 文件和 HTML 文件在同一个文件夹中。在这种情况下，你可以写`href="style.css"`。

如果 CSS 文件和 HTML 文件在不同的文件夹中，你需要写出从 HTML 文件到 CSS 文件的正确路径。

例如，一种常见的情况是 CSS 文件位于 HTML 文件的同级文件夹中，如下所示:

```
project --- index.html
            css ---------- style.css
```

在这种情况下，您需要编写一个类似于`"css/styles.css"`的路径。

## **`type`属性**

```
<link rel="stylesheet" href="style.css" type="text/css">
```

使用`type`属性来定义链接内容的类型。对于样式表，这将是`text/css`。但是由于`css`是 web 上唯一使用的样式表语言，它不仅是可选的，而且不包含它甚至是一个最佳实践。

## **`media`属性**

```
<link rel="stylesheet" href="style.css" media="screen and (max-width: 600px)">
```

媒体属性在示例中不可见。这是一个可选属性，可以用来指定何时导入某个样式表。其值必须是媒体类型/媒体查询。

如果您想在不同的文件中分离不同设备和屏幕尺寸的样式，这可能会很有用。您需要导入每个带有自己的`link`元素的 CSS 文件。

您可以查看这些关于媒体查询的文章(或其他来源),以了解更多关于可以作为属性值编写的内容:

*   [如何使用 CSS 媒体查询创建响应式网站](https://www.freecodecamp.org/news/how-to-use-css-media-queries-to-create-responsive-websites/)
*   [如何设置 CSS 媒体查询的宽度范围](https://www.freecodecamp.org/news/media-queries-width-ranges/)
*   [媒体查询 CSS 教程–标准分辨率、CSS 断点和目标手机尺寸](https://www.freecodecamp.org/news/css-media-queries-breakpoints-media-types-standard-resolutions-and-more/)

# **结论**

在本文中，您了解了如何使用`link`元素以及`href`和`src`属性向 web 页面添加外部样式表。

您还了解了可以导入多个样式表，并使用`media`属性来确定何时应该应用每个样式表。

享受创建网页的乐趣！