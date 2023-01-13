# 如何使用 Memoize 缓存 JavaScript 函数结果并加速您的代码

> 原文：<https://www.freecodecamp.org/news/understanding-memoize-in-javascript-51d07d19430e/>

函数是编程不可或缺的一部分。他们帮助我们的代码增加了模块性和可重用性。

使用函数将我们的程序分成块是很常见的，我们可以调用这些函数来执行一些有用的动作。

有时，一个函数调用多次会变得很昂贵(比如，计算一个数的[阶乘](https://en.wikipedia.org/wiki/Factorial)的函数)。但是有一种方法可以优化这些函数，让它们执行得更快:**缓存**。

例如，假设我们有一个`function`来返回一个数的阶乘:

```
function factorial(n) {
    // Calculations: n * (n-1) * (n-2) * ... (2) * (1)
    return factorial
}
```

太好了，现在我们来找`factorial(50)`。计算机会进行计算并返回最终答案，太棒了！

完成后，我们去找`factorial(51)`。计算机再次执行一些计算并得到结果，但是你可能已经注意到我们已经重复了一些本可以避免的步骤。一种优化的方式是:

```
factorial(51) = factorial(50) * 51
```

但是我们的`function`每次被调用时都从头开始计算:

```
factorial(51) = 51 * 50 * 49 * ... * 2 * 1
```

如果我们的`factorial`函数能够记住之前计算的值，并使用它们来加速执行，这不是很酷吗？

接下来是**记忆**，这是我们`function`记忆(缓存)结果的一种方式。既然你已经对我们想要达到的目标有了基本的了解，下面是一个正式的定义:

> [](https://en.wikipedia.org/wiki/Memoization)**记忆化是一种优化技术，主要用于通过**存储昂贵的函数调用**的结果并在相同的输入再次出现时返回缓存的结果来加速计算机程序**

****记忆**简单来说就是**记忆**或者存储在内存中。记忆化的函数通常更快，因为如果函数随后用先前的值被调用，那么我们不是执行函数，而是从缓存中获取结果。**

**下面是一个简单的记忆化函数的样子*(这里有一个[代码笔](https://codepen.io/divyanshu013/pen/xdQPvp?editors=0011)，以防你想与之交互)*:**

```
`// a simple function to add something
const add = (n) => (n + 10);
add(9);
// a simple memoized function to add something
const memoizedAdd = () => {
  let cache = {};
  return (n) => {
    if (n in cache) {
      console.log('Fetching from cache');
      return cache[n];
    }
    else {
      console.log('Calculating result');
      let result = n + 10;
      cache[n] = result;
      return result;
    }
  }
}
// returned function from memoizedAdd
const newAdd = memoizedAdd();
console.log(newAdd(9)); // calculated
console.log(newAdd(9)); // cached`
```

### **记忆外卖**

**以上代码的一些要点如下:**

*   **`memoizedAdd`返回稍后调用的`function`。这是可能的，因为在 JavaScript 中，函数是第一类对象，这让我们可以将它们用作[高阶函数](http://eloquentjavascript.net/05_higher_order.html#h_xxCc98lOBK)，并返回另一个函数。**
*   **`cache`可以记住它的*值*，因为返回的函数有一个[闭包](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures)。**
*   **重要的是记忆函数是纯 T2 的。一个纯函数将为一个特定的输入返回相同的输出，不管它被调用多少次，这使得`cache`按预期工作。**

### **编写自己的`memoize`函数**

**前面的代码工作正常，但是如果我们想把任何函数变成记忆函数呢？**

**下面是如何编写自己的 memoize 函数( [codepen](https://codepen.io/divyanshu013/pen/zwMPdK?editors=0011#code-area) ):**

```
`// a simple pure function to get a value adding 10
const add = (n) => (n + 10);
console.log('Simple call', add(3));
// a simple memoize function that takes in a function
// and returns a memoized function
const memoize = (fn) => {
  let cache = {};
  return (...args) => {
    let n = args[0];  // just taking one argument here
    if (n in cache) {
      console.log('Fetching from cache');
      return cache[n];
    }
    else {
      console.log('Calculating result');
      let result = fn(n);
      cache[n] = result;
      return result;
    }
  }
}
// creating a memoized function for the 'add' pure function
const memoizedAdd = memoize(add);
console.log(memoizedAdd(3));  // calculated
console.log(memoizedAdd(3));  // cached
console.log(memoizedAdd(4));  // calculated
console.log(memoizedAdd(4));  // cached`
```

**太棒了！这个简单的`memoize`函数将把任何简单的`function`包装成一个内存化的等价物。这些代码对于简单的函数来说工作得很好，并且可以根据你的需要很容易地调整来处理任意数量的`arguments`。另一种方法是利用一些事实上的库，例如:**

*   **[lodash](https://lodash.com/docs/4.17.4#memoize)s`_.memoize(func, [resolver])`**
*   **ES7 `@memoize` [装修工](https://babeljs.io/docs/plugins/transform-decorators/)来自[德科](https://github.com/developit/decko#memoize)**

### **记忆递归函数**

**如果您尝试将一个递归函数传递给上面的`memoize`函数或来自 Lodash 的`_.memoize`,结果不会像预期的那样，因为递归函数在后续调用中最终会调用自己，而不是内存化的函数，因此没有使用`cache`。**

**只要确保你的递归函数调用的是记忆函数。以下是你如何修改教科书上的[阶乘](https://en.wikipedia.org/wiki/Factorial)例子( [codepen](https://codepen.io/divyanshu013/pen/JNevOm) ):**

```
`// same memoize function from before
const memoize = (fn) => {
  let cache = {};
  return (...args) => {
    let n = args[0];
    if (n in cache) {
      console.log('Fetching from cache', n);
      return cache[n];
    }
    else {
      console.log('Calculating result', n);
      let result = fn(n);
      cache[n] = result;
      return result;
    }
  }
}
const factorial = memoize(
  (x) => {
    if (x === 0) {
      return 1;
    }
    else {
      return x * factorial(x - 1);
    }
  }
);
console.log(factorial(5)); // calculated
console.log(factorial(6)); // calculated for 6 and cached for 5`
```

**这段代码有几点需要注意:**

*   **`factorial`函数递归调用自身的记忆版本。**
*   **记忆化函数缓存了前一个阶乘的值，这极大地改善了计算，因为它们可以重复使用`factorial(6) = 6 * factorial(5)`**

### **记忆化和缓存一样吗？**

**是的，有点。记忆化实际上是一种特定类型的缓存。虽然缓存一般可以指任何存储技术(如 [HTTP 缓存](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching))以备将来使用，但记忆化特别涉及*缓存*a`function`的返回值。**

### **何时记住你的功能**

**尽管看起来记忆化可以用于所有的函数，但它实际上只有有限的用例:**

*   **为了记忆一个函数，它应该是纯的，这样每次对于相同的输入，返回值都是相同的**
*   **内存化是在增加的空间和增加的速度之间的权衡，因此仅对具有有限输入范围的函数有意义，以便可以更频繁地使用缓存的值**
*   **看起来你应该记住你的 API 调用，但是这并不是必须的，因为浏览器会自动为你缓存它们。更多细节见 [HTTP 缓存](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)**
*   **我发现记忆化函数的最佳用例是计算量大的函数的**,它可以显著提高性能(阶乘和斐波那契并不是真实世界中很好的例子)****
*   **如果你喜欢 React/Redux，你可以查看一下 [reselect](https://github.com/reactjs/reselect#creating-a-memoized-selector) ，它使用了一个*记忆选择器*来确保只有在状态树的相关部分发生变化时才会进行计算。**

#### **进一步阅读**

**如果您想更详细地了解本文中的一些主题，下面的链接会很有用:**

*   **[JavaScript 中的高阶函数](http://eloquentjavascript.net/05_higher_order.html#h_xxCc98lOBK)**
*   **JavaScript 中的[闭包](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures)**
*   **[纯函数](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)**
*   **洛达什的`_.memoize` [文档](https://lodash.com/docs/4.17.4#memoize)和[源代码](https://github.com/lodash/lodash/blob/4.17.4/lodash.js#L10554-L10572)**
*   **更多记忆示例[这里](https://www.sitepoint.com/implementing-memoization-in-javascript/)和[这里](http://inlehmansterms.net/2015/03/01/javascript-memoization/)**
*   **[反应/重选](https://github.com/reactjs/reselect)**

**我希望这篇文章对您有用，并且您已经对 JavaScript 中的记忆化有了更好的理解:)**

* * *

**你可以在 twitter 上关注我的最新消息。我也开始在我的个人[博客](https://divyanshu013.dev/)上发布更多最近的帖子。**