# 高级演员-评论家方法介绍:强化学习课程

> 原文：<https://www.freecodecamp.org/news/intro-to-advanced-actor-critic-methods-reinforcement-learning-course/>

演员-评论家方法是非常有用的强化学习技术。

行动者-批评家方法对机器人应用最有用，因为它们允许软件输出连续的动作，而不是离散的动作。这使得能够控制电动机来驱动机器人系统中的运动，代价是增加了计算复杂性。

我们刚刚在 freeCodeCamp.org YouTube 频道上发布了一个关于演员评论方法的综合课程。

泰伯博士开发了这个课程。他是一名物理学家和前半导体工程师，现在是一名数据科学家。

演员-评论家方法背后的基本思想是有两个深层神经网络。行动者网络近似于代理人的策略:一种概率分布，它告诉我们在给定环境状态的情况下选择(连续)行动的概率。批评家网络近似于价值函数:代理人对跟随当前状态的未来回报的估计。这两个网络相互作用，将政策转向更有利可图的状态，在这种状态下，盈利能力取决于与环境的相互作用。

这不需要事先了解我们的环境是如何工作的，也不需要任何关于游戏规则的输入。我们要做的就是让算法和环境互动，看着它学习。

本课程还结合了深度 Q 学习中的一些有用的创新，如经验重放缓冲区和目标网络的使用。这增加了学习策略的稳定性和健壮性，因此我们的代理能够学习有效的策略来导航开放的人工智能健身房环境。

以下是本课程中涉及的算法:

*   演员评论家
*   深度确定性政策梯度(DDPG)
*   双延迟深度确定性策略梯度(TD3)
*   近似策略优化(PPO)
*   软演员评论家(SAC)
*   异步优势行动者评论家(A3C)

观看 freeCodeCamp.org YouTube 频道的全部课程(6 小时观看)。

[https://www.youtube.com/embed/K2qjAixgLqk?feature=oembed](https://www.youtube.com/embed/K2qjAixgLqk?feature=oembed)