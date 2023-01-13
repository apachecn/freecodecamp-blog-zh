# 我最喜欢的代码行

> 原文：<https://www.freecodecamp.org/news/my-favourite-line-of-code-53627668aab4/>

每个开发人员都有自己喜欢的模式、功能或代码。这是我的，我每天都用它。

### 这是什么？

这个小函数接受一个承诺，并返回一个包含错误和承诺结果的数组。它超级简单，但可以用于一些惊人的事情。

### 我能做什么

#### 用 async / await 进行干净的错误处理

这是我每天使用这种方法的主要原因。在工作中，我们试图使用`async` / `await`语法来编写所有代码，以提高未来的可读性和可维护性。问题是，等待承诺并不能告诉你承诺是成功了还是失败了。

```
let unimportantPromiseTask = () => {
    Math.random() > 0.5 ? 
        Promise.reject('random fail') : 
        Promise.resolve('random pass');
};

let data = await unimportantPromiseTask();
```

如果该承诺通过，则`data = ‘random pass'`，但是如果它失败，则存在未处理的承诺拒绝，并且数据从不被赋值。这可能不是您在通读代码时所期望的。

将 promise 传递给这个`handle`函数会返回一个非常明确的结果，任何人在阅读时都很容易理解。

```
let [err, res] = await handle(unimportantPromiseTask());
```

然后，您可以对错误和结果做您想做的事情。下面是我们经常使用的一种常见模式:

```
if (err 
 (res && !res.data)) { 
    // error handling
    return {err: 'there was an error’}
}
// continue with successful response
```

我们使用这个而不是用`try / catch`块包装等待的承诺的主要原因是我们发现它更容易阅读。

#### 停止未处理的承诺拒绝

这个函数可以用来处理承诺(因此得名)。因为该函数将`.catch`链接到承诺上，如果失败，错误就会被捕获。这意味着如果有一个你打电话的承诺，不管它是通过还是失败，只要把它传入`handle`！

```
unimportantPromiseTask(); // 50% chance of erroring
handle(unimportantPromiseTask()); // never errors
```

如果没有将承诺传递给`handle`函数，它就有可能失败。这变得越来越重要，因为 Node 的未来版本将在遇到*未处理的承诺拒绝*时终止进程。

处理承诺拒绝的其他方法是将函数包装在一个 try catch 中，或者只是将一个`.catch`链接到承诺上。虽然这两个都非常有效，但是使用`handle`可以使我们的代码更加一致。

感谢阅读这篇关于我最喜欢的代码行的快速帖子。如果你有最喜欢的一行代码，请在评论中告诉我是什么，为什么！