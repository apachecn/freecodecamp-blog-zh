# 如何使用 Apollo 全新的查询组件管理本地状态

> 原文：<https://www.freecodecamp.org/news/updated-for-apollo-v2-1-managing-local-state-with-apollo-d1882f2fbb7/>

注意:本文处理的是利用 Apollo 全新的查询和变异组件，而不是 hoc。对于那些读过原文[这里](https://itnext.io/managing-local-state-with-apollo-client-3be522258645)的人来说，要知道这两篇文章非常相似。

### 介绍

Web 开发最大的优势之一——也是最大的弱点——是它的模块化方法。一个关键的编程口头禅是选择一些东西(一个函数，一个包)来完成一项工作，并把它做好。这种方法的缺点是，单个项目可能会涉及到几十种独立的技术和概念，每一种都专注于特定的东西。

所以选择 Apollo 客户机来处理我的本地状态和远程数据似乎是显而易见的。既然我已经设置了 Apollo/GraphQL 从后端获取数据，为什么还要处理 Redux 的样板文件和习惯用法呢？

虽然本文将讨论如何设置 Apollo 来处理本地状态，但它并不是对该技术的介绍。(这篇正统的 howtographql 教程是一个很好的开端)。

注:已完成的回购可在处[找到。如果遇到困难或感到困惑，您可以仔细阅读代码。](https://github.com/andrico1234/apollo-local-state-starter)

### 正在设置

我们将从这里的[开始克隆相应的回购协议。这个 repo 包含一个简单的 react 网站，有一个侧边栏、标题和主体。它本质上是相当静态的，没有动态内容(…还没有)。本教程结束时，我们将让 Apollo 管理网站的状态。点击侧边栏中的一个项目会改变网站的状态，进而更新标题以显示新数据。](https://github.com/andrico1234/apollo-state-blog-repo)

如果你检查`package.json`，你会看到我们只有基本的，加上一些关于我们的包裹设置的额外的包裹。

克隆 repo 后，在命令行界面中运行您的标准命令。

```
> yarn
> yarn dev
```

要安装所有的软件包并创建一个本地服务器，请访问 localhost:1234，你将有希望看到演示网站的辉煌。它现在是静态的，所以点击什么也做不了。

我们首先要做的是让 Apollo 进入我们的项目，所以安装这些包。`apollo-client`让我们配置 Apollo 的实例，而`react-apollo`是允许我们将其集成到 React 应用程序中的驱动程序。由于包裹的问题(我想)，我们也需要安装`graphql`。

```
> yarn add apollo-client react-apollo graphql
```

创建一个新目录`src/apollo`，打开一个`index.js`文件，并添加以下内容:

```
import ApolloClient from ‘apollo-client’;
export const client = new ApolloClient({});
```

这将初始化我们的 Apollo 客户端，然后我们将通过在我们的`src/index.js`文件中添加以下内容来包装我们的 React 应用程序。

```
import { ApolloProvider } from ‘react-apollo’;
import { client } from ‘./apollo’;

const WrappedApp = (
  <ApolloProvider client={client} >
    <App />
  </ApolloProvider>
);

ReactDOM.render(WrappedApp, document.getElementById(‘root’));
// Don’t be a sap. Wrap your app.
```

现在，我们已经可以在应用程序中使用 Apollo 了。当我们重新启动我们的开发服务器时，一切都构建好了，但是当我们试图在浏览器中访问它时，我们得到了一个错误。控制台会告诉我们，我们需要为 Apollo 客户机指定链接和缓存属性，所以我们就这么做吧。

```
> yarn add apollo-link apollo-cache-inmemory apollo-link-state
```

前一行向我们的应用程序添加了新的 Apollo 依赖项，而下面的代码解决了我们遇到的控制台错误。所以回到`apollo/index.js`并更新它，使文件看起来像这样:

```
import ApolloClient from ‘apollo-client’;
import { InMemoryCache } from ‘apollo-cache-inmemory’;
import { ApolloLink } from ‘apollo-link’;
import { withClientState } from ‘apollo-link-state’;

const cache = new InMemoryCache();
const stateLink = withClientState({
  cache
});

export const client = new ApolloClient({
  cache,
  link: ApolloLink.from([
    stateLink,
  ]),
})
```

让我们创建一个缓存实例。缓存是 Apollo 的规范化数据存储，它将查询结果存储在一个扁平的数据结构中。当我们进行 GraphQL 查询时，我们将从缓存中读取，当我们进行变异解析时，我们将向缓存中写入。

您可以看到，我们还向客户端对象添加了`link`。`ApolloLink.from()`方法让我们模块化地配置如何通过 HTTP 发送查询。我们可以用它来处理错误和授权，并提供对我们后端的访问。我们不会在教程中做这些，但是我们将在这里设置我们的客户端状态。所以我们创建了上面的`const stateLink`,并传入我们的缓存。稍后我们将在这里添加默认状态和解析器。

回到浏览器，你会看到我们可爱的静态网站显示其所有的辉煌。让我们为项目添加一些默认状态，并启动我们的第一个查询。

在 Apollo 目录中，创建一个名为`defaults`的新目录，并在其中添加一个`index.js`。该文件将包含以下内容:

```
export default {
  apolloClientDemo: {
    __typename: ‘ApolloClientDemo’,
    currentPageName: ‘Apollo Demo’,
  }
}
```

我们创建了一个对象作为我们站点的默认状态。apolloClientDemo 是我们进行查询时想要访问的数据结构的名称。`__typename`是我们的缓存使用的强制标识符，currentPageName 是我们的头将用来显示当前页面名称的特定数据项。

我们需要将它添加到我们的`apollo/index.js`文件中:

```
import defaults from ‘./defaults’;

const stateLink = withClientState({
  cache,
  defaults,
});
```

让我们澄清一下。`import`和`default`都是与导入模块相关的关键字，但是巧合的是，我们从`./defaults`导出的对象的名称也叫做`defaults`(所以不要认为我用错了`import/export`)。将该导入行视为普通的 ol' named import。

这样一来，我们就可以进行查询了！

### 如何进行查询

将以下包添加到您的项目中:

```
> yarn add graphql-tag
```

并创建一个新目录`src/graphql`。在那里，创建两个新文件:`index.js`和`getPageName.js`。GraphQL 目录将容纳所有的查询和变化。我们将通过编写以下代码在`getPageName.js`中创建我们的查询:

```
import gql from ‘graphql-tag’;

export const getPageNameQuery = gql`
  query {
    apolloClientDemo @client {
      currentPageName
    }
  }
`;

export const getPageNameOptions = ({
  props: ({ data: { apolloClientDemo } }) => ({
    apolloClientDemo
  })
});
```

所以我们导出两个变量，查询和选项。如果您以前使用过 GraphQL，那么这个查询看起来会很熟悉。我们对 apolloClientDemo 数据结构进行查询，只检索 currentPageName。您会注意到我们已经在查询中添加了`@client`指令。这告诉 Apollo 查询我们的本地状态，而不是将请求发送到后端。

下面您会看到我们正在导出一些选项。这只是定义当我们将结果映射到道具时，我们希望数据看起来是什么样子。我们正在析构 GraphQL 响应，并将其发送到我们的视图，因此它看起来像这样:

```
props: {
  currentPageName: ‘Apollo Demo’,
}
// and not this
props: {
  data: {
    apolloClientDemo: {
      currentPageName: ‘Apollo Demo’,
    }
  }
}
```

转到`graphql/index.js`文件并导出查询，如下所示:

```
export { getPageNameQuery, getPageNameOptions } from ‘./getPageName’; 
```

同样，虽然这对于一个小的演示/项目来说不是完全必要的，但是如果您的应用程序变得更大，这个文件是很方便的。让您的查询从一个集中的位置导出，可以保持一切井然有序和可伸缩性。

添加到您的 Header.js:

```
import React from 'react';
import { Query } from 'react-apollo';
import { getPageNameQuery } from '../graphql';

const Header = () => (
    <Query query={getPageNameQuery}>
        {({ loading, error, data }) => {
            if (error) return <h1>Error...</h1>;
            if (loading || !data) return <h1>Loading...</h1>;

            return <h1>{data.apolloClientDemo.currentPageName}</h1>
        }}
    </Query>
);

export default Header;
```

这是我们第一次使用 Apollo 的新查询组件，它是在 2.1 中添加的。我们从`react-apollo`导入`Query`,并用它来包装组件的其余部分。然后，我们将 getPageNameQuery 作为查询属性中的一个值进行传递。当我们的组件呈现时，它触发查询并让组件的其余部分访问数据，我们通过析构来访问加载、错误和数据。

查询组件使用 render props 模式让组件的其余部分访问查询返回的信息。如果您在 16.3 中使用过 React Context API，那么您以前应该见过这个语法。否则值得在这里查看一下官方的 React 文档，因为渲染道具模式变得越来越流行。

在我们的组件中，我们做一些检查，看看在启动查询时是否有任何错误，或者我们是否仍在等待数据返回。如果这两种情况中有一种是真的，我们就返回相应的 HTML。如果正确触发了查询，组件将动态显示当前页面的标题。由于我们还没有添加我们的变异，它将只显示默认值。但是你可以改变默认状态，网站会反映出来。

现在剩下要做的就是通过点击侧边栏条目来改变 Apollo 缓存中的数据。

![0*OHpQBcsRCsX5Wk_b](img/4f848746ff811d24e912618a72ae62b4.png)

A refreshing image to break up the text. [Jeff Sheldon](https://unsplash.com/@ugmonk?utm_source=medium&utm_medium=referral)

### 突变

当处理突变时，事情变得有点复杂。我们不再只是从 Apollo 存储中检索数据，还会更新数据。突变的架构如下:

**> U** ser 点击侧边栏项目

**> Se** nds 变来变去

**> Fi** res 随变量突变

**> G** ets 派往阿波罗的实例

**> Fi** nds 对应的解析器

**>将** ies 逻辑应用于阿波罗商店

**> Se** nds 数据回表头

如果这很难记住，那就用这个用助记符生成器创造的方便的助记符:城市老年牧神庄严地摸索着不忠的阿斯兰。(简单……)

首先创建一个文件`graphql/updatePageName.js`。

```
import gql from ‘graphql-tag’;

export const updatePageName = gql`
  mutation updatePageName($name: String!) {
    updatePageName(name: $name) @client {
      currentPageName
    }
  }
`;
```

并像我们处理查询一样导出它。

```
export { updatePageNameMutation } from ‘./updatePageName’; 
```

你会注意到一些关于突变的不同之处。首先，我们将关键字从 query 改为 mutation。这让 GraphQL 知道我们正在执行的动作的类型。我们还定义了查询的名称，并向传入的变量添加了类型。在这里，我们指定了我们将用来执行更改的解析器的名称。我们还传递变量并添加了`@client`指令。

与查询不同，我们不能只是将突变添加到我们的视图中，然后期望发生任何事情。我们将不得不回到我们的阿波罗目录，并添加我们的解决方案。所以继续创建一个新目录`apollo/resolvers`，以及文件`index.js`和`updatePageName.js`。在`updatePageName.js`内添加以下内容:

```
import gql from ‘graphql-tag’;

export default (_, { name }, { cache }) => {
  const query = gql`
    query GetPageName {
      apolloClientDemo @client {
        currentPageName
      }
    }
  `;

  const previousState = cache.readQuery({ query });

  const data = {
    apolloClientDemo: {
      …previousState.apolloClientDemo,
      currentPageName: name,
    },
  };

  cache.writeQuery({
    query,
    data,
  });

  return null;
};
```

这个文件里有很多有趣的事情。幸运的是，这一切都非常符合逻辑，并没有给我们之前看到的内容添加许多新概念。

因此，默认情况下，当调用解析器时，Apollo 会传入所有变量和缓存。第一个参数是一个简单的“_”，因为我们不需要使用它。第二个参数是变量对象，最后一个参数是缓存。

在对 Apollo 存储进行更改之前，我们需要检索它。因此，我们发出一个简单的请求，从存储中获取当前内容，并将其分配给 previousState。在数据变量内部，我们创建了一个新的对象，其中包含了我们想要添加到存储中的新信息，然后我们向其中写入数据。你可以看到我们已经将之前的状态扩展到了这个对象中。这样，只有我们明确想要更改的数据才会得到更新。其他一切都保持原样。这可以防止 Apollo 不必要地更新那些数据没有改变的组件。

注意:虽然对于这个例子来说这不是完全必要的，但是当查询和突变处理大量数据时，它非常有用，所以为了可伸缩性，我保留了它。

同时在`resolvers/index.js`文件中…

```
import updatePageName from ‘updatePageName’;

export default {
  Mutation: {
    updatePageName,
  }
};
```

这是 Apollo 期望的对象的形状，当我们将解析器传递给 stateLink 时:

```
import resolvers from ‘./resolvers’;

const stateLink from = withClientState({
  cache,
  defaults,
  resolvers,
});
```

剩下要做的就是将变异添加到我们的侧栏组件中。

```
// previous imports
import { Mutation } from ‘react-apollo’;
import { updatePageNameMutation } from ‘../graphql’;

class Sidebar extends React.Component {
  render() {
    return (
      <Mutation mutation={updatePageNameMutation}>
        {updatePageName => (
          // outer div elements
          <li className=“sidebar-item” onClick={() => updatePageName({ variables: { name: ‘React’} })}>React</li>
          // other list items and outer div elements
        )}
      </Mutation>
    );
  }
}

export default Sidebar;
```

像我们的解析器文件一样，这个文件中有很多内容，但它是新的。我们从`react-apollo`导入我们的`Mutation`组件，将它包裹在我们的组件周围，并将`updatePageNameMutation`传递到`mutation`道具内部。

组件现在可以访问`updatePageName`方法，该方法在被调用时触发突变。我们通过将该方法作为处理程序添加到`<`李>的 onClick 属性中来实现这一点。该方法期望接收包含变量的对象作为参数，因此传入要将标头更新到的名称。如果一切正常，你应该能够运行你的开发服务器并点击侧边栏项目，这将改变我们的标题。

### 包扎

万岁！希望一切顺利。如果你卡住了，那么检查回购[在这里](https://github.com/andrico1234/apollo-local-state-starter)。它包含了所有完成的代码。如果你想在下一个 React 应用中使用本地状态管理，那么你可以派生这个 repo 并从那里继续。如果你有兴趣在聚会或会议上谈论这篇文章/话题，那么给我发个信息吧！

我还想在本教程中介绍更多内容，比如异步解析器(想想 Redux thunk)、类型检查/创建模式以及变异更新。所以谁知道呢…也许不久我会再发表一篇文章。

我真的希望这篇教程对你有用。我也想大声说出 Sara Vieira 的 youtube 教程，因为它帮助我理解了 Apollo 客户端。如果我没有做好我的工作，让你挠头，那么请点击链接。最后，请随时在社交媒体上联系我，我是一个超级音乐和技术迷，所以请和我聊聊极客。

#### 感谢阅读！

如果你有兴趣邀请我参加会议、meetup 或作为演讲嘉宾参加任何活动，那么你可以在 [twitter](https://twitter.com/andricokaroulla?lang=en) 上给我发 DM！

#### 你可以看看我下面的其他文章:

[*如何使用 Apollo 全新的查询组件管理本地状态*](https://medium.com/@andricokaroulla/updated-for-apollo-v2-1-managing-local-state-with-apollo-d1882f2fbb7)

[*用 React.lazy()*](http://Add a touch of Suspense to your web app with React.lazy()) 为你的 web app 增添一丝悬念

[*不用等节假日，现在就开始装修*](https://codeburst.io/no-need-to-wait-for-the-holidays-start-decorating-now-67b9dabd60d7)

[*用阿波罗和高阶组件管理本地状态*](https://itnext.io/managing-local-state-with-apollo-client-3be522258645)

[*React 大会喝酒游戏*](https://medium.com/@andricokaroulla/the-react-conference-drinking-game-7a996bfbef3)

[*使用 Lerna、Travis 和现在的*](https://codeburst.io/develop-and-deploy-your-own-react-monorepo-app-in-under-2-hours-using-lerna-travis-and-now-2b140d647238) ，在 2 小时内开发和部署您自己的 React monorepo 应用程序