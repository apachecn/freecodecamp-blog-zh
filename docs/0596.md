# 刷新 JavaScript-JS 重新加载窗口教程中的页面

> 原文：<https://www.freecodecamp.org/news/refresh-the-page-in-javascript-js-reload-window-tutorial/>

当您开发像博客或页面这样的应用程序时，其中的数据可能会根据用户操作而改变，您会希望该页面经常刷新。

当页面刷新或重新加载时，它将显示基于这些用户交互的任何新数据。好消息是——您可以用一行代码在 JavaScript 中实现这种功能。

在本文中，我们将学习如何在 JavaScript 中重新加载网页，并了解其他一些我们可能希望实现这些重新加载的情况以及如何实现。

## 如何用`location.reload()`刷新 JavaScript 中的页面

您可以使用`location.reload()` JavaScript 方法来重新加载当前的 URL。该方法的功能类似于浏览器的刷新按钮。

`reload()`方法是负责页面重载的主要方法。另一方面，`location`是一个接口，表示它链接到的对象的实际位置(URL)——在本例中是我们想要重新加载的页面的 URL。可以通过`document.location`或`window.location`进入。

以下是重新加载页面的语法:

```
window.location.reload(); 
```

**注意:**当你通读“JavaScript 中的页面重新加载”的一些资源时，你会遇到各种解释，说明 reload 方法接受布尔值作为参数，`location.reload(true)`帮助强制重新加载以绕过其缓存。但事实并非如此。

根据 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/Location/reload)，布尔参数不是当前`location.reload()`规范的一部分——事实上*从未*成为任何已发布的`location.reload()`规范的一部分。

另一方面，Firefox 之类的浏览器支持使用非标准的布尔参数`forceGet`来表示`location.reload()`，该参数指示 Firefox 绕过其缓存并强制重新加载当前文档。

除了 Firefox，您在其他浏览器的`location.reload()`调用中指定的任何参数都将被忽略，没有任何效果。

## 当点击按钮时，如何在 JavaScript 中执行页面重新加载/刷新

到目前为止，我们已经看到了重载在 JavaScript 中是如何工作的。现在，让我们来看看当一个事件发生时，或者当一个像点击按钮这样的动作发生时，如何实现这一点:

```
<button type="button" onClick="window.location.reload()">
   Reload Page
</button> 
```

**注意:**这与我们使用`document.location.reload()`时的工作方式类似。

## 如何在 JavaScript 中自动刷新/重新加载页面

我们也可以使用如下所示的`setTimeOut()`方法允许页面在固定时间后重新刷新:

```
setTimeout(() => {
  document.location.reload();
}, 3000); 
```

使用上面的代码，我们的网页将每 3 秒钟重新加载一次。

到目前为止，我们已经看到了如何在 HTML 文件中将 reload 方法附加到特定事件上，以及如何在 JavaScript 文件中使用它。

## 如何使用 JavaScript 中的历史功能刷新/重新加载页面

历史功能是刷新页面的另一种方法。history 函数通常用于通过传入正值或负值来向后或向前导航。

例如，如果我们想回到过去，我们会使用:

```
history.go(-1); 
```

这将加载页面，并把我们带到我们导航到的上一页。但是如果我们只想刷新当前页面，我们可以通过不传递任何参数或者传递`0`(一个中性值)来做到这一点:

```
history.go();
history.go(0); 
```

**注意:**这也和我们在 HTML 中添加`reload()`方法到`setTimeOut()`方法和点击事件的方式一样。

## 包扎

在本文中，我们学习了如何使用 JavaScript 刷新页面。我们还澄清了一个常见的误解，它导致人们将布尔参数传递给`reload()`方法。

感谢阅读！