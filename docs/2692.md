# JavaScript toString 示例——如何在 JS 中把数字转换成字符串等等

> 原文：<https://www.freecodecamp.org/news/javascript-tostring-example-convert-number-to-string-in-js/>

有时，您希望在不手动更改值的情况下将一种数据类型转换为另一种数据类型。

例如，您可能希望将数字转换为字符串。JavaScript 有时会隐式地这样做。

比如当你使用双等号运算符(`==`)时，或者当你试图对一个数据类型与运算不兼容的值做一些事情时。这叫做[式强制](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/)。

也就是说，您也可以显式转换数据类型。我将在本文中向您展示如何做到这一点。

字符串数据类型是 JavaScript 中非常常见的数据类型。对于几乎所有其他数据类型，您都需要有一个字符串表示。

正如当你用一个对象代替一个实际的字符串时，你一定见过类似于`"[object Object]"`的东西。

在本文中，我们将学习什么是`toString`方法，以及如何使用该方法将一个数字(和一些其他数据类型)转换成一个字符串。

## `toString`法

顾名思义，该方法用于将数据更改为字符串。数组、数字和布尔值都有这个方法，它以不同的方式转换它们的数据。现在让我们分别来看一下。

### 如何将数字转换成字符串

`toString`方法存在于每个数字文字上。它将数字转换成字符串形式。下面是它的使用方法:

```
const num = 54;
console.log(num.toString())
// "54" 
```

但是还有更多。用于数字的`toString`方法也接受一个`base`参数。此参数允许您将一个数字转换为另一个基数。

返回值是新数字的字符串表示形式。下面是它的使用方法:

```
const num = 54;
const num2 = num.toString(2);
console.log(num2);
// "110110" 
```

`parseInt`是另一个 JavaScript 方法，它将字符串转换成它们各自的数字表示。它是这样工作的:

```
const numInStr = "54";
const str = "Hello";
console.log(parseInt(numInStr));
// 54
console.log(parseInt(str));
// NaN 
```

对于不类似于数字的变量，`parseInt`返回`Nan`，如上所示。

## 如何在 JavaScript 中将数组转换成字符串

数组也有`toString`方法。此方法的返回值是由逗号分隔的数组(和深层嵌套数组)的所有值的串联。下面是它的使用方法:

```
const arr = ["javascript", "toString", [1, "deep1", [3, 4, "array"]]];
console.log(arr.toString());
// "javascript,toString,1,deep1,3,4,array" 
```

## 如何在 JavaScript 中将对象转换成字符串

对象上的`toString`的返回值是——正如您可能经常遇到的那样——`"[object Object]"`。例如:

```
const obj = {name: 'Object'};
const obj2 = {type: 'data', number: 100};
console.log(obj.toString());
// [object Object]
console.log(obj2.toString());
// [object Object] 
```

对象到字符串的默认转换是`[object Object]`。注意这里有两个`object`而不是一个？而另一个是大写的？

对象有更多的表示，如下所示:

```
function print() {};
const arr = [];
const obj = {};
console.log(
  Object.prototype.toString.call(print),
  Object.prototype.toString.call(arr),
  Object.prototype.toString.call(obj)
)
// [object Function] [object Array] [object Object] 
```

函数、数组、对象，甚至日期和正则表达式都是对象。而且他们每个人都有`toString`方法。

当对它们调用`toString`时，它获取值是什么类的对象，然后像上面看到的那样打印出来(“函数，数组，对象”)。

我们使用`call(variable)`，因为`toString`通过`this`属性获得对象类。

## 结论

`.toString`方法返回它所使用的数据的字符串转换。这对于某些情况非常有用，尤其是`number` s。

在本文中，我们学习了 JavaScript `toString`方法如何与`number`、`array`和`object`一起工作，我们还看了一下`parseInt`。