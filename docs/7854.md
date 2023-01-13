# JavaScript 要点:如何用 if/else 语句做出人生决策

> 原文：<https://www.freecodecamp.org/news/javascript-essentials-how-to-make-life-decisions-with-if-else-statements-1908ff7cf5da/>

假设你走在市中心一条繁忙的街道上。你正要过马路，这时你注意到行人交通灯变成了红色。你是做什么的？

你停下来了，是吗？

当灯再次变绿时会发生什么？你开始走路。

我们也可以把这个类比放到代码中。听起来像是:“如果灯变红，停止行走。否则，继续走”。

我的朋友，这是一个声明的基础。

### if/else 语句

`if/else`语句有助于控制你的程序在特定情况下做什么。看起来是这样的:

```
if (condition) {     // Do something } else {     // Do some other thing }
```

`condition`告诉 JavaScript 在继续之前要检查什么。如果条件评估为`true`，JavaScript 执行`if`块中的代码。

如果条件评估为`false`，JavaScript 执行来自`else`块的代码。

在交通灯的例子中，我们检查灯是否是红色的:

```
// Note: This example doesn't contain valid code yet if (light is red) {  stop walking } else {  continue walking }
```

如果需要检查多个条件，可以使用`else if`，它位于`if`和`else`之间。

你什么时候需要这样的第二个条件？

好吧，假设你想穿过一条小路。如果周围没有车，你会等交通灯变绿吗？你还在穿越，不是吗？

在代码中，这将类似于:

```
if (light is red) {   // Stop walking } else if (cars around) {   // Stop walking } else if (yet another condition) {   // Do yet another thing } else {   // Do the final thing }
```

在这种情况下，如果第一个条件评估为`true`，JavaScript 执行`if`块中的代码。

如果第一个条件评估为`false`，JavaScript 检查下一个`else if`块中的条件，看它是否评估为`true`。它会一直持续下去，直到所有的`else if`区块都用完为止。

为了检查条件的计算结果是`true`还是`false`，JavaScript 依赖于两件事情:

1.  比较运算符
2.  真值和假值

先说比较运算符。

### 比较运算符

有四种主要类型的比较运算符:

1.  大于(`&`gt；)或者大于等于 t `o` ( > =)
2.  小于(`&`lt；)或者小于等于 t `o` ( < =)
3.  严格相等(`===`)或相等`==`
4.  严格不相等(`!==`)或不相等`!=`

前两种类型的比较运算符非常简单。你用它们来比较数字。

```
24 > 23 // True 24 > 24 // False 24 >= 24 // True 
```

```
24 < 25 // True 24 < 24 // False 24 <= 24 // True
```

接下来的两种比较运算符也非常简单。你用它们来检查事物之间是相等还是不相等。

```
24 === 24 // True 24 !== 24 // False
```

然而，严格相等(`===`)与相等(`==`)和严格不相等(`!==`)与不相等(【T3))是有区别的:

```
'24' === 24 // False '24' == 24 // True 
```

```
'24' !== 24 // True '24' != 24 // False
```

从上面的例子中可以看出，当您将一个字符串`24`与数字 24 进行比较时，`===`的计算结果为`false`，而`==`的计算结果为 true。

为什么会这样呢？我们来看看严格相等和相等的区别。

### === vs ==(或者！== vs！=)

JavaScript 是一种松散类型的语言。这意味着，当我们声明变量时，我们并不关心变量是什么类型的值。

您可以声明任何原语或对象，JavaScript 会自动为您完成剩下的工作:

```
const aString = 'Some string' const aNumber = 123 const aBoolean = true
```

当比较严格相等(`===`)或严格不相等(`!==`)的事物时，JavaScript 检查变量的类型。这就是为什么`24`的一根**弦**和一个**号** `24`不等同的原因。

```
'24' === 24 // False '24' !== 24 // True
```

当比较相等(`==`)或不相等(`!=`)的事物时，JavaScript 会转换(或强制转换)类型，使它们彼此匹配。

通常，当您使用转换运算符时，JavaScript 会尝试将所有类型转换为数字。在下面的例子中，在比较之前，**字符串** `24`被转换为**数字** 24。

这就是为什么当你使用`==`时，一串`24`等于 24。

```
'24' == 24 // True '24' != 24 // False
```

布尔值也可以转换成数字。当 JavaScript 将布尔值转换成数字时，`true`变成 1，`false`变成 0。

```
0 == false // True 1 == true // True 2 == true // False
```

自动类型转换(使用比较运算符时)是难以发现的错误的常见原因之一。**每当你比较相等时，总是使用严格版本** ( `===`或`!==`)。

### 比较对象和数组

尝试用`===`或`==`比较对象和数组。你会非常惊讶的。

```
const a = { isHavingFun: true } const b = { isHavingFun: true } 
```

```
console.log(a === b) // false console.log(a == b) // false
```

在上面的例子中，`a`和`b` **看起来完全一样。它们都是对象，具有相同的值。**

奇怪的是，`a === b`永远都是假的。为什么？

假设你有一个同卵双胞胎兄弟/姐妹。你和你的双胞胎长得一模一样。一样的发色，一样的脸，一样的衣服，一样的一切。人们如何区分你们两个？会很难。

在 JavaScript 中，每个对象都有一张“身份证”。这个身份证被称为**参考**对象。当您使用相等运算符比较对象时，您是在要求 JavaScript 检查这两个对象是否具有相同的引用(相同的身份证)。

现在`a === b`总是要假，是不是很意外？

让我们稍微调整一下，将`a`分配给`b`。

```
const a = { isHavingFun: true } const b = a
```

在这种情况下，`a === b`评估为真，因为`b`现在指向与`a`相同的引用。

```
console.log(a === b) // true
```

### 真理和谬误

如果您编写一个单独的变量(如下例中的`hasApples`)作为一个`if/else`语句的条件，JavaScript 会检查 true 或 falsy 值。

```
const hasApples = 'true' 
```

```
if (hasApples) {   // Eat apple } else {   // Buy apples }
```

一个**false**值是一个在转换为布尔值时计算结果为`false`的值。JavaScript 中有六种可能的错误值:

1.  `false`
2.  `undefined`
3.  `null`
4.  `0`(数字零)
5.  `""`(空字符串)
6.  `NaN`(不是数字)

另一方面，**真值**是在转换为布尔值时计算结果为`true`的值。在数字的例子中，任何不是 T1 的东西都会转换成 T2。

在 JavaScript 中，非常鼓励将类型自动转换为 truthy 和 falsy 值，因为它们使代码更短，更容易理解。

例如，如果您想检查一个字符串是否为空，您可以直接在条件中使用该字符串。

```
const str = '' 
```

```
if (str) {   // Do something if string is not empty } else {   // Do something if string is empty }
```

### 包扎

语句用来控制你的程序在特定情况下做什么。它让你决定是否步行或过马路，这取决于给你的条件。

为了检查条件是真还是假，Javascript 依赖于两件事:

1.  比较运算符
2.  真/假价值观

如果你喜欢这篇文章，你会喜欢 Learn**Learn JavaScript**——这是一门帮助你学习用 JavaScript 从零开始**构建真正的组件**的课程。[如果你感兴趣，点击这里了解更多关于学习 JavaScript 的信息](https://learnjavascript.today/)。

(哦，对了，如果你喜欢这篇文章，如果你能[分享一下](http://twitter.com/share?text=What%20determines%20if%20a%20condition%20is%20true%20or%20false%3F%0A%0A1.%20comparison%20operators.%0A2.%20Truthy%2Ffalsy%20values%0A%0ABy%20%40zellwk%20?%20&url=https://zellwk.com/blog/js-if-else/&hashtags=)，我会很感激。？)

*最初发表于[zellwk.com](https://zellwk.com/blog/js-if-else/)。*