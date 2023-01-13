# HTML 备忘单–HTML 元素列表参考

> 原文：<https://www.freecodecamp.org/news/html-cheat-sheet-html-elements-list-reference/>

在本教程中，我们将讨论常用的 HTML 标签、元素和属性。我们还将看到这些标签、元素和属性如何工作的例子。

无论您是初学者还是有经验的开发人员，都可以将本文作为参考指南。

## HTML 文档是由什么组成的？

以下标签定义了基本的 HTML 文档结构:

*   `<html>`标签——该标签充当文档中除了`<!DOCTYPE *html*>`标签之外的所有其他元素的容器。
*   `<head>`标签–包括文档的所有元数据。
*   `<title>`标签–定义显示在浏览器标题栏中的文档标题。
*   标签——充当浏览器上显示的文档内容的容器。

这就是所有的样子:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title> My HTML Cheat Sheet </title>
  </head>
  <body></body>
</html> 
```

`<!DOCTYPE *html*>`指定我们正在处理一个 HTML5 文档。

以下标签为 HTML 文档提供了附加信息:

*   标签–该标签可用于定义网页的附加信息。
*   `<link>`标签–用于将文档链接到外部资源。
*   `<style>`标签–用于定义文档的样式。
*   标签–用于编写代码片段(通常是 JavaScript)或将文档链接到外部脚本。

```
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>My HTML Cheat Sheet</title>

    <style>
      * {
        font-size: 100px;
      }
    </style>

    <script>
      alert('This is an alert');
    </script>
 </head>
```

## HTML 文档结构

当您充实 HTML 文档时，您将使用某些标签来创建结构。

`<h1>`到`<h6>`标签显示文档中不同级别的标题，其中< h1 >最大，< h6 >最小。

```
<h1> Heading 1 </h1>
<h2> Heading 2 </h2>
<h3> Heading 3 </h3>
<h4> Heading 4 </h4>
<h5> Heading 5 </h5>
<h6> Heading 6 </h6>
```

您使用`<p>`标签创建一个段落。

```
<p> This is a paragraph </p>
```

标签可以用来划分和样式化文档的不同部分。它还充当其他元素的父容器。它是这样工作的:

```
<div class="newsSection">
  <h1> This is the news section </h1>
  <p> Welcome to the news section! </p>
</div>

<div class="contactSection">
  <h1> This is the contact us section </h1>
  <p> Hello world! </p>
</div>
```

我们还有`<span>`标签。这类似于`<div>`,但是您将它用作一个内嵌容器。

```
<p> I love <span class="keyword"> coding! </span></p>
```

有一个`<br/>`标签，我们用它来创建换行符。这没有结束标记。

```
<p> I love <br/> freeCodeCamp </p>
```

`<hr/>`标签用于创建一条水平线。它也没有结束标记。

## HTML 中的图像

在 HTML 中，我们使用`<img/>`标签来显示图像。

以下是`<img/>`标签的一些属性:

*   `src`用于指定图像在计算机或网络上的位置路径。
*   `alt`定义图像无法渲染时显示的替代文本。alt 文本对于屏幕阅读器也很有用。
*   `height`指定图像的高度。
*   `width`指定图像的宽度。
*   `border`指定边框的粗细，如果没有添加边框，则设置为 0。

```
<img src="ihechikara.png" alt="a picture of Ihechikara" width="300" height="300">
```

## HTML 中的文本格式

我们有很多方法来格式化 HTML 中的文本。现在让我们简要地回顾一下。

*   `<i>`以斜体显示文本。
*   `<b>`以粗体显示文本。
*   `<strong>`以粗体显示文本。用于强调重点。
*   另一个以斜体显示文本的强调标签。
*   `<sub>`定义下标文本，像 CO₂.中的两个氧原子
*   `<sup>`定义一个上标像一个数的幂，10。
*   `<small>`缩小文本尺寸。
*   `<del>`通过在文本上划线来定义删除的文本。
*   `<ins>`定义通常带下划线的插入文本。
*   `<blockquote>`用来括起引用自另一来源的一段文字。
*   `<q>`用于较短的行内引号。
*   `<cite>`用于引用引文的作者。
*   `<address>`用于显示作者的联系方式。
*   `<abbr>`表示缩写。
*   `<code>`显示代码片段。
*   `<mark>`高亮显示文本。

```
<p><i> italic text </i></p>
<p><b>bold text </b></p>
<p><strong> strong text </strong></p>
<p><em> strong text </em></p>
<p><sub> subscripted text </sub></p>
<p><sup> superscripted text </sup></p>
<p><small> small text </small></p>
<p><del> deleted text </del></p>
<p><ins> inserted text </ins></p>
<p><blockquote> quoted text </blockquote></p>
<p><q> short quoted text </q></p>
<p><cite> cited text </cite></p>
<p><address> address </address></p>
<p><abbr> inserted text </abbr></p>
<p><code> code snippet </code></p>
<p><mark> marked text </mark></p>
```

## 链接

`<a>`标签，也称为锚标签，用于定义链接到其他页面(包括外部网页)或同一页面的某个部分的超链接。

以下是`<a>`标签的一些属性:

*   `href`指定单击时链接将用户带到的 URL。
*   `download`指定点击的目标或资源是可下载的。
*   `target`指定链接打开的位置。这可以在同一个或单独的窗口中。

```
<a href="https://www.freecodecamp.org/" target="_blank"> Learn to code </a>
```

## 列表

标签定义了一个有序列表，而标签定义了一个无序列表。

`<li>`标签用于在列表中创建项目。

```
<!-- Unordered list -->
<ul>
  <li> HTML </li>
  <li> CSS </li>
  <li> JavaScrip t</li>
</ul>

<!-- Ordered list -->
<ol>
  <li> HTML </li>
  <li> CSS </li>
  <li> JavaScript </li>
</ol>
```

## 形式

标签用来在 HTML 中创建一个表单。表单用于获取用户输入。以下是与`<form>`元素相关的一些属性:

*   `action`指定提交时表单信息的位置。
*   `target`指定显示表单响应的位置。
*   `autocomplete`可以有开或关的值。
*   `novalidate`指定不应验证表单。
*   `method`指定发送表单数据时使用的 HTTP 方法。
*   `name`指定表格的名称。
*   `required`指定输入元素不能为空。
*   当页面加载时，将焦点放在输入元素上。
*   `disabled`禁用一个输入元素，这样用户就可以与它交互了。
*   `placeholder`用于提示用户输入元素需要什么信息。

以下是与表单相关的其他输入元素:

*   `<textarea>`用于获取多行用户文本输入。
*   `<select>`指定用户可以选择的选项列表。
*   `<option>`在选择元素下创建一个选项。
*   `<input>`指定用户可以输入数据的输入字段。它有一个`type`属性，指定用户可以输入什么类型的数据。
*   创建一个按钮。

```
<form action="/info_url/" method="post">

    <label for="firstName"> First name: </label>
    <input type="text" 
           name="firstName" 
           placeholder="first name" 
           required
    >

    <label for="lastName"> Last name: </label>
    <input type="text" 
           name="lastName" 
           placeholder="last name" 
           required
    >

    <label for="bio"> Bio: </label>
    <textarea name="bio"></textarea>

    <select id="age">
      <option value="15-18">15-18</option>
      <option value="19-25">19-25</option>
      <option value="26-30">26-30</option>
      <option value="31-36">31-36</option>
    </select>

    <input type="submit" value="Submit">

  </form>
```

## 桌子

*   标签定义了一个 HTML 表格。
*   `<thead>`指定表格中每一列的信息。
*   `<tbody>`指定表格的主体或内容。
*   `<tfoot>`指定表格的页脚信息。
*   `<tr>`表示表格中的一行。
*   `<td>`表示表格中的单个单元格。
*   `<th>`表示值列的标题。

```
<table>

  <thead>
    <tr>
      <th> Course </th>
      <th> Progress </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td> HTML </td>
      <td> 90% </td>
    </tr>
    <tr>
      <td> CSS </td>
      <td> 80% </td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td> JavaScript </td>
      <td> 95% </td>
    </tr>
  </tfoot>

</table>
```

## HTML5 中引入的标签

以下是 HTML5 中引入的一些标签:

*   `<header>`指定网页标题
*   `<footer>`指定网页页脚。
*   `<main>`指定主要内容部分。
*   指定文章的章节，通常用于网页上独立的内容。
*   `<section>`用于创建单独的章节。
*   `<aside>`通常用于在侧边栏中放置项目时。
*   `<time>`用于格式化日期和时间。
*   `<figure>`用于图表之类的数字。
*   `<figcaption>`表示对图形的描述。
*   `<nav>`用于嵌套导航链接。
*   `<meter>`用于测量一个范围内的数据。
*   `<progress>`用作进度条。
*   `<dialog>`用于创建一个对话框。
*   `<audio>`用于在网页中嵌入一个音频文件。
*   `<video>`用于嵌入视频。

```
 <header>
    <h1> Welcome </h1>
    <h3> Hello World! </h3>
  </header>

  <nav>
    <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#">Services</a></li>
        <li><a href="#">About us</a></li>
    </ul>
  </nav>

  <article>

    <h1> An article about us </h1> 
    <p> Article content </p>

    <aside>
      <p> It's sunny today </p>
    </aside>

  </article>

  Progress: <progress min="0" max="100" value="50"> </progress>

  <footer> Copyright 2022-2099\. All Rights Reserved. </footer>
```

## 结论

在本文中，我们看到了很多开发人员常用的 HTML 标记、元素和属性。这不是全部，但应该是一个很好的参考资源。

我希望这能对你有所帮助。感谢您的阅读。