# 如何使用 HTML 在新标签页中打开链接

> 原文：<https://www.freecodecamp.org/news/how-to-use-html-to-open-link-in-new-tab/>

标签很棒，不是吗？它们让我们所有人都可以同时处理一堆在线任务。

标签现在如此普遍，以至于当你点击一个链接时，它很可能会在一个新的标签中打开。

如果你曾经想知道如何用你自己的链接做到这一点，你来对地方了。

## 锚定元件

要在 web 页面上创建一个链接，您需要将一个元素(文本、图片等等)包装在一个 anchor ( `<a>`)元素中，并将其`href`属性设置为您想要链接到的 URL。

```
<p>Check out <a href="https://www.freecodecamp.org/">freeCodeCamp</a>.</p>
```

查看 [freeCodeCamp](https://www.freecodecamp.org/) 。

如果您单击上面的链接，浏览器将在当前窗口或选项卡中打开该链接。这是每个浏览器的默认行为。

要在新标签页中打开链接，我们需要查看锚元素的其他属性。

## 目标属性

这个属性告诉浏览器如何打开链接。

要在新标签页中打开链接，只需将`target`属性设置为`_blank`:

```
<p>Check out <a href="https://www.freecodecamp.org/" target="_blank">freeCodeCamp</a>.</p>
```

现在，当有人点击链接时，它会在一个新的标签中打开，或者可能是一个新的窗口，这取决于此人的浏览器设置。

## `target="_blank"`的安全问题

我强烈建议您在使用`target`属性时总是将`rel="noreferrer noopener"`添加到 anchor 元素中:

```
<p>Check out <a href="https://www.freecodecamp.org/" target="_blank" rel="noopener noreferrer">freeCodeCamp</a>.</p>
```

这会产生以下输出:

查看 [freeCodeCamp](https://www.freecodecamp.org/) 。

属性设置你的页面和链接的 URL 之间的关系。将其设置为`noopener noreferrer`是为了防止一种被称为[的网络钓鱼](https://en.wikipedia.org/wiki/Tabnabbing)。

## 什么是 tabnabbing？

Tabnabbing，有时也称为反向 tabnabbing，是一种利用浏览器的默认行为和`target="_blank"`通过`window.object` API 获得对页面的部分访问权的攻击。

使用 tab 键，您链接到的页面可能会导致您的页面重定向到一个虚假的登录页面。大多数用户很难注意到这一点，因为焦点会在刚刚打开的标签上，而不是你的页面原来的标签。

然后当一个人切换回你页面的标签时，他们会看到虚假的登录页面，并可能输入他们的登录信息。

如果你有兴趣了解更多关于 tabnabbing 是如何工作的，以及坏演员可以利用这个漏洞做什么，请查看亚历克斯·尤马舍夫的文章和 T2·奥瓦斯普的文章。

如果你想看一个*安全* 的工作示例，请查看这个[页面](https://mathiasbynens.github.io/rel-noopener/)和它的 [GitHub repo](https://github.com/mathiasbynens/rel-noopener) 以获得更多关于漏洞和`rel`属性的信息。

## 概括起来

使用 HTML 在新标签页中打开链接很容易。您只需要一个具有三个重要属性的锚(`<a>`)元素:

1.  设置为您想要链接的页面的 URL 的`href`属性
2.  `target`属性设置为`_blank`，它告诉浏览器根据浏览器的设置在新的标签页/窗口中打开链接
3.  将`rel`属性设置为`noreferrer noopener`,以防止来自所链接页面的恶意攻击

同样，这里有一个完整的工作示例:

```
<p>Check out <a href="https://www.freecodecamp.org/" target="_blank" rel="noopener noreferrer">freeCodeCamp</a>.</p>
```

这会在浏览器中产生以下输出:

查看 [freeCodeCamp](https://www.freecodecamp.org/) 。

再次感谢您的阅读。快乐编码。