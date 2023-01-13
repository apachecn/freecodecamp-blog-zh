# 构建 React 项目的更好方法

> 原文：<https://www.freecodecamp.org/news/a-better-way-to-structure-react-projects/>

大家好！很多电子墨水已经被用在相对容易的“在 React 中做 X”或者“使用 React 和 X 技术”上。

因此，我想谈谈我在 [DelightChat](https://delightchat.io/) 和我之前的公司从零开始构建前端的经历。

这些项目需要对 React 有更深入的理解，并在生产环境中扩展使用。

如果你想看这个教程的视频版本来补充你的阅读，[你可以在这里做](https://www.youtube.com/watch?v=lViIdphWTwY)。

[https://www.youtube.com/embed/lViIdphWTwY?feature=oembed](https://www.youtube.com/embed/lViIdphWTwY?feature=oembed)

## 介绍

简而言之，一个复杂的 React 项目应该这样构建。虽然我在生产中使用 NextJS，但是这个文件结构在任何 React 设置中都应该非常有用。

```
src
|---adapters
|---contexts
|---components
|---styles
|---pages 
```

*注意:在上面的文件结构中，资产或静态文件应该放在您的框架的* `public *` *文件夹的任何变体中。*

对于上面的每个文件夹，让我们按照优先顺序来讨论它们。

## 1.适配器

**`Adapters`** 是你的应用与外界的连接器。为了与外部服务或客户端共享数据，任何形式的 API 调用或 websocket 交互都应该在适配器内部进行。

有些情况下，一些数据总是在所有适配器之间共享——例如，跨 AJAX (XHR)适配器共享 cookies、基本 URL 和标题。这些可以在 xhr 文件夹中初始化，然后导入到其他适配器中以供进一步使用。

这个结构看起来像这样:

```
adapters
|---xhr
|---page1Adapter
|---page2Adapter 
```

在 axios 的情况下，您可以使用`axios.create`创建一个基本适配器，或者导出这个初始化的实例，或者为 get、post、patch 和 delete 创建不同的函数来进一步抽象它。这看起来像这样:

```
// adapters/xhr/index.tsx

import Axios from "axios";

function returnAxiosInstance() {
  return Axios.create(initializers);
}

export function get(url){
  const axios = returnAxiosInstance();
  return axios.get(url);
}

export function post(url, requestData){
  const axios = returnAxiosInstance();
  return axios.post(url, requestData);
}

... and so on ... 
```

准备好基础文件后，根据应用程序的复杂程度，为每个页面或每组功能创建单独的适配器文件。一个好名字的函数使得理解每个 API 调用做什么和它应该完成什么变得非常容易。

```
// adapters/page1Adapter/index.tsx

import { get, post } from "adapters/xhr";
import socket from "socketio";

// well-named functions
export function getData(){
  return get(someUrl);
}

export function setData(requestData){
  return post(someUrl, requestData);
}

... and so on ... 
```

但是这些适配器有什么用呢？让我们在下一节中找出答案。

## 2.成分

虽然在这一节我们应该讨论上下文，但我想先讨论组件。这是为了理解为什么在复杂的应用程序中需要上下文。

**`Components`** 是你应用程序的生命线。它们将保存应用程序的用户界面，有时还会保存业务逻辑和任何必须维护的状态。

如果一个组件变得太复杂而无法用您的 UI 来表达业务逻辑，那么最好能够将它分成一个单独的 bl.tsx 文件，您的根 index.tsx 从其中导入所有的函数和处理程序。

这个结构看起来像这样:

```
components
|---page1Components
        |--Component1
        |--Component2
|---page2Component
        |--Component1
               |---index.tsx
               |---bl.tsx 
```

在这种结构中，每个页面在组件中有自己的文件夹，所以很容易判断哪个组件影响了什么。

限制组件的范围也很重要。因此，一个组件应该只使用 **`adapters`** 来获取数据，为复杂的业务逻辑创建一个单独的文件，并且只关注 UI 部分。

```
// components/page1Components/Component1/index.tsx

import businessLogic from "./bl.tsx";

export default function Component2() {

  const { state and functions } = businessLogic();

  return {
    // JSX
  }
} 
```

而 BL 文件只导入数据并返回它:

```
// components/page1Components/Component1/bl.tsx

import React, {useState, useEffect} from "react";
import { adapters } from "adapters/path_to_adapter";

export default function Component1Bl(){
  const [state, setState] = useState(initialState);

  useEffect(() => {
    fetchDataFromAdapter().then(updateState);
  }, [])
} 
```

然而，所有复杂的应用程序都有一个共同的问题。状态管理，以及如何跨远程组件共享状态。例如，考虑以下文件结构:

```
components
|---page1Components
        |--Component1
               |---ComponentA
|---page2Component
        |--ComponentB 
```

在上面的例子中，如果某个状态必须在组件 a 和 B 之间共享，它将必须通过所有的中间组件，以及任何其他想要与该状态交互的组件。

为了解决这个问题，有几种方法可以使用，比如 Redux、Easy-Peasy 和 React Context，每种方法都有自己的优缺点。通常，React 上下文应该“足够好”来解决这个问题。我们将所有与上下文相关的文件存储在 **`contexts`** 中。

## 3.上下文

**`contexts`** 文件夹是一个最小的文件夹，仅包含必须在这些组件间共享的状态。每个页面可以有几个嵌套的上下文，每个上下文只向下向前传递数据。但是为了避免复杂性，最好只有一个上下文文件。这个结构看起来像这样:

```
contexts
|---page1Context
        |---index.tsx (Exports consumers, providers, ...)
        |---Context1.tsx (Contains part of the state)
        |---Context2.tsx (Contains part of the state)
|---page2Context
        |---index.tsx (Simple enough to also have state) 
```

在上面的例子中，由于`page1`可能有点复杂，我们通过将子上下文作为子上下文传递给父上下文来允许一些嵌套上下文。然而，一般来说，包含状态和导出相关文件的单个`index.tsx`文件就足够了。

我不会深入 React 状态管理库的实现部分，因为每个库都有自己的优点和缺点。因此，我建议浏览你决定使用的任何教程，以学习他们的最佳实践。

允许上下文从 **`adapters`** 导入，以获取外部效果并对其做出反应。在 React 上下文的情况下，提供者被导入到页面中以跨所有组件共享状态，并且在这些 **`components`** 中使用了类似`useContext`的东西，以便能够利用这些数据。

接下来是最后一块主要拼图， **`pages`** 。

## 4.页

我想避免偏向于一个框架，但是一般来说，有一个特定的文件夹来放置路由级组件是一个很好的实践。

Gatsby & NextJS 强制将所有路线放在一个名为 **`pages`** 的文件夹中。这是一种非常易读的定义路由级组件的方式，在您的 CRA 生成的应用程序中模仿这种方式也会产生更好的代码可读性。

路由的集中位置还可以帮助您利用大多数 ide 的“转到文件”功能，通过使用(Cmd 或 Ctrl) +单击导入来跳转到文件。

这有助于您快速地浏览代码，并且清楚地知道什么属于哪里。它还在 **`pages`** 和 **`components`** 之间设置了一个清晰的区分层次，其中一个页面可以导入一个组件来显示它，而不做任何其他事情，甚至不包括业务逻辑。

但是，可以在页面内部导入上下文提供者，以便子组件可以使用它。或者，对于 NextJS，编写一些服务器端代码，使用 getServerSideProps 或 getStaticProps 将数据传递给组件。

## 5.风格

最后，我们来谈谈风格。虽然我的首选方法是通过使用 CSS-in-JS 解决方案(如 Styled-Components)将样式嵌入到 UI 中，但有时在 CSS 文件中包含一组全局样式会很有帮助。

一个普通的旧 CSS 文件更容易在项目间共享，并且还会影响样式化组件无法访问的组件的 CSS(例如，第三方组件)。

因此，你可以将所有这些 CSS 文件存储在 **`styles`** 文件夹中，并从你希望的任何地方自由地导入或链接它们。

这就是我的想法。如果你想讨论什么或者对如何改进有更多的意见，请随时给我发电子邮件！

如需进一步更新或讨论，您可以在 Twitter 上关注我[这里](https://twitter.com/thewritingdev)。

我上一篇关于 freeCodeCamp 的文章写的是如何通过构建一个 URL shortener 来开始使用 Deno，你可以在这里阅读。