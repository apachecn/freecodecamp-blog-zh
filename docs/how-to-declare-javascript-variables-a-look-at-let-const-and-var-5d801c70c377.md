# 如何声明 JavaScript 变量:看看 let、const 和 var

> 原文：<https://www.freecodecamp.org/news/how-to-declare-javascript-variables-a-look-at-let-const-and-var-5d801c70c377/>

在旧的 JavaScript 中，我们只有一种方法来声明变量，那就是用`var`，就像`var x = 10`。它将创建一个名为 x 的变量，并将值 10 赋给它。现在有了现代的 ES6 JavaScript，我们有 3 种不同的方法来声明变量:`let`、`const`和`var`。我们后面会讲到`let` & `const`。现在，我们来关注一下`var`。

## 定义变量

我们已经知道如何用`var`声明一个变量。现在让我们参考一些代码来正确理解`var`。

```
var x = 20;
function foo() {
    var y = 10;
    console.log(x);
    console.log(y);
}
foo(); // will print 20 and 10
console.log(x); // will print 20
console.log(y); // will throw a reference error
```

熟悉 C 或 C++的人可能会理解为什么输出是这样的。这是因为`x`在全局范围内，而`y`在函数 foo 的范围内。由于函数`foo`可以访问全局范围，从函数内部我们可以访问`x`和`y`。打印`x`也很顺利，因为`x`在全球范围内，我们可以从任何地方访问它。当我们试图从全局范围访问`y`时会出错，因为`y`仅限于函数范围。

类似于 C 或者 C++对吧？不。让我们看看为什么不。

```
var x = 20;
function foo() {
    var y = 10;
    {
        var z = 30;
    }
    console.log(x);
    console.log(y);
    console.log(z);
}
foo();
```

你认为代码的输出会是什么？如果你认为第`console.log(z)`行会有引用错误，那么从 C 或 C++的角度来看，你是正确的。但是使用 JavaScript，情况就不一样了。上面的代码将打印 20 10 30。

这是因为在带有`var`的 JavaScript 中，不像在 C 和 C++中，我们没有任何块级作用域。我们只有全局和功能级别的范围。所以`z`属于函数 foo 的范围。

现在我们又有了一个例子:

```
var x = 20;
var x = 30;
console.log(x); // this will print 30
```

在 C 或 C++中，如果我们在同一个范围内多次声明一个变量，我们会得到一个错误。但是 JavaScript 中的`var`却不是这样。在上面的例子中，它只是重新定义了`x`并赋值为 30。

让我们考虑下面的代码片段:

```
function foo() {
    x = 20;
    console.log(x);
}
foo();
console.log(x);
```

上面的代码将打印 20 ^ 20。那么这里发生了什么？如果你在没有关键字`var`的地方声明一个变量，它将成为全局范围的一部分。从`foo`的内部和外部均可进入。

```
'use strict'
function foo() {
    x = 20;
    console.log(x);
}
foo();
console.log(x);
```

在上面的代码中，我们使用的是严格模式。在严格模式下，不允许使用`x = 20`类型的声明。它将抛出一个引用错误。你必须使用`var`、`let`或`const`来声明一个变量。

## 让

现在是时候看一看`let`了。`let`是 ES6 中的新 var，但有一些不同。

```
let x = 20;
function foo() {
    let y = 10;
    {
        let z = 30;
    }
    console.log(x);
    console.log(y);
    console.log(z);
}
foo();
```

还记得在 JavaScript 中，`var`没有任何块级作用域吗？现在块级作用域又回到了`let`中。如果您执行上面的代码，您将在`console.log(z)`行得到一个引用错误。用`let`声明的变量`z`现在在一个不同的块级范围内，在这个范围之外是不可访问的。

```
let x = 10;
let x = 20; // will throw an error
```

不允许用`let`重新声明变量。

```
var x = 10;
let y = 20;
console.log(window.x); // 10
console.log(window.y); // undefined
```

用`var`全局声明的全局变量被添加到`global`对象中，在浏览器中是`window`。用 let 全局声明的变量不会添加到`window`(全局对象)。虽然它们在全球范围内都可以访问，但就好像它就在那里，只是你看不见。

```
console.log(x); //undefined
console.log(y); //reference error
var x;
let y;
```

与`var`不同的是，`let`变量在它们的定义被评估之前不会被初始化为 undefined。如果在此之前尝试访问该变量，将会遇到引用错误。这也被称为时间死区。简单来说，吊装只有`var`才有，`let` & `const`没有。

## 常数

`const`代表常数，与`let`非常相似。唯一的区别是它的值不能改变，并且需要在声明它的地方进行初始化。

```
const x = 20;
console.log(x); // will print 20
x = 30 // will throw an error
```

并不是说在`const`对象的情况下你可以改变那个对象的属性——只是你不能重新分配一个`const`变量。

```
const obj = {firstName: "James", lastName: "Bond"}
console.log(obj); // will print the obj object
obj.firstName = "Ruskin";
console.log(obj); // will print the obj object, it has new firstName
obj = {firstName: "James", lastName: "Bond"}; // will throw an error
```

如前所述，你必须初始化一个`const`变量，你不能让它不初始化。

```
const x; // will throw an error
some other code;
```

本文到此结束，再见！

## 感谢您的阅读:)