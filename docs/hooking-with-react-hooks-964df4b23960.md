# React 钩子:一种处理 React 状态的新方法

> 原文：<https://www.freecodecamp.org/news/hooking-with-react-hooks-964df4b23960/>

**更新:在 React 16.8 中， [React 钩子](https://reactjs.org/docs/hooks-intro.html)在稳定版本中可用！**

过时:钩子仍然是一个实验性的提议。他们目前在 React v16.7.0-alpha 中

**TL；**
博士在这篇文章中，我们将试图理解什么是[反应钩子](https://reactjs.org/docs/hooks-intro.html)以及如何为我们的利益使用它们。我们将实现不同的例子，看看钩子给我们带来的不同(收获)。如果你想跳过阅读，[这里](https://mihailgaberov.github.io/react-hooks/)你可以在几张幻灯片中找到更短的版本。还有这里的？你可以拿到例子，自己试试。

### 什么是*反应钩*？

> 从功能组件中挂钩到反应状态和生命周期特性的简单功能。

这意味着钩子允许我们轻松地操作功能组件的状态，而不需要将它们转换成类组件。这使我们不必处理所有相关的样板代码。

钩子在类内部不起作用——它们让你在没有类的情况下使用 React。而且，通过使用它们，我们可以完全避免使用生命周期方法，例如*组件卸载*、*组件更新*等。相反，我们将使用内置的挂钩，如 *useEffect* 、 *useMutationEffect* 或 *useLayoutEffect* 。我们一会儿就能看到。

钩子是 JavaScript 函数，但是它们强加了两个额外的[规则](https://reactjs.org/docs/hooks-rules.html):

❗️只在最高层叫钩子**。不要在循环、条件或嵌套函数中调用钩子。**

❗️只从 React 函数组件中调用钩子**。不要从常规的 JavaScript 函数中调用钩子。只有一个地方可以调用钩子——你自己的定制钩子。我们将在本文后面看到它们。**

### 为什么它们是好东西？

？R **使用逻辑**
到目前为止，如果我们想在 React 中重用一些逻辑，我们有两个选择:h [高阶组件](https://tylermcginnis.com/react-higher-order-components/)或 r [ender 道具。有了 React 钩子，我们有了一个更容易理解的选择(在我个人看来！)语法和逻辑流程。](https://www.robinwieruch.de/react-render-props-pattern/)

？通过避免我们在使用类时需要编写的样板代码，或者通过消除对多个嵌套层次的需要(这可能在使用渲染道具时出现)，React Hooks 解决了拥有 giants 组件的问题(这确实很难维护和调试)。

？C **onfusing classes**
再次强调，允许我们在应用程序中不使用类或类组件使得开发人员(尤其是初学者)的生活更加轻松。这是因为我们不需要使用“This”关键字，也不需要了解绑定和范围在 React(和 JavaScript)中是如何工作的。

这并不是说我们(开发人员)不必学习这些概念——相反，我们必须意识到它们。但是这样的话，在使用 React 钩子的时候，我们的担心就少了一个？。

> 那么，在指出钩子解决了什么问题之后，我们什么时候使用它们呢？

> 如果你写了一个函数组件，并意识到你需要给它添加一些状态，以前你必须把它转换成一个类。现在，您可以在现有的函数组件中使用一个钩子。我们将在接下来的例子中这样做。

### 如何使用 *React 钩子*？

React 钩子来到我们这里有[内置的](https://reactjs.org/docs/hooks-overview.html)和[定制的](https://reactjs.org/docs/hooks-custom.html)。后者是我们可以用来在多个 React 组件之间共享逻辑的。

正如我们已经知道的，钩子是简单的 JavaScript 函数，这意味着我们将只编写它，但是是在 React *function* 组件的上下文中。以前这些组件被称为*无状态*，这个术语不再有效，因为*钩子*给了我们一种在这些组件中使用状态的方法？。

> 需要记住的一件重要事情是，我们可以在组件中多次使用内置和自定义挂钩。我们只需要遵循挂钩的[规则。](https://reactjs.org/docs/hooks-rules.html)

下面的例子试图说明这一点。

#### 基本内置挂钩

*   [useState](https://github.com/mihailgaberov/react-hooks/blob/master/src/components/Counter/CounterHooked.js) hook —返回一个有状态值和一个更新它的函数。
*   [use effect](https://reactjs.org/docs/hooks-effect.html)hook——接受包含命令性的、可能有效的代码的函数(例如获取数据或订阅服务)。这个钩子可以返回一个函数，这个函数在每次效果运行之前和组件被卸载时被执行——从最后一次运行开始清理。
*   [use context](https://github.com/mihailgaberov/react-hooks/blob/master/src/components/Counter/CounterHooked.js)hook——接受一个[上下文](https://reactjs.org/docs/context.html)对象并返回当前[上下文](https://github.com/mihailgaberov/react-hooks/blob/master/src/ColorContext.js)值，该值由给定上下文的最近上下文提供者给出。

#### 定制挂钩

**自定义钩子是一个 JavaScript 函数，它的名字以`use`开头，可以调用其他钩子。**举例来说， [useFriendName](https://github.com/mihailgaberov/react-hooks/blob/master/src/useFriendName.jshttps://github.com/mihailgaberov/react-hooks/blob/master/src/useFriendName.js) 下面是我们的第一个定制钩子:

```
export default function useFriendName(friendName) {
  const [isPresent, setIsPresent] = useState(false);

  useEffect(() => {
    const data = MockedApi.fetchData();
    data.then((res) => {
      res.forEach((e) => {
        if (e.name === friendName) {
          setIsPresent(true);
        }
     });
    });
  });

  return isPresent;
}
```

构建您自己的定制钩子允许您将组件逻辑提取到可重用的函数中。这可能是您的应用程序的共享功能，您可以在任何需要的地方导入它。此外，我们不能忘记，我们的自定义钩子是其他允许调用内置钩子的地方。

### 结论

React 钩子并不是一个刚刚出现的新特性。它们是 React 组件的另一种(更好的❓)方式，这些组件需要有*状态*和/或*生命周期*方法。实际上，它们使用了类组件当前使用的相同的内部逻辑。使用还是不使用它们——这是未来将给出最佳答案的问题。

> *我个人的看法？这将是任何涉及状态和生命周期使用的 React 开发的未来。*

让我们看看社区对这个提议会有什么反应？希望我们能在下一个 React 版本中看到它们的完善和全面运行。？

？感谢阅读！？

### 参考

在这里，您可以找到我在撰写本文时发现有用的资源的链接:

*   [https://github.com/mihailgaberov/react-hooks](https://github.com/mihailgaberov/react-hooks)/—链接到 GitHub repo 的示例和演示。
*   [https://mihailgaberov.github.io/react-hooks/](https://mihailgaberov.github.io/react-hooks/#0)—链接到演示幻灯片。
*   [https://reactjs.org/docs/hooks-intro.html](https://reactjs.org/docs/hooks-intro.html)—官方反应博客。
*   [https://youtu.be/dpw9EHDh2bM](https://youtu.be/dpw9EHDh2bM)—Hooks 简介，React Conf 2018
*   [https://medium . com/@ Dan _ abra mov/making-sense-of-react-hooks-FD bde 8803889](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)—Dan abra mov 的解释性文章。
*   [https://daveceddia.com/useeffect-hook-examples/](https://daveceddia.com/useeffect-hook-examples/)—一篇非常有用的文章，解释了 useEffect 钩子的不同用例。
*   https://ppxnl191zx.codesandbox.io/——React 动画库试验钩子的一个例子。
*   https://dev . to/oieduardorabelo/React-Hooks-how-to-create-and-update-context provider-1 f68—一篇短小精悍的文章，展示了如何使用 React Hooks 创建和更新上下文提供者。