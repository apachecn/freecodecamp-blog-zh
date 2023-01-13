# 所有数据科学家都应该知道的机器学习中的 8 种聚类算法

> 原文：<https://www.freecodecamp.org/news/8-clustering-algorithms-in-machine-learning-that-all-data-scientists-should-know/>

机器学习有三种不同的方法，取决于你拥有的数据。你可以选择监督学习、半监督学习或无监督学习。

在监督学习中，您已经标记了数据，因此您可以确定输出是输入的正确值。这就像根据品牌、型号、风格、传动系统和其他属性来了解汽车价格一样。

使用半监督学习，你有一个大数据集，其中一些数据被标记，但大多数数据没有被标记。

这涵盖了大量真实世界的数据，因为让专家标记每个数据点可能会很昂贵。您可以通过结合使用监督学习和非监督学习来解决这个问题。

无监督学习意味着你有一个完全未标记的数据集。你不知道数据中是否隐藏着任何模式，所以你把它留给算法去寻找它能找到的任何东西。

这就是聚类算法的用武之地。这是在无监督学习问题中可以使用的方法之一。

## 什么是聚类算法？

聚类是一项无监督的机器学习任务。由于这种方法的工作方式，您可能还会听说这种方法被称为聚类分析。

使用聚类算法意味着您将为算法提供大量没有标签的输入数据，并让它尽可能在数据中找到任何分组。

这些分组被称为*簇*。聚类是一组基于与周围数据点的关系而彼此相似的数据点。聚类用于功能工程或模式发现之类的事情。

当您从您一无所知的数据开始时，聚类可能是获得一些洞察力的好地方。

## 聚类算法的类型

有不同类型的聚类算法可以处理各种独特的数据。

### 基于密度

在基于密度的聚类中，数据按被低密度数据点包围的高密度数据点区域进行分组。基本上，该算法会找到数据点密集的地方，并将其称为聚类。

最棒的是，集群可以是任何形状。你不受预期条件的约束。

这种类型下的聚类算法不会试图将离群值分配给聚类，因此它们会被忽略。

### 基于分布的

使用基于分布的聚类方法时，所有数据点都被视为一个聚类的一部分，这是基于它们属于给定聚类的概率。

它是这样工作的:有一个中心点，随着数据点离中心的距离增加，它成为该簇的一部分的概率降低。

如果您不确定数据的分布情况，您应该考虑不同类型的算法。

### 基于质心的

基于质心的聚类可能是你听到最多的一种。对你给它的初始参数有点敏感，但是很快很高效。

这些类型的算法基于数据中的多个质心来分离数据点。每个数据点根据其距质心的平方距离被分配给一个聚类。这是最常用的聚类类型。

### 基于层次的

基于分层的聚类通常用于分层数据，就像从公司数据库或分类法中获得的数据一样。它构建了一个集群树，因此一切都是自上而下组织的。

这比其他聚类类型更具限制性，但它非常适合特定种类的数据集。

## 何时使用集群

当你有一组未标记的数据时，很可能你会使用某种无监督学习算法。

有许多不同的无监督学习技术，如神经网络、强化学习和聚类。您想要使用的算法的具体类型将取决于您的数据看起来像什么。

当您尝试进行异常检测以尝试发现数据中的异常值时，您可能希望使用聚类。它有助于找到这些聚类组，并显示确定数据点是否为异常值的边界。

如果您不确定要为机器学习模型使用什么功能，聚类会发现一些模式，您可以使用这些模式来找出数据中突出的部分。

聚类对于探索您一无所知的数据尤其有用。找出哪种类型的聚类算法效果最好可能需要一些时间，但是当您这样做时，您将获得对数据的宝贵见解。你可能会发现你从未想过的联系。

聚类的一些实际应用包括保险业中的欺诈检测、图书馆中的图书分类以及市场营销中的客户细分。它也可以用于更大的问题，如地震分析或城市规划。

## 8 大聚类算法

现在，您已经对聚类算法的工作原理和可用的不同类型有了一些背景知识，我们可以谈谈您在实践中经常看到的实际算法了。

我们将在 Python 中的 [sklearn 库](https://scikit-learn.org/stable/)的一个示例数据集上实现这些算法。

我们将使用 sklearn 库中的 *make_classification* 数据集来演示不同的聚类算法如何不适用于所有的聚类问题。

你可以在这里找到下面所有例子的代码。

### k-均值聚类算法

K-means 聚类是最常用的聚类算法。这是一种基于质心的算法，也是最简单的无监督学习算法。

该算法试图最小化聚类中数据点的方差。这也是大多数人了解无监督机器学习的方式。

K-means 最适用于较小的数据集，因为它迭代所有的数据点。这意味着如果数据集中有大量的数据点，将需要更多的时间来分类。

因为这是 k-means 聚类数据点的方式，所以它不能很好地扩展。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import KMeans

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
kmeans_model = KMeans(n_clusters=2)

# assign each data point to a cluster
dbscan_result = dbscan_model.fit_predict(training_data)

# get all of the unique clusters
dbscan_clusters = unique(dbscan_result)

# plot the DBSCAN clusters
for dbscan_cluster in dbscan_clusters:
    # get data points that fall in this cluster
    index = where(dbscan_result == dbscan_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the DBSCAN plot
pyplot.show()
```

### DBSCAN 聚类算法

DBSCAN 代表对有噪声的应用程序进行基于密度的空间聚类。这是一种基于密度的聚类算法，不像 k-means。

这是一个在数据集中寻找离群点的好算法。它根据不同区域中数据点的密度找到任意形状的聚类。它通过低密度区域来分隔区域，以便能够检测高密度聚类之间的异常值。

在处理形状奇怪的数据时，该算法优于 k-means。

DBSCAN 使用两个参数来确定如何定义聚类: *minPts* (对于要被视为高密度的区域，需要聚类在一起的数据点的最小数量)和 *eps* (用于确定数据点是否与其他数据点位于同一区域的距离)。

选择正确的初始参数对于该算法的运行至关重要。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import DBSCAN

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
dbscan_model = DBSCAN(eps=0.25, min_samples=9)

# train the model
dbscan_model.fit(training_data)

# assign each data point to a cluster
dbscan_result = dbscan_model.predict(training_data)

# get all of the unique clusters
dbscan_cluster = unique(dbscan_result)

# plot the DBSCAN clusters
for dbscan_cluster in dbscan_clusters:
    # get data points that fall in this cluster
    index = where(dbscan_result == dbscan_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the DBSCAN plot
pyplot.show()
```

### 高斯混合模型算法

k-means 的一个问题是数据需要遵循循环格式。k-means 计算数据点之间距离的方式与圆形路径有关，因此非圆形数据不会正确聚类。

这是高斯混合模型解决的问题。你不需要圆形的数据就能很好地工作。

高斯混合模型使用多个高斯分布来拟合任意形状的数据。

在这个混合模型中，有几个单高斯模型充当隐藏层。因此，该模型计算数据点属于特定高斯分布的概率，这就是它将落入的聚类。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.mixture import GaussianMixture

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
gaussian_model = GaussianMixture(n_components=2)

# train the model
gaussian_model.fit(training_data)

# assign each data point to a cluster
gaussian_result = gaussian_model.predict(training_data)

# get all of the unique clusters
gaussian_clusters = unique(gaussian_result)

# plot Gaussian Mixture the clusters
for gaussian_cluster in gaussian_clusters:
    # get data points that fall in this cluster
    index = where(gaussian_result == gaussian_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the Gaussian Mixture plot
pyplot.show()
```

### 母狗算法

使用层次结构的平衡迭代缩减和聚类(BIRCH)算法在大型数据集上比 k-means 算法工作得更好。

它将数据分解成小的汇总，这些汇总代替了原始数据点。摘要包含尽可能多的关于数据点的分布信息。

该算法通常与其他聚类算法一起使用，因为其他聚类技术可以用于 BIRCH 生成的摘要。

BIRCH 算法的主要缺点是它只对数字数据值有效。除非进行一些数据转换，否则不能将它用于分类值。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import Birch

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
birch_model = Birch(threshold=0.03, n_clusters=2)

# train the model
birch_model.fit(training_data)

# assign each data point to a cluster
birch_result = birch_model.predict(training_data)

# get all of the unique clusters
birch_clusters = unique(birch_result)

# plot the BIRCH clusters
for birch_cluster in birch_clusters:
    # get data points that fall in this cluster
    index = where(birch_result == birch_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the BIRCH plot
pyplot.show() 
```

### 相似传播聚类算法

这种聚类算法在对数据进行聚类的方式上与其他算法完全不同。

每个数据点与所有其他数据点通信，让彼此知道它们有多相似，并开始揭示数据中的聚类。您不必告诉该算法在初始化参数中预期有多少个集群。

当消息在数据点之间发送时，被称为*样本*的数据集被发现，并且它们代表聚类。

在数据点已经相互传递消息并且形成关于什么数据点最好地代表一个聚类的共识之后，发现一个样本。

当您不确定预期有多少个聚类时，例如在计算机视觉问题中，这是一个很好的开始算法。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import AffinityPropagation

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
model = AffinityPropagation(damping=0.7)

# train the model
model.fit(training_data)

# assign each data point to a cluster
result = model.predict(training_data)

# get all of the unique clusters
clusters = unique(result)

# plot the clusters
for cluster in clusters:
    # get data points that fall in this cluster
    index = where(result == cluster)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the plot
pyplot.show()
```

### 均值漂移聚类算法

这是另一种对处理图像和计算机视觉处理特别有用的算法。

Mean-shift 类似于 BIRCH 算法，因为它也可以在没有设置初始聚类数的情况下查找聚类。

这是一种分层聚类算法，但缺点是在处理大型数据集时扩展性不好。

它的工作方式是迭代所有的数据点，并将它们移向模式。本文中的模式是一个区域中数据点的高密度区域。

这就是为什么你可能会听到这个算法被称为模式搜索算法。它将对每个数据点进行这种迭代过程，并将它们移动到更靠近其他数据点的位置，直到所有数据点都被分配到一个聚类。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import MeanShift

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
mean_model = MeanShift()

# assign each data point to a cluster
mean_result = mean_model.fit_predict(training_data)

# get all of the unique clusters
mean_clusters = unique(mean_result)

# plot Mean-Shift the clusters
for mean_cluster in mean_clusters:
    # get data points that fall in this cluster
    index = where(mean_result == mean_cluster)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the Mean-Shift plot
pyplot.show()
```

### 光学算法

OPTICS 代表用于识别聚类结构的排序点。这是一种基于密度的算法，类似于 DBSCAN，但它更好，因为它可以在密度不同的数据中找到有意义的聚类。它通过对数据点进行排序来做到这一点，使得最近的点在排序中是邻居。

这使得更容易检测不同密度的簇。光学算法仅处理每个数据点一次，类似于 DBSCAN(尽管它比 DBSCAN 运行得慢)。还为每个数据点存储了一个特殊的距离，表示一个点属于一个特定的聚类。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import OPTICS

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
optics_model = OPTICS(eps=0.75, min_samples=10)

# assign each data point to a cluster
optics_result = optics_model.fit_predict(training_data)

# get all of the unique clusters
optics_clusters = unique(optics_clusters)

# plot OPTICS the clusters
for optics_cluster in optics_clusters:
    # get data points that fall in this cluster
    index = where(optics_result == optics_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the OPTICS plot
pyplot.show()
```

### 凝聚层次聚类算法

这是最常见的层次聚类算法。它用于根据对象之间的相似程度将对象分组。

这是一种自下而上的聚类形式，其中每个数据点都被分配到自己的聚类中。然后这些簇结合在一起。

在每次迭代中，相似的聚类被合并，直到所有的数据点都是一个大的根聚类的一部分。

凝聚聚类最擅长发现小的聚类。最终结果看起来像一个树状图，这样当算法完成时，您可以很容易地看到这些分类。

**实施:**

```
from numpy import unique
from numpy import where
from matplotlib import pyplot
from sklearn.datasets import make_classification
from sklearn.cluster import AgglomerativeClustering

# initialize the data set we'll work with
training_data, _ = make_classification(
    n_samples=1000,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=4
)

# define the model
agglomerative_model = AgglomerativeClustering(n_clusters=2)

# assign each data point to a cluster
agglomerative_result = agglomerative_model.fit_predict(training_data)

# get all of the unique clusters
agglomerative_clusters = unique(agglomerative_result)

# plot the clusters
for agglomerative_cluster in agglomerative_clusters:
    # get data points that fall in this cluster
    index = where(agglomerative_result == agglomerative_clusters)
    # make the plot
    pyplot.scatter(training_data[index, 0], training_data[index, 1])

# show the Agglomerative Hierarchy plot
pyplot.show()
```

## 其他类型的聚类算法

我们已经介绍了八种顶级的聚类算法，但是还有更多可用的算法。有一些专门调整过的聚类算法可以快速精确地处理您的数据。这里有几个你可能会感兴趣的例子。

还有另一种分层算法，与凝聚方法相反。它从自顶向下的聚类策略开始。因此，它将从一个大的根集群开始，并从那里分出各个集群。

这就是所谓的**分裂分层**聚类算法。有研究表明，这比聚集聚类创建了更精确的层次结构，但它要复杂得多。

**小批量 K-means** 类似于 K-means，除了它使用固定大小的随机小数据块，因此它们可以存储在内存中。这有助于它比 K-means 运行得更快，从而在更短的时间内收敛到一个解。

这种算法的缺点是速度的提高会降低集群的质量。

我们将简要介绍的最后一个算法是**谱聚类**。这个算法完全不同于我们看到的其他算法。

它的工作原理是利用图论。该算法不对数据集中的聚类进行任何初步猜测。它将数据点视为图中的节点，并且基于具有连接边的节点社区来发现聚类。

## 其他想法

注意聚类算法的伸缩问题。您的数据集可能有数百万个数据点，由于聚类算法是通过计算所有数据点对之间的相似性来工作的，因此您最终可能会得到一个扩展性不好的算法。

## 结论

聚类算法是从旧数据中学习新事物的好方法。有时你会对得到的集群感到惊讶，它可能会帮助你理解问题。

使用聚类进行无监督学习最酷的事情之一是，您可以在有监督的学习问题中使用结果。

聚类可能是您在完全不同的数据集上使用的新功能！你可以在任何无监督的机器学习问题上使用聚类，但要确保你知道如何分析结果的准确性。