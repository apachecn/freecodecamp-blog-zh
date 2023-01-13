# JavaScript 回调函数——JS 中的回调是什么以及如何使用它们

> 原文：<https://www.freecodecamp.org/news/javascript-callback-functions-what-are-callbacks-in-js-and-how-to-use-them/>

如果你熟悉编程，你已经知道函数是做什么的以及如何使用它们。但是什么是回调函数呢？回调函数是 JavaScript 的一个重要部分，一旦你理解了回调是如何工作的，你就会在 JavaScript 中变得更好。

所以在这篇文章中，我想通过一些例子来帮助你理解什么是回调函数以及如何在 JavaScript 中使用它们。

## 什么是回调函数？

在 JavaScript 中，函数是对象。我们可以将对象作为参数传递给函数吗？是的。

因此，我们也可以将函数作为参数传递给其他函数，并在外部函数中调用它们。听起来很复杂？让我用下面的例子来说明:

```
function print(callback) {  
    callback();
}
```

print()函数将另一个函数作为参数，并在内部调用它。这在 JavaScript 中是有效的，我们称之为“回调”。所以作为参数传递给另一个函数的函数是回调函数。但这还不是全部。

**您还可以观看以下回调函数的视频版本:**

[https://www.youtube.com/embed/qtfi4-8dj9c?feature=oembed](https://www.youtube.com/embed/qtfi4-8dj9c?feature=oembed)

### 为什么我们需要回调函数？

JavaScript 按自顶向下的顺序运行代码。然而，在某些情况下，代码在其他事情发生之后运行(或者必须运行),并且也不是按顺序运行的。这被称为异步编程。

回调确保函数不会在任务完成之前运行，而是会在任务完成之后立即运行。它帮助我们开发异步 JavaScript 代码，让我们远离问题和错误。

在 JavaScript 中，创建回调函数的方法是将它作为参数传递给另一个函数，然后在某件事情发生或某项任务完成后立即回调它。让我们看看如何…

## 如何创建回调

为了理解我上面的解释，让我从一个简单的例子开始。我们想在控制台记录一条消息，但它应该在 3 秒钟后出现。

```
const message = function() {  
    console.log("This message is shown after 3 seconds");
}

setTimeout(message, 3000);
```

JavaScript 中有一个内置的方法叫做“setTimeout”，在给定的一段时间(以毫秒为单位)后调用一个函数或者对一个表达式求值。所以在这里,“消息”功能在 3 秒钟后被调用。(1 秒= 1000 毫秒)

换句话说，消息函数是在事情发生后被调用的(在本例中是在 3 秒后)，而不是在此之前。所以消息函数是回调函数的一个例子。

### 什么是匿名函数？

或者，我们可以直接在另一个函数中定义一个函数，而不是调用它。它看起来会像这样:

```
setTimeout(function() {  
    console.log("This message is shown after 3 seconds");
}, 3000);
```

我们可以看到，这里的回调函数没有名字，JavaScript 中没有名字的函数定义被称为“匿名函数”。这与上面的示例执行完全相同的任务。

### 作为箭头函数的回调

如果您愿意，也可以编写与 ES6 arrow 函数相同的回调函数，这是 JavaScript 中一种较新的函数类型:

```
setTimeout(() => { 
    console.log("This message is shown after 3 seconds");
}, 3000);
```

## 事件呢？

JavaScript 是一种事件驱动的编程语言。我们也为事件声明使用回调函数。例如，假设我们希望用户点击一个按钮:

```
<button id="callback-btn">Click here</button>
```

这一次，只有当用户单击按钮时，我们才会在控制台上看到一条消息:

```
document.queryselector("#callback-btn")
    .addEventListener("click", function() {    
      console.log("User has clicked on the button!");
});
```

所以在这里，我们首先用它的 id 选择按钮，然后用 addEventListener 方法添加一个事件侦听器。它需要两个参数。第一个是它的类型，“click”，第二个参数是一个回调函数，当按钮被单击时，它记录消息。

如您所见，回调函数也用于 JavaScript 中的事件声明。

## 包裹

回调在 JavaScript 中经常使用，我希望这篇文章能帮助你理解回调实际上是做什么的，以及如何更容易地使用回调。接下来，你可以了解一下 [JavaScript Promises](https://www.freecodecamp.org/news/javascript-es6-promises-for-beginners-resolve-reject-and-chaining-explained/) ，这是一个类似的话题，我已经在我的新帖中解释过了。

**如果你想了解更多关于 web 开发的知识，欢迎在 Youtube 上关注我**[](https://www.youtube.com/channel/UC1EgYPCvKCXFn8HlpoJwY3Q)****！****

**感谢您的阅读！**