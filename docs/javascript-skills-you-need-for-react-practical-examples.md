# React 所需的 JavaScript 技能(+实际例子)

> 原文：<https://www.freecodecamp.org/news/javascript-skills-you-need-for-react-practical-examples/>

关于 React，需要理解的最重要的一点是*它从根本上来说是* *JavaScript* 。这意味着你对 JavaScript 越在行，你在 React 上就越成功。

为了掌握 React，我们来分解一下你应该知道的关于 JavaScript 的 7 个基本概念。

当我说这些概念是必不可少的时候，我的意思是它们被用在 React 开发人员开发的每一个应用程序中，几乎没有例外。

学习这些概念是你可以做的最有价值的事情之一，可以加速你制作 React 项目的能力，成为一名熟练的 React 开发人员，所以让我们开始吧。

## 想要一本自己的指南吗？

[**在此下载 PDF 格式的备忘单**](http://bit.ly/7-js-skills-for-react) (耗时 5 秒)。

## 1.函数声明和箭头函数

任何 React 应用程序的基础都是组件。在 React 中，组件是用 JavaScript 函数和类定义的。

但是与 JavaScript 函数不同，React 组件返回用于构建应用程序界面的 JSX 元素。

```
// JavaScript function: returns any valid JavaScript type
function javascriptFunction() {
  return "Hello world";
}

// React function component: returns JSX
function ReactComponent(props) {
  return <h1>{props.content}</h1>   
}
```

注意 JavaScript 函数和 React 函数组件名称之间的不同大小写。JavaScript 函数用 camel 大小写命名，而 React 函数组件用 pascal 大小写(其中所有单词都大写)。

用 JavaScript 写函数有两种不同的方式:传统方式，使用`function`关键字，称为**函数声明**，以及作为**箭头函数**，这是在 ES6 中引入的。

函数声明和箭头函数都可以用来在 React 中编写函数组件。

箭头函数的主要好处是简洁。我们可以使用几个快捷键来编写函数，以删除不必要的样板文件，这样我们甚至可以在一行中编写所有内容。

```
// Function declaration syntax
function MyComponent(props) {
  return <div>{props.content}</div>;
}

// Arrow function syntax
const MyComponent = (props) => {
  return <div>{props.content}</div>;
}

// Arrow function syntax (shorthand)
const MyComponent = props => <div>{props.content}</div>;

/* 
In the last example we are using several shorthands that arrow functions allow:

1\. No parentheses around a single parameter
2\. Implicit return (as compared to using the "return" keyword)
3\. No curly braces for function body
*/
```

在箭头函数上使用函数声明的一个小好处是，你不必担心**提升**的问题。

由于提升的 JavaScript 行为，您可以在单个文件中以您喜欢的任何顺序使用由函数声明组成的多个函数组件。

然而，由箭头函数构成的函数组件不能按照您喜欢的方式排序。因为 JavaScript 变量是提升的，所以箭头函数组件必须在使用前声明:

```
function App() {
  return (
    <>
      {/* Valid! FunctionDeclaration is hoisted */}
      <FunctionDeclaration />
      {/* Invalid! ArrowFunction is NOT hoisted. Therefore, it must be declared before it is used */}
      <ArrowFunction />
    </>
}

function FunctionDeclaration() {
  return <div>Hello React!</div>;   
}

function ArrowFunction() {
  return <div>Hello React, again!</div>;   
} 
```

使用函数声明语法的另一个小区别是，在声明函数之前，您可以使用`export default`或`export`立即从文件中导出组件。您只能在箭头函数前使用`export`关键字(默认导出必须放在组件下面的一行上)。

```
// Function declaration syntax can be immediately exported with export default or export
export default function App() {
  return <div>Hello React</div>;   
}

// Arrow function syntax must use export only
export const App = () => {
  return <div>Hello React</div>;     
}
```

## 2.模板文字

JavaScript 在处理字符串方面有着笨拙的历史，尤其是当你想将**和**连接起来或者将多个字符串连接在一起的时候。在 ES6 到来之前，要将字符串加在一起，需要使用`+`操作符将每个字符串段加到一起。

随着 ES6 的加入，我们得到了一种新形式的字符串，称为**模板文字**，它由两个反勾``` `` ```组成，而不是单引号或双引号。

我们可以通过在特殊的`${}`语法中加入一个 JavaScript 表达式(比如一个变量)来连接字符串，而不必使用`+`操作符:

```
/* 
Concatenating strings prior to ES6.
Notice the awkward space after the word Hello?
*/
function sayHello(text) {
  return 'Hello ' + text + '!';
}

sayHello('React'); // Hello React!

/* 
Concatenating strings using template literals.
See how much more readable and predictable this code is?
*/
function sayHelloAgain(text) {
  return `Hello again, ${text}!`;
}

sayHelloAgain('React'); // Hello again, React!
```

模板文字的强大之处在于它们能够在`${}`语法中使用任何 JavaScript 表达式(即 JavaScript 中任何可以解析为值的内容)。

我们甚至可以使用三元运算符包含条件逻辑，这对于有条件地向给定的 JSX 元素添加或删除类或样式规则来说是完美的:

```
/* Go to react.new and paste this code in to see it work! */
import React from "react";

function App() {
  const [isRedColor, setRedColor] = React.useState(false);

  const toggleColor = () => setRedColor((prev) => !prev);

  return (
    <button
      onClick={toggleColor}
      style={{
        background: isRedColor ? "red" : "black",
        color: 'white'
      }}
    >
      Button is {isRedColor ? "red" : "not red"}
    </button>
  );
}

export default App;
```

简而言之，每当我们需要动态创建字符串时，模板文字对于 React 来说都是非常好的。例如，当我们在站点的头元素或体元素中使用可以改变的字符串值时:

```
import React from 'react';
import Head from './Head';

function Layout(props) {
  // Shows site name (i.e. Reed Barger) at end of page title
  const title = `${props.title} | Reed Barger`;  

  return (
     <>
       <Head>
         <title>{title}</title>
       </Head>
       <main>
        {props.children}
       </main>
     </>
  );
} 
```

## 3.短条件句:&&，||，三元运算符

考虑到 React 只是 JavaScript，使用简单的 if 语句，有时使用 switch 语句，有条件地显示(或隐藏)JSX 元素是非常容易的。

```
import React from "react";

function App() {
  const isLoggedIn = true;

  if (isLoggedIn) {
    // Shows: Welcome back!
    return <div>Welcome back!</div>;
  }

  return <div>Who are you?</div>;
}

export default App;
```

在一些基本 JavaScript 操作符的帮助下，我们减少了重复，使代码更加简洁。

我们可以使用**三元运算符将上面的 if 语句转换成下面的语句。**三元运算符的功能与 if 语句完全相同，但它更短，是一个表达式(不是语句)，可以插入 JSX:

```
import React from "react";

function App() {
  const isLoggedIn = true;

  // Shows: Welcome back!
  return isLoggedIn ? <div>Welcome back!</div> : <div>Who are you?</div>
}

export default App;
```

三元运算符也可以用在花括号内(同样，因为它是一个表达式):

```
import React from "react";

function App() {
  const isLoggedIn = true;

  // Shows: Welcome back!
  return <div>{isLoggedIn ? "Welcome back!" : "Who are you?"</div>;
}

export default App;
```

如果我们要改变上面的例子，只想在用户登录时显示文本(如果`isLoggedIn`为真)，这将是`&&` (and)操作符的一个很好的用例。

如果条件中的第一个值(**操作数**)为真，则`&&`运算符显示第二个操作数。否则它返回第一个操作数。而且由于是 **falsy** (是 JavaScript 自动转换为布尔值`false`的值)，所以不是由 JSX 渲染的:

```
import React from "react";

function App() {
  const isLoggedIn = true;

  // If true: Welcome back!, if false: nothing
  return <div>{isLoggedIn && "Welcome back!"}</div>;
}

export default App;
```

假设我们想要与现在相反的结果:只说“你是谁？”如果`isLoggedIn`为假。如果是真的，我们什么都不会展示。

对于这个逻辑，我们可以使用`||` (or)运算符。它本质上与`&&`操作符相反。如果第一个操作数为 true，则返回第一个(falsy)操作数。如果第一个操作数为假，则返回第二个操作数。

```
import React from "react";

function App() {
  const isLoggedIn = true;

  // If true: nothing, if false: Who are you?
  return <div>{isLoggedIn || "Who are you?"}</div>;
}

export default App;
```

## 4.三个数组方法:。map()，。filter()，。减少()

将原始值插入 JSX 元素很容易——只需使用花括号。

我们可以插入任何有效的表达式，包括包含原始值的变量(字符串、数字、布尔值等等)以及包含原始值的对象属性。

```
import React from "react";

function App() {
  const name = "Reed";
  const bio = {
    age: 28,
    isEnglishSpeaker: true
  };

  return (
    <>
      <h1>{name}</h1>
      <h2>I am {bio.age} years old</h2>
      <p>Speaks English: {bio.isEnglishSpeaker}</p>
    </>
  );
}

export default App;
```

如果我们有一个数组，我们想遍历该数组，以显示单个 JSX 元素中的每个数组元素，该怎么办？

为此，我们可以使用`**.map()**`方法。它允许我们以内部函数指定的方式转换数组中的每个元素。

请注意，当与箭头函数结合使用时，它尤其简洁。

```
/* Note that this isn't exactly the same as the normal JavaScript .map() method, but is very similar. */
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {programmers.map(programmer => <li>{programmer}</li>)}
    </ul>
  );
}

export default App;
```

还有其他口味的。map()方法，这些方法执行相关的任务，了解这些方法非常重要，因为它们可以相互组合起来。

为什么？因为`.map()`像许多数组方法一样，返回它迭代过的数组的浅表副本。这使其返回的数组能够传递给链中的下一个方法。

`**.filter()**`，顾名思义，允许我们从数组中过滤掉某些元素。例如，如果我们想删除所有以“J”开头的程序员的名字，我们可以用`.filter()`来实现:

```
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {/* Returns 'Reed' */}
      {programmers
       .filter(programmer => !programmer.startsWith("J"))
       .map(programmer => <li>{programmer}</li>)}
    </ul>
  );
}

export default App;
```

理解`.map()`和`.filter()`只是`**.reduce()**`数组方法的不同变体很重要，后者能够将数组值转换成几乎任何数据类型，甚至是非数组值。

这里的`.reduce()`执行与上面的`.filter()`方法相同的操作:

```
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {/* Returns 'Reed' */}
      {programmers
        .reduce((acc, programmer) => {
          if (!programmer.startsWith("J")) {
            return acc.concat(programmer);
          } else {
            return acc;
          }
        }, [])
        .map((programmer) => (
          <li>{programmer}</li>
        ))}
    </ul>
  );
}

export default App;
```

## 5.对象技巧:属性速记、析构、扩展运算符

像数组一样，对象也是一种数据结构，在使用 React 时，您需要熟悉它。

因为对象是为了有组织的键值存储而存在的，不像数组，所以您需要非常舒适地访问和操作对象属性。

若要在创建对象时向其添加属性，请命名该属性及其相应的值。需要记住的一个非常简单的方法是，如果属性名与值相同，那么只需列出属性名。

这是**对象属性简写**:

```
const name = "Reed";

const user = {
  // instead of name: name, we can use...
  name
};

console.log(user.name); // Reed
```

从对象访问属性的标准方法是使用点符号。然而，更方便的方法是在中析构**对象。它允许我们从给定的对象中提取属性作为同名的独立变量。**

这看起来有点像你在逆向编写一个对象，这使得这个过程很直观。这比每次想从对象名中获取值时都要多次使用对象名要好得多。

```
const user = {
  name: "Reed",
  age: 28,
  isEnglishSpeaker: true
};

// Dot property access
const name = user.name;
const age = user.age;

// Object destructuring
const { age, name, isEnglishSpeaker: knowsEnglish } = user;
// Use ':' to rename a value as you destructure it

console.log(knowsEnglish); // true
```

现在，如果您想从现有的对象中创建对象，您可以逐个列出属性，但这会非常重复。

除了手动复制属性，还可以使用“对象扩散”操作符将一个对象的所有属性扩散到另一个对象中(在创建时):

```
const user = {
  name: "Reed",
  age: 28,
  isEnglishSpeaker: true
};

const firstUser = {
  name: user.name,
  age: user.age,
  isEnglishSpeaker: user.isEnglishSpeaker
};

// Copy all of user's properties into secondUser 
const secondUser = {
  ...user  
};
```

对象扩散的伟大之处在于，您可以将尽可能多的对象扩散到一个新对象中，并且可以像属性一样对它们进行排序。但是请注意，随后出现的同名属性将覆盖以前的属性:

```
const user = {
  name: "Reed",
  age: 28
};

const moreUserInfo = {
  age: 70,
  country: "USA"
};

// Copy all of user's properties into secondUser 
const secondUser = {
  ...user,
  ...moreUserInfo,
   computer: "MacBook Pro"
};

console.log(secondUser);
// { name: "Reed", age: 70, country: "USA", computer: "Macbook Pro" }
```

## 6:承诺+异步/等待语法

事实上，每个 React 应用程序都由**异步代码**–**代码组成，这些代码的执行时间不定。特别是如果你需要使用浏览器特性从外部 API 获取或更改数据，比如**获取 API** 或第三方库 **axios** 。**

**承诺用于解析异步代码，使其像正常的同步代码一样解析，我们可以从上到下阅读。**

**承诺传统上使用回调来解析我们的异步代码。我们使用`.then()`回调来解析成功解析的承诺，而使用`.catch()`回调来解析响应错误的承诺。**

**这是一个使用 React 从我的 GitHub API 获取数据的真实示例，使用 Fetch API 显示我的个人资料图像。使用承诺解析数据:**

```
`/* Go to react.new and paste this code in to see it work! */
import React from 'react';

const App = () => {
  const [avatar, setAvatar] = React.useState('');

  React.useEffect(() => {
    /* 
      The first .then() lets us get JSON data from the response.
      The second .then() gets the url to my avatar and puts it in state. 
    */
    fetch('https://api.github.com/users/reedbarger')
      .then(response => response.json())
      .then(data => setAvatar(data.avatar_url))
      .catch(error => console.error("Error fetching data: ", error);
  }, []);

  return (
    <img src={avatar} alt="Reed Barger" />
  );
};

export default App;`
```

**我们不需要总是使用回调来解析来自承诺的数据，我们可以使用一个看起来与同步代码相同的干净语法，称为 **async/await 语法**。**

**async 和 await 关键字仅用于函数(普通的 JavaScript 函数，而不是 React 函数组件)。**

**为了使用它们，我们需要确保我们的异步代码在一个带有关键字`async`的函数中。任何承诺的值都可以通过在它前面放置关键字`await`来解析。**

```
`/* Go to react.new and paste this code in to see it work! */
import React from "react";

const App = () => {
  const [avatar, setAvatar] = React.useState("");

  React.useEffect(() => {
    /* 
	  Note that because the function passed to useEffect cannot be async, we must create a separate function for our promise to be resolved in (fetchAvatar)
    */
    async function fetchAvatar() {
      const response = await fetch("https://api.github.com/users/reedbarger");
      const data = await response.json();
      setAvatar(data.avatar_url);
    }

    fetchAvatar();
  }, []);

  return <img src={avatar} alt="Reed Barger" />;
};

export default App;` 
```

**我们使用`.catch()`回调来处理传统承诺中的错误，但是如何用 async/await 来捕捉错误呢？**

**我们仍然使用`.catch()`,当我们遇到错误时，比如当我们从我们的 API 得到一个处于 200 或 300 状态范围内的响应时:**

```
`/* Go to react.new and paste this code in to see it work! */
import React from "react";

const App = () => {
  const [avatar, setAvatar] = React.useState("");

  React.useEffect(() => {
    async function fetchAvatar() {
      /* Using an invalid user to create a 404 (not found) error */
      const response = await fetch("https://api.github.com/users/reedbarge");
      if (!response.ok) {
        const message = `An error has occured: ${response.status}`;
        /* In development, you'll see this error message displayed on your screen */
        throw new Error(message);
      }
      const data = await response.json();

      setAvatar(data.avatar_url);
    }

    fetchAvatar();
  }, []);

  return <img src={avatar} alt="Reed Barger" />;
};

export default App;` 
```

## **7.ES 模块+导入/导出语法**

**ES6 让我们能够使用 **ES 模块**在我们自己的 JavaScript 文件和第三方库之间轻松共享代码。**

**此外，当我们利用像 Webpack 这样的工具时，我们可以导入像图像和 SVG 这样的资产，以及 CSS 文件，并在我们的代码中使用它们作为动态值。**

```
`/* We're bringing into our file a library (React), a png image, and CSS styles */
import React from 'react';
import logo from '../img/site-logo.png';
import '../styles/app.css';

function App() {
  return (
    <div>
      Welcome!
      <img src={logo} alt="Site logo" />
    </div>
  );
}

export default App;`
```

**ES 模块背后的想法是能够将我们的 JavaScript 代码分割成不同的文件，使其模块化或在我们的应用程序中可重用。**

**就 JavaScript 代码而言，我们可以导入和导出变量和函数。有两种方式的进口和出口，作为**命名进口/出口**和作为**默认进口/出口。****

**每个文件只能有一个默认的导入或导出，我们可以根据自己的喜好创建任意多个名为 imports/export 的东西。例如:**

```
`// constants.js
export const name = "Reed";

export const age = 28;

export default function getName() {
  return name;   
}

// app.js
// Notice that named exports are imported between curly braces
import getName, { name, age } from '../constants.js';

console.log(name, age, getName());`
```

**我们还可以将所有导出写在文件的末尾，而不是写在每个变量或函数的旁边:**

```
`// constants.js
const name = "Reed";

const age = 28;

function getName() {
  return name;   
}

export { name, age };
export default getName;

// app.js
import getName, { name, age } from '../constants.js';

console.log(name, age, getName());`
```

**对于命名导入，您还可以使用`as`关键字对导入的内容进行别名或重命名。默认导出的好处是它们可以被命名为您喜欢的任何名称。**

```
`// constants.js
const name = "Reed";

const age = 28;

function getName() {
  return name;   
}

export { name, age };
export default getName;

// app.js
import getMyName, { name as myName, age as myAge } from '../constants.js';

console.log(myName, myAge, getMyName());`
```

### **在观看你最喜欢的电视节目的时间里，你可以在 React 开始一个 10 万美元/年的职业生涯。**

 **在这个 premium React 培训课程中，您可以释放知识、技能和信心，以实实在在的金钱带来改变生活的结果。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*点击此处获得开课通知***