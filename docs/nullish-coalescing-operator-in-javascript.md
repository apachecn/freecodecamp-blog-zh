# JavaScript 中的 Nullish 合并运算符是如何工作的

> 原文：<https://www.freecodecamp.org/news/nullish-coalescing-operator-in-javascript/>

ES11 增加了一个 nullish 合并运算符，用两个问号表示，就像这样:`??`。

在这篇文章中，我们将探索为什么它如此有用，以及如何使用它。

让我们开始吧。

## 背景资料

在 JavaScript 中，有一个短路逻辑 OR 运算符`||`。

> ||运算符返回第一个`truthy`值。

以下是 JavaScript 中被认为是`falsy`值的`only six`值。

*   错误的
*   不明确的
*   空
*   " "(空字符串)
*   圆盘烤饼
*   Zero

所以如果有什么东西不在上面的列表中，那么它将被认为是一个`truthy`值。

`Truthy`和`Falsy`值是在执行某些操作时被强制为`true`
或`false`的非布尔值。

```
const value1 = 1;
const value2 = 23;

const result = value1 || value2; 

console.log(result); // 1
```

由于||运算符返回第一个`truthy`值，在上面的代码中，`result`将是存储在`value1`中的值，即`1`。

如果`value1`是`null`、`undefined`、`empty`或任何其他`falsy`值，那么||运算符之后的下一个操作数将被求值，这将是 total 表达式的结果。

```
const value1 = 0;
const value2 = 23;
const value3 = "Hello";

const result = value1 || value2 || value3; 

console.log(result); // 23
```

这里，因为`value1`为 0，所以`value2`会被检查。因为它是真值，所以整个表达式的结果将是`value2`。

> ||操作符的问题在于它没有区分`false`、`0`、空字符串`""`、`NaN`、`null`和`undefined`。它们都被认为是`falsy`值。

如果其中任何一个是||，那么我们将得到第二个操作数作为结果。

## 为什么 javascript 需要看涨合并运算符

||操作符非常好用，但是有时我们只希望在第一个操作数只有`null`或`undefined`时计算下一个表达式。

因此，ES11 增加了 nullish 合并运算符。

在表达式`x ?? y`中，

*   如果 x 是`null`或`undefined`或**，那么只有**结果将是`y`。
*   如果 x 是**而不是** `null`或`undefined`，那么结果将是`x`。

这将使条件检查和调试代码变得容易。

## 你自己试试

```
let result = undefined ?? "Hello";
console.log(result); // Hello

result = null ?? true; 
console.log(result); // true

result = false ?? true;
console.log(result); // false

result = 45 ?? true; 
console.log(result); // 45

result = "" ?? true; 
console.log(result); // ""

result = NaN ?? true; 
console.log(result); // NaN

result = 4 > 5 ?? true; 
console.log(result); // false because 4 > 5 evaluates to false

result = 4 < 5 ?? true;
console.log(result); // true because 4 < 5 evaluates to true

result = [1, 2, 3] ?? true;
console.log(result); // [1, 2, 3] 
```

所以从上面所有的例子中，很明显，只有当`x`是`undefined`或`null`时，运算结果`x ?? y`才是`y`。

在所有其他情况下，操作的结果将总是`x`。

## 结论

如您所见，当您只关心任何变量的`null`或`undefined`值时，nullish 合并操作符非常有用。

从 ES6 开始，JavaScript 增加了许多有用的东西，比如

*   ES6 破坏
*   导入和导出语法
*   箭头功能
*   承诺
*   异步/等待
*   可选链接运算符

还有很多。

你可以在[掌握现代 JavaScript](https://modernjavascript.yogeshchavan.dev/) 这本书里详细了解所有 ES6+特性的一切。

你可以以 40%的折扣获得《掌握现代 JavaScript》这本书。

**订阅我的[每周简讯](https://yogeshchavan.dev/)，加入 1000 多名其他订阅者的行列，直接在收件箱中获得惊人的提示、技巧、文章和折扣。**