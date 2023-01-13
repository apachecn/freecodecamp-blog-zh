# React 路由器版本 6 教程–如何设置路由器并路由到其他组件

> 原文：<https://www.freecodecamp.org/news/how-to-use-react-router-version-6/>

在本教程中，我们将讨论什么是 React 路由器以及如何使用它。然后我们将讨论它的特性，以及如何在 React 应用程序中使用它们来导航和呈现多个组件。

### **先决条件**

*   React 应用程序
*   很好地理解 React 中有哪些组件。
*   Node.js 已安装。

## 作为单页应用程序(SPA)进行响应

在开始路由之前，您需要了解页面在 React 应用程序中是如何呈现的。这一部分是针对初学者的——如果你已经理解 SPA 是什么以及它与反应的关系，你可以跳过它。

在非单页应用程序中，当您单击浏览器中的链接时，在 HTML 页面呈现之前，会向服务器发送一个请求。

在 React 中，页面内容是从我们的组件中创建的。因此，React Router 所做的是拦截发送到服务器的请求，然后从我们创建的组件中动态注入内容。

这是 SPAs 背后的一般思想，它允许在不刷新页面的情况下更快地呈现内容。

当您创建一个新项目时，您总是会在 public 文件夹中看到一个`index.html`文件。您在充当根组件的`App`组件中编写的所有代码都会呈现到这个 HTML 文件中。这意味着只有一个 HTML 文件，您的代码将被呈现到其中。

当你有一个不同的组件，你更喜欢呈现为一个不同的页面，会发生什么？你创建一个新的 HTML 文件吗？答案是否定的。React Router——顾名思义——帮助您路由到/导航到并在`index.html`文件中呈现您的新组件。

因此，作为一个单页面应用程序，当您使用 React Router 导航到一个新组件时，`index.html`将被该组件的逻辑重写。

## 如何安装 React 路由器

要安装 React Router，您所要做的就是在项目终端中运行`npm install react-router-dom@6`，然后等待安装完成。

如果你正在使用 yarn，那么使用这个命令:`yarn add react-router-dom@6`。

## 如何设置 React 路由器

安装完成后要做的第一件事就是让 React Router 在你的应用中随处可用。

为此，打开`src`文件夹中的`index.js`文件，从`react-router-dom`导入`BrowserRouter`，然后将根组件(即`App`组件)包装在其中。

这是`index.js`最初的样子:

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
); 
```

使用 React 路由器进行更改后，您应该会看到以下内容:

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```

我们所做的就是用从`react-router-dom`进口的`BrowserRouter`替换`React.StrictMode`。现在，您可以从应用的任何部分访问路由器功能。

## 如何路由到其他组件

我们终于完成了设置，所以现在我们将看看路由和渲染不同的组件。

### 步骤 1 -创建多个组件

我们将创建如下的`Home`、`About`和`Contact`组件，如下所示:

```
function Home() {
  return (
    <div>
      <h1>This is the home page</h1>
    </div>
  );
}

export default Home; 
```

Home component

```
import React from 'react'

function About() {
    return (
        <div>
            <h1>This is the about page</h1>
        </div>
    )
}

export default About
```

About component

```
import React from 'react'

function Contact() {
    return (
        <div>
            <h1>This is the contact page</h1>
        </div>
    )
}

export default Contact
```

Contact component

### 步骤 2 -定义路线

因为`App`组件作为根组件，我们的 React 代码最初就是从这里开始呈现的，所以我们将在其中创建所有的路线。

如果这没有多大意义，不要担心——看了下面的例子后，你会更好地理解。

```
import { Routes, Route } from "react-router-dom"
import Home from "./Home"
import About from "./About"
import Contact from "./Contact"

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path="/" element={ <Home/> } />
        <Route path="about" element={ <About/> } />
        <Route path="contact" element={ <Contact/> } />
      </Routes>
    </div>
  )
}

export default App 
```

App component

我们首先导入了将要使用的特性—`Routes`和`Route`。在那之后，我们导入了我们需要附加一个路由的所有组件。现在我们来分解一下流程。

`Routes`充当将在我们的应用程序中创建的所有单独路线的容器/父代。

`Route`用于创建单一路线。它包含两个属性:

*   `path`，指定所需组件的 URL 路径。您可以随意命名该路径名。上面，你会注意到第一个路径名是一个反斜杠(/)。每当应用程序第一次加载时，路径名是反斜杠的任何组件都将首先被渲染。这意味着`Home`组件将是第一个被渲染的组件。
*   `element`，指定路线应该呈现的组件。

我们现在所做的就是定义我们的路线和它们的路径，并将它们附加到各自的组件上。

如果你来自版本 5，那么你会注意到我们没有使用`exact`和`switch`，这很棒。

### 步骤 3 -使用`Link`导航至路线

如果到目前为止您一直没有任何错误地编写代码，那么您的浏览器应该会呈现出`Home`组件。

我们现在将使用不同的 React 路由器特性，根据我们在`App`组件中创建的路由和路径名导航到其他页面。那就是:

```
import { Link } from "react-router-dom";

function Home() {
  return (
    <div>
      <h1>This is the home page</h1>
      <Link to="about">Click to view our about page</Link>
      <Link to="contact">Click to view our contact page</Link>
    </div>
  );
}

export default Home; 
```

Home component

`Link`组件类似于 HTML 中的锚元素(< a >)。它的`to`属性指定链接将您带到哪条路径。

回想一下，我们创建了在`App`组件中列出的路径名，因此当您单击链接时，它将浏览您的路线并呈现具有相应路径名的组件。

在使用之前，一定要记住从`react-router-dom`导入`Link`。

## 结论

至此，我们已经了解了如何安装、设置和使用 React Router 的基本功能来导航到应用程序中的不同页面。这基本上涵盖了入门的基本知识，但是还有很多更酷的特性。

比如可以用`useNavigate`将用户推送到各种页面，可以用`useLocation`获取当前 URL。好吧，我们不会在文章的最后开始另一个教程。

你可以在 [React 路由器文档](https://reactrouter.com/docs/en/v6)中查看更多特性。

可以在 Twitter [@ihechikara2](https://twitter.com/Ihechikara2) 找到我。订阅我的[简讯](https://www.getrevue.co/profile/The-Congregation-of-Programmers)获取免费学习资源。

编码快乐！