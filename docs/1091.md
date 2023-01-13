# 2022 年的 React 备忘单

> 原文：<https://www.freecodecamp.org/news/the-react-cheatsheet/>

你想尽快跟上 React 的速度吗？

我整理了一份超级有用的备忘单，给你一个 2022 年你需要知道的所有 React 概念的完整概述。

我们开始吧！

## 目录

*   [反应元素](#react-elements)
*   [反应元素属性](#react-element-attributes)
*   [反应元素样式](#react-element-styles)
*   [反应碎片](#react-fragments)
*   [反应成分](#react-components)
*   [反应道具](#react-props)
*   [反应儿童道具](#react-children-props)
*   [反应条件句](#react-conditionals)
*   [反应列表](#react-lists)
*   [反应上下文](#react-context)
*   [反应钩子](#react-hooks)
*   [反应使用状态挂钩](#react-usestate-hook)
*   [反应使用效果挂钩](#react-useeffect-hook)
*   [React useRef Hook](#react-useref)
*   [反应使用上下文挂钩](#react-usecontext)
*   [反应使用回调挂钩](#react-usecallback)
*   [反应使用备忘录挂钩](#react-usememo)

## 反应元素

React 元素的编写就像普通的 HTML 元素一样。您可以在 React 中编写任何有效的 HTML 元素。

```
<h1>My Header</h1>
<p>My paragraph>
<button>My button</button> 
```

我们使用一个叫做 *JSX* 的特性来编写 React 元素。

然而，因为 JSX 实际上只是 JavaScript 函数(而不是 HTML)，所以语法有点不同。

与 HTML 不同，单标记元素(如 img 元素)必须是自结束的。它们必须以正斜杠`/`结尾:

```
<img src="my-image.png" />
<br />
<hr />
```

## 反应元素属性

此外，JSX 要求其属性使用不同的语法。

由于 JSX 实际上是 JavaScript，而 JavaScript 使用 camelcase 命名约定(即“camelCase”)，因此属性的编写不同于 HTML。

最常见的例子是`class`属性，我们写为`className`。

```
<div className="container"></div> 
```

## 反应元素样式

为了应用内联样式，我们不使用双引号("")，而是使用两组花括号。

内联样式不是作为普通字符串编写的，而是作为对象的属性编写的:

```
<h1 style={{ fontSize: 24, margin: '0 auto', textAlign: 'center' }}>My header</h1> 
```

## 反应碎片

React 还为我们提供了一个叫做*片段*的元素。

React 要求所有返回的元素都在一个“父”组件中返回。

例如，我们不能返回两个兄弟元素，比如组件中的 h1 和段落:

```
// this syntax is invalid
function MyComponent() {
  return (
    <h1>My header</h1>
    </p>My paragraph</p>
  );
} 
```

如果我们不想像 div 一样将元素包装在容器元素中，我们可以使用片段:

```
// valid syntax
function MyComponent() {
  return (
    <>
      <h1>My header</h1>
      </p>My paragraph</p>
    </>
  );
} 
```

我们可以用常规或速记的语法写片段: <react.fragment></react.fragment> 或<> >。

## 反应组分

我们可以将元素组织成反应组件。

一个基本的函数组件的编写类似于一个普通的 JavaScript 函数，只是有一些不同。

1.  组件名必须以大写字母开头(即 MyComponent，而不是 myComponent)
2.  与 JavaScript 函数不同，组件必须返回 JSX。

以下是反应函数组件的基本语法:

```
function App() {
  return (
     <div>Hello world!</div>
  );
} 
```

## 反应道具

React 组件可以接受传递给它们的数据，称为 *props* 。

道具从父组件传递到子组件。

在这里，我们将从应用程序向用户组件传递一个属性`name`。

```
function App() {
  return <User name="John Doe" />
}

function User(props) {
  return <h1>Hello, {props.name}</h1>; // Hello, John Doe!
} 
```

Props 是一个对象，所以我们可以在`User`中选择`name`道具来获取它的值。

> 要在 JSX 中嵌入任何动态值(即变量或表达式)，必须用花括号将它括起来。

因为我们只在 props 对象上使用了`name`属性，所以我们可以通过对象析构来简化代码:

```
function App() {
  return <User name="John Doe" />
}

function User({ name }) {
  return <h1>Hello, {name}!</h1>; // Hello, John Doe!
} 
```

任何 JavaScript 值都可以作为道具传递，包括其他元素和组件。

## 反应儿童道具

还可以通过在组件的开始和结束标记之间放置数据来传递 Props。

以这种方式传递的道具被放置在`children`属性上。

```
function App() {
  return (
   <User>
     <h1>Hello, John Doe!</h1>
   </User>
  );
}

function User({ children }) {
  return children; // Hello, John Doe!
} 
```

## 反应条件句

React 组件和元素可以有条件地显示。

一种方法是用 if 语句创建一个单独的返回。

```
function App() {
	const isAuthUser = useAuth();

  if (isAuthUser) {
    // if our user is authenticated, let them use the app
    return <AuthApp />;
  }

  // if user is not authenticated, show a different screen
  return <UnAuthApp />;
} 
```

但是，如果要在 return 语句中编写条件语句，则必须使用解析为值的条件语句。

要使用三元运算符，请用花括号将整个条件括起来。

```
function App() {
	const isAuthUser = useAuth();

  return (
    <>
      <h1>My App</h1>
      {isAuthUser ? <AuthApp /> : <UnAuthApp />}
    </>
  ) 
} 
```

## 反应列表

使用`.map()`功能可以输出 React 组件列表。

允许我们循环访问数据数组并输出 JSX。

这里我们使用 SoccerPlayer 组件输出足球运动员的列表。

```
function SoccerPlayers() {
  const players = ["Messi", "Ronaldo", "Laspada"];

  return (
    <div>
      {players.map((playerName) => (
        <SoccerPlayer key={playerName} name={playerName} />
      ))}
    </div>
  );
} 
```

每当循环一个数据数组时，必须在循环的元素或组件上包含*键* prop。

此外，必须给这个 key prop 一个惟一的值，而不仅仅是一个元素索引。

在上面的例子中，我们使用了一个我们知道是唯一的值，即`playerName`。

## 反应上下文

React context 允许我们在不使用 props 的情况下向组件树传递数据。

道具的问题是，有时我们通过不需要接收它们的组件传递它们。这个问题叫做*道具钻*。

下面是一个将道具传递给不需要它的`Body`组件的过于简单的例子:

```
function App() {
  return (
    <Body name="John Doe" />
  );
} 

function Body({ name }) {
  return (
    <Greeting name={name} />
  );
} 

function Greeting({ name }) {
  return <h1>Welcome, {name}</h1>;
} 
```

> 在使用上下文之前，最好看看我们的组件是否可以更好地组织，以避免通过不需要它的组件传递道具。

为了使用上下文，我们使用 React 中的`createContext`函数。

我们可以用一个放在上下文中的初始值来调用它。

创建的上下文包括一个`Provider`和一个`Consumer`属性，它们都是组件。

我们将提供者包装在组件树中，我们希望将给定的值传递下去。接下来，我们将消费者放在我们想要消费价值的组件中。

```
import { createContext } from 'react';

const NameContext = createContext('');

function App() {
  return (
    <NameContext.Provider value="John Doe">
      <Body />
    <NameContext.Provider>
  );
} 

function Body() {
  return <Greeting />;
} 

function Greeting() {
  return (
    <NameContext.Consumer>
      {name => <h1>Welcome, {name}</h1>}
    </NameContext.Consumer>
  );
} 
```

## 反应钩

React 钩子是在 React 版本 16.8 中引入的，作为一种向 React 功能组件添加可重用的、有状态逻辑的方式。

钩子让我们可以使用所有以前只能在类组件中使用的特性。

此外，我们可以创建自己的定制挂钩，为我们的应用程序提供定制功能。

React 核心库也增加了许多 React 挂钩。我们将涵盖你绝对需要知道的 6 个基本要点:

*   useState
*   使用效果
*   useRef
*   使用上下文
*   使用回调
*   使用编号

## 反应使用状态挂钩

`useState`确实如其所说——它允许我们在函数组件中使用有状态值。

使用 useState 而不是简单的变量，因为当状态更新时，我们的组件会重新呈现，通常是为了显示更新后的值。

像所有的钩子一样，我们在组件的顶部调用`useState`,并可以向它传递一个初始值来放置它的状态变量。

我们对从`useState`返回的值使用数组析构来访问(1)存储的状态和(2)更新该状态的函数。

```
import { useState } from 'react';

function MyComponent() {
  const [stateValue, setStateValue] = useState(initialValue);
} 
```

使用`useState`的一个基本例子是递增计数器。

我们可以从`count`变量中看到当前计数，并且可以通过将`count + 1`传递给`setCount`函数来增加状态。

```
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  function updateCount() {
    setCount(count + 1);
  }

  return <button onClick={updateCount}>Count is: {count}</button>;
} 
```

## 反应使用效果挂钩

如果我们想要与“外部世界”交互，比如使用 API，我们使用`useEffect`钩子。

useEffect 用于执行副作用，这意味着执行存在于我们的应用程序之外的操作，该操作没有可预测的结果。

useEffect 的基本语法要求函数作为第一个参数，数组作为第二个参数。

```
import { useEffect } from 'react';

function MyComponent() {
   useEffect(() => {
     // perform side effect here
   }, []);
} 
```

如果我们想获取数据，我们可以使用`useEffect`，比如获取和显示一个帖子列表:

```
import { useEffect } from 'react';

function PostList() {
	 const [posts, setPosts] = useState([]);

   useEffect(() => {
	   fetch('https://jsonplaceholder.typicode.com/posts')
       .then(response => response.json())
       .then(posts => setPosts(posts));
   }, []);

   return posts.map(post => <Post key={post.id} post={post} />
} 
```

如果我们需要使用一个来自效果函数之外的值，它必须包含在依赖数组中。

如果该值改变，效果函数将重新执行。

例如，这里有一段代码，每当打开或关闭移动菜单时，它都向 body 元素添加或删除“overflow-hidden”类。

```
function Mobile({ open }) {
  useEffect(() => {
    const body = document.querySelector("#__next");

    if (open) {
      body.classList.add("overflow-hidden");
    } else {
      body.classList.remove("overflow-hidden");
    }
  }, [open]);

  // ...
} 
```

## React useRef

允许我们直接访问 JSX 元素。

要使用`useRef`，调用它，获取返回值，并将其放在给定 React 元素的`ref` prop 上。

> 引用在组件上没有内置的道具，只有反应元素。

下面是`useRef`的基本语法:

```
import { useRef } from 'react';

function MyComponent() {
  const ref = useRef();

  return <div ref={ref} />
} 
```

一旦一个 ref 被附加到一个给定的元素上，我们就可以使用存储在`ref.current`上的值来访问元素本身。

例如，如果我们想编写一些代码，当用户使用组合键 Control + K 时，这些代码将聚焦于搜索输入。

```
import { useWindowEvent } from "@mantine/hooks";
import { useRef } from "react";

function Header() {
	const inputRef = useRef();

  useWindowEvent("keydown", (event) => {
    if (event.code === "KeyK" && event.ctrlKey) {
      event.preventDefault();
      inputRef.current.focus();
    }
  });

  return <input ref={inputRef} />
} 
```

## 反应使用上下文

`useContext`提供了一种比使用标准上下文更简单的使用上下文的方法。消费组件。

语法包括将我们想要使用的整个上下文对象传递到`useContext`中。返回值是传递给上下文的值。

```
import { useContext } from 'react';

function MyComponent() {
  const value = useContext(Context);

  // ...
} 
```

使用`useContext`钩子重写我们之前的例子:

```
import { createContext, useContext } from 'react';

const NameContext = createContext('');

function App() {
  return (
    <NameContext.Provider value="John Doe">
      <Body />
    <NameContext.Provider>
  );
} 

function Body() {
  return <Greeting />;
} 

function Greeting() {
	const name = useContext(NameContext);

  return (
    <h1>Welcome, {name}</h1>
  );
} 
```

## 反应使用回调

`useCallback`是一个挂钩，我们用它来帮助我们的应用程序的性能。

具体来说，它防止每次组件重新呈现时都重新创建函数，这会损害应用程序的性能。

如果我们回到前面的`PlayerList`例子，并添加向我们的数组添加玩家的功能，当我们通过 props 传递一个函数来移除他们(`handleRemovePlayer`)时，该函数每次都会被重新创建。

解决这个问题的方法是将我们的回调函数包装在`useCallback`中，并将它的一个参数`player`包含在依赖数组中:

```
function App() {
  const [player, setPlayer] = React.useState("");
  const [players, setPlayers] = React.useState(["Messi", "Ronaldo", "Laspada"]);

  function handleChangeInput(event) {
    setPlayer(event.target.value);
  }
  function handleAddPlayer() {
    setPlayers(players.concat(player));
  }
  const handleRemovePlayer = useCallback(player => {
    setPlayers(players.filter((p) => p !== player));
  }, [players])

  return (
    <>
      <input onChange={handleChangeInput} />
      <button onClick={handleAddPlayer}>Add Player</button>
      <PlayerList players={players} handleRemovePlayer={handleRemovePlayer} />
    </>
  );
}

function PlayerList({ players, handleRemovePlayer }) {
  return (
    <ul>
      {players.map((player) => (
        <li key={player} onClick={() => handleRemovePlayer(player)}>
          {player}
        </li>
      ))}
    </ul>
  );
} 
```

## 反应使用备忘录

`useMemo`是另一个性能挂钩，允许我们“记忆”给定的操作。

记忆化使得我们可以在已经计算过的时候记住昂贵的计算结果，这样我们就不必再做一次了。

像`useEffect`和`useCallback`，`useMemo`接受一个回调函数和一个依赖数组。

然而，与这两个函数不同的是，`useMemo`旨在返回值。

> 您必须使用关键字`return`显式地或者隐式地返回该值，但是使用 arrow 函数速记(见下文)。

`useMemo`的一个真实例子来自 mdx-bundler 文档。`mdx-bundler`是一个用于转换的库。mdx 文件到 React 组件中。

这里，它使用`useMemo`将原始代码字符串转换成 React 组件。

```
import * as React from 'react'
import {getMDXComponent} from 'mdx-bundler/client'

function Post({code, frontmatter}) {
  const Component = React.useMemo(() => getMDXComponent(code), [code]);

  return (
    <>
      <header>
        <h1>{frontmatter.title}</h1>
        <p>{frontmatter.description}</p>
      </header>
      <main>
        <Component />
      </main>
    </>
  )
} 
```

这样做的原因是为了防止组件重新渲染时不必要地重新创建`Component`值。

因此，`useMemo`将只在`code`依赖性改变时执行其回调函数。

## 想进行下一步吗？

如果你喜欢这个备忘单，并且正在寻找学习 React 的终极资源，请查看 **[React 训练营](https://reactbootcamp.com)** 。

它将为您提供所需的所有培训:

*   每天只需 30 分钟，就能从完全的初学者变成专业的反应者
*   从零开始到部署，构建 4 个全栈 React 项目
*   了解构建您喜欢的任何应用程序的强大技术堆栈

[![Click to join the React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](https://reactbootcamp.com) 
*点击加入 React 训练营*