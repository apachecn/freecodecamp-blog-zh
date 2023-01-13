# 如何在 JavaScript 中使用 map()、filter()和 reduce()

> 原文：<https://www.freecodecamp.org/news/map-filter-reduce-in-javascript/>

如果您想学习 React，首先对一些核心 JavaScript 概念有一个公平的理解是很重要的。

所以，如果这是你正在做的，首先，干得好！您做出了明智的决定，没有直接从 React 开始。

其次，React 建立在 map()、filter()和 reduce() JavaScript 方法等关键概念之上(毕竟——React 是一个 JavaScript 库)。所以这使得这些方法成为必须学习的。

Map、filter 和 reduce 是三种最有用、最强大的高阶数组方法。在本教程中，您将看到这些高阶数组方法是如何工作的。在类比和例子的帮助下，你还会学到你想在哪里使用它们以及如何使用它们。会很好玩的！

## 如何使用`map()`方法

假设你有一个数组`arrOne`,里面存储了一些数字，你想对每个数字进行一些计算。但是你也**不想弄乱原来的阵**。

这就是`map()`出现的原因。`map`方法将帮助您做到这一点:

```
let arrOne = [32, 45, 63, 36, 24, 11]
```

map()最多接受三个参数，即值/元素、索引和数组。

```
arrOne.map(value/element, index, array)
```

假设您想在不改变原始数组的情况下将每个元素乘以 5。

下面是实现这一点的代码:

```
let arrOne = [32, 45, 63, 36, 24, 11]
const multFive = (num) => {
return num * 5; //'num' here, is the value of the array.
}
let arrTwo = arrOne.map(multFive)
console.log(arrTwo)
```

这是输出结果:

```
[ 160, 225, 315, 180, 120, 55 ]
```

这是怎么回事？我有一个包含 6 个元素的`arrOne`数组。接下来，我用“num”作为参数初始化了一个箭头函数`multFive`。这将返回`num`和 5 的乘积，其中“num”变量由 map()方法提供数据。

如果您不熟悉箭头函数，但熟悉常规函数，则箭头函数如下所示:

```
function(num) 
	{  
    	return num * 5;
    }
```

然后，我初始化了另一个变量`arrTwo`，它将存储 map()方法创建的新数组。

在右边，我调用了“arrOne”数组上的 map()方法。因此，map()方法将从索引[0]开始选取该数组的每个值，并对每个值执行所需的计算。然后它会用计算出的值组成一个新的数组。

**重要的**:注意我是如何强调不要改变原始数组的。这是因为这个属性使得 map()方法不同于“forEach()”方法。map()方法生成一个新数组，而“forEach()”方法用计算出的数组改变原始数组。

## 如何使用`filter()`方法

这个名字有点暴露了，不是吗？您可以使用此方法根据您提供的条件过滤数组。filter()方法还创建了一个新数组。

**让我们以**为例:假设你有一个数组`arrName`，这个数组存储了一串数字。现在，您想看看哪些数字可以被 3 整除，并从中创建一个单独的数组。

下面是实现这一点的代码:

```
let arrNum = [15, 39, 20, 32, 30, 45, 22]
function divByFive(num) {
  return num % 3 == 0
}
let arrNewNum = arrNum.filter(divByFive)
console.log(arrNewNum)
```

这是输出结果:

```
[ 15, 39, 30, 45 ]
```

让我们来分解这个代码。这里，我有一个包含 7 个元素的数组`arrNum`。接下来，我用“num”作为参数初始化了一个函数`divByFive`。每次为比较传递新的 num 时，它返回 true 或 false，其中“num”变量由 filter()方法提供数据。

然后，我初始化了另一个变量`arrNewNum`，它将存储 filter()方法创建的新数组。

在右边，我调用了`arrNum`数组上的 filter()方法。因此，filter()方法将从索引[0]开始选取该数组的每个值，并对每个值执行操作。然后它会用计算出的值组成一个新的数组。

## 如何使用 reduce()方法

假设你被要求找出一个数组中所有元素的和。现在，您可以使用 for 循环或 forEach()方法，但是 reduce 是为这种任务而构建的。

`reduce()`方法通过对元素共同执行所需的操作，将数组缩减为单个值。

让我们以上面的例子为例，对它使用 reduce:

```
let arrNum = [15, 39, 20, 32, 30, 45, 22]
function sumOfEle(num, ind) {
  return num + ind;
}
let arrNum2 = arrNum.reduce(sumOfEle)
console.log(arrNum2)
```

以下是输出结果:

`203`

一切都与 map()和 filter()方法相同，但是理解 reduce 方法是如何工作的很重要。

reduce()方法没有明确的[语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce#syntax)。让我们来看看最简单的一个，它将为您提供使用 reduce()的所有方法的要点。

下面是一些`reduce()`的语法示例:

```
//Taking the above array for an example
let arrNum = [15, 39, 20, 32, 30, 45, 22]arr.reduce((a1, a2) => { 
 return a1 + a2
})
```

看一下这个语法。这里，reduce 采用两个参数，`a1`和`a2`，其中`a1`充当累加器，而`a2`具有索引值。

现在，在第一次运行时，累加器等于零，`a2`保存数组的第一个元素。reduce 所做的是将 a2 保存的值放入累加器中，并递增到下一个值。之后，reduce()方法对两个操作数执行运算。在这种情况下，它是加法。

因此，基本上`a1`是累加器，当前为 0，而`a2`为 15。第一次运行后，累加器中有 15，而`a2`保存下一个值 39。

所以，`0 + 15 = 15`

现在，在第二次运行时，reduce 在累加器中抛出`a2`的值 39，然后对两个操作数执行运算。

所以，`15 + 39 = 54`

现在，在第三次运行时，累加器的和为 15 和 39，即 54。`a2`现在保存 reduce 方法在累加器上抛出的 20，累加到`54 + 20 = 74`。

这个过程一直持续到数组的末尾。

## 签署

好了，就这样吧，各位！希望您现在对这些高阶数组方法的工作原理有了很好的了解。如果你读得很开心，觉得很有帮助，可以考虑分享一下。

查看我的最新故事[这里](https://medium.com/geekculture/5-reasons-why-you-should-invest-in-a-vpn-90e95e9524fe)，查看我的 Git 电子书[这里](https://bhaveshrawat.gumroad.com/l/lets-git-it-beginners-guide-to-git-bash-commands)。