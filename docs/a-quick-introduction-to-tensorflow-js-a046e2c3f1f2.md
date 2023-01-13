# TensorFlow.js 快速介绍

> 原文：<https://www.freecodecamp.org/news/a-quick-introduction-to-tensorflow-js-a046e2c3f1f2/>

保罗·帕万

# TensorFlow.js 快速介绍

![uSoZAymZzFUzkM3pLLq2gcdCwr0XI8J0Vsju](img/efe46d8012faa13c806d7c9898343f41.png)

Photo by [Fabian Grohs](https://unsplash.com/@grohsfabian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TensorFlow 已经存在一段时间了。然而，直到上个月，它还只适用于 Python 和其他一些编程语言，比如 C 和 Java。你可能认为这些已经足够了。

我以前听说过张量流。尽管我对机器或深度学习一无所知，但我非常确定这是实现这些目的最常用的框架之一。我见过很多用它做的很酷的事情:图像中的物体检测，语音识别，甚至音乐创作！

想象一下，能够在浏览器中做所有这些很酷的 ML 事情，而不需要安装任何库，也不需要一次又一次地编译所有这些代码行。好吧，这就是 TensorFlow.js 要做的。

如果你想了解更多关于这个令人惊奇的框架是如何以及为什么出现的，你可以在 Medium 上查看 [TensorFlow](https://www.freecodecamp.org/news/a-quick-introduction-to-tensorflow-js-a046e2c3f1f2/undefined) ！

所以我一发现这个，就想开始学习 TensorFlow.js 是怎么工作的。这正是我已经开始做的事情:我在 Youtube 上找到了[编码列车的一组教程，它们仍在进行中(事实上，它们才刚刚开始)，我已经开始对一些东西进行修改。](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6YIeVA3dNxbR9PYj4wV31oQ)

我想给你一个关于 TensorFlow (TF)的快速介绍，这样你就可以跟随我的旅程，和我一起学习。

### TensorFlow.js 的基础

我们开始吧！首先你要知道所有的文档都在 [TF 的网站](https://js.tensorflow.org/)上，在 API 参考部分下面。

> 但是等等，为什么叫 TensorFlow？张量到底是什么？

很高兴你问了。张量基本上是一种数字结构。在数学中，数字有不同的表示方法。你可以只有数字本身，一个向量，一个矩阵等等。张量只是所有这些不同的数据表示的通用术语。

在 TF 中，张量被区分是因为它们的**等级**，或者换句话说，它们拥有的**维度**的数量。

这些是最常见的:

#### 标量(秩-0)

只是一个数字。这是创建和控制台记录一个日志的方法:

```
tf.scalar(4.5).print();
```

输出如下所示:

```
Tensor  4.5
```

#### Tensor1d、tensor2d、tensor3d 和 tensor4d(分别排名- 1、2、3 和 4)

这些是更高维的张量。例如，如果你想创建一个秩为 1 的张量，你可以简单地这样做:

```
tf.tensor1d([3, 7, 8]).print();
```

它将输出:

```
Tensor  [3, 7, 8]
```

#### 张量(秩 n)

如果你不知道你的张量的维数，你可以简单地用下面的函数创建一个(注意上面的两个例子是如何用另一种方法工作的):

```
tf.tensor(4.5).print();tf.tensor([3, 7, 8]).print();
```

这与之前的输出完全相同。

此外，您还可以向这些函数传递几个参数。

#### tf.tensor(数值，形状？，dtype？)

先来看**值**。这是唯一的强制参数，也是我们在前面的例子中传递的唯一参数。您可以传递一个值的平面数组(或者标量中的单个数字)并自己指定形状，也可以传递一个嵌套数组。

现在你可能想知道**的形状**是什么。假设你想输出下面的张量:

```
[[1, 5], [4, 7]]
```

也就是你猜对了，一个 2x2 的矩阵。您可以通过传递一个平面数组并将形状指定为函数的第二个参数来创建这个张量

```
tf.tensor([1, 5, 4, 7], [2, 2]).print();
```

或者通过传递嵌套数组

```
tf.tensor([[1, 5], [4, 7]]).print();
```

最后，我们有**数据类型**。这指定了数据类型。目前， **int32，float32** 和 **bool** 是支持的三种类型。

#### 操作

但是，你能用这些张量做什么呢？嗯，除了其他事情之外，你可以对它们进行数学计算，比如算术运算:

**注**:以下操作是按元素执行的，这意味着所涉及的第一个张量的每一项都与另一个张量中相同位置的项相关联。

```
const a = tf.tensor1d([4, 7, 2, 1]);const b = tf.tensor1d([20, 30, 40, 50]);
```

有两种方法可以将这两者相加:

```
a.add(b).print();
```

或者，

```
tf.add(a, b);
```

两个输出:

```
Tensor  [24, 37, 42, 51]
```

这是它们如何做减法的，

```
tf.sub(a, b).print();
```

```
Tensor //output  [-16, -23, -38, -49]
```

乘法，

```
tf.mul(a, b).print();
```

```
Tensor //output  [80, 210, 80, 50]
```

和部门:

```
tf.div(a, b).print();
```

```
Tensor //output  [0.2, 0.2333333, 0.05, 0.02]
```

这非常简单明了。

### 你自己试试！

如果你还没有，我鼓励你自己尝试一下上面的方法。这是 TF 中最基本的东西，但是这些概念是理解更复杂(也更有趣)部分的关键。

感谢阅读！

编辑:在 TensorFlow 上查看一下 [ADL](https://www.freecodecamp.org/news/a-quick-introduction-to-tensorflow-js-a046e2c3f1f2/undefined) 的 [Youtube 播放列表](https://www.youtube.com/playlist?list=PL1jmMFbLDfgX0Q-rBbfoBdNFl8MS3kTh9)，我相信它会帮到你的！