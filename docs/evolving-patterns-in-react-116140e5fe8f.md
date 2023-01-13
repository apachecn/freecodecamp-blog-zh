# React 中的进化模式

> 原文：<https://www.freecodecamp.org/news/evolving-patterns-in-react-116140e5fe8f/>

让我们仔细看看 React 生态系统中出现的一些模式。这些模式提高了可读性、代码清晰性，并将您的代码推向组合和可重用性。

大约 3 年前，我开始与 [**React**](https://reactjs.org/) 合作。当时，没有现成的实践可供学习以利用其功能。

这个社区花了大约两年的时间来解决一些想法。我们从`React.createClass`转向 ES6 `class`和纯功能组件。我们放弃了 mixins，简化了 API[。](https://reactjs.org/blog/2016/04/07/react-v15.html)

现在，随着社区比以往任何时候都大，我们开始看到一些好的模式**在发展**。

为了理解这些模式，你需要对**反应**概念及其生态系统有一个基本的了解。但是，请注意，我不会在本文中涉及它们。

所以让我们开始吧！

#### 条件渲染

我在很多项目中见过下面的场景。

当人们想到 **React** 和 **JSX** 时，他们仍然会想到 **HTML** 和 **JavaScript** 。

所以自然的步骤是**将** 的条件逻辑从实际的返回代码中分离出来。

```
const condition = true;

const App = () => {
  const innerContent = condition ? (
    <div>
      <h2>Show me</h2>
      <p>Description</p>
    </div>
  ) : null;

  return (
    <div>
      <h1>This is always visible</h1>
      { innerContent }
    </div>
  );
};
```

这往往会失去控制，在每个`render`函数的开始有多个[项](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。您必须不断地在函数内部跳转，以了解某个元素何时呈现或不呈现。

或者，尝试下面的模式，在这种模式中，您可以从语言的执行模型中获益。

```
const condition = true;

const App = () => (
  <div>
    <h1>This is always visible</h1>
    {
      condition && (
        <div>
          <h2>Show me</h2>
          <p>Description</p>
        </div>
      )
    }
  </div>
);
```

如果`condition`为假，则不计算`&&`运算符的第二个操作数。如果为真，则返回第二个操作数——**或我们希望呈现的 JSX**。

这允许我们以一种**声明性的**方式将 UI 逻辑与实际 UI 元素混合在一起。

对待 JSX 就像它是你的代码不可分割的一部分！毕竟只是 **JavaScript** 。

#### 传承道具

当您的应用程序增长时，您有更小的组件作为其他组件的容器。

当这种情况发生时，您需要通过组件传递大量的道具。组件不需要它们，但是它的子组件需要。

绕过这个的一个好方法是使用**道具解构**和 **JSX 展开**，正如你在这里看到的:

```
const Details = ( { name, language } ) => (
  <div>
    <p>{ name } works with { language }</p>
  </div>
);

const Layout = ( { title, ...props } ) => (
  <div>
    <h1>{ title }</h1>
    <Details { ...props } />
  </div>
);

const App = () => (
  <Layout 
    title="I'm here to stay"
    language="JavaScript"
    name="Alex"
  />
);
```

所以现在，你可以改变`Details`所需的道具，并确保那些道具没有在多个组件中被引用。

#### 破坏道具

一个应用程序会随着时间而变化，你的组件也是如此。您两年前编写的组件可能是有状态的，但现在它可以转换成无状态的。反过来也发生了很多次！

既然我们讨论了道具解构，这里有一个我用来让我的生活长期更轻松的好技巧。对于这两种类型的组件，你可以用相似的方式来析构你的道具，如下所示:

```
const Details = ( { name, language } ) => (
  <div>
    <p>{ name } works with { language }</p>
  </div>
);

class Details extends React.Component {
  render() {
    const { name, language } = this.props;
    return (
      <div>
        <p>{ name } works with { language }</p>
      </div>
    )
  }
}
```

注意线`2–4`和`11–13`是**相同的。**使用这种模式转换组件要容易得多。此外，您还限制了组件中`this`的使用。

#### 提供商模式

我们看了一个例子，其中道具需要通过另一个组件向下发送。但是如果你必须把它分成 15 个部分呢？

进入[反应上下文](https://reactjs.org/docs/context.html)！

这不一定是 React 最值得推荐的特性，但它可以在需要时完成工作。

最近[宣布](https://twitter.com/acdlite/status/956390180637650944)上下文正在获得一个新的 API，它实现了开箱即用的**提供者模式**。

如果你正在使用像 [React Redux](https://github.com/reactjs/react-redux) 或 [Apollo](https://github.com/apollographql/react-apollo) 这样的东西，你可能对这个模式很熟悉。

了解它如何与当今的 API 一起工作，也将有助于您理解新的 API。你可以玩下面的沙盒。

[https://codesandbox.io/embed/rww6k3mq94?fontsize=14](https://codesandbox.io/embed/rww6k3mq94?fontsize=14)

顶层组件——称为**提供者**——在上下文中设置一些值。被称为**消费者**的子组件将从上下文中获取这些值。

当前的上下文语法有点奇怪，但是即将到来的版本正在实现这种模式。

#### 高阶组件

再来说说复用性。除了放弃旧的`React.createElement()`工厂，React 团队也放弃了对 [mixins](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) 的支持。在某种程度上，它们是通过简单对象组合来组合组件的标准方式。

[高阶组件](https://reactjs.org/docs/higher-order-components.html)——HOCs 从现在开始——被淘汰，以满足跨多个组件重用行为的需求。

HOC 是一个函数，它接受一个输入组件，并返回该组件的增强/修改版本。你会发现 hoc 有不同的名字，但我更愿意把它们看作是**装修工**。

如果你正在使用 Redux，你会发现`connect`函数是一个特设的——获取你的组件并添加一堆*道具*到其中。

让我们实现一个可以向现有组件添加道具的基本 HOC。

```
const withProps = ( newProps ) => ( WrappedComponent ) => {
  const ModifiedComponent = ( ownProps ) => ( // the modified version of the component
    <WrappedComponent { ...ownProps } { ...newProps } /> // original props + new props
  );

  return ModifiedComponent;
};

const Details = ( { name, title, language } ) => (
  <div>
    <h1>{ title }</h1>
    <p>{ name } works with { language }</p>
  </div>
);

const newProps = { name: "Alex" }; // this is added by the hoc
const ModifiedDetails = withProps( newProps )( Details ); // hoc is curried for readability

const App = () => (
  <ModifiedDetails 
    title="I'm here to stay"
    language="JavaScript"
  />
);
```

如果你喜欢函数式编程，你会喜欢使用高阶组件。 [Recompose](https://github.com/acdlite/recompose) 是一个伟大的软件包，给你所有这些漂亮的实用程序，如`**withPro**ps`、`**withContext**`、`**lifecycle**`等等。

让我们来看一个非常有用的**重用功能**的例子。

```
function withAuthentication(WrappedComponent) {
  const ModifiedComponent = (props) => {
    if (!props.isAuthenticated) {
      return <Redirect to="/login" />;
    }

    return (<WrappedComponent { ...props } />);
  };

  const mapStateToProps = (state) => ({
    isAuthenticated: state.session.isAuthenticated
  });

  return connect(mapStateToProps)(ModifiedComponent);
}
```

当您想要渲染路线中的敏感内容时，可以使用`withAuthentication`。该内容仅对已登录的用户可用。

这是一个[横切关注点](https://en.wikipedia.org/wiki/Cross-cutting_concern)你的应用程序在一个地方实现，并可在整个应用程序中重用。

然而，HOCs 也有不好的一面。每个 HOC 都会在 DOM/vDOM 结构中引入一个额外的 React 组件。随着应用程序的扩展，这可能会导致潜在的性能问题。

由[迈克尔杰克逊](https://twitter.com/mjackson)撰写的[这篇伟大的文章](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce)中总结了关于 HOCs 的一些其他问题。他主张用我们接下来要讨论的模式来代替 HOCs。

#### 渲染道具

虽然**渲染道具**和**特效**可以互换，但我并不偏爱其中一个。这两种模式都用于提高可重用性和代码清晰度。

这个想法是，你**将渲染函数的控制权让给另一个组件**，然后这个组件通过一个函数属性将控制权传递给你。

有些人喜欢用**动态道具**来做这个，有些人就用`**this.props.children**`。

我知道，这仍然很混乱，但是让我们看一个简单的例子。

```
class ScrollPosition extends React.Component {
  constructor( ) {
    super( );
    this.state = { position: 0 };
    this.updatePosition = this.updatePosition.bind(this);
  }

  componentDidMount( ) {
    window.addEventListener( "scroll", this.updatePosition );
  }

  updatePosition( ) {
    this.setState( { position: window.pageYOffset } )
  }

  render( ) {
    return this.props.children( this.state.position )
  }
}

const App = () => (
  <div>
    <ScrollPosition>
      { ( position ) => (
        <div>
          <h1>Hello World</h1>
          <p>You are at { position }</p>
        </div>
      ) }
    </ScrollPosition>
  </div>
);
```

这里我们使用`children`作为渲染道具。在>组件的`*<ScrollPositi*`中，我们将发送一个函数，该函数接收`s the po`位置作为参数。

Render props 可以用在这样的情况下:你需要一些可重用的逻辑**在组件**中，并且你不想把你的组件包装在一个 HOC 中。

React-Motion 是一个提供一些使用渲染道具的例子的库。

最后，让我们看看如何将**异步**流与渲染道具集成在一起。这里有一个创建可重用 `Fetch`组件的好例子。

我正在分享一个沙盒链接，所以你可以玩它，并看到结果。

[https://codesandbox.io/embed/myv3nywvp?fontsize=14](https://codesandbox.io/embed/myv3nywvp?fontsize=14)

你可以让**多个**渲染道具用于同一个组件。有了这种模式，您就有无限的组合和重用功能的可能性。

你用什么样的模式？他们中的哪一个适合这篇文章？给我留言或者在推特上写下你的想法。

如果你觉得这篇文章有用，帮我分享给社区吧！