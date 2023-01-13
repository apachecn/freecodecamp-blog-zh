# 帮助您记住 CSS 自定义属性的备忘单

> 原文：<https://www.freecodecamp.org/news/css-customs-properties-cheatsheet-c86778541f7d/>

CSS 自定义属性也称为 CSS 变量，表示可以在 CSS 中声明和调用的自定义属性。

### 在 CSS 中声明自定义属性

要在 CSS 中声明自定义属性，需要使用`--`语法:

```
:root { --colorPrimary: hsla(360, 100%, 74%, 0.6); }
```

注意`:root`伪类选择器——我们可以使用它全局声明变量。我们也可以使用其他选择器来声明它们，然后在这些选择器中确定它们的作用域。

```
.theme-dark { --colorPrimary: hsla(360, 100%, 24%, 0.6); }
```

### 在 CSS 中使用自定义属性

要在 CSS 中使用 CSS 自定义属性，可以使用`var()`函数:

```
:root { --colorPrimary: tomato; } 
.theme-dark { --colorPrimary: lime; } body { background-color: var(--colorPrimary); }
```

在这种情况下，`body`的背景颜色为`tomato`，而`body.theme-dark`的背景颜色为`lime`。

### 使用不带单位的自定义属性

如果 CSS 自定义属性与`calc()`函数一起使用，它们可以不带单位声明。

```
:root { --spacing: 2; } 
.container { 
  padding: var(--spacing) px; /*Doesn't Work ?*/ 
  padding: calc(var(--spacing) * 1rem); /*Will output 2rem ?*/ 
}
```

## 在 JavaScript 中使用自定义属性

要获得自定义属性，我们可以使用以下方法:

```
getComputedStyle(element).getPropertyValue("--my-var"); 
// Or if inline 
element.style.getPropertyValue("--my-var");
```

要更新自定义属性值，请执行以下操作:

```
element.style.setProperty("--my-var", newVal);
```

获取和替换值的示例:

在下面的示例中，我们使用 [dat.gui 控制器库](https://workshop.chromeexperiments.com/examples/gui/)来更改- scenePerspective、-cuberrotatey 和-cuberrotatex 自定义属性的值。这种方法使得应用新的样式更加容易，因为您不必对每个 DOM 元素应用内联样式。

感谢阅读！

### 资源

[定义自定义属性:–*系列属性](https://drafts.csswg.org/css-variables/#defining-variables)

[自定义属性:CSS 变量— MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)

[var() — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/var)

[使用 CSS 自定义属性(变量)— MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)

你可以在我的博客上阅读更多我的文章[。](https://vinceumo.github.io/devNotes/css/2019/02/20/css-customs-properties.html)