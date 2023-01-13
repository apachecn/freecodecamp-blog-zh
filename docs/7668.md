# 回调和承诺在 API 和谐中共存

> 原文：<https://www.freecodecamp.org/news/callbacks-and-promises-living-together-in-api-harmony-7ed26204538b/>

在本文中，我们将学习如何更新基于回调的 API 来支持承诺。

首先，什么是 API，或者说应用程序编程接口？它有时被称为*模块*。它是开发人员可以在自己的应用程序中使用的方法和变量的集合。

在这里观看附带的真实编码插曲[。](https://youtu.be/6DphXwRbPlo)

### 回调函数

许多 JavaScript API 和模块在它们的方法中为回调方法提供了一个最终参数。大多数时候你会看到这被定义为`done`、`next`、`callback`或`cb`(回调的缩写)。回调函数非常有用，因为它们使其他开发人员能够从您的函数中获得更多，比如错误处理和异步请求。

例如，一个 API 方法可能会产生各种错误，如果处理不当，这些错误会导致整个应用程序崩溃。利用回调方法**的 API 应该**返回所有的`Error`对象作为回调中的第一个参数。假设回调函数中的第一个参数总是一个错误实例。

下面的函数是一个简单的例子。其目的是将参数`x`加倍，并通过指定的`callback`函数返回。`error`始于`null`。如果任何条件检查失败，一个`Error`实例被分配给`error`。那么如果`error`存在(它不为空或未定义)，那么我们不 double `x`，我们将变量`double`设置为`null`；否则，`x`被加倍并赋给`double`变量。一切完成后，函数`doublePositiveOnly`将返回回调方法，第一个参数引用`error`变量，第二个参数引用`double`变量。

```
function doublePositiveOnly(x, callback) {
  let error
  if ( !x )
    error = new Error('x must be defined')
  if ( typeof x !== 'number' )
    error = new Error('x must be a number')
  if ( x < 0 )
    error = new Error('x must be positive')

  const double = error ? null : x * 2

  return callback(error, double)
}
```

你会如何使用这个功能？

```
doublePositiveOnly(16, function (err, result) {
  if (err) console.error(err.message)
  console.log(result)
})
```

### 承诺功能

生产中的 Promise 函数很容易识别，因为它们利用`.then` 和`.catch`方法将信息返回给用户。几乎所有的回调函数都可以被承诺代替，所以让我们使用承诺来重建我们的`doublePositiveOnly`方法。

```
function doublePositiveOnly( x ) {
  return new Promise(function (resolve, reject) {
    let error
    if ( !x )
      error = new Error('x must be defined')
    if ( typeof x !== 'number' )
      error = new Error('x must be a number')
    if ( x < 0 )
      error = new Error('x must be positive')

    error ? reject(error) : resolve(x * 2)
  })
}
```

上述函数服务于回调实现的完全相同的目的。但是，这个版本不再将回调方法作为参数。相反，它要么是一个错误，要么是结果。你可以这样使用这个方法:

```
doublePositiveOnly(16).then(function (result) {
  // do cool stuff with the result
  console.log(result)
}).catch(function (err) {
  // oh no an error! Handle it however you please
  console.error(err.message) 
})
```

Promise 函数的可读性比回调函数清晰得多，因为您可以轻松地处理结果以及任何潜在的错误。关于 Promises 函数还有很多我没有在这里介绍，我鼓励你尽可能多地了解它们。

### 一起回电和承诺

我们有回调，我们有承诺。它们可以互换，并且都满足相似的需求。现在考虑这样一个场景，我们有一个只支持回调方法的 API。这个 API 被下载了 1000 次，现在正在无数的应用程序中运行。但是现在维护者也想支持承诺。他们能在维护回调支持的同时做到这一点吗？**是的！**

让我们再次看看`doublePositiveOnly`的回调实现，但是现在也有了承诺支持:

```
function doublePositiveOnly(x, callback) {
  const func = this.doublePositiveOnly

  if ( callback === undefined ) {
    return new Promise(function (resolve, reject) {
      func(x, function (err, result) {
        err ? reject(err) : resolve(result)
      })
    })
  }

  let error
  if ( !x )
    error = new Error('x must be defined')
  if ( typeof x !== 'number' )
    error = new Error('x must be a number')
  if ( x < 0 )
    error = new Error('x must be positive')

  const double = error ? null : x * 2

  return callback(error, double)
}
```

就像这样,`doublePositiveOnly`方法现在也支持承诺。它之所以有效，是因为它首先将对函数的引用存储在`func`变量中。接下来，它检查回调是否被传递给了函数。如果没有，它返回一个承诺，将`x`参数传递给另一个`doublePositiveOnly`调用，并且它包含一个回调函数。这个回调函数要么`rejects`要么`resolves`承诺，就像仅承诺实现所做的那样。

这个解决方案的伟大之处在于，你可以在任何地方使用它，而且你不需要编辑原始功能的任何部分！你可以在我最近贡献的一个名为 [fastify-jwt](https://github.com/fastify/fastify-jwt/) 的模块中看到它的运行。`[requestVerify](https://github.com/fastify/fastify-jwt/blob/master/jwt.js#L108-L114)` [](https://github.com/fastify/fastify-jwt/blob/master/jwt.js#L108-L114)和`[replySign](https://github.com/fastify/fastify-jwt/blob/master/jwt.js#L79-L85)`方法都支持回调和承诺。

如果您有任何问题，请联系我们！

你可以在 [Github](https://github.com/Ethan-Arrowood) 和 [Twitter](https://twitter.com/ArrowoodTech) 上关注我，或者查看我的[网站](https://ethanarrowood.com)。

继续努力。

~伊森·阿罗伍德