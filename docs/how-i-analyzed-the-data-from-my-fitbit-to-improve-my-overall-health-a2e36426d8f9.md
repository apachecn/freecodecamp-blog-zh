# 我如何分析来自 FitBit 的数据来改善我的整体健康状况

> 原文：<https://www.freecodecamp.org/news/how-i-analyzed-the-data-from-my-fitbit-to-improve-my-overall-health-a2e36426d8f9/>

作者亚什·索尼

# 我如何分析来自 FitBit 的数据来改善我的整体健康状况

#### 原来数据可以让你保持健康

![QZMrnNh5FJCEunBOXlCOllbiiZIZRaub1Kfq](img/2900d8461fc1ed4c299bb40c30050cd6.png)

Photo Credit: [Franki Chamaki](https://unsplash.com/@franki?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

身体活动追踪器已经成为价值数百万美元的产品类别。我也有过不少花哨的追踪器，从早期的[耐克 Fuelband](https://www.wareable.com/fitness-trackers/not-so-happy-birthday-nike-fuelband-2351) 到后来的 [MI 活动乐队](https://www.mi.com/in/miband/)。就我个人而言，我不能很好地采用它们中的任何一个，最终，它们都变成了一个花哨的数字手表，需要每隔几天重新充电才能告诉他们正确的时间。

几个月前，我的一个朋友送了我一个 [Fitbit Versa](https://www.fitbit.com/versa) 。有东西发出了声音。为什么？

自从我放弃健康带有效的想法后，发生了很多事情。当我收到这份礼物时，我正处在一个被粘在椅子和笔记本电脑上的阶段。结果，我有严重的背痛和姿势失衡问题。多年的错误不可能在几天或几周内解决。在与久坐这种现在被许多人认为是新的“吸烟”方式的艰苦斗争中，我开始注意自己的姿势和保持积极生活方式的想法。

所以，我想到给 Fitbit 一个清白的石板，试了一个星期。Fitbit 应用程序在以可消费的格式捕捉和展示数据方面做得非常好。

![OW8rJRXNpRL94YLQyv3H9J3fn6sSmrgCZuRd](img/5be46da6e72f6ae290f13530e3f8bebd.png)![W3lmQTP1gQr6BMKZolZng8t-jm6BUuYdCvpJ](img/ad5a24d6c528bbda1a4b329ffb4b6bc5.png)![xQ6WXzbyd2CoctCHdiKP0Gobe-V8X47BTHTm](img/e27e04a7fbe73cd089c515ffc462863d.png)

Activity, calorie burn and sleep analysis on the Fitbit app.

我和机械表分手已经两个星期了。Fitbit 能够捕捉到的各种各样的数据点引起了人们的兴趣，人们开始迫切地想看看数据背后隐藏着什么。

![mY1YeWyGdMshlqTrJWGh6Lo-RyMmG7Dv395T](img/64c5c2efe1e0edd3bee08241171581a4.png)

The experimentation cycle. [Src](https://thehealthsciencesacademy.org/health-tips/how-to-conduct-an-effective-self-experiment/)

我很快开始了一项实验，看看我能在多大程度上实现我设定的目标。我还想知道是否有任何外部因素影响了这些目标。最后，我想在这个过程中发现任何隐藏的有趣的发现。我特别感兴趣地发现:

*   我的日子有多活跃？我是否花了大量时间久坐不动？
*   这些数据在工作日和周末有何不同？
*   哪些因素导致最高卡路里燃烧？
*   哪些练习是实现我每天目标的最好和最简单的方法？
*   我一直遵循稳定的睡眠时间表吗？有哪些因素影响？
*   了解睡眠阶段，并找出如何获得更好的深度睡眠。
*   网飞狂欢对周末睡眠有什么影响？
*   训练一个简单的机器学习模型，看看是否有隐藏的模式来获得更好的睡眠。

我无法从标准的 Fitbit 应用程序中找到这些问题的答案。我需要原始数据。

### 获取数据

第一项任务是找出从我的设备中提取数据的方法。浏览开发者页面，我发现他们提供了 [Web API](https://dev.fitbit.com/build/reference/web-api/) 来访问用户数据。检查这些 API 时，我震惊地看到每分钟捕获和保存的数据量之大。走过的步数，消耗的卡路里，睡眠阶段，甚至任何一天的[心率/分钟](https://dev.fitbit.com/build/reference/web-api/heart-rate/)都被记录下来！

有时，了解我们总体健康状况的诱人诱惑让我们忘记了我们最终分享了哪些个人信息。通读他们的隐私政策，我发现 Fitbit 已经进行了额外的检查以保证数据的安全。不管怎样，这需要一个单独的帖子，所以不要偏离我们的主要目标，让我们继续。

我注册了我的应用程序，并获得了必要的客户端凭证，开始数据抓取。经过必要的授权步骤后，我收集并合并了我的日常活动、睡眠和心率数据，并将其转储到一个 Excel 文件中。在一些数据清理之后，数据集就准备好了！

![fkeipr40UH05TkJaRBwOsL8wbP2DLSmhbWN2](img/0affe9820c0278d62dc8eb45914d3df9.png)![3qRwsTxyxlSj-bS7E7Nrq4cyYnmPjim7Eoeh](img/b240c33f0f4fff119f2884b75cf0de79.png)

Sample data grabber session and the corresponding Pandas DataFrame

*PS。全部代码可以在[这里](https://github.com/yashatgit/fitbit-analyzer)以及 [Jupyter 笔记本](https://github.com/yashatgit/fitbit-analyzer/blob/master/Fitbit_Data_Analysis.ipynb)找到。*

*PPS。免责声明:该数据分析基于非常有限的一组数据点，很难推广到大众。请把它当作一个有趣的阅读！*

### 活性分析

Fitbit 有大量的数据点来衡量日常活动水平。步数、卡路里和地板是一些标准的措施。它还记录了我每天中度、轻度和非常活跃的时间。

我没有对每天消耗的卡路里大惊小怪，我在我的 Fitbit 设备上保持了每天达到 8000 步的目标。下面的图表显示，我平均每天走大约 7800 步，非常接近我的目标。有一些研究表明，每天达到 [10000](https://blog.fitbit.com/should-you-really-take-10000-steps-a-day/) [步是理想的](https://www.huffingtonpost.ca/leigh-vanderloo/10000-steps-a-day_b_16077702.html)，这将是下一个目标。

周二到周六是我平均 40 分钟非常活跃的时间——，简单地翻译为[积极锻炼](https://help.fitbit.com/articles/en_US/Help_article/1853/?l=en_US&c=Topics%3ADashboard&fs=Search&pn=1)。周日更少的时间纯粹是由于懒惰/恢复时间。周一*活跃分钟*的下降证明我陷入了周一忧郁，我猜是时候解决这个问题了。？？

![pvzvoW8pbiJ9an-55Rh8CnNqI4BvCxoLynJM](img/9a23c7dda889d4b1c3ae74bcd14814de.png)

通过分析各种活动每分钟消耗的卡路里量，我们发现了一些有趣的现象。虽然互联网上有很多类似的数据，但很难对每个人概括这些数字。因为这在很大程度上取决于健康水平、人口统计、技能组合，最重要的是，取决于我有多喜欢做一些特定的运动。

![YFwjh8mzMQ5EzyNzTR9W2iNUcxJwwbIZwxmA](img/0347fa1da7345c5e0d48797ea86aae70.png)

有趣的是，跑步帮助我每分钟燃烧 12 卡路里。数学很简单:为了补偿一瓶啤酒，我需要跑 10 分钟。？？‍+ ?= ?

网球？最受欢迎的活动位居第二。这又是一个双赢的局面！当我提高我的技能时，看看这个数字是否会改变将会很有趣。

游泳数据并没有让我震惊，因为我仍然在努力跟上我连续的圈数。在泳池里泡了一段时间后，锻炼变成了一种休闲活动。

这里要注意的一点是，燃烧的卡路里不应该是这些活动分级的唯一标准。但是，这恰好是我目前可以通过 Fitbit 衡量的唯一指标。

最后，了解各种数据点如何相互关联是很有用的。绘制相关热图有助于揭示一些发现。

![fV5HQJGs-Wj6sMf0WF2-S6i5yrzAFyzGaesg](img/a4ee5e722c9b54dfaea1f5a4e5ca7120.png)

燃烧的卡路里与*步数*和*活动分钟数*密切相关。*久坐的分钟数*与工作日成负相关，这意味着我在周末会花更多时间偷懒。

### 睡眠分析

睡眠对帮助维持情绪、记忆和认知能力至关重要，不可逃避。我们一生中大约有三分之一的时间在睡觉。这是一个惊人的 26 年睡在床上的时间！虽然新陈代谢普遍放缓，但所有主要器官和调节系统仍继续发挥作用。因此，充分利用睡眠变得很重要。

读了更多这方面的内容，我发现有一些标准的方法可以帮助获得良好的睡眠。

*   [遵循良好的睡眠时间表](http://time.com/3183183/best-time-to-sleep/)
*   [晚上睡觉前避免强光/蓝光](https://justgetflux.com/research.html)
*   当天晚些时候避免摄入咖啡因
*   睡在凉爽黑暗的房间里
*   获得至少 7-9 个小时的睡眠。有一些研究表明，即使在[的 5 小时](https://blog.bulletproof.com/sleep-hacking-1-million-people-prove-sleeping-5-hours-is-healther-than-sleeping-8-hours/)里，你也能从睡眠中获得最大的收获。

在这个实验过程中，我试着按照上面的步骤来约束自己严格的睡眠时间表。是验证它们的时候了。

从下面的图表中，我发现我平均睡了 7 个小时，数字没有太大的偏差。虽然我能在 11 点前上床睡觉，但起床时间仍然在 5:30-7:00 之间。

![1aoBJ18QRW91xMe3VeYvnXk0MaJKt34VSQNZ](img/66ebc9cbe9c65ab6ba144f502a96ed00.png)![NSDjE1w-cbpmfyQ0s4sW2cJJtmlpNGMEOWgx](img/28027ae63f9e2a1e2ba762b851d8b072.png)

尽管平均持续时间有些相似，但总体睡眠质量并不相同。有几天，我非常活跃，甚至达到了 6 个小时的睡眠，而有许多情况下，即使睡得晚，我也不觉得新鲜。我通过分析神秘的[](https://www.sleepfoundation.org/how-sleep-works)**睡眠周期找到了答案。**

**当我们睡觉时，我们的身体通常会经历几个睡眠周期，在以下阶段之间交替:**

**浅睡眠:这个阶段通常在入睡后几分钟内开始。在此阶段，呼吸和心率通常会略有下降。浅睡眠促进精神和身体的恢复。**

**深度睡眠:深度睡眠通常发生在睡眠的最初几个小时。呼吸变得缓慢，肌肉放松，而心率通常变得更加规律。当我们早上醒来感觉神清气爽时，很可能我们已经经历了长时间的深度睡眠。深度睡眠有助于身体恢复，提高记忆力和学习能力。**

**快速眼动睡眠:快速眼动睡眠是一种活跃的睡眠期，以大脑的剧烈活动为标志。快速眼动睡眠的第一阶段通常发生在深度睡眠的初始阶段之后。呼吸更加急促、不规则、浅。眼睛向各个方向快速移动，因此得名快速动眼期——快速眼动睡眠。这是我们一般在睡梦中看到梦的阶段。快速眼动睡眠已被证明在情绪调节、学习和记忆中起着重要作用。**

**下图显示，平均而言，我的身体只有 17%处于深度睡眠状态，19%处于快速眼动状态，其余时间处于轻度或微醒状态。浅睡眠和深睡眠的日期时间图显示，这些数字变化很大。**

**![5hV7luHn9ANVZ2daqzbNIKec9YOd3t-gaFY9](img/0426cc51af6154fbf3e87a8462a8201f.png)****![oSi2W4JKlla6RB1ufA0BBWvO-DvS3SOXiy6S](img/9f412b20ef70a53e8e337c7469806afe.png)**

**如果我们绘制不同睡眠阶段的相关性，我们会发现在床上度过的时间与浅睡眠高度相关，但与深睡眠没有很强的相关性。**

**![mdlNPY-xnzrhFsQCvhY1BK55IojcCPG7aINE](img/d308c0a94f9a74cc945c37a27c08424f.png)**

**这实质上意味着仅仅多睡并不总是能保证良好的深度睡眠。我想这有助于验证关于睡眠的重要知识:**

> **重要的是睡眠的质量，而不是数量。**

**在工作日遵循严格的睡眠时间表很容易，但是周末就完全不同了。**

**![lP-H1bLB-YwClB0r0vSmoFBCG6Ifq45ul7Gw](img/02493c7c44cdd107fb96446d8c72b6a4.png)**

**上面的方框图显示，周六受影响最大，人们在床上的时间从 5-9 小时不等。网飞狂欢和周末派对是影响这种日常生活的一些恶习。相反，周日较小的方框图描绘了我为周一早上做准备。看到这些潜意识的身体行为如何在这些情节中清晰地暴露出来是很有趣的。**

**最后，我想看看我的日常活动对我的睡眠有没有影响。虽然我没有太多机器学习模型的数据，但最初的运行显示了一些有趣的结果。该研究预测，白天积极活动并在晚上 11 点前上床睡觉对最后的深度睡眠有一些积极的贡献。**

**虽然现在验证它还为时过早，但一旦我有了更多的睡眠数据和额外的功能来提高模型的准确性，我会再次重复这一点。详情可在[本 Jupyter 笔记本](https://github.com/yashatgit/fitbit-analyzer/blob/master/Fitbit_Data_Analysis.ipynb)中找到。**

#### **这一切值得吗？**

**这个实验是一次有益的经历。我发现了我的身体对外界刺激做出反应的一些有趣的方式。它就像一台机器，调整某些旋钮可以帮助实现不同的结果。**

**接下来，我计划设立一些改进的活动目标。我还将尝试一些 T2 生物黑客技术，看看它们对我的睡眠质量是否有积极的影响。我还在考虑开发一个 Fitbit 闹钟应用程序，它只会在我获得足够的高质量睡眠时叫醒我(不确定这是否已经存在？).**

**最后，我不打算再把这称为一个实验。开始时觉得很有力量的日常事务现在已经变成了一种习惯。在过去，我也看到过许多研究早起重要性的文章，最后我直接接受了它。这篇中型文章，实际上是我的第一篇，是这个新发现的习惯的许多副产品之一。**

**谢谢你花时间看我的分析。我将非常感谢知道你是否喜欢它，并有任何改进的建议！:)**