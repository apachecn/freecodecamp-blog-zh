# 如何在不使用计数器的情况下计算字符串中 0 和 1 的个数

> 原文：<https://www.freecodecamp.org/news/how-to-count-no-of-0-and-1-in-a-string-without-using-counter/>

这个问题经常在面试中被问到，以测试候选人思考代码的方法。

### 问题陈述

你有一个只包含 0 和 1 的字符串，你需要写一个程序返回 0 和 1 的个数，并且你不允许以任何方式使用计数器变量。

看起来很棘手，对吧？

别担心，这很简单。我将教你两种方法:

*   使用计数器(经典方法)
*   不使用计数器(如面试问题中所要求的)

## 让我们编码

### 带计数器

```
function count(str) {
  var countForZero = 0;
  var countForOne = 0;

  for (var i = 0, length = str.length; i < length; i++) {
    if (str[i] === '0') {
      countForZero++;
    }
    else {
      countForOne++;
    }
  }

  return {
    'zero': countForZero,
    'One': countForOne
  };
} 
```

**上面代码的逻辑是:**

*   保留两个变量用于计算零和一。
*   循环遍历字符串中的字符，当发现一个零时，递增 countForZero。当找到一个 1 时，递增 countForOne。
*   在最后一部分，我们返回一个对象中的计数。

你可以使用这个链接试试上面的代码:[https://repl.it/repls/PurpleIdolizedInstructions](https://repl.it/repls/PurpleIdolizedInstructions)

现在，让我们看看没有计数器的解决方案。

### 没有计数器

```
function count(str) {
  var sum = 0;

  for (var i = 0, length = str.length; i < length; i++) {
    sum += Number(str[i]);
  }

  return {
    'zero': str.length - sum,
    'One': sum
  };
}
```

正如你在上面的代码中看到的，我没有使用任何计数器，而是使用了一个名为 sum 的变量。那么这里的概念是什么呢？

**概念是:**

> 包含 0 和 1 的字符串中的一个字符的和总是等于 1 的个数。

例如:0011001:0+0+1+1+0+0+1 = 3 = 1 的个数

> 零的数量=字符串的长度-1 的数量

**上面代码的逻辑是:**

*   保持 sum 变量用零值初始化。
*   遍历字符串中的字符，并对所有字符求和。
*   字符串中所有字符的总和将是 1 的数量，而零的数量将是(字符串的长度-1 的数量)。
*   在最后一部分，我们返回一个对象中的计数。

你可以使用这个链接试试上面的代码:[https://repl.it/repls/ComfortableOrdinaryConversions](https://repl.it/repls/ComfortableOrdinaryConversions)

我希望你现在能够明白如何回答这个问题，不管有没有计数器。

如果你有任何问题，欢迎在评论区提问。或者如果你有任何其他的窍门，也让我知道。