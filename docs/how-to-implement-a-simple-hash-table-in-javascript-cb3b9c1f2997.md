# 如何用 JavaScript 实现一个简单的哈希表

> 原文：<https://www.freecodecamp.org/news/how-to-implement-a-simple-hash-table-in-javascript-cb3b9c1f2997/>

亚历克斯·纳达林

`{}`有多美？

它允许你通过键存储值，并以一种非常经济的方式检索它们。

在这篇文章中，我想实现一个非常基本的哈希表，看看它的内部工作原理，解释计算机科学中最巧妙的想法之一。

### 问题是

想象一下，你正在构建一种新的编程语言:你从非常简单的类型(字符串、整数、浮点等等)开始，然后继续实现非常基本的数据结构。首先是数组(`[]`)，然后是哈希表(也称为字典、关联数组、hashmap、map，等等)。

想知道它们是如何工作的吗？他们怎么这么快？

好吧，假设 JavaScript 没有`{}`或`new Map()`，让我们实现我们自己的`DumbMap`！

### 关于复杂性的一个注记

在我们开始之前，我们需要理解函数的复杂性是如何工作的:维基百科有一个关于计算复杂性的很好的复习，但是我将为那些懒惰的添加一个简短的解释。

复杂性衡量我们的函数需要多少步骤——步骤越少，执行越快(也称为“运行时间”)。

让我们来看看下面的片段:

```
function fn(n, m) {  return n * m}
```

`fn`的计算复杂度(从现在开始简称为“复杂度”)是`O(1)`，意味着它是常数(你可以把`O(1)`读作“*代价是一个*”):不管你传递什么参数，运行这段代码的平台只需要做一次运算(把`n`乘以`m`)。同样，因为是一次操作，所以成本称为`O(1)`。

复杂性是通过假设函数的参数可能有很大的值来衡量的。让我们看看这个例子:

```
function fn(n, m) {  let s = 0
```

```
 for (i = 0; i < 3; i++) {    s += n * m  }
```

```
 return s}
```

你会觉得它的复杂度是`O(3)`吧？

同样，由于复杂性是在非常大的参数环境中测量的，我们倾向于“丢弃”常量，并认为`O(3)`与`O(1)`相同。所以，即使在这种情况下，我们也会说`fn`的复杂度是`O(1)`。不管`n`和`m`的值是多少，你最终总是要做三个操作——这又是一个不变的成本(因此`O(1)`)。

这个例子有点不同:

```
function fn(n, m) {  let s = []
```

```
 for (i = 0; i < n; i++) {    s.push(m)  }
```

```
 return s}
```

如你所见，我们循环的次数和`n`的值一样多，可能有几百万次。在这种情况下，我们将这个函数的复杂度定义为`O(n)`，因为您将需要执行与您的一个参数的值一样多的操作。

其他例子？

```
function fn(n, m) {  let s = []
```

```
 for (i = 0; i < 2 * n; i++) {    s.push(m)  }
```

```
 return s}
```

这个例子循环了`2 * n`次，意味着复杂度应该是`O(2n)`。由于我们提到在计算函数的复杂度时常数被“忽略”，这个例子也被归类为`O(n)`。

再来一杯？

```
function fn(n, m) {  let s = []
```

```
 for (i = 0; i < n; i++) {    for (i = 0; i < n; i++) {      s.push(m)    }  }
```

```
 return s}
```

在这里，我们在`n`上循环，并在主循环中再次循环，这意味着复杂性是“平方”(`n * n`):如果`n`是 2，我们将运行`s.push(m)` 4 次，如果是 3，我们将运行它 9 次，以此类推。

在这种情况下，函数的复杂度称为`O(n²)`。

最后一个例子？

```
function fn(n, m) {  let s = []
```

```
 for (i = 0; i < n; i++) {    s.push(n)  }
```

```
 for (i = 0; i < m; i++) {    s.push(m)  }
```

```
 return s}
```

在这种情况下，我们没有嵌套循环，但是我们在两个不同的参数上循环两次:复杂度被定义为`O(n+m)`。非常清楚。

现在你已经对复杂性有了一个简单的介绍(或复习),很容易理解复杂性为`O(1)`的函数比复杂性为`O(n)`的函数执行得更好。

哈希表有一个`O(1)`复杂性:通俗地说，它们是**超快的**。我们继续吧。

*(我有点像躺在总有`O(1)`复杂性的散列表上，但只是继续读下去；)*

### 让我们建立一个(哑)散列表

我们的哈希表有两个简单的方法— `set(x, y)`和`get(x)`。让我们开始写一些代码:

让我们实现一个非常简单、低效的方法来存储这些键值对，并在以后检索它们。我们首先将它们存储在一个内部数组中(请记住，我们不能使用`{}`，因为我们正在实现`{}`——令人惊讶！):

然后，只需从列表中获取正确的元素:

我们的完整示例:

我们的`DumbMap` 太神奇了！它开箱即用，但是当我们添加大量的键值对时，它的性能如何呢？

让我们尝试一个简单的基准测试。我们将首先尝试在具有很少元素的散列表中找到一个不存在的元素，然后在具有大量元素的散列表中尝试同样的操作:

结果呢？不太令人鼓舞:

```
with very few records in the map: 0.118mswith lots of records in the map: 14.412ms
```

在我们的实现中，我们需要遍历`this.list`中的所有元素，以便找到一个具有匹配键的元素。成本是`O(n)`，相当可怕。

### 快点(呃)

我们需要找到一种方法来避免循环遍历我们的列表:是时候将*散列*放回*散列表*中了。

想知道为什么这种数据结构被称为**散列表**吗？这是因为散列函数被用在你设置和获取的密钥上。我们将使用这个函数将我们的键转换成一个整数`i`，并将我们的值存储在内部列表的索引`i`中。由于通过索引从列表中访问一个元素的开销是不变的(`O(1)`，那么哈希表的开销也是`O(1)`。

让我们试试这个:

这里我们使用的是[字符串散列](https://www.npmjs.com/package/string-hash)模块，它只是将一个字符串转换成一个数字散列。我们用它来存储和获取列表中索引`hash(key)`处的元素。结果呢？

```
with lots of records in the map: 0.013ms
```

这就是我所说的！

我们不必遍历列表中的所有元素，从`DumbMap`中检索元素速度超快！

让我尽可能直截了当地说明这一点:**散列使得散列表极其高效**。没有魔法。仅此而已。没有。只是一个简单，聪明，巧妙的想法。

### 选择正确散列函数的成本

当然，**挑选一个快速的哈希函数非常重要。**如果我们的`hash(key)`在几秒钟内运行，我们的函数不管有多复杂都会很慢。

与此同时，**确保我们的哈希函数不会产生大量的冲突**是非常重要的，因为它们会损害我们哈希表的复杂性。

迷茫？让我们仔细看看碰撞。

### 碰撞

你可能会想"*啊，一个好的散列函数永远不会产生冲突！*“:好吧，回到现实世界再想想。[谷歌能够为 SHA-1 哈希算法](https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html)产生冲突，在哈希函数破解并为两个不同的输入返回相同的哈希之前，这只是时间或计算能力的问题。始终假设您的哈希函数会产生冲突，并针对这种情况实施正确的防御措施。

举个例子，让我们试着使用一个产生大量碰撞的`hash()`函数:

这个函数使用一个由 10 个元素组成的数组来存储值，这意味着元素可能会被替换——这是我们的`DumbMap`中的一个令人讨厌的错误:

为了解决这个问题，我们可以简单地在同一个索引中存储多个键值对。因此，让我们修改我们的哈希表:

您可能会注意到，这里我们回到了最初的实现:存储一个键值对列表，并遍历每个键值对。当列表中某个特定的索引有很多冲突时，这个过程会很慢。

让我们使用自己的`hash()`函数进行基准测试，该函数生成从 1 到 10 的索引:

```
with lots of records in the map: 11.919ms
```

通过使用来自`string-hash`的散列函数，生成随机索引:

```
with lots of records in the map: 0.014ms
```

哇哦。选择正确的散列函数是有代价的——足够快，它本身不会降低我们的执行速度，足够好，它不会产生很多冲突。

### 一般为 O(1)

记得我的话吗？

> 哈希表有一种复杂性

好吧，我撒谎了:哈希表的复杂性取决于你选择的哈希函数。生成的碰撞越多，复杂性就越趋向于`O(n)`。

哈希函数，例如:

```
function hash(key) {  return 0}
```

意味着我们的哈希表的复杂度是`O(n)`。

这就是为什么，一般来说，计算复杂性有三个衡量标准:最好、一般和最坏的情况。哈希表在最好和一般情况下有一个`O(1)`复杂度，但是在最坏情况下降到了`O(n)`。

记住:**一个好的哈希函数是一个高效哈希表**的关键——不多也不少。

### 关于碰撞的更多信息…

我们用来解决`DumbMap`冲突的技术被称为[分离链接](https://xlinux.nist.gov/dads/HTML/separateChaining.html):我们将所有产生冲突的密钥对存储在一个列表中，并遍历它们。

另一种流行的技术是[开放式寻址](https://en.wikipedia.org/wiki/Open_addressing):

*   在列表的每个索引处，我们存储**一个并且只有一个键值对**
*   当试图在索引`x`处存储一个对时，如果已经有一个键-值对，尝试在`x + 1`处存储我们的新对
*   如果`x + 1`被占用，尝试`x + 2`以此类推…
*   当检索一个元素时，散列这个键并查看在那个位置(`x`)的元素是否匹配我们的键
*   如果没有，尝试访问位置`x + 1`的元素
*   清洗并重复，直到到达列表的末尾，或者当您发现一个空索引时——这意味着我们的元素不在哈希表中

聪明、简单、优雅并且[通常非常高效](http://cseweb.ucsd.edu/~kube/cls/100/Lectures/lec16/lec16-28.html)！

### FAQ(或者 TL；博士)

#### 哈希表会对我们存储的值进行哈希运算吗？

不，键被散列，以便它们可以被转换成整数`i`，并且键和值都被存储在列表中的位置`i`。

#### 哈希表使用的哈希函数会产生冲突吗？

当然——所以哈希表是用[防御策略](https://en.wikipedia.org/wiki/Hash_table#Collision_resolution)实现的，以避免讨厌的错误。

#### 哈希表内部使用列表还是链表？

看情况，[都可以](https://stackoverflow.com/questions/13595767/why-do-hash%20tables-use-a-linked-list-over-an-array-for-the-bucket)工作。在我们的例子中，我们使用 JavaScript 数组(`[]`)，它可以被[动态调整大小](https://www.quora.com/Do-arrays-in-JavaScript-grow-dynamically):

```
> a = []
```

```
> a[3] = 1
```

```
> a[ <3 empty items>, 1 ]
```

#### 为什么选择 JavaScript 作为例子？JS 数组是哈希表！

例如:

```
>  a = [][]
```

```
> a["some"] = "thing"'thing'
```

```
> a[ some: 'thing' ]
```

```
> typeof a'object'
```

我知道，该死的 JavaScript。

JavaScript 是“通用的”,可能是查看一些示例代码时最容易理解的语言。JS 可能不是最好的语言，但是我希望这些例子足够清楚。

#### 您的例子是哈希表的一个很好的实现吗？真的这么简单吗？

不，一点也不。

看看[马特·泽纳特](http://www.mattzeunert.com/)的《[用 JavaScript](http://www.mattzeunert.com/2017/02/01/implementing-a-hash-table-in-javascript.html) 实现散列表》，它会给你更多的上下文。还有很多东西需要学习，所以我也建议您查看:

*   [Paul Kube 的散列表课程](http://cseweb.ucsd.edu/~kube/cls/100/Lectures/lec16/lec16.html)
*   [用 Java 实现我们自己的散列表和独立链接](https://www.geeksforgeeks.org/implementing-our-own-hash-table-with-separate-chaining-in-java/)
*   [算法，第 4 版—哈希表](https://algs4.cs.princeton.edu/34hash/)
*   [设计快速哈希表](http://www.ilikebigbits.com/blog/2016/8/28/designing-a-fast-hash-table)

### 最后…

哈希表是我们经常使用的一个非常聪明的想法:无论你是在 Python 中创建一个[字典，在 PHP](https://stackoverflow.com/questions/114830/is-a-python-dictionary-an-example-of-a-hash-table) 中创建一个[关联数组，还是在 JavaScript](https://stackoverflow.com/a/3134315/934439) 中创建一个[地图。它们都有相同的概念，并且很好地让我们以(很可能)不变的成本通过标识符存储和检索元素。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

希望你喜欢这篇文章，并随时与我分享你的反馈。

特别感谢[乔](https://github.com/joejean)帮助我审阅了这篇文章。

再见！