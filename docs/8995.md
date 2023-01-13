# Node.js module.exports 与 exports

> 原文：<https://www.freecodecamp.org/news/node-js-module-exports-vs-exports-ec7e254d63ac/>

作者:lazlojuly

#### 它们是什么，如何使用它们以及如何不使用它们

(注意，本文是在 Node.js 6.1.0 发布之后撰写的)

#### **TL；博士**

*   将 module.exports 视为从 require()返回的变量。默认是空对象，改成什么都可以。
*   出口？嗯，“出口”本身是永远不会退回来的！它只是对 module.exports 的引用；一个方便的变量，帮助模块作者编写更少的代码。使用它的属性是安全的，建议这样做。

```
exports.method = function() {…} 
```

```
vs.
```

```
module.exports.method = function() {…}
```

### 简单的模块示例

首先，我们需要一个示例代码库。让我们从一个简单的计算器开始:

用法:

### 模块包装

Node.js **在函数包装器中内部包装**所有需要的()-ed 模块:

### 模块对象

变量“**模块**是代表当前模块的对象。它**是每个模块**本地的 **，并且也是私有的(只能从模块代码访问):**

### 模块.导出

*   从 require()调用中返回的是对象引用。
*   它由 Node.js 自动创建。
*   它只是对普通 JavaScript 对象的引用。
*   默认情况下它也是空的(我们的代码为它附加了一个“add()”方法)

有两种方法可以使用 module.exports:

1.  **给它附加公共方法**(就像我们在计算器例子中做的那样)。
2.  **用我们自定义的对象或函数替换** **it** 。

为什么要更换？当替换时，我们可以返回其他类的任意实例。下面是一个用 ES2015 编写的示例:

以上，“计算器基础”导出课件。
让我们扩展“Calculator”类，这次导出一个实例:

用法:

### 导出别名

*   **“exports”只是一个方便的变量，因此模块作者可以编写更少的代码**
*   使用它的属性是安全的，建议这样做。
    (例如:exports.add = function…)
*   **Exports** 不是 require()返回的(module.exports 是！)

以下是一些好的和坏的例子:

**注意:**用自定义函数或对象替换 module.exports 是常见的做法。如果我们这样做了，但仍然想继续使用“出口”的简写；然后“exports”必须重新指向我们新的自定义对象(也是在上面第 12 行的代码中完成的):

```
exports = module.exports = {}
```

```
exports.method = function() {...}
```

### 结论

一个名为**的变量 exports** 并没有被完全导出，这让人感到困惑，尤其是对于 Node.js 的新手来说。甚至官方文档也对它有一点奇怪的理解:

> 作为指导，如果导出和 module.exports 之间的关系对您来说似乎很神奇，那么忽略导出，只使用 module.exports。

我认为代码不是魔法。开发人员应该始终寻求对他们所使用的平台和语言的更深入的理解。通过这样做；程序员获得了宝贵的信心和知识，这反过来积极地影响代码质量、系统架构和生产力。

谢谢你看我的帖子。欢迎在评论区提出反馈和想法。

[lazlojuly](https://twitter.com/lazlojuly)

#### 相关文章:

*   [node . js 模块是单线态的吗？](https://medium.com/@lazlojuly/are-node-js-modules-singletons-764ae97519af)

#### 来源:

*   [关于模块的 Node.js 文档](https://nodejs.org/api/modules.html)

**看看我关于单元测试的新博客系列:**

[**如何入门单元测试？第一部分**](https://medium.com/@lazlojuly/how-to-get-started-with-unit-testing-part-1-7f490bbf560a)
[*我想我们很多人都会联想到上面描述的情况。*](https://medium.com/@lazlojuly/how-to-get-started-with-unit-testing-part-1-7f490bbf560a)
[*在一个地方，单元测试被认为是一件苦差事。*medium.com](https://medium.com/@lazlojuly/how-to-get-started-with-unit-testing-part-1-7f490bbf560a)