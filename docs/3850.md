# JavaScript 承诺解释

> 原文：<https://www.freecodecamp.org/news/javascript-promises-explained/>

## **JavaScript 中的承诺是什么？**

JavaScript 是单线程的，意味着两个位的脚本不能同时运行；他们不得不一个接一个地跑。承诺是表示异步操作的最终完成(或失败)及其结果值的对象。

```
var promise = new Promise(function(resolve, reject) {
  // do thing, then…

  if (/* everything worked */) {
    resolve("See, it worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```

## **承诺存在于这些状态之一**

*   待定:初始状态，既未完成也未拒绝。
*   已完成:操作成功完成。
*   拒绝:操作失败。

Promise 对象充当创建承诺时不一定知道的值的代理。它允许您将处理程序与异步操作的最终成功值或失败原因相关联。

这使得异步方法像同步方法一样返回值:异步方法不是立即返回最终值，而是返回一个在未来某个时间提供该值的承诺。

## **使用‘Then’(承诺链)**

要获得几个异步调用并使它们一个接一个地同步，可以使用承诺链。这允许在随后的回调中使用第一个承诺的值。

```
Promise.resolve('some')
  .then(function(string) { // <-- This will happen after the above Promise resolves (returning the value 'some')
    return new Promise(function(resolve, reject) {
      setTimeout(function() {
        string += 'thing';
        resolve(string);
      }, 1);
    });
  })
  .then(function(string) { // <-- This will happen after the above .then's new Promise resolves
    console.log(string); // <-- Logs 'something' to the console
  });
```

## **承诺 API**

Promise 类中有 4 个静态方法:

*   承诺.决心
*   承诺.拒绝
*   承诺。所有
*   承诺.比赛

## **承诺可以连在一起**

写承诺解决某个特定问题的时候，你可以把它们串联起来，形成逻辑。

```
var add = function(x, y) {
  return new Promise((resolve,reject) => {
    var sum = x + y;
    if (sum) {
      resolve(sum);
    }
    else {
      reject(Error("Could not add the two values!"));
    }
  });
};

var subtract = function(x, y) {
  return new Promise((resolve, reject) => {
    var sum = x - y;
    if (sum) {
      resolve(sum);
    }
    else {
      reject(Error("Could not subtract the two values!"));
    }
  });
};

// Starting promise chain
add(2,2)
  .then((added) => {
    // added = 4
    return subtract(added, 3);
  })
  .then((subtracted) => {
    // subtracted = 1
    return add(subtracted, 5);
  })
  .then((added) => {
    // added = 6
    return added * 2;    
  })
  .then((result) => {
    // result = 12
    console.log("My result is ", result);
  })
  .catch((err) => {
    // If any part of the chain is rejected, print the error message.
    console.log(err);
  });
```

这对于遵循一个*函数式编程*范例是很有用的。通过创建操作数据的函数，您可以将它们链接在一起，组合成一个最终结果。如果在函数链的任何一点，一个值被*拒绝*，该链将跳到最近的`catch()`处理程序。

关于函数式编程的更多信息:[函数式编程](https://en.wikipedia.org/wiki/Functional_programming)

## **函数发生器**

在最近的版本中，JavaScript 引入了更多的方式来处理承诺。其中一种方法是函数生成器。函数生成器是“可暂停”的函数。当与承诺一起使用时，生成器可以使使用更容易阅读，并且看起来是“同步的”。

```
const myFirstGenerator = function* () {
  const one = yield 1;
  const two = yield 2;
  const three = yield 3;

  return 'Finished!';
}

const gen = myFirstGenerator();
```

这是我们的第一个生成器，你可以通过`function*`语法看到。我们声明的`gen`变量将不会运行`myFirstGenerator`，而是“这个生成器已经可以使用了”。

```
console.log(gen.next());
// Returns { value: 1, done: false }
```

当我们运行`gen.next()`时，它将解除发电机暂停并继续运行。由于这是我们第一次调用`gen.next()`，它将运行`yield 1`并暂停，直到我们再次调用`gen.next()`。当`yield 1`被调用时，它将返回给我们被产出的`value`以及生成器是否为`done`。

```
console.log(gen.next());
// Returns { value: 2, done: false }

console.log(gen.next());
// Returns { value: 3, done: false }

console.log(gen.next());
// Returns { value: 'Finished!', done: true }

console.log(gen.next());
// Will throw an error
```

当我们继续调用`gen.next()`时，它将继续进入下一个`yield`并每次暂停。一旦没有剩余的`yield`，它将继续运行生成器的剩余部分，在这种情况下只返回`'Finished!'`。如果你再次调用`gen.next()`，当生成器完成时，它将抛出一个错误。

现在，想象一下，如果这个例子中的每个`yield`都是一个`Promise`，那么代码本身就会显得非常同步。

### **Promise.all(iterable)对于不同来源的多个请求非常有用**

Promise.all(iterable)方法返回一个承诺，当 iterable 参数中的所有承诺都已解析或 iterable 参数不包含承诺时，该承诺将解析。它用拒绝的第一个承诺的理由拒绝。

```
var promise1 = Promise.resolve(catSource);
var promise2 = Promise.resolve(dogSource);
var promise3 = Promise.resolve(cowSource);

Promise.all([promise1, promise2, promise3]).then(function(values) {
  console.log(values);
});
// expected output: Array ["catData", "dogData", "cowData"]
```

## 关于承诺的更多信息:

*   【JavaScript 承诺的实际工作方式
*   [如何用 JavaScript 实现承诺](https://www.freecodecamp.org/news/how-to-implement-promises-in-javascript-1ce2680a7f51/)
*   [如何在 JavaScript 中使用承诺](https://www.freecodecamp.org/news/how-javascript-promises-actually-work-from-the-inside-out-76698bb7210b/)
*   [如何写一个 JavsScript 的承诺](https://www.freecodecamp.org/news/how-to-write-a-javascript-promise-4ed8d44292b8/)