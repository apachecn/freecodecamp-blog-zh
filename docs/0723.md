# CSS 函数–如何使用 calc()、max()、min()和 clamp()

> 原文：<https://www.freecodecamp.org/news/css-functions-for-beginners/>

在 CSS 中有许多不同的度量单位。你有 px 和百分比，vh，vw，em，rem，等等。

总有一天，你会想通过组合两个或多个不同的单位来得到一个值。CSS 有一个函数可以用来进行这样的计算。在本教程中，您将了解它是如何工作的。

这些相对单位还可以使用其他功能，如`max`、`min`和`clamp`，当遇到不同的值时，使用合适的值。这些在响应式布局中非常有用，可以替代媒体查询。

如果你学会恰当地使用它们，你可以避免在使用媒体查询和更少的代码来调整窗口大小时可能发生的布局变化！

## 如何使用 CSS `calc()`函数

calc 函数只接受一个参数。该参数可以是任意长度单位与四个数学运算符`+`、`-`、`/`、`*`组合的表达式。

您也可以使用括号来表示不同于标准优先规则的评估顺序。

在`calc()`函数内部的表达式中，你可以使用 CSS 变量、用`[attr()](https://developer.mozilla.org/en-US/docs/Web/CSS/attr)`获得的值以及来自`max()`、`min()`和`clamp()`函数的值。

`calc()`允许您从复杂参数中计算一个值。

```
div {
    width: calc(100% - 2em);
}
```

**注意**:数学运算符两边都要留一个空格。

## 如何使用 CSS `max()`函数

`max`函数接受逗号分隔的值列表，并返回最大的值。每个值也可以是一个表达式(任何可以用作`calc()`函数参数的东西也可以是这个函数的参数之一)。

max 函数可以看做是为某个事物确定一个*最小值*的一种方式。

这个函数的一个用例是使文本具有响应性，同时给尺寸一个最小值。

例如:

```
h1 {
    font-size: max(1rem, 10vh);
}
```

这样，文本将是视口高度的十分之一，除非视口高度变得太小。文本将始终具有至少为`1rem`的`font-size`以确保易读性。

## 如何使用 CSS `min()`函数

与 max 函数一样，`min`可以接受任意数量的参数，包括其他的`max`、`min`或`clamp`函数，并返回它们之间的最小值。

min 函数可以被视为确定一个最大值。

例如，假设您正在创建一个表单，并且希望它在屏幕宽度改变时也能响应。你需要给它一个最大的宽度，以避免在最大的屏幕上出现的水平拉伸的外观。

你可以这样写:

```
.form {
    width: min(600px, 90vw);
}
```

页面的宽度将等于视窗宽度的 90%,或 600 像素宽，取最小值。因此，如果视窗宽度大于 670 像素，窗体将不会水平拉伸。

## `min()`和`max()`的例子:

你可以在这支笔中看到代码片段。尝试在水平和垂直方向上调整笔的大小，并观察表单宽度和字体大小的相应变化。

[https://codepen.io/nethleen/embed/preview/XWZMGVd?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=XWZMGVd](https://codepen.io/nethleen/embed/preview/XWZMGVd?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=XWZMGVd)

## 如何使用 CSS `clamp()`函数

箝位功能将值箝位在上限和下限之间。它在限定的范围内选择一个值。

`clamp`取三个值。第一个是最小值，第二个是首选值，第三个是最大值。

箝位功能将返回首选值，除非该值小于最小值(在这种情况下，它将返回最小值)或大于最大值(在这种情况下，它将返回最大值)。

您可以使用`clamp`让布局元素在一个范围内相应地调整大小。

```
h1 {
    font-size: clamp(1rem, 10vw, 2rem);
}

div {
    padding: clamp(10px, 6vw, 50px);
    width: clamp(140px, 90vw, 600px);
}
```

## `clamp`功能示例:

看看这支笔的实际效果吧！水平调整笔的大小，并查看各种元素如何改变尺寸。

[https://codepen.io/nethleen/embed/preview/ExQWJZo?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=ExQWJZo](https://codepen.io/nethleen/embed/preview/ExQWJZo?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=ExQWJZo)

MDN 提供了关于所有这些功能的更多详细信息:

*   [calc](https://developer.mozilla.org/en-US/docs/Web/CSS/calc)
*   [最大](https://developer.mozilla.org/en-US/docs/Web/CSS/max)
*   [分钟](https://developer.mozilla.org/en-US/docs/Web/CSS/min)
*   [夹钳](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp)

至此，你已经大致了解了这四种神奇的功能。你已经学得足够开始使用它们了，所以出去玩吧！