# 朴素贝叶斯分类器如何工作 Python 代码示例

> 原文：<https://www.freecodecamp.org/news/how-naive-bayes-classifiers-work/>

朴素贝叶斯分类器(NBC)是简单而强大的机器学习算法。它们基于条件概率和贝叶斯定理。

在这篇文章中，我将解释 NBC 背后的“诡计”,并给你一个我们可以用来解决分类问题的例子。

在接下来的部分，我将谈论 NBC 背后的数学。如果您对数学不感兴趣，可以跳过这些部分，直接进入实现部分。

在实现部分，我将向您展示一个简单的 NBC 算法。然后我们用它来解决一个分类问题。任务将是确定泰坦尼克号上的某个乘客是否在事故中幸存。

## 条件概率

在说算法本身之前，先说一下背后简单的数学。我们需要了解什么是条件概率，以及如何使用贝叶斯定理来计算它。

想想一个有六个面的骰子。掷骰子得到 6 的概率有多大？那很简单，是 1/6。我们有六种可能的结果，但我们只对其中一种感兴趣。所以，是 1/6。

但是如果我告诉你我已经掷骰子了，结果是偶数，会发生什么呢？我们现在拿到 6 的概率有多大？

这一次，可能的结果只有三个，因为骰子上只有三个偶数。我们仍然只对其中一个结果感兴趣，所以现在可能性更大了:1/3。两种情况有什么区别？

在第一种情况下，我们没有关于结果的**先验**信息。因此，我们需要考虑每一个可能的结果。

在第二种情况下，我们被告知结果是一个偶数，因此我们可以将可能结果的空间缩小到只出现在常规六面骰子中的三个偶数。

一般来说，在计算一个事件 A 的概率时，给定另一个事件 B 的发生，我们说我们在计算给定 B 的**条件概率**，或者仅仅是给定 B 的概率，我们表示为`P(A|B)`。

例如，给定我们得到的数字是偶数，得到 6 的概率:`P(Six|Even) = 1/3`。在这里，我们用**6**表示得到 6 的事件，用**偶数**表示得到偶数的事件。

但是，我们如何计算条件概率呢？有公式吗？

## 如何计算条件问题和贝叶斯定理

现在，我给你几个计算条件问题的公式。我保证它们不会很难，而且如果你想理解我们稍后会谈到的机器学习算法的见解，它们很重要。

给定另一个事件 B 的发生，事件 A 的概率可以计算如下:

```
P(A|B) = P(A,B)/P(B) 
```

其中`P(A,B)`表示 A 和 B 同时发生的概率，`P(B)`表示 B 的概率。

注意，我们需要`P(B) > 0`，因为如果 B 不可能出现，谈论给定 B 的概率是没有意义的。

我们还可以计算事件 A 的概率，给定多个事件 B1，B2，...，Bn:

```
P(A|B1,B2,...,Bn) = P(A,B1,B2,...,Bn)/P(B1,B2,...,Bn) 
```

还有另一种计算条件问题的方法。这种方式就是所谓的贝叶斯定理。

```
P(A|B) = P(B|A)P(A)/P(B)

P(A|B1,B2,...,Bn) = P(B1,B2,...,Bn|A)P(A)/P(B1,B2,...,Bn) 
```

注意，给定事件 B，我们通过*颠倒事件发生的顺序*来计算事件 A 的概率。

现在我们假设事件 A 已经发生，我们想计算事件 B(或事件 B1，B2，...在第二个和更一般的例子中是 Bn)。

从这个定理可以推导出一个重要的事实，就是计算`P(B1,B2,...,Bn,A)`的公式。这就是所谓的概率链式法则。

```
P(B1,B2,...,Bn,A) = P(B1 | B2, B3, ..., Bn, A)P(B2,B3,...,Bn,A)
= P(B1 | B2, B3, ..., Bn, A)P(B2 | B3, B4, ..., Bn, A)P(B3, B4, ..., Bn, A)
= P(B1 | B2, B3, ..., Bn, A)P(B2 | B3, B4, ..., Bn, A)...P(Bn | A)P(A) 
```

那是一个丑陋的公式，不是吗？但在某些情况下，我们可以制定一个变通办法来避免它。

让我们谈谈理解算法需要知道的最后一个概念。

## 独立性ˌ自立性

我们要讲的最后一个概念是独立。我们说事件 A 和 B 是独立的，如果

```
P(A|B) = P(A) 
```

这意味着事件 A 的概率不受事件 b 发生的影响。一个直接后果是`P(A,B) = P(A)P(B)`。

简单地说，这意味着 A 和 B 同时发生的概率等于 A 和 B 分别发生的概率的乘积。

如果 A 和 B 是独立的，它也认为:

```
P(A,B|C) = P(A|C)P(B|C) 
```

现在我们准备讨论朴素贝叶斯分类器！

## 朴素贝叶斯分类器

假设我们有一个 *n* 个特征的向量 **X** ，我们想从一组 *k* 个类 *y1，y2，...，yk* 。例如，如果我们想确定今天是否会下雨。

我们有两种可能的类别( *k = 2* ): *下雨*、*不下雨*，特征向量的长度可能是 3 ( *n = 3* )。

第一个特征可能是多云还是晴天，第二个特征可能是湿度是高还是低，第三个特征可能是温度是高、中还是低。

这些可能是特征向量。

```
<Cloudy, H_High, T_Low>
<Sunny, H_Low, T_Medium>
<Cloudy, H_Low, T_High> 
```

鉴于天气特点，我们的任务是确定是否会下雨。

在了解了条件概率之后，根据以下特征，通过尝试计算下雨的可能性来解决问题似乎是很自然的:

```
R = P(Rain | Cloudy, H_High, T_Low)
NR = P(NotRain | Cloudy, H_High, T_Low) 
```

如果我们回答说会下雨，否则我们说不会。

一般来说，如果我们有 *k* 类 *y1，y2，...，yk* ，和一个矢量 *n* 特征 **X = < X1，X2，...，Xn >** ，我们要找到最大化的类 *yi*

```
P(yi | X1, X2, ..., Xn) = P(X1, X2,..., Xn, yi)/P(X1, X2, ..., Xn) 
```

注意，分母是常数，它不依赖于类 *yi* 。所以，我们可以忽略它，只关注分子。

在上一节中，我们看到了如何通过将`P(X1, X2,..., Xn, yi)`分解成条件概率的乘积来计算它(难看的公式):

```
P(X1, X2,..., Xn, yi) = P(X1 | X2,..., Xn, yi)P(X2 | X3,..., Xn, yi)...P(Xn | yi)P(yi) 
```

假设所有特征 **Xi** 都是独立的，使用贝叶斯定理，我们可以计算条件概率如下:

```
P(yi | X1, X2,..., Xn) = P(X1, X2,..., Xn | yi)P(yi)/P(X1, X2, ..., Xn)
= P(X1 | yi)P(X2 | yi)...P(Xn | yi)P(yi)/P(X1, X2, ..., Xn) 
```

我们只需要关注分子。

通过找到最大化先前表达式的类 *yi* ，我们对输入向量进行分类。但是，我们怎样才能得到所有这些概率呢？

## 如何计算概率

当解决这类问题时，我们需要一组以前分类的例子。

例如，在猜测是否会下雨的问题中，我们需要从过去的天气预报中获得几个特征向量及其分类的例子。

所以，我们会有这样的东西:

```
...
<Cloudy, H_High, T_Low> -> Rain
<Sunny, H_Low, T_Medium> -> Not Rain
<Cloudy, H_Low, T_High> -> Not Rain
... 
```

假设我们需要对一个新的向量`<Coudy, H_Low, T_Low>`进行分类。我们需要计算:

```
P(Rain | Cloudy, H_Low, T_Low) = P(Cloudy | H_Low, T_Low, Rain)P(H_Low | T_Low, Rain)P(T_Low | Rain)P(Rain)/P(Cloudy, H_Low, T_Low) 
```

通过应用条件概率的定义和链式法则，我们得到了前面的表达式。记住我们只需要关注分子，这样我们就可以去掉分母。

我们还需要计算`NotRain`的概率，但我们可以用类似的方法来做。

我们可以找到`P(Rain) = # Rain/Total`。这意味着计算数据集中被归类为 *Rain* 的条目数，并将该数除以数据集的大小。

为了计算`P(Cloudy | H_Low, T_Low, Rain)`，我们需要计算具有特征 *H_Low、T_Low* 和*多云*的所有条目。那些条目也需要归类为`Rain`。然后，这个数字除以数据总量。我们以类似的方式计算公式的其余因子。

为每一个可能的类进行这些计算是非常昂贵和缓慢的。所以我们需要对问题做一些假设来简化计算。

朴素贝叶斯分类器假设所有的特征都是相互独立的。因此，我们可以应用贝叶斯定理并假设每对特征之间的独立性来重写我们的公式:

```
P(Rain | Cloudy, H_Low, T_Low) = P(Cloudy | Rain)P(H_Low | Rain)P(T_Low | Rain)P(Rain)/P(Cloudy, H_Low, T_Low) 
```

现在我们计算被归类为`Rain`和`Cloudy`的条目的数量`P(Cloudy | Rain)`。

> 由于这种独立性假设，该算法被称为 *Naive* 。大多数情况下，功能之间存在依赖关系。例如，我们不能说在现实生活中，湿度和温度之间没有相关性。朴素贝叶斯分类器也称为独立贝叶斯或简单贝叶斯。

一般公式是:

```
P(yi | X1, X2, ..., Xn) = P(X1 | yi)P(X2 | yi)...P(Xn | yi)P(yi)/P(X1, X2, ..., Xn) 
```

记住你可以去掉分母。我们只计算分子，回答使其最大化的类。

现在，让我们实现我们的 NBC，并在一个问题中使用它。

## 我们来编码吧！

我将向您展示一个简单的 NBC 的实现，然后我们将在实践中看到它。

我们要解决的问题是，根据一些特征，如性别和年龄，确定泰坦尼克号上的乘客是否幸存。

在这里，您可以看到一个非常简单的 NBC 的实现:

```
class NaiveBayesClassifier:

    def __init__(self, X, y):

        '''
        X and y denotes the features and the target labels respectively
        '''
        self.X, self.y = X, y 

        self.N = len(self.X) # Length of the training set

        self.dim = len(self.X[0]) # Dimension of the vector of features

        self.attrs = [[] for _ in range(self.dim)] # Here we'll store the columns of the training set

        self.output_dom = {} # Output classes with the number of ocurrences in the training set. In this case we have only 2 classes

        self.data = [] # To store every row [Xi, yi]

        for i in range(len(self.X)):
            for j in range(self.dim):
                # if we have never seen this value for this attr before, 
                # then we add it to the attrs array in the corresponding position
                if not self.X[i][j] in self.attrs[j]:
                    self.attrs[j].append(self.X[i][j])

            # if we have never seen this output class before,
            # then we add it to the output_dom and count one occurrence for now
            if not self.y[i] in self.output_dom.keys():
                self.output_dom[self.y[i]] = 1
            # otherwise, we increment the occurrence of this output in the training set by 1
            else:
                self.output_dom[self.y[i]] += 1
            # store the row
            self.data.append([self.X[i], self.y[i]])

    def classify(self, entry):

        solve = None # Final result
        max_arg = -1 # partial maximum

        for y in self.output_dom.keys():

            prob = self.output_dom[y]/self.N # P(y)

            for i in range(self.dim):
                cases = [x for x in self.data if x[0][i] == entry[i] and x[1] == y] # all rows with Xi = xi
                n = len(cases)
                prob *= n/self.N # P *= P(Xi = xi)

            # if we have a greater prob for this output than the partial maximum...
            if prob > max_arg:
                max_arg = prob
                solve = y

        return solve 
```

这里，我们假设每个特征都有一个离散的域。这意味着它们从有限的可能值集中取一个值。

类也是如此。注意，我们在`__init__`方法中存储了一些数据，所以我们不需要重复一些操作。新条目的分类在`classify`方法中进行。

> 这是一个简单的实现示例。在真实的应用程序中，您不需要(最好不要)自己的实现。例如，Python 中的`sklearn`库包含了 NBC 的几个很好的实现。

请注意实现它是多么容易！

现在，让我们应用我们的新分类器来解决一个问题。我们有一个描述泰坦尼克号上 887 名乘客的数据集。我们还可以看到某个乘客是否幸免于难。

所以我们的任务是确定没有包括在训练集中的另一个乘客是否成功了。

在这个例子中，我将使用`pandas`库来读取和处理数据。我不使用任何其他工具。

数据存储在一个名为 *titanic.csv* 的文件中，所以第一步是读取数据并获得它的概述。

```
import pandas as pd

data = pd.read_csv('titanic.csv')

print(data.head()) 
```

输出是:

```
Survived  Pclass                                               Name  \
0         0       3                             Mr. Owen Harris Braund   
1         1       1  Mrs. John Bradley (Florence Briggs Thayer) Cum...   
2         1       3                              Miss. Laina Heikkinen   
3         1       1        Mrs. Jacques Heath (Lily May Peel) Futrelle   
4         0       3                            Mr. William Henry Allen   

      Sex   Age  Siblings/Spouses Aboard  Parents/Children Aboard     Fare  
0    male  22.0                        1                        0   7.2500  
1  female  38.0                        1                        0  71.2833  
2  female  26.0                        0                        0   7.9250  
3  female  35.0                        1                        0  53.1000  
4    male  35.0                        0                        0   8.0500 
```

注意我们有每个乘客的名字。我们不会在分类器中使用这个特性，因为它对我们的问题并不重要。我们还将取消票价功能，因为它是连续的，而我们的功能需要是离散的。

> 有支持连续特征的朴素贝叶斯分类器。例如，高斯朴素贝叶斯分类器。

```
y = list(map(lambda v: 'yes' if v == 1 else 'no', data['Survived'].values)) # target values as string

# We won't use the 'Name' nor the 'Fare' field

X = data[['Pclass', 'Sex', 'Age', 'Siblings/Spouses Aboard', 'Parents/Children Aboard']].values # features values 
```

然后，我们需要将数据集分为训练集和验证集。后者用于验证我们的算法做得有多好。

```
print(len(y)) # >> 887

# We'll take 600 examples to train and the rest to the validation process
y_train = y[:600]
y_val = y[600:]

X_train = X[:600]
X_val = X[600:] 
```

我们使用训练集创建 NBC，然后对验证集中的每个条目进行分类。

我们通过将正确分类的条目数除以验证集中的条目总数来衡量算法的准确性。

```
## Creating the Naive Bayes Classifier instance with the training data

nbc = NaiveBayesClassifier(X_train, y_train)

total_cases = len(y_val) # size of validation set

# Well classified examples and bad classified examples
good = 0
bad = 0

for i in range(total_cases):
    predict = nbc.classify(X_val[i])
#     print(y_val[i] + ' --------------- ' + predict)
    if y_val[i] == predict:
        good += 1
    else:
        bad += 1

print('TOTAL EXAMPLES:', total_cases)
print('RIGHT:', good)
print('WRONG:', bad)
print('ACCURACY:', good/total_cases) 
```

输出:

```
TOTAL EXAMPLES: 287
RIGHT: 200
WRONG: 87
ACCURACY: 0.6968641114982579 
```

这不算伟大，但也算了不起。如果我们去掉其他特征，比如上的*兄弟姐妹/配偶和*上的*父母/子女，我们可以获得大约 10%的准确率提升。*

你可以看到一个笔记本，上面有代码和数据集[在这里](https://github.com/josejorgers/naive-bayes-classifier-example)

## 结论

今天，我们到处都有神经网络和其他复杂而昂贵的 ML 算法。

NBCs 是非常简单的算法，让我们在一些分类问题上取得好的结果，而不需要很多资源。它们的扩展性也很好，这意味着我们可以添加更多的功能，算法仍然快速可靠。

即使在 NBC 不适合我们试图解决的问题的情况下，它们也可能是非常有用的基线。

我们可以先尝试用 NBC 解决这个问题，只需几行代码，不费吹灰之力。然后，我们可以尝试用更复杂、更昂贵的算法来获得更好的结果。

这个过程可以节省我们很多时间，并给我们一个即时的反馈，告诉我们复杂的算法对于我们的任务是否真的值得。

在这篇文章中，你读到了条件概率、独立性和贝叶斯定理。这些是朴素贝叶斯分类器背后的数学概念。

之后，我们看到了一个 NBC 的简单实现，并解决了确定泰坦尼克号上的一名乘客是否在事故中幸存的问题。

我希望这篇文章对你有用。你可以在我的[个人博客](https://jj.hashnode.dev/)和在[推特](https://twitter.com/josejorgexl)上关注我，了解计算机科学的相关话题。