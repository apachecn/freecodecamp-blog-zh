# 编码面试中需要注意的 3 个 JavaScript 问题

> 原文：<https://www.freecodecamp.org/news/3-questions-to-watch-out-for-in-a-javascript-interview-725012834ccb/>

JavaScript 是所有现代网络浏览器的官方语言。因此，JavaScript 问题在各种开发人员面试中都会出现。

本文不是关于最新的 JavaScript 库、通用开发实践或任何新的 [ES6 函数](https://hackernoon.com/es6-features-you-need-to-know-now-b525e2b0755e#.seo0weyr4)。相反，在讨论 JavaScript 时，这是面试中经常出现的三件事。我自己也被问过这些问题，我的朋友告诉我他们也被问过。

当然，这些并不是你在 JavaScript 面试前应该学习的唯一 3 件事——有许多[T2](http://jstherightway.org/#getting-started)的方法你[可以](http://www.thatjsdude.com/interview/js2.html)更好地为即将到来的面试做准备——但是下面是面试官可能会问的 3 个问题，以判断你对 JavaScript 语言和 DOM[的了解和理解程度。](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

所以让我们开始吧！注意，在下面的例子中我们将使用普通的 JavaScript，因为你的面试官通常想知道你在没有 jQuery 等库的帮助下对 JavaScript 和 DOM 的理解程度。

### 问题#1:事件委托

构建应用程序时，有时您需要将事件侦听器附加到页面上的按钮、文本或图像，以便在用户与元素交互时执行某些操作。

如果我们以一个简单的待办事项列表为例，面试官可能会告诉你，当用户点击列表中的一个项目时，他们希望发生一个动作。他们希望您使用 JavaScript 实现这一功能，假设使用以下 HTML 代码:

```
<ul id="todo-app">
  <li class="item">Walk the dog</li>
  <li class="item">Pay bills</li>
  <li class="item">Make dinner</li>
  <li class="item">Code for one hour</li>
</ul>
```

您可能希望执行类似下面的操作，将事件侦听器附加到元素上:

```
document.addEventListener('DOMContentLoaded', function() {

  let app = document.getElementById('todo-app');
  let items = app.getElementsByClassName('item');

  // attach event listener to each item
  for (let item of items) {
    item.addEventListener('click', function() {
      alert('you clicked on item: ' + item.innerHTML);
    });
  }

});
```

虽然这在技术上可行，但问题是您要将事件侦听器单独附加到每一项上。这对于 4 个元素来说没问题，但是如果有人在待办事项列表中添加了 10，000 个项目(他们可能有很多事情要做)会怎么样呢？然后，您的函数将创建 10，000 个独立的事件侦听器，并将它们分别附加到 DOM。这不是很有效率。

在面试中，最好先问面试官用户最多可以输入多少个元素。例如，如果它不能超过 10，那么上面的代码就可以正常工作。但是如果用户可以输入的条目数量没有限制，那么您会希望使用更有效的解决方案。

如果您的应用程序最终拥有数百个事件监听器，那么更有效的解决方案将是实际上为整个容器附加一个**事件监听器，然后当它实际被点击时能够访问每个项目。这被称为[事件委托](https://davidwalsh.name/event-delegate)，它比附加单独的事件处理程序更有效。**

下面是事件委托的代码:

```
document.addEventListener('DOMContentLoaded', function() {

  let app = document.getElementById('todo-app');

  // attach event listener to whole container
  app.addEventListener('click', function(e) {
    if (e.target && e.target.nodeName === 'LI') {
      let item = e.target;
      alert('you clicked on item: ' + item.innerHTML);
    }
  });

});
```

### 问题 2:在循环中使用闭包

闭包有时会在面试中出现，这样面试官可以判断你对这门语言的熟悉程度，以及你是否知道何时实现闭包。

一个闭包基本上是当一个[内部函数可以访问](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#.44xk49tyt)其作用域之外的变量时。闭包可以用于类似于[实现隐私](https://medium.com/written-in-code/practical-uses-for-closures-c65640ae7304#.70gp35hbn)和创建[功能工厂](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e#.1817w0lmb)的事情。一个关于闭包使用的常见面试问题是这样的:

编写一个函数，它将遍历一个整数列表，并在 3 秒钟的延迟后打印出每个元素的索引。

对于这个问题，我见过的一个常见的(不正确的)实现如下所示:

```
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```

如果你运行这个程序，你会看到实际上每次都会打印出 **4** ，而不是预期的 **0，1，2，3** 在 3 秒钟的延迟后。

为了正确识别为什么会发生这种情况，理解 JavaScript 中为什么会发生这种情况是很有用的，这正是面试官想要测试的。

这样做的原因是因为`setTimeout`函数创建了一个可以访问其外部作用域的函数(闭包)，该外部作用域是包含索引`i`的循环。3 秒钟后，该函数被执行并打印出`i`的值，该值在循环结束时为 4，因为它循环通过 0、1、2、3、4，循环最终在 4 处停止。

对于这个问题，[其实有](https://coderbyte.com/algorithm/3-common-javascript-closure-questions)[几种方法](http://stackoverflow.com/questions/3572480/please-explain-the-use-of-javascript-closures-in-loops)正确编写函数。这是其中的两个:

```
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  // pass in the variable i so that each function 
  // has access to the correct index
  setTimeout(function(i_local) {
    return function() {
      console.log('The index of this number is: ' + i_local);
    }
  }(i), 3000);
}
```

```
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  // using the ES6 let syntax, it creates a new binding
  // every single time the function is called
  // read more here: http://exploringjs.com/es6/ch_variables.html#sec_let-const-loop-heads
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```

### 问题 3:去抖

有些浏览器事件可以在很短的时间内触发很多次，比如调整窗口大小或向下滚动页面。例如，如果您将一个事件侦听器附加到窗口滚动事件，并且用户持续快速地向下滚动页面，那么您的事件可能会在 3 秒钟内触发数千次。这可能会导致一些严重的性能问题。

如果你在面试中讨论构建一个应用程序，并且出现了滚动、调整窗口大小或按键等事件，一定要提到去抖动和/或节流是提高页面速度和性能的一种方法。一个真实的例子摘自这篇关于 css-tricks 的[客座博文:](https://css-tricks.com/debouncing-throttling-explained-examples/)

> 2011 年，Twitter 网站上出现了一个问题:当你向下滚动 Twitter feed 时，它变得很慢，没有反应。John Resig 发表了一篇关于这个问题的博客文章,其中解释了将昂贵的函数直接附加到`scroll`事件上是多么糟糕的想法。

去抖是解决这个问题的一种方法，它限制了函数再次被调用之前需要经过的时间。因此，去抖动的正确实现应该是*将几个函数调用组合成一个，并在一段时间后只执行一次。这里有一个普通 JavaScript 的实现，它利用了诸如[范围](https://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/)、闭包、 [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) 和[定时事件](http://www.w3schools.com/jsref/met_win_settimeout.asp)这样的主题:*

```
// debounce function that will wrap our event
function debounce(fn, delay) {
  // maintain a timer
  let timer = null;
  // closure function that has access to timer
  return function() {
    // get the scope and parameters of the function 
    // via 'this' and 'arguments'
    let context = this;
    let args = arguments;
    // if event is called, clear the timer and start over
    clearTimeout(timer);
    timer = setTimeout(function() {
      fn.apply(context, args);
    }, delay);
  }
}
```

这个函数——当包装在一个事件周围时——只有在经过一定时间后才会执行。

您可以像这样使用这个函数:

```
// function to be called when user scrolls
function foo() {
  console.log('You are scrolling!');
}

// wrap our function in a debounce to fire once 2 seconds have gone by
let elem = document.getElementById('container');
elem.addEventListener('scroll', debounce(foo, 2000));
```

节流是另一种类似于去抖动的技术，只不过它不是等待一段时间后再调用函数，而是将函数调用分散在更长的时间间隔内。因此，如果一个事件在 100 毫秒内发生 10 次，节流可以将每个函数调用分散为每 2 秒执行一次，而不是在 100 毫秒内全部触发。

有关去抖动和节流的更多信息，以下文章和教程可能会有所帮助:

*   [JavaScript 中的节流和去抖动](https://medium.com/@_jh3y/throttling-and-debouncing-in-javascript-b01cad5c8edf#.ly8uqz8v4)
*   [节流和去抖动的区别](https://css-tricks.com/the-difference-between-throttling-and-debouncing/)
*   [节流和去抖动示例](https://css-tricks.com/debouncing-throttling-explained-examples/)
*   [雷米·夏普关于节流函数调用的博文](https://remysharp.com/2010/07/21/throttling-function-calls)

如果您喜欢阅读本文，那么您可能会喜欢阅读 JavaScript 教程，并解决我在 [Coderbyte](https://www.coderbyte.com) 上主持的一些 JavaScript 编码挑战。我很想听听你的想法！