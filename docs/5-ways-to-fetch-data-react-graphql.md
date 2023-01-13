# 如何从 GraphQL API 获取 React 中的数据

> 原文：<https://www.freecodecamp.org/news/5-ways-to-fetch-data-react-graphql/>

让我们来看一下使用 React 从 GraphQL API 获取数据的五种最佳方式。

虽然有一些流行的库可以与来自 React 应用程序的 GraphQL APIs 进行交互，但是使用 GraphQL 获取数据有许多不同的方式。

我包含了一些代码示例，向您展示了如何用尽可能短的代码获取或“查询”数据，以及如何使用这些不同的方法连接 React 和 GraphQL。

> 想用 React 和 GraphQL 从前到后构建令人惊叹的应用吗？查看 [React 训练营](https://reactbootcamp.com/)。

## 入门指南

在这些例子中，我们将使用 SpaceX GraphQL API 来获取和显示 SpaceX 过去进行的 10 次任务。

如果您试图将 React 应用程序与 GraphQL API 连接，请随意使用下面的代码。在这些例子中，我们将从最先进的 GraphQL 客户端库 React 到查询 GraphQL 端点的最简单方法。

## 1.阿波罗客户端

最流行和最全面的 GraphQL 库是 Apollo Client。

你不仅可以用它通过 GraphQL 获取远程数据，就像我们在这里做的那样，它还允许我们通过内部缓存和整个状态管理 API 来本地管理数据。

要开始使用 Apollo 客户机，您需要安装主要的 Apollo 客户机依赖项，以及 GraphQL:

```
npm install @apollo/client graphql
```

Apollo 客户机背后的想法是，它将在您的整个应用程序中使用。为此，您将使用一个特殊的 Apollo Provider 组件将创建的 Apollo 客户机传递到整个组件树中。

当您创建 Apollo 客户端时，您需要指定一个`uri`值，即 GraphQL 端点。此外，您需要指定一个缓存。

Apollo 客户端自带内存缓存，用于缓存或本地存储和管理您的查询及其相关数据:

```
import React from "react";
import ReactDOM from "react-dom";
import { ApolloProvider, ApolloClient, InMemoryCache } from "@apollo/client";

import App from "./App";

const client = new ApolloClient({
  uri: "https://api.spacex.land/graphql/",
  cache: new InMemoryCache()
});

const rootElement = document.getElementById("root");
ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  rootElement
);
```

一旦在应用程序组件中设置了提供者和客户端，您就可以使用 Apollo Client 为所有不同的 GraphQL 操作提供的所有不同的 React 挂钩。这些包括查询、突变和订阅。您甚至可以使用名为`useApolloClient`的定制钩子直接使用创建的 Apollo 客户端。

因为您只是在这里查询数据，所以您将使用`useQuery`钩子。

您将包含一个 GraphQL 查询作为编写查询的第一个参数。您将使用函数`gql`，它可以做很多事情，比如为您提供编辑器语法高亮显示和自动格式化功能，如果您为您的项目使用更漂亮的工具的话。

一旦执行了这个查询，就会得到值`data`、`loading`和`error`:

```
import React from "react";
import { useQuery, gql } from "@apollo/client";

const FILMS_QUERY = gql`
  {
    launchesPast(limit: 10) {
      id
      mission_name
    }
  }
`;

export default function App() {
  const { data, loading, error } = useQuery(FILMS_QUERY);

  if (loading) return "Loading...";
  if (error) return <pre>{error.message}</pre>

  return (
    <div>
      <h1>SpaceX Launches</h1>
      <ul>
        {data.launchesPast.map((launch) => (
          <li key={launch.id}>{launch.mission_name}</li>
        ))}
      </ul>
    </div>
  );
}
```

在你显示你的数据，你的任务之前，你需要处理加载状态。当您处于加载状态时，您从远程端点获取查询。

您还希望处理发生的任何错误。您可以通过在查询中犯语法错误来模拟错误，例如查询不存在的字段。为了处理该错误，您可以方便地返回并显示来自`error.message`的消息。

## 2.Urql

另一个连接 React 应用程序和 GraphQL APIs 的全功能库是 urql。

它试图为您提供 Apollo 拥有的许多特性和语法，同时体积更小，需要的设置代码更少。如果您愿意，它可以为您提供缓存功能，但是它不像 Apollo 那样包含集成的状态管理库。

要使用 urql 作为您的 GraphQL 客户端库，您需要安装 urql 和 GraphQL 包。

```
npm install urql graphql
```

就像 Apollo 一样，您将希望使用专用的 Provider 组件，并使用 GraphQL 端点创建一个客户机。请注意，您不需要指定现成的缓存。

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { createClient, Provider } from 'urql';

const client = createClient({
  url: 'https://api.spacex.land/graphql/',
});

const rootElement = document.getElementById("root");
ReactDOM.render(
  <Provider value={client}>
    <App />
  </Provider>,
  rootElement
);
```

与 Apollo 非常相似，urql 为您提供了处理所有标准 GraphQL 操作的定制挂钩，因此具有相似的名称。

同样，您可以使用 urql 包中的`useQuery`钩子。虽然不需要函数`gql`，但是您可以放弃它，只使用模板文字来编写您的查询。

当调用`useQuery`时，你得到一个数组，你可以把它作为一个数组而不是一个对象来析构。这个数组中的第一个元素是一个名为`result`的对象，它提供了许多可以析构的属性:`data`、`fetching`和`error`。

```
import React from "react";
import { useQuery } from 'urql';

const FILMS_QUERY = `
  {
    launchesPast(limit: 10) {
      id
      mission_name
    }
  }
`;

export default function App() {
  const [result] = useQuery({
    query: FILMS_QUERY,
  });

  const { data, fetching, error } = result;

  if (fetching) return "Loading...";
  if (error) return <pre>{error.message}</pre>

  return (
    <div>
      <h1>SpaceX Launches</h1>
      <ul>
        {data.launchesPast.map((launch) => (
          <li key={launch.id}>{launch.mission_name}</li>
        ))}
      </ul>
    </div>
  );
}
```

与显示用 Apollo 获取的数据一样，在获取远程数据时，您可以处理错误和加载状态。

## 3.React 查询+ GraphQL 请求

在这一点上需要注意的是，您不需要像 urql 或 Apollo 这样复杂、重量级的 GraphQL 客户端库来与您的 GraphQL API 进行交互，我们将在后面看到这一点。

创建像 Apollo 和 urql 这样的库不仅是为了帮助您执行所有标准的 GraphQL 操作，而且是为了通过许多附加工具更好地管理 React 客户机中的服务器状态。除此之外，它们还附带了定制的钩子，使得管理重复性任务(如处理加载、错误和其他相关状态)变得简单。

记住这一点，让我们看看如何使用一个非常精简的 GraphQL 库来获取数据，并将其与管理和缓存应用程序中的服务器状态的更好方法相结合。在包`graphql-request`的帮助下，你可以非常简单地获取数据。

GraphQL Request 是一个不需要您设置客户机或提供者组件的库。它本质上是一个只接受端点和查询的函数。与 HTTP 客户端非常相似，您只需传入这两个值，然后就可以得到您的数据。

现在，如果你想在你的应用中管理这种状态，你可以使用一个很棒的库，通常用于与 Rest APIs 交互，但它对 GraphQL APIs 同样有帮助，那就是 React Query。它给了你一些名字非常相似的 React 钩子，`useQuery`和`useMutation`，它们执行与 Apollo 和 urql 钩子相同的任务。

React Query 还为您提供了一系列用于管理状态的工具，以及一个集成的开发工具组件，该组件允许您查看 React Query 的内置缓存中存储了什么。

> 通过将非常基本的 GraphQL 客户端 GraphQL request 与 React Query 配对，您可以获得像 urql 或 Apollo 这样的库的所有功能。

要开始这种配对，您只需要安装 React Query 和 GraphQL Request:

```
npm install react-query graphql-request
```

您使用 React Query 的 Provider 组件并创建一个查询客户机，如果愿意，您可以在其中设置一些默认的数据获取设置。然后在你的应用组件本身，或者`App`的任何子组件中，你可以使用`useQuery`钩子。

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { QueryClient, QueryClientProvider } from "react-query";

const client = new QueryClient();

const rootElement = document.getElementById("root");
ReactDOM.render(
  <QueryClientProvider client={client}>
    <App />
  </QueryClientProvider>,
  rootElement
);
```

要将操作的结果存储在 React 查询缓存中，只需给它一个键值作为第一个参数，作为标识符。这允许您非常容易地从缓存中引用和提取数据，以及重新提取或使给定的查询无效以提取更新的数据。

因为您正在获取启动数据，所以让我们称这个查询为“启动”。

这个钩子将再次返回请求的结果。对于`useQuery`的第二个参数，您需要指定如何获取数据，React Query 将负责解析 GraphQL 请求返回的承诺。

```
import React from "react";
import { request, gql } from "graphql-request";
import { useQuery } from "react-query";

const endpoint = "https://api.spacex.land/graphql/";
const FILMS_QUERY = gql`
  {
    launchesPast(limit: 10) {
      id
      mission_name
    }
  }
`;

export default function App() {
  const { data, isLoading, error } = useQuery("launches", () => {
    return request(endpoint, FILMS_QUERY);
  });

  if (isLoading) return "Loading...";
  if (error) return <pre>{error.message}</pre>;

  return (
    <div>
      <h1>SpaceX Launches</h1>
      <ul>
        {data.launchesPast.map((launch) => (
          <li key={launch.id}>{launch.mission_name}</li>
        ))}
      </ul>
    </div>
  );
}
```

与 Apollo 类似，您可以获得一个对象，您可以通过析构该对象来获得数据的值，以及您是否处于加载状态和错误状态。

## 4.React 查询+ Axios

您可以使用与 GraphQL 无关的更简单的 HTTP 客户端库来获取数据。

在这种情况下，您可以使用流行的库 axios。同样，您可以将它与 React Query 配对，以获得所有特殊的挂钩和状态管理。

```
npm install react-query axios
```

使用像 Axios 这样的 HTTP 客户机来执行来自 GraphQL API 的查询，需要对 API 端点执行 POST 请求。对于您在请求中发送的数据，您将提供一个具有名为`query`的属性的对象，该属性将被设置为 films 查询。

对于 axios，您需要包含更多关于如何兑现承诺和取回数据的信息。您需要告诉 React Query 数据在哪里，这样就可以将数据放在`useQuery`返回的`data`属性中。

特别是，您可以从`response.data`的数据属性中获取数据:

```
import React from "react";
import axios from "axios";
import { useQuery } from "react-query";

const endpoint = "https://api.spacex.land/graphql/";
const FILMS_QUERY = `
  {
    launchesPast(limit: 10) {
      id
      mission_name
    }
  }
`;

export default function App() {
  const { data, isLoading, error } = useQuery("launches", () => {
    return axios({
      url: endpoint,
      method: "POST",
      data: {
        query: FILMS_QUERY
      }
    }).then(response => response.data.data);
  });

  if (isLoading) return "Loading...";
  if (error) return <pre>{error.message}</pre>;

  return (
    <div>
      <h1>SpaceX Launches</h1>
      <ul>
        {data.launchesPast.map((launch) => (
          <li key={launch.id}>{launch.mission_name}</li>
        ))}
      </ul>
    </div>
  );
}
```

## 5.反应查询+获取 API

在所有这些获取数据的不同方法中，最简单的方法就是使用 React query 和 fetch API。

由于 fetch API 包含在所有现代浏览器中，您不需要安装第三方库——您只需要在您的应用程序中安装`react-query`。

```
npm install react-query
```

一旦为整个应用程序提供了 React 查询客户端，就可以用 fetch 替换掉 axios 代码。

有一点不同的是，您需要指定一个头，其中包含您希望从请求中返回的数据的内容类型。在这种情况下，它是 JSON 数据。

您还需要使用设置为电影查询的查询属性来字符串化作为有效负载发送的对象:

```
import React from "react";
import axios from "axios";
import { useQuery } from "react-query";

const endpoint = "https://api.spacex.land/graphql/";
const FILMS_QUERY = `
  {
    launchesPast(limit: 10) {
      id
      mission_name
    }
  }
`;

export default function App() {
  const { data, isLoading, error } = useQuery("launches", () => {
    return fetch(endpoint, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ query: FILMS_QUERY })
    })
      .then((response) => {
        if (response.status >= 400) {
          throw new Error("Error fetching data");
        } else {
          return response.json();
        }
      })
      .then((data) => data.data);
  });

  if (isLoading) return "Loading...";
  if (error) return <pre>{error.message}</pre>;

  return (
    <div>
      <h1>SpaceX Launches</h1>
      <ul>
        {data.launchesPast.map((launch) => (
          <li key={launch.id}>{launch.mission_name}</li>
        ))}
      </ul>
    </div>
  );
}
```

使用 axios 胜过 fetch 的一个好处是它会自动为您处理错误。使用 fetch，正如您在上面的代码中看到的，您需要检查某个状态代码，特别是高于 400 的状态代码。

这意味着您的请求解决了一个错误。如果是这种情况，您需要手动抛出一个错误，这个错误将被您的`useQuery`钩子捕获。否则，如果是 200 或 300 范围的响应，意味着请求成功，只需返回 JSON 数据并显示出来。

## 结论

本文致力于向您展示使用 React 从 GraphQL API 中有效获取数据的多种不同方法。

从这些选项中，希望您可以评估哪一个最适合您和您的应用程序。现在您有了一些有用的代码，可以让您更快地开始使用这些工具和库。

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*