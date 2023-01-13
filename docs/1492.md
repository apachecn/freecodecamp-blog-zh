# 如何在 JavaScript 中使用 Currying 和 Composition

> 原文：<https://www.freecodecamp.org/news/how-to-use-currying-and-composition-in-javascript/>

今天晚上的一次很棒的谈话让我思考并重温了一个我以前玩过的概念——奉承。但这一次，我愿意和大家一起探索！

curry 这个概念并不新鲜，但是很有用。它也是函数式编程的基础，是以更加模块化的方式思考函数的一种途径。

组合的想法，组合函数来创建更大、更复杂、更有用的函数，看起来很直观，但也是函数式编程中的一个关键组成部分。

当我们开始把它们结合起来，一些有趣的事情就会发生。让我们看看这是如何工作的。

## 有人要咖喱吗？

Curried 函数和其他函数做的差不多，但是你处理它们的方式有点不同。

假设我们想要一个函数来检查两点之间的距离:例如，`{x1, y1}`和`{x2, y2}`。这个公式有点复杂，但没什么我们不能处理的:

![distance-formula](img/3b0e7b5f15d50c304ebf2e1ad73be557.png)

The formula for distance between two points, which is an application of the Pythagorean theorem.

通常，调用我们的函数可能看起来像这样:

```
const distance = (start, end) => Math.sqrt( Math.pow(end.x-start.x, 2) + Math.pow(end.y-start.y, 2) );

console.log( distance( {x:2, y:2}, {x:11, y:8} );
// logs 10.816653826391969
```

现在，currying 一个函数是强迫它一次接受一个参数。因此，与其像这样称呼它，我们不如这样称呼它:`distance(start)(end)`。每个参数都被单独传入，每个函数调用都返回另一个函数，直到所有参数都被提供。

展示可能比解释容易，所以让我们看看上面的距离函数:

```
const distance = function(start){
  // we have a closed scope here, but we'll return a function that
  //  can access it - effectively creating a "closure".
  return function(end){
    // now, in this function, we have everything we need. we can do
    //  the calculation and return the result.
    return Math.sqrt( Math.pow(end.x-start.x, 2) + Math.pow(end.y-start.y, 2) );
  }
}

console.log( distance({x:2, y:2})({x:11, y:8});
// logs 10.816653826391969 again
```

要得到同样的结果，似乎要做大量的工作！我们*可以*通过使用 ES6 箭头函数将其缩短一些:

```
const distancewithCurrying = 
        (start) => 
          (end) => Math.sqrt( Math.pow(end.x-start.x, 2) +
                              Math.pow(end.y-start.y, 2) );
```

Indenting added for readability, doesn't affect the runnability

但是，除非我们开始以一种更抽象的方式来思考我们的功能，否则这似乎是一种没有实际收益的空谈。

记住，函数只能返回一个东西。虽然我们可能提供任意数量的参数，但我们将只返回一个值，无论它是数字、数组、对象还是函数。我们只能拿回一样东西。现在，有了一个 curried 函数，我们有了一个只能接收一个东西的函数。这其中可能有联系。

碰巧的是，curried 函数的强大之处在于能够组合和组成它们。

考虑一下我们的距离公式——如果我们正在编写一个“捕捉旗帜”游戏，并且它可能有助于快速轻松地计算每个玩家与旗帜的距离。我们可能有一个玩家数组，每个玩家都包含一个`{x, y}`位置。有了一组`{x,y}`值，一个可重用的函数会变得非常方便。让我们先考虑一下这个想法:

```
const players = [
  {
    name: 'Alice',
    color: 'aliceblue',
    position: { x: 3, y: 5}
  },{
    name: 'Benji',
    color: 'goldenrod',
    position: { x: -4, y: -4}
  },{
    name: 'Clarissa',
    color: 'firebrick',
    position: { x: -2, y: 8}
  }
];
const flag = { x:0, y:0}; 
```

这是我们的设置:我们有一个起始位置，`flag`，我们有一组玩家。我们定义了两个不同的函数来计算差异，让我们来看看差异:

```
// Given those, let's see if we can find a way to map 
//  out those distances! Let's do it first with the first
//  distance formula.
const distances = players.map( player => distance(flag, player.position) );
/***
 * distances == [
 *   5.830951894845301, 
 *   5.656854249492381, 
 *   8.246211251235321
 * ]
 ***/

// using a curried function, we can create a function that already
//  contains our starting point.
const distanceFromFlag = distanceWithCurrying(flag);
// Now, we can map over our players to extract their position, and
//  map again with that distance formula:
const curriedDistances = players.map(player=>player.position)
                                .map(distanceFromFlag)
/***
 * curriedDistances == [
 *   5.830951894845301, 
 *   5.656854249492381, 
 *   8.246211251235321
 * ]
 ***/
```

所以在这里，我们使用了我们的`distanceCurried`函数来应用一个参数，起点。这个函数返回了另一个参数，终点。通过映射玩家，我们可以用我们需要的数据创建一个新的数组，然后将数据传递给我们的 curried 函数！

这是一个强大的工具，可能需要一些时间来适应。但是通过创建 curried 函数并将它们与其他函数组合，我们可以从更小、更简单的部分创建一些非常复杂的函数。

## 如何编写定制函数

能够映射 curried 函数是非常有用的，但是您还会发现它们的其他重要用途。这是“函数式编程”的开始:编写小而纯的函数，作为这些原子位正确执行，然后像构建块一样组合它们。

让我们看看如何使用 curried 函数，并将它们组合成更大的函数。接下来的探索将涉及过滤函数。

首先，做一点基础工作。`Array.prototype.filter()`(ES6 过滤函数)允许我们定义一个回调函数，它接受一个或多个输入值，并基于该值返回 true 或 false。这里有一个例子:

```
// a source array,
const ages = [11, 14, 26, 9, 41, 24, 108];
// a filter function. Takes an input, and returns true/false from it.
function isEven(num){
  if(num%2===0){
    return true;
  } else {
    return false;
  }
}
// or, in ES6-style:
const isEven = (num) => num%2===0 ? true : false;
// and applied:
console.log( ages.filter(isEven) );
// [14, 26, 24, 108]
```

filtering an array of numbers for even values

现在，过滤函数`isEven`以一种非常特殊的方式编写:它接受一个值(或者多个值，如果我们想包含数组的索引)，执行某种内部 hoojinkery，并返回 true 或 false。每次都是。

这是“过滤器回调函数”的本质，尽管它不是过滤器独有的——`Array.prototype.every`和`Array.prototype.some`使用相同的风格。针对数组的每个成员测试回调，回调接受一些值并返回 true 或 false。

让我们创建几个更有用的过滤函数，但这次稍微高级一点。在这种情况下，我们可能希望将我们的功能“抽象”一点，让我们使它们更加可重用。

例如，一些有用的函数可能是`isEqualTo`或`isGreaterThan`。这些方法更先进，因为它们需要*两个*值:一个定义为比较的一项(称之为`comparator`，另一个来自数组*被比较的*(我们称之为`value`)。下面是更多的代码:

```
// we write a function that takes in a value...
function isEqualTo(comparator){
  // and that function *returns* a function that takes a second value
  return function(value){
    // and we simply compare those two values.
    return value === comparator;
  }
}
// again, in ES6:
const isEqualTo = (comparator) => (value) => value === comparator;
```

从这一点来看，我将坚持使用 ES6 版本，除非有特别具有挑战性的理由将代码扩展到经典版本。继续前进:

```
const isEqualTo = (comparator) => (value) => value === comparator;
const isGreaterThan = (comparator) => (value) => value > comparator;

// and in application:
const isSeven = isEqualTo(7);
const isOfLegalMajority = isGreaterThan(18); 
```

所以，前两个函数是我们的约定函数。它们需要单个参数，并返回一个同样需要单个参数的函数。

基于这两个单参数函数，我们做一个简单的比较。后两个函数`isSeven`和`isOfLegalMajority`，只是这两个函数的简单实现。

到目前为止，我们还没有变得复杂或介入，我们可以继续保持小规模:

```
// function to simply invert a value: true <=> false
const isNot = (value) => !value;

const isNotEqual = (comparator) => (value) => isNot( isEqual(comparator)(value) );
const isLessThanOrEqualTo = (comparator) => (value) => isNot( isGreaterThan(comparator)(value) );
```

这里，我们有一个效用函数，它简单地反转一个值`isNot`的*真值*。使用它，我们可以开始组成更大的片段:我们使用比较器和值，通过`isEqual`函数运行它们，然后我们`isNot`那个值来说`isNotEqual`。

这是构图的开始，平心而论——看起来绝对很傻。写这么多有什么用呢？

```
// all of the building blocks...
const isGreaterThan = (comparator) => (value) => value > comparator;
const isNot = (value) => !value;
const isLessThanOrEqualTo = (comparator) => (value) => isNot( isGreaterThan(comparator)(value) );

// simply to get this?
const isTooYoungToRetire = isLessThanOrEqualTo(65)

// and in implementation:
const ages = [31, 24, 86, 57, 67, 19, 93, 75, 63];
console.log(ages.filter(isTooYoungToRetire)

// is that any cleaner than:
console.log(ages.filter( num => num <= 65 ) )
```

“在这种情况下，最终结果非常相似，所以它并没有真正为我们节省任何东西。事实上，考虑到前三个函数的设置，比简单地做一个比较需要花费更多的时间来构建！”

这是真的。我不否认这点。但这只是一个更大拼图的一小部分。

*   首先，我们正在编写更加*自文档化*的代码。通过使用表达性函数名，我们一眼就能看出我们在过滤值`isTooYoungToRetire`的`ages`。我们看到的不是数学，而是描述。
*   第二，通过使用非常小的原子函数，我们能够孤立地测试每一部分，确保它每次都执行完全相同的功能。稍后，当我们重用这些小函数时，我们可以确信它们会工作——随着函数复杂性的增加，我们不用再测试每一个小部分。
*   第三，通过创建抽象函数，我们可能会在以后的其他项目中找到它们的应用。构建一个功能组件库是一项非常强大的资产，也是我强烈建议培养的一项资产。

综上所述，我们还可以将那些较小的功能组合成越来越大的功能。现在让我们试试:有了`isGreaterThan`和`isLessThan`，我们可以编写一个漂亮的`isInRange`函数！

```
const isInRange = (minComparator) 
                 => (maxComparator)
                   => (value) => isGreaterThan(minComparator)(value)
                              && isLessThan(maxComparator)(value)

const isTwentySomething = isInRange(19)(30);
```

这太棒了——我们现在有了一种一次性测试多种条件的方法。但是看看这个，它看起来不太自证。中间的那个并不可怕，但我们可以做得更好。

或许如果我们为*编写另一个*函数，我们可以称之为`and()`。`and`函数可以接受任意数量的条件，并根据给定的值测试它们。这将是有用的，可扩展的。

```
const and = (conditions) = 
             (value) => conditions.every(condition => condition(value) )

const isInRange = (min)
                 =>(max) 
                  => and([isGreaterThan(min), isLessThan(max) ])
```

因此`and`函数接受任意数量的过滤函数，并且只有当它们对于给定值都为真时才返回真。最后一个中的`isInRange`函数做的事情与前一个完全相同，但是它看起来可读性更好，并且是自文档化的。

此外，它将允许我们组合任意数量的函数:假设我们想要得到 20 到 40 之间的偶数，我们将简单地使用一个`and`将我们的`isEven`函数与我们的`isInRange`函数组合起来，这很简单。

## 概述

通过使用 curried 函数，我们能够干净地将函数组合在一起。我们可以将一个函数的输出直接连接到下一个函数的输入中，因为现在两个函数都接受单个参数。

通过使用组合，我们可以将较小的功能或简化的功能组合成更大、更复杂的结构，并确信最小的部分都按预期工作。

这需要消化很多东西，而且是一个很深的概念。但是如果你花时间深入研究，我想你会开始看到我们还没有接触到的应用程序，你可能会代替我写下一篇这样的文章！