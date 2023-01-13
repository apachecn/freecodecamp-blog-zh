# 如何在 JavaScript 中将值转换为布尔值

> 原文：<https://www.freecodecamp.org/news/how-to-convert-value-to-boolean-javascript/>

布尔值是一个代表真或假的原始值。在布尔上下文中，JavaScript 利用[类型转换](https://developer.mozilla.org/en-US/docs/Glossary/Type_Conversion)将值转换为真/假。有隐式和显式的方法将值转换成它们的布尔对应值。

本文概述了 truthy 和 falsy 值以及如何在 JavaScript 中将值转换为布尔值。

### JavaScript Truthy 和 Falsy 值清单

```
Boolean(false);         // false
Boolean(undefined);     // false
Boolean(null);          // false
Boolean('');            // false
Boolean(NaN);           // false
Boolean(0);             // false
Boolean(-0);            // false
Boolean(0n);            // false

Boolean(true);          // true
Boolean('hi');          // true
Boolean(1);             // true
Boolean([]);            // true
Boolean([0]);           // true
Boolean([1]);           // true
Boolean({});            // true
Boolean({ a: 1 });      // true
```

Source: [What are truthy and falsy values in JavaScript?](https://www.30secondsofcode.org/articles/s/javascript-truthy-falsy-values)

最好首先了解哪些值被 JavaScript 解释为 are 或 falsy。理解[隐性强制](https://betterprogramming.pub/implicit-and-explicit-coercion-in-javascript-b23d0cb1a750)与[显性强制](https://www.bookstack.cn/read/TypesGrammar/spilt.3.ch4.md#Explicitly:%20*%20%E2%80%94%3E%20Boolean)相比也很重要。

隐式强制由 JavaScript 引擎发起并自动发生。显式强制是通过手动转换值来执行的，JavaScript 提供了处理这一点的内置方法。

### `!!`操作员

```
!!value
```

您可能已经熟悉了作为逻辑非运算符的`!`。当连续使用两个(`!!`)时，第一个`!`将值强制转换为布尔值并反转。例如`!true`会导致假。第二个`!`反转之前的反转值，得到真正的布尔值。

这通常是首选方法，因为它具有更好的性能。这种方法的一个潜在缺点是可读性下降，主要是如果其他开发人员不熟悉这个操作符是如何工作的。

```
const value = "truthy string"
!!value // true
```

下面是一个将此分解为多个步骤的示例:

```
const value = "truthy string";

!value; // false
!!value; // true
```

下面是使用`!!`操作符的示例输出列表。

```
// Falsy Values

!!'' // false
!!false // false
!!null // false
!!undefined // false
!!0 // false
!!NaN // false

// Truthy Values

!![] // true
!!"false" // true
!!true // true
!!1 // true
!!{} // true
```

### `Boolean()`功能

```
Boolean(value)
```

`Boolean()`是一个全局函数，它将传递的值转换成布尔值。

你不应该将它与 new 关键字(`new Boolean`)一起使用，因为这会创建一个具有 object 类型的 Boolean 实例。以下是正确使用该功能的示例。

```
const value = "truthy string"
Boolean(value) // true
```

## TL；速度三角形定位法(dead reckoning)

在 JavaScript 中有两种方法可以将值转换为布尔值。

### 1.`!!`

```
!!value 
```

### 2.`Boolean()`

```
Boolean(value) 
```

```
const finalThoughts = "I really enjoyed writing this article. Thanks for reading!"

!!finalThoughts // true
Boolean(finalThoughts) // true 
```