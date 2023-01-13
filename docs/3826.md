# JavaScript APIs 简介:Reduce 函数

> 原文：<https://www.freecodecamp.org/news/applications-of-the-reduce-function-in-javascript/>

随着今年的开始，我决定撰写一系列文章来解释 JavaScript 语言中的各种 API(应用程序编程接口)。在每篇文章中，我们将分解 JavaScript 中的一个常用函数，并尝试研究它的各种应用。

我们将经历的第一个函数是' **Reduce** 高阶函数。这主要是因为，在所有的 JS 数组方法中，我花了一点时间来理解 Reduce 函数是如何工作的。

本文假设读者理解其他数组方法，如 **Map** 和 **Filter** ，因为这将有助于理解 **Reduce** 是如何工作的。

为了完全掌握 Reduce **、**背后的思想，我们将看几个使用**进行**循环的简单解决方案的例子，然后使用 Reduce **、**函数实现那些相同的解决方案。然后我们将看看 Reduce 函数的一些更高级的用例。

## 示例 1

我们要看的第一个例子是一个常见的例子:计算数组中项的总和。这需要一个简单的解决方案，对循环使用**应该是这样的:**

```
const arrayItems = [1,2,3,4,5,6];
let sum = 0;

for (let i = 0; i < arrayItems.length; i++) {
	sum = sum + arrayItems[i];
}
// sum = 21
```

上面的解决方案非常简单，我们将数组中的每一项相加，并将结果存储在`sum`变量中。所以下一步是使用 **Reduce** ，实现相同的解决方案，看起来应该像下面的代码:

```
const arrayItems = [1,2,3,4,5,6];

const sum = arrayItems.reduce(function(accumulator, currentItemInArray){
	accumulator = accumulator + currentItemInArray;
    return accumulator;
}, 0);

// sum = 21
```

看上面的两个例子，很明显, **for** 循环的例子看起来更简单，这也是生态系统中一些争论的原因。但是这个例子是多余的，我们使用它只是为了便于理解 Reduce 函数是如何工作的，所以让我们完成这个例子。

首先，我们需要理解 Reduce 函数是什么。它是每个 JavaScript 数组都有的方法。它使我们能够遍历数组中的每一项，并对每一项执行一个函数。

这非常类似于 **Map** 函数的行为，但是它有一个变化——它允许我们在特定的迭代中从我们的函数返回任何值，然后在下一次迭代中作为该函数的参数(自变量)存在(该值通常被称为**累加器**)。

为了进一步解释，Reduce 函数有两个参数:

*   回调函数:这是一个通常包含 4 个参数的函数。但是现在我们只关心第一个，累加器，和第二个，也就是迭代中数组的当前项。
*   初始值:这是迭代开始时累加器的初始值。在上面的例子中，值是 0，这意味着累加器的初始值将是 0。

回到我们的例子:

```
const arrayItems = [1,2,3,4,5,6];

const sum = arrayItems.reduce(function(accumulator, currentItemInArray){
	accumulator = accumulator + currentItemInArray;
    return accumulator;
}, 0);

// sum = 21
```

它可以进一步分解为回调函数和初始值:

```
const arrayItems = [1,2,3,4,5,6];

function callbackFunction(accumulator, currentItemInArray){
    accumulator = accumulator + currentItemInArray;
    return accumulator;
}

const initialValue = 0;

const sum = arrayItems.reduce(callbackFunction, initialValue);

// sum = 21
```

对我来说，棘手的部分是理解累加器如何工作。为了解释它，我们将遍历循环中的每个迭代。

### 迭代 1

在第一次迭代中，因为我们的初始值是 0，所以累加器的值将是 0。所以我们的函数看起来像这样:

```
const arrayItems = [1,2,3,4,5,6];
// 1 is the current item in the array

function callbackFunction(accumulator = 0, currentItemInArray = 1){
    accumulator = 0 + 1;
    return accumulator // which is 1;
}
```

`callbackFunction`将返回值 1。这将在第二次迭代中自动用作累加器的下一个值。

### 迭代 2

```
const arrayItems = [1,2,3,4,5,6];
// 2 is the current item in the array

function callbackFunction(accumulator = 1, currentItemInArray = 2){
    accumulator = 1 + 2;
    return accumulator // which is 3;
}
```

在这个迭代中，我们的累加器将有一个值 1，这个值在我们的第一次迭代中返回。在这次迭代中,`callbackFunction`将返回值 3。这意味着我们的累加器在第三次迭代中将有一个值 3。

### 迭代 3

```
const arrayItems = [1,2,3,4,5,6];
// 3 is the current item in the array

function callbackFunction(accumulator = 3, currentItemInArray = 3){
    accumulator = 3 + 3;
    return accumulator // which is 6;
}
```

在第三次迭代中，我们的累加器将有一个值 3，它是由迭代 2 中的`callbackFunction`返回的。`callbackFunction`将返回一个值 6，该值将在第 4 次迭代中用作累加器的值。这些步骤将重复进行，直到我们到达数组中的最后一项，即 6。

正如我之前提到的，上面的例子可能有点矫枉过正，所以让我们看看使用 Reduce 更常见的一个问题。(然而，这并不意味着循环的**不能用于实现工作解决方案)。**

## 示例 2

第二个例子是计算数组中每个元素出现的次数，例如:

```
//Given an input
const fruits = ['apples', 'apples', 'bananas', 'oranges', 'apples', 'oranges', 'bananas', 'grapes'];

// should give an output of
const count = { 'apples': 3,'oranges': 2,'bananas': 2, 'grapes': 1 };
```

让我们实现解决方案，然后遍历每个迭代，看看会发生什么:

```
const fruits = ['apples', 'apples', 'bananas', 'oranges', 'apples', 'oranges', 'bananas', 'grapes'];

function countOccurrence(accumulator, currentFruit){
	const currentFruitCount = accumulator[currentFruit];
    // if the fruit exists as a key in the  object, increment its value, else add the fruit as a key to the object with a value of 1

    if(currentFruitCount) {
    	accumulator[currentFruit] = currentFruitCount + 1;
    } else {
    	accumulator[currentFruit] = 1
    }

    return accumulator;
}

const initialValue = {};

const count = fruits.reduce(countOccurrence, initialValue);
```

解决方案写得尽可能详细，这样我们就可以理解代码中发生了什么。就像我们之前做的一样，让我们来看看最初的几次迭代。

### 迭代 1

在第一次迭代中，由于我们将初始值设为空对象，因此`accumulator`的值将为空对象。这意味着`countOcurrence`函数在被调用时将类似于下面的代码:

```
const fruits = ['apples', 'apples', 'bananas', 'oranges', 'apples', 'oranges', 'bananas', 'grapes'];

// current element is 'apples'

function countOccurrence(accumulator = {}, currentFruit = 'apples'){
    // since currentFruit = 'apples' then accumulator[currentFruit] = accumulator['apples']

	const currentFruitCount = accumulator[currentFruit];
    // currentFruitCount will be null since accumulator is an empty object

    if(currentFruitCount) {
    	accumulator[currentFruit] = currentFruitCount + 1;
    } else {
        // this block will run since accumulator is empty
        // currentFruit = 'apples'
    	accumulator['apples'] = 1
        // accumulator should look like this: { 'apples': 1 }
    }

    return accumulator // which is { 'apples': 1 };
}
```

由于`accumulator`是一个空对象，`currentFruitCount`将是`null`。这意味着`else`块将运行，其中值为 1 的新键(苹果)将被添加到`accumulator`。这将从函数中返回，该函数将在第二次迭代中作为累加器的值传递。

### 迭代 2

在第二次迭代中，我们的`accumulator`将具有`{ 'apples': 1 }`的值，该值由第一次迭代中的`countOccurrence`函数返回。然后`countOccurrence`函数将看起来像下面的代码:

```
const fruits = ['apples', 'apples', 'bananas', 'oranges', 'apples', 'oranges', 'bananas', 'grapes'];

// current element is 'apples'

function countOccurrence(accumulator = { 'apples': 1 }, currentFruit = 'apples'){
    // since currentFruit = 'apples' then accumulator[currentFruit] = accumulator['apples']

	const currentFruitCount = accumulator[currentFruit];
    // currentFruitCount will be 1 

    if(currentFruitCount) {
        // this block will run since currentFruitCount is 1
        // currentFruit = 'apples'
    	accumulator['apples'] = 1 + 1;
        // accumulator should look like this: { 'apples': 2 }
    } else {
    	accumulator[currentFruit] = 1
    }

    return accumulator // which is { 'apples': 2 };
}
```

因为`accumulator`包含一个值为 1 的键(‘apple’)，所以`currentFruit`将为 1，这意味着`if`块将被运行。在该块中，`apple`键的值将增加 1，使其为 2，这个新值将在累加器对象中更新，使其为`{ 'apples' : 2 }`。这个值将由`countOccurrence`函数返回，并在第三次迭代中作为累加器的值传递。

### 迭代 3

对于我们的第三次迭代，`accumulator`有`{ apples: 2 }`的值，它是由`countOccurence`在第二次迭代中返回的。`countOccurence`函数看起来像下面的代码:

```
const fruits = ['apples', 'apples', 'bananas', 'oranges', 'apples', 'oranges', 'bananas', 'grapes'];

// current element is 'bananas'

function countOccurrence(accumulator = { 'apples': 2 }, currentFruit = 'bananas'){
    // since currentFruit = 'bananas' then accumulator[currentFruit] = accumulator['bananas']

	const currentFruitCount = accumulator[currentFruit];
        // currentFruitCount will be null since accumulator doesn't contain 'bananas'

    if(currentFruitCount) {
        accumulator[currentFruit] = currentFruitCount + 1;
    } else {
        // this block will run since currentFruitCount is null
        // currentFruit = 'bananas'
    	accumulator['bananas'] = 1
    }

    return accumulator // which is { 'apples': 2, 'bananas': 1  };
}
```

这个迭代类似于第一个——因为`bananas`在`accumulator`中不存在，它将被添加到对象中，并被赋予一个值`1`，使得`accumulator`看起来像这样:`{ 'apples': 2, 'bananas': 1 }`。这将成为第四次迭代的`accumulator`的值。

该过程将重复进行，直到 Reduce 函数遍历完数组中的每个元素。

## 包扎

我真的希望这些例子足够清晰，能够创建一个关于 **Reduce** 函数如何工作的心智模型。

如果你正在读这篇文章，并且你想看更多的高级例子(比如实现`pipe`函数),请随意发微博给我，我会尽快回复。此外，如果你有其他的例子，我很乐意看到他们。谢谢！！！