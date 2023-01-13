# JavaScript forEach()–每个循环示例的 JS 数组

> 原文：<https://www.freecodecamp.org/news/javascript-foreach-js-array-for-each-example/>

使用数组时，有时需要循环或迭代数组的值，以便输出或操作它们。

这些数组可以保存任何数据类型，包括对象、数字、字符串等等。

在本文中，我们将了解如何使用 JavaScript `forEach()` array 方法遍历所有类型的数组，以及它与 for 循环方法有何不同。

JavaScript 中有许多迭代方法，包括`forEach()`方法，它们几乎都执行相同的功能，只有微小的差异。是否使用特定的循环方法完全由您决定，但重要的是我们要理解它们中的每一个以及它们是如何工作的。

## JavaScript forEach()

`forEach()` array 方法循环遍历任何数组，按照索引升序对每个数组元素执行一次提供的函数。这个函数被称为回调函数。

> 注意:数组是可以是任何数据类型的元素的集合。

### forEach()循环的语法和参数

以下是编写 forEach 循环的标准方法:

```
array.forEach(callbackFunction);
array.forEach(callbackFunction, thisValue);
```

回调函数最多可以接受三个不同的参数，尽管并非所有参数都是必需的。下面是一些使用普通函数和 ES6 方法来声明回调函数的`forEach()`循环的例子:

```
// Using only Current Element
array.forEach((currentElement) => { /* ... */ })
array.forEach(function(currentElement) { /* ... */ })

// Using only Current Element and Index
array.forEach((currentElement, index) => { /* ... */ })
array.forEach(function(currentElement, index) { /* ... */ })

// Using only Current Element, Index and array
array.forEach((currentElement, index, array) => { /* ... */ })
array.forEach(function(currentElement, index, array){ /* ... */ })

// Using all parameters with thisValue (value of this in the callback) 
array.forEach((currentElement, index, array) => { /* ... */ }, thisValue)
array.forEach(function(currentElement, index, array) { /* ... */ }, thisValue)
```

上面的语法可能看起来令人困惑，但它是根据您想要使用的值编写 forEach 循环的通用语法。让我们回顾一下我们使用的所有参数:

*   `callbackFunction`:回调函数是一个对每个元素只执行一次的函数，它可以接受在回调函数中使用的以下参数:

1.  `currentElement`:当前元素，顾名思义，就是循环发生时正在处理的数组中的元素。这是唯一必要的论点。
2.  `index` : index 是可选参数，携带`currentElement`的索引。
3.  `array`:array 是可选参数，返回传递给`forEach()`方法的数组。

*   `thisValue`:可选参数，指定回调函数中使用的值。

总之，`forEach()`数组迭代方法接受一个回调函数，该函数保存可以在回调函数中用于每个数组项的参数，比如数组项、项的`index`和整个数组。

## JavaScript 中的 forEach()示例

在我们看其他可能的例子之前，让我们看一下传递给回调函数的所有参数，以及它们的用途。

### 如何使用`currentElement`参数

假设我们有一组雇员的详细信息，包括他们的姓名、年龄、工资金额和货币:

```
const staffsDetails = [
  { name: "Jam Josh", age: 44, salary: 4000, currency: "USD" },
  { name: "Justina Kap", age: 34, salary: 3000, currency: "USD" },
  { name: "Chris Colt", age: 37, salary: 3700, currency: "USD" },
  { name: "Jane Doe", age: 24, salary: 4200, currency: "USD" }
];
```

如果我们想单独显示所有的名字，并在名字周围加上一些单词，我们可以使用如下的`forEach()`方法:

```
staffsDetails.forEach((staffDetail) => {
  let sentence = `I am ${staffDetail.name} a staff of Royal Suites.`;
  console.log(sentence);
});
```

输出:

```
"I am Jam Josh a staff of Royal Suites."
"I am Justina Kap a staff of Royal Suites."
"I am Chris Colt a staff of Royal Suites."
"I am Jane Doe a staff of Royal Suites."
```

**注意:**我们也可以这样析构`currentElement`值，以防它是一个包含键/值对的对象:

```
staffsDetails.forEach(({ name }, index) => {
  let sentence = `I am ${name} a staff of Royal Suites.`;
  console.log(sentence);
});
```

### 如何使用`index`参数

我们还可以通过使用未构建索引参数来获得每个数组项的`index`,如下所示:

```
staffsDetails.forEach((staffDetail, index) => {
  let sentence = `index ${index} : I am ${staffDetail.name} a staff of Royal Suites.`;
  console.log(sentence);
});
```

输出:

```
"index 0 : I am Jam Josh a staff of Royal Suites."
"index 1 : I am Justina Kap a staff of Royal Suites."
"index 2 : I am Chris Colt a staff of Royal Suites."
"index 3 : I am Jane Doe a staff of Royal Suites."
```

### 如何使用`array`参数

`array`参数是保存被迭代的原始数组的第三个参数。例如，我们可以尝试以这种方式在控制台中显示值:

```
staffsDetails.forEach((staffDetail, index, array) => {
  console.log(array);
});
```

这将输出整个数组 4 次，因为我们有 4 项，迭代运行 4 次。让我们对一个有几个值的数组这样做，这样我可以在这里添加输出:

```
let scores = [12, 55, 70];

scores.forEach((score, index, array) => {
  console.log(array);
});
```

输出:

```
[12,55,70]
[12,55,70]
[12,55,70]
```

到目前为止，我们已经使用了回调函数的所有参数。在与 for 循环方法进行快速比较之前，让我们看一些其他的例子来充分理解它是如何工作的。

### 如何用`forEach()`将一个数字数组中的所有值相加

假设我们有一个数组`scores`。我们可以使用`forEach()`数组方法循环遍历并帮助添加这些数字:

```
const scores = [12, 55, 70, 47];

let total = 0;
scores.forEach((score) => {
  total += score;
});

console.log(total);
```

回想一下，在前面，我们使用了一组人员详细信息。现在，让我们试着将所有员工的工资加在一起，看看它是如何处理对象的:

```
let totalSalary = 0;
staffsDetails.forEach(({salary}) => {
  totalSalary += salary;
});

console.log(totalSalary + " USD"); // "14900 USD"
```

**注意:**我们析构了`currentElement`的对象。

### 如何在`forEach()`回调函数中使用条件

当循环遍历数组时，我们可能希望检查特定的条件，就像 for 循环方法通常所做的那样。我们可以将这些条件传递给我们的回调函数或任何其他我们想要在每个数组项上运行的操作。

例如，如果我们只想显示我们之前声明的人员详细信息数组中薪水大于或等于`4000`的人员的姓名，我们可以执行以下操作:

```
staffsDetails.forEach(({name, salary}) => {
  if(salary >= 4000){
    console.log(name);
  }
});
```

输出:

```
"Jam Josh"
"Jane Doe"
```

## 将 forEach()与 for 循环进行比较

for 循环与 forEach 方法非常相似，但两者都有一些独特的功能，例如:

### 爆发并继续循环

当循环遍历一个数组时，当满足某个条件时，我们可能想要中断或继续循环(意味着我们跳过)。这可以通过`break`和`continue`指令来实现，但不能通过`forEach()`方法来实现，如下所示:

```
const scores = [12, 55, 70, 47];

scores.forEach((score) => {
  console.log(score);

  if (score === 70) 
    break;
});
```

这将抛出一个语法错误`Illegal break statement`。这也适用于 continue 指令，它也会抛出一个`Illegal continue statement: no surrounding iteration statement`。

```
const scores = [12, 55, 70, 47];

scores.forEach((score) => {
  if (score === 70) 
    continue;

  console.log(score);
});
```

但幸运的是，这与 for 循环方法配合得很好:

```
const scores = [12, 55, 70, 47];

for (i = 0; i < scores.length; i++) {
  console.log(scores[i]);

  if (scores[i] === 70) 
    break;
} 
```

输出:

```
12
55
70
```

与`continue`指令相同:

```
const scores = [12, 55, 70, 47];

for (i = 0; i < scores.length; i++) {
  if (scores[i] === 70) 
    continue;

  console.log(scores[i]);
} 
```

输出:

```
12
55
47
```

### 缺少元素的数组

另一个重要的比较是在我们迭代的数组有一些缺失值/数组项的情况下，如下所示:

```
const studentsScores = [70, , 12, 55, , 70, 47];
```

这可能是由于开发人员的错误或其他原因，但是这两种方法采用了两种不同的方法来遍历这些类型的数组。for 循环在缺少值的地方返回 undefined，而`forEach()`方法跳过它们。

**为循环**

```
const studentsScores = [70, , 12, 55, , 70, 47];

for (i = 0; i < studentsScores.length; i++) {
  console.log(studentsScores[i]);
}
```

输出:

```
70
undefined
12
55
undefined
70
47
```

**forEach()**

```
const studentsScores = [70, , 12, 55, , 70, 47];

studentsScores.forEach((stundentScore) => {
  console.log(stundentScore);
});
```

输出:

```
70
12
55
70
47
```

**注意:** Async/Await 不支持`forEach()`数组方法，但支持 for 循环方法。

## 结论

在本文中，我们学习了如何使用`forEach()`数组方法，该方法允许我们遍历任何类型的项目数组。它还允许我们编写比 for 循环更简洁、可读性更强的代码。