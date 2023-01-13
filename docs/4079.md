# 如何使用 JavaScript Math.floor 生成一个范围内的随机整数

> 原文：<https://www.freecodecamp.org/news/generate-random-whole-numbers-within-a-range-javascript/>

## 快速解

```
function randomRange(myMin, myMax) {
  return Math.floor(Math.random() * (myMax - myMin + 1) + myMin);
} 
```

## 代码解释

*   `Math.random()`生成 0 到≈ 0.9 之间的随机数。
*   在相乘之前，由于分组操作符`(   )`的原因，它会解析圆括号`(myMax - myMin + 1)`之间的部分。
*   乘法的结果是加上`myMin`，然后“四舍五入”到小于或等于它的最大整数(例如:9.9 将得到 9)

如果值为`myMin = 1, myMax= 10`，结果可能如下:

1.  `Math.random() = 0.8244326990411024`
2.  `(myMax - myMin + 1) = 10 - 1 + 1 -> 10`
3.  `a * b =  8.244326990411024`
4.  `c + myMin = 9.244326990411024`
5.  `Math.floor(9.244326990411024) = 9`

`randomRange`应该同时使用`myMax`和`myMin`，并返回一个你所在范围内的随机数。

如果你只是在你的`randomRange`公式中重用函数`ourRandomRange`，你就不能通过测试。您需要编写自己的公式，使用变量`myMax`和`myMin`。它将完成与使用`ourRandomRange`相同的工作，但是确保你已经理解了`Math.floor()`和`Math.random()`功能的原理。