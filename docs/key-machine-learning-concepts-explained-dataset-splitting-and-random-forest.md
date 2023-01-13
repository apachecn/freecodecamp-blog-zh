# 解释关键机器学习概念-数据集分割和随机森林

> 原文：<https://www.freecodecamp.org/news/key-machine-learning-concepts-explained-dataset-splitting-and-random-forest/>

## **数据集分割**

分成培训、交叉验证和测试集是常见的最佳实践。这使您可以调整算法的各种参数，而无需做出特别符合训练数据的判断。

### **动机**

在最大似然算法中，数据集分割是消除训练数据偏差的必要手段。修改 ML 算法的参数以最佳拟合训练数据通常会导致过拟合算法，该算法在实际测试数据上表现不佳。出于这个原因，我们将数据集分成多个离散的子集，在这些子集上训练不同的参数。

#### **训练集**

训练集用于计算您的算法在面对新数据时将使用的实际模型。该数据集通常占整个可用数据的 60%-80%(取决于是否使用交叉验证集)。

#### **交叉验证集**

交叉验证集用于模型选择(通常约占数据的 20%)。使用此数据集尝试在训练集上训练的算法的不同参数。例如，您可以在交叉验证集上评估不同的模型参数(多项式次数或 lambda，正则化参数),以查看哪个可能最准确。

#### **测试设置**

测试集是您接触的最终数据集(通常约占数据的 20%)。它是真理的源泉。你预测测试集的准确性就是你的 ML 算法的准确性。

## 随机森林

随机森林是一组决策树，它们作为一个整体比单独做出更好的决策。

### **问题**

决策树本身容易出现 ****过拟合**** 。这意味着树变得如此习惯于训练数据，以至于它很难对以前从未见过的数据做出决策。

### **随机森林解决方案**

随机森林属于 ****集成学习**** 算法的范畴。这类算法使用许多估计器来产生更好的结果。这使得随机森林通常 ****比普通决策树更准确**** 。在随机森林中，会创建一堆决策树。每棵树都在数据的随机子集和该数据的随机特征子集上被训练。这样，估计器习惯于数据(过拟合)的可能性大大降低，因为 ****每个估计器处理的数据和特征**** 都不同于其他估计器。这种创建一组估计器并在随机数据子集上训练它们的方法是*集成学习*中的一种技术，称为 ****打包**** 或*引导聚合*。为了得到预测，每个决策树对正确的预测进行投票(分类)，或者得到结果的平均值(回归)。

### **Python 中的 Boosting 示例**

在这场竞赛中，我们得到了一个碰撞事件及其属性的列表。然后我们将预测在这次碰撞中是否发生了τ → 3μ衰变。这种τ → 3μ目前被科学家认为不会发生，而这次比赛的目标是发现τ → 3μ发生的频率比科学家目前所能理解的更高。这里的挑战是设计一个机器学习问题，解决以前没有人观察过的问题。欧洲核子研究中心的科学家开发了以下设计来实现这一目标。[https://www.kaggle.com/c/flavours-of-physics/data](https://www.kaggle.com/c/flavours-of-physics/data)

```
#Data Cleaning
import pandas as pd
data_test = pd.read_csv("test.csv")
data_train = pd.read_csv("training.csv")
data_train = data_train.drop('min_ANNmuon',1)
data_train = data_train.drop('production',1)
data_train = data_train.drop('mass',1)

#Cleaned data
Y = data_train['signal']
X = data_train.drop('signal',1)

#adaboost
from sklearn.ensemble import AdaBoostClassifier
from sklearn.tree import DecisionTreeClassifier
seed = 9001 #this ones over 9000!!!
boosted_tree = AdaBoostClassifier(DecisionTreeClassifier(max_depth=1), algorithm="SAMME", 
                                  n_estimators=50, random_state = seed)
model = boosted_tree.fit(X, Y)

predictions = model.predict(data_test)
print(predictions)
#Note we can't really validate this data since we don't have an array of "right answers"

#stochastic gradient boosting
from sklearn.ensemble import GradientBoostingClassifier
gradient_boosted_tree = GradientBoostingClassifier(n_estimators=50, random_state=seed)
model2 = gradient_boosted_tree.fit(X,Y)

predictions2 = model2.predict(data_test)
print(predictions2)
```