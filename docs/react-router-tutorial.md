# React 路由器教程——如何渲染、重定向、切换、链接等等，包括代码示例

> 原文：<https://www.freecodecamp.org/news/react-router-tutorial/>

如果您刚刚开始使用 React，您可能还在思考整个单页面应用程序的概念。

传统的路由工作是这样的:假设你在 URL 中输入`/contact`。浏览器将向服务器发出 GET 请求，服务器将返回一个 HTML 页面作为响应。

但是，有了新的单页面应用程序范例，所有的 URL 请求都使用客户端代码来处理。

在 React 的上下文中应用这一点，每个页面都将是一个 React 组件。React-Router 匹配 URL 并加载特定页面的组件。

一切都发生得如此之快，如此无缝，以至于用户可以在浏览器上获得类似原生应用的体验。在路线转换之间没有浮华的空白页。

在本文中，您将了解如何使用 React-Router 及其组件创建单页面应用程序。所以打开你最喜欢的文本编辑器，让我们开始吧。

## 设置项目

通过运行以下命令创建一个新的 React 项目。

```
yarn create react-app react-router-demo
```

我将使用 yarn 来安装依赖项，但是您也可以使用 npm。

接下来，我们来安装`react-router-dom`。

```
yarn add react-router-dom
```

为了设计组件的样式，我将使用布尔玛 CSS 框架。所以让我们也加上这一点。

```
yarn add bulma
```

接下来，在`index.js`文件中导入`bulma.min.css`,并从`App.js`文件中清除所有样板代码。

```
import "bulma/css/bulma.min.css";
```

现在您已经设置好了项目，让我们从创建几个页面组件开始。

## 创建页面组件

在 src 文件夹中创建一个 pages 目录，我们将在其中存放所有的页面组件。

对于这个演示，创建三个页面——主页、关于和个人资料。

将以下内容粘贴到“主页”和“关于”组件中。

```
// pages/Home.js

import React from "react";

const Home = () => (
  <div>
    <h1 className="title is-1">This is the Home Page</h1>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras gravida,
      risus at dapibus aliquet, elit quam scelerisque tortor, nec accumsan eros
      nulla interdum justo. Pellentesque dignissim, sapien et congue rutrum,
      lorem tortor dapibus turpis, sit amet vestibulum eros mi et odio.
    </p>
  </div>
);

export default Home; 
```

```
// pages/About.js

import React from "react";

const About = () => (
  <div>
    <h1 className="title is-1">This is the About Page</h1>
    <p>
      Class aptent taciti sociosqu ad litora torquent per conubia nostra, per
      inceptos himenaeos. Vestibulum ante ipsum primis in faucibus orci luctus
      et ultrices posuere cubilia curae; Duis consequat nulla ac ex consequat,
      in efficitur arcu congue. Nam fermentum commodo egestas.
    </p>
  </div>
);

export default About; 
```

我们将在本文的后面创建概要页面。

## 创建导航栏组件

让我们从创建应用程序的导航栏开始。该组件将利用来自`react-router-dom`的`<NavLink />`组件。

在 src 文件夹内创建一个名为“组件”的目录。

```
// components/Navbar.js

import React, { useState } from "react";
import { NavLink } from "react-router-dom";

const Navbar = () => {
  const [isOpen, setOpen] = useState(false);
  return ( 
  	<nav
      className="navbar is-primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div className="container">
      	{/* ... */}
      </div>
    </nav>
  );
 };

 export default Navbar;
```

`isOpen`状态变量将用于触发移动或平板设备上的菜单。

所以让我们添加汉堡菜单。

```
const Navbar = () => {
  const [isOpen, setOpen] = useState(false);
  return ( 
  	<nav
      className="navbar is-primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div className="container">
      <div className="navbar-brand">
          <a
            role="button"
            className={`navbar-burger burger ${isOpen && "is-active"}`}
            aria-label="menu"
            aria-expanded="false"
            onClick={() => setOpen(!isOpen)}
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>
      	{/* ... */}
      </div>
    </nav>
  );
 };
```

要在菜单中添加链接，请通过`react-router-dom`使用`<NavLink />`组件。

`NavLink`组件提供了一种在应用程序中导航的声明性方式。它类似于`Link`组件，除了如果链接是活动的，它可以将活动样式应用于链接。

要指定导航到哪条路线，使用`to`属性并传递路径名。
`activeClassName`prop 将添加一个活动类到链接，如果它当前是活动的。

```
<NavLink
    className="navbar-item"
    activeClassName="is-active"
    to="/"
    exact
>
	Home
</NavLink>
```

在浏览器上，`NavLink`组件被呈现为一个`<a>`标签，带有在`to`属性中传递的`href`属性值。

此外，这里您需要指定`exact`属性，以便它与 URL 精确匹配。

添加所有链接，完成`Navbar`组件。

```
import React, { useState } from "react";
import { NavLink } from "react-router-dom";

const Navbar = () => {
  const [isOpen, setOpen] = useState(false);
  return (
    <nav
      className="navbar is-primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div className="container">
        <div className="navbar-brand">
          <a
            role="button"
            className={`navbar-burger burger ${isOpen && "is-active"}`}
            aria-label="menu"
            aria-expanded="false"
            onClick={() => setOpen(!isOpen)}
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

        <div className={`navbar-menu ${isOpen && "is-active"}`}>
          <div className="navbar-start">
            <NavLink className="navbar-item" activeClassName="is-active" to="/">
              Home
            </NavLink>

            <NavLink
              className="navbar-item"
              activeClassName="is-active"
              to="/about"
            >
              About
            </NavLink>

            <NavLink
              className="navbar-item"
              activeClassName="is-active"
              to="/profile"
            >
              Profile
            </NavLink>
          </div>

          <div className="navbar-end">
            <div className="navbar-item">
              <div className="buttons">
                <a className="button is-white">Log in</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
  );
};

export default Navbar; 
```

如果你注意到了，我添加了一个登录按钮。当我们稍后在指南中讨论受保护的路线时，我们将再次回到`Navbar`组件。

## 呈现页面

既然已经设置了`Navbar`组件，让我们将它添加到页面中，并从呈现页面开始。

由于导航栏是所有页面的通用组件，因此在通用布局中呈现`Navbar`将是一种更好的方法，而不是在每个页面组件中调用该组件。

```
// App.js

function App() {
  return (
    <>
      <Navbar />
      <div className="container mt-2" style={{ marginTop: 40 }}>
        {/* Render the page here */}
      </div>
    </>
  );
}
```

现在，在容器中添加页面组件。

```
// App.js

function App() {
  return (
    <>
      <Navbar />
      <div className="container mt-2" style={{ marginTop: 40 }}>
        <Home />
      	<About />
      </div>
    </>
  );
}
```

如果现在查看结果，您会注意到 Home 和 About 页面组件都呈现在页面上。那是因为我们还没有添加任何路由逻辑。

您需要从 React Router 导入`BrowserRouter`组件来添加路由组件的功能。您需要做的就是将所有页面组件包装在`BrowserRouter`组件中。这将使所有页面组件都具有路由逻辑。完美！

但是同样，结果不会发生任何变化——您仍然会看到两个页面都被渲染。只有当 URL 匹配特定路径时，才需要呈现页面组件。这就是 React 路由器的`Route`组件发挥作用的地方。

`Router`组件有一个接受页面路径的`path`属性，页面组件应该用`Router`包装，如下所示。

```
<Route path="/about">
  <About />
</Route>
```

所以让我们对`Home`组件做同样的事情。

```
<Route exact path="/">
  <Home />
</Route>
```

上面的`exact` prop 告诉`Router`组件精确匹配路径。如果不在`/`路径上添加`exact`道具，它会匹配所有以`/`开头的路线，包括`/about`。

如果您现在查看结果，您仍然会看到两个组件都被渲染。但是，如果你去`/about`，你会注意到只有`About`组件被渲染。您之所以会看到这种现象，是因为即使路由器已经匹配了一条路由，它仍会继续将 URL 与路由进行匹配。

我们需要告诉路由器，一旦它匹配了一条路由，就停止进一步匹配。这是使用 React 路由器中的`Switch`组件完成的。

```
function App() {
  return (
    <BrowserRouter>
      <Navbar />
      <div className="container mt-2" style={{ marginTop: 40 }}>
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
        </Switch>
      </div>
    </BrowserRouter>
  );
}
```

这就对了。您已经在 React 应用中成功配置了路由。

## 受保护的路由和重定向

在处理真实世界的应用程序时，您会在身份验证系统后面有一些路径。您将拥有只能由登录用户访问的路线或页面。在本节中，您将学习如何实现这样的路由。

***请注意*** *我不会创建任何登录表单或让任何后端服务验证用户。在实际的应用程序中，您不会像这里演示的那样实现身份验证。*

让我们创建只应由经过身份验证的用户访问的配置文件页面组件。

```
// pages/Profile.js

import { useParams } from "react-router-dom";

const Profile = () => {
  const { name } = useParams();
  return (
    <div>
      <h1 className="title is-1">This is the Profile Page</h1>
      <article className="message is-dark" style={{ marginTop: 40 }}>
        <div className="message-header">
          <p>{name}</p>
        </div>
        <div className="message-body">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.{" "}
          <strong>Pellentesque risus mi</strong>, tempus quis placerat ut, porta
          nec nulla. Vestibulum rhoncus ac ex sit amet fringilla. Nullam gravida
          purus diam, et dictum <a>felis venenatis</a> efficitur. Aenean ac{" "}
          <em>eleifend lacus</em>, in mollis lectus. Donec sodales, arcu et
          sollicitudin porttitor, tortor urna tempor ligula, id porttitor mi
          magna a neque. Donec dui urna, vehicula et sem eget, facilisis sodales
          sem.
        </div>
      </article>
    </div>
  );
}; 
```

我们将使用路由参数从 URL 中获取用户名。

将配置文件路由添加到路由器。

```
<Route path="/profile/:name">
  <Profile />
</Route>
```

目前可以直接访问个人资料页面。因此，要使它成为认证路由，需要创建一个高阶组件(HOC)来包装认证逻辑。

```
const withAuth = (Component) => {
  const AuthRoute = () => {
    const isAuth = !!localStorage.getItem("token");
    // ...
  };

  return AuthRoute;
};
```

要确定用户是否经过身份验证，请在用户登录时获取存储在浏览器中的身份验证令牌。如果用户未通过身份验证，则将用户重定向到主页。React 路由器的`Redirect`组件可用于将用户重定向到另一条路径。

```
const withAuth = (Component) => {
  const AuthRoute = () => {
    const isAuth = !!localStorage.getItem("token");
    if (isAuth) {
      return <Component />;
    } else {
      return <Redirect to="/" />;
    }
  };

  return AuthRoute;
};
```

您还可以使用 props 向包装的组件传递其他用户信息，如姓名和用户 ID。

接下来，在概要文件组件中使用`withAuth` HOC。

```
import withAuth from "../components/withAuth";

const Profile = () => {
 // ...
}

export default withAuth(Profile);
```

现在，如果你尝试访问`/profile/JohnDoe`，你将被重定向到主页。这是因为您的浏览器存储中尚未设置身份验证令牌。

好了，让我们回到`Navbar`组件，添加登录和注销功能。当用户通过身份验证时，显示注销按钮，当用户未登录时，显示登录按钮。

```
// components/Navbar.js

const Navbar = () => {
	// ...
    return (
    	<nav
          className="navbar is-primary"
          role="navigation"
          aria-label="main navigation"
        >
        <div className="container">
        	{/* ... */}
            <div className="navbar-end">
            <div className="navbar-item">
              <div className="buttons">
                {!isAuth ? (
                  <button className="button is-white" onClick={loginUser}>
                    Log in
                  </button>
                ) : (
                  <button className="button is-black" onClick={logoutUser}>
                    Log out
                  </button>
                )}
              </div>
            </div>
          </div>
        </div>
        </nav>
    );
} 
```

当用户单击登录按钮时，在本地存储中设置一个虚拟令牌，并将用户重定向到配置文件页面。

但是在这种情况下我们不能使用重定向组件——我们需要以编程方式重定向用户。出于安全原因，用于身份验证的敏感令牌通常存储在 cookies 中。

React Router 有一个`withRouter` HOC，它在组件的 props 中注入`history`对象，以利用历史 API。它还将更新后的`match`和`location`道具传递给被包装的组件。

```
// components/Navbar.js

import { NavLink, withRouter } from "react-router-dom";

const Navbar = ({ history }) => { 
  const isAuth = !!localStorage.getItem("token");

  const loginUser = () => {
    localStorage.setItem("token", "some-login-token");
    history.push("/profile/Vijit");
  };

  const logoutUser = () => {
    localStorage.removeItem("token");
    history.push("/");
  };

  return ( 
   {/* ... */}
  );
};

export default withRouter(Navbar);
```

然后*瞧啊*！就是这样。你也征服了认证路线的土地。

查看这里的现场演示[和这个](https://react-router-demo.vijitail.dev/)[回购](https://github.com/vijitail/react-router-demo)中的完整代码供你参考。

## 结论

我希望到目前为止，您已经对客户端路由的一般工作原理以及如何使用 React 路由器库在 React 中实现路由有了一个大致的了解。

在本指南中，您了解了 React 路由器中的重要组件，如`Route`、`withRouter`、`Link`等，以及构建应用程序所需的一些高级概念，如认证路由。

请务必查看 React Router [文档](https://reacttraining.com/react-router/web/guides/quick-start)以获得每个组件的更详细概述。