# 我最喜欢的用 JavaScript 写管道和作曲的方法

> 原文：<https://www.freecodecamp.org/news/10-ways-to-write-pipe-compose-in-javascript-f6d54c575616/>

`compose`，尤其是`pipe`，是我最喜欢的功能。

这篇文章只是为了娱乐和探索这两种宝石的不同实现。我建议你在阅读这篇文章之前先了解他们是做什么的；或许在这里查看一下我的深度探索。

```
pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x); 
```

经典。

从最左边的函数开始，通过用前一个函数的输出调用下一个函数，将函数数组缩减为一个值。

```
double = (x) => x * 2;
add1 = (x) => x + 1;

pipe(
  double,
  add1
)(100); // 201 
```

我通过 [Eric Elliott](https://medium.com/@_ericelliott) 发现了这个实现，并在这里写了一篇关于它的深度文章[。](https://medium.com/front-end-hacking/pipe-and-compose-in-javascript-5b04004ac937)

使用`reduceRight`实现`compose`。现在你的函数从右向左被调用。

```
compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);

compose(
  double,
  add1
)(100);
// 202 
```

您也可以反转`fns`并继续使用`reduce`(性能较差)。

```
compose = (...fns) => (x) => fns.reverse().reduce((v, f) => f(v), x);

compose(
  double,
  add1
)(100); // 202 
```

虽然改变了数组，但你可能会先复制它(这样性能会更差)。

```
compose = (...fns) => (x) => [...fns].reverse().reduce((v, f) => f(v), x);

compose(
  double,
  add1
)(100); // 202 
```

使用`reduceRight`返回到`pipe`。

```
pipe = (...fns) => (x) => [...fns].reverse().reduceRight((v, f) => f(v), x);

pipe(
  double,
  add1
)(100); // 201 
```

### 但是它们都是一元的

顺便说一下，以上所有片段都是*一元*。每个函数可能只接受*一个参数*。

如果您的管道的第一个函数必须是*无*(接受`n`参数)，请尝试以下实现:

```
multiply = (x, y) => x * y;
pipe = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));

pipe(
  multiply,
  add1
)(10, 10); // 101
// Takes multiple args now 
```

这个片段来自 30secondsofcode.org。您的第一个(最左边的)函数可能接受`n`参数——所有其他函数必须是一元的。

同样，`reduceRight`给了我们`compose`。现在你最右边的函数可以接受`n`参数。让我们把`multiply`移到链条的末端。

```
compose = (...fns) => fns.reduceRight((f, g) => (...args) => g(f(...args)));

compose(
  add1,
  multiply
)(10, 10); // 101
// Takes multiple args now
// Put multiply first 
```

像以前一样，您可以反转`fns`数组并继续使用`reduce`:

```
compose = (...fns) =>
  [...fns].reverse().reduce((f, g) => (...args) => g(f(...args)));

compose(
  add1,
  multiply
)(10, 10); // 101 
```

如果你想保持`reduce`而没有轻微的性能损失，只需切换`g`和`f`:

```
compose = (...fns) => fns.reduce((f, g) => (...args) => f(g(...args)));

compose(
  add1,
  multiply
)(10, 10); // 101 
```

并使用`reduceRight`切换回`pipe`。

```
pipe = (...fns) => fns.reduceRight((f, g) => (...args) => f(g(...args)));

pipe(
  multiply,
  add1
)(10, 10); // 101
// put multiply first now 
```

### 结论

唷！有很多方法来吹奏和作曲！

它只是证明，无论如何，你必须循环一个函数数组，用前一个函数的结果调用下一个函数。

不管您使用`reduce`、`reduceRight`，切换调用顺序，还是其他什么。

> 如果你想要`pipe()`，从左到右。想要 compose()？从右向左走。

简单明了。下次见！