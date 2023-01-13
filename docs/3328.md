# JS For Loop 教程——如何在 JavaScript 中迭代数组

> 原文：<https://www.freecodecamp.org/news/javascript-loop-tutorial-how-to-iterate-over-an-array-in-javascript/>

这篇文章将为你提供如何在 JavaScript 中迭代数组数据结构的坚实理解。

无论您是刚开始学习 JavaScript，还是想复习一下，这两种方式对您都有价值。本文将带您了解最广泛使用的 JavaScript 概念之一。

### 什么是数组？

```
let cars = ["Tesla", "Ferrari", "Lamborghini", "Audi"];
```

上面是一个用来存储多个值的 JavaScript 数组。这是数组最简单的形式之一。它在数组中包含 4 个“元素”，都是字符串。如你所见，每个元素都用逗号分隔。

这个示例数组包含不同品牌的汽车，可以用`cars`变量来引用。

有很多方法可以迭代这个数组。JavaScript 的特性非常丰富，所以我们可以选择最好的方式来解决我们的问题。

下面是我们如何在 JavaScript 中处理数组迭代:

1.  突出显示迭代数组的 5 种常用方法
2.  展示对每个迭代方法的一些见解
3.  也提供一些您可以用来测试它的代码！

## 传统 For 循环

### 什么是 For 循环？

下面是循环的[的简单定义:](https://en.wikipedia.org/wiki/For_loop)

> “在计算机科学中， **for-loop** (或简称为 **for loop** )是一个[控制流](https://en.wikipedia.org/wiki/Control_flow) [语句](https://en.wikipedia.org/wiki/Statement_(programming))，用于指定[迭代](https://en.wikipedia.org/wiki/Iteration)、**，允许代码重复执行**[](https://en.wikipedia.org/wiki/Execution_(computers))****。”****

**for 循环是一种重复执行代码的方式。你可以把它放在一个 for 循环中，而不是敲入`console.log(“hi”)`五次。

在第一个例子中，我们将学习如何迭代上面看到的 cars 数组，并打印出每个元素。迭代器或计数器将从第一个索引“Tesla”开始，到最后一个“Audi”结束。它在数组中移动，一次打印一个元素。**

```
`let cars = ["Tesla", "Ferrari", "Lamborghini", "Audi"];

for(let i = 0; i < cars.length; i++) {
  console.log(cars[i]);
}`
```

****输出:****

```
`Tesla
Ferrari
Lamborghini
Audi`
```

**深入到代码中，我们将三个选项传递给 for 循环**

*   **迭代器变量- `let i = 0;`**
*   **迭代器应该停止的地方- `i < card.length`**
*   **迭代器每次循环要递增多少- `i++`**

**这个循环从`0`开始，每次循环增加一个变量，当我们到达数组的最后一个元素时停止。**

***传统 for 循环的主要优势在于您有更多的控制权。*

可以访问数组中的不同元素，或者以复杂的方式迭代数组来解决复杂的问题。例如，用传统的 For 循环可以很容易地跳过数组中的每隔一个元素。**

## **forEach 方法**

### **什么是 forEach 方法？**

**方法用来为数组的每个元素执行一个函数。如果数组的长度“未知”，或者肯定会改变，那么这种方法是一个很好的选择。此方法只能用于数组、集合和映射。**

```
`const amounts = [65, 44, 12, 4];
let doubledAmounts = [];

amounts.forEach(item => {
  doubledAmounts.push(item * 2);
})

console.log(doubledAmounts);`
```

**一个`forEach`循环的最大好处是能够访问每一项，即使你的数组可能会变大。对于许多用例来说，这是一个可扩展的解决方案，并且比传统的 for 循环更容易阅读和理解，因为我们知道我们将对每个元素迭代一次。**

## **While 循环**

### **什么是 While 循环？**

**以下是 While 循环的简单定义:**

> **“一个 **while 循环**是一个控制流语句，它允许基于给定的[布尔](https://en.wikipedia.org/wiki/Boolean_datatype)条件重复执行代码。 *while* 循环可以被认为是一个重复的 if 语句。**

****`while`循环是一种重复执行代码来检查条件是真还是假的方法。因此，我们可以使用 while 循环，而不是使用嵌套 if 语句的 for 循环。或者，如果我们不能找到数组的长度，while 循环是一个很好的选择。****

****while 循环通常由计数器控制。在下面的例子中，我们展示了一个迭代数组的基本 while 循环。关键是要控制正在创建的 while 循环。****

******While 循环示例(好):******

```
**`let i = 0 

while (i < 5) { 
  console.log(i); 
  i++; 
}`**
```

******输出**:****

```
**`0
1
2
3
4`**
```

****while 循环的 if 语句是`i < 5`，或者大声说出“I 小于 5”。变量`i`在每次循环运行时递增。****

****简单地说，这意味着每当循环执行一次完整的迭代时，变量`i`就加 1。在第一次迭代中，`i`等于 0，我们将“0”打印到控制台。****

****使用 while 循环的最大风险是编写一个永远不会满足的条件。****

****这在现实场景中经常发生，有人编写了一个 while 循环，但是忘记测试他们的循环，这就在代码库中引入了一个[无限循环](https://en.wikipedia.org/wiki/Infinite_loop)。****

****当条件永远不满足时，就会出现无限循环，循环会一直运行下去。这通常会导致破坏性的更改，然后导致整个软件应用程序中断并停止工作。****

****警告:不要运行这段代码。

**无限循环示例(坏):******

```
**`let i = 0 

while (i < 5) { 
  console.log(i); 
}`**
```

******输出:**

结果可能有所不同。****

## ****Do While 循环****

### ****什么是 do while 循环？****

****下面是 Do-While 循环的简单定义:****

> ****“一个 **do while 循环**是一个控制流语句，它至少执行一次代码块**，然后根据代码块末尾给定的布尔条件重复执行或不执行该代码块。”******

******do-while 循环与 while 循环几乎相同，但是有一个关键的区别。do-while 循环将保证在检查 if 语句之前总是至少执行一次代码块。******

******我经常把它想成一个反向 while 循环，几乎从不使用它们。然而，在一些非常特殊的情况下，它们确实会派上用场。******

********Do-Loop 示例(好):********

```
**`let i = 0; 

do { 
  console.log(i); 
  i++; 
} while (i < 5);`**
```

******输出**:****

```
**`0
1
2
3
4`**
```

****使用 do 循环的最大风险是写出一个永远不会满足的条件。(与 While 循环相同。)****

****警告:不要运行这段代码。

**无限循环示例(坏):******

```
**`let i = 0; 

do { 
  console.log(i); 
} while (i < 5);`**
```

******输出** :

结果可能会有所不同。****

****想让你的 JavaScript 知识更上一层楼吗？了解一下`map`，和`forEach`一样，但是有加成！！？****

## ****额外示例(带地图的迭代)****

### ****什么是地图？****

****以下是地图的简单定义:****

> ****“在许多[编程语言](https://en.wikipedia.org/wiki/Programming_language)，**映射**是一个[高阶函数](https://en.wikipedia.org/wiki/Higher-order_function)的名字，该函数将一个[给定函数](https://en.wikipedia.org/wiki/Procedural_parameter)应用于一个[仿函数](https://en.wikipedia.org/wiki/Functor_(disambiguation))的每个元素，例如一个[列表](https://en.wikipedia.org/wiki/List_(computing))，以相同的顺序返回一个结果列表。当在[功能形式](https://en.wikipedia.org/wiki/Functional_form)中考虑时，它通常被称为*适用于所有*****

****它是如何工作的？JavaScript 中的`map`函数将函数应用于数组*中的每个元素**。*请花点时间重读这句话。****

****然后，`map`函数返回一个新的数组，其结果是将一个函数应用于数组中的每个元素。****

******地图示例:******

```
**`const array = [1, 1, 1, 1];

// pass a function to map
const results = array.map(x => x * 2);

console.log(results);`**
```

******输出**:****

```
**`[2,2,2,2]`**
```

****我们已经将`map`函数应用于包含四个 1 的数组。然后`map`函数将每个元素乘以 2，即`x * 2`，并返回一个新数组。然后，新数组被存储在`results`变量中。****

****通过查看我们的输出，我们可以看到这是成功的。数组中的每个元素都被乘以 2。在某些情况下，这种方法可以替代循环，而且非常强大。****

## ****结论****

****干得好！您已经学习了在 JavaScript 中迭代数组的五种不同方法。这些是在 JavaScript 编程之旅中为您的成功奠定基础的基础。****

****您还接触到了一个高级概念`map`，它经常在大型软件应用程序中使用。****

****所以，我把这个留给你:你将如何在你的项目中使用数组？你觉得哪种迭代方法最令人兴奋？****

*******感谢阅读！*******

****如果你喜欢我的文章，请关注我和/或给我发消息！****

******[推特](https://twitter.com/MartyJacobsDev)[媒体](https://medium.com/@majikarpp)[Github](https://github.com/majikarp)******