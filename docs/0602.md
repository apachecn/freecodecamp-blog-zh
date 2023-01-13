# 如何在 NLP 项目中执行数据扩充

> 原文：<https://www.freecodecamp.org/news/how-to-perform-data-augmentation-in-nlp-projects/>

在机器学习中，为了实现强大的模型性能，你需要拥有大量的数据。

使用一种称为数据扩充的方法，您可以为您的机器学习项目创建更多数据。数据扩充是一组技术，用于管理在现有数据基础上自动生成高质量数据的过程。

在计算机视觉应用中，增强方法非常普遍。例如，如果您正在进行计算机视觉项目(如图像分类)，您可以对每张图像应用许多技术:移动、修改颜色强度、缩放、旋转、裁剪等等。

如果您的 ML 项目有一个很小的数据集，或者希望减少机器学习模型中的过度拟合，那么应用数据扩充方法可能是一个好主意。

> “我们没有更好的算法。我们只是有更多的数据。”——彼得[诺威格](https://research.google/people/author205/?ref=hackernoon.com)

在自然语言处理领域，语言的巨大复杂性使得扩充文本变得非常困难。扩充文本数据的过程更具挑战性，也不像您预期的那样简单。

在本文中，您将学习如何使用名为 [TextAttack](https://github.com/QData/TextAttack?ref=hackernoon.com) 的库来改进自然语言处理的数据。

## 什么是 TextAttack？

TextAttack 是一个 Python 框架，由 [QData 团队](https://qdata.github.io/qdata-page/?ref=hackernoon.com)构建，目的是在自然语言处理中进行对抗性攻击、对抗性训练和数据扩充。

TextAttack 具有可独立用于各种基本自然语言处理任务的组件，包括句子编码、语法检查和单词替换。

TextAttack 擅长执行以下三个功能:

1.  对抗性攻击(Python: `**textattack.Attack**`，Bash: `**textattack attack**`)。
2.  数据增强(Python: `**textattack.augmentation.Augmenter**`，Bash: `**textattack augment**`)。
3.  模型训练(Python: `**textattack.Trainer**`，Bash: `**textattack train**`)。

在本文中，我们将关注如何使用 TextAttack 库进行数据扩充。

## 如何安装 TexAttack

要使用这个库，请确保您的环境中有 Python 3.6 或更高版本。

运行以下命令安装 textAttack:

```
pip install textattack
```

**注意:**一旦安装了 TexAttack，就可以通过 Python 模块或者命令行来运行它。

## 文本数据的数据扩充技术

TextAttack 库有各种增强技术，您可以在 NLP 项目中使用这些技术来添加更多的文本数据。

以下是您可以应用的一些技巧:

### `CharSwapAugmenter`技术

这种技术通过将字符换成其他字符来扩充单词。

```
from textattack.augmentation import CharSwapAugmenter

text = "I have enjoyed watching that movie, it was amazing."

charswap_aug = CharSwapAugmenter()

charswap_aug.augment(text)
```

[“我很喜欢看那部电影，太棒了。”]

增强器将单词**“电影”**换成了**“奥姆维”**。

### `DeletionAugmenter`技术

这种方法通过删除文本的某些部分来生成新的文本，从而扩充了文本。

```
from textattack.augmentation import DeletionAugmenter

text = "I have enjoyed watching that movie, it was amazing."

deletion_aug = DeletionAugmenter()

deletion_aug.augment(text)
```

[“我看过了，太棒了。”]

该方法删除了单词**“enjoined”**来创建新的增强文本。

### `EasyDataAugmenter`技术

这通过不同方法的组合来扩充文本，例如:

*   随机交换单词在句子中的位置。
*   从句子中随机删除单词。
*   在随机位置随机插入随机单词的随机同义词。
*   用同义词随机替换单词。

```
from textattack.augmentation import EasyDataAugmenter

text = "I was billed twice for the service and this is the second time it has happened"

eda_aug = EasyDataAugmenter()

eda_aug.augment(text)
```

['我为这项服务开了两次账单，这是第二次发生'，'我为一项服务开了两次账单，这是第二次发生'，'我为这项服务开了两次账单，这是第二次发生'，
'我为这项服务开了两次账单，这是第二次发生']

正如您从增强文本中看到的，它根据应用的方法显示了不同的结果。例如，在第一个扩充文本中，最后一个单词已经从**“发生”**修改为**“发生”**。

### `WordNetAugmenter`技术

这种技术可以通过用 WordNet 同义词库中的同义词替换文本来扩充文本。

```
from textattack.augmentation import WordNetAugmenter

text = "I was billed twice for the service and this is the second time it has happened"

wordnet_aug = WordNetAugmenter()

wordnet_aug.augment(text)
```

['我为这项服务开了两次账单，这是第二次了']

该方法将单词**“发生”**改为**“经过”**，以创建新的扩充文本。

### 如何创建自己的增强器

从`textattack.transformations` 和`textattack.constraints`导入变换和约束允许你从头开始构建你自己的增强器。

以下是使用`WordSwapRandomCharacterDeletion`算法生成字符串扩充的示例:

```
from textattack.transformations import WordSwapRandomCharacterDeletion
from textattack.transformations import CompositeTransformation
from textattack.augmentation import Augmenter

my_transformation = CompositeTransformation([WordSwapRandomCharacterDeletion()])
augmenter = Augmenter(transformation=my_transformation, transformations_per_example=3)

text = 'Siri became confused when we reused to follow her directions.'

augmenter.augment(text)
```

[“当我们再次听从她的指示时，Siri 变得困惑起来。”“当她重复听从她的指示时，Siri 变得很困惑，”“当我们重复使用 Siri 来遵循 hr 的指示时，Siri 变得很困惑。”]

执行`WordSwapRandomCharacterDeletion`方法后，输出显示不同的增强文本。例如，在第一个扩充文本中，该方法随机移除单词“ **confused”中的字符“**o”**。**

## 结论

在本文中，您已经了解了数据扩充对于您的机器学习项目的重要性。您还了解了如何使用 TextAttack 库对文本数据执行数据扩充。

据我所知，这些技术是完成 NLP 项目任务的最有效的方法。希望它们能对你的工作有所帮助。

您还可以尝试使用 TextAttack 库中其他可用的增强技术，例如:

*   嵌入式 augmentor
*   核对表增强器
*   clareaugmentor

如果你学到了新的东西或者喜欢阅读这篇文章，请分享给其他人看。在那之前，下期帖子再见！

你也可以在 Twitter 上找到我 [@Davis_McDavid](https://twitter.com/Davis_McDavid?ref=hackernoon.com) 。

而且你可以在这里阅读更多类似这样的文章[。](https://hackernoon.com/u/davisdavid)