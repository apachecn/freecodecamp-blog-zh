# 如何准备 React 面试——前端技术面试指南

> 原文：<https://www.freecodecamp.org/news/prepare-for-react-technical-interviews/>

前端技术面试是潜在雇主评估你在网站开发方面的技能和知识的机会。面试官会问你一些关于你在 HTML、CSS 和 JavaScript 方面的经验和技能的问题。他们还可能会问你一些关于 React、Angular、Vue 或他们使用的任何框架的特定问题。他们可能还会给你一个编码挑战，以测试你在某个领域的能力。

今天，我们来看看前端技术面试中最常见的问题，重点是 React 和 JavaScript。

## 面试官在寻找什么

当面试一个前端 web 开发职位时，要准备好讨论你在各种编程语言、工具和框架方面的技能和经验。面试官还希望看到你对 web 开发的最新趋势和技术有很强的理解。

准备好谈论你过去的项目，以及你是如何解决各种挑战的。

通过讨论你在发展过程中如何应对各种挑战，一定要展示你解决问题的能力。

最后，别忘了突出自己的优点。

## 前端技术面试中最常见的问题

前端技术面试问题非常简单和常见。如果你已经从事编码工作至少 6 个月了，你将会熟悉大部分被问到的概念。

一旦你用一种基于时间的方法练习了正确的问题，你应该能够清楚面试。

让我们看看最常见的问题。

## 映射、ForEach、过滤和减少

最常见的问题(通常在面试开始时)是关于`array methods`。面试官想评估你对数组操作的适应程度。

#### `.map()`法

`.map()`方法遍历一个数组，计算您在 map 主体中编写的任何逻辑，并返回一个**新的**数组。

```
let arr = [
  { id: 1, age: 12, name: 'Manu' },
  { id: 2, age: 24, name: 'Quincy' },
  { id: 3, age: 22, name: 'Abbey' },
]

let names = arr.map((el) => el.name)
console.log(names)
// Output: [ 'Manu', 'Quincy', 'Abbey' ]
```

#### `.forEach()`法

ForEach 类似于`.map()`，但它不返回数组。

```
let arr = [
  { id: 1, age: 12, name: 'Manu' },
  { id: 2, age: 24, name: 'Quincy' },
  { id: 3, age: 22, name: 'Abbey' },
]

arr.forEach((el) => el.age+= 10);
console.log(arr);

// Output: 22 32 44
```

#### `.filter()`法

顾名思义，filter 方法有助于根据布尔条件过滤掉数组中的值。

如果布尔条件为真，结果将被返回并添加到最终数组中。如果不是，它将被跳过。Filter 也返回一个数组，就像`.map()`方法一样。

```
let arr = [
  { id: 1, age: 12, name: 'Manu' },
  { id: 2, age: 24, name: 'Quincy' },
  { id: 3, age: 22, name: 'Abbey' },
]

let tooYoung = arr.filter((el) => el.age <= 14);
console.log(tooYoung);

// Output: [ { id: 1, age: 12, name: 'Manu' } ]
```

#### `.reduce()`法

简单来说，`.reduce()`方法考虑了一个`previous value`、当前值和一个`accumulator`。

`.reduce()`方法的返回类型总是单个值。当您想要处理数组中的所有值，并且想要获得一些累积结果时，这是非常有用的。

```
// Calculates the total age of all the three persons.
let arr = [
  { id: 1, age: 12, name: 'Manu' },
  { id: 2, age: 24, name: 'Quincy' },
  { id: 3, age: 22, name: 'Abbey' },
]

let totalAge = arr.reduce((acc, currentObj) => acc + currentObj.age, 0)
console.log(totalAge)

// Output: 57
```

这里，`currentObj`是被迭代的对象。另外，`acc`值存储结果并最终输出到 totalAge 数组中。

## 如何实现聚合填充

另一个重要的面试问题是[如何实现 map 和 filter 数组方法的 polyfills](https://www.algochurn.com/frontend/polyfills) 。

polyfill 是一个代码片段(就 JavaScript web 架构而言),用于未在本地实现的旧浏览器上的现代功能。

简单地说，polyfill 是本地 JavaScript 函数的自定义实现。有点像创建自己的`.map()`或`.filter()`方法。

#### 如何使用`.map()`聚合填料

```
let data = [1, 2, 3, 4, 5];

Array.prototype.myMap = function (cb) {
  let arr = [];
  for (let i = 0; i < this.length; i++) {
    arr.push(cb(this[i], i, this));
  }
  return arr;
};
const mapLog = data.myMap((el) => el * 2);
console.log(mapLog);
```

`myMap`方法接收一个在`myMap`主体内部执行的`callback`。我们基本上在 myMap 主体中有一个本地的`for`循环，它在`this.length`上迭代。这就是调用`myMap`函数的数组的长度。

因为`map()`的语法是`arr.map(currentElement, index, array)`，而`myMap()`函数正是考虑到了这一点。

此外，由于`map()`返回了一个新数组，我们创建了一个空数组并将结果推入其中。最后我们把它退回去。

#### 如何使用`.filter()`聚合填料

```
let data = [1, 2, 3, 4, 5];

Array.prototype.myFilter = function (cb) {
  let arr = [];
  for (let i = 0; i < this.length; i++) {
    if (cb(this[i], i, this)) {
      arr.push(this[i]);
    }
  }
  return arr;
};
const filterLog = data.myFilter((el) => el < 4);
console.log(filterLog);
```

`.filter()`在实现上和`.map()`非常相似。但是由于`filter`基于一个布尔值过滤结果，我们有一个额外的`if()`条件来过滤结果并有条件地推入数组。

## 什么是去抖？

这是一个著名的面试问题，有很多实际的应用和实现。

`Debouncing`是一种防止函数调用过于频繁的方法，而是在调用之前等待一定的时间，直到最后一次调用。

这种情况下想到亚马逊。每当您在搜索栏中键入任何内容，当您停留至少 0.5 秒时，就会获取结果。这正是去抖的意义所在。

为了实现去抖动，让我们举一个例子:根据用户输入为用户生成用户名。

[https://codesandbox.io/embed/proud-surf-uiu2v?file=/src/index.js](https://codesandbox.io/embed/proud-surf-uiu2v?file=/src/index.js)

```
import "./styles.css";
let inputEle = document.getElementById("inputElement");
let username = document.getElementById("username");

let generateUsername = (e) => {
  username.innerHTML = e.target.value.split(" ").join("-");
};
let debounce = function (cb, delay) {
  let timer;
  return function () {
    let context = this;
    clearTimeout(timer);
    timer = setTimeout(() => {
      cb.apply(context, arguments);
    }, delay);
  };
};

inputEle.addEventListener("keyup", debounce(generateUsername, 300));
```

这里，我们试图根据用户输入创建一个自定义用户名。现在，如果用户开始输入，我们不想立即创建它，而是实际上等待 300 毫秒后创建用户名。我们在这里试图模拟一个 API 调用，所以假设用户输入任何东西，它必须对后端进行 API 调用并获取响应。

`debounce()`函数接受两个值，`cb`和`delay`。`cb`是定时器超时时执行的回调函数。

我们使用`setTimeout()`来创建一个超时定时器，这意味着 setTimeout 主体中的函数将在一定时间后被执行。

`apply`方法用于使用最初调用的`object`调用回调函数，并对其应用参数和上下文。

## 什么是闭包？

根据[闭包的 mdn 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)，

> 闭包是捆绑在一起(封闭)的函数与对其周围状态(词法环境)的引用的组合。换句话说，闭包允许您从内部函数访问外部函数的范围。在 JavaScript 中，闭包是在每次创建函数时创建的。

为了简化这一点，让我们举一个例子来理解闭包是如何工作的。

```
function start() {
  var name = "Manu"; // name is a local variable created by start()
  function displayName() {
    // displayName() is the inner function, a `closure`
    alert(name); // use variable declared in the parent function
  }
  displayName();
}
start(); // "Manu" alert box is displayed
```

这里，`start()`和`displayName()`函数之间形成了一个闭包。`displayName()`函数可以访问`start()`函数中的`name`变量。

简单地说，内部函数知道它的周围环境(词汇环境)。

我写了一整篇关于如何清除 JavaScript 面试的博客。如果你想更深入地了解 JavaScript 面试过程，看看这个吧。

## 反应钩

当谈到 React hooks 时，前端编码面试中最常见的问题是:

1.  `useState()`
2.  `useReducer()`
3.  `useEffect()`
4.  `useRef()`
5.  自定义钩子及其实现。

### 挂钩是如何工作的

为了管理组件内部的状态，`useState()`挂钩是您的首选挂钩。

让我们举个例子来理解:

[https://codesandbox.io/embed/thirsty-lewin-uo8ylh?fontsize=14&hidenavigation=1&theme=dark](https://codesandbox.io/embed/thirsty-lewin-uo8ylh?fontsize=14&hidenavigation=1&theme=dark)

```
import { useState } from "react";
import "./styles.css";

export default function App() {
  const [title, setTitle] = useState("freeCodeCamp");
  const handleChange = () => {
    setTitle("FCC");
  };
  return (
    <div className="App">
      <h1>{title} useState</h1>
      <button onClick={handleChange}>Change Title</button>
    </div>
  );
} 
```

`useState()`方法给了我们两个值，变量`state`和状态变量`a function to change`。

在上面的代码片段中，我们创建了一个`title`状态来存储页面的标题。初始状态作为`freeCodeCamp`传递。

单击按钮时，我们可以使用`setTitle()`方法将状态变量更改为`FCC`。

方法是您在功能组件中进行状态管理的首选资源。

### 挂钩是如何工作的

简单来说，`useReducer()`是管理应用程序状态的一种很酷的方式。它更加结构化，有助于维护应用程序中的复杂状态。

让我们举个例子来理解 useReducer 钩子:

[https://codesandbox.io/embed/ecstatic-marco-o8wh00?fontsize=14&hidenavigation=1&theme=dark](https://codesandbox.io/embed/ecstatic-marco-o8wh00?fontsize=14&hidenavigation=1&theme=dark)

```
import "./styles.css";
import { useReducer } from "react";

const initialState = { title: "freeCodeCamp", count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "change-title":
      return { ...state, title: "FCC" };
    case "increment-counter":
      return { ...state, count: state.count + 1 };
    default:
      throw new Error();
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      <div className="App">
        <h1>{state.title} CodeSandbox</h1>
        <button onClick={() => dispatch({ type: "change-title" })}>
          Change Title
        </button>
        <button onClick={() => dispatch({ type: "increment-counter" })}>
          Increment Counter
        </button>
      </div>
      <p style={{ textAlign: "center" }}>{state.count}</p>.
    </>
  );
} 
```

`useReducer()`钩子有两个参数，一个是`reducer`函数，另一个是`initialState`值。

reducer 函数是一个基于`switch-case`的实现，它返回最终状态值，`useReduer()`在内部使用该值来提供给组件。

从`useReducer()`函数返回的值是`state`和`dispatch`。`state`是可以在组件内部使用的实际`state`值。在我们的例子中，状态有两个值:`title and count`。这个标题和计数可以使用由`useReducer()`方法返回的`dispatch()`方法来操作。

在上面的案例中，改一下标题，我们在 reducer 函数内部写了一个`change-title`的案例。这可以在`dispatch({ type: "change-title" })`功能的帮助下触发。这将触发更改标题功能，并改变`title`属性的状态。

同样，应用程序中的`count`部分也会发生同样的情况。正如我之前所说，这是一种在应用程序内部实现状态的很酷的方式。😉

### 挂钩是如何工作的

可以这样想:如果你想让一个状态变量有一个`side effect`改变，你可以使用`useEffect()`钩子来触发它。

比如说你的输入框的`input value`发生了变化，你想在它发生变化后调用一个 API。你可以在`useEffect()`块中写下`API handle`的逻辑。

```
import React, {useState, useEffect} from 'react';

export const App = () => {
    const [value, setValue] = useState('');
    useEffect(() => {
      console.log('value changed: ', value);
    }, [value])
	return <div>
        	<input type="text" name="username" value={value} onChange={(e) => setValue(e.target.value)} />
        </div>
}
```

这里，我们有一个附加了状态值`value`的`input box`。当用户试图输入任何内容时，该值将会改变。

这里，`useEffect()`的一个很好的用例就是实现`API calls`。让我们假设您想用输入字段值调用一个 API。useEffect 功能块将是最好的方法。

另一部分是`dependency array`，它是`useEffect()`钩子的第二个参数。在我们的例子中，我们提到`[value]`作为第二个参数。

这基本上意味着每次`value`改变时，useEffect 中的函数都会被触发。如果您没有在`dependency array`中传递任何东西，该功能块将被触发一次。

### 挂钩是如何工作的

useRef 钩子给了我们改变 DOM 的能力(但这不是 useRef 的唯一含义)。

根据文件:

> useRef 返回一个可变的 Ref 对象。当前属性被初始化为传递的参数(initialValue)。返回的对象将在组件的整个生存期内保持不变。

简单地说，如果我们想在整个组件生命周期中保持某个东西的价值，我们就要使用 useRef。useRef 的基本实现附带了 DOM 元素。让我们举个例子:

```
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

这里，我们给`input`块分配一个`ref`属性。这将与我们创建的`inputEl`引用相关联。

现在这个`input`元素可以按照我们想要的方式进行操作。我们可以修改`style`属性并使它变得漂亮，我们可以使用`value`属性来查看输入元素作为值帮助了什么，等等。

在上面的例子中，当我们点击按钮时，`input`被聚焦，我们可以立即开始输入。我们可以在`inputEl.current.focus()`的帮助下做到这一点——本质上是存在于`current`对象上的`focus()`方法。

### 什么是定制挂钩？

我在前端面试中看到的最常见的问题之一是[为键盘事件](https://www.algochurn.com/frontend/usekeypress-custom-hook)创建一个自定义挂钩。

我们看到了许多不同的挂钩，但面试官可能会要求你创造一个你自己的挂钩。这可能对一些人来说很有挑战性，但是经过一些练习，它会变得容易得多。

我们先来了解一下什么是`Hook`:

自定义钩子的基本用法是将函数的逻辑提取到它自己的组件中。

想象一下如果你不得不`listen for an enter press`进入你的每一个组件会发生什么。不用一遍又一遍地为`listening`编写逻辑，我们可以将逻辑提取到它自己的组件中，并在任何我们想要的地方使用它(就像我们使用`useState()`或`useEffect()`)。

函数被称为`Hook`有几个条件:

1.  它应该总是以关键字`use`开头。
2.  我们可以决定它接受什么作为参数，以及它应该返回什么(如果有的话)。

```
// Custom Hook: useAvailable
function useAvailabe(resource) {
  const [isAvailable, setIsAvailable] = useState(null);

  // ...

  return isAvailable;
}

// Usage:
  const isAvailable = useAvailable(cpu); 
```

在这里，无论我们在自定义钩子内部调用`useState`和`useEffects`多少次，它们都将完全独立于我们使用自定义钩子的函数。

让我们以给`storing local storage values`创建一个自定义钩子为例。

### 如何创建一个自定义钩子 useLocalStorage 示例

useLocalStorage 自定义挂钩是将数据保存到本地存储的一种方式。使用`key`和`value`对在本地存储中获取和设置值，这样无论用户何时回到你的 web 应用，他们都能看到他们之前使用的相同结果。

下面的实现是一个简单的`select`标记值，一旦改变，就将数据保存到本地存储中。

`useLocalStorage.js`

```
// Use Local Storage Custom Hook
import { useState } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    if (typeof window === 'undefined') {
      return initialValue;
    }
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  });
  const setValue = (value) => {
    try {
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      if (typeof window !== 'undefined') {
        window.localStorage.setItem(key, JSON.stringify(valueToStore));
      }
    } catch (error) {
      console.log(error);
    }
  };
  return [storedValue, setValue] as const;
}

export default useLocalStorage; 
```

`App.js`

```
import * as React from 'react';
import './style.css';
import useLocalStorage from './useLocalStorate';

export default function App() {
  const [storedValue, setStoredValue] = useLocalStorage(
    'select-value',
    'light'
  );

  return (
    <div>
      <select
        className="select"
        value={storedValue}
        onChange={(e) => setStoredValue(e.target.value)}
      >
        <option value="dark">Dark</option>
        <option value="light">Light</option>
      </select>
      <p className="desc">
        Value from local storage: <span>{storedValue}</span>
      </p>
    </div>
  );
}
```

这里，`useLocalStorage`钩子接受两个参数，一个是要存储的`local storage key name`，另一个是必须存在的`default`值。

钩子返回两个值:你正在使用的键的`local storage value`和通过给我们一个`setter method`到达`change that key value`的方法。在这种情况下，`setStoredValue`方法。

在`useLocalStorage.js`文件中，我们试图首先使用`localStorage.getItem()`方法将本地存储值与那个键关联起来`GET`。如果存在，我们将设置值。如果找到了，我们`JSON.parse()`这个值并返回它。否则，所提供的 initialValue 将被设置为默认值。

`setLocalStorage()`函数考虑传递的值是函数还是简单的变量值。它还负责使用`localStorage.setItem()`功能设置本地存储的值。

## 如何通过创建辅助项目脱颖而出

一直为我工作并帮助我脱颖而出的是我建立的副业项目。

在我看来，你不必建立 10 个基本的千篇一律的附带项目。相反，尝试构建一两个真正好的项目，在那里你可以实现 React/HTML/CSS/JavaScript 的所有概念以及你所学的一切。

假设面试官一周有 14 次面试，并且要审阅 14 位候选人的简历。他们更有可能对你的个人资料感兴趣，因为你创建了一个`link shortener website that charges $1 after every 1000 link visits`而不是亚马逊/网飞的复制品。

还是那句话，创造克隆体，练习技能，没有错。但是至少有一个独特的项目可以帮助你脱颖而出，这总是好的。此外，创建辅助项目将帮助你提升开发人员的技能。从头开始创建项目时，不可能事先知道所有的事情。在这个过程中，你将不得不学习许多不同的技能，并擅长于此。

## 练习，练习，练习。

有一句名言是这样说的:

> 在你得到第一份前端工作之前，每次面试都是模拟面试。
> 
> ——威廉·莎士比亚。
> 
> — Manu Arora (@mannupaaji) [September 4, 2022](https://twitter.com/mannupaaji/status/1566350128767987712?ref_src=twsrc%5Etfw)