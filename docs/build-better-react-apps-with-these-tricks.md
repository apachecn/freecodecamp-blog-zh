# 用这些简单的技巧构建更好的 React 应用

> 原文：<https://www.freecodecamp.org/news/build-better-react-apps-with-these-tricks/>

这里有一个惊人的技巧列表，你可以用它来立即改进你的 React 应用程序。

这些技巧不仅会让你的代码更干净、更可靠，还会让你的开发体验更简单、更愉快。

今天就在 React 项目中尝试这些技术吧！

> 想要从前端到后端成为 React 开发专家的完整指南吗？查看 [**反应训练营**](https://bit.ly/join-react-bootcamp) 。

## 用 React 查询替换 Redux

随着应用程序变得越来越大，跨组件管理状态变得越来越困难。因此，您可能需要像 Redux 这样的状态管理库。

如果您的应用程序依赖于从 API 获得的数据，您通常使用 Redux 来获取服务器状态，然后更新您的应用程序状态。

这可能是一个具有挑战性的过程——您不仅需要获取数据，还需要处理不同的状态，这取决于您是拥有数据还是处于加载或错误状态。

不要使用 Redux 来管理从服务器获得的数据，而是使用 React Query 这样的库。

首先，React Query 通过有用的钩子，让您能够更好地控制 React 应用程序中的 HTTP 请求，并能够轻松地重新提取数据。它还使您能够无缝地管理应用程序组件的状态，通常无需自己手动更新状态。

下面是如何在 index.js 文件中设置 React Query:

```
import { QueryClient, QueryClientProvider } from 'react-query'
import ReactDOM from "react-dom";

import App from "./App";

const queryClient = new QueryClient()

const rootElement = document.getElementById("root");
ReactDOM.render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>,
  rootElement
);
```

这里，我们设置了一个查询客户机，它将为我们设置一个缓存，以便毫不费力地管理我们过去发出的任何请求。我们还设置了一个查询客户机提供程序组件，将它传递给整个组件树。

### 如何使用 React Query 发出请求

您可以使用 useQuery 钩子发出请求，它为您的查询获取一个标识符(在本例中，因为我们正在获取用户数据，所以我们将它称为“user”)，外加一个用于获取该数据的函数。

```
import { useQuery } from "react-query";

export default function App() {
  const { isLoading, isError, data } = useQuery("user", () =>
    fetch("https://randomuser.me/api").then((res) => res.json())
  );

  if (isLoading) return "Loading...";
  if (isError) return "Error!";

  const user = data.results[0];
  return user.email;
}
```

如您所见，React Query 负责管理我们获取数据时可能发生的各种状态。我们不再需要自己管理这些状态，我们可以从从`useQuery`返回的内容中析构它们。

useQuery 的状态管理部分在哪里发挥作用？

现在我们已经获取了用户数据，并将其存储在我们的内部缓存中，为了能够跨任何其他组件使用它，我们需要做的就是用我们与之关联的“user”键调用`useQuery()`:

```
import { useQuery } from "react-query";

export default function OtherComponent() {
  const { data } = useQuery('user');

  console.log(data);
}
```

## 使用自定义挂钩使反应上下文更容易

React Context 是跨组件树传递数据的好方法。它允许我们将数据传递到我们喜欢的任何组件中，而不必使用道具。

为了在 React 函数组件中使用上下文，我们使用了`useContext`钩子。

然而，这样做有一点不好。在我们想要使用上下文传递的数据的每个组件中，我们必须导入创建的上下文对象和 import React 来获取 useContext 挂钩。

我们可以简单地创建一个自定义的 React 钩子，而不是每次想从上下文中读取时都必须编写多个 import 语句。

```
import React from "react";

const UserContext = React.createContext();

function UserProvider({ children }) {
  const user = { name: "Reed" };
  return <UserContext.Provider value={user}>{children}</UserContext.Provider>;
}

function useUser() {
  const context = React.useContext(UserContext);
  if (context === undefined) {
    throw new Error("useUser in not within UserProvider");
  }
  return context;
}

export default function App() {
  return (
    <UserProvider>
      <Main />
    </UserProvider>
  );
}

function Main() {
  const user = useUser();

  return <h1>{user.name}</h1>; // displays "Reed"
}
```

在本例中，我们在自定义 UserProvider 组件上传递用户数据，该组件接受一个用户对象并包装在主组件周围。

我们有一个`useUser`钩子来更容易地使用上下文。我们只需要导入钩子本身，就可以在我们喜欢的任何组件中使用我们的用户上下文，比如我们的主组件。

## 管理自定义组件中的上下文提供程序

在您创建的几乎任何 React 应用程序中，您都需要许多上下文提供者。

我们不仅需要为我们正在创建的 React 上下文提供上下文，还需要依赖它的第三方库(如 React Query)提供上下文提供者，以便将它们的工具传递给需要它们的组件。

一旦你开始在 React 项目上工作一段时间，它看起来会是这样的:

```
ReactDOM.render(
  <Provider3>
    <Provider2>
      <Provider1>
        <App />
      </Provider1>
    </Provider2>
  </Provider3>,
  rootElement
);
```

我们能对这种混乱做些什么呢？

我们可以创建一个名为 context providers 的组件，而不是将所有的上下文提供程序放在 App.js 文件或 index.js 文件中。

这允许我们使用 children prop，然后我们所要做的就是将所有这些提供者放入这个组件中:

```
src/context/ContextProviders.js

export default function ContextProviders({ children }) {
  return (
    <Provider3>
      <Provider2>
        <Provider1>
          {children}
        </Provider1>
      </Provider2>
    </Provider3>
  );
}
```

然后，围绕 App 包装 ContextProviders 组件:

```
src/index.js

import ReactDOM from "react-dom";
import ContextProviders from './context/ContextProviders'
import App from "./App";

const rootElement = document.getElementById("root");
ReactDOM.render(
  <ContextProviders>
    <App />
  </ContextProviders>,
  rootElement
);
```

## 使用“对象扩散”操作符更容易传递道具

当涉及到使用组件时，我们通常在 props 的帮助下传递数据。我们创建一个适当名称，并将其设置为与其相应的值相等。

但是，如果我们有很多需要传递给一个组件的道具，我们需要单独列出它们吗？

不，我们没有。

一种非常简单的方法是使用`{...props}`模式，它能够传递我们喜欢的所有属性，而不必写下所有属性的名称和它们相应的值。

这涉及到将我们所有的道具数据放在一个对象中，并将所有这些道具分别传播给我们想要传递给它的组件:

```
export default function App() {
  const data = {
    title: "My awesome app",
    greeting: "Hi!",
    showButton: true
  };

  return <Header {...data} />;
}

function Header(props) {
  return (
    <nav>
      <h1>{props.title}</h1>
      <h2>{props.greeting}</h2>
      {props.showButton && <button>Logout</button>}
    </nav>
  );
}
```

## 用反应片段映射片段

React 中的`.map()`函数允许我们获取一个数组并遍历它，然后在某个 JSX 中显示每个元素的数据。

然而，在某些情况下，我们希望迭代该数据，但不希望在结束的 JSX 元素中返回它。也许使用封闭的 JSX 元素会修改我们的应用程序，或者我们只是不想在 DOM 中添加另一个元素。

一个鲜为人知的技巧是使用`React.Fragment`来迭代一组数据，并且不将父元素作为 HTML 元素。

使用 React 片段的手写形式可以为它提供`key`属性，这是我们正在迭代的任何元素所需要的。

```
import React from 'react'

export default function App() {
  const users = [
    {
      id: 1,
      name: "Reed"
    },
    {
      id: 2,
      name: "John"
    },
    {
      id: 3,
      name: "Jane"
    }
  ];

  return users.map((user) => (
    <React.Fragment key={user.id}>{user.name}</React.Fragment>
  ));
}
```

请注意，我们不能使用必需的`key`属性作为速记片段的替代属性:`<></>`。

## 想要更多吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 是为了让你成为一名超级巨星，一名准备好工作的 React 开发人员而创建的，它包含视频、备忘单和更多内容。

获得内幕信息**100 名开发人员**已经成为 React 专家，找到他们梦想的工作，掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*