# 内联 CSS 指南——如何直接设计 HTML 标签的样式

> 原文：<https://www.freecodecamp.org/news/inline-css-guide-how-to-style-an-html-tag-directly/>

您已经编写了一些 HTML，现在需要用 CSS 对其进行样式化。一种方法是使用内联样式，这正是本文所要讨论的。

```
<p style="color: red; font-size: 20px;">This is my first paragraph.</p> 
```

在我们深入探讨内联样式的细微差别——何时、为何以及如何使用它们——之前，了解其他样式化 HTML 的方法是很重要的。这样，您就为您的代码选择了最佳选项。

这是你的选择汇总。

### 外部样式表

开发人员通常将他们所有的 CSS 保存在一个外部样式表中。在 HTML 文件中，使用 **`<link>`** 元素链接到包含 CSS 的外部样式表。

```
<head>
    <link rel="stylesheet" href="./index.css">
</head> 
```

在 index.css 文件中，我们有自己的 css 规则。

```
p {
    color: red;
    font-size: 20px;
} 
```

### 内部样式表

样式化 CSS 的另一个选择是使用内部样式表。这意味着在 HTML 文件的 **`<style>`** 元素中定义 CSS 规则。

```
<head>  
   <style>
       p {
           color: red;
           font-size: 20px;
       }
   </style>
 </head> 
```

### 内嵌样式

偶尔，你会发现自己在追求内联样式。但是了解它们仍然很重要，因为在某些情况下它们会派上用场。

使用内联样式，您可以将 style 属性添加到 HTML 标签中，然后使用 CSS 来设置元素的样式。

```
<p style="color: red; font-size: 20px;">This is my first paragraph.</p>
<p>This is my second paragraph.</p> 
```

所以在我们的例子中，第一段的文本是红色的，字体大小为 20px。然而，第二个保持不变。

让我们仔细看看如何以及何时使用内联样式。我们还将揭示为什么只有一个段落是有样式的。

# 什么是 HTML 标签？

使用内联样式，您可以将 CSS 应用于开始 HTML 标记中的`style`属性。

HTML 标签的例子包括:

*   ...
*   # ...

*   ...

开始和结束标签通常是 HTML [元素](https://developer.mozilla.org/en-US/docs/Glossary/element)的一部分，它可以包含文本、数据、图像或者什么都不包含。

这里，我们有一个文本元素。

```
<p>This is my first paragraph.</p> 
```

我们可以使用内联样式来设置这个元素的样式，方法是将样式属性添加到开始标记，后面跟着 CSS 属性-值对。

```
<body>
   <p style="color: red; font-size: 20px;">This is my first paragraph.</p>
   <p>This is my second paragraph.</p>
</body> 
```

让我们回顾一下我们是如何使用内联样式的。

# 如何使用内嵌样式

将样式属性添加到您想要设置样式的标签中，后跟一个等号。用双引号开始和结束你的 CSS。

```
<p style="...."> 
```

向样式属性添加属性-值对。在每个属性-值对后添加一个分号。

```
color: red; font-size: 20px; 
```

所以当我们把所有东西放在一起时，它看起来像这样:

```
<p style="color: red; font-size: 20px;">This is my first paragraph.</p> 
```

## 需要了解的要点

与内部和外部样式表不同，内联样式不包含花括号或换行符。也就是说，当使用内联样式时，把你的 CSS 都写在同一行。

此外，请记住，内联样式*仅*影响您添加带有 CSS 属性-值对的样式属性的特定元素。

例如，在*下面的代码中，只有*的第一段是红色的，字体大小为 20px。

```
<body>
   <p style="color: red; font-size: 20px;">This is my first paragraph.</p>
   <p>This is my second paragraph.</p>
</body> 
```

如果我们想用内嵌样式来样式化两个段落的文本，那么我们也需要将 CSS 添加到第二个 **`<p>`** 的样式属性中。

```
<body>
  <p style="color: red; font-size: 20px;">This is my first paragraph.</p>
  <p style="color: red; font-size: 20px;">This is my second paragraph.</p>
</body> 
```

然而，如果我们使用外部样式表，例如，我们可以通过使用一个 CSS 选择器轻松地样式化两个段落，而不需要重复我们的代码。

```
p {
    color: red;
    font-size: 20px;
} 
```

这给我们带来了一个重要的话题:什么时候使用，什么时候不使用内联样式。

# 何时使用(以及何时不使用)内联样式

假设您有一个包含十个或更多段落标签的 HTML 文件。你能想象用内联样式单独设计每一个吗？

这样做将很快使你的代码混乱，难以阅读和维护。

此外，如果您还使用内部或外部样式表，内联样式可能会引入特异性问题。

那是因为内嵌样式有一个[高特异性](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#Understanding_the_cascade)。这意味着它们将覆盖内部和外部样式表中的大多数其他规则，除了`!important`声明。

例如，我们向两个段落元素添加了内联样式。我们还添加了一个内部样式表。

```
<html>
  <head>
      <title>My New Webpage</title>
      <style>
       p {
           color: pink;
       }
   </style>
  </head>

<body>
   <p style="color: blue;">A blue paragraph.</p>
   <p style="color: blue;">Another blue paragraph.</p>
</body>
</html> 
```

内联样式中的 CSS 覆盖了内部样式表中的 CSS。所以我们以两个蓝色的段落结束。

当您或其他人需要做出更改时，外部样式表也更容易维护。这是因为外部或内部样式表的样式可以应用于多个元素，而内联样式必须单独应用于每个元素。

例如，假设您需要将一个样式更新为十个元素。在外部样式表中进行一次更改比在 HTML 文件中进行十次更改更容易。

一般来说，将 CSS 放在一个单独的文件中通常是最佳实践。这样，你的 HTML 文件包含了网站的结构和内容，CSS 文件包含了你的风格。这样做可以使您的代码更容易阅读和管理。

但是，有时使用内联样式可能是有意义的:

*   添加一个样式，很快就能看到变化，这对[测试](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/style)很有用。
*   使用 JavaScript 中的 style 属性来应用样式。

大多数情况下，您会希望使用外部样式表。但是您偶尔会发现自己在使用内联样式，最常见的是在上述情况下。

我在[amymhaddad.com](https://amymhaddad.com/)的博客上写了关于学习编程的文章，以及学习编程的最佳方法。