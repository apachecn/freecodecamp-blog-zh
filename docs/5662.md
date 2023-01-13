# JSX 到底是什么，为什么你应该用它来构建你的 React 应用

> 原文：<https://www.freecodecamp.org/news/what-the-heck-is-jsx-and-why-you-should-use-it-to-build-your-react-apps-1195cbd9dbc6/>

瑞安·哈里斯

# JSX 到底是什么，为什么你应该用它来构建你的 React 应用

![EPkS1ES3N-sbSReJXEzzLVW2yM2T2xy12l8n](img/4d509bb81e54d23c1d8a01023e623eed.png)

Image source [NESA by Makers](https://unsplash.com/@nesabymakers)

作为开发人员，我们使用各种工具和开源包来简化我们的工作。其中一些在整个社区中被广泛使用，以至于它们似乎是 JavaScript 的原生语言。但是，它们不是，它们可以从根本上改变你每天编写代码的方式。

其中一项你可能已经在使用的技术是 JSX - **，它是 JavaScript** 的一种类似 XML 的语法扩展。由脸书团队创建，旨在简化开发者体验。正如说明书所说，创建 JSX 的理由是:

> “…定义一个简洁而熟悉的语法，用于定义带有属性的树结构。”~ [JSX 规格](https://facebook.github.io/jsx/)

现在，你可能会对自己说，“嘿，Ryan，这听起来很棒，但是**已经开始写代码了**，这是我们的第一个例子。

```
const helloWorld = <h1>Hello, World!</h1>;
```

就是这样！

上面的代码片段看起来很熟悉，但是你有没有停下来想想它的威力？JSX 设计它是为了让我们能够传递由 HTML 或 React 元素组成的树结构，就好像它们是标准的 JavaScript 值一样。

虽然在编写 React ( [或使用 React 来尝试 JSX](https://github.com/babel/babel/tree/master/packages/babel-plugin-syntax-jsx) )时，您不必使用 JSX，但不可否认，它是 React 生态系统的重要组成部分，所以让我们深入了解一下，看看到底发生了什么。

#### JSX 入门

使用 JSX 语法时要注意的第一件事是 React 必须在范围内。这是由于它是如何被编译的。以这个组件为例:

```
function Hello() {  return <h1>Hello, World!</h1>}
```

在幕后，由`Hello`组件呈现的每个元素都被传输到一个`React.createElement`调用中。

在这种情况下:

```
function Hello() {  return React.createElement("h1", {}, "Hello, World!")}
```

![i45KojKuegRdDklO7hgQUgFvqxOIUvvI-hZv](img/9c130b7d2f3bec05c6b7a566c4c09afb.png)

Image source [rawpixel](https://unsplash.com/@rawpixel)

嵌套元素也是如此。下面的两个例子最终会呈现相同的标记。

```
// Example 1: Using JSX syntaxfunction Nav() {  return (    <ul>      <li>Home</li>      <li>About</li>      <li>Portfolio</li>      <li>Contact</li>    </ul>  );}
```

```
// Example 2: Not using JSX syntaxfunction Nav() {  return (    React.createElement(      "ul",      {},      React.createElement("li", null, "Home"),      React.createElement("li", null, "About"),      React.createElement("li", null, "Portfolio"),      React.createElement("li", null, "Contact")    )  );}
```

#### React.createElement

当 React 创建元素时，它调用这个方法，这个方法有三个参数。

1.  元素名称
2.  表示元素道具的对象
3.  元素的子元素的数组

这里需要注意的一点是，React 将小写元素解释为 HTML 和 Pascal case(例如。ThisIsPascalCase)元素作为自定义组件。因此，下面的例子会有不同的解释。

```
// 1\. HTML elementReact.createElement("div", null, "Some content text here")
```

```
// 2\. React elementReact.createElement(Div, null, "Some content text here")
```

第一个例子将生成一个`<d` iv >，其中 s `tring "Some content text`作为它的子节点。然而，第二个版本会抛出一个错误(当然，除非自定义 comp`onent &`lt；Div / >在 sco `pe) bec`中，因为< Div / >未定义。

#### JSX 的道具

在 React 中工作时，您的组件通常会渲染子组件，并且需要向它们传递数据，以便子组件能够正确渲染。这些叫道具。

我喜欢把 React components 看作一群朋友。朋友是做什么的？他们互相给予对方支持。值得庆幸的是，JSX 为我们提供了很多实现这一目标的方法。

```
// 1\. Props defaulted to true<User loggedIn />
```

```
// 2\. String literals<User name="Jon Johnson" />
```

```
// 3\. JavaScript expressions<User balance={5 + 5 + 10} />
```

```
// 4\. Spread attributes<User preferences={...this.state} />
```

但是要小心！不能将 if 语句或 for 循环作为道具传递，因为它们是[语句，而不是表达式](https://dev.to/promhize/javascript-in-depth-all-you-need-to-know-about-expressions-statements-and-expression-statements-5k2)。

![540ZKVGYlZkyJmuY6dpHkUwAozpqC2Xf4l5w](img/597a152d128ebd3e3d55788084e3215e.png)

Image source [Kevin Ku](https://unsplash.com/@ikukevk)

#### JSX 的儿童

当你构建你的应用程序时，你最终开始让组件渲染子组件。然后这些组件有时必须呈现子组件。诸如此类。

因为 JSX 是为了让我们更容易推理元素的树状结构，所以这一切都变得非常容易。基本上，组件返回的任何元素都会成为其子元素。

使用 JSX 呈现子元素有四种方式:

#### 用线串

这是 JSX 孩子最简单的例子。在下面的例子中，React 创建了一个带有一个子元素的`<` h1 > HTML 元素。然而，子元素不是另一个 HTML 元素，只是一个简单的字符串。

```
function AlertBanner() {  return (    <h1>Your bill is due in 2 days</h1>  )}
```

#### **JSX 元素**

这可能是新 React 开发人员最熟悉的用例。在下面的组件中，我们返回一个 HTML 子组件(即`<head` er >，它有两个子组件`s own &`lt；娜`v /> and &l`t；ProfilePic / >两者都是自定义定义的 JSX 元素。

```
function Header(props) {  return (    <header>      <Nav />      <ProfilePic />    </header>  )}
```

#### **表情**

表达式让我们可以轻松地在 UI 中呈现 JavaScript 计算的结果。基本加法就是一个简单的例子。

假设我们有一个名为`<BillFooter` / >的组件来呈现关于账单或收据的信息。让我们假设它需要一个代表税前成本的 prop c `alled` total 和另一个代表适用税率的 `prop t` axRate。

使用表达式，我们可以很容易地为用户呈现一些有用的信息！

```
function BillFooter(props) {  return (    <div>      <h5>Tax: {props.total * props.taxRate}</h5>      <h5>Total: {props.total + props.total * props.taxRate}</h5>    </div>  );}
```

#### **功能**

通过函数，我们可以编程地创建元素和结构，然后 React 将为我们呈现这些元素和结构。这使得创建一个组件的多个实例或呈现重复的 UI 元素变得容易。

举个例子，让我们使用 JavaScript 的`.map()`函数来创建一个导航栏。

```
// Array of page informationconst pages = [  {    id: 1,    text: "Home",    link: "/"  },  {    id: 2,    text: "Portfolio",    link: "/portfolio"  },  {    id: 3,    text: "Contact",    link: "/contact"  }];// Renders a <ul> with programmatically created <li> childrenfunction Nav() {  return (    <ul>      {pages.map(page => {        return (          <li key={page.id}>            <a href={page.link}>{page.text}</a>          </li>        );      })}    </ul>  );}
```

现在，如果我们想给我们的站点添加一个新页面，我们需要做的就是给`pages`数组添加一个新对象，React 会处理剩下的事情！

**注意到** `key` **道具**。我们的函数返回一个兄弟元素的数组，在本例中是`<` li > s，React 需要一种方法来跟踪哪些元素被装载、卸载或更新。为此，它依赖于每个元素的唯一标识符。

#### 使用工具！

![ry5AANkjQySL0wIL0UlvdjlsG8ZHH720HGtf](img/033c00c157e146c0f2fa1e4cf7a2b240.png)

Image source [Barn Images](https://unsplash.com/@barnimages)

当然，没有 JSX，你也可以编写 React 应用程序，但是我不确定你为什么要这么做。

JSX 让我们像对待一等公民一样传递 JavaScript 中的元素，这种能力非常适合与 React 生态系统的其他部分合作。那么好吧，事实上，你可能每天都在写，甚至不知道。

底线是:用 JSX 就好。你会很高兴你做到了。