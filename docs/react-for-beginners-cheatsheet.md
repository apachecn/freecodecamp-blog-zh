# React for 初学者:2021 年完整的 React 备忘单

> 原文：<https://www.freecodecamp.org/news/react-for-beginners-cheatsheet/>

欢迎阅读 React 初学者指南。它旨在教你在 2021 年开始构建 React 应用程序所需要知道的所有核心 React 概念。

我创建这个资源是为了给你一个最完整和初学者友好的从头开始学习 React 的途径。

到最后，您将彻底理解大量基本的 React 概念，包括:

*   原因、内容和反应方式
*   如何轻松创建 React 应用
*   JSX 和基本语法
*   JSX 元素
*   组件和道具
*   React 中的事件
*   状态和状态管理
*   React 挂钩的基础

### 想要你自己的副本吗？‬ 📄

****[在此下载 PDF 格式的备忘单](https://reedbarger.com/resources/react-beginners-2021)**** (耗时 5 秒)。

以下是获取可下载版本的一些快速制胜之道:

*   快速参考指南，可随时查看
*   大量可复制的代码片段，便于重复使用
*   在最适合你的地方阅读这本大部头的指南。在火车上，在你的办公桌前，排队...任何地方。

有很多很棒的东西要介绍，所以让我们开始吧。

## 反应基础

### React 到底是什么？

React 的官方定义是“用于创建用户界面的 JavaScript 库”，但这到底意味着什么呢？

React 是一个用 JavaScript 编写的库，我们用 JavaScript 编码，用来构建在 web 上运行的优秀应用程序。

### 学习 React 我需要知道什么？

换句话说，要成为一名优秀的 React 程序员，你确实需要对 JavaScript 有一个基本的了解？

您应该熟悉的最基本的 JavaScript 概念是变量、基本数据类型、条件、数组方法、函数和 es 模块。

我如何学习所有这些 JavaScript 技能？[查看综合指南](https://reactbootcamp.com/javascript-skills-for-react-2021/)了解 React 所需的所有 JavaScript。

### 如果 React 是用 JavaScript 做的，为什么我们不直接用 JavaScript 呢？

React 是用 JavaScript 编写的，它完全是为了构建 web 应用程序而构建的，并为我们提供了这样做的工具。

JavaScript 是一种有 20 多年历史的语言，它是为通过脚本向浏览器添加少量行为而创建的，而不是为创建完整的应用程序而设计的。

换句话说，虽然 JavaScript 是用来创建 React 的，但它们是为了完全不同的目的而创建的。

### 我可以在 React 应用程序中使用 JavaScript 吗？

是啊！您可以在 React 应用程序中包含任何有效的 JavaScript 代码。

您可以使用任何浏览器或窗口 API，如地理定位或获取 API。

此外，由于 React(当它被编译时)在浏览器中运行，您可以执行常见的 JavaScript 操作，如 DOM 查询和操作。

## 如何创建 React 应用

### 创建 React 应用程序的三种不同方式

1.  用外部脚本将 React 放入 HTML 文件中
2.  使用浏览器内的 React 环境，如 CodeSandbox
3.  使用 Create React App 等工具在计算机上创建 React 应用程序

### 创建 React 应用程序的最佳方式是什么？

哪种方法最适合你？创建应用程序的最佳方式取决于您想用它做什么。

如果您想要创建一个完整的 web 应用程序，并最终将其推送到 web 上，那么最好使用 Create React App 之类的工具在您的计算机上创建 React 应用程序。

如果你有兴趣在你的电脑上创建 React 应用，[查看完整的使用 Create React 应用指南](https://reactbootcamp.com/create-react-app-10-steps/)。

创建和构建用于学习和原型制作的 React 应用程序的最简单和对初学者最友好的方式是使用 CodeSandbox 这样的工具。您可以通过访问 [react.new](https://react.new) 在几秒钟内创建一个新的 React 应用程序！

## JSX 元素

### JSX 是构建应用程序的强大工具

JSX 旨在让用 JavaScript 应用程序创建用户界面变得更容易。

它借用了最广泛使用的编程语言 HTML 的语法。因此，JSX 是构建我们应用程序的强大工具。

下面的代码示例是显示文本“Hello World”的 React 元素的最基本示例:

```
<div>Hello React!</div>
```

注意，要在浏览器中显示，React 元素需要通过**渲染**(使用`ReactDOM.render()`)。

### JSX 与 HTML 有何不同

我们可以在 JSX 中编写有效的 HTML 元素，但是略有不同的是一些属性的编写方式。

由多个单词组成的属性以 camel-case 语法编写(如`className`)，并且与标准 HTML ( `class`)具有不同的名称。

```
<div id="header">
  <h1 className="title">Hello React!</h1>
</div>
```

JSX 有这种不同的编写属性的方式，因为它实际上是使用 JavaScript 函数编写的(稍后会详细介绍)。

### 如果 JSX 由一个标签组成，它必须有一个尾随斜线

与标准 HTML 不同，像`input`、`img`或`br`这样的元素必须以尾随的正斜杠结束，它才是有效的 JSX。

```
<input type="email" /> // <input type="email"> is a syntax error
```

### 具有两个标签的 JSX 元素必须有一个结束标签

应该有两个标签的元素，如`div`、`main`或`button`，必须有它们的结束标签，即 JSX 中的第二个标签，否则将导致语法错误。

```
<button>Click me</button> // <button> or </button> is a syntax error
```

### JSX 元素是如何设计的

内联样式的书写方式不同于普通 HTML。

*   内联样式不能作为字符串包含在对象中。
*   同样，我们使用的样式属性必须以驼峰式风格编写。

```
<h1 style={{ color: "blue", fontSize: 22, padding: "0.5em 1em" }}>
  Hello React!
</h1>;
```

接受像素值(如宽度、高度、填充、边距等)的样式属性可以使用整数而不是字符串。比如用`fontSize: 22`代替`fontSize: "22px"`。

### JSX 可以有条件地显示

React 新开发人员可能想知道 React 使用 JavaScript 代码有什么好处。

一个简单的例子是，为了有条件地隐藏或显示 JSX 内容，我们可以使用任何有效的 JavaScript 条件，比如 if 语句或 switch 语句。

```
const isAuthUser = true;

if (isAuthUser) {
  return <div>Hello user!</div>   
} else {
  return <button>Login</button>
}
```

我们在哪里返回这个代码？在 React 组件中，我们将在后面的小节中介绍。

### 浏览器无法理解 JSX

如上所述，JSX 不是 HTML，而是由 JavaScript 函数组成的。

事实上，在 JSX 编写`<div>Hello React</div>`只是一种更方便、更容易理解的代码编写方式，如下所示:

```
React.createElement("div", null, "Hello React!")
```

两段代码都有相同的“Hello React”输出。

要编写 JSX 并让浏览器理解这种不同的语法，我们必须使用一个 **transpiler** 将 JSX 转换成这些函数调用。

最常见的 transpiler 叫做 **Babel。**

## 反应组分

### 什么是 React 组件？

我们可以将它们包含在 React **组件**中，而不仅仅是呈现一组或另一组 JSX 元素。

组件是使用看似普通的 JavaScript 函数创建的，但不同之处在于它返回 JSX 元素。

```
function Greeting() {
  return <div>Hello React!</div>;   
}
```

### 为什么使用 React 组件？

React 组件允许我们在 React 应用程序中创建比单独使用 JSX 元素更复杂的逻辑和结构。

可以把 React 组件想象成我们的定制 React 元素，它们有自己的功能。

众所周知，函数允许我们创建自己的功能，并在应用程序中重用它。

组件可以在我们的应用程序中任意重用，次数不限。

### 组件不是普通的 JavaScript 函数

我们如何呈现或显示从上面的组件返回的 JSX？

```
import React from 'react';
import ReactDOM from 'react-dom';

function Greeting() {
  return <div>Hello React!</div>;   
}

ReactDOM.render(<Greeting />, document.getElementById("root));
```

我们使用`React`导入来解析 JSX，使用`ReactDOM`将组件呈现给 id 为“root”的**根元素**

### React 组件可以返回什么？

组件可以返回有效的 JSX 元素，以及字符串、数字、布尔值、值`null`，以及数组和片段。

为什么我们要返回`null`？如果我们想让一个组件什么都不显示，通常会返回`null`。

```
function Greeting() {
  if (isAuthUser) {
    return "Hello again!";   
  } else {
    return null;
  }
}
```

另一个规则是 JSX 元素必须包装在一个父元素中。不能返回多个同级元素。

如果需要返回多个元素，但不需要向 DOM 添加另一个元素(通常是条件性的)，可以使用一个特殊的 React 组件，称为 fragment。

片段可以写成`<></>`或者当你导入 React 到你的文件中时，用`<React.Fragment></React.Fragment>`。

```
function Greeting() {
  const isAuthUser = true;  

  if (isAuthUser) {
    return (
      <>
        <h1>Hello again!</h1>
        <button>Logout</button>
      </>
    );
  } else {
    return null;
  }
}
```

请注意，当试图返回分布在多行中的多个 JSX 元素时，我们可以使用一组括号()来返回所有元素，如上面的示例所示。

### 组件可以返回其他组件

组件可以返回的最重要的东西是其他组件。

下面是一个 React 应用程序的基本示例，它包含在一个名为`App`的组件中，该组件返回多个组件:

```
import React from 'react';
import ReactDOM from 'react-dom';

import Layout from './components/Layout';
import Navbar from './components/Navbar';
import Aside from './components/Aside';
import Main from './components/Main';
import Footer from './components/Footer';

function App() {
  return (
    <Layout>
      <Navbar />
      <Main />
      <Aside />
      <Footer />
    </Layout>
  );
}

ReactDOM.render(<App />, document.getElementById('root'));
```

这是非常强大的，因为我们使用组件的定制来描述它们是什么(即布局)以及它们在应用程序中的功能。这告诉我们，只要看它们的名字，就应该如何使用它们。

此外，我们正在使用 JSX 的力量来组成这些组件。换句话说，使用 JSX 的类似 HTML 的语法，以一种容易理解的方式来组织它们(比如导航条在应用程序的顶部，页脚在底部，等等)。

### JavaScript 可以在 JSX 使用花括号

正如我们可以在组件中使用 JavaScript 变量一样，我们也可以在 JSX 中直接使用它们。

不过，在 JSX 内部使用动态值有一些核心规则:

*   JSX 可以接受任何原始值(字符串、布尔值、数字)，但不接受普通对象。
*   JSX 还可以包含解析这些值的表达式。

例如，可以使用三元运算符将条件语句包含在 JSX 中，因为它解析为一个值。

```
function Greeting() {
  const isAuthUser = true;  

  return <div>{isAuthUser ? "Hello!" : null}</div>;
}
```

## React 中的道具

### 可以使用 props 向组件传递值

JavaScript 中传递给组件的数据被称为 **props** 。

属性看起来与普通 JSX/HTML 元素的属性一样，但是您可以在组件本身中访问它们的值。

属性在它们被传递到的组件的参数中可用。道具总是作为对象的属性包含在内。

```
ReactDOM.render(
  <Greeting username="John!" />,
  document.getElementById("root")
);

function Greeting(props) {
  return <h1>Hello {props.username}</h1>;
} 
```

### 道具不能直接换

决不能在子组件中直接更改属性。

换句话说，道具永远不应该被**变异**，因为道具是一个普通的 JavaScript 对象

```
// We cannot modify the props object:
function Header(props) {
  props.username = "Doug";

  return <h1>Hello {props.username}</h1>;
}
```

组件被认为是纯函数。也就是说，对于每个输入，我们应该能够期待相同的输出。这意味着我们不能改变 props 对象，只能读取它。

### 特殊道具:儿童道具

如果我们想将元素/组件作为道具传递给其他组件，那么 **children** 道具非常有用

当您希望同一个组件(如布局组件)包装所有其他组件时，children 属性特别有用。

```
function Layout(props) {
  return <div className="container">{props.children}</div>;
}

function IndexPage() {
  return (
    <Layout>
      <Header />
      <Hero />
      <Footer />
    </Layout>
  );
}

function AboutPage() {
  return (
    <Layout>
      <About />
      <Footer />
    </Layout>
  );
}
```

这种模式的好处是，应用于布局组件的所有样式都将与其子组件共享。

## React 中的列表和键

### 如何使用 map 迭代 JSX 中的数组

我们如何使用数组数据在 JSX 显示列表？我们使用 **`.map()`** 函数将数据列表(数组)转换成元素列表。

```
const people = ["John", "Bob", "Fred"];
const peopleList = people.map((person) => <p>{person}</p>); 
```

你可以使用`.map()`作为组件和普通的 JSX 元素。

```
function App() {
  const people = ["John", "Bob", "Fred"];

  return (
    <ul>
      {people.map((person) => (
        <Person name={person} />
      ))}
    </ul>
  );
}

function Person({ name }) {
  // we access the 'name' prop directly using object destructuring
  return <p>This person's name is: {name}</p>;
}
```

### 列表中键的重要性

元素列表中的每个 React 元素都需要一个特殊的**键属性**。

对于 React 来说，关键是能够跟踪用`.map()`函数迭代的每个元素。

React 使用键在数据改变时高效地更新单个元素(而不是重新呈现整个列表)。

键需要有唯一的值，以便能够根据键值来识别每个键。

```
function App() {
  const people = [
    { id: "Ksy7py", name: "John" },
    { id: "6eAdl9", name: "Bob" },
    { id: "6eAdl9", name: "Fred" },
  ];

  return (
    <ul>
      {people.map((person) => (
        <Person key={person.id} name={person.name} />
      ))}
    </ul>
  );
}
```

## React 中的状态和管理数据

### React 中的状态是什么？

**状态**是一个概念，指的是我们的应用程序中的数据如何随时间变化。

React 中状态的意义在于，它是一种与用户界面(用户所看到的)分开讨论我们的数据的方式。

我们之所以谈论状态管理，是因为我们需要一种有效的方法，在用户与组件交互时跟踪和更新组件中的数据。

要将我们的应用程序从静态 HTML 元素变成用户可以与之交互的动态元素，我们需要状态。

### 如何在 React 中使用状态的示例

当用户希望与应用程序交互时，我们需要经常管理状态。

当用户在表单中键入内容时，我们会跟踪该组件中的表单状态。

当我们从 API 获取数据并显示给用户时(比如博客中的文章)，我们需要将数据保存在 state 中。

当我们想要改变一个组件从 props 接收的数据时，我们使用状态来改变它，而不是改变 props 对象。

### 介绍带有使用状态的 React 挂钩

在特定组件中“创建”状态的方法是使用`useState`钩子。

什么是钩子？它非常像一个 JavaScript 函数，但只能在组件顶部的 React function 组件中使用。

我们使用钩子来“挂钩”某些特性，而`useState`给了我们创建和管理状态的能力。

`useState`是一个直接来自 React 库的核心 React 钩子的例子:`React.useState`。

```
import React from 'react';

function Greeting() {
  const state = React.useState("Hello React");  

  return <div>{state[0]}</div> // displays "Hello React"
}
```

`useState`是如何工作的？像普通函数一样，我们可以给它传递一个起始值(比如“Hello React”)。

从 useState 返回的是一个数组。为了访问状态变量及其值，我们可以使用数组中的第一个值:`state[0]`。

然而，有一种方法可以改进我们的写法。我们可以使用数组析构来直接访问这个状态变量，我们喜欢怎么叫就怎么叫，比如`title`。

```
import React from 'react';

function Greeting() {
  const [title] = React.useState("Hello React");  

  return <div>{title}</div> // displays "Hello React"
}
```

如果我们想让我们的用户更新他们看到的问候语，该怎么办？如果我们包含一个表单，用户可以输入一个新值。然而，我们需要一种方法来更新标题的初始值。

```
import React from "react";

function Greeting() {
  const [title] = React.useState("Hello React");

  return (
    <div>
      <h1>{title}</h1>
      <input placeholder="Update title" />
    </div>
  );
} 
```

我们可以借助于 useState 返回的数组中的第二个元素来实现。这是一个 setter 函数，我们可以向它传递任何我们希望新状态的值。

在我们的例子中，当用户正在输入时，我们希望获得输入到输入中的值。我们可以在 React 事件的帮助下得到它。

### React 中的事件是什么？

事件是获取用户在我们的应用程序中执行的特定操作的数据的方式。

用于处理事件的最常见的道具是`onClick`(用于点击事件)、`onChange`(当用户键入输入内容时)和`onSubmit`(当表单被提交时)。

事件数据是通过将一个函数连接到列出的这些道具中的每一个(除了这三个之外，还有更多可供选择)而提供给我们的。

当我们的输入改变时，为了获得关于事件的数据，我们可以在输入上添加`onChange`,并将其连接到将处理事件的函数。该功能将被称为`handleInputChange`:

```
import React from "react";

function Greeting() {
  const [title] = React.useState("Hello React");

  function handleInputChange(event) {
    console.log("input changed!", event);
  }

  return (
    <div>
      <h1>{title}</h1>
      <input placeholder="Update title" onChange={handleInputChange} />
    </div>
  );
}
```

请注意，在上面的代码中，每当用户键入输入内容时，浏览器的控制台都会记录一个新事件

事件数据是作为一个对象提供给我们的，它具有许多取决于事件类型的属性。

### 如何在 React with useState 中更新状态

要用 useState 更新状态，我们可以使用 useState 在其数组中返回给我们的第二个元素。

这个元素是一个函数，它允许我们更新状态变量(第一个元素)的值。当我们调用 setter 函数时，传递给它的任何东西都将被置于状态。

```
import React from "react";

function Greeting() {
  const [title, setTitle] = React.useState("Hello React");

  function handleInputChange(event) {
    setTitle(event.target.value);
  }

  return (
    <div>
      <h1>{title}</h1>
      <input placeholder="Update title" onChange={handleInputChange} />
    </div>
  );
}
```

使用上面的代码，无论用户在输入中键入什么(文本来自于`event.target.value`)都将被置于使用`setTitle`的状态，并显示在`h1`元素中。

状态的特殊之处以及为什么必须用像 useState 这样的专用钩子来管理它是因为状态更新(比如当我们调用`setTitle`时)会导致重新呈现。

重新呈现是指某个组件基于新数据再次呈现或显示。如果我们的组件在数据改变时没有重新呈现，我们将永远看不到应用程序的外观改变！

## **下一步是什么**

我希望你能从这本指南中获益匪浅。

如果您希望保留一份备忘单以供学习之用，您可以[在此](https://reedbarger.com/resources/react-beginners-2021)下载该备忘单的完整 PDF 版本。

完成本指南后，您可以学习许多东西来提升您的技能，包括:

*   [如何编写自定义的 React 钩子](https://reactbootcamp.com/how-to-code-react-hooks/)
*   [反应道具完全指南](https://reactbootcamp.com/react-props-cheatsheet/)
*   [如何从前端到后端读取 React 中的数据](https://reactbootcamp.com/fetch-data-in-react/)
*   [如何在 React with Node 中构建全栈应用](https://reactbootcamp.com/react-app-node-backend/)
*   [了解更多关于反应状态的信息](https://reactbootcamp.com/what-to-know-about-react-state/)
*   [如何使用 React 路由器向 React 应用添加路由](https://reactbootcamp.com/react-router-cheatsheet/)
*   [使用高级 React 备忘单学习 React 的每个部分](https://reactbootcamp.com/react-cheatsheet-2021/)