# React 路由器的完全初学者指南(包括路由器挂钩)

> 原文：<https://www.freecodecamp.org/news/a-complete-beginners-guide-to-react-router-include-router-hooks/>

React 是一个用于构建用户界面的 JavaScript 库。在 React Router 的帮助下，我们还可以扩展它来构建多页面应用程序。这是一个第三方库，支持 React 应用中的路由。

在本教程中，我们将介绍 React 路由器入门所需的所有知识。

*   [设置项目](#setting-up-the-project)
*   [什么是路由？](#what-is-routing)
*   [设置路由器](#setting-up-the-router)
*   [渲染路线](#rendering-routes)
*   [使用链接切换页面](#using-links-to-switch-pages)
*   [通过路线参数](#passing-route-parameters)
*   [以编程方式导航](#navigating-programmatically)
*   [重定向到另一个页面](#redirecting-to-another-page)
*   [重定向到 404 页面](#redirecting-to-404-page)
*   [守卫路线](#guarding-routes)
*   [路由器挂钩](#router-hooks)
*   [使用历史](#usehistory)
*   [useParams](#useparams)
*   [使用位置](#uselocation)
*   [最终想法](#final-thoughts)
*   [接下来的步骤](#next-steps)

## 设置项目

为了能够跟进，您需要通过在终端中运行以下命令来创建一个新的 React 应用程序:

```
npx create-react-app react-router-guide 
```

然后，将这些代码行添加到`App.js`文件中:

```
import React from "react";
import "./index.css"

export default function App() {
  return (
    <main>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
        </nav>
     </main>
  );
}
// Home Page
const Home = () => (
  <Fragment>
    <h1>Home</h1>
    <FakeText />
  </Fragment>
  );
// About Page
const About = () => (
  <Fragment>
    <h1>About</h1>
    <FakeText />
  </Fragment>
  );
// Contact Page
const Contact = () => (
  <Fragment>
    <h1>Contact</h1>
    <FakeText />
  </Fragment>
  );

const FakeText = () => (
  <p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
  </p>
  ) 
```

然后，如果你准备好了，让我们从回答一个重要的问题开始:什么是路由？

## 什么是路由？

路由是向用户显示不同页面的能力。这意味着用户可以通过输入 URL 或点击元素在应用程序的不同部分之间移动。

您可能已经知道，默认情况下，React 没有路由。为了在我们的项目中启用它，我们需要添加一个名为 [react-router](https://reacttraining.com/react-router/web/guides/quick-start) 的库。

要安装它，您必须在终端中运行以下命令:

```
yarn add react-router-dom 
```

或者

```
npm install react-router-dom 
```

现在，我们已经成功安装了路由器，让我们在下一部分开始使用它。

## 设置路由器

为了在 React 应用程序中启用路由，我们首先需要从`react-router-dom`导入`BrowserRouter`。

在`App.js`文件中，输入以下内容:

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router } from "react-router-dom";

export default function App() {
  return (
  <Router>
    <main>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
    </main>
</Router>
  );
} 
```

这应该包含我们的应用程序中需要路由的所有内容。这意味着，如果我们需要在整个应用程序中进行路由，我们必须用`BrowserRouter`包装更高的组件。

顺便说一句，你不必像我在这里一样重命名`BrowserRouter as Router`，我只是想保持东西的可读性。

光靠路由器做不了多少事。所以让我们在下一节中添加一条路线。

## 渲染路线

为了渲染路由，我们必须从路由器包中导入`Route`组件。

在您的`App.js`文件中，添加以下代码:

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route } from "react-router-dom";

export default function App() {
  return (
  <Router>
    <main>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
  <Route path="/" render={() => <h1>Welcome!</h1>} />
    </main>
</Router>
  );
} 
```

然后，将它添加到我们想要呈现内容的位置。组件有几个属性。但是在这里，我们只需要`path`和`render`。

`path`:路线的路径。这里，我们使用`/`来定义主页的路径。

`render`:每当到达路线时将显示内容。这里，我们将向用户呈现一条欢迎消息。

在某些情况下，像这样的服务路线是非常好的。但是想象一下，当我们必须处理一个真实的组件时——使用`render`可能不是正确的解决方案。

那么，怎样才能展示一个真实的组件呢？嗯，`Route`组件有另一个名为`component`的属性。

让我们稍微更新一下我们的例子，看看它是如何工作的。

在您的`App.js`文件中，添加以下代码:

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route } from "react-router-dom";

export default function App() {
  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>

    <Route path="/" component={Home} />
    </main>
</Router>
  );
}

const Home = () => (
  <Fragment>
    <h1>Home</h1>
    <FakeText />
  </Fragment>
  ); 
```

现在，我们的路由将加载`Home`组件，而不是呈现消息。

为了获得 React Router 的全部功能，我们需要多个页面和链接。我们已经有了页面(如果您需要，也可以有组件)，所以现在让我们添加一些链接，以便我们可以在页面之间切换。

## 使用链接切换页面

为了向我们的项目添加链接，我们将再次使用 React 路由器。

在您的`App.js`文件中，添加以下代码:

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route, Link } from "react-router-dom";

export default function App() {
  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/contact">Contact</Link></li>
        </ul>
      </nav>

    <Route path="/" exact component={Home} />
    <Route path="/about"  component={About} />
    <Route path="/contact"  component={Contact} />

    </main>
</Router>
  );
}

const Home = () => (
  <Fragment>
    <h1>Home</h1>
    <FakeText />
  </Fragment>
  );

const About = () => (
  <Fragment>
    <h1>About</h1>
    <FakeText />
  </Fragment>
  );

const Contact = () => (
  <Fragment>
    <h1>Contact</h1>
    <FakeText />
  </Fragment>
  ); 
```

导入`Link`之后，我们必须对我们的导航栏进行一点更新。现在，React Router 不再使用`a`标签和`href`，而是使用`Link`和`to`来切换页面，而无需重新加载。

然后，我们需要添加两个新路径，`About`和`Contact`，以便能够在页面或组件之间切换。

现在，我们可以通过链接访问应用程序的不同部分。但是我们的路由器有一个问题:即使我们切换到其他页面,`Home`组件也总是显示。

这是因为 React 路由器会检查定义的`path`是否以`/`开头。如果是这种情况，它将呈现组件。这里，我们的第一条路线从`/`开始，所以每次都会渲染`Home`组件。

然而，我们仍然可以通过向`Route`添加`exact`属性来改变默认行为。

在`App.js`中，添加:

```
 <Route path="/" exact component={Home} /> 
```

通过用`exact`更新`Home`路线，现在只有当它匹配完整路径时才会被渲染。

我们还可以通过用`Switch`包装我们的路由来增强它，告诉 React 路由器一次只加载一条路由。

在`App.js`中，添加:

```
import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";

  <Switch>
    <Route path="/" exact component={Home} />
    <Route path="/about"  component={About} />
    <Route path="/contact"  component={Contact} />
  </Switch> 
```

现在我们有了新的链接，让我们用它们来传递参数。

## 传递路线参数

为了在页面之间传递数据，我们必须更新我们的示例。

在您的`App.js`文件中，添加以下代码:

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";

export default function App() {
  const name = 'John Doe'
  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to={`/about/${name}`}>About</Link></li>
          <li><Link to="/contact">Contact</Link></li>
        </ul>
      </nav>
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/about/:name"  component={About} />
      <Route path="/contact"  component={Contact} />
    </Switch>
    </main>
</Router>
  );
}

const Home = () => (
  <Fragment>
    <h1>Home</h1>
    <FakeText />
  </Fragment>
  );

const About = ({match:{params:{name}}}) => (
  // props.match.params.name
  <Fragment>
    <h1>About {name}</h1>
    <FakeText />
  </Fragment>
);

const Contact = () => (
  <Fragment>
    <h1>Contact</h1>
    <FakeText />
  </Fragment>
  ); 
```

正如您在这里看到的，我们首先声明一个新的常量`name`，它将作为参数传递给`About`页面。我们将`name`附加到相应的链接上。

这样，我们现在必须通过调整路径来更新`About`路由，以接收`name`作为参数`path="/about/:name"`。

现在，参数将作为道具从`About`组件接收。我们现在唯一要做的就是销毁道具，拿回`name`属性。顺便说一下，`{match:{params:{name}}}`和`props.match.params.name`是一样的。

到目前为止，我们已经做了很多工作。但是在某些情况下，我们不想使用链接在页面之间导航。

有时，我们必须等待一个操作完成后才能导航到下一页。

所以，让我们在下一节处理这个案例。

## 以编程方式导航

我们收到的道具有一些方便的方法，可以用来在页面之间导航。

在`App.js`中，添加:

```
const Contact = ({history}) => (
  <Fragment>
    <h1>Contact</h1>
    <button onClick={() => history.push('/') } >Go to home</button>
    <FakeText />
  </Fragment>
  ); 
```

在这里，我们从收到的道具中提取`history`对象。它有一些简便的方法，如`goBack`、`goForward`等。但是在这里，我们将使用`push`方法来访问主页。

现在，让我们来处理当我们想要在一个动作之后重定向我们的用户时的情况。

## 重定向到另一个页面

React 路由器还有另一个名为`Redirect`的组件。正如您所猜测的，它帮助我们将用户重定向到另一个页面

在`App.js`中，添加:

```
import { BrowserRouter as Router, Route, Link, Switch, Redirect } from "react-router-dom";

const About = ({match:{params:{name}}}) => (
  // props.match.params.name
  <Fragment>
    { name !== 'John Doe' ? <Redirect to="/" /> : null }
    <h1>About {name}</h1>
    <FakeText />
  </Fragment>
); 
```

现在，如果作为参数传递的`name`不等于`John Doe`，用户将被重定向到主页。

你可能会说你应该用`props.history.push('/)`重定向用户。嗯，`Redirect`组件替换了页面，因此用户不能返回到上一页。但是，用推的方法他们可以。然而，你可以用`props.history.replace('/)`来模仿`Redirect`的行为。

现在让我们继续处理用户点击一条不存在的路线的情况。

## 重定向到 404 页面

要将用户重定向到 404 页面，您可以创建一个组件来显示它。但是在这里，为了简单起见，我将只显示一条带有`render`的消息。

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";

export default function App() {
  const name = 'John Doe'

  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to={`/about/${name}`}>About</Link></li>
          <li><Link to="/contact">Contact</Link></li>
        </ul>
      </nav>
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/about/:name"  component={About} />
      <Route path="/contact"  component={Contact} />
      <Route render={() => <h1>404: page not found</h1>} />

    </Switch>
    </main>
</Router>
  );
} 
```

我们添加的新路由将捕获每一条不存在的路径，并将用户重定向到 404 页面。

现在，让我们继续前进，在下一节学习如何保护我们的路线。

## 守卫路线

有许多方法可以保护路线做出反应。但是在这里，我只是检查用户是否通过了身份验证，并将他们重定向到适当的页面。

```
import React, { Fragment } from "react";
import "./index.css"

import { BrowserRouter as Router, Route, Link, Switch } from "react-router-dom";

export default function App() {
  const name = 'John Doe'
  const isAuthenticated = false
  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to={`/about/${name}`}>About</Link></li>
          <li><Link to="/contact">Contact</Link></li>
        </ul>
      </nav>
    <Switch>
      <Route path="/" exact component={Home} />
      {
      isAuthenticated ? 
      <>
      <Route path="/about/:name"  component={About} />
      <Route path="/contact"  component={Contact} />
      </> : <Redirect to="/" />
      }

    </Switch>
    </main>
</Router>
  );
} 
```

正如您在这里看到的，我声明了一个变量来模拟身份验证。然后，检查用户是否经过身份验证。如果是，则呈现受保护的页面。否则将他们重定向到主页。

到目前为止，我们已经介绍了很多，但是还有一个有趣的部分:路由器挂钩。

让我们转到最后一节，介绍钩子。

## 路由器挂钩

路由器挂钩让事情变得简单多了。现在，您可以以一种简单而优雅的方式访问历史、位置或参数。

### 使用历史记录

挂钩让我们可以访问历史实例，而不用从 props 中取出它。

```
import { useHistory } from "react-router-dom";

const Contact = () => {
const history = useHistory();
return (
  <Fragment>
    <h1>Contact</h1>
    <button onClick={() => history.push('/') } >Go to home</button>
  </Fragment>
  )
  }; 
```

### 区分

这个钩子帮助我们在不使用 props 对象的情况下获得 URL 上传递的参数。

```
import { BrowserRouter as Router, Route, Link, Switch, useParams } from "react-router-dom";

export default function App() {
  const name = 'John Doe'
  return (
   <Router>
    <main>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to={`/about/${name}`}>About</Link></li>
        </ul>
      </nav>
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/about/:name"  component={About} />
    </Switch>
    </main>
</Router>
  );
}

const About = () => {
  const { name } = useParams()
  return (
  // props.match.params.name
  <Fragment>
    { name !== 'John Doe' ? <Redirect to="/" /> : null }
    <h1>About {name}</h1>
    <Route component={Contact} />
  </Fragment>
)
}; 
```

### 混乱

这个钩子返回代表当前 URL 的位置对象。

```
import { useLocation } from "react-router-dom";

const Contact = () => {
const { pathname } = useLocation();

return (
  <Fragment>
    <h1>Contact</h1>
    <p>Current URL: {pathname}</p>
  </Fragment>
  )
  }; 
```

## 最后的想法

React Router 是一个非常棒的库，它可以帮助我们从单页到多页的应用程序，具有很好的可用性。(请记住，归根结底，它仍然是一个单页应用程序)。

现在有了路由器挂钩，你可以看到它们是多么的简单和优雅。它们绝对是你下一个项目中要考虑的东西。

你可以在我的博客上阅读更多我的文章。

## 后续步骤

[React 路由器文档](https://reacttraining.com/react-router/web)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com/s/photos/route?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片