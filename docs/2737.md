# 用简单的英语解释 JavaScript 中的函数式编程

> 原文：<https://www.freecodecamp.org/news/functional-programming-in-javascript-explained-in-plain-english/>

编程中最难的事情之一就是控制复杂性。如果不仔细考虑，程序的规模和复杂性会增长到甚至让程序的创建者都感到困惑的程度。

事实上，正如一位作者所说:

> “编程的艺术是控制复杂性的技巧”——马林·哈弗贝克

在本文中，我们将分解一个主要的编程概念。这种编程概念可以帮助你控制复杂性，写出更好的程序。

到本文结束，你会知道什么是函数式编程，函数有哪些类型，函数式编程的原理，对高阶函数有更深的理解。

我假设您已经具备了函数的基础知识。函数的基本概念将不在本文中讨论。

如果你想快速回顾 JavaScript 中的函数，那么我在这里写了一篇详细的文章。

## 什么是函数式编程？

函数式编程是一种编程范式或编程风格，它在很大程度上依赖于纯函数和孤立函数的使用。

正如你可能从名字中猜到的那样，函数的使用是函数式编程的主要组成部分。但是，仅仅使用函数并不能转化为函数式编程。

在函数式编程中，我们使用的是纯函数，也就是没有副作用的函数。我会解释这一切意味着什么。

在深入本文之前，让我们了解一些术语和函数类型。

## 功能类型

有四种主要类型的函数。

### 一级函数

在 JavaScript 中，所有的函数都是一级函数。这意味着它们可以像其他变量一样对待。

一级函数是可以赋值给变量、从其他函数返回以及作为参数传递给其他函数的函数。

考虑这个传递给变量的函数的例子:

```
const helloWorld = () => {
	console.log("Hello, World"); // Hello, World
};
helloWorld(); 
```

### 回调函数

回调函数是作为参数传递给其他函数的函数，并由传递它们的函数调用。

简单地说，回调函数是我们作为参数写在其他函数中的函数。我们不能调用回调函数。当调用将它们作为参数传递的 main 函数时，它们被调用。

让我们看一个例子:

```
const testValue = (value, test) => {
    if (test(value)) {
        return `${value} passed the test`;
    } else 
        return `${value} did not pass the test`;
};
const checkString = testValue('Twitter',  string  =>  typeof  string === 'string');
checkString; // Twitter passed the test 
```

`testValue`是一个函数，它接受一个值和一个回调函数`test`,如果值在传递给回调函数时返回 true，则回调函数返回“值通过测试”。

在本例中，回调函数是我们传递给`testValue`函数的第二个参数。当调用`testValue`函数时，它被调用。

### 高阶函数

高阶函数是接收其他函数作为参数或返回一个函数的函数。

在这篇文章中，我将进一步阐述高阶函数，以及为什么它们是如此强大的条款。现在，您只需要知道这些类型的函数接收其他函数作为参数或返回函数。

### 异步函数

异步函数是没有名字并且不能被重用的函数。这些函数通常是在我们只需要在一个地方执行一次时编写的。

异步函数的一个完美例子是我们在文章前面写的。

```
const checkString = testValue('Twitter',  value  =>  typeof  value === 'string');
checkString;

// Refer to previous code snippet
```

`checkString`是值为函数的变量。我们向该函数传递两个参数。

`'Twitter'`是第一个参数，第二个是异步函数。这个函数没有名字，只有一个任务:检查给定值是否是一个字符串。

![Principles Meme](img/abcd89250e34ddef0d06ae2020be65c4.png)

## 函数式编程原理

在文章的前面，我提到了这样一个事实，仅仅使用函数并不能转化为函数式编程。

如果我们的程序要符合函数式编程标准，我们需要理解一些原则。让我们看看那些。

### 避免突变和副作用。

函数式编程的首要原则是避免改变事物。函数不应该改变任何东西，比如全局变量。

这一点非常重要，因为变更往往会导致 bug。例如，如果一个函数改变了一个全局变量，它可能会在所有使用该变量的地方导致意外的行为。

第二个原则是函数必须是纯的，这意味着它没有副作用。在函数式编程中，所做的改变被称为突变，结果被称为副作用。

一个纯函数不做这两者。一个纯函数对于相同的输入总是有相同的输出。

如果一个函数依赖于一个全局变量，那么这个变量应该作为参数传递给这个函数。这使得我们可以用相同的输入获得相同的输出。

这里有一个例子:

```
const legalAgeInTheUS = 21;
const checkLegalStatus = (age, legalAge) => {
    return age >= legalAge ? 'Of legal age.' : 'Not of legal age.';
};
const johnStatus = checkLegalStatus(18, legalAgeInTheUS);
johnStatus; // Not of legal age
legalAgeInTheUS; // 21 
```

### 抽象

抽象隐藏了细节，允许我们在更高的层次上讨论问题，而无需描述问题的所有实现细节。

我们在生活的几乎所有方面都使用抽象，尤其是在演讲中。

例如，你最有可能说*“我要去买一台电视机”*，而不是说*“我要去换钱，换一台插上电源就能显示有声音的动态图像的机器”*。

在这种情况下,**购买**和**电视**都是抽象概念。这些形式的抽象使得演讲变得更加容易，并且减少了说错话的机会。

但是你会同意我的观点，在使用像 **buy** 这样的抽象术语之前，你需要首先理解这个术语的含义以及它所抽象的问题。

函数允许我们实现类似的东西。我们可以为最有可能反复重复的任务创建函数。函数允许我们创建自己的抽象。

除了创建我们自己的抽象之外，已经为我们创建了一些函数来抽象我们最有可能反复做的任务。

所以我们要看看一些高阶函数，它们已经存在来抽象重复性的任务。

### 过滤阵列

当处理像数组这样的数据结构时，我们很可能会发现自己只对数组中的某些项目感兴趣。

要获得这些项目，我们可以轻松地创建一个函数来完成任务:

```
function filterArray(array, test) {
    const filteredArray = [];
    for (let item of array) {
        if (test(item)) {
            filteredArray.push(item);
        }
    }
    return filteredArray;
};
const mixedArray = [1, true, null, "Hello", undefined, "World", false];
const onlyStrings = filterArray(mixedArray, item => typeof item === 'string');
onlyStrings; // ['Hello', 'World'] 
```

`filterArray`是一个接受数组和回调函数的函数。它遍历数组，将回调函数中通过测试的项添加到名为`filteredArray`的数组中。

使用这个函数，我们能够过滤一个数组并返回我们感兴趣的项目，比如在`mixedArray`的情况下。

想象一下，如果我们有 10 个不同的程序，在每个程序中我们需要过滤一个数组。一遍又一遍地重写同一个函数迟早会变得非常令人厌倦。

幸运的是，有人已经想到了这一点。数组有一个标准的`filter`方法。它返回一个新的数组，数组中的项目通过了我们提供的测试。

```
const mixedArray = [1, true, null, "Hello", undefined, "World", false];
const stringArray = mixedArray.filter(item => typeof item === 'string')
stringArray; // ['Hello', 'World'] 
```

使用标准筛选方法，我们能够获得与在前面的示例中定义自己的函数时相同的结果。所以，filter 方法是我们写的第一个函数的抽象。

### 用映射转换数组项

想象另一个场景，我们有一个项目数组，但是我们想对所有项目执行某个操作。我们可以编写一个函数来完成这项工作:

```
function transformArray(array, test) {
    const transformedArray = [];
    for (let item of array) {
        transformedArray.push(test(item));
    }
    return transformedArray;
};
const ages = [12, 15, 21, 19, 32];
const doubleAges = transformArray(ages, age => age * 2);
doubleAges; // [24, 30, 42, 38, 64]; 
```

就像这样，我们创建了一个函数，它循环遍历任何给定的数组，并根据我们提供的回调函数转换数组中的所有项。

但是，如果我们不得不在 20 个不同的程序中重写函数，这又会变得乏味。

又一次，有人替我们想到了这一点，幸运的是数组有一个叫做`map`的标准方法来做同样的事情。它对给定数组中的所有项应用回调函数，然后返回一个新数组。

```
const ages = [12, 15, 21, 19, 32];
const doubleAges = ages.map(age => age * 2);
doubleAges; // [24, 30, 42, 38, 64]; 
```

### 用 Reduce 减少数组

这里有另一个场景:你有一个数字数组，但是你想计算所有这些数字的和并返回它。当然你可以写一个函数来帮你做到这一点。

```
function reduceArray(array, test, start) {
    let sum = start;
    for (let item of array) {
        sum = test(sum, item)
    }
    return sum;
}
let numbers = [5, 10, 20];
let doubleNumbers = reduceArray(numbers, (a, b) => a + b, 0);
doubleNumbers; // 35 
```

类似于我们刚刚看到的前面的例子，数组有一个标准的`reduce`方法，它与我们刚刚写的函数有相同的逻辑。

reduce 方法用于根据我们提供的回调函数将数组缩减为单个值。它还带有一个可选的第二个参数，该参数指定我们希望回调中的操作从哪里开始。

我们在 reduce 函数中提供的回调函数有两个参数。默认情况下，第一个参数是数组中的第一项。否则这是我们提供给 reduce 方法的第二个参数。第二个参数是数组中的当前项。

```
let numbers = [5, 10, 20];
let doubleNumbers = numbers.reduce((a, b) => a + b, 10);
doubleNumbers;  // 45

//The above example uses the reduce method to add all the items in the array starting from 10.
```

## 其他有用的数组方法

### Array.some()

所有数组都有接受回调函数的`some`方法。如果**数组中的任何**元素通过了回调函数中给定的测试，它将返回`true`。否则返回`false`:

```
const numbers = [12, 34, 75, 23, 16, 63]
console.log(numbers.some(item => item < 100)) // true
```

### Array.every()

every 方法与 some 方法相反。它还接受一个回调函数，如果**数组中的所有**项都通过了回调函数中给出的测试，则返回`true`。否则返回`false`:

```
const numbers = [12, 34, 75, 23, 16, 63]
console.log(numbers.every(item => item < 100)) // true
```

### Array.concat()

`concat`方法是 concatenate 的缩写，是一个标准的数组方法，它连接两个数组并返回一个新数组:

```
const array1 = ['one', 'two', 'three'];
const array2 = ['four', 'five', 'six'];
const array3 = array1.concat(array2);
array3; // [ 'one', 'two', 'three', 'four', 'five', 'six' ]
```

### Array.slice()

`slice`方法是一个数组方法，它从一个给定的索引中复制一个数组的项，并返回一个包含复制项的新数组。`slice`方法接受两个参数。

第一个参数接收开始复制的索引。第二个参数接收停止复制的索引。它返回一个新数组，其中包含从起始索引(不包括)到最终索引(包括)的复制项。

但是请注意，切片方法不使用零索引。所以第一个数组项的索引是 1 而不是 0:

```
const numbers = [1,2,3,4,5,7,8];
console.log(theArray.slice(1, 4)); // [ 2, 3, 4 ] 
```

## 结论

我希望你喜欢阅读这篇文章，同时学到一些新东西。

还有很多数组和字符串方法我在文章中没有提到。如果你愿意，花些时间研究一下这些方法。

如果你想和我联系或者只是打个招呼？你可以通过[推特](http://twitter.com/joeepm)随意这么做。我也为开发者分享有趣的技巧和资源。？