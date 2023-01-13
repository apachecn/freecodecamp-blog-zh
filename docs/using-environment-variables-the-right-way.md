# 如何正确使用环境变量

> 原文：<https://www.freecodecamp.org/news/using-environment-variables-the-right-way/>

环境变量是应用程序开发人员的基本概念之一。它们是我们日常使用的东西。

环境变量甚至在事实上的十二因素应用程序中占有一席之地。他们有一长串的好处，包括应用程序的可配置性和安全性，这些都包含在许多资源中，如[的这个](https://hyperlane.co/blog/the-benefits-of-environment-variables-and-how-to-use-them)，甚至是[的 StackOverflow 的这个](https://serverfault.com/questions/892481/what-are-the-advantages-of-putting-secret-values-of-a-website-as-environment-var)。

环境变量很大，我完全支持这个想法。然而，任何事情都是有代价的——如果不小心使用环境变量，会对我们的代码库和应用程序产生有害的影响。

## 环境变量的诅咒

如果环境变量帮助我们编写更安全的代码，更容易地为不同的环境配置我们的应用程序，那么环境变量怎么会是一件坏事呢？

有趣的是，环境变量的缺点实际上来源于它们伟大的本质:它们是全局的和外部的，通过它们，应用程序开发人员能够注入配置，并在更难妥协的地方管理这些秘密。

作为开发人员，我们都知道全局状态对我们的应用程序有多糟糕。请不要相信我的话，这些邪恶已经在很多地方讨论过了，比如这里的、[这里的](https://thevaluable.dev/global-variable-explained/)和[这里的](https://stackoverflow.com/questions/19209468/why-is-global-state-bad)。

在这篇文章中，我将关注我在处理环境变量时最常遇到的两个主要缺陷:

*   不灵活/可测试性差
*   代码理解/可读性

## 如何正确使用环境变量

类似于我如何处理应用在坏位置的全局变量或全局模式(比如 singleton)，我最喜欢的武器是[依赖注入](https://en.wikipedia.org/wiki/Dependency_injection)。

这不会和我们对代码依赖所做的完全一样，但是原则是一样的。我们不直接使用环境变量(依赖项)，而是在调用点(也就是实际使用它们的地方)注入它们。这就把“依赖于调用点”的关系转变为“需要调用点”。

依赖注入通过以下方式解决了这些问题:

*   允许开发人员在测试时更容易地注入配置
*   将代码阅读者的思维范围缩小到包，消除所有的外部性

## 那么我们如何应用这些原则呢？

我将使用 Node.js 示例来演示我们如何重构代码库并消除分散的环境变量。

### 假设的情况

假设我们有一个简单的应用程序，它只有一个端点，可以查询 PostGres 数据库中的所有 TODOs。这是我们的数据库模块，其中分散着环境变量:

```
const { Client } = require("pg");

function Postgres() {
  const c = new Client({
    connectionString: process.env.POSTGRES_CONNECTION_URL,
  });
  this.client = c;
  return this;
}

Postgres.prototype.init = async function () {
  await c.connect();
  return this;
};

Postgres.prototype.getTodos = async function () {
  return this.client.query("SELECT * FROM todos");
};

module.exports = Postgres; 
```

这个模块将通过应用程序的入口点注入到我们的 HTTP 控制器中:

```
const express = require("express");
const TodoController = require("./controller/todo");
const Postgres = require("./pg");

const app = express();

(async function () {
  const db = new Postgres();
  await db.init();
  const controller = new TodoController(db);
  controller.install(app);

  app.listen(process.env.PORT, (err) => {
    if (err) console.error(err);
    console.log(`UP AND RUNNING @ ${process.env.PORT}`);
  });
})(); 
```

看一下上面的入口点文件，我们无法知道应用程序对环境变量(或一般的环境配置)的要求是什么(代码简略性的负分👎 ).

### 重构代码

改进先前布局代码的第一步是识别所有直接使用环境变量的位置。

对于我们上面的具体例子，这很简单，因为代码库很小。但是对于更大的代码库，你可以使用像 [eslint](https://github.com/eslint/eslint) 这样的林挺工具来扫描所有直接使用环境变量的位置。只需建立一个规则，比如禁止环境变量(比如 [eslint-plugin-node](https://github.com/mysticatea/eslint-plugin-node#readme) 的`node/no-process-env`)。

现在是时候从我们的应用程序模块中删除对环境变量的直接使用，并将这些配置作为模块需求的一部分:

```
...
function Postgres(opts) {
  const { connectionString } = opts;
  const c = new Client({
    connectionString,
  });
  this.client = c;
  return this;
}
... 
```

这些配置将只从我们应用程序的入口点提供:

```
...
const db = new Postgres({
  connectionString: process.env.POSTGRES_CONNECTION_URL,
});
... 
```

看看入口点，我们的应用程序现在的环境需求就清楚多了。这防止了忘记添加环境变量的潜在问题。

以上演示的完整源代码可以在[这里](https://github.com/stanleynguyen/dispelling-environment-variable-curse-demo)找到。

## 额外收获:常见问题

这些是我认为阅读这篇文章的人可能会问的一些问题。也许它们不是真正的常见问题，但是，嘿，提出可能的不同意见又有什么坏处呢？

### 为什么不使用中央配置文件/模块？

我见过很多尝试使用一个中心位置来提取这些值来解决这个问题(比如节点项目的`config.js`文件/模块)。

但是这种方法并不比实际使用应用程序运行时提供的环境变量(如`process.env`)更好，因为所有的东西仍然被整合在某种程度上的全局状态中(在整个应用程序中使用单个配置对象)。

事实上，情况可能会更糟，因为现在我们引入了另一个代码腐烂的位置。

### 如果我想对我的模块进行零配置设置，该怎么办？

是的，谁不喜欢零配置、现成的模块呢？我想再一次重申，构建软件就是要进行权衡，正如整篇文章所讨论的，这是以牺牲可读性为代价的。

如果您仍然希望有一个可能的零配置设置，我建议使用配置对象(即前面代码示例中的`opts`构造函数参数),并且只将环境变量的使用作为后备，如下所示:

```
function Postgres(opts) {
  const connectionString =
    opts.connectionString || process.env.POSTGRES_CONNECTION_URL;
  const c = new Client({
    connectionString,
  });
  this.client = c;
  return this;
} 
```

通过这种方式，我们代码的读者仍然能够识别模块的需求(尽管不那么容易看，因为它已经被换成了零可配置性)。

### 感谢您的阅读！

最后但同样重要的是，如果你喜欢我的作品，请前往[我的博客](https://blog.stanleynguyen.me/)获取类似的评论，并在 Twitter 上关注[我。🎉](https://twitter.com/stanley_ngn)