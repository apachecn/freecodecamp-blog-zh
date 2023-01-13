# 假设 React 路由器是您的总机接线员

> 原文：<https://www.freecodecamp.org/news/imagine-react-router-as-your-switchboard-operator-f4f1ac22188c/>

杰伊·帕皮桑

# 假设 React 路由器是您的总机接线员

![eA-McoMPbr8LySuCNQ8j4Lj0I9lyKv-xFDxm](img/26e52ed49de72e43b5f9a3aecc38fe3d.png)

像老式的电话交换机一样，React 路由器是单页(SPA) React 应用程序的一个平滑的幕后操作者。用户点击一个链接，网址会改变，浏览次数会改变，他们知道自己已经进入了另一个页面——但事实并非如此。React Router 是一个关键工具，它可以让您的 SPA 具有多页面应用程序的感觉，同时保持 React 虚拟 DOM 的快速渲染。

让我们来了解一下使用 React 路由器设置简单导航的基本组件。对于 web 应用程序，您将运行`yarn add react-router-dom`。有许多方法来扩展您的路由器组件，但是为了便于解释，我们将使用下面的方法来说明所需的组件层次结构。我们将浏览下面每个加粗的组件— **提供商、浏览器路由器、交换机**和**路由**。

```
import React from 'react';import ReactDOM from 'react-dom';import { Provider } from 'react-redux';import { createStore, applyMiddleware } from 'redux';import { BrowserRouter, Route, Switch } from 'react-router-dom';import promise from 'redux-promise';import { PostsNew, PostsShow, PostsIndex } from './components'
```

```
const createStoreWithMiddleware = applyMiddleware(promise)(createStore);
```

```
ReactDOM.render( &lt;Provider store={createStoreWithMiddleware(reducers)}&gt;  <BrowserRouter>   <div>    &lt;Switch>     <Route path="/posts/new" component={ PostsNew } />     <Route path="/posts/:id" component={ PostsShow } />     <Route path="/" component={ PostsIndex } />    </Switch>   </div>  </BrowserRouter> </Provider>, document.getElementById('root'));
```

### 操作员…又名

那么，如果你的电话掉线了，你需要再试一次*(页面重新加载)*？想回去给你最后的朋友*(浏览器返回)*？没问题，接线员——又名`Provider`已经帮你搞定了。它使用 Redux 的`store`来保存你所有的浏览历史。可以把它想象成所有 React 路由器组件的易删除操作符(和**父组件**)。

```
<Provider store={createStoreWithMiddleware(reducers)}>
```

### 电话还是电报？…又名

你不能用电报打电话。类似地，您需要针对本地和 web 的正确路由器。对于大多数网络应用来说，导入可以在 HTML5 浏览器上运行的`BrowserRouter`就可以了。

### 谁啊。..…又名

你的接线员很懒……他们在电话簿上搜索手指，然后给第一个匹配的人打电话。假设您只想为每个 URL 呈现一个组件，`Switch`允许独占呈现(通过后台的正则表达式匹配)。注意排列顺序——把最具体的放在前面，一般的放在最后。

例如，不要把`path="/"`放在第一位，否则你将永远不会离开家…

```
&lt;Switch&gt;  <Route path="/posts/new" component={ PostsNew } /&gt;  <Route path="/posts/:id" component={ PostsShow } />  <Route path="/" component={ PostsIndex } /> </Switch>
```

### 电话号码…又名

总机接线员使用电话号码吗？无论如何……`Route`组件呈现您设置的匹配 URL 的特定组件。注意在`:id`下面——使用 React 路由器的[匹配对象](https://reacttraining.com/react-router/core/api/match)的能力，在`:`之后的任何东西都可以在你的渲染组件中访问，例如:`this.props.match.params.id`

```
&lt;Route path="/posts/:id" component={ PostsShow } />
```

### 重定向呼叫…又名<link>

这个电话越来越多余了，让我们赶紧打电话给别人或回家。有了 React Router 的`Link`组件(一个 HTML 锚标签的奇特包装器)，你可以像在组件中放置 home 按钮或锚标签一样放入一个链接。只需将您的目的地放入`to`字段，并像内嵌一样进行样式化，或者添加一个按钮包装器。

```
import { Link } from 'react-router-dom';
```

```
class YourThing extends Component {   render() {     return(       <div>        <Link to="/"> Home </Link>       </div>     )   }}
```

这是一些非常基本的，虽然还有更多可用的。查看优秀的[文档](https://reacttraining.com/react-router/)和教程，了解定制路线的无限方法。感谢阅读我的第二篇文章。如果有帮助的话，请给我一些评论、纠正或掌声。