# ...在 JavaScript 中——JS 中的三点运算符

> 原文：<https://www.freecodecamp.org/news/three-dots-operator-in-javascript/>

JavaScript 中的三点运算符是 ES6 附带的重要更新之一。

这个操作符(`...`)帮助您完成许多以前需要许多行代码、不熟悉的语法等等的事情。

在这篇短文中，您将了解三点运算符的含义和作用。我们将通过一些例子来展示可能的用例，并且我们将看看您过去是如何执行这些操作的。通过这种方式，您将看到这三个点为 JavaScript 开发人员提供了什么。

三点运算符在 JavaScript 中有两种不同的含义。语法非常相似，但是你在不同的上下文中使用每一个。`...`的这两种不同用法是 spread 和 rest 操作符。

## 如何在 JavaScript 中使用 Spread 运算符

在 JavaScript 中，顾名思义，使用 spread 操作符在指定的接收器中展开 iterable。

这个接收者可以是任何东西，比如一个对象、一个数组等等。iterable 可以是我们可以循环的任何东西，包括字符串、数组、对象等等。

### 扩展运算符语法:

```
const newArray = ['firstItem', ...oldArray]; 
```

现在让我们看看可以使用 spread 运算符的各种情况。

### 如何使用 Spread 运算符复制数组

当我们想要将一个特定数组的元素复制到一个新数组中而不影响原始数组时，我们可以使用 spread 操作符。

这里有一个例子:

```
let studentNames = ["Daniel", "Jane", "Joe"];

let names = [...studentNames];

console.log(names); // ["Daniel","Jane","Joe"] 
```

这为我们节省了编写循环语句的时间:

```
let studentNames = ["Daniel", "Jane", "Joe"];

let names = [];

studentNames.map((name) => {
    names.push(name);
});

console.log(names); // ["Daniel","Jane","Joe"] 
```

### 如何使用扩展操作符复制对象

正如我们对数组所做的那样，您也可以使用一个对象作为 spread 操作符的接收者。

```
let user = { name: "John Doe", age: 10 };

let copiedUser = { ...user };
console.log(copiedUser); // { name: "John Doe", age: 10 } 
```

而旧的方法是这样使用`Object.assign()`方法:

```
let user = { name: "John Doe", age: 10 };

let copiedUser = Object.assign({}, user);
console.log(copiedUser); // { name: "John Doe", age: 10 } 
```

### 如何用 Spread 运算符连接或合并数组

当我们有两个或更多的数组想要合并成一个新的数组时，我们可以很容易地用 spread 操作符来完成。它允许我们从数组中复制元素:

```
let femaleNames = ["Daniel", "Peter", "Joe"];
let maleNames = ["Sandra", "Lucy", "Jane"];

let allNames = [...femaleNames, ...maleNames];

console.log(allNames); // ["Daniel","Peter","Joe","Sandra","Lucy","Jane"] 
```

同样重要的是，我们可以对尽可能多的阵列使用相同的方法。我们还可以在数组中添加单个元素:

```
let femaleNames = ["Daniel", "Peter", "Joe"];
let maleNames = ["Sandra", "Lucy", "Jane"];
let otherNames = ["Bill", "Jill"];

let moreNames = [...otherNames, ...femaleNames, ...maleNames];
let names = [...moreNames, "Ben", "Fred"]; 
```

这为我们节省了使用复杂语法(如`concat()`方法)的压力:

```
let femaleNames = ["Daniel", "Peter", "Joe"];
let maleNames = ["Sandra", "Lucy", "Jane"];
let otherNames = ["Bill", "Jill"];

let allNames = femaleNames.concat(maleNames);
let moreNames = femaleNames.concat(maleNames, otherNames); 
```

### 如何使用扩展运算符连接或合并对象

我们还可以像使用 spread 操作符处理数组一样连接对象:

```
let userName = { name: "John Doe" };
let userSex = { sex: "Male" };

let user = { ...userName, ...userSex };

console.log(user); // { name: "John Doe", sex: "Male" } 
```

**注意:**在一个键有另一个属性的情况下，最后一个属性覆盖第一个实例:

```
let userName = { name: "John Doe" };
let userSex = { sex: "Female", name: "Jane Doe" };

let user = { ...userName, ...userSex }; // { name: "Jane Doe", sex: "Female" } 
```

### 如何用 Set 方法检索唯一元素

使用 spread 操作符的一个重要情况是，当您试图将一个数组中的唯一元素检索到另一个数组中时。

例如，假设我们有一个水果数组，其中我们重复了一些水果，我们希望将这些水果放入一个新的数组，以避免重复。我们可以在 spread 操作符旁边使用`set()`方法，在一个新数组中列出它们:

```
let fruits = ["Mango", "Apple", "Mango", "Banana", "Mango"];

let uniqueFruits = [...new Set(fruits)];
console.log(uniqueFruits); // ["Mango","Apple","Banana"] 
```

### 如何用 Spread 运算符在函数调用中传递数组元素

当你有一个接受数字的函数，并且你有这些数字作为数组的元素时:

```
let scores = [12, 33, 6]

const addAll = (a, b, c) => {
    console.log(a + b + c);
}; 
```

您可以使用 spread 运算符将这些元素作为参数传递给函数调用:

```
let scores = [12, 33, 6]

const addAll = (a, b, c) => {
    console.log(a + b + c);
};

addAll(...scores); // 51 
```

一种旧的方法是使用`apply()`方法:

```
let scores = [12, 33, 6]

const addAll = (a, b, c) => {
    console.log(a + b + c);
};

addAll.apply(null, scores); // 51 
```

### 如何使用扩展运算符将字符串拆分成字符

假设我们有一根绳子。我们可以利用 spread 运算符将其拆分成字符:

```
let myString = "freeCodeCamp";

const splitString = [...myString];

console.log(splitString); // ["f","r","e","e","C","o","d","e","C","a","m","p"] 
```

这类似于`split()`方法:

```
let myString = "freeCodeCamp";

const splitString = myString.split('');

console.log(splitString); // ["f","r","e","e","C","o","d","e","C","a","m","p"] 
```

## 如何在 JavaScript 中使用 Rest 操作符

另一方面，rest 操作符允许您将任意数量的参数组合成一个数组，然后对它们做您喜欢的任何事情。它使用一个数组来表示无限数量的参数。

### rest 运算符的语法

```
const func = (first, ...rest) => {}; 
```

一个很好的例子来说明这一点，如果我们有一个数字列表，我们想使用第一个数字作为乘数。然后，我们希望将剩余数字的乘积放入一个数组中:

```
const multiplyArgs = (multiplier, ...otherArgs) => {
    return otherArgs.map((number) => {
    return number * multiplier;
    });
};

let multipiedArray = multiplyArgs(6, 5, 7, 9);

console.log(multipiedArray); // [30,42,54] 
```

下面是 rest 操作符及其值的一个很好的表示:

```
const multiplyArgs = (multiplier, ...otherArgs) => {
    console.log(multiplier); // 6
    console.log(otherArgs); // [5,7,9]
};

multiplyArgs(6, 5, 7, 9); 
```

**注意:**Rest 参数必须是最后一个形参。

```
const multiplyArgs = (multiplier, ...otherArgs, lastNumber) => {
    console.log(lastNumber); // Uncaught SyntaxError: Rest parameter must be last formal parameter
};

multiplyArgs(6, 5, 7, 9); 
```

## JavaScript 中 Spread 和 Rest 操作符的区别

此时，您可能会感到困惑，因为这两种方法看起来非常相似。但是 JS 团队在命名方面做得很好，因为它定义了每一次使用`...`的目的。

我们使用 spread 操作符将数组值或可重复项扩展到一个数组或对象中。

而我们使用 Rest 操作符来收集作为数组传递到函数中的剩余元素。

```
const myFunction = (name1, ...rest) => { // used rest operator here
    console.log(name1);
    console.log(rest);
};

let names = ["John", "Jane", "John", "Joe", "Joel"];
myFunction(...names); // used spread operator here 
```

## 包扎

在本文中，您了解了 JavaScript 中三点运算符的含义。您还看到了可以使用三点运算符及其两种不同含义/用例的各种情况。

祝编码愉快！