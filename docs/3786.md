# JavaScript Math.random()方法解释

> 原文：<https://www.freecodecamp.org/news/javascript-math-random-method-explained/>

## **随机方法**

JavaScript `Math.random()`方法是产生随机数的一个很好的内置方法。当执行`Math.random()`时，它返回一个随机数，可以是 0 到 1 之间的任意值。包含 0 并排除 1。

## 生成 0 到 1 之间的随机浮点数

`Math.random()`方法将返回一个大于或等于 0 且小于(但绝不等于)1 的浮点数(十进制)。换句话说`0 <= x < 1`。例如:

```
console.log(Math.random());
// 0.7069207248635578

console.log(Math.random());
// 0.765046694794209

console.log(Math.random());
// 0.14069121642698246
```

(当然，每次返回的数字都会不一样。以下所有示例都将假设这一点——每次通过都会产生不同的结果。)

要得到一个更大范围内的随机数，将`Math.random()`的结果乘以一个数。

## 生成 0 和指定最大值之间的随机浮点数

通常你不需要 0 到 1 之间的随机数——你需要更大的数，甚至整数。

例如，如果您想要一个 0 到 10 之间的随机浮点数，您可以使用:

```
var x = Math.random()*10;

console.log(x);
// 4.133793901445541
```

## 生成一个范围内的随机浮点数

如果您需要一个介于两个特定数字之间的随机浮点数，您可以这样做:

```
var min = 83.1;
var max = 193.36;

var x = Math.random()*(max - min)+min;

console.log(x);
// 126.94014012699063
```

## 生成介于 0 和最大值之间的随机整数

通常你需要整数。要做到这一点，你必须使用来自`Math`对象、`Math.floor()`(向下舍入到最近的整数)和`Math.ceil()`(向上舍入到最近的整数)的一些其他方法。

例如，如果您需要从一个包含 10 个元素的数组中随机选择，您将需要一个介于 0 和 9 之间的随机数(请记住数组是零索引的)。

```
var x = Math.floor(Math.random()*10);

console.log(x);
// 7
```

(记住`Math.random()`永远不会精确地返回 1，所以`Math.random()*10`永远不会精确地返回 10。这意味着向下舍入后，结果将始终是 9 或更小。)

## 生成介于 1 和最大值之间的随机整数

如果你需要一个最小为 1 的随机数(例如选择一月中的任意一天)，你可以使用`Math.ceil()`方法。

```
var x = Math.ceil(Math.random()*31);

console.log(x);
// 23
```

另一种方法是使用前面的函数(使用`Math.floor()`)并加 1:

```
var x = Math.floor(Math.random()*31)+1;

console.log(x);
// 17
```

## 生成一个范围内的随机整数

最后，偶尔你需要一个介于两个特定整数之间的随机整数。例如，如果您正在尝试挑选彩票，并且您知道最小和最大的数字:

```
var min = 1718;
var max = 3429;

var x = Math.floor(Math.random()*(max-min+1)+min);

console.log(x);
//2509
```

## Math.random()有多随机？

可以指出的是，由`Math.random()`返回的数字是伪随机数，因为没有计算机能够生成真正的随机数，其在所有规模和所有大小的数据集上表现出随机性。然而，由`Math.random()`生成的伪随机数通常足以满足您可能编写的几乎任何程序的需求。非真正的随机性只有在天文数字集合中或者需要非常精确的小数时才变得明显。