# 学习 React 及其虚拟 DOM 如何工作的快速指南

> 原文：<https://www.freecodecamp.org/news/a-quick-guide-to-learn-react-and-how-its-virtual-dom-works-c869d788cd44/>

> 这是我的“React 初学者”系列文章的一部分，介绍 React、它的核心特性和应该遵循的最佳实践。更多文章来了！

> [下一篇>](https://www.freecodecamp.org/news/how-to-bring-reactivity-into-react-with-states-exclude-redux-solution-4827d293dfc4/)

要不要学 React 不爬[文档](https://reactjs.org/docs/hello-world.html)(顺便说一句写得不错)？你点击了正确的文章。

我们将学习如何用一个 HTML 文件运行 React，然后展示第一个代码片段。

最后，你将能够解释这些概念:道具、功能组件、JSX 和虚拟 DOM。

我们的目标是制造一款显示小时和分钟的手表。React 提供用组件来构建我们的代码。“让我们创建我们的手表组件。

```
<!-- Skipping all HTML5 boilerplate -->
<script src="https://unpkg.com/react@16.2.0/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16.2.0/umd/react-dom.development.js"></script>

<!-- For JSX support (with babel) -->
<script src="https://unpkg.com/babel-standalone@6.24.2/babel.min.js" charset="utf-8"></script> 

<div id="app"></div> <!-- React mounting point-->

<script type="text/babel">
  class Watch extends React.Component {
    render() {
      return <div>{this.props.hours}:{this.props.minutes}</div>;
    }
  }

  ReactDOM.render(<Watch hours="9" minutes="15"/>, document.getElementById('app'));
</script>
```

忽略 HTML 样板文件和脚本导入的依赖关系(用 [unpkg](https://unpkg.com/#/) ，见 [React 示例](https://raw.githubusercontent.com/reactjs/reactjs.org/master/static/html/single-file-example.html))。剩下的几行实际上是 React 代码。

首先，定义观察组件及其模板。然后在 DOM 中挂载 React 并请求呈现一个手表。

### 将数据注入组件

我们的手表很笨，它显示我们提供给它的小时和分钟。

您可以尝试改变这些属性的值(在 React 中称为 **props** )。它会一直显示你所要求的，即使它不是数字。

这种只具有渲染功能的 React 组件是**功能组件。与类相比，它们有更简洁的语法。**

```
const Watch = (props) =>
  <div>{props.hours}:{props.minutes}</div>;

ReactDOM.render(<Watch hours="Hello" minutes="World"/>, document.getElementById('app'));
```

道具只是传递给组件的数据，通常由周围的组件传递。该组件使用 props 进行业务逻辑和呈现。

但是一旦道具不属于组件，它们就**不可变**。因此，提供 props 的组件是唯一能够更新 props 值的代码。

使用道具非常简单。用组件名作为标记名创建一个 DOM 节点。然后赋予它以道具命名的属性。然后道具会通过组件中的`this.props`可用。

### 不加引号的 HTML 呢？

我确信你会注意到由`render`函数返回的未加引号的 HTML。这段代码使用的是 JSX 语言，这是一种在 React 组件中定义 HTML 模板的速记语法。

```
// Equivalent to JSX: <Watch hours="9" minutes="15"/>
React.createElement(Watch, {'hours': '9', 'minutes': '15'});
```

现在你可能想避免 JSX 来定义组件的模板。实际上，JSX 看起来像是[句法糖](https://en.wikipedia.org/wiki/Syntactic_sugar)。

看看下面的代码片段，它展示了 JSX 和 React 语法来建立你的观点。

```
// Using JS with React.createElement
React.createElement('form', null, 
  React.createElement('div', {'className': 'form-group'},
    React.createElement('label', {'htmlFor': 'email'}, 'Email address'),
    React.createElement('input', {'type': 'email', 'id': 'email', 'className': 'form-control'}),
  ),
  React.createElement('button', {'type': 'submit', 'className': 'btn btn-primary'}, 'Submit')
)

// Using JSX
<form>
  <div className="form-group">
    <label htmlFor="email">Email address</label>
    <input type="email" id="email" className="form-control"/>
  </div>
  <button type="submit" className="btn btn-primary">Submit</button>
</form>
```

### 虚拟 DOM 走得更远

这最后一部分更复杂，但非常有趣。它将帮助您理解 React 是如何工作的。

更新网页上的元素(DOM 树中的一个节点)需要使用 DOM API。它会重新绘制页面，但是会很慢(原因见[这篇文章](https://hashnode.com/post/the-one-thing-that-no-one-properly-explains-about-react-why-virtual-dom-cisczhfj41bmssp53mvfwmgrq))。

React 和 Vue.js 等很多框架都绕开了这个问题。他们提出了一个名为虚拟 DOM 的解决方案。

```
{
   "type":"div",
   "props":{ "className":"form-group" },
   "children":[
     {
       "type":"label",
       "props":{ "htmlFor":"email" },
       "children":[ "Email address"]
     },
     {
       "type":"input",
       "props":{ "type":"email", "id":"email", "className":"form-control"},
       "children":[]
     }
  ]
}
```

这个想法很简单。读取和更新 DOM 树是非常昂贵的。所以尽可能少做改动，尽可能少更新节点。

减少对 DOM API 的调用包括将 DOM 树表示保存在内存中。既然我们在讨论 JavaScript 框架，选择 JSON 听起来是合理的。

这种方法立即反映了虚拟 DOM 的变化。

此外，它收集一些更新，以便稍后立即应用到真实的 DOM 上(以避免性能问题)。

你还记得`React.createElement`吗？实际上，这个函数(直接调用或通过 JSX 调用)在虚拟 DOM 中创建了一个新节点。

```
// React.createElement naive implementation (using ES6 features)
function createElement(type, props, ...children) {
  return { type, props, children };
}
```

为了应用更新，虚拟 DOM 核心特性开始发挥作用，即[协调算法](https://reactjs.org/docs/reconciliation.html)。

它的工作是提出最佳的解决方案来解决先前和当前虚拟 DOM 状态之间的差异。

然后将新的虚拟 DOM 应用于真实 DOM。

### 进一步阅读

本文深入探讨了 React 内部和虚拟 DOM 的解释。尽管如此，在使用一个框架时，了解它的工作原理还是很重要的。

如果您想详细了解虚拟 DOM 是如何工作的，请遵循我的阅读建议。你可以写你自己的虚拟 DOM 并且[学习 DOM 渲染](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)。

> [**如何编写自己的虚拟 DOM**](https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060) ‌‌

> [*构建自己的虚拟 DOM 需要知道两件事。你甚至不需要深入 React 的源代码……*](https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060)

感谢您的阅读。抱歉，如果这对于您的 React 第一步来说太专业了。但是我希望现在你知道什么是道具、功能组件、JSX 和虚拟 DOM。

**如果您觉得这篇文章有用，请点击**？**按钮几下，让别人找到文章，以示支持！？**

**别忘了关注我，获取我即将发布的文章的通知**？

> 这是我的“React 初学者”系列文章的一部分，介绍 React、它的核心特性和应该遵循的最佳实践。

> [下一篇>](https://www.freecodecamp.org/news/how-to-bring-reactivity-into-react-with-states-exclude-redux-solution-4827d293dfc4/)

### 查看我的其他文章

#### JavaScript

*   [如何通过编写自己的 Web 开发框架来提高自己的 JavaScript 技能](https://medium.freecodecamp.org/how-to-improve-your-javascript-skills-by-writing-your-own-web-development-framework-eed2226f190)？
*   [使用 Vue.js 时要避免的常见错误](https://medium.freecodecamp.org/common-mistakes-to-avoid-while-working-with-vue-js-10e0b130925b)

#### 秘诀和技巧

*   [停止痛苦的 JavaScript 调试，用源代码图拥抱 Intellij】](https://medium.com/dailyjs/stop-painful-javascript-debug-and-embrace-intellij-with-source-map-6fe68eda8555)
*   [如何毫不费力地减少庞大的 JavaScript 包](https://medium.com/dailyjs/how-to-reduce-enormous-javascript-bundle-without-efforts-59fe37dd4acd)

最初发表于 2018 年 2 月 6 日 www.linkedin.com。