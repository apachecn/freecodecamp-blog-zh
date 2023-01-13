# 2021‬的 React 备忘单(+真实世界示例)

> 原文：<https://www.freecodecamp.org/news/react-cheatsheet-with-real-world-examples/>

我已经整理了一个全面的视觉备忘单，来帮助你在 2021 年掌握 React 库的所有主要概念和特性。

我创建了这个备忘单来帮助你在最短的时间内优化你的反应学习。

它包括大量的实际例子来说明这个库的每个特性，以及它是如何使用您可以在自己的项目中应用的模式来工作的。

除了每个代码片段，我还添加了许多有用的注释。如果您阅读了这些注释，您将会看到每一行代码是做什么的，不同的概念是如何相互关联的，并对 React 的使用有一个更完整的理解。

请注意，对于 React 开发人员来说特别有用的关键词会以粗体突出显示，所以请留意这些关键词。

### 想要一份自己的备忘单吗？‬

[**在此下载 PDF 格式的备忘单**](http://bit.ly/react-sheet-2021) (耗时 5 秒)。

以下是获取可下载版本的一些快速制胜之道:

*   快速参考指南，可随时查看
*   大量可复制的代码片段，便于重复使用
*   在最适合你的地方阅读这份内容丰富的指南。在火车上，在你的办公桌前，排队...任何地方。

有很多很棒的东西要介绍，所以让我们开始吧。

> 想运行下面的代码片段吗？创建一个新的 React 应用程序，使用(免费)在线工具 CodeSandbox 来尝试这些示例。您可以通过访问 [react.new](https://react.new) 立即这样做。

## 目录

### 反应基础

*   [JSX 元素](#jsx-elements)
*   [组件和道具](#components-and-props)
*   [列表和键](#lists-and-keys)
*   [事件监听器和处理事件](#event-listeners-and-handling-events)

### 基本反应挂钩

*   [状态和使用状态](#state-and-usestate)
*   [副作用和使用效果](#side-effects-and-useeffect)
*   [Refs 和 useRef](#refs-and-useref)

### 挂钩和性能

*   [防止重新渲染和反应备忘录](#preventing-re-renders-and-react-memo)
*   [回调函数和使用回调](#callback-functions-and-usecallback)
*   [记忆和使用备忘录](#memoization-and-usememo)

### 高级反应挂钩

*   [上下文和使用上下文](#context-and-usecontext)
*   [减速器和用户减速器](#reducers-and-usereducer)
*   [编写自定义钩子](#writing-custom-hooks)
*   [挂钩规则](#rules-of-hooks)

## 反应基础

### JSX 元素

React 应用程序是使用一种叫做 **JSX** 的语法构建的。这是一个基本 **JSX 元素**的语法。

```
/* 
  JSX allows us to write in a syntax almost identical to plain HTML.
  As a result, JSX is a powerful tool to structure our applications.
  JSX uses all valid HTML tags (i.e. div/span, h1-h6, form/input, img, etc).
*/

<div>Hello React!</div>

/* 
  Note: this JSX would not be visible because it needs to be rendered by our application using ReactDOM.render() 
*/
```

JSX 是构造 React 应用程序最常见的方式，但它不是 React 所必需的。

```
/* JSX is a simpler way to use the function React.createElement()
In other words, the following two lines in React are the same: */

<div>Hello React!</div>  // JSX syntax

React.createElement('div', null, 'Hello React!'); // createElement syntax
```

浏览器不理解 JSX。需要编译成浏览器能理解的普通 JavaScript。

JSX 最常用的编译器叫做巴别塔。

```
/* 
  When our project is built to run in the browser, our JSX will be converted by Babel into simple React.createElement() function calls. 
  From this... 
*/
const greeting = <div>Hello React!</div>;

/* ...into this: */
"use strict";

const greeting = /*#__PURE__*/React.createElement("div", null, "Hello React!");
```

JSX 在几个重要方面不同于 HTML:

```
/* 
  We can write JSX like plain HTML, but it's actually made using JavaScript functions.
  Because JSX is JavaScript, not HTML, there are some differences:

  1) Some JSX attributes are named differently than HTML attributes. Why? Because some attribute words are reserved words in JavaScript, such as 'class'. Instead of class, JSX uses 'className'.

  Also, because JSX is JavaScript, attributes that consist of multiple words are written in camelcase:
*/

<div id="header">
  <h1 className="title">Hello React!</h1>
</div>

/* 
  2) JSX elements that consist of only a single tag (i.e. input, img, br elements) must be closed with a trailing forward slash to be valid (/): 
*/

<input type="email" /> // <input type="email"> is a syntax error

/* 
  3) JSX elements that consist of an opening and closing tag (i.e. div, span, button element), must have both or be closed with a trailing forward slash. Like 2), it is a syntax error to have an unterminated element. 
*/

<button>Click me</button> // <button> or </button> is a syntax error
<button /> // empty, but also valid
```

可以使用 style 属性将内联样式添加到 JSX 元素中。和样式是在一个对象中更新的，而不是像 HTML 那样在一组双引号中更新。

请注意，样式属性名称也必须用 camelcase 编写。

```
/* 
  Properties that accept pixel values (like width, height, padding, margin, etc), can use integers instead of strings.
  For example: fontSize: 22\. Instead of: fontSize: "22px"
*/
<h1 style={{ color: 'blue', fontSize: 22, padding: '0.5em 1em' }}>
  Hello React!
</h1>
```

JSX 元素是 JavaScript 表达式，可以这样使用。JSX 直接在我们的用户界面中为我们提供了 JavaScript 的全部功能。

```
/* 
  JSX elements are expressions (resolve to a value) and therefore can be assigned to plain JavaScript variables... 
*/
const greeting = <div>Hello React!</div>;

const isNewToReact = true;

// ... or can be displayed conditionally
function sayGreeting() {
  if (isNewToReact) {
    // ... or returned from functions, etc.
    return greeting; // displays: Hello React!
  } else {
    return <div>Hi again, React</div>;
  }
}
```

JSX 允许我们使用花括号语法插入(或嵌入)简单的 JavaScript 表达式:

```
const year = 2021;

/* We can insert primitive JS values (i.e. strings, numbers, booleans) in curly braces: {} */
const greeting = <div>Hello React in {year}</div>;

/* We can also insert expressions that resolve to a primitive value: */
const goodbye = <div>Goodbye previous year: {year - 1}</div>

/* Expressions can also be used for element attributes */
const className = 'title';
const title = <h1 className={className}>My title</h1>

/* Note: trying to insert object values (i.e. objects, arrays, maps) in curly braces will result in an error */
```

JSX 允许我们嵌套元素，就像 HTML 一样。

```
/* 
  To write JSX on multiple lines, wrap in parentheses: ()
  JSX expressions that span multiple lines are called multiline expressions
*/

const greeting = (
  // div is the parent element
  <div>
    {/* h1 and p are child elements */}
    <h1>Hello!</h1>
    <p>Welcome to React</p>
  </div>
);
/* 'parents' and 'children' are how we describe JSX elements in relation
to one another, like we would talk about HTML elements */
```

JSX 中的注释被写成多行 JavaScript 注释，写在花括号之间，像这样:

```
const greeting = (
  <div>
    {/* This is a single line comment */}
  	<h1>Hello!</div>
	<p>Welcome to React</p>
    {/* This is a 
      multiline
      comment */} 
  </div>
);
```

所有 React 应用都需要三样东西:

1.  用于将我们的应用程序装载到一个 HTML 元素上来呈现(显示)
2.  JSX 元素:称为“根节点”，因为它是我们应用程序的根。也就是说，渲染它将会渲染其中的所有子元素
3.  HTML (DOM)元素:应用程序插入 HTML 页面的位置。该元素通常是一个 id 为“root”的 div，位于 index.html 文件中。

```
// Packages can be installed locally or brought in through a CDN link (added to head of HTML document) 
import React from "react";
import ReactDOM from "react-dom";

// root node (usually a component) is most often called "App"
const App = <h1>Hello React!</h1>;

// ReactDOM.render(root node, HTML element)
ReactDOM.render(App, document.getElementById("root"));
```

### 组件和道具

JSX 可以在称为**组件**的单个功能内组合在一起。

React 中有两种类型的组件:**功能组件**和**类组件**。

函数或类组件的组件名是大写的，以区别于不返回 JSX 的普通 JavaScript 函数:

```
import React from "react";

/* 	
  Function component
  Note the capitalized function name: 'Header', not 'header'
*/
function Header() {
  return <h1>Hello React</h1>;
}

// Function components which use an arrow function syntax are also valid
const Header = () => <h1>Hello React</h1>;

/* 
  Class component
  Class components have more boilerplate (note the 'extends' keyword and 'render' method)
*/
class Header extends React.Component {
  render() {
    return <h1>Hello React</h1>;
  }
}
```

组件尽管是函数，但并不像普通的 JavaScript 函数那样被调用。它们是通过渲染来执行的，就像我们在应用程序中渲染 JSX 一样。

```
// Do we call this function component like a normal function?

// No, to execute them and display the JSX they return...
const Header = () => <h1>Hello React</h1>;

// ...we use them as 'custom' JSX elements
ReactDOM.render(<Header />, document.getElementById("root"));
// renders: <h1>Hello React</h1>
```

组件的巨大好处是它们能够在我们的应用中重用，无论我们需要它们在哪里。

由于组件利用了 JavaScript 函数的强大功能，我们可以逻辑地向它们传递数据，就像传递一个或多个参数一样。

```
/* 
  The Header and Footer components can be reused in any page in our app.
  Components remove the need to rewrite the same JSX multiple times.
*/

// IndexPage component, visible on '/' route of our app
function IndexPage() {
  return (
    <div>
      <Header />
      <Hero />
      <Footer />
    </div>
  );
}

// AboutPage component, visible on the '/about' route
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

JavaScript 中传递给组件的数据被称为 **props** 。属性看起来与普通 JSX/HTML 元素的属性一样，但是您可以在组件本身中访问它们的值。

属性在它们被传递到的组件的参数中可用。道具总是作为对象的属性包含在内。

```
/* 
  What if we want to pass custom data to our component from a parent component?
  For example, to display the user's name in our app header.
*/

const username = "John";

/* 
  To do so, we add custom 'attributes' to our component called props.
  We can add many of them as we like and we give them names that suit the data we pass in.
  To pass the user's name to the header, we use a prop we appropriately called 'username'
*/
ReactDOM.render(
  <Header username={username} />,
  document.getElementById("root")
);
// We called this prop 'username', but can use any valid identifier we would give, for example, a JavaScript variable

// props is the object that every component receives as an argument
function Header(props) {
  // the props we make on the component (username)
  // become properties on the props object
  return <h1>Hello {props.username}</h1>;
}
```

决不能在子组件中直接更改属性。

另一种说法是，道具永远不应该是**变异的**，因为道具是一个普通的 JavaScript 对象。

```
/* 
  Components should operate as 'pure' functions.
  That is, for every input, we should be able to expect the same output.
  This means we cannot mutate the props object, only read from it.
*/

// We cannot modify the props object :
function Header(props) {
  props.username = "Doug";

  return <h1>Hello {props.username}</h1>;
}
/* 
  But what if we want to modify a prop value that is passed to our component?
  That's where we would use state (see the useState section).
*/
```

如果我们想将元素/组件作为道具传递给其他组件，那么 **children** 道具非常有用。

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

同样，由于组件是 JavaScript 表达式，我们可以将它们与 if-else 语句和 switch 语句结合使用，以有条件地显示内容，如下所示:

```
function Header() {
  const isAuthenticated = checkAuth();

  /* if user is authenticated, show the authenticated app, otherwise, the unauthenticated app */
  if (isAuthenticated) {
    return <AuthenticatedApp />   
  } else {
    /* alternatively, we can drop the else section and provide a simple return, and the conditional will operate in the same way */
    return <UnAuthenticatedApp />   
  }
}
```

要在组件返回的 JSX 中使用条件，可以使用三元运算符 or short-circuiting (&&和||运算符)。

```
function Header() {
  const isAuthenticated = checkAuth();

  return (
    <nav>
      {/* if isAuth is true, show nothing. If false, show Logo  */}
      {isAuthenticated || <Logo />}
      {/* if isAuth is true, show AuthenticatedApp. If false, show Login  */}
      {isAuthenticated ? <AuthenticatedApp /> : <LoginScreen />}
      {/* if isAuth is true, show Footer. If false, show nothing */}
      {isAuthenticated && <Footer />}
    </nav>
  );
}
```

**片段**是特殊的组件，用于显示多个组件，而无需向 DOM 添加额外的元素。它们是具有多个相邻组件或元素的条件逻辑的理想选择。

```
/*
  We can improve the logic in the previous example.
  If isAuthenticated is true, how do we display both the AuthenticatedApp and Footer components?
*/
function Header() {
  const isAuthenticated = checkAuth();

  return (
    <nav>
      <Logo />
      {/* 
        We can render both components with a fragment. 
        Fragments are very concise: <> </>
      */}
      {isAuthenticated ? (
        <>
          <AuthenticatedApp />
          <Footer />
        </>
      ) : (
        <Login />
      )}
    </nav>
  );
}
/* 
  Note: An alternate syntax for fragments is React.Fragment:
  <React.Fragment>
     <AuthenticatedApp />
     <Footer />
  </React.Fragment>
*/
```

### 列表和键

使用**。map()** 函数将数据列表(数组)转换成元素列表。

```
const people = ["John", "Bob", "Fred"];
const peopleList = people.map(person => <p>{person}</p>); 
```

`.map()`可用于组件以及普通 JSX 元素。

```
function App() {
  const people = ['John', 'Bob', 'Fred'];
  // can interpolate returned list of elements in {}
  return (
    <ul>
      {/* we're passing each array element as props to Person */}
      {people.map(person => <Person name={person} />}
    </ul>
  );
}

function Person({ name }) {
  // we access the 'name' prop directly using object destructuring
  return <p>This person's name is: {name}</p>;
}
```

元素列表中的每个 React 元素都需要一个特殊的**键道具**。对于 React 来说，关键是能够跟踪用`.map()`函数迭代的每个元素。

React 使用键在数据改变时高效地更新单个元素(而不是重新呈现整个列表)。

键需要有唯一的值，以便能够根据键值来识别每个键。

```
function App() {
  const people = [
    { id: 'Ksy7py', name: 'John' },
    { id: '6eAdl9', name: 'Bob' },
    { id: '6eAdl9', name: 'Fred' },
  ];

  return (
    <ul>
      {/* keys need to be primitive values, ideally a unique string, such as an id */}
      {people.map(person =>
         <Person key={person.id} name={person.name} />
      )}
    </ul>
  );
}

// If you don't have some ids with your set of data that are unique // and primitive values, use the second parameter of .map() to get each // elements index

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

### 事件侦听器和处理事件

监听 JSX 元素和 HTML 元素上的事件有几个重要的不同。

首先，您不能监听 React 组件上的事件，只能监听 JSX 元素。例如，向 React 组件添加一个名为`onClick`的道具只是向 props 对象添加的另一个属性。

```
/* 
  The convention for most event handler functions is to prefix them with the word 'handle' and then the action they perform (i.e. handleToggleTheme)
*/
function handleToggleTheme() {
  // code to toggle app theme
}

/* In HTML, onclick is all lowercase, plus the event handler includes a set of parentheses after being referenced */
<button onclick="handleToggleTheme()">
  Toggle Theme
</button>

/* 
  In JSX, onClick is camelcase, like attributes / props.
  We also pass a reference to the function with curly braces.
*/
<button onClick={handleToggleTheme}>
  Toggle Theme
</button>
```

需要了解的最基本的反应事件是`onClick`、`onChange`和`onSubmit`。

*   `onClick`处理 JSX 元素(即按钮)上的点击事件
*   `onChange`处理键盘事件(即用户在输入区或文本区打字)
*   `onSubmit`处理用户提交的表单

```
function App() {
  function handleInputChange(event) {
    /* When passing the function to an event handler, like onChange we get access to data about the event (an object) */
    const inputText = event.target.value; // text typed into the input
    const inputName = event.target.name; // 'email' from name attribute
  }

  function handleClick(event) {
    /* onClick doesn't usually need event data, but it receives event data as well that we can use */
    console.log('clicked!');
    const eventType = event.type; // "click"
    const eventTarget = event.target; // <button>Submit</button>
  }

  function handleSubmit(event) {
    /* 
     When we hit the return button, the form will be submitted, as well as when a button with type="submit" is clicked.
     We call event.preventDefault() to prevent the default form behavior from taking place, which is to send an HTTP request and reload the page.
    */
    event.preventDefault();
    const formElements = event.target.elements; // access all element within form
    const inputValue = event.target.elements.emailAddress.value; // access the value of the input element with the id "emailAddress"
  }

  return (
    <form onSubmit={handleSubmit}>
      <input id="emailAddress" type="email" name="email" onChange={handleInputChange} />
      <button onClick={handleClick}>Submit</button>
    </form>
  );
} 
```

## 基本反应挂钩

### 状态和使用状态

钩子给了我们一个函数组件的状态。**状态**允许我们随时访问和更新组件中的某些值。

本地组件状态由 React 钩子`useState`管理，它给了我们一个状态变量和一个允许我们更新它的函数。

当我们调用`useState`时，我们可以给我们的状态一个默认值，当我们调用`useState`时，通过提供它作为第一个参数。

```
import React from 'react';

/* 
  How do you create a state variable?
  Syntax: const [stateVariable] = React.useState(defaultValue);
*/
function App() {
  const [language] = React.useState('JavaScript');
  /* 
    We use array destructuring to declare state variable.
    Like any variable, we declare we can name it what we like (in this case, 'language').
  */

  return <div>I am learning {language}</div>;
}
```

注意:本节中的任何钩子都来自 React 核心库，可以单独导入。

```
import React, { useState } from "react";

function App() {
  const [language] = useState("javascript");

  return <div>I am learning {language}</div>;
}
```

`useState`也给了我们一个‘setter’函数来更新创建后的状态。

```
function App() {
  /* 
   The setter function is always the second destructured value.
   The naming convention for the setter function is to be prefixed with 'set'.
  */
  const [language, setLanguage] = React.useState("javascript");

  return (
    <div>
      <button onClick={() => setLanguage("python")}>
        Learn Python
      </button>
      {/*  
        Why use an inline arrow function here instead of immediately calling it like so: onClick={setterFn()}? 
        If so, setLanguage would be called immediately and not when the button was clicked by the user.
        */}
      <p>I am now learning {language}</p>
    </div>
  );
}

/* 
 Note: whenever the setter function is called, the state updates,
 and the App component re-renders to display the new state.
 Whenever state is updated, the component will be re-rendered
*/
```

`useState`可以在单个组件内使用一次或多次。它可以接受原语或对象值来管理状态。

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

如果新状态依赖于以前的状态，为了保证更新可靠地完成，我们可以在 setter 函数中使用一个函数，它给出正确的以前状态。

```
/* We have the option to organize state using whatever is the most appropriate data type, according to the data we're managing */
function App() {
  const [developer, setDeveloper] = React.useState({
    language: "",
    yearsExperience: 0
  });

  function handleChangeYearsExperience(event) {
    const years = event.target.value;
    /* We must pass in the previous state object we had with the spread operator to spread out all of its properties */
    setDeveloper({ ...developer, yearsExperience: years });
  }

  return (
    <div>
      {/* No need to get previous state here; we are replacing the entire object */}
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
      {/* We can also pass a reference to the function */}
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

如果你管理多个原始值，多次使用`useState`通常比一个对象使用一次要好。你不用担心忘记把旧状态和新状态结合起来。

```
function App() {
  const [developer, setDeveloper] = React.useState({
    language: "",
    yearsExperience: 0,
    isEmployed: false
  });

  function handleToggleEmployment(event) {
    /* We get the previous state variable's value in the parameters.
       We can name 'prevState' however we like.
    */
    setDeveloper(prevState => {
      return { ...prevState, isEmployed: !prevState.isEmployed };
      // It is essential to return the new state from this function
    });
  }

  return (
    <button onClick={handleToggleEmployment}>Toggle Employment Status</button>
  );
} 
```

### 副作用和使用效果

让我们在函数组件中执行副作用。那么副作用是什么呢？

副作用是我们需要接触外部世界的地方。例如，从 API 获取数据或使用 DOM。

它们是能够以不可预测的方式改变我们的组件状态的动作(会导致“副作用”)。

接受一个回调函数(称为“效果”函数)，默认情况下，每次重新渲染时都会运行。

它在我们的组件挂载后运行，这是在组件生命周期中执行副作用的恰当时机。

```
/* What does our code do? Picks a color from the colors array and makes it the background color */
import React, { useState, useEffect } from 'react';

function App() {
  const [colorIndex, setColorIndex] = useState(0);
  const colors = ["blue", "green", "red", "orange"];

  /* 
    We are performing a 'side effect' since we are working with an API.
    We are working with the DOM, a browser API outside of React.
  */
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  });
  /* Whenever state is updated, App re-renders and useEffect runs */

  function handleChangeColor() {
    /* This code may look complex, but all it does is go to the next color in the 'colors' array, and if it is on the last color, goes back to the beginning */
    const nextIndex = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(nextIndex);
  }

  return (
    <button onClick={handleChangeColor}>
      Change background color
    </button>
  );
}
```

为了避免在每次渲染后执行效果回调，我们提供了第二个参数，一个空数组。

```
function App() {
  ...
  /* 
    With an empty array, our button doesn't work no matter how many times we click it... 
    The background color is only set once, when the component first mounts.
  */
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  }, []);

  /* 
    How do we not have the effect function run for every state update  but still have it work whenever the button is clicked? 
  */

  return (
    <button onClick={handleChangeIndex}>
      Change background color
    </button>
  );
}
```

让我们用依赖数组有条件地执行效果。

**dependencies 数组**是第二个参数，如果数组中的任何一个值发生变化，effect 函数将再次运行。

```
function App() {
  const [colorIndex, setColorIndex] = React.useState(0);
  const colors = ["blue", "green", "red", "orange"];

  /* 
    Let's add colorIndex to our dependencies array
    When colorIndex changes, useEffect will execute the effect function again
  */
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
    /* 
      When we use useEffect, we must think about what state values
      we want our side effect to sync with
    */
  }, [colorIndex]);

  function handleChangeIndex() {
    const next = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(next);
  }

  return (
    <button onClick={handleChangeIndex}>
      Change background color
    </button>
  );
}
```

让我们通过在最后返回一个函数来取消订阅某些效果。

```
function MouseTracker() {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });

  React.useEffect(() => {
    // .addEventListener() sets up an active listener...
    window.addEventListener("mousemove", handleMouseMove);

    /* ...So when we navigate away from this page, it needs to be
       removed to stop listening. Otherwise, it will try to set
       state in a component that doesn't exist (causing an error)

     We unsubscribe any subscriptions / listeners w/ this 'cleanup function')
     */
    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
    };
  }, []);

function handleMouseMove(event) {
   setMousePosition({
     x: event.pageX,
     y: event.pageY
   });
}

  return (
    <div>
      <h1>The current mouse position is:</h1>
      <p>
        X: {mousePosition.x}, Y: {mousePosition.y}
      </p>
    </div>
  );
}
```

`useEffect`是当您想要发出 HTTP 请求(即，当组件挂载时的 GET 请求)时使用的钩子。

注意，用更简洁的 async/await 语法处理承诺需要创建一个单独的函数。(为什么？效果回调函数不能是异步的。)

```
const endpoint = "https://api.github.com/users/reedbarger";

// Using .then() callback functions to resolve promise
function App() {
  const [user, setUser] = React.useState(null);

  React.useEffect(() => {
    fetch(endpoint)
      .then(response => response.json())
      .then(data => setUser(data));
  }, []);
}

// Using async / await syntax to resolve promise:
function App() {
  const [user, setUser] = React.useState(null);
  // cannot make useEffect callback function async
  React.useEffect(() => {
    getUser();
  }, []);

  // We must apply async keyword to a separate function
  async function getUser() {
    const response = await fetch(endpoint);
    const data = await response.json();
    setUser(data);
  }
} 
```

### Refs 和 useRef

参考是一个特殊的属性，所有的反应组件都有。它们允许我们在安装组件时创建对给定元素/组件的引用。

允许我们轻松地使用 React refs。我们调用 useRef(在组件的顶部)并将返回值附加到元素的 Ref 属性来引用它。

一旦我们创建了一个引用，我们就使用当前属性来修改(改变)元素的属性，或者可以调用该元素上任何可用的方法(比如`.focus()`来聚焦一个输入)。

```
function App() {
  const [query, setQuery] = React.useState("react hooks");
  /* We can pass useRef a default value.
     We don't need it here, so we pass in null to reference an empty object
  */
  const searchInput = useRef(null);

  function handleClearSearch() {
    /* 
      .current references the input element upon mount
      useRef can store basically any value in its .current property
    */
    searchInput.current.value = "";
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

## 挂钩和性能

### 防止重新渲染和反应。备忘录

`React.memo`是一个允许我们优化组件渲染方式的功能。

特别是，它执行一个称为**记忆化**的过程，帮助我们防止组件在不需要时重新呈现(有关记忆化的更完整定义，请参见 React.useMemo)。

`React.memo`最有助于防止组件列表在其父组件重新呈现时被重新呈现。

```
/* 
  In the following application, we are keeping track of our programming skills. We can create new skills using an input, and they are added to the list (shown in the SkillList component). If we click on a skill, it is deleted.
*/

function App() {
  const [skill, setSkill] = React.useState('')
  const [skills, setSkills] = React.useState([
    'HTML', 'CSS', 'JavaScript'
  ])

  function handleChangeInput(event) {
    setSkill(event.target.value);
  }

  function handleAddSkill() {
    setSkills(skills.concat(skill))
  }

  return (
    <>
      <input onChange={handleChangeInput} />
      <button onClick={handleAddSkill}>Add Skill</button>
      <SkillList skills={skills} />
    </>
  );
}

/* But the problem, if you run this code yourself, is that when we type into the input, because the parent component of SkillList (App) re-renders, due to the state being updated on every keystroke, the SkillList is rerendered constantly (as indicated by the console.log) */

/* However, once we wrap the SkillList component in React.memo (which is a higher-order function, meaning it accepts a function as an argument), it no longer re-renders unnecessarily when our parent component does. */
const SkillList = React.memo(({ skills }) => {
  console.log('rerendering');
  return (
    <ul>
    {skills.map((skill, i) => <li key={i}>{skill}</li>)}
    </ul>
  )
})

export default App
```

### 回调函数和使用回调

`useCallback`是一个挂钩，用于提高我们的组件性能。**回调函数**是父组件中被“回调”的函数的名字。

最常见的用法是拥有一个带有状态变量的父组件，但是您希望从子组件更新该状态。你是做什么的？您将回调函数从父节点传递给子节点。这允许我们更新父组件中的状态。

`useCallback`的功能与`React.memo`相似。它会记忆回调函数，所以不会在每次重新渲染时重新创建。正确使用`useCallback`可以提高我们应用的性能。

```
/* Let's keep the exact same App as above with React.memo, but add one small feature. Let's make it possible to delete a skill when we click on it. To do that, we need to filter the skills array according to the skill we click on. For that, we create the handleRemoveSkill function in App */

function App() {
  const [skill, setSkill] = React.useState('')
  const [skills, setSkills] = React.useState([
    'HTML', 'CSS', 'JavaScript'
  ])

  function handleChangeInput(event) {
    setSkill(event.target.value);
  }

  function handleAddSkill() {
    setSkills(skills.concat(skill))
  }

  function handleRemoveSkill(skill) {
    setSkills(skills.filter(s => s !== skill))
  }

  /* Next, we pass handleRemoveSkill down as a prop, or since this is a function, as a callback function to be used within SkillList */
  return (
    <>
      <input onChange={handleChangeInput} />
      <button onClick={handleAddSkill}>Add Skill</button>
      <SkillList skills={skills} handleRemoveSkill={handleRemoveSkill} />
    </>
  );
}

/* When we try typing in the input again, we see rerendering in the console every time we type. Our memoization from React.memo is broken! 

What is happening is the handleRemoveSkill callback function is being recreated everytime App is rerendered, causing all children to be rerendered, too. We need to wrap handleRemoveSkill in useCallback and only have it be recreated when the skill value changes.

To fix our app, replace handleRemoveSkill with:

const handleRemoveSkill = React.useCallback((skill) => {
  setSkills(skills.filter(s => s !== skill))
}, [skills])

Try it yourself!
*/
const SkillList = React.memo(({ skills, handleRemoveSkill }) => {
  console.log('rerendering');
  return (
    <ul>
    {skills.map(skill => <li key={skill} onClick={() => handleRemoveSkill(skill)}>{skill}</li>)}
    </ul>
  )
})

export default App
```

### 记忆和使用记忆

`useMemo`与`useCallback`非常相似，都是为了提高性能。但是它不是用于回调，而是用于存储昂贵计算的结果

`useMemo`允许我们**记住**，或者记住已经为某些输入进行的昂贵计算的结果。

记忆化意味着如果一个给定的输入已经计算过了，就没有必要再做一次，因为我们已经有了那个操作的存储结果。

`useMemo`从计算中返回一个值，然后存储在一个变量中。

```
/* Building upon our skills app, let's add a feature to search through our available skills through an additional search input. We can add this in a component called SearchSkills (shown above our SkillList).
*/

function App() {
  const [skill, setSkill] = React.useState('')
  const [skills, setSkills] = React.useState([
    'HTML', 'CSS', 'JavaScript', ...thousands more items
  ])

  function handleChangeInput(event) {
    setSkill(event.target.value);
  }

  function handleAddSkill() {
    setSkills(skills.concat(skill))
  }

  const handleRemoveSkill = React.useCallback((skill) => {
    setSkills(skills.filter(s => s !== skill))
  }, [skills])

  return (
    <>
      <SearchSkills skills={skills} />
      <input onChange={handleChangeInput} />
      <button onClick={handleAddSkill}>Add Skill</button>
      <SkillList skills={skills} handleRemoveSkill={handleRemoveSkill} />
    </>
  );
}

/* Let's imagine we have a list of thousands of skills that we want to search through. How do we performantly find and show the skills that match our search term as the user types into the input ? */
function SearchSkills() {
  const [searchTerm, setSearchTerm] = React.useState('');  

  /* We use React.useMemo to memoize (remember) the returned value from our search operation and only run when it the searchTerm changes */
  const searchResults = React.useMemo(() => {
    return skills.filter((s) => s.includes(searchTerm);
  }), [searchTerm]);

  function handleSearchInput(event) {
    setSearchTerm(event.target.value);
  }

  return (
    <>
    <input onChange={handleSearchInput} />
    <ul>
      {searchResults.map((result, i) => <li key={i}>{result}</li>
    </ul>
    </>
  );
}

export default App
```

## 高级反应挂钩

### 上下文和使用上下文

在 React 中，我们希望避免下面的问题，即创建多个 props 来从父组件向下传递两层或更多层的数据。

```
/* 
  React Context helps us avoid creating multiple duplicate props.
  This pattern is also called props drilling.
*/

/* In this app, we want to pass the user data down to the Header component, but it first needs to go through a Main component which doesn't use it */
function App() {
  const [user] = React.useState({ name: "Fred" });

  return (
    // First 'user' prop
    <Main user={user} />
  );
}

const Main = ({ user }) => (
  <>
    {/* Second 'user' prop */}
    <Header user={user} />
    <div>Main app content...</div>
  </>
);

const Header = ({ user }) => <header>Welcome, {user.name}!</header>;
```

上下文有助于从父组件向下传递多个级别的子组件。

```
/* 
  Here is the previous example rewritten with Context.
  First we create context, where we can pass in default values
  We call this 'UserContext' because we're passing down user data
*/
const UserContext = React.createContext();

function App() {
  const [user] = React.useState({ name: "Fred" });

  return (
    {/* 
      We wrap the parent component with the Provider property 
      We pass data down the component tree on the value prop
     */}
    <UserContext.Provider value={user}>
      <Main />
    </UserContext.Provider>
  );
}

const Main = () => (
  <>
    <Header />
    <div>Main app content</div>
  </>
);

/* 
  We can't remove the two 'user' props. Instead, we can just use the Consumer property to consume the data where we need it
*/
const Header = () => (
    {/* We use a pattern called render props to get access to the data */}
    <UserContext.Consumer>
      {user => <header>Welcome, {user.name}!</header>}
    </UserContext.Consumer>
);
```

`useContext`钩子允许我们在提供者的任何子功能组件中使用上下文，而不是使用 render props 模式。

```
function Header() {
  /* We pass in the entire context object to consume it and we can remove the Consumer tags */
  const user = React.useContext(UserContext);

  return <header>Welcome, {user.name}!</header>;
}; 
```

### 减速器和用户减速器

Reducers 是简单的、可预测的(纯)函数，它接受一个先前的状态对象和一个动作对象，并返回一个新的状态对象。

```
/* This reducer manages user state in our app: */

function userReducer(state, action) {
  /* Reducers often use a switch statement to update state in one way or another based on the action's type property */

  switch (action.type) {
    /* If action.type has the string 'LOGIN' on it, we get data from the payload object on action */
    case "LOGIN":
      return { 
        username: action.payload.username, 
        email: action.payload.email
        isAuth: true 
      };
    case "SIGNOUT":
      return { 
        username: "",
        email: "",
        isAuth: false 
      };
    default:
      /* If no case matches the action received, return the previous state */
      return state;
  }
}
```

Reducers 是一种强大的状态管理模式，用于流行的状态管理库 Redux(通常与 React 一起使用)。

与 useState(用于本地组件状态)相比，Reducers 可以在 React 中与`useReducer`挂钩一起使用，以便管理整个应用程序的状态。

`useReducer`可以与`useContext`配对，以管理数据并轻松地在组件间传递数据。

因此`useReducer` + `useContext`可以成为我们应用程序的一个完整的状态管理系统。

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

创建钩子是为了方便地重用组件之间的行为，类似于创建组件是为了在我们的应用程序中重用结构。

钩子让我们可以为我们的应用程序添加定制的功能来满足我们的需求，并且可以与我们已经讨论过的所有现存的钩子相结合。

为了所有 React 开发者的利益，钩子也可以包含在第三方库中。有很多很棒的 React 库提供定制钩子，比如`@apollo/client`、`react-query`、`swr`等等。

```
/* Here is a custom React hook called useWindowSize that I wrote in order to calculate the window size (width and height) of any component it is used in */

import React from "react";

export default function useWindowSize() {
  const isSSR = typeof window !== "undefined";
  const [windowSize, setWindowSize] = React.useState({
    width: isSSR ? 1200 : window.innerWidth,
    height: isSSR ? 800 : window.innerHeight,
  });

  function changeWindowSize() {
    setWindowSize({ width: window.innerWidth, height: window.innerHeight });
  }

  React.useEffect(() => {
    window.addEventListener("resize", changeWindowSize);

    return () => {
      window.removeEventListener("resize", changeWindowSize);
    };
  }, []);

  return windowSize;
}

/* To use the hook, we just need to import it where we need, call it, and use the width wherever we want to hide or show certain elements, such as in a Header component. */

// components/Header.js

import React from "react";
import useWindowSize from "../utils/useWindowSize";

function Header() {
  const { width } = useWindowSize();

  return (
    <div>
      {/* visible only when window greater than 500px */}
      {width > 500 && (
        <>
         Greater than 500px!
        </>
      )}
      {/* visible at any window size */}
	  <p>I'm always visible</p>
    </div>
  );
}
```

### 钩子的规则

使用 React 挂钩有两个基本规则，我们不能违反它们才能正常工作:

*   钩子只能在函数组件中使用(不是普通的 JavaScript 函数或类组件)
*   钩子只能在组件的顶部被调用(它们不能在条件、循环或嵌套函数中)

## **结论**

您还可以学习其他有价值的概念，但是如果您致力于学习本备忘单中涵盖的概念，您将会很好地掌握 React 库中最重要和最强大的部分。

*想保留本指南以备将来参考吗？*

[**点击此处下载此备忘单的完整 PDF 版本。**](http://bit.ly/react-sheet-2021)