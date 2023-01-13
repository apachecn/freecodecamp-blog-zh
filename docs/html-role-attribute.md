# HTML 角色属性解释

> 原文：<https://www.freecodecamp.org/news/html-role-attribute/>

属性描述了一个元素在程序中的作用，比如屏幕阅读器或放大镜。

使用示例:

```
<a href="#" role="button">Button Link</a>
```

屏幕阅读器会把这个元素读成“按钮”而不是“链接”。

有四类角色:

*   抽象角色
*   小部件角色
*   文档结构角色
*   标志性角色

## 关于 HTML 属性的更多信息:

[<脚本 src >属性](https://guide.freecodecamp.org/html/attributes/script-src-attribute/)

[< a href >属性](https://guide.freecodecamp.org/html/attributes/a-href-attribute/)

[<一个目标>属性](https://guide.freecodecamp.org/html/attributes/a-target-attribute/)

[<身体背景>属性](https://guide.freecodecamp.org/html/attributes/body-background-attribute/)

[< p 对齐>属性](https://guide.freecodecamp.org/html/attributes/p-align-attribute/)

[< img src >属性](https://guide.freecodecamp.org/html/attributes/img-src-attribute/)

[<字体>属性](https://guide.freecodecamp.org/html/attributes/font-color-attribute/)

## 关于 HTML 属性的更多信息

HTML 元素可以有属性，属性包含元素的附加信息。

HTML 属性通常以名称-值对的形式出现，并且总是出现在元素的开始标记中。属性名表示您提供的关于元素的信息类型，属性值是实际的信息。

例如，HTML 文档中的锚(`<a>`)元素创建到其他页面或页面其他部分的链接。在开始的`<a>`标签中使用`href`属性告诉浏览器链接将用户发送到哪里。

下面是一个将用户发送到 freeCodeCamp 主页的链接示例:

```
<a href="www.freecodecamp.org">Click here to go to freeCodeCamp!</a>
```

请注意，属性名(`href`)和值(“www.freeCodeCamp.org”)用等号分隔，值用引号括起来。

有许多不同的 HTML 属性，但大多数只对某些 HTML 元素有效。例如，`href`属性如果放在开始的`<h1>`标签中就不起作用。

在上面的例子中，提供给`href`属性的值可以是任何有效的链接。但是，有些属性只有一组可用的有效选项，或者值需要采用特定的格式。属性告诉浏览器 HTML 元素中内容的默认语言。属性`lang`的值应该使用标准语言或国家代码，比如`en`代表英语，或者`it`代表意大利语。

## **布尔属性**

有些 HTML 属性不需要值，因为它们只有一个选项。这些被称为布尔属性。标签中属性的存在将把它应用于 HTML 元素。然而，写出属性名并将其设置为值的一个选项也是可以的。在这种情况下，该值通常与属性名相同。

例如，表单中的`<input>`元素可以有一个`required`属性。这要求用户在提交表单之前填写该项目。

以下是做同样事情的例子:

```
<input type="text" required >
<input type="text" required="required" >
```