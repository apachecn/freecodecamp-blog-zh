# 如何编写更好的 React 组件

> 原文：<https://www.freecodecamp.org/news/how-to-write-better-react-components/>

ES6 中的 JavaScript 增加了许多特性。这些变化有助于开发人员编写简短、易于理解和维护的代码。

当您使用 [create-react-app](https://github.com/facebook/create-react-app) 创建 react 应用程序时，您已经拥有了对这些更改的支持。这是因为它使用 Babel.js 将 ES6+代码转换成所有浏览器都能理解的 ES5 代码。

在本文中，我们将探索编写更短、更简单、更容易理解的 React 代码的各种方法。所以让我们开始吧。

看看下面的代码沙盒演示:

[https://codesandbox.io/embed/quirky-chebyshev-zff8p](https://codesandbox.io/embed/quirky-chebyshev-zff8p)

这里，我们有两个接受用户输入的输入文本框，以及两个计算输入数字的加减的按钮。

## 避免手动绑定事件处理程序

如你所知，在 React 中，当我们附加任何`onClick`或`onChange`或任何其他类似的事件处理程序时:

```
<input
  ...
  onChange={this.onFirstInputChange}
/> 
```

然后，处理函数(onFirstInputChange)不保留`this`的绑定。

这不是 React 的问题，但这就是 JavaScript 事件处理程序的工作方式。

所以我们必须使用`.bind`方法来正确绑定`this`,如下所示:

```
constructor(props) {
  // some code
  this.onFirstInputChange = this.onFirstInputChange.bind(this);
  this.onSecondInputChange = this.onSecondInputChange.bind(this);
  this.handleAdd = this.handleAdd.bind(this);
  this.handleSubtract = this.handleSubtract.bind(this);
} 
```

上面几行代码将在处理函数中正确维护`this`的类绑定。

但是为每个新的事件处理程序添加新的绑定代码是很繁琐的。幸运的是，我们可以使用类属性语法来修复它。

使用类属性允许我们直接在类内部定义属性。

Create-react-app 内部使用巴别塔版本> = 7 的`@babel/babel-plugin-transform-class-properties`插件和巴别塔版本< 7 的`babel/plugin-proposal-class-properties`插件，所以你不必手动配置。

要使用它，我们需要将事件处理函数转换成箭头函数语法。

```
onFirstInputChange(event) {
  const value = event.target.value;
  this.setState({
    number1: value
  });
} 
```

上述代码可以写成如下形式:

```
onFirstInputChange = (event) => {
  const value = event.target.value;
  this.setState({
    number1: value
  });
} 
```

以类似的方式，我们可以转换其他三个函数:

```
onSecondInputChange = (event) => {
 // your code
}

handleAdd = (event) => {
 // your code
}

handleSubtract = (event) => {
 // your code
} 
```

此外，不需要在构造函数中绑定事件处理程序。这样我们就可以删除代码了。现在，构造函数将如下所示:

```
constructor(props) {
  super(props);

  this.state = {
    number1: "",
    number2: "",
    result: "",
    errorMsg: ""
  };
} 
```

我们可以进一步简化它。类属性语法允许我们直接在类中声明任何变量，这样我们就可以完全移除构造函数并将状态声明为类的一部分，如下所示:

```
export default class App extends React.Component {
  state = {
    number1: "",
    number2: "",
    result: "",
    errorMsg: ""
  };

  render() { }
} 
```

这里有一个代码沙盒演示:[https://codesandbox.io/s/trusting-dust-ukvx2](https://codesandbox.io/s/trusting-dust-ukvx2)

如果您查看上面的代码沙盒演示，您将会看到该功能仍然像以前一样工作。

但是使用类属性使得代码更加简单和易于理解。

如今，你会发现 React 代码是这样写的。

## 使用单个事件处理程序方法

如果您检查上面的代码，您会看到对于每个输入字段，我们都有一个单独的事件处理函数，`onFirstInputChange`和`onSecondInputChange`。

如果输入字段的数量增加，事件处理函数的数量也会增加，这并不好。

例如，如果您正在创建一个注册页面，那么将会有许多输入字段。因此为每个输入字段创建单独的处理函数是不可行的。

让我们改变这一点。

为了创建一个处理所有输入字段的事件处理程序，我们需要给每个输入字段一个惟一的名称，这个名称与相应的状态变量名称完全匹配。

我们已经有了这个设置。我们赋予输入字段的名称`number1`和`number2`也在状态中定义。因此，让我们将两个输入字段的处理程序方法更改为`onInputChange`，如下所示:

```
<input
  type="text"
  name="number1"
  placeholder="Enter a number"
  onChange={this.onInputChange}
/>

<input
  type="text"
  name="number2"
  placeholder="Enter a number"
  onChange={this.onInputChange}
/> 
```

并添加一个新的`onInputChange`事件处理程序，如下所示:

```
onInputChange = (event) => {
  const name = event.target.name;
  const value = event.target.value;
  this.setState({
    [name]: value
  });
}; 
```

这里，在设置状态时，我们用动态值设置动态状态名。因此，当我们更改`number1`输入字段值时，`event.target.name`将是`number1`，而`event.target.value`将是用户输入的值。

当我们更改`number2`输入字段值时，`event.target.name`将是`number2`，而`event.taget.value`将是用户输入的值。

所以这里我们使用 ES6 动态键语法来更新相应的状态值。

现在您可以删除`onFirstInputChange`和`onSecondInputChange`事件处理程序方法。我们不再需要他们了。

这里有一个代码沙盒演示:[https://codesandbox.io/s/withered-feather-8gsyc](https://codesandbox.io/s/withered-feather-8gsyc)

## 使用单一计算方法

现在，让我们重构`handleAdd`和`handleSubtract`方法。

我们使用两个独立的方法，它们有几乎相同的代码，这造成了代码重复。我们可以通过创建一个方法并将一个参数传递给标识加减运算的函数来解决这个问题。

```
// change the below code:
<button type="button" className="btn" onClick={this.handleAdd}>
  Add
</button>

<button type="button" className="btn" onClick={this.handleSubtract}>
  Subtract
</button>

// to this code:
<button type="button" className="btn" onClick={() => this.handleOperation('add')}>
  Add
</button>

<button type="button" className="btn" onClick={() => this.handleOperation('subtract')}>
  Subtract
</button> 
```

这里，我们为`onClick`处理程序添加了一个新的内联方法，通过传递操作名来手动调用一个新的`handleOperation`方法。

现在，像这样添加一个新的`handleOperation`方法:

```
handleOperation = (operation) => {
  const number1 = parseInt(this.state.number1, 10);
  const number2 = parseInt(this.state.number2, 10);

  let result;
  if (operation === "add") {
    result = number1 + number2;
  } else if (operation === "subtract") {
    result = number1 - number2;
  }

  if (isNaN(result)) {
    this.setState({
      errorMsg: "Please enter valid numbers."
    });
  } else {
    this.setState({
      errorMsg: "",
      result: result
    });
  }
}; 
```

并移除之前添加的`handleAdd`和`handleSubtract`方法。

这里有一个代码沙盒演示:[https://codesandbox.io/s/hardcore-brattain-zv09d](https://codesandbox.io/s/hardcore-brattain-zv09d)

## 使用 ES6 析构语法

在`onInputChange`方法中，我们有这样的代码:

```
const name = event.target.name;
const value = event.target.value; 
```

我们可以使用 ES6 析构语法来简化它，如下所示:

```
const { name, value } = event.target; 
```

在这里，我们从`event.target`对象中提取`name`和`value`属性，并创建本地`name`和`value`变量来存储这些值。

现在，在`handleOperation`方法中，不用每次使用`this.state.number1`和`this.state.number2`时都引用状态，我们可以预先分离出那些变量。

```
// change the below code:

const number1 = parseInt(this.state.number1, 10);
const number2 = parseInt(this.state.number2, 10);

// to this code:

let { number1, number2 } = this.state;
number1 = parseInt(number1, 10);
number2 = parseInt(number2, 10); 
```

这里有一个代码沙盒演示:[https://codesandbox.io/s/exciting-austin-ldncl](https://codesandbox.io/s/exciting-austin-ldncl)

## 使用增强的对象文字语法

如果您检查`handleOperation`函数中的`setState`函数调用，它看起来像这样:

```
this.setState({
  errorMsg: "",
  result: result
}); 
```

我们可以使用增强的对象文字语法来简化这段代码。

如果属性名与变量名如`result: result`完全匹配，那么我们可以忽略冒号后面的部分。所以上面的`setState`函数调用可以简化成这样:

```
this.setState({
  errorMsg: "",
  result
}); 
```

这里有一个代码沙盒演示:[https://codesandbox.io/s/affectionate-johnson-j50ks](https://codesandbox.io/s/affectionate-johnson-j50ks)

## 将类组件转换为反应挂钩

从 React 版本 16.8.0 开始，React 增加了一种使用 React 钩子在功能组件内部使用状态和生命周期方法的方式。

使用 React 钩子可以让我们编写一个更短、更易于维护和理解的代码。所以让我们把上面的代码转换成使用 React Hooks 语法。

如果你是 React 钩子的新手，可以看看我的[React 钩子介绍](https://levelup.gitconnected.com/an-introduction-to-react-hooks-50281fd961fe?source=friends_link&sk=89baff89ec8bc637e7c13b7554904e54)文章。

让我们首先将一个 App 组件声明为功能组件:

```
const App = () => {

};

export default App; 
```

为了声明状态，我们需要使用`useState`钩子，所以在文件的顶部导入它。然后创建 3 个`useState`调用，一个用于将号码作为一个对象存储在一起。我们可以使用一个处理函数和另外两个用于存储结果和错误消息的`useState`调用来一起更新它们。

```
import React, { useState } from "react";

const App = () => {
  const [state, setState] = useState({
    number1: "",
    number2: ""
  });
  const [result, setResult] = useState("");
  const [errorMsg, setErrorMsg] = useState("");
};

export default App; 
```

将`onInputChange`处理程序方法改为:

```
const onInputChange = () => {
  const { name, value } = event.target;

  setState((prevState) => {
    return {
      ...prevState,
      [name]: value
    };
  });
}; 
```

这里，我们使用 updater 语法来设置状态，因为在使用 React 钩子时，状态不会在更新对象时自动合并。

所以我们首先展开状态的所有属性，然后添加新的状态值。

将`handleOperation`方法改为:

```
const handleOperation = (operation) => {
  let { number1, number2 } = state;
  number1 = parseInt(number1, 10);
  number2 = parseInt(number2, 10);

  let result;
  if (operation === "add") {
    result = number1 + number2;
  } else if (operation === "subtract") {
    result = number1 - number2;
  }

  if (isNaN(result)) {
    setErrorMsg("Please enter valid numbers.");
  } else {
    setErrorMsg("");
    setResult(result);
  }
}; 
```

现在，返回从类组件的 render 方法返回的同一个 JSX，但是从 JSX 中删除所有对`this`和`this.state`的引用。

这里有一个代码沙盒演示:[https://codesandbox.io/s/musing-breeze-ec7px?file=/src/App.js](https://codesandbox.io/s/musing-breeze-ec7px?file=/src/App.js)

## 隐式返回对象

现在，我们已经优化了我们的代码，以使用现代的 ES6 特性并避免代码重复。还有一件事我们可以做的就是简化`setState`函数调用。

如果您在`onInputChange`处理程序中检查当前的`setState`函数调用，它看起来像这样:

```
setState((prevState) => {
  return {
    ...prevState,
    [name]: value
  };
}); 
```

在箭头函数中，如果我们有这样的代码:

```
const add = (a, b) => {
 return a + b;
} 
```

那么我们可以将其简化如下:

```
const add = (a, b) => a + b; 
```

这是因为如果在 arrow 函数体中只有一个语句，那么我们可以跳过花括号和 return 关键字。这被称为隐式返回。

所以如果我们像这样从 arrow 函数返回一个对象:

```
const getUser = () => {
 return {
  name: 'David,
  age: 35
 }
} 
```

那么我们**不能**这样简化:

```
const getUser = () => {
  name: 'David,
  age: 35
} 
```

这是因为开花括号表示函数的开始，所以上面的代码是无效的。为了使它工作，我们可以像这样把对象放在圆括号中:

```
const getUser = () => ({
  name: 'David,
  age: 35
}) 
```

上面的代码与下面的代码相同:

```
const getUser = () => {
 return {
  name: 'David,
  age: 35
 }
} 
```

所以我们可以使用同样的技术来简化我们的`setState`函数调用。

```
setState((prevState) => {
  return {
    ...prevState,
    [name]: value
  };
});

// the above code can be simplified as:

setState((prevState) => ({
  ...prevState,
  [name]: value
})); 
```

这里有一个代码沙盒演示:[https://codesandbox.io/s/sharp-dream-l90gf?file=/src/App.js](https://codesandbox.io/s/sharp-dream-l90gf?file=/src/App.js)

这种用圆括号将代码包装起来的技术在 React 中被大量使用:

*   要定义功能组件，请执行以下操作:

```
const User = () => (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
); 
```

*   react-redux 中的 mapStateToProps 函数内部:

```
const mapStateToProps = (state, props) => ({ 
   users: state.users,
   details: state.details
}); 
```

*   Redux 操作创建器功能:

```
const addUser = (user) => ({
  type: 'ADD_USER',
  user
}); 
```

和许多其他地方。

## 帮助您编写更好的 React 组件的额外提示

如果我们有这样一个组件:

```
const User = (props) => (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
); 
```

并且以后希望将 props 记录到控制台，只是为了测试或调试，那么不需要将代码转换为以下代码:

```
const User = (props) => {
 console.log(props);
 return (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
 );
} 
```

您可以像这样使用逻辑 OR 运算符(`||`):

```
const User = (props) => console.log(props) || (
   <div>
    <h1>Welcome, User</h1>
    <p>You're logged in successfully.</p>
   </div>
); 
```

它是如何工作的？

`console.log`函数只是打印传递给它的值，但它不返回任何东西——所以它将被评估为未定义的`||`(...).

并且因为`||`操作符返回第一个真值，所以`||`之后的代码也将被执行。

### 感谢阅读！

你可以在我的[掌握现代 JavaScript](https://modernjavascript.yogeshchavan.dev/) 一书中详细了解 ES6+的所有特性。

你也可以看看我的免费的[React 路由器](https://yogeshchavan.podia.com/react-router-introduction)介绍课程。

****订阅我的[每周简讯](https://yogeshchavan.dev/)加入 1000 多名其他订阅者的行列，直接在您的收件箱中获得惊人的提示、技巧、文章和折扣优惠。****