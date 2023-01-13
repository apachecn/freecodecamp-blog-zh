# JavaScript 中的提升是什么？

> 原文：<https://www.freecodecamp.org/news/what-is-hoisting-in-javascript/>

在 JavaScript 中，提升允许您在声明函数和变量之前使用它们。在这篇文章中，我们将学习什么是起重以及它是如何工作的。

## 什么是吊装？

看看下面的代码，猜猜它运行时会发生什么:

```
console.log(foo);
var foo = 'foo'; 
```

可能会让你吃惊的是，这段代码输出了`undefined`并且没有失败或抛出错误——即使`foo`是在我们`console.log`它之后被赋值的！

这是因为 JavaScript 解释器拆分了函数和变量的声明和赋值:在执行之前，它将您的声明“提升”到其包含范围的顶部。

这个过程叫做提升，它允许我们在上面的例子中在声明之前使用`foo`。

让我们更深入地了解函数和变量提升，以理解这意味着什么以及它是如何工作的。

## JavaScript 中的变量提升

提醒一下，我们用`var`、`let`和`const`语句将声明为变量。例如:

```
var foo;
let bar; 
```

我们**使用赋值操作符给**变量赋值:

```
// Declaration
var foo;
let bar;

// Assignment
foo = 'foo';
bar = 'bar'; 
```

在许多情况下，我们可以将声明和赋值合并到一个步骤中:

```
var foo = 'foo';
let bar = 'bar';
const baz = 'baz'; 
```

变量提升根据变量的声明方式而有所不同。让我们从理解`var`变量的行为开始。

### 用`var`变量提升

当解释器提升一个用`var`声明的变量时，它将其值初始化为`undefined`。下面的第一行代码将输出`undefined`:

```
console.log(foo); // undefined

var foo = 'bar';

console.log(foo); // "bar" 
```

正如我们之前定义的，提升来自于解释器对变量声明和赋值的拆分。我们可以通过将声明和赋值分成两个步骤来手动实现相同的行为:

```
var foo;

console.log(foo); // undefined

foo = 'foo';

console.log(foo); // "foo" 
```

记住第一个`console.log(foo)`输出`undefined`是因为`foo`被提升并被赋予了一个默认值(而不是因为变量从未被声明)。使用未声明的变量将会抛出一个`ReferenceError`:

```
console.log(foo); // Uncaught ReferenceError: foo is not defined 
```

在赋值前使用未声明的变量也会抛出`ReferenceError`,因为没有声明被挂起:

```
console.log(foo); // Uncaught ReferenceError: foo is not defined
foo = 'foo';      // Assigning a variable that's not declared is valid 
```

到目前为止，您可能在想，“嗯，JavaScript 让我们在声明变量之前访问它们，这有点奇怪。”这种行为是 JavaScript 不寻常的一部分，可能会导致错误。在声明变量之前使用变量通常是不可取的。

谢天谢地，ECMAScript 2015 中引入的`let`和`const`变量表现不同。

### 用`let`和`const`变量提升

用`let`和`const`声明的变量被提升，但没有用默认值初始化。在声明之前访问一个`let`或`const`变量会导致一个`ReferenceError`:

```
console.log(foo); // Uncaught ReferenceError: Cannot access 'foo' before initialization

let foo = 'bar';  // Same behavior for variables declared with const 
```

注意，解释器仍然在提升`foo`:错误消息告诉我们变量在某处被初始化。

### 时间死区

当我们试图在声明前访问一个`let`或`const`变量时，我们得到一个引用错误的原因是因为时间死区(TDZ)。

TDZ 从变量封闭范围的开始处开始，在声明变量时结束。访问这个 TDZ 中的变量会抛出一个`ReferenceError`。

这里有一个显式的[块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)的例子，它显示了`foo`的 TDZ 的开始和结束:

```
{
 	// Start of foo's TDZ
  	let bar = 'bar';
	console.log(bar); // "bar"

	console.log(foo); // ReferenceError because we're in the TDZ

	let foo = 'foo';  // End of foo's TDZ
} 
```

TDZ 也存在于默认的函数参数中，这些参数是从左到右计算的。在以下示例中，`bar`处于 TDZ 中，直到其默认值被设置:

```
function foobar(foo = bar, bar = 'bar') {
  console.log(foo);
}
foobar(); // Uncaught ReferenceError: Cannot access 'bar' before initialization 
```

但是这段代码可以工作，因为我们可以在 TDZ 之外访问`foo`:

```
function foobar(foo = 'foo', bar = foo) {
  console.log(bar);
}
foobar(); // "foo" 
```

### `typeof`在太阳穴死区

在 TDZ 中使用`let`或`const`变量作为`typeof`操作符的操作数会引发错误:

```
console.log(typeof foo); // Uncaught ReferenceError: Cannot access 'foo' before initialization
let foo = 'foo'; 
```

这种行为与我们已经看到的 TDZ 中的其他`let`和`const`的情况是一致的。我们在这里得到一个`ReferenceError`的原因是`foo`被声明但没有初始化——我们应该知道我们在初始化之前使用它([来源:Axel Rauschmayer](https://2ality.com/2015/10/why-tdz.html) )。

然而，当在声明前使用一个`var`变量时，情况就不一样了，因为它在被提升时是用`undefined`初始化的:

```
console.log(typeof foo); // "undefined"
var foo = 'foo'; 
```

此外，这是令人惊讶的，因为我们可以检查不存在的变量的类型而不出错。`typeof`安全返回一个字符串:

```
console.log(typeof foo); // "undefined" 
```

事实上，`let`和`const`的引入打破了`typeof`对于任何操作数总是返回一个字符串值的保证。

## JavaScript 中的函数提升

函数声明也被挂起。函数提升允许我们在定义函数之前调用它。例如，以下代码成功运行并输出`"foo"`:

```
foo(); // "foo"

function foo() {
	console.log('foo');
} 
```

注意，只提升函数*声明*，不提升函数*表达式*。这应该是有意义的:正如我们刚刚学到的，变量赋值是不提升的。

如果我们试图调用函数表达式被赋给的变量，我们将得到一个`TypeError`或`ReferenceError`，这取决于变量的作用域:

```
foo(); // Uncaught TypeError: foo is not a function
var foo = function () { }

bar(); // Uncaught ReferenceError: Cannot access 'bar' before initialization
let bar = function () { }

baz(); // Uncaught ReferenceError: Cannot access 'baz' before initialization
const baz = function () { } 
```

这不同于调用一个从未声明的函数，后者抛出一个不同的`ReferenceError`:

```
foo(); // Uncaught ReferenceError: baz is not defined 
```

## 如何在 JavaScript 中使用提升

### 可变提升

因为`var`提升会造成混乱，所以最好避免在声明变量之前使用它们。如果你在一个[绿地项目](https://en.wikipedia.org/wiki/Greenfield_project)中写代码，你应该使用`let`和`const`来执行。

如果你在一个旧的代码库中工作，或者因为其他原因不得不使用`var`， [MDN 建议](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)尽可能靠近它们作用域的顶部编写`var`声明。这将使你的变量范围更加清晰。

你也可以考虑使用`no-use-before-define` [ESLint 规则](https://eslint.org/docs/rules/no-use-before-define)，它将确保你不会在声明变量之前使用变量。

### 功能提升

函数提升很有用，因为我们可以将函数实现隐藏在文件的更深处，让读者专注于代码在做什么。换句话说，我们可以打开一个文件，看看代码做了什么，而不必先了解它是如何实现的。

以下面这个虚构的例子为例:

```
resetScore();
drawGameBoard();
populateGameBoard();
startGame();

function resetScore() {
	console.log("Resetting score");
}

function drawGameBoard() {
	console.log("Drawing board");
}

function populateGameBoard() {
	console.log("Populating board");
}

function startGame() {
	console.log("Starting game");
} 
```

我们无需阅读所有的函数声明，就能立即知道这段代码做了什么。

然而，在声明之前使用函数是个人喜好的问题。一些开发者，比如 Wes Bos，更喜欢避免这种情况，把函数放到可以按需导入的模块中([来源:Wes Bos](https://wesbos.com/javascript/03-the-tricky-bits/hoisting) )。

Airbnb 的风格指南更进一步，鼓励命名函数表达式优先于声明，以防止声明前引用:

> 函数声明是提升的，这意味着在文件中定义函数之前引用它很容易——太容易了。这损害了可读性和可维护性。如果你发现一个函数的定义太大或者太复杂，以至于妨碍了对文件其余部分的理解，那么也许是时候把它提取到自己的模块中了！([来源:Airbnb JavaScript 风格指南](https://airbnb.io/javascript/#functions--declarations))

## 结论

感谢您的阅读，我希望这篇文章能帮助您了解 JavaScript 中的提升。如果您想联系或有任何问题，请随时通过 LinkedIn 联系我！