# 如何从头开始编写自己的 Promisify 函数

> 原文：<https://www.freecodecamp.org/news/write-your-own-promisify-function-from-scratch/>

### 介绍

在本文中，您将学习如何从头开始编写自己的 promisify 函数。

承诺有助于处理基于回调的 API，同时保持代码与承诺一致。

我们可以用`new Promise()`包装任何函数，根本不用担心它。但是当我们有很多功能的时候这样做是多余的。

如果你理解了承诺和回调，那么学习如何编写 promisify 函数应该很容易。所以让我们开始吧。

### 但是你有没有想过 promisify 是如何工作的？

> 重要的是不要停止质疑。好奇心有它存在的理由。
> 
> —阿尔伯特·爱因斯坦

在 2015 年 6 月出版的第六版(ES6)ECMA-262 标准中引入了承诺。

相对于回调，这是一个很大的进步，因为我们都知道“回调地狱”是多么不可读:)

![callback-1](img/8c8ca96f17e5da527624df6baec17624.png)

作为 Node.js 开发者，你应该知道[什么是承诺](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)和[内部如何运作](https://101node.io/blog/how-promises-actually-work-inside-out/)，这也会在 js 面试中帮到你。在继续阅读之前，请随意快速回顾一下。

### 为什么我们需要将回电转化为承诺？

1.  对于回调，如果你想按顺序做一些事情，你必须在每个回调中指定一个`err`参数，这是多余的。在 promises 或 async-await 中，您可以添加一个`.catch`方法或块，它将捕获 promise 链中发生的任何错误
2.  使用回调，您无法控制何时调用它，在什么上下文中调用，或者调用多少次，这可能导致内存泄漏。
3.  使用承诺，我们可以控制这些因素(尤其是错误处理),这样代码更具可读性和可维护性。

## 如何让基于回调的函数返回一个承诺

有两种方法可以做到:

1.  将该函数包装在另一个返回承诺的函数中。然后，它根据回调参数进行解析或拒绝。
2.  承诺——我们创建了一个 util/helper 函数`promisify`,它将转换所有基于错误优先回调的 API。

示例:有一个基于回调的 API，它提供两个数字的和。我们想要承诺它，所以它返回一个`thenable`承诺。

```
const getSumAsync = (num1, num2, callback) => {

  if (!num1 || !num2) {
    return callback(new Error("Missing arguments"), null);
  }
  return callback(null, num1 + num2);
}
getSumAsync(1, 1, (err, result) => {
  if (err){
    doSomethingWithError(err)
  }else {
    console.log(result) // 2
  }
})
```

### 承诺

如您所见，`getSumPromise`将所有工作委托给了原始函数`getSumAsync`，提供了自己的回调函数，该回调函数转化为 promise `resolve/reject`。

### 许诺

当我们需要指定许多函数时，我们可以创建一个辅助函数`promisify`。

### 什么是允诺？

许诺意味着转变。它是接受回调的函数到返回承诺的函数的转换。

使用 Node.js 的`util.promisify()`:

```
const { promisify } = require('util')
const getSumPromise = promisify(getSumAsync) // step 1
getSumPromise(1, 1) // step 2
.then(result => {
  console.log(result)
})
.catch(err =>{
  doSomethingWithError(err);
})
```

因此，这看起来像是一个神奇的函数，它将`getSumAsync`转换为`getSumPromise`，其中包含了`.then`和`.catch`方法

### 让我们编写自己的 promisify 函数:

如果你看看上面代码中的 ****步骤 1**** ，`promisify`函数接受一个函数作为参数，那么我们首先要做的就是写一个能做同样事情的函数:

```
const getSumPromise = myPromisify(getSumAsync)
const myPromisify = (fn) => {}
```

之后，`getSumPromise(1, 1)`是函数调用。这意味着我们的 promisify 应该返回另一个函数，可以用原始函数的相同参数调用该函数:

```
const myPromisify = (fn) => {
 return (...args) => {
 }
}
```

在上面的代码中，你可以看到我们在扩散参数，因为我们不知道原始函数有多少个参数。`args`将是一个包含所有参数的数组。

当你调用`getSumPromise(1, 1)`时，你实际上是在调用`(...args)=> {}`。在上面的实现中，它返回一个承诺。这就是为什么你能使用`getSumPromise(1, 1).then(..).catch(..)`。

我希望你已经得到了包装函数`(...args) => {}`应该返回一个承诺的提示。

### 兑现承诺

```
const myPromisify = (fn) => {
  return (...args) => {
    return new Promise((resolve, reject) => {

    })
  }
}
```

现在棘手的部分是如何决定何时兑现承诺。实际上，这将由最初的`getSumAsync`函数实现决定——它将调用最初的回调函数，我们只需要定义它。然后基于`err`和`result`我们将`reject`或`resolve`的承诺。

```
const myPromisify = (fn) => {
  return (...args) => {
    return new Promise((resolve, reject) => {
      function customCallback(err, result) {
       if (err) {
         reject(err)
       }else {
         resolve(result);
        }
      }
   })
  }
}
```

我们的`args[]`只包含`getSumPromise(1, 1)`传递的参数，回调函数除外。因此，您需要将`customCallback(err, result)`添加到`args[]`中，因为我们正在跟踪`customCallback`中的结果，所以原始函数`getSumAsync`会相应地调用这个函数。

### 将 customCallback 推送到参数[]

```
const myPromisify = (fn) => {
   return (...args) => {
     return new Promise((resolve, reject) => {
       function customCallback(err, result) {
         if (err) {
           reject(err)
         }else {
          resolve(result);
         }
        }
        args.push(customCallback)
        fn.call(this, ...args)
      })
  }
}
```

正如你所看到的，我们已经添加了`fn.call(this, args)`，它将在相同的上下文中使用参数`getSumAsync(1, 1, customCallback)`调用原始函数。那么我们的 promisify 函数应该能够相应地`resolve/reject`。

当原始函数期望一个带有两个参数`(err, result)`的回调时，上面的实现将会工作。这是我们最常遇到的情况。那么我们的自定义回调就是完全正确的格式，`promisify`非常适合这种情况。

****但是如果原来的**** `****fn****` ****期望一个回调用更多的自变量**像**`****callback(err, result1, result2, ...)****` ****呢？****

为了使其兼容，我们需要修改我们的`myPromisify`函数，这将是一个高级版本。

```
const myPromisify = (fn) => {
   return (...args) => {
     return new Promise((resolve, reject) => {
       function customCallback(err, ...results) {
         if (err) {
           return reject(err)
         }
         return resolve(results.length === 1 ? results[0] : results) 
        }
        args.push(customCallback)
        fn.call(this, ...args)
      })
   }
}
```

示例:

```
const getSumAsync = (num1, num2, callback) => {

  if (!num1 || !num2) {
    return callback(new Error("Missing dependencies"), null);
  }

  const sum = num1 + num2;
  const message = `Sum is ${sum}`
  return callback(null, sum, message);
}
const getSumPromise = myPromisify(getSumAsync)
getSumPromise(2, 3).then(arrayOfResults) // [6, 'Sum is 6']
```

仅此而已！谢谢你能走到这一步！

我希望你能理解这个概念。试着重读一遍。这是一点代码，但不会太复杂。让我知道它是否有帮助？

别忘了分享给刚开始用 Node.js 或者需要提升 Node.js 技能的朋友。

参考资料:

[https://nodejs . org/dist/latest-V8 . x/docs/API/util . html # util _ util _ promisify _ original](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original)

[https://github.com/digitaldesignlabs/es6-promisify](https://github.com/digitaldesignlabs/es6-promisify)

你可以在 [101node.io](https://101node.io/blog/write-your-own-promisify-function-from-scratch/) 阅读其他类似的文章。