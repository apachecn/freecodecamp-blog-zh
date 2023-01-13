# JavaScript 中的高阶函数——在 JS 代码中达到新的高度

> 原文：<https://www.freecodecamp.org/news/higher-order-functions-in-javascript-examples/>

## 什么是高阶函数？

让我们看看名字，并考虑我们如何谈论事情。

我们深入细节，但有时我们希望对事物有一个高层次的看法。

这种高层次的观点表明了更多的抽象。我们深入细节，但我们提升到一个更抽象的观点。

高阶函数就是:比典型函数更高的抽象层次。

### 那么我们如何定义一个高阶函数呢？

高阶函数是对其他函数执行运算的函数。

在这个定义中，*操作*可能意味着接受一个或多个函数作为参数，或者返回一个函数作为结果。它不必两者都做。做其中的一件事，就可以把一个函数称为高阶函数。

## 让我们看一个高阶函数的例子

在没有高阶函数的情况下，如果我想给数组中的每个数字加 1 并在控制台中显示它，我可以执行以下操作:

```
const numbers = [1, 2, 3, 4, 5];

function addOne(array) {
  for (let i = 0; i < array.length; i++) {
    console.log(array[i] + 1);
  }
}

addOne(numbers); 
```

函数`addOne()`接受一个数组，给数组中的每个数字加 1，并在控制台中显示出来。数组中的原始值保持不变，但是函数会对每个值做一些处理。

然而，使用最常见的高阶函数`forEach()`，我们可以简化这一过程:

```
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((number) => console.log(number + 1)); 
```

**哇。**

我们已经将上面原始代码中的函数定义和调用抽象为一行代码！

我们将`forEach()`应用于名为“numbers”的数组在`forEach()`的开头有一个匿名函数，它接受数组的每个元素——一次一个。

对于名为 numbers 的数组，将数组的每个元素命名为“number”是有意义的，尽管我们也可以将其命名为“element”或“el”甚至“whatever”。

匿名箭头函数将数字+ 1 的值记录到控制台。

高阶函数`forEach()`对数组的每个元素应用一个函数。

## 另一个高阶函数的例子

如果没有更高阶的函数，如果我想创建一个新的数组，它只包含 numbers 数组中的奇数，我可以这样做:

```
const numbers = [1, 2, 3, 4, 5];

function isOdd(array, oddArr = []) {
  for (let i = 0; i < array.length; i++) {
    if (array[i] % 2 !== 0) {
      oddArr.push(array[i]);
    }
  }
  return oddArr;
}

const oddArray = isOdd(numbers);
console.log(oddArray); 
```

函数`isOdd()`接受一个数组，并为数组提供了第二个可选参数。如果没有提供，数组的默认值为空数组。

该函数检查数组中的每个数字，看它是否是奇数。如果数字是奇数，它从第二个参数将它添加到数组中。检查完所有数字后，返回第二个参数的数组。

所以，是的，有很多东西需要记录。

如果我们使用高阶函数`filter()`，我们可以抽象出这么多:

```
const numbers = [1, 2, 3, 4, 5];

const oddArray = numbers.filter((number) => number % 2 !== 0);
console.log(oddArray); 
```

**是的！**

原谅我激动，但这是一个很大的进步。

我们从定义新数组`oddArray`开始，因为应用`filter()`将创建一个新数组。高阶函数将返回满足匿名函数中设置的条件的每个元素。匿名函数再次应用于数字数组中的每个元素。

## 因为我们在滚动——另一个高阶函数的例子

我们已经走了这么远，我想你开始明白为什么高阶函数这么好了！

让我们看另一个例子...

回到我们的`forEach()`示例，我们给数组中的每个数字加 1，并将每个值记录到控制台。但是用这些新值创建一个新数组怎么样呢？如果没有高阶函数，我可以做以下事情:

```
const numbers = [1, 2, 3, 4, 5];

function addOneMore(array, newArr = []) {
  for (let i = 0; i < array.length; i++) {
    newArr.push(array[i] + 1);
  }
  return newArr;
}

const newArray = addOneMore(numbers);
console.log(newArray); 
```

函数`addOneMore()`再次接受一个数组，并将一个数组作为第二个参数，该参数的默认值为空。现有 numbers 数组的每个元素都加 1，并将结果推送到返回的新数组。

我们用高阶函数`map()`将它抽象出来:

```
const numbers = [1, 2, 3, 4, 5];

const newArray = numbers.map((number) => number + 1);
console.log(numbers); 
```

我们从定义 newArray 开始，因为`map()`创建了一个新数组。和`forEach()`一样，`map()`对数字数组的每个元素应用一个匿名函数。然而，`map()`在这个过程中创建了一个新的数组。

## 再举一个例子

如果我们想找到数字数组中所有值的总和呢？

如果没有高阶函数，我可以这样做:

```
const numbers = [1, 2, 3, 4, 5];

function getTotalValue(array) {
  let total = 0;
  for (let i = 0; i < array.length; i++) {
    total += array[i];
  }
  return total;
}

const totalValue = getTotalValue(numbers);
console.log(totalValue); 
```

函数`getTotalValue()`接受一个数组，将 total 变量定义为等于零，并在将每个元素添加到 total 变量时遍历数组。最后，它返回总数。

利用高阶函数`reduce()`，这个过程可以再次被抽象掉:

```
const numbers = [1, 2, 3, 4, 5];

const totalValue = numbers.reduce((sum, number) => sum + number);
console.log(totalValue); 
```

高阶函数`reduce()`需要匿名函数中的两个参数。

第一个参数是一个累加器，第二个参数是数字数组中的一个元素。

累加器参数(上例中的 sum)跟踪总数，因为`reduce()`将匿名函数应用于数组的每个元素。

## 结论

高阶函数为函数提供了更高层次的抽象。

它们有可能将您的 JavaScript 代码带到新的高度！

我将从我的 YouTube 频道给你们留下一个教程，它将高阶函数应用于 JSON 数据。

[https://www.youtube.com/embed/7BeT6lsudL4?feature=oembed](https://www.youtube.com/embed/7BeT6lsudL4?feature=oembed)

Higher Order Functions tutorial video