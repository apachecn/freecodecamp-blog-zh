# JS 检查空值——解释了 JavaScript 中的空值检查

> 原文：<https://www.freecodecamp.org/news/how-to-check-for-null-in-javascript/>

Null 是 JavaScript 中的一种基本类型。这意味着你应该能够用`typeof()`方法检查一个变量是否为`null`。但不幸的是，由于一个无法修复的[历史错误](https://www.turbinelabs.com/blog/the-odd-history-of-javascripts-null)，它返回了“object”。

```
let userName = null;

console.log(typeof(userName)); // object 
```

那么现在如何检查 null 呢？本文将教您如何检查 null，以及 JavaScript 类型 null 和 undefined 之间的区别。

## JavaScript 中的空与未定义

Null 和 undefined 在 JavaScript 中非常相似，都是原语类型。

如果一个变量有意包含了`null`的值，那么它的类型就是`null`。相反，当你声明一个变量而没有初始化一个值时，它的类型是`undefined`。

```
// This is null
let firstName = null;

// This is undefined
let lastName; 
```

Undefined 工作得很好，因为当您使用`typeof()`方法检查类型时，它将返回`undefined`:

```
let lastName;

console.log(typeof(lastName)); // undefined 
```

现在让我们看看检查`null`的两种主要方法，以及它与`undefined`的关系。

## 如何用等式运算符检查 JavaScript 中的空值

相等运算符提供了检查`null`的最佳方式。您可以使用宽松/双重相等运算符(`==`)或严格/三重相等运算符(`===`)。

### 如何使用宽松的等式运算符检查 null

您可以使用宽松的等式运算符来检查`null`值:

```
let firstName = null;

console.log(firstName == null); // true 
```

但是，这可能很棘手，因为如果变量未定义，它也将返回`true`，因为`null`和`undefined`大致相等。

```
let firstName = null;
let lastName;

console.log(firstName == null); // true
console.log(lastName == null); // true
console.log(firstName == undefined); // true
console.log(lastName == undefined); // true
console.log(firstName == lastName); // true
console.log(null == undefined); // true 
```

**注意:**当你想检查一个变量是否没有值时，这可能是有用的，因为当一个变量没有值时，它可能是`null`或`undefined`。

但是假设您只想检查`null`——那么您可以使用严格的等式操作符。

### 如何使用严格相等运算符检查 null

与宽松的相等操作符相比，严格的相等操作符将只在您正好有一个`null`值时返回`true`。否则，它将返回`false`(这包括`undefined`)。

```
let firstName = null;
let lastName;

console.log(firstName === null); // true
console.log(lastName === null); // false
console.log(firstName === undefined); // false
console.log(lastName === undefined); // true
console.log(firstName === lastName); // false
console.log(null === undefined); // false 
```

如你所见，只有当一个空变量与`null`比较，一个未定义变量与`undefined`比较时，它才返回`true`。

## 如何用`Object.is()`方法检查 JavaScript 中的空值

`Object.is()`是确定两个值是否相同的 ES6 方法。这类似于严格的相等运算符。

```
// Syntax
Object.is(value1, value2) 
```

让我们利用前面的例子来看看它是否像严格的等式运算符那样工作:

```
let firstName = null;
let lastName;

console.log(Object.is(firstName, null)); // true
console.log(Object.is(lastName, null)); // false
console.log(Object.is(firstName, undefined)); // false
console.log(Object.is(lastName, undefined)); // true
console.log(Object.is(firstName, lastName)); // false
console.log(Object.is(null, undefined)); // false 
```

这是因为只有当两个值相同时，它才返回`true`。这意味着只有当一个设置为`null`的变量与`null`进行比较，一个未定义的变量与`undefined`进行比较时，它才会返回`true`。

## 结论

现在您知道如何放心地检查 null 了。你也可以检查一个变量是否被设置为`null`或者`undefined`，你知道宽松的和严格的相等操作符之间的区别。

我希望这有所帮助。祝编码愉快！