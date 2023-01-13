# 回应面试问题——用答案和例子准备面试

> 原文：<https://www.freecodecamp.org/news/react-interview-questions-and-answers/>

React 是一个用于创建用户界面的 JavaScript 库。它在超过 30，000 个现场网站中使用，拥有超过 70，000 个 GitHub 星级。

根据 [2021 Stack Overflow 开发者调查](https://insights.stackoverflow.com/survey/2021#section-most-popular-technologies-web-frameworks)显示，React 已经超过 jQuery 成为最受欢迎的 web 框架，占有大约 40.14%的市场份额。React 也是最受欢迎的，四分之一的开发者使用它。

超过 8000 家行业领导者，包括 LinkedIn、Twitter 和 AirBnB，都在使用 React。

一个 React 开发者在美国的平均年薪是 12 万美元。许多企业和公司使用 React，这迫使他们不断寻找新的人才。

在这篇文章中，我们将回顾一些 React 基础知识，然后看看一些面试问题及其正确答案，以帮助你在任何 React 面试中取得成功。

## 什么是反应？

React 是一个用于创建用户界面的开源前端 JavaScript 库。它是声明性的、高效的和灵活的，并且它坚持基于组件的方法，这允许我们创建可重用的 UI 组件。

开发人员主要使用 React 来创建单页面应用程序，而库只关注 MVC 的视图层。它也非常快。

## React 的特性

React 有许多与众不同的功能，但这里有几个亮点:

*   React 使用虚拟 DOM，而不是真实的/浏览器 DOM。
*   React 使用单向的单向数据绑定。
*   它用于开发 web 应用程序以及使用 [React Native](https://reactnative.dev/) 的移动应用程序，这允许我们构建跨平台的应用程序。

## 如何在 React 中启动一个新项目

我们可以通过初始化一个项目并安装所有依赖项来从头开始创建一个 React 应用程序。但是最简单的方法是通过下面的命令使用 [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app) :

```
npx create-react-app my-app
```

**注意:** my-app 是我们正在创建的应用程序的名称，但是您可以将其更改为您选择的任何名称。

你可以在本文中阅读更多关于如何[开始使用 React 的信息。](https://www.freecodecamp.org/news/get-started-with-react-for-beginners/)

## DOM 代表什么？

术语“DOM”代表文档 **D** 对象 **O** 对象 **M** 模型，指的是将 web 应用程序的整个用户界面表示为树形数据结构。

### DOM 的类型

我们有两种类型的 DOM，即虚拟 DOM 和真实 DOM。

## React 的优点和缺点

以下是 React 的一些优点和缺点:

### React 的优势

1.  对于 JavaScript 开发人员来说，它的学习曲线更短，并且由于其活跃的社区，可以获得大量的手册、教程和培训材料。
2.  React 是搜索引擎友好的
3.  它使得创建丰富的 UI 和定制组件变得更加容易。
4.  React 具有快速渲染功能
5.  JSX 的使用允许我们编写更简单、更吸引人、更容易理解的代码。

### React 的缺点

1.  因为 React 是前端库，所以需要其他语言和库来构建一个完整的应用。
2.  没有经验的程序员可能很难理解，因为它使用了 JSX。
3.  由于开发周期短，现有文档很快就过时了。

## 什么是 JSX？

JavaScript XML 缩写为 JSX。JSX 支持并简化了 React 中 HTML 的创建，从而使标记更加易读易懂。

例如:

```
const App = () => {
  return (
    <div>
       <h1>Hello World</h1>
    </div>
  )
}
```

你可以在这篇文章中阅读更多关于 [JSX 的内容。](https://www.freecodecamp.org/news/jsx-in-react-introduction/)

## 为什么浏览器不能读取 JSX？

JSX 不是有效的 JavaScript 代码，也没有内置的实现让浏览器能够阅读和理解它。我们需要将来自 JSX 的代码转换成浏览器可以理解的有效 JavaScript 代码，我们使用 JavaScript 编译器/trans piler[Babel](https://babeljs.io/)来完成这项工作。

注意: [create-react-app](https://github.com/facebook/create-react-app) 在内部使用 babel 进行 JSX 到 JavaScript 的转换，但是您也可以使用 Webpack 设置自己的 Babel 配置。

你可以在这篇文章中阅读更多关于 [JSX 的内容。](https://www.freecodecamp.org/news/jsx-in-react-introduction/)

## 什么是组件？

组件是一个自包含的、可重用的代码块，它将用户界面分成更小的部分，而不是在单个文件中构建整个 UI。

React 中有两种组件:功能组件和类组件。

### 什么是类组件？

类组件是返回 JSX 的 ES6 类，需要使用 React 扩展。因为在早期版本的 React (16.8)中不可能在功能组件中使用状态，所以功能组件仅用于呈现 UI。

示例:

```
import React, { Component } from 'react'
export default class App extends Component {
  render() {
    return (
      <div>
         <h1>Hello World</h1>
      </div>
    )
  }
}
```

阅读本文中关于 React [组件和组件类型](https://www.freecodecamp.org/news/react-components-jsx-props-for-beginners/)的更多内容。

### 什么是功能组件？

返回 React 元素的 JavaScript/ES6 函数被称为功能组件(JSX)。

自从引入 React 钩子后，我们已经能够在功能组件中使用状态，这使得许多人采用它们，因为它们的语法更简洁。

示例:

```
import React from 'react'
const App = () => {
  return (
    <div>
       <h1>Hello World</h1>
    </div>
  )
}
export default App; 
```

阅读本文中关于 React [组件和组件类型](https://www.freecodecamp.org/news/react-components-jsx-props-for-beginners/)的更多内容。

### 功能组件和类组件之间的差异

| **功能组件** | **类组件** |
| 功能组件是一个 JavaScript/ES6 函数，它接受一个参数、道具并返回 JSX。 | 你必须从 React 扩展才能使用一个类组件。创建一个组件和一个返回 React 元素的呈现函数。 |
| 功能组件中没有使用渲染方法。 | 它必须有 render()方法返回 JSX |
| 功能组件自上而下运行，一旦函数返回就无法保持活动状态。 | 类组件被实例化，各种生命周期方法保持活动状态，根据类组件所处的阶段运行和调用。 |
| 它们也被称为无状态组件，因为它们只接受数据并以某种形式显示，它们主要负责 UI 渲染。 | 它们也被称为有状态组件，因为它们实现逻辑和状态。 |
| React 生命周期方法不能用于功能组件。 | React 生命周期方法可以在类组件内部使用。 |
| 像 `useState()` 这样的钩子被创建用于功能组件中，使它们有状态。 | 在一个类组件内部需要不同的语法来实现钩子。 |
| 不使用构造函数。 | 根据需要使用构造函数来存储状态。 |

## 如何在 React 中使用 CSS

用 CSS 设计 react 应用程序有三种方式:

*   内嵌样式
*   外部造型
*   JS 中的 CSS

阅读本文中关于[如何设计 React 应用程序的更多内容。](https://www.freecodecamp.org/news/how-to-style-react-apps-with-css/)

## React 中的 render()有什么用？

在类组件中使用`Render()`来返回将在组件中显示的 HTML。它用于读取道具和状态，并将我们的 JSX 代码返回给我们的应用程序的根组件。

## 什么是道具？

道具也被称为属性。它们用于将数据从一个组件传输到下一个组件(父组件到子组件)。它们通常用于呈现动态生成的数据。

注意:子组件永远不能向父组件发送属性，因为这个流是单向的(父到子)。

示例:

```
function App({name, hobby}) {
    return (
      <div>
        <h1>My name is {name}.</h1>
        <p>My hobby is {hobby}.</p>
      </div>
    );
}

export default App;
```

点击这里阅读更多关于【props 如何在 React 中工作的信息。

## React 中的状态是什么？

状态是一个内置的 React 对象，用于创建和管理组件中的数据。它与 props 的不同之处在于，它是用来存储数据而不是传递数据的。

状态是可变的(数据可以改变)并且可以通过`this.state()`访问。

示例:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: "John Doe"
    };
  }

  render() {
    return (
      <div>
        <h1>My name is {this.state.name}</h1>
      </div>
    );
  }
} 
```

## 如何在 React 中更新组件的状态

重要的是要知道，当我们直接更新一个状态时，它不会重新呈现组件——这意味着我们看不到更新。

如果我们想让它重新呈现，那么我们必须使用`setState()`方法来更新组件的状态对象并重新定义组件。

示例:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: "John Doe"
    };
  }
  changeName = () => {
    this.setState({name: "Jane Doe"});
  }
  render() {
    return (
      <div>
        <h1>My {this.state.name}</h1>
        <button
          type="button"
          onClick={this.changeName}
        >Change Name</button>
      </div>
    );
  }
}
```

你可以在这里了解更多[。](https://www.freecodecamp.org/news/react-js-for-beginners-props-state-explained/)

## 如何区分状态和道具

状态和道具是具有不同功能的 JavaScript 对象。

Props 用于将数据从父组件传输到子组件，而 state 是一个本地数据存储，只对该组件可用，不能与其他组件共享。

## React 中的事件是什么？

在 React 中，事件是可以由用户操作或系统生成的事件触发的操作。鼠标点击、网页加载、按键、调整窗口大小、滚动和其他交互都是事件的例子。

示例:

```
// For class component
<button type="button" onClick={this.changeName} >Change Name</button>

// For function component
<button type="button" onClick={changeName} >Change Name</button>
```

## 如何在 React 中处理事件

React 中的事件处理方式类似于 DOM 元素。我们必须考虑的一个区别是我们的事件的命名，它是用大写字母而不是小写字母命名的。

示例:

### 类别组件

```
class App extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log('This button was Clicked');
  }
  render() {
    return (
      <div>
         <button onClick={this.handleClick}>Click Me</button>
      </div>
   );
  }
}
```

### 功能组件

```
const App = () => {
   const handleClick = () => {
      console.log('Click happened');
   };
   return (
      <div>
         <button onClick={handleClick}>Click Me</button>
      </div>
   );
};
export default App;
```

## 如何向事件处理程序传递参数

在功能组件中，我们可以这样做:

```
const App = () => {
   const handleClick = (name) => {
      console.log(`My name is ${name}`);
   };
   return (
      <div>
         <button onClick={() => handleClick('John Doe')}>Click Me</button>
      </div>
   );
};
export default App; 
```

在类组件中，我们可以这样做:

```
class App extends React.Component {
  call(name) {
    console.log(`My name is ${name}`);
  }
  render() {
    return (
      <button onClick= {this.call.bind(this,"John Doe")}>
        Click the button!
      </button>
    );
  }
}
export default App;
```

## Redux 是什么？

Redux 是一个流行的开源 JavaScript 库，用于管理和集中应用程序状态。它通常与 React 或任何其他视图库一起使用。

点击了解更多 [redux。](https://www.freecodecamp.org/news/redux-tutorial-for-beginners/#:~:text=Redux%20is%20a%20popular%20open,you%20how%20to%20use%20Redux.)

## 什么是 React 钩子？

React 挂钩是在 v16.8 中添加的，允许我们使用状态和其他 React 特性，而不必编写类。

它们不在类内操作，而是帮助我们从功能组件中挂钩到 React 状态和生命周期特性。

### 我们什么时候开始在 React 中使用钩子的？

React 团队于 2018 年 10 月下旬在 React Conf 期间首次向世界推出 React Hooks，随后于 2019 年 2 月初推出 React v16。8.0.

## 解释 useState()钩子

useState 挂钩是一个存储，它支持在功能组件中使用状态变量。您可以将初始状态传递给该函数，它将返回一个包含当前状态值(不一定是初始状态)的变量和另一个更新该值的函数。

示例:

```
import React, { useState } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      // ...
    </div>
  );
}
```

## 解释 useEffect()钩子

useEffect 钩子允许您在组件中执行副作用，比如数据获取、直接 DOM 更新、setTimeout()之类的计时器等等。

这个钩子接受两个参数:回调和依赖项，这允许您控制何时执行副作用。

注意:第二个参数是可选的。

示例:

```
import React, {useState, useEffect} from 'react';

const App = () => {
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(true);
    setTimeout(() => {
      setLoading(false);
    }, 2000);
  }, []);

  return(
    <div>
      // ...
    </div>
  )
}

export default App;
```

## useMemo()钩子有什么用？

在函数组件中使用`useMemo()`钩子来记忆昂贵的函数，这样它们只在一组输入改变时被调用，而不是每次渲染。

示例:

```
const result = useMemo(() => expensivesunction(input), [input]);
```

这类似于 useCallback 挂钩，用于优化 React 函数组件的呈现行为。useMemo 用于记忆昂贵的函数，这样就不必在每次渲染时都调用它们。

## 什么是 useRefs 钩子？

`useRefs()`钩子，也称为 References 钩子，用于存储更新时不需要重新呈现的可变值。它还用于存储对特定 React 元素或组件的引用，这在我们需要 DOM 度量或直接向组件添加方法时非常有用。

当我们需要做以下事情时，我们使用 useRefs:

*   调整焦点，并在文本和媒体播放之间进行选择。
*   使用第三方 DOM 库。
*   启动命令式动画

示例:

```
import React, {useEffect, useRef} from 'react';

const App = () => {
  const inputRef = useRef(null)

  useEffect(()=>{
    inputElRef.current.focus()
  }, [])

  return(
    <div>
      <input type="text" ref={inputRef} />
    </div>
  )
}

export default App;
```

## 什么是定制挂钩？

定制钩子是一个 JavaScript 函数，它允许您在多个组件之间共享逻辑，这在以前使用 React 组件是不可能的。

如果你有兴趣了解这是如何工作的，这里有一个[一步一步的指南来帮助你构建你自己的定制 React 钩子](https://www.freecodecamp.org/news/how-to-create-react-hooks/)。

## React 中的上下文是什么？

Context 旨在共享 React 组件树的“全局”数据，允许数据在 React 应用程序中我们需要的任何组件中传递和使用(消费),而无需使用 props。它允许我们跨组件更容易地共享数据(状态)。

阅读本指南中关于 [React 上下文的更多信息。](https://www.freecodecamp.org/news/react-context-for-beginners/)

## 什么是 React 路由器？

React router 是 React 应用程序中使用的标准库，用于处理路由并允许在各种组件的视图之间导航。

将这个库安装到 React 项目中非常简单，只需在终端中键入以下命令:

```
npm install – -save react-router-dom
```

## 结论

在本教程中，我们复习了一些 React 面试问题，以帮助你准备面试。我们 FreeCodeCamp 的所有人祝你好运，并在面试中取得成功。

如果你想学得更多，掌握反应能力，以便在实际面试中表现出色，不要试图一次上几门课，找一本有用的教程并完成它。查看 freeCodeCamp 的 2022 年免费 React 课程或 [Learn React -初学者完整课程](https://www.freecodecamp.org/news/learn-react-course/)。