# 如何使用标签上的 HREF 属性制作 HTML 超链接

> 原文：<https://www.freecodecamp.org/news/how-to-make-html-hyperlinks-using-the-href-attribute-on-tags/>

网站是网页的集合。而这些页面需要通过某种东西来链接或连接。为此，我们需要使用 HTML 提供的标签:`a`标签。

这个标签定义了一个超链接，用于从一个页面链接到另一个页面。`a`元素最重要的属性是`href`属性，它表示链接的目的地。

在本指南中，我将向您展示如何使用`a`标签上的`href`属性制作 HTML 超链接。

*   [什么是链接？](#what-is-a-link)
*   [如何创建内部链接](#how-to-create-internal-links)
*   [浏览同一级别的页面](#browse-to-pages-on-the-same-level)
*   [浏览位于另一个文件夹的页面](#browse-to-pages-located-on-another-folder)
*   [从文件夹中的页面浏览到根目录](#browse-from-a-page-located-in-a-folder-to-the-root)
*   [如何创建外部链接](#how-to-create-external-links)
*   [如何创建锚点链接](#how-to-create-anchor-links)
*   [在同一页面上导航](#navigate-on-the-same-page)
*   [在另一个页面导航](#navigate-on-another-page)
*   [结论](#conclusion)

## 什么是链接？

链接是可点击的文本，允许您从一个页面浏览到另一个页面，或者浏览到同一页面的不同部分。

在 web 开发中，有几种方法可以创建链接，但最常用的是使用`a`标签和`href`属性。这最后是我们指定链接的目的地址。

标签帮助我们创建三种主要的链接:内部链接、外部链接和锚链接。也就是说，我们现在可以在下一节深入探讨如何创建内部链接。

## 如何创建内部链接

当建立一个网站时，页面之间的连接是有意义的。而且只要这些链接能让我们从 A 页导航到 B 页，就叫内部链接(因为总是在同一个域名或者同一个网站)。因此，内部链接是一个允许导航到网站上另一个页面的链接。

现在，考虑到我们的文件夹结构如下:

```
├── folder1
|  └── page2.html
├── page1.html
├── index.html 
```

这里我们有三个用例，对于每一个，我们将创建一个例子。

### 浏览到同一级别的页面

*   `index.html`

```
<a href="page1.html">Browse to Page 1</a> 
```

如您所见，页面`page1.html`在同一层，因此`href`属性的路径就是页面的名称。

### 浏览到位于另一个文件夹中的页面

*   `page1.html`

```
<a href="./folder1/page2.html">Browse to Page 2</a> 
```

这里，我们有一个不同的用例，因为我们想要访问的页面现在不在同一层上。为了能够导航到该页面，我们应该指定包括文件夹在内的文件的完整路径。

太好了！现在让我们深入最后一个用例。

### 从文件夹中的页面浏览到根目录

*   `page2.html`

```
<a href="../index.html">Browse to the Home Page</a> 
```

现在，为了导航到`index.html`页面，我们应该先向上一级，然后才能浏览到该页面。

我们已经讨论了内部链接，所以让我们继续介绍如何导航到外部链接。

## 如何创建外部链接

拥有导航到外部网站的能力总是很有用的。

```
<a href="https://www.google.com/">Browse to Google</a> 
```

正如您在这里看到的，要导航到 Google，我们必须将它的 URL 指定给`href`属性。

外部和内部链接用于从页面 A 导航到页面 b。然而，有时我们希望停留在同一页面并导航到特定部分。要做到这一点，我们必须使用一种叫做锚定链接的东西，让我们在下一节深入探讨它。

## 如何创建锚定链接

锚链接更具体一点:它允许我们从 A 点导航到 B 点，同时保持在同一页面上。它还可以用于转到另一个页面上的某个部分。

### 在同一页面上导航

```
<a href="#about">Go to the anchor</a>
<h1 id="about"></h1> 
```

和其他的相比，这个有点不同。的确是这样，但它仍然以同样的方式工作。

这里，我们有一个对元素的引用，而不是页面链接。当我们点击链接时，它会寻找没有标签(`about`)的同名元素。如果找到该 id，它将浏览到该部分。

### 在另一页上导航

```
<a href="page1.html#about">Go to the anchor</a> 
```

这与上一个例子非常相似，除了我们必须定义我们想要浏览的页面并添加锚。

## 结论

`href`是`a`标签最重要的属性。它允许我们在页面之间导航，或者只在页面的特定部分导航。希望这个指南能帮助你建立一个有很多链接的网站。

感谢阅读。

请随时联系我！

[推特](https://twitter.com/ibrahima92_) [博客](https://www.ibrahima-ndaw.com/) [简讯](https://ibrahima-ndaw.us5.list-manage.com/subscribe?u=8dedf5d07c7326802dd81a866&id=5d7bcd5b75)[GITHUB](https://github.com/ibrahima92)[LINKEDIN](https://www.linkedin.com/in/ibrahima-ndaw/)[CODEPEN](https://codepen.io/ibrahima92)[DEV](https://dev.to/ibrahima92)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/link?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)