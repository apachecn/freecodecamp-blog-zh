# 2022 年你应该知道的 10 个 React 面试问题

> 原文：<https://www.freecodecamp.org/news/react-interview-questions-to-know/>

对自己的反应知识有信心吗？测试一下！

我已经选择了 2022 年作为 React 开发人员你应该知道的所有主要问题，不管你是不是在面试一个被雇佣的职位。

这些问题涵盖了从 React 的核心概念到何时应该使用某些特性的实际理解的所有内容。

为了从本指南中获得最佳结果，请确保在查看答案之前尝试自己回答每个问题。

我们开始吧！

> 想让第一资源成为 React 雇佣的开发人员吗？使用 [**React 训练营**](https://reactbootcamp.com) 在 4-5 周内成为 React pro。

## 1.什么是反应？为什么要用？

React 是 JavaScript **库**，不是框架。

我们使用 React 是因为它给了我们 JavaScript 的所有功能，但是内置的特性改进了我们构建和考虑构建应用程序的方式。

*   它给了我们一种方法，用像 JSX 这样的工具轻松地创建用户界面
*   它给了我们组件来轻松地共享我们的用户界面(UI)的一部分，这是静态 HTML 本身无法做到的
*   它允许我们用 React 钩子在任何组件上创建可重用的行为
*   当我们的数据改变时，React **负责更新我们的 UI** ,而不需要我们自己手动更新 DOM

**额外奖励**:React 中有一些框架可以为你提供构建应用所需的一切(几乎没有第三方库)，比如 Next.js 和 Gatsby。

React 是专门为构建单页面应用程序而创建的，但你可以用相同的 React 概念创建从静态网站到移动应用程序的任何东西。

## 2.什么是 JSX？

JSX 是一种构建 React 用户界面的方式，它使用 HTML 的简单语法，但增加了 JavaScript 的功能和动态特性。

简而言之，它是用于构建我们的 React 应用程序的 HTML + JavaScript。

虽然 JSX 看起来像 HTML，但实际上它是 JavaScript 函数调用。

如果你在 JSX 写一个`div`，它实际上相当于调用`React.createElement()`。

我们可以通过手动调用`React.createElement`来构建我们的用户界面，但是随着我们添加更多的元素，阅读我们构建的结构变得越来越困难。

**浏览器本身无法理解 JSX，**所以我们经常使用一个叫做 **Babel** 的 JavaScript 编译器，把看起来像 HTML 的东西转换成浏览器能够理解的 JavaScript 函数调用。

## 3.如何将数据传递给 React 组件？

向 React 组件传递数据有两种主要方式:

1.  小道具
2.  上下文 API

属性是从组件的直接父级传递的数据。Props 是在子组件上声明的，可以命名为任何名称，并且可以接受任何有效值。

```
function Blog() {
  const post = { title: "My Blog Post!" };

  return <BlogPost post={post} />;
}
```

道具在子组件中消耗。道具在子对象中总是可用的，作为对象的**属性。**

```
function BlogPost(props) {
  return <h1>{props.post.title}</h1>
}
```

因为 props 是普通的对象属性，所以可以对它们进行析构以获得更直接的访问。

```
function BlogPost({ post }) {
  return <h1>{post.title}</h1>
}
```

上下文是从上下文提供者传递到任何使用上下文的组件的数据。

上下文允许我们在应用程序中的任何地方访问数据(如果提供者在整个组件树中传递)，而不使用 props。

使用`Context.Provider`组件在`value` prop 上传递上下文数据。它可以通过上下文来使用。消费组件或`useContext`挂钩。

```
import { createContext, useContext } from 'react';

const PostContext = createContext()

function App() {
  const post = { title: "My Blog Post!" };

  return (
    <PostContext.Provider value={post}>
      <Blog />
    </PostContext.Provider>
  );
}

function Blog() {
  return <BlogPost />
}

function BlogPost() {
  const post = useContext(PostContext)

  return <h1>{post.title}</h1>
}
```

## 4.状态和道具有什么区别？

状态是我们可以在 React 组件中读取和更新的值。

Props 是传递给 React 组件的**值，并且是只读的**(它们不应该被更新)。

您可以将 props 看作是存在于组件外部的函数的参数，而 state 是随时间变化的值，但它存在于组件内部并在其中声明。

状态和道具是相似的，因为对它们的更改会导致它们所在的组件重新呈现。

## 5.React 片段有什么用？

React 片段是 React 中的一个特殊特性，它允许您编写分组子元素或组件，而无需在 DOM 中创建实际的节点。

片段语法看起来像一组空的标签`<></>`或者标签为`React.Fragment`的标签。

更简单地说，有时我们需要将多个 React 元素放在单个父元素下，但我们不想使用像`div`这样的通用 HTML 元素。

例如，如果您正在编写一个表格，这将是无效的 HTML:

```
function Table() {
  return (
    <table>
      <tr>
        <Columns />
      </tr>
    </table>
  );
}

function Columns() {
  return (
    <div>
      <td>Column 1</td>
      <td>Column 2</td>
    </div>
  );
} 
```

我们可以通过在我们的`Columns`组件中使用片段而不是`div`元素来避免这个问题。

```
function Columns() {
  return (
    <>
      <td>Column 1</td>
      <td>Column 2</td>
    </>
  );
}
```

选择片段的另一个原因是，有时添加额外的 HTML 元素可能会改变我们应用 CSS 样式的方式。

## 6.为什么我们需要 React 列表的键？

键是一个惟一的值，当我们使用`.map()`函数遍历一个元素或组件时，我们必须将它传递给`key`属性。

如果我们映射一个元素，它看起来会像这样:

```
posts.map(post => <li key={post.id}>{post.title}</li>)
```

或者像这样，如果我们映射一个组件:

```
posts.map(post => <li key={post.id}>{post.title}</li>)
```

在这两种情况下，我们都需要添加一个惟一值键，否则 React 会警告我们。

为什么？因为**键告诉 React 列表中的哪个元素或组件是哪个**。

否则，如果我们试图通过插入更多的条目或以某种方式编辑它们来改变列表中的条目，React 将不知道它们的排列顺序。

这是因为 React 为我们处理了所有更新 DOM 的事务(使用虚拟 DOM)，但是对于 React 来说，**键是正确更新它所必需的**。

## 7.什么是裁判？你如何使用它？

ref 是对 React 中 DOM 元素的**引用。**

Refs 是在`useRef`钩子的帮助下创建的，可以立即放入变量中。

然后将这个变量传递给给定的 React 元素(不是组件)，以获得对底层 DOM 元素(即 div、span 等)的引用。

元素本身及其属性现在可以在**上获得。引用的当前属性**。

```
import { useRef } from 'react'

function MyComponent() {
  const ref = useRef();

  useEffect(() => {
    console.log(ref.current) // reference to div element
  }, [])

  return <div ref={ref} />
}
```

Refs 通常被称为能够直接使用 DOM 元素的“出口”。它们允许我们做一些通过 React 无法完成的操作，比如清除或聚焦输入。

## 8.useEffect 钩子是用来做什么的？

`useEffect`钩子用于在我们的 React 组件中执行副作用。

**副作用**是通过“外部世界”或 React 应用程序环境之外的东西执行的操作。

副作用的一些例子包括向外部 API 端点发出 GET 或 POST 请求，或者使用类似于`window.navigator`或`document.getElementById()`的浏览器 API。

我们不能在 React 组件的主体中直接执行这样的操作。给我们一个在其中执行副作用的函数和一个列出该函数所依赖的任何外部值的依赖数组。

如果 dependencies 数组中的任何值发生变化，effect 函数将再次运行。

## 9.什么时候使用 React Context vs Redux？

> Redux 可能是 React 最常用的第三方全局状态库，但是您可以将单词“Redux”替换为 React 的任何全局状态库。

React context 是一种在我们的应用程序**中提供和消费数据的方式，而不需要使用 props** 。

React context 有助于我们防止" **props drilling** "的问题，即当你通过不需要它的组件传递带有 props 的数据时。

相反，有了上下文，我们可以**在需要数据的组件中准确地消费数据**。

虽然我们在应用程序中只使用上下文来获取或“读取”全局值，但 Redux 和其他第三方状态库**允许我们读取和更新状态**。

Context 不是 Redux 等第三方状态库的替代品，因为它不是为状态更新而构建的。这是因为每当 Context 上提供的值发生变化时，它的所有子元素都会重新呈现，这会影响性能。

## 10.useCallback & useMemo 挂钩有什么用途？

`useCallback`和`useMemo`挂钩的存在是为了提高我们组件的性能。

`useCallback`是为了防止在函数组件体内声明的函数在每次渲染时被重新创建。

这可能会导致不必要的性能问题，尤其是对于传递给子组件的回调函数。

另一方面，我们给它做了一个昂贵的手术。

**记忆化**是一个技术术语，指的是那些能够“记住”过去的值的函数，如果它们的自变量没有改变的话。如果是，该函数将返回“记忆的”值。

换句话说，我们可能有一个占用大量计算资源的计算，我们希望它尽可能少地执行。

在这种情况下，我们使用`useMemo`钩子，它与`useCallback`钩子的不同之处在于它返回一个值，而不是一个函数。

## 想让 React 变得简单吗？

如果你想以最简单的方式成为一名 React 开发人员，请查看 React 训练营 。

它将为您提供所需的所有技能:

*   每天只需 30 分钟，就能从完全的初学者变成专业的反应者
*   从零开始到部署，构建 4 个全栈 React 项目
*   了解构建您喜欢的任何应用程序的强大技术堆栈

[![Click to join the React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](https://reactbootcamp.com) 
*点击加入 React 训练营*