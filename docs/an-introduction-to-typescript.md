# 如何使用 TypeScript 初学者友好的 TS 教程

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-typescript/>

大家好！在本文中，我们将讨论什么是 TypeScript，为什么它很酷，以及如何使用它。

## 目录

*   [简介](#intro)
*   [关于类型](#abouttypes)
    *   [琴弦](#strings)
    *   [数字](#numbers)
    *   [布尔型](#booleans)
    *   [未定义](#undefined)
    *   [Null](#null)
    *   [物体](#objects)
    *   [数组](#arrays)
    *   [功能](#functions)
*   类型和 JavaScript 有什么关系？
*   [进来打字稿](#incomestypescript)
*   [打字稿基础知识](#typescriptbasics)
    *   [推理类型](#typesbyinference)
    *   [声明类型](#declaringtypes)
        *   [接口](#interfaces)
        *   [条件句](#conditionals)
        *   [工会](#unions)
        *   [打字功能](#typingfunctions)
        *   [输入数组](#typingarrays)
*   [TypeScript 的编译器](#typescriptscompiler)
*   [如何创建 Typescript 项目](#howtocreateatypescriptproject)
    *   [关于图书馆的评论](#acommentaboutlibraries)
*   [TypeScript 的其他功能](#otherfunctionalitiesoftypescript)
*   [综述](#roundup)

# 介绍

TypeScript 是 JavaScript 的一个**超集**。超集意味着它在 JavaScript 提供的基础上增加了一些特性。TypeScript 将 JavaScript 提供的所有功能和结构作为一种语言，并添加了一些东西。

TypeScript 主要提供的是**静态类型**。所以要真正理解这意味着什么，我们首先需要理解什么是类型。让我们开始吧。

# 关于类型

在编程语言中，类型指的是给定程序存储的信息的种类或类型。信息或数据可以根据其内容分为不同的类型。

编程语言通常有内置的数据类型。JavaScript 中有**六种基本数据类型**，可以分为**三大类**:

*   原始数据类型
*   复合数据类型
*   特殊数据类型

*   字符串、数字和布尔是**原始的**数据类型。
*   对象、数组和函数(都是对象类型)是**复合**数据类型。
*   而 Undefined 和 Null 是特殊的数据类型。

**原始**数据类型一次只能保存**的一个值**，而**复合**数据类型可以保存**的值集合**和更复杂的实体。

让我们快速看一下这些数据类型。

## 用线串

字符串数据类型用于表示文本数据(即字符序列)。字符串是用单引号或双引号括住一个或多个字符创建的，如下所示:

```
let a = "Hi there!"; 
```

## 数字

数字数据类型用于表示带或不带小数点的正数或负数:

```
let a = 25; 
```

数字数据类型还包括一些特殊值:`Infinity`、`-Infinity`和`NaN`。

`Infinity`代表数学上的无穷大∞，大于任何数。`-Infinity`是非零数除以 0 的结果。而`NaN`代表一个特殊的非数字值。它是无效或未定义的数学运算的结果，如取-1 的平方根或用 0 除 0，等等。

## 布尔运算

布尔数据类型只能保存两个值:`true`或`false`。它通常用于存储 yes(真)或 no(假)、on(真)或 off(假)等值，如下所示:

```
let areYouEnjoyingTheArticle = true; 
```

## 不明确的

未定义的数据类型只能有一个值，特殊值`undefined`。如果一个变量已经被声明，但是没有被赋值，那么它的值就是 undefined。

```
let a;

console.log(a); // Output: undefined 
```

## 空

空值意味着没有值。它不等同于空字符串("")或 0，它只是什么都不是。

```
let thisIsEmpty = null; 
```

## 目标

对象是一种复杂的数据类型，允许您存储数据集合。一个对象包含**属性**，定义为一个**键值对**。

属性键(名称)总是字符串，但值可以是任何数据类型，如字符串、数字、布尔值或复杂数据类型，如数组、函数和其他对象。

```
let car = {
  modal: "BMW X3",
  color: "white",
  doors: 5
}; 
```

## 数组

数组是一种用于在单个变量中存储多个值的对象。数组中的每个值(也称为元素)都有一个数字位置，称为其**索引**，它可能包含任何数据类型的数据(数字、字符串、布尔值、函数、对象，甚至其他数组)。

数组索引从 0 开始，所以第一个数组元素是`arr[0]`。

```
let arr = ["I", "love", "freeCodeCamp"];

console.log(arr[2]); // Output: freeCodeCamp 
```

## 功能

函数是执行代码块的可调用对象。首先声明函数，并在函数中声明希望它执行的代码。之后，只要你想让它的代码执行，就调用这个函数。

因为函数是对象，所以可以将它们赋给变量，如下例所示:

```
let greeting = function () {
  return "Hello World!";
};

console.log(greeting()); // Output: Hello World! 
```

# 类型和 JavaScript 是怎么回事？

现在我们对什么是类型有了一个清晰的概念，我们可以开始讨论它是如何与 JavaScript 一起工作的——以及为什么一开始就需要像 TypeScript 这样的东西。

事实是 JavaScript 是一种松散类型的动态语言。
这意味着在 JavaScript 中，变量不直接与任何特定的值类型相关联，任何变量都可以被赋值(和重新赋值)为所有类型的值。

请参见以下示例:

```
let foo = 42; // foo is now a number
foo = "bar";  // foo is now a string
foo = true;   // foo is now a boolean 
```

您可以看到我们如何毫无问题地更改变量的内容和类型。

这是 JavaScript 创建时的设计，因为它是一种对程序员和设计人员都友好的脚本语言，只用于为网站添加功能。

但是 JavaScript 随着时间的推移成长了很多，不仅开始被用来为网站添加简单的功能，还被用来构建大型应用程序。当构建大型应用程序时，动态类型会导致代码库中出现愚蠢的错误。

让我们看一个简单的例子。假设我们有一个接收三个参数并返回一个字符串的函数:

```
const personDescription = (name, city, age) =>
  `${name} lives in ${city}. he's ${age}. In 10 years he'll be ${age + 10}`; 
```

如果我们以这种方式调用函数，我们会得到正确的输出:

```
console.log(personDescription("Germán", "Buenos Aires", 29));
// Output: Germán lives in Buenos Aires. he's 29\. In 10 years he'll be 39. 
```

但是，如果我们不小心将第三个参数作为字符串传递给函数，我们会得到错误的输出:

```
console.log(personDescription("Germán", "Buenos Aires", "29"));
// output: Germán lives in Buenos Aires. he's 29\. In 10 years he'll be **2910**. 
```

JavaScript 没有显示错误，因为程序无法知道函数应该接收什么类型的数据。它只接受我们给定的参数并执行我们编程的动作，与数据类型无关。

作为一名开发人员，很容易犯这样的错误，特别是在处理大型代码库和不熟悉函数或 API 所需的参数时。这正是 TypeScript 要解决的问题。

# 进来的是打字稿

TypeScript 于 2012 年推出。它是由微软开发和维护的。

在 TypeScript 中，就像在其他编程语言如 Java 或 C#中一样，每当我们创建一个数据结构时，我们都需要声明一个数据类型。

通过声明它的数据类型，我们为程序提供了信息，以便稍后评估分配给该数据结构的值是否与声明的数据类型匹配。

如果匹配，程序运行，如果不匹配，我们得到一个错误。这些错误是非常有价值的，因为作为开发人员，我们可以更早地发现错误。；)

让我们用 TypeScript 重复前面的例子。

在 TypeScript 中，我的函数应该是这样的(看起来完全一样，只是除了每个参数之外，我还声明了它的数据类型):

```
const personDescription = (name: string, city: string, age: number) =>
  `${name} lives in ${city}. he's ${age}. In 10 years he'll be ${age + 10}.`; 
```

现在，如果我尝试用错误的参数数据类型调用函数，我会得到以下错误输出:

```
console.log(personDescription("Germán", "Buenos Aires", "29"));
// Error: TSError: ⨯ Unable to compile TypeScript: Argument of type 'string' is not assignable to parameter of type 'number'. 
```

TypeScript 的美妙之处在于它仍然像 JavaScript 代码一样简单，我们只是向它添加了类型声明。这就是为什么 TypeScript 被称为 JavaScript 超集，因为 TypeScript only **向 JavaScript 添加了某些特性。**

# 打字稿基础

让我们看看 TypeScript 的语法，并学习如何使用它。

## 推理类型

有几种方法可以在 TypeScript 中声明类型。

我们将学习的第一个是**推断**，其中您根本不需要声明类型，但是 TypeScript 会为您推断(猜测)它。

假设我们像这样声明一个字符串变量:

```
let helloWorld = "Hello World"; 
```

如果稍后我试图将它重新分配给一个数字，我会得到以下错误:

```
helloWorld = 20;
// Type 'number' is not assignable to type 'string'.ts(2322) 
```

在创建变量并将其赋给特定值时，TypeScript 将使用该值作为其类型。

如[类型脚本文件](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)中所述:

> 通过理解 JavaScript 如何工作，TypeScript 可以构建一个接受 JavaScript 代码但具有类型的类型系统。这提供了一个类型系统，而不需要添加额外的字符来使类型在代码中显式。

在上面的例子中，TypeScript 就是这样“知道”`helloWorld`是一个字符串的。

虽然这是一个很好的特性，它允许您在没有任何额外代码的情况下实现 TypeScript，但它更具可读性，并且推荐您显式声明类型。

## 声明类型

声明类型的语法非常简单:只需在要声明的内容的右边添加一个冒号及其类型。

例如，声明变量时:

```
let myName: string = "Germán"; 
```

如果我尝试将它重新分配给一个数字，我会得到以下错误:

```
myName = 36; // Error: Type 'number' is not assignable to type 'string'. 
```

### 接口

当处理**对象**时，我们有一个不同的声明类型的语法，叫做**接口**。

一个接口看起来很像一个 JavaScript 对象——但是我们使用了 interface 关键字，我们没有等号或逗号，除了每个键之外，我们还有它的数据类型而不是它的值。

稍后，我们可以将该接口声明为任何对象的数据类型:

```
interface myData {
  name: string;
  city: string;
  age: number;
}

let myData: myData = {
  name: "Germán",
  city: "Buenos Aires",
  age: 29
}; 
```

再说一遍我把年龄作为字符串传递，我会得到下面的错误:

```
let myData: myData = {
  name: "Germán",
  city: "Buenos Aires",
  age: "29" // Output: Type 'string' is not assignable to type 'number'.
}; 
```

### 条件式

例如，如果我想设置一个有条件的键，允许它存在或不存在，我们只需要在界面中的键的末尾添加一个问号:

```
interface myData {
  name: string;
  city: string;
  age?: number;
} 
```

### 联盟

如果我希望一个变量能够被赋予多种不同的数据类型，我可以通过使用如下的**联合**来声明:

```
interface myData {
  name: string;
  city: string;
  age: number | string;
}

let myData: myData = {
  name: "Germán",
  city: "Buenos Aires",
  age: "29" // I get no error now
}; 
```

### 打字功能

在键入函数时，我们可以键入它的参数以及返回值:

```
interface myData {
  name: string;
  city: string;
  age: number;
  printMsg: (message: string) => string;
}

let myData: myData = {
  name: "Germán",
  city: "Buenos Aires",
  age: 29,
  printMsg: (message) => message
};

console.log(myData.printMsg("Hola!")); 
```

### 键入数组

对于类型化数组，语法如下:

```
let numbersArray: number[] = [1, 2, 3]; // We only accept numbers in this array
let numbersAndStringsArray: (number | string)[] = [1, "two", 3]; // Here we accept numbers and strings. 
```

**元组**是每个位置具有固定大小和类型的数组。它们可以这样建造:

```
let skill: [string, number];
skill = ["Programming", 5]; 
```

# TypeScript 的编译器

TypeScript 检查我们已经声明的类型的方式是通过它的**编译器**。编译器是一种将指令转换成机器代码或低级形式的程序，这样它们就可以被计算机读取和执行。

每次运行 TypeScript 文件时，TypeScript 都会编译我们的代码，并在那时检查类型。只有一切正常，程序才会运行。这就是为什么我们可以在程序执行之前检测到错误。

另一方面，在 JavaScript 中，类型是在运行时检查的。这意味着在程序执行之前不会检查类型。

值得一提的是，TypeScript **将代码转换成 JavaScript。**

> 翻译是将一种语言编写的源代码转换成另一种语言的过程。

浏览器不读取 TypeScript，但是它们可以执行 TypeScript 编写的程序，因为代码在构建时被转换为 JavaScript。

我们还可以选择要转换到哪种“风格”的 JavaScript，例如 es4、es5 等等。这个选项和许多其他选项可以从每次创建 TypeScript 项目时生成的`tsconfig.json`文件中进行配置。

```
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "target": "es6",
    "moduleResolution": "node",
    "sourceMap": true,
    "outDir": "dist"
  },
  "lib": ["es2015"]
} 
```

我们不会深入研究 TypeScript 的编译器，因为这只是一个介绍。但是要知道，您可以从这个文件和其他文件中配置大量的东西，并以这种方式使 TypeScript 适应您需要它做的事情。

# 如何创建 TypeScript 项目

我们可以通过在终端中运行几个命令来启动一个新的 TypeScript 项目。我们需要在系统中安装节点和 NPM。

一旦我们进入项目目录，我们首先运行`npm i typescript --save-dev`。这将安装 TypeScript 并将其保存为开发依赖项。

然后我们跑`npx tsc --init`。这将通过在您的目录中创建一个`tsconfig.json`文件来初始化您的项目。如前所述，这个 tsconfig.json 文件将允许您进一步配置和定制 TypeScript 和 tsc 编译器的交互方式。

您将看到这个文件附带了一组默认选项和大量注释掉的选项，因此您可以看到您所拥有的一切，并根据需要实现它。

```
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Enable incremental compilation */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./",                          /* Specify the folder for .tsbuildinfo incremental compilation files. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    ... 
```

仅此而已。然后我们可以创建一个扩展名为`.ts`的文件，并开始编写我们的类型脚本代码。每当我们需要将代码转换成普通的 JS 时，我们可以通过运行`tsc <name of the file>`来完成。

例如，我的项目中有一个包含以下代码的`index.ts`文件:

```
const personDescription = (name: string, city: string, age: number) =>
  `${name} lives in ${city}. he's ${age}. In 10 years he'll be ${age + 10}.`; 
```

运行`tsc index.ts`后，在同一目录下自动创建一个新的`index.js`文件，内容如下:

```
var personDescription = function (name, city, age) { return name + " lives in " + city + ". he's " + age + ". In 10 years he'll be " + (age + 10) + "."; }; 
```

很简单，对吧？=)

## 关于图书馆的评论

如果你正在使用 React，你应该知道 [create-react-app](https://create-react-app.dev/docs/adding-typescript/) 提供了一个 TypeScript 模板，所以当项目被创建时，TypeScript 已经为你安装和配置好了。

类似的模板也可用于 Node-Express 后端应用和 React 本地应用。

要做的另一个评论是，当使用外部库时，通常它们会为您提供特定的类型，您可以安装并使用这些类型来检查这些库。

例如，使用我提到的 create-react-app 的 TypeScript 模板，将安装以下依赖项:

`"@types/react":`

这将允许我们以如下方式键入我们的组件:

```
const AboutPage: React.FC = () => {
  return (
    <h1>This is the about page</h1>
  )
} 
```

我们将在将来更深入地了解如何使用带有 React 的 TypeScript。但首先，要知道这是存在的。；)

# TypeScript 的其他功能

TypeScript 也可以被认为是一个 **linter** ，一个在编写代码时向开发人员提供实时建议的工具。特别是当与 VS 代码结合时，TypeScript 可以根据我们声明的类型提出一些很好的建议，这些建议通常可以为我们节省时间和减少错误。

TypeScript 的另一个功能是作为一个自动文档工具。想象一下，你得到了一份新工作，你必须了解一个庞大的代码库。为每个函数声明类型在第一次使用它们时有很大的帮助，并且减少了任何项目的学习曲线。

# 综述

这些是 TypeScript 的基础。正如我们所看到的，这会给我们的代码增加一些样板文件。但是通过防止错误，帮助我们熟悉我们的代码库，并全面改善我们的开发体验，尤其是在大型复杂项目中工作时，它肯定是有回报的。

我希望你喜欢这篇文章，并学到一些新东西。如果你愿意，你也可以在 [LinkedIn](https://www.linkedin.com/in/germancocca/) 或 [Twitter](https://twitter.com/CoccaGerman) 上关注我。

干杯，下期再见！=D