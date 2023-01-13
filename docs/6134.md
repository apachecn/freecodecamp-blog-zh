# 机器学习:均方差和回归线介绍

> 原文：<https://www.freecodecamp.org/news/machine-learning-mean-squared-error-regression-line-c7dde9a26b93/>

摩西·比涅利

# 机器学习:均方差和回归线介绍

![gTEepgnyLsstSg1f91QYpxRBtTlPz0jZkSPG](img/87a313b9b33aa8bceed4a6adf74fbe4e.png)

Introduction image

### **简介**

本文将讨论统计方法**均方误差**，我将描述该方法与**回归线**的关系。

该示例由笛卡尔轴上的点组成。我们将定义一个数学函数，它将给出一条在笛卡儿轴上所有点之间通过最好的直线。

通过这种方式，我们将了解这两种方法之间的联系，以及它们联系在一起的结果是怎样的。

### 一般解释

这是来自维基百科的定义:

> 在统计学中，估计量的均方误差(MSE)衡量误差平方的平均值，即估计值和估计值之间的平均平方差。MSE 是一个风险函数，对应于平方误差损失的期望值。MSE 几乎总是严格为正(且不为零)的事实是因为随机性或者因为估计器没有考虑可以产生更精确估计的信息。

### 文章的结构

*   感受一下这个想法，图形可视化，均方差方程。
*   数学部分，包含代数运算和二元函数的导数，用于求最小值。这一节是**给那些想了解**我们后来如何得到数学公式的人的，如果你对此不感兴趣，你可以跳过。
*   解释我们收到的数学公式以及公式中每个变量的作用。
*   例子

### 感受一下这个想法

假设我们有七个点，我们的目标是找到一条线，使**最小化**到这些不同点的距离的平方。

让我们试着理解这一点。

我举一个例子，在这两点之间画一条线。当然，我的画不是最好的，但它只是为了演示的目的。

![MNskFmGPKuQfMLdmpkT-X7-8w2cJXulP3683](img/ab19a7c38e1800d4b160241197e868a6.png)

Points on a simple graph.

你可能会问自己，这个图是什么？

*   紫色的点是图表上的点。每个点都有一个 x 坐标和一个 y 坐标。
*   蓝色的线是我们的预测线。这是一条穿过所有点并以最佳方式拟合它们的线。这条线包含预测的点。
*   每个紫色点和预测线之间的**红线**是**误差。**每个误差都是该点到其预测点的距离。

你应该还记得学生时代的这个方程， ***y=Mx+B*** ，其中 **M** 是直线的[斜率](https://en.wikipedia.org/wiki/Slope)，而 **B** 是直线的 [y 截距](https://en.wikipedia.org/wiki/Y-intercept)。

我们想找到 M ( [斜率](https://en.wikipedia.org/wiki/Slope)和 B ( [y 截距](https://en.wikipedia.org/wiki/Y-intercept))使得**最小化**平方误差！

让我们定义一个数学方程，它将给出我们所有点的均方误差。

![hmZydSW9YegiMVPWq2JBpOpai3CejzQpGkNG](img/0a9735f6d63abeda5e1a1b99ae4dea45.png)

General formula for mean squared error.

我们来分析一下这个等式实际上是什么意思。

*   在数学中，看起来像怪异 E 的字符被称为求和(希腊文 sigma)。它是一系列数字的总和，从 i=1 到 n。让我们把它想象成一个点的数组，我们遍历所有的点，从第一个(i=1)到最后一个(i=n)。
*   对于每个点，我们取该点的 y 坐标和 y '-坐标。y 坐标是我们的紫点。y 点位于我们创建的线上。我们从 y '坐标值中减去 y 坐标值，并计算结果的平方。
*   第三部分是取所有(y-y’)值之和，除以 n，得出平均值。

我们的目标是最小化这个平均值，这将为我们提供通过所有点的最佳直线。

### 从概念到数学方程式

这部分是给那些想了解我们如何得到数学方程式的人的。如果你愿意，可以跳到下一部分。

大家知道，直线方程是 y=mx+b，其中 m 是[斜率](https://en.wikipedia.org/wiki/Slope)，b 是 [y 截距](https://en.wikipedia.org/wiki/Y-intercept)。

让我们取图上的每一点，然后我们来计算(y-y’)。但是什么是 y '，我们如何计算它？我们没有把它作为数据的一部分。

但是我们知道，为了计算 y '，我们需要使用我们的线方程，y=mx+b，并把 x 放入方程中。

从这里我们得到下面的等式:

![wSige6ZLxM-QaVt3fRWXIAzsHvX7wdcJ4XOy](img/000e07a255e7ba5f0998a3540b3e8975.png)

让我们重写这个表达式来简化它。

![JFi5pzT7YtJ-0Fkx59jP0hCNHzc8tvsrXgPg](img/8935b50098bf020114b08a2baee59cba.png)

让我们从打开等式中的所有括号开始。为了更容易理解，我把等式之间的区别涂上了颜色。

![vWLTze9HzNDSg4LRM5dbpkYUpkXkhTW6TnRl](img/763097c40ecd5e3e4e03d02c2bb52d38.png)

现在，让我们应用另一个操作。我们将把每一部分组装起来。我们将所有的 y，和(-2ymx)等，我们将把它们并排。

![y3gkwSWxwAOcxfxMILLV0teW1273PFtFiqW4](img/e905ea5ee58dc99bef0976c83e5c9026.png)

在这一点上我们开始变得混乱，所以让我们取 y，xy，x，x 所有平方值的平均值。

让我们为每一个定义一个新的字符，它将代表所有平方值的平均值。

我们来看一个例子，我们取所有的 y 值，由于是均值，所以用 n 除，称之为 y(HeadLine)。

![L3NWDFs1LUKgQU223EAFXXUXX3OTFWR0gLtE](img/00ec37e1b150b94d20ae88b6e5acc047.png)

如果我们将等式两边都乘以 n，我们得到:

![jyiOt9MVCg460395d6mkHlrmK9ssfr8nQGJC](img/5cc3ab4976c4808aa220d544a4a75128.png)

这将引导我们得出下面的等式:

![bv3wucYBgHc3Zch115zMYjhH-zYe5VgwjMAH](img/45445138cacaac8b7c27bdd7b4868892.png)

如果我们看看我们得到了什么，我们可以看到我们有一个三维表面。它看起来像一个玻璃杯，向上急剧上升。

我们想找到使函数最小的 M 和 B。我们将对 M 做偏导数，对 b 做偏导数。

因为我们在寻找一个极小点，我们将取偏导数并与 0 比较。

![88voRjo799rIopVP8YjsHlNhrBSJ8REg26hY](img/432196dc025021125740ff7759269df7.png)

Partial derivatives formula

![6t-4Uq4Y4GMGg9mYWPUUmHHsmaTvxuDPZCj3](img/8075ecea7bc4c7140c68657f980e1102.png)

Partial derivatives

让我们把收到的两个方程，从两个方程中分离出变量 b，然后从下面的方程中减去上面的方程。

![-I3Ly2wOtJf9WiecfOjvFiY6U9DXB4PJBQ6t](img/fa6c7e86e8f04663356d57af15d37670.png)

Different writing of the equations after the derivation by parts

让我们从第二个方程中减去第一个方程

![6WzsJxr0jSG8XPYz-F2dSmINqnexxJLxWsxi](img/a3ca39fdc9bab707bcc00ce26b3e93ea.png)

Merge two equations together

让我们从等式中去掉分母。

![Ac05NR92faqptoFE35F2XFcKjllJhJPdwGnE](img/47d5e493d3074b870078b0151fc87c91.png)

Final equation to find M.

好了，这是求 M 的方程，让我们用这个写下 B 方程。

![pjxjeSICBJNckegf3WXCHtfrf7dyIxVfqbBB](img/1b80bc50c7a5e20e2b068ec36706dae8.png)

Final equation to find B.

### 斜率和 y 轴截距的公式

让我们提供数学方程式，帮助我们找到所需的[斜率](https://en.wikipedia.org/wiki/Slope)和 [y 轴截距](https://en.wikipedia.org/wiki/Y-intercept)。

![290zZ8roKAfKNCrfq1LN7QuTooJjbH19Isiv](img/321a2adddeccd4156bba4473577dc4a5.png)

Slope and y-intercept equations

所以你可能会想，这些奇怪的方程式到底是什么？

它们实际上很容易理解，所以我们来简单介绍一下。

![KTFy4uhGXnGSrCoyInhSWfHH4VTEnAJyncpm](img/5e2eae18299e4634c7f7aa9d6e4fad7a.png)

Sum of x divided by n

![lQSFx0h7KiRB0uOcriwpFrmhsev3kt4cCUU5](img/cdfea382346a0bf20e3fa587b145a2e6.png)

Sum of x² divided by n

![LYZL8LPc8vyZ0wPV2J2sp-pXiuCzvslY8EAQ](img/f6d1d369c92402cb66c45c76bf672e00.png)

Sum of xy divided by n

![0E27klUj208HeeecnRKR9Eokb2PmKfUNoO-O](img/4dfd1b4c5a2f166a56d968e56111cf28.png)

Sum of y divided by n

既然我们已经理解了我们的方程，现在是时候把所有的东西放在一起，展示一些例子了。

### 例子

非常感谢[汗学院](https://www.khanacademy.org/)提供的例子。

#### 示例#1

我们取 3 分，(1，2)，(2，1)，(4，3)。

![IudmVD0mo4BMYqPEjFyETchb5GGsDv5ikxwB](img/307e3a957d9c153cb937eb2bc469ec6a.png)

Points on graph.

让我们找出方程 y=mx+b 的 M 和 B。

![KFDixcE4WidM6Pez8RNDwOgBorpnj1QuLw5S](img/43890b9bf0c9ebc59fb59aa03b54b30b.png)

Sum the x values and divide by n

![Rqkh4dC9zZ11V4McMwJFspxv5UySTiI9Sv1L](img/11d9a8029d462528195043233aa77fd2.png)

Sum the y values and divide by n

![tkUVYMlF-9qDaK69dWj0bFy1ApEK4DHw05vK](img/77239dfd6acce4b1790addf1b88112ea.png)

Sum the xy values and divide by n

![80W3OcjPxF9ek2HIjv0VYnwCEhpzURavMAlj](img/f64b54f8ecb459245cf48900baf72244.png)

Sum the x² values and divide by n

在我们计算完 M 方程和 B 方程的相关部分后，让我们将这些值放入方程中，得到[斜率](https://en.wikipedia.org/wiki/Slope)和 [y 截距](https://en.wikipedia.org/wiki/Y-intercept)。

![Hri9luC8oVUAgZLnLoDgey4X0T6LEZwIFMav](img/53d070f5b88b30d836f3445e1e569709.png)

Slope calculation

![H4Ss6UYBdSfJgx63lz93uXaubcE3-6e1niFS](img/9be5d25c436b63ca5ff4b3fcd1633d33.png)

y-intercept calculation

让我们把这些结果放在线方程 y=mx+b 中。

![S9EESO6mBvglt1o--YlQZQFqhNGPg4we6Kju](img/f546709f4ed2f0afc15da235e52f818b.png)

现在让我们画一条线，看看这条线是如何穿过这些线的，使得距离的平方最小。

![DlKy-Eekc0SdHpcOeQPGJobo7jYLfTh0pI8Q](img/695f74c6144082e20ec26cc69c760bc5.png)

Regression line that minimizes the MSE.

#### 实施例 2

我们取 4 点，(-2，-3)，(-1，-1)，(1，2)，(4，3)。

![MrlSNVYUJEh-4OcRGXEe3hbeU10wjTH-vmDB](img/978c86d46e78af67d7928f1c97980864.png)

Points on graph.

让我们找出方程 y=mx+b 的 M 和 B。

![MqNv9HXhu7koehCq1WgBSH2Mje3VoHUM6Dsb](img/13fb788cd3499766bca993a8905677c4.png)

Sum the x values and divide by n

![I8bZESRhxejhmNWbxMlusVlxfCgnrJPbn2En](img/32dabfd1677995e6b86129add55061e2.png)

Sum the y values and divide by n

![YwF2k-wP1YkSiPUoZZ5kV99p5xpS4VeBtlxP](img/281d381915cd72c06e680dd1fe65b89f.png)

Sum the xy values and divide by n

![Sbo7-PaRePrfBM1sOME5du5GDQ-1r1ntdoD1](img/5162e90a7edf973dbf44badbc4986254.png)

Sum the x² values and divide by n

和以前一样，让我们把这些值放入我们的方程中，找到 M 和 b。

![LUideJM-zrCgulLv83Gh08ySgcChQXY6BpxC](img/c21d600f915d42bab3371ad60ca07944.png)

Slope calculation

![F9K53LF0Dp3kjIYYC3UJoLfGJqICCIhtqTMo](img/6185621a82146fb792b7b4a2cb4ce3f0.png)

y-intercept calculation

让我们把这些结果放入方程 y=mx+b。

![0o5OFw2QwtBJYntrz4vRJn9ywrdsumLxH5rg](img/1c447e558ef533f1a414981d69cb7040.png)

现在让我们画一条线，看看这条线是如何穿过这些线的，使得距离的平方最小。

![yAMNsNJmTBdZ2MKPbD8JX-es3d-5Oj4OIHRl](img/b4275485c2a259d734abfab58c31b71b.png)

Regression line that minimizes the MSE

### 最后

如你所见，整个想法很简单。我们只需要了解主要部分以及如何使用它们。

您可以使用公式在另一个图形上找到直线，并执行简单的计算，得到[斜率](https://en.wikipedia.org/wiki/Slope)和 [y 轴截距](https://en.wikipedia.org/wiki/Y-intercept)的结果。

就这样，简单吗？？

欢迎所有评论和反馈——如果有必要，我会修改文章。

欢迎直接在 LinkedIn 上联系我— [点击这里](http://www.linkedin.com/in/moshe-binieli-22b11a137)。