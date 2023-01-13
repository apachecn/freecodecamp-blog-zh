# 如何用 CSS 网格构建日历

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-calendar-with-css-grid/>

用 CSS Grid 构建日历实际上相当容易。我想告诉你怎么做。

在本文结束时，您将创建以下内容:

![A calendar built with CSS Grid](img/d674d5de24efd1698dff95f8cccc6eb1.png)

# 创建 HTML

从图中可以看出，日历包含三个部分:

1.  月份指示器
2.  工作日/周末指示器
3.  日期本身

![Structure of the calendar](img/bf9f6c89496014ab364c81d564b46cbb.png)

构建 HTML 的最佳方式是选择感觉正确的内容。我们将根据这三个部分创建 HTML:

```
<div class="calendar">
  <div class="month-indicator">...</div>
  <div class="day-of-week">...</div>
  <div class="date-grid">...</div>
</div> 
```

您还应该能够看到，我们需要七列网格。

![Seven columns required for the grid](img/0988ce06733c5caa47c5d98e5926791c.png)

我们将把话题集中在`.day-of-week`和`.date-grid`上，因为我们只讨论网格。

## 构建网格

有两种方法可以创建 CSS 网格。

第一种方法是将`.day-of-week`和`.date-grid`中的元素合并到一个选择器中。如果我们这样做，我们可以在`display: grid`中设置选择器。如果我们这样做，HTML 看起来会是这样的:

```
<div class="grid">
  <!-- Day of week -->
  <div>Su</div>
  <div>Mo</div>
  <div>Tu</div>
  <div>We</div>
  <div>Th</div>
  <div>Fr</div>
  <div>Sa</div>

  <!-- Dates -->
  <button><time datetime="2019-02-01">1</time></button>
  <button><time datetime="2019-02-02">2</time></button>
  <button><time datetime="2019-02-03">3</time></button>
  <!-- ... -->
  <button><time datetime="2019-02-28">28</time></button>
</div> 
```

我不鼓励这种方法，因为 HTML 失去了它的结构意义。如果可能的话，我更喜欢将`.day-of-week`和`date-grid`作为独立的元素。这让我很容易阅读/理解我写的代码。

以下是我选择的 HTML 结构:

```
<div class="day-of-week">
  <div>Su</div>
  <div>Mo</div>
  <div>Tu</div>
  <div>We</div>
  <div>Th</div>
  <div>Fr</div>
  <div>Sa</div>
</div>

<div class="date-grid">
  <button><time datetime="2019-02-01">1</time></button>
  <button><time datetime="2019-02-02">2</time></button>
  <button><time datetime="2019-02-03">3</time></button>
  <!-- ... -->
  <button><time datetime="2019-02-28">28</time></button>
</div> 
```

用我提出的结构创建 CSS 网格的最好方法是使用 subgrid。不幸的是，大多数浏览器还不支持子网格。同时，最好的方法是创建两个独立的网格——一个用于`.day-of-week`，一个用于`.date-grid`。

`.day-of-week`和`.date-grid`都可以使用相同的七列网格。

```
/* The grid */
.day-of-week,
.date-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
} 
```

![1 Feb 2019 begins on a Friday](img/7809589c7878bd4b42efcb1d4b532b16.png)

## 推迟日期

2019 年 2 月始于一个星期五。如果我们希望日历是正确的，我们需要确保:

1.  2019 年 2 月 1 日星期五
2.  2019 年 2 月 2 日星期六
3.  2019 年 2 月 3 日是周日
4.  等等...

有了 CSS Grid，这部分就简单了。

CSS 网格的布局算法遵循以下规则(如果您没有将`grid-auto-flow`设置为`dense`):

1.  首先放置具有显式`grid-column`或`grid-row`的项目
2.  根据最后一项填写其余部分

这意味着:

1.  如果第一项落在第 6 列
2.  第二个项目将放在第 7 列。
3.  第三个项目将放在下一行的第 1 列(因为只有 7 列)。
4.  第四个项目将被放置在列 2 中，
5.  等等...

因此，如果我们将 2 月 1 日放在第六列(星期五)，其余的日期将被正确放置。

就这么简单！

```
/* Positioning the first day on a Friday */
.date-grid button:first-child {
  grid-column: 6;
} 
```

![1 Feb 2019 begins on a Friday](img/d674d5de24efd1698dff95f8cccc6eb1.png)

这里有一支笔供你使用:

见 [CodePen](https://codepen.io) 上 Zell Liew ( [@zellwk](https://codepen.io/zellwk) )的笔[用 CSS 网格](https://codepen.io/zellwk/pen/xNpKwp/)构建日历。