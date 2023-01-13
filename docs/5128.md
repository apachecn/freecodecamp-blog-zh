# React 入门指南

> 原文：<https://www.freecodecamp.org/news/a-beginners-guide-to-getting-started-with-react-c7f34354279e/>

作者:安基塔·马桑

# React 入门指南

在本教程中，我将首先帮助你理解为什么脸书认为有必要建立一个名为 *React* 的库。我将介绍创建第一个 React 应用程序所需的所有基本概念。本教程旨在通过清楚地解释 React 的基本原理来解释 React，如 JSX 和 ES6 的*使用，构建有状态的&无状态组件，React 元素，虚拟 DOM 和区分算法*。

![NsBhsN56E38anPtJhwdA44UQQg8RKAgKktfM](img/6f19a9e45686236da247f20e96a1b270.png)

Credits: [https://matwrites.com](https://matwrites.com)

web 已经从一堆静态的 HTML 页面发展成为可靠的、交互式的和高性能的应用程序。随着 AJAX 的出现，我们可以分部分异步加载整个应用程序。

每当应用程序的任何部分由于实时更新或用户输入而发生变化时，该部分仅被异步加载以反映更新后的状态。这意味着只有相应的文档对象模型( [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) )容器应该被更新以反映客户端的变化。

例如，考虑关于脸书的评论部分。初始加载时获取的注释数据被附加到 DOM 中。现在，当您添加一个新的注释时，它向服务器发出一个异步请求，将这个注释保存在数据库中，并更新 DOM 以反映当前状态。

让我们深入这个例子的细节并理解它是如何工作的。假设我的注释数组中有 20 条注释。comments 数组是真实的来源，它反映了任意时间点的当前状态。当用户添加新的评论时，一旦新的评论被成功地添加到数据库中，我们就需要修改这个评论的数组。comments 数组现在包含 21 条注释。

哦，对了，我们还需要编写代码来更新 DOM 以反映当前状态(假设我们在这里使用普通的 JavaScript 或 jQuery。接下来是很酷的东西！).这意味着 comments DOM 容器订阅了 comments 数组。当注释数组中有任何变化时，应该对其进行修改。我们可以有把握地说，注释数组是这里的模型，注释容器是视图。

让我们添加更多的东西—数百个 div 元素订阅了这个 comments 模型，这意味着我们需要编写代码来更新这些`div`元素中的每一个。

```
function updateCommentsSubscriber (response) {    commentsArr = [ ...response ] // commentsArr = ['Comment 1', 'Comment 2', ...]    var firstDiv = document.getElementById('first')    firstDiv.innerHTML = commentsArr.length    var secondDiv = document.getElementById('second')    secondDiv.innerHTML = commentsArr.toString()    ...}
```

唉！想象一下，如果这发生在脸书经营的规模上。每当我们有一个订阅了该模型的新元素时，我们都必须编写代码来对该元素进行更改。这种方法不可扩展。

脸书通过构建一个名为 **React** 的库来处理可伸缩性和 DOM 慢的问题。React 于 2011 年首次部署在脸书的 *Newsfeed* 版块，随后于 2012 年部署在 Instagram 上。它于 2013 年开源，此后一直受到全球社区的欢迎。

在本教程中，我们将建立在 React 的基础之上。本教程结束时，您应该能够编写您的第一个 React 应用程序。

### 目录

1.  介绍 React
2.  JSX
3.  是六个
4.  反应元素
5.  成分
6.  状态和道具
7.  无状态组件
8.  生命周期方法
9.  虚拟 DOM
10.  使用 create-react-app 构建您的第一个 React 应用

虽然我建议深入研究这些主题，但如果你已经有信心，请随意跳过其中任何一个。

### 介绍 React

**React 是一个 JavaScript 库，用于构建用户界面**。它通过有效地更新 DOM 解决了前面提到的可伸缩性问题。

更新 DOM 的一种方法是手动在相应的 DOM 节点中输入值，这显然很慢并且不可伸缩。

Angular 通过使用*数据绑定*解决了这个问题。它将视图中使用的变量与它们各自在模型中的对应物绑定在一起。当变量在模型中的相应值发生变化时，它会自动更新视图中变量的所有实例。

另一方面，React 提供了一种不同的方法来解决这个问题。它使用一种叫做*协调*的技术来评估两个时间点上 DOM 表示的差异。它只更新被改变的部分。一旦我们深入了解 React 如何工作的细节，这一点就会变得很清楚。现在，让我们考虑一个简单的例子:

这将进入 HTML:

```
<div id='app'></div>
```

将以下代码放入 JavaScript 文件中:

```
class HelloReact extends React.Component {    render () {        return (            <div>Hello React!</div>        )    }}ReactDOM.render(<HelloReact>, document.getElementById('app'))
```

上面的例子会在屏幕上打印出`Hello React!`。`HelloReact`被称为*的成分*在起反应。该组件中的`render`方法返回 DOM 表示。

我用 JavaScript 写 HTML 不是巧合。`<div>Hello React!` < /div `> i` nside render *方法是*一种 JSX 语法。它让我们用 JavaScript 编写 HTML。

最后一条语句完成了在我们的`app`容器中呈现`HelloReact`组件的工作。在接下来的两节中，我们将深入探讨 JSX 和 ES6。

### JSX

如果我们没有 JSX，你必须把上面的 JSX 代码写成:

```
React.createElement("div", null, "Hello React!")
```

如果您需要处理嵌套元素，这会使事情变得复杂。

例如:

```
React.createElement("div", { className: "container" },    React.createElement("span", null, "Hello React!"),    React.createElement("span", null, "I'm inside HelloReact Component"))
```

这是上面代码的 JSX 语法:

```
<div className='container'>    <span>Hello React!</span>    <span>I'm inside HelloReact Component</span></div>
```

在 React 组件中编写 JSX 可以清楚地知道组件将向 DOM 呈现什么。看起来干净直观。

如果你已经确信使用 JSX 与反应，继续阅读了解更多。如果没有，React 允许你在没有 JSX 的情况下使用它。你可以在这里了解更多信息。

JSX 只是一个 JavaScript 扩展，允许编写类似 HTML 的代码:

```
<div>Hello {name}!</div>
```

花括号内的表达式将被计算为变量`name`的值。你甚至可以在 JSX 内部调用函数:

```
<div>Hello, {formatName('james.gosling')!}</div>
```

这将调用`formatName`函数并将`james.gosling`作为参数传递给它。它将呈现从函数`formatName`返回的值。

#### 条件语句

通常情况下，只有在设置了特定值时，才需要呈现 DOM 节点。

这可以通过使用 JSX 来实现:

```
greetUser (userName) {    if (isUserLoggedIn) {        return <div>Hello, {userName}!</div>    }    return <div>Hello, Guest!</div>}
```

上面的例子会用他的名字问候一个登录的用户，当一个用户没有登录的时候作为*客人*。

请注意，这种条件渲染不同于显示/隐藏场景。根据变量`isUserLoggedIn`的值，一次只能将两个元素中的一个呈现给 DOM。

#### 使用 map 函数呈现子节点

让我们考虑一下上面的注释例子。这里，我们有一个注释数组，我们需要在一个 DOM 容器中呈现所有的注释。

`map`是一个本地 JavaScript 函数，用于迭代数组中的元素。您可以传递一个自定义迭代器函数作为它的一个参数。数组中的每个元素都将作为这个迭代器函数的输入。`map`将根据每次迭代返回的值输出一个数组。

构建 React 组件时很方便。你可以在这里了解更多。

```
renderComments (commentsArr) {    /* commentsArr would look similar to this structure below    commentsArr = [{        id: 1,        text: "I'm a comment!"    }]    */    return (        <div class='comments-container'>            {                commentsArr.map (function (comment) {                   return <div key={comment.id}>{comment.text}</div>                })            }        </div>    )}
```

上面的`renderComments`方法将返回`map`方法的结果。在本例中，`map`返回一个 DOM 节点数组。

注意每个`div`元素都使用了`key`。稍后，我们将详细讨论如何在 DOM 元素中使用`key`。目前，它只是作为唯一标识 DOM 元素的一种方式。

#### 将 JSX 编译成 JavaScript

React 库不理解 JSX。它只是用来让开发人员的工作更容易。那么，当 React 在 JavaScript 代码中遇到这种奇怪的类似于 HTML 的语法时，它会怎么做呢？

在 React 遇到 JavaScript 语句之前，JSX 通过[巴别塔插件](https://babeljs.io/docs/en/babel-plugin-transform-react-jsx)被传送到 JavaScript 语句。*翻译是将一种源语言转换成另一种源语言的过程*。所以，React 看到的只是简单的 JavaScript 语句。

上面简单的 JSX 电码

```
<div class='heading'>Hello React!</div>
```

被传送到

```
React.createElement("div", { className: 'heading' }, "Hello React!")
```

`createElement`在 React 上定义方法。

`createElement`方法的第一个参数是您希望 React 为您创建的 DOM 节点的类型。可以是`div`、`span`、`p`等等。

第二个参数用于指定 DOM 节点的属性。在这种情况下，我们告诉 React 节点的`className`是`heading`。

注意使用了`className`而不是传统的`class`属性。在 React 中，`class`属性被指定为`className`，以避免与现有属性冲突。

第三个参数携带关于 DOM 节点的子节点的信息。在这种情况下，只是普通文本将作为`innerHTML`添加到 DOM 节点

`createElement`方法类似于 JavaScript 中的`document.createElement`方法，但是它输出一个 React 元素。我们将在后面讨论 React 元素的细节。

### 是六个

ES6 是 ECMAScript 6 或 ECMAScript 2015 的缩写。在这一部分中，我们将学习一些常见的 ES6 技术，这些技术使得开发 React 组件变得更加容易。

#### `let`&

`let`用于声明块级变量，`const`用于声明常量。JavaScript 遵循一个**函数级范围**。

在函数内部声明的变量可以在整个函数中使用，但不能在函数外部使用。

让我们考虑一个帮助我们理解函数级范围的例子:

```
function pokemon (id) { //id = 12    if (id === 12) {        var pokemonObj = {            id: 12,            name: 'butterfree',            height: 11,            weight: 22,            abilities: [                {                    name: 'tinted-lens'                }            ]        }    }    return pokemonObj    }
```

上面的`pokemon`方法以`id`为参数，如果`id`等于`12`，则返回`pokemonObj`。继续在您的浏览器控制台中运行此功能。

如果您执行`pokemon(12)`，它会按照预期打印`pokemonObj`。但是，如果您执行`pokemon(13)`，它不会显示错误，而是打印`undefined`。

注意，`pokemonObj`在`if`构造中定义，这意味着如果`id`不等于 12，`pokemonObj`在`pokemon`函数的上下文中不可用。

JavaScript 遵循函数级作用域，这意味着在函数内部的任何语句中声明的变量在整个函数中都是可用的。所以，`pokemonObj`是可用的，即使它是在`if`块中声明的。

`let`构造解决了这个问题，因为它允许我们将变量的范围限制在块级别。

使用`let`声明`pokemonObj`并检查结果:

```
function pokemon (id) { //id = 12    if (id === 12) {        let pokemonObj = {            id: 12,            name: 'butterfree',            height: 11,            weight: 22,            abilities: [                {                    name: 'tinted-lens'                }            ]        }    }    return pokemonObj    }
```

您将得到`pokemonObj is not defined`，这证明了使用`let`构造声明的变量的块级范围。

使用`const`声明的变量不能修改:

```
const POKEMON = 'butterfree' pokemon = 'pikachu'
```

上面的第二条语句显示了一个错误，因为不允许修改常量变量。

#### 对象析构

考虑一个物体，`berries`，它是一个小水果，当被神奇宝贝吃掉时，可以提供 HP 和状态条件恢复，stat 增强，甚至伤害否定:

```
var berries = {    id: 1,    name: 'cheri',    growth_time: '3',    max_harvest: 5,    natural_gift_power: 60,    size: 20,    smoothness: 25,    soil_dryness: 15,    natural_gift_type: {        name: 'fire',        url: 'https://pokeapi.co/api/v2/type/10/'    }}
```

如果我需要使用上述对象中的一些键，传统的方法是:

```
var id = berries.idvar name = berries.namevar growthTime = berries.growth_time
```

对于 ES6:

```
let { id, name, growth_time, max_harvest, natural_gift_power, size } = berries
```

上面的语句创建了本地变量`id`、`name`、`growth_time`、`max_harvest`、`natural_gift_power`和`size`，每个变量的值都对应于`berries`对象中的值。是不是很酷？

这里，我们析构对象来引用单个键。如果对象中没有定义任何键，那么它的值就是`undefined`。

#### 字符串模板化

以下是在 JavaScript 中连接字符串的老方法:

```
var cheriDescription = "One of the berries is " + name + ". Its growth time is " + growth_time + ". Its size is " + size + ". Its Max Harvest is " + max_harvest.
```

这是 ES6 的方式:

```
let cheriDescription = `One of the berries is ${name}. Its growth time is ${growth_time}. Its size is ${size}. Its Max Harvest is ${max_harvest}.`
```

您可以用反斜线书写整个句子，而不必将静态文本和变量部分连接起来。将变量包装在`'${}'`中。这看起来干净多了。

#### 箭头功能

这是我最喜欢的。下面是用 JavaScript 编写函数的老方法:

```
function getBerrySize (berries) {    return berries.size}
```

这是 ES6 编写函数的方式:

```
const getBerrySize = (berries) => berries.size
```

箭头函数遵循以下语法:

`declaration` `functionName` = `(functionParams)` = & g `t; return res` ult

上面的函数`getBerrySize`返回浆果的大小。注意，我们没有在上面的箭头函数中写单词`return`。如果 arrow 函数的主体只有一条语句，并且返回该语句，那么使用`return`是可选的。

如果一个函数体有多个语句，用花括号把它们括起来。

当与`this`构造一起使用时，箭头函数的行为与普通函数稍有不同。从我的文章[这里](https://hackernoon.com/lets-get-this-this-once-and-for-all-f59d76438d34)中了解更多关于`this`和箭头函数与`this`一起使用时的不同行为。

#### 班级

使用 ES6，我们可以将用于实现特定功能的相关函数包装在一个类中。

让我们构建一个评估浆果的类:

```
class Berries {    constructor (berries) {        this.berries = berries    }    getSize () {        return this.berries.size    }    getGrowthTime () {        return this.berries.growth_time    }}const cherries = new Berries(berries)cherries.getSize() // 20cherries.getGrowthTime() // 3
```

我们已经将相关的浆果方法包装在类`Berries`中。

语句`const cherries = new Berries(berries)`实例化了 Berries 类并创建了一个类型为`Berries`的对象。

传递给 Berries 构造函数的输入是我们之前创建的`berries`对象。我们可以在这个对象上使用在`Berries`类中定义的方法。

既然我们已经学习了大多数常见的 ES6 技术，我们可以在下面的部分中使用它们。

### 反应元素

React 元素是表示 DOM 在任何时间点的状态的最小单位。

浏览器不理解 React。正如我们前面看到的，JSX 被转换成 JavaScript 语句来创建 DOM 节点。

有了这句话:

```
React.createElement("div", { className: 'heading' }, "Hello React!")
```

`div` DOM 节点还没有被追加到 DOM 中。上面的语句被转换成普通的 JavaScript 对象，如下所示:

```
{    type: 'div',    props: {        className: 'heading',        children: 'Hello React!'    }}
```

这些对象被称为**反应元素**。

它们包含两个重要的密钥:

`type`指定 DOM 节点的类型。可以是`div`、`span`、`p`等等。

`props`描述该元素的属性。在这种情况下，我们有`className`和`children`。

嵌套元素可以指定为`children`键的值。

将这些 React 元素转换成实际的 DOM 节点，并相应地更新它们。

React 元素是不可变的，并且创建起来很便宜。如果 React 元素中有任何变化，它的旧实例将被销毁，一个新的实例将从头开始创建。

但是，相应的 DOM 节点并不总是被销毁，以便为新的节点腾出空间。DOM 操作开销很大，因此在所有可能的情况下都应该避免。

ReactDOM 在这方面做得很好。它创建新的 React 元素，因为创建它们的成本很低，但是，它只有效地更新实际 DOM 节点中被修改的部分。

React 使用一种*差分*算法来找出需要更新到 DOM 的内容。我们将在虚拟 DOM 一节中了解更多。

让我们回顾一下到目前为止我们所学的内容:

*   JSX 是一种用于 React 组件的类似 HTML 的语法。它只是 DOM 节点的一个表示，实际上并没有向 DOM 添加元素。
*   Babel 插件将其转换成普通的 JavaScript 语句，如`React.createElement`。
*   `React.createElement`语句被进一步转换成 JavaScript 对象。
*   ReactDOM 使用上面创建的对象有效地更新真实的 DOM。

### 成分

组件是定义特定功能的可重用类。React 遵循基于组件的结构。我们在 React 中定义的每个组件都扩展了原生 React 组件的基本功能。

让我们为输入文本创建一个简单的 React 组件:

```
class Text extends React.Component {    state = {        value: ''    }    onChange = (e) => {        this.setState({ value: e.target.value })    }    render () {        return (            <input                type='text'                onChange={this.onChange}                value={this.state.value}            />        )    }}export default Text
```

每个 React 组件都有一个`render`方法的实现。该方法返回该组件的 DOM 表示。在我们的例子中，它返回一个`input`元素。记住，这个`input`元素实际上不是一个 DOM 节点。这只是一个 DOM 表示。

传递给`input`元素的属性— `text`、`onChange`和`value`在 React 中被称为**道具**。`onChange`是一个事件处理程序，每当输入框中的文本值发生变化时就会被调用。`props`在 React 中是只读的。

React 组件维护一个内部状态，用于处理基于各种参数的复杂性。在组件的开始，我们已经初始化了一个名为`state`的对象。这个状态变量处理文本组件的状态。可以使用`setState`方法修改状态，如上面定义的`onChange`方法所示。

`Text`组件是一个独立的单元。当这些组件可以直接用于其他复杂的组件时，就变得有趣了。React 遵循基于`Composition`的模型，这意味着导入更小的组件来实现复杂的功能。它将不同的较小零部件组合成一个较大的零部件，以弥补工作特征的不足。

让我们创建一个表单组件，从上面导入文本组件:

```
import Text from './text'class Forms extends React.Component {    state = {}    render () {        return (            <div>                <p>Form for basic details</p>                <Text />            </div>        )    }}
```

首先，我们导入文本组件。注意文本组件是如何包含在表单组件的`render`方法中的。

当表单组件被呈现时，`<Text` / >被文本组件中的`f the` 呈现方法的返回值替换。

### 关于`state`和`props`的更多信息

如前所述，React 遵循基于组合的模型。较大的组件可以通过发送相关的道具来定制要导入的组件。

让我们将上述表单组件的`render`方法修改为

```
render () {    return (        <div>            <p>Form for basic details</p>            <Text                type='text'            />;        </div>    )}
```

注意我们是如何在文本组件中传递`type`道具的。

我们需要修改我们的文本组件来接受这些属性。

查看`render`方法的变化:

```
render () {    let { type } = this.props    return (        <input            type={type}            onChange={this.onChange}            value={this.state.value}        />    )}
```

表单组件传递的`props`可以作为文本组件中的一个对象。注意我们是如何在由表单组件传递的文本组件中使用`type`属性的。

状态变量管理组件的内部状态。它可以根据网络变化、用户输入或任何计划的更新而改变。

`state`不过是一个 JavaScript 对象。可以使用`setState`方法进行更改。`setState`方法接受一个对象作为输入。每当状态中的任何值改变时，我们只在`setState`方法中传递该值，而不是传递整个状态对象。

假设我们有我们的 Berries 组件，并且它的内部状态是我们上面定义的`berries`对象。

如果`berries`对象的`growth_time`属性发生了变化，我们可以将其更新为:

```
this.setState({ growth_time: new_growth_time })
```

我们没有将整个`berries`对象传递给`setState`方法。我们只传递需要更新的值。

`setState`方法作为异步函数运行。多个状态更新调用被批处理并在稍后的时间间隔一起调用，以处理性能问题。

### 无状态组件

上面定义的组件有自己的状态。然而，我们也可以定义无状态组件。

这些组件是纯函数。他们不修改传递给他们的输入。这些只是代表性的组成部分。

让我们看一个无状态组件的例子:

```
const getBerrySize = (props) => {    let { size } = props    return (        <p>{size}<;/p>    )}
```

该组件唯一的工作是返回一个 DOM 表示来显示浆果的大小。

### 组件生命周期

React 组件遵循特定的生命周期模式。它经历了四个重要阶段:

1.  初始化
2.  增加
3.  更新
4.  卸载

让我们看看在这些阶段调用的一些方法

**初始化**
初始化状态和道具。

**挂载**
`componentWillMount` —该方法在挂载发生之前调用。它在`render`方法之前被调用。对于初始状态&道具的设置，建议使用`constructor`代替此方法。

`render` —这是类组件中唯一需要的方法。它返回任意时间点的 DOM 表示。它检查`state`和`props`的值，并返回更新后的 DOM 表示。

`componentDidMount` —该方法在组件安装后立即被调用。这是实例化对服务器的网络请求的好地方。一旦从服务器加载了数据，就可以调用`setState`方法来调用`render`来更新 DOM 节点。

**更新**
`componentWillReceiveProps` —如果`props`有变化，调用此方法渲染更新后的状态。从 React 版本 16 开始，不建议使用此方法。

`shouldComponentUpdate` —如果该方法返回`true`，则调用`render`方法为更新后的`state`和`props`腾出空间。

`componentWillUpdate` —在 render 开始工作之前立即调用该方法。

`componentDidUpdate` —一旦更新的状态被呈现给 DOM，就调用这个方法。

**卸载**
`componentWillUnmount` —在卸载组件之前调用该方法。这是从组件中删除任何订阅的好地方。

这是 React 组件生命周期的简要概述。我将在以后的文章中详细解释这些方法。

### 虚拟 DOM

我在前面介绍 React 时提到了可伸缩性问题。在这一节中，我们将看到 React 如何有效地解决这个问题。

从您目前所了解的情况来看，很明显 React 并不直接更新真正的 DOM。React 只知道 DOM 只不过是一个 JavaScript 对象。这个巨大的 JavaScript 对象叫做**虚拟 DOM** 。

如果任何组件中的`render`方法的输出有任何变化，虚拟 DOM 通过销毁它的旧实例并创建一个新实例来适应这些变化。

如前所述，React 使用一种叫做*协调*的技术来评估真实 DOM 和虚拟 DOM 之间的差异。

调和是使一种观点或信仰与另一种观点或信仰相容的行为。而这正是 ReactDOM 所做的。它对真实 DOM 进行修改，使其与虚拟 DOM 的当前状态兼容。React 使用一种*差分*算法来评估将对真实 DOM 做出的更改。

它使用基于两个假设的启发式算法:

1.  两种不同类型的元素会产生不同的树
2.  稳定的元素，或者没有被修改的元素，使用一个名为`key`的唯一标识符来标识。

#### 不同类型的元素

React 首先比较根元素的类型。如果根元素的类型在真实 DOM 和虚拟 DOM 中不同，它会销毁真实 DOM 并创建一个新的。

例如，如果真实 DOM 中的根类型是`div`，而虚拟 DOM 中的根类型是`span`，那么 diffing 算法将从真实 DOM 中删除整个`div`容器。

#### 相同类型的 DOM 元素

这种情况处理的是相同类型的元素，但是它们的属性值有一些不同。

考虑真实 DOM 中的这个 DOM 节点:

```
<div class='heading'>React Diffing Algorithm</div>
```

现在，考虑更新的 DOM 节点，它是某个组件的`render`方法的输出:

```
<div className='sub-heading'>React Diffing Algorithm</div>
```

通过比较这两个节点，React 将知道只有节点的类名发生了变化，并且它将有效地更新真实 DOM 中的类的值。

#### 标识子容器在父容器中的位置

如果要将相似的子节点附加到特定的 DOM 节点，React 建议使用`key`属性。

让我们考虑注释数组的例子:

```
commentsArr.map (comment => {    return <div key={comment.id}>{comment.text}</div>})
```

在 comments 容器的子节点中使用`key`有助于 React 唯一地标识不同的 comment 节点。如果任何一个注释节点发生了变化，React 会根据键值知道应该更新哪个节点。

### 使用`create-react-app`开始反应

`create-react-app`是一个 react 命令行界面工具，用于为任何 React 应用程序创建初始样板文件。

如前所述，它完成了设置开发服务器、处理 JSX 到 JavaScript 的编译等繁琐工作，还做了其他一些事情。

```
npm i create-react-app -g
```

这将在您的系统上全局安装`create-react-app`节点模块。

命令`create-react-app my-app`在
`my-app`目录中创建一个 react 项目。摆弄这些文件，尝试构建一个更小的 React 组件。

我希望您现在已经知道 React 是什么以及如何使用它了。

### 让我们回顾一下到目前为止我们所学的内容

1.  我们看到使用原始 JavaScript 代码更新 DOM 节点是不可伸缩的。
2.  React 是一个用于构建用户界面的 JavaScript 库。它通过有效地更新 DOM 解决了可伸缩性问题。
3.  React 组件以 JSX 的形式输出 DOM 表示。JSX 只是用来简化开发工作的语法糖。它以`React.createElement`的形式被转换成 JavaScript 语句。
4.  我们学习了大多数常见的 ES6 技术，比如使用`let`和`const`、对象析构、类、箭头函数和字符串模板。这些是在编写 React 组件时使用的。
5.  我们学习了 React 元素以及它们是如何创建的。
6.  我们了解到 React 遵循基于组件的模式，以及它如何使用简单的组件通过组合来创建复杂的组件。该组件使用`state`和`props`来处理内部状态。
7.  我们深入研究了 React 如何创建虚拟 DOM，也理解了用于有效更新真实 DOM 的底层差分算法。

在关于 React 的下一系列文章中，我将更详细地讨论以下主题:

1.  React 组件的生命周期
2.  虚拟 DOM
3.  使用`create-react-app`开始反应

*最初发表于[hashnode.com](https://hashnode.com/post/a-reintroduction-to-react-cjsbli2an02b7dys2y3ypng2z)。*