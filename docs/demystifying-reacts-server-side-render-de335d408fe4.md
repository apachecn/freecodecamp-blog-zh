# 揭开 React 中服务器端渲染的神秘面纱

> 原文：<https://www.freecodecamp.org/news/demystifying-reacts-server-side-render-de335d408fe4/>

让我们仔细看看这个特性，它允许你用**反应**构建**通用** **应用**。

服务器端呈现——从这里开始称为 SSR 是指**前端框架**在**后端系统**上运行时呈现标记的能力。

能够在服务器和客户端上渲染的应用程序被称为通用应用程序。

### 何必呢？

为了理解为什么需要 SSR，我们需要理解 web 应用程序在过去 10 年中的演变。

这与 [*单页应用*](https://medium.com/@NeotericEU/single-page-application-vs-multiple-page-application-2591588efe58)*——*SPA 从这里开始*的兴起紧密相连。与传统的服务器渲染应用相比，spa 在速度和 UX 方面具有巨大优势。*

但是有一个问题。最初的服务器请求通常是返回一个**空的** **HTML** 文件和一堆 CSS 和 JavaScript (JS)链接。然后需要获取外部文件以呈现相关的标记。

这意味着用户将不得不等待更长时间的初始渲染。这也意味着爬虫程序可能会将您的页面解释为空的。

因此，我们的想法是首先在服务器上呈现您的应用程序，然后在客户端利用 SPAs 的功能。

**SSR + SPA =通用应用***

*你会在一些文章中发现术语*同构 app*——这是一回事。

现在，用户不必等待你的 JS 加载，只要最初的请求返回一个响应，就会得到一个完全呈现的******HTML**。****

****想象一下用户在慢速 3G 网络上导航的巨大改善。你几乎可以立即在他们的屏幕上看到内容，而不是等待 20 多秒来加载网站。****

****![1*v0wyppYBPaeqoeKRplinvQ](img/1e05bfe7fda3ce81efc489374a5b305b.png)****

****现在，对服务器的所有请求都返回完全呈现的 HTML。对你的 SEO 部门来说是个好消息！****

****[爬虫](https://en.wikipedia.org/wiki/Web_crawler)现在会将你的网站视为网络上任何其他静态网站，并将**索引**你在服务器上呈现的所有内容。****

****概括来说，我们从 SSR 中获得的两个主要好处是:****

*   ****初始页面渲染速度更快****
*   ****完全可索引的 HTML 页面****

## ****了解 SSR —一步一个脚印****

****让我们采用迭代方法来构建完整的 SSR 示例。我们从 React 的服务器渲染 API 开始，我们将在每一步添加一些东西。****

****您可以跟随[这个存储库](https://github.com/alexnm/react-ssr)和在那里为每个步骤定义的标签。****

### ****基本设置****

****重要的事情先来。为了使用 SSR，我们需要一个服务器！我们将使用一个简单的 *Express* 应用来呈现我们的 React 应用。****

```
**`import express from "express";
import path from "path";

import React from "react";
import { renderToString } from "react-dom/server";
import Layout from "./components/Layout";

const app = express();

app.use( express.static( path.resolve( __dirname, "../dist" ) ) );

app.get( "/*", ( req, res ) => {
    const jsx = ( <Layout /> );
    const reactDom = renderToString( jsx );

    res.writeHead( 200, { "Content-Type": "text/html" } );
    res.end( htmlTemplate( reactDom ) );
} );

app.listen( 2048 );

function htmlTemplate( reactDom ) {
    return `
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>React SSR</title>
        </head>

        <body>
            <div id="app">${ reactDom }</div>
            <script src="./app.bundle.js"></script>
        </body>
        </html>
    `;
}`**
```

****我们需要告诉 Express 从我们的输出文件夹提供静态文件——第 10 行。****

****我们创建一个处理所有非静态传入请求的路由。该路由将使用呈现的 HTML 进行响应。****

****我们使用`renderToString`——第 13–14 行——将我们的起始 JSX 转换成一个`string`,然后插入到 HTML 模板中。****

****注意，我们对客户端代码和服务器代码使用了相同的 Babel 插件。于是 *JSX* 和 *ES 模块*在`server.js`内部工作。****

****客户端上对应的方法现在是`ReactDOM.hydrate`。该函数将使用服务器呈现的 React 应用程序，并将附加事件处理程序。****

```
**`import ReactDOM from "react-dom";
import Layout from "./components/Layout";

const app = document.getElementById( "app" );
ReactDOM.hydrate( <Layout />, app );`**
```

****要查看完整的例子，请查看[库](https://github.com/alexnm/react-ssr/tree/basic)中的`basic`标签。****

****就是这样！您刚刚创建了您的第一个**服务器渲染的** React 应用程序！****

#### ****反应路由器****

****我们必须诚实地说，这个应用程序并没有做太多的事情。所以让我们添加一些路由，看看我们如何处理服务器部分。****

```
**`import { Link, Switch, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import Contact from "./Contact";

export default class Layout extends React.Component {
    /* ... */

    render() {
        return (
            <div>
                <h1>{ this.state.title }</h1>
                <div>
                    <Link to="/">Home</Link>
                    <Link to="/about">About</Link>
                    <Link to="/contact">Contact</Link>
                </div>
                <Switch>
                    <Route path="/" exact component={ Home } />
                    <Route path="/about" exact component={ About } />
                    <Route path="/contact" exact component={ Contact } />
                </Switch>
            </div>
        );
    }
}`**
```

****`Layout`组件现在在客户机上呈现多条路线。****

****我们需要在服务器上模拟路由器设置。下面你可以看到应该做的主要改变。****

```
**`/* ... */
import { StaticRouter } from "react-router-dom";
/* ... */

app.get( "/*", ( req, res ) => {
    const context = { };
    const jsx = (
        <StaticRouter context={ context } location={ req.url }>
            <Layout />
        </StaticRouter>
    );
    const reactDom = renderToString( jsx );

    res.writeHead( 200, { "Content-Type": "text/html" } );
    res.end( htmlTemplate( reactDom ) );
} );

/* ... */`**
```

****在服务器上，我们需要将我们的 React 应用程序包装在`StaticRouter`组件中，并提供`location`。****

****顺便提一下，`context`用于在呈现 React DOM 时跟踪潜在的重定向。这需要用来自服务器的 [3XX 响应](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#Redirection_messages)来处理。****

****完整的例子可以在[同一个库](https://github.com/alexnm/react-ssr/releases/tag/router)的`router`标签上看到。****

#### ****Redux****

****现在我们有了路由功能，让我们集成 [Redux](https://redux.js.org/) 。****

****在简单的场景中，我们需要 Redux 来处理客户端的状态管理。但是如果我们需要根据那个状态来呈现 DOM 的一部分呢？在服务器上初始化 Redux 是有意义的。****

****如果你的应用程序正在**服务器**上**调度** **动作**，它需要**捕获**状态，并通过网络与 HTML 一起发送。在客户端，我们将初始状态输入 Redux。****

****让我们先来看看服务器:****

```
**`/* ... */
import { Provider as ReduxProvider } from "react-redux";
/* ... */

app.get( "/*", ( req, res ) => {
    const context = { };
    const store = createStore( );

    store.dispatch( initializeSession( ) );

    const jsx = (
        <ReduxProvider store={ store }>
            <StaticRouter context={ context } location={ req.url }>
                <Layout />
            </StaticRouter>
        </ReduxProvider>
    );
    const reactDom = renderToString( jsx );

    const reduxState = store.getState( );

    res.writeHead( 200, { "Content-Type": "text/html" } );
    res.end( htmlTemplate( reactDom, reduxState ) );
} );

app.listen( 2048 );

function htmlTemplate( reactDom, reduxState ) {
    return `
        /* ... */

        <div id="app">${ reactDom }</div>
        <script>
            window.REDUX_DATA = ${ JSON.stringify( reduxState ) }
        </script>
        <script src="./app.bundle.js"></script>

        /* ... */
    `;
}`**
```

****它看起来很难看，但是我们需要将完整的 JSON 状态和我们的 HTML 一起发送。****

****然后我们看看客户:****

```
**`import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router } from "react-router-dom";
import { Provider as ReduxProvider } from "react-redux";

import Layout from "./components/Layout";
import createStore from "./store";

const store = createStore( window.REDUX_DATA );

const jsx = (
    <ReduxProvider store={ store }>
        <Router>
            <Layout />
        </Router>
    </ReduxProvider>
);

const app = document.getElementById( "app" );
ReactDOM.hydrate( jsx, app );`**
```

****注意，我们调用了两次`createStore`,第一次在服务器上，然后在客户机上。然而，在客户机上，我们用保存在服务器上的任何状态来初始化状态。这个过程类似于 DOM 水合。****

****完整的例子可以在[同一个库](https://github.com/alexnm/react-ssr/releases/tag/redux)的`redux`标签上看到。****

#### ****取数据****

****拼图的最后一块是加载数据。这就是事情变得有点棘手的地方。假设我们有一个服务于 JSON 数据的 API。****

****在我们的代码库中，我从公共 API 中获取 2018 年一级方程式赛季[的所有事件。假设我们想在**主页**上显示所有事件。](https://ergast.com/mrd/)****

****在安装了 React 应用程序并呈现了所有内容之后，我们只能从客户端调用 API。但是这将对 UX 产生不好的影响，可能会在用户看到相关内容之前显示一个旋转器或加载器。****

****我们已经有了 Redux，作为一种在服务器上存储数据并将其发送到客户端的方式。****

****如果我们在服务器上进行 API 调用，将结果存储在 Redux 中，然后为客户端呈现包含相关数据的完整 HTML，会怎么样？****

****但是我们怎么知道哪些电话需要打呢？****

****首先，我们需要一种不同的方式来声明路由。所以我们切换到所谓的路由配置文件。****

```
**`export default [
    {
        path: "/",
        component: Home,
        exact: true,
    },
    {
        path: "/about",
        component: About,
        exact: true,
    },
    {
        path: "/contact",
        component: Contact,
        exact: true,
    },
    {
        path: "/secret",
        component: Secret,
        exact: true,
    },
];`**
```

****我们静态地声明每个组件的数据需求。****

```
**`/* ... */
import { fetchData } from "../store";

class Home extends React.Component {
    /* ... */

    render( ) {
        const { circuits } = this.props;

        return (
            /* ... */
        );
    }
}
Home.serverFetch = fetchData; // static declaration of data requirements

/* ... */`**
```

****记住`serverFetch`是编出来的，你可以用任何听起来对你更好的。****

****这里需要注意的是，`fetchData`是一个 [Redux thunk 动作](https://github.com/gaearon/redux-thunk)，在被调度时返回一个承诺。****

****在服务器上，我们可以使用来自`react-router`的一个特殊函数，叫做`matchRoute`。****

```
**`/* ... */
import { StaticRouter, matchPath } from "react-router-dom";
import routes from "./routes";

/* ... */

app.get( "/*", ( req, res ) => {
    /* ... */

    const dataRequirements =
        routes
            .filter( route => matchPath( req.url, route ) ) // filter matching paths
            .map( route => route.component ) // map to components
            .filter( comp => comp.serverFetch ) // check if components have data requirement
            .map( comp => store.dispatch( comp.serverFetch( ) ) ); // dispatch data requirement

    Promise.all( dataRequirements ).then( ( ) => {
        const jsx = (
            <ReduxProvider store={ store }>
                <StaticRouter context={ context } location={ req.url }>
                    <Layout />
                </StaticRouter>
            </ReduxProvider>
        );
        const reactDom = renderToString( jsx );

        const reduxState = store.getState( );

        res.writeHead( 200, { "Content-Type": "text/html" } );
        res.end( htmlTemplate( reactDom, reduxState ) );
    } );
} );

/* ... */`**
```

****这样，我们得到了一个组件列表，当 React 被渲染为当前 URL 上的字符串时，这些组件将被挂载。****

****我们收集*数据需求*，并等待所有 API 调用返回。最后，我们继续服务器渲染，但是数据已经在 Redux 中可用。****

****完整的例子可以在[同一个库](https://github.com/alexnm/react-ssr/tree/fetch-data)的`fetch-data`标签上看到。****

****您可能会注意到这带来了性能损失，因为我们将渲染延迟到获取数据之后。****

****这是您开始比较指标并尽最大努力理解哪些呼叫是必要的，哪些不是必要的。例如，获取电子商务应用程序的产品可能是至关重要的，但价格和侧栏过滤器可以延迟加载。****

#### ****头盔****

****作为奖励，我们来看看 SEO。在使用 React 时，您可能希望在您的`<he` ad >标签中设置不同的值。例如，你可能想要 se *t the* t *itle，meet*a*标签，key* 单词等等。****

****请记住，`<he` ad >标签通常不是 React 应用程序的一部分！****

****[反应——头盔](https://github.com/nfl/react-helmet)让你在这个场景中覆盖。并且对 SSR 有很大的支持。****

```
**`import React from "react";
import Helmet from "react-helmet";

const Contact = () => (
    <div>
        <h2>This is the contact page</h2>
        <Helmet>
            <title>Contact Page</title>
            <meta name="description" content="This is a proof of concept for React SSR" />
        </Helmet>
    </div>
);

export default Contact;`**
```

****您只需将您的`head`数据添加到组件树中的任意位置。这为您在客户端上安装的 React 应用程序之外更改值提供了支持。****

****现在我们增加了对 SSR 的支持:****

```
**`/* ... */
import Helmet from "react-helmet";
/* ... */

app.get( "/*", ( req, res ) => {
    /* ... */
        const jsx = (
            <ReduxProvider store={ store }>
                <StaticRouter context={ context } location={ req.url }>
                    <Layout />
                </StaticRouter>
            </ReduxProvider>
        );
        const reactDom = renderToString( jsx );
        const reduxState = store.getState( );
        const helmetData = Helmet.renderStatic( );

        res.writeHead( 200, { "Content-Type": "text/html" } );
        res.end( htmlTemplate( reactDom, reduxState, helmetData ) );
    } );
} );

app.listen( 2048 );

function htmlTemplate( reactDom, reduxState, helmetData ) {
    return `
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            ${ helmetData.title.toString( ) }
            ${ helmetData.meta.toString( ) }
            <title>React SSR</title>
        </head>

        /* ... */
    `;
}`**
```

****现在我们有了一个全功能的 React SSR 示例！****

****我们从一个简单的 HTML 渲染开始，在一个 *Express* 应用的上下文中。我们逐渐增加了路由、状态管理和数据获取。最后，我们处理了 React 应用程序范围之外的更改。****

****最终的代码库在[的`master`上，也就是之前提到的那个库](https://github.com/alexnm/react-ssr)上。****

#### ****结论****

****正如您所看到的，SSR 不是什么大事，但它可能会变得复杂。如果你一步一步地建立你的需求，它会更容易掌握。****

****在你的应用中加入 SSR 值得吗？一如既往，视情况而定。如果你的网站是公开的，成千上万的用户可以访问，这是必须的。但是，如果您正在构建一个类似工具/仪表板的应用程序，这可能不值得。****

****然而，利用通用应用程序的力量对于前端社区来说是向前迈进了一步。****

****您对 SSR 使用类似的方法吗？或者你认为我错过了什么？请在下面或 Twitter 上给我留言。****

****如果你觉得这篇文章有用，帮我分享给社区吧！****