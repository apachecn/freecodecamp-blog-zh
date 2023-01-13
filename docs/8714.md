# Javascript 中 Reduce 方法的指南

> 原文：<https://www.freecodecamp.org/news/reduce-f47a7da511a9/>

乔希·皮扎利斯

# JavaScript 的 Reduce 方法是如何工作的，何时使用，以及它能做的一些很酷的事情

![WX-9gl6slkkXL3HYziycARUhKAH1hIdtY2CX](img/e4c720ba1e3c2f245410461ff79520bd.png)

Image credit to [Karthik Srinivas](https://thenounproject.com/term/minimize/592575/). Thank you [Karthik](https://twitter.com/aathis18).

JavaScript 的 **reduce 方法**是函数式编程的基石之一。让我们来探索一下它是如何工作的，什么时候应该使用它，以及它能做的一些很酷的事情。

#### 基本缩减

当时使用它:你有一个数量数组，你想把它们加起来。

```
const euros = [29.76, 41.85, 46.5];

const sum = euros.reduce((total, amount) => total + amount); 

sum // 118.11
```

如何使用它:

*   在本例中，Reduce 接受两个参数，即总额和当前金额。
*   reduce 方法遍历数组中的每个数字，就像在 for 循环中一样。
*   当循环开始时，总值是最左边的数字(29.76)，当前金额是它旁边的数字(41.85)。
*   在这个特殊的例子中，我们希望将当前金额添加到总额中。
*   对数组中的每个数量重复计算，但每次当前值都变为数组中的下一个数字，并向右移动。
*   当数组中没有更多的数字时，该方法返回总值。

#### **JavaScript 中 Reduce 方法的 ES5 版本**

如果您以前从未使用过 ES6 语法，不要被上面的例子吓倒。和写字一模一样:

```
var euros = [29.76, 41.85, 46.5]; 

var sum = euros.reduce( function(total, amount){
  return total + amount
});

sum // 118.11
```

我们使用`const`代替`var`，并且在参数后面用一个“粗箭头”(`=>`)代替单词`function`，我们省略了单词“返回”。

我将在其余的例子中使用 ES6 语法，因为它更简洁，出错的可能性更小。

#### 用 JavaScript 中的 Reduce 方法求平均值

在返回最终值之前，您可以将总和除以数组的长度，而不是记录总和。

实现这一点的方法是利用 reduce 方法中的其他参数。第一个参数是索引。与 for 循环非常相似，索引指的是缩减器在数组上循环的次数。最后一个论点是*阵*本身。

```
const euros = [29.76, 41.85, 46.5];

const average = euros.reduce((total, amount, index, array) => {
  total += amount;
  if( index === array.length-1) { 
    return total/array.length;
  }else { 
    return total;
  }
});

average // 39.37
```

#### 作为缩减进行映射和过滤

如果你可以用 reduce 函数计算出一个平均值，那么你可以用任何你想用的方法。

例如，您可以将总数增加一倍，或者将每个数字减半，然后将它们加在一起，或者在 reducer 中使用 if 语句只将大于 10 的数字相加。我的观点是，JavaScript 中的 *Reduce 方法为您提供了一个迷你代码笔，您可以在其中编写任何您想要的逻辑。*它*将对数组中的每个数量重复该逻辑，然后返回一个值。*

问题是，你不必总是返回一个值。您可以将一个数组缩减为一个新数组。

例如，让我们将一个数组中的数量减少到另一个数组中，其中每个数量都加倍。为此，我们需要将累加器的初始值设置为一个空数组。

初始值是开始减少时总参数的值。您可以通过在括号内添加一个逗号，后跟您的初始值来设置初始值，但要在花括号之后(在下面的示例中，中的**为粗体)。**

```
const average = euros.reduce((total, amount, index, array) => {
  total += amount
  return total/array.length
}, 0);
```

在前面的例子中，初始值是零，所以我省略了它。通过省略初始值，*总计*将默认为数组中的第一个数量。

通过将初始值设置为空数组，我们可以将每个*数量*推入*总数*。如果我们想将一个数组中的值减少到另一个数组中，其中每个值都加倍，我们需要将*的数量* * 2。然后，当没有更多的数据需要推送时，我们返回总数。

```
const euros = [29.76, 41.85, 46.5];

const doubled = euros.reduce((total, amount) => {
  total.push(amount * 2);
  return total;
}, []);

doubled // [59.52, 83.7, 93]
```

我们已经创建了一个新的数组，其中每一个数量都加倍。我们还可以通过在 reducer 中添加一个 if 语句来过滤掉不想加倍的数字。

```
const euro = [29.76, 41.85, 46.5];

const above30 = euro.reduce((total, amount) => {
  if (amount > 30) {
    total.push(amount);
  }
  return total;
}, []);

above30 // [ 41.85, 46.5 ]
```

这些操作是被重写为 reduce 方法的*映射*和*过滤器*方法。

对于这些示例，使用贴图或过滤器更有意义，因为它们更容易使用。当您想要一起映射和过滤并且有大量数据要查看时，使用 reduce 的好处就显现出来了。

如果你把贴图和过滤链接在一起，你就做了两次工作。您过滤每个值，然后映射其余的值。使用 reduce，您可以在一次操作中进行过滤，然后进行贴图。

使用 map 和 filter，但是当你开始将许多方法链接在一起时，你现在知道减少数据会更快。

#### 用 JavaScript 中的 Reduce 方法创建计数

**用在**的时候:你有一个物品集合，你想知道集合里每个物品有多少。

```
const fruitBasket = ['banana', 'cherry', 'orange', 'apple', 'cherry', 'orange', 'apple', 'banana', 'cherry', 'orange', 'fig' ];

const count = fruitBasket.reduce( (tally, fruit) => {
  tally[fruit] = (tally[fruit] || 0) + 1 ;
  return tally;
} , {})

count // { banana: 2, cherry: 3, orange: 3, apple: 2, fig: 1 }
```

要计算数组中的项目，我们的初始值必须是一个空对象，而不是像上一个例子中那样的空数组。

因为我们将返回一个对象，所以我们现在可以在总数中存储键值对。

```
fruitBasket.reduce( (tally, fruit) => {
  tally[fruit] = 1;
  return tally;
}, {})
```

在我们的第一次传递中，我们希望第一个键的名称是我们的当前值，我们希望给它一个值 1。

这给了我们一个对象，所有的水果都作为键，每个值为 1。如果它们重复，我们希望每种水果的数量增加。

为此，在我们的第二个循环中，我们检查我们的 total 是否包含一个带有 reducer 当前结果的键。如果没有，我们就创建它。如果是的话，我们就把数量加 1。

```
fruitBasket.reduce((tally, fruit) => {
  if (!tally[fruit]) {
    tally[fruit] = 1;
  } else {
    tally[fruit] = tally[fruit] + 1;
  }
  return tally;
}, {});
```

我用更简洁的方式重写了完全相同的逻辑。

#### 用 JavaScript 中的 Reduce 方法展平数组中的数组

我们可以使用 reduce 将嵌套的数量展平到一个数组中。

我们将初始值设置为一个空数组，然后将当前值连接到总数。

```
const data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

const flat = data.reduce((total, amount) => {
  return total.concat(amount);
}, []);

flat // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

通常情况下，信息以更复杂的方式嵌套。例如，假设我们只需要下面数据变量中的所有颜色。

```
const data = [
  {a: 'happy', b: 'robin', c: ['blue','green']}, 
  {a: 'tired', b: 'panther', c: ['green','black','orange','blue']}, 
  {a: 'sad', b: 'goldfish', c: ['green','red']}
];
```

我们将遍历每个对象并提取颜色。为此，我们为数组中的每个对象指向 amount.c。然后，我们使用 forEach 循环将嵌套数组中的每个值推入总数组中。

```
const colors = data.reduce((total, amount) => {
  amount.c.forEach( color => {
      total.push(color);
  })
  return total;
}, [])

colors //['blue','green','green','black','orange','blue','green','red']
```

如果我们只需要唯一的数字，那么在推送之前，我们可以检查该数字是否已经存在于 *total* 中。

```
const uniqueColors = data.reduce((total, amount) => {
  amount.c.forEach( color => {
    if (total.indexOf(color) === -1){
     total.push(color);
    }
  });
  return total;
}, []);

uniqueColors // [ 'blue', 'red', 'green', 'black', 'orange']
```

#### 带变径的管道

JavaScript 中 reduce 方法的一个有趣的方面是，除了数字和字符串，您还可以对函数进行 reduce。

假设我们有一组简单的数学函数。这些功能允许我们增加、减少、加倍和减半一个数量。

```
function increment(input) { return input + 1;}

function decrement(input) { return input — 1; }

function double(input) { return input * 2; }

function halve(input) { return input / 2; }
```

不管出于什么原因，我们需要增加，然后增加一倍，然后减少一个数量。

您可以编写一个函数，它接受一个输入，并返回(input + 1) * 2 -1。问题是我们知道我们将需要增加三次，然后增加一倍，然后减少，然后在未来的某个时间减半。我们不想每次都重写我们的函数，所以我们将使用 reduce 来创建一个管道。

管道是一个术语，指的是将某个初始值转换为最终值的一系列函数。我们的管道将按照我们希望使用它们的顺序由我们的三个函数组成。

```
let pipeline = [increment, double, decrement];
```

我们不是减少一个数组的值，而是通过我们的函数管道来减少。这是可行的，因为我们将初始值设置为要转换的量。

```
const result = pipeline.reduce(function(total, func) {
  return func(total);
}, 1);

result // 3
```

因为管道是一个数组，所以很容易修改。如果我们想减少某个东西三次，然后增加一倍，减少一倍，减半一倍，那么我们只需要改变流水线。

```
var pipeline = [

  increment,

  increment,

  increment,

  double,

  decrement,

  halve

];
```

reduce 函数保持不变。

#### 要避免的愚蠢错误

如果没有传入初始值，reduce 将假设数组中的第一项是初始值。这在前几个例子中工作得很好，因为我们正在添加一个数字列表。

如果你想计算水果的总数，而你忽略了初始值，那么事情就变得很奇怪。不输入初始值是一个容易犯的错误，也是调试时应该首先检查的事情之一。

另一个常见的错误是忘记返回总数。要使 reduce 函数工作，必须返回一些内容。总是要仔细检查，确保你确实返回了你想要的值。

**工具，提示&参考文献**

*   这篇文章中的所有内容都来自 egghead 上一个名为[介绍 Reduce](https://egghead.io/courses/reduce-data-with-javascript) 的精彩视频系列。我完全信任 [Mykola Bilokonsky](https://twitter.com/mykola) ，我感谢他让我知道了在 JavaScript 中使用 Reduce 方法的一切。我试图用我自己的话重写他解释的大部分内容，作为更好地理解每个概念的练习。此外，当我需要记住如何做某事时，参考一篇文章比视频更容易。
*   [MDN Reduce 文档](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)标注了我所谓的*总计*的`accumulator`。知道这一点很重要，因为如果你在网上读到它，大多数人会把它称为累加器。有些人称之为`prev`如*前一个值*。都是指同一件事。当我学习 reduce 的时候，我发现想出一个总数更容易。
*   如果你想练习使用 reduce，我建议你注册 [freeCodeCamp](https://www.freecodecamp.com/) 并使用 reduce 完成尽可能多的[中间算法](https://www.freecodecamp.com/map-aside#nested-collapseIntermediateAlgorithmScripting)。
*   如果示例片段中的“const”变量对您来说是新的，我写了另一篇关于 [ES6 变量以及为什么您可能想要使用它们](https://medium.com/@joshpitzalis/es6-variables-and-why-you-might-want-to-use-them-fbc84ce27262#.981ejmn1f)的文章。
*   我还写了一篇名为[循环的麻烦](https://medium.com/@joshpitzalis/the-trouble-with-loops-f639e3cc52d9#.8xkmhn7z6)的文章，解释了如何使用 map()和 filter()，如果你对它们不熟悉的话。

感谢阅读！如果你想在我写新文章时得到通知，请在这里输入你的电子邮件。

如果你喜欢这篇文章，请在社交媒体上分享，以便其他人可以找到它。