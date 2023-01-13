# 如何在 React 中使用 ES6 特性

> 原文：<https://www.freecodecamp.org/news/how-to-use-es6-javascript-features-in-react/>

许多 JavaScript 框架使用 ES6 特性。因此，为了帮助您学习这些方便的特性，我将向您介绍它们，然后展示如何在 React.js 中应用它们。

以下是我们将在本指南中介绍的 ES6 功能:

*   模块
*   解构
*   传播算子
*   箭头功能
*   模板文字

我们在这里看到的所有例子都很基本，应该很容易让初学者掌握。

## 如何使用 ES6 模块

模块可以帮助你将应用程序的各种功能分解到不同的文件/脚本中。您可以为表单验证、用户登录等使用不同的脚本。

这里，我们将有两个脚本:一个用于数字加法，另一个用于数字减法。我们会一步一步来。

这是我们文件夹的结构:

> index.html
> script . js
> my modules/
> add . js
> sub . js

首先，我们将看看如何在普通 JavaScript 中使用模块。然后我们将看到如何在 React 中应用它们。

### 步骤 1–创建 HTML 文件并链接您的脚本

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>ES6 Modules</title>
</head>
<body>
    <script type="module" src="script.js"></script>
</body>
```

您会注意到脚本标签有一个值为`module`的`type`。如果您打算使用模块特性，这应该是您要做的第一件事。

您可能会遇到使用不同方法的资源，比如给它们的文件添加一个`.mjs`扩展名，但是为了安全起见，我推荐这种方法。`script.js`将作为“父脚本”,我们将模块导入其中。

### 步骤 2–创建函数并将其导出到单独的文件中

下面是`add.js`中的加法功能:

```
export function add(a, b){
    return a + b;
}
```

下面是`sub.js`中的减法函数:

```
export function sub(a, b){
    return a - b;
}
```

你注意到`export`的陈述了吗？为了能够在其他脚本中使用这些函数，您必须通过添加`export`语句来导出它们。

这里，我们通过在函数前添加语句来使用内联导出——但是您可以选择在文档底部导出该函数，如下所示:`export default add;`。

### 步骤 3–将功能导入`script.js`

```
import { add } from "./myModules/add.js";
import { sub } from "./myModules/sub.js"

console.log(add(6, 4)); // 10

console.log(sub(6, 4));  // 2
```

为了导入`add`函数，我们首先输入了`import`语句，后面是嵌套在花括号中的函数名，然后是函数所在文件的路径。

你可以看到我们是如何使用`add(6, 4);`的，通过从头开始创建函数，没有重新发明轮子。现在你可以把这个函数导入到任何你想要的脚本中。

### 步骤 4–如何在 React.js 中应用模块

既然您已经看到了如何在普通 JavaScript 中使用模块，那么让我们看看如何在 React 应用程序中使用它们。

当您创建 React 应用程序时，`App.js`组件通常充当主组件。我们将创建另一个名为`User.js`的组件，其中包含一些关于用户的内容。

下面是`App.js`组件:

```
function App() {
  return (
    <div className="App">

    </div>
  )
}

export default App 
```

这个组件只有一个没有任何内容的`div`。

这里是`User.js`组件:

```
function User() {
    return (
        <div>
            <h1>My name is Ihechikara.</h1>
            <p>I am a web developer.</p>
            <p>I love writing.</p>
        </div>
    )
}

export default User 
```

如果您还记得，您可以像我们刚才做的那样，在脚本的底部导出您的函数。接下来，我们将把这个函数导入到`App.js`组件中:

```
import User from "./User"

function App() {
  return (
    <div className="App">
      <User/>
    </div>
  )
}

export default App 
```

这个脚本只增加了两项内容:`import User from "./User"`指向组件的位置，而`<User/>`是组件本身。

现在你可以在你的应用程序中重用`User.js`组件中的逻辑，并且你可以使用 props 而不是硬编码用户信息来使它更加动态——但这超出了本教程的范围。

## 如何使用 ES6 析构

destructure 的意思是拆除某物的结构。在 JavaScript 中，这个结构可以是一个数组、一个对象，甚至是一个字符串，其中组成该结构的属性将用于创建一个新的相同的结构(属性可以改变)。

如果我所说的对你来说仍然很抽象，不要担心，因为从例子中你会理解得更好。

在 ES6 之前，这是您在 JavaScript 中提取一些数据的方式:

```
var scores = [500, 400, 300];

var x = scores[0],
    y = scores[1],
    z = scores[2];

    console.log(x,y,z); // 500 400 300
```

但是在 ES6 中，使用析构，我们可以这样做:

```
let scores = [500, 400, 300];

let [x, y, z] = scores;

console.log(x,y,z); //500 400 300
```

x、y 和 z 变量将按照出现的顺序继承分数数组中的值，即`x = 500`、`y = 400`和`z = 300`。在数组中的所有值都已被继承的情况下，任何没有父值的其他值都将作为未定义的值返回。那就是:

```
let scores = [500, 400, 300];

let [x, y, z, w] = scores;

console.log(x,y,z,w); //500 400 300 undefined
```

下面是一个使用对象的示例:

```
let scores = {
    pass: 70,
    avg: 50,
    fail: 30
};

let { pass, avg, fail} = scores;

console.log(pass, avg, fail); // 70 50 30
```

该过程与析构数组相同。

这是另一个例子，但是有字符串:

```
let [user, interface] = 'UI';

console.log(user); // U

console.log(interface); // I
```

字符串被分割成单个字母，然后分配给数组中的变量。

### 如何在 React.js 中使用析构

在 React 中，有很多可能需要使用析构的场景。但是一个非常常见的例子是使用`useState`钩子。

```
import { useState } from 'react';

function TestDestructuring() {
    const [grade, setGrade] = useState('A');

    return(
        <>

        </>
    )
}

export default TestDestructuring 
```

上面，我们创建了一个常量变量`grade`和一个函数`setGrade`，其目的是更新变量的值。我们使用析构将`grade`的值设置为‘A’。

## 如何使用 ES6 扩展运算符

spread 运算符`...`允许您将一个数组、对象或字符串的全部或部分复制到另一个数组、对象或字符串中。例如:

```
const collectionOne = [10, 20, 30];
const collectionTwo = [40, 50, 60];

const allCollections = [...collectionOne, ...collectionTwo];

console.log(allCollections); //10, 20, 30, 40, 50, 60
```

这真的没什么。使用`...`符号，将前两个集合的所有值分配给第三个集合。

现在我们已经将所有的集合放在了一个数组中，我们将使用 spread 操作符来复制数组并输出最大的数字。那就是:

```
const allCollections = [10, 20, 30, 40, 50, 60];

const maxNumber = Math.max(...allCollections);
console.log(maxNumber) //60
```

### 如何将扩展运算符与析构结合起来

在上一节中，我们看到了 JavaScript 中析构的应用。现在，让我们看看如何将析构和扩展操作符结合起来:

```
let scores = [500, 400, 300];

let [x, ...y] = scores;

console.log(x); // 500

console.log(y); // [400, 300]
```

这里，`x`变量继承了数组中的第一个数字，然后`y`变量分布在数组中，并复制剩下的所有数字。

## 如何使用 ES6 箭头功能

基本上，arrow 函数允许我们使用更短的语法来编写函数。在 ES6 之前，您应该这样编写函数:

```
var greetings = function() {
  console.log("Hello World!")
}

//OR

function greetings2() {
  console.log("HI!")
}
```

ES6 引入了不同的语法:

```
var greetings = () => {
  console.log("Hello World!")
}

var greetings = () => {
  console.log("HI!")
} 
```

在引入粗箭头操作符`=>`的同时，删除了`function`关键字。

注意，箭头函数是匿名的。

### 如何使用带参数的箭头函数

箭头函数中的参数被传递到粗箭头操作符前面的括号中。示例:

```
var add = (a,b)=>{
  return a + b;
}
console.log(add(2,2)) //4
```

## 如何使用 ES6 模板文字

模板文本允许您使用反勾号(``)而不是引号("")来定义字符串。这有各种好处。

ES6 之前:

```
var name = "My name is Ihechikara" 

console.log(fullname);
```

对于 ES6:

```
var name = `My name is Ihechikara` 

console.log(fullname);
```

### 模板文字中的插值

字符串插值允许您在字符串中包含变量和语句，而不用用`+`操作符将其分解。那就是:

```
var me = 'Ihechikara';

var fullname = `My name is Abba ${me}`;

console.log(fullname);
```

要将一个变量插入到字符串中，可以使用`${}`并将变量名传递到花括号中。请始终记住，您的字符串必须嵌套在反勾号内，而不是引号内。

这同样适用于用 JavaScript 动态创建 DOM 元素的情况。你会这样做:

```
let name = 'Ihechikara';
let myHtmlTemplate = `<h1> This is a paragraph created by ${name}</h1>`;
```

## 结论

本文介绍了一些最重要的 ES6 特性，比如模块、析构、扩展操作符、箭头函数和模板文字。

在学习或理解 JavaScript 框架的过程中，您会看到这些特性被频繁使用，因此它应该有助于您掌握它们在任何框架中的应用。

如果你对这些功能有什么疑问，可以在 Twitter 上找我 [@ihechikara2](https://twitter.com/Ihechikara2) 。感谢您的阅读！