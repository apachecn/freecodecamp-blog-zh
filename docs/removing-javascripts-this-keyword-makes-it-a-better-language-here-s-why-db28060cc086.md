# 去掉 JavaScript 的“this”关键字使它成为一种更好的语言。原因如下。

> 原文：<https://www.freecodecamp.org/news/removing-javascripts-this-keyword-makes-it-a-better-language-here-s-why-db28060cc086/>

用 React 和 Redux 阅读 [****功能架构，学习如何以功能风格构建应用。****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)

当然，这是 JavaScript 中许多混乱的根源。原因是`this`取决于函数是如何被调用的，而不是函数是在哪里定义的。

没有`this`的 JavaScript 看起来是更好的函数式编程语言。

### 这种失去的背景

方法是存储在对象中的函数。为了让一个函数知道在哪个对象上工作，使用了`this`。`this`表示函数的上下文。

在许多情况下会失去上下文。它在嵌套函数中丢失了上下文，在回调中丢失了上下文。

让我们以定时器对象为例。timer 对象会等待上一个调用完成，然后再进行新的调用。它实现了递归 setTimeout 模式。[在下一个例子](https://jsfiddle.net/cristi_salcescu/h3pbc42u/)中，在嵌套函数和回调中，`this`丢失了上下文:

```
class Timer {
 constructor(callback, interval){
    this.callback = callback;
    this.interval = interval;
    this.timerId = 0;
  }

 executeAndStartTimer(){
   this.callback().then(function startNewTimer(){
       this.timerId =  
       setTimeout(this.executeAndStartTimer, this.interval);
   });
 }

 start(){
   if(this.timerId === 0){
     this.executeAndStartTimer();
   }
 }
 stop(){
   if(this.timerId !== 0){
     clearTimeout(this.timerId);
     this.timerId = 0;
   }
 }
}

const timer = new Timer(getTodos, 2000);
timer.start();
function getTodos(){
  console.log("call");
  return fetch("https://jsonplaceholder.typicode.com/todos");
}
```

当方法被用作事件处理程序时，会丢失上下文。让我们以构建搜索查询的 React 组件为例。在用作事件处理程序的两种方法中，`this`丢失了上下文:

```
class SearchForm extends React.Component {
  handleChange(event) {
    const newQuery = Object.freeze({ text: event.target.value });
    this.setState(newQuery);
  }
  search() {
    const newQuery = Object.freeze({ text: this.state.text });
    if (this.props.onSearch) this.props.onSearch(newQuery);
  }
  render() {
    return (
      <form>
      <input onChange={this.handleChange} value={this.state.text} />
      <button onClick={this.search} type="button">Search</button>
      </form>
    );
  }
}
```

这些问题有许多解决方案:方法`bind()`、that/self 模式、arrow 函数。

关于如何解决`this`相关问题的更多信息，请看一下[当“this”失去上下文](https://medium.freecodecamp.org/what-to-do-when-this-loses-context-f09664af076f)时该做什么。

### 这没有封装

产生了安全问题。在`this`上声明的所有成员都是公共的。

```
class Timer {
 constructor(callback, interval){
    this.timerId = "secret";
  }
}

const timer = new Timer();
timer.timerId; //secret
```

### 没有这个，没有定制原型

如果我们不去尝试解决失去上下文和安全的问题，而是一起摆脱它，会怎么样？

删除`this`有一系列的含义。

没有`this`基本上就是没有 `class`，没有函数构造器，没有`new`，没有`Object.create()`。

移除`this`通常意味着没有定制原型。

### 更好的语言

JavaScript 既是一种函数式编程语言，也是一种基于原型的语言。如果我们去掉`this`，我们就剩下 JavaScript 作为一种函数式编程语言。那就更好了。

同时，没有了`this`，JavaScript 提供了一种新的、独特的方式，在没有类和继承的情况下进行面向对象编程。

### 没有这个的面向对象编程

问题是如何在没有`this`的情况下构建对象。

将有两种对象:

*   纯数据对象
*   行为对象

#### 纯数据对象

纯数据对象只包含数据，没有行为。

任何计算字段都将在创建时填写。

纯数据对象应该是不可变的。我们需要在创作的时候就把它们考虑进去。

#### 行为对象

行为对象将是共享相同私有状态的闭包集合。

[让我们用一种无`this`的方法创建](https://jsfiddle.net/cristi_salcescu/8z7mLkca/)定时器对象。

```
function Timer(callback, interval){
  let timerId;
  function executeAndStartTimer(){
    callback().then(function makeNewCall(){
      timerId = setTimeout(executeAndStartTimer, interval);
    });
  }
  function stop(){
    if(timerId){
      clearTimeout(timerId);
      timerId = 0;
    }
  }
  function start(){
    if(!timerId){
      executeAndStartTimer();
    }
  }
  return Object.freeze({
    start,
    stop
  });  
}

const timer = Timer(getTodos, 2000);
timer.start();
```

`timer`对象有两个公共方法:`start`和`stop`。其他的都是隐私。没有`this`丢失上下文的问题，因为没有`this`。

关于为什么在构建行为对象时更倾向于少用`this`的方法，请看一下[类与工厂函数:探索前进的道路](https://medium.freecodecamp.org/class-vs-factory-function-exploring-the-way-forward-73258b6a8d15)。

#### 记忆

原型系统在内存保存方面更好。所有方法在原型对象中只创建一次，由所有实例共享。

当创建数千个相同的对象时，使用闭包构建行为对象的内存开销是显而易见的。在应用程序中，我们有一些行为对象。如果我们以一个 store behavior 对象为例，在应用程序中只有它的一个实例，所以在使用闭包构建它时没有额外的内存开销。

在一个应用程序中，可能有成百上千个纯数据对象。纯数据对象不使用闭包，所以没有内存开销。

### 没有这个的组件

许多组件的框架可能需要它，例如 React 或 Vue。

在 React 中，我们可以创建没有`this`的无状态功能组件，作为纯函数。

```
function ListItem({ todo }){
  return (
    <li>
      <div>{ todo.title}</div>
      <div>{ todo.userName }</div>
    </li>
  );
}
```

我们也可以用 [React 钩子](https://reactjs.org/docs/hooks-overview.html)创建没有`this`的有状态组件。[看看下一个例子](https://codesandbox.io/s/31v5w58wo1):

```
import React, { useState } from "react";
function SearchForm({ onSearch }) {
  const [query, setQuery] = useState({ text: "" });
  function handleChange(event) {
    const newQuery = Object.freeze({ text: event.target.value });
    setQuery(newQuery);
  }
  function search() {
    const newQuery = Object.freeze({ text: query.text });
    if (onSearch) onSearch(newQuery);
  }
  return (
    <form>
      <input type="text" onChange={handleChange} />
      <button onClick={search} type="button">Search</button>
    </form>
  );
};
```

### 移除参数

如果我们去掉了`this`，我们也应该去掉`arguments`，因为它们具有相同的动态绑定行为。

摆脱`arguments`非常简单。我们只是使用新的 rest 参数语法。这次 rest 参数是一个数组对象:

```
function addNumber(total, value){
  return total + value;
}

function sum(...args){
  return args.reduce(addNumber, 0);
}

sum(1,2,3); //6
```

### 结论

避免`this`相关问题的最好方法是根本不使用`this`。

没有`this`的 JavaScript 可以是更好的函数式编程语言。

我们可以构建封装的对象，而不使用`this`，作为闭包的集合。

使用 React 钩子，我们可以创建`this`-更少状态的组件。

也就是说，在不破坏所有现有应用程序的情况下，`this`无法从 JavaScript 中移除。然而，有些事情是可以做的。我们可以不用`this`自己写代码，让它在库中使用。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** ****[函数式 React](https://www.amazon.com/dp/B088FZQ1XN) 。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)