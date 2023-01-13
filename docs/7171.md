# 如何使用 React 上下文保护您的路由

> 原文：<https://www.freecodecamp.org/news/how-to-protect-your-routes-with-react-context-717670c4713a/>

保罗·克利斯朵夫

# 如何使用 React 上下文保护您的路由

![1*wmwNRYeBumNlEKriJxLHjg](img/4a6b8344284f936be933450bb973e3c5.png)

Photo by [Antonina Bukowska](https://unsplash.com/photos/PpwqEpJ9UaQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/lock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 [React 16.3](https://reactjs.org/blog/2018/03/29/react-v-16-3.html) 的变化中，有一个新的稳定版本的**上下文 API** 。我们将通过构建一个**受保护的路由组件**来看看它是如何工作的。

#### 什么是语境？

上下文是关于封装状态的。它允许我们将数据从父提供者组件传递到树中任何订阅的组件。如果没有状态管理，我们经常不得不在整个过程中对每个组件进行“演练”。

#### Redux 不就是干这个的吗？

**是**，上下文的操作类似于组件如何连接到 Redux 的全局状态。然而，对于不需要 Redux 的复杂开销的中小型应用程序来说，像 Context 这样的原生元素通常是更好的解决方案。

#### 基本概念

上下文有三个要素:

*   `createContext` —调用这个函数返回一对组件，`Provider`和`Consumer`。
*   `Provider` —允许一个或多个`Consumers`订阅变更的组件。
*   `Consumer`—订阅给提供商的组件

### 让我们开始建造吧

我们将构建一个有**两条**路线的应用程序。一个是全球访问的登陆页面**。另一个是**，一个登录用户访问受限的仪表板页面**。你可以在这里找到[的最终版本](https://codesandbox.io/s/p71pr7jn50)。**

> **尝试一下:注销后前往/dashboard。登录并在路线之间自由导航。从仪表板上，注销，它会把你踢到登录页面。**

#### **上下文标题**

**为了演示 Context 的基本功能，让我们从构建一个允许我们登录和注销的 header 组件开始。首先，在一个新文件中创建我们的上下文。**

```
`/* AuthContext.js */`
```

```
`import React from 'react';`
```

```
`const AuthContext = React.createContext();`
```

**导出一个组件`AuthProvider`来定义我们的状态(用户是否登录)并将其状态传递给`Provider`上的`value` prop。我们将简单地用一个有意义的名字公开`AuthConsumer`。**

```
`/* AuthContext.js */`
```

```
`...`
```

```
`class AuthProvider extends React.Component {  state = { isAuth: false }`
```

```
 `render() {    return (      <AuthContext.Provider        value={{ isAuth: this.state.isAuth }}      >        {this.props.children}      </AuthContext.Provider>    )  }}`
```

```
`const AuthConsumer = AuthContext.Consumer`
```

```
`export { AuthProvider, AuthConsumer }`
```

**在 index.js 中，用`AuthProvider`包装我们的 app。**

```
`/* index.js */import React from 'react';import { render } from 'react-dom';import { AuthProvider } from './AuthContext';import Header from './Header';`
```

```
`const App = () => (  <;div>    <AuthProvider>      <Header />    </AuthProvider>  </div>);`
```

```
`render(<App />, document.getElementById('root'));`
```

**现在创建我们的`Header`并导入我们的`AuthConsumer`(为了清晰起见，我将不再进行造型)。**

```
`/* Header.js */import React from 'react'import { AuthConsumer } from './AuthContext'import { Link } from 'react-router-dom'`
```

```
`export default () => (  <header>    <AuthConsumer>    </AuthConsumer>  </header>)`
```

**上下文消费者必须有一个**函数作为他们的直接孩子。**这将从我们的`Provider`传递值。**

```
`/* Header.js */...export default () => (  <header>    <AuthConsumer>`
```

```
 `{({ isAuth }) => (        <div>          <h3>            <Link to="/">              HOME            &lt;/Link>          </h3>`
```

```
 `{isAuth ? (            <ul>              <Link to="/dashboard">                Dashboard              </Link>              <button>                logout              </button>            </ul>          ) : (            <button>login</button>          )}        </div>      )}`
```

```
 `</AuthConsumer>  </header>)`
```

**因为`isAuth`被设置为 false，所以只有登录按钮是可见的。尝试将值更改为`true`(它将切换到注销按钮)。**

**好了，让我们试着用代码切换`isAuth`。我们将从我们的`Provider`中传递一个登录和注销功能。**

```
`/* AuthContext.js */...class AuthProvider extends React.Component {  state = { isAuth: false }`
```

```
 `constructor() {    super()    this.login = this.login.bind(this)    this.logout = this.logout.bind(this)  }`
```

```
 `login() {    // setting timeout to mimic an async login    setTimeout(() => this.setState({ isAuth: true }), 1000)  }`
```

```
 `logout() {    this.setState({ isAuth: false })  }`
```

```
 `render() {    return (      <AuthContext.Provider        value={{          isAuth: this.state.isAuth,          login: this.login,          logout: this.logout        }}      >        {this.props.children}      </AuthContext.Provider>    )  }}`
```

**这些函数将允许我们在`Header`中切换我们的授权状态。**

```
`/* Header.js */...export default () => (  <header>    <AuthConsumer>      {({ isAuth, login, logout }) => (        <div>          <h3>            <Link to="/">              HOME            </Link>          </h3>`
```

```
 `{isAuth ? (            <ul>              <Link to="/dashboard">                Dashboard              </Link>              <button onClick={logout}>                logout              </button>            </ul>          ) : (            <button onClick={login}>login</button>          )}        </div>      )}    </AuthConsumer>  </header>)`
```

### **带上下文的受保护路由**

**现在我们已经介绍了基础知识，让我们扩展我们所学的知识，创建一个受保护的路由组件。**

**首先制作`Landing`和`Dashboard`页面组件。我们的仪表板只有在用户登录后才可见。两个页面都很简单，如下所示:**

```
`/* Dashboard.js */import React from 'react'`
```

```
`const Dashboard = () => <h2>User Dashboard</h2>`
```

```
`export default Dashboard`
```

**现在让我们翻到这几页。**

```
`/* index.js */import React from 'react';import { render } from 'react-dom';import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';import { AuthProvider } from './AuthContext';import Landing from './Landing';import Dashboard from './Dashboard';import Header from './Header';`
```

```
`const App = () => (  <;div>    <Router>      <AuthProvider>;        <Header />        <Switch>          <Route path="/dashboard" component={Dashboard} />          <Route path="/" component={Landing} />        &lt;/Switch>      </AuthProvider>    </Router>  </div>);`
```

```
`render(<App />, document.getElementById('root'));`
```

**在当前状态下，您可以导航到`/`和`/dashboard`。我们将创建一个名为`ProtectedRoute`的特殊路由组件来检查用户是否登录。设置类似于我们的`Header`组件。**

```
`/* ProtectedRoute.js */import React from 'react';import { Route, Redirect } from 'react-router-dom';import { AuthConsumer } from './AuthContext';`
```

```
`const ProtectedRoute = () => (  <AuthConsumer>    {({ isAuth }) => (`
```

```
 `)}  </AuthConsumer&gt;);`
```

```
`export default ProtectedRoute;`
```

**私有路由的功能就像常规的`react-router`路由一样，所以我们将公开组件和传递给它的任何其他属性。**

```
`const ProtectedRoute = ({ component: Component, ...rest }) => (`
```

**现在有趣的部分来了:我们将使用`isAuth`变量来确定它是否应该重定向或呈现受保护路由的组件。**

```
`const ProtectedRoute = ({ component: Component, ...rest }) => (  <AuthConsumer>    {({ isAuth }) => (      <Route        render={          props =>            isAuth             ? <Component {...props} />             : <Redirect to="/" />        }        {...rest}      />    )}  </AuthConsumer>)`
```

**在我们的`index`文件中，让我们导入`ProtectedRoute`并在我们的仪表板路线上使用它。**

```
`/* index.js */...`
```

```
 `<ProtectedRoute path="/dashboard" component={Dashboard} />`
```

**太棒了，现在我们有受保护的路线了！尝试将浏览器指向`/dashboard`，看着它把你踢回登陆页面。**

**同样，这是[工作演示](https://codesandbox.io/s/p71pr7jn50)的链接。从 [React 的官方文档](https://reactjs.org/docs/context.html)中阅读更多关于上下文的信息。**