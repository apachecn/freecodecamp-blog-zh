# JavaScript 数组方法的终极指南——Map

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-array-methods-map/>

`map()`方法将一个函数应用于数组中的每个元素，并返回一个包含修改值(如果有的话)的原始数组的副本。

## 语法:

```
const newArr = oldArr.map(function(currentValue, index, array) {
  // Do stuff with currentValue (index and array are optional)
});
```

*   `newArr` -返回的新数组
*   `oldArr` -正在操作的旧数组。该数组不会被更改
*   `currentValue` -正在处理的当前值
*   `index` -正在处理的值的当前索引
*   `array` -原始阵列

## 示例:

### 是五个

```
var arr = [1, 2, 3, 4];

var newArray = arr.map(function(element) {
  return element * 2
});

console.log(arr); // [1, 2, 3, 4]
console.log(newArray); // [2, 4, 6, 8]
```

### 是六个

```
const arr = [1, 2, 3, 4];

const newArray = arr.map(element => {
  return element * 2;
});

const newArrayOneLiner = arr.map(element => element * 2);

console.log(arr); // [1, 2, 3, 4]
console.log(newArray); // [2, 4, 6, 8]
console.log(newArrayOneLiner); // [2, 4, 6, 8]
```

## `map`对`forEach`

从表面上看，`map()`和`forEach()`方法非常相似。这两种方法都遍历一个数组，并对每个元素应用一个函数。主要区别是`map()`返回一个新数组，而`forEach()`不返回任何东西。

那么应该用哪种方法呢？一般情况下，如果不需要改变原数组中的值，最好使用`forEach()`。如果您需要做的只是将数组的每个元素记录到控制台，或者将它们保存到数据库，那么`forEach()`是一个不错的选择:

```
const letters = ['a', 'b', 'c', 'd'];

letters.forEach(letter => {
  console.log(letter);
});
```

如果需要更新原始数组中的值，那么`map()`是更好的选择。如果您想将更新后的数组存储为一个变量并保留原始数组作为引用，这将非常有用。

## 如何将`map`与其他数组方法一起使用

由于`map()`返回一个数组，您可以将它与其他数组方法一起使用，使您的代码更加简洁易读。

### 使用`map`和`filter`

使用`map()`时要记住的一点是，它对原始数组的每个元素都应用一个函数到*，并返回一个与旧数组长度相同的新数组。换句话说，不可能跳过不想修改的数组元素:*

```
const nums = [5, 10, 15, 20];
const doublesOverTen = nums.map(num => {
  if (num > 10) {
    return num * 2;
  }
});

console.log(doublesOverTen); // [undefined, undefined, 30, 40]
```

这就是`filter()`方法发挥作用的地方。`filter()`返回满足特定条件的过滤元素的新数组，然后您可以将`map()`链接到:

```
const nums = [5, 10, 15, 20];
const doublesOverTen = nums.filter(num => {
  return num > 10;
}).map(num => {
  return num * 2;
});

console.log(doublesOverTen); // [30, 40]
```

这段代码可以进一步简化:

```
const nums = [5, 10, 15, 20];
const doublesOverTen = nums.filter(num => num > 10).map(num => num * 2);

console.log(doublesOverTen); // [30, 40]
```

### 使用`map`和`reverse`

在映射数组时，有时可能需要反转数组。`reverse()`方法使这变得容易，但是重要的是要记住，虽然`map()`是不可变的，但是`reverse()`不是。换句话说，`reverse()`方法将改变原始数组:

```
const nums = [1, 2, 3, 4, 5];
const reversedDoubles = nums.reverse().map(num => num * 2);

console.log(nums); // [5, 4, 3, 2, 1]
console.log(reversedDoubles); // [10, 8, 6, 4, 2]
```

`map()`的一个主要优点是它不会改变原始数组，这样使用`reverse()`就违背了初衷。然而，这是一个简单的修复——只要记住首先使用`map()`,然后使用`reverse()`它返回的新数组:

```
const nums = [1, 2, 3, 4, 5];
const reversedDoubles = nums.map(num => num * 2).reverse();

console.log(nums); // [1, 2, 3, 4, 5]
console.log(reversedDoubles); // [10, 8, 6, 4, 2]
```

### 在对象上使用`map`

虽然`map()`是用来操作数组的，但是只需要一点额外的工作，你就可以遍历对象。`Object.keys()`、`Object.values()`和`Object.entries()`都返回一个数组，这意味着`map()`可以很容易地链接到每个方法:

```
const obj = { 
  a: 1, 
  b: 2, 
  c: 3 
}
const doubles = Object.values(obj).map(num => num * 2);

console.log(doubles); // [2, 4, 6]
```

现在向前去做所有的事情！