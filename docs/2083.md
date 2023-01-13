# React 路由器备忘单–您需要知道的一切

> 原文：<https://www.freecodecamp.org/news/react-router-cheatsheet/>

如果您正在为 web 构建 React 应用程序，您将需要使用一个专用的路由器来显示页面并导航您的用户。

这就是为什么今天我们要讨论 React 应用中最流行、最强大的路由器——React 路由器。

我们将讨论 11 个你需要知道的基本特性，如果你今天在你的项目中使用 React Router，特别是使用包`react-router-dom`的 web。

## 想要你自己的副本吗？‬ 📄

****[点击此处下载 PDF 格式的备忘单](http://bit.ly/react-router-cheatsheet)**** (耗时 5 秒)。

它包括所有的基本信息，作为一个方便的 PDF 指南。

## 安装 React 路由器

使用 React 路由器的第一步是安装合适的软件包。

它们在技术上是三个不同的包:React Router、React Router DOM 和 React Router Native。

它们之间的主要区别在于它们的用法。React Router DOM 用于 web 应用程序，React Router Native 用于使用 React Native 制作的移动应用程序。

您需要做的第一件事是使用 npm(或 yarn)安装 React 路由器 DOM:

```
npm install react-router-dom
```

## 基本 React 路由器设置

一旦安装完毕，我们就可以引入使用 React 路由器所需的第一个组件，即 BrowserRouter。

请注意，除了 BrowserRouter 之外,`react-router-dom`还提供了多种类型的路由器，我们不会深入讨论。在导入 BrowserRoute 时，通常将其别名(重命名)为“Router”。

如果我们想在我们的整个应用程序中提供路由，它需要围绕我们的整个组件树。这就是为什么您通常会看到它包裹在主应用程序组件周围或内部:

```
import { BrowserRouter as Router } from 'react-router-dom';

export default function App() {
  return (
    <Router>
      {/* routes go here, as children */}
    </Router>
  );
}
```

这是 BrowserRouter 的主要功能:能够在我们的应用程序中声明单独的路由。

请注意，任何特定于路由器的数据都不能在路由器组件之外访问。例如，我们不能访问路由器外部的历史数据(即使用`useHistory`钩子),也不能在路由器组件外部创建路由。

## 路由组件

下一个组件是路由组件。

我们将路由器组件中的路由声明为子路由。我们可以声明尽可能多的路线，我们需要为每条路线提供至少两个道具，`path`和`component`(或`render`):

```
import { BrowserRouter as Router, Route } from 'react-router-dom';

export default function App() {
  return (
    <Router>
      <Route path="/about" component={About} />
    </Router>
  );
}

function About() {
  return <>about</>   
}
```

属性指定了给定路线位于我们应用程序的哪条路径上。

例如，对于 about 页面，我们可能希望可以通过路径“/about”访问该路由。

`render`或`component`道具用于显示我们路径的特定组件。

`component` props 只能接收对给定组件的引用，而`render`通常用于应用一些条件逻辑来呈现一个或另一个组件。对于渲染，可以使用对组件的引用，也可以使用函数:

```
import { BrowserRouter as Router, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Route path="/" render={() => <Home />} />
      <Route path="/about" component={About} />
    </Router>
  );
}

function Home() {
  return <>home</>;
}

function About() {
  return <>about</>;
}
```

值得注意的是，您可以完全删除`render`或`component`属性，并使用您希望与给定路由关联的组件作为 route 的子组件:

```
import { BrowserRouter as Router, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Route path="/about">
        <About />
      </Route>
    </Router>
  );
}
```

最后，如果想让一个组件(比如 navbar)在每个页面上都可见，就把它放在浏览器路由器中，但是在声明的 routes 之上(或之下):

```
import { BrowserRouter as Router, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </Router>
  );
}

function Navbar() {
  // visible on every page
  return <>navbar</>
}

function Home() {
  return <>home</>;
}

function About() {
  return <>about</>;
}
```

## 开关组件

当我们开始添加多条路由时，我们会注意到一些奇怪的事情。

假设我们有一个主页和关于页面的路由。尽管我们指定了两个不同的路径，'/'和'/about '，但当我访问 about 页面时，我们会看到 home 和 about 组件。

我们可以在归属路由上使用确切的属性来解决这个问题，以确保我们的路由器与路径“/”而不是“/about”完全匹配:

```
import { BrowserRouter as Router, Switch, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
}
```

当谈到我们的路由器应该显示的不同路由之间的切换时，如果您的路由器中有多条路由，实际上您应该使用一个专用组件，那就是交换机组件。

交换机组件应该包含在路由器中，我们可以在其中放置所有路由:

```
import { BrowserRouter as Router, Switch, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
}
```

交换机组件查看其所有子路由，并显示其路径与当前 URL 匹配的第一个路由。

这个组件是我们希望在大多数应用程序的大多数情况下使用的，因为我们的应用程序中有多个路线和多个板块页面，但我们只想一次显示一个页面。

如果出于某种原因，您确实希望同时显示多个页面，那么您可以考虑不使用 switch 组件。

## 404 路线

如果我们试图去一个在我们的应用程序中不存在的路径，我们会看到什么？

如果我们没有相应的路线，我们什么也看不到。我们如何制定一条包罗万象的路线？

如果用户试图访问我们没有定义路由的页面，我们可以创建一个路由，然后将路径设置为星号*:

```
import { BrowserRouter as Router, Switch, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="*" component={NotFound} />
      </Switch>
    </Router>
  );
}

function NotFound() {
  return <>You have landed on a page that doesn't exist</>;
}
```

这将匹配任何试图访问一个不存在的页面，我们可以将它连接到一个未找到的组件，告诉我们的用户他们已经“登陆了一个不存在的页面。”

## 链接组件

假设在我们的 NavBar 中，我们实际上想要创建一些链接，这样我们就可以更容易地在应用程序中移动，而不必在浏览器中手动更改 URL。

我们可以使用 React 路由器 DOM 中的另一个特殊组件 Link 来实现。它接受`to`属性，该属性指定了我们希望链接将用户导航到的位置。在我们的例子中，我们可能有一个 home 和 about 链接:

```
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
}

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
  )
}
```

link 组件允许我们提供一些内联样式，就像任何标准的 React 组件一样。它还为我们提供了一个有用的`component`道具，因此我们可以将我们的链接设置为我们自己的定制组件，以获得更简单的样式。

## NavLink 组件

此外，React Router DOM 为我们提供了一个 NavLink 组件，如果我们想要应用一些特殊的样式，这个组件会很有帮助。

如果我们在链接指向的当前路径上，这允许我们创建一些活动的链接样式来告诉我们的用户，通过查看我们的链接，他们在哪个页面上。

例如，如果我们的用户在主页上，当他们在主页上时，我们可以通过使用`activeStyle`道具将我们的链接加粗为红色来告诉他们:

```
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink
} from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
}

function Navbar() {
  return (
    <nav>
      <NavLink
        activeStyle={{
          fontWeight: "bold",
          color: "red"
        }}
        to="/"
      >
        Home
      </NavLink>
      <NavLink activeClassName="active" to="/about">
        About
      </NavLink>
    </nav>
  );
} 
```

如果您不想包含内联样式或者想要更多可重用的样式来执行与`activeStyle`相同的功能，也可以设置一个`activeClassName`属性。

## 重定向组件

React Router DOM 提供给我们的另一个非常有用的组件是重定向组件。

让一个组件在显示时执行重定向用户的功能可能看起来很奇怪，但这是非常实用的。

每当我们使用类似私有路由的东西，并且我们有一个用户没有被认证的情况，我们想要重定向他们回到登录页面。

下面是一个私有路由组件的实现示例，该组件确保在向用户显示使用该组件声明的特定路由之前，对用户进行身份验证。

否则，如果他们没有通过身份验证，一旦显示重定向组件，他们将被重定向到公共路由(可能是登录的路由):

```
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Redirect
} from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={Home} />
        <PrivateRoute path="/hidden" component={Hidden} />
      </Switch>
    </Router>
  );
}

function PrivateRoute({ component: Component, ...rest }) {
  // useAuth is some custom hook to get the current user's auth state
  const isAuth = useAuth();

  return (
    <Route
      {...rest}
      render={(props) =>
        isAuth ? <Component {...props} /> : <Redirect to="/" />
      }
    />
  );
}

function Home() {
  return <>home</>;
}

function Hidden() {
  return <>hidden</>;
}
```

重定向组件使用起来非常简单，具有很强的声明性，让我们看到了 React 路由器 DOM 基于组件的巨大优势，就像 React 中的所有东西一样。

## 使用历史挂钩

在所有这些强大的组件之上，React Router DOM 给了我们一些非常有用的钩子。

它们主要通过提供我们可以在组件中使用的附加信息来提供帮助。它们可以被称为普通的 React 钩子，我们可以按照自己的意愿使用它们的值。

也许最强大的钩子是`useHistory`钩子。我们可以在路由器组件中声明的任何组件的顶部调用它，并获取`history`数据，其中包括与我们的组件相关联的位置等信息。

这告诉我们用户当前在哪里，比如他们所在的路径名，以及任何可能附加到我们的 URL 的查询参数。从`history.location`可以访问所有位置数据:

```
import { useHistory } from "react-router-dom";

function About() {
  const history = useHistory();

  console.log(history.location.pathname); // '/about'

  return (
    <>
     <h1>The about page is on: {history.location.pathname}</h1>
    </>
  );
}
```

此外，history 对象直接包含有用的方法，允许我们以编程方式将用户定向到应用程序中的不同页面。

这非常有帮助，例如，在登录后重定向我们的用户，或者在我们需要将用户从一个页面带到另一个页面的任何情况下。

我们可以使用`history.push`将用户从一个页面推到另一个页面。当我们使用推送方法时，我们只需要提供我们希望用户使用这种方法的路径。它为我们的历史增添了新的一页:

```
import { useHistory } from "react-router-dom";

function About() {
  const history = useHistory();

  console.log(history.location.pathname); // '/about'

  return (
    <>
     <h1>The about page is on: {history.location.pathname}</h1>
     <button onClick={() => history.push('/')}>Go to home page</button>
    </>
  );
}
```

我们还可以用`history.replace`重定向我们的用户，它也接受一个路径值，但是在导航完成后，清除历史中的所有内容。这对于不再需要回溯历史的情况很有帮助，例如在用户已经注销之后。

## 使用定位钩

`useLocation`钩子包含所有与`useHistory`钩子相同的信息。

值得注意的是，如果您既需要位置数据，又需要使用历史来以编程方式导航您的用户，请确保使用历史。但是，如果您只想要位置数据，那么您需要做的就是调用 useLocation 或者获取与`history. location`上提供的数据相同的对象上的所有位置数据:

```
import { useLocation } from "react-router-dom";

function About() {
  const location = useLocation();

  console.log(location.pathname); // '/about'

  return (
    <>
     <h1>The about page is on: {location.pathname}</h1>
    </>
  );
}
```

## useParams Hook +动态路由

谈到路由时，我们没有提到的一点是，我们可以自然地创建动态路由。这意味着路径不是固定不变的，可以是任意数量的字符。

比如说，在我们有一篇独特的博客文章的情况下，动态路线是很有帮助的。假设我们的博客文章片段可能完全不同，我们如何确保显示适当的数据和适当的组件？

要在给定的路由上声明路由参数，必须以冒号`:`为前缀。如果我想为一个博客文章组件创建一个动态路由“/blog/:postSlug”，它可能如下所示:

```
import React from "react";
import { BrowserRouter as Router, Switch, Route } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/blog/:postSlug" component={BlogPost} />
      </Switch>
    </Router>
  );
}

function Home() {
  return <>home</>;
}

function BlogPost() {
  return <>blog post</>;
} 
```

我们正在匹配合适的成分或者是子弹。但是在我们 BlogPost 组件中，我们如何接收 post slug 数据呢？

我们可以使用`useParams`钩子访问一个已声明的路由及其相关组件的任何路由参数。

useParams 将返回一个对象，该对象将包含与我们的路由参数匹配的属性(在本例中为`postSlug`)。我们可以使用对象析构来立即访问并声明一个名为`postSlug`的变量:

```
import React from "react";
import { BrowserRouter as Router, Switch, Route, useParams } from "react-router-dom";

export default function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/blog/:postSlug" component={BlogPost} />
      </Switch>
    </Router>
  );
}

function Home() {
  return <>home</>;
}

function BlogPost() {
  const [post, setPost] = React.useState(null);
  const { postSlug } = useParams();

  React.useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/posts/${postSlug}`)
      .then((res) => res.json())
      .then((data) => setPost(data));
  }, [postSlug]);

  if (!post) return null;

  return (
    <>
      <h1>{post.title}</h1>
      <p>{post.description}</p>
    </>
  );
}
```

如果我们转到路径'/blog/my-blog-post '，我可以访问`postSlug`变量上的字符串' my-blog-post ',并在 useEffect 中获取帖子的相关数据。

## 断背钩

如果我们想知道给定的组件是否在某个页面上，我们可以使用`useRouteMatch`钩子。

例如，在我们的博客文章中，要查看我们所在的页面是否与路线“/blog/:postsluck”匹配，我们可以获得一个布尔值，该值将告诉我们我们所在的路线是否与我们指定的模式匹配:

```
import { useRouteMatch } from "react-router-dom";

function BlogPost() {
  const isBlogPostRoute = useRouteMatch("/blog/:postSlug");

  // display, hide content, or do something else
}
```

这在我们想要根据我们是否在某条路线上来显示一些特定的东西的情况下很有帮助。

## 想保留此指南以供将来参考吗？‬

****[点击此处下载有用的 PDF 格式备忘单](http://bit.ly/react-router-cheatsheet)。****

当您获得可下载版本时，您将获得 3 个快速胜利:

*   您将获得大量可复制的代码片段，以便在您自己的项目中重用。
*   这是一个很好的参考指南，可以加强你作为 React 开发人员的技能，也可以用于工作面试。
*   你可以在任何你喜欢的地方拿走、使用、打印、阅读和重读这个指南。