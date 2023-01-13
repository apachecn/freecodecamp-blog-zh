# 如何对数组中的对象进行计数

> 原文：<https://www.freecodecamp.org/news/how-to-count-objects-in-an-array/>

知道如何快速遍历一个数组并计数对象看似简单。`length()`方法会告诉您数组中值的总数，但是如果您只想根据某些条件来计算这些值的数目，该怎么办呢？

例如，假设您有一个这样的数组:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];
```

而你只想在`status`设置为`'0'`的情况下统计对象的数量。

就像编程中的所有事情一样，有很多方法可以做到这一点。我们将在下面介绍一些常见的方法。

## 使用`for`循环

最简单的方法可能是声明一个`counter`变量，遍历数组，只有当`status`等于`'0'`时才迭代`counter`:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

let counter = 0;
for (let i = 0; i < storage.length; i++) {
  if (storage[i].status === '0') counter++;
}

console.log(counter); // 6
```

您可以通过使用一个`for...of`循环来简化这一点:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

let counter = 0;
for (const obj of storage) {
  if (obj.status === '0') counter++;
}

console.log(counter); // 6
```

此外，如果您有其他对象数组要有条件地计数，您可以创建一个函数来做同样的事情:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

function statusCounter(inputs) {
  let counter = 0;
  for (const input of inputs) {
    if (input.status === '0') counter += 1;
  }
  return counter;
}

statusCounter(storage); // 6
```

## 使用数组方法

JavaScript 在处理数组时包含了一堆[有用的方法](https://www.freecodecamp.org/news/javascript-standard-objects-arrays/)。每一个都可以链接到一个数组，并在遍历数组中的元素时传递不同的参数。

我们要看的两个是`filter()`和`reduce()`。

### `filter()`

filter 方法就是这样做的——它遍历数组中的每个元素，过滤掉所有不符合您提供的条件的元素。然后，它返回一个新数组，其中包含根据您的条件返回 true 的所有元素。

例如:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.filter(function(item){
  if (item.status === 0) {
    return true;
  } else {
    return false;
  }
});

/*
[
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' }
] 
*/
```

既然已经过滤掉了带有`status: '1'`的对象，只需在新数组上调用`length()`方法，就可以获得带有`status: '1'`的对象的总数:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.filter(function(item){
  if (item.status === 0) {
    return true;
  } else {
    return false;
  }
}).length; // 6
```

但是这可以用 ES6 语法缩短很多:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.filter(item => item.status === '0').length; // 6
```

### `reduce()`

把`reduce()`方法想象成一把瑞士军刀——它非常灵活，允许你把一个数组作为输入，并把它转换成任何东西。更好的是，像`filter()`一样，这个方法返回一个新的数组，保持原来的不变。

你可以在[这篇文章](https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-array-methods-reduce/)中阅读更多关于`reduce()`的内容。

就我们的目的而言，我们想要获取一个数组，检查它的内容，并产生一个数字。这里有一个简单的方法:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.reduce((counter, obj) => {
  if (obj.status === '0') counter += 1
  return counter;
}, 0); // 6
```

您可以通过使用 ES6 语法和三元运算符来进一步简化:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.reduce((counter, obj) => obj.status === '0' ? counter += 1 : counter, 0); // 6
```

通过使用[对象析构](https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/)甚至更多:

```
const storage = [
  { data: '1', status: '0' },
  { data: '2', status: '0' },
  { data: '3', status: '0' },
  { data: '4', status: '0' },
  { data: '5', status: '0' },
  { data: '6', status: '0' },
  { data: '7', status: '1' },
];

const count = storage.reduce((counter, { status }) => status === '0' ? counter += 1 : counter, 0); // 6
```

这是遍历数组元素并有条件计数的几种方法。现在出去自信地数数吧！