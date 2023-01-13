# 基本 HTML5 模板:使用这个 HTML 样板作为任何 Web 开发项目的开始

> 原文：<https://www.freecodecamp.org/news/basic-html5-template-boilerplate-code-example/>

当你在建立一个新的网站时，有一个良好的基础是很重要的。在本文中，我将解释什么是 HTML 5 样板文件，以及如何创建一个基本模板用于您的项目。

## 什么是 HTML 5 样板文件？

根据[维基百科](https://en.wikipedia.org/wiki/Boilerplate_code#HTML)，

> 样板代码或仅仅是**样板代码**是在多个地方重复的代码段，几乎没有变化。

HTML 中的样板文件是您将在项目开始时添加的模板。您应该将这个样板文件添加到所有的 HTML 页面中。

## HTML 5 样板文件示例

让我们看一个基本的例子。

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5 Boilerplate</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
	<script src="index.js"></script>
  </body>
</html>
```

### HTML 中的 doctype 是什么？

HTML 代码中的第一行应该是 doctype 声明。doctype 告诉浏览器页面是用什么版本的 HTML 编写的。

```
<!DOCTYPE html>
```

如果您忘记在文件中包含这一行代码，那么浏览器可能不支持一些 HTML 5 标签，如`<article>`、`< footer >`和`<header>`。

### 什么是 HTML 根元素？

标签是 HTML 文件的顶层元素。您将在其中嵌套`<head>`和`<body>`标签。

```
<!DOCTYPE html>
<html lang="en">
  <head></head>
  <body></body>
</html>
```

开始标记`<html>`中的`lang`属性设置页面的语言。出于可访问性的原因，包含它也是很好的，因为屏幕阅读器将知道如何正确地发音。

### HTML 中的 head 标签是什么？

标签包含由机器处理的信息。在`<head>`标签中，您将嵌套元数据，元数据是向机器描述文档的数据。

```
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5 Boilerplate</title>
    <link rel="stylesheet" href="style.css">
</head>
```

### 什么是 UTF 8 字符编码？

UTF-8 是你应该在你的网页中使用的标准字符编码。这通常是在`<head>`元素中显示的第一个`<meta>`标签。

```
 <meta charset="UTF-8">
```

据[环球网财团](https://www.w3.org/International/questions/qa-choosing-encodings)，

> 基于 Unicode 的编码(如 UTF 8)可以支持多种语言，并且可以容纳这些语言混合使用的页面和表单。它的使用还消除了服务器端逻辑为提供的每个页面或每个输入表单提交单独确定字符编码的需要。

### HTML 中的 viewport meta 标签是什么？

该标签将页面的宽度呈现为设备屏幕尺寸的宽度。如果您的移动设备是 600 像素宽，那么浏览器窗口也将是 600 像素宽。

初始比例控制缩放级别。初始比例值为 1 会阻止浏览器的默认缩放。

```
 <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
```

### X-UA 兼容是什么意思？

这个`<meta>`标签指定了 Internet Explorer 的文档模式。`IE=edge`是最高支持的模式。

```
 <meta http-equiv="X-UA-Compatible" content="ie=edge"> 
```

### 什么是 HTML 标题标签？

标签是网页的标题。该文本显示在浏览器的标题栏中。

```
 <title>HTML 5 Boilerplate</title> 
```

![Screen-Shot-2021-07-30-at-4.15.25-AM](img/a4b7774a73865b77830983bd1b149d4a.png)

### CSS 样式表

这段代码将把你的自定义 CSS 链接到 HTML 页面。`rel="stylesheet"`定义 HTML 文件和外部样式表之间的关系。

```
 <link rel="stylesheet" href="style.css"> 
```

### HTML 中的脚本标记

外部脚本标记将放在结束 body 标记之前。这是您可以链接外部 JavaScript 代码的地方。

```
 <script src="index.js"></script> 
```

## 结论

您应该在每个 HTML 页面中添加一个 HTML 5 样板文件。这个起始代码包含重要的信息，比如 doctype、元数据、外部样式表和脚本标签。