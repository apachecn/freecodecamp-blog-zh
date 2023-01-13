# 如何在 HTML 字体样式教程中更改文本颜色

> 原文：<https://www.freecodecamp.org/news/how-to-change-text-color-in-html/>

文本在我们的网页上扮演着重要的角色。这是因为它帮助用户了解网页是关于什么的，以及他们可以在那里做什么。

当您向网页添加文本时，该文本默认为黑色。但有时你会想改变文字的颜色，使其更加个性化。

例如，假设你有一个更深的颜色作为你网站的背景。在这种情况下，你会希望文本颜色变得更浅、更亮，以提高网站的可读性和可访问性。

在这篇文章中，你将学习如何改变 HTML 文本的颜色。我们会看各种方法，我们会讨论哪种方法是最好的。

## 如何更改 HTML5 之前的文本颜色

在引入 HTML5 之前，你可以使用`<font>`向网站添加文本。该标签采用`color`属性，该属性接受颜色作为名称或十六进制代码值:

```
<font color="#9900FF"> Welcome to freeCodeCamp. </font>

// Or

<font color="green"> Welcome to freeCodeCamp. </font> 
```

当 HTML5 被引入时，这个标签被贬低了。这是有意义的，因为 HTML 是一种标记语言，而不是一种样式语言。当处理任何类型的样式时，最好使用 CSS，它具有样式的主要功能。

这意味着你要给你的网页添加颜色，你需要利用 CSS。

如果你急着想知道如何改变文字的颜色，那么下面就是:

```
// Using inline CSS
<h1 style="color: value;"> Welcome to freeCodeCamp! </h1>

// Using internal/external CSS
selector {
    color: value;
} 
```

假设你不着急。让我们简单地开始吧。

## 如何在 HTML 中改变文本颜色

您可以使用 CSS color 属性来更改文本颜色。该属性接受十六进制代码、RGB、HSL 或颜色名称等颜色值。

例如，如果您想将文本颜色改为天蓝色，您可以使用名称`skyblue`、十六进制代码`#87CEEB`、RGB 十进制代码`rgb(135,206,235)`或 HSL 值`hsl(197, 71%, 73%)`。

有三种方法可以用 CSS 改变文本的颜色。它们使用内联、内部或外部样式。

### 如何用内联 CSS 改变 HTML 中的文本颜色

内联 CSS 允许你直接应用样式到你的 HTML 元素。这意味着你将 CSS 直接放入 HTML 标签中。

您可以使用 style 属性，该属性包含您希望应用于该标签的所有样式。

```
<p style="...">Welcome to freeCodeCamp!</p> 
```

您将在首选颜色值旁边使用 CSS 颜色属性:

```
// Color Name Value
<p style="color: skyblue">Welcome to freeCodeCamp!</p>

// Hex Value
<p style="color: #87CEEB">Welcome to freeCodeCamp!</p>

// RGB Value
<p style="color: rgb(135,206,235)">Welcome to freeCodeCamp!</p>

// HSL Value
<p style="color: hsl(197, 71%, 73%)">Welcome to freeCodeCamp!</p> 
```

但是如果你的应用变得更大更复杂，内联样式并不是最好的选择。所以让我们看看你能做些什么。

### 如何用内部或外部 CSS 改变 HTML 中的文本颜色

另一种改变文本颜色的首选方法是使用内部或外部样式。这两个非常相似，因为它们都使用选择器。

对于内部样式，您可以在 HTML 文件的`<head>`标签中完成。在`<head>`标签中，您将添加`<style>`标签，并将您所有的 CSS 样式放在那里，如下所示:

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      selector {
        color: value;
      }
    </style>
  </head>

  // ...

</html> 
```

而对于外部样式，您只需使用以下通用语法将 CSS 样式添加到您的 CSS 文件中:

```
selector {
  color: value;
} 
```

选择器可以是你的 HTML 标签，也可以是一个`class`或者一个`ID`。例如:

```
// HTML
<p> Welcome to freeCodeCamp! </p>

// CSS
p {
  color: skyblue;
} 
```

或者你可以使用一个`class`:

```
// HTML
<p class="my-paragraph" > Welcome to freeCodeCamp! </p>

// CSS
.my-paragraph {
   color: skyblue;
} 
```

或者你可以使用一个`id`:

```
// HTML
<p id="my-paragraph" > Welcome to freeCodeCamp! </p>

// CSS
#my-paragraph {
   color: skyblue;
} 
```

**注意:**正如您在前面看到的，使用内联 CSS，您可以在内部或外部样式中使用颜色名称、十六进制代码、RGB 值和 HSL 值。

## 包扎

在本文中，您已经学习了如何使用 CSS 改变 HTML 元素的字体/文本颜色。您还了解了在引入带有`<font>`标签和颜色属性的 HTML5 之前，开发人员是如何做的。

此外，请记住，用内部或外部样式设计 HTML 元素总是比内联样式更可取。这是因为它提供了更多的灵活性。

例如，您可以对所有的标签元素使用一个 CSS `class`,而不是添加相似的内联样式。

内联样式不被认为是最佳实践，因为它们会导致大量重复——您不能在其他地方重用这些样式。要了解更多，你可以阅读我关于 HTML 内联样式的文章。你还可以在这篇文章中学习如何改变[文本大小](https://www.freecodecamp.org/news/how-to-change-text-size-in-html/)和[背景颜色](https://www.freecodecamp.org/news/html-background-color-change-bg-color-tutorial/)。

我希望这篇教程能给你改变 HTML 文本颜色的知识，让它看起来更好。

祝编码愉快！