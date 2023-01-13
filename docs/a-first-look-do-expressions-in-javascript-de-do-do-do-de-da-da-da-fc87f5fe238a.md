# 初看:JavaScript 中的 do 表达式(De Do Do Do，De Da Da Da)

> 原文：<https://www.freecodecamp.org/news/a-first-look-do-expressions-in-javascript-de-do-do-do-de-da-da-da-fc87f5fe238a/>

多纳文·韦斯特

# 初看:JavaScript 中的 do 表达式(De Do Do Do，De Da Da Da)

![1*1EjPzePghALoUfc2lUcQOQ](img/f5851ade9f6480bc5f41a27ddfb9187a.png)

Image: CC BY-SA 2.0 by [isuperwang](https://www.flickr.com/photos/44376038@N00/1036875658)

这篇文章是**而不是**关于警察 1980 年专辑 [*中的热门歌曲*](https://en.wikipedia.org/wiki/Zenyatta_Mondatta) (尽管那会很棒！)不*，*是关于 T39 的提议叫[做表情](https://github.com/tc39/proposal-do-expressions)。如果得到 TC39 的批准，“do 表达式”将成为 JavaScript 的一部分，有助于引导你的代码走出三元地狱。

Do 表达式允许你在表达式中嵌入一个语句。结果值从表达式中返回。

目前它正处于 TC39 进程的“第一阶段”,这意味着 do 表达式在问世之前还有很长的路要走。

[Axel Rauschmayer](https://www.freecodecamp.org/news/a-first-look-do-expressions-in-javascript-de-do-do-do-de-da-da-da-fc87f5fe238a/undefined) 在他名为[探索 ES2016 和 ES2017](https://leanpub.com/exploring-es2016-es2017/) 的书中解释了 [TC39 流程](http://exploringjs.com/es2016-es2017/ch_tc39-process.html)(免费在线，或购买电子书)。

### 什么是 do 表达式？

下面是一个简单 do 表达式的简单例子。

```
const status = do {  if (isLoading) {    'Loading';  } else if (isError) {    'Error'  } else {    'Running'  };};
```

它将“返回”的内容作为语句的值，并将其赋给`status`。在我看来，对于一个基本的`if`语句来说不是很有用。

它真正出彩的地方是在 React 应用程序中的 JSX 内部使用。

### 在 JSX 使用

Do 表达在 JSX 特别有用。让我们来看看如何在 JSX 的上下文中使用它们作为通用的反应模式:根据`loading`和`error`道具决定渲染什么。

```
const View = ({ loading, error, ...otherProps }) => (  <;div>    {do {      if (loading) {        <Loading />      } else if (error) {        <Error error={error} />      } else {        <MyLoadedComponent {...otherProps} />      };    }}  </div>);
```

哇！这是非常强大的。

同样的事情可以用逻辑 AND 语句来执行，但是你最终否定了所有其他的值。这会变得相当混乱，并且很难理解。从执行的角度来看，效率也不高，因为这大致相当于执行三条`if`语句。

```
const View = ({ loading, error, ...otherProps }) => (  <;div>    {loading && !error &&      <Loading />    }    {!loading && error &&      <Error error={error} />    }    {!loading && !error &&      <MyLoadedComponent {...otherProps} />    }  </div>);
```

### 三元地狱是什么？

简单介绍一下背景，[三元组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)是一个接受三个操作数的 JavaScript 操作符:一个条件，后跟两个表达式。它经常被用来代替一个`if`语句。

下面是一个三元运算符的实际例子。

```
const text = isLoading ? 'Loading' : 'Loaded';
```

变量`text`根据`isLoading`二进制标志设置为“加载”或“已加载”。这大致相当于下面的`if`语句。

```
let text;
```

```
if (isLoading) {  text = 'Loading';} else {  text = 'Loaded';}
```

但是，请注意，我们需要使用一个`let`语句，而不是一个`const`语句。您可以看到三元组是减少代码混乱的一个很好的方法，而且它允许您避免使用不必要的`let`语句来代替`const`。

当我进行代码评审时，我把 let 语句看作是一个危险信号。`if`语句也是如此。

但是如果我们有一个非二进制的值呢？你可以合并 ternaries，最后得到这样的结果。

```
const text =   stopSignColor === 'red'     ? 'Stop' :  stopSignColor === 'yellow'     ? 'Caution' :  stopSignColor === 'green'     ? 'Go' :  'Error';
```

这就是我所说的三元地狱。

这是一种使用三元组编写的`if/else if/else if/else`语句。许多人发现这种形式很难遵循。事实上，您甚至可以通过 [ESLint `no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary) 设置来防止这种行为。

下面是使用嵌入在 do 表达式中的`switch`语句编写的相同代码。

```
const text = do {  switch (stopSignColor) {  case 'red': 'Stop'  case 'yellow': 'Caution'  case 'green': 'Go'  case default: 'Error'  }};
```

### 未来就在现在

即使表达不是正式的语言的一部分(还没有？)，您仍然可以在您的项目中使用它们**现在**。这是因为我们大多数人并不真正用 JavaScript 编码——我们用 Babel 编码。幸运的是，有一个[巴别塔变换](https://babeljs.io/docs/plugins/transform-do-expressions/)，它将为我们今天带来明天的语言句法。

但是选择这个选项的时候要小心。不能保证这个提议会原封不动地通过。规范可能会发生巨大的变化，使您的代码需要一些重构。事实上，该规范完全有可能被一起删除。

### 结论

表情有它的位置，但几乎不是银弹。在巴别塔变换的帮助下，它们可以在今天使用。do 表达式的最大好处之一是在 JSX 内部使用。

这就是“我想对你说的一切”。

我也为美国运通工程博客写稿。请登录 [AmericanExpress.io](http://americanexpress.io/) 查看我的其他作品以及我才华横溢的同事的作品。你也可以[在 Twitter 上关注我](https://twitter.com/donavon)。