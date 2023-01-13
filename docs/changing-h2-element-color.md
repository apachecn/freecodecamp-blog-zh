# 更改 H2 元素颜色

> 原文：<https://www.freecodecamp.org/news/changing-h2-element-color/>

在编码中，一个给定的问题通常有许多不同的解决方案。在设计 HTML 元素的样式时尤其如此。

最容易改变的事情之一是文本的颜色。但有时你尝试的东西似乎都不起作用:

```
<style>
  h2 .red-text {
    color: red;
  }
</style>

<h2 class="red-text" color=red;>CatPhotoApp</h2>
```

那么，如何将 H2 元素的颜色改为红色呢？

### 选项 1:内联 CSS

一种方法是直接使用内联 CSS 来设计元素的样式。

与其他方法一样，格式化也很重要。再看一下上面的代码:

```
<h2 class="red-text" color=red;>CatPhotoApp</h2>
```

要使用内联样式，请确保使用`style`属性，并将属性和值用双引号(")括起来:

```
<h2 class="red-text" style="color: red;">CatPhotoApp</h2>
```

### 选项 2:使用 CSS 选择器

您可以将 HTML 或者页面的结构和内容与样式或者 CSS 分开，而不是使用内联样式。

首先，去掉内联 CSS:

```
<style>
  h2 .red-text {
    color: red;
  }
</style>

<h2 class="red-text">CatPhotoApp</h2>
```

但是你需要小心你使用的 CSS 选择器。看一看`<style>`元素:

```
h2 .red-text {
  color: red;
}
```

当有一个空格时，选择器`h2 .red-text`告诉浏览器以类`red-text`的元素为目标，类`red-text`是`h2`的子元素。但是，H2 元素没有子元素——您正在尝试设计 H2 元素本身的样式。

要解决此问题，请删除空格:

```
h2.red-text {
  color: red;
}
```

或者直接针对`red-text`类:

```
.red-text {
  color: red;
}
```