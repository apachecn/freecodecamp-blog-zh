# HTML 背景颜色–更改背景颜色教程

> 原文：<https://www.freecodecamp.org/news/html-background-color-change-bg-color-tutorial/>

当你创建网页时，你总是想创建独特的布局。你希望你的网页能吸引你的用户，并且不刺眼。

为了做到这一点，你可以选择背景色和文本色，这两种颜色可以很好地融合，互相补充。

默认情况下，您会注意到您的网页有一个透明的背景颜色，您可以将其更改为任何您想要的颜色。

例如，您可能希望在网页上创建一个深色模式功能，以便背景为深色，而文本为浅色。这有助于读者避免影响他们眼睛的刺目颜色。

在这篇文章中，你将学习如何用 HTML 和 CSS 改变网页的背景颜色。

## 我们过去如何改变背景颜色

在过去，在引入 HTML5 之前，一些基本的样式是由 HTML 处理的。

例如，当您想要更改页面的背景颜色时，您可以很容易地在开始的 body 标记中添加`bgcolor`属性，并将其设置为您喜欢的颜色值。这可能是它的十六进制代码或名称。

```
<body bgcolor="grey">

// Or

<body bgcolor="#808080"> 
```

然而，当 HTML5 被引入时，这个属性被贬低了。现在它被一个更好的替代物 CSS `background-color`属性所取代。这是有意义的，因为 HTML 是一种标记语言，而不是一种样式语言。处理造型的时候，最好用 CSS。

如果你急着想知道如何改变你的网页、div 和其他元素的背景颜色，那么这里就是:

```
// Using inline CSS
<body style="background-color: value;"> 
  // ... 
</body>

// Using internal/external CSS
selector {
  background-color: value;
} 
```

假设你有多余的时间。让我们快速开始吧。

## 如何在 HTML 中改变背景颜色

您可以使用 CSS 背景色属性来更改网页的颜色。该属性的工作方式与其他 CSS 属性类似，这意味着您可以使用它以三种方式设置页面样式:

*   在 HTML 标签中(内联样式)，
*   在 head 标签中的 style 标签内(内部样式)，
*   或者在专用的 CSS 文件中(外部样式)。

根据您的喜好，您可以将`background-color`属性设置为一个颜色名称、一个十六进制代码、一个 RGB 值，甚至一个 HSL 值。您不仅可以使用该属性来设计网页正文的样式，还可以设计 div、标题、表格等等。

在 CodePen 中查看以下示例:

[https://codepen.io/olawanlejoel/embed/preview/BaxKLdd?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaxKLdd](https://codepen.io/olawanlejoel/embed/preview/BaxKLdd?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaxKLdd)

[https://codepen.io/olawanlejoel/embed/BaxKLdd?default-tab=html%2Cresult](https://codepen.io/olawanlejoel/embed/BaxKLdd?default-tab=html%2Cresult)

See the Pen [Background-color](https://codepen.io/olawanlejoel/pen/BaxKLdd) by Olawanle Joel ([@olawanlejoel](https://codepen.io/olawanlejoel)) on [CodePen](https://codepen.io).

## 如何用内联 CSS 改变 HTML 中的背景颜色

内联 CSS 允许你直接应用样式到你的 HTML 元素。这意味着你将 CSS 直接放入 HTML 标签中。使用 style 属性可以做到这一点，该属性保存了您希望应用于 HTML 标记的所有样式。

```
<body style="...">
  // ...
</body> 
```

您将在首选颜色值旁边使用 CSS 背景色属性:

```
// Color Name Value
<body style="background-color: skyblue">
  // Hex Value
  <div style="background-color: #87CEEB">
    // RGB Value
    <h1 style="background-color: rgb(135,206,235)">
      // ...
    </h1>

    // HSL Value
    <span style="background-color: hsl(197, 71%, 73%)">
      // ...
    </span>
  </div>
</body> 
```

## 如何用内部/外部 CSS 改变 HTML 中的背景颜色

样式化网页的最好方法是外部样式化，但是当你只有几行样式的时候，你总是可以使用内部样式化。

内部和外部都使用相同的方法:它们都使用选择器为 HTML 元素添加样式。

对于内部样式，所有样式都被添加到 HTML 文件的`<style>`标签中。这个样式标签被放在`<head>`标签中，如下所示:

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      selector {
        background-color: value;
      }
    </style>
  </head>

  // ...

</html> 
```

对于外部样式，您只需使用以下通用语法将 CSS 样式添加到您的 CSS 文件中:

```
selector {
  background-color: value;
} 
```

选择器可以是你的 HTML 标签，也可以是一个`class`或者一个`ID`。例如:

```
// HTML
<div> 
  <h1> Welcome to freeCodeCamp! </h1>
</div>

// CSS
div {
  background-color: skyblue;
} 
```

或者你可以使用一个`class`:

```
// HTML
<div class="container"> 
  <h1> Welcome to freeCodeCamp! </h1>
</div>

// CSS
.container {
  background-color: skyblue;
} 
```

或者你可以使用一个`id`:

```
// HTML
<div id="container"> 
  <h1> Welcome to freeCodeCamp! </h1>
</div>

// CSS
#container {
  background-color: skyblue;
} 
```

**注意:**正如您之前看到的，使用内联 CSS，您可以在内部或外部样式中使用颜色名称、十六进制代码、RGB 值和 HSL 值。

## 包扎

在本文中，您已经学习了如何使用 CSS 背景色属性来更改 HTML 元素的背景色。您还了解了在引入带有`bgcolor`属性的 HTML5 之前，开发人员是如何做的。

重要的是要记住，用内部或外部样式化 HTML 元素总是比内联样式化更可取，因为它提供了更多的灵活性。例如，您可以为所有的`<div>`标签元素使用一个 CSS `class`，而不是添加相似的内联样式。

内联样式不被认为是最佳实践，因为它们会导致大量重复——您不能在其他地方重用这些样式。要了解更多，你可以阅读我关于 HTML 内联样式的文章。你还可以在这篇文章中学习如何[改变文本大小](https://www.freecodecamp.org/news/how-to-change-text-size-in-html/)，以及如何在这篇文章中改变[文本颜色](https://www.freecodecamp.org/news/how-to-change-text-color-in-html/)。

我希望这篇教程能给你改变 HTML 文本颜色的知识，让它看起来更好。

祝编码愉快！