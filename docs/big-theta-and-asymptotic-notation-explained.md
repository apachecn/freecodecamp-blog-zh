# 大θ和渐近符号解释

> 原文：<https://www.freecodecamp.org/news/big-theta-and-asymptotic-notation-explained/>

大ω告诉我们函数运行时的下界，大 O 告诉我们上界。

通常情况下，它们是不同的，我们不能保证运行时间——它会在两个界限和输入之间变化。但是当它们相同时会发生什么呢？然后我们可以给一个****θ****(θ)有界——我们的函数会在那个时间运行，不管我们给它什么输入。

一般来说，如果可能的话，我们总是希望给出一个θ界，因为这是最精确和最紧密的界。如果我们不能给出θ界，那么下一个最好的选择就是尽可能紧的 O 界。

例如，一个函数在数组中搜索值 0:

```
def containsZero(arr): #assume normal array of length n with no edge cases
  for num x in arr:
    if x == 0:
       return true
  return false
```

1.  最好的情况是什么？好吧，如果我们给它的数组第一个值是 0，那么它需要常数时间:ω(1)
2.  最坏的情况是什么？如果数组不包含 0，我们将遍历整个数组:O(n)

我们已经给了它一个ω和 O 的界限，那么θ呢？我们不能给它！根据我们给它的数组，运行时间将介于常数和线性之间。

让我们稍微修改一下代码。

```
def printNums(arr): #assume normal array of length n with no edge cases
  for num x in arr:
    print(x)
```

你能想到最好的情况和最坏的情况吗？？我不能！无论我们给它什么数组，我们都必须遍历数组中的每个值。所以这个函数至少需要 n 次(ω(n))，但我们也知道它不会比 n 次(O(n))更长。这是什么意思？我们的函数会取 ****恰好**** n 时间:θ(n)。

如果界限很混乱，可以这样想。我们有两个数，x 和 y，给定 x <= y，y <= x，如果 x 小于等于 y，y 小于等于 x，那么 x 必须等于 y！

如果你熟悉链表，测试一下你自己，想想这些函数的运行时间！

1.  得到
2.  去除
3.  增加

当你考虑一个双向链表时，事情变得更加有趣！

## **渐近符号**

我们如何衡量算法的性能值？

想想时间是我们最宝贵的资源之一。在计算中，我们可以用完成一个进程所花费的时间来衡量性能。如果两种算法处理的数据相同，我们可以决定解决问题的最佳实现。

我们通过定义算法的数学极限来做到这一点。这些是大 O，大ω和大θ，或者算法的渐近符号。对于任何给定的数据集，图中的 big-O 将是一个算法可能花费的最长时间，或者说是“上限”。Big-omega 就像 big-O 的反义词，“下界”。对于任何数据集，这是算法达到最高速度的地方。大 theta 要么是算法的精确性能值，要么是狭窄的上下限之间的有用范围。

一些例子:

*   “快递会在你有生之年送到。”(大 O，上限)
*   “我至少可以付给你一美元。”(大ω，下限)
*   今天最高气温 25 摄氏度，最低气温 19 摄氏度
*   "到海滩要走一公里。"(大θ，精确)

#### **更多信息:**

[https://www . khanacademy . org/computing/computer-science/algorithms/渐近-notation/a/big-big-theta-notation](https://www.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/big-big-theta-notation)[https://stack overflow . com/questions/10376740/what-exact-does-big-% D3 % A8-notation-represent](https://stackoverflow.com/questions/10376740/what-exactly-does-big-%D3%A8-notation-represent)[https://www . geeks forgeeks . org/analysis-of-algorithms-set-3a symptotic-notations/](https://www.geeksforgeeks.org/analysis-of-algorithms-set-3asymptotic-notations/)