# JavaScript 数组长度解释

> 原文：<https://www.freecodecamp.org/news/javascript-array-length/>

`length`是 JavaScript 中数组的属性，返回或设置给定数组中元素的数量。

数组的`length`属性可以这样返回。

```
let desserts = ["Cake", "Pie", "Brownies"];
console.log(desserts.length); // 3
```

赋值操作符，结合`length`属性，可以用来设置数组中元素的数量，如下所示。

```
let cars = ["Saab", "BMW", "Volvo"];
cars.length = 2;
console.log(cars.length); // 2
```

## 关于阵列的更多信息:

### isArray()方法

如果一个对象是数组，`Array.isArray()`方法返回`true`，否则返回`false`。

**语法:**

```
Array.isArray(obj)
```

**参数:**

****obj**** 被检查的对象。

[MDN 链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) | [MSDN 链接](https://msdn.microsoft.com/en-us/LIBRary/ff848265%28v=vs.94%29.aspx)

**例子:**

```
// all following calls return true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// Little known fact: Array.prototype itself is an array:
Array.isArray(Array.prototype); 

// all following calls return false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ __proto__: Array.prototype });
```

### Array.prototype.forEach

“forEach”数组方法用于循环访问数组中的每一项。在 array 对象上调用该方法，并向其传递一个在数组中的每一项上调用的函数。

```
var arr = [1, 2, 3, 4, 5];

arr.forEach(number => console.log(number * 2));

// 2
// 4
// 6
// 8
// 10
```

回调函数还可以接受索引的第二个参数，以防需要引用数组中当前项的索引。

```
var arr = [1, 2, 3, 4, 5];

arr.forEach((number, i) => console.log(`${number} is at index ${i}`));

// '1 is at index 0'
// '2 is at index 1'
// '3 is at index 2'
// '4 is at index 3'
// '5 is at index 4'
```

## 关于阵列的进一步阅读:

[数组.原型.过滤器](https://guide.freecodecamp.org/javascript/standard-objects/array/array-prototype-filter/)

[array.prototype.reduce](https://guide.freecodecamp.org/javascript/standard-objects/array/array-prototype-reduce/)