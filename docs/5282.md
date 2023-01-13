# array.prototype.map()的工作原理

> 原文：<https://www.freecodecamp.org/news/how-array-prototype-map-works-b6b69379c3af/>

作者帕拉德普·波希尼尼

# **array . prototype . map()如何工作**

JavaScript 现在是一种无处不在的语言。曾经仅限于客户端使用，现在你可以在服务器上找到它的许多版本。随着 JavaScript 的发展，用户可以使用的功能也越来越多。大多数时候，你满足于使用这些方法，很少会想采取额外的步骤来了解真正发生了什么。

关于这一点，今天让我们多走一步，探索一个非常流行的函数:`**Array.prototype.map()**` *。*

![1*sxqOoI2RvGq8n7MYwoWX-w](img/b708451212f2395f84a34f2b77c48cd0.png)

*免责声明*:我不会解释如何使用`**map()**`**——**下面的例子说明了这一点，或者你可以在谷歌上找到无数的例子。相反，让我们详细讨论一下 map 实际上是如何在幕后实现的。

`**map()**`方法创建一个新数组，其结果是在调用数组中的每个元素上调用一个提供的函数。

示例:

```
var array1 = [1, 4, 9, 16];
// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

#### **实施**

让我们直接从马嘴中挑选实现，并尝试对其进行剖析。下面是 MDN polyfill。花些时间理解代码，复制它并在你的机器上运行。如果您是初级/中级 JavaScript 开发人员，您肯定会遇到至少两个问题。

```
/*Array.prototype.map implementation*/
Array.prototype.map = function (callback/*, thisArg*/) {
    var T, A, k;
    if (this == null) {
        throw new TypeError('this is null or not defined');
    }
    var O = Object(this);
    var len = O.length >>> 0;
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    if (arguments.length > 1) { 
        T = arguments[1];
    }
    A = new Array(len);
    k = 0;
    while (k < len) {
        var kValue, mappedValue;
        if (k in O) {
            kValue = O[k];
            mappedValue = callback.call(T, kValue, k, O);            
            A[k] = mappedValue;
        }
        k++;
    }
    return A;
};
```

我强调了下面代码注释中可能出现的一些常见问题。

```
/*Array.prototype.map implementation*/
Array.prototype.map = function (callback/*, thisArg*/) {
    var T, A, k;
    if (this == null) {
        throw new TypeError('this is null or not defined');
    }
    var O = Object(this);
    var len = O.length >>> 0;// QUESTION 1 : What is the need for this line of code?
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    if (arguments.length > 1) { 
        T = arguments[1];
    }
    //  QUESTION 2 :What is the need for the if condition and why are we assiging T=arguments[1]?
    A = new Array(len);
    k = 0;
    while (k < len) {
        var kValue, mappedValue;
        if (k in O) {
            kValue = O[k];
            mappedValue = callback.call(T, kValue, k, O); 
            // QUESTION 3: why do we pass T,k and O when all you need is kvalue?
            A[k] = mappedValue;
        }
        k++;
    }
    return A;
};
```

让我们从底层开始逐一解决它们

**问题 3:当你需要的只是 kValue 时，为什么我们要传递 T，k 和 O？**

```
mappedValue = callback.call(T, kValue, k, O);
```

这是三个问题中最简单的一个，所以我选择了这个作为开始。在大多数情况下，将 **kValue** 传递给**回调**就足够了，但是:

*   如果有一个用例，您只需要对每隔一个元素执行一次操作，那该怎么办呢？嗯，你需要一个索引是 **(k)** 。
*   类似地，在其他用例中，您需要数组 **(O)** 本身在回调中可用。
*   为什么 **T** ？现在只知道 **T** 正在被传递以维护上下文。一旦你完成了问题 2，你会更好地理解这一点。

问题 2:if 条件的必要性是什么，为什么我们要指定 T=arguments[1]？

```
if (arguments.length > 1) {   T = arguments[1];    }
```

上述实现中的 map 函数有两个参数:回调函数**和可选的参数 **thisArg** *。Callback 是一个强制参数，而 thisArg 是可选的。***

通过提供第二个可选参数，可以在**回调**中传递应该是**“this”**的值。这就是为什么代码检查是否有多个参数，并将第二个可选参数赋给一个可以传递给回调的变量。

为了更好地说明，假设您有一个模拟需求，如果它能被 2 整除，您需要返回*数/2* ，如果它不能被 2 整除，您需要返回调用方的用户名。下面的代码说明了如何实现这一点:

```
const myObj = { user: "John Smith" }
var x = [10, 7];
let output = x.map(function (n) {
  if (n % 2 == 0) {
    return n / 2;
  } else {
    return this.user
  }
}, myObj) // myObj is the second optional argument arguments[1]

console.log(output); // [5,'John Smith']
//if you run the program without supplying myObj it would be //undefined as it cannot access myObj values
console.log(output); // [ 5, undefined ]
```

**问题 1:这行代码有什么用？**

```
var len = O.length >>> 0
```

这个我花了一些时间才弄明白。这行代码中有很多内容。在 JavaScript 中，您可以通过使用**调用*来调用方法，从而在函数中重新定义**“this”**。*** 你可以使用**绑定**或者**应用**来实现，但是对于这个讨论，我们还是坚持使用**调用。**

```
const anotherObject={length:{}} 
const myObj = { user: "John Smith" }
var x = [10, 7];
let output = x.map.call(anotherObject,function (n) {
  if (n % 2 == 0) {return n / 2;}
  else 
  {return this.user}
}, myObj)
```

当您使用**调用、**调用时，第一个参数将是 map 函数执行的上下文。通过发送参数，您用另一个对象的**“this”**覆盖了地图内的**“this”**。

如果观察的话，anotherObject 的 **length** 属性是一个空对象，不是整数。如果你只是使用 **O.length 而不是 O.length >** > > 0，会导致一个未定义的值。通过零移位，你实际上是把任何分数和非整数转换成一个整数。在这种情况下，结果将被强制为 0。

大多数用例不需要这种检查，但是可能有一种边缘情况需要处理这种场景。设计规范的优秀程序员确实考虑周全了！说到规范，实际上您可以在这里找到 Ecmascript 中每个函数必须如何实现的规范:

[**ECMAScript 语言规范- ECMA-262 版 5.1**](https://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.19)
[*本文档及其可能的翻译可能会被复制并提供给他人，以及评论…*](https://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.19)
[www.ecma-international.org](https://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.19)

规范(**步骤 3** )明确指出长度必须是 32 位无符号整数。这就是我们进行零填充移位以确保长度是一个整数的原因，因为 map 本身不要求 **this** 值是一个数组对象。

就是这样！

我要感谢几个人，我从未见过他们，但他们很友好，花时间(在互联网论坛上)帮助我理解一些细微差别。

萨拉蒂尔·杰尼斯，[乔丹·哈班德](https://twitter.com/ljharb)——谢谢！

注意:如果你陷入了不同的代码行，请随意在评论中提出，我会尽力澄清。

感谢您的时间和快乐的编码！