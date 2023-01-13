# 让我们探索 JavaScript 中的 Slice()、Splice() & Spread 语法(…)

> 原文：<https://www.freecodecamp.org/news/lets-explore-slice-splice-spread-syntax-in-javascript-e242a6f21e60/>

我遇到了这个 [freeCodeCamp](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/slice-and-splice) 的挑战，并陷入了一段时间思考如何找到解决它的方法。他们已经提到用切片&拼接解决。我当时很困惑什么时候用切片，什么时候用拼接。

在这里，我要分享一下我是如何用那些方法解决的。

切片和拼接都用于操作数组。让我们看看他们是怎么做的。

### 切片:

Slice 方法有两个参数。

**第一个参数**:指定选择应该从哪里开始。

例如:

```
var arr1 = [1,5,8,9];
arr1.slice(1); // [5,8,9]
```

从第一个索引(5)开始，它将返回元素。

**第二个参数**:指定端点应该在哪个级别。如果在调用 slice 方法时没有将它放在括号中，它将返回从起始索引到数组末尾的元素。

```
var arr1 = [1,5,8,9];
console.log(arr1.slice(1,3));
//[ 5, 8 ]
```

如果在调用时输入负数，将从数组末尾开始选择。

```
var arr1 = [1,5,8,9];
console.log(arr1.slice(-2));
//[ 8, 9 ]
```

**注意:Slice 总是从数组中返回选中的元素。**

**切片不会改变数组。阵列保持不变。参见下面的例子:**

```
var arr1 = [1,5,8,9];
arr1.slice(2);
console.log(arr1);
// [ 1, 5, 8, 9 ]
```

即使您对阵列进行了一些更改，也不会影响它。它将返回初始数组。

### **拼接:**

它可以接受多个参数。

这意味着，

**第一个参数**:指定应该在哪个位置添加/删除新元素或现有元素。如果值为负，将从数组末尾开始计算位置。

**第二个参数**:从起始位置移除的元素个数。如果为 0，则不会移除任何元素。如果没有通过，它将从起始位置删除所有元素。

```
var arr1 = [1,5,8,9];
console.log(arr1.splice(1,2));
// [ 5, 8 ]
```

**第三个参数- >第 n 个参数** ent:要添加到数组中的项目的值。

```
var arr1 = [1,5,8,9];
console.log(arr1.splice(1,2,'Hi','Medium'));
// [5,8]
```

您可能认为我们已经将“Hi”、“Medium”添加到数组中，但它没有显示在这里…对吗？

是的，我们已经安慰了 arr1.splice(1，2，' Hi '，' Medium ')。

**注:**

*   **Splice 将只返回从数组中删除的元素。**
*   **拼接会改变原来的数组**

```
var arr1 = [1,5,8,9];
arr1.splice(1,2,'Hi','Medium');
console.log(arr1);
// [ 1, 'Hi', 'Medium', 9 ]
```

### **传播语法:**

**定义**:允许在需要零个或多个参数(用于函数调用)或元素(用于数组文本)的地方扩展一个可迭代对象，例如数组表达式或字符串，或者在需要零个或多个键值对(用于对象文本)的地方扩展一个对象表达式。

让我们举个例子:

```
var arr1 = [1,3,6,7];
var arr2 = [5,arr1,8,9];
console.log(arr2);
// [ 5, [ 1, 3, 6, 7 ], 8, 9 ]
```

我希望它在一个单独的数组中，就像 **[ 5，1，3，6，7，8，9 ]。**

我可以用这个扩展语法来解决这个问题:

```
var arr1 = [1,3,6,7];
var arr2 = [5,...arr1,8,9];
console.log(arr2);
//[ 5, 1, 3, 6, 7, 8, 9 ]
```

Spread 语法的另一个主要用途是复制数组:

```
var arr = [1, 2, 3];
var arr2 = arr;
arr2.push(4);

console.log(arr2);
// [ 1, 2, 3, 4 ]

console.log(arr);
// [ 1, 2, 3, 4 ]
```

我只给 **arr2** 加了 **4** 。但它也对 arr 进行了更改。

我们可以使用 Spread 语法解决这个问题，如下所示...

```
var arr = [1, 2, 3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4);

console.log(arr2);
// [ 1, 2, 3, 4 ]

console.log(arr);
// [ 1, 2, 3]
```

你可以参考 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) 文档来获得更多关于 Spread 语法的信息。

**那么，让我们来看看挑战。**

```
function frankenSplice(arr1, arr2, n) {

// It's alive. It's alive!

let array2Copy = [...arr2];

array2Copy.splice(n,0, ...arr1);

             //console.log(array2Copy);

return array2Copy;

}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

该挑战的主要条件是“在函数执行后不应改变 arr1/arr2”。

于是，使用拼接 **方法**创建了 arr2、和**的**副本数组，将 arr1 添加到 arr2 的副本中，命名为 **array2Copy。******

#### **最终结论:**

->切片法将

*   从数组中返回选定的元素
*   接受两个参数
*   不改变原始数组

->拼接法将

*   返回数组中移除的元素
*   接受多个参数
*   改变原始数组

这是我的第一篇关于编码的教程——感谢阅读！你的反馈将帮助我提高我的编码和写作技能。

快乐编码…！

通过 [Twitter](https://twitter.com/parathantl) 与我联系