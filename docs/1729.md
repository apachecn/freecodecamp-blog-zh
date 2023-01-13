# 什么是超链接？用例子解释 HTML 链接

> 原文：<https://www.freecodecamp.org/news/what-is-a-hyperlink-definition-for-beginners/>

让我们从一个小问题开始:你今天是如何登陆这篇文章和这个网站的？

嗯，很确切地说，通过点击或轻敲链接，对不对？这就是这个元素的强大之处——只需点击一个链接，它就能让你进入互联网的任何地方。

那么，HTML 中的链接和超链接是什么呢？

## HTML 中的链接和超链接是什么？

链接是连接一个网站内的页面和其他网站的链。没有链接，我们就不会有任何网站。

比如我们来看看这个网址，`https://www.freecodecamp.org/`。当你在地址栏输入它，它会带你到官方的 freeCodeCamp 网站。

简单来说，我们可以说链接只是网页的网址，它允许你连接不同的服务器。

那么，也许你想知道什么是超级链接？嗯，它们允许我们通过称为锚标签的引用将文档链接到其他文档或资源。它们是万维网背后的一个基本概念，它通过链接使网页之间的导航更加容易。

超链接可以以不同的形式呈现，如图像、图标、文本或任何类型的可视元素，单击这些元素会将您重定向到指定的 url。

例如，如果你点击[这里](https://www.freecodecamp.org/news/author/larymak/)，你将进入我的个人资料，里面有我的其他文章列表。那是一个超链接。

## 如何在 HTML 中创建链接

### 如何使用`<a>`链接

您可以通过将文本(或任何其他相关内容)包装在`<a></a>`元素中并使用包含地址的`href`属性来创建一个基本链接。

下面是一个实际例子:

```
<a href="https://www.freecodecamp.org">Visit: Freecode Camp!</a> 
```

### 如何设计链接的样式

通常在`.html`文件中插入链接，将主文件链接到样式和功能文件。它们通常以文件扩展名`.css`和`.js`命名。

以下是链接到 CSS 文件的方法:

```
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html> 
```

下面是如何链接到一个 JS 文件。您可以将想要链接的内容放在 head 或 body 标签中。

```
<!DOCTYPE html>
<html>
<head>
  <script src="myScript.js"></script>
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html> 
```

## 如何在网站内链接

### 链接到网站内的不同页面

您可以使用这种类型的链接将用户定向到同一站点上的不同页面。

举个例子，你的网站有 5 个页面。你会希望用户能够从一个点访问所有的页面，比如导航条。

首先，你必须创建所有的网页，然后链接它们。在这种情况下，我们将这样做:

```
<nav>
    <ul>
        <li><a href="home.html">Home</a></li>
        <li><a href="services.html">Services</a></li>
        <li><a href="pricing.html">Pricing</a></li>
        <li><a href="about.html">About</a></li>
        <li><a href="contact.html">Contact</a></li>
    </ul>
</nav> 
```

上面的例子将链接到网站的不同部分，依次是“主页”、“服务”、“定价”和“关于”。只写文件名就足够了，因为所有的工作都在同一个工作文件夹中共享。

### 如何链接到网站的特定部分

比方说，在你网站的某个地方，你提到了一个特定的主题或页面。你希望你的读者或访问者点击一下就直接跳到那个部分。

首先，您必须将`id`属性添加到页面的该部分。也许这是一个段落——如果是这样，它应该是这样的:

```
<p id="detailed-info"> Read more on Upcoming Events </p> 
```

在此之后，在链接的`href`中添加`id`:

```
<a href="#detailed-info"> Read more about upcoming events </a> 
```

这段代码将把他们带到即将到来的事件部分。

## HTML 中其他类型的链接

### 如何链接到可下载文件

当您想要链接到用户需要下载而不是在浏览器中打开的资源时，您可以使用`download`属性。这提供了默认的保存文件名。

下载属性对于 pdf、图像文件、视频和音频剪辑以及您希望用户保存在其计算机或移动设备上的其他媒体内容非常有用。

下面是一个带有下载链接的示例:

```
<a href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"
   download="firefox-latest-64bit-installer.exe">
  Download Latest Firefox for Windows (64-bit) (English, US)
</a> 
```

### 如何添加电子邮件链接

电子邮件链接允许我们创建指向电子邮件地址的超链接。您可以使用 HTML `<a></a>`标签创建这些链接——但是在这种情况下，我们传递的不是 URL，而是收件人的电子邮件地址。

您使用`mailto`属性在锚标记中指定电子邮件地址。

例如:

```
<a href="mailto:hillarynyk@gmail.com">Send email to Me</a> 
```

除了电子邮件地址，您还可以提供其他信息。事实上，任何标准的邮件头字段都可以添加到您提供的`mailto` URL 中。最常用的是“主题”、“抄送”和“正文”。

下面是一个包括抄送、密件抄送、主题和正文的示例:

```
<a href="mailto:hillarynyk@gmail.com?cc=larymak4@gmail.com&bcc=larymak8@gmail.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
  Send mail with cc, bcc, subject and body
</a> 
```

## HTML 链接属性

HTML 链接有各种属性，可以用来提供更多的特定信息。下面是一些最常用的。

1.  **href** 属性
    `href`属性定义了 HTML 链接的目标 URL 地址。它使标记的单词或短语可点击。href 属性创建一个到另一个网页的超链接，如果没有它，HTML 链接将无法正常工作。

2.  **目标**属性
    `target`属性定义了 HTML 链接将被打开的位置。曾经访问过一个网站，当你点击一个链接，它会自动在一个新的标签页打开？这就是这个属性的工作。

以下是所有可用于`target`属性的选项:

*   **-空白** = >在新标签页打开链接。大多用于避免失去网站的访问者。默认情况下，当用户点击一个链接时，它会在同一个标签中打开，但这改变了这一点。

```
<a href= "https://www.freecodecamp.org/" target="_blank"> freeCodeCamp</a> 
```

*   **_self** = >在同一个窗口或者它被点击的标签中加载 URL。这通常是默认设置，不需要指定。

```
<a href="https://www.freecodecamp.org" target="_self">freeCodeCamp</a> 
```

*   **_parent** = >加载父框架中的 URL。它只和框架一起使用。

```
<a href="https://www.freecodecamp.org" target="_parent">freeCodeCamp</a> 
```

*   **_top** = >将链接的文档载入整个窗口。

```
<a href="https://www.freecodecamp.org" target="_top">freeCodeCamp</a> 
```

3.  **标题**属性
    `title`属性概述了关于链接目的的额外信息。如果用户将鼠标悬停在链接上，将会出现一个工具提示，描述目标、标题或关于链接的任何其他信息:

```
<a href="https://www.freecodecamp.org" title="Link to free learning Resources">Learn to code</a> 
```

## 快速 HTML 链接提示和摘要

在这篇文章中，我们学习了 HTML 中的所有链接和超链接。以下是帮助您使用链接的一些最终提示。

首先，当你使用图片作为链接时，在文本中包含`alt`标签总是一个好主意。这提供了在图片无法加载的情况下显示的替代文本。

其次，您应该练习使用`hreflang`属性指定锚链接到的文档的语言。

```
<a href="https://www.freecodecamp.org" hreflang="en">Freecode Camp</a> 
```

Web 实际上只是一个超链接文档库，其中锚标签充当相关文档之间的桥梁。

我们已经看到了如何使用和创建链接，以及为什么它们在 web 开发中很重要。

是时候去练习和完善你的新技能了。

享受编码❤