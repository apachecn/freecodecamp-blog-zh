# JavaScript 数组切片与拼接:用 Cake 解释差异

> 原文：<https://www.freecodecamp.org/news/javascript-array-slice-vs-splice-whats-the-difference/>

这个标题可能是“如何不要混淆 JavaScript 的拼接和切片”，因为我从来不记得这两者之间的区别。

所以我希望这个技巧在未来对你我都有帮助:

```
S (p) lice = Slice + (p) => Slice + in (p) lace 
```

## Array.prototype.slice()

Array.prototype.slice()用于从`start`点到`end`点对数组进行切片，不包括`end`。

顾名思义，它用于从数组中分割元素。但与切蛋糕不同，切数组并不切实际的数组，而是保持不变(无限蛋糕！).

```
arr.slice(start, [end]) 
```

规则

1.  返回一个新数组，原始数组未被修改。
2.  如果省略`end`，end 将成为数组的结尾(最后一个元素)。
3.  如果`start`为-ve，则元素从末尾开始计数。

```
const infiniteCake = ['?','?','?','?','?','?']

let myPieceOfCake = infiniteCake.slice(0,1) // ['?']
let yourDoublePieceOfCake = infiniteCake.slice(0,2) // (2) ["?", "?"]
console.log(infiniteCake) //['?','?','?','?','?','?'] 
```

如你所见，`inifinteCake`是未修改的。

## Array.prototype.splice()

Splice 在适当的位置执行操作**，这意味着它修改了现有的数组。**

除了删除元素，拼接还用于添加元素。拼接是现实世界的蛋糕“切片”:

```
arr.splice(start, [deleteCount, itemToInsert1, itemToInsert2, ...]) 
```

规则

1.  手术就地进行。
2.  返回一个包含已删除项的数组。
3.  如果`start`为-ve，则元素从末尾开始计数。
4.  如果省略`deleteCount`,则删除数组末尾之前的元素。
5.  如果省略了要插入的项目，如`itemToInsert1`，则仅删除元素。

```
const cake = ['?','?','?','?','?','?'];
let myPieceOfCake = cake.splice(0, 1) // ["?"]
console.log(cake) // (5) ["?", "?", "?", "?", "?"]

let yourDoublePieceOfCake = cake.splice(0,2) //(2) ["?", "?"]
console.log(cake) //(3) ["?", "?", "?"] 
```

这里，`cake`被修改并缩小尺寸。

## 代码示例

```
const myArray  = [1,2,3,4,5,6,7] 

console.log(myArray.slice(0))       // [ 1, 2, 3, 4, 5, 6, 7 ]
console.log(myArray.slice(0, 1))    // [ 1 ]
console.log(myArray.slice(1))       // [ 2, 3, 4, 5, 6, 7 ]
console.log(myArray.slice(5))       // [ 6, 7 ]
console.log(myArray.slice(-1))      // [ 7 ]
console.log(myArray)                // [ 1, 2, 3, 4, 5, 6, 7 ]

const secondArray = [10, 20, 30, 40, 50]

console.log(secondArray.splice(0, 1))   // [ 10 ] : deletes 1 element starting at index 0
console.log(secondArray.splice(-2, 1))  // [ 40 ] : deletes 1 element starting at index end-2 
console.log(secondArray)                // [ 20, 30, 50 ]
console.log(secondArray.splice(0))      // [ 20, 30, 50 ] : deletes all elements starting at index 0
console.log(secondArray)                // []
console.log(secondArray.splice(2, 0, 60, 70)) // [] : deletes 0 elements starting at index 2 (doesn't exist so defaults to 0) and then inserts 60, 70
console.log(secondArray)                // [60, 70] 
```

## TL；速度三角形定位法(dead reckoning)

如果需要修改原始数组，或者需要添加元素，使用`splice`。

如果不应该修改原始数组，使用`slice`删除元素。

* * *

对我的更多教程和 JSBytes 感兴趣？注册订阅我的时事通讯。或[在推特上关注我](https://twitter.com/shrutikapoor08)