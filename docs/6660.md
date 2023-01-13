# 我们来谈谈 JavaScript 中的分号

> 原文：<https://www.freecodecamp.org/news/lets-talk-about-semicolons-in-javascript-f1fe08ab4e53/>

#### 使用它们，或者不使用它们…

JavaScript 中的分号划分社区。有些人喜欢无论如何都使用它们。其他人喜欢避开它们。

我在 Twitter 上发起了一项民意调查，我发现了许多分号的支持者:

在使用分号多年后，2017 年秋天，我决定尽可能避免使用分号。我设置了[beauty](https://flaviocopes.com/prettier/)来自动从我的代码中删除分号，除非有特殊的代码结构需要它们。

现在我发现避免分号是很自然的，我认为代码看起来更好，读起来更干净。

这都是可能的，因为 JavaScript 并不严格要求分号。当有需要分号的地方，它就在幕后添加。

这叫做**自动分号插入**。

了解支持分号的规则很重要。这将允许您避免编写会在它没有像您预期的那样运行之前产生错误的代码。

### JavaScript 自动插入分号的规则

在解析源代码的过程中，当 JavaScript 解析器发现以下特殊情况时，它会自动添加一个分号:

1.  当下一行以中断当前行的代码开始时(代码可以在多行中产生)
2.  当下一行以`}`开始时，关闭当前块
3.  当到达源代码文件的结尾时
4.  当一行中有一个`return`语句时
5.  当一行中有一个`break`语句时
6.  当一行中有一个`throw`语句时
7.  当一行中有一个`continue`语句时

### 不按您的想法运行的代码示例

基于这些规则，这里有一些例子。

拿着这个:

```
const hey = 'hey'
const you = 'hey'
const heyYou = hey + ' ' + you

['h', 'e', 'y'].forEach((letter) => console.log(letter))
```

您将得到错误`Uncaught TypeError: Cannot read property 'forEach' of undefined`，因为基于规则`1`，JavaScript 试图将代码解释为

```
const hey = 'hey';
const you = 'hey';
const heyYou = hey + ' ' + you['h', 'e', 'y'].forEach((letter) => console.log(letter))
```

这段代码:

```
(1 + 2).toString()
```

印刷品`"3"`。

```
const a = 1
const b = 2
const c = a + b
(a + b).toString()
```

相反，它引发了一个`TypeError: b is not a function`异常，因为 JavaScript 试图将其解释为

```
const a = 1
const b = 2
const c = a + b(a + b).toString()
```

另一个基于规则 4 的例子:

```
(() => {
  return
  {
    color: 'white'
  }
})()
```

您希望这个被立即调用的函数的返回值是一个包含`color`属性的对象，但事实并非如此。取而代之的是`undefined`，因为 JavaScript 在`return`后面插入了一个分号。

相反，您应该将左括号放在`return`之后:

```
(() => {
  return {
    color: 'white'
  }
})()
```

您可能认为这段代码在警告中显示“0 ”:

```
1 + 1
-1 + 1 === 0 ? alert(0) : alert(2)
```

但是它显示的是 2，因为 JavaScript(根据规则 1)将其解释为:

```
1 + 1 -1 + 1 === 0 ? alert(0) : alert(2)
```

### 结论

小心——有些人对分号非常固执己见。老实说，我不在乎。该工具为我们提供了不使用分号的选项，所以如果我们愿意，我们可以避免使用分号。

我不是建议任何一方或另一方。只要根据什么对你有效来做你自己的决定。

不管怎样，我们只需要稍微注意一下，即使大多数时候这些基本场景不会出现在你的代码中。

挑选一些规则:

*   小心使用`return`语句。如果你退货，把它加在退货的同一行上(同样适用于`break`、`throw`、`continue`)
*   不要用圆括号开始一行，因为圆括号可能与前一行连接在一起，形成函数调用或数组元素引用

最终，总是测试你的代码，以确保它做你想要的。

> 我在[flaviocopes.com](https://flaviocopes.com)上每天发布 1 篇免费编程教程，看看吧！

*最初发表于[flaviocopes.com](https://flaviocopes.com/javascript-automatic-semicolon-insertion/)。*