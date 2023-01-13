# 如何在新标签页中打开链接–HTML 目标空白属性解释

> 原文：<https://www.freecodecamp.org/news/how-to-open-a-link-in-a-new-tab/>

有时你会希望用户点击一个网站链接，并在新的浏览器标签中打开它。但是如何在 HTML 中实现呢？

在本文中，我将通过代码示例向您展示如何使用`target="_blank"`属性。我还会谈到什么时候应该考虑使用这个属性。

## 如何使用`target="_blank"`打开新的浏览器标签

`target="_blank"`属性用在开始锚标记中，如下所示:

```
<a href="website-link-goes-here" target="_blank">
```

当用户点击链接时，一个新的浏览器标签将自动打开该页面。

在这个例子中，我在一组段落标签中嵌套了一个链接，将人们引向 freeCodeCamp。

```
<p>To learn how to code for free, please visit <a href="https://www.freecodecamp.org/learn" target="_blank">freeCodeCamp.org</a></p>
```

当你点击 freeCodeCamp 链接时，它会为你打开一个新的浏览器标签。

[https://codepen.io/jessica-wilkins/embed/preview/zYRRdmQ?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYRRdmQ](https://codepen.io/jessica-wilkins/embed/preview/zYRRdmQ?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYRRdmQ)

如果我省略了`target="_blank"`属性，那么默认行为将是离开当前网页，直接进入链接，而不打开新的浏览器标签。

## 什么是`noopener`关键词？

`rel`属性中的`noopener`关键字主要用于安全原因，以防止恶意用户通过`[Window.opener](https://developer.mozilla.org/en-US/docs/Web/API/Window/opener)`属性篡改原始网页。如果恶意用户获得了对您的窗口对象的访问权限，那么他们可以将您的页面重定向到恶意的 URL。

您可以使用`noopener`关键字来帮助防止安全问题的发生。下面是`noopener`关键字的用法:

```
<a target="_blank" rel="noopener" href="https://devdocs.io/html/element/heading_elements">DevDocs documentation</a>
```

如果你想了解更多关于`rel=noopener`帮助解决的安全问题，那么请通读[这篇有帮助的文章](https://mathiasbynens.github.io/rel-noopener/)。

### 对关键字`noopener`的更新

2021 年，进行了一次更新，现代浏览器现在使用`target=_blank`属性将`rel=noopener`设置为任何链接。正如你在这个[我能使用表格](https://caniuse.com/rel-noopener)中看到的，`noopener`关键字被大多数浏览器支持，除了 Internet Explorer 11。

即使有了这次更新，很多开发者仍然会使用`target=_blank`属性将`rel=noopener`用于链接。

## 应该一直使用`target="_blank"`属性吗？

当用户点击一个链接时，默认行为是让该链接在他们当前所在的页面上打开，而不打开新的浏览器标签。在很多情况下，您不希望更改这种默认行为，因为用户已经习惯了这种行为。

您必须仔细考虑何时是使用`target="_blank"`属性的好时机。一个很好的例子是，用户正在处理一个页面，如果他们点击了一个链接，他们不想离开这个页面。

在这个例子中，我们链接到了 [DevDocs](https://devdocs.io/) 文档，因此用户可以停留在他们当前的页面上，并在新的选项卡上查找参考。

[https://codepen.io/jessica-wilkins/embed/preview/qBxxPdb?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=qBxxPdb](https://codepen.io/jessica-wilkins/embed/preview/qBxxPdb?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=qBxxPdb)

## 结论

如果希望用户点击一个链接打开一个新的浏览器标签，可以使用`target="_blank"`属性。

像这样在开始锚标记中使用了`target="_blank"`属性。

```
<a href="website-link-goes-here" target="_blank">
```

添加`rel`属性中的`noopener`关键字是为了防止恶意用户通过`[Window.opener](https://developer.mozilla.org/en-US/docs/Web/API/Window/opener)`属性篡改原始网页。

```
<a target="_blank" rel="noopener" href="link-goes-here">
```

您必须仔细考虑什么时候是使用`target="_blank"`属性的好时机，因为您不希望总是改变链接的默认行为。

我希望您喜欢这篇文章，并祝您的编程之旅好运。