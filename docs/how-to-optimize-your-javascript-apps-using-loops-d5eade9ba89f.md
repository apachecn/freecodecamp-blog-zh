# 如何使用循环优化你的 JavaScript 应用

> 原文：<https://www.freecodecamp.org/news/how-to-optimize-your-javascript-apps-using-loops-d5eade9ba89f/>

每个人都想要高性能的应用，所以在这篇文章中，我们将学习如何实现这个目标。

要提高 JavaScript 应用程序的性能，最简单也是最容易被忽略的一件事就是学习如何编写高性能的循环语句。这篇文章的想法是帮助解决这个问题。

> 我们将看到 JavaScript 中使用的主要循环类型，以及如何以一种高性能的方式编写它们。

我们开始吧！

### 环路性能

当谈到循环性能时，争论总是关于使用哪个循环。哪个速度最快，性能最好？事实是，在 JavaScript 提供的四种循环类型中，只有一种明显慢于其他类型——`for-in`循环。*回路类型的选择应基于您的需求，而不是性能考虑*。

对循环性能有贡献的有两个主要因素——每次迭代完成的**工作**和**迭代次数**。

在下面的章节中，我们将看到如何通过减少它们来对环路性能产生积极的整体影响。

### For 循环

第三版 ECMA-262(定义 JavaScript 基本语法和行为的规范)定义了四种类型的循环。第一个是标准的`for`循环，它与其他类 C 语言共享语法:

```
for (var i = 0; i < 10; i++){    //loop body}
```

这可能是最常用的 JavaScript 循环结构。为了理解我们如何优化它的工作，我们需要对它进行一点剖析。

#### 解剖

`for`循环由四部分组成:初始化、预测试条件、循环体和后执行。其工作方式如下:首先执行初始化代码(var I = 0；).然后是预测试条件(我<10；).如果预测试条件评估为`to t`true，则执行循环体。之后，运行后执行代码(i++)。

#### 最佳化

优化循环工作量的第一步是最小化对象成员和数组项查找的数量。

您还可以通过颠倒循环顺序来提高循环的性能。在 JavaScript 中，如果消除了额外的操作，反转循环确实会使循环的性能有所提高。

以上两种说法对其他两个更快的循环也有效(`while`和`do-while`)。

```
// original loop
for (var i = 0; i < items.length; i++){
    process(items[i]);
}
// minimizing property lookups
for (var i = 0, len = items.length; i < len; i++){
    process(items[i]);
}
// minimizing property lookups and reversing
for (var i = items.length; i--; ){
    process(items[i]);
}
```

### While 循环

第二种循环是`while`循环。这是一个简单的预测试循环，由预测试条件和循环体组成。

```
var i = 0;
while(i < 10){
    //loop body
    i++;
}
```

#### 解剖

如果预测试条件评估为`true`，则执行循环体。如果不是，则跳过它。每个`while`回路都可以用`for`代替，反之亦然。

#### 最佳化

```
// original loop
var j = 0;
while (j < items.length){
    process(items[j++]);
}
// minimizing property lookups
var j = 0,
    count = items.length;
while (j < count){
    process(items[j++]);
}
// minimizing property lookups and reversing
var j = items.length;
while (j--){
    process(items[j]);
}
```

#### Do-While 循环

`do-while`是第三种循环，也是 JavaScript 中唯一的测试后循环。它由车身回路和后测试条件组成:

```
var i = 0;
do {
    //loop body
} while (i++ < 10);
```

#### 解剖

在这种类型的循环中，循环体总是至少执行一次。然后评估后测试条件，如果是`true`，则执行另一个循环。

#### 最佳化

```
// original loop
var k = 0;
do {
    process(items[k++]);
} while (k < items.length);
// minimizing property lookups
var k = 0,
    num = items.length;
do {
    process(items[k++]);
} while (k < num);
// minimizing property lookups and reversing
var k = items.length - 1;
do {
    process(items[k]);
} while (k--);
```

### For-In 循环

第四种也是最后一种循环称为`for-in`循环。它有一个非常特殊的用途——**枚举任何 JavaScript 对象的命名属性。**看起来是这样的:

```
for (var prop in object){
    //loop body
}
```

#### 解剖

它类似于常规的*`for`循环，只是名字不同。它的工作方式完全不同。这种差异使得它比其他三个循环慢得多，其他三个循环具有相同的性能特征，因此试图确定哪个最快是没有用的。*

*每次执行循环时，变量`prop`都有另一个属性的名字，这是一个*字符串*，它将在`object.`上执行，直到所有属性都被返回。这些将是对象本身的属性，以及通过其原型链继承的属性。*

#### ***注释***

***你不应该使用`for-in”`来迭代数组**的成员。*

*这个循环的每次迭代都会导致在实例或原型上进行属性查找，这使得`for-in`循环比其他循环慢得多。对于相同的迭代次数，它可能比其他的慢七倍。*

### *结论*

*   *`for`、`while`和`do-while`环路都具有相似的性能特征，因此没有一种环路类型明显快于或慢于其他类型。*
*   *避免`for-in`循环，除非你需要迭代许多未知的对象属性。*
*   *提高循环性能的最好方法是**减少每次迭代完成的工作量，并减少循环迭代的次数**。*

*我希望这对你有用，就像对我一样！*

*感谢阅读。*

### *资源*

*"[高性能 JavaScript](https://www.amazon.com/High-Performance-JavaScript-Application-Interfaces/dp/059680279X) " —尼古拉斯·c·扎卡斯*

*在 [mihail-gaberov.eu](https://mihail-gaberov.eu/optimizing-javascript-apps-loops/) 阅读更多我的文章。*