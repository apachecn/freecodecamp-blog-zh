# 初学者反应环境-完全指南(2021)

> 原文：<https://www.freecodecamp.org/news/react-context-for-beginners/>

React 上下文是每个 React 开发者都要知道的必备工具。它让您可以轻松地在应用程序中共享状态。

在这个全面的指南中，我们将涵盖什么是 React 上下文，如何使用它，何时以及何时不使用上下文，等等。

即使您以前从未使用过 React 上下文，您也处于正确的位置。您将通过简单、循序渐进的示例了解您需要了解的一切。

我们开始吧！

> 想要从头到尾学习 React 的终极指南？查看 [**反应训练营**](https://reactbootcamp.com) 。

## 目录

*   [什么是 React 上下文？](#what-is-react-context)
*   什么时候应该使用 React 上下文？
*   【React context 解决什么问题？
*   如何使用 React 上下文？
*   [什么是 useContext 挂钩？](#what-is-the-usecontext-hook)
*   [你可能不需要上下文](#you-may-not-need-context)
*   【React 上下文是否取代 Redux？
*   [反应上下文警告](#react-context-caveats)

## 什么是反应上下文？

React context 允许我们在 React 应用程序中的任何组件中传递和使用(消费)数据，而无需使用 props。

换句话说，React context 允许我们更容易地跨组件共享数据(状态)。

## 什么时候应该使用 React 上下文？

当您传递可以在应用程序的任何组件中使用的数据时，React context 非常有用。

**这些类型的数据包括:**

*   主题数据(如暗模式或亮模式)
*   用户数据(当前经过身份验证的用户)
*   特定于位置的数据(如用户语言或地区)

数据应该放在不需要经常更新的 React 上下文中。

为什么？因为上下文并不是作为一个完整的状态管理系统。它是为了让消费数据更容易。

您可以将 React 上下文视为 React 组件的全局变量。

## React context 解决什么问题？

React context 帮助我们避免道具演练的问题。

**Props drilling** 是一个术语，用来描述当你通过不需要它的组件将 Props 向下传递到一个嵌套组件的时候。

这里有一个道具演练的例子。在这个应用程序中，我们可以访问主题数据，我们希望将这些数据作为道具传递给应用程序的所有组件。

不过你也看到了，`App`的直系子孙比如`Header`也是要用道具把主题数据传下来的。

```
export default function App({ theme }) {
  return (
    <>
      <Header theme={theme} />
      <Main theme={theme} />
      <Sidebar theme={theme} />
      <Footer theme={theme} />
    </>
  );
}

function Header({ theme }) {
  return (
    <>
      <User theme={theme} />
      <Login theme={theme} />
      <Menu theme={theme} />
    </>
  );
}
```

这个例子有什么问题？

问题是我们正在钻`theme`道具穿过多个并不立即需要它的组件。

除了将组件传递给其子组件之外，`Header`组件不需要`theme`。换句话说，`User`、`Login`、`Menu`直接消费`theme`数据会更好。

这就是 React context 的好处——我们可以完全绕过使用道具，从而避免道具演练的问题。

## 如何使用 React 上下文？

Context 是一个内置于 React 的 API，从 React 版本 16 开始。

这意味着我们可以通过在任何 React 项目中导入 React 来直接创建和使用上下文。

**使用 React 上下文有四个步骤:**

1.  使用`createContext`方法创建上下文。
2.  获取您创建的上下文，并将上下文提供者包装在您的组件树周围。
3.  使用`value` prop 将您喜欢的任何值放在您的上下文提供程序中。
4.  使用上下文使用者读取任何组件中的值。

所有这些听起来是不是令人困惑？比你想象的简单。

让我们来看一个非常基本的例子。在我们的`App`中，让我们使用 Context 传递我们自己的名字，并在嵌套组件:`User`中读取它。

```
import React from 'react';

export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Reed">
      <User />
    </UserContext.Provider>
  )
}

function User() {
  return (
    <UserContext.Consumer>
      {value => <h1>{value}</h1>} 
      {/* prints: Reed */}
    </UserContext.Consumer>
  )
}
```

让我们一步一步地分解我们正在做的事情:

1.  在我们的`App`组件之上，我们用`React.createContext()`创建上下文，并将结果放入变量`UserContext`。几乎在每种情况下，您都希望像我们在这里所做的那样导出它，因为您的组件将在另一个文件中。注意，当我们调用`React.createContext()`时，我们可以传递一个初始值给我们的`value`属性。
2.  在我们的`App`组件中，我们使用的是`UserContext`。确切地说是`UserContext.Provider`。创建的上下文是一个具有两个属性的对象:`Provider`和`Consumer`，这两个属性都是组件。为了将我们的价值传递给应用程序中的每个组件，我们将提供者组件包装在它周围(在本例中，`User`)。
3.  在`UserContext.Provider`上，我们把我们想要的值传递给整个组件树。为此，我们将它设置为等于`value`。在这种情况下，它是我们的名字(在这里，芦苇)。
4.  在`User`中，或者在我们想要消费(或使用)上下文中提供的内容的任何地方，我们使用消费者组件:`UserContext.Consumer`。为了使用我们传递下来的值，我们使用所谓的**渲染道具模式**。它只是消费组件作为道具给我们的一个功能。而在那个函数的返回中，我们可以返回并使用`value`。

## 什么是 useContext 挂钩？

看上面的例子，消费上下文的 render props 模式对您来说可能有点奇怪。

随着 React 钩子的出现，React 16.8 提供了另一种使用上下文的方式。我们现在可以使用 **useContext 钩子**来使用上下文。

我们可以将整个上下文对象传递给`React.useContext()`来使用组件顶部的上下文，而不是使用渲染道具。

下面是上面使用 useContext 钩子的例子:

```
import React from 'react';

export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Reed">
      <User />
    </UserContext.Provider>
  )
}

function User() {
  const value = React.useContext(UserContext);  

  return <h1>{value}</h1>;
}
```

useContext 钩子的好处是它使我们的组件更加简洁，并允许我们创建自己的定制钩子。

您可以直接使用消费者组件，也可以使用 useContext 挂钩，这取决于您喜欢哪种模式。

## 你可能不需要上下文

许多开发人员犯的错误是，一旦他们不得不将道具向下传递到组件的几个级别时，就去寻找上下文。

这是一个嵌套了`Avatar`组件的应用程序，它需要来自`App`组件的两个道具`username`和`avatarSrc`。

```
export default function App({ user }) {
  const { username, avatarSrc } = user;

  return (
    <main>
      <Navbar username={username} avatarSrc={avatarSrc} />
    </main>
  );
}

function Navbar({ username, avatarSrc }) {
  return (
    <nav>
      <Avatar username={username} avatarSrc={avatarSrc} />
    </nav>
  );
}

function Avatar({ username, avatarSrc }) {
  return <img src={avatarSrc} alt={username} />;
} 
```

如果可能的话，我们希望避免通过不需要的组件传递多个道具。

我们能做什么？

我们应该更好地组合我们的组件，而不是因为我们正在进行适当的训练而立即求助于上下文。

因为只有最顶层的组件`App`需要知道`Avatar`组件，所以我们可以直接在`App`中创建它。

这允许我们传递一个道具，而不是两个。

```
export default function App({ user }) {
  const { username, avatarSrc } = user;

  const avatar = <img src={avatarSrc} alt={username} />;

  return (
    <main>
      <Navbar avatar={avatar} />
    </main>
  );
}

function Navbar({ avatar }) {
  return <nav>{avatar}</nav>;
}
```

简而言之:不要马上去找上下文。看看你是否能更好地组织你的组件，以避免支柱钻孔。

## React 上下文是否替代 Redux？

是也不是。

对于许多 React 初学者来说，Redux 是一种更容易传递数据的方式。这是因为 Redux 自带 React 上下文本身。

然而，如果您不同时更新状态，而只是将它传递到组件树中，您就不需要像 Redux 这样的全局状态管理库。

## 对上下文警告做出反应

*为什么不能更新 React 上下文传递下来的值？*

虽然可以将 React context 与一个类似 useReducer 的挂钩结合起来，创建一个临时的状态管理库，而不需要任何第三方库，但出于性能原因，通常不建议这样做。

这种方法的问题在于 React 上下文触发重新渲染的方式。

如果您在 React 上下文提供者上传递一个对象，并且它的任何属性都更新了，会发生什么呢？任何使用该上下文的组件都将重新呈现。

对于状态值很少且不经常更新的小型应用程序(如主题数据)，这可能不是性能问题。但是，如果您要在一个组件树中有很多组件的应用程序中执行许多状态更新，就会出现问题。

## 结论

我希望这篇指南能让你更好地理解如何从前到后使用 React 上下文。

如果您想更深入地了解如何使用 React 上下文来构建令人惊叹的 React 项目，请查看 React 训练营。

## 想成为 React pro 吗？加入 React 训练营

**[React 训练营](https://reactbootcamp.com/)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得内幕信息**100 名开发人员**已经成为 React pro，找到他们梦想的工作，掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](https://reactbootcamp.com/) 
*打开时点击此处通知*