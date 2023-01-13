# 如何使用厄拉多塞算法的筛子

> 原文：<https://www.freecodecamp.org/news/sieve-of-eratosthenes-algorithm/>

有一天，在学习 JavaScript 的算法时，我发现了这个挑战:

> 使用一个`for`循环，从 0 到 100 迭代，并返回该范围内所有素数的数组。

起初看起来很容易，但我不太明白。所以我用谷歌搜索了一下，发现了一个完美的算法:厄拉多塞的**筛子。**

## 你说的这个*筛子*是什么？

厄拉多塞的筛子是由昔兰尼的厄拉多塞创造的一种古老的数学算法。它查找 0 和给定极限之间的所有质数。

## 有意思！厄拉多塞的筛子是如何工作的？

让我们来分解一下:

*   我们的输入是一个代表极限的正数。
*   该算法遍历从 0 到我们输入的所有数字。
*   在每次迭代中，如果这个数是质数，它会将这个数的所有倍数标记为非质数。

酷吧？！现在让我们来解决我们最初的挑战:

```
function getPrimes(input) {
  // Create an array where each element starts as true
  const numsArr = Array.from({ length: input + 1 }, () => true);

  // Create an array to store the prime numbers
  const primeNumbers = [];

  /*
  Loop through numsArr starting from numsArr[2]
  because 0 and 1 are definitely not prime numbers
  */
  for (let i = 2; i <= input; i++) {
    // Check if numsArr[i] === true
    if (numsArr[i]) {
      // add the i to the primeNumbers array
      primeNumbers.push(i);

      /* 
      convert all elements in the numsArr 
      whose indexes are multiples of i 
      to false
      */
      for (let j = i + i; j <= input; j += i) {
        numsArr[j] = false;
      }
    }
  }

  return primeNumbers;
}

console.log(getPrimes(100)); 
```

在上面的代码中，我们执行了以下操作:

*   创建了一个包含`true`个元素的数组。JavaScript 数组是零索引的，所以我们设置了`length: input + 1`来利用这一点。
*   创建了`primeNumbers[]`来存储质数。
*   使用了一个`for`循环来迭代`numsArr[]`中的每个元素。如果当前元素是`true`，则将其加到`primeNumbers[]`，并将所有其索引倍数的元素转换为`false`。
*   返回了`primeNumbers[]`，现在包含了所有带 0 的质数和我们的输入。

所以这是可行的，但是有一个小问题(或者一个大问题，取决于输入大小)。在循环的某个时刻，数组中所有可能的非素数都已经是`false`，但是到达一个`true`元素仍然会触发它的嵌套循环。那是多余的！

让我们优化:

```
// Sieve of Eratosthenes Algorithm

function getPrimes(input) {
  // Create an array where each element starts as true
  const numsArr = Array.from({ length: input + 1 }, () => true);

  // Loop through numsArr starting from numsArr[2]
  // keep running the loop until i is greater than the input's square root
  for (let i = 2; i <= Math.floor(Math.sqrt(input)); i++) {
    // Check if numsArr[i] === true
    if (numsArr[i]) {
      /* 
      convert all elements in the numsArr 
      whose indexes are multiples of i 
      to false
      */
      for (let j = i + i; j <= input; j += i) {
        numsArr[j] = false;
      }
    }
  }

  /*
  Using Array.prototype.reduce() method:
    loop through each element in numsArr[]
      if element === true, 
      add the index of that element to result[]
      return result
  */
  const primeNumbers = numsArr.reduce(
    (result, element, index) =>
      element ? (result.push(index), result) : result,
    []
  );

  // Return primeNumbers[]
  return primeNumbers;
}

console.log(getPrimes(100)); 
```

上面的代码中发生了什么？

从数学上来说，不可能得到超过任何给定输入的平方根的任何新倍数。

简单地说，当我们得到`input`的平方根时，`numsArr[]`中所有可能的倍数都已经被转换为`false`，所以没有必要继续检查倍数。

这就是我们所做的:

*   当`i <= Math.floor(Math.sqrt(input))`为假时，更新`for`循环结束。
*   使用 JavaScript 的`reduce()`方法遍历`numsArr[]`并返回一个包含所有`true`元素的`index`的数组。

**有趣的事实:**如果我们将第一个`for`循环替换为:

```
// keep running the loop until input is less than i^2 (i squared)
for (let i = 2; i * i <= input; i++) {
  // same super-awesome code hehehe!
} 
```

试试看！

## 不错！厄拉多塞的筛子有什么局限性吗？👀

厄拉多塞的筛子在投入很少的情况下工作效率很高-`n < 10 million`(*1000 万算小吗？？？*)。然而，较大的输入需要大量的时间和内存。[分段筛](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#:~:text=usually%20the%20case.-,Segmented%20sieve,-%5Bedit%5D)就是针对这个问题提出的解决方案。

## 几句临别赠言

这种算法有不同的版本，每个版本都解决了最初版本的一些限制。

学习这个算法拓宽了我对嵌套循环、质数和时间复杂性的知识。要深入探讨这些主题，请查看下面的参考资料。