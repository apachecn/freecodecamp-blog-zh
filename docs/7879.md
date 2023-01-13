# 如何通过添加一行 CSS 来使你的 HTML 具有响应性

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-html-responsive-by-adding-a-single-line-of-css-2a62de81e431/>

在这篇文章中，我将教你如何使用 CSS Grid 来创建一个超级酷的图像网格，它可以根据屏幕的宽度来改变列数。

而最美的部分:**响应度会加上一行 CSS。**

这意味着我们不必用难看的类名(即`col-sm-4`、`col-md-8`)来混淆 HTML，也不必为每个屏幕尺寸创建媒体查询。

如果你想学习建立一个专业水平的响应式网站，你可以考虑参加 Scrimba 的响应式网页设计训练营，因为它通过 15 个小时的互动教程带领学生从初级到高级。

现在让我们开始吧！

### 设置

对于本文，我们将继续使用我在第一篇 CSS 网格文章中使用的网格。然后我们会在文章末尾添加图片。下面是我们的初始网格:

![1*fJNIdDiScjhI9CZjdxv3Eg](img/5c22b3dde8f5bbfc892454c1a1a41834.png)

这是 HTML:

```
<div class="container">  
  <div>1</div>  
  <div>2</div>  
  <div>3</div>  
  <div>4</div>  
  <div>5</div>  
  <div>6</div>  
</div> 
```

和 CSS:

```
.container {  
    display: grid;  
    grid-template-columns: 100px 100px 100px;  
    grid-template-rows: 50px 50px;  
} 
```

注意:这个例子也有一些基本的样式，我不会在这里深入讨论，因为它与 CSS 网格无关。

如果这段代码让你困惑，我推荐你阅读我的[在 5 分钟内学会 CSS 网格](https://medium.freecodecamp.org/learn-css-grid-in-5-minutes-f582e87b1228)的文章，在那里我解释了布局模块的基础。

让我们从提高色谱柱的响应能力开始。

### 分数单位的基本响应度

CSS Grid 带来了一个全新的值，叫做分数单位。分数单元的写法类似于`fr`，它允许你将容器拆分成任意多的分数。

让我们把每一列的宽度改为一个分数单位。

```
.container {  
    display: grid;  
    grid-template-columns: 1fr 1fr 1fr;  
    grid-template-rows: 50px 50px;  
} 
```

这里发生的事情是，网格将整个宽度分成三部分，每一列占一个单位。结果如下:

![1*JgGPqT2AfFNDD8DhG2wPdQ](img/3483cabd57c8bc9cfaef8cbf097a7cc7.png)

如果我们将`grid-template-columns`值改为`1fr 2fr 1fr`，第二列的宽度将是其他两列的两倍。总宽度现在是四个分数单位，第二个单位占其中的两个，而其他单位各占一个。这是它的样子:

![1*cpfokc1HBgCwOTNhRU9SHg](img/6f9c91977068fd23f3b382937750a4c1.png)

换句话说，分数单位值使您可以非常容易地更改列宽。

### 高级响应能力

然而，上面的例子没有给出我们想要的响应，因为这个网格总是三列宽。我们希望网格根据容器的宽度改变列数。要做到这一点，你必须学习三个新概念。

#### 重复()

我们将从`repeat()`函数开始。这是指定列和行的更有效的方法。让我们将原始网格改为使用 repeat():

```
.container {  
    display: grid;  
    grid-template-columns: repeat(3, 100px);  
    grid-template-rows: repeat(2, 50px);  
} 
```

换句话说，`repeat(3, 100px)`等同于`100px 100px 100px`。第一个参数指定了需要多少列或多少行，第二个参数指定了它们的宽度，因此这将为我们提供与开始时完全相同的布局:

![1*fJNIdDiScjhI9CZjdxv3Eg](img/5c22b3dde8f5bbfc892454c1a1a41834.png)

#### 自动 it

然后是自动适应。让我们跳过固定数量的列，而是用`auto-fit`代替 3。

```
.container {  
    display: grid;  
    grid-gap: 5px;  
    grid-template-columns: repeat(auto-fit, 100px);
    grid-template-rows: repeat(2, 100px);  
} 
```

这会导致以下行为:

![1*vLZ9RD3dt0Q3hCieYfRuMg](img/de4c45b3277f62cc3fc763cdedc6a8b9.png)

网格现在根据容器的宽度改变列数。

它只是试图将尽可能多的 100 像素宽的列放入容器中。

然而，如果我们将所有列硬编码为精确的 100px，我们将永远得不到我们想要的灵活性，因为它们很少能加起来达到全宽。正如你在上面的 gif 上看到的，网格经常在右边留出空白。

#### 最小最大值()

为了解决这个问题，我们需要的最后一个要素叫做`minmax()`。我们就简单的用`minmax(100px, 1fr)`代替 100px。这是最终的 CSS。

```
.container {  
    display: grid;  
    grid-gap: 5px;  
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: repeat(2, 100px);  
} 
```

注意，所有的响应都发生在一行 CSS 中。

这会导致以下行为:

![1*1FOrkyNbaabo3_LJxcdDbg](img/8bb5e24e9ddbecf48911e41911121de2.png)

如你所见，这非常有效。`minmax()`功能定义了大于或等于最小**和小于或等于最大**的尺寸范围。

所以列现在总是至少 100 像素。但是，如果有更多的可用空间，网格会简单地将这些空间平均分配给每个列，因为这些列会变成一个分数单位，而不是 100 px。

#### 添加图像

现在最后一步是添加图像。这与 CSS 网格无关，但我们还是来看看代码。

我们将从在每个网格项目中添加一个图像标签开始。

```
<div>
    <img src="img/forest.jpg"/>
</div> 
```

为了使图像适合项目，我们将它设置为与项目本身一样宽和一样高，然后使用`object-fit: cover;`。这将使图像覆盖它的整个容器，如果需要，浏览器将裁剪它。

```
.container > div > img {  
    width: 100%;  
    height: 100%;  
    object-fit: cover;  
} 
```

其结尾如下:

![1*jCNANupl0ECRzF6cOLuWNw](img/338606545b9d47c1406324798569f96e.png)

就是这样！您现在已经知道了 CSS Grid 中最复杂的概念之一，所以给自己一点鼓励吧。

#### 浏览器支持

在结束之前，我还需要提一下浏览器支持。写这篇文章的时候， [93%的全球网站流量支持 CSS 网格](https://caniuse.com/#feat=css-grid)，而且还在攀升。越来越明显的是，网格正在变成前端开发人员的必备技能。很像几年前 CSS Flexbox 发生的事情。

如果你想一劳永逸地学习 Flexbox、Grid 和响应式设计，你应该看看 Scrimba 上的[响应式网页设计训练营。通过简单易懂的互动教程，你的初学者可以进入高级阶段。](https://scrimba.com/g/gresponsive?utm_source=freecodecamp.org&utm_medium=referral&utm_campaign=gresponsive_single_line_responsive)

![bootcamp-banner](img/d73d65bd22f73ba9a8d9d2e0e8942cf3.png)

[Click here to get to the advanced bootcamp.](https://scrimba.com/g/gresponsive?utm_source=freecodecamp.org&utm_medium=referral&utm_campaign=gresponsive_single_line_responsive)

* * *

感谢阅读！我叫 Per Borgen，是 Scrimba 的联合创始人，这是一个学习编程的互动学习平台。