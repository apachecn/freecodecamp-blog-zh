# JavaScript Nullable——如何检查 JS 中的空值

> 原文：<https://www.freecodecamp.org/news/javascript-nullable-how-to-check-for-null-in-js/>

有时候你得检查一下，以确保没有什么不是真的...没什么。😲❗❓

在 JavaScript 中， **null** 是有意包含 null 值的原始类型。 **Undefined** 是一个原始类型，代表一个你声明但没有初始化值的变量。

因此，null 什么都不是，undefined 只是缺少了一些东西。🤣

![nothing](img/156201f08cb7e6120238e849f94c1505.png)

null is nothing; undefined is not something

我知道没什么帮助。让我们潜入更深的地方。

## 如何定义空值和未定义的值

举个例子会有帮助。下面我们声明两个变量。让我们保持简单，使用`null`和`undefined`来比较结果，因为它们有时会因为相似性而被混淆。

```
let leviticus = null;
// leviticus is null

let dune;
// dune is undefined
```

Declaring two variables: one null and one undefined.

`leviticus`是*故意*不带对象值( **null** )。虽然`dune`被声明，但是*无意中*丢失了一个值(**未定义**)。

## 如何用`typeof()`检查空值

您可以在 JavaScript 中用`typeof()`操作符检查 null。

```
console.log(typeof(leviticus))
// object

console.log(typeof(dune))
// undefined
```

typeof() will return 'object' when called on a null variable

奇怪的是，如果用`typeof()`检查，一个空变量将返回`object`。这是因为 JavaScript 中的一个[历史错误](https://www.turbinelabs.com/blog/the-odd-history-of-javascripts-null)。

## 如何用相等运算符检查空值

另一个奇怪的地方是，当你使用 [double equals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) `==`，`null`和`undefined`松散地检查相等性时，将返回`true`。

```
console.log(leviticus == dune)
// true

console.log(leviticus === dune)
// false

console.log(leviticus == null)
// true (but not as good a habit to use as strict equality shown in next example)
```

但是当你使用[三重等于](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) `===`严格检查相等时，null 和 undefined 会返回`false`。

这是因为在 JavaScript 中 null 和 undefined 都是错误的 T4。Falsy 意味着当在布尔(`true`或`false`)上下文中遇到一个值时，该值被认为是`false`。

JavaScript 使用强制将值从一种类型强制转换为另一种类型，以便能够在布尔上下文中使用它们。

但是通过严格检查是否相等，可以看出它们实际上并不相等。

## 如何用严格相等来检查空值

检查 null 的最佳方法是使用严格和显式的等式:

```
console.log(leviticus === null)
// true

console.log(dune === null)
// false
```

## 如何用`Object.is()`方法检查空值

检查 null 的一个同样简单的方法是使用内置的`Object.is()`方法:

```
console.log(Object.is(leviticus, null)
// true

console.log(Object.is(dune, null)
// false
```

## 摘要

*   `null`是一个变量的原始类型，它对 falsy 求值，有一个`typeof()`对象，通常有意声明为`null`
*   `undefined`是一个变量的原始类型，它对 falsy 求值，有一个未定义的`typeof()`，表示一个已声明但缺少初始值的变量。
*   `null == undefined`评估为真，因为它们*与*大致相等。
*   `null === undefined`评估为假，因为它们不，*实际上*相等。
*   `<null_variable> === null`是**严格检查 null 的最好方法**。
*   `Object.is(<null_variable>,null)`是一种**同样可靠的方式**来检查空值。

鼓起勇气！正如您可能已经收集到的，在 JavaScript 生态系统中有太多像这样的脑筋急转弯。但是当你把它分解开来，你就能自信地理解它们/

![denzel](img/ca1b6ed942d5e84620a0ea83ec1f0969.png)

You can do it!

## 感谢阅读！

我希望这是一个对你有帮助的分解。继续编码，继续向前倾斜！

在推特上向我问好:[https://twitter.com/EamonnCottrell](https://twitter.com/EamonnCottrell)

祝你过得愉快👋。