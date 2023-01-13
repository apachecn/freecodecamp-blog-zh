# 如何在 TypeScript 中使用函数式编程使您的生活更轻松

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-life-easier-using-functional-programming-in-typescript-a2def76c468b/>

让马提乌斯成为猎鹰

# 如何在 TypeScript 中使用函数式编程使您的生活更轻松

在过去的两年中，JavaScript 社区一直在谈论函数式编程。函数式编程允许我们不用设计复杂的类树就能构建更好的软件。今天，我将解释如何在[类型脚本](https://www.typescriptlang.org/)和 [Lodash](https://lodash.com/) 中使用函数组合。

代码可以在 Github 上找到[。](https://github.com/mateuszsokola/function-composition-typescript)

![1*HMvoDO2f0kCl49CLCdzMEQ](img/37d02ab1ee4c1c9660df06cc1f0a7e89.png)

#### 什么是函数构成？

功能组合是指将两个或多个功能组合起来，创建一个更复杂的功能。感到困惑？别担心，下面的例子会让你明白:

```
const f = function (a) { return a + 1 };const g = function (b) { return b * b };
```

```
const x = 2;const result = f(g(x)); // => 5
```

我在这里组合了两个函数——函数 *f* 和函数 *g.* 函数 f 给 *a* 参数加 1，函数 *g* 将 *b* 参数乘以 *b* 。结果是 5。

让我们对它进行逆向工程:

1.  常数 *x* 等于 2
2.  常数 *x* 成为函数 *g* 的自变量
3.  函数 *g* 返回 4
4.  函数 *g* 输出(4) 成为函数 *f* 的一个自变量
5.  函数 *f* 返回 5

不是火箭科学，但看起来也不是特别有用。实际上，它看起来比保持在一个函数中更复杂。这可能是真的，但是让我们考虑一些实际的用例。

### 现实世界的例子:货币格式化

我为开发人员创建了一个简单的职位发布。其中一个要求是在每份工作机会旁边显示薪资范围。所有的薪水都以美分的形式存储，需要看起来像这样:

```
from: 6000000 to:   60,000.00 USD
```

看起来很容易，但是处理文本很难。几乎每个开发者都讨厌它。

我们都花费数小时编写正则表达式和处理 unicode。当我需要格式化文本时，我总是试图用谷歌搜索解决方案。在删除了所有的库(对我来说太多了)和所有糟糕的代码片段之后，就没剩下多少了。

我决定需要在我自己的格式化程序上构建它。

#### 我们如何构建它？

在我们开始写代码之前，让我们深入了解一下我发现的一个想法:

1.  分割美元和美分。
2.  格式美元——添加千位分隔符并不容易。
3.  格式化美分——处理美分很容易，几乎是现成的。
4.  用分隔符将美元和美分连在一起。

现在看起来很清楚了。我们需要考虑的最后一件事是如何给美元加上千位分隔符。让我们考虑下面的算法:

1.  反串。
2.  将字符串每隔 3 个字符拆分到数组中。
3.  通过在数组元素之间添加千位分隔符，将所有数组元素连接在一起。
4.  反串。

所有这些步骤都可以翻译成下面的伪代码:

```
1\. "60000"        => "00006"2\. "00006"        => ["000", "06"]3\. ["000", "06"]  => "000.06"4\. "000.06"       => "60.000"
```

这就是这个算法如何被转化成复合函数:

在第一行，您会注意到我使用了 [Lodash](https://lodash.com/docs/) 库。Lodash 包含许多实用程序，使得函数式编程更加容易。让我们分析第 22 行的代码:

```
return flow([  reverse,  splitCurry(match),  joinCurry(separator),  reverse]);
```

[流](https://lodash.com/docs/4.17.4#flow)函数是一个函数编辑器。它将*反向*函数的结果通过管道传输到*split cury*的输入，依此类推。这创造了一个全新的功能。还记得上面的千分算法吗？就是这样！

你可以看到我用“Curry”后缀了 split 和 join 函数并调用了它们。这种技术被称为 currying。

#### 什么是 Currying？

[curry](https://lodash.com/docs/4.17.4#curry)是将一个多自变量函数翻译成一个单自变量函数的过程。如果仍然需要任何参数，单参数函数将返回另一个函数。

听起来很难？考虑下面的例子:

*split* 函数需要两个参数——一个字符串和分隔模式。该函数需要知道如何拆分文本。在这种情况下，我们不能组合这个函数，因为它需要不同于其他函数的参数。这就是 currying 的用武之地。

```
import { curry } from "lodash";import split from "./split";
```

```
const splitCurry = curry(split);
```

```
splitCurry("000001")(/[0-9]{1,3}/g); // => ["000", "001"]
```

现在 *splitCurry* 功能与 *reverse* 功能一致。两者都需要一个论证。不幸的是，我们还没有用分离模式调用 *splitCurry* 函数。这样是行不通的。

如果我们颠倒咖喱的顺序会怎么样？

```
import { curryRight } from "lodash";import split from "./split";
```

```
const splitCurry = curryRight(split);
```

```
splitCurry(/[0-9]{1,3}/g)("000001"); // => ["000", "001"]
```

现在我们的代码可以工作了，因为我们可以使用 curries 作为工厂函数。让我们再看一遍代码:

```
return flow([  reverse,  splitCurry(match),  joinCurry(separator),  reverse]);
```

所有这些函数都有一个参数(字符串)，所以我们可以将它们组合在一起。然后将它们作为独立的函数使用。等一下…

#### 什么是工厂功能？

工厂函数是创建新对象的函数。在我们的例子中，是一个函数。让我们再次考虑我们的千位分隔符。理论上，我们可以一直使用同一个函数。不幸的是，有些国家用逗号分隔千位，有些国家用点分隔千位。当然，我可以参数化分隔符，但我决定使用工厂函数。

```
const formatUS = thousandSeparator(',');const formatEU = thousandSeparator('.');
```

```
formatUS(10000); // 10,000formatEU(10000); // 10.000
```

### 摘要

去年，我花时间更新了以前学习的知识，以便在日常工作中应用。在本文中，我没有深入研究单子定律或幺半群——我不想让它变得混乱。这门学科非常广泛，深深植根于计算机科学。我想在尽可能简要地描述所有术语的同时，给你一个关于如何进行函数式思考的想法。

我决定保留我的 Github 上的 [**所有代码，这样文章更容易阅读。**](https://github.com/mateuszsokola/function-composition-typescript)

**如果你有任何问题或建议，请写下评论。**

—

PS。我在 YouTube 上开设了一个关于节目 的频道。请查看并订阅:)