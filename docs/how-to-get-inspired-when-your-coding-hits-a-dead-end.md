# 当你的编码走进死胡同时，如何获得灵感

> 原文：<https://www.freecodecamp.org/news/how-to-get-inspired-when-your-coding-hits-a-dead-end/>

编程时遇到死胡同是很常见的。这在任何类型的问题解决中都很常见。

我们陷入了看待问题的特定方式，很难获得新的视角。

我最近偶然发现了一个来自创意艺术的工具，并意识到它可以适用于编程。

启发我的工具是倾斜战略卡。它们促使我们打破思维循环，激发我们以不同的方式思考。卡片包括“去除细节，转化为歧义”和“使用旧的想法”。

它们最著名的用途之一是在大卫·鲍依的专辑《英雄》中。例如，在“怀疑的感觉”赛道上，鲍伊和布赖恩·伊诺根据倾斜的策略卡[轮流做过度测试，而不向对方透露他们的卡上说了什么](https://dangerousminds.net/comments/brian_eno_and_peter_schmidts_oblique_strategies_the_original_handwritt)。

我觉得编码死胡同需要更直接的东西。在被一个问题卡住的时候，我们都需要灵感和安慰。

## 解决办法？编码灵感机

所以我收集了专家们对编码过程的评价——他们创造了编程语言和操作系统。我将这些睿智的话语融入到 Gordana Minovska 的设计中，让这些名言占据了中心位置。

![vFx4xkI63PkxfNy1UQGuiGvKYBeVl_-83ScRc68smMQSa1AgRRGi9mPtHnet_XHDYk-hZono_wHUz_F7fXY1dbYhsJ0nq24ynFU52md65YZfqmdMRd_LQR-4zYID4ZK0Vg7l0NVD](img/10bc03a91b35bb264a5a6b94ed2fd7c1.png)

[https://ryandawsonuk.github.io/CodingInspirationMachine/](https://ryandawsonuk.github.io/CodingInspirationMachine/)

目的是有一个工具来帮助从“我看不到任何解决方案”到“也许这种方法会走到某个地方”再到“啊哈”。

我们经常陷入一种不允许我们看到“可能”方法的视角。在那些时刻，我们可以用解决问题大师的一些睿智的话来给我们一个震撼。

例如，罗伯特·c·马丁的这些话:

> 当你在解决一个问题时，你有时会离它太近，以至于看不到所有的选项。你错过了优雅的解决方案，因为你头脑中的创造性部分被你专注的强度所压制。

想法是要么将 url 加入书签[,要么派生 repo 并配置 GitHub 页面来托管您自己的版本。](https://ryandawsonuk.github.io/CodingInspirationMachine/)

使用 fork，您可以将引号更改为您认为最有帮助的内容。然后你可以在卡壳的时候回到编码灵感机。

当然，这只是一个灵感工具。它不会取代头脑风暴和思维导图。

大卫·鲍依使用了许多工具来获得灵感，他的音乐可能更多地来自于剪报，而不是倾斜的策略卡片。

但是编码灵感机器的关键是要有一个简单易用的工具来提醒我们停滞不前没关系，这意味着困难，会有前进的道路。

## 编码灵感机器的真实世界应用

以下是我最近遇到的一些情况，让我开始思考这个技巧。

### 变得有创造力

我工作的一个系统出现了授权问题。适用于多个授权提供者的授权代码不适用于特定的 Active Directory 设置。

我们最初不知道这是提供商端的配置问题、应用程序端的配置问题、连接问题还是我们代码中的问题。我们甚至[构建了一个定制的测试工具](https://github.com/ryandawsonuk/oauth2-test-tool)来缩小问题范围。

最终，我们需要一个额外的 resource_uri 参数来包含在我们的一个 http 调用中。

### 寻找解决方案

对于同一个系统，我们希望显示长期的指标。这导致尝试在对于 Prometheus 查询来说太大的数据范围上进行 Prometheus 查询。

有一系列的方法来处理这个问题，从改变我们查询的内容，到使用不同的/更多的工具，到重新构造数据。我们选择了相当于[重组数据](https://github.com/SeldonIO/seldon-core/pull/2484)的东西。

### 看到不太明显的答案

我的岳父向我展示了他的智能电视与网飞不兼容。

在浏览了几个令人困惑的菜单并尝试了各种无线网络后，我们发现这是电视偏好的网络的信号强度问题(在电视认为强度较低的网络上工作正常)。

## 包扎

这些问题各不相同，但有共同的特征。

每一个都需要研究和实验，排除各种可能性。每一个问题最初都令人惊讶，需要时间来调整预期，并意识到为什么会出现问题。探索多条道路是必要的，而每一次失败都令人沮丧。

很容易陷入这些情况，发现我们再也看不到任何路径。其他曾经经历过的人的话可以帮助我们用新鲜的眼光看待这些情况。

看一下[编码灵感机](https://ryandawsonuk.github.io/CodingInspirationMachine/)并随时向 [github repo](https://github.com/ryandawsonuk/CodingInspirationMachine) 或[在 twitter 上联系我](https://twitter.com/ryandawsongb)提交建议。