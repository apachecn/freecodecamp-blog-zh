# 在学习 React 之前，您应该了解的 JavaScript 概念

> 原文：<https://www.freecodecamp.org/news/javascript-concepts-you-should-know-before-learning-react/>

React 是用于构建单页面应用程序的最流行的 JavaScript 框架之一。不用说，作为一个 JavaScript 框架，它要求你对 JavaScript 概念有很好的了解。

在本文中，我们将了解一些在学习 React 之前必须了解的 JavaScript 概念。对这些主题的良好理解是构建大规模 React 应用程序的基础。所以事不宜迟，我们开始吧。

## 1.JavaScript 基础知识

React 是一个 JS 框架，您将在 React 代码中广泛使用 JavaScript。因此，毫无疑问，您必须了解基本的 JavaScript 概念。

所谓基础，我指的是变量、数据类型、操作符、条件、数组、函数、对象、事件等等。

正确理解这些概念对您在 React 中正确导航很重要，因为您将在构建 React 应用程序的每个步骤中使用它们。

因此，如果你对这些东西不确定，或者想快速修改一下，可以去看看这些免费课程中的一些(T1)或(T2)freeCodeCamp JavaScript 课程(T3)。 [MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) 文档和 [JavaScript.info](https://javascript.info/) 也是有用的快速搜索参考。

## 2.三元运算符

[三元运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)是一个简短的单行条件运算符，用来替换 if/else。当需要快速检查条件以呈现组件、更新状态或显示一些文本时，它非常有用。

让我们比较一下三元运算符如何与 If/Else 语句一起工作:

```
// Example of Ternary Operator
condition ? 'True' : 'False' 
```

```
// Example of If/Else statement
if(condition) {
    'True'
}
else {
    'False'
}: 
```

您可以自己看到使用三元运算符比使用 If/Else 要干净和简短得多。

它的工作方式是你写一个条件，如果条件为真，你的程序就会执行`?`之后的语句。如果条件为假，程序将执行`:`后的语句

很简单，不是吗？

## 3.解构

[析构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)帮助我们从数组和对象中解包值，并以简单平滑的方式将它们分配给单独的变量。让我们用一些代码来理解它:

```
// With Destructuring
const objects = ['table', 'iPhone', 'apple']
const [furniture, mobile, fruit] = objects

// Without Destructuring
const furniture = objects[0]
const mobile = objects[1]
const fruit = objects[2] 
```

在上面的例子中，析构为我们节省了 3 行代码，并使代码更加简洁。现在让我们看另一个在 React with destructuring 中传递属性的例子:

```
// With Destructuring Ex-1
function Fruit({apple}) {
    return (
        <div>
            This is an {apple}
        </div>
    )
}

// With Destructuring Ex-2

function Fruit(props) {
    const {apple, iphone, car} = props
    return (
        <div>
            This is an {apple}
        </div>
    )
}

// Without Destructuring
function Fruit(props) {
    return (
        <div>
            This is an {props.apple}
        </div>
    )
} 
```

注意当你在道具中不使用析构的时候，你不得不一次又一次地使用`props`。

析构使我们的代码更干净，并避免我们每次使用 prop 变量时都使用关键字 **props** 。析构还有更多的[，当你开始用 JavaScript 和 React 构建应用程序时，你会学到这些东西。](https://www.freecodecamp.org/news/destructuring-patterns-javascript-arrays-and-objects/)

## 4.扩展运算符

在 ES6 中，JavaScript 引入了[扩展操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)。它接受一个[可迭代](https://www.freecodecamp.org/news/demystifying-es6-iterables-iterators-4bdd0b084082/)，并将其扩展成单个元素。

React 中 spread 运算符的一个常见用例是在状态更新期间将一个对象的值复制到另一个对象中，以合并两个对象的属性。请看下面的语法:

```
const [person, setPerson] = useState({
    id: '',
    name: '',
    age: ''
});

 setPerson([
            ...person,
            {
                id:"1",
                name: "Steve",
                age:"25"
            }
        ]); 
```

在上面的例子中，`...person`将 person 对象的所有值复制到新的 state 对象中，然后用具有相同属性的其他自定义值进一步替换，从而更新 state 对象。

这是 React 中 spread 操作符的众多用例之一。随着您的应用程序变得越来越大，像 spread 操作符这样的工具可以方便地以更好、更有效的方式处理数据。

## 5.数组方法

在 React 中构建大中型应用程序时，数组方法非常常见。几乎在你构建的每一个 React 应用中，你都会用到某种数组方法。

所以，花点时间学习这些方法吧。有些方法非常常见，如 **map()** 。每次从外部资源获取数据并将其显示在 UI 上时，都要使用 map()。

还有其他方法，如过滤、减少、排序、包含、查找、forEach、拼接、连接、推送和弹出、移位和取消移位等等。

有些是常用的，有些是很少用的。关键是要很好地理解常见的数组方法，并且要意识到其他方法的存在，这样无论何时你需要它们，你都可以很快学会它们。

这里有一本关于数组方法和在 JavaScript 中使用数组的有用手册,这样你可以了解更多。

## 6.箭头功能

[箭头函数](https://www.freecodecamp.org/news/arrow-function-javascript-tutorial-how-to-declare-a-js-function-with-the-new-es6-syntax/)允许我们用更短的语法以简单的方式创建函数。

```
// Regular Functions
function hello() {
    return 'hello'
}

// Arrow Functions
let hello = () => 'hello' 
```

上面代码片段中的两个函数工作方式相同，但是您可以看到箭头函数更加简洁。上述语法中的 empty()表示参数。即使没有参数，这些括号也应该存在。

但是，如果函数中只有一个参数，可以跳过这些括号，如下所示:

```
let square = num => num * num 
```

在单行箭头函数中，可以跳过 **return** 语句。也可以使用类似于常规函数的花括号{}来声明多行箭头函数。

```
let square = num => {
    return num * num
} 
```

## 7.承诺

在现代 JavaScript 中，您使用[承诺](https://www.freecodecamp.org/news/what-is-promise-in-javascript-for-beginners/)来处理[异步操作](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)。一旦你用 JavaScript 创建了一个承诺，它要么成功，要么失败——用 JavaScript 术语来说就是解决或拒绝。

在某种程度上，JavaScript 中的承诺也可以比作我们人类做出的承诺。就像人类的承诺是由某个动作的未来实现驱动的一样，JavaScript 中的承诺是关于代码的未来实现，导致它要么被解决，要么被拒绝。

承诺有三种状态:

1.  **待定**——承诺的最终结果尚未确定时。
2.  **已解决**–承诺成功解决时
3.  **拒绝**–当承诺被拒绝时。

一旦承诺被成功解决或拒绝，您可以使用**。然后()**或者**。【catch()【方法】在上面。**

*   **。当承诺被解决或拒绝时，调用 then()** 方法。它接受两个回调函数作为参数。第一个在解决承诺并收到结果时执行，第二个是可选参数，以防承诺被拒绝。
*   **。catch()** 方法用作错误处理程序，在承诺被拒绝或执行中出现错误时调用。

理论到此为止，让我们以一个承诺的例子来结束这一部分，包括`.then()`和`.catch()`方法的用法:

```
let promise = new Promise((resolve, reject) => {
  const i = "Promise";
  i === "Promise" ? resolve() : reject(); // Checkout the above Ternary Operator section to better understand the syntax
  }
);

promise.
    then(() => {
        console.log('Your promise is resolved');
    }).
    catch(() => {
        console.log('Your promise is rejected');
    }); 
```

## 8.获取 API

[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 允许我们从浏览器向 web 服务器发出异步请求。它在每次发出请求时返回一个承诺，然后进一步用于检索请求的响应。

一个基本的 fetch()接受一个参数，即您想要获取的资源的 URL。然后它返回另一个用`Response`对象解决的承诺。这个**响应**对象是 HTTP 响应的表示。

所以，要从这个承诺中获取 JSON 内容，必须使用**。json()** 响应对象上的方法。这最终将返回一个承诺，该承诺根据来自响应体的解析 JSON 数据的结果进行解析。

这可能有点令人困惑，所以请密切注意下面的例子:

```
fetch('http://example.com/books.json') // fetching the resource URL
  .then(response => response.json()); // calling .json() method on the promise
  .then(data => setState(data)); // updating the state with the JSON data 
```

## 9.异步/等待

[Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 功能提供了一种更好、更简洁的方式来处理承诺。JavaScript 本质上是同步的，async/await 帮助我们编写基于承诺的函数，通过停止执行进一步的代码直到承诺被解决或拒绝，就好像它们是同步的一样。

为了让它工作，你必须在声明一个函数之前首先使用 **async** 关键字。比如`async function promise() {}`。将 **async** 放在一个函数之前意味着这个函数将总是返回一个承诺。

在异步函数中，可以使用关键字`await`来暂停代码的进一步执行，直到该承诺被解决或拒绝。你只能在一个**异步**函数中使用**等待**。

现在，让我们通过一个示例快速结束这一部分:

```
async function asyncFunction() {
    let promise = new Promise(resolve => {
        resolve();
    });
    let response = await promise; // further execution will be stopped until the promise is resolved or rejected
    return console.log(response);
} 
```

在这篇深入的指南中，您可以了解关于 async 和 wait [的更多信息。](https://www.freecodecamp.org/news/javascript-async-await-tutorial-learn-callbacks-promises-async-await-by-making-icecream/)

## 10.ES 模块和导入/导出

模块是在 ES6 的 JavaScript 中引入的。每个文件都是一个独立的模块。您可以从一个文件中取出对象、变量、数组、函数等，并在另一个文件中使用它们。这被称为导入和导出模块。

在 React 中，我们使用 ES6 模块为组件创建单独的文件。每个组件都从其模块中导出，并导入到要渲染的文件中。让我们通过一个例子来了解这一点:

```
function Component() {
    return(
        <div>This is a component</div>
    )
}

export default Component 
```

```
import Component from './Component'

function App() {
    return (
        <Component />
    )
} 
```

在 React 中，您必须呈现在 App.js 组件中声明的每个组件。

在上面的例子中，我们创建了一个名为**组件**的组件，并用我们的代码`export default Component`将其导出。接下来，我们转到 **App.js** ，导入**组件**，代码如下:`import Component from './Component'`。

## **结论**

您已到达文章结尾！到目前为止，我们已经介绍了 JavaScript 基础知识，包括三元运算符、析构、扩展运算符、数组方法、箭头函数、承诺、Fetch API、Async/Await 和 ES6 模块以及导入/导出。

我希望你已经从这篇文章中学到了很多，并且理解了一些重要的 JavaScript 概念，以及为什么你需要在进入 React 之前彻底地学习它们。

这篇文章不能代替你自己彻底学习这些概念。我只是对它们做了一个大概的介绍，以及它们为什么重要。现在取决于你如何学习这些东西，并从这里建立你的知识。祝旅途好运！

您可以使用整篇文章中的参考资料来深入研究这些重要的概念。

查看我的[博客](https://fullstackstage.com)阅读更多类似这样的优质内容。如果你有问题要问或者想和我打招呼，请在[推特](https://twitter.com/ashutoshmishrae)上联系我。