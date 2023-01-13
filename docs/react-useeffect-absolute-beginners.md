# React useEffect 挂钩，适合绝对初学者

> 原文：<https://www.freecodecamp.org/news/react-useeffect-absolute-beginners/>

如果你理解 useEffect 挂钩有困难，你并不孤单。

初学者和有经验的开发人员都发现这是最难理解的问题之一，因为它需要理解一些不熟悉的编程概念。

在这个快速指南中，我们将介绍为什么这个钩子存在，如何更好地理解它，以及如何在您今天的 React 项目中正确使用它。

> 想要学习 React 的第一方法吗？每天使用 [**React 训练营**](https://reactbootcamp.com) 在 30 分钟内成为 React pro。

## 为什么叫 useEffect？

当 2018 年核心的 React 钩子(useState、useEffect 等等)被添加到库中时，很多开发者都被这个钩子的名字搞糊涂了:“useEffect”。

究竟什么是“效果”？

effect 这个词指的是一个叫做**“副作用”**的函数式编程术语。

但是为了真正理解什么是副作用，我们首先必须掌握一个**纯函数**的概念。

您可能不知道，大多数 React 组件都是纯函数。

将 React 组件视为函数可能很奇怪，但它们确实如此。

看到像 JavaScript 函数一样声明一个常规的 React 函数组件很有帮助:

```
function MyReactComponent() {}
```

大多数 React 组件都是纯函数，这意味着它们接收输入并产生 JSX 的可预测输出。

JavaScript 函数的输入是参数。然而，React 组件的输入是什么呢？**道具**！

这里我们有一个声明了属性`name`的`User`组件。在`User`中，属性值显示在标题元素中。

```
export default function App() {
  return <User name="John Doe" />   
}

function User(props) {
  return <h1>{props.name}</h1>; // John Doe
}
```

这是纯粹的，因为给定相同的输入，它将**总是**返回相同的输出。

如果我们传递给`User`一个值为“John Doe”的`name`道具，我们的输出将始终是 John Doe。

你可能会说，“谁在乎呢？为什么我们要给它起一个名字？”

纯函数的最大好处是**可预测，** **可靠，并且易于测试。**

这是与我们需要在组件中执行副作用相比而言的。

## React 有什么副作用？

副作用是不可预测的，因为它们是与“外部世界”一起执行的动作

当我们需要在 React 组件之外做一些事情时，我们会产生一个副作用。然而，产生副作用不会给我们一个可预测的结果。

想象一下，如果我们从一个发生故障的服务器请求数据(比如博客文章),而不是我们的文章数据，给我们一个 500 状态代码响应。

除了最简单的应用之外，几乎所有的应用都依赖副作用以某种方式工作。

常见的副作用包括:

*   向 API 请求来自后端服务器的数据
*   与浏览器 API 交互(即直接使用`document`或`window`)
*   使用不可预测的计时功能，如`setTimeout`或`setInterval`

这就是 useEffect 存在的原因:提供一种方法来处理在纯 React 组件中执行这些副作用。

例如，如果我们想改变 title meta 标签以在他们的浏览器标签中显示用户的名字，我们可以在组件本身中这样做，但是我们不应该这样做。

```
function User({ name }) {
  document.title = name; 
  // This is a side effect. Don't do this in the component body!

  return <h1>{name}</h1>;   
}
```

如果我们直接在组件体中执行副作用，它会妨碍 React 组件的渲染。

副作用应该与渲染过程分开。如果我们需要执行一个副作用，它应该严格地在我们的组件渲染完成后完成。

这就是 useEffect 给我们的。

简而言之， **useEffect 是一个工具，它让我们与外部世界进行交互，但不影响它所在的组件的渲染或性能。**

## 如何使用 useEffect？

useEffect 的基本语法如下:

```
// 1\. import useEffect
import { useEffect } from 'react';

function MyComponent() {
  // 2\. call it above the returned JSX  
  // 3\. pass two arguments to it: a function and an array
  useEffect(() => {}, []);

  // return ...
}
```

在我们的`User`组件中执行副作用的正确方法如下:

1.  我们从“反应”中导入`useEffect`
2.  在我们的组件中，我们将它称为“返回的 JSX 之上”
3.  我们传递给它两个参数:一个函数和一个数组

```
import { useEffect } from 'react';

function User({ name }) {
  useEffect(() => {
    document.title = name;
  }, [name]);

  return <h1>{name}</h1>;   
}
```

传递给 useEffect 的函数是一个回调函数。这将在组件呈现后调用。

在这个功能中，我们可以执行我们的副作用或多个副作用，如果我们想要的话。

第二个参数是一个数组，称为 dependencies 数组。这个数组应该包含副作用依赖的所有值。

在上面的例子中，由于我们根据外部作用域`name`中的值来更改标题，所以我们需要将它包含在依赖关系数组中。

这个数组要做的是检查并查看一个值(在这个例子中是 name)在两次渲染之间是否发生了变化。如果是，它将再次执行我们的使用效果函数。

这是有意义的，因为如果名称改变，我们希望显示改变的名称，因此再次运行我们的副作用。

## 如何用 useEffect 修复常见错误

有一些微妙的细节需要注意，以避免使用 useEffect 的错误。

如果你不提供依赖数组，只提供一个函数来使用 Effect，**它将在每次渲染后运行**。

当您试图在 useEffect 钩子内更新状态时，这可能会导致问题。

如果您忘记正确提供依赖项，并且在状态更新时设置了一个本地状态，React 的默认行为是重新呈现组件。因此，由于 useEffect 在没有依赖数组的情况下在每次渲染后运行，我们将有一个无限循环。

```
function MyComponent() {
  const [data, setData] = useState([])  

  useEffect(() => {
    fetchData().then(myData => setData(myData))
    // Error! useEffect runs after every render without the dependencies array, causing infinite loop
  }); 
}
```

在第一次渲染后，将运行 useEffect，状态将被更新，这将导致重新渲染，这将导致 useEffect 再次运行，无限地再次开始该过程。

这被称为**无限循环**，这实际上破坏了我们的应用程序。

如果要在 useEffect 中更新状态，请确保提供一个空的依赖关系数组。如果您确实提供了一个空数组(我建议您在使用 useEffect 时默认这样做),这将导致 Effect 函数在组件第一次呈现后只运行一次。

一个常见的例子是获取数据。对于一个组件，您可能只想获取一次数据，将它放入状态，然后在您的 JSX 中显示它。

```
function MyComponent() {
  const [data, setData] = useState([])  

  useEffect(() => {
    fetchData().then(myData => setData(myData))
    // Correct! Runs once after render with empty array
  }, []); 

  return <ul>{data.map(item => <li key={item}>{item}</li>)}</ul>
}
```

## useEffect 中的清理功能是什么？

在 React 中正确执行副作用的最后一部分是**效果清理函数**。

有时我们的副作用需要被关闭。例如，如果您有一个使用`setInterval`功能的倒计时器，除非我们使用`clearInterval`功能，否则该间隔不会停止。

另一个例子是通过 WebSockets 使用订阅。当我们不再使用订阅时，需要将其“关闭”,这就是清理功能的作用。

如果我们使用`setInterval`设置状态，并且副作用没有被清除，当我们的组件被卸载并且我们不再使用它时，状态随着组件被破坏——但是`setInterval`函数将继续运行。

```
function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    setInterval(() => setTime(1), 1000); 
    // counts up 1 every second
    // we need to stop using setInterval when component unmounts
  }, []);
}
```

如果组件被破坏，问题是`setInterval`将尝试更新一个不再存在的状态变量`time`。这是一个叫做内存泄漏的错误。

要使用 cleanup 函数，我们需要从 useEffect 函数中返回一个函数。

在这个函数中，我们可以执行清理，在这个例子中使用`clearInterval`并停止`setInterval`。

```
function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    let interval = setInterval(() => setTime(1), 1000); 

    return () => {
      // setInterval cleared when component unmounts
      clearInterval(interval);
    }
  }, []);
}
```

当组件被卸载时，将调用清理函数。

组件被卸载的一个常见例子是在我们的应用程序中进入一个新的页面或新的路径，在那里组件不再被呈现。

当一个组件被卸载时，我们的清理函数运行，我们的间隔被清除，我们不再得到一个试图更新一个不存在的状态变量的错误。

最后，并不是在所有情况下都需要清除副作用。只有在少数情况下才需要它，例如当您的组件卸载时需要停止重复的副作用。

## 想真正学习 React？

如果你喜欢这样有帮助的课程，并且正在寻找学习 React 的终极资源，请查看 React 训练营 。

它将为您提供所需的所有培训:

*   每天只需 30 分钟，就能从完全的初学者变成专业的反应者
*   从零开始到部署，构建 4 个全栈 React 项目
*   了解构建您喜欢的任何应用程序的强大技术堆栈

[![Click to join the React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](https://reactbootcamp.com) 
*点击加入 React 训练营*