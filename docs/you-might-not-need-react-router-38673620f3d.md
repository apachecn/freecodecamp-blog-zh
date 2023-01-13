# 你可能不需要反应路由器

> 原文：<https://www.freecodecamp.org/news/you-might-not-need-react-router-38673620f3d/>

康斯坦丁·塔库斯

# 你可能不需要反应路由器

![7eXQpRAaM8FMVy4PMHQYfQl1UJaJ4rFILVgO](img/c577a1bd4e99348a064cd1e4e4ec2dfe.png)

如果您曾经使用过脸书的 React.js 库，您可能会注意到 React 社区中的一些误解。其中之一就是肯定 React 只是 MVC 架构的 V，需要和一堆其他库混合才能作为开发 web 应用的框架。

实际上，你很少看到 React 开发人员使用来自 MVC 的控制器和模型。基于组件的 UI 架构在前端社区中逐渐占据主导地位，现在越来越少的人使用 MVC 模式。

另一个误解是 React 路由器库(RR)是脸书的官方路由解决方案。事实上，脸书的大多数项目甚至都不使用它。

说到路由，大量的 web 应用程序项目和用例可以用一个小小的定制路由器做得很好。在您将这个概念归类为完全的异端邪说之前，请让我向您展示如何用不到 50 行代码实现一个功能全面的路由解决方案。

#### 航行

首先，不需要像在 RR 中那样在同一个组件中组合路由和客户端导航。这样，您的路由器就可以真正实现通用——在客户端和服务器端环境中，使用相同的 API，以完全相同的方式工作。有一个很棒的 npm 模块叫做 **history** ，可以处理导航部分(仅供参考，它是 HTML5 History API 的一种包装器，也由 RR 内部使用)。您只需在项目中创建 history.js 文件，在该文件中初始化该组件(类),并在您的应用程序中将其作为单例使用:

```
import createHistory from 'history/lib/createBrowserHistory';import useQueries from 'history/lib/useQueries';
```

```
export default useQueries(createHistory)();
```

从现在开始，只要需要将用户重定向到一个新位置(URL)而不刷新整个页面，就可以引用这个文件并调用 history.push('/new-page ')。在主应用程序文件(引导代码)中，您可以订阅所有 URL 更改，如下所示:

```
import history from './history';
```

```
function render(location) { /* Render React app, read on */ }
```

```
render(history.getCurrentLocation()); // render the current URLhistory.listen(render);               // render subsequent URLs
```

具有客户端工作链接的 React 组件可能如下所示:

```
import React from 'react';import history from '../history';
```

```
class App extends React.Component {
```

```
 transition = event => {    event.preventDefault();    history.push({      pathname: event.currentTarget.pathname,      search: event.currentTarget.search    });  };
```

```
 render() {    return (      <ul>        <li><a href="/" onClick={this.transition}>Home</a></li>        <li><a href="/one" onClick={this.transition}>One</a></li>        <li><a href="/two" onClick={this.transition}>Two</a></li>      </ul>    );  }
```

```
}
```

但是，在实践中，您可能希望将这种“转换”功能提取到一个独立的 React 组件中。参见 [React 静态样板](https://github.com/kriasoft/react-static-boilerplate) (RSB)中的[链接](https://github.com/kriasoft/react-static-boilerplate/blob/master/components/Link/Link.js)组件。所以你可以像这样写客户端链接:<链接到="/some-page" >点击</链接>。

需要在用户离开页面前显示确认信息吗？只需注册 history.listenBefore(..)事件处理程序，如历史模块的[文档](https://github.com/ReactJSTraining/history/blob/master/docs/ConfirmingNavigation.md)中所述。同样的方法可以用来制作页面之间的动画过渡([演示](https://jsfiddle.net/frenzzy/4ota5fag/2/))。

#### 按指定路线发送

您可以通过普通的 JavaScript 对象来描述路由列表和每条路由，这里不需要使用 JSX。例如:

```
const routes = [  { path: '/', action: () => <HomePage /> },  { path: '/tasks', action: () => <TaskList /> },  { path: '/tasks/:id', action: () => <TaskDetails /> }];
```

顺便说一句，如果有人知道为什么这么多人喜欢用 JSX 做与 UI 渲染无关的事情，请留下评论。

您可以使用 ES2015+ async/await 语法编写路由处理程序，不需要像 RR 中那样使用回调。例如:

```
{  path: '/tasks/:id(\\d+)',  async action({ params }) {    const resp = await fetch(`/api/tasks/${params.id}`);    const data = await resp.json();    return data && <TaskDetails {...data} />;  }}
```

在我所熟悉的大多数用例中，没有必要像在 RR 中那样使用嵌套路由。使用嵌套路由会使事情变得更加复杂，并导致过于复杂的 hariy 路由实现更难维护。据我所知，即使在脸书，考虑到他们应用的规模，他们也不会在客户端使用嵌套路线(至少不是在他们所有的项目中)。

您可以嵌套 React 组件，而不是嵌套路由，例如:

```
import React from 'react';import Layout from '../components/Layout';
```

```
class AboutPage extends React.Component {  render() {    return (      <Layout title="About Us" breadcrumbs="Home > About">        <h1>Welcome!</h1>        <p>Here your can learn more about our product.</p>      </Layout>    );  }}
```

```
export default AboutPage;
```

这种方法在实现上比嵌套路由简单得多，同时也更加灵活、直观，并且可以使用更多的用例(注意如何将面包屑组件传递到布局中)。

路由器本身可以写成一对两个函数——match uri()，一个内部(私有)函数，帮助比较参数化的路径字符串和实际的 URLresolve()函数遍历路由列表，找到与给定位置匹配的路由，执行路由处理函数并将结果返回给调用者。下面是它可能的样子(router.js):

```
import toRegex from 'path-to-regexp';
```

```
function matchURI(path, uri) {  const keys = [];  const pattern = toRegex(path, keys); // TODO: Use caching  const match = pattern.exec(uri);  if (!match) return null;  const params = Object.create(null);  for (let i = 1; i < match.length; i++) {    params[keys[i - 1].name] =      match[i] !== undefined ? match[i] : undefined;  }  return params;}
```

```
async function resolve(routes, context) {  for (const route of routes) {    const uri = context.error ? '/error' : context.pathname;    const params = matchURI(route.path, uri);    if (!params) continue;    const result = await route.action({ ...context, params });    if (result) return result;  }  const error = new Error('Not found');  error.status = 404;  throw error;}
```

```
export default { resolve };
```

查看 [path-to-regexp](https://github.com/pillarjs/path-to-regexp) 库的文档。这个图书馆太棒了！例如，您可以使用同一个库将参数化的路径字符串转换为 URL:

```
const toUrlPath = pathToRegexp.compile('/tasks/:id(\\d+)')toUrlPath({ id: 123 }) //=> "/user/123"toUrlPath({ id: 'abc' }) /=> error, doesn't match the \d+ constraint
```

现在，您可以更新主应用程序文件(入口点)来使用该路由器:

```
import ReactDOM from 'react-dom';import history from './history';import router from './router';import routes from './routes';
```

```
const container = document.getElementById('root');
```

```
function renderComponent(component) {  ReactDOM.render(component, container);}
```

```
function render(location) {  router.resolve(routes, location)    .then(renderComponent)    .catch(error => router.resolve(routes, { ...location, error })    .then(renderComponent));}
```

```
render(history.getCurrentLocation()); // render the current URLhistory.listen(render);               // render subsequent URLs
```

就是这样！您可能还想看看我的 React 样板项目，这些项目都采用了这种路由方法:

[通用路由器](https://github.com/kriasoft/universal-router) —简单的中间件风格路由解决方案
[React 初学者工具包](https://github.com/kriasoft/react-starter-kit) —同构 web app 样板(Node.js，GraphQL，React)
[React 静态样板](https://github.com/kriasoft/react-static-boilerplate) —无服务器 web app (React，Redux，Firebase)
[ASP.NET 核心初学者工具包](https://github.com/kriasoft/aspnet-starter-kit) —单页 app(ASP.NET 核心，C#，React)

这些样板文件非常受欢迎，并在全球许多真实项目中成功使用。绝对值得一探:)

**P.S.** :声明式路线的粉丝？找到这种路由方法的声明性风格&g[t；h](https://github.com/kriasoft/react-static-boilerplate/blob/master/docs/routing-and-navigation.md) ere <。在& g t 上查看这篇文章[的评论；Reddit <。](https://www.reddit.com/r/reactjs/comments/4s8ulw/you_might_not_need_react_router/)

下一篇:你可能不需要 React 路由器——第二部分(即将推出)