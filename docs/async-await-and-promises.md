# 异步/等待和承诺解释

> 原文：<https://www.freecodecamp.org/news/async-await-and-promises/>

`async` / `await` [操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)使得实现许多异步承诺变得更加容易。它们还允许工程师编写更清晰、更简洁、可测试的代码。

要理解这个主题，你应该对[承诺](https://guide.freecodecamp.org/javascript/promises)如何运作有一个坚实的理解。

## **基本语法**

```
function slowlyResolvedPromiseFunc(string) { 
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(string);
    }, 5000);
  });
}

async function doIt() {
  const myPromise = await slowlyResolvedPromiseFunc("foo");
  console.log(myPromise); // "foo"
}

doIt();
```

有几件事需要注意:

*   包含`await`声明的函数必须包含`async`操作符。这将告诉 JS 解释器，它必须等待，直到承诺被解决或拒绝。
*   在 const 声明期间，`await`操作符必须是内联的。
*   这对`reject`和`resolve`都有效。

* * *

## **嵌套承诺 vs. `Async` / `Await`**

实现一个承诺非常简单。相反，连锁承诺或依赖模式的创建可能会产生“意大利面条式代码”。

下面的例子假设 [`request-promise`](https://github.com/request/request-promise) 库作为`rp`可用。

### **连锁/嵌套承诺**

```
// First Promise
const fooPromise = rp("http://domain.com/foo");

fooPromise.then(resultFoo => {
    // Must wait for "foo" to resolve
    console.log(resultFoo);

    const barPromise = rp("http://domain.com/bar");
    const bazPromise = rp("http://domain.com/baz");

    return Promise.all([barPromise, bazPromise]);
}).then(resultArr => {
    // Handle "bar" and "baz" resolutions here
    console.log(resultArr[0]);
    console.log(resultArr[1]);
});
```

### **`async`和`await`承诺**

```
// Wrap everything in an async function
async function doItAll() {
    // Grab data from "foo" endpoint, but wait for resolution
    console.log(await rp("http://domain.com/foo"));

    // Concurrently kick off the next two async calls,
    // don't wait for "bar" to kick off "baz"
    const barPromise = rp("http://domain.com/bar");
    const bazPromise = rp("http://domain.com/baz");

    // After both are concurrently kicked off, wait for both
    const barResponse = await barPromise;
    const bazResponse = await bazPromise;

    console.log(barResponse);
    console.log(bazResponse);
}

// Finally, invoke the async function
doItAll().then(() => console.log('Done!'));
```

使用`async`和`await`的优势应该很清楚。这段代码更具可读性、模块化和可测试性。

公平地说，即使增加了并发性，底层的计算过程与前面的例子是一样的。

* * *

## **处理错误/拒绝**

一个基本的 try-catch 块处理一个被拒绝的承诺。

```
async function errorExample() {
  try {
    const rejectedPromise = await Promise.reject("Oh-oh!");
  } catch (error) {
    console.log(error); // "Uh-oh!"
  }
}

errorExample();
```