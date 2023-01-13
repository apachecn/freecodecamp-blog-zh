# 最好的反应例子

> 原文：<https://www.freecodecamp.org/news/react-examples-reactjs/>

React(也称为 React.js)是最流行的 JavaScript 前端开发库之一。这里是 React 语法和用法的集合，您可以将其用作方便的指南或参考。

## **反应元件示例**

React.js 中的组件是可重用的。您可以将值注入 props，如下所示:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Faisal Arkan" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

`name="Faisal Arkan"`将从`function Welcome(props)`给`{props.name}`赋值，并返回一个由`name="Faisal Arkan"`赋值的组件。之后，React 会将元素呈现为 html。

### **声明组件的其他方式**

使用 React.js 声明组件的方式有很多种，有两种组件， *****无状态***组件和 *****有状态***** 组件。**

### **有状态**

#### **类类型组件**

```
class Cat extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      humor: 'happy'
    }
  }
  render() {
    return(
      <div>
        <h1>{this.props.name}</h1>
        <p>
          {this.props.color}
        </p>
      </div>
    );
  }
}
```

### **无状态组件**

#### **功能组件(来自 ES6 的箭头功能)**

```
const Cat = props => {
  return (  
    <div>
      <h1>{props.name}</h1>
      <p>{props.color}</p>
    </div>;
  );
};
```

#### **隐式返回组件**

```
const Cat = props => 
  <div>
    <h1>{props.name}</h1>
    <p>{props.color}</p>
  </div>;
```

# **反应片段示例**

片段是在不使用包装元素的情况下呈现多个元素的方法。当试图在 JSX 中呈现没有封闭标签的元素时，您会看到错误消息`Adjacent JSX elements must be wrapped in an enclosing tag`。这是因为当 JSX 转换文件时，它用相应的标记名创建元素，如果发现多个元素，它不知道使用什么标记名。

在过去，一个常见的解决方案是使用包装器 div 来解决这个问题。不过 React 版本带来了`Fragment`的加入，使得这个不再必要。

充当一个包装器，不会给 DOM 添加不必要的 div。您可以从 React 导入中直接使用它，或者解构它:

```
import React from 'react';

class MyComponent extends React.Component {
  render(){
    return (
      <React.Fragment>
        <div>I am an element!</div>
        <button>I am another element</button>
      </React.Fragment>
    );
  }
}

export default MyComponent;
```

```
// Deconstructed
import React, { Component, Fragment } from 'react';

class MyComponent extends Component {
  render(){
    return (
      <Fragment>
        <div>I am an element!</div>
        <button>I am another element</button>
      </Fragment>
    );
  }
}

export default MyComponent;
```

React 版进一步简化了这个过程，允许将空的 JSX 标签解释为片段:

```
return (
  <>
    <div>I am an element!</div>
    <button>I am another element</button>
  </>
);
```

# 反应 JSX 的例子

### JSX

JSX 是 JavaScript XML 的缩写。

JSX 是一个在 JavaScript 中使用有效 HTML 语句的表达式。您可以将该表达式赋给一个变量，并在其他地方使用。通过将其他有效的 JavaScript 表达式和 JSX 放在大括号(`{}`)中，可以将它们组合在这些 HTML 语句中。巴别塔进一步将 JSX 编译成一个类型为`React.createElement()`的对象。

### **单行&多行表达式**

单行表达式使用起来很简单。

```
const one = <h1>Hello World!</h1>;
```

当您需要在单个 JSX 表达式中使用多行时，请将代码写在单个括号内。

```
const two = (
  <ul>
    <li>Once</li>
    <li>Twice</li>
  </ul>
);
```

### **仅使用 HTML 标签**

```
const greet = <h1>Hello World!</h1>;
```

### **结合 JavaScript 表达式和 HTML 标签**

我们可以在括号中使用 JavaScript 变量。

```
const who = "Quincy Larson";
const greet = <h1>Hello {who}!</h1>;
```

我们还可以在大括号内调用其他 JavaScript 函数。

```
function who() {
  return "World";
}
const greet = <h1>Hello {who()}!</h1>;
```

### **只允许一个父标签**

JSX 表达式只能有一个父标记。我们只能添加嵌套在父元素中的多个标签。

```
// This is valid.
const tags = (
  <ul>
    <li>Once</li>
    <li>Twice</li>
  </ul>
);

// This is not valid.
const tags = (
  <h1>Hello World!</h1>
  <h3>This is my special list:</h3>
  <ul>
    <li>Once</li>
    <li>Twice</li>
  </ul>
);
```

## 反应状态示例

州是数据的来源。

我们应该总是尽量使我们的状态简单，并尽量减少有状态组件的数量。例如，如果我们有十个组件需要来自状态的数据，我们应该创建一个容器组件来保存所有组件的状态。

状态基本上就像一个全局对象，在组件中随处可见。

有状态类组件的示例:

```
import React from 'react';

class App extends React.Component {
  constructor(props) {
    super(props);

    // We declare the state as shown below

    this.state = {                           
      x: "This is x from state",    
      y: "This is y from state"
    }
  }
  render() {
    return (
      <div>
        <h1>{this.state.x}</h1>
        <h2>{this.state.y}</h2>
      </div>
    );
  }
}
export default App;
```

另一个例子:

```
import React from 'react';

class App extends React.Component {
  constructor(props) {
    super(props);

    // We declare the state as shown below
    this.state = {                           
      x: "This is x from state",    
      y: "This is y from state"
    }
  }

  render() {
    let x1 = this.state.x;
    let y1 = this.state.y;

    return (
      <div>
        <h1>{x1}</h1>
        <h2>{y1}</h2>
      </div>
    );
  }
}
export default App;
```

## **更新状态**

您可以使用组件上的`setState`方法来更改应用程序状态中存储的数据。

```
this.setState({ value: 1 });
```

记住`setState`是异步的，所以在使用当前状态设置新状态时要小心。一个很好的例子就是如果你想在你的状态中增加一个值。

#### **走错了路**

```
this.setState({ value: this.state.value + 1 });
```

如果在同一个更新周期中多次调用上面的代码，这可能会导致应用程序出现意外行为。为了避免这种情况，你可以传递一个更新回调函数给`setState`而不是一个对象。

#### **正确的方式**

```
this.setState(prevState => ({ value: prevState.value + 1 }));
```

## **更新状态**

您可以使用组件上的`setState`方法来更改应用程序状态中存储的数据。

```
this.setState({value: 1});
```

记住`setState`可能是异步的，所以在使用当前状态设置新状态时要小心。一个很好的例子就是如果你想在你的状态中增加一个值。

##### **走错了路**

```
this.setState({value: this.state.value + 1});
```

如果在同一个更新周期中多次调用上面的代码，这可能会导致应用程序出现意外行为。为了避免这种情况，你可以传递一个更新回调函数给`setState`而不是一个对象。

##### **正确的方式**

```
this.setState(prevState => ({value: prevState.value + 1}));
```

##### **清洁工的方式**

```
this.setState(({ value }) => ({ value: value + 1 }));
```

当只需要状态对象中有限数量的字段时，对象析构可以用于更干净的代码。

### 反应状态与道具示例

当我们开始使用 React 组件时，我们经常会听到两个术语。他们是`state`和`props`。因此，在本文中，我们将探讨这些是什么以及它们之间的区别。

## **状态:**

*   状态是组件拥有的东西。它属于定义它的那个特定组件。比如一个人的年龄就是那个人的一种状态。
*   状态是可变的。但是它只能由拥有它的组件来更改。因为我只能改变我的年龄，其他人不行。
*   您可以使用`this.setState()`改变状态

参见下面的示例，了解状态的概念:

#### **Person.js**

```
 import React from 'react';

  class Person extends React.Component{
    constructor(props) {
      super(props);
      this.state = {
        age:0
      this.incrementAge = this.incrementAge.bind(this)
    }

    incrementAge(){
      this.setState({
        age:this.state.age + 1;
      });
    }

    render(){
      return(
        <div>
          <label>My age is: {this.state.age}</label>
          <button onClick={this.incrementAge}>Grow me older !!<button>
        </div>
      );
    }
  }

  export default Person;
```

在上面的例子中，`age`是`Person`组件的状态。

## **道具:**

*   道具类似于方法参数。它们被传递给使用该组件的组件。
*   道具是不可改变的。它们是只读的。

参见下面的示例，了解道具的概念:

#### **Person.js**

```
 import React from 'react';

  class Person extends React.Component{
    render(){
      return(
        <div>
          <label>I am a {this.props.character} person.</label>
        </div>
      );
    }
  }

  export default Person;

  const person = <Person character = "good"></Person>
```

在上面的例子中，`const person = <Person character = "good"></Person>`我们将`character = "good"`道具传递给`Person`组件。

它给出的输出是“我是个好人”，事实上我就是。

关于状态和道具还有很多要学的。许多事情可以通过真正钻研编码来学习。所以通过编码来弄脏你的手。

## **反应高阶分量示例**

在 React 中， ****高阶组件**** (HOC)是接受一个组件并返回一个新组件的函数。程序员使用 HOCs 实现 ****组件逻辑复用**** 。

如果您使用过 Redux 的`connect`，那么您已经使用过高阶组件。

核心思想是:

```
const EnhancedComponent = enhance(WrappedComponent);
```

其中:

*   `enhance`是高阶分量；
*   `WrappedComponent`是要增强的组件；和
*   `EnhancedComponent`是新创建的组件。

这可能是特设的主体:

```
function enhance(WrappedComponent) {
  return class extends React.Component {
    render() {
      const extraProp = 'This is an injected prop!';
      return (
        <div className="Wrapper">
          <WrappedComponent
            {...this.props}
            extraProp={extraProp}
          />
        </div>
      );
    }
  }
} 
```

在这种情况下，`enhance`返回一个扩展了`React.Component`的 ****匿名类**** 。这个新组件做三件简单的事情:

*   在`div`元素中呈现`WrappedComponent`；
*   将自己的道具传递给`WrappedComponent`；和
*   给`WrappedComponent`注入额外的道具。

hoc 只是一种使用 React 的组合性质的模式。 ****他们给一个组件添加特性**** 。你可以用它们做更多的事情！