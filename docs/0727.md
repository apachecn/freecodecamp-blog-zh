# JavaScript Promises–解释了 promise.then、promise.catch 和 promise.finally 方法

> 原文：<https://www.freecodecamp.org/news/javascript-promise-methods/>

承诺是 JavaScript 中的一个对象，它将在未来的某个时候产生一个值。这通常适用于异步操作。

在应用程序中，异步操作经常发生。这可能是 API 请求、延迟的数据处理等等。

不必在数据加载之前阻止代码执行，您可以将它们定义为承诺，以便代码的其他部分继续执行。当承诺完成时，您可以使用其中的数据。

你可以在这篇简化的文章中了解更多关于[承诺的内容。](https://dillionmegida.com/p/javascript-promises/)

在某些情况下，一个承诺通过了，在其他情况下，它失败了。你如何处理每个结果的结果？

在本文的其余部分，我们将了解承诺的`then`、`catch`和`finally`方法。

## JavaScript 中的承诺状态

承诺有三种状态:

*   **待定**:承诺还在进行中
*   **兑现**:承诺解析成功，返回值
*   **拒绝**:承诺失败，出现错误

**履行**和**拒绝**状态有一个共同点:无论承诺是履行还是拒绝，承诺都是**解决**。因此，一个稳定的状态可能是一个履行的承诺，也可能是一个拒绝的承诺。

## 当承诺兑现时

当一个承诺实现时，您可以在承诺的`then`方法中访问解析的数据:

```
promise.then(value => {
 // use value for something
}) 
```

把`then`方法想成“这个有效，**然后**用从承诺返回的数据做这个”。如果没有数据，可以跳过`then`方法。

也有可能`then`方法可以返回另一个承诺，所以你可以像这样链接另一个`then`方法:

```
promise
  .then(value => {
    return value.anotherPromise()
  })
  .then(anotherValue => {
    // use this value
  }) 
```

## 当承诺被拒绝时

当承诺被拒绝(即承诺失败)时，您可以访问承诺的`catch`方法中返回的错误信息:

```
promise.catch(error => {
  // interpret error and maybe display something on the UI
}) 
```

承诺可能因为不同的原因而失败。对于 API 请求，可能是网络连接失败，或者是服务器返回的错误。这样的承诺如果有错误就会被拒绝。

把`catch`方法想成“这不起作用，所以我**捕捉**错误，这样它就不会破坏应用程序”。如果您没有捕捉到错误，这在某些情况下会破坏您的应用程序。

## 当承诺兑现时

承诺还有最后一个阶段。无论承诺实现还是被拒绝，承诺都已经完成(已经**结算**)。在这个完成的阶段，你可以`finally`做一些事情。

当你在承诺确定后想做某事时，承诺法很有用。这可能是你想要在`then`和`catch`方法中复制的清理或代码。

例如，不做:

```
let dataIsLoading = true;

promise
  .then(data => {
    // do something with data
    dataIsLoading = false;
  })
  .catch(error => {
   // do something with error
   dataIsLoading = false;
  }) 
```

您可以像这样使用`finally`方法:

```
let dataIsLoading = true;

promise
  .then(data => {
    // do something with data
  })
  .catch(error => {
   // do something with error
  })
  .finally(() => {
    dataIsLoading = false;
  }) 
```

不管承诺的结果(实现或拒绝)如何，都会调用`finally`方法。

## 包裹

承诺有`then`、`catch`和`finally`的方法，根据承诺的结果做不同的事情。总而言之:

*   `then`:当一个承诺成功时，你可以**然后**使用解析后的数据
*   `catch`:当一个承诺失败时，你**捕捉**错误，并用错误信息做一些事情
*   当一个承诺达成(失败或通过)，你可以**最终**做一些事情