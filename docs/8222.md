# 为什么 React 渲染中的箭头函数和绑定有问题

> 原文：<https://www.freecodecamp.org/news/why-arrow-functions-and-bind-in-reacts-render-are-problematic-f1c08b060e36/>

#### (提示:它使 shouldComponentUpdate 和 PureComponent 变得古怪)

在之前的一篇文章中，我解释了如何[提取 React 子组件以避免在 render](https://medium.freecodecamp.org/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e) 中使用 bind 或 arrow 函数。但是我没有提供一个清晰的演示来说明为什么这是有用的。

这里有一个简单的例子。

在这个例子中，我在 render 中使用一个箭头函数将相关的用户 ID 绑定到每个删除按钮。

[https://codesandbox.io/embed/54k49onoyl?from-embed](https://codesandbox.io/embed/54k49onoyl?from-embed)

在第 35 行，我使用一个箭头函数向 deleteUser 函数传递一个值。这是个问题。

要了解原因，请查看 User.js(在示例中，单击左上角的 hamburger 图标来选择不同的文件)。每次调用 render 时，我都会登录到控制台。我已经将用户声明为一个纯组件。所以用户应该只在道具或状态改变时才重新渲染。但是**当你为一个用户点击 delete 时，注意 render 是为所有用户实例**调用的。

原因如下:父组件在道具上传递箭头功能。箭头函数在每次渲染时被重新分配(使用 bind 也是如此)。因此，尽管我已经将 User.js 声明为一个 PureComponent，但是 User 的 parent 中的 arrow 函数会导致 User 组件看到一个在 props 上为所有用户发送的新函数。所以当 ***任何一个*** 删除按钮被点击时，每个用户都会重新渲染。？

总结:

> 避免渲染中的箭头函数和绑定。它破坏了像 shouldComponentUpdate 和 PureComponent 这样的性能优化。

### 我该怎么办？

作为对比，这里有一个在渲染中不使用箭头功能的例子。

[https://codesandbox.io/embed/jnowr0ww7v?from-embed](https://codesandbox.io/embed/jnowr0ww7v?from-embed)

在本例中，index.js 在 render 中没有箭头功能。而是将相关数据向下传递给 User.js，在 User.js 中，onDeleteClick 用相关的 user.id 调用 props 上传入的 onClick 函数。

有了这个改变，当你点击删除时，注意到渲染没有为其他用户调用！？

### 摘要

为了获得最佳性能，

1.  避免使用箭头函数并在渲染中绑定。
2.  怎么会？[提取子组件](https://medium.freecodecamp.org/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e)，或者[在 HTML 元素上传递数据](https://medium.com/@mgnrsb/another-way-to-avoid-binding-in-render-in-simple-cases-like-this-where-all-you-need-is-to-remember-68af83da0258)。

### 想了解 React 的更多信息吗？⚛️

我已经在 Pluralsight 上创作了[多个 React 和 JavaScript 课程](http://bit.ly/psauthorpageimmutablepost)([免费试用](http://bit.ly/pstrialimmutablepost))。我的最新文章“T4 创建可重用的 React 组件”刚刚发表！？

![1*BkPc3o2d2bz0YEO7z5C2JQ](img/75cd1679a8478654d74616ed5fa09538.png)

[Cory House](https://twitter.com/housecor) 是关于 JavaScript、React、clean code、[多门课程的作者。NET，以及 Pluralsight](http://pluralsight.com/author/cory-house) 上的更多内容。他是 reactjsconsulting.com[公司的首席顾问，微软 MVP 公司 VinSolutions 的软件架构师，在国际上培训软件开发人员，如前端开发和干净编码。Cory 在 Twitter 上以](http://www.reactjsconsulting.com) [@housecor](http://www.twitter.com/housecor) 的身份发关于 JavaScript 和前端开发的推文。