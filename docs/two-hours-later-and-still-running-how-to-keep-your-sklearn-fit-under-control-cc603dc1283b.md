# 两个小时后还在跑？如何让你的 sklearn.fit 处于掌控之中？

> 原文：<https://www.freecodecamp.org/news/two-hours-later-and-still-running-how-to-keep-your-sklearn-fit-under-control-cc603dc1283b/>

内森·图比亚纳

# 两个小时后还在跑？如何控制你的 sklearn.fit

*作者[加布里埃尔·勒纳](https://medium.com/@gabi10004)和[内森·图比亚纳](https://medium.com/@toubiana.nathan)*

您想做的只是测试您的代码，然而两个小时后，您的 Scikit-learn fit 没有显示出任何完成的迹象。Scitime 是一个预测机器学习算法运行时间的包，这样你就不会被无休止的拟合弄得措手不及。

![1*aVzJTznRRfP1lM7AXe9yLw](img/a0c90a79fc0c766b3e35f7bd2a0f4bd0.png)

Image by Kevin Ku on [unsplash.com](https://unsplash.com/photos/aiyBwbrWWlo)

无论您是在构建机器学习模型的过程中，还是在将代码部署到生产中，了解算法需要多长时间才能适应是简化工作流程的关键。使用 Scitime，您可以在几秒钟内估计出最常用的 Scikit 学习算法的拟合时间。

已经有几篇关于那个主题的研究文章发表了(比如[这篇](https://www.sciencedirect.com/science/article/pii/S0004370213001082))。然而，据我们所知，还没有实际的实现。这里的目标不是预测算法的准确运行时间，而是给出一个粗略的近似值。

### 什么是 Scitime？

Scitime 是一个 python 包，要求至少是 python 3.6，带有[熊猫](https://github.com/pandas-dev/pandas)、 [scikit-learn](https://github.com/scikit-learn/scikit-learn) 、 [psutil](https://github.com/giampaolo/psutil) 和 [joblib](https://github.com/joblib/joblib) 依赖项。你可以在这里找到 sci time repo。

这个包中的主要函数叫做“ *time* ”。给定矩阵向量 X、估计向量 Y 以及您选择的 Scikit 学习模型， *time* 将输出估计时间及其置信区间。该软件包目前支持以下 Scikit 学习算法，并计划在不久的将来添加更多算法:

*   [k 表示](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)
*   [随机应变回归者](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)
*   [SVC](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)
*   随机应变分类器

### 快速启动

让我们安装软件包并运行基础程序。

首先创建一个新的 virtualenv(这是可选的，以避免任何版本冲突！)

```
❱ virtualenv env❱ source env/bin/activate
```

然后运行:

```
❱ (env) pip install scitime
```

或使用康达:

```
❱ (env) conda install -c conda-forge scitime
```

一旦安装成功，您就可以估计第一个算法的时间了。

例如，假设您想要训练一个 kmeans 集群。您首先需要导入 scikit-learn 包，设置 kmeans 参数，并选择输入(又名 *X)* ，为了简单起见，这里随机生成了。

在进行实际拟合之前运行这个函数将会给出运行时间的近似值:

正如您所看到的，您只需要一行额外的代码就可以获得这些信息！ *time* 函数的输入正是运行拟合所需要的(即算法本身和 X)，这使得它更容易使用。

仔细看看上面代码的最后一行，第一个输出(*估计值:*本例中为 15 秒)是您正在寻找的预测运行时。Scitime 还会输出一个置信区间(本例中的*下限*和*上限:* 10 和 30 秒)。您可以通过运行以下命令将其与实际训练时间进行比较:

在这种情况下，在我们的本地机器上，估计是 15 秒，而实际的训练时间是 20 秒(但是您可能不会得到相同的结果，我们将在后面解释)。

**快速使用指南:**

*估计器(meta_algo，verbose，confidence)类:*

*   **meta_algo** :用于预测时间的估计器，可以是“RF”或“NN”(详见下一段)——默认为“RF”
*   **verbose** :控制日志输出量(0、1、2 或 3) —默认为 0
*   **置信度**:区间的置信度——默认为 95%

*estimator.time(algo，X，y)函数:*

*   **算法**:用户想要预测其运行时间的算法
*   **X** :要训练的输入的 numpy 数组
*   **y** :要训练的输出的 numpy 数组(如果算法是无监督的，则设置为*无*

快速提示:为了避免任何混淆，值得强调的是， **algo** 和 **meta_algo** 在这里是两个不同的东西: **algo** 是我们想要估计其运行时间的算法， **meta_algo** 是 Scitime 用来预测运行时间的算法。

### sitime 如何工作

我们能够通过使用我们自己的估计器来预测运行时间，我们称之为元算法( *meta_algo* )，其权重存储在包元数据中的专用 pickle 文件中。对于每个 Scikit 学习模型，您将在 Scitime 的代码库中找到相应的 meta algo pickle 文件。

你可能会想:

> 为什么不用大 O 符号手动估计时间复杂度？

那是一个公平的观点。这是解决问题的有效方法，也是我们在项目开始时就考虑的事情。然而，有一件事是，我们需要为每个算法和参数集明确地制定复杂性，这在某些情况下是相当具有挑战性的，因为有许多因素在运行时发挥作用。meta_algo 基本上为你做了所有的工作，我们将解释如何做。

已经训练了两种类型的元算法来估计适合的时间(都来自 Scikit Learn):

*   **RF** meta algo，一个 [RandomForestRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html) 估计器。
*   **NN** meta algo，一个基本的 [MLPRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPRegressor.html) 估计器。

这些元算法使用一系列“元”特征来估计拟合时间。以下是我们如何构建这些功能的总结:

首先，我们获取您的输入矩阵 X 和输出向量 y 的形状。其次，您提供给 Scikit Learn 模型的参数也被考虑在内，因为它们也会影响训练时间。最后，还会考虑您的机器特有的特定硬件，如可用内存和 cpu 数量。

如前所述，我们还提供了时间预测的置信区间。计算方法取决于所选的元算法:

*   对于 **RF** ，由于任何随机森林回归量都是多棵树的组合(也称为*估计量*)，置信区间将基于每个估计量计算的预测集的分布。
*   对于 **NN** ，这个过程有点不太直接:我们首先计算一组 [MSE](https://en.wikipedia.org/wiki/Mean_squared_error) 以及一个测试集上的观察数量，按预测的持续时间区间(即从 0 到 1 秒，1 到 5 秒，等等)分组，然后我们计算一个 [t-stat](https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.stats.t.html) 来获得估计的下限和上限。由于我们没有非常长的模型的大量数据，这些数据的置信区间可能会变得非常宽。

### 我们如何建造它

你可能会想:

> 您是如何在各种参数和硬件配置下获得所有这些 sciki- learn fits 训练时间的足够数据的？

(乏味的)答案是，我们自己使用计算机和虚拟机硬件的组合来生成数据，以模拟不同系统上的训练时间。然后，我们在这些随机生成的数据点上拟合我们的元算法，以建立一个无论您的系统如何都可靠的估计器。

虽然 [estimate.py](https://github.com/nathan-toubiana/scitime/blob/master/scitime/estimate.py) 文件处理运行时预测，但是 [*_model.py*](https://github.com/nathan-toubiana/scitime/blob/master/scitime/_model.py) 文件使用我们专用的模型类帮助我们生成数据来训练我们的元算法。下面是针对 kmeans 的相应代码示例:

注意，你也可以通过命令行直接使用文件 [*_data.py*](https://github.com/nathan-toubiana/scitime/blob/master/_data.py) 来生成数据或者训练一个新的模型。相关说明可在 repo 自述文件中找到。

生成数据点时，您可以编辑想要训练的 Scikit 学习模型的参数。你可以进入[*sci time/_ config . JSON*](https://github.com/nathan-toubiana/scitime/blob/master/scitime/_config.json)编辑模型的参数以及你想要训练的行数和列数。

我们使用一个 [itertool](https://docs.python.org/2/library/itertools.html#itertools.product) 函数来遍历每一个可能的组合，同时设置一个介于 0 和 1 之间的丢弃率来控制循环在不同的可能迭代中的跳转速度。

### Scitime 有多准确？

下面，我们重点介绍我们的预测在特定情况下的表现。我们生成的数据集包含大约 100k 个数据点，我们将其分成一个训练集和测试集(75% — 25%)。

我们根据不同的时间段对训练预测时间进行分组，并使用 RF 元算法和 NN 元算法计算所有估计器在每个时间段的 [MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) 和 [RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation) 。

请注意，这些结果是在受限数据集上执行的，因此它们在未探索的数据点上可能有所不同(例如其他系统/某些模型参数的极值)。对于这个特定的训练集，[的 R 平方](https://en.wikipedia.org/wiki/Coefficient_of_determination)大约是 NN 的 80%和 RF 的 90%。

正如我们可以看到的，毫不奇怪，无论是 NN 还是 RF，训练集的精度始终高于测试集。我们还看到 RF 的整体表现似乎比 NN 好得多。射频的 MAPE 在训练装置上约为 20%,在测试装置上约为 40%。NN MAPE 出奇的高。

让我们用预测的秒数来划分 MAPE(在测试集上):

需要记住的一件重要事情是，在某些情况下，时间预测对所选的元算法(RF 或 NN)很敏感。根据我们的经验，RF 在数据集输入范围内表现非常好，如上所示。然而，对于超出范围的点，NN 可能表现得更好，正如上图末尾所示。这可以解释为什么 NN MAPE 相当高，而 RMSE 却相当不错:它在小数值上表现不佳。

例如，如果您尝试使用默认参数和几千行的输入矩阵来预测 kmeans 的运行时间，RF meta algo 将是精确的，因为我们的训练数据集包含类似的数据点。然而，对于预测非常具体的参数(例如，非常多的聚类)，NN 可能会表现得更好，因为它是从训练集外推的，而 RF 则不是。NN 在上面的图表中表现较差，因为这些图仅基于接近训练数据输入集的数据。

然而，如此图所示，超范围值(细线)由 NN 估计器外推，而 RF 估计器逐步预测输出。

现在让我们来看看 kmeans 示例中最重要的“元”特性:

正如我们所看到的，只有 6 个特征占模型方差的 80%以上。其中，最重要的是 scikit-learn kmeans 类本身的一个参数(集群数)，但很多外部因素对运行时有很大影响，如行数/列数和可用内存。

### 限制

如前所述，第一个限制与置信区间有关:它们可能非常宽，尤其是对于 NN 和重模型(这至少需要一个小时)。

此外，神经网络在中小规模的预测中可能表现不佳。有时，对于小的持续时间，神经网络甚至可能预测负的持续时间，在这种情况下，我们自动切换回 RF。

当使用“特殊”算法参数值时，估计器的另一个限制出现了。例如，在 RandomForest 场景中，当 max_depth 设置为 *None* 时，深度可以取任何值。这可能会导致更长的时间来适应，这对于元算法来说更加困难，尽管我们尽了最大努力来解决它们。

当运行 *estimator.time(algo，X，y)* 时，我们要求用户输入实际的 X 和 y 向量，这似乎是不必要的，因为我们可以简单地请求数据的形状来估计训练时间。这样做的原因是，我们实际上在预测运行时间之前试图拟合模型，以便引发任何即时错误。我们在一个子流程中运行 *algo.fit(X，y)* 一秒钟，以检查任何拟合误差，之后我们进入预测部分。然而，有时候算法(和/或输入矩阵)太大，以至于运行 *algo.fit(X，y)* 最终会抛出一个内存错误，这是我们无法解释的。

### 未来的改进

提高我们当前预测性能的最有效和最明显的方法是在不同的系统上生成更多的数据点，以更好地支持广泛的硬件/参数。

我们将在不久的将来考虑添加更多受支持的 Scikit 学习算法。我们也可以实现其他算法，比如 [lightGBM](https://github.com/Microsoft/LightGBM) 或者 [xgboost](https://github.com/dmlc/xgboost) 。如果您希望我们在 Scitime 的下一次迭代中实现某个算法，请随时联系我们！

提高估计器性能的其他有趣途径是包括更多关于输入矩阵的粒度信息，如方差或与输出的相关性。我们目前完全随机地生成数据，其拟合时间可能比真实世界数据集更长。所以在某些情况下，它可能高估了训练时间。

此外，我们可以跟踪更精细的硬件特定信息，如 cpu 的频率或当前 cpu 的使用情况。

理想情况下，由于算法可能会从一个 scikit-learn 版本变为另一个版本，从而对运行时产生影响，因此我们也会考虑到这一点，例如通过将该版本用作“元”特性。

随着我们获取更多的数据来适应我们的元算法，我们可能会考虑使用更复杂的元算法，如复杂的神经网络(使用正则化技术，如丢弃或批量归一化)。我们甚至可以考虑使用 [tensorflow](https://www.tensorflow.org) 来拟合元算法(并将其添加为可选):这不仅可以帮助我们获得更好的精度，还可以使用[丢弃](https://towardsdatascience.com/uncertainty-estimation-for-neural-network-dropout-as-bayesian-approximation-7d30fc7bc1f2)来建立更稳健的置信区间。

### 向 sitime 投稿并向我们发送您的反馈

首先，任何形式的反馈，尤其是关于预测性能和改进数据生成过程的想法，都是非常受欢迎的！

如前所述，您可以使用我们的 repo 来生成您自己的数据点，以便训练您自己的元算法。这样做时，您可以通过共享您在结果 CSV(*~/sci time/sci time/[algo]_ results . CSV*)中找到的数据点来帮助改进 Scitime，以便我们可以将其集成到我们的模型中。

要生成您自己的数据，您可以运行与此类似的命令(从软件包回购源):

```
❱ python _data.py --verbose 3 --algo KMeans --drop_rate 0.99
```

注意:如果直接使用代码源运行(使用*模型*类)，不要忘记将 *write_csv* 设置为 true，否则生成的数据点不会被保存。

我们使用 GitHub 问题来跟踪所有的 bug 和功能请求。如果你发现了一个 bug 或者希望看到一个新特性的实现，请随时提出问题。更多信息可以在 Scitime repo 中找到。

*对于培训时间预测的问题，在提交反馈时，包括适合您的模型的完整参数字典可能会有所帮助，这样我们就可以诊断为什么性能低于您的特定用例。为此，只需将 verbose 参数设置为 3，并将参数 dic 的日志复制粘贴到问题描述中。*

*找到[代码来源](https://github.com/nathan-toubiana/scitime)*

*找到[文档](https://scitime.readthedocs.io)*

### 信用

*   [*Gabriel Lerner*](https://github.com/gabrielRTR)*&[Nathan Toubiana](https://github.com/nathan-toubiana)是这个包的主要贡献者和这篇文章的合著者*
*   *特别感谢 [Philippe Mizrahi](https://github.com/philippemizrahi) 一路上的帮助*
*   *感谢我们从早期评论/ beta 测试中得到的所有帮助*