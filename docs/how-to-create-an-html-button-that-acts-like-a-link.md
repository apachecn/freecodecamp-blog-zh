# 如何创建一个像链接一样的 HTML 按钮

> 原文：<https://www.freecodecamp.org/news/how-to-create-an-html-button-that-acts-like-a-link/>

有时你可能想用一个按钮链接到另一个页面或网站，而不是提交一个表格或类似的东西。这很容易做到，可以通过几种方式实现。

## 如何使用 A 标签中的 Button 标签创建一个 HTML 按钮

一种方法是简单地将您的`<button>`标签包装在`<a>`标签中:

```
<a href='https://www.freecodecamp.org/'><button>Link To freeCodeCamp</button></a>
```

这将把整个按钮转换成一个链接。

## 如何用 CSS 把链接变成按钮

第二种选择是像平常一样使用`<a>`标签创建链接，然后通过 CSS 对其进行样式化:

```
<a href='https://www.freecodecamp.org/'>Link To freeCodeCamp</a>
```

一旦你创建了链接，你可以使用 CSS 使它看起来像一个按钮。例如，你可以添加一个边框，一个背景色，当用户悬停链接时的一些样式。

你可以[在这里](https://www.freecodecamp.org/news/a-quick-guide-to-styling-buttons-using-css-f64d4f96337f/)阅读更多关于 CSS 样式链接的内容。

## 如何使用 HTML 在表单中放置一个按钮

添加按钮的另一种方法是将`input`包装在`form`标签中。在表单操作属性中指定所需的目标 URL。

```
<form action="http://google.com">
    <input type="submit" value="Go to Google" />
</form>
```

我希望你发现教程是有帮助的。快乐编码。