# CSS 网格布局

> 原文：<https://www.freecodecamp.org/news/css-grid-layout/>

## **网格布局**

CSS 网格布局，简称 Grid，是 CSS 中最新最强大的一种布局方案。所有主流浏览器都支持它，它提供了一种在页面上定位和移动项目的方法。

它可以自动将项目分配到*区域*，调整它们的大小，根据您定义的模式创建列和行，并使用新引入的`fr`单元进行所有计算。

### **为什么网格？**

*   你可以很容易地用一行 CSS 创建一个 12 列的网格。`grid-template-columns: repeat(12, 1fr)`
*   网格可以让你向任何方向移动物体。与 Flex 不同，在 Flex 中你可以水平(`flex-direction: row`)或垂直(`flex-direction: column`)移动项目——不能同时移动两者，Grid 允许你将任何*网格项目*移动到页面上任何预定义的*网格区域*。您移动的项目不必相邻。
*   有了 CSS Grid，你可以 ****只使用 CSS**** 改变 HTML 元素的顺序。将某些东西从顶部移到右边，将页脚中的元素移到侧边栏等等。不用将 HTML 中的`<div>`从`<footer>`移动到`<aside>`，只需在 CSS 样式表中用`grid-area`改变它的位置。

### **网格与伸缩**

*   Flex 是一维的——水平或垂直，而 Grid 是二维的，这意味着您可以在水平和垂直平面上移动元素
*   在 Grid 中，我们将布局样式应用于父容器，而不是项目。另一方面，Flex 以 flex 项为目标来设置属性，如`flex-basis`、`flex-grow`和`flex-shrink`
*   Grid 和 Flex 并不相互排斥。您可以在同一个项目中使用这两种方法。

### **检查浏览器与`@supports`** 的兼容性

理想情况下，当你建立一个网站时，你应该用 Grid 设计它，并使用 Flex 作为后备。你可以用`@support` CSS 规则(又名特征查询)来发现你的浏览器是否支持网格。这里有一个例子:

```
.container {
  display: grid; /* display grid by default */
}

@supports not (display: grid) { /* if grid is not supported by the browser */
  .container {
    display: flex; /* display flex instead of grid */
  }
}
```

### **入门**

要使任何元素成为网格，您需要将它的`display`属性赋给`grid`，就像这样:

```
.conatiner {
  display: grid;
}
```

仅此而已。你刚刚做了一个网格。`.container`中的每一个元素都会自动变成一个网格项目。

### **定义模板**

行和列

```
grid-template-columns: 1fr 1fr 1fr 1fr;
grid-template-rows: auto 300px;
```

区域

```
grid-template-areas: 
  "a a a a"
  "b c d e"
  "b c d e"
  "f f f f";
```

或者

```
grid-template-areas:
  "header header header header"
  "nav main main sidebar";
```

### **网格区域**

下面是一些关于如何定义和分配网格区域的示例代码

```
.site {
  display: grid;
  grid-template-areas: /* applied to grid container */
    "head head" /* you're assigning cells to areas by giving the cells an area name */
    "nav  main" /* how many values kind of depends on how many cells you have in the grid */
    "nav  foot";
}

.site > header {
  grid-area: head;
}

.site > nav {
  grid-area: nav;
}

.site > main {
    grid-area: main;
}

.site > footer {
    grid-area: foot;
}
```

### **`fr`单位**

网格引入了一个新的`fr`单位，代表*分数*。使用`fr`单元的好处是它会为您处理计算。使用`fr`可以避免边距和填充问题。有`%`和`em`等。计算`grid-gap`时就变成了数学方程式。如果你使用了`fr`单元，它会自动计算列和装订线的大小，并相应地调整列的大小，而且在末尾也不会有出血的间隙。

### **例题**

#### **根据屏幕尺寸改变元素的顺序**

假设你想在小屏幕上把页脚移到底部，在大屏幕上移到右边，在这两者之间有一堆其他的 HTML 元素。

简单的解决方法是根据屏幕尺寸改变`grid-template-areas`。您也可以根据屏幕尺寸 ****改变列数和行数**** 。这是 Bootstrap 的网格系统(`col-xs-8 col-sm-6 col-md-4 col-lg-3`)的一个更干净、更简单的替代方案。

```
.site {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-areas:
    "title title"
    "main header"
    "main sidebar"
}

@media screen and (min-width: 34em) { /* If the screen is big enough, use a different template for grid areas */
  .site {
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-areas:
      "title title title"
      "main header header"
      "main sidebar footer"
  }
}
```

参见 [CodePen](https://codepen.io/) 上 Aamnah Akram ( [@aamnah](https://codepen.io/aamnah) )的 Pen [CSS Grid by example - 2(网格面积+网格间隙)](https://codepen.io/aamnah/pen/RLVVoE/)。