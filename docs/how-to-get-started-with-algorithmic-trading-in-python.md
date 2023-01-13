# 如何入门 Python 中的算法交易

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-with-algorithmic-trading-in-python/>

当我在一家投资管理公司担任系统开发工程师时，我了解到要在量化金融领域取得成功，你需要精通数学、编程和数据分析。

[算法或量化交易](https://www.freecodecamp.org/news/algorithmic-trading-in-python/)可以定义为设计和开发统计和数学交易策略的过程。这是一个极其复杂的金融领域。

**所以，问题是你如何开始算法交易？**

我将带你了解五个你应该学习的基本话题，为你进入这个迷人的交易世界铺平道路。

我个人更喜欢 Python，因为它提供了适当程度的定制、开发的简易性和速度、测试框架和执行速度。正因为如此，所有这些话题都集中在 [**Python 对于**](https://medium.com/datadriveninvestor/getting-starting-with-algorithmic-trading-with-python-1ae169cc1705) 的交易上。

## 1.学习 [Python 编程](https://www.freecodecamp.org/learn/)

为了在数据科学领域有一个繁荣的职业生涯，你需要坚实的基础。无论您选择哪种语言，您都应该彻底理解该语言中的某些主题。

以下是您应该在 Python 数据科学生态系统中掌握的内容:

*   [**环境设置**](https://towardsdatascience.com/ideal-python-environment-setup-for-data-science-cdb03a447de8)——这包括创建一个虚拟环境，安装所需的软件包，以及[使用 Jupyter notebook](https://towardsdatascience.com/the-complete-guide-to-jupyter-notebooks-for-data-science-8ff3591f69a4) s 或 Google colabs。
*   **数据结构** —一些最重要的 pythonic 数据结构是列表、字典、NumPy 数组、元组和集合。我在链接文章中收集了[的几个例子](https://medium.com/p/python-fundamentals-for-data-science-6c7f9901e1c8)，供你学习。
*   **面向对象编程—** 作为一名定量分析师，你应该确保自己擅长编写结构良好的代码，并定义适当的类。在使用像 Pandas、NumPy、SciPy 等外部包时，您必须学会使用对象及其方法。

freeCodeCamp 课程还提供了一个用 Python 进行[数据分析的认证，帮助你从基础开始。](https://www.freecodecamp.org/learn/data-analysis-with-python/data-analysis-with-python-course/)

## 了解如何处理财务数据

数据分析是金融的重要组成部分。除了学习使用 Pandas 处理数据框架之外，在处理交易数据时，还有一些特定的主题需要注意。

### 如何使用熊猫探索数据

Python 数据科学堆栈中最重要的包之一无疑是 Pandas。您可以使用包中定义的函数完成几乎所有的主要任务。

专注于创建数据帧、过滤(`loc`、`iloc`、`query`)、描述性统计(汇总)、连接/合并、分组和子集化。

### 如何处理时间序列数据

交易数据都是时间序列分析。您应该学会对数据进行重新采样或重新索引，以改变数据的频率，从几分钟到几小时，或者从一天结束时的 OHLC 数据到一周结束时的数据。

例如，您可以使用重采样功能将 1 分钟时间序列转换为 3 分钟时间序列数据:

```
df_3min = df_1min.resample('3Min', label='left').agg({'OPEN': 'first', 'HIGH': 'max', 'LOW': 'min', 'CLOSE': 'last'}) 
```

## 3.如何编写基本的交易算法

从事定量金融需要对统计假设检验和数学有扎实的理解。对多元微积分、线性代数、概率论等概念的深刻理解将有助于你为设计和编写算法打下良好的基础。

你可以从计算股票定价数据的移动平均线开始，编写简单的算法策略，如移动平均线交叉或均值回复策略，并学习相对强弱交易。

在实践和理解基本统计算法如何工作的这一微小但重要的飞跃之后，你可以研究机器学习技术的更复杂的领域。这些都需要对统计学和数学有更深入的理解。

这里有两本书可以让你开始阅读:

*   [量化交易:如何建立自己的算法交易业务](http://www.amazon.com/gp/product/0470284889/ref=as_li_tf_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470284889&linkCode=as2&tag=quant0f-20)—欧内斯特·陈博士
*   巴里·约翰逊写的关于算法交易和 DMA 的书

这里有几门课程可以帮助你开始使用 Python 进行交易，并且涵盖了我在这里捕捉到的大部分主题:

*   [Quantra 提供的用于多商品交易所交易的 Python](https://quantra.quantinsti.com/course/python-for-trading?utm_source=harshit_tyagi&utm_medium=affiliate&utm_campaign=python_finance_article)
*   [Python 算法交易](https://www.freecodecamp.org/news/algorithmic-trading-using-python-course/)——Nick mccull um 在 freeCodeCam YouTube 频道上的 4 小时免费课程

使用我的代码 **HARSHIT10，你可以在 Quantra 课程中获得 10%的折扣。**

## 4.了解回溯测试

一旦你完成了你的交易策略的编码，你就不能简单地用实际的资金在真实的市场中测试它，对吗？

下一步是将这个策略暴露给历史交易数据流，这将产生交易信号。执行的交易将产生相关的利润或损失(P&L)，所有交易的累计将得出总损益。这称为回测。

回溯测试要求你精通许多领域，比如数学、统计学、软件工程和市场微观结构。为了更好地理解回溯测试，您应该学习以下一些概念:

*   可以从了解技术指标开始。探索名为 TA_Lib 的 Python 包来使用这些指示器。
*   采用抛物线 SAR 这样的动量指标，尝试计算交易成本和滑点。
*   学会绘制累积策略回报图，研究策略的整体表现。
*   影响回溯测试性能的一个非常重要的概念是偏差。你应该了解优化偏差、前瞻偏差、心理承受力和生存偏差。

## 5.绩效指标——如何评估交易策略

简明扼要地解释你的策略对你来说很重要。如果你不理解你的策略，那么任何外部监管的改变或体制的改变都有可能让你的策略变得异常。

一旦您自信地理解了该战略，以下绩效指标可以帮助您了解该战略实际上是好是坏:

*   **夏普比率** —启发式地描述策略的风险/回报比率。它量化了股票曲线经历的波动水平所能带来的回报。
*   **波动性** —量化与策略相关的“风险”。夏普比率也体现了这一特点。基础资产的较高波动性通常会导致权益曲线的较高风险，并导致较小的夏普比率。
*   **最大缩减** —该策略权益曲线上最大的总体峰谷百分比下降。最大提取量通常与动量策略一起研究，因为它们会受到动量策略的影响。学习使用`numpy`库计算它。
*   **产能/流动性** —决定进一步增加资本的策略的可扩展性。当资本配置策略增加时，许多基金和投资管理公司会遭遇这些能力问题。
*   **CAGR —** 衡量一个战略在一段时间内的平均增长率。其计算公式为:(交易日累计策略 returns)^(252/number)-1

## 更多资源

这篇文章作为建议的课程，帮助你开始算法交易。这是一个需要掌握的概念列表。

现在的问题是，哪些资源可以帮助您快速了解这些主题？

这里有一些经典书籍和有用的课程，还有我觉得很有帮助的作业和练习:

*   **【课程】** [**由 Quantra**](https://quantra.quantinsti.com/course/python-for-trading?utm_source=harshit_tyagi&utm_medium=affiliate&utm_campaign=python_finance_article)**【promo code:harsh it 10】**提供的多商品交易所交易课程 Python
*   **【书】** [**量化交易:如何打造自己的算法交易业务**](http://www.amazon.com/gp/product/0470284889/ref=as_li_tf_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470284889&linkCode=as2&tag=quant0f-20) **—陈欧内斯特**
*   **【课程】** [**陈欧内斯特博士在 Quantra 平台上的交易课程**](https://quantra.quantinsti.com/courses?utm_source=harshit_tyagi&utm_medium=affiliate&utm_campaign=python_finance_article)
*   **【书】** [**金融巨蟒——伊夫·希尔皮施**](https://www.amazon.in/Python-Finance-Yves-Hilpisch/dp/1491945281)
*   **【期刊】:** [arXiv](http://arxiv.org/archive/q-fin) ，[威利的数理金融](http://onlinelibrary.wiley.com/journal/10.1111/%28ISSN%291467-9965)，[计算金融](http://www.risk.net/type/journal/source/journal-of-computational-finance)。

### [数据科学与 Harshit](https://www.youtube.com/c/DataSciencewithHarshit?sub_confirmation=1)

[https://www.youtube.com/embed/yapSsspJzAw?feature=oembed](https://www.youtube.com/embed/yapSsspJzAw?feature=oembed)

通过这个渠道，我计划推出几个涵盖整个数据科学领域的[系列。以下是你应该订阅](https://towardsdatascience.com/hitchhikers-guide-to-learning-data-science-2cc3d963b1a2?source=---------8------------------)[频道](https://www.youtube.com/channel/UCH-xwLTKQaABNs2QmGxK2bQ)的原因:

*   本系列将涵盖所有必需/要求的高质量教程，涉及每个主题和子主题，如 [Python 数据科学基础](https://towardsdatascience.com/python-fundamentals-for-data-science-6c7f9901e1c8?source=---------5------------------)。
*   解释了为什么我们在 ML 和深度学习中这样做的数学和推导。
*   [与谷歌、微软、亚马逊等公司的数据科学家和工程师](https://www.youtube.com/watch?v=a2pkZCleJwM&t=2s)以及大数据驱动型公司的首席执行官的播客。
*   [项目和说明](https://towardsdatascience.com/building-covid-19-analysis-dashboard-using-python-and-voila-ee091f65dcbb?source=---------2------------------)实施到目前为止所学的主题。了解新的认证、训练营以及破解这些认证的资源，如谷歌的 [**TensorFlow 开发者证书考试。**](https://youtu.be/yapSsspJzAw)

如果这个教程有帮助，你应该看看我在 [Wiplane Academy](https://www.wiplane.com/) 上的数据科学和机器学习课程。它们全面而紧凑，帮助您建立一个坚实的工作基础来展示。