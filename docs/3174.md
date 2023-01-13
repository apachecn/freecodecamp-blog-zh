# React Hooks 初学者——关于使用状态和使用效果的大脑友好指南

> 原文：<https://www.freecodecamp.org/news/beginners-guide-to-using-react-hooks/>

"钩子是什么鬼东西？"

我发现自己问这个问题时，我以为我已经涵盖了 React 的所有基础。这就是前端开发人员的生活，游戏总是在变化。输入钩子。

学点新东西总是好的，对吧？当然啦！但有时我们必须问自己“为什么？这个新生事物有什么意义？我必须学吗"？

对于钩子，答案是“不是马上”。如果您一直在学习 React，并且到目前为止一直在使用基于类的组件，那么不必急于转向钩子。挂钩是可选的，可以与现有组件协同工作。当你不得不重写你的整个代码库来让一些新的东西工作时，你不讨厌吗？

无论如何，这里有一些最初引入钩子的原因，以及我建议初学者应该学习它们的原因。

## 在功能组件中使用状态

在钩子出现之前，我们不能在功能组件中使用状态。这意味着，如果您有一个精心制作和测试的功能组件，突然需要存储状态，那么您将陷入将功能组件重构为类组件的痛苦任务中。

万岁！在功能组件中允许状态意味着我们不必重构我们的表示组件[查看本文了解更多](https://scotch.io/courses/5-essential-react-concepts-to-know-before-learning-redux/presentational-and-container-component-pattern-in-react)。

## 类组件很笨重

让我们面对现实吧，类组件有很多样板文件。构造函数，绑定，处处用“这个”。使用功能组件消除了很多这种情况，所以我们的代码变得更容易跟踪和维护。

你可以在 React 文档上阅读更多关于这个[的内容:](https://reactjs.org/docs/hooks-intro.html#classes-confuse-both-people-and-machines)

## 更可读的代码

因为钩子让我们使用功能组件，这意味着与类组件相比代码更少。这使得我们的代码可读性更好。嗯，反正就是这个意思。

我们不必担心函数的绑定，也不必记住“this”与什么相关，等等。我们可以担心如何写我们的代码。

> 如果你刚刚开始使用 React，我的博客上有一堆入门帖子可能会帮到你！点击这里查看:

## 反应状态挂钩

啊，国家。React 生态系统的基石。让我们通过介绍你将要使用的最常见的钩子来尝试一下。

让我们来看一个有状态的类组件。

```
 import React, { Component } from 'react';
import './styles.css';

class Counter extends Component {
	state = {
		count: this.props.initialValue,
	};

	setCount = () => {
		this.setState({ count: this.state.count + 1 });
	};

	render() {
		return (
			<div>
				<h2>This is a counter using a class</h2>
				<h1>{this.state.count}</h1>

				<button onClick={this.setCount}>Click to Increment</button>
			</div>
		);
	}
}

export default Counter; 
```

有了 React 钩子，我们可以重写这个组件，去掉很多东西，让它更容易理解:

```
 import React, { useState } from 'react';

function CounterWithHooks(props) {
	const [count, setCount] = useState(props.initialValue);

	return (
		<div>
			<h2>This is a counter using hooks</h2>
			<h1>{count}</h1>
			<button onClick={() => setCount(count + 1)}>Click to Increment</button>
		</div>
	);
}

export default CounterWithHooks; 
```

从表面上看，代码很少，但这是怎么回事呢？

### 反应状态语法

所以我们已经看到了我们的第一个钩子！万岁！

```
 const [count, setCount] = useState(); 
```

基本上，这使用数组的析构赋值。`useState()`函数给了我们两件事:

*   **保存状态值**的变量，在这种情况下称为`count` - **改变值**的函数，在这种情况下称为`setCount`。

您可以随意命名:

```
 const [myCount, setCount] = useState(0); 
```

您可以像普通变量/函数一样在整个代码中使用它们:

```
 function CounterWithHooks() {
	const [count, setCount] = useState();

	return (
		<div>
			<h2>This is a counter using hooks</h2>
			<h1>{count}</h1>
			<button onClick={() => setCount(count + 1)}>Click to Increment</button>
		</div>
	);
} 
```

注意顶部的`useState`钩。我们声明/析构了两件事:

*   `counter`:保存我们状态值的值
*   `setCounter`:改变我们的`counter`变量的函数

当我们继续阅读代码时，您会看到这一行:

```
 <h1>{count}</h1> 
```

这是一个我们如何使用状态挂钩变量的例子。在我们的 JSX 中，我们将我们的`count`变量放在`{}`中，以 JavaScript 的形式执行它，然后`count`值被呈现在页面上。

将这与使用状态变量的旧的“基于类”的方式进行比较:

```
 <h1>{this.state.count}</h1> 
```

你会注意到我们不再需要担心使用`this`，这让我们的生活变得容易多了——例如，如果没有定义`{count}`, VS 代码编辑器会给我们一个警告，让我们可以尽早发现错误。然而在代码运行之前，它不会知道`{this.state.count}`是否未定义。

继续下一行！

```
 <button onClick={() => setCount(count + 1)}>Click to Increment</button> 
```

这里，我们使用`setCount`函数(记得我们从`useState()`钩子中析构/声明了它)来改变`count`变量。

当按钮被点击时，我们用`1`更新`count`变量。因为这是状态的改变，这触发了重新渲染，React 用新的`count`值更新视图。太棒了。

### 如何设置初始状态？

您可以通过向`useState()`语法传递一个参数来设置初始状态。这可以是一个硬编码值:

```
 const [count, setCount] = useState(0); 
```

或者可以从道具中取出:

```
 const [count, setCount] = useState(props.initialValue); 
```

这将把`count`的值设置为`props.initialValue`的值。

这就是总结。它的美妙之处在于，你可以像自己编写其他变量/函数一样使用状态变量/函数。

### 如何处理多个状态变量？

这是钩子的另一个很酷的地方。我们可以在一个组件中拥有任意多的组件:

```
 const [count, setCount] = useState(props.initialValue);
 const [title, setTitle] = useState("This is my title");
 const [age, setAge] = useState(25); 
```

如你所见，我们有 3 个独立的状态对象。例如，如果我们想更新年龄，我们只需调用 **setAge()** 函数。同样的还有**伯爵**和**爵位**。我们不再受限于旧的笨重的类组件方式，在这种方式下，我们使用 **setState()** 存储一个大规模的状态对象:

```
 this.setState({ count: props.initialValue, title: "This is my title", age: 25 }) 
```

## 那么，当道具或者状态发生变化的时候，更新东西呢？

当使用钩子和功能组件时，我们不再能够访问 React 生命周期方法，如`componentDidMount`、`componentDidUpdate`等等。哦，亲爱的！不要惊慌，我的朋友，React 给了我们另一个可以使用的钩子:

*   *鼓声* *

## 输入 useEffect！

效果挂钩( **useEffect()** )就是我们放“副作用”的地方。

呃，副作用？什么？让我们离题一会儿，讨论一下副作用到底是什么。这将帮助我们理解`useEffect()`是做什么的，以及它为什么有用。

无聊的电脑解释应该是。

> 在编程中，一个副作用是当一个过程在其作用域之外改变一个变量

用 React-y 术语来说，这意味着“当一个组件的变量或状态基于一些外部事物而改变时”。例如，这可能是:

*   当组件接收到改变其状态的新属性时
*   当一个组件发出一个 API 调用并对响应做一些事情时(例如，改变状态)

那么为什么叫副作用呢？嗯，*我们无法确定行动的结果会是什么*。我们永远无法 100%确定我们将会收到什么样的道具，或者 API 调用会得到什么样的响应。而且，我们无法确定这将如何影响我们的组件。

当然，我们可以编写代码来验证和处理错误，等等，但最终我们无法确定这些事情的副作用是什么。

举个例子，当我们改变状态，基于一些外界的东西，这就是所谓的副作用。

解决了这个问题，让我们回到 React 和 useEffect 钩子上来！

当使用功能组件时，我们不再能够使用生命周期方法，如`componentDidMount()`、`componentDidUpdate()`等。因此，实际上(双关语)，useEffect 挂钩取代了当前的 React 生命周期挂钩。

让我们比较一下基于类的组件和我们如何使用 useEffect 钩子:

```
import React, { Component } from 'react';

class App extends Component {
	componentDidMount() {
		console.log('I have just mounted!');
	}

	render() {
		return <div>Insert JSX here</div>;
	}
} 
```

现在使用 useEffect():

```
function App() {
	useEffect(() => {
		console.log('I have just mounted!');
	});

	return <div>Insert JSX here</div>;
} 
```

在我们继续之前，重要的是要知道，默认情况下，**use effect 钩子在每次渲染和重新渲染**时运行。因此，每当组件中的状态发生变化或组件收到新的道具时，它将重新呈现并导致 useEffect 钩子再次运行。

### 运行一次效果(componentDidMount)

那么，如果钩子在每次组件渲染时都运行，我们如何确保钩子在组件挂载时只运行一次呢？例如，如果一个组件从一个 API 获取数据，我们不希望每次组件重新渲染时都发生这种情况！

`useEffect()`钩子接受第二个参数，一个数组，**包含将导致 useEffect 钩子运行**的事情列表。改变时会触发效果挂钩。运行一次效果的关键是传入一个空数组:

```
useEffect(() => {
	console.log('This only runs once');
}, []); 
```

所以这意味着 useEffect 钩子将在第一次渲染时正常运行。然而，当你的组件重新呈现时，useEffect 会想“我已经运行过了，数组里什么都没有，所以我不用再运行了。给我回去睡觉！”什么都不做。

> 总之，空数组= `useEffect`钩子在挂载时运行一次

### 当事情发生变化时使用效果(componentDidUpdate)

我们已经介绍了如何确保一个`useEffect`钩子只运行一次，但是当我们的组件收到一个新的道具时呢？或者我们想在状态改变时运行一些代码？胡克让我们也这样做！

```
 useEffect(() => {
	console.log("The name props has changed!")
 }, [props.name]); 
```

注意我们这次是如何将东西传递给 useEffect 数组的，即 *props.name* 。

在这个场景中，useEffect 钩子将像往常一样在第一次加载时运行。每当你的组件从它的父组件收到一个新的**名称属性**时，useEffect 钩子就会被触发，其中的代码就会运行。

我们可以对状态变量做同样的事情:

```
const [name, setName] = useState("Chris");

 useEffect(() => {
    console.log("The name state variable has changed!");
 }, [name]); 
```

每当`name`变量改变时，组件重新呈现器和 useEffect 钩子将运行并输出消息。由于这是一个数组，我们可以向它添加多项内容:

```
const [name, setName] = useState("Chris");

 useEffect(() => {
    console.log("Something has changed!");
 }, [name, props.name]); 
```

这一次，当`name`状态变量改变，或者`name prop`改变时，useEffect 钩子将运行并显示控制台消息。

### 我们可以使用 componentWillUnmount()吗？

要在组件即将卸载时运行钩子，我们只需从`useEffect`钩子返回一个函数:

```
useEffect(() => {
	console.log('running effect');

	return () => {
		console.log('unmounting');
	};
}); 
```

## 我可以一起使用不同的挂钩吗？

是啊！您可以在一个组件中使用任意数量的挂钩，并随意混合搭配:

```
function App = () => {
	const [name, setName] = useState();
	const [age, setAge] = useState();

	useEffect(()=>{
		console.log("component has changed");
	}, [name, age])

	return(
		<div>Some jsx here...<div>
	)
} 
```

## 结论——接下来呢？

这就是了。钩子允许我们使用老式的 JavaScript 函数来创建更简单的 React 组件，并减少大量的样板代码。

现在，跑进 Reac hooks 的世界，试着自己动手制作吧！说到自己动手做东西...