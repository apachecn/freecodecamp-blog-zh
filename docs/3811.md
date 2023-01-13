# 2020 年 React 备忘单(+真实世界示例)

> 原文：<https://www.freecodecamp.org/news/the-react-cheatsheet-for-2020/>

我已经为你整理了一份完整的视觉备忘单，里面有你在 2020 年需要掌握的所有概念和技能。

但是不要让“作弊纸”这个标签欺骗了你。这不仅仅是对 React 特性的总结。

我在这里的目的是清楚而简明地展示我作为一名专业开发人员通过使用 React 获得的知识和模式。

每一部分都旨在通过向您展示真实世界的实际例子和有意义的评论来指导您前进，从而提供极大的帮助。

### 想要你自己的副本吗？？

在这里 抓取 PDF 备忘单 **[(耗时 5 秒)。](https://reedbarger.com/resources/react-cheatsheet-2020/)**

以下是获取可下载版本的一些快速制胜之道:

*   快速参考指南，可随时查看
*   大量可复制的代码片段，便于重复使用
*   在最适合你的地方阅读这份内容丰富的指南。在火车上，在你的办公桌前，排队...任何地方。

> 注意:在本备忘单中，对类组件的介绍有限。对于现有的 React 项目来说，知道类组件仍然是有价值的，但自从 2018 年 Hooks 的到来，我们能够只用函数组件来制作我们的应用程序。我想给初学者和有经验的开发者一个钩子优先的 React 方法。

有很多很棒的东西要介绍，所以让我们开始吧。

## 目录

### 核心概念

*   元素和 JS
*   组件和道具
*   列表和键
*   事件和事件处理程序

### 反应钩

*   状态和使用状态
*   副作用和使用效果
*   性能和使用回调
*   记忆和使用记忆
*   Refs 和 useRef

### 高级挂钩

*   上下文和使用上下文
*   减速器和用户减速器
*   编写自定义挂钩
*   钩子的规则

## 核心概念

### 元素和 JSX

React 元素的基本语法如下:

```
// In a nutshell, JSX allows us to write HTML in our JS
// JSX can use any valid html tags (i.e. div/span, h1-h6, form/input, etc)
<div>Hello React</div> 
```

JSX 元素是表达式:

```
// as an expression, JSX can be assigned to variables...
const greeting = <div>Hello React</div>;

const isNewToReact = true;

// ... or can be displayed conditionally
function sayGreeting() {
  if (isNewToReact) {
    // ... or returned from functions, etc.
    return greeting; // displays: Hello React
  } else {
    return <div>Hi again, React</div>;
  }
} 
```

JSX 允许我们嵌套表达式:

```
const year = 2020;
// we can insert primitive JS values in curly braces: {}
const greeting = <div>Hello React in {year}</div>;
// trying to insert objects will result in an error 
```

JSX 允许我们嵌套元素:

```
// to write JSX on multiple lines, wrap in parentheses: ()
const greeting = (
  // div is the parent element
  <div>
    {/* h1 and p are child elements */}
    <h1>Hello!</h1>
    <p>Welcome to React</p>
  </div>
);
// 'parents' and 'children' are how we describe JSX elements in relation
// to one another, like we would talk about HTML elements 
```

HTML 和 JSX 的语法略有不同:

```
// Empty div is not <div></div> (HTML), but <div/> (JSX)
<div/>

// A single tag element like input is not <input> (HTML), but <input/> (JSX)
<input name="email" />

// Attributes are written in camelcase for JSX (like JS variables
<button className="submit-button">Submit</button> // not 'class' (HTML) 
```

最基本的 React 应用程序需要三样东西:

*   ReactDOM.render()来呈现我们的应用程序
*   JSX 元素(在这个上下文中称为根节点)
*   在其中安装应用程序的 DOM 元素(在 index.html 文件中通常是一个 id 为 root 的 div)

```
// imports needed if using NPM package; not if from CDN links
import React from "react";
import ReactDOM from "react-dom";

const greeting = <h1>Hello React</h1>;

// ReactDOM.render(root node, mounting point)
ReactDOM.render(greeting, document.getElementById("root")); 
```

### 组件和道具

这是基本 React 组件的语法:

```
import React from "react";

// 1st component type: function component
function Header() {
  // function components must be capitalized unlike normal JS functions
  // note the capitalized name here: 'Header'
  return <h1>Hello React</h1>;
}

// function components with arrow functions are also valid
const Header = () => <h1>Hello React</h1>;

// 2nd component type: class component
// (classes are another type of function)
class Header extends React.Component {
  // class components have more boilerplate (with extends and render method)
  render() {
    return <h1>Hello React</h1>;
  }
} 
```

组件的使用方式如下:

```
// do we call these function components like normal functions?

// No, to execute them and display the JSX they return...
const Header = () => <h1>Hello React</h1>;

// ...we use them as 'custom' JSX elements
ReactDOM.render(<Header />, document.getElementById("root"));
// renders: <h1>Hello React</h1> 
```

组件可以在我们的应用中重复使用:

```
// for example, this Header component can be reused in any app page

// this component shown for the '/' route
function IndexPage() {
  return (
    <div>
      <Header />
      <Hero />
      <Footer />
    </div>
  );
}

// shown for the '/about' route
function AboutPage() {
  return (
    <div>
      <Header />
      <About />
      <Testimonials />
      <Footer />
    </div>
  );
} 
```

数据可以通过 props 动态传递给组件:

```
// What if we want to pass data to our component from a parent?
// I.e. to pass a user's name to display in our Header?

const username = "John";

// we add custom 'attributes' called props
ReactDOM.render(
  <Header username={username} />,
  document.getElementById("root")
);
// we called this prop 'username', but can use any valid JS identifier

// props is the object that every component receives as an argument
function Header(props) {
  // the props we make on the component (i.e. username)
  // become properties on the props object
  return <h1>Hello {props.username}</h1>;
} 
```

道具绝对不能直接改动(变异):

```
// Components must ideally be 'pure' functions.
// That is, for every input, we be able to expect the same output

// we cannot do the following with props:
function Header(props) {
  // we cannot mutate the props object, we can only read from it
  props.username = "Doug";

  return <h1>Hello {props.username}</h1>;
}
// But what if we want to modify a prop value that comes in?
// That's where we would use state (see the useState section) 
```

如果我们想将元素/组件作为道具传递给其他组件，子道具是很有用的。

```
// Can we accept React elements (or components) as props?
// Yes, through a special property on the props object called 'children'

function Layout(props) {
  return <div className="container">{props.children}</div>;
}

// The children prop is very useful for when you want the same
// component (such as a Layout component) to wrap all other components:
function IndexPage() {
  return (
    <Layout>
      <Header />
      <Hero />
      <Footer />
    </Layout>
  );
}

// different page, but uses same Layout component (thanks to children prop)
function AboutPage() {
  return (
    <Layout>
      <About />
      <Footer />
    </Layout>
  );
} 
```

有条件地显示带有端子和短路的元件:

```
// if-statements are fine to conditionally show , however...
// ...only ternaries (seen below) allow us to insert these conditionals
// in JSX, however
function Header() {
  const isAuthenticated = checkAuth();

  return (
    <nav>
      <Logo />
      {/* if isAuth is true, show AuthLinks. If false, Login  */}
      {isAuthenticated ? <AuthLinks /> : <Login />}
      {/* if isAuth is true, show Greeting. If false, nothing. */}
      {isAuthenticated && <Greeting />}
    </nav>
  );
} 
```

片段是特殊的组件，用于显示多个组件，而无需向 DOM 添加额外的元素。

片段是条件逻辑的理想选择:

```
// we can improve the logic in the previous example
// if isAuthenticated is true, how do we display both AuthLinks and Greeting?
function Header() {
  const isAuthenticated = checkAuth();

  return (
    <nav>
      <Logo />
      {/* we can render both components with a fragment */}
      {/* fragments are very concise: <> </> */}
      {isAuthenticated ? (
        <>
          <AuthLinks />
          <Greeting />
        </>
      ) : (
        <Login />
      )}
    </nav>
  );
} 
```

### 列表和键

使用。map()将数据列表(数组)转换为元素列表:

```
const people = ["John", "Bob", "Fred"];
const peopleList = people.map(person => <p>{person}</p>); 
```

。map()也用于组件和元素:

```
function App() {
  const people = ['John', 'Bob', 'Fred'];
  // can interpolate returned list of elements in {}
  return (
    <ul>
      {/* we're passing each array element as props */}
      {people.map(person => <Person name={person} />}
    </ul>
  );
}

function Person({ name }) {
  // gets 'name' prop using object destructuring
  return <p>this person's name is: {name}</p>;
} 
```

迭代的每个 React 元素都需要一个特殊的“key”属性。对于 React 来说，键是很重要的，它能够跟踪每个被 map 迭代的元素

如果没有键，it 部门就很难弄清楚当数据发生变化时元素应该如何更新。

键应该是唯一的值，以表示这些元素相互独立的事实。

```
function App() {
  const people = ['John', 'Bob', 'Fred'];

  return (
    <ul>
      {/* keys need to be primitive values, ideally a generated id */}
      {people.map(person => <Person key={person} name={person} />)}
    </ul>
  );
}

// If you don't have ids with your set of data or unique primitive values,
// you can use the second parameter of .map() to get each elements index
function App() {
  const people = ['John', 'Bob', 'Fred'];

  return (
    <ul>
      {/* use array element index for key */}
      {people.map((person, i) => <Person key={i} name={person} />)}
    </ul>
  );
} 
```

### 事件和事件处理程序

React 和 HTML 中的事件略有不同。

```
// Note: most event handler functions start with 'handle'
function handleToggleTheme() {
  // code to toggle app theme
}

// in html, onclick is all lowercase
<button onclick="handleToggleTheme()">
  Submit
</button>

// in JSX, onClick is camelcase, like attributes / props
// we also pass a reference to the function with curly braces
<button onClick={handleToggleTheme}>
  Submit
</button> 
```

需要了解的最基本的反应事件是 onClick 和 onChange。

*   onClick 处理 JSX 元素(即按钮)上的点击事件
*   onChange 处理键盘事件(即输入)

```
function App() {
  function handleChange(event) {
    // when passing the function to an event handler, like onChange
    // we get access to data about the event (an object)
    const inputText = event.target.value;
    const inputName = event.target.name; // myInput
    // we get the text typed in and other data from event.target
  }

  function handleSubmit() {
    // on click doesn't usually need event data
  }

  return (
    <div>
      <input type="text" name="myInput" onChange={handleChange} />
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
} 
```

## 反应钩

### 状态和使用状态

useState 为我们提供了函数组件中的本地状态:

```
import React from 'react';

// create state variable
// syntax: const [stateVariable] = React.useState(defaultValue);
function App() {
  const [language] = React.useState('javascript');
  // we use array destructuring to declare state variable

  return <div>I am learning {language}</div>;
} 
```

注意:本节中的任何钩子都来自 React 包，可以单独导入。

```
import React, { useState } from "react";

function App() {
  const [language] = useState("javascript");

  return <div>I am learning {language}</div>;
} 
```

useState 还为我们提供了一个“setter”函数来更新它创建的状态:

```
function App() {
  // the setter function is always the second destructured value
  const [language, setLanguage] = React.useState("python");
  // the convention for the setter name is 'setStateVariable'

  return (
    <div>
      {/*  why use an arrow function here instead onClick={setterFn()} ? */}
      <button onClick={() => setLanguage("javascript")}>
        Change language to JS
      </button>
      {/*  if not, setLanguage would be called immediately and not on click */}
      <p>I am now learning {language}</p>
    </div>
  );
}

// note that whenever the setter function is called, the state updates,
// and the App component re-renders to display the new state 
```

useState 可以在单个组件中使用一次或多次:

```
function App() {
  const [language, setLanguage] = React.useState("python");
  const [yearsExperience, setYearsExperience] = React.useState(0);

  return (
    <div>
      <button onClick={() => setLanguage("javascript")}>
        Change language to JS
      </button>
      <input
        type="number"
        value={yearsExperience}
        onChange={event => setYearsExperience(event.target.value)}
      />
      <p>I am now learning {language}</p>
      <p>I have {yearsExperience} years of experience</p>
    </div>
  );
} 
```

useState 可以接受原语或对象值来管理状态:

```
// we have the option to organize state using whatever is the
// most appropriate data type, according to the data we're tracking
function App() {
  const [developer, setDeveloper] = React.useState({
    language: "",
    yearsExperience: 0
  });

  function handleChangeYearsExperience(event) {
    const years = event.target.value;
    // we must pass in the previous state object we had with the spread operator
    setDeveloper({ ...developer, yearsExperience: years });
  }

  return (
    <div>
      {/* no need to get prev state here; we are replacing the entire object */}
      <button
        onClick={() =>
          setDeveloper({
            language: "javascript",
            yearsExperience: 0
          })
        }
      >
        Change language to JS
      </button>
      {/* we can also pass a reference to the function */}
      <input
        type="number"
        value={developer.yearsExperience}
        onChange={handleChangeYearsExperience}
      />
      <p>I am now learning {developer.language}</p>
      <p>I have {developer.yearsExperience} years of experience</p>
    </div>
  );
} 
```

如果新状态依赖于以前的状态，为了保证更新可靠地完成，我们可以在 setter 函数中使用一个函数，它给出正确的以前状态。

```
function App() {
  const [developer, setDeveloper] = React.useState({
    language: "",
    yearsExperience: 0,
    isEmployed: false
  });

  function handleToggleEmployment(event) {
    // we get the previous state variable's value in the parameters
    // we can name 'prevState' however we like
    setDeveloper(prevState => {
      return { ...prevState, isEmployed: !prevState.isEmployed };
      // it is essential to return the new state from this function
    });
  }

  return (
    <button onClick={handleToggleEmployment}>Toggle Employment Status</button>
  );
} 
```

### 副作用和使用效果

useEffect 让我们在函数组件中执行副作用。那么副作用是什么呢？

*   副作用是我们需要接触外部世界的地方。例如，从 API 获取数据或使用 DOM。
*   副作用是能够以不可预测的方式改变我们的组件状态的行为(已经导致了“副作用”)。

useEffect 接受一个回调函数(称为“Effect”函数)，默认情况下，每次重新渲染时都会运行该函数。它在我们的组件挂载后运行，这是在组件生命周期中执行副作用的恰当时机。

```
// what does our code do? Picks a color from the colors array
// and makes it the background color
function App() {
  const [colorIndex, setColorIndex] = React.useState(0);
  const colors = ["blue", "green", "red", "orange"];

  // we are performing a 'side effect' since we are working with an API
  // we are working with the DOM, a browser API outside of React
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  });
  // whenever state is updated, App re-renders and useEffect runs

  function handleChangeIndex() {
    const next = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(next);
  }

  return <button onClick={handleChangeIndex}>Change background color</button>;
} 
```

为了避免在每次渲染后执行效果回调，我们提供了第二个参数，一个空数组:

```
function App() {
  ...
  // now our button doesn't work no matter how many times we click it...
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  }, []);
  // the background color is only set once, upon mount

  // how do we not have the effect function run for every state update...
  // but still have it work whenever the button is clicked?

  return (
    <button onClick={handleChangeIndex}>
      Change background color
    </button>
  );
} 
```

useEffect 让我们有条件地使用依赖数组来执行效果。

dependencies 数组是第二个参数，如果数组中的任何一个值发生变化，effect 函数将再次运行。

```
function App() {
  const [colorIndex, setColorIndex] = React.useState(0);
  const colors = ["blue", "green", "red", "orange"];

  // we add colorIndex to our dependencies array
  // when colorIndex changes, useEffect will execute the effect fn again
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
    // when we use useEffect, we must think about what state values
    // we want our side effect to sync with
  }, [colorIndex]);

  function handleChangeIndex() {
    const next = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(next);
  }

  return <button onClick={handleChangeIndex}>Change background color</button>;
} 
```

useEffect 让我们通过在末尾返回一个函数来取消订阅某些效果:

```
function MouseTracker() {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });

  React.useEffect(() => {
    // .addEventListener() sets up an active listener...
    window.addEventListener("mousemove", event => {
      const { pageX, pageY } = event;
      setMousePosition({ x: pageX, y: pageY });
    });

    // ...so when we navigate away from this page, it needs to be
    // removed to stop listening. Otherwise, it will try to set
    // state in a component that doesn't exist (causing an error)

    // We unsubscribe any subscriptions / listeners w/ this 'cleanup function'
    return () => {
      window.removeEventListener("mousemove", event => {
        const { pageX, pageY } = event;
        setMousePosition({ x: pageX, y: pageY });
      });
    };
  }, []);

  return (
    <div>
      <h1>The current mouse position is:</h1>
      <p>
        X: {mousePosition.x}, Y: {mousePosition.y}
      </p>
    </div>
  );
}

// Note: we could extract the reused logic in the callbacks to
// their own function, but I believe this is more readable 
```

*   使用 useEffect 提取数据

注意，用更简洁的 async/await 语法处理承诺需要创建一个单独的函数。(为什么？效果回调函数不能是异步的。)

```
const endpoint = "https://api.github.com/users/codeartistryio";

// with promises:
function App() {
  const [user, setUser] = React.useState(null);

  React.useEffect(() => {
    // promises work in callback
    fetch(endpoint)
      .then(response => response.json())
      .then(data => setUser(data));
  }, []);
}

// with async / await syntax for promise:
function App() {
  const [user, setUser] = React.useState(null);
  // cannot make useEffect callback function async
  React.useEffect(() => {
    getUser();
  }, []);

  // instead, use async / await in separate function, then call
  // function back in useEffect
  async function getUser() {
    const response = await fetch("https://api.github.com/codeartistryio");
    const data = await response.json();
    setUser(data);
  }
} 
```

### 性能和使用回调

useCallback 是一个钩子，用于提高组件的性能。

如果您有一个经常重新呈现的组件，useCallback 防止组件内的回调函数在每次组件重新呈现时被重新创建(这意味着函数组件重新运行)。

useCallback 仅在其中一个依赖项发生变化时重新运行。

```
// in Timer, we are calculating the date and putting it in state a lot
// this results in a re-render for every state update

// we had a function handleIncrementCount to increment the state 'count'...
function Timer() {
  const [time, setTime] = React.useState();
  const [count, setCount] = React.useState(0);

  // ... but unless we wrap it in useCallback, the function is
  // recreated for every single re-render (bad performance hit)
  // useCallback hook returns a callback that isn't recreated every time
  const inc = React.useCallback(
    function handleIncrementCount() {
      setCount(prevCount => prevCount + 1);
    },
    // useCallback accepts a second arg of a dependencies array like useEffect
    // useCallback will only run if any dependency changes (here it's 'setCount')
    [setCount]
  );

  React.useEffect(() => {
    const timeout = setTimeout(() => {
      const currentTime = JSON.stringify(new Date(Date.now()));
      setTime(currentTime);
    }, 300);

    return () => {
      clearTimeout(timeout);
    };
  }, [time]);

  return (
    <div>
      <p>The current time is: {time}</p>
      <p>Count: {count}</p>
      <button onClick={inc}>+</button>
    </div>
  );
} 
```

### 记忆和使用记忆

useMemo 与 useCallback 非常相似，都是为了提高性能。但是它不是用于回调，而是用于存储昂贵计算的结果。

useMemo 允许我们“记忆”,或者在已经为某些输入进行了昂贵的计算时记住它们的结果(我们已经为这些值做过一次，所以再做一次并不新鲜)。

useMemo 从计算中返回一个值，不是一个回调函数(但可以是一个函数)。

```
// useMemo is useful when we need a lot of computing resources
// to perform an operation, but don't want to repeat it on each re-render

function App() {
  // state to select a word in 'words' array below
  const [wordIndex, setWordIndex] = useState(0);
  // state for counter
  const [count, setCount] = useState(0);

  // words we'll use to calculate letter count
  const words = ["i", "am", "learning", "react"];
  const word = words[wordIndex];

  function getLetterCount(word) {
    // we mimic expensive calculation with a very long (unnecessary) loop
    let i = 0;
    while (i < 1000000) i++;
    return word.length;
  }

  // Memoize expensive function to return previous value if input was the same
  // only perform calculation if new word without a cached value
  const letterCount = React.useMemo(() => getLetterCount(word), [word]);

  // if calculation was done without useMemo, like so:

  // const letterCount = getLetterCount(word);

  // there would be a delay in updating the counter
  // we would have to wait for the expensive function to finish

  function handleChangeIndex() {
    // flip from one word in the array to the next
    const next = wordIndex + 1 === words.length ? 0 : wordIndex + 1;
    setWordIndex(next);
  }

  return (
    <div>
      <p>
        {word} has {letterCount} letters
      </p>
      <button onClick={handleChangeIndex}>Next word</button>
      <p>Counter: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
} 
```

### Refs 和 useRef

引用是一种特殊属性，可用于所有 React 组件。它们允许我们在组件挂载时创建对给定元素/组件的引用

useRef 允许我们轻松地使用 React refs。我们调用 useRef(在组件的顶部)并将返回值附加到元素的 Ref 属性来引用它。

一旦我们创建了一个引用，我们就使用当前的属性来修改(改变)元素的属性。或者我们可以调用该元素上任何可用的方法(比如。focus()聚焦一个输入)。

```
function App() {
  const [query, setQuery] = React.useState("react hooks");
  // we can pass useRef a default value
  // we don't need it here, so we pass in null to ref an empty object
  const searchInput = useRef(null);

  function handleClearSearch() {
    // current references the text input once App mounts
    searchInput.current.value = "";
    // useRef can store basically any value in its .current property
    searchInput.current.focus();
  }

  return (
    <form>
      <input
        type="text"
        onChange={event => setQuery(event.target.value)}
        ref={searchInput}
      />
      <button type="submit">Search</button>
      <button type="button" onClick={handleClearSearch}>
        Clear
      </button>
    </form>
  );
} 
```

## 高级挂钩

### 上下文和使用上下文

在 React 中，我们希望避免以下问题，即创建多个属性来从父组件向下传递两个或更多级别的数据:

```
// Context helps us avoid creating multiple duplicate props
// This pattern is also called props drilling:
function App() {
  // we want to pass user data down to Header
  const [user] = React.useState({ name: "Fred" });

  return (
   {/* first 'user' prop */}
    <Main user={user} />
  );
}

const Main = ({ user }) => (
  <>
    {/* second 'user' prop */}
    <Header user={user} />
    <div>Main app content...</div>
  </>
);

const Header = ({ user }) => <header>Welcome, {user.name}!</header>; 
```

上下文有助于从父组件向下传递多个级别的子组件。

```
// Here is the previous example rewritten with Context
// First we create context, where we can pass in default values
const UserContext = React.createContext();
// we call this 'UserContext' because that's what data we're passing down

function App() {
  // we want to pass user data down to Header
  const [user] = React.useState({ name: "Fred" });

  return (
    {/* we wrap the parent component with the provider property */}
    {/* we pass data down the computer tree w/ value prop */}
    <UserContext.Provider value={user}>
      <Main />
    </UserContext.Provider>
  );
}

const Main = () => (
  <>
    <Header />
    <div>Main app content...</div>
  </>
);

// we can remove the two 'user' props, we can just use consumer
// to consume the data where we need it
const Header = () => (
  {/* we use this pattern called render props to get access to the data*/}
  <UserContext.Consumer>
    {user => <header>Welcome, {user.name}!</header>}
  </UserContext.Consumer>
); 
```

然而，useContext 挂钩可以移除这种看起来不寻常的呈现道具模式，以便能够在我们喜欢的任何函数组件中使用上下文:

```
const Header = () => {
  // we pass in the entire context object to consume it
  const user = React.useContext(UserContext);
  // and we can remove the Consumer tags
  return <header>Welcome, {user.name}!</header>;
}; 
```

### 减速器和用户减速器

Reducers 是简单的、可预测的(纯)函数，它接受一个先前的状态对象和一个动作对象，并返回一个新的状态对象。例如:

```
// let's say this reducer manages user state in our app:
function reducer(state, action) {
  // reducers often use a switch statement to update state
  // in one way or another based on the action's type property
  switch (action.type) {
    // if action.type has the string 'LOGIN' on it
    case "LOGIN":
      // we get data from the payload object on action
      return { username: action.payload.username, isAuth: true };
    case "SIGNOUT":
      return { username: "", isAuth: false };
    default:
      // if no case matches, return previous state
      return state;
  }
} 
```

Reducers 是一种强大的状态管理模式，用于流行的状态管理库 Redux(通常与 React 一起使用)。

与 useState(用于本地组件状态)相比，Reducer 可以在 React 中与 useReducer 挂钩一起使用，以便管理整个应用程序的状态。

useReducer 可以与 useContext 成对使用，以管理数据并轻松地在组件之间传递数据。

因此，useReducer + useContext 可以是我们应用程序的整个状态管理系统。

```
const initialState = { username: "", isAuth: false };

function reducer(state, action) {
  switch (action.type) {
    case "LOGIN":
      return { username: action.payload.username, isAuth: true };
    case "SIGNOUT":
      // could also spread in initialState here
      return { username: "", isAuth: false };
    default:
      return state;
  }
}

function App() {
  // useReducer requires a reducer function to use and an initialState
  const [state, dispatch] = useReducer(reducer, initialState);
  // we get the current result of the reducer on 'state'

  // we use dispatch to 'dispatch' actions, to run our reducer
  // with the data it needs (the action object)
  function handleLogin() {
    dispatch({ type: "LOGIN", payload: { username: "Ted" } });
  }

  function handleSignout() {
    dispatch({ type: "SIGNOUT" });
  }

  return (
    <>
      Current user: {state.username}, isAuthenticated: {state.isAuth}
      <button onClick={handleLogin}>Login</button>
      <button onClick={handleSignout}>Signout</button>
    </>
  );
} 
```

### 编写自定义挂钩

钩子的创建是为了方便地重用组件之间的行为。

对于类组件，比如高阶组件或渲染道具，它们比以前的模式更容易理解

最棒的是，除了我们已经介绍过的 React 提供的挂钩之外，我们还可以根据自己的项目需求创建自己的挂钩:

```
// here's a custom hook that is used to fetch data from an API
function useAPI(endpoint) {
  const [value, setValue] = React.useState([]);

  React.useEffect(() => {
    getData();
  }, []);

  async function getData() {
    const response = await fetch(endpoint);
    const data = await response.json();
    setValue(data);
  };

  return value;
};

// this is a working example! try it yourself (i.e. in codesandbox.io)
function App() {
  const todos = useAPI("https://todos-dsequjaojf.now.sh/todos");

  return (
    <ul>
      {todos.map(todo => <li key={todo.id}>{todo.text}</li>}
    </ul>
  );
} 
```

### 钩子的规则

使用 React 挂钩有两个核心规则，我们不能违反它们才能正常工作:

1.  钩子只能在组件的顶部被调用(它们不能在条件、循环或嵌套函数中)
2.  钩子只能在函数组件中使用(它们不能在普通的 JavaScript 函数或类组件中)

```
function checkAuth() {
  // Rule 2 Violated! Hooks cannot be used in normal functions, only components
  React.useEffect(() => {
    getUser();
  }, []);
}

function App() {
  // this is the only validly executed hook in this component
  const [user, setUser] = React.useState(null);

  // Rule 1 violated! Hooks cannot be used within conditionals (or loops)
  if (!user) {
    React.useEffect(() => {
      setUser({ isAuth: false });
      // if you want to conditionally execute an effect, use the
      // dependencies array for useEffect
    }, []);
  }

  checkAuth();

  // Rule 1 violated! Hooks cannot be used in nested functions
  return <div onClick={() => React.useMemo(() => doStuff(), [])}>Our app</div>;
} 
```

## 下一步是什么

还有许多其他的 React 概念需要学习，但我相信这些是你必须在任何其他人之前知道的，以便在 2020 年让你走上 React 掌握的道路。

想要快速参考所有这些概念吗？

点击此处下载所有这些信息的完整 PDF 备忘单[。](https://reedbarger.com/resources/react-cheatsheet-2020/)

继续编写代码，我将在下一篇文章中介绍您！

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*