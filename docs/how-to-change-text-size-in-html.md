# HTML 字体大小–如何用 HTML 标签改变文本大小

> 原文：<https://www.freecodecamp.org/news/how-to-change-text-size-in-html/>

当您使用 HTML 标签将文本添加到 HTML 文件中时，您不会总是希望文本保持默认大小。您将希望能够调整文本在浏览器中的显示方式。

在本文中，您将学习如何用 HTML 标签改变文本大小。

在继续之前，有必要知道只有一种方法可以做到这一点:通过 CSS 的`font-size`属性。我们可以通过内联、内部或外部样式来使用`font-size`属性。

过去，我们可以在不使用 CSS 的情况下调整 HTML 标签中的文本大小。但那是在 HTML5 之前。然后我们使用`<font>`标签添加文本，它可以接受如下所示的大小属性:

```
<font size="5">  
    Hello World 
</font> 
```

这个大小属性可以接受 1-7 的值，其中文本大小从 1 增加到 7。但就像我说的，这个早就贬值了，大多数人甚至不知道它的存在。

如果你急着想知道如何改变你的文本大小，那么这里就是:

```
// Using inline CSS
<h1 style="font-size: value;"> Hello World! </h1>

// Using internal/external CSS
selector {
    font-size: value;
} 
```

假设你不着急。让我们简单地开始吧。

## 如何用内联 CSS 改变文本大小

内联 CSS 允许您将样式应用于特定的 HTML 元素。这意味着我们将 CSS 直接放入 HTML 标签中。我们使用 style 属性，它保存了我们所有的样式。

```
<h1 style="...">Hello World!</h1> 
```

我们使用`font-size`属性和我们的值，通过内联 CSS 来改变文本大小。这个值可以使用任何您喜欢的 CSS 单位，比如 em、px、rem 等等。

```
<h1 style="font-size:4em; "> Hello World! </h1>
<p style="font-size:14px; "> Any text whose font we want to change </p> 
```

完美的语法应该是:

```
<TEXT-TAG style="font-size:value;"> ... </TEXT-TAG> 
```

## 如何用内部或外部 CSS 改变文本大小

在内部和外部 CSS 样式中更改文本大小的方法是相似的，因为您使用了选择器。一般的语法是:

```
selector {
    font-size: value;
} 
```

选择器可以是我们的 HTML 标签，也可以是一个类或一个 ID。例如:

```
// HTML
<p> Any text whose font we want to change </p>

// CSS
p {
    font-size: 14px;
} 
```

或者我们可以用一个类:

```
// HTML
<p class="my-paragraph" > Any text whose font we want to change </p>

// CSS
.my-paragraph {
    font-size: 14px;
} 
```

## 包扎

在本文中，您了解了如何使用 CSS 来更改 HTML 元素的字体/文本大小。你也看到了在 HTML5 推出之前，开发者是怎么做的。

此外，请记住，使用内部或外部样式来设计 HTML 元素总是更好，因为与内联样式相比，它提供了很大的灵活性。

例如，您可以为所有的 p 标记使用一个 CSS 类，而不必为所有的 p 标记元素添加内联样式。

使用内联样式不被认为是最佳实践，因为它会导致大量的重复——您不能在其他地方重用这些样式。要了解更多，你可以阅读我关于 HTML 内联样式的文章。

我希望这篇教程能给你改变 HTML 文本大小的知识，这样你就能让它看起来更好。

祝编码愉快！