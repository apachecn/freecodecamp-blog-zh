# JavaScript 中的 find()与 filter()的区别用例子解释

> 原文：<https://www.freecodecamp.org/news/find-vs-filter-javascript/>

JavaScript 开发人员经常被问到的一个常见面试问题是解释 find()和 filter()方法之间的区别。

在今天的教程中，我将带你了解这些方法是什么，以及何时应该使用它们。

## 什么是`filter()`法？

该方法返回数组中满足回调函数中指定条件的所有元素。

让我们通过一个例子来看看它实际上是如何工作的:

```
const x = [1, 2, 3, 4, 5];

const y = x.filter(el => el*2 === 2);

console.log("y is: ", y); // y is: [1]
```

example of filter()

如果您查看上面示例的输出，y 的**值是满足条件的 1 个元素**的数组 **。这是因为 filter()方法迭代数组的所有元素，然后返回满足指定条件的筛选数组。**

## 什么是`find()`法？

此方法返回满足回调函数中指定条件的数组的第一个元素。

让我们通过一个例子来看看它实际上是如何工作的:

```
const x = [1, 2, 3, 4, 5];

const y = x.find(el => el*2 === 2);

console.log("y is: ", y); // y is: 1
```

example of find()

现在，如果你看到上面例子的输出，y 的**值是 1** 。这是因为 find()方法搜索数组中满足指定条件的第一个元素。

以上例子之间的主要区别是:

1.  `filter()`返回包含满足条件的元素的数组，但是`find()`返回满足条件的元素本身。
2.  在`filter()`中，整个数组被迭代，尽管被搜索的元素出现在开始处。但是在`find()`中，只要找到满足条件的元素，它就会被返回。

## `find()`和`filter()`的用例

当您有一个用例，期望返回不止一个元素，并且您想要对所有元素执行操作，那么您可以使用 **filter()** 方法。但是如果您希望从数组中只返回一个元素，那么您可以使用 **find()** 并避免额外的迭代。

让我们看看这两种使用情形的示例:

### 1.filter()用例示例

```
const x = [1, 2, 3, 4, 5];

const y = x.filter(el => el%2 === 0);

console.log("y is: ", y); // y is: [2, 4]
```

filter() use case example

在上面的例子中，`**filter()**`更有意义，因为您想要迭代数组中的所有元素，以找到能被 2 整除的元素。

### 2.find()用例示例

```
const emp = [
    {
        name: "Ram",
        empID: 101
    },
    {
        name: "Sham",
        empID: 102
    },
    {
        name: "Mohan",
        empID: 103
    }
];

const res = emp.find(el => el.empID === 102);

console.log("res is: ", res); // res is: {name: 'Sham', empID: 102}
```

find() use case example

在上面的例子中，`**find()**`更有意义，因为只有一个雇员将`102`作为`empID` ，因此，`find()`有助于避免迭代数组中的第三个对象。

### **感谢阅读！**

如果你觉得这篇文章有用，请与你的朋友和同事分享。