# 介绍 ECMAScript 2016 (ES7)添加到 JavaScript 的新功能

> 原文：<https://www.freecodecamp.org/news/ecmascript-2016-es7-features-86903c5cab70/>

作者:Sanket Meghani

# 介绍 ECMAScript 2016 (ES7)添加到 JavaScript 的新功能

![53wZyWg7per6tVs1mxLSusncVfmkRN0EXW1g](img/5c8f7d4fac3804a1f5c53e8767dbc8a0.png)

自 ECMAScript 2015(也称为 ES6)发布以来，它引入了大量新功能。它们包括箭头函数、集合、映射、类和析构等等。在许多方面，ES2015 几乎就像学习新版本的 JavaScript。

Ecma 技术委员会 39 管理 ECMA 规范。他们决定从 2015 年开始每年发布一个新版本的 ECMAScript。一年一次的更新意味着不会再有像 ES6 这样的大版本。

ECMAScript 2016 仅引入了两项新功能:

*   Array.prototype.includes()
*   指数运算符

### Array.prototype.includes()

`Array.prototype.includes()`检查作为`argument`传递的`value`的数组。如果数组中包含`value`，则返回`true`，否则返回`false`。

之前，我们需要使用`Array.prototype.indexOf()`来检查给定的数组是否包含元素。

```
let numbers = [1, 2, 3, 4];
```

```
if(numbers.indexOf(2) !== -1) {  console.log('Array contains value');}
```

借助 ECMA2016，我们可以编写:

```
if(numbers.includes(2)) {  console.log('Array contains value');}
```

`Array.prototype.includes()`处理`NaN`比`Array.prototype.indexOf()`好。如果数组包含`NaN`，那么`indexOf()`在搜索`NaN`时不会返回正确的索引。

`Array.prototype.includes()`在搜索`NaN`时返回正确的值。

`NaN`是 JavaScript 全局对象的一个属性，代表一个非数字值。当[将`NaN`与另一个值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)进行比较时，存在已知的怪癖。这些在`Array.prototype.includes()`中有所涉及，但在`Array.protoype.indexOf`中没有。

```
let numbers = [1, 2, 3, 4, NaN];
```

```
console.log(numbers.indexOf(NaN)); //Prints -1console.log(numbers.includes(NaN)); //Prints true
```

### 指数运算符

JavaScript 已经支持许多算术运算符，比如`+, -, *`等等。

ECMAScript 2016 引入了求幂运算符，`**`。

和`Math.pow()`的目的一样。它返回第一个参数的第二次幂。

```
let base = 3;let exponent = 4;let result = base**exponent;
```

```
console.log(result); //81
```

### 结论

ECMA2016 引入的新功能为现有功能提供了便捷的替代方案。

展望未来，ECMA2017 已于今年 6 月敲定。新特性包括`async/await`、`SharedArrayBuffer`和一些对`Object.prototype`有用的方法。