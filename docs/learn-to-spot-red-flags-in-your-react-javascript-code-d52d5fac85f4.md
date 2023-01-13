# 学会发现 React/JavaScript 代码中的危险信号？

> 原文：<https://www.freecodecamp.org/news/learn-to-spot-red-flags-in-your-react-javascript-code-d52d5fac85f4/>

多纳文·韦斯特

# 学会发现 React/JavaScript 代码中的危险信号？

![92Mj61KMZCO5TSFOUpW9ncdgNXrklqJGHnsC](img/959836675dfc0198e98cda1cec0614b0.png)

Original un-colorized photo by [Artem Sapegin](https://unsplash.com/photos/b18TRXc8UPQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这篇观点鲜明的文章将解释在评审 React/JavaScript 项目时需要注意的一些危险信号。避免这些模式可以使您的代码更高效、更可靠、更易于维护。

### ？注意 l `et` 关键字

回到 ES5 时代，`var`是我们唯一可以用来创造变量的方法。ES6 引入了块范围的`let`和`const`关键字。

以我的经验来看，我很少看到你应该使用`let`的情况。当然，它有自己的位置(比如像计数器)，但是对于大多数应用来说`const`更合适。你很快就会明白为什么。

以下面的常见用例为例。如果是负数，它将把`amount`渲染成红色，否则，它将是黑色。

```
let color;
```

```
if (amount < 0) {  color = 'red';} else {  color = 'black';}
```

```
return (  <span style={{ color }}>    {formatCurrency(amount)}  </span>);
```

代码正在使用一个`let`，但是在初始化之后，`color`变量就不再被重新赋值。**这正是`const`的用例！**

由于代码的结构，我们不能简单地用一个`const`代替`let`。然而，如果我们重构使用一个三元组，一个`const`会完美地工作。

```
const color = amount < 0 ? 'red' : 'black';
```

我们不仅从 6 行代码变成了 1 行，而且通过使用`const`而不是`let`，如果我们不小心在代码中的其他地方重新分配了`const`，我们的工具将会抛出一个错误。

如果我在定义了`color`之后试图将它设置为`null`,这就是 ESLint 的输出。

```
/Users/donavon/Projects/my-project/src/index.jsx
```

```
43:5  error 'color' is constant
```

所以下次你感觉你的肌肉记忆开始输入`let`，抓住你自己，用一个`const`代替。十有八九，它会很好地为你服务。

#### 使用 const 的好处

*   强迫你写更干净的代码
*   无意变量重新赋值的编译时检查

### ？破坏是你的朋友

根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) :

> **析构赋值**语法是一个 JavaScript 表达式，它可以将数组中的值或对象中的属性解包到不同的变量中。

这也使得代码更容易阅读。以这个片段为例。

```
render() {  return(    <div className={this.props.className}>      {        this.props.isLoading          ? 'Loading...'          : this.props.children      }    </div>  );}
```

有多个`this.props`操作正在进行。这执行起来比较慢(好吧，勉强可以，但仍然需要进行多次对象属性查找)，而且，这又增加了视觉上的混乱。

```
render() {  const { className, isLoading, children } = this.props;
```

```
 return(    <div className={className}>      {isLoading ? 'Loading...' : children}    </div>  );}
```

通过添加上面的单行代码，代码的其余部分可读性更好。

#### **破坏**的好处

*   潜在的更快执行
*   清洁工
*   不容易出现由错别字引起的隐藏错误

### ？分布在 Object.assign 上

过去，制作一个对象的浅层副本或用其他对象构建一个对象需要`Object.assign`。但是今天，在 babel 的帮助下，我们可以使用新的 ES spread 语法。

下面是一些使用`Object.assign`的代码。

```
const defaults = { foo: 'foo', bar: 'bar' };const obj1 = Object.assign({}, defaults, { bar: 'baz' });// {foo:'foo', bar:'baz'}
```

下面的代码输出相同的结果，但是使用了对象扩展语法。

```
const defaults = { foo: 'foo', bar: 'bar' };const obj1 = { ...defaults, bar: 'baz' };// {foo:'foo', bar:'baz'}
```

这种“语法糖”允许您看到您正在处理的数据，而没有 ES5 管道造成的所有噪音和混乱。你可以在我的文章《 [**噪音就在我们身边**](https://medium.freecodecamp.org/noise-is-all-around-us-d0c0fcb8d48) 》中读到更多关于噪音和杂乱的内容。

 在他的文章“[es 2018:Rest/Spread Properties](http://2ality.com/2016/10/rest-spread-properties.html)”中对 spread vs `Object.assign`做了非常深入的解释。如果你喜欢在管道系统里挖来挖去，你的时间是很值得的。

#### 使用差价的好处

*   更少杂乱
*   潜在效率更高

### ？使用三元组而不是逻辑 AND

对于简单的`if`条件，三元组不是合适的工具。我在关于 ternaries and logical 的文章和我的文章“ [**在 React 中使用 Ternaries and Logical AND**](https://medium.freecodecamp.org/conditional-rendering-in-react-using-ternaries-and-logical-and-7807f53b6935) 的条件渲染”中详细解释了这一点。

#### 使用逻辑 AND 的好处

*   更少杂乱

### ？表达式体箭头函数

箭头函数非常适合在 React 中编写无状态功能组件(sfc ),它有两种形式。语句体是这样的。

```
const SomeFunction = () =&gt; {  return 'value';};
```

以及具有隐含的`return`语句的表达式体形式。

```
const SomeFunction = () =&gt; 'value';
```

有些人错误地称之为“单行”，但是正如你在下面看到的，表达式可以跨越多行。请注意，我使用的是圆括号，而不是花括号。

```
const SomeFunction = () =&gt; (  'value');
```

所以如果你的函数返回一个单一的表达式，并且你不需要任何中间的计算性的`const`语句，在 arrow 函数上使用表达式形式。这是一个简单的想法，当它像这样写出来时，几乎太明显了，但对一些人来说，这是一件容易忽视的事情。

幸运的是，ESLint 可以再次拯救你。

```
/Users/donavon/Projects/my-project/src/index.jsx
```

```
 12:17  error Unexpected block statement surrounding arrow body;         move the returned value immediately after the `=>`
```

唯一要记住的是，为了以表达式的形式返回一个对象，必须将对象文字括在括号中，就像这样。

```
const SomeFunction = () =&gt; ({  foo: 'foo',  bar: 'bar',});
```

#### 表达式体箭头函数的好处

*   更少杂乱

### ？消除重复代码

你不会认为我会在你的代码中写一篇关于红旗的文章而不提 DRY 吧？开始了…

看看下面这两个 sfc。

```
const Foo = () => (  <;div>    <h2 className="sectionTitle">      Foo Title    </h2>    ...  </div>);
```

```
const Bar = () => (  <;div>    <h2 className="sectionTitle">      Bar Title    </h2>    ...  </div>);
```

注意上面`Foo`和`Bar`中突出显示的代码部分几乎是相同的。很明显，它们都以某种风格显示标题。如果您在 4 个、5 个或更多的地方有相同的代码会怎样？很有可能，未来所做的任何更改都需要您在多个地方进行更改。

您应该将重复的代码重构为它自己的函数。这叫晒或者[不要重复自己](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)。

```
const Title = text => (  <h2 className="sectionTitle">    {text}  </h2>);
```

```
const Foo = () => (  <div>    <Title text="Foo Title" />    ...  </div>);
```

```
const Bar = () => (  <div>    <Title text="Bar Title" />    ...  </div>);
```

#### 干代码的好处

*   清洁工
*   更易维护

### ？为什么使用构造函数？

很多时候，在编写有状态 React 类组件时，会创建一个构造函数来设置`state`的初始值。这里有一个常见的例子。

```
constructor(props) {  super(props);  this.state = { count: 0 };}
```

但是您是否知道您可以使用新的类属性提案来完成所有这些工作？上面的代码可以简单地写成这样。

```
state = { count: 0 };
```

你可以在我的文章《 [**构造者已死，构造者**](https://hackernoon.com/the-constructor-is-dead-long-live-the-constructor-c10871bea599) 万岁》中了解更多。

#### 不使用构造函数的好处

*   清洁工

### ？结论

正如我在开篇所说的，学会避免这些模式可以让你的代码更高效、更可靠、更易于维护。

这些不是硬性的规则，而是一般的指导方针，旨在让你开阔眼界，以其他方式看待代码。慎用。您的里程可能会有所不同。

严格的 ESLint 规则会帮助你发现其中的一些，或者…你可以在你的拉取请求中标记我。？

如果这篇文章看起来像你最喜欢的情景喜剧(我讨厌那些)的闪回片段，我很抱歉，但我想揭露那些从未读过我其他文章的人，有机会深入研究。

我也为美国运通工程博客写稿。请登录 [AmericanExpress.io](http://americanexpress.io/) 查看我的其他作品以及我才华横溢的同事的作品。你也可以[在 Twitter 上关注我](https://twitter.com/donavon)。