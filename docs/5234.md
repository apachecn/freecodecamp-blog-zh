# 如何在 JavaScript 中使用正则表达式来确认字符串的结尾

> 原文：<https://www.freecodecamp.org/news/how-to-use-a-regexp-to-confirm-the-ending-of-a-string-in-javascript-4b42f3749af1/>

凯瑟琳·瓦桑特(又名 Codingk8)

# 如何在 JavaScript 中使用正则表达式来确认字符串的结尾

使用正则表达式？️建造师

![0*YLruZvgbUfHmlITM](img/d8b9a26d69acc1df98e3a51683a99992.png)

Photo by [Justin Luebke](https://unsplash.com/@jluebke?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

*本文基于 [freeCodeCamp](https://www.freecodecamp.org/) 的基础算法脚本“[确认结局](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/confirm-the-ending)”。*

这个挑战包括检查字符串是否以特定的字母序列结尾。

在本文中，我将解释如何使用正则表达式解决这个挑战。

这个解决方案有趣的一面是使用 RegExp 构造函数来创建特定的 RegExp，您需要它来检查作为参数传递的字符串。

#### 算法挑战

> 检查一个字符串(第一个参数，`str`)是否以给定的目标字符串(第二个参数，`target`)结束。

> 这一挑战可以通过 ES2015 中推出的`.endsWith()`方法来解决。但是为了这个挑战的目的，我们希望您使用 JavaScript substring 方法。

#### 提供的测试案例

> `confirmEnding("Bastian", "n")`应该返回 true。

> `confirmEnding("Congratulation", "on")`应该返回 true。

> `confirmEnding("Connor", "n")`应返回 false。

> `confirmEnding("Walking on water and developing software from a specification are easy if both are frozen", "specification")`应返回 false。

> `confirmEnding("He has to give me a new name", "name")`应该返回 true。

> `confirmEnding("Open sesame", "same")`应该返回 true。

> `confirmEnding("Open sesame", "pen")`应返回 false。

> `confirmEnding("Open sesame", "game")`应返回 false。

> `confirmEnding("If you want to save our world, you must hurry. We dont know how much longer we can withstand the nothing", "mountain")`应返回 false。

> `confirmEnding("Abstraction", "action")`应该返回 true。

> 不要使用内置方法`.endsWith()`来解决挑战。

### 1.第一个想法根本行不通

如果像我一样，你是一个 RexExp 爱好者，你的第一次尝试可能是尝试用下面的**代码解决挑战，而它**不会工作**。**

原因是，使用这种语法，test()函数将查找特定的“目标”字符串，而不是将“目标”作为变量作为参数传递。

如果我们回到我们的测试用例，那些应该返回“false”的确实通过了，但是没有一个应该返回“true”的通过了，这是可以预见的。

![0*eIBStwAQ1PDwZkrJ](img/26a2764491c776e8191f1962cbd92592.png)

Photo by [Pablo Lancaster Jones](https://unsplash.com/@fotolancaster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 2.通过使用 RegExp 构造函数创建您需要的特定 RegExp 来解决这个问题

为了使用能够“理解”参数“target”是一个变量而不是字符串“target”的 RegExp，您必须使用 RegExp 构造函数来**创建一个泰勒制作的 RegExp。**

并且，在我们继续之前，让我们回过头来看一下我们想要测试的内容:“target”参数应该是“str”参数的结尾。这意味着**我们的正则表达式应该以“$”字符**结束。

#### **现在，我们可以用三个步骤来解决这个挑战**

**步骤 1**——创建一个变量，在“target”参数的末尾添加“$ ”,在本例中使用 concat()方法。

**第 2 步**——使用 RegExp 构造函数和“new”操作符用上面的变量创建正确的 RexExp。

**第三步**——返回 test()函数的结果。

这就完美地通过了所有的案例测试？

#### **这可以用两行代码来重构**

**注意**:因为没有一个测试用例意味着测试字母的大小写，所以没有必要使用“I”标志。

#### 有用的链接

[MDN 中的 String.prototype.concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/concat)

[MDN 中的 RegExp.prototype.test()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)

[MDN 中的 RegExp 构造函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[正则表达式](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/regular-expressions)在[自由代码营](https://www.freecodecamp.org/)

#### 应对这一挑战的其他解决方案

**挑战"[得到提示](https://guide.freecodecamp.org/certifications/javascript-algorithms-and-data-structures/basic-algorithm-scripting/confirm-the-ending/) "** 使用**切片()方法**建议解决方案。

你可以找到另外两种方法来解决这个挑战，一种是用 substr()方法，另一种是用 T2 endsWith()方法，这篇文章中的索尼娅·莫伊塞(Sonya Moisset)对此做了解释。

这个特别的 RegExp 解决方案还可以**帮助您解决 freeCodeCamp 中级算法脚本“[搜索和替换](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/intermediate-algorithm-scripting/search-and-replace)”挑战**。

**感谢您的阅读！** ✨

如果你喜欢这篇文章，**请“拍手”你喜欢的次数**并分享它**以帮助其他人找到它。那可能会让他们开心一天。**

如果你有**反应/问题/建议**，一定要在下面留下**评论。我将很高兴收到你的来信！**

您也可以在 Twitter 上联系和/或关注 [**我**](https://twitter.com/codingk8) **。**