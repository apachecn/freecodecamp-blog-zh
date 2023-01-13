# 如何消除(或处理)隐藏的依赖

> 原文：<https://www.freecodecamp.org/news/eliminating-hidden-dependencies-a95c7b03aa54/>

作者:Shalvah

# 如何消除(或处理)隐藏的依赖

![odAr3dMCBRLyHI-rlaIudmFTMUp96ncKZz2S](img/21fa42266570a0a57705ef3ed76578b4.png)

Photo by [Blake Connally](https://unsplash.com/photos/B3l0g6HLxr8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在软件中，当应用程序中的一个模块 *A* 依赖于另一个模块或环境 *B* 时，就会产生依赖。当 *A* 以一种不明显的方式依赖 *B* 时，一种隐藏的依赖性就发生了。

要发现一个隐藏的依赖关系，通常需要深入研究模块的源代码。模块可以指任何东西，从整个服务到一个类或函数，再到几行代码。

这里有一个依赖关系的小例子，比较了它的两种表达方式:

```
let customer = Customer.find({id: 1});
```

```
// explicit — the customer has to be passed to the cartfunction Cart(customer) { this.customer = customer;}let cart = new Cart(customer);
```

```
// hidden — the cart still needs a customer,// but it doesn’t say so outrightfunction Cart() { this.customer = customer; // a global variable `customer`}let cart = new Cart();
```

注意到细微的差别了吗？Cart 构造函数的两种实现都依赖于一个客户对象。但是第一个要求您传入这个对象，而第二个要求环境中已经有一个可用的 customer 对象。

一个 dev seeing `let cart = new Cart()` 没有办法知道 cart 对象依赖于一个全局客户变量，除非他们看一下 Cart 构造函数。

#### 野外隐藏的依赖

我将分享一些我在现实世界代码库中遇到的隐藏依赖的例子。

*   **PHP `include`文件**

我们拿一个典型的 PHP 后端 app 来说。在我们的“index.php ”(我们应用程序的入口点)中，我们可以有这样的内容:

```
include 'config.php';include 'loader.php';$app = new Application($config);
```

代码看起来很可疑，不是吗？`$config`变量从何而来？让我们看看。

`include`指令类似于 HTML `<scri` pt >标签。它告诉解释器获取指定文件的内容，执行它，如果它有返回语句，将返回值传递给调用者。这是一种将代码拆分到多个文件中的方法。l`ike a &l`t；scri `pt>` 标签，include 也可以把变量放入全局范围。

让我们看看我们包括的文件。`config.php`文件包含后端应用程序的典型配置设置:

```
$config = [  'database' => [    'host' => '127.0.0.1',    'port' => '3306',    'name' => 'home',  ],  'redis' => [    'host' => '127.0.0.1',  ]];
```

`loader.php`基本上是一个自制的类加载器。以下是其内容的简化版本:

```
$loader = new Loader(__DIR__);$loader->configure($config);
```

看到问题了吗？`loader.php`中的代码(以及`index.php`中的其余代码)依赖于某个名为`$config`的变量，但是在打开`config.php`之前并不清楚`$config`在哪里定义。这种编码模式其实并不少见。

*   **包括通过`<scri`pt>T2 的 JavaScript 文件；标签**

这大概是一个比较常见的例子。比较下面两段代码片段(假设`cart-fx`和`cart-utils` 是一些随机的 JS 库):

附件 A:

```
<script src="//some-cdn/cart-fx.js"></script><script src="//some-cdn/cart-utils.js"></script>
```

```
/* lots and lots of code */
```

```
<script>var cart = new Cart(CartManager.default, new Customer());</script>
```

附件 B:

```
import Cart from ‘cart-fx’;import CartManager from ‘cart-utils’;
```

```
/* lots and lots of code */
```

```
const cart = new Cart(CartManager.default, new Customer());
```

在第二个例子中，很明显`Cart` 和`CartManager` 变量是分别从`cart-fx`和`cart-utils`模块引入的。首先，我们只能猜测哪个模块拥有`Cart`，哪个模块拥有`CartManager`。别忘了还有`Customer` ！(记住，我们自己的代码也是一个模块。)

*   **从环境中读取**

我是这件事的罪魁祸首。我前段时间做了一个 PHP 包，用来和一个 API 交互。这个包允许您将 API 键传递给构造函数。不幸的是，它也允许您将您的键指定为环境变量，包会自动使用它们。[偷看一眼，别笑我](https://github.com/Hng-X/moneywave-php/blob/ef4d491c9a23f084d7a13e7279219213e8d4f87f/README.md#configuration)。

当我这样做的时候，我相信我让开发人员的生活变得更容易了。然而，实际上，我是在对代码运行的环境进行假设，并将包的功能与环境中满足的某些条件联系起来。

#### 那么，为什么隐藏依赖是危险的呢？

我能想到两个主要原因:

*   **在没有移除依赖关系**的情况下，很容易无意中移除所依赖的模块。例如，以我上面的包为例。想象一个开发者建立了一个应用程序，在一个新的环境中使用这个包。在决定从旧环境中继承哪些环境变量时，开发人员可能会忽略添加包所需的变量，*，因为他们在代码库*中找不到它们。
*   对相关代码的小改动可能会破坏整个应用程序，或者使其出错。以我们上面的`index.php`文件为例——交换前两行看似无害的改变，但它会破坏应用程序，因为第 2 行依赖于第 1 行中设置的变量。更严重的情况是这样的:

```
$config = […];include 'bootstrap.php';$app = new Application($config);
```

假设我们的`bootstrap.php`文件对`$config`做了一些重要的修改。如果出于某种原因，第二行被移到底部，应用程序将运行而不会抛出任何错误，但是`bootstrap.php`所做的关键配置更改将不会被应用程序看到。

#### 摆脱隐藏的依赖关系

像许多软件工程一样，没有处理隐藏依赖的硬性规则，但是我发现了一些对我有用的基本原则:

1.  *编写真正模块化的代码*，而不仅仅是分散在多个文件中。一个理想的模块应该是独立的，并且对共享的全局状态有最小的依赖。一个模块也应该明确地声明它的依赖关系。
2.  *减少模块需要对其环境或其他模块做出的*假设的数量。
3.  *露出一个清晰的界面。*理想情况下，除了函数/类签名之外，你的模块的用户不需要看源代码就能知道模块的依赖关系。
4.  避免乱扔环境垃圾。抵制向父作用域添加变量的诱惑。尽可能经常地，倾向于向调用者显式地返回或导出变量。

我将演示如何使用这些原则重构上面的第一个例子。首先要做的是让配置文件**返回**配置数组，这样调用者就可以显式地将它保存在一个变量中:

```
// config.phpreturn [  'database' => [    'host' => '127.0.0.1',    'port' => '3306',    'name' => 'home',  ],  'redis' => [    'host' => '127.0.0.1',  ]];
```

接下来我们要做的是改变加载文件来返回一个函数。这个函数将接受一个配置参数。通过这种方式，我们明确了加载文件的过程依赖于一些预设的配置。

```
// loader.php
```

```
return function (array $config){  $loader = new Loader(__DIR__);  $loader->configure($config);}
```

将这些放在一起，我们最终得到的`index.php` 文件如下所示:

```
$config = include 'config.php';(include 'loader.php')($config);
```

```
$app = new Application($config);
```

我们甚至可以更进一步，在调用返回的函数之前，将它保存在一个变量中:

```
$config = include 'config.php';
```

```
$loadClasses = include 'loader.php';$loadClasses($config);
```

```
$app = new Application($config);
```

现在，任何人看到`index.php`一眼就能看出:

1.  文件`config.php`返回 ***某物*** (我们可以猜测这是某种配置，但现在那不重要)。
2.  加载器文件和`Application`都依赖于那个*来完成它们的工作。*

*好多了，不是吗？*

*让我们来看看第二个例子。我们可以用几种方式重构它:切换到受支持的浏览器的`import` / `require` ，或者使用提供聚合填充的构建工具。但是我们可以做一个小小的改变，让事情变得更好:*

```
*`<script src="//some-cdn/cart-fx.js"></script><script src="//some-cdn/cart-utils.js"></script>`*
```

```
*`/* lots and lots of code */`*
```

```
*`<script>var cart = new CartFx.Cart(CartUtils.CartManager.default, new Customer());</script>`*
```

*通过将`CartManager` 和`Cart` 对象附加到全局`CartFx` 和`CartUtils` 对象上，我们已经有效地将它们移动到名称空间中。我们将对这些库想要提供的任何其他变量做同样的事情，将潜在的隐藏依赖项的数量减少到每个模块只有一个。*

#### *有时候，你就是忍不住*

*有时候，您可能会受到可用工具、有限资源等等的限制。但是，重要的是要记住，对于代码作者来说显而易见的事情，对于新手来说可能并不如此。寻找一些你可以改进的小优化。*

*有任何关于隐藏依赖或处理它们的技术的经验吗？请在评论中分享。*