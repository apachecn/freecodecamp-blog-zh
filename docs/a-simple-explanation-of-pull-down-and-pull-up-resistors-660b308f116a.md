# 硬件基础:下拉和上拉电阻的工作原理

> 原文：<https://www.freecodecamp.org/news/a-simple-explanation-of-pull-down-and-pull-up-resistors-660b308f116a/>

作者 Taron Foxworth

# 硬件基础:下拉和上拉电阻的工作原理

![3SphnRhCroB7x-q3fgqaZGP9QoZnea-fA8Y5](img/bf7abaae79e37f335aa4a8de3673beb7.png)

An axial-lead resistor

如果你曾经将一个按钮连接到 Arduino，你会看到这个图表:

![5z3GVJwnEtxRQZnrIeMa6806A0l45ZDHoLLc](img/a03a90cce94b9bbd2fd8e673f8439d53.png)

起初，这可能会令人困惑。我的第一个想法是:“为什么我需要一个电阻？我只是想让它告诉我按钮是否被按下了。”

看了很多之后，没有一个简单的解释。

### 这是怎么回事

![lnCBI4aQPD72ryakAoXRZOOIscOKzO1TN8T-](img/90dfd62d37fd9cfb5437bc8bd7b0865b.png)

Diagram 1

在那个按钮——也就是开关——里，电线的形状是“H”形。但是中间没有接通——或者说电路没有接通——直到我们按下按钮。

实际上，我们希望在没有连接时从 Arduino 读取 a `0`，在按钮被按下时读取 a `1`。

在 Arduino 上，这被称为通用输入输出( [GPIO](https://en.wikipedia.org/wiki/General-purpose_input/output) )。

所以，我们可以这样做:

![ei-WY10bPEXDJh5eHRZ8VW0K1Xp0E-fWPQz5](img/9746d89c86b4e4514916251e2f865a98.png)

Diagram 2

我们将正极(5v、3.3V 或 VCC)连接到电路的左侧。

现在，当按下按钮时，GPIO 将读取一个`1`，一切正常。

![WjBnoryrJcaJusAFgohNgpnKrR0mKXYI7fmd](img/64d23a49c3e20e23285d968be7d85d33.png)

Diagram 3

不，让我们再看一下图表 2:

![1T7hhKBigZliDdNhgut-kKK8KMmgId1jEAEv](img/6f7babf4dad8d626fb1cff0597811cb8.png)

Diagram 2

当什么都不连接时，我们想要一个`0`,但是你如何保证这一点？目前没有办法保证 GPIO 是`0`。

空气中的电磁频率也会将你的 GPIO 吸引到`0`或`1`。它甚至可以在两者之间波动！这样，我们不能肯定这是一个`0`(我不擅长双关语)。这也被称为逻辑`0`。

获得逻辑`0`的一种方法是将引脚接地:

![mZLL1wlSMz6ReTNeJZDAQMaIOVnkOTrg5qqY](img/46216d37746fa692b86f3801f3b3299a.png)

耶！所以，现在它是一个保证的逻辑 0。按下按钮时，现在是`1`了。对吗？

嗯，没有。

![sMUiXkmyybe-DrcMuYHb0IqpU3FFLtTYm-uy](img/653af8857edeab4fae4f3cdb9fdae050.png)

你刚刚造成了[短路](https://en.wikipedia.org/wiki/Short_circuit)。？

这就是电阻的用武之地。为了避免短路，我们需要在电路中增加电阻。电阻器使事情处于控制之下。

![T3HTmawK4YN37wNqYke-QQdh5EVmFnNY8nec](img/e159aa960a14f6fb31aa40b692c3f0eb.png)

电将走阻力最小的路径。当按钮被按下时，您的 GPIO 现在会记录一个`1`。像这样:

![yLtF3UnfFhhhZ4Kjdqg81d4MvXqVDsYuCclh](img/935643c020812fd69830d0130b481bf5.png)![msnI0gnXpKvs5h6JuTVHExkeAWJuzxyl7J2x](img/fbb33b4c5a2de74f48546e5dce0bc26f.png)

吼吼！现在我们在研究一些东西。

现在我们来看看相反的情况:上拉电阻。这是同样的事情，但方向相反。当按钮未按下时，GPIO 将记录一个`1`。当您按下按钮时，GPIO 将为`0`。

虽然没有按下，我们有 GPIO 连接到正极(VCC)。因此，那里的任何电流都将被上拉，以便 GPIO 记录一个逻辑`1`。

![zUGnUEh9axrnFyaWOvkUNt1B5uCZxQBewETh](img/baff53b6a23240a31a28a1524fce77aa.png)

这里需要注意的是，电总是要接地的。所以，当我们按下按钮，电流就会流向地面。因此，任何流向 GPIO 的电流都会随之流走，使 GPIO 处于逻辑`0`。

![4NW0bqGqmZbUolmRv4LObF5qR8fccQ1z9zl0](img/30bb4a6889781f6bc7540c41911617d4.png)

？结束了。

#### 我为什么要写这个？

我是 2016 年 9 月加入 [Losant](https://losant.com) 的，没有硬件经验。每一个硬件初学者工具包都给你一个按钮，没有这个概念的解释。希望这也能让你的灯泡熄灭。？

这仅仅触及了表面。如果您想深入了解，请查看以下资源:

[**上拉电阻——learn.sparkfun.com**](https://learn.sparkfun.com/tutorials/pull-up-resistors)
[*另外需要指出的是，上拉电阻越大，引脚响应越慢……*learn.sparkfun.com](https://learn.sparkfun.com/tutorials/pull-up-resistors)

我喜欢反馈。所以，请让我知道这是否可以改善。**如果我完全错过了这个球，[让我知道](http://twitter.com/anaptfox)！我很乐意为他人做得更好。**