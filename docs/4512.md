# 如何升级到 React Router 4

> 原文：<https://www.freecodecamp.org/news/a-guide-to-upgrading-to-react-router-4/>

![image-82](img/fa88bded3e737e405fdca0e5b70e38e9.png)

在我开始在当前职位工作后不久，团队意识到我们有必要升级到 React 16，这样我们就可以使用我们热衷于采用的新 UI 库。

为了弄清楚这次升级需要多长时间，我们查看了我们当前所有的包，看看它们是否与 React 16 兼容，并看看我们是否仍在使用不支持或不推荐的包。

我们的代码库的开端是由开发人员构建的，他们使用他们想要的任何开源或第三方库，而没有真正审查它们。因此，我们发现很多包都被弃用了，需要尽快替换。

对我们来说，最大的惊喜之一是`react-router-redux`的贬值。我们将`react-router-redux`与 react-router v3 结合使用。这让我们开始批判性地思考为什么我们要在路由器中使用`redux`。

一旦我们开始研究 react 路由器 v4，我们意识到新的特性将几乎消除我们使用额外的库来连接我们的路由器和`redux`的任何理由。因此，我们只能从 react 路由器 3 升级到 4，并从我们的应用程序中删除`react-router-redux`。

因此，我的任务是将我们的路由器升级到 v4，之前我只在 React 工作了大约 2 个月。这是因为从 React Router 3 升级到 React Router 4 听起来应该是一件小事。但是，我很快发现，这比我预想的要复杂一些。

通过查看[文档](https://reacttraining.com/react-router/web/guides/quick-start)、 [GitHub repo](https://github.com/ReactTraining/react-router/) 和许多许多堆栈溢出答案，我终于拼凑出了升级的步骤，并希望分享我的发现——特别是解释如何以及为什么做出某些改变。

需要注意的最大变化来自 React 路由器的创造者，从 React 路由器 3 到 React 路由器 4 的升级不仅仅是更新一些库和功能，它允许您从根本上改变路由器的工作方式。React 路由器的创建者希望回到简单的路由器，允许开发者按照他们喜欢的方式定制它。

我将本指南分为 5 个不同的部分:

1.  包裹
2.  历史
3.  途径
4.  小道具
5.  冗余集成

* * *

# 包裹

React Router v4 包结构发生了变化，因此不再需要安装 react-Router——您必须安装`react-router-dom`(并卸载`react-router`)，但您不会丢失任何东西，因为它会重新导出`react-router`的所有导出。这意味着您还必须将任何`react-router`导入语句更新到`react-router-dom`。

* * *

# 历史

历史是路线的重要组成部分，它让我们记住我们从哪里来，现在在哪里。react-router 的历史有多种形式，可能需要一段时间来解释。因此，为了保持这篇文章的主题，我将简单地建议你通读一下这篇文章，它解释了与 react router 4 相关的历史。这篇文章应该涵盖你使用历史的大多数情况。

* * *

# 途径

React Router v3 允许我们将所有应用程序路由放在一个文件中，我们称之为 router.js。然而，React Router v4 允许您将路由放在它们正在呈现的组件中。这里的想法是*动态路由*应用程序——换句话说，路由发生在应用程序渲染的时候。

然而，如果你有一个相当大的遗留代码库，你可能不会做那么大的改变。幸运的是，React Router v4 仍然允许您将所有路由放在一个中心文件中，这就是我将如何创建我们所有的示例。但是，有一些较旧的组件和功能需要更换。

## 索引路径

以前，`IndexRoute`被用作某个父路由的默认 UI 的路由。但是，在 v4 中，`IndexRoute`不再使用，因为该功能现在在 Route 中可用。

为了提供默认 UI，具有相同路径的多条路线将让所有关联的组件呈现:

```
import { BrowserRouter as Router, Route } from 'react-router-dom';

<Router>
    // example of our route components
    <Route path="/" component={Home} />
    <Route path="/" component={About} />
    <Route path="/" component={Contact} />
</Router>
```

因此，所有的组件——`Home`、`About`和`Contact`——都将被渲染。因此，您也不能再嵌套路由。

此外，为了在不使用`IndexRoute`的情况下实现更好的匹配，您可以使用 exact 关键字。

```
import { BrowserRouter as Router, Route } from 'react-router-dom';

<Router>
    // example of our route components
    <Route exact path="/" component={Home} />
    <Route path="/about" component={About} />
</Router>
```

## 独占路由

添加精确的关键字后，当路由器看到路径`“/about”`时，`“something.com/about”`将被路由到。但是现在如果你有另一条路怎么办，`“/about/team”`？如前所述，路由器将呈现任何匹配的内容。因此，与`“/about”`和`“/about/team”`相关联的组件将会呈现。如果这是你的本意，那太好了！但是，如果这不是您想要的，您可能需要在这组路由周围放置一个开关。这将允许呈现匹配 URL 的第一个路径。

```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

<Router>
    <Switch>
       <Route exact path ="/" component={Home} />
       <Route path="/about/team" component={Team} />
       <Route path="/about" component={About} />
    </Switch>
</Router>
```

请注意，关键字 exact 仍然必须出现在 Home 组件中，否则它将匹配后续的路线。还要注意，我们必须将`“/about/team”`列在`“/about”`之前，这样当`About`组件看到`“something.com/about/team”`时，路由会转到`Team`组件，而不是`About`组件。如果它首先看到了`“/about”`，它将在那里停止并呈现`About`组件，因为 Switch 只呈现第一个匹配的组件。

## 默认路由

默认路由，或通常用于 404 页面的“全部捕获”路由，是当没有路由匹配时使用的路由。

在 React 路由器 v3 中，默认的`Route`是:

`<Route path=”*” component={NotFound} />`

在 React 路由器 v4 中，默认的`Route`被更改为:

```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

<Router>
    <Switch>
       <Route exact path ="/" component={Home} />
       <Route path="/about/team" component={Team} />
       <Route path="/about" component={About} />
       <Route component={NotFound} /> // this is our default route
    </Switch>
</Router>
```

当你没有在`Route`中包含路径时，组件将一直呈现。因此，正如我们上面所讨论的，我们可以使用`Switch`只获得一个要渲染的组件，然后将“catch all”路径放在最后(因此在`Router`有机会检查其余路径之前，它不会使用该路径)，因此即使其他路径不匹配，某些内容也会一直渲染。

## onEnter

以前，您可以使用`onEnter`来确保`Route`的组件拥有它所需要的所有信息，或者在呈现组件之前进行检查(比如确保用户已经过身份验证)。

这个特性已经被否决了，因为路由的新结构是它们应该像组件一样工作，所以您应该利用组件生命周期方法。

在 React 路由器 v3 中:

`<Route path=”/about” onEnter={fetchedInfo} component={Team}/>`

变成了:

```
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

<Router>
    <Switch>
       <Route exact path ="/" component={Home} />
       <Route path="/about/team" component={Team} />
       <Route path="/about" component={About} />
       <Route component={NotFound} />
    </Switch>
</Router>
```

router.js

```
...

componentDidMount() {
    this.props.fetchInfo();
}

...
```

Team.js

* * *

# 小道具

在 React Router v4 中，通过路由器传递的属性发生了变化，访问它们的方式也发生了变化。路线现在经过三个道具:

*   `history`
*   `location`
*   `match`

## 历史

包含了很多其他的属性和方法，所以我不会把它们都列出来，但是这里有一些可能是最常用的:

*   `length`:历史堆栈中的条目数
*   `location`:包含如下相同信息
*   `push(path, [state])`:在历史堆栈上推送新条目
*   `goBack()`:允许您将历史堆栈上的指针向后移动 1 个条目

重要的是要注意到`history`是可变的，虽然它包含一个`location`属性，但是`location`的这个实例不应该被使用，因为它可能已经被改变了。相反，您想要使用下面讨论的实际的`location`道具。

## 位置

该位置具有属性:

*   `pathname`
*   `search`
*   `hash`
*   `state`

`location.search`用来代替`location.query`，必须解析。我用`URLSearchParams`来解析它。所以像`“https://something.com/about?string=’hello’”`这样的 URL 将被解析如下:

```
...

const query = new URLSearchParams(this.props.location.search)
const string = query.get('string') // string = 'hello'

...
```

About.js

此外，`state`属性可用于通过 props 传递组件的特定于`location`的`state`。因此，如果您想将一些信息从一个组件传递到另一个组件，您可以像这样使用它:

```
...
// To link to another component, we could do this:
<Link to='/path/' />

// However, if we wanted to add state to the location, we could do this:
const location = {
    pathname: '/path/',
    state: { fromDashboard: true },
}
<Link to={location} />
...
```

因此，一旦我们到达由该路径呈现的组件，我们就可以从`location.state.fromDashboard`访问`fromDashboard`。

## 比赛

`match`具有以下特性:

*   `params`:从 URL 获取路径的动态段——例如，如果路径是`“/about/:id”`,在组件中，访问`this.props.match.params`将得到 URL 中的 id
*   `isExact`:如果匹配了整个 URL，则为 true
*   `path`:匹配的路径中的路径
*   `url`:URL 的匹配部分

# 冗余集成

正如我前面提到的，我们发现在我们的情况下，我们不需要额外的库来连接`redux`和我们的路由器，特别是因为我们的主要用例——阻止更新——已经被 react router 覆盖。

## 阻止的更新

在某些情况下，当位置发生变化时，应用程序不会更新。这称为“阻止更新”。如果同时满足以下两个条件，就会发生这种情况:

1.  该组件通过`connect()(Component)`连接到 Redux。
2.  组件不是由`<Route>`呈现的

在这些情况下，我用`withRouter`包装了组件的连接。

这允许路由器信息在链接到组件时跟随组件，因此当 Redux 状态改变时，应用程序仍然会更新。

* * *

就是这样！

这次升级花了我一个多星期——花了几天时间试图弄清楚到底该怎么做，然后又花了几天时间开始真正做出改变。升级到 React Router 4 是一个巨大的变化，不要掉以轻心，但它会使您的路由器更加轻便和易于使用。

请不吝评论/提问！