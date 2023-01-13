# 网站可访问性最佳实践:网站的所有提示

> 原文：<https://www.freecodecamp.org/news/web-accessibility-best-practices-a11y-tips/>

这个简短的指南将提供如何在网站中实现可访问性的实例。

在学校期间，可访问性没有得到足够的重视，在 web 开发的现实世界中也没有得到足够的重视。我们希望这篇文章，以及其他许多文章，将鼓励开发者从现在开始创建可访问的站点。

获得如何做事的实际动手例子总是有帮助的。因此，本指南将关注我作为一名 web 开发人员在日常生活中遇到的真实例子。

### 跳过导航

为了让视觉障碍用户在您的网站上有愉快的体验，他们需要能够快速有效地访问内容。如果你从来没有通过屏幕阅读器浏览过网站，我建议你这样做。这是测试一个网站对于非视觉用户来说有多容易导航的最好方法。NVDA 是一个非常好的免费屏幕阅读器应用程序。如果您使用屏幕阅读器并发现它很有帮助，请考虑向开发团队捐款。屏幕阅读器可以从[nvaccess.org](https://www.nvaccess.org/download/)下载。

要允许视力有障碍的用户跳到网站的主要内容，并避免在所有主要导航链接中跳转:

1.  创建一个“跳过导航链接”,它直接位于开始的`body`标签下面。

```
<a tabindex="0" class="skip-link" href="#main-content">Skip to Main Content</a> 
```

添加`tabindex="0"`是为了确保链接在所有浏览器上都是键盘焦点。关于键盘可访问性的更多信息可以在 webaim.org 找到。
2。“跳过导航”链接需要使用 ID 标签与网页文档中的主 html 标签相关联。

```
<main id="main-content">
...page content
</main> 
```

1.  默认情况下隐藏“跳过导航”链接。这确保了当链接处于焦点时，该链接仅对视力正常的用户可见。
    用 CSS 为链接创建一个类。在我的例子中，我已经添加了类`skip-link`。

```
.skip-link {
position: absolute;
width: 1px;
height: 1px;
padding: 0;
overflow: hidden;
clip: rect(0, 0, 0, 0);
white-space: nowrap;
-webkit-clip-path: inset(50%);
clip-path: inset(50%);
border: 0;
}
.skip-link:active, .skip-link:focus {
position: static;
width: auto;
height: auto;
overflow: visible;
clip: auto;
white-space: normal;
-webkit-clip-path: none;
clip-path: none;
} 
```

默认情况下，这些 CSS 样式隐藏链接，只有当链接获得键盘焦点时才显示链接。欲了解更多信息，请访问[a11y 项目](http://a11yproject.com/posts/how-to-hide-content)和这篇[博客文章](http://hugogiraudel.com/2016/10/13/css-hide-and-seek/)。

### 可访问的标题结构

*   角色“banner”被添加到`header`标签中，以向屏幕阅读器表明该标签是最上面的部分。在 HTML5 中，`header`上的角色被贬低了，但是为了支持尽可能多的屏幕阅读器，还是应该添加。
*   当`header`元素是`body`元素的子元素时，这个角色被添加到该元素中。

```
<header role="banner">
</header> 
```

### 可访问的主要内容结构

*   角色“main”被添加到`main`标签中，以向屏幕阅读器表明该标签是`main`内容部分。需要在`main`上添加角色在 HTML5 中被贬低了，但是为了支持尽可能多的屏幕阅读器，还是应该添加。
*   当`main`元素是页面的主要内容部分时，该角色被添加到该元素中。如果有不止一个`main`元素，那么每个元素都需要一个`aria-labelledby`属性或者一个`aria-label`。
*   更多信息可在[https://www . w3 . org/TR/2017/NOTE-wai-aria-practices-1.1-2017 12 14/examples/landmarks/main . html](https://5d0c2cfff1d8938bf45c6427--freecodecamp-dev.netlify.com/guide/accessibility/W3C%20Website%20documentation%20for%20Role)查询。

```
<main role="main">
</main> 
```