# CSS 变量定义——什么是 CSS 变量，如何使用它们？

> 原文：<https://www.freecodecamp.org/news/what-are-css-variables-and-how-to-use-them/>

CSS 变量是自定义变量，可以在样式表中创建和重用。

在本文中，我将向您展示如何在`:root`伪选择器上创建 CSS 变量，以及如何使用`var()`函数访问它们。

## 如何创建 CSS 自定义变量

下面是定义自定义 CSS 变量的基本语法:

```
--css-variable-name: css property value;
```

最佳实践是在样式表的顶部定义所有变量。对于较大的项目，通常只为自定义颜色变量创建一个单独的文件，这样就可以在其他样式表中重用它们。

如果你想访问那个变量，那么你可以使用`var()`函数。下面是基本语法。

```
css property: var(--css-variable-name);
```

在本例中，我想创建自定义的背景和文本颜色变量，以便在整个样式表中重用。我将这些变量命名为`--main-bg-color`和`--main-text-color`。

```
 --main-bg-color: #000080;
  --main-text-color: #fff;
```

我将把这些变量放在代表 HTML 文档中根元素的`:root`伪选择器中。

```
:root {
  --main-bg-color: #000080;
  --main-text-color: #fff;
}
```

在我的`body`选择器中，我将使用`var()`函数引用这些变量。

```
body {
  background-color: var(--main-bg-color);
  color: var(--main-text-color);
}
```

下面是一个工作示例:

[https://codepen.io/jessica-wilkins/embed/preview/LYeoOmP?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=LYeoOmP](https://codepen.io/jessica-wilkins/embed/preview/LYeoOmP?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=LYeoOmP)

如果我想给页面添加更多的内容，我可以在样式表的其余部分重用这些变量，避免不必要的重复，如下所示:

```
.example-class-1 {
  background-color: #000080;
  display: flex;
  flex-direction: column;
}

.example-class-2 {
  font-size: 2.1rem;
  color: #fff;
}

.example-class-3 {
  background-color: #000080;
  color: #fff;
  border: 2px solid black;
} 
```

在第二个例子中，我在页面上有 6 个红色、绿色和蓝色的框。我首先为盒子创建了 HTML 标记。

```
<div class="box-container">
  <div class="box red-box">Box 1</div>
  <div class="box green-box">Box 2</div>
  <div class="box blue-box">Box 3</div>
</div>
<div class="box-container">
  <div class="box blue-box">Box 4</div>
  <div class="box green-box">Box 5</div>
  <div class="box red-box">Box 6</div>
</div>
```

然后，我创建了一组自定义的红色、绿色和蓝色变量。

```
:root {
  --maroon-red: #800000;
  --dark-green: #013220;
  --navy-blue: #000080;
  --white: #fff;
}
```

最后，我使用`var()`函数将这些变量应用到我的盒子中。

```
.red-box {
  background-color: var(--maroon-red);
}

.green-box {
  background-color: var(--dark-green);
}

.blue-box {
  background-color: var(--navy-blue);
}
```

下面是完整的代码和最终结果:

[https://codepen.io/jessica-wilkins/embed/preview/WNdBXXZ?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=WNdBXXZ](https://codepen.io/jessica-wilkins/embed/preview/WNdBXXZ?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=WNdBXXZ)

## 如何命名 CSS 变量

当提到命名变量时，您希望选择简短的描述性名称，以便其他开发人员知道这些变量是什么。

这是一个错误变量名的例子:

```
 background-color:var(--color);
```

什么是`--color`？

是某种红色吗？绿色？蓝色？还有别的吗？

是主背景色还是主文字色？

您不希望其他开发人员不得不回去查看 CSS 定义，以确定它应该是什么。

另一个例子是，如果您正在为不同的颜色创建自定义 CSS 变量，那么您可以选择以这种方式命名它们:

```
:root {
  --maroon-red: #800000;
  --light-red: #ff0000;
  --crimson-red: #e32636;
}
```

每个开发人员都有自己命名变量的方式。要记住的重要事情是为变量提供描述性的名称。

## 结论

CSS 变量是自定义变量，可以在样式表中创建和重用。

下面是定义自定义 CSS 变量的基本语法。

```
--css-variable-name: css property value;
```

如果你想访问那个变量，那么你可以使用`var()`函数。下面是基本语法。

```
css property: var(--css-variable-name);
```

每个开发人员都有自己命名变量的方式，但最好是创建简短的描述性名称。

我希望你喜欢这篇文章，并祝你的 CSS 之旅好运。