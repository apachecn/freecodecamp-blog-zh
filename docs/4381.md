# JavaScript 的 queueMicrotask 简介

> 原文：<https://www.freecodecamp.org/news/queuemicrotask/>

## 介绍

**queueMicrotask** 是一个新的浏览器 API，可用于将同步代码转换为异步代码:

```
queueMicrotask(() => {
    console.log('hey i am executed asychronously by queueMicrotask');
});
```

这类似于我们使用 setTimeout 所做的事情:

```
setTimeout(() => {
    console.log('hey i am executed asychronously by setTimeout');
}, 0);
```

那么，当我们已经有了 **setTimeout** 时， **queueMicrotask** 还有什么用呢？

> **queueMicrotask** 将函数(任务)添加到一个队列中，在当前任务完成其工作后，并且在执行上下文的控制返回到浏览器的事件循环之前没有其他代码等待运行时，每个函数被逐一执行(FIFO)。

基本上， **queueMicrotask** 的任务是在当前调用堆栈为空之后，在将执行传递给事件循环之前执行的。

> 在 **setTimeout** 的情况下，在将控制交给事件循环之后，从事件队列中执行每个任务。

所以如果我们先执行 **setTimeout** ，然后再执行 **queueMicrotask** ，哪个会先被调用？执行下面的代码，自己检查一下:

```
setTimeout(() => {
    console.log('hey i am executed asychronously by setTimeout');
},0);

queueMicrotask(() => {
    console.log('hey i am executed asychronously by queueMicrotask');
}); 
```

Node.js 用“process.nextTick”做同样的事情。

## 何时使用 **It**

什么时候使用 **queueMicrotask，**没有规则，但是可以巧妙地使用它来运行一段代码，而不停止当前的执行。

例如，假设我们有一个数组，我们在其中维护一个值列表。插入每个值后，我们对数组进行排序，以便更快地搜索值。

```
var arr=[];

function add(value){
  arr.push(value);
  arr.sort();
}
```

但是，只要有人使用搜索输入框，就可以搜索元素。这意味着将在控制转移到事件循环后调用事件处理程序，因此对数据进行排序会阻止其他重要同步代码的执行。

下面是我们如何使用 **queueMicrotask** 来改进我们的代码:

```
var arr = [];

function add(value) {
  arr.push(value);
  queueMicrotask(() => {
    arr.sort();
  })
}
```

## 参考

*   [https://developer . Mozilla . org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/queueMicrotask](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/queueMicrotask)