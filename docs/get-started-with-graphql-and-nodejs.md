# 如何开始使用 GraphQL 和 Node.js

> 原文：<https://www.freecodecamp.org/news/get-started-with-graphql-and-nodejs/>

这个服务器-客户端 [Node.js](https://nodejs.org/en/) 项目的主要目的是帮助其他人**理解 GraphQL 如何从服务器公开数据以及客户端如何获取数据**。

我试图让它尽可能简单——如果你想深入了解该项目的代码，你可以在这里找到它。

现在，直截了当地说: [GraphQL](https://graphql.org/) 是由脸书开发的[API](https://en.wikipedia.org/wiki/Application_programming_interface)和[开源的一种**查询语言，用于加快请求过程。**](https://engineering.fb.com/core-data/graphql-a-data-query-language/)

REST 已经成为一种从服务器公开数据的流行方式。但是 GraphQL 没有返回固定数据结构的**多个端点**，而是只有**一个端点**。而客户的工作就是指定它需要从它那里得到什么数据。

## 目录

*   入门指南
*   如何定义模式
*   如何添加解析器功能
*   如何设置服务器
*   如何设置客户端
*   如何从服务器获取数据
*   如何显示数据
*   结论
*   有用的资源
    *   文件（documents 的简写）📚
    *   学习📝
    *   工具🔧
    *   5 月💻
    *   临时演员🍍

## 入门指南

第一步是[下载并安装 Node.js](https://nodejs.org/en/download/) ，以防你还没有。安装好之后，让我们从目录结构开始。

该项目将由**两个目录**组成，一个用于客户端，另一个用于服务器。我选择将两者都保存在项目根目录中，但是您可以将它分成两个单独的项目，或者以您想要的任何方式。

```
📁 project
├── 📁 client
└── 📁 server 
```

现在我们将在服务器目录中初始化项目。在你的终端中将位置更改为 server 文件夹，运行`npm init`填充项目信息，生成 **package.json** 文件。

或者您可以`npm init -y`告诉生成器使用缺省值(而不是询问问题并简单地生成一个空的 npm 项目，而不经过交互过程)。

下一步将是在我们的服务器上安装 GraphQL.js 和 T2 阿波罗服务器。GraphQL.js 将提供两个重要的功能:

*   构建类型模式，我们将在下一步的[中完成。](#define-the-schema)
*   针对该类型模式提供查询。

要安装它，只需运行`npm install graphql`。我假设你使用的是等于或高于 **5.0.0** 的 NPM 版本，这样你[就不需要](https://blog.npmjs.org/post/161081169345/v500)在安装要保存在`package.json`中的依赖项时添加`--save`。

另一方面，Apollo Server 将帮助我们实现 GraphQL 功能。它是[阿波罗数据图平台](https://www.apollographql.com/)的一部分。

> Apollo 是一个用于构建数据图的平台，数据图是一个将您的应用程序客户端(如 React 和 iOS 应用程序)无缝连接到后端服务的通信层。是 GraphQL 的一个实现，旨在满足产品工程团队构建现代数据驱动应用程序的需求。- [阿波罗文档](https://www.apollographql.com/docs/)

关于 Apollo，你需要知道的是，至少现在，它是一个构建在 GraphQL 之上的社区，提供不同的**工具来帮助你构建你的项目**。Apollo 提供的工具主要是 2:客户端和服务器端。

*   **Apollo 客户端**帮助您的前端与 GraphQL API 通信。它支持最流行的框架，如 React、Vue 或 Angular 以及 iOS 和 Android 上的原生开发。

*   **Apollo Server** 是后端的 GraphQL 服务器层，它将响应传递回客户端请求。

现在您更好地理解了 Apollo 以及我们为什么会使用它，让我们继续设置 GraphQL。

## 如何定义模式

GraphQL 模式是任何 GraphQL 服务器实现的核心。它**描述了您的数据**的形状，用具有从您的数据源填充的字段的**类型**的层次结构来定义它。它还指定了哪些**查询**和**变异**可用，因此客户端知道可以请求或发送的信息。

例如，如果我们想要构建一个音乐应用程序，我们最简单的模式，通常定义在一个`schema.graphql`文件中，将包含两个**对象类型** : `Song`和`Author`，如下所示:

```
type Song {
  title: String
  author: Author
}

type Author {
  name: String
  songs: [Song]
} 
```

然后我们将有一个**查询类型**来定义可用的查询:`getSongs`和`getAuthors`，每个查询返回相应类型的列表。

```
type Query {
  getSongs: [Song]
  getAuthors: [Author]
} 
```

为了尽可能简单，我们的模式将只有**一个返回`String`的单一查询**类型。

```
type Query {
  greeting: String
} 
```

我们可以使用**任何编程语言**创建一个 GraphQL 模式，**围绕它构建一个接口**，但是正如我之前解释的，我们将使用 Apollo 服务器来执行 GraphQL 查询。

因此，我们在服务器目录中创建新的`server.js`文件，以在其上定义模式。

```
📁 project
├── 📁 client
└── 📁 server
    └── 📄 server.js 
```

现在我们通过运行`npm install apollo-server`来安装 apollo-server。

我们必须从 **apollo-server** 中导入**标签函数** `gql`，以此方式解析模式:`const {gql} = require('apollo-server');`，然后声明一个`typeDefs`常量，这是 Graphql 代码的一个[抽象语法树](https://en.wikipedia.org/wiki/Abstract_syntax_tree)。

> 当 GraphQL 服务器接收到要处理的查询时，它通常以字符串的形式出现。这个字符串必须被标记化并解析成机器能够理解的表示形式。这种表示被称为抽象语法树。

如果你想学习更多关于抽象语法树的知识，AST Explorer 是一个在线工具，它可以让你作为一个解析器探索由一种选择的语言创建的语法树。

`server.js`文件看起来像这样:

```
const { gql } = require('apollo-server');

const typeDefs = gql`
  type Query {
    greeting: String
  }
`; 
```

## 如何添加解析器功能

现在我们已经定义了模式，我们需要一种方法来响应客户端对数据的请求:解析器。

**解析器是一个为你的模式**的每个字段处理数据的函数。你可以通过**获取后端数据库**或第三方 **API** 等将数据发送给客户端。

它们必须**匹配模式的类型定义**。在我们的例子中，我们只有一个类型定义 Query，它返回类型为`String`的问候，所以我们将为`greeting`字段定义一个解析器，如下所示:

```
const resolvers = {
  Query: {
    greeting: () => 'Hello GraphQL world!👋',
  },
}; 
```

正如我在开始时解释的，我们将尽可能保持这个例子简单。但是请记住，在真实情况下**这里是您必须对数据库、外部 API 或您想要提取查询数据的任何一个进行查询的地方**。

## 如何设置服务器

在同一个`server.js`中，我们定义并创建了一个新的`ApolloServer`对象，将`Schema`(typedef)和`resolvers`作为参数传递。

```
const { ApolloServer, gql } = require('apollo-server');

const server = new ApolloServer({ typeDefs, resolvers }); 
```

然后调用`listen`方法，我们在参数中指定的`port`上启动服务器。

```
server
  .listen({ port: 9000 })
  .then(serverInfo => console.log(`Server running at ${serverInfo.url}`)); 
```

我们也可以在登录时**析构**服务器信息`url`。

```
server
  .listen({ port: 9000 })
  .then(({ url }) => console.log(`Server running at ${url}`)); 
```

`server.js`文件现在应该是这样的:

```
const { ApolloServer, gql } = require('apollo-server');

const typeDefs = gql`
  type Query {
    greeting: String
  }
`;

const resolvers = {
  Query: {
    greeting: () => 'Hello GraphQL world!👋',
  },
};

const server = new ApolloServer({ typeDefs, resolvers });
server
  .listen({ port: 9000 })
  .then(({ url }) => console.log(`Server running at ${url}`)); 
```

现在，如果我们运行`node server/server.js`，我们将最终启动并运行我们的 GraphQL 服务器！🎉

可以去 [http://localhost:9000/](http://localhost:9000/) 上查看一下

```
~/graphql-hello-world-server
> node server/server.js
Server running at http://localhost:9000/ 
```

如果这是您第一次使用 GraphQL，您可能会想**如果我们还没有编写一行客户端代码，我面前的这个应用程序是什么？**。

这个问题的答案是 GraphQL 游乐场。

![GraphQL Playground. Source: https://github.com/prisma-labs/graphql-playground](img/e7eb1bd746eab54a584a9b3a14d723c3.png)

> GraphQL Playground 是一个图形化、交互式、浏览器内的 GraphQL IDE，由 [Prisma](https://www.prisma.io/) 创建，基于[graph QL](https://github.com/graphql/graphiql)。- [阿波罗文档](https://www.apollographql.com/docs/apollo-server/testing/graphql-playground/)

但这意味着什么呢？这意味着在这个环境中，我们可以对我们的模式执行查询、变更或订阅，并与它的数据进行交互。

如果你以前处理过 RESTful 请求，这就相当于 T2 邮递员 T3。只是在这里你**不需要下载和配置任何东西**，它只是**默认自带**Apollo！

![Awesome](img/fb78fbf5145b28910e89131b42ce5793.png)
那么我们来试试吧！

1.  在左边的面板上写下我们在模式中定义的`greeting`查询。
2.  然后按中间的按钮。
3.  瞧，瞧！右边的面板显示了我们在解析器中定义的要返回的数据。

![Writting query on GraphQl Playground](img/7c3129cf47a25253436f86eee477a50a.png)

## 如何设置客户端

现在我们已经启动并运行了服务器，让我们把重点放在客户端部分。我们将从在客户端文件夹中创建一个`client.html`文件开始。

```
📁 project
├── 📁 client
|   └── 📄 client.html
└── 📁 server
    └── 📄 server.js 
```

当我们从服务器请求数据时，`index.html`文件将拥有任何`HTML`文件的基础和一个加载头`<h1>Loading...</h1>`来显示给用户一些东西。

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello World GraphQL Client</title>
  </head>

  <body>
    <h1>Loading...</h1>

    <script src="app.js"></script>
  </body>
</html> 
```

## 如何从服务器获取数据

首先，在同一个客户机文件夹中，我们创建了一个`app.js`文件，我们将在其中编写客户机逻辑来从服务器获取数据。

```
📁 project
├── 📁 client
|   └── 📄 client.html
|   └── 📄 app.js
└── 📁 server
    └── 📄 server.js 
```

在其中，我们将服务器 URL 设置为发出请求的 URL。

```
const GRAPHQL_URL = 'http://localhost:9000/'; 
```

接下来，我们定义异步函数`fetchGreeting()`从服务器获取问候。我们将使用 [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 来执行 HTTP 请求，该请求默认返回一个承诺，我们可以订阅该承诺并异步获得答案。

```
async function fetchGreeting() {
  const response = await fetch(GRAPHQL_URL, {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
    },
    body: JSON.stringify({
      query: `
        query {
          greeting
        }
      `,
    }),
  });

  const responseBody = await response.json();
  console.log(responseBody);
} 
```

需要考虑的一个细节是请求的方法是`POST`。如果我们习惯于使用`RESTful`的话，这可能会让我们感到困惑，因为在`RESTful`中提出的同样的请求，我们只是想从服务器读取信息，通常会用方法`GET`来完成。

事情是这样的，使用 GraphQL，我们总是发出`POST`请求，并在有效载荷(主体)中传递**查询。**

最后，我们只调用我们的方法`fetchGreeting();`

```
const GRAPHQL_URL = 'http://localhost:9000/';

async function fetchGreeting() {
  const response = await fetch(GRAPHQL_URL, {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
    },
    body: JSON.stringify({
      query: `
        query {
          greeting
        }
      `,
    }),
  });

  const responseBody = await response.json();
  console.log(responseBody);
}

fetchGreeting(); 
```

如果您在浏览器中打开该文件，并查看开发人员工具上的**控制台，您可以看到我们实际上从查询中获得了问候数据🙌！**

![Fetching data from the server](img/a2ac663ee491fafb7861528373280381.png)

## 如何显示数据

现在我们已经成功地从服务器获得了数据，让**更新加载标题**。我们要做的第一件事是析构响应并从中返回`data`。

只需替换这部分代码:

```
const responseBody = await response.json();
console.log(responseBody); 
```

有了这个:

```
const { data } = await response.json();
return data; 
```

然后，我们将使用响应的数据中返回的**来更新标题**

```
fetchGreeting().then(({ greeting }) => {
  const title = document.querySelector('h1');
  title.textContent = greeting;
}); 
```

所以我们的`app.js`文件最终会是这样的:

```
const GRAPHQL_URL = 'http://localhost:9000/';

async function fetchGreeting() {
  const response = await fetch(GRAPHQL_URL, {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
    },
    body: JSON.stringify({
      query: `
        query {
          greeting
        }
      `,
    }),
  });

  const { data } = await response.json();
  return data;
}

fetchGreeting().then(({ greeting }) => {
  const title = document.querySelector('h1');
  title.textContent = greeting;
}); 
```

我们的`index.html`将使用从我们的服务器获取的数据更新加载标题！🎉

![Displaying the data](img/7ef58e733839b364ef3c21befc704dd1.png)

## 结论

我希望你喜欢这篇文章，并且这个项目向你展示了 graphql 是如何工作的，至少是以一种非常简单的方式。

我知道有很多事情我没有解释清楚，或者我本可以解释得更深入。但是像任何项目一样，这是为初学者设计的，所以我想让它尽可能简单。

我期待着学习更多关于 GraphQL 的知识，并在未来的项目中使用它。如果你有任何问题、建议或反馈，不要犹豫，通过邮件从[我的网站](https://ramonmorcillo.com)或[联系任何社交网络](mailto:hey@ramonmorcillo.com)。

## 有用的 GraphQL 资源

这里收集了一些链接和资源，对我提高和学习 GraphQL 很有帮助

### 文件（documents 的简写）📚

*   [项目源代码](https://github.com/reymon359/graphql-hello-world-server)-Github 储存库，包含项目的所有代码。
*   [图形 SQL 主站点](https://graphql.org/)图形 SQL 主站点。
*   [阿波罗文档](https://graphql.org/)——阿波罗平台文档。

### 学习📝

*   [如何使用 GraphQL](https://www.howtographql.com/)——免费开源教程，了解 graph QL 从零到生产的方方面面。
*   [GraphQL 示例](https://www.udemy.com/course/graphql-by-example/)——通过使用 Node.js、Express、Apollo Server、React、Apollo Client 编写全栈 JavaScript 应用程序来学习 GraphQL 的绝佳课程。
*   [GraphQL 简介](https://graphql.org/learn/)——一系列文章，了解 graph QL，它是如何工作的，以及如何使用它。

### 工具🔧

*   [Apollo graph QL](https://www.apollographql.com/)——Apollo graph QL 实现的主站点。
*   graph QL Playground——我们在项目中使用的 GraphQL Playground IDE 的存储库。

### 5 月💻

*   JS GraphQL - WebStorm 和其他基于 IntelliJ 的 IDEs 插件，支持 GraphQL 语言，包括 JavaScript 和 TypeScript 中的标记模板文字。
*   VSCode 的 GraphQL 扩展为 GraphQL 项目增加了语法高亮、验证和语言特性，如转到定义、悬停信息和自动完成。这个扩展也适用于用 gql 标签注释的查询。
*   VSCode GraphQL 语法高亮、林挺、自动完成等等！

### 临时演员🍍

*   graph QL API-一个公共 graph QL API 列表，用来测试你的技能或者用它们来构建一些东西。
*   GraphQL:纪录片——一个 30 分钟的视频，探索了 GraphQL 为什么以及如何出现的故事，以及它对全球大型科技公司的影响，包括脸书、Twitter、Airbnb 和 Github。

我希望你喜欢这篇文章。你也可以在我的网站和其他人一起阅读[！如果你有任何问题、建议或反馈，请不要犹豫，从](https://ramonmorcillo.com/getting-started-with-graphql-and-nodejs/ )[我的网站](https://ramonmorcillo.com/)的任何社交网络上联系。