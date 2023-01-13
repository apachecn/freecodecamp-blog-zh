# Python 数据分析:如何用 Pandas、Matplotlib 和 Seaborn 可视化 Kaggle 数据集

> 原文：<https://www.freecodecamp.org/news/kaggle-dataset-analysis-with-pandas-matplotlib-seaborn/>

印度超级联赛或 IPL 是由印度板球管理委员会(BCCI)每年组织的一次 T20 板球锦标赛。八个城市的特许经营相互竞争超过 6 周，以找到赢家。

在这篇文章中，我将分析 IPL 过去几个赛季的数据，看看哪些球队赢得了最多的比赛，当赢得一场比赛时，球队的表现如何，谁的遗产最大，等等。

我从历史的角度做了这个分析，概述了 IPL 这些年来发生的事情。我使用了诸如 **【熊猫】****Matplotlib**和 **Seaborn** 以及**python*n*之类的工具来给出我们面前的数据的视觉和数字表示。

**Pandas** 代表 *Python 数据分析*库。它通常用于处理表格数据(类似于存储在电子表格中的数据)。Pandas 提供了助手功能，可以从各种文件格式读取数据，如 CSV、Excel 电子表格、HTML 表格、JSON、SQL，并对它们执行操作。

Matplotlib 和 **Seaborn** 是两个用于产生绘图的 Python 库。Matplotlib 通常用于绘制线条、饼图和条形图。

Seaborn 用更少的语法和更多的定制提供了一些更高级的可视化特性。在分析过程中，我在它们之间来回切换。

## 目录

1.  [获取数据集](#1-getting-the-dataset)
2.  [数据准备和清理](#2-data-preparation-and-cleaning)
3.  [探索性分析和可视化](#3-exploratory-analysis-and-visualization)
4.  [提问和回答问题](#asking-and-answering-questions)
5.  [从分析中得出的推论](#5-inferences-from-the-analysis)
6.  [结论](#6-conclusion)

## 1.获取数据集

我从 [Kaggle](https://www.kaggle.com/nowke9/ipldata) 下载了数据集。您将看到有两个 CSV(逗号分隔值)文件，matches.csv 和 deliveries.csv。

要找到更多有趣的数据集，可以看看[这个](https://jovian.ml/forum/t/recommended-datasets-for-course-project/11711)页面。

## 2.数据准备和清理

数据集包含许多列和行。对于一个或多个列，某些行总是可能缺少值或`NaN`。

您也可能希望从分析中丢弃某些列或行。您还可以组合两个或更多数据集进行深入分析。

清理数据包括对数据进行更正、删除不必要的列或行、合并数据集等等。

在采取这些步骤之前，我需要安装并导入分析过程中使用的工具(*库*)。我导入了不同别名的库，比如`pd`、`plt`和`sns`。然后我为这些情节设置了一些基本的风格。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=5](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=5)

注意特殊命令`%matplotlib inline`。它确保绘图显示并嵌入 Jupyter 笔记本本身。如果没有此命令，有时弹出窗口中可能会显示出图。

使用来自*熊猫*库的`read_csv()`方法，我加载了 *matches.csv* 文件*。*

文件中的数据被读取并存储在一个`DataFrame`对象中 Pandas 中存储和处理表格数据的核心数据结构之一。我在数据框的变量名中使用了`_df`后缀。

我对数据框使用了名称`matches_raw_df`。这表明这是未处理的数据，我将对其进行清理、过滤和修改，以准备用于分析的数据框。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=9](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=9)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=10](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=10)

使用一个`Dataframe`对象的`shape`属性，我发现数据集包含 756 行和 18 列。为了找到这些列的名称，我使用了`columns`属性。它返回数据框中的列列表。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=11](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=11)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=13](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=13)

为了获得数据帧包含内容的摘要，我使用了`info()`。这提供了关于列、每列中非空值的数量、它们的数据类型和内存使用的信息。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=15](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=15)

除了`umpire3`之外，几乎所有的列都没有或只有很少的空值。空值的出现可能是由于缺少信息或数据输入不正确。

有趣的是，尽管`result`列没有空值，但是`winner`和`player_of_match`列有一些空值。让我们找出原因。

我首先使用*点符号* ( `matches_raw_df.result`)访问了`result`列。然后我在`result`栏上使用了`vaule_counts()`方法。

`value_counts()`返回一个包含唯一值计数的*序列*。这里，它告诉我们`result`中存在的不同值以及每个值的总数。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=18](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=18)

因此，在 756 个匹配(行)中，有 4 个匹配以*无结果*结束。

板球是一项户外运动，不像足球，下雨的时候就不能玩。由于持续下雨，比赛被取消是很平常的事。因此，我们没有这 4 场比赛的赢家或选手。

对于这个分析，不需要`umpire3`列。所以我使用`drop()`方法通过传递列名和轴值来删除列。如果您想要删除多个列，则在列表中给出列名。

我把这个**清理过的** 数据帧分配给了`matches_df`。我用这个数据框做了进一步的分析。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=22](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=22)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=23](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=23)

## 3.探索性分析和可视化

探索性分析包括对数据集执行操作，以了解数据并找到模式。它帮助我们理解我们拥有的数据。

可视化是数据的图形表示。它包括制作图表，将这些数据中的模式传达给观众。

现在，我们来看看我分析的数据，以及在这个过程中我学到了什么。

### 比赛和队伍的数量

我试图找到 IPL 从成立到 2019 年每个赛季的比赛次数。

因为我需要每个赛季的比赛，所以根据不同的赛季对我们的数据进行分组是有意义的。Pandas 有一个`groupby()`方法来实现这一点，其中我将`season`作为参数传递。

由于一个`id`对于每场比赛(行)都是唯一的，所以计算每个赛季的 id 数就可以得出我们想要的结果。我使用了`id`列上的`count()`方法来查找每个赛季举行的比赛数量。这个序列*T5 被分配给变量`matches_per_season`。*

然后我使用 Seaborn 库中的`barplot()`方法来绘制这个系列。序列的指数，即季节，作为 x 值给出，而这些指数的值作为 y 值给出。

我用了`figure()`、`xticks()`、`title()`等各种`matpllotlib.pyplot`方法来设置剧情大小、剧情标题等等。

`figure`接受一个参数`figsize`，我将它设置为`(12,6)`。请注意，大小是以元组的形式给出的。对于`xticks()`，我给了`rotation`参数一个值`75`，以便于阅读。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=30](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=30)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=31](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=31)

每个赛季都有近 60 场比赛。然而，从 2011 年到 2013 年，我们看到了比赛数量的激增。这是因为引入了两个新的专营权，即 ****浦那勇士** s** 和**高知獠牙喀拉拉邦**、，使球队数量增加到 10 支。

然而，高知在第二个赛季就被移除了，而浦那勇士队在 2013 年被移除，从 2014 年起人数下降到 8 人。

2016 赛季开始前， ****钦奈超王**** 和 ****拉贾斯坦皇家**** 两支球队被禁赛两个赛季。为了弥补他们的缺席，两支新的队伍( ****正在崛起的浦那超级巨人**** 和 ****古吉拉特雄狮**** )参加了比赛。

当金奈超级国王队和拉贾斯坦皇家队回来后，这两支球队被从比赛中除名。

### 分析投掷结果

任何板球比赛中最重要的事件之一是掷硬币，它发生在比赛一开始。掷硬币的获胜者可以选择他们是想先击球还是先击球(先防守)。

让我们看看不同赛季的球队有什么样的趋势。

我再次按照季节对行进行分组，然后使用`value_counts()`对`toss_decision`列的不同值进行计数。

由于百分比给出了更清晰的画面，所以我用`matches_per_season`除了上面的结果，再乘以 100。这个系列被分配给了`toss_decision_percentage`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=35](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=35)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=36](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=36)

这里，`toss_decision_percentage`是一个带有*多指标*的数列。如果我们使用`index`属性打印系列的索引，我们会看到它的形式是`(2008, 'bat'), (2008, 'field')`等等。

该系列同时使用了`season`和`toss_decision`作为索引。但我只想让季节成为一个索引。我用`unstack()`来实现这一点。

通过对序列使用`unstack()`方法，它将`toss_decision`(即`bat`和`field`)的值转换为单独的列。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/85&cellId=38](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/85&cellId=38)

接下来，我使用 Matplotlib 中的`plot()`方法将这些值表示为条形图。`plot()`有一个参数`kind`，它决定绘制什么类型的图。该值被设置为`bar`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/85&cellId=39](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/85&cellId=39)

在 2008-2013 赛季，球队似乎都倾向于第一和第二击球。在此期间，球队在 2009 年、2010 年和 2013 年更多地选择先击球。另一方面，他们在 2008 年和 2011 年更多地选择了菲尔丁第一。事情扯平了——2012 年史蒂文。

这可能是因为 IPL 和 T20 板球总体上处于萌芽阶段。因此，团队可能正在学习并试图找出哪个选项更有益。

然而，从 2014 年开始，球队压倒性地选择了第二次击球。特别是 2016 年以来，各队选择赛场第一 ****超过 80%**** 的时间。

击球首先需要团队评估条件和球场，然后相应地设定目标。追逐没有那么复杂，因为有一个固定的目标要实现。

条件也变得更加有利于击球手，击球手的技能也有了巨大的提高(*阅读更多* [*这里*](https://www.espncricinfo.com/story/_/id/18568387/tim-wigmore-how-batting-second-become-more-fruitful-more-popular) )。

### 获胜次数

我们看到在最近的过去，球队选择第二棒的次数超过了 5 次中的 4 次。这个决定改变了结果吗？让我们看看。

对于`wins_batting_first`，`win_by_wickets`的值必须为 0。此外，`result`列应该有一个值`normal`，因为平局比赛的赢差也为 0。该条件被存储为`filter1`。

类似地，对于`wins_fielding_first`,`win_by_runs`的值必须为 0，并且`result`列的值应该为`normal`。该条件被存储为`filter1`。

在这两个系列中，我在`winner`列上使用了`count()`方法来查找过滤条件下的获胜匹配。为了更好地理解，我将结果除以之前计算的`matches_per_season`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=43](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=43)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=44](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=44)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=45](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/88&cellId=45)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/89&cellId=46](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/89&cellId=46)

为了把这两个系列放在一起，我用 Pandas 的`concat()`方法把它们结合起来。我将这两个系列名称作为一个列表传递，并将`axis`的值设置为`1`。这给了我们一个新的数据帧，它被存储为`combined_wins_df`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/89&cellId=47](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/89&cellId=47)

接下来，我使用`plot()`将`combined_wins_df`绘制成条形图。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=44](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=44)

我们之前看到，在 2008-2013 赛季，球队面临着一个难题，是先击球还是先投球。这在结果中也部分可见。

先击球的胜率与先防守的胜率非常接近。然而，在 2013 年情况相同的情况下，只有一个赛季击球第一的球队赢得更多。

还是那句话，从 2014 年开始，除了 2015 年，事情都是有利于车队追逐的。离开 2015 年，事情已经压倒性地有利于球队先发制人。

所以，选择更多场地的队伍在他们的决定中是合理的。

### 有“历史”的团队

在不同体育项目的联赛中，总会有人谈论那些有“历史”的球队——那些在联赛中出场次数最多并且还在继续这样做的球队。让我们在 IPL 中找到那些队。

现在，在两个团队 A 和 B 之间，可能是“A 对 B”或“B 对 A”，这取决于数据输入是如何完成的。所以我决定使用`value_counts()`来计算`team1`和`team2`列中不同值的总数。然后我把它们加在一起。

我用熊猫的`sort_values()`方法对结果进行了降序排序。`ascending`参数被设置为`False`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=48](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=48)

在这里，我使用`sns.barplot()`来绘制图表。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=49](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=49)

孟买的印度人 打了最多的比赛。紧随其后的是皇家挑战者班加罗尔，加尔各答骑士骑士，国王 XI 旁遮普和钦奈超级国王。

钦奈超级国王和拉贾斯坦王室如果没有被禁，可能会更高。

你会看到有两支来自德里的队伍，分别是 ****德里夜魔侠**** 和 ****德里首都**** 。这是 2018 年所有权和团队名称发生变化的结果。

对于 ****德干充电器**** 和 ****太阳升起者海得拉巴**** 来说，这是一个类似的故事，因为德干充电器在 2013 年被从 IPL 中移除，太阳升起者取而代之。

还有两支几乎同名的球队:崛起的浦那超级巨头**和崛起的浦那超级巨头**。他们是同一支球队，所有权没有变化——这更多是因为迷信。****

**2016 赛季，崛起的浦那超级巨人队获得第 7 名。老板们更换了 2017 年的队长，还从超级巨星 ****中去掉了“s”****。嗯，这是值得的，因为他们在那个赛季获得了亚军！**

### **具有“传统”的团队**

**现在，球队可能有很多历史，但这是他们的“遗产”——他们获胜的频率——使他们受欢迎，并吸引新的和中立的球迷。**

**为了找到这样的团队，我简单地在`winner`列上使用了`value_counts()`。这给了我们每支球队获胜的比赛次数。**

 **[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=53](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=53)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=54](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=54)

所以孟买赢的最多。但是一个更好的衡量标准是胜率。为了计算胜率，我用`most_wins`除以`total_matches_played`来计算每个团队的`win_percentage`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=57](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=57)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=58](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=58)

正在崛起的浦那超级巨人和德里首都的胜率最高。这很大程度上是因为与大多数球队相比，他们踢的比赛更少。尤其是上升的浦那超级巨人，技术上成为一个新的团队后，下降的 s。

钦奈超级国王队，尽管比孟买印第安人队少打了两个赛季，也只少赢了 9 场。他们和孟买印度人是 2008 年参加 IPL 的前五名中仅有的两支队伍。

****金奈**** 和 ****孟买**** 是遗产最多的队伍。

## 4.从数据中提问和回答问题

通过研究数据集的各个列，我们已经对 IPL 有了一些了解。

我们来问一些具体的问题，尽量用数据框操作和有趣的可视化来回答。

### 谁赢得了 IPL 锦标赛？

*   使用`groupby()`根据季节对行进行分组。
*   用`tail()`找到每个赛季的最后一场比赛，也就是决赛。它根据位置返回 Dataframe 对象或系列的最后 n 行。
*   使用`sort_values()`对每个季节的值进行排序。
*   用`winner`上的`value_counts()`数不同的赢家和他们赢的次数。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=65](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=65)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=66](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=66)

然后我用`sns.barplot()`绘制了系列`ipl_winners`。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=67](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=67)

孟买和钦奈，我们的 **遗产** 队，已经赢得了 IPL 至少 3 次。海德拉巴太阳队是唯一一支后来加入联盟并赢得奖杯的球队。

### 问:哪支球队在整个赛季中最稳定和最不稳定？

*   使用`pd.crosstab()`在`winner`和`season`的不同值之间创建了一个数据帧。
*   将数据框绘制为热点图。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=71](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=71)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=72](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=72)

`pd.crosstab()`给出了`winner`和`season`列的简单交叉列表。对于`winner`的每个不同值，`pd.crosstab()`在`season`中找到其频率。

然后我用`sns.heatmap()`绘制了`matches_won_each_season`。我传递了数据帧`matches_won_each_season`，用`annot`作为`True`，也显示了值。这里，颜色越深表示赢得的比赛越多。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=73](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=73)

钦奈超级国王队 是最稳定的球队，在他们参加的每个赛季中至少赢了 8 场比赛。事实证明，他们是唯一一支每个赛季都能进入季后赛的**球队。**

**在光谱的另一端是 3 支球队，分别是 ****德里夜魔侠********Xi 国王旁遮普**** 和 ****拉贾斯坦皇家**** 。他们三个都在过去的两个赛季中表现出色。然而，他们在其他赛季表现平平。**

### **问:在 IPL 比赛中，最大的胜率是多少？**

*   **使用所需条件过滤数据帧。**
*   **使用`sort_values()`按降序对值进行排序。**
*   **使用`head()`方法找到列表中最大的 10 场胜利。与`tail()`相反，返回前 n 行。**

 **[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=81](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=81)

我使用`sns.scatterplot()`绘制了过滤后的数据帧`highest_wins_by_runs_df`。对于`x`参数，我使用了`season`，并且我使用了`win_by_runs`作为`y`参数。我使用`s`参数使前 10 名的分数变大。

为了强调 10 大胜利，我使用了不同的颜色，并用`plt.annotate()`标注了这些数据点。第一个参数是注释的文本。要注释的点的位置以元组的形式给出。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=82](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=82)

最大胜率是 ****146 分**** 。2017 年，孟买印度人以这一差距击败了德里夜魔侠。皇家挑战者班加罗尔在前 5 名中有 3 场胜利。

### 问:孟买和钦奈是目前为止最成功的两个车队。哪个队在单挑记录中领先？

*   使用所需条件过滤数据框，以查找两支球队之间的比赛。
*   使用`winner`栏上的`value_counts()`来查找每个队赢了多少次。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=105](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=105)

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=108](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=108)

为了更好的可视化，我将系列`mivcsk`绘制成了一个条形图。

[https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=109](https://jovian.ai/embed?url=https://jovian.ai/srijansrj5901/ipl-data-analysis/v/83&cellId=109)

MI 统治了 CSK，并以 17-11 领先。我们可以看到他们的统治地位，特别是在 2019 赛季，MI 在他们相遇的 4 次比赛中击败了 CSK，包括季后赛和决赛。

## 5.从分析中得出的推论

我们得出了一些有趣的推论，现在比我们开始时更了解 IPL。以下是我们通过分析得出的结论:

*   在每个 IPL 赛季，8 支球队之间会进行近 60 场比赛。
*   有人试图将 IPL 扩大到 10 支球队，但 8 支球队的想法被带了回来，并一直持续至今。
*   在前六个赛季(2008-2013)，各队在掷硬币获胜后，正在计算先击球还是追逐会更好。这可能是因为 IPL 和 T20 板球都处于早期阶段，所以团队正在尝试不同的策略。
*   但是，自 2014 年以来，球队更喜欢追逐，特别是在过去的 4 个赛季(2016-2019)，球队在 5 次比赛中选择了 4 次以上。这很可能是因为有一个固定的总数可以让事情变得简单。这也可能是车队更喜欢在 ODI 中追逐的结果。
*   尽管绝大多数球队选择先上场，但选择击球或上场后的胜率并不是一边倒的。然而，他们的差异正在增加。
*   孟买的印度人参加了最多的 IPL 比赛。由于短暂的扩张，所有者的改变，球队的移除和禁止，已经有 15 支球队参加了 IPL。
*   钦奈和孟买是胜率最高的两支队伍。事实上，他们是第一个赛季仅有的两支进入前五的球队，这显示了他们的统治地位。
*   孟买印度人赢得了 4 次 IPL，最多的一次。排在他们之后的是金奈第三，加尔各答骑士第二。Sunrisers Hyderabad，Deccan Chargers 和 Rajasthan Royals 完成了 IPL 冠军名单，都赢得了一次。
*   146 分是得分最多的胜利。2017 年，孟买印度人以这一优势击败了德里夜魔侠。wickets 获胜的最大优势是 10，这已经实现了很多次。
*   孟买和钦奈这两个重量级城市的正面交锋记录是 17 胜 11 负。孟买在 2019 赛季的每一次相遇中都占据了上风，包括决赛。

## 6.结论

在本文中，我们做了大量的分析，并看到了一些有趣的可视化效果。然而，这只是触及了表面。

您可以对作为独立数据集的 *matches.csv* 执行更有趣的分析。但是将 *deliveries.csv* 与这个数据集结合起来可能会导致更深入的分析。

我做了这个数据分析和可视化项目，作为为期 6 周的课程[用 Python 进行数据分析:零到熊猫](https://www.freecodecamp.org/news/kaggle-dataset-analysis-with-pandas-matplotlib-seaborn/zerotopandas.com)。该课程由 [Jovian.ml](https://jovian.ml) 与[freeCodeCamp.org](https://www.freecodecamp.org/news/kaggle-dataset-analysis-with-pandas-matplotlib-seaborn/www.freecodecamp.org)合作进行。点击查看项目[。](https://jovian.ml/srijansrj5901/ipl-data-analysis)

此外，彩光现在正在进行。去看看吧，好好享受！****