# 如何避免 JavaScript 中的空检查污染:使用选项

> 原文：<https://www.freecodecamp.org/news/avoiding-null-check-pollution-in-javascript-4ed8e2702ce3/>

康斯坦丁·布洛欣

# 如何避免 JavaScript 中的空检查污染:使用选项

![1*uJIvAC_iHveiJ5BEse6YMA](img/4a402fcaf080784c910c3d2e9e80ac5c.png)

[Wash your code!](https://dribbble.com/shots/2634230) by [*Charlie Chauvin*](https://dribbble.com/charliechauvin)

在过去的几年里，我一直在使用 JavaScript，并且总体上很喜欢它。但是它缺少其他语言的一些很酷的特性。例如，没有内置的安全导航，也没有避免空检查的方法。代码被样板条件分支污染了。它容易出错，可读性差。

你可能会问，空支票有什么问题？

首先，空引用的发明者，[东尼·霍尔](http://en.wikipedia.org/wiki/Tony_Hoare)，[称之为](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)它犯了一个**的亿元错误:**

> 但是我无法抗拒放入空引用的诱惑，仅仅是因为它太容易实现了。这导致了数不清的错误、漏洞和系统崩溃，在过去的四十年里，这可能造成了数十亿美元的痛苦和损失。

JavaScript 也不例外。你遇到过多少次`TypeError: Cannot read property 'bar' of null`错误？为了避免这些，开发人员总是不得不把这种零可能性记在心里。他们更愿意专注于真正的东西，比如特定应用逻辑。

接下来，您实际上必须在代码中引入新的条件分支。通常你不希望有太多的语句，因为`**if**` **语句会降低代码的可读性。**以[箭头反模式](http://wiki.c2.com/?ArrowAntiPattern)为极端例子。此外，就您的领域或业务逻辑而言，这些陈述几乎毫无意义。

我喜欢解决这个问题的一种方法，Michael Feathers 在这里解释了“命令代替查询”的方法。他也发展了这个想法，[讲述了](https://www.youtube.com/watch?v=AnZ0uTOerUI)无条件代码的好处。

基本上，我们不是查询一段数据并检查它是否存在以供进一步处理，而是表达我们的意图并让模块内部决定是否应该采取行动。

假设我们想获取一个用户及其朋友最喜欢的书籍进行推荐。代码可能是这样的:

所以，我宁愿用一些模块来封装检查逻辑。因此，我们只是告诉它如何处理活跃用户:

代码现在更加关注业务逻辑。我们可以重用它来执行活动用户的其他操作，而没有空检查的负担。

但是给出的解决方案有点太具体了。此外，在我们的控制器或其他使用该函数的地方，我们仍然有一个空检查。

需要一种更通用的方法。我们需要一个特殊的数据类型，一个可空值的容器——比如 Java 中的[可选](https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html)类。要点是，给予容器的任何动作将只在非空的包含值上执行。

还有一些 JS 库(比如 [Optional.js](https://github.com/JasonStorey/Optional.js) )，实现了和 Java Optional 几乎一样的接口。但是他们没有考虑到 JS 的异步特性，也没有承诺。

在大多数情况下，当可能缺少值时，我们实际上必须处理承诺和异步函数。例如，像数据库查询和 API 调用这样的外部资源请求。

这时候[异步](https://github.com/treble-snake/async-optional)来救援了。因此，**是一个异步可选值的容器**。*可选*表示该值可能存在也可能不存在。`null`和`undefined`都被认为缺席。

只要我们想告诉程序如何处理非空值，工厂方法就被称为“with”:

```
const withUser = AsyncOptional.with(user);
```

然后我们可以做一些处理，如下例所示。一旦我们完成了，并且想在某个地方修正结果，应该使用一个终端方法。

例如，当我们不需要对空值做出反应时:

我们还可以用这种类似自然语言的方式指定在缺少值的情况下做什么:

在工厂和终端方法调用之间，可以有任何处理逻辑，在[自述文件](https://github.com/treble-snake/async-optional#transform-it)中有描述。保证不会对空值采取任何操作。

您可以用来处理该值的一些操作:

让我们最后看一下我们的例子:

那么，和最初的相比怎么样更好呢？

我认为答案是——它更加清晰易读，可读性是好代码的本质。

首先，我们摆脱了空检查条件分支，因此我们可以专注于重要的事情——即系统的业务逻辑。

接下来，我们消除了这里出现空指针异常的可能性。这意味着我们不必记住值的可空性，这是引入 bug 的一个更少的方法。

库可以派上用场的另一种情况是在“缺省值”的情况下。假设我们有某种用户要填写的表单，在其他字段中有水果选择。用户可以选择橘子、苹果或者什么都不选。

所以输出是:

```
conditionalChooseFruit(‘Joe’);// => You chose nothing, Joe.
```

```
conditionalChooseFruit(‘Joe’, {notFruit: ‘x’});// => You chose nothing, Joe.
```

```
conditionalChooseFruit(‘Joe’, {fruit: 1});// => You chose apple, Joe.
```

```
conditionalChooseFruit(‘Joe’, {fruit: 11});// => You chose a wrong gardener to mess with, Joe.
```

对我来说，即使有 async/await 和 reversed 条件的帮助，这个方法看起来也有点乱。条件分支模糊了业务逻辑。

使用 AsyncOptional，可以用更简单的方式重写它:

那不是可读性更强吗？

因此， [AsyncOptional](https://github.com/treble-snake/async-optional) 库可以帮助您:

*   编写你可能会觉得更可读、更易维护、更干净、更美观的代码；
*   更好地避免空相关的`TypeErrors`，从而增加你的系统的稳定性；
*   以同样干净的方式处理承诺和异步函数(它也适用于同步函数)。

*如果你喜欢这些例子，请随意浏览 [GitHub](https://github.com/treble-snake/async-optional) repo 中的 [Readme](https://github.com/treble-snake/async-optional#about) 文件，甚至查看完整的 [API 文档](https://github.com/treble-snake/async-optional/blob/master/docs/APIDOC.md)。我甚至敢建议你可以通过 npm 安装[包。:)我也很感激任何反馈。](https://www.npmjs.com/package/async-optional)*

[**异步可选**](https://www.npmjs.com/package/async-optional)
[*可选实现与异步支持*www.npmjs.com](https://www.npmjs.com/package/async-optional)