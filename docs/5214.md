# 一个骗子的反应路由器 v4 指南

> 原文：<https://www.freecodecamp.org/news/bluffers-guide-to-react-router-v4-20f607a10478/>

在本文中，我们将介绍一些重要的事情，您可以很快学会如何在 React Router v4 上使用和交流。

## 什么是 React 路由器？

React Router 是一个客户端路由器(CSR)，用于 React 项目(*我知道，对吗？*)。它提供了*路由*，这是根据 React web 应用程序中的 URL 路径呈现不同组件的花哨术语。

## 如何安装和使用？

在项目中运行以下命令，将`react-router-dom`保存为项目依赖项:

```
npm i -S react-router-dom
```

使用 ES2015 语法，您可以使用以下命令将 React 路由器导入 React 组件:

```
import * from 'react-router-dom'
```

### 设置您的基本路线

`Link`组件可以被认为是典型的锚链接，当点击时，会将用户重定向到其`to`属性中指定的路径。

组件可以被认为是渲染的控制器。当你看到这些组件时，请想象它们是这样说的:

当*我看到 URL 是我的* `path` *属性中指定的内容时，*然后*我将呈现我的* `component` *或* `render` *属性中列出的组件。*

下面的基本例子大部分摘自 [ReactTraining 的基本例子](https://reacttraining.com/react-router/web/example/basic):

### 嵌套路线

嵌套路由与创建高级路由完全相同，除了定义一个`BrowserRouter`组件(因为嵌套路由是由高级`BrowserRouter`组件组成的)。它只需要更多的`Link`和`Route`组件。我们可以无限期地将更多的路由嵌套在任何其他嵌套路由中。

### 传递 URL 参数

在前面的例子中，我们在嵌套路由中为每个主题定义了不同的组件。在下面的例子中，我们有一个单独的`Topic`组件，它是为三条不同的路线呈现的。`Topic`组件动态呈现`topicId`，它由`Route`作为属性传递，其值使用`:`定义为 URL 的一部分。

当一个`Route`定义了一个要渲染的组件时，它将一个`match`对象传递给它的 props(还有`location`和`history`，但现在不重要)。这个`match`对象有一个`params`对象，它包含由`Route`传递的任何变量，这些变量是使用`path` URL 中的`:`符号定义的(也称为路由参数)。

通过这种方式，我们可以减少为每个`Link`单独创建组件，而是用传递给它的信息生成一个可重用的组件。

### 避免硬编码嵌套链接

当创建嵌套链接时，我们的`Link`组件仍然需要引用整个 URL 路径，而不是它真正关心的位置。这意味着嵌套链接将具有指向其父链接的硬编码位置，这在发生名称更改并且需要大量重命名工作时不是很好。

相反，使用由`Route`传递的`match`对象，我们可以动态地引用它的位置，并使用它来避免硬编码。

例如:
`<Route path="/topics" component={Topics`}/>pas`ses a`match object w`ith`一个带有`value "/t` opi `cs" .` 主题的 url 属性，通过它的 props，可以在定义它的嵌套链接时重用`e the mat` ch.url。

### 避免模糊匹配

默认情况下，当您指定`Route`组件时，*每个匹配的路径将被包含在*中。使用 URL 参数，这就成问题了。由于该参数有效地充当了通配符(因此任何文本都被视为匹配)，您将发现当这些与硬编码的路由混合时，它们都将显示。用`exact`也无济于事。

React 路由器的解决方案是`Switch`组件。`Switch`组件将在*第一个匹配路径上排他地呈现子`Route`组件*。这意味着，如果首先指定了所有硬编码的路线，那么将只呈现这些路线。

### 多路径渲染

有些时候你不希望`Route`组件的不明确匹配，但也会有你希望的时候。

记住，当我看到这个路径时，我们可以将`Route`看作一个简单的“*，然后呈现这个组件*，这意味着我们可以有多个`Route`组件匹配一个页面，但是提供不同的内容。

在下面的示例中，我们使用一个组件来传递 URL 参数，以向用户显示他们的当前路径，并使用另一个组件来呈现内容组件。这意味着为一个 URL 路径呈现两个不同的组件。

### 直接从路线渲染

一个`Route`组件可以被传递一个`component`来呈现，如果有一个可用的话。它也可以使用`render`属性直接呈现一个组件。

```
<Route exact path={url} render={  () => <h3>Please select a topic</h3>} />
```

### 使用 Route 将属性传递给组件

URL 传递的参数很好，但是向组件(如对象)传递需要更多数据的属性怎么办？不允许您添加它无法识别的属性。

```
<Route exact path="/" component={Home} doSomething={() => "doSomething" } /> // doesn't work
```

然而，能够传递属性的是使用`Route` *渲染*方法。

```
<Route exact path="/" render={(props) => <Home {...props} doSomething={() => "doSomething"} />
```

### 使用 Link 向组件传递属性

您还可以通过`Link`组件将属性传递给组件。我们可以传入一个对象，而不是传入一个字符串给`to`属性。在这个对象上，我们可以声明代表我们想要导航到的 URL 的`pathname`。我们还声明了一个`state`对象，它包含我们想要的任何自定义属性。这些属性包含在`location`对象中(在`location.state`中)。

在下面的例子中(*artified，I know…* )，我们传入了一个显示在页面上的消息属性。

### 重定向—使用重定向

您可以使用`Redirect`组件将用户的直接 URL 重定向到另一个 URL。

在下面的例子中，我们看到根据组件`RedirectExample`上的`isAuthenticated`状态是真还是假对用户进行重定向，如果他们登录(到*/主页*)或注销(到*/仪表板*)，则适当地重定向:

### 重定向—使用 withRouter()

另一种重定向方式是使用高阶组件`withRouter`。这允许您将`Route` ( `match`、`location`，以及本例中重要的`history`)的属性传递给*没有通过典型的`Route`组件呈现的组件。我们通过*用`withRouter`包装*我们导出的组件来做到这一点。*

为什么拥有`history`很重要？我们可以使用`history`通过推送一个 URL 到`history`对象来强制重定向。

这种路由方式有一些注意事项，我就不详述了(参见 [withRouter 文档](https://reacttraining.com/react-router/web/api/withRouter))。此外，导出的组件必须仍然由另一个组件组成，该组件在`BrowserRouter`中呈现(在本例中我没有显示)。

### 默认路由组件

有时候一个`Link`可能会引用一个没有对应的`<Route path='/something' component={Something` }/ >的 URL。否则用户将在浏览器栏中键入不正确的 URL。

当这种情况发生时，我们得到的只是一个没有响应的页面或链接，什么也没有发生(这并不像实际导航到一个不存在的页面那样糟糕)。

大多数时候，我们希望至少向用户显示我们找不到他们的内容，也许可以用一个诙谐的图片，比如 Github 的 404 页面。在这种情况下，您将需要一个默认组件，也称为无匹配或无遗漏组件。

当用户单击一个链接时(或者在浏览器的导航栏中输入了错误的内容)，只要没有其他匹配的组件，我们就会被定向到要呈现的默认组件。

注意`Switch`的使用(与 URL 参数的不明确匹配)。因为这个`Route`没有路径，所以它总是被有效地渲染。我们要求一个`Switch`只有在找不到任何其他匹配的`Route`路径时才进行渲染。

### 自定义链接

组件的核心是为作为其属性传递的任何内容呈现一个锚元素。有时候，我们希望对链接组件进行定制，而不必到处都有这些定制。React Router 允许这样做的一种方法是通过创建一个自定义链接(参见 [React Router 培训 guid](https://reacttraining.com/react-router/web/example/custom-link) e 了解更多信息——我们在下面或多或少会用到他们的例子)。

对于一个定制链接，本质上是将现有的`Link`组件包装在一个定制组件中，并提供额外的信息，与[高阶组件模式](https://reactjs.org/docs/higher-order-components.html)没有什么不同。

在我们的例子中，我们只显示没有显示的页面的链接。为了让我们的`CustomLink`组件知道当前显示的是什么页面，我们需要将`Link`组件包装在一个`Route`组件中，这样我们就可以传递 React 路由器的`Route`附带的`match`对象。我们将包装我们的`Link`组件作为子组件传递给`Route`组件。

如果您还记得的话，Route 只是检查当前路径，当我们匹配时，将呈现“某物”(要么由`component` / `render`属性定义，要么作为`Route`的子元素，比如我们的链接组件)。

我们用一个等式检查稍微颠覆了这一点，如果我们没有找到一个`match`对象(如果我们的当前路径与在`CustomLink`中声明的`Route`的`path`属性中定义的不匹配)，那么渲染一个`Link`，否则不渲染任何东西。

### 解析查询字符串

查询字符串是 URL 中看起来像变量的参数，以问号为前缀(比如`www.example.com/search?name=Greg`)。

我要快速撕掉创可贴——React 路由器没有提供解析查询字符串的方法？。然而，它在 l `ocation.search,`内将查询字符串定位为其传递的属性中的字符串，其中上面的示例将被定义为 s `earch: "?name=Greg".`

那怎么办呢？有很多方法可以解决这个问题，包括重新发明轮子。我想有趣地强调一下 npm 包[查询字符串](https://www.npmjs.com/package/query-string)作为一个解决方案，我曾经使用过它，它已经成为我事实上的查询字符串解析器。

### 转换时提示用户

React 路由器 v4 带有一个`Prompt`组件，正如您所猜测的，它会向用户显示一个提示。当用户被警告如果从当前表单页面转换可能会丢失表单数据时，这对于用户来说是一个有用的 UX 功能。

实现导航提示的基本模式是用一个布尔状态属性来决定是否应该提示它们。根据是否为任何表单字段选择了值，通过表单更新此状态。在表单中，呈现一个`Prompt`组件，传入两个属性:一个`when`(这是之前设置的布尔状态)和一个要显示的`message`。

下面是一个例子(主要是根据[反应训练防止转变](https://reacttraining.com/react-router/web/example/preventing-transitions)的例子):

## 摘要

在本文中，您将学习到在 web 应用程序中使用 React Router 的所有基础知识。你甚至应该能够与你的朋友和家人谈论 React 路由的乐趣。

> 如果你喜欢这篇文章，请友好地鼓掌分享你的喜欢。

> 如果你不喜欢这篇文章，并想表达你的不满，你可以发出充满仇恨的掌声。

> *本出版物中表达的**观点**均为作者个人观点。它们无意反映**观点**或作者可能涉及的任何组织或企业的观点。*