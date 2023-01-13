# JavaScript 中的作用域和闭包——用例子解释

> 原文：<https://www.freecodecamp.org/news/scope-and-closures-in-javascript/>

在编写 JavaScript 时，您可能遇到过或编写过类似的代码:

```
function sayWord(word) {
	return () => console.log(word);
}

const sayHello = sayWord("hello");

sayHello(); // "hello"
```

这段代码很有趣，有几个原因。首先，我们可以在从`sayWord`返回的函数中访问`word`。第二，当我们调用`sayHello`时，我们可以访问`word`的值——即使我们调用`sayHello`,否则我们无法访问`word`。

在本文中，我们将了解支持这种行为的作用域和闭包。

## JavaScript 中的作用域介绍

范围是帮助我们理解前面例子的第一部分。变量的作用域是程序中可供使用的部分。

JavaScript 变量是词汇范围的，这意味着我们可以从源代码中声明变量的位置来确定变量的范围。(这并不完全正确:`var`变量不在词汇范围内，但是我们将很快讨论这一点。)

举以下例子:

```
if (true) {
	const foo = "foo";
	console.log(foo); // "foo"
}
```

`if`语句通过使用[块语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)引入块范围。我们说`foo`是`if`语句的块范围。这意味着只能从该块中访问它。

如果我们试图访问块外的`foo`，我们会得到一个`ReferenceError`,因为它超出了作用域:

```
if (true) {
	const foo = "foo";
	console.log(foo); // "foo"
}

console.log(foo); // Uncaught ReferenceError: foo is not defined
```

其他形式的块语句，比如`for`和`while`循环，也会为块范围的变量创建一个范围。例如，`foo`的作用域在下面的函数体内:

```
function sayFoo() {
	const foo = "foo";
	console.log(foo);
}

sayFoo(); // "foo"

console.log(foo); // Uncaught ReferenceError: foo is not defined
```

## 嵌套的范围和函数

JavaScript 允许嵌套块，因此也允许嵌套范围。嵌套作用域创建一个作用域树或作用域链。

考虑下面的代码，它嵌套了多个块语句:

```
if (true) {
	const foo = "foo";
	console.log(foo); // "foo"

	if (true) {
		const bar = "bar";
		console.log(foo); // "foo"

		if (true) {
			console.log(foo, bar); // "foo bar"
		}
	}
}
```

JavaScript 还允许我们嵌套函数:

```
function foo(bar) {
	function baz() {
		console.log(bar);
	}

	baz();
}

foo("bar"); // "bar"
```

正如所料，我们可以从变量的直接作用域(声明变量的作用域)访问变量。我们还可以从变量的内部作用域(嵌套在其直接作用域内的作用域)访问变量。也就是说，我们可以从声明变量的作用域和每个内部作用域访问变量。

在我们继续之前，我们应该澄清变量声明类型之间在这种行为上的区别。

## JavaScript 中 let、const 和 var 的作用域

我们可以用`let`、`const`和`var`声明来创建变量。对于`let`和`const`，块范围的工作如上所述。但是，`var`表现不同。

### 让和 const

`let`和`const`创建块范围的变量。当在块中声明时，它们只能在该块中访问。这种行为在我们之前的示例中已经演示过了:

```
if (true) {
	const foo = "foo";
	console.log(foo); // "foo"
}

console.log(foo); // Uncaught ReferenceError: foo is not defined
```

### 定义变量

用`var`创建的变量的作用域是最近的函数或全局范围(我们将很快讨论)。它们不是块范围的:

```
function foo() {
	if (true) {
		var foo = "foo";
	}
	console.log(foo);
}

foo(); // "foo"
```

`var`可能会造成混乱的情况，此信息仅供参考。可能的话最好使用`let`和`const`。本文的其余部分将只涉及`let`和`const`变量。

如果你对上面例子中`var`的表现感兴趣，你应该看看[我写的关于提升](https://www.freecodecamp.org/news/what-is-hoisting-in-javascript/)的文章。

## JavaScript 中的全局和模块范围

除了块范围之外，变量的范围可以是全局和模块范围。

在 web 浏览器中，全局范围位于脚本的顶层。它是我们之前描述的范围树的根，并且包含所有其他范围。因此，在全局范围内创建一个变量就可以在每个范围内访问它:

```
<script>
	const foo = "foo";
</script>
<script>
	console.log(foo); // "foo"

	function bar() {
		if (true) {
			console.log(foo);
		}
	}

	bar(); // "foo"
</script>
```

每个模块也有自己的范围。在模块级声明的变量仅在该模块内可用，它们不是全局的:

```
<script type="module">
	const foo = "foo";
</script>
<script>
	console.log(foo); // Uncaught ReferenceError: foo is not defined
</script>
```

## JavaScript 中的闭包

现在我们已经了解了范围，让我们回到我们在简介中看到的例子:

```
function sayWord(word) {
	return () => console.log(word);
}

const sayHello = sayWord("hello");

sayHello(); // "hello"
```

回想一下，这个例子有两个有趣的地方:

1.  从`sayWord`返回的函数可以访问`word`参数
2.  当`sayHello`在`word`范围之外被调用时，返回的函数保持`word`的值

第一点可以用词法作用域来解释:返回的函数可以访问`word`，因为它存在于它的外部作用域中。

第二点是因为闭包:闭包是一个函数，它结合了对其外部定义的变量的引用。闭包维护变量引用，这允许函数访问其作用域之外的变量。它们将函数和变量“封装”在其环境中。

## JavaScript 中闭包的例子

您可能经常遇到并使用闭包，但并没有意识到这一点。让我们探索更多使用闭包的方法。

### 回收

回调引用在自身外部声明的变量是很常见的。例如:

```
function getCarsByMake(make) {
	return cars.filter(x => x.make === make);
}
```

由于词法范围的原因，`make`在回调中是可用的，并且由于闭包的原因，当匿名函数被`filter`调用时，`make`的值被持久化。

### 存储状态

我们可以使用闭包从存储状态的函数中返回对象。考虑下面的`makePerson`函数，它返回一个可以存储和改变一个`name`的对象:

```
function makePerson(name) {
	let _name = name;

	return {
		setName: (newName) => (_name = newName),
		getName: () => _name,
	};
}

const me = makePerson("Zach");
console.log(me.getName()); // "Zach"

me.setName("Zach Snoek");
console.log(me.getName()); // "Zach Snoek"
```

这个例子说明了闭包不仅仅是在创建过程中冻结了函数外部作用域的变量值。相反，它们在闭包的整个生命周期中维护引用。

### 私有方法

如果您熟悉面向对象编程，您可能已经注意到我们前面的例子非常类似于一个存储私有状态并公开公共 getter 和 setter 方法的类。通过使用闭包来实现私有方法，我们可以进一步扩展这种面向对象的并行方式:

```
function makePerson(name) {
	let _name = name;

	function privateSetName(newName) {
		_name = newName;
	}

	return {
		setName: (newName) => privateSetName(newName),
		getName: () => _name,
	};
}
```

`privateSetName`不能被消费者直接访问，它可以通过闭包访问私有状态变量`_name`。

### 反应事件处理程序

最后，闭包在 React 事件处理程序中很常见。以下`Counter`组件由[反应文件](https://reactjs.org/docs/hooks-reference.html#functional-updates)修改而来:

```
function Counter({ initialCount }) {
	const [count, setCount] = React.useState(initialCount);

	return (
		<>
			<button onClick={() => setCount(initialCount)}>Reset</button>
			<button onClick={() => setCount((prevCount) => prevCount - 1)}>
				-
			</button>
			<button onClick={() => setCount((prevCount) => prevCount + 1)}>
				+
			</button>
			<button onClick={() => alert(count)}>Show count</button>
		</>
	);
}

function App() {
	return <Counter initialCount={0} />;
}
```

闭包使得以下操作成为可能:

*   复位、减量和增量按钮点击处理程序访问`setCount`
*   从`Counter`的道具中访问`initialCount`的重置按钮
*   “显示计数”按钮显示`count`状态。

闭包在 React 的其他部分也很重要，比如道具和钩子。关于这些主题的讨论超出了本文的范围。我推荐阅读肯特·c·多兹的[这篇文章](https://epicreact.dev/how-react-uses-closures-to-avoid-bugs/)或者丹·阿布拉莫夫的[这篇文章](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)来学习更多关于闭包在 React 中的作用。

## 结论

作用域是指程序中我们可以访问变量的部分。JavaScript 允许我们嵌套作用域，在外部作用域中声明的变量可以从所有内部作用域中访问。变量可以是全局、模块或块范围的。

闭包是一个函数，包含了对其外部作用域中变量的引用。闭包允许函数保持与外部变量的连接，甚至在变量范围之外。

闭包有许多用途，从创建存储状态和实现私有方法的类结构到向事件处理程序传递回调。

## 让我们连接

如果你对更多类似的文章感兴趣，[订阅我的简讯](https://mailchi.mp/2df4b6d5458f/signup-page)并通过 [LinkedIn](https://www.linkedin.com/in/zach-snoek-5b327b179/) 和 [Twitter](https://twitter.com/zach_snoek) 与我联系！

## 承认

感谢[布莱恩·史密斯](https://github.com/bryanrsmith)对这篇文章的草稿提供反馈。

由[郭佳欣·阿维蒂斯扬](https://unsplash.com/@kar111?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的封面照片。