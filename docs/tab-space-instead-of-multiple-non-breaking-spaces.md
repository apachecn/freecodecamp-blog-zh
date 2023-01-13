# 为什么在 HTML 中应该使用制表符而不是多个不间断空格

> 原文：<https://www.freecodecamp.org/news/tab-space-instead-of-multiple-non-breaking-spaces/>

在 HTML 中插入空格有多种方法。最简单的方法是简单地在目标文本前后添加空格或多个`&nbsp;`字符实体。当然，这不是最干燥的方法。

相反，为了让您的代码易于维护并减少重复，您可以使用`<span>`和`<pre>`元素，以及一些 CSS:

## **使用`<span>`元素**

如何使用`<span>`控制文本间距的示例如下所示:

```
<p>Hello my name is <span class="tab"> James</p>
```

注意`<span>`标签是自动关闭的，这意味着它们不需要`/>`。

然后，您可以使用外部或内部样式赋予类`tab`一些属性。例如，以下代码将在外部样式表中工作:

```
.tab {
  padding-left: 2px;
}
```

您还可以给`<span>`一些内联样式的属性，如下所示。

或者，您可以赋予`<span>`与内联样式相同的属性，如下所示:

```
<p>Hello my name is <span style="padding-left: 2px;"> James</p>
```

## 使用`<pre>`元素

添加制表符的一个简单方法是将文本放在`<pre>`标签中。例如:

`<pre>`元素仅仅代表*预格式化的*文本。换句话说，将呈现内部文本中的任何空格或制表符。例如:

```
<pre>
function greeting() {
  console.log('Hello world!');
}
</pre>
```

请记住，您用这种方法使用的任何实际的制表符(不是一堆看起来像制表符的空格)可能会显得非常宽。这是因为`<pre>`元素的`tab-size`属性默认设置为 8 个空格。

您可以用一点 CSS 来改变这一点:

```
pre {
  tab-width: 2;
}
```

## 关于 HTML 的更多信息:

超文本标记语言(HTML)是一种用于构建在线文档的标记语言，是当今大多数网站的基础。

像 HTML 这样的标记语言允许我们

*   创建到其他文档的链接，
*   构建我们文档中的内容，以及
*   为我们文档的内容赋予上下文和含义。

HTML 文档有两个方面。它包含结构化信息(标记)和指向其他文档的文本链接(超文本)。

我们使用 [HTML 元素](https://guide.freecodecamp.org/html#)来构建页面。它们是语言的构造，在我们的文档中为浏览器提供了[结构](https://guide.freecodecamp.org/html#)和[含义](https://guide.freecodecamp.org/html#)，以及到互联网上其他文档的元素链接。

互联网最初是为了存储和呈现静态(不变)文档而创建的。上面讨论的 HTML 的各个方面在这些缺少所有设计和样式的文档中表现得非常完美。他们展示了包含其他文档链接的结构化信息。

HTML5 是 HTML 的最新版本或规范。万维网联盟(W3C)是开发万维网标准(包括 HTML 标准)的组织。随着网页和网络应用变得越来越复杂，W3C 更新了 HTML 的标准。

HTML5 引入了一大堆语义元素。虽然 HTML 有助于为我们的文档提供意义，但直到 HTML5 引入了[语义元素](https://guide.freecodecamp.org/html#)，它的潜力才被充分认识。

## 一个简单的 HTML 文档的例子

```
<!DOCTYPE html>
<html>
<head>
  <title>Page Title</title>
</head>
<body>

  <h1>My First Heading</h1>
  <p>My first paragraph.</p>

</body>
</html>
```

！DOCTYPE html:将这个文档定义为 HTML5

HTML:HTML 页面的根元素

head:元素包含关于文档的元信息

title:元素指定了文档的标题

body:元素包含可见的页面内容

h1:元素定义了一个大标题

元素定义了一个段落

## HTML 版本

自从网络出现以来，HTML 已经有了很多版本

*   HTML1991
*   HTML 2.01995
*   HTML 3.21997
*   HTML 4.011999X
*   HTML2000
*   HTML52014

#### **其他资源**

*   [HTML 元素](https://guide.freecodecamp.org/html/elements)
*   [语义 HTML](https://guide.freecodecamp.org/html/html5-semantic-elements)
*   [HTML 属性](https://guide.freecodecamp.org/html/attributes)