# 所有基本的 React.js 概念都集中在这篇文章中

> 原文：<https://www.freecodecamp.org/news/all-the-fundamental-react-js-concepts-jammed-into-this-single-medium-article-c83f9b53eac2/>

> **更新:**这篇文章现在是我的书《React.js Beyond The Basics》的一部分。

> 阅读此内容的更新版本以及更多关于在[**【jscomplete.com/react-beyond-basics】**](https://jscomplete.com/g/react-fundamentals)*做出反应。*

这篇文章不会涉及 React 是什么或者[为什么你应该学习它](https://medium.freecodecamp.org/yes-react-is-taking-over-front-end-development-the-question-is-why-40837af8ab76)。相反，这是对 React.js 基础的实用介绍，面向那些已经熟悉 JavaScript 并了解 [DOM API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) 基础的人。

下面的所有代码示例都有标记以供参考。它们纯粹是为了提供概念的例子。大部分都可以用好得多的方式写出来。

### 基础#1: React 是关于组件的

React 是围绕可重用组件的概念设计的。您定义小的组件，并将它们放在一起形成更大的组件。

所有组件，无论大小，都是可重用的，甚至跨不同的项目。

React 组件——最简单的形式——是一个普通的 JavaScript 函数:

```
// Example 1
// https://jscomplete.com/repl?j=Sy3QAdKHW
function Button (props) {
  // Returns a DOM element here. For example:
  return <button type="submit">{props.label}</button>;
}
// To render the Button component to the browser
ReactDOM.render(<Button label="Save" />, mountNode)
```

按钮标签的花括号解释如下。现在不用担心他们。`ReactDOM`后面也会解释，但是如果你想测试这个例子和所有即将出现的代码例子，上面的`render`函数就是你需要的。

`ReactDOM.render`的第二个参数是 React 将要接管和控制的目标 DOM 元素。在[jsComplete React Playground](https://jscomplete.com/react/)中，可以只使用特殊变量`mountNode`。

[**JavaScript REPL 和 Playground for React.js**](https://jscomplete.com/react/)
[*在不做任何配置的情况下在浏览器中测试现代 JavaScript 和 react . js 代码*jscomplete.com/react](https://jscomplete.com/react/)

请注意以下有关示例 1 的内容:

*   组件名称以大写字母开头。这是必需的，因为我们将处理 HTML 元素和 React 元素的混合。小写名称是为 HTML 元素保留的。事实上，继续尝试将 React 组件命名为“button ”,看看 ReactDOM 如何忽略该函数并呈现一个常规的空 HTML 按钮。
*   每个组件都接收一个属性列表，就像 HTML 元素一样。在 React 中，这个列表被称为**道具**。对于一个函数组件，你可以给它起任何名字。
*   我们奇怪地在上面的`Button`函数组件的返回输出中写了看起来像 HTML 的东西。这既不是 JavaScript 也不是 HTML，甚至也不是 React.js。但是，它非常流行，以至于成为 React 应用程序的默认设置。它叫做[](https://facebook.github.io/jsx/)*，是一个 JavaScript 扩展。JSX 也是**妥协**！继续尝试在上面的函数中返回任何其他 HTML 元素，看看它们是如何被支持的(例如，返回一个文本输入元素)。*

### *基本面# 2:JSX 的流量是多少？*

*上面的示例 1 可以用没有 JSX 的纯 React.js 编写如下:*

```
*`// Example 2 -  React component without JSX
// https://jscomplete.com/repl?j=HyiEwoYB-
function Button (props) {
  return React.createElement(
    "button",
    { type: "submit" },
    props.label
  );
}
// To use Button, you would do something like
ReactDOM.render(
  React.createElement(Button, { label: "Save" }),
  mountNode
);`*
```

*`createElement`函数是 React 顶级 API 中的主函数。这是你需要学习的 8 件事情中的一件。React API 就是这么小。*

*就像 DOM 本身有一个`document.createElement`函数来创建由标签名指定的元素一样，React 的`createElement`函数是一个更高级的函数，可以做`document.createElement`所做的事情，但它也可以用来创建一个元素来表示 React 组件。当我们在上面的例子 2 中使用`Button`组件时，我们做到了后者。*

*与`document.createElement`不同，React 的`createElement`在第二个参数之后接受动态数量的参数，以表示所创建元素的**子元素**。所以`createElement`实际上创造了一棵**树** *。**

*这里有一个例子:*

```
*`// Example 3 -  React’s createElement API
// https://jscomplete.com/repl?j=r1GNoiFBb
const InputForm = React.createElement(
  "form",
  { target: "_blank", action: "https://google.com/search" },
  React.createElement("div", null, "Enter input and click Search"),
  React.createElement("input", { name: "q", className: "input" }),
  React.createElement(Button, { label: "Search" })
);
// InputForm uses the Button component, so we need that too:
function Button (props) {
  return React.createElement(
    "button",
    { type: "submit" },
    props.label
  );
}
// Then we can use InputForm directly with .render
ReactDOM.render(InputForm, mountNode);`*
```

*请注意上面示例中的一些事项:*

*   *`InputForm`不是 React 组件；这只是一个反应过来的**元素**。这就是为什么我们在`ReactDOM.render`通话中直接使用它，而不是与`<InputForm` / >一起使用。*
*   *`React.createElement`函数在前两个参数之后接受多个参数。它的参数列表从第三个开始，包括所创建元素的子元素列表。*
*   *我们能够嵌套`React.createElement`调用，因为这都是 JavaScript。*
*   *当元素不需要属性或属性时，`React.createElement`的第二个参数可以是 null 或空对象。*
*   *我们可以混合 HTML 元素和 React 元素。*
*   *React 的 API 试图尽可能接近 DOM API，这就是为什么我们对输入元素使用`className`而不是`class`。私下里，我们都希望 React 的 API 能够成为 DOM API 本身的一部分。因为，你知道，这样好多了。*

*当您包含 React 库时，上面的代码是浏览器能够理解的。浏览器不处理任何 JSX 业务。然而，我们人类喜欢看到和使用 HTML 而不是这些`createElement`调用(想象一下只用`document.createElement`建立一个网站，你可以的！).这就是 JSX 妥协存在的原因。我们可以用与 HTML 非常相似的语法来编写上面的表单，而不是用`React.createElement`调用:*

```
*`// Example 4 - JSX (compare with Example 3)
// https://jscomplete.com/repl?j=SJWy3otHW
const InputForm =
  <form target="_blank" action="https://google.com/search">
    <div>Enter input and click Search</div>
    <input name="q" className="input" />
    <Button label="Search" />
  </form>;
// InputForm "still" uses the Button component, so we need that too.
// Either JSX or normal form would do
function Button (props) {
  // Returns a DOM element here. For example:
  return <button type="submit">{props.label}</button>;
}
// Then we can use InputForm directly with .render
ReactDOM.render(InputForm, mountNode);`*
```

*请注意以上几点:*

*   *不是 HTML。例如，我们仍然在做`className`而不是`class`。*
*   *我们仍然在考虑把上面看起来像 HTML 的东西作为 JavaScript。看我是怎么在最后加分号的。*

*我们上面写的(例 4)是 JSX。然而，我们带到浏览器的是它的编译版本(例 3)。为了实现这一点，我们需要使用一个预处理器将 JSX 版本转换成`React.createElement`版本。*

*那是 JSX。这是一种妥协，允许我们用类似于 HTML 的语法编写 React 组件，这是一个很好的交易。*

> *上面标题中的单词“Flux”被用来押韵，但它也是由脸书推广的一个非常流行的应用架构的名字。其中最著名的实现是 Redux。Flux 完全符合 React 反应模式。*

*顺便说一下，JSX 可以单独使用。这不是只反应的事情。*

### *基础#3:你可以在 JSX 的任何地方使用 JavaScript 表达式*

*在 JSX 部分，您可以在一对花括号内使用任何 JavaScript 表达式。*

```
*`// To use it:ReactDOM.render(<RandomValue />, mountNode);// Example 5 -  Using JavaScript expressions in JSX
// https://jscomplete.com/repl?j=SkNN3oYSW
const RandomValue = () => 
  <div>
    { Math.floor(Math.random() * 100) }
  </div>;
// To use it:
ReactDOM.render(<RandomValue />, mountNode);`*
```

*任何 JavaScript 表达式都可以放在这些花括号里。这相当于 JavaScript [模板文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)中的`${}`插值语法。*

*这是 JSX 内部唯一的约束:只有表达。所以，比如你不能用常规的`if`语句，但是三元表达式是可以的。*

*JavaScript 变量也是表达式，所以当组件收到一个道具列表时(`RandomValue`组件没有，`props`是可选的)，你可以在花括号内使用这些道具。我们在上面的`Button`组件中做到了这一点(例 1)。*

*JavaScript 对象也是表达式。有时我们在花括号内使用一个 JavaScript 对象，这使它看起来像双花括号，但它实际上只是花括号内的一个对象。其中一个用例是将 CSS 样式对象传递给 React 中的特殊属性`style`:*

```
*`// Example 6 - An object passed to the special React style prop
// https://jscomplete.com/repl?j=S1Kw2sFHb
const ErrorDisplay = ({message}) =>
  <div style={ { color: 'red', backgroundColor: 'yellow' } }>
    {message}
  </div>;
// Use it:
ReactDOM.render(
  <ErrorDisplay 
    message="These aren't the droids you're looking for" 
  />,
  mountNode
);`*
```

*注意我是如何**析构**出道具参数的唯一消息的。还要注意上面的`style`属性是一个特殊的属性(同样，它不是 HTML，它更接近于 DOM API)。我们使用一个对象作为`style`属性的值。该对象定义了样式，就像我们用 JavaScript 一样(因为我们就是这样)。*

*你甚至可以在 JSX 里面使用一个 React 元素，因为那也是一个表达式。请记住，React 元素本质上是一个函数调用:*

```
*`// Example 7 - Using a React element within {}
// https://jscomplete.com/repl?j=SkTLpjYr-
const MaybeError = ({errorMessage}) =>
  <div>
    {errorMessage && <ErrorDisplay message={errorMessage} />}
  </div>;

// The MaybeError component uses the ErrorDisplay component:
const ErrorDisplay = ({message}) =>
  <div style={ { color: 'red', backgroundColor: 'yellow' } }>
    {message}
  </div>;
// Now we can use the MaybeError component:
ReactDOM.render(
  <MaybeError
    errorMessage={Math.random() > 0.5 ? 'Not good' : ''}
  />,
  mountNode
);`*
```

*上面的`MaybeError`组件将只显示`ErrorDisplay`组件，如果有一个`errorMessage`字符串传递给它并且有一个空的`div`。React 认为`{true}`、`{false}`、`{undefined}`和`{null}`是有效的元素子元素，它们不呈现任何内容。*

*您还可以在 JSX 内部的集合上使用所有的 JavaScript 函数方法(`map`、`reduce`、`filter`、`concat`等等)。同样，因为它们返回表达式:*

```
*`// Example 8 - Using an array map inside {}
// https://jscomplete.com/repl?j=SJ29aiYH-
const Doubler = ({value=[1, 2, 3]}) =>
  <div>
    {value.map(e => e * 2)}
  </div>;
// Use it
ReactDOM.render(<Doubler />, mountNode);`*
```

*注意上面我是如何给`value` prop 一个默认值的，因为它只是 Javascript。还要注意，我在`div`中输出了一个数组表达式。反应是没问题的；它会将每个双精度值放在一个文本节点中。*

### *基础#4:您可以用 JavaScript 类编写 React 组件*

*简单的功能组件非常适合简单的需求，但有时我们需要更多。React 也支持通过 [JavaScript 类语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)创建组件。下面是用类语法编写的`Button`组件(在示例 1 中):*

```
*`// Example 9 - Creating components using JavaScript classes
// https://jscomplete.com/repl?j=ryjk0iKHb
class Button extends React.Component {
  render() {
    return <button>{this.props.label}</button>;
  }
}
// Use it (same syntax)
ReactDOM.render(<Button label="Save" />, mountNode);`*
```

*这个类的语法很简单。定义一个扩展`React.Component`的类(你需要学习的另一个顶级 React API)。该类定义了一个实例函数`render()`，这个呈现函数返回虚拟 DOM 元素。每次我们使用上面的`Button`基于类的组件(比如通过做`<Button ...` / >)，React 都会从这个基于类的组件中实例化一个对象，并使用那个对象在 DOM 树中渲染一个 DOM 元素。*

*这就是为什么我们在上面的渲染输出中的 JSX 内部使用了`this.props.label`。因为通过类组件呈现的每个元素都有一个名为`props`的特殊的**实例**属性，该属性保存创建时传递给该元素的所有值。*

*因为我们有一个与组件的单次使用相关联的实例，所以我们可以根据需要定制该实例。例如，我们可以在使用常规的 JavaScript `constructor`函数构造之后定制它:*

```
*`// Example 10 -  Customizing a component instance
// https://jscomplete.com/repl?j=rko7RsKS-
class Button extends React.Component {
  constructor(props) {
    super(props);
    this.id = Date.now();
  }
  render() {
    return <button id={this.id}>{this.props.label}</button>;
  }
}
// Use it
ReactDOM.render(<Button label="Save" />, mountNode);`*
```

*我们还可以定义类函数，并在我们希望的任何地方使用它们，包括在返回的 JSX 输出中:*

```
*`// Example 11 — Using class properties
// https://jscomplete.com/repl?j=H1YDCoFSb
class Button extends React.Component {
  clickCounter = 0;
  handleClick = () => {
    console.log(`Clicked: ${++this.clickCounter}`);
  };

  render() {
    return (
      <button id={this.id} onClick={this.handleClick}>
        {this.props.label}
      </button>
    );
  }
}
// Use it
ReactDOM.render(<Button label="Save" />, mountNode);`*
```

*请注意上面示例 11 中的一些内容:*

*   *在 JavaScript 中，`handleClick`函数是使用新提出的[类字段语法](https://github.com/tc39/proposal-class-fields)编写的。这仍处于第 2 阶段，但出于多种原因，这是访问组件挂载实例的最佳选择(感谢 arrow 函数)。但是，您需要使用 Babel 这样的编译器来理解 stage-2(或 class-field 语法)以使上面的代码工作。jsComplete REPL 已经预先配置好了。*
*   *我们还使用相同的类字段语法定义了`clickCounter`实例变量。这允许我们完全跳过使用类构造函数调用。*
*   *当我们将`handleClick`函数指定为特殊的`onClick` React 属性的值时，我们没有调用它。我们将**引用**传递给了`handleClick`函数。在该级别调用函数是使用 React 时最常见的错误之一。*

```
*`// Wrong:
onClick={this.handleClick()}
// Right:
onClick={this.handleClick}`*
```

### *基础# 5:React 中的事件:两个重要的区别*

*在 React 元素中处理事件时，与我们使用 DOM API 的方式有两个非常重要的区别:*

*   *所有的 React 元素属性(包括事件)都使用**的茶色**命名，而不是**的小写**。是`onClick`，不是`onclick`。*
*   *我们传递一个实际的 JavaScript 函数引用作为事件处理器，而不是一个字符串。是`onClick={**handleClick**}`，不是`onClick="**handleClick"**`。*

*React 用自己的对象包装 DOM 事件对象，以优化事件处理的性能。但是在事件处理程序内部，我们仍然可以访问 DOM 事件对象上所有可用的方法。React 将包装事件对象传递给每个处理程序调用。例如，要阻止表单的默认提交操作，您可以:*

```
*`// Example 12 - Working with wrapped events
// https://jscomplete.com/repl?j=HkIhRoKBb
class Form extends React.Component {
  handleSubmit = (event) => {
    event.preventDefault();
    console.log('Form submitted');
  };

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <button type="submit">Submit</button>
      </form>
    );
  }
}
// Use it
ReactDOM.render(<Form />, mountNode);`*
```

### *基础#6:每个 React 组件都有一个故事*

*以下内容仅适用于类组件(扩展`React.Component`的组件)。功能组件的情况略有不同。*

1.  *首先，我们为 React 定义一个模板，从组件中创建元素。*
2.  *然后，我们指示 React 在某个地方使用它。例如，在另一个组件的`render`调用中，或者使用`ReactDOM.render`。*
3.  *然后，React 实例化一个元素，并给它一组我们可以用`this.props`访问的**道具**。这些道具正是我们在上面的步骤 2 中传递的。*
4.  *因为都是 JavaScript，所以会调用`constructor`方法(如果定义的话)。这就是我们所说的第一种:**组件生命周期方法** *。**
5.  *React 然后计算 render 方法的输出(虚拟 DOM 节点)。*
6.  *由于这是 React 第一次呈现元素，React 将与浏览器通信(代表我们，使用 DOM API)以在那里显示元素。这个过程俗称**装**。*
7.  *React 然后调用另一个生命周期方法，称为`componentDidMount`。例如，我们可以使用这个方法在浏览器中的 DOM 上做一些事情。在这种生命周期方法之前，我们使用的 DOM 都是虚拟的。*
8.  *一些组件的故事到此结束。出于各种原因，其他组件会从浏览器 DOM 中卸载。就在后者发生之前，React 调用另一个生命周期方法`componentWillUnmount`。*
9.  *任何已安装元件的**状态**可能会改变。该元素的父元素可能会重新呈现。在这两种情况下，被挂载的元素可能会收到一组不同的道具。反应魔法在这里发生，我们实际上开始**需要**在这一点上反应！老实说，在这之前，我们根本不需要作出反应。*

*这个组件的故事还在继续，但在此之前，我们需要理解我所说的这个**状态**的事情。*

### *基础#7: React 组件可以有私有状态*

*以下内容也仅适用于类组件。我提到过有些人把只表示的组件叫做愚蠢的组件吗？*

*在任何 React 类组件中，`state`属性都是一个特殊的属性。React 监控每个组件状态的变化。但是为了让 React 更有效，我们必须通过我们需要学习的另一个 React API 来更改 state 字段，`this.setState`:*

```
*`// Example 13 -  the setState API
// https://jscomplete.com/repl?j=H1fek2KH-
class CounterButton extends React.Component {
  state = {
    clickCounter: 0,
    currentTimestamp: new Date(),
  };

  handleClick = () => {
    this.setState((prevState) => {
     return { clickCounter: prevState.clickCounter + 1 };
    });
  };

  componentDidMount() {
   setInterval(() => {
     this.setState({ currentTimestamp: new Date() })
    }, 1000);
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Click</button>
        <p>Clicked: {this.state.clickCounter}</p>
        <p>Time: {this.state.currentTimestamp.toLocaleString()}</p>
      </div>
    );
  }
}
// Use it
ReactDOM.render(<CounterButton />, mountNode);`*
```

*这是需要理解的最重要的例子。它将基本上完成你的反应方式的基础知识。在这个例子之后，您还需要学习一些其他的小东西，但是主要是您和您的 JavaScript 技能。*

*让我们从类字段开始，浏览示例 13。它有两个。特殊的`state`字段由一个对象初始化，该对象包含一个以`0`开始的`clickCounter`和一个以`new Date()`开始的`currentTimestamp`。*

*第二个类字段是一个`handleClick`函数，我们将其传递给 render 方法中按钮元素的`onClick`事件。`handleClick`方法使用`setState`修改这个组件实例状态。注意这一点。*

*我们修改状态的另一个地方是我们在`componentDidMount`生命周期方法中启动的间隔计时器。它每秒滴答一次，执行另一个对`this.setState`的调用。*

*在 render 方法中，我们使用了具有正常读取语法的状态的两个属性。对此没有专门的 API。*

*现在，请注意，我们使用两种不同的方式更新了状态:*

1.  *通过传递一个返回对象的函数。我们是在`handleClick`函数中完成的。*
2.  *通过传递一个常规对象。我们是在区间回调的时候做的。*

*这两种方式都是可以接受的，但是当您同时读写状态时，首选第一种方式(我们就是这样做的)。在区间回调中，我们只写状态，不读状态。如果有疑问，请始终使用第一个函数作为参数的语法。使用竞争条件更安全，因为`setState`应该总是被视为异步方法。*

*我们如何更新状态？我们用我们想要更新的新值返回一个对象。注意在对`setState`的两次调用中，我们只从 state 字段传递一个属性，而不是两个都传递。这完全没问题，因为`setState`实际上**将您传递给它的内容(函数参数的返回值)与现有状态合并到了**。因此，在调用`setState`时不指定属性意味着我们不希望更改该属性(但不希望删除它)。*

> *哦， [#Reactjs](https://twitter.com/hashtag/Reactjs?src=hash&ref_src=twsrc%5Etfw) ，如果你对冗长的名字如此着迷，为什么你要把它命名为 setState，而你本应该把它命名为 scheduleShallowMergeWithState*
> 
> *— Samer Buna (@samerbuna) [June 1, 2017](https://twitter.com/samerbuna/status/870383561983090689?ref_src=twsrc%5Etfw)*