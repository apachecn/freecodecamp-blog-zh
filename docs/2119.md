# JavaScript Switch 语句–带有 JS Switch 案例示例代码

> 原文：<https://www.freecodecamp.org/news/javascript-switch-statement-with-js-switch-case-example-code/>

创建条件来决定执行什么动作是 JavaScript 编程中最基本的部分之一。本教程将帮助你学习如何使用关键字`switch`创建多个条件。

## JavaScript 中 switch 语句的工作方式

JavaScript `switch`关键字用于创建多个条件语句，允许您基于不同的条件执行不同的代码块。

下面的代码向您展示了一个正在运行的`switch`语句:

```
var score = 20;

switch(age){
    case 10:
        console.log("Score value is 10");
        break;
    case 20:
        console.log("Score value is 20");
        break;
    default:
        console.log("Score value is neither 10 or 20");
}
```

A simple switch statement in action

上面的代码将把`"Score value is 20"`打印到控制台。switch 语句的工作原理是将赋予它的一个`expression`与每个`case`子句中的表达式进行比较。

首先，您需要向`switch`语句中传递一个`expression`，然后用一对圆括号`()`将它括起来。您可以传递变量或文字值，如下所示:

```
var age = 29;

switch(age){}
// or
switch(true){}
switch("A string"){}
switch(5+5){}
```

A switch statement accepting different expressions

从上到下，`expression`将被计算一次，然后与您在每个`case`子句中定义的表达式进行比较。

在下面的例子中，`switch`语句将评估变量`flower`的值，然后将它与每个`case`子句进行比较，看它是否返回`true`:

*   第一个`case`将比较是否`flower === "rose"`
*   第二个`case`将比较是否`flower === "violet"`
*   第三个`case`将比较是否`flower === "sunflower"`
*   当所有三个`case`子句都返回`false`时，`default`案例将被执行

```
var flower = "tulip";

switch (flower){
    case "rose":
        console.log("Roses are red");
        break;
    case "violet":
        console.log("Violets are blue");
        break;
    case "sunflower":
        console.log("Sunflowers are yellow");
        break;
    default:
        console.log("Please select another flower");
}
```

Another switch statement example with flowers

`default`的情况是可选的，这意味着您可以简单地运行`switch`语句，而不生成任何输出。但是最好包含一个`default`案例，这样您就知道 JavaScript 正确执行了`switch`语句。

在一个`switch`语句中只能包含一个`default` case，否则 JavaScript 会抛出错误。

最后，您需要在每个`case`子句的正文中包含`break`关键字，以便一旦找到匹配的事例，就停止`switch`语句的执行。如果省略`break`关键字，JavaScript 将继续计算表达式，直到最后一个`case`子句。

以下代码将打印出`"Roses are red"`和`"Please select another flower"`，因为`case`子句中省略了`break`关键字，导致 JavaScript 继续进行表达式比较，直到最后一种情况，即`default`的情况:

```
var flower = "rose";

switch (flower){
    case "rose":
        console.log("Roses are red");
    case "violet":
        console.log("Violets are blue");
    case "sunflower":
        console.log("Sunflowers are yellow");
    default:
        console.log("Please select another flower");
}
```

The switch statement without break keyword

即使表达式`"rose"`已经在第一个`case`子句中找到匹配，JavaScript 仍然继续运行`switch`语句，因为没有`break`关键字。

*注意:在最后一种情况下不需要`break`关键字，因为到那时`switch`语句将被完全执行。*

总结一下，`switch`语句是如何工作的:

*   首先，你需要一个`expression`来和一些条件句进行比较。
*   然后，您编写所有条件句来与每个`case`子句中的`expression`进行比较，包括没有匹配的`case`时的`default`情况
*   最后，编写希望在每个`case`中执行的代码，后跟`break`关键字，以阻止 JavaScript 进一步比较`expression`和`case`子句。

现在你已经知道了`switch`语句是如何工作的，让我们来学习什么时候应该使用`switch`语句而不是`if..else`语句。

## 何时使用 switch 语句

`switch`语句和`if..else`语句都用于创建条件。经验法则是，`switch`语句只在条件句有**精确值**时使用。

这是因为一个`if..else`语句可以用来比较一个`expression`和一个**不精确值**，比如大于或小于:

```
var score = 70;

if(score > 50){
  console.log("Score is higher than 50");
} else {
  console.log("Score is 50 or lower");
}
```

A simple if..else statement example

但是你不能用`score > 50`作为一个`case`子句的条件。下面的例子将打印出`default`案例，即使`score > 50`:

```
var score = 70;

switch(score){
    case score > 50:
        console.log("Score is higher than 50");
        break;
    default:
        console.log("Score is 50 or lower");
}
```

A wrong case clause with imprecise value

如果您想使用`switch`语句评估一个不精确的值，您需要通过评估一个`true`表达式来创建一个变通方法，如下面的代码所示:

```
var score = 70;

switch(true){
    case score > 50:
        console.log("Score is higher than 50");
        break;
    default:
        console.log("Score is 50 or lower");
}
```

Evaluating imprecise value with switch statement

虽然上面的代码可以工作，但是最好使用一个`if..else`语句，因为它可读性更好。

## **************感谢阅读本教程**************

你可能也对我写的其他 JavaScript 教程感兴趣，包括[寻找 JavaScript 字符串长度](https://sebhastian.com/javascript-string-length/)和 [如何大写字符串的第一个字母](https://sebhastian.com/javascript-capitalize-first-letter/)。这些是人们日常遇到的最常见的 JavaScript 问题。

我还有一个关于 web 开发教程的[免费简讯](https://sebhastian.com/newsletter/)(大部分是 JavaScript 相关的)。