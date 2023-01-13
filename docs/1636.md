# 什么是函数？JavaScript 函数示例

> 原文：<https://www.freecodecamp.org/news/what-is-a-function-javascript-function-examples/>

函数是计算机程序的主要部分之一。

它们被广泛使用，是 JavaScript 的基本构件之一。

在这篇文章中，我们将回顾函数的定义以及它们为什么如此重要。我还将向您展示如何开始用 JavaScript 编写函数。

让我们开始吧！

## JavaScript 中的函数是什么？

函数是一个代码块，它封装了一个孤立的、自包含的行为，供计算机执行。

功能是一组有组织的指令，对应于用户希望在程序中实现的特定任务或特定功能，以实现单一的预期结果。

函数内部的代码只在需要的时候运行，也就是说只在被*调用*的时候运行。

函数是编程中重要且有用的部分，因为它们创建了可重用的代码。

您可以使用函数只在一个地方编写代码，而不是在程序的不同部分复制、粘贴和重复相同的代码。然后你可以在任何需要的时候反复使用它。

当您想要实现对程序的更改或调试并尝试修复错误时，这也很有帮助。

你不必寻找你的代码可能在的不同部分，你只需要看一个特定的地方，使你的代码更可读。

## 如何在 JavaScript 中声明函数

在 JavaScript 中创建函数的一般语法如下:

```
function name(parameter1,parameter2,...) {
    // the code statements to be executed
} 
```

让我们来分解一下:

*   你用关键字`function`声明一个函数。
*   接下来，您给这个函数起一个自己选择的名字。JavaScript 中的函数名区分大小写，惯例和最佳实践是使用`camelCase`。
*   函数名后面是一组左括号和右括号。

函数能够通过*输入*接收数据。这些输入包含在括号中，称为*参数*。

参数充当值的局部占位符变量，这些值将在调用函数时作为输入传递给函数。它们完全是可选的，如果有多个，用逗号分隔。

*   最后是花括号，在花括号里是函数的主体，以及调用函数时要执行的代码语句。这是处理函数输入的地方。

### 如何在 JavaScript 中声明和调用一个简单的函数

```
 function greeting() {
  console.log('Hello World!');
} 
```

上面，我们创建了一个名为`greeting`的函数。

这是一个非常基本的功能，你不能用它做很多事情。它不接受任何输入，唯一发生的事情是文本`Hello World!`被打印到控制台。

在函数中定义函数本身并不运行函数体内的代码。对于要执行的代码，并且为了在控制台中看到该消息，必须调用函数。这也被称为*函数调用*。

要调用一个不接受输入的函数，你只需写下函数名，后跟括号和一个分号。

```
greeting();

//output
//Hello World! 
```

现在，您可以通过多次调用函数本身来多次重用该函数。这有助于避免重复代码:

```
greeting();
greeting();
greeting();

//output
// Hello World!
// Hello World!
// Hello World! 
```

### 如何在 JavaScript 中声明和调用带参数的函数

我们可以修改前面的例子来接受输入。如前所述，我们将通过参数来实现这一点。

参数是当函数被*声明为*时传递给函数的值。

```
function greeting(name) {
  console.log('Hello ' + name + ' !' );
} 
```

名为`greeting`的函数现在接受一个参数`name`。该字符串由字符串`Hello` 和一个感叹号连接在一起(`+`)。

当调用接受参数的函数时，需要传入参数。

参数是您在调用函数时提供的值，它们对应于在函数的十进制行中传递的参数。

例如:

```
greeting('Jenny');
//Output
// Hello Jenny ! 
```

参数是值`Jenny`，你可以把它想成`name = 'Jenny'`。参数`name`是占位符变量，`Jenny`是调用函数时传入的值。

函数可以接受多个参数，也可以将数据返回给程序的用户:

```
function addNums(num1,num2) {
    return num1 + num2;
} 
```

上面的代码创建了一个名为`addNums`的函数，它接受两个参数——`num1`和`num2`，用逗号分隔。

同样，函数有输入，它们也输出*输出*

函数*返回`num1`和`num2`之和*作为其输出。这意味着它处理这两个参数，进行所请求的计算，并将最终值作为结果返回给用户。

当调用该函数时，必须传递两个参数，因为它接受两个参数:

```
addNums(10,20);
//Output
// 30
// think of it as num1 = 10 and num2 = 20 
```

每次调用该函数时，可以传入不同的参数:

```
addNums(2,2);
// 4
addNums(3,15);
//18 
```

### JavaScript 函数中的变量范围

变量作用域指的是*可见*变量对于程序的不同部分是怎样的。

在函数块的外*和*前*定义的变量具有全局范围，可以从函数内部访问；*

```
const num = 7;

function myFunc() {
  console.log(num);
}

//Access the variable with a global scope from anywhere in the program:
console.log(num);
//Output
//7

//Call the function with the variable with global scope
myFunc();
//Output
// 7 
```

但是如果那个变量被定义在函数的内部，那么它将会有局部范围，并且它将会被限制并且只在定义它的函数中可见。

您不能从函数外部访问它:

```
function myFunc() {
  const num = 7;
  console.log(num);
}

// Try to access the variable with local scope from outside the function scope:
console.log(num);
//Otput:
//Uncaught ReferenceError: num is not defined

//Call the function with the variable defined inside the function:
myFunc();
//Ouput
//7 
```

### 函数表达式

您也可以使用表达式创建函数。

这些函数是在表达式中创建的，而不是像您到目前为止看到的那样用函数声明来创建的。

```
const name = function(firstName) {
  return 'Hello ' + firstName ;
  } 
```

这里，我们使用变量`name`来存储函数。

要调用该函数，可以像这样使用变量名:

```
console.log(name('Jenny'));
//Output
//"Hello Jenny" 
```

这种类型的函数也称为匿名函数，因为它们不需要名字。

下面列出了命名函数和匿名函数之间的区别:

```
 //named
function name(firstName) {
    console.log('Hello ' + firstName);
 }

name('Jenny');

//anonymous
const name = function(firstName) {
  return 'Hello ' + firstName ;
  }
 console.log(name('Jenny')); 
```

匿名函数中的变量也可以用作其他变量的值:

```
const name = function(firstName) {
  return 'Hello ' + firstName ;
  }

const myName = name('Timmy');
console.log(myName);
//Ouput
//"Hello Timmy" 
```

## 结论

现在你知道了！这标志着我们对 JavaScript 函数以及创建它们的一些方法的介绍到此结束。

如果你想了解更多，[箭头函数](https://www.freecodecamp.org/news/arrow-function-javascript-tutorial-how-to-declare-a-js-function-with-the-new-es6-syntax/)是在 JavaScript 中创建函数的一种新的更有效的方法，它们是在 ES6 中引入的。

如果你想开始交互式地学习 JavaScript 的基础知识，并在构建项目的过程中获得对该语言的全面理解，freeCodeCamp 有一个免费的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

感谢阅读，快乐学习！