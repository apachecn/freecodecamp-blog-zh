# 三元运算符 JavaScript If 语句教程

> 原文：<https://www.freecodecamp.org/news/ternary-operator-javascript-if-statement-tutorial/>

本教程将帮助您学习如何用一种更简洁的称为三元运算符的速记语法来替换`if/else`语句。

条件运算符——也称为三元运算符——是`if/else`语句的另一种形式，有助于您以更简洁的方式编写条件代码块。

条件运算符的语法如下所示:

```
conditional ? expression_when_true : expression_when_false;
```

conditional operator basic syntax

首先，您需要编写一个评估为`true`或`false`的*条件表达式*。如果表达式返回 true，JavaScript 将执行你写在冒号操作符(`:`)左边的代码，返回 false 时，执行冒号操作符右边的代码。

为了理解它是如何工作的，让我们将其与常规的`if/else`语句进行比较。假设您有一个小程序，它根据您的考试分数分配不同的考试分数:

*   当你的分数高于 80 分时，你的分数是“A”。
*   否则，你指定“B”作为等级。

下面是编写程序的一种方法:

```
let score = 85;
let grade;
if(score >= 80){
    grade = "A";
} else {
    grade = "B";
}

console.log(`Your exam grade is ${grade}`);
```

A regular if/else statement

或者，您可以使用三元运算符编写上述代码，如下所示:

```
let score = 85;
let grade = score >= 80 ? "A" : "B";

console.log(`Your exam grade is ${grade}`);
```

A ternary operator replacing if/else statement

给你。三元运算符简写看起来比常规的`if/else`语句更简洁。

但是如果您的代码需要多个`if/else`语句呢？如果在评价中加上“C”和“D”的成绩会怎么样？

```
let score = 85;
let grade;
if(score >= 80){
    grade = "A";
} else if (score >= 70) {
    grade = "B";
} else if (score >= 60) {
    grade = "C";
} else {
    grade = "D";
}

console.log(`Your exam grade is ${grade}`);
```

Multiple if/else statements in a program

别担心！您可以编写多个三元运算符来替换上面的代码，如下所示:

```
let score = 85;
let grade = score >= 80 ? "A" 
  : score >= 70 ? "B" 
  : score >= 60 ? "C" 
  : "D";

console.log(`Your exam grade is ${grade}`);
```

Multiple ternary operators in action

但是，不建议用多个三元运算符替换多个`if/else`语句，因为这会使代码在将来更难阅读。对于这种情况，最好坚持使用`if/else`或`switch`声明。

## 感谢阅读本教程

我希望它能帮助你理解三元运算符是如何工作的。如果你喜欢这个教程，你可能想看看我在 sebhastian.com 的网站，了解更多的 JavaScript 好东西。

此外，我还有一份关于 web 开发教程的免费每周简讯(大多与 JavaScript 相关)。