# JavaScript setTimeout()–如何在 JavaScript 中设置一个定时器或者休眠 N 秒

> 原文：<https://www.freecodecamp.org/news/javascript-settimeout-how-to-set-a-timer-in-javascript-or-sleep-for-n-seconds/>

本教程将帮助您理解内置 JavaScript 方法`setTimeout()`如何与直观的代码示例一起工作。

## 如何在 JavaScript 中使用 setTimeout()

`setTimeout()`方法允许你在一段时间后执行一段代码。您可以将该方法视为设置计时器以在特定时间运行 JavaScript 代码的一种方式。

例如，下面的代码将在 2 秒钟后将“Hello World”打印到 JavaScript 控制台:

```
setTimeout(function(){
    console.log("Hello World");
}, 2000);

console.log("setTimeout() example...");
```

setTimeout() method example

上面的代码将首先打印“setTimeout()示例...”到控制台，然后在 JavaScript 执行代码两秒钟后打印“Hello World”。

`setTimeout()`方法语法如下:

```
setTimeout(function, milliseconds, parameter1, parameter2, ...);
```

setTimeout() method syntax

`setTimeout()`方法的第一个参数是您想要执行的 JavaScript `function`。传递的时候可以直接写`function`，也可以引用一个命名的函数，如下图:

```
function greeting(){
  console.log("Hello World");
}

setTimeout(greeting);
```

setTimeout() method using named function as its argument

接下来，您可以传递`milliseconds`参数，这将是 JavaScript 在执行代码之前等待的时间。

一秒钟等于一千毫秒，所以如果要等待 3 秒钟，需要将`3000`作为第二个参数传递:

```
function greeting(){
  console.log("Hello World");
}

setTimeout(greeting, 3000);
```

setTimeout() method sleeping for 3 seconds

如果你省略了第二个参数，那么`setTimeout()`会立即执行传递的`function`而不会等待。

最后，您还可以将额外的参数传递给可以在`function`中使用的`setTimeout()`方法，如下所示:

```
function greeting(name, role){
  console.log(`Hello, my name is ${name}`);
  console.log(`I'm a ${role}`);
}

setTimeout(greeting, 3000, "Nathan", "Software developer");
```

setTimeout() with additional parameters for the function

现在您可能会想，“为什么不直接将参数传递给函数呢？”

这是因为如果像这样直接传递参数:

```
setTimeout(greeting("Nathan", "Software developer"), 3000);
```

然后 JavaScript 会立即执行`function`，而不会等待，因为你传递的是一个*函数调用*，而不是一个*函数引用*作为第一个参数。

这就是为什么如果您需要向函数传递任何参数，您需要从`setTimeout()`方法传递它们。

但是老实说，在我作为软件开发人员的角色中，我从来没有发现有必要向`setTimeout()`方法传递额外的参数，所以不要担心😉

## 如何取消 setTimeout 方法

您还可以通过使用`clearTimeout()`方法来防止`setTimeout()`方法执行`function`。

`clearTimeout()`方法要求`setTimeout()`返回的`id`知道要取消哪个`setTimeout()`方法:

```
clearTimeout(id);
```

clearTimeout() syntax

下面是一个使用`clearTimeout()`方法的例子:

```
const timeoutId = setTimeout(function(){
    console.log("Hello World");
}, 2000);

clearTimeout(timeoutId);
console.log(`Timeout ID ${timeoutId} has been cleared`);
```

clearTimeout() method in action

如果您有多个`setTimeout()`方法，那么您需要保存每个方法调用返回的 id，然后根据需要多次调用`clearTimeout()`方法来清除它们。

## 结论

JavaScript `setTimeout()`方法是一个内置的方法，允许您对某个`function`的执行进行计时。你需要通过`milliseconds`中等待的时间量，也就是说等待一秒，你需要通过一千个`milliseconds`。

要取消一个`setTimeout()`方法的运行，需要使用`clearTimeout()`方法，传递调用`setTimeout()`方法时返回的 ID 值。

## ******感谢阅读本教程******

如果你想学习更多关于 JavaScript 的知识，你可能想看看我在 sebhastian.com 的网站，在那里我已经发表了超过 100 篇关于用 JavaScript 编程的教程，都使用了简单易懂的解释和代码示例。

教程包括字符串操作、日期操作、数组和对象方法、JavaScript 算法解决方案等等。