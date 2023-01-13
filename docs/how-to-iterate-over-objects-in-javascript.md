# JavaScript 中的对象循环——如何在 JS 中迭代对象

> 原文：<https://www.freecodecamp.org/news/how-to-iterate-over-objects-in-javascript/>

在 JavaScript 中，当您听到“循环”这个术语时，您可能会想到使用各种循环方法，如 [`for`循环](https://www.freecodecamp.org/news/javascript-for-loops/)、 [`forEach()`、](https://www.freecodecamp.org/news/javascript-foreach-js-array-for-each-example/)、`map()`等。

但是在对象的情况下，不幸的是，这些方法不起作用，因为对象是不可迭代的。

这并不意味着我们不能遍历一个对象，而是意味着我们不能像遍历数组一样直接遍历一个对象:

```
let arr = [24, 33, 77];
arr.forEach((val) => console.log(val)); // ✅✅✅

for (val of arr) {
  console.log(val); // ✅✅✅
}

let obj = { age: 12, name: "John Doe" };
obj.forEach((val) => console.log(val)); // ❌❌❌

for (val of obj) {
  console.log(val); // ❌❌❌
} 
```

在本文中，您将了解如何在 JavaScript 中遍历一个对象。有两种方法可以使用——其中一种是在 ES6 推出之前。

## 如何用一个`for…in`循环遍历 JavaScript 中的对象

在 ES6 之前，每当我们想要遍历一个对象时，我们都依赖于`for...in`方法。

`for...in`循环遍历原型链中的属性。这意味着每当我们使用`for…in`循环遍历一个对象时，我们需要使用`hasOwnProperty`来检查属性是否属于该对象:

```
const population = {
  male: 4,
  female: 93,
  others: 10
};

// Iterate through the object
for (const key in population) {
  if (population.hasOwnProperty(key)) {
    console.log(`${key}: ${population[key]}`);
  }
} 
```

为了避免循环的压力和困难并使用`hasOwnProperty`方法，ES6 和 ES8 引入了对象静态方法。这些将对象属性转换为数组，允许我们直接使用数组方法。

## 如何使用对象静态方法在 JavaScript 中遍历对象

一个对象由具有键值对的属性组成，也就是说每个属性总是有一个对应的值。

对象静态方法允许我们提取数组中的`keys()`、`values()`，或者同时提取键和值作为`entries()`，允许我们像处理实际数组一样灵活地处理它们。

我们有三个对象静态方法，它们是:

*   `Object.keys()`
*   `Object.values()`
*   `Object.entries()`

### 如何使用`Object.keys()`方法在 JavaScript 中遍历对象

在 ES6 中引入了`Object.keys()`方法。它将我们想要循环的对象作为一个参数，并返回一个包含所有属性名(也称为键)的数组。

```
const population = {
  male: 4,
  female: 93,
  others: 10
};

let genders = Object.keys(population);

console.log(genders); // ["male","female","others"] 
```

这为我们提供了应用任何数组循环方法来遍历数组并检索每个属性的值的优势:

```
let genders = Object.keys(population);

genders.forEach((gender) => console.log(gender)); 
```

这将返回:

```
"male"
"female"
"others" 
```

我们还可以使用如下所示的`population[gender]`这样的括号符号来使用键获取值:

```
genders.forEach((gender) => {
  console.log(`There are ${population[gender]} ${gender}`);
}) 
```

这将返回:

```
"There are 4 male"
"There are 93 female"
"There are 10 others" 
```

在我们继续之前，让我们通过循环使用此方法对所有人口求和，这样我们就知道了总人口:

```
const population = {
  male: 4,
  female: 93,
  others: 10
};

let totalPopulation = 0;
let genders = Object.keys(population);

genders.forEach((gender) => {
  totalPopulation += population[gender];
});

console.log(totalPopulation); // 107 
```

### 如何使用`Object.values()`方法在 JavaScript 中遍历对象

`Object.values()`方法与`Object.keys()`方法非常相似，在 ES8 中引入。这个方法将我们想要循环的对象作为一个参数，并返回一个包含所有键值的数组。

```
const population = {
  male: 4,
  female: 93,
  others: 10
};

let numbers = Object.values(population);

console.log(numbers); // [4,93,10] 
```

这给了我们应用任何数组循环方法来遍历数组并检索每个属性的`value`的优势:

```
let numbers = Object.values(population);

numbers.forEach((number) => console.log(number)); 
```

这将返回:

```
4
93
10 
```

我们可以有效地执行总计算，因为我们可以直接循环:

```
let totalPopulation = 0;
let numbers = Object.values(population);

numbers.forEach((number) => {
  totalPopulation += number;
});

console.log(totalPopulation); // 107 
```

### 如何使用 Object.entries()方法在 JavaScript 中遍历对象

ES8 也引入了`Object.entries()`方法。从基本意义上来说，它所做的是输出一个数组的数组，其中每个内部数组都有两个元素，即属性和值。

```
const population = {
  male: 4,
  female: 93,
  others: 10
};

let populationArr = Object.entries(population);

console.log(populationArr); 
```

这将输出:

```
[
  ['male', 4]
  ['female', 93]
  ['others', 10]
] 
```

这将返回一个数组的数组，每个内部数组都有`[key, value]`。您可以使用任何数组方法来遍历:

```
for (array of populationArr){
  console.log(array);
}

// Output:
// ['male', 4]
// ['female', 93]
// ['others', 10] 
```

我们可以决定[析构数组](https://www.freecodecamp.org/news/destructuring-patterns-javascript-arrays-and-objects/)，因此我们得到`key`和值:

```
for ([key, value] of populationArr){
  console.log(key);
} 
```

您可以在本文中了解更多关于如何[遍历数组的信息。](https://www.freecodecamp.org/news/how-to-loop-through-an-array-in-javascript-js-iterate-tutorial/)

## 包扎

在本教程中，您了解了遍历对象的最佳方式是根据您的需要使用任何对象静态方法，在循环之前先转换为数组。

祝编码愉快！