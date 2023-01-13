# 反跳解释了如何让你的 JavaScript 等待你的用户完成输入

> 原文：<https://www.freecodecamp.org/news/debounce-explained-how-to-make-your-javascript-wait-for-your-user-to-finish-typing-2/>

JavaScript 中的去抖函数是高阶函数，它限制另一个函数被调用的速率。

> 高阶函数是这样一种函数，它要么接受一个函数作为参数，要么返回一个函数作为其 return 语句的一部分。我们的去抖功能两者兼而有之。

反跳最常见的用例是将其作为参数传递给附加到 HTML 元素的事件侦听器。为了更好地理解这看起来像什么，以及它为什么有用，让我们看一个例子。

假设您有一个名为`myFunc`的函数，每当您在输入字段中键入内容时，它都会被调用。在经历了项目的需求之后，您决定要改变体验。

相反，你想让`myFunc`在你最后一次输入东西至少 2 秒后执行。

这就是去抖功能发挥作用的地方。不是将`myFunc`传递给事件监听器，而是传递去抖。然后，去抖本身会将`myFunc`和数字 2000 一起作为参数。

现在，无论何时点击按钮，`myFunc`只有在最后一次调用`myFunc`之前至少过了 2 秒才会执行。

## 如何实现去抖功能

从头到尾，只需要 7 行代码就可以实现一个去抖功能。本节的其余部分集中在这 7 行代码上，这样我们可以看到我们的去抖功能是如何在内部工作的。

```
function debounce( callback, delay ) {
    let timeout;
    return function() {
        clearTimeout( timeout );
        timeout = setTimeout( callback, delay );
    }
}
```

从第 1 行开始，我们声明了一个名为`debounce`的新函数。这个新函数有两个参数，`callback`和`delay`。

```
function debounce( callback, delay ) {

} 
```

`callback`是任何需要限制执行次数的函数。

`delay`是在`callback`可以再次执行之前需要经过的时间(以毫秒为单位)。

```
function debounce( callback, delay ) {
    let timeout;
}
```

在第 2 行，我们声明了一个名为`timeout`的未初始化变量。
这个新变量保存了我们稍后在`debounce`函数中调用`setTimeout`时返回的`timeoutID`。

```
function debounce( callback, delay ) {
    let timeout;
    return function() {
    }
}
```

在第 3 行，我们返回一个匿名函数。这个匿名函数将关闭`timeout`变量，这样即使在对`debounce`的初始调用完成后，我们也可以保持对它的访问。

> 每当内部函数保留对其外部函数的词法范围的访问时，JavaScript 中就会发生闭包，即使外部函数已经执行完毕。如果你想了解更多关于闭包的知识，你可以阅读 Kyle Simpson 的《你不了解 JS》的第 7 章

```
function debounce( callback, delay ) {
    let timeout;
    return function() {
        clearTimeout( timeout );
    }
}
```

在第 4 行，我们调用了`WindowOrWorkerGlobalScope` mixin 的`clearTimeout`方法。这将确保每次我们调用我们的`debounce`函数时，`timeout`被重置，并且计数器可以再次启动。

JavaScript 的`WindowOrWorkerGlobalScope` mixin 让我们可以访问一些众所周知的方法，比如`setTimeout`、`clearTimeout`、`setInterval`、`clearInterval`和`fetch`。

你可以通过[阅读这篇文章](https://www.freecodecamp.org/news/an-introduction-to-scope-in-javascript-cbd957022652/)了解更多。

```
function debounce( callback, delay ) {
    let timeout;
    return function() {
        clearTimeout( timeout );
        timeout = setTimeout( callback, delay );
    }
}
```

在第 5 行，我们已经到达了`debounce`函数实现的末尾。

这一行代码做了一些事情。第一个动作是给我们在第 2 行声明的`timeout`变量赋值。这个值是一个在我们调用`setTimeout`时返回的`timeoutID`。这将允许我们引用通过调用`setTimeout`创建的超时，这样我们可以在每次使用`debounce`函数时重置它。

执行的第二个动作是调用`setTimeout`。这将创建一个超时，一旦`delay`(传递给我们的`debounce`函数的数字参数)过去，将执行`callback`(传递给我们的`debounce`函数的函数参数)。

因为我们使用了超时，所以只有当超时值达到 0 时，`callback`才会执行。这就是我们的`debounce`函数发挥作用的地方，因为我们在每次调用`debounce`时都会重置超时。这就是允许我们限制`myFunc`的执行率的地方。

第 5 行和第 6 行只包含了括号，所以我们就不赘述了。

就是这样。这就是我们的`debounce`函数的内部工作方式。现在，让我们从头开始补充上一个例子。我们将创建一个输入字段并附加一个事件监听器，将我们的`debounce`函数作为它的一个参数。

## 真实世界的例子

首先，我们需要创建一个输入字段。

```
<label for="myInput">Type something in!</label>
<input id="myInput" type="text">
```

接下来，我们需要创建一个函数，每当我们在输入字段中键入内容时，我们都希望执行这个函数。

```
function helloWorld() {
    console.log("Hello World!")
}
```

最后，我们需要选择上面创建的输入字段，并为其附加一个`keyup`事件监听器。

```
const myInput = document.getElementById("myInput");

myInput.addEventListener(
    "keyup",
    debounce( helloWorld, 2000 )
);
```

这就结束了我们的真实世界的例子！每次我们在输入框中输入内容时，如果距离上次输入内容已经过去了至少 2 秒钟，`helloWorld`就会执行。

> 特别感谢 Reddit 用户 **stratoscope** 帮助修复了本文中的一些初始代码。[这是他在 Repl.it 上创建的这个`debounce`函数的工作演示](https://repl.it/@geary/JsDebounce#script.js)

## 结束语

去抖函数简单而强大，可以对大多数 JavaScript 应用程序产生显著的影响。

虽然我们的例子既有趣又简单，但许多大型组织使用去抖功能来提高应用程序的性能。

如果你想了解更多关于 JavaScript 的知识，请访问我的网站！我正在 https://juanmvega.com 做一些很酷的东西。