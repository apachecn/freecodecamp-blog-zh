# JavaScript 标准对象:字符串

> 原文：<https://www.freecodecamp.org/news/javascript-standard-objects-strings/>

你肯定听说过，在 JavaScript 中，一切都是对象。字符串、数字、函数、数组以及对象都被认为是对象。

在本教程中，我们将深入研究**字符串**“全局”或“标准内置”对象，以及与之相关的方法。

## String.prototype.toUpperCase()

JavaScript 方法`String.toUpperCase()`返回它被调用的同一个字符串，但是全部是大写字符。

### 句法

```
str.toUpperCase()
```

### 例子

```
console.log("hello world".toUpperCase()); // "HELLO WORLD"
```

## String.prototype.fromCharCode()

`String.fromCharCode()`方法返回一个使用指定的 Unicode 值序列创建的字符串。

### 句法

```
String.fromCharCode(num1, num2...)
```

### **参数**

表示 Unicode 值的数字序列。

### 例子

```
String.fromCharCode(65, 66, 67);  // "ABC"

var test = String.fromCharCode(112, 108, 97, 105, 110);
document.write(test);

// Output: plain
```