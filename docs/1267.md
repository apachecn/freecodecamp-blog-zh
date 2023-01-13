# HTML 最佳实践——如何构建一个更好的基于 HTML 的网站

> 原文：<https://www.freecodecamp.org/news/html-best-practices/>

**HTML** 是任何网站的支柱。这是人们首先看到的。没有它，就没有网站。

因此，坚持良好的编码实践非常重要。如果您不遵循最佳实践，将会给 web 用户带来糟糕的用户体验。

不管你是编码新手还是有经验的专业人士，在 HTML 中总有新的东西要学。

在本文中，我们将讨论 HTML 的基本最佳实践。

让我们开始吧。💃

## HTML 最佳实践

HTML 最佳实践是帮助你创建易于维护和阅读的网站的规则。

以下是构建基于 HTML 的网站时需要记住的一些准则:

### 一个代码表只使用一个

# 元素

HTML 中有六种不同的标题标签，`<h1>`到`<h6>`。`<h1>`标签是主标题(网页的主题)，而`<h6>`标签是最不重要的标题。

`<h1>`标签比`<h2>`标签大，`<h2>`标签比`<h3>`标签大，一直到`<h6>`标签。每个标题的大小根据其重要性递减。

避免对一个代码表使用多个`<h1>`元素是很重要的。

别这样，⬇️:

```
<main>
<div>
<h1>Can Coding be fun?</h1>
<p>The more you code the better you become</p>
</div>

<div>
<h1>Coding is fun</h1>
<p>It is always better when you have fun coding</p>
</div>
</main>
```

在上面的例子中，我们在第一个和第二个`<div>`上使用了`<h1>`标签。以这种方式编码是可行的，但是尽管你会达到同样的目标，这并不是最佳实践。

做这个代替⬇️:

```
<main>

<div>
<h1>Can coding be fun?</h1>
<p>The more you code the better you become</p>
</div>

<div>
<h2>Coding is fun</h2>
<p>It is always better when you have fun coding</p>
</div>

</main> 
```

一个网页上只有一个`<h1>`元素对于搜索引擎优化(SEO)至关重要。它帮助搜索引擎理解一个网页是关于什么的(网页的主要思想)。

### 不要跳过 HTML 中的标题级别

使用标题标签时，从`<h1>`到`<h2>`再到`<h3>`到`<h4>`等等非常重要...

使用 header 标签时不要先使用`<h1>`再跳转到`<h3>`。当您跳过标题级别时，使用屏幕阅读器的 web 访问者很难理解您的网页内容。

屏幕阅读器是一种帮助有视觉障碍的人访问数字内容并与之交互的技术，如通过音频或触摸访问网站或应用程序。屏幕阅读器的主要用户是盲人或视力非常有限的人。

你可以在这里看一点[对屏幕阅读器的介绍。](https://abilitynet.org.uk/factsheets/introduction-screen-readers)

别这样，⬇️:

```
<h1>Coding is fun</h1>
<h3>It is always better when you have fun coding</h3>
<h5>Consistency is Key</h5>
```

做这个代替⬇️:

```
<h1>Can coding be fun?</h1>
<h2>The more you code the better you become</h2>
<h3>Coding is fun</h3>
```

### 使用 figure 元素在 HTML 中为图像添加标题

给图片添加标题时，建议使用`<figure>`元素。重要的是将`<figcaption>`元素和`<figure>`元素一起使用。

别这样，⬇️:

```
<div>
<img src=“a-man-coding.jpg” alt=“A man working on his computer”>
<p>This is a picture of a man working on his computer</p>
</div>
```

上面的例子将按预期工作，但不是最好的方法。在图像加载失败的情况下，屏幕上会显示`alt`文本和`<p>`元素上的文本。使用屏幕阅读器的网络访问者很难区分`<p>`和`alt`文本。

永远记住，仅仅因为你的代码工作并不意味着你遵循了最佳实践。

做这个代替⬇️:

```
<figure>
<img src=“a-man-coding.jpg” alt=“A man working on his computer”>
<figcaption> This is a picture of a man working on his computer</figcaption>
</figure>
```

上面的例子是给图片添加标题的最好方法。

以这种方式为图像添加标题很重要，因为:

*   搜索引擎优化:在搜索引擎上更容易找到你的图片。
*   使用屏幕阅读器的 web 访问者会更容易理解您的网页内容。

### 不要使用 div 来创建页眉和页脚，而是使用语义元素

HTML 语义元素在网页上以更有意义的方式标记文档的结构。最佳实践是使用 HTML 语义元素来正确组装您的网页。

避免使用`<divs>`来代替 HTML 语义。不要使用`<div>`元素在你的网页上显示页眉和页脚。使用语义的`<header>`和`<footer>`元素来代替。

`<header>`元素显示网页的导航或打开部分。

元素显示了网页的版权信息或导航链接。

别这样，⬇️:

```
<div class="header">
<a href="index.html">Home</a>
<a href="#">About</a>
<a href="#">Contact</a>
</div>

<div class="footer">
<a href="index.html">Home</a>
<a href="#">About</a>
<a href="#">Contact</a>
</div>
```

在上面的例子中，我们使用了`<div>`标签作为`<header>`和`<footer>`的容器。以这种方式编码是可行的，但是尽管你会达到同样的目标，这并不是最佳实践。

做这个代替⬇️:

```
<header>
<h1></h1>
</header>

<footer>
<a href="index.html">Home</a>
<a href="#">About</a>
<a href="#">Contact</a>
</footer>
```

上面的例子是将`<footers>`和`<headers>`添加到你的网页的最好方法。

使用 HTML 语义元素添加`<footer>`和`<header>`很重要，因为:

*   为你的`header`和`footer`使用语义元素使得你的代码更容易阅读。

*   它为 web 访问者提供了更好的用户体验。使用屏幕阅读器的 web 访问者会更容易理解您的网页内容。

查看这篇文章，了解更多关于 HTML 语义元素的知识。

### 避免使用`<b>`和`<i>`来加粗和倾斜网页上的文本

`<b>`和`<i>`标签也被称为粗体和斜体标签。它们都用于在网页上突出显示文本中的单词。

你不应该用`<b>`和`<i>`来表示粗体和斜体，因为它们没有语义。使用`font-weight` CSS 属性或者使用`<strong>`和`<em>`标签。

您可以使用`<strong>`标签使网页上的文本变得重要。它突出显示或加粗网页上的文本。标签强调网页中的文本。它还像`<i>`标签一样以斜体显示文本。

别这样，⬇️:

```
<p><i>Code at your own pace</i><p>

<p><b>code at your own pace</b><p>
```

在上面的例子中，显示的文本将以粗体和斜体显示。对于使用屏幕阅读器的网络用户来说，这并不重要。它没有语义意义。

****html 5**规范说`<b>`和`<i>`标签应该只在没有其他标签可用时才使用。**

**做这个代替⬇️:**

```
 `<p><strong>Code at your own pace</strong><p>

<p><em>code at your own pace</em><p>`
```

### **不要将块级元素放在内联元素中**

**块级元素在网页上从新行开始。默认情况下，它们在网页上从行首延伸到行尾。如果不使用 CSS，您将无法向 block 元素内联添加更多内容。**

**`<p>`、`<h1>-<h6>`和`<div>`元素是块级元素的一些例子。**

**inline 元素覆盖网页上最小的区域。它们不会在网页上另起一行。**

**`<span>`、`<em>`和`<a>`元素是内联元素的一些例子。**

**不能将块元素放在内联元素中。**

**别这样，⬇️:**

```
`<a href="#" >

    <p> Visit freecodecamp </p>

</a>` 
```

 **您不能将`<p>`包装在`<a>`元素中，因为`<p>`是一个块级元素，而`<a>`是一个行内元素。

做这个代替⬇️:

```
<p>
Visit <a href="www.freecodecamp. org" target="_blank">FreecodeCamp</a> 
to learn Javascript
</p> 
```

上面的例子是在块级元素中嵌套行内元素的最佳方式。

值得注意的是:

*   块级元素不能嵌套在内联元素中。
*   内联元素可以嵌套在块级元素中。
*   内联元素和块级元素可以嵌套在块级元素中。

简单说明一下:在上面的例子中，嵌套意味着放在里面。所以我说不能嵌套，是指不能放在里面。

我希望您理解这三个用于嵌套元素的简单规则。

也可以使用 CSS 将块级元素转换为行内元素，反之亦然。使用`display: inline-block`和`display: inline`从块级转换为行内元素。

重要的是要记住，仅仅因为你的代码工作并不意味着你遵循了最佳实践。

这就是为什么我总是推荐使用 [W3C 标记验证服务](https://validator.w3.org/)来仔细检查你的代码。

这个验证器检查 HTML、XHTML、SMIL、MathML 等 web 文档的标记有效性: [W3c 标记验证服务](https://validator.w3.org/)。

您可以通过复制代码的 URL 并将其粘贴到网站上或上传 HTML 文件来仔细检查您的代码。

## 结论

我希望这篇文章能帮助你了解一些关于 HTML 最佳实践的东西。我试图只包括最有用的提示，这样你就可以马上开始使用它们了！

如果您有任何其他问题或意见，请随时在 Twitter: [@cessss_](http://www.twitter.com/cessss_) 和 LinkedIn: [Success](https://www.linkedin.com/in/success-eriamiantoe/) 联系我

我会尽快回复的！感谢您的阅读💙。**