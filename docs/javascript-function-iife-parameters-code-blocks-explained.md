# JavaScript 函数教程——解释生命、函数参数和代码块

> 原文：<https://www.freecodecamp.org/news/javascript-function-iife-parameters-code-blocks-explained/>

函数是编程中使用最广泛的特性之一。因此，对它们如何工作有一个坚实的理解是有帮助的。

本教程讨论了像专业人士一样使用 JavaScript 函数所需要知道的一切。

## 目录

1.  [什么是函数？](#what-is-a-function)
2.  [为什么是函数？](#why-functions)
3.  [JavaScript 函数的语法](#syntax-of-a-javascript-function)
4.  [什么是`function`关键词？](#what-is-a-function-keyword)
5.  [什么是函数名？](#what-is-a-function-name)
6.  [什么是参数？](#what-is-a-parameter)
7.  [什么是代码块？](#what-is-a-code-block)
8.  [什么是函数体？](#what-is-a-function-body)
9.  [JavaScript 函数的类型](#types-of-javascript-functions)
10.  [什么是 JavaScript 函数声明？](#what-is-a-javascript-function-declaration)
11.  [什么是 JavaScript 函数表达式？](#what-is-a-javascript-function-expression)
12.  [什么是 JavaScript 箭头函数表达式？](#what-is-a-javascript-arrow-function-expression)
13.  [什么是 JavaScript 立即调用函数表达式？](#what-is-a-javascript-immediately-invoked-function-expression)
14.  [概述](#overview)

所以，让我们从基础开始。

## 什么是函数？

JavaScript 函数是一段可执行的代码，开发者用它来捆绑一个由零个或多个语句组成的块。

换句话说，函数是一个可执行的子程序(迷你程序)。

JavaScript 函数是一个子程序，因为它的主体由一系列对计算机的语句(指令)组成，就像一个常规程序一样。

函数体中的指令可以是[变量](https://codesweetly.com/javascript-variable#example-3-javascript-variable)声明、[返回](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return)调用、 [console.log()](https://developer.mozilla.org/en-US/docs/Web/API/console/log) 调用、[函数](#syntax-of-a-javascript-function)定义，或者任何其他 JavaScript [语句](https://codesweetly.com/javascript-statement)。

**注:**

*   程序是为计算机执行而编写的一系列指令。
*   与其他[对象类型](https://codesweetly.com/javascript-non-primitive-data-type#types-of-javascript-objects)不同，您可以调用一个函数，而不需要将它存储在变量中。
*   JavaScript 函数类似于其他编程语言的过程或子例程。

## 为什么是函数？

函数提供了一种将代码段捆绑在一起的方法，并且可以随时随地无限期地重用它们。这有助于您消除重复编写同一组代码的负担。

比如， [alert()](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) 是一个[内置的窗口函数](https://developer.mozilla.org/en-US/docs/Web/API/Window)，有人写了一次，供所有开发者随时随地使用。

## JavaScript 函数的语法

```
function nameOfFunction(parameter1, parameter2, ..., parameterX) {
  // function's body
}
```

一项功能由五个要素组成:

1.  一个关键字
2.  函数的名称
3.  零个或多个参数的列表
4.  一个代码块(`{...}`)
5.  函数体

让我们讨论每一个元素。

## 什么是`function`关键词？

我们使用`function`关键字向浏览器声明一段特定的代码是 JavaScript 函数——而不是数学或其他通用函数。

## 什么是函数名？

一个**函数的名字**允许你为你的函数创建一个标识符，你可以用它来引用它。

## 什么是参数？

一个**参数**指定了您希望调用函数的[参数](https://codesweetly.com/javascript-arguments)的名称。

参数是函数的可选组件。换句话说，如果函数不接受任何参数，则不需要指定参数。

例如，JavaScript 的 [pop()](https://codesweetly.com/javascript-pop-method) 方法是一个没有任何参数的函数，因为它不接受参数。

另一方面， [forEach()](https://codesweetly.com/javascript-foreach-method) 有两个接受两个参数的参数。

### JavaScript 参数的示例

```
// Define a function with two parameters:
function myName(firstName, lastName) {
  console.log(`My full name is ${firstName} ${lastName}.`);
}

// Invoke myName function while passing two arguments to its parameters:
myName("Oluwatobi", "Sofela");

// The invocation above will return:
"My full name is Oluwatobi Sofela."
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-edvj3g?devToolsHeight=33&file=index.js)

上面代码片段中的`myName()`函数有两个参数:`firstName`和`lastName`。

假设您希望为您的参数预定义值，如果用户没有使用所需的参数调用函数，浏览器可以使用这些值。在这种情况下，您可以创建默认参数。

### 什么是默认参数？

**默认参数**允许你用默认值初始化你的函数参数。

例如，假设用户在没有提供必需参数的情况下调用您的函数。在这种情况下，浏览器会将参数值设置为`undefined`。

但是，默认参数允许您定义浏览器应该使用的值，而不是`undefined`。

### 默认参数的示例

下面是默认参数在 JavaScript 中如何工作的例子。

#### 如何定义没有默认参数的函数

```
// Define a function with two parameters:
function myName(firstName, lastName) {
  console.log(`My full name is ${firstName} ${lastName}.`);
}

// Invoke myName function while passing one argument to its parameters:
myName("Oluwatobi");

// The invocation above will return:
"My full name is Oluwatobi undefined."
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-9ce3xt?devToolsHeight=33&file=index.js)

计算机自动将`lastName`参数设置为`undefined`，因为我们没有提供默认值。

#### 如何定义一个带有`undefined`参数而没有默认参数的函数

```
// Define a function with two parameters:
function myName(firstName, lastName) {
  console.log(`My full name is ${firstName} ${lastName}.`);
}

// Invoke myName function while passing two arguments to its parameters:
myName("Oluwatobi", undefined);

// The invocation above will return:
"My full name is Oluwatobi undefined."
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-csbxta?devToolsHeight=33&file=index.js)

计算机将`lastName`参数设置为`undefined`，因为我们提供了`undefined`作为`myName()`的第二个参数。

#### 如何定义带有默认参数的函数

```
// Define a function with two parameters:
function myName(firstName, lastName = "Sofela") {
  console.log(`My full name is ${firstName} ${lastName}.`);
}

// Invoke myName function while passing one argument to its parameters:
myName("Oluwatobi");

// The invocation above will return:
"My full name is Oluwatobi Sofela."
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-ghiyzm?devToolsHeight=33&file=index.js)

JavaScript 使用`"Sofela"`而不是`undefined`作为`lastName`参数的默认参数。

#### 如何用一个`undefined`参数和一个默认参数定义一个函数

```
// Define a function with two parameters:
function myName(firstName, lastName = "Sofela") {
  console.log(`My full name is ${firstName} ${lastName}.`);
}

// Invoke myName function while passing two arguments to its parameters:
myName("Oluwatobi", undefined);

// The invocation above will return:
"My full name is Oluwatobi Sofela."
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-umqgqp?devToolsHeight=33&file=index.js)

JavaScript 使用`"Sofela"`而不是`undefined`作为`lastName`参数的默认参数。

现在让我们讨论 JavaScript 函数的第四个元素:一个**代码块。**

## 什么是代码块？

一个**块**是一对大括号(`{...}`)，用于将多个语句组合在一起。

**这里有一个例子:**

```
{
  const hourNow = new Date().getHours();
}
```

上面代码片段中的代码块包含了一个 JavaScript 语句。

这里还有一个例子:

```
if (new Date().getHours() < 18) {
  const hourNow = new Date().getHours();
  const minutesNow = new Date().getMinutes();
  console.log(`The time is ${hourNow}:${minutesNow}.`);
}
```

`if`条件的代码块将三个 JavaScript 语句组合在一起。

现在，考虑这个片段:

```
function getTime() {
  const hourNow = new Date().getHours();
  const minutesNow = new Date().getMinutes();
  console.log(`The time is ${hourNow}:${minutesNow}.`);
}
```

`getTime()`函数的代码块将三个 JavaScript 语句组合在一起。注意，“函数体”是函数代码块内部的空间。现在就多说说吧。

## 什么是函数体？

一个**函数体**是放置你想要执行的一系列语句的地方。

### JavaScript 函数体的语法

```
function nameOfFunction() {
  // function's body
} 
```

### 函数体示例

下面是我们如何使用函数体的例子。

#### 如何定义一个函数体中有三条语句的函数

```
function getName() {
  const firstName = "Oluwatobi";
  const lastName = "Sofela";
  console.log(firstName + " " + lastName);
} 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-flz5tf?devToolsHeight=33&file=index.js)

在上面的代码片段中，函数体包含三条语句，每当调用该函数时，计算机都会执行这些语句。

**注意:** `console.log()`是一个[方法](https://codesweetly.com/web-tech-terms-m#method)，我们用它来记录(写)消息到 web 控制台。

#### 如何定义一个函数体中有两条语句

```
const bestColors = ["Coral", "Blue", "DeepPink"];

function updateMyBestColors(previousColors, newColor) {
   const mybestColors = [...previousColors, newColor];
   return mybestColors;
}

updateMyBestColors(bestColors, "GreenYellow");
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-nb9sr6?devToolsHeight=33&file=index.js)

在上面的代码片段中，函数体包含两条语句，每当调用该函数时，计算机都会执行这两条语句。

**注:**

*   我们加在`previousColors`前面的三个点叫做[扩散算子](https://codesweetly.com/spread-operator)。我们用它将[数组](https://codesweetly.com/javascript-array-object)参数扩展成单个元素。
*   如果您希望添加两种或更多新颜色，您也可以在`newColor`参数前加上一个 [rest 操作符](https://codesweetly.com/javascript-rest-operator)。
*   `return`关键字结束其函数的执行，并返回指定的[表达式](https://codesweetly.com/javascript-statement#what-is-a-javascript-expression-statement)(如果没有提供表达式，则返回`undefined`)。

所以，现在我们知道了 JavaScript 函数的组件，我们可以讨论它的类型了。

## JavaScript 函数的类型

四种类型的 JavaScript 函数是:

*   函数声明
*   函数表达式
*   箭头函数表达式
*   立即调用函数表达式

让我们讨论每一种类型。

## 什么是 JavaScript 函数声明？

**函数声明**是一个没有给[变量](https://codesweetly.com/javascript-variable)赋值就创建的函数。

**注:**我们有时称函数声明为“函数定义”或“函数语句”

**这里有一个例子:**

```
function addNumbers() {
  return 100 + 20;
} 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-pwh2jr?devToolsHeight=33&file=index.js)

上面的函数是一个函数声明，因为我们没有将它存储在变量中就定义了它。

## 什么是 JavaScript 函数表达式？

**函数表达式**是你创建并赋给变量的函数。

**这里有一个例子:**

```
const myFuncExpr = function addNumbers() {
  return 100 + 20;
}; 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-hjg1iq?devToolsHeight=33&file=index.js)

上面的函数是一个名为的**函数表达式，我们将其赋给了`myFuncExpr`变量。**

您也可以将上面的代码片段写成一个**匿名**函数表达式，如下所示:

```
const myFuncExpr = function() {
  return 100 + 20;
}; 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-pmeqy4?devToolsHeight=33&file=index.js)

上面的函数是一个匿名函数表达式，我们将其赋给了`myFuncExpr`变量。

**注:**

*   **匿名函数**是没有名字的函数。
*   一个**命名函数**是一个有名字的函数。

命名函数的主要优点是它的名字使得追踪错误的来源更加容易。

换句话说，假设您的函数抛出了一个错误。在这种情况下，如果函数被命名，调试器的[堆栈跟踪](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/stack)将包含函数的名称。因此，您会发现识别错误的来源更容易。

## 什么是 JavaScript 箭头函数表达式？

一个**箭头函数表达式**是一种写函数表达式的简写方式。

### 箭头函数的语法

我们用等号和大于号(`=>`)定义了一个箭头函数。下面是语法:

```
const variableName = () => {
  // function's body
} 
```

### 箭头函数的示例

```
const myFuncExpr = () => {
  return 100 + 20;
}; 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-1euhch?devToolsHeight=33&file=index.js)

您可以看到，我们在定义函数时没有使用`function`关键字和函数名。

在编写箭头函数表达式时，必须省略`function`关键字和函数名。否则，JavaScript 会抛出一个`SyntaxError`。

### 关于 JavaScript 箭头函数表达式需要了解的重要信息

使用箭头函数表达式时，需要记住三个基本事实。

#### 1.参数的括号是可选的

假设您的 arrow 函数只包含一个参数。在这种情况下，可以省略括号。

```
const myFuncExpr = a => {
  return a + 20;
}; 
```

[**在 CodePen 上试试**](https://codepen.io/oluwatobiss/pen/OJQJejr)

#### 2.花括号和`return`关键字是可选的

假设您的 arrow 函数只包含一条语句。在这种情况下，您可以省略它的花括号和关键字`return`。

```
const myFuncExpr = (x, y) => x + y;
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-8t2udu?devToolsHeight=33&file=index.js)

在上面的代码片段中，我们通过删除花括号和`return`关键字隐式返回了参数`x`和`y`的总和。

**注意:**无论何时你选择省略花括号，也要确保你删除了`return`关键字。否则电脑会抛出一个`SyntaxError`。

#### 3.使用括号将任何隐式对象返回括起来

假设您希望隐式返回一个对象。在这种情况下，将对象包装在一个[分组操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Grouping) `(...)`中。

例如，考虑下面的代码:

```
const myFuncExpr = () => {
  carColor: "White",
  shoeColor: "Yellow",
}; 
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/wrong-way-to-use-an-arrow-function-expression-to-return-an-object-implicitly-s8w132?file=/src/index.js)

上面的代码片段将抛出一个`SyntaxError`，因为 JavaScript 假定花括号(`{}`)是函数体的代码块，而不是[对象文字](https://codesweetly.com/javascript-properties-object)。

因此，无论何时您希望隐式地返回一个对象文字——而不是显式地使用`return`关键字——请确保将对象文字包含在一个分组操作符中。

**这里有一个例子:**

```
const myFuncExpr = () => ({
  carColor: "White",
  shoeColor: "Yellow",
}); 
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/correct-way-to-use-an-arrow-function-expression-to-return-an-object-implicitly-p61l5c?file=/src/index.js)

请注意，您可以使用分组运算符返回任何单个值。例如，下面的代码片段对`x`和`56`的总和进行了分组。

```
const myFuncExpr = x => (x + 56);
```

[**在 CodePen 上试试**](https://codepen.io/oluwatobiss/pen/rNJNXeG)

现在让我们讨论第四种类型的 JavaScript 函数。

## 什么是 JavaScript 立即调用函数表达式？

一个**立即调用的函数表达式(life)**是自动调用自身的函数表达式。

**注:**我们有时称一个生命为“自调用函数表达式”或“自执行匿名函数表达式”

### 生活的语法

```
(function() {
  /* ... */
})(); 
```

生活由三个主要部分组成:

1.  **一个分组运算符:**第一对括号`()`
2.  **一个函数:**括在分组运算符内
3.  **一个 invocator:** 最后一对括号`()`

### 例子

以下是生活的例子。

#### 如何定义命名生命

```
(function addNumbers() {
  console.log(100 + 20);
})(); 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-nkjjnz?devToolsHeight=33&file=index.js)

上面代码片段中的函数是一个命名的自调用函数表达式。

#### 如何定义匿名生活

```
(function() {
  console.log(100 + 20);
})(); 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-ywrmkt?devToolsHeight=33&file=index.js)

上面代码片段中的函数是一个匿名的自调用函数表达式。

#### 如何定义箭头函数 IIFE

```
(() => console.log(100 + 20))();
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-xqlfgi?devToolsHeight=33&file=index.js)

上面代码片段中的函数是一个箭头自调用函数表达式。

#### 如何定义异步生活

```
(async () => console.log(await 100 + 20))();
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-ryxftq?devToolsHeight=33&file=index.js)

上面代码片段中的函数是一个[异步](https://codesweetly.com/asynchronous-javascript)自调用函数表达式。

所以，现在我们知道了什么是立即调用的函数表达式，我们可以讨论它是如何工作的。

### 生活是如何运作的？

默认情况下，计算机不知道什么是[数据类型](https://codesweetly.com/javascript-data-types) [代码](https://codesweetly.com/document-vs-data-vs-code#what-exactly-is-code)，直到它评估它。

例如，假设你要求计算机处理`4`。在这种情况下，系统在评估之前不会知道`4`是否是一个数字类型。

因此，如果你直接在`4`上使用任何数字方法，JavaScript 都会抛出一个`SyntaxError`。

**这里有一个例子:**

```
// Convert 4 to a string value:
4.toString();

// The invocation above will return:
"Uncaught SyntaxError: Invalid or unexpected token" 
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/how-iife-works-example-1-kc8g5b)

计算机抛出了一个`SyntaxError`，因为它不识别`4`为数字数据类型。

然而，假设你将`4`赋给一个变量。在这种情况下，计算机会先将其转换为数字数据类型，然后再将其存储到变量中。

之后，JavaScript 将允许您在数字变量上使用任何数字方法。

**这里有一个例子:**

```
// Assign 4 to a variable:
const myNum = 4;

// Convert myNum's content to a string value:
myNum.toString();

// The invocation above will return:
"4" 
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/how-iife-works-example-2-jz4kwi)

上面的代码片段没有返回任何错误，因为 JavaScript 将`myNum`评估为数字数据类型。

但是在计算机能够适当地评估其数据类型之前，您不必将`4`赋给变量。

你也可以把你的值放在[括号](https://www.computerhope.com/jargon/p/parenthe.htm)中，强迫计算机在使用它做其他事情之前评估它的数据类型。

例如，考虑以下代码片段:

```
// Evaluate 4's data type and turn it into a string value:
(4).toString();

// The invocation above will return:
"4" 
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/how-iife-works-example-3-rz4j9b)

上面的代码片段将`4`括在括号中，以使计算机在使用 [`toString()`](https://codesweetly.com/javascript-number-tostring-method) 方法将其转换为字符串值之前评估其数据类型。

在直接调用函数表达式(IIFE)中，首先使用括号让 JavaScript 评估代码的数据类型。

例如，考虑这个例子:

```
// Evaluate the function's data type and immediately invoke it:
(function addNumbers() {
  console.log(100 + 20);
})();

// The invocation above will return:
120 
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-nkjjnz?devToolsHeight=33&file=index.js)

上面的代码片段用括号将`addNumbers`函数括起来，让计算机在评估之后立即调用它之前评估它的数据类型。

## 概观

在本文中，我们讨论了什么是 JavaScript 函数对象。我们还用例子来看看它是如何工作的。

感谢阅读。

### 这里有一个有用的资源:

我写了一本关于 React 的书！

*   这是初学者友好的✔
*   它有✔的现场代码片段
*   它包含可扩展的项目✔
*   ✔有很多容易理解的例子

[React 解释清楚](https://www.amazon.com/dp/B09KYGDQYW)的书就是你理解 ReactJS 所需要的全部。

[![React Explained Clearly Book Now Available at Amazon](img/01a810717269c181d904ccbfc9224fa0.png)](https://www.amazon.com/dp/B09KYGDQYW)