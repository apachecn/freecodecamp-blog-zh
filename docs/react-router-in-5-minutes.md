# 5 分钟学会 React 路由器-初学者教程

> 原文：<https://www.freecodecamp.org/news/react-router-in-5-minutes/>

有时你只有 5 分钟的空闲时间。与其浪费在社交媒体上，不如来个 5 分钟的 React-Router 介绍吧！在本教程中，我们将通过为 Scrimba 针织店网站构建导航来学习 React 中路由的基础知识。这不是真的，但也许有一天...；)

如果你想对这个主题有一个适当的介绍，你可以加入我的[即将到来的高级 React 课程](https://scrimba.com/g/greact?utm_source=freecodecamp.org&utm_medium=referral&utm_campaign=router_article)的等候名单，或者如果你仍然是一个初学者，请查看我在 React 上的[介绍课程。](https://scrimba.com/g/glearnreact?utm_source=freecodecamp.org&utm_medium=referral&utm_campaign=router_article)

### React-Router 到底是什么？

许多现代网站实际上是由单个页面组成的，它们看起来就像多个页面，因为它们包含了呈现为独立页面的组件。这些通常被称为 T2 单身申请。React Router 的核心是根据 URL 中使用的*路由*(主页使用`/`，`/about`关于页面等)有条件地呈现某些组件。).

例如，我们可以使用 React 路由器将*www.knit-with-scrimba.com/*连接到【www.knit-with-scrimba.com/about】T2 或【www.knit-with-scrimba.com/shop】T4

### 听起来很棒-我如何使用它？

要使用 React 路由器，您首先必须使用 NPM 安装它:

```
npm install react-router-dom 
```

或者，你可以在 Scrimba 中使用[这个操场，它已经写好了完整的代码。](https://scrimba.com/c/cNq8MzCr)

您需要从`react-router-dom`包导入浏览器路由器、路由和交换机:

```
import React, { Component } from 'react';
import { BrowserRouter, Route, Switch } from 'react-router-dom'; 
```

在我的例子中，我将登录页面与另外两个“页面”(实际上只是组件)链接起来，这两个页面分别叫做`Shop`和`About`。

首先，您需要设置您的应用程序，以便与 React Router 配合使用。渲染的所有内容都需要放入`<BrowserRouter>`元素中，所以首先将你的应用程序放入这些元素中。它是一个组件，负责显示您提供给它的各种组件的所有逻辑。

```
// index.js
ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>, 
    document.getElementById('root')
) 
```

接下来，在您的应用程序组件中，添加`Switch`元素(开始和结束标记)。这确保了一次只能渲染一个组件。如果我们不使用这个，我们可以默认使用`Error`组件，我们将在后面编写。

```
function App() {
    return (
        <main>
            <Switch>

            </Switch>
        </main>
    )
} 
```

现在是添加您的`<Route>`标签的时候了。这些是组件之间的链接，应该放在`<Switch>`标签中。

要告诉`<Route>`标签加载哪个组件，只需添加一个`path`属性和您想用`component`属性加载的组件的名称。

```
<Route path='/' component={Home} /> 
```

很多首页网址都是站点名称后面跟`"/"`，比如*www.knit-with-scrimba.com/*。在这种情况下，我们将`exact`添加到路由标签中。这是因为其他 URL 也包含“/”，所以如果我们不告诉应用程序它只需要查找`/`，它会加载第一个匹配路线的 URL，我们会遇到一个非常棘手的问题。

```
function App() {
    return (
        <main>
            <Switch>
                <Route path="/" component={Home} exact />
                <Route path="/about" component={About} />
                <Route path="/shop" component={Shop} />
            </Switch>
        </main>
    )
} 
```

现在将组件导入到应用程序中。您可能希望将它们放在一个单独的“组件”文件夹中，以保持代码的整洁和可读性。

```
import Home from './components/Home';
import About from './components/About';
import Shop from './components/Shop'; 
```

我之前提到的错误消息，如果用户输入了不正确的 URL，就会加载该错误消息。这就像一个普通的`<Route>`标签，但是没有路径。错误组件包含`<h1>Oops! Page not found!</h1>`。别忘了导入到 app 里。

```
function App() {
    return (
        <main>
            <Switch>
                <Route path="/" component={Home} exact />
                <Route path="/about" component={About} />
                <Route path="/shop" component={Shop} />
                <Route component={Error} />
            </Switch>
        </main>
    )
} 
```

到目前为止，我们的网站只能通过输入网址来导航。为了向站点添加可点击的链接，我们使用 React Router 中的`Link`元素，并设置一个新的`Navbar`组件。同样，不要忘记将新组件导入到应用程序中。

现在为应用程序中的每个组件添加一个`Link`，并使用`to="URL"`来链接它们。

```
function Navbar() {
  return (
    <div>
      <Link to="/">Home </Link>
      <Link to="/about">About Us </Link>
      <Link to="/shop">Shop Now </Link>
    </div>
  );
}; 
```

您的网站现在有可点击的链接，可以在您的单页应用程序中导航！

### 结论

所以我们有它。如果你想在 React 应用中轻松导航，忘记锚标签，添加 React 路由器。它是干净的，有条理的，它使得添加和删除页面变得更加容易。

要了解更多关于 React 挂钩和 React 的其他强大功能，您可以加入我的[即将到来的高级 React 课程的等候名单。](https://scrimba.com/g/greact?utm_source=freecodecamp.org&utm_medium=referral&utm_campaign=router_article)

或者如果你正在寻找一些对初学者更友好的东西，你可以看看我在 React 上的入门课程。

快乐编码；)