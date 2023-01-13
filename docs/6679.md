# 在没有反射的情况下在 Go 语言中使用 Lodash 的好处

> 原文：<https://www.freecodecamp.org/news/the-benefits-of-using-lodash-in-the-go-language-without-reflection-1d64b5115486/>

我的天啊！

# 在没有反射的情况下在 Go 语言中使用 Lodash 的好处

![T7io7sSXVIifWU1Iaugs8ti6OaQu-ukfiBSB](img/8c23ee64af926171d4aa2655729d86ad.png)

在 Node.js 的工作中，我逐渐依赖于作为无价工具的 [Lodash](https://lodash.com/) 。它用一组方便的集合操作符完善了 JavaScript 标准库。我不记得我最近几年参与的任何一个 JavaScript 项目没有使用过它。

我换围棋的经历非常愉快。Go 解决了我多年来在 Node.js 上遇到的许多问题，但仍然保持了高生产率。不过，我一直非常想念一个东西——像 Lodash 这样的图书馆。

### 洛达什有什么了不起的？

JavaScript 不是纯粹的函数式语言，我用 JavaScript 写的大部分代码都是命令式的。然而，函数式编程的一些原则(如链接操作符和不变性)在处理集合时非常方便。

假设我有一个数组，里面有一些我想删除的重复项。只需要一行 Lodash:

假设我只想保留名称超过 4 个字母的颜色。这也需要一行代码:

现在假设我想大写每种颜色的第一个字母…是的，不超过一行。我甚至可以同时做到以上几点:

Lodash 可能是在 JavaScript 中处理集合的最方便的工具。

### 在围棋中你会怎么做？

让我们从第一个任务开始:获得一个独特的切片。在 Go 中搜索最佳实践解决方案，第一个结果是这篇[博文](https://kylewbanks.com/blog/creating-unique-slices-in-go)。这是代码，信用到[凯尔银行](https://www.freecodecamp.org/news/the-benefits-of-using-lodash-in-the-go-language-without-reflection-1d64b5115486/undefined):

你可以想象，如果我们必须过滤和大写每种颜色的第一个字母，我们将花费太多的时间在这些动作的机制上。这会有点累人。我的俏皮话在哪里？

### 拯救图书馆

当然，有一些图书馆为我们做这些方便的收集工作。令人惊讶的是，围棋中受欢迎的并不多。这是为什么呢？

要使这样的库有用，它必须支持多种类型的收藏。这是因为您可能有一部分字符串、一部分整数或一部分结构。在 Go 中支持一个通用类型的切片并不简单。

在大多数严格类型的语言中，这是通过一种叫做[泛型](https://en.wikipedia.org/wiki/Generic_programming)的语言结构来实现的。遗憾的是，Go 目前[还不支持它](https://golang.org/doc/faq#generics)。

那么，没有泛型，我们如何实现这样一个库呢？围棋大师 Rob Pike 展示了这个 [Github repo](https://github.com/robpike/filter) 中函数`filter`的一个示例实现。注意[反射](https://github.com/robpike/filter/blob/master/reduce.go#L22)的大量使用。事实上，扩展这种技术并实现各种 Lodash 实用方法并不困难。在这里，你可以看到一个尝试做那件事的示例项目[。](https://github.com/arifsetiawan/lodash-go)

### 为什么我们应该更喜欢避免反思？

反射在运行时检查类型。通过用反射解决 Go 缺乏泛型的问题，我们将处理多种集合类型的重担从编译时转移到了运行时。

处理集合的基本实用函数应该是高效的。我没有进行过适当的基准测试，但是依靠反射来完成这样一个普通的任务感觉是完全错误的。

那么，作为一个思想实验，我们能做些什么呢？我们能否在 Go 中创建一个真正高效的 Lodash 实现，在性能上与普通的 loop 方法相匹敌？

### 编译时代码生成

在 Go 中，生成代码作为开发工具链的一部分并不是一个陌生的概念。它被 [Protobuf 编译器](https://github.com/golang/protobuf)用来为协议定义生成 Go 访问方法。从 1.4 版本开始，它甚至被作为官方的 Go 工具链特性引入到`[go generate](https://blog.golang.org/generate)`中。

如果我们希望将处理泛型集合类型的重担转移到编译时，该怎么办？目前最好的方法是为我们项目中需要的每一种类型生成特定类型的代码！

考虑在`string`片上`uniq`之前的实现:

如果我们需要对`int`片的支持，实现会有什么不同？

正如你所看到的，除了每次出现的`string`都被替换成了`int`之外，这几乎是一样的。

我们能自动做到这一点吗？

### 无反射围棋中的 Lodash

让我们首先设计我们的 API。我们可以从 JavaScript 中最初的 [Lodash](https://lodash.com/) 实现中寻找灵感。在这里，库与下划线字符`_`一起使用。比如`_.uniq()`。

我们可以通过遵守同样的惯例来表达敬意。不同之处在于，在我们的例子中，下划线后面会跟一个类型。例如，`_int.Uniq()`表示整数，`_string.Uniq()`表示字符串。我们必须显式地指定类型，因为我们将为我们需要的特定类型获得整个库的全新和专用的实现。这将保证我们的运行时尽可能高效。

用法非常简单。只需导入您想要的实现:

有几十种潜在的类型。这是不是意味着我们的库必须和每一个人一起出现？不尽然，因为这不切实际。我们将在编译时动态生成所需的实现！

为了实现这一点，我们将引入一个有趣的命令行工具:`_gen`，它是我们的 Lodash 库实现的代码生成器(注意它是如何以下划线开始的)。

当`_gen`在任何项目的源根目录中运行时，它会检查项目中的所有源文件，并寻找`github.com/go-dash/slice/_TYPE`导入。在上面的例子中，它将为`_string`和`_int`分别找到一个。然后，生成器将为这些特定类型动态生成一个实现，并将其添加到 Go 工作区中的库，路径为`$GOPATH/src/github.com/go-dash/slice`。

在 [Github](https://github.com/go-dash/slice) 上`go-dash/slice`的实现其实很精益。它只包含单个泛型类型的模板化实现。当您编译项目时，代码生成器依赖此模板根据您的要求创建特定的类型实现。

### 自定义类型呢？

假设您的项目定义了一个定制的复杂类型，如`Person`:

如果我们也能在一个`Person`片上使用我们的 Lodash 库，那就太方便了。这实际上可以以完全相同的方式工作。只需导入一个在`Person`上工作的`go-dash/slice`的实现，代码生成器就会处理剩下的事情:

### 工作原型

在 [Github](https://github.com/go-dash/slice) 上可以找到一个`go-dash/slice`库的工作原理证明，它提供了几个有用的功能，比如`uniq`、`filter`和`chain`。

该项目还包括一个工作的[命令行生成器](https://github.com/go-dash/_gen)。

命令行生成器可以通过[自制软件](https://brew.sh/) : `brew install go-dash/tools/gen`方便地安装在 Mac 上

回到我们的第一个例子:假设我们有一个字符串颜色名称，我们希望保持唯一，并过滤长度超过 4 个字母的颜色。我们最终可以用一行代码实现它:

如果你喜欢这个方向，并且想帮助移植其他有用的 Lodash 函数，请投稿。

### 关于语法的一些最终想法

对于我们选择的语法，仍然困扰我的主要问题是我们有一个每类型的导入。有没有可能将它们组合成一个单一的实现，通过类型路由到正确的位置？

请考虑以下情况:

这段代码将所有的实现组合成一个统一的`Uniq`，它采用`interface{}`。虽然它本身不使用反射，但它仍然有两个运行时动态转换和会影响性能的开关。我们可能应该进行基准测试，看看这会增加多少影响。我们可能还应该对反射实现进行基准测试，看看我们对运行时性能的怀疑是否真的有根据。

然而，这是一个有趣的思想实验。

Tal 是 Orbs.com 的创始人，这是一个面向拥有数百万用户的大规模消费应用的区块链公共基础设施。要了解更多并阅读 Orbs 白皮书[点击这里](https://orbs.com/white-papers)。【跟进[电报](https://t.me/orbs_network)，[推特](https://twitter.com/orbs_network)， [Reddit](https://www.reddit.com/r/ORBS_Network/) ，

注意:如果你对区块链感兴趣——来投稿吧！Orbs 是一个完全开源的项目，任何人都可以参与。

![JWn9bUEABxzB3UAy7kCsYdYnHPsCsH8cDevu](img/f3495498f27bbf4dff11227445743845.png)