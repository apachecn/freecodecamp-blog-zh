# 如何在 GraphQL API 中使用 React 钩子来管理状态

> 原文：<https://www.freecodecamp.org/news/hooks-and-graphql/>

在这篇博文中，我们将学习-

1.  什么是反应钩子
2.  如何使用钩子进行状态管理

在我们开始使用钩子之前，让我们花一点时间来讨论一下状态管理。

**状态管理**是管理在我们的应用程序组件之间流动的数据。它可以是一个组件内部的数据流(本地状态)，也可以是多个组件之间的数据流(共享状态)。我们需要管理状态，因为有时组件需要通过可靠的信息源相互通信。在 Redux 中，这种可靠的信息源被称为商店。

## 第 1 部分:React 钩子——什么和为什么

### 什么是钩子？

钩子是让你不用类组件就能访问状态的函数。钩子是思考 React 的一种更自然的方式。现在，您可以编写考虑如何以及何时需要使用数据的组件，而不是考虑使用什么生命周期方法。

React hooks 于 2018 年 10 月推出，2019 年 2 月发布。React 16.8 及更高版本现已提供。React 挂钩一经推出就非常受欢迎。

### 为什么 React 挂钩这么受欢迎？

1.  没有样板文件:要使用钩子，您不需要导入任何新的库或者编写任何样板文件代码。在 React 16.8 及更高版本中，您可以简单地开始使用现成的钩子。
2.  不需要使用类组件来使用状态:传统上，如果您正在使用一个功能组件，并决定该组件需要 React 状态，您必须将其转换为 React 类组件。通过增加挂钩，您可以在功能组件中使用 React 状态。
3.  React 的更合乎逻辑的思考方式:你不再需要考虑 React 何时挂载一个组件，你应该在`componentDidMount`中做什么，并且记得在`componentWillUnmount`中清理它。现在，您可以更直接地考虑组件如何使用数据。React 负责为您处理挂载和清理功能。

### 有哪些常见的挂钩？

#### 1\. useState

useState 用于设置和更新类组件中的状态，如`this.state`。

```
const [ state, setState] = useState(initialState); 
```

#### 2.使用效果

useEffect 用于执行具有副作用的功能。副作用可能包括 DOM 操作、订阅和 API 调用。

```
useEffect(() => {
  document.title = 'New Title' 
}); 
```

#### 3\. useReducer

useReducer 的工作方式类似于 Redux 中 Reducer 的工作方式。当状态更复杂时，使用 useReducer。实际上，您可以使用 useReducer 来完成使用 useState 所做的任何事情。除了状态变量之外，它还提供了一个调度函数。

```
const [ state, dispatch ] = useReducer(reducer, initialArg, init); 
```

#### 4.使用上下文

useContext 类似于上下文 API。在上下文 API 中，有提供者方法和消费者方法。类似地，对于 useContext，使用最近的提供者方法来读取数据。

```
const value = useContext(MyContext); 
```

关于这些如何工作的更多详细解释，请阅读[官方文件](https://reactjs.org/docs/hooks-reference.html#usestate)。

## 第 2 部分:如何使用钩子进行状态管理

使用 React 16.8，您可以使用现成的钩子。

我们将构建一个应用程序来制作歌曲列表。这是它将要做的-

1.  获取歌曲列表的 GraphQL API，并将其呈现在 UI 上。
2.  有能力添加一首歌曲到列表中。
3.  当歌曲被添加到列表中时，在前端更新列表，在后端存储数据。

顺便说一下，所有代码都可以在 [my GitHub](https://github.com/shrutikapoor08/hooks-graphql) 上找到。要看到这一点，你可以去[这个代码沙箱](https://codesandbox.io/embed/github/shrutikapoor08/hooks-graphql/tree/master/)。

我们将在这个应用程序中使用 GraphQL API，但是您也可以使用 REST API 来完成以下步骤。

### 步骤 0:主要要点

这里的主要思想是我们将使用`context`将数据向下传递给我们的组件。我们将使用钩子，`useContext`和`useReducer`，来读取和更新这个状态。我们将使用`useState`来存储任何本地状态。对于调用 API 这样的副作用，我们将使用`useEffect`。

我们开始吧！

### 步骤 1:设置上下文

```
import { createContext } from 'react';

const Context = createContext({
  songs: []
});

export default Context
```

### 第二步:初始化你的状态。称之为初始状态

我们将使用中的上下文来初始化我们的状态:

```
 const initialState = useContext(Context); 
```

### 步骤 3:使用 useReducer 设置减速器

现在，我们将在 App.js 中设置初始状态为`useReducer`的 reducers:

```
 const [ state, dispatch ] = useReducer(reducer, initialState); 
```

### 第四步:找出哪个是顶层组件。

我们需要在这里设置一个上下文提供者。对于我们的例子，它将是`App.js`。此外，在此处传入从 useReducer 返回的分派，以便孩子们可以访问分派:

```
 <Context.Provider value={state,dispatch}>
    // children components
      <App />
  <Context.Provider value={state,dispatch}> 
```

### 步骤 5:现在使用 useEffect 挂钩来连接 API

```
 const {state, dispatch} = useContext(Context);

  useEffect(() => {
      if(songs) {
          dispatch({type: "ADD_CONTENT", payload: songs});
      }
  }, [songs]); 
```

### 步骤 6:更新状态

您可以使用`useContext`和`useReducer`来接收和更新全局应用程序状态。对于像表单组件这样的本地状态，可以使用`useState`。

```
 const [artist, setArtist] = useState("");

  const [lyrics, setLyrics] = useState(""); 
```

就是这样！国家管理现已建立。

你学到新东西了吗？有什么要分享的吗？在推特上给我发微博。

> DevJoke 万圣节版时间！
> 
> 问:嘿官！
> 黑客是如何逃脱的？
> 
> 答:他们只是勒索软件。[# dev joke](https://twitter.com/hashtag/devjoke?src=hash&ref_src=twsrc%5Etfw)[#万圣节](https://twitter.com/hashtag/Halloween?src=hash&ref_src=twsrc%5Etfw)
> 
> — Shruti Kapoor @ #GraphQLSummit (@shrutikapoor08) [October 31, 2019](https://twitter.com/shrutikapoor08/status/1189975126705504256?ref_src=twsrc%5Etfw)