# 如何使用 React 的高阶组件

> 原文：<https://www.freecodecamp.org/news/react-higher-order-components-635d0bc38b6c/>

作者:亚当·雷夫洛赫

# 如何使用 React 的高阶组件

![xn6oqsiP9ZUwYN98Yidh-Obr7UhkAHHqcFvh](img/2059265767796f865022851d1fcff6b2.png)

Image credit: [https://facebook.github.io/react/img/logo.svg](https://facebook.github.io/react/img/logo.svg)

当 React 第一次出现时，它带来了一种开发前端架构的新方法。它被视为模型-视图-控制器中的“视图”。

随着时间的推移，React 生态系统的核心贡献者已经锁定了容器/表示模式。

在这篇文章中，您将了解更多关于容器/表示模式的知识，以及如何使用它来创建 React 组件。

在我继续之前，让我们澄清一些术语:

*   容器是获取和/或转换数据的组件。
*   表示组件呈现/显示它通过其 props 传递的数据。没别的了。
*   容器充当高阶组件。将高阶组件组合在一起可以将这两层分开。这可以防止您用不必要的逻辑来混淆表示层。

如果您愿意，可以使用 ESNextbin 来跟进。

导航到该页面后，在顶部您应该会看到三个选项卡:**代码**、 **HTML** 和**包**。点击**包**。导航到该选项卡后，添加 *react* 和 *react-dom* 作为*依赖关系*。

```
{ “name”: “esnextbin-sketch”, “version”: “0.0.0”, “dependencies”: {   “react” : “latest”,   “react-dom”: “latest” }}
```

现在点击**代码**标签，然后从 *react-dom* 导入 *react* 和 **render** 。

```
import { default as React } from ‘react’import { render } from ‘react-dom’
```

现在让我们创建一个无状态的功能组件来呈现“Hello，world！”

```
const Presentational = () => <div>Hello, world!</div>
```

之后，您只需要使用 *render* 将该组件安装到 DOM。

```
render(<Presentational />, document.querySelector(‘#root’))
```

您还没有在 **HTML** 中创建**根**元素，所以现在让我们这样做。你所做的就是添加一个 *div* ，其 id 为**根**的*。*

```
<!doctype html><html><head> <meta charset=”utf-8"> <title>ESNextbin Sketch</title> <! — put additional styles and scripts here →</head><body> // Notice something different <div id=‘root’></div></body></html>
```

现在是关键时刻了。点击页面右上角的**执行**。你应该看看*你好，世界！*在屏幕上。

现在，您有了一个表示组件。但是您还需要一个容器组件来获取数据。

让我们在表示组件之上创建一个容器组件。要创建容器，您将使用 React 的类组件。因此，您需要在页面顶部导入它。

```
import { default as React, Component } from ‘react’
```

此时，您可以忙于创建容器组件。

```
class Container extends Component {  state = {   data: [] }  componentDidMount() {   fetch(‘https://fcctop100.herokuapp.com/api/fccusers/top/alltime')   .then(response => response.json())   .then(data => this.setState({ data })) }  render() {   return <Presentational data={this.state.data} /> }}
```

在顶部，你有一个空数组。您正在使用一个名为 *componentDidMount* 的生命周期方法，您将在其中获取数据。

当响应返回时，用数据更新状态。然后，数据作为道具传递给表示组件。这意味着你也需要更新你的演示组件。

```
const Presentational = ({ data }) =>   <div>{JSON.stringify(data)}</div>
```

这里我使用析构从道具对象中提取数据。否则它会是这样的:

```
const Presentational = props =>   <div>{JSON.stringify(props.data)}</div>
```

这些更改迫使您更新要安装的组件。现在您需要呈现容器组件。

```
render(<Container />, document.querySelector(‘#root’))
```

当你点击**执行**时，你应该会看到一大堆数据溢出到你的屏幕上。我的天啊。

你在这里所做的是抽象出容器组件和它所做的一切。但是不灵活。您必须将表示组件放入容器中。有没有其他更灵活的方法？输入高阶分量。

高阶分量是接受一个分量并返回一个新分量的函数。在实践中会是什么样子。让我们创建一个函数，它接受一个组件，并返回一个包含来自 FCC API 的数据的新组件。为此，您将更新容器组件。

```
const container = Presentational =>  class extends Component {  state = {   data: [] }  componentDidMount() {   fetch(‘https://fcctop100.herokuapp.com/api/fccusers/top/alltime')   .then(response => response.json())   .then(data => this.setState({ data })) }  render() {   return <Presentational data={this.state.data} /> }}
```

这里没什么不同。但是，您必须执行一个额外的步骤。您需要将这个容器和表示组件组合在一起。我只是用表示组件作为参数调用容器。这被添加到您的表示组件的正下方。

```
const HigherOrderComponent = container(Presentational)
```

现在你要做的就是渲染高阶分量。

```
render(<HigherOrderComponent />, document.querySelector(‘#root’))
```

你看到你做了什么吗？现在，您可以将容器用于任何组件，而不仅仅是表示组件！

让我们更进一步。让我们调用另一个告诉容器组件获取什么数据的函数。我认为这将使容器更加灵活。

新容器看起来会像这样:

```
const container = endpoint => Presentational =>  class extends Component {  state = {   data: [] }  componentDidMount() {   fetch(endpoint)     .then(response => response.json())     .then(data => this.setState({ data })) }  render() {   return <Presentational data={this.state.data} /> }}
```

现在，您需要更新高阶组件来处理对端点的新调用。

```
const HigherOrderComponent = container(  ‘https://fcctop100.herokuapp.com/api/fccusers/top/alltime')(Presentational)
```

如果这看起来有点偏离你，那么你有很好的眼光。如果你能把它进一步抽象出来呢？回车*重新排版*！

现在让我们暂停一会儿，讨论一下作曲的概念是什么。要理解 compose，您需要理解 mapping。

计算机编程中的映射是指将一个值转换成另一个值。这基本上是所有被创建的函数！让我给你举个例子。

```
function double(x) {   return x * 2}
```

```
const doubled = double(2)
```

现在假设我想做另一个变换，比如三倍。我该怎么做？

```
function triple(x) {   return x * 3 }
```

```
function double(x) {   return x * 2}
```

```
const messedAroundAndGotATripleDouble = triple(double(2))
```

很简单，对吧？如果我想做另一个转变，再一个，再一个。它会变得很长。在 compose 函数的帮助下，你不仅可以使代码更具可读性，而且更具可组合性。

```
function compose(f, g) {   return function(x) {     return f(g(x))  }}
```

```
function triple(x) {   return x * 3 }
```

```
function double(x) {   return x * 2}
```

```
const tripleThenDouble = compose(triple, double)
```

```
const messedAroundAndGotATripleDouble = tripleThenDouble(2)
```

这是一个简单的例子，但是它展示了如何对同一个初始值进行多种变换。

现在假设初始值是表示组件，而容器是转换初始值的函数之一。神魂颠倒！

要开始使用*重组*,你需要引入*重组*库。在**包**下，让我们添加*重组*库。

```
{ “name”: “esnextbin-sketch”, “version”: “0.0.0”, “dependencies”: {   “react”: “15.3.2”,   “react-dom”: “15.3.2”,   “recompose”: “latest”,   “babel-runtime”: “6.11.6” }}
```

回到**代码**让我们从*改编*导入**作曲**。

```
import { compose } from ‘recompose’
```

然后，您可以使用 compose 进一步抽象高阶组件。

```
const HigherOrderComponent = compose( container(  ‘https://fcctop100.herokuapp.com/api/fccusers/top/alltime' ))
```

您可以更加明确地说明整个组件在做什么。

```
const FetchAndDisplayFCCData = HigherOrderComponent(Presentational)
```

```
render(<FetchAndDisplayFCCData />, document.querySelector(‘#root’))
```

这最终允许您创建通用和灵活的组件。您甚至可以将表示性组件与高阶组件分开测试。高阶元件 FTW！

为了好玩，让我们更新表示组件来呈现一个项目列表，而不是一大堆数据。

```
const Presentational = ({ data }) =>  <ul>   {data.map((v, k) =>      <li key={k}>       {v.username}     </li>   )} </ul>
```

咻！太多了。如果你想看成品，看看我在这里创造的要点[。](https://gist.github.com/arecvlohe/c5005643ea4fcb9637ccd9f60b98d305)

要了解更多信息，请访问 Andrew Clark 的 React 实用程序库，名为 *recompose* 以了解更多信息！

[**ACD lite/recompose**](https://github.com/acdlite/recompose)
[*recompose-一个用于函数组件和高阶组件的 React 实用程序带。*github.com](https://github.com/acdlite/recompose)