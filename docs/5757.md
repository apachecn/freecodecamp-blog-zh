# 单词包的介绍以及如何用 Python 为 NLP 编写单词包

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-bag-of-words-and-how-to-code-it-in-python-for-nlp-282e87a9da04/>

作者:普拉文·杜比

# 介绍单词的 B *ag 以及如何用 Python 为 NLP 编写代码*

![YxESbVFNtB9LgMODW6nNKs1rMWr7z8BSuJLA](img/32b303f90bf321d2f85089934542ab8a.png)

White and black scrabble tiles on black surface by [Pixabay](https://www.pexels.com/photo/alphabet-black-and-white-business-child-279013/)

单词包(BOW)是一种从文本文档中提取特征的方法。这些特征可以用于训练机器学习算法。它创建了在训练集中的所有文档中出现的所有唯一单词的词汇表。

简单来说，它是一个单词的集合，用单词计数来表示一个句子，并且大多数情况下不考虑它们出现的顺序。

BOW 是一种广泛用于以下情况的方法:

1.  自然语言处理
2.  文档信息检索
3.  文档分类

在高层次上，它包括以下步骤。

![qRGh8boBcLLQfBvDnWTXKxZIEAk5LNfNABHF](img/17207a184c06ad242fcb3426a00a1941.png)

**生成的向量可以输入到你的机器学习算法中。**

让我们从一个例子开始，通过取一些句子并为它们生成向量来理解。

考虑下面两句话。

```
1\. "John likes to watch movies. Mary likes movies too."
```

```
2\. "John also likes to watch football games."
```

这两个句子也可以用单词集合来表示。

```
1\. ['John', 'likes', 'to', 'watch', 'movies.', 'Mary', 'likes', 'movies', 'too.']
```

```
2\. ['John', 'also', 'likes', 'to', 'watch', 'football', 'games']
```

此外，对于每个句子，删除多次出现的单词，并使用单词计数来表示。

```
1\. {"John":1,"likes":2,"to":1,"watch":1,"movies":2,"Mary":1,"too":1}
```

```
2\. {"John":1,"also":1,"likes":1,"to":1,"watch":1,"football":1,   "games":1}
```

假设这些句子是文档的一部分，下面是我们整个文档的组合词频。两个句子都考虑进去了。

```
 {"John":2,"likes":3,"to":2,"watch":2,"movies":2,"Mary":1,"too":1,  "also":1,"football":1,"games":1}
```

来自文档中所有单词的上述词汇，以及它们各自的单词计数，将被用于创建每个句子的向量。

向量的长度总是等于词汇量。在这种情况下，向量长度是 11。

为了在一个向量中表示我们的原始句子，每个向量都用全零初始化—**【0，0，0，0，0，0，0，0，0】**

接下来是与我们词汇表中的每个单词进行迭代和比较，如果句子中有这个单词，就增加向量值。

```
John likes to watch movies. Mary likes movies too.[1, 2, 1, 1, 2, 1, 1, 0, 0, 0]
```

```
John also likes to watch football games.[1, 1, 1, 1, 0, 0, 0, 1, 1, 1]
```

例如，在句子 1 中，单词`likes`出现在第二个位置，并且出现了两次。因此，句子 1 的向量的第二个元素将是 2:**【1，2，1，1，2，1，1，0，0，0】**

向量总是与我们的词汇量成正比。

一个生成的词汇表很大的大文档可能会产生一个包含很多 0 值的向量。这被称为**稀疏向量**。稀疏矢量建模时需要更多的内存和计算资源。对于传统算法来说，大量的位置或维度会使建模过程非常具有挑战性。

### 编写我们的 BOW 算法

我们代码的输入将是多个句子，输出将是向量。

输入数组如下:

```
["Joe waited for the train", "The train was late", "Mary and Samantha took the bus",
```

```
"I looked for Mary and Samantha at the bus station",
```

```
"Mary and Samantha arrived at the bus station early but waited until noon for the bus"]
```

#### 第一步:标记一个句子

我们将从删除句子中的停用词开始。

**停用词**是不包含足够重要性的词，在没有我们的算法的情况下不能使用。我们不希望这些单词占用我们的数据库空间，或者占用宝贵的处理时间。为此，我们可以通过存储您认为是停用词的单词列表来轻松删除它们。

**记号化**是将一串字符串分解成单词、关键词、短语、符号和其他元素的行为，称为**记号**。记号可以是单个的单词、短语，甚至是整个句子。在标记化的过程中，一些类似标点符号的字符被丢弃。

```
def word_extraction(sentence):    ignore = ['a', "the", "is"]    words = re.sub("[^\w]", " ",  sentence).split()    cleaned_text = [w.lower() for w in words if w not in ignore]    return cleaned_text
```

为了更健壮地实现停用词，可以使用 python **nltk** 库。每种语言都有一组预定义的单词。这里有一个例子:

```
import nltkfrom nltk.corpus import stopwords set(stopwords.words('english'))
```

#### 第二步:对所有句子应用标记化

```
def tokenize(sentences):    words = []    for sentence in sentences:        w = word_extraction(sentence)        words.extend(w)            words = sorted(list(set(words)))    return words
```

该方法迭代所有句子，并将提取的单词添加到一个数组中。

该方法的输出将是:

```
['and', 'arrived', 'at', 'bus', 'but', 'early', 'for', 'i', 'joe', 'late', 'looked', 'mary', 'noon', 'samantha', 'station', 'the', 'took', 'train', 'until', 'waited', 'was']
```

#### 第三步:建立词汇并生成向量

使用步骤 1 和 2 中定义的方法创建文档词汇表，并从句子中提取单词。

```
def generate_bow(allsentences):        vocab = tokenize(allsentences)    print("Word List for Document \n{0} \n".format(vocab));
```

```
for sentence in allsentences:        words = word_extraction(sentence)        bag_vector = numpy.zeros(len(vocab))        for w in words:            for i,word in enumerate(vocab):                if word == w:                     bag_vector[i] += 1                            print("{0}\n{1}\n".format(sentence,numpy.array(bag_vector)))
```

下面是我们代码的定义输入和执行:

```
allsentences = ["Joe waited for the train train", "The train was late", "Mary and Samantha took the bus",
```

```
"I looked for Mary and Samantha at the bus station",
```

```
"Mary and Samantha arrived at the bus station early but waited until noon for the bus"]
```

```
generate_bow(allsentences)
```

每个句子的输出向量是:

```
Output:
```

```
Joe waited for the train train[0\. 0\. 0\. 0\. 0\. 0\. 1\. 0\. 1\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 2\. 0\. 1\. 0.]
```

```
The train was late[0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 1\. 0\. 0\. 0\. 0\. 0\. 1\. 0\. 1\. 0\. 0\. 1.]
```

```
Mary and Samantha took the bus[1\. 0\. 0\. 1\. 0\. 0\. 0\. 0\. 0\. 0\. 0\. 1\. 0\. 1\. 0\. 0\. 1\. 0\. 0\. 0\. 0.]
```

```
I looked for Mary and Samantha at the bus station[1\. 0\. 1\. 1\. 0\. 0\. 1\. 1\. 0\. 0\. 1\. 1\. 0\. 1\. 1\. 0\. 0\. 0\. 0\. 0\. 0.]
```

```
Mary and Samantha arrived at the bus station early but waited until noon for the bus[1\. 1\. 1\. 2\. 1\. 1\. 1\. 0\. 0\. 0\. 0\. 1\. 1\. 1\. 1\. 0\. 0\. 0\. 1\. 1\. 0.]
```

如您所见，**每个句子都与我们在步骤 1 中生成的单词列表进行了比较。基于该比较，向量元素值可以递增**。这些向量可以在 ML 算法中用于文档分类和预测。

我们编写了代码并生成了向量，但现在让我们多了解一下这个单词包。

### 洞察词汇袋

BOW 模型只考虑一个已知单词是否出现在文档中。它不关心它们出现的意义、上下文和顺序。

这提供了相似文档将具有彼此相似的字数的洞察力。换句话说，两个文档中的单词越相似，文档就越相似。

### 弓的局限性

1.  **语义**:基本的 BOW 方法不考虑文档中单词的含义。它完全忽略了使用它的上下文。根据上下文或邻近单词，同一个单词可以用在多个地方。
2.  **矢量大小**:对于一个大文档，矢量大小可能很大，导致大量的计算和时间。您可能需要根据与您的用例的相关性来忽略单词。

这是弓法的一个小介绍。代码展示了它在底层是如何工作的。关于保龄球还有更多需要了解。例如，不要把我们的句子分成一个单词(1-gram)，你可以分成两个单词对(双-gram 或 2-gram)。有时，双元表示法似乎比单元表示法好得多。这些通常可以用 N-gram 符号来表示。为了获得更深入的知识，我在参考资料部分列出了一些研究论文。

您不必在需要时编写 BOW 代码。它已经是许多可用框架的一部分，如 sci-kit learn 中的 CountVectorizer。

我们之前的代码可以替换为:

```
from sklearn.feature_extraction.text import CountVectorizervectorizer = CountVectorizer()X = vectorizer.fit_transform(allsentences)print(X.toarray())
```

理解框架中的库是如何工作的，理解它们背后的方法总是好的。你对概念理解得越好，你就能更好地利用框架。

感谢您阅读这篇文章。显示的代码可以在我的 [GitHub](https://gist.github.com/edubey/c52a3b34541456a76a2c1f81eebb5f67) 上找到。

你可以在[媒体](https://medium.com/@edubey)、[推特](https://twitter.com/edubey1)、 [LinkedIn](https://www.linkedin.com/in/edubey/) 上关注我，有任何问题可以发邮件联系我(praveend806 [at] gmail [dot] com)。

### **阅读更多关于单词袋的资源**

1.  [维基百科-弓](https://en.wikipedia.org/wiki/Bag-of-words_model)
2.  [理解词袋模型:一个统计框架](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.453.5924&rep=rep1&type=pdf)
3.  [语义保持的词袋模型及应用](https://ieeexplore.ieee.org/document/5428847)