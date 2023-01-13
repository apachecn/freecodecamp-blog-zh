# 如何用 Bootstrap 5 从 jQuery 切换到普通 JavaScript

> 原文：<https://www.freecodecamp.org/news/bootstrap-5-vanilla-js-tutorial/>

Bootstrap 5 是一个免费的开源 CSS 框架，致力于快速响应、移动优先的前端 web 开发。

以防你不知道， [Bootstrap 5 alpha 已经正式推出](https://themesberg.com/blog/bootstrap/bootstrap-version-5-alpha-whats-new)。它取消了 jQuery 的依赖性，放弃了对 Internet Explorer 9 和 10 的支持，并为 Sass 文件、标记和新的实用 API 带来了一些令人惊叹的更新。

本教程将向您展示在使用最新版本的 Bootstrap 5 构建网站时，如何开始使用 VanillaJS 而不是 jQuery。

## 入门指南

您需要在项目中包含 Bootstrap 5。有几种方法可以做到这一点，但为了简单起见，我们将通过 CDN 来包含这个框架。

首先，让我们在项目文件夹中创建一个空白的`index.html`页面:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bootstrap 5 Vanilla JS tutorial by Themesberg</title>
</head>
<body>

</body>
</html>
```

在您的`<head>`标签的结尾之前包含`bootstrap.min.css`样式表:

```
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/5.0.0-alpha1/css/bootstrap.min.css" integrity="sha384-r4NyP46KrjDleawBgD5tp8Y7UzmLA05oM1iAEQ17CSuDqnUK2+k9luXQOfXJCJ4I" crossorigin="anonymous">
```

然后，在您的`<body>`标记结束之前，包含 Popper.js 库和主引导 JavaScript 文件:

```
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/5.0.0-alpha1/js/bootstrap.min.js" integrity="sha384-oesi62hOLfzrys4LxRF63OJCXdXDipiYWBnvTl9Y9/TRlw5xlKIEHpNyvvDShgf/" crossorigin="anonymous"></script>
```

好奇`integrity`和`crossorigin`属性是什么意思？以下是解释:

****完整性属性** :** 允许浏览器检查文件来源，以确保如果来源被操纵，代码永远不会被加载。

****Crossorigin 属性** :** 出现在使用‘CORS’加载请求时，这是不是从‘同源’加载时 SRI 检查的一个要求。

太好了！现在，我们已经成功地将最新版本的 Bootstrap 包含在我们的项目中。一个明显的区别是，我们不再需要 jQuery 作为依赖项，如果处于缩小状态，可以节省大约 82.54 KB 的带宽。

## 从 jQuery 转换到普通 JS

在我们开始本节之前，您应该知道，根据官方文档，在 jQuery 中使用 Bootstrap 5 仍然是可能的。

如果您使用 jQuery 的唯一原因是为了查询选择器，我们建议使用普通的 JavaScript，因为您可以像使用`$('#element')`一样使用`document.querySelector('#element')`做同样的事情。

第一步是创建一个 JavaScript 文件，并将其包含在 body 标记的末尾之前，但在其他两个 includes 之后:

```
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/5.0.0-alpha1/js/bootstrap.min.js" integrity="sha384-oesi62hOLfzrys4LxRF63OJCXdXDipiYWBnvTl9Y9/TRlw5xlKIEHpNyvvDShgf/" crossorigin="anonymous"></script>
<script src="js/app.js"></script>
```

那么实际上什么时候需要在 Bootstrap 5 中使用 Javascript 呢？框架中的某些组件只有通过 Javascript 初始化才能工作，比如工具提示、popovers 和 toasts。

此外，对于 modals、dropdowns 和 carousels 等组件，您可能希望能够基于用户操作或应用程序逻辑以编程方式更改它们。

### 通过普通 JavaScript 初始化工具提示

我们都喜欢工具提示，但是除非通过 JavaScript 初始化，否则它们不起作用。让我们首先在我们的`index.html`文件中创建一个工具提示元素:

```
<button id="tooltip" type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="Tooltip on top">
    Tooltip on top
</button>
```

将鼠标悬停在按钮上不会显示工具提示，因为默认情况下，它是 Bootstrap 的一个可选功能，需要使用 JavaScript 手动初始化。如果您想用 jQuery 来做，下面是它的样子:

```
$('#tooltip').tooltip();
```

使用普通 JS，您需要使用以下代码来启用工具提示:

```
var tooltipElement = document.getElementById('tooltip');
var tooltip = new bootstrap.Tooltip(tooltipElement);
```

上面的代码选择了具有惟一 id“tooltip”的元素，然后创建了一个 Bootstrap tooltip 对象。然后，您可以使用它来操作工具提示的状态，例如以编程方式显示或隐藏工具提示。

这里有一个关于如何通过方法显示/隐藏它的例子:

```
var showTooltip = true;
if (showTooltip) {
    tooltip.show();
} else {
    tooltip.hide();
}
```

如果您想启用所有工具提示，也可以使用以下代码:

```
var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-toggle="tooltip"]'));
var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
  return new bootstrap.Tooltip(tooltipTriggerEl)
});
```

这里发生的事情是，我们选择所有具有`data-toggle="tooltip"`属性和值的元素，并为每个元素初始化一个工具提示对象。它还将它们保存到 tooltipList 变量中。

### 使用折叠 JavaScript 方法切换元素的可见性

Bootstrap 上的折叠特性是通过数据属性或 JavaScript 显示和隐藏元素的另一种非常方便的方式。

这里有一个例子，当某个按钮被点击时，我们如何显示或隐藏一张卡片。让我们首先创建 HTML 标记:

```
<button id="toggleCardButton" type="button" class="btn btn-primary mb-2">Toggle Card</button>
    <div id="card" class="card collapse show" style="width: 18rem;">
        <img src="https://dev-to-uploads.s3.amazonaws.com/i/rphqzfoh2cbz3zj8m8t1.png" class="card-img-top" alt="...">
        <div class="card-body">
            <h5 class="card-title">Freecodecamp.org</h5>
            <p class="card-text">Awesome resource to learn programming from.</p>
            <a href="#" class="btn btn-primary">Learn coding for free</a>
        </div>
    </div>
```

所以我们创建了一个 id 为`toggleCardButton`的按钮和一个 id 为`card`的卡片。让我们从选择两个元素开始:

```
var toggleButton = document.getElementById('toggleCardButton');
var card = document.getElementById('card');
```

然后我们需要使用新选择的 card 元素创建一个可折叠的对象:

```
var collapsableCard = new bootstrap.Collapse(card, {toggle: false});
```

`toggle:false`标志的作用是在创建对象后保持元素可见。如果不存在，它将默认隐藏卡片。

现在我们需要为单击动作的按钮添加一个事件侦听器:

```
toggleButton.addEventListener('click', function () {
    // do something when the button is being clicked
});
```

最后，我们需要使用像这样初始化的 collapsable 对象来切换卡片:

```
toggleButton.addEventListener('click', function () {
    collapsableCard.toggle();
});
```

就是这样！现在，只要点击按钮，卡片就会显示/隐藏。当然，所有这些都可以使用 Bootstrap 的[数据属性特性](https://v5.getbootstrap.com/docs/5.0/components/collapse/#via-data-attributes)来完成，但是您可能希望根据应用程序中的 API 调用或逻辑来切换某些元素。

## 结论

如果您遵循了本教程，您现在应该能够使用最流行的 CSS 框架，而不需要在您的项目中使用 jQuery。这可以让你**保存高达 85 KB 的数据**，让你的网站更快！恭喜？

如果你喜欢这个教程，并且喜欢使用 Bootstrap 作为你的项目的 CSS 框架，我邀请你去看看我们在❤️主题公园制作的一些免费和高级的 Bootstrap 主题、模板和 UI 工具包