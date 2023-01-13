# JavaScript For Loop——如何在 JS 中遍历一个数组

> 原文：<https://www.freecodecamp.org/news/javascript-for-loop-how-to-loop-through-an-array-in-js/>

JavaScript 中有各种类型的循环，它们本质上都做同样的事情:一次又一次地重复一个动作。

如果您想将同一个代码块重复一定的次数，循环就派上了用场。基本上，它们是重复某事的快速有效的方法。

本文主要关注 JavaScript 编程语言中的`for`循环，并介绍它的基本语法。

此外，我将解释如何使用`for`循环遍历数组，这是一个基本的编程概念。

## 什么是 for 循环？基本语法分解

当特定条件为`true`时，`for`循环重复一个动作。当条件最终评估为`false`时，它停止重复动作。

JavaScript 中的`for`循环与 C 和 Java 中的`for`循环非常相似。

JavaScript 中有许多不同类型的`for`循环，但最基本的是这样的:

```
for( initialization of expression; condition; action for initialized expression ) {
  instruction statement to be executed;
} 
```

这种类型的循环以关键字`for`开始，后面是一组括号。在它们里面，有三个*可选的*表达式语句，用分号、`;`隔开。最后，有一组花括号`{}`，它包含了要执行的代码块语句。

这里有一个例子:

```
for (let i = 0; i < 10; i++) {
  console.log('Counting numbers');
  // runs and prints "Counting numbers" 10 times
  // values of i range from 0 to 9 
  } 
```

更详细地说，当执行`for`循环时:

*   如果有任何初始表达式，它将首先运行，并且在块中的任何代码运行之前和循环开始之前只执行一次。在这个步骤中，初始化一个或多个计数器，并声明循环中使用的一个或多个变量，如`let i =0`。如果有多个变量，则用逗号分隔。
*   接下来是循环运行必须满足的条件表达式的定义，`i < 10`。如前所述，代码块中的指令语句只有在这个条件评估为`true`时才会运行。如果值为`false`，循环停止运行。如果没有条件，总是`true`产生*无限循环*。
*   然后，执行带花括号的块中的指令语句`{..}`。可以有多个，在多行上。
*   每次执行完代码块后，可能会有一个针对初始化表达式的动作发生在最后，比如更新初始表达式的 increment 语句( `i++`)。
*   之后，再次检查条件，如果评估为真，则重复该过程。

## 什么是数组？

数组是一种数据结构。

它是多个项目(称为元素)的有序集合，这些项目以相同的名称存储在一个列表中。这样就可以很容易地对它们进行分类或搜索。

JavaScript 中的数组可以包含具有不同数据类型值的元素。一个数组可以同时包含数字、字符串、另一个数组、布尔值等等。

索引总是从`0`开始。这意味着数组中的第一项引用了零索引，第二项引用了一个索引，最后一项是`array length - 1`。

创建数组最简单也是最可取的方法是使用*数组文字符号*，你可以用方括号和逗号分隔的元素列表来创建数组，就像下面的字符串数组:

```
let programmingLanguages = ["JavaScript","Java","Python","Ruby"]; 
```

要访问第一个项目，我们使用索引号:

```
console.log(programmingLanguages[0]);
// prints JavaScript 
```

## 如何用 for 循环迭代数组

每次`for`循环运行时，它都有不同的值——数组也是如此。

for 循环以一种快速、有效和更可控的方式检查和迭代数组包含的每个元素。

循环遍历数组的一个基本示例是:

```
 const  myNumbersArray = [ 1,2,3,4,5];

for(let i = 0; i < myNumbersArray.length; i++) {
    console.log(myNumbersArray[i]);
} 
```

输出:

```
1
2
3
4
5 
```

这比单独打印每个值要有效得多:

```
console.log(myNumbersArray[0]);
console.log(myNumbersArray[1]);
console.log(myNumbersArray[2]);
console.log(myNumbersArray[3]);
console.log(myNumbersArray[4]); 
```

让我们来分解一下:

迭代器变量`i`被初始化为 0。`i`在这里指的是访问数组的索引。这意味着循环将在第一次运行时访问第一个数组值。

条件`i < myNumbersArray.length`告诉循环什么时候停止，增量语句`i++`告诉每次循环迭代器变量增加多少。

换句话说，循环从`0 index`开始，检查数组的长度，将值打印到屏幕上，然后将变量加 1。循环一次打印出一个数组的内容，当它到达长度时，它停止。

## 结论

本文介绍了如何开始使用 JavaScript 中的`for`循环的基础知识。我们学习了如何使用这种方法循环数组，这是你开始学习这门语言时最常用的方法之一。

如果你想了解其他 JavaScript 数组方法，你可以在这里阅读所有相关内容。

感谢阅读和快乐编码！