# javascript 中作用域和宿主的简要回顾

> 原文：<https://www.freecodecamp.org/news/a-brief-review-of-scoping-and-hoisting-in-javascript-e74c38283b65/>

蒂亚戈·罗梅罗·加西亚

# javascript 中作用域和宿主的简要回顾

![0PVN1ZvZ0uPX8QzwuwMpJ5C8UjzjD4wzPW8K](img/44ef2a29456aeec3553c51683f865055.png)

Hoisting is common to both Mariners and Javascript developers.

我敢打赌，任何 JavaScript 开发人员都希望更好地理解范围和提升的概念。它们会悄悄地产生这些可怕的无法解释的问题，也称为副作用。

简而言之，作用域和提升会影响我们编写的代码如何处理我们的声明(比如 var、`let`、`const`和`function`)。

让我们从第一个开始回顾。

### 处理风险值

当使用`var`声明变量时，声明变量的父函数是你唯一的*事实上的*范围分隔符。通过这种方式，父函数创建并保存了在其内部声明的所有局部变量的范围。

换句话说，在父函数内部，局部变量诞生了，它们完成了它们的工作，当函数执行结束时，它们也消亡了(除非它们被传递给比父函数活得更久的其他函数)。

这就是**局部作用域的定义。一个**与**全局作用域**相反，当变量在你的函数之外被声明时。任何人、任何地方都可以接触到它们。它们像我们呼吸的空气一样无所不在，或者像浏览器中的`window`对象一样无所不在。

因此，作为条件和循环的其他代码块(如`if`、`for`、`while`、`switch`和`try`)不像大多数其他语言那样限定范围。

然后，这些块中的任何`var`都将在其包含该块的父函数范围内。

不仅如此，在运行时，在这样的代码块中发现的每个`var`声明都被移动到其父函数(其作用域)的开头。这就是**吊装**的定义。

既然如此，你不应该在一个块中声明一个`var`并认为这个`var`没有向外泄漏，因为它可能会泄漏！

这里有一个例子:

```
function stepSum() {  var total = 0;
```

```
 for (var i = 0; i < arguments.length; i++) {    var parameter = arguments[i];     total += parameter;    console.log(`${i}) adding ${parameter}`);  }
```

```
 // outside the loop, we can still access vars i and parameter  // even though the were declared within the for loop  console.log(`i=${i}, parameter=${parameter}`);  return total;}
```

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

输出是:

```
0) adding 31) adding 22) adding 1i=3, parameter=1total=6
```

这里，我们可以观察到变量`i`和`parameter`正在泄漏，因为它们都可以从父函数访问。那是因为它们被悬挂在那里，就像这样宣布的:

```
function stepSum() {  var total = 0;  var i;  var parameter;
```

```
 for (i = 0; i < arguments.length; i++) {    parameter = arguments[i];     total += parameter;    console.log(`${i}) adding ${parameter}`);  }
```

```
 console.log(`i=${i}, parameter=${parameter}`);  return total;}
```

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

为了避免混淆，通常的做法是在父函数的第一行声明变量。这样做是为了避免对任何`var`的错误预期，它可能在函数的某个地方被声明，但碰巧在此之前已经保存了一个值。

对于来自具有块范围的语言(如 C 或 Java)的程序员来说，这是一个困惑的来源。他们通常在第一次使用时声明他们的变量。

#### 与`var`有关的问题

让我们考虑这个代码片段。它与上一个类似，只是它异步计算每个总和:

```
function stepSum() {  var total = 0;
```

```
 for (var i = 0; i < arguments.length; i++) {    var parameter = arguments[i];
```

```
 setTimeout(function() {      total += parameter;      console.log(`${i}) adding ${parameter}, total=${total}`);    }, i*1000);  }}
```

```
stepSum(3, 2, 1);
```

输出是:

```
3) adding 1, total=13) adding 1, total=23) adding 1, total=3
```

为什么会这样？它只对最后一个参数 1 求和三次。步数也总是 3，我们会看到 1，2 和 3。这里出了什么问题？

答案如下:变量`i`和`parameter`被提升到`stepSum`函数的开头，现在它们可用于整个父函数。不仅如此，`parameter`实际上只被定义了一次，然后在 for 循环的每次迭代中被重新赋值。

假设我们在这里使用了`setTimeout`调用，我们现在可以预期，当这个函数第一次执行时(一秒钟后)，`stepSum`函数将已经完成。所以`parameter`以它最后一次赋值的值结束，这个值来自 for 循环的最后一次迭代，当时它被设置为 1。同样的事情还有`i`以值 3 结束。

这就是为什么当 3 个`setTimeout`调用最终被执行时，这些值被拾取。

怎么才能修好？简单地通过很好地利用范围和提升。我们可以提供一个新的函数作用域来保护`i`和`parameter`不被重新分配。这为它们创建了一个本地范围。也许通过使用除 var 之外的东西，也可以给我们一个块内的局部范围，正如我们接下来看到的。

### 处理 let 和 const

ES2015 引入了`let`和`const`，它们是尊重块范围的变量。这意味着在块内声明它们是安全的，不会泄漏到外部，如下例所示:

```
function stepSum() {  let total = 0;
```

```
 for (let i = 0; i < arguments.length; i++) {    const parameter = arguments[i];     total += parameter;    console.log(`${i}) adding ${parameter}`);  }
```

```
 // outside the loop, we can no longer access i and parameter  console.log(`i=${i}, parameter=${parameter}`);  return total;}
```

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

输出是:

```
0) adding 31) adding 22) adding 1Uncaught ReferenceError: i is not defined  at stepSum (<anonymous>:10:20)  at <anonymous>:13:1
```

好了，现在我们已经学习了如何在块范围内防止泄漏和保护变量，让我们来试试吧！

回到上面的`setTimeout`问题，我们现在可以使用`let`和`const`来解决我们的问题:

```
function stepSum() {  let total = 0;
```

```
 for (let i = 0; i < arguments.length; i++) {    const parameter = arguments[i];
```

```
 setTimeout(function() {      total += parameter;     console.log(`${i}) adding ${parameter}, total=${total}`);    }, i*1000);  }}
```

```
stepSum(3, 2, 1);
```

瞧，现在输出是我们所期望的:

```
0) adding 3, total=31) adding 2, total=52) adding 1, total=6
```

请记住，我们已经为 for 循环的每次迭代创建了一对`i`和`parameter`变量。与之前相比，我们每次只重写一个`i`和`parameter`。这对内存消耗有一点影响。

最后，由于我们也在相同的范围内创建了`setTimeout`回调函数，它们将与`i`和`parameter`的受保护值共存。即使在`stepSum`完成执行后，块范围仍将保留。

### 处理函数

这里有一些值得注意的事情:声明一个`function`不同于声明一个`var`并给它分配一个函数。

例如，这里有一个在使用后声明一个`function`的例子，以理解提升是如何工作的。这是有效的 JavaScript:

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

```
function stepSum(...args) {  let total = 0;
```

```
 args.forEach((parameter, i) => {     total += parameter;    console.log(`${i}) adding ${parameter}`);  });
```

```
 return total;}
```

输出是:

```
0) adding 31) adding 22) adding 1total=6
```

为什么会这样？因为功能`stepSum`还没用就被完全吊起来了。

然而，将其声明为`var`会导致错误:

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

```
var stepSum = function(...args) {  let total = 0;
```

```
 args.forEach((parameter, i) => {     total += parameter;    console.log(`${i}) adding ${parameter}`);  });
```

```
 return total;}
```

输出是:

```
Uncaught TypeError: stepSum is not a function  at <anonymous>:1:22
```

为什么坏了？

这里不同的是，当一个`function`被吊起时，它的身体也被吊起。与`var`被提升相比，只有它的声明被提升，而它的赋值没有被提升。所以上面的代码应该与此类似，在函数被分配给它之前，我们试图使用`stepSum`。

```
var stepSum;console.log(`total=${stepSum(3, 2, 1)}`);
```

```
stepSum = function(...args) {  let total = 0;
```

```
 args.forEach((parameter, i) => {     total += parameter;    console.log(`${i}) adding ${parameter}`);  });
```

```
 return total;}
```

### 准备好接受挑战了吗？

现在你明白了这一点，我想给你一个挑战，让你解释下面的代码到底是怎么回事:

```
function stepSum(...args) {  let total = 0;
```

```
 args.forEach((parameter, i) => {     total += parameter;    console.log(`${i}) adding ${parameter}`);    return;    function total() {}  });
```

```
 return total;}
```

```
console.log(`total=${stepSum(3, 2, 1)}`);
```

输出是:

```
0) adding 31) adding 22) adding 1total=0
```

为什么是 0？？我邀请你在下面的评论区留下你的解释:)

### 了解更多信息

要了解更多关于范围和托管的有趣场景，我建议阅读这篇澄清性文章:

[**JavaScript 作用域和提升**](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
[*这种方法其实相当灵活，可以用在任何你需要临时作用域的地方，而不仅仅是块内……*www.adequatelygood.com](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)

之后，你可以用一些面试问题来检验你的知识:

[**函数提升&提升面试问题**](https://medium.freecodecamp.org/function-hoisting-hoisting-interview-questions-b6f91dbc2be8)
[*这是我之前关于变量提升的文章《JavaScript 变量提升指南？与…米*edium.freecodecamp.org](https://medium.freecodecamp.org/function-hoisting-hoisting-interview-questions-b6f91dbc2be8)

本文最初发表于 2014 年 2 月 4 日(ES2015 之前的版本)，名为 [Javascript 吊装](https://coderwall.com/p/jj635w/javascript-hoisting--2)。