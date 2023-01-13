# Apollo GraphQL:如何用 React 和 Node Js 构建全栈 App

> 原文：<https://www.freecodecamp.org/news/apollo-graphql-how-to-build-a-full-stack-app-with-react-and-node-js/>

Apollo Client 是一个完整的 JavaScript 应用程序状态管理库。这是一个强大的工具，因为它可以在后端和前端使用。

在本教程中，我们将通过使用 Node JS 构建一个 Apollo GraphQL 服务器，在前端和后端使用它。然后我们将使用 React JS 在客户端消费数据。

如果你是 GraphQl 新手，[这篇教程](https://www.ibrahima-ndaw.com/blog/graphql-api-express-mongodb/)可能会对你有所帮助。否则，我们开始吧。

*   [用 Apollo、Node 和 GraphQl 构建服务器](#building-the-server-with-apollo-node-and-graphql)
*   [GraphQl 模式](#graphql-schema)
*   [图 Ql 解析器](#graphql-resolvers)
*   [创建阿波罗服务器](#creating-the-apollo-server)
*   [用 React 构建客户端](#building-the-client-side-with-react)
*   [连接反应阿波罗](#connecting-react-to-apollo)
*   [获取数据](#fetching-the-data)
*   [显示数据](#showing-the-data)

## 用 Apollo、Node 和 GraphQl 构建服务器

在本指南中，我将使用 Github API 来显示数据，该操作将由使用 Apollo 和 Node JS 构建的 GraphQl 服务器来完成。

为此，我们需要在终端上运行以下命令来设置一个新的 Node JS 项目:

```
 yarn init 
```

设置完成后，我们现在可以通过运行以下命令来安装必要的软件包:

```
 yarn add apollo-server graphql axios 
```

太好了，我们现在有了构建服务器所需的所有东西。因此，首先让我们在根目录下创建一个新文件`app.js`,这将是我们服务器的入口点。

接下来，我们需要定义一个 Graphql 模式来反映我们的数据应该是什么样子。

### GraphQl 模式

模式描述了数据图的形状。它定义了一组类型，这些类型的字段是从后端数据存储中填充的。因此，让我们在`app.js`文件中添加一个新的模式。

*   `app.js`

```
const { ApolloServer, gql } = require("apollo-server")
const axios = require("axios")

const typeDefs = gql`
  type User {
    id: ID
    login: String
    avatar_url: String
  }

  type Query {
    users: [User]
  }
` 
```

如你所见，我们没有使用 Github API 提供的所有数据。我们只需要在 React 应用程序、登录和 avatar_url 上用作参考键的 id。我们还有一个查询`users`返回一组用户。

现在我们有了一个 GraphQL 模式，是时候构建相应的解析器来完成查询操作了。

### GraphQl 解析器

解析器是帮助 GraphQL 查询生成响应的函数集合。因此，让我们在`app.js`文件中添加一个新的解析器。

*   `app.js`

```
const resolvers = {
  Query: {
    users: async () => {
      try {
        const users = await axios.get("https://api.github.com/users")
        return users.data.map(({ id, login, avatar_url }) => ({
          id,
          login,
          avatar_url,
        }))
      } catch (error) {
        throw error
      }
    },
  },
} 
```

解析器必须在名称上匹配适当的模式。因此，这里的`users`指的是我们的模式中定义的`users`查询。这个函数在`axios`的帮助下从 API 中获取数据，并按照预期返回 id、登录名和 avatar_url。

该操作可能需要一些时间才能完成。这就是为什么这里使用 async/await 来处理它。

至此，我们可以在下一节创建 Apollo 服务器了。

### 创建阿波罗服务器

如果您还记得，在`app.js`文件中，我们从`apollo-server`包中导入了`ApolloServer`。它是一个接受对象作为参数的构造函数。并且该对象必须包含模式和解析器才能创建服务器。

所以，让我们用`ApolloServer`稍微调整一下`app.js`。

*   `app.js`

```
const server = new ApolloServer({
  typeDefs,
  resolvers,
})
//  typeDefs: typeDefs,
//  resolvers: resolvers
server.listen().then(({ url }) => console.log(`Server ready at ${url}`)) 
```

这里，我们将保存模式和解析器的对象作为参数传递给`ApolloServer`来创建服务器，然后监听它。有了它，我们现在就有了一个可以使用的功能服务器。

通过运行以下命令，您已经可以在 GraphQL playground 的帮助下使用它并发送查询了:

```
 yarn start 
```

您现在可以在`http://localhost:400`上预览

*   完整的`app.js`文件

```
const { ApolloServer, gql } = require("apollo-server")
const axios = require("axios")

const typeDefs = gql`
  type User {
    id: ID
    login: String
    avatar_url: String
  }

  type Query {
    users: [User]
  }
`

const resolvers = {
  Query: {
    users: async () => {
      try {
        const users = await axios.get("https://api.github.com/users")
        return users.data.map(({ id, login, avatar_url }) => ({
          id,
          login,
          avatar_url,
        }))
      } catch (error) {
        throw error
      }
    },
  },
}

const server = new ApolloServer({
  typeDefs,
  resolvers,
})

server.listen().then(({ url }) => console.log(`Server ready at ${url}`)) 
```

单靠一台服务器做不了多少事。正如您所猜测的，我们需要在`package.json`文件中添加一个启动脚本来启动服务器。

*   `package.json`

```
 // first add nodemon: yarn add nodemon --dev
  "scripts": {
    "start": "nodemon src/index.js"
  } 
```

这样，我们就有了一个从 Github API 获取数据的服务器。所以现在是时候转移到客户端并使用数据了。

让我们开始吧。

![yaay](img/7f05944b0e8c80410d2e4b5b54919637.png)

## 用 React 构建客户端

我们要做的第一件事是通过在终端中运行以下命令来创建一个新的 React 应用程序:

```
npx create-react-app client-react-apollo 
```

接下来，我们需要安装 Apollo 和 GraphQl 包:

```
 yarn add apollo-boost @apollo/react-hooks graphql 
```

现在，我们可以通过更新`index.js`文件将 Apollo 与 React 应用程序连接起来。

### 连接反应到阿波罗

*   `index.js`

```
import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-boost'
import { ApolloProvider } from '@apollo/react-hooks';

import App from './App';
import './index.css';
import * as serviceWorker from './serviceWorker';

const client = new ApolloClient({
  uri: 'https://7sgx4.sse.codesandbox.io'
})

ReactDOM.render(
  <React.StrictMode>
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>
  </React.StrictMode>,
  document.getElementById('root')
);

serviceWorker.unregister(); 
```

如你所见，我们从导入`ApolloClient`和`ApolloProvider`开始。第一个帮助我们通知 Apollo 在获取数据时使用哪个 URL。而如果没有`uri`传递给`ApolloClient`，则取当前域名加`/graphql`。

第二个是期望接收客户机对象的提供者，它能够连接 Apollo 以作出反应。

也就是说，我们现在可以创建一个显示数据的组件。

### 获取数据

*   `App.js`

```
import React from "react"
import { useQuery } from "@apollo/react-hooks"
import gql from "graphql-tag"
import "./App.css"

const GET_USERS = gql`
  {
    users {
      id
      login
      avatar_url
    }
  }
` 
```

这里，我们有一个简单的获取数据的 GraphQL 查询。该查询稍后将被传递给`useQuery`以告诉 Apollo 获取哪些数据。

*   `App.js`

```
const User = ({ user: { login, avatar_url } }) => (
  <div className="Card">
    <div>
      <img alt="avatar" className="Card--avatar" src={avatar_url} />
      <h1 className="Card--name">{login}</h1>
    </div>
    <a href={`https://github.com/${login}`} className="Card--link">
      See profile
    </a>
  </div>
) 
```

这个表示组件将用于显示用户。它从应用程序组件接收数据并显示出来。

### 显示数据

*   `App.js`

```
function App() {
  const { loading, error, data } = useQuery(GET_USERS)

  if (error) return <h1>Something went wrong!</h1>
  if (loading) return <h1>Loading...</h1>

  return (
    <main className="App">
      <h1>Github | Users</h1>
      {data.users.map(user => (
        <User key={user.id} user={user} />
      ))}
    </main>
  )
}

export default App 
```

Apollo 提供的`useQuery`钩子接收 GraphQL 查询并返回三种状态:加载、错误和数据。

如果数据获取成功，我们将它传递给用户组件。否则我们抛出一个错误。

*   完整的`App.js`文件

```
import React from "react"
import { useQuery } from "@apollo/react-hooks"
import gql from "graphql-tag"
import "./App.css"

const GET_USERS = gql`
  {
    users {
      id
      login
      avatar_url
    }
  }
`

const User = ({ user: { login, avatar_url } }) => (
  <div className="Card">
    <div>
      <img alt="avatar" className="Card--avatar" src={avatar_url} />
      <h1 className="Card--name">{login}</h1>
    </div>
    <a href={`https://github.com/${login}`} className="Card--link">
      See profile
    </a>
  </div>
)

function App() {
  const { loading, error, data } = useQuery(GET_USERS)

  if (error) return <h1>Something went wrong!</h1>
  if (loading) return <h1>Loading...</h1>

  return (
    <main className="App">
      <h1>Github | Users</h1>
      {data.users.map(user => (
        <User key={user.id} user={user} />
      ))}
    </main>
  )
}

export default App 
```

太好了！至此，我们已经使用 React 和 Node JS 构建了一个全栈的 Apollo GraphQL 应用程序。

预览阿波罗 GraphQL 服务器[在这里](https://codesandbox.io/s/node-apollo-graphql-7sgx4?file=/package.json:139-193)

在此预览 React 应用

在此找到源代码

你可以在[我的博客](https://www.ibrahima-ndaw.com/blog/apollo-graphql-fullstack-app-with-react-and-nodejs/)上找到其他类似的精彩内容

感谢阅读！

![congrats](img/62223143a0d7905b01e0f8be03823605.png)