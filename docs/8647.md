# 函数集状态是 React 的未来

> 原文：<https://www.freecodecamp.org/news/functional-setstate-is-the-future-of-react-374f30401b6b/>

由大法官 Mba

# 函数集状态是 React 的未来

![1*K8A3aXts5rTCHYRcdHIR6g](img/421304da1ead4d99c7715be960add12f.png)

更新:我在 React 集会上做了一个关于这个话题的后续报告。虽然这篇文章更多的是关于“功能性 setState”模式，但是这篇文章更多的是关于深入理解 setState

**React 普及了 JavaScript 中的函数式编程。这导致大型框架采用 React 使用的基于组件的 UI 模式。现在，功能热正蔓延到整个 web 开发生态系统。**

**但是 React 团队远没有放松。他们继续深入挖掘，发现了隐藏在传奇图书馆中的更多功能性珍宝。**

**所以今天我向你揭示一个埋藏在 React 中的新功能黄金，最好保密 React—**功能设置状态！****

**好吧，我刚刚编造了这个名字…而且它并不完全是新的或者秘密。不，不完全是。看，这是 React 内置的模式，只有少数真正深入研究过的开发人员知道。它从来没有名字。但是现在它做到了——**功能设置状态！****

**按照[丹·阿布拉莫夫](https://www.freecodecamp.org/news/functional-setstate-is-the-future-of-react-374f30401b6b/undefined)描述这种模式的话来说，**函数集状态**是这样一种模式**

> **"独立于组件类声明状态变化."**

**啊？**

### **好吧…你已经知道的**

**React 是一个基于组件的 UI 库。组件基本上是一个接受某些属性并返回 UI 元素函数。**

```
`function User(props) {
  return <div>A pretty user</div>;
}` 
```

**组件可能需要拥有并管理它的状态。在这种情况下，您通常将组件编写为一个类。然后在类`constructor`函数中激活它的状态:**

```
`class User {
  constructor() {
    this.state = { score: 0 };
  }
  render() {
    return <div>This user scored {this.state.score}</div>;
  }
}`
```

**为了管理状态，React 提供了一个名为`setState()`的特殊方法。你像这样使用它:**

```
`class User {
  ...
  increaseScore() {
    this.setState({ score: this.state.score + 1 });
  }
  ...
}` 
```

**注意`setState()`是如何工作的。你传递给它 ***一个对象*** 包含你想要更新的状态的一部分。换句话说，您传递的对象将具有与组件状态中的键相对应的键，然后`setState()`通过将对象合并到状态来更新或*设置状态。因此，“设置状态”。***

### **你可能不知道的是**

**还记得我们说过`setState()`是如何工作的吗？那么，如果我告诉你，你可以传递一个函数 ，而不是传递一个对象，会怎么样呢？**

**是的。`setState()`也接受一个函数。该函数接受组件的*前一个*状态和*当前*属性，并使用它们来计算和返回下一个状态。请看下面:**

```
`this.setState(function (state, props) {
  return { score: state.score - 1 };
});` 
```

**注意`setState()`是一个函数，我们正在传递另一个函数给它(函数式编程… **functional setState** )。乍一看，这似乎很难看，设置状态需要太多的步骤。你为什么会想这么做？**

### **为什么要传递一个函数给`setState?`**

**事实是，[状态更新可能是异步的](https://facebook.github.io/react/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)。**

**想想[把`setState`](https://facebook.github.io/react/docs/reconciliation.html) `()` [叫做](https://facebook.github.io/react/docs/reconciliation.html)会怎么样。React 会首先将你传递给`setState()`的对象合并到当前状态。然后就会开始那个*和解*的事情。它将创建一个新的 React 元素树(UI 的对象表示)，将新树与旧树进行比较，根据您传递给`setState()`的对象找出发生了什么变化，然后最终更新 DOM。**

**咻！这么多工作！事实上，这甚至是一个过于简化的总结。但是相信 React！**

> **React 并不是简单的“设置状态”。**

**因为工作量很大，打电话给`setState()`可能不会让*立刻更新你的状态*。**

> **React 可能会将多个`setState()`调用批处理到单个更新中以提高性能。**

**React 这是什么意思？**

**首先，“*多次`setState()`调用”*可能意味着在一个函数中多次调用`setState()`，就像这样:**

```
`state = { score: 0 };
// multiple setState() calls
function increaseScoreBy3() {
  this.setState({ score: this.state.score + 1 });
  this.setState({ score: this.state.score + 1 });
  this.setState({ score: this.state.score + 1 });
}` 
```

**现在，当 React 遇到“*多个`setState()`调用”、*时，React 将会避开我上面描述的大量工作，并聪明地对自己说:“不！我不会爬这座山三次，每次旅行都携带和更新一些州的信息。不，我宁愿得到一个容器，将所有这些切片打包在一起，然后只更新一次。”而那个，朋友们，就是**配料**！**

**记住你传递给`setState()`的是一个普通的对象。现在，假设任何时候 React 遇到“*多个`setState()`调用”，*它通过提取传递给每个`setState()`调用的所有对象，将它们合并在一起形成一个单独的对象，然后使用这个单独的对象做`setState()`的事情。**

**在 JavaScript 中，合并对象可能看起来像这样:**

```
`const singleObject = Object.assign(
  {},
  objectFromSetState1,
  objectFromSetState2,
  objectFromSetState3
);` 
```

**这种模式被称为**物体构成。****

**在 JavaScript 中，组成对象的“合并”或**的工作方式是:如果三个对象具有相同的键值，则传递给`Object.assign()`的*最后一个对象*的键值获胜。例如:****

```
`const me = { name: "Justice" },
  you = { name: "Your name" },
  we = Object.assign({}, me, you);
we.name === "Your name"; //true
console.log(we); // {name : "Your name"}`
```

**因为`you`是最后一个合并到`we`中的对象，所以`you`对象中`name`的值——“你的名字”——会覆盖`me`对象中`name`的值。所以“你的名字”成为了`we`对象……`you`赢了！:)**

**因此，如果你用一个对象多次调用`setState()`——每次传递一个对象——React 将**合并**。或者换句话说，它将从我们传递给它的多个对象中**组成**一个新对象。如果任何对象包含相同的关键字，则存储具有相同关键字的最后一个*对象的关键字值。对吗？***

**这意味着，给定我们上面的`increaseScoreBy3`函数，该函数的最终结果将只是 1 而不是 3，因为 React 没有按照我们称为`setState()`的顺序*立即*更新状态。但是首先，React 将所有对象组合在一起，结果是:`{score : this.state.score + 1}`，然后只对新组合的对象“设置状态”一次。大概是这样:`User.setState({score : this.state.score + 1}`。**

**要超级清楚，把 object 传递给`setState()`不是这里的问题。真正的问题是，当您想要从前一个状态计算下一个状态时，将对象传递给`setState()`。所以别再这样了。不安全！**

> **因为`*this.props*`和`*this.state*`可能会异步更新，所以您不应该依赖它们的值来计算下一个状态。**

**索菲亚·舒梅克的钢笔展示了这个问题。玩弄它，并注意这支笔中坏的和好的解决方案:**

### **功能设置状态拯救**

**如果你还没有花时间玩上面的钢笔，我强烈建议你这样做，因为它将帮助你掌握这篇文章的核心概念。**

**当你在玩上面的笔时，你无疑看到了 **functional setState** 修复了我们的问题。但是具体怎么做呢？**

**让我们咨询一下 React-Dan 的奥普拉。**

**注意他给出的答案。当你做函数设置时…**

> **更新将被排队，稍后按调用的顺序执行。**

**因此，当 React 遇到“*多个`functional setState()`调用”时，*不是将对象合并在一起，(当然没有要合并的对象)，React *将函数“*按照它们被调用的顺序排队****

**之后，React 继续通过调用“队列”中的每个函数来更新状态，将*先前的*状态传递给它们——也就是说，第一个函数`setState()`调用之前的状态*(如果它是当前执行的第一个函数 setState())或者队列中的*先前的*函数`setState()`调用的*最新*更新的状态。***

**同样，我认为看到一些代码会很棒。不过，这次我们要假装一切。要知道这不是真实的东西，而是给你一个关于 React 正在做什么的想法。**

**此外，为了使它不那么冗长，我们将使用 ES6。如果你想的话，你可以以后再写 ES5 版本。**

**首先，让我们创建一个组件类。然后，在它里面，我们将创建一个*假* `setState()`方法。此外，我们的组件将有一个`increaseScoreBy3()` 方法*，*，它将执行一个多功能的 setState。最后，我们将实例化该类，就像 React 会做的那样。**

```
`class User {
  state = { score: 0 };
  //let's fake setState
  setState(state, callback) {
    this.state = Object.assign({}, this.state, state);
    if (callback) callback();
  }
  // multiple functional setState call
  increaseScoreBy3() {
    this.setState((state) => ({ score: state.score + 1 })),
      this.setState((state) => ({ score: state.score + 1 })),
      this.setState((state) => ({ score: state.score + 1 }));
  }
}
const Justice = new User();` 
```

**注意，setState 还接受可选的第二个参数—回调函数。如果它存在，React 会在更新状态后调用它。**

**现在，当用户触发`increaseScoreBy3()`时，React 将多个函数 setState 排队。我们不会在这里伪造这个逻辑，因为我们的重点是**实际上是什么使得函数 setState 安全*。*** 但是你可以把那个“排队”过程的结果想象成一个函数数组，就像这样:**

```
`const updateQueue = [
  (state) => ({ score: state.score + 1 }),
  (state) => ({ score: state.score + 1 }),
  (state) => ({ score: state.score + 1 }),
];` 
```

**最后，让我们假装更新过程:**

```
`// recursively update state in the order
function updateState(component, updateQueue) {
  if (updateQueue.length === 1) {
    return component.setState(updateQueue[0](component.state));
  }
  return component.setState(updateQueue[0](component.state), () =>
    updateState(component, updateQueue.slice(1))
  );
}
updateState(Justice, updateQueue);` 
```

**没错，这不是一个性感的代码。我相信你能做得更好。但是这里的关键焦点是，每次 React 执行来自您的 **functional setState 的函数时，** React 通过向它传递一个更新状态的*新*副本来更新您的状态。这使得**功能设置状态**到**可以基于先前状态**设置状态。**

**这里我用完整的代码做了一个 bin。修补它(可能使它看起来更性感)，只是为了获得更多的感觉。**

**[**functionalsetstateactivation**](http://jsbin.com/najewe/edit?js,console)
[*在这个 bin 里玩代码会很有趣。记住！我们只是假装反应来理解这个想法...*jsbin.com](http://jsbin.com/najewe/edit?js,console)**

**玩弄它才能完全掌握它。当你回来的时候，我们将会看到是什么让 setState 变得如此完美。**

### **保守得最好的反应秘密**

**到目前为止，我们已经深入探讨了为什么在 React 中执行多个函数 setStates 是安全的。但是我们实际上还没有完成函数 setState 的 *complete* 定义:“独立于组件类声明状态变化。”**

**多年来，设置状态的逻辑——也就是我们传递给`setState()`的函数或对象——一直存在于组件类的*中。这比声明性更重要。***

**好了，今天，我向大家介绍新发掘的宝藏——保守得最好的**反应秘密:****

**感谢丹·阿布拉莫夫！**

**这就是函数式 setState 的强大之处。在组件类外声明状态更新逻辑*。然后在*里面把它叫做*你的组件类。***

```
`// outside your component class
function increaseScore(state, props) {
  return { score: state.score + 1 };
}
class User {
  ...
  // inside your component class
  handleIncreaseScore() {
    this.setState(increaseScore);
  }
  ...
}` 
```

**这是声明性的！你的组件类不再关心状态如何更新。它简单地*声明*它想要的更新的*类型*。**

**为了更好地理解这一点，请考虑那些通常有许多状态切片的复杂组件，它们根据不同的动作更新每个切片。有时，每个更新函数都需要许多行代码。所有这些逻辑都存在于组件的内部。但现在不是了！**

**此外，如果你像我一样，我喜欢让每个模块尽可能短，但现在你觉得你的模块太长了。现在，您可以将所有状态更改逻辑提取到不同的模块中，然后导入并在组件中使用它。**

```
`import { increaseScore } from "../stateChanges";
class User {
  ...
  // inside your component class
  handleIncreaseScore() {
    this.setState(increaseScore);
  }
  ...
}` 
```

**现在，您甚至可以在不同的组件中重用 increaseScore 函数。导入就好。**

**用函数 setState 还能做什么？**

**让测试变得简单！**

**你也可以通过**额外的**参数来计算下一个状态(这个让我大吃一惊……# funfunfunfunction)。**

**期待更多…**

### **[React 的未来](https://github.com/reactjs/react-future/tree/master/07%20-%20Returning%20State)**

**![0*uInBa_PPwz5aLo0j](img/d07a9f351df715937764c826bd1acf6d.png)**

**多年来，react 团队一直在试验如何最好地实现[有状态函数](https://github.com/reactjs/react-future/blob/master/07%20-%20Returning%20State/01%20-%20Stateful%20Functions.js)。**

**Functional setState 似乎是这个问题的正确答案(很可能)。**

**嘿，丹！有什么遗言吗？**

**如果你已经走到这一步，你可能和我一样兴奋。今天就开始尝试这个**功能集状态**！**

**如果你觉得我做得不错，或者其他人应该有机会看到这一点，请点击下面的绿色心脏，帮助在我们的社区中传播对 React 的更好理解。**

**如果你有一个问题没有得到回答，或者你不同意这里的一些观点，请在这里或通过 [Twitter](https://twitter.com/Daajust) 发表评论。**

**编码快乐！**