# 递归可能看起来很可怕——但并不一定如此

> 原文：<https://www.freecodecamp.org/news/recursion-doesnt-have-to-be-scary/>

任何一个我们没有完全理解的概念，一开始都可能很恐怖。

递归是编程学生不会马上学会的话题。但这并不意味着它需要令人害怕或引起焦虑。

事实上，递归是一个我们可以简单定义的概念。

据[维基百科](https://en.wikipedia.org/wiki/Recursion_(computer_science)):

> 在计算机科学中，递归是一种解决问题的方法，其解决方案依赖于同一问题的较小实例的解决方案。

您可以通过创建一个调用自身的函数在代码中应用递归。

## 任何有循环的函数都可以是递归的

这里有一个名为`countToTen`的函数，它使用一个 while 循环来记录从 1 到 10 的每个数字:

```
const countToTen = (num = 1) => {
    while (num <= 10) {
        console.log(num);
        num++;
    }
}

countToTen(); 
```

我们可以用递归代替循环来写同样的函数。

注意递归函数有两个部分:

1.  该函数调用自身(也称为递归调用)
2.  至少一个退出递归的条件(也称为基本情况)

```
const countToTen = (num = 1) => {
    if (num > 10) return; //base case
    console.log(num);
    num++;
    countToTen(num); //recursive call
}

countToTen(); 
```

这并不是说我们**应该** **总是**用递归替换循环。

有些情况下使用递归是最好的选择——同样，有些情况下也不是最好的选择。

## 为什么使用递归

让我们看看使用递归的一些理由。我们将在下面看到一些例子。

### 较少的代码行

与不使用递归的解决方案相比，使用递归的解决方案通常代码行更少。

### 更优雅的代码

除了更少的代码行之外，递归解决方案通常看起来更令人愉快。换句话说，递归解决方案通常被认为是优雅的。

### 可读性增强

原因 1 和 2 通常结合在一起形成原因 3，这是增加代码的可读性。记住，我们不只是为自己写代码。我们为跟随我们的开发人员编写代码，他们必须理解我们的代码。

## 不使用递归的原因

### 特性损失

重复函数调用不如应用循环高效或高效。我们不想简单地选择递归，因为我们可以。

### 调试问题

递归的逻辑并不总是容易理解的。利用递归可能会使代码更难调试。

### 可读性提高了吗？

使用递归并不能保证增加可读性。事实上，这可能是固执己见和/或视情况而定的。您应该评估可读性，如果没有改进，递归可能不是最好的答案。

## 递归示例

递归问题是面试的最爱。

一个这样的问题需要一个返回斐波纳契数列的`x`数字的函数。

斐波纳契数列将数列的前两个数字相加，产生数列的下一个数字。下面是序列的前十个数字:
`[0,1,1,2,3,5,8,13,21,34]`

我们可以不用递归来写这个函数:

```
const fibonacci = (num = 2, array = [0, 1]) => {
    while (num > 2) {
        const [nextToLast, last] = array.slice(-2);
        array.push(nextToLast + last);
        num -= 1;
    }
    return array;
}

console.log(fibonacci(10)); 
```

这是同一个函数的递归版本:

```
const fibonacci = (num = 2, array = [0, 1]) => {
    if (num < 2) return array.slice(0, array.length - 1);
    const [nextToLast, last] = array.slice(-2);
    return fibonacci(num - 1, [...array, nextToLast + last]);
}

console.log(fibonacci(10)); 
```

递归函数确实有更少的代码行。但是我不确定我们是否可以自信地说它增加了优雅和可读性。

让我们来看另一个递归影响更大的问题。

另一个面试的最爱是要求一个函数返回斐波那契数列中的第 n 个数字。因此，如果函数接收`10`作为参数，它应该返回`34`。

如果不使用递归，可能的解决方案如下所示:

```
const fibonacciPos = (pos = 1) => {
    if (pos < 2) return pos;
    const seq = [0, 1];
    for (let i = 2; i <= pos; i++) {
        const [nextToLast, last] = seq.slice(-2);
        seq.push(nextToLast + last);
    }
    return seq[pos];
}

console.log(fibonacciPos(10)); 
```

然而，使用递归，解决方案要小得多，而且可以说更优雅:

```
const fibonacciPos = (pos = 1) => {
    if (pos < 2) return pos;
    return fibonacciPos(pos - 1) + fibonacciPos(pos - 2);
}

console.log(fibonacciPos(10)); 
```

哇！这造成了巨大的差异。

注意返回行实际上是如何调用函数两次的。

你理解这些递归函数中的逻辑吗？如果没有，花些时间对它们进行实验，以了解它们是如何工作的。这样做之后，您可能会同意可读性也提高了。

为了强调可读性的提高是如何自以为是的，让我们看一下上面用一行代码编写的相同递归函数(这一行可能会在浏览器中换行，但它是一行代码):

```
const fibonacciPos= pos => pos < 2 ? pos : fibonacciPos(pos - 1) + fibonacciPos(pos - 2);

console.log(fibonacciPos(10)); 
```

我们最初的递归解决方案从四行代码变成了一行！

对你来说可读性更强吗？你还遵循它背后的逻辑吗？

你的回答将取决于你目前的理解水平。一行解决方案利用了一个三元语句，具有不带括号的箭头函数，这也意味着一个 return 语句，并像以前一样应用递归。

我通常不写像上面一行解决方案那样的函数，因为我经常教初级 web 开发课程。因此，我经常把我的代码分成更容易理解的步骤。

这并不是说上面的一行解决方案有什么问题。

事实上，如果您理解其背后的逻辑，这个函数是优雅的、可读的、高效的。

如果您在团队中工作，您的团队可能会有一个风格指南。如果可能的话，一行函数是首选，那就去做吧！如果更谨慎，一步一步的风格是首选，遵循你的指南。这些决定完全取决于具体情况。

## 结论

递归可能看起来很可怕，但不一定如此。

我们可以把递归的概念分解成一个简单的定义。

不要仅仅因为可以就使用递归的能力。

您应该根据效率、性能、优雅和可读性来决定在代码中使用递归。

你可能想知道递归在“真实世界”中的应用，而不仅仅是回答斐波纳契数列的面试问题。

我将从我的 Youtube 频道给你留下一个教程。我不仅深入研究了上面的例子，还揭示了一些应用递归是最佳选择的“真实世界”实例:

[https://www.youtube.com/embed/Q0alTGQ-lXk?feature=oembed](https://www.youtube.com/embed/Q0alTGQ-lXk?feature=oembed)

Javascript Recursion Tutorial