# JavaScript 中有哪些函数？初学者指南

> 原文：<https://www.freecodecamp.org/news/what-are-functions-in-javascript-a-beginners-guide/>

函数是编程中的基本概念之一。它们让我们编写简洁、模块化、可重用和可维护的代码。它们还帮助我们在编写代码时遵守 DRY 原则。

在本文中，您将了解 JavaScript 中有哪些函数，如何编写自己的自定义函数，以及如何实现它们。

作为先决条件，您应该熟悉一些基本的 JavaScript 概念，如变量、表达式和条件语句，以便阅读本文。

## JavaScript 中的函数是什么？

函数是为执行特定任务而编写的可重用代码块。

你可以把一个函数看作是主程序中的一个子程序。函数由一组语句组成，但作为一个单元执行。

在 JavaScript 中，我们有一些浏览器内置的函数，比如 alert()、prompt()和 confirm()。你可能以前在你的项目中使用过这些，对吗？但是您仍然可以创建自己的自定义函数。

定义函数有几种方法。最常见的是，我们有函数声明和函数表达式。

## 如何使用函数声明定义函数

您可以像这样编写一个函数声明:

```
function nameOfFunction() {
	//some code here....
}
```

基本上，它包括以下内容:

*   函数关键字
*   函数的名称
*   括号(可以带参数，也可以是空的)
*   函数体(用花括号括起来)。

这里有一个例子:

```
function sayHello() {
	console.log("Hello world"); 
}
```

这个函数不会做任何事情——在这个例子中，输出*Hello world*——除非你调用它。这个术语叫做*调用函数。*

下面是调用该函数的方法:

```
sayHello();

//output: Hello world
```

这是另一个例子:

```
function sum(num1, num2){
	return num1 + num2;
} 
```

要调用这个函数，我们像这样调用它:

```
sum(1, 2);

//output: 3
```

您可以看到我们的第一个函数示例和第二个函数示例之间的细微差别。

如果你猜是第二个函数括号内的内容，那么你就对了！

当我们定义函数`sum()`时，它接受了两个参数——`num1`和`num2`。当我们调用它时，我们传入两个值——参数，`1`和`2`。让我解释一下这两个术语(参数和自变量)的含义。

一个**参数**是当你声明一个函数时传递给它的一个变量。

假设您希望函数是动态的，以便在不同的时间将函数的逻辑应用于不同的数据集。这就是参数派上用场的地方。这样，你的函数不只是重复输出相同的结果。相反，它的结果取决于您传入的数据。

另一方面，**参数**是当你调用函数时传递给它的参数的等价值。

因此，声明带参数的函数的语法如下所示:

```
function nameOfFunction(parameters){
	//function body.....
}
```

要调用它:

```
nameOfFunction(arguments)
```

## 如何使用函数表达式定义函数

函数表达式是定义函数的另一种符号。在语法方面，它类似于函数声明。但是函数表达式允许您定义一个命名函数或省略函数名来创建一个匿名函数。

让我们看看函数表达式是什么样子的:

```
let namedFunction = function myFunction(){
	//some code here...
}
```

注意，在这个例子中，函数有一个名字`myFunction`。匿名函数不是这种情况。定义匿名函数时，可以省略函数名，如下例所示:

```
let anonymousFunction = function(){
	//some code here...
}
```

你可以看到两个函数例子都赋给了一个变量，对吧？

**function 关键字创建一个函数值，当它被用作表达式**时，该函数值可以被分配给一个变量。

因此，为了调用这个函数，我们使用变量名作为新的函数名来调用它。

函数声明和函数表达式之间的一个主要区别是，使用函数声明，您甚至可以在定义函数之前就调用它。这在函数表达式中是不可能的。

例如:

```
console.log(greeting());

function greeting(){
  console.log("Hope you're are good?");

}
//output: Hope you're good?
```

如果函数被定义为函数表达式，这将不起作用，因为函数表达式遵循自顶向下的控制流序列。

## 如何在 JavaScript 中使用箭头函数

箭头函数是函数表达式的另一种表示法，但它们的语法更短。它们是在 ES6 中引入的，帮助我们编写更简洁的代码。

这里，function 关键字被排除，我们使用一个箭头符号(= >)来代替。语法如下所示:

```
let nameOfFunction = (parameters) => {
	//function body
} 
```

如果花括号内的函数体只包含一个语句，那么花括号可以省略。带花括号的箭头函数必须包含 return 关键字。

## 什么是立即调用的函数表达式？

IIFE 是另一种独立工作的函数表达式符号(显式匿名函数),独立于任何其他代码。它在被定义的地方被立即调用。

语法如下:

```
(function (){
	//function body
})();
```

IIFE 的一个用例是在你的代码中包含一个你可能不会再使用的变量。所以，一旦函数被执行，变量就不存在了。

## **如何在函数中使用 Return 关键字**

要创建一个在函数被调用后将解析为一个值的函数，可以使用 return 关键字。你把它写在函数体内。

**`return`** 是一个指令，在执行完函数中的代码后，将一个值返回给函数。

下面是一个返回值的函数示例，在本例中，是两个数的和:

```
function sum(a, b){
	return  a + b;
}

sum(10, 20);

//output will be 30 
```

在函数中使用`return`使得操作函数返回的数据变得容易，可以将数据作为一个值传递给另一个函数，或者对其执行额外的操作。

## JavaScript 中的**函数** S **cope 和闭包**是如何工作的？

作用域是一个嵌套的名称空间，它将在作用域内创建的名称本地化，这样这些名称就不会干扰在该作用域外创建的类似名称。在一个函数中有一些特定的作用域规则。

您定义的每个新函数都会创建一个新的作用域，称为**函数作用域**。在函数作用域内创建的变量在该作用域外是不可见或不可访问的。

然而，在函数作用域之外但在定义函数的作用域之内创建的变量可以在函数内部访问。因此，如果在全局范围内定义一个函数，它可以访问在该全局范围内声明的所有变量。

此外，假设您有一个嵌套在父函数(即外部函数)中的子函数(即内部函数)。子函数可以访问其父函数中声明的所有变量和函数，以及父函数可以访问的所有变量和函数——即使其父函数已经完成执行，并且其变量在该函数之外不再可访问。这个概念在 JavaScript 中被称为闭包。

但是，父函数不能访问在子函数中创建的变量。这样，子函数中的变量和函数就被限制在它们自己的范围内。

让我们看一个这样的代码示例:

```
//variables defined in the global scope

let  a = 40;
let b = 20;

//this function is also defined in the global scope

function parentFunc(){
	//access variables a and b inside this function

	console.log(a + b); 
}

//output: 60 
```

假设我在父函数中嵌套了一个内部函数，如下所示:

```
//variables defined in the global scope

let a = 40;
let b = 20;

//this function is also defined in the global scope

function parentFunc(){
	let  name = “Doe”;

    //this inner function is defined inside the parent function scope

	function childFunc(){
		console.log(`${name} is ${a - b} years old`); 
      }
    return childFunc();
}

parentFunc(); //ouput: Doe is 20 years old 
```

现在，如果我在一个函数中创建一个变量，并试图从全局范围访问它，我们将得到一个引用错误。这是因为该变量对于函数范围是局部的，对于全局范围是不可见的。

```
console.log(c);

function parentFunc(){
	let c = 30
} 

//output: reference error - c is not defined
```

让我们尝试访问在父函数的嵌套函数中创建的变量:

```
function parentFunc(){
	console.log(age);
	function childFunc(){
		let  age = 20;
	} 
    return childFunc();
}

parentFunc(); //output: reference error - age is not defined. 
```

## JavaScript 中的默认参数是如何工作的？

最初，当没有值被显式传递给函数参数时，函数参数被赋值为 *undefined* 。默认参数允许您在定义函数时为参数指定默认值。例如:

```
function greeting(name, message = ”Hello”){
	console. log(`${messgae}  ${name}`)
}

greeting(‘John’); //output: Hello John

//you can also assign a new value to the default parameter 
//when you call the function

greeting(‘Doe’, ‘Hi’); //output: Hi Doe 
```

需要注意的是，在声明默认参数时，它必须在常规参数之后。

## **Rest 参数在 JavaScript 中是如何工作的？**

使用 rest 参数，您可以定义一个函数在单个数组中存储多个参数。这在调用带有多个参数的函数时特别有用。这里有一个例子:

```
function sayHello(message, ...names){
  names.forEach(name => console.log(`${message} ${name}`));
}

sayHello('Hello', "John", "Smith", "Doe");

/*
output:

Hello John

Hello Smith

Hello Doe 

*/ 
```

`...`是使`names`成为静止参数的原因。

就像默认参数一样，rest 参数应该出现在函数中任何常规参数之后。

## 结论

在本文中，您了解了 JavaScript 中的函数是什么，以及如何编写自己的函数。

使用函数，您可以通过将所有内容分组到执行不同任务的独立块中来组织代码。

我希望你喜欢阅读这篇文章。要了解关于函数的更多信息，您可以参考以下资源:

*   [JavaScript 函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
*   [关闭](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

这一块就这么多了。快乐编码:)