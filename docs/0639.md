# 如何将 CSS 链接到 HTML–样式表文件链接

> 原文：<https://www.freecodecamp.org/news/how-to-link-css-to-html/>

HTML 是帮助你定义网页结构的标记语言。CSS 是一种样式表语言，用于使结构看起来更好，布局更好。

要使你用 CSS 实现的样式反映在 HTML 中，你必须找到一种方法将 CSS 链接到 HTML。

您可以通过编写内联 CSS、内部 CSS 或外部 CSS 来进行链接。

将 CSS 与 HTML 分开是一种最佳实践，因此本文主要讨论如何将外部 CSS 链接到 HTML。

## 如何将 CSS 链接到 HTML

要将 CSS 链接到 HTML，您必须使用带有一些相关属性的 link 标签。

链接标签是一个自结束标签，你应该把它放在你的 HTML 的头部。

要用它将 CSS 链接到 HTML，你可以这样做:

```
<link rel="stylesheet" type="text/css" href="styles.css" /> 
```

将链接标签放在 HTML 的头部，如下所示:

```
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" type="text/css" href="styles.css" /> 
    <title>Link CSS to HTML</title>
</head> 
```

## 链接标签的属性

### `rel`属性

`rel`是外部文件和当前文件之间的关系。对于 CSS，你用`stylesheet`。例如，`rel="stylesheet"`。

### `type`属性

`type`是您链接到 HTML 的文档的类型。对于 CSS 来说，就是`text/css`。比如说`type="text/css"`。

### `href`属性

`href`代表“超文本参考”。您可以使用它来指定 CSS 文件的位置和文件名。它是一个可点击的链接，所以你也可以按住`CTRL`点击它来查看 CSS 文件。

例如，`href="styles.css"`如果 CSS 文件与 HTML 文件位于同一文件夹中。或者如果 CSS 文件位于另一个文件夹中，则显示`href="folder/styles.css"`。

## 最后的想法

本文向您展示了如何使用`link`标签和必要的属性将外部 CSS 文件正确链接到 HTML。

我们还看了一下每个属性的含义，这样你就不会在不知道它们如何工作的情况下使用它们。

继续编码…