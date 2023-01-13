# 如何利用 JavaScript 的默认参数进行依赖注入

> 原文：<https://www.freecodecamp.org/news/how-to-take-advantage-of-javascripts-default-parameters-for-dependency-injection-98fc423328e1/>

作者里卡多·索萨

# 如何利用 JavaScript 的默认参数进行依赖注入

![1*FbjjCStVtHqlJdO4vpLLtQ](img/a3d24f1c4e3f966ae0928142d6bbb338.png)

如今，使用[依赖注入](https://en.wikipedia.org/wiki/Dependency_injection)非常普遍，它允许项目的模块松散耦合。但是随着项目复杂性的增长，我们有天文数字的依赖项需要控制。

为了解决这个问题，我们经常求助于依赖注入容器。但是在每种情况下都有必要吗？

在这篇文章中，我将介绍 JavaScript 的默认参数如何帮助我们解决这个问题。为此，我们将在 Node.JS 中实现一个简单的应用程序。它将具有使用三种不同方法创建和读取用户信息的功能:

1.  没有依赖注入
2.  使用依赖注入
3.  使用依赖注入和默认参数

### 项目结构

我们将通过特性来构建我们的示例项目[(顺便说一下，不要围绕角色来构建您的文件)。所以，结构应该是这样的:](https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/)

```
├── users/│   ├── users-repository.js│   ├── users.js│   ├── users.spec.js│   ├── index.js├── app.js
```

**注意**:在这个例子中，我们将用户信息保存在内存中。

### 没有依赖注入

通过分析前面的代码，我们验证了我们受到了语句的限制:**用户**中的`const usersRepo = require('./users-repository')`。使用这种方法，**用户**模块与**用户存储库紧密耦合。**

这限制了我们在不改变 **require** 语句的情况下使用另一个存储库的实现。当使用 **require** 时，我们创建一个对所需模块的静态依赖。这样，除了由**用户-存储库**模块定义的存储库之外，我们不能在应用模型中使用另一个存储库。

除此之外，由于前面提到的静态依赖，我们还被绑定到**用户规范**中的**用户库**。这些单元测试仅用于测试**用户**模块，仅此而已。想象一下，如果存储库连接到外部数据库。为了能够进行测试，我们必须与数据库进行交互。

### 使用依赖注入

通过依赖注入，**用户**模块不再与**用户-存储库**模块耦合。

与前一种方法的主要区别在于，现在我们在**用户**模块中没有静态依赖(我们没有语句:`const usersRepo = require('./users-repository')`)。取而代之的是，**用户**模块导出一个[工厂函数](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1)，带有一个用于存储库的参数。这允许我们将任何存储库传递给更高级别的模块。

工厂函数的替代方法是在函数 **create** 和 **read** 中为存储库的参数添加一个参数。但是如果这两个函数依赖于同一个存储库，我们可以用一个函数封装它们，并利用 [JavaScript 的闭包](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)。

现在，在 **app** 模块中，我们可以自由定义我们想要使用的存储库。另外，看看单元测试。我们现在可以测试**用户**模块，而不用担心存储库。刚刚[嘲讽了一下](https://en.wikipedia.org/wiki/Mock_object)！

然而，让我们诚实地说——我们多久定义一次在应用程序生命周期中变化的依赖关系？通常，我们会尽量避免静态依赖，因为这会增加测试的难度。但是现在，因为我们想要可测试性，所以每次我们想要使用它时，我们必须将存储库的一个实例传递给 **users** 模块。

你知道什么最棒吗？如果我们能够使用模块，而不必每次都给它一个存储库。我们可以用默认参数在 JavaScript 中实现这一点。

### 使用依赖注入和默认参数

使用这种策略，除了我们在前面的方法中看到的依赖注入之外，由**用户**模块导出的工厂函数中定义的参数现在是默认参数:`usersRepo = defaultUsersRepo`。

对于默认参数，如果我们不传递参数，函数将使用默认参数的值。否则，将使用参数的值。这与使用前面方法中定义的依赖注入技术是一样的。

现在，我们在**用户**模块中又有了静态依赖。但是，如果没有参数传递给工厂函数，这个静态依赖项只是用来定义默认参数中使用的值。

使用这种方法，当需要**用户**模块时，我们没有义务通过**应用**模块中的存储库。尽管如此，我们还是能做到。我们还可以验证单元测试可以继续使用模拟存储库，因为我们能够传递它，而不是使用默认参数的值。

### 结论

[默认参数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)是一个简单的 JavaScript 特性，但是非常强大。有了它们，我们可以实施更好的解决方案。

随时问我任何事情。

### 更多资源

GitHub 仓库中的例子:[这里](https://github.com/rikmms/js-default-params-and-di)。

[Mattias Petter Johansson](https://www.freecodecamp.org/news/how-to-take-advantage-of-javascripts-default-parameters-for-dependency-injection-98fc423328e1/undefined) 有很大的依赖注入讲解视频:

如果你喜欢这篇文章，请给我一些掌声，让更多的人看到它。谢谢大家！