# JavaScript 循环解释:For 循环，While 循环，Do...while 循环等等

> 原文：<https://www.freecodecamp.org/news/javascript-loops-explained-for-loop-for/>

JavaScript 中使用循环来根据条件执行重复的任务。条件通常返回`true`或`false`。循环将继续运行，直到定义的条件返回`false`。

## `for`循环

### **语法**

```
for (initialization; condition; finalExpression) {
  // code
} 
```

`for`循环由三个可选表达式组成，后跟一个代码块:

*   `initialization` -该表达式在第一个循环执行之前运行，通常用于创建计数器。
*   `condition` -每次循环运行前检查该表达式。如果计算结果为`true`，则执行循环中的`statement`或代码。如果计算结果为`false`，循环停止。如果这个表达式被省略，它将自动计算为`true`。
*   `finalExpression` -该表达式在每次循环迭代后执行。这通常用于递增计数器，但也可用于递减计数器。

这三个表达式中的任何一个或代码块中的代码都可以省略。

循环通常用于运行代码一定的次数。此外，在`condition`表达式计算为`false`之前，您可以使用`break`提前退出循环。

### **例题**

1.遍历 0-8 之间的整数:

```
for (let i = 0; i < 9; i++) {
  console.log(i);
}

// Output:
// 0
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
```

2.在`condition`到达`false`之前，使用`break`退出`for`循环:

```
for (let i = 1; i < 10; i += 2) {
  if (i === 7) {
    break;
  }
  console.log('Total elephants: ' + i);
}

// Output:
// Total elephants: 1
// Total elephants: 3
// Total elephants: 5
```

### 常见陷阱: ****超过**B**A**A**rray****

当迭代一个数组时，很容易意外地超出数组的边界。

例如，您的循环可能试图引用只有 3 个元素的数组的第 4 个元素:

```
const arr = [ 1, 2, 3 ];

for (let i = 0; i <= arr.length; i++) {
  console.log(arr[i]);
}

// Output:
// 1
// 2
// 3
// undefined
```

有两种方法可以修复这个代码:将`condition`设置为`i < arr.length`或`i <= arr.length - 1`。

## `for...in`循环

### 句法

```
for (property in object) {
  // code
}
```

`for...in`循环遍历对象的属性。对于每个属性，执行代码块中的代码。

### 例子

1.迭代对象的属性，并将其名称和值记录到控制台:

```
const capitals = {
  a: "Athens",
  b: "Belgrade",
  c: "Cairo"
};

for (let key in capitals) {
  console.log(key + ": " + capitals[key]);
}

// Output:
// a: Athens
// b: Belgrade
// c: Cairo 
```

### 常见陷阱:**迭代数组时的意外行为**

虽然您可以使用`for...in`循环来迭代数组，但是建议使用常规的`for`或`for...of`循环。

`for...in`循环可以遍历数组和类似数组的对象，但是它可能不总是按顺序访问数组索引。

另外，`for...in`循环返回数组或类似数组的对象的所有属性和继承属性，这可能会导致意外的行为。

例如，这个简单的循环按预期工作:

```
const array = [1, 2, 3];

for (const i in array) {
  console.log(i);
}

// 0
// 1
// 2 
```

但是，如果您使用的 JS 库之类的东西直接修改了`Array`原型，那么`for...in`循环也会对其进行迭代:

```
const array = [1, 2, 3];

Array.prototype.someMethod = true;

for (const i in array) {
  console.log(i);
}

// 0
// 1
// 2
// someMethod 
```

虽然修改只读原型，如`Array`或`Object`直接违背了最佳实践，但这可能是一些库或代码库的问题。

另外，因为`for...in`是针对对象的，所以数组比其他循环慢得多。

简而言之，只要记住只使用`for...in`循环来迭代对象，而不是数组。

## `for...of`循环

### 句法

```
for (variable of object) {
  // code
} 
```

`for...of`循环遍历许多类型的可迭代对象的值，包括数组和特殊的集合类型，如`Set`和`Map`。对于 iterable 对象中的每个值，执行代码块中的代码。

### 例子

1.迭代数组:

```
const arr = [ "Fred", "Tom", "Bob" ];

for (let i of arr) {
  console.log(i);
}

// Output:
// Fred
// Tom
// Bob 
```

2.迭代一个`Map`:

```
const m = new Map();
m.set(1, "black");
m.set(2, "red");

for (let n of m) {
  console.log(n);
}

// Output:
// [1, black]
// [2, red] 
```

3.迭代一个`Set`:

```
const s = new Set();
s.add(1);
s.add("red");

for (let n of s) {
  console.log(n);
}

// Output:
// 1
// red 
```

## `while`循环

### 句法

```
while (condition) {
  // statement
} 
```

`while`循环从评估`condition`开始。如果`condition`评估为`true`，代码块中的代码将被执行。如果`condition`评估为`false`，则不执行代码块中的代码，循环结束。

### 示例:

1.  当变量小于 10 时，将其记录到控制台，并以 1 递增:

```
let i = 1;

while (i < 10) {
  console.log(i);
  i++;
}

// Output:
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9 
```

## `do...while`循环

### 语法:

```
do {
  // statement
} while (condition); 
```

`do...while`回路与`while`回路密切相关。在一个`do...while`循环中，`condition`在循环的每一次迭代结束时被检查，而不是在循环开始之前被检查。

这意味着一个`do...while`循环中的代码保证至少运行一次，即使`condition`表达式已经计算为`true`。

### 示例:

1.  当变量小于 10 时，将其记录到控制台，并以 1 递增:

```
let i = 1;

do {
  console.log(i);
  i++;
} while (i < 10);

// Output:
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9 
```

2.推入数组，即使`condition`的计算结果为`true`:

```
const myArray = [];
let i = 10;

do {
  myArray.push(i);
  i++;
} while (i < 10);

console.log(myArray);

// Output:
// [10]
```