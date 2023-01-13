# D3.js 教程——面向初学者的数据可视化

> 原文：<https://www.freecodecamp.org/news/d3js-tutorial-data-visualization-for-beginners/>

在这篇文章中，我将一步一步地向你介绍如何使用 D3.js，这是一种初学者友好的方式。

我们将讨论 D3.js 是什么，它是如何工作的，我们将创建一些基本的可视化来添加过渡、交互和缩放。

## 目录

*   [入门](#getting-started)
    D3 . js 是什么？
    如何设置 D3.js 环境
*   [选择](#selections)
    如何在 D3 中选择元素
    如何在 D3 中修改元素
*   [D3 是数据驱动](#data-driven)
    数据加入 D3
    数据载入 D3
*   [D3](#scales)中的刻度
*   [用 d3 中的 d3.js](#create-a-bar-chart-with-d3.js)
    轴组件创建条形图
    D3 边距约定
    如何用 D3 中的 CSS 对其进行样式化
*   [用 d3.js](#create-a-world-map-with-d3.js)
    创建世界地图如何在 D3.js
    地图中使用多个数据集用城市名
    事件处理用 D3.js
    地图用平移和缩放
    程序化缩放
    添加工具提示
*   [结论](#conclusion)

## 这篇文章是写给谁的？

这篇文章的目标读者是已经掌握 HTML、CSS、SVG 和 JavaScript 基础知识的开发人员，他们希望学习如何用 D3.js 可视化数据。

这篇文章既适合完全的初学者，也适合已经有一些使用 D3.js 的经验的人。

到本文结束时，您应该理解 D3.js 是如何工作的，以及如何用您的数据创建可视化。

## D3.js 入门

D3.js 是一个 JavaScript 库，用于在 web 上创建图表、地图等可视化内容。

> **D3.js** (也称为 **D3** ，是**数据驱动文档**的缩写)是一个 JavaScript 库，用于在 web 浏览器中生成动态、交互式[数据可视化](https://en.wikipedia.org/wiki/Data_visualization)。它利用了可缩放矢量图形(SVG)、HTML5 和级联样式表(CSS)标准。–维基百科

不像许多其他提供现成图表的数据可视化库，D3 给了你很多创作的自由，因为你可以完全控制你创建的可视化。D3 还使用了 HTML、CSS、SVG 和 JavaScript 等网络技术。

除了 D3 使用这些熟悉的技术之外，它还有其他几个好处:

*   D3 非常快，
*   它鼓励代码重用
*   它支持大型数据集，并提供了一种加载和转换数据的简单方法
*   这有利于创建具有丰富交互的可视化效果

### 如何设置一个 D3 环境

D3 可以在所有现代浏览器中工作，在撰写本文时，D3.js 在版本 7 (v7)上。

要使用 D3 的最新版本，您必须在您的网页上链接到它，如下所示:

```
<script src="https://d3js.org/d3.v7.min.js"></script>
```

然而出于教学的目的，本文中的所有例子都在 [Codepen](https://codepen.io) 上，所以你可以编辑真实的例子。

## 如何在 D3 中选择元素

当您用 JavaScript 编码并且需要修改页面上的元素时，您需要选择这些元素。D3.js 以同样的方式工作，并为我们提供了两种选择 DOM 元素的方法:

*   `d3.select()`
*   `d3.selectAll()`

这两个选择器方法都将接受任何 CSS 选择器，并返回与指定选择器匹配的元素。如果没有元素匹配选择器，它将返回一个空选择。

`d3.select()` 方法将选择 DOM 中第一个匹配的元素(从上到下)。

```
d3.select("#d3_p").style("color", "blue");
```

Example output

你好世界 1

如果有多个元素匹配指定的选择器，`d3.select()`将匹配它找到的第一个元素。

Example output

你好世界 1

你好世界 2

你好世界 3

你好世界 4

`d3.selectAll()`方法的工作方式与`d3.select()`非常相似——但是它选择所有与选择器匹配的元素:

```
d3.selectAll
(".d3_p").style("color", "blue");
```

Example output

你好世界 1

你好世界 2

你好世界 3

你好世界 4

### 如何在 D3 中修改元素

选择 DOM 元素后，D3 提供了以下方法来修改它们:

| 方法 | 使用 | 例子 |
| --- | --- | --- |
| `.attr()` | 更新所选元素属性 | `d3.select("p").attr("name", "fred")` |
| `..classed()` | 在选定的元素上分配或取消分配指定的 CSS 类名 | `d3.select("p").classed("radio", true);` |
| `.style()` | 更新样式属性 | `d3.select("p").style("color", "blue");` |
| `.property()` | 用于设置元素属性 | `d3.select('input').property('value', 'hello world')` |
| `.text()` | 更新选定元素的文本内容 | `d3.select('h1').text('Learning d3.js')` |
| `.html()` | 将内部 HTML 设置为所有选定元素的指定值 | `d3.select('div').html('h1>learning d3.js</h1>')` |
| `.append()` | 追加新元素作为选定元素的最后一个子元素 | `d3.select("div").append("p")` |
| `.insert()` | 与`.append()`方法相同，除了您可以指定另一个元素插入之前 | `d3.select("div").insert("p", "h1")` |
| `.remove()` | 从 DOM 中移除选定的元素 | `d3.select("div").remove("p")` |

不要担心所有这些不会马上有意义——我们将很快在我们的例子中使用所有这些方法。

上面的每个 DOM 操作方法都接受一个常量值或函数作为参数，这导致了创建**动态属性。**

该函数接受两个属性:第一个是习惯上在 d3.js 中称为`d`的数据，另一个是`index`。

```
d3.selectAll("circle").attr('cx', ((d, i) => i * 100))
```

Example output

正如您在上面看到的，在这个函数中，我们可以应用任何逻辑来操作数据和输出。

## D3 是数据驱动的

D3.js 本身是数据驱动的，也就是说它的超能力是从数据中获得的。D3 支持不同类型的数据，比如数组、CSV、XML、TSV、JSON 等等。

这些数据可以来自工作目录中的本地文件，也可以从 API 中获取。

### D3 中的数据连接

D3 的数据连接允许我们将指定的数据连接到选定的元素。要创建数据连接，可以使用`.data()`方法:

```
let fruits = ['Apple', 'Orange', 'Mango']

d3.selectAll(".d3_fruit").data(fruits).text((d) => d)

// html

<p class="d3_fruit"></p>
```

Example output

让我们看看这里发生了什么，为什么我们只有一个输出，而不是三个。

到目前为止，我们已经:

1.  水果系列中的 3 个数据点
2.  我们选择的 1 `p`元素

D3 只是将数组中的第一个水果(苹果)分配给它得到的唯一选择`p`,并忘记其余的。

对此的一个快速解决方法是手动创建其他的 2 p 元素，然后继续你的生活。但是大多数时候，您实际上并不知道从外部 API 获取的数据数组中有多少项。

为了解决这个问题，最新版本的 D3 为我们提供了一个`.join()`方法。它根据需要追加、移除和重新排序元素，以匹配指定的数据。让我们用前面的例子来试试，看看会发生什么:

```
let fruits = ['Apple', 'Orange', 'Mango']

d3.select(".d3_fruit")
    .selectAll("p")
    .data(fruits)
    .join("p") // the join method
        .attr("class", "d3_fruit")
        .text((d) => d)

// html

<div class="d3_fruit"></div>
```

Example output

让我们稍微分解一下:

1.  选择`div`包装器`d3_fruit`
2.  选择所有的`p`元素，即使 div 中没有`p`元素——这将返回一个空选择
3.  `.data(fruits)` -将水果数组绑定到空选择
4.  `.join("p")` -这个方法为数组中的每一项创建所有的`p`元素
5.  `.attr("class", "d3_fruit")` -我们为创建的每个`p`元素设置了一个类
6.  `.text((d) => d)` -基于水果数组设置每个创建的`p`的文本

### D3 中的数据加载

我们已经看到了什么是 D3 的数据，以及如何将数据连接到我们的选择中。但是到目前为止我们只使用了我们自己创造的数据`let fruits = ['Apple', 'Orange', 'Mango']`。

在真实世界的场景中，情况通常不是这样——有时您必须从 API 或本地文件中获取数据。

D3 有一些加载不同类型文件的方法:

*   d3.json
*   d3.csv
*   d3.xml
*   d3.tsv
*   d3 .文本

使用这些方法时，语法通常是相同的:

```
// async await
const data = await d3.csv("/path/to/file.csv");
console.log(data);

// or
d3.json("/path/to/file.json").then((data) => {  console.log(data); })
```

让我们通过从一个实际的外部 JSON 文件加载数据来看看这一点。

对于这个例子，我有一个 JSON 文件，其中包含了尼日利亚及其所有州的所有信息:

```
const el = d3.select("#d3_svg_demo2");

d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/nigeria-states.json").then(({data}) => {
    el
     .selectAll("p")
     .data(data)
	 .join("p")
	  .text((d) => d.Name)
}); 
```

Example output

... + 31 others

使用上面的方法，你可以获取 D3 的任何数据。

## D3 比例

到目前为止，您已经学习了如何加载和使用 D3.js 中的数据。现在我们需要学习关于 **Scales** 的知识。 **T** 这对大多数人来说可能是最难学的部分，也是 D3 最重要的概念。

在我们刚刚看到的上一个例子中，我们从一个 API 加载 JSON 数据，对于尼日利亚的每个州，我们将名称附加到一个`p`元素上。该 JSON 文件还包含每个州的人口和一些其他信息。

每个州的人口从最低的`2 million`到最高的`16 million`。例如，为了在条形图上正确地表示数据，您需要创建一个高度为`16000000px`的条形图。

想象一下，你可能会同意这是一个很长的条形图。这就是`d3.scale`的用武之地。

`d3.scale`函数接收数据作为输入，并返回以像素为单位的视觉值。`d3.scale`需要设置一个**域**和一个**范围。**领域为我们试图可视化表示的数据设置了一个限制。

```
const x_scale = d3.scaleLinear()
    .domain([10, 500])
    .range([2000000, 16000000]);
```

让我们稍微分解一下:

*   我们告诉 D3 我们将使用 scaleLinear
*   `.domain([10, 500])` -我们设置域(限制)从 10 到 500
*   `.range([2000000, 16000000])` -我们将最小值设为 200 万，最大值设为 1600 万，这意味着我们将 200 万分配给`10px`，1600 万分配给`500px`

现在，如果我们有一个人口大约为`8000000`(1500 万的一半)的城市，它将映射为像素值`250px`(500 的一半)。

需要指出的是，D3 具有各种形式的[标度](https://github.com/d3/d3-scale)。您决定使用哪一个取决于您试图表示的数据类型。

*   当你处理代表日期的数据时，使用 [d3.scaleTime](https://github.com/d3/d3-scale#scaleTime)
*   当你创建条形图时，使用 [d3.scaleBand](https://github.com/d3/d3-scale#scaleBand)
*   对于其他刻度，请参考 [d3 .刻度](https://github.com/d3/d3-scale)

## 如何用 D3.js 创建条形图

现在让我们应用我们所学的一切，用 D3 创建一个真实世界的条形图。

对于此示例，我们将继续从本教程的数据加载部分的示例代码进行构建:

```
const el = d3.select("#d3_svg_demo2");

d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/nigeria-states.json").then(({data}) => {
    el
     .selectAll("p")
     .data(data)
	 .join("p")
	  .text((d) => d.Name)
}); 
```

首先，让我们为条形图创建刻度:

```
const width = 960, height = 500;
const x_scale = d3.scaleBand().range([0, width])
const y_scale = d3.scaleLinear().range([height, 0]) 
```

这是怎么回事:

*   首先，我们用最小值 0 和最大值 SVG 宽度来定义我们的 x 标度(水平标度)
*   其次，我们将 y 轴(垂直轴)设置为从 0 到 SVG 高度

接下来我们需要在文档中选择我们的 svg lemon:

```
const svg = d3.select("#d3_demo")
    .attr("width", width)
    .attr("height", height) 
```

这里我们选择了 SVG 元素，并将高度和宽度设置为我们指定的高度和宽度。接下来，让我们从 API 中获取 JSON 数据:

```
d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/nigeria-states.json").then(({ data }) => {
    data.forEach((d) => (d.Population = +d.info.Population))
})
```

如果这看起来不熟悉，请重新阅读数据加载部分。因为我们的 JSON 数据的构造方式，我从 API 中析构了`{ data }`。

提取的数据以字符串的形式出现，但是我们需要人口字段是一个数字。因此，我们使用 JavaScript `+`操作符将每个人口字段转换成一个数字:

```
data.forEach((d) => (d.Population = +d.info.Population))
```

接下来，我们需要设置秤的范围，现在我们已经获取了数据，可以这样做了:

```
x_scale.domain(data.map((d) => d.Name);
y_scale.domain([0, d3.max(data, (d) => d.Population)]);
```

让我们看看这里发生了什么:

*   `x_scale.domain(data.map((d) => d.Name)`-x 标度是一个波段标度，因此我们将域设置为州名(36 个州)
*   `y_scale.domain([0, d3.max(data, (d) => d.Population)])`-y 刻度是线性刻度，因此我们将最小值设置为 0。我们没有自己设置最大值，而是让 D3 通过使用`d3.max()`方法来为我们设置。

注意:使用`d3.max()`方法，我们遍历提供的数据并总是返回指定字段的最大值(在我们的例子中是人口)。

最后，我们需要添加矩形，这样我们就可以看到我们的条形图:

```
svg
 .selectAll("rect")
 .data(data)
 .join("rect")
  .attr("class", "bar")
  .attr("x", (d) => x_scale(d.Name))
  .attr("y", (d) => y_scale(d.Population))
  .attr("width", x_scale.bandwidth())
  .attr("height", (d) => height - y_scale(d.Population));
```

好吧，这不是什么新鲜事，对吧？如果这还是新的，请重新阅读本教程的数据连接部分。但是有些东西我们是第一次看到:

*   `.attr("x", (d) => x_scale(d.Name))` -我们根据生成的刻度设置每个`rect`的 x(水平)位置。y 轴也一样(垂直位置`.attr("y", (d) => y_scale(d.Population))`)。
*   `.attr("width", x_scale.bandwidth())` -这里我们设定每个`rect`的宽度。当然，我们可以将其设置为任何我们喜欢的数字，但是使用`x_scale.bandwidth()` D3 会自动调整`rect`的大小，以匹配我们的 SVG 的宽度。
*   `.attr("height", (d) => height - y_scale(d.Population))` -最后，我们将每个`rect`的高度设置为 SVG 高度，然后减去由`y_scale(d.Population)`生成的高度，确保每个`rect`被正确表示。

以下是放在一起的完整代码:

```
const width = 960, height = 500;

const x_scale = d3.scaleBand().range([0, width]).padding(0.1);
const y_scale = d3.scaleLinear().range([height, 0]);

const svg = d3.select("#d3_demo")
    .attr("width", width)
    .attr("height", height);

d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/nigeria-states.json")
    .then(({ data }) => {

	 data.forEach((d) => (d.Population = +d.info.Population));

	 // Scale the Domain
	 x_scale.domain(data.map((d) => d.Name));
	 y_scale.domain([0, d3.max(data, (d) => d.Population)]);

	 // add the rectangles for the bar chart
	 svg
	  .selectAll("rect")
	  .data(data)
	  .join("rect")
	  .attr("class", "bar")
	  .attr("x", (d) => x_scale(d.Name))
	  .attr("y", (d) => y_scale(d.Population))
	  .attr("width", x_scale.bandwidth())
	  .attr("height", (d) => height - y_scale(d.Population));
	}); 
```

这是输出结果:

[https://codepen.io/Spruce_khalifa/embed/preview/porxVVd?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=porxVVd](https://codepen.io/Spruce_khalifa/embed/preview/porxVVd?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=porxVVd)

这是一个非常基本的 D3.js 条形图。但是如果你把那个条形图给同事或朋友看，他们可能会问你“这是怎么回事，我们在看什么？”这将把我们引向另一个话题——轴。

### D3 中的轴组件

> 轴组件为刻度呈现人类可读的参考标记。–D3 文档

为了创建这些人类可读的参考标记，`d3.axis`make 使用`d3.scale`函数来确定要生成的节拍数。

为了给我们的轴创建不同的方向，D3 提供了四种方法:

*   d3.axisTop
*   d3 .轴底部
*   d3 .轴左
*   d3 .轴右

让我们来看一个例子:

```
let svg = d3.select("#d3_demo8").attr('width', 200).attr('height', 200)
let scale = d3.scaleLinear().domain([0, 100]).range([0, 200]);

let bottom_axis = d3.axisBottom(scale);

svg.append("g").call(bottom_axis);

// html
<svg id="d3_demo">
</svg>
```

Example output

要完成所有这些工作，您只需要传入您现有的`d3.scale`函数。让我们把这个应用到前面的例子中。

我们需要做的第一件事是建立 D3 保证金惯例。

### D3 保证金惯例

边距惯例只是给我们的图形添加边距的一种方式，以便有空间添加我们的轴。

要创建边距，首先创建一个四边都有属性的对象:

```
const margin = { top: 20, right: 30, bottom: 55, left: 70 }
```

然后，您需要为我们的 SVG 定义宽度和高度。对于响应图形，我们设置文档正文的宽度:

```
const width = document.querySelector("body").clientWidth;
const height = 500;
```

接下来，我们需要将这个宽度作为视图框应用于我们的 SVG 元素:

```
const svg = d3.select("#d3_demo").attr("viewBox", [0, 0, width, height])
```

接下来，我们需要设置`x_scale`和`y_scale`来处理新的边距:

```
const x_scale = d3
	.scaleBand()
	.range([margin.left, width - margin.right])
	.padding(0.1);

const y_scale = d3.scaleLinear()
    .range([height - margin.bottom, margin.top]);
```

接下来，让我们定义我们的左轴和底轴-记住，我们只需要传入我们现有的标尺(上面的那些):

```
let x_axis = d3.axisBottom(x_scale);

let y_axis = d3.axisLeft(y_scale);
```

除了我们添加轴的最后一部分之外，其他一切都与我们之前的示例相同:

```
// append x axis
svg
 .append("g")
  .attr("transform", `translate(0,${height - margin.bottom})`)
  .call(x_axis)
  .selectAll("text") // everything from this point is optional
  .style("text-anchor", "end")
  .attr("dx", "-.8em")
  .attr("dy", ".15em")
  .attr("transform", "rotate(-65)");

// add y axis
svg
 .append("g")
  .attr("transform", `translate(${margin.left},0)`)
  .call(y_axis);
```

您可以在 Codepen 上查看输出和完整代码:

[https://codepen.io/Spruce_khalifa/embed/preview/RwZvOPx?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=RwZvOPx](https://codepen.io/Spruce_khalifa/embed/preview/RwZvOPx?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=RwZvOPx)

### 在 D3 中如何用 CSS 来样式化

你会注意到我们的条形图是绿色的——为什么？我们为图表中的每个条形添加了一个类`bar`:

```
.attr("class", "bar")
```

我们可以使用这个类通过 CSS 来设计条形图的样式:

```
.bar {
  fill: green;
} 
```

## 如何用 D3.js 创建世界地图

我个人喜欢 D3 的一点是它处理地理数据的能力。与我们之前使用 JSON 数据格式地图的例子不同，现在我们将使用一种特殊形式的 JSON 数据，称为 [GeoJSON](http://geojson.org/) 。

你可以在这里找到我们将要使用的[geo JSON 数据。](https://raw.githubusercontent.com/iamspruce/intro-d3/main/data/countries-110m.geojson)

像在我们的其他例子中一样，让我们首先在文档中选择我们的 SVG 元素，并设置**边距约定:**

```
const margin = { top: 5, right: 5, bottom: 5, left: 5 },
	width = document.querySelector("body").clientWidth,
	height = 500;

const svg = d3.select("#d3_demo").attr("viewBox", [0, 0, width, height]);

// html
<body>
<svg id="d3_demo"></svg>
</body>
```

接下来，为了生成我们的地图，我们需要一个投影来呈现球面坐标(在我们的数据文件中)和一个路径生成器来将投影坐标转换为 SVG 路径，然后在屏幕上呈现:

```
let projection = d3.geoEquirectangular().center([0, 0]);
```

D3 提供了很多[投影](https://github.com/d3/d3-geo-projection)(我只用了这个，因为我喜欢)。现在我们已经选择了投影，让我们把它转换成一个 SVG 路径。当我们使用`d3.geoPath()`方法时，D3 为我们处理转换。这个方法接受一个投影(我们上面定义的那个):

```
const pathGenerator = d3.geoPath().projection(projection);
```

我们不想直接在 SVG 上绘制地图，因为我们稍后会添加动画和缩放。因此，我们将一个`g`元素附加到所选的 SVG:

```
let g = svg.append("g");
```

然后，我们将为地图加载数据:

```
d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/data/countries-110m.geojson")
  .then((data) => {
      console.log(data)
  });
```

如果这没有意义，我建议您重新阅读数据加载部分。

最后，让我们使用`pathGenerator`来生成路径:

```
 g.selectAll("path")
    .data(data.features)
    .join("path")
    .attr("d", pathGenerator);
```

上面我们使用 D3 数据连接为每个国家添加了一个路径，然后将`d`属性设置为我们的`pathGenerator`:

```
.attr("d", pathGenerator);
```

如果不清楚的话，可以这样写:

```
.attr('d', (d) => pathGenerator(d))
```

您可以在 Codepen 上找到最终代码和实时预览:

[https://codepen.io/Spruce_khalifa/embed/preview/dyzLyxp?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=dyzLyxp](https://codepen.io/Spruce_khalifa/embed/preview/dyzLyxp?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=dyzLyxp)

### 如何在 D3.js 中使用多个数据集

有时，您可能希望可视化来自不同来源的两个数据集。例如，我有一个包含尼日利亚地理数据的数据文件和另一个包含尼日利亚各州信息的[文件](https://github.com/iamspruce/intro-d3/blob/main/data/nigeria-states.json)。

在本教程的数据加载部分，我们只讲述了加载单个数据集。在 D3 中加载多个数据集看起来像这样:

```
Promise.all([
	d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/data/nigeria_state_boundaries.geojson"),
	d3.json("https://raw.githubusercontent.com/iamspruce/intro-d3/main/data/nigeria-states.json")
]).then(([geoJSONdata, countryData]) => {
    console.log(geoJSONdata)
    console.log(countryData)
});
```

通过在`Promise.all`中添加所有的 D3 数据加载方法`d3.json()`，只有当所有的数据都完成加载时，才会调用`.then()`回调，尽管如果其中一个数据文件加载失败，回调将不会被调用并导致错误。

### 带有城市名称的地图

现在让我们使用**加载多个数据集**想法来创建一个带有城市名称的地图。

为了简单起见，我们将省略创建地图的部分，因为我们已经在上面讨论过了。现在，我们将只关注添加城市名称:

一旦我们加载了需要格式化的数据:

```
countryData.data.forEach((d) => {
 d.info.Longitude = +d.info.Longitude;
 d.info.Latitude = +d.info.Latitude;
});
```

上面我们转换了经度和纬度。接下来，我们需要使我们的地图适合我们的容器。为此，您将使用`d3.fitSize()`方法:

```
projection.fitSize([width, height], geoJSONdata);
```

最后，我们需要添加城市名称:

```
g.selectAll("text")
 .data(countryData.data)
 .join("text")
  .attr("x", (d) => projection([d.info.Longitude, d.info.Latitude])[0])
  .attr("y", (d) => projection([d.info.Longitude, d.info.Latitude])[1])
  .attr("dy", -7)
  .style("fill", "black")
  .attr("text-anchor", "middle")
  .text((d) => d.Name);
```

就是这样！我们有一张标有城市名称的地图(我可能也加了圈，因为我觉得这很酷)。完整代码在 Codepen 上:

[https://codepen.io/Spruce_khalifa/embed/preview/BadEywO?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BadEywO](https://codepen.io/Spruce_khalifa/embed/preview/BadEywO?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BadEywO)

## 用 D3.js 处理事件

在本教程的开始，我们讨论了选择，但是有一点我们没有涉及到，那就是事件处理。

在 D3 中，我们可以使用`.on()`方法在选定的文档元素中添加或删除事件处理程序。

`.on()`方法接受两个参数:

1.  事件类型(通常是字符串)
2.  事件触发时调用的回调函数

#### D3 中的事件类型

D3 `.on()`事件类型可以是任何 [DOM 事件类型](https://developer.mozilla.org/en-US/docs/Web/Events#Standard_events)，但是 D3 最常见的事件是:

| 事件类型 | 描述 |
| --- | --- |
| 嗡嗡声 | 正在平移和缩放选择 |
| 点击 | 选择被点击 |
| 鼠标悬停 | 鼠标指针在所选内容上移动 |
| 鼠标移出 | 鼠标指针离开选择 |

### 具有平移和缩放功能的地图

为了查看 D3 事件处理是如何工作的，让我们将平移和缩放添加到我们之前创建的地图中。

我们需要做的第一件事是定义缩放函数:

```
let zooming = d3
  .zoom()
  .scaleExtent([1, 8])
  .on("zoom", (event) => {
   console.log(event)
  })
```

我们需要做的第一件事是使用`d3.zoom()`方法。我们还设置了`scaleExtent([1,8])`。我们这样做是为了设置缩放的极限，否则你会一直缩放到无穷大。现在让我们将转换添加到回调函数中的映射路径中:

```
.on("zoom", (event) => {
  // transform paths when zoomed
  g.selectAll("path").attr("transform", event.transform);

  // transform circles when zoomed
  g.selectAll("circle")
    .attr("transform", event.transform)
	.attr("r", 5 / event.transform.k);

  // transform text when zoomed
  g.selectAll("text")
	.attr("transform", event.transform)
	.style("font-size", `${18 / event.transform.k}`)
	.attr("dy", -7 / event.transform.k);
});
```

注意:`event.transform`是设置`translate('x','y')`和`scale` (event.transform.k)的快捷键。

最后，让我们调用 SVG 选择的缩放功能:

```
svg.call(zooming)
```

您可以在 Codepen 上找到完整的代码和预览:

[https://codepen.io/Spruce_khalifa/embed/preview/MWvROBq?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=MWvROBq](https://codepen.io/Spruce_khalifa/embed/preview/MWvROBq?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=MWvROBq)

### D3 的程序化放大

在 D3 中，我们可以通过编程来控制缩放，这让我们可以创建按钮来控制缩放行为:

让我们将这些按钮添加到之前的地图中:

```
<body>
 <div class="btn-group-vertical" role="group" aria-label="..." id="float-button-group">
  <button class="btn-default" id="zoomIn">
   <svg class="svg-icon" viewBox="0 0 20 20">
    <title>Zoom In</title>
	...svg icon
	</svg>
  </button>
  <button class="btn-default" id="zoomOut">
   <svg class="svg-icon" viewBox="0 0 20 20">
  <title>Zoom Out</title>
  ...svg icon
	</svg>
  </button>
  <button class="btn-default" id="resetZoom">
   <svg class="svg-icon" viewBox="0 0 20 20">
	<title>Reset Zoom</title>
	...svg icon
	</svg>
  </button>
</div>

<svg id="d3_demo"></svg>
</body>
```

下一步是选择这些按钮并控制缩放行为:

```
d3.select("#zoomIn").on("click", () => {
  svg.transition().call(zooming.scaleBy, 2);
});
d3.select("#zoomOut").on("click", () => {
  svg.transition().call(zooming.scaleBy, 0.5);
});
d3.select("#resetZoom").on("click", () => {
  svg.transition().call(zooming.scaleTo, 1);
});
```

什么是`scaleBy`和`scaleTo`？`scaleBy`将当前比例乘以我们的给定值(2)，而`scaleTo`将比例因子设置为我们的给定值(1)，这将重置缩放。

您可以在 Codepen 上找到预览版和完整代码:

[https://codepen.io/Spruce_khalifa/embed/preview/eYEoyYo?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=eYEoyYo](https://codepen.io/Spruce_khalifa/embed/preview/eYEoyYo?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=eYEoyYo)

### 如何在 D3 中添加工具提示

让我们在地图上添加工具提示。当用户将鼠标悬停在某项上时，工具提示会显示该项的更多信息。

让我们首先创建工具提示:

```
let tooltip = d3
  .select("body")
  .append("div")
  .attr("class", "tooltip")
  .style("opacity", 0);
```

接下来，让我们在圆圈悬停时添加工具提示，并在鼠标指针离开圆圈时移除它:

```
g.selectAll("circle")
  ...
  .style("fill", "green")
  .on("mouseover", (event, d) => {
    tooltip.transition().duration(200).style("opacity", 0.9);
    tooltip.html(`<p>Population: ${d.info.Population}</a>` + `<p>Name: ${d.Name}</p>`)
    .style("left", event.pageX + "px")
    .style("top", event.pageY - 28 + "px");
  })
  .on("mouseout", (d) => {
    tooltip.transition().duration(500).style("opacity", 0);
  });
```

下面是最终代码和预览(试着悬停在圆圈上):

[https://codepen.io/Spruce_khalifa/embed/preview/mdMYEBJ?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=mdMYEBJ](https://codepen.io/Spruce_khalifa/embed/preview/mdMYEBJ?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=mdMYEBJ)

## 结论

恭喜 D3 忍者！你已经走到这一步了。希望你已经学习了 D3 数据可视化的基础知识。

以下是一些后续步骤:

*   查看 [freeCodeCamp 数据可视化](https://www.freecodecamp.org/learn/data-visualization/)认证
*   在 [D3 的官方网站](https://d3js.org/)上查看文档和更多内容

如果你用这个创造了一些美妙的东西，请随意发微博，并标记我[@ sprucehalifa](https://twitter.com/sprucekhalifa)。别忘了点击“跟随”按钮。

哦，祝编码快乐！