# 开始使用 React 所需了解的一切

> 原文：<https://www.freecodecamp.org/news/everything-you-need-to-know-to-get-started-in-react-11311ae997cb/>

> “关于开始，最困难的事情就是开始。”——盖伊·川崎

React 是当今最流行的前端库。但是开始使用 React 有时会很困难。涉及到组件层次结构、状态、道具和函数式编程。这篇文章试图解决这个问题，给你一个很好的简单的方法来开始使用 React。所以不浪费任何时间，让我们开始吧。

### 环境

在本文中，我们将使用一个简单的 HTML 文件。只需确保在 HTML 文件的头部分包含以下脚本标记。

```
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.js"></script>
```

所以我们的工作文件应该是这样的。

```
<!DOCTYPE html>
<html>
<head>    
    <title>My React App</title>

    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.js"></script>    
</head>
<body>

    <div id="root"></div>

    <script type="text/babel" >   

       //React code should go here
    </script>
</body>
</html>
</script></body></html>
```

我们现在可以走了。

![Dgrr7QYoN3uYtpLisYwkUzzXyuj-oQhyZdNH](img/fc1a1fe0cabce6edaa77142816619b8c.png)

credits : [https://www.pexels.com](https://www.pexels.com/photo/flight-sky-earth-space-2166/)

### 成分

组件是 React 应用程序的核心。

它们是构建 React 应用程序的独立且可重用的代码块。

让我们看看我们的第一个组件。

```
class App extends React.Component{
 render(){
  return <h3>Hello React World.</h3>
 }
}
ReactDOM.render(            
 <App />,
 document.getElementById('root')
);
```

我们的 App 组件是一个 ES6 类，它扩展了 React 的组件类。它现在有一个名为`render`的方法，该方法返回一个 **h3** 元素，该元素返回文本“Hello React World”。浏览器将只呈现由`render()`方法返回的元素。

#### 但是等一下，这个渲染方法有必要吗？

是的，一个类组件必须包含一个呈现方法。所有其他方法都是可选的。

`ReactDOM.render()`正在 id 为“root”的 div 元素中呈现应用程序组件。它将组件作为第一个参数，将父 div 作为第二个参数。

#### JavaScript 语法扩展(JSX)

我们在 App 组件中声明的 h3 元素不是 HTML，而是 JavaScript 语法扩展(JSX)。JSX 是 JavaScript 中的语法扩展。它使我们能够用 JavaScript 编写类似 JavaScript 对象(JSX)的 HTML。

```
class App extends React.Component{
 render(){
  const element = <h3>Hello React World</h3>;
  return <div>{element}</div>;
 }
}
```

JSX 在编写 HTML 时给了我们 JavaScript 的力量。上面例子中的花括号`{}`告诉 React 编译器**元素**是一个 JavaScript 变量。让我们看另一个更实际的例子。

```
render() {
 const users = [‘Abdul Moiz’,’Linda Lee’,’John Frank’];
 const listItems = users.map(user => <li>{user}</li>);
 return <ul>{listItems}</ul>; 
}
```

在上面的例子中，我们在一个数组中有一个用户列表，我们在这个列表上映射了一个由`li`元素组成的数组。稍后我们将在`ul` 元素中使用它。

JSX 是推荐的方式，也是在 React 中声明用户界面的行业标准。

### 小道具

属性是由父组件传递给子组件的属性。

在 React 中，抽象出子组件中的通用 UI 逻辑是一种常见的模式。在这些情况下，父组件通常将一些数据作为属性传递给子组件。

```
class App extends React.Component {
 render() {
  return <Greet greeting="Hello" />;  
 }
}
class Greet extends React.Component{
 render(){
  return <h3>{this.props.greeting} World</h3>;
 }
}
```

在上面的例子中，我们已经向 Greet 组件传递了一个 greeting 属性，并在我们的 App 组件中使用了它。我们可以从我们类的 *this.props* 对象中访问所有的道具。在这种情况下，我们将*问候语*访问为 *this.props.greeting* 。

#### 好的，但是我可以在道具中传递什么类型的数据呢？

JavaScript 中几乎所有的默认数据结构:字符串、数字、数组、对象，甚至函数。是的，我们可以传递函数，但是我们现在不会讨论这个。

### 状态

状态和道具一样，也保存数据，但数据类型不同。

Props 保存父组件发送的数据。状态保存组件的私有动态数据。状态保存在组件的多个渲染之间变化的数据。

> 属性被传递给组件(比如函数参数)，而状态在组件中管理(比如函数中声明的变量)- React Docs

复杂？别担心，一会儿就都说得通了。

```
class App extends React.Component {
 constructor(){
  super();
  this.state = {name :"Abdul Moiz"};
 }
 changeName(){
  this.setState({name : "John Doe"});
 }

 render(){
  return (
   <div>
     <h3>Hello {this.state.name}</h3>
     <button type='button' onClick=this.changeName.bind(this)}>
      Change
     </button>
   </div>
  );
 }
}
```

正如我们看到的，我们必须在构造函数中初始化状态，然后我们可以在 render 方法中使用它。像 props 一样，我们使用“this.state”对象访问状态。在我们的 *Change* 按钮的点击事件中，我们将 state 中 name 的值更改为“John Doe”。

#### set state()

我们使用 *setState()* 方法来改变我们的状态。 *setState()* 在 React 组件中默认可用，并且是改变状态的唯一方式。我们将一个对象作为参数传递给 *setState()* 。React 将查看传递的对象，并仅使用提供的值更改提供的状态键。

但是等一下，如果 setState() 是改变状态的唯一方法，这是否意味着我不能马上改变状态？

是的，我们不能像这样立即改变状态:

```
this.state.name = “John Doe”;
```

因为当我们调用 *setState()* 时，它告诉 React 数据已经被更改，我们需要用更新后的数据重新渲染组件。直接更新状态对 UI 没有任何影响。

### 事件处理程序

React 中的事件处理程序与 DOM 中的事件处理程序没有太大区别。但是它们有一些小而重要的区别。

在 DOM 中，事件处理程序是小写的，但是在 React 中，事件处理程序是 camelCase。其次，在 DOM 中，事件处理程序以值为字符串，而在 React 中，事件处理程序以函数引用为值。

以下是我们如何在 DOM 中处理事件的示例:

```
<button type=”submit” onclick=”doSomething()”></button>
```

React 中是这样做的:

```
<button type=”submit” onClick=doSomething></button>
```

如果您注意到，在 DOM 中，我们使用`onclick`(小写)DOM 属性处理 click 事件。在 React 中，我们使用 React 中的`onClick` (camelCase)事件处理程序。此外，我们在 DOM 中传递一个字符串值`doSomething()`。但是在 React 中，我们将函数`doSomething`的引用作为值传递。

如果你想阅读 React 提供的完整事件列表(和往常一样，有很多)，可以考虑阅读官方文档中的这篇文章。

累吗？我也是，但是我们就快到了——继续学习！

### 生命周期方法(生命周期挂钩)

React 给了我们一些特殊的方法，叫做生命周期挂钩。这些生命周期挂钩在组件生命周期的特定时间运行。幸运的是，我们可以将自己的功能放入这些生命周期挂钩中，方法是在组件中覆盖它们。让我们看看一些常用的生命周期挂钩。

#### componentDidMount()

挂载是组件第一次在浏览器中呈现的时间。`componentDidMount()`组件安装后运行。这是一个获取任何数据或启动任何东西的好地方。

#### componentDidUpdate()

顾名思义，`componentDidUpdate()`在组件更新后运行。它是处理数据更改的地方。也许您想要处理一些网络请求，或者根据更改的数据执行计算。`componentDidUpdate()`是做这一切的地方。

让我们看看实际情况:

```
class App extends React.Component {
 constructor(){
  super(); 
  this.state = {
   person : {name : "" , city : ""}
  };
 }
 componentDidMount(){
  //make any ajax request
  this.setState({
   person : {name : "Abdul Moiz",city : "Karachi"}
  });
 }
 componentDidUpdate(){
  //because I could'nt come up with a simpler example of //componentDidUpdate
  console.log('component has been updated',this.state);
 }
 render(){
  return (
   <div>
    <p>Name : {this.state.person.name}</p>
    <p>City : {this.state.person.city}</p>
   </div>
  );
 }
}
```

我们的初始状态有两个属性，name 和 city，并且都有一个空字符串作为值。在`componentDidMount()` 中，我们设置了州，并将名称改为“Abdul Moiz ”,城市改为“Karachi”。因为我们改变了状态，所以组件作为执行`componentDidUpdate()`的结果而更新。

### 结论

React 会一直存在。学习 React 可能很难，但是一旦你超越了最初的学习曲线，你就会爱上它。这篇文章旨在让你的学习过程变得简单一些。

别忘了在推特上关注我。

#### 资源

*   [https://reactjs.org/docs](https://reactjs.org/docs/faq-state.html)
*   [http://lucybain.com/blog](http://lucybain.com/blog/2016/react-state-vs-pros/)
*   [https://thinkster.io](https://thinkster.io)