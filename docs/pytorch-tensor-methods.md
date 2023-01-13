# PyTorch 张量方法——如何用 Python 创建张量

> 原文：<https://www.freecodecamp.org/news/pytorch-tensor-methods/>

PyTorch 是一个基于 Python 的开源库。它在构建、训练和部署深度学习模型时提供了高度的灵活性和速度。

PyTorch 的核心是涉及张量的运算。张量是一个数，向量，矩阵，或者任何 n 维数组。

在本文中，我们将看到使用 PyTorch 张量方法(函数)创建张量的不同方法。

## 我们将涉及的主题

*   张量
*   零
*   二进制反码
*   全部
*   阿兰格
*   linspace
*   边缘
*   randint
*   眼睛
*   复杂的

### 张量()方法

当传递给这个方法`data`时，它返回一个张量。`data`可以是标量、元组、列表或者 *NumPy* 数组。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=76](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=76)

在上面的例子中，使用`np.arange()`创建的 NumPy 数组被传递给了`tensor()`方法，产生了一个一维张量。

我们可以通过传递一个元组、一个列表列表或一个多维 NumPy 数组来创建一个多维张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=77](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=77)

当一个空元组或列表被传入`tensor()`时，它会创建一个空张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=78](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=78)

### zeros()方法

该方法返回一个指定`size`(形状)的张量，其中所有元素为零。`size`**可以以元组或列表的形式给出，也可以两者都不给出。**

 **[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=4](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=4)

我们也可以在元组或列表中传递`3, 2`。传递负数或浮点数会导致运行时错误，这是不言而喻的。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=8](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=8)

传递空元组或空列表会给出一个大小(维度)为 0 的张量，0 是其唯一的元素，其数据类型为 float。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=10](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=10)

### ones()方法

类似于`zeros()`，`ones()`返回一个张量，其中所有元素都是 1，具有指定的`size`(形状)。`size`**可以以元组或列表的形式给出，也可以两者都不给出。**

 **[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/53&cellId=19](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/53&cellId=19)

和`zeros()`一样，传递一个空元组或者列表给出一个 0 维的张量，以 1 为唯一元素，数据类型为 float。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=23](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=23)

### full()方法

如果你想让一个张量的所有元素都等于某个值但不仅仅是 0 和 1 呢？可能 2.9？

`full()`返回由`size`参数给定形状的张量，其所有元素等于`fill_value`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=27](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=27)

这里，我们创建了一个形状为`3, 2`的张量，其中`fill_value`为 3。这里，传递一个空的元组或列表会创建一个零维的标量张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=31](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=31)

而使用`full`，则是 ****需要**** 将`size`作为元组或者列表给出。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=33](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=33)

### arange()方法

这个方法返回一个一维张量，元素从`start`(包含)到`end`(不包含)，有一个共同的区别`step`。`start`的默认值为 0，而`step`的默认值为 1。

张量的元素可以说是以 ****为等差数列**** ，以`step`为共差。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=37](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=37)

这里，我们创建了一个张量，它从 2 开始，一直到 20，其`step`(普通差)为 2。

所有三个参数，`start`、`end`和`step`可以是正数、负数或浮点数。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=41](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=41)

在选择`start`、`end`、`step`时，我们需要确保`start`、`end`与`step`符号一致。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=41](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=41)

由于`step`设置为-2，所以-42 没有办法达到-22(独占)。因此，它会给出一个错误。

### linspace()方法

该方法返回一个一维张量，元素从`start`(含)到`end`(含)。然而，与`arange()`不同的是，这里的`steps`不是普通的差，而是张量中元素的数量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=45](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=45)

PyTorch 根据给定的`steps`自动决定公共差异。

不推荐为`steps`提供值。对于 **的向后兼容性** ，不提供 steps 的值会创建一个具有 ****100 个**** 元素的张量。根据官方文档，在未来的 PyTorch 版本中，未能提供步骤值将会引发运行时错误。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=47](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=47)

与`arange()`不同，`linspace`的`start`可以大于`end`，因为公差带是自动计算的。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=49](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=49)

由于这里的`steps`不是普通的差，而是元素个数，所以只能是非负整数。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=51](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=51)

### rand()方法

此方法返回一个张量，其中填充了从 0(含)到 1(不含)区间内均匀分布的随机数。形状由`size`参数给出。`size`参数可以以元组或列表的形式给出，也可以两者都不给出。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=55](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=55)

传递空元组或列表会创建一个零维标量张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=56](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=56)

### randint()方法

该方法返回一个张量，其中填充了在`low`(含)和`high`(不含)之间均匀生成的随机整数。形状是由`size`参数给出的。`low`的默认值为 0。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=59](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=59)

当只传递了一个`int`参数时，默认情况下，`low`获取值 0，`high`获取传递的值。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=60](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=60)

`size`参数只接受一个元组或一个列表。空元组或列表创建一个零维张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=62](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=62)

### eye()方法

这个方法返回一个对角线上为 1，其他地方为 0 的二维张量。行数由`n`给出，列数由`m`给出。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=65](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=65)

`m`的默认值是`n`的值。当只有`n`被传递时，它创建一个 ****形式的张量单位矩阵**** 。单位矩阵的对角元素为 1，所有其他元素为 0。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=66](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=66)

### complex()方法

这个方法返回一个复张量，其实部等于`real`，虚部等于`imag`。`real`和`imag`都是张量。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=69](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=69)

`real`和`imag`张量的数据类型应该是`float`或`double`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=70](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=70)

同样，两个张量`real`和`imag`的`size`应该相同，因为两个矩阵的对应元素形成一个复数。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=72](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/different-ways-to-create-tensors/v/51&cellId=72)

## 结论

我们已经介绍了使用 PyTorch 方法创建张量的十种不同方法。你可以通过[官方文档](https://pytorch.org/docs/stable/torch.html)了解更多关于其他 PyTorch 方法的信息。

你可以点击[这里](https://jovian.ai/srijansrj5901/different-ways-to-create-tensors)进入 Jupyter 笔记本，在那里你可以尝试这些方法。

如果你想了解更多关于 PyTorch 的知识，请查看 freeCodeCamp 的 [YouTube](https://www.youtube.com/watch?v=5ioMqzMRFgM&t=3s&ab_channel=freeCodeCamp.org) 频道上的[这门](https://jovian.ai/learn/deep-learning-with-pytorch-zero-to-gans)令人惊叹的课程。

注意安全！****