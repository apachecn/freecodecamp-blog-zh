# 深度学习和数据科学的 Python 入门方法

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-with-python-for-deep-learning-and-data-science-3bed07f91a08/>

作者李宗德·韦恩

# 深度学习和数据科学的 Python 入门方法

#### 完全初学者设置 Python 的分步指南

![f2tiBCqS6IVb1yWNurylwXhNi4fEDOeiVlib](img/1cb02ee2574849059ae6906d01af3351.png)

如今，你可以用几行代码编写自己的数据科学或深度学习项目。这并不夸张；许多程序员已经完成了编写大量代码供我们使用的艰苦工作，因此我们需要做的就是即插即用，而不是从头开始编写代码。

你可能在数据科学/深度学习博客帖子上看到过一些这样的代码。也许你会想:“好吧，如果真的那么简单，那我为什么不自己尝试一下呢？”

如果你是 Python 的初学者，并且想开始这段旅程，那么这篇文章将指导你迈出第一步。我从完全初学者那里听到的一个常见抱怨是设置 Python 非常困难。我们如何在第一时间启动一切，以便我们可以即插即用数据科学或深度学习代码？

这篇文章将逐步指导你如何为你的数据科学和深度学习项目设置 Python。我们将:

*   设置水蟒和木星笔记本
*   创建 Anaconda 环境并安装软件包(其他人编写的使我们的生活变得非常简单的代码)，如 tensorflow、keras、pandas、scikit-learn 和 matplotlib。

一旦你设置了以上内容，你就可以在本教程中建立你的第一个神经网络来预测房价了:

[用 Keras 建立第一个预测房价的神经网络](https://medium.com/intuitive-deep-learning/build-your-first-neural-network-to-predict-house-prices-with-keras-eb5db60232c)

### 设置 Anaconda 和 Jupyter 笔记本

我们要用的主要编程语言叫 Python，是深度学习从业者最常用的编程语言。

第一步是下载 Anaconda，你可以把它想象成一个让你“开箱即用”使用 Python 的平台。

访问此页面:[https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)向下滚动查看:

![Y9PBNXOl0NLmLPp1DBpQrodepqZ0UCwk6KWT](img/f7372e0000752ef2934696960949f22b.png)

Download Anaconda

本教程是专门为 Windows 用户编写的，但对其他操作系统用户的说明并没有什么不同。请务必点击“Windows”作为您的操作系统(或您所在的任何操作系统)，以确保您下载的是正确的版本。

本教程将使用 Python 3，因此单击“Python 3.7 版本”下的绿色下载按钮。应该会出现一个弹出窗口，让您点击“保存”进入您想要的任何目录。

![nIBsLmbM3QXFAvKh5DvOgQTAvaIlaEzbtmIO](img/15678cd56d1a17a8fb81aceaf7673108.png)

下载完成后，只需按如下步骤逐步完成设置:

![j7MTrYBa-vBDFljeE6ELgJ2Rb1r8XJwfppgg](img/74afa23677c76ae1eedb24b20270da35.png)

Click Next

![xIqTDiWBgQIDNdr6ZKUbe0cD2YDdcHG0znwT](img/c727dc7dfca3a028dfead35df83b671b.png)

Click “I Agree”

![JK4bVL-3d5CAoMaLIFdWKKN6fOY6iEe54KP2](img/4734ca6fd9a8b1f2d17a6facded14b26.png)

Click Next

![82jlqCIonJ-6eSkwL7yIfWZfV8Zw4V6oJayA](img/9facb63f6b355c753d34c4ec97853da2.png)

Choose a destination folder and click Next

![irLpIjKObhEjNp9fguypnF8AQo4FvglVqHDY](img/fcc8c918e67a433d88c1ac1b82ab70c7.png)

Click Install with the default options, and wait for a few moments as Anaconda installs

![cAyLf4jFGXquhZynYWlPinokrY-ZP6RGstLu](img/57e3fce0cadfd3f94e37ed9a6e185a29.png)

Click Skip as we will not be using Microsoft VSCode in our tutorials

![qdbsbeVWov2V5zW0RXjZSNnYxRFmVivKC3Jn](img/4b3ce74734a338795f6d3e258ae1c792.png)

Click Finish, and the installation is done!

安装完成后，进入开始菜单，您应该会看到一些新安装的软件:

![tOWuum3G7ONEAIhgmji6dYRurvqA5Z2lI1jw](img/f65ed2094468182fc30b00b5d1f72c5b.png)

You should see this on your start menu

点击 Anaconda Navigator，这是一个一站式中心，可以导航我们需要的应用程序。您应该会看到这样的首页:

![pGBii7zWZSsvRP9qrBXbRFusVmj5lgcSCIe4](img/c396b05790dde8f62c79de92c9467cd2.png)

Anaconda Navigator Home Screen

点击 Jupyter 笔记本下的“启动”,这是我屏幕上方的第二个面板。Jupyter Notebook 允许我们在 web 浏览器上交互式地运行 Python 代码，这是我们编写大部分代码的地方。

应该会打开一个浏览器窗口，显示您的目录列表。我准备在桌面上创建一个文件夹，名为“直觉深度学习教程”。如果您导航到该文件夹，您的浏览器应该如下所示:

![Q7CdWULn4gjQ8GPSGnzIUfvSoD3Dt2dOj-Ud](img/e1d259ea7c673951be7ef6445143c31f.png)

Navigating to a folder called Intuitive Deep Learning Tutorial on my Desktop

在右上角，单击“新建”并选择“Python 3”:

![27sYFSx5iqHK-y-cxD9sMXXkg7zP9brYRsyS](img/d928a1f3842ffd98956eb45c0ed95c69.png)

Click on New and select Python 3

一个新的浏览器窗口应该像这样弹出。

![k9-ZWdXld94RG03TXpaM-ELcFerj-kmY7Ldw](img/37c71187d9c8cce11fdec196e39eb23e.png)

Browser window pop-up

祝贺您，您已经创建了您的第一个 Jupyter 笔记本！现在是时候写一些代码了。Jupyter 笔记本允许我们编写代码片段，然后运行这些片段，而无需运行整个程序。这可能有助于我们查看程序的任何中间输出。

首先，让我们编写代码，在运行时显示一些单词。这个功能叫做*打印*。将以下代码复制并粘贴到 Jupyter 笔记本的灰色方框中:

```
print("Hello World!")
```

您的笔记本应该是这样的:

![felCZ7xmEgHusNkuzlT5KwoGnjFZl23INRRJ](img/3fc66ac748c51c5942a01e6d1846324a.png)

Entering in code into our Jupyter Notebook

现在，按下键盘上 Alt-Enter 来运行这段代码:

![0q66xA7p0oKXMFqmtv0ZzH1ZLGp37LeZ8MKT](img/b7d54402705ae8941b764a3877590bfa.png)

Press Alt-Enter to run that snippet of code

你可以看到 Jupyter 笔记本已经显示了“Hello World！”代码片段下面的显示面板上！数字 1 也填充在方括号中，这意味着这是我们到目前为止运行的第一个代码片段。这将帮助我们跟踪代码片段的运行顺序。

除了 Alt-Enter，请注意，当代码段突出显示时，您也可以单击 Run:

![ZSo3TOPK1RIkjPhlyrfWhPcscCaYf72XEqOT](img/42f7f6183d145ab8b0ad5e4c93458e53.png)

Click Run on the panel

如果您希望创建新的灰色块来编写更多的代码片段，您可以在“插入”下这样做。

![8PmsMQDCyquKK3GSM9cUWCw1bDOkGedAvsB2](img/5af6cd33689110a72593e1e1c6f2f637.png)

Jupyter Notebook 还允许您编写普通文本，而不是代码。点击当前显示“代码”的下拉菜单，并选择“降价”:

![R2LdzqHjihNjlx54HHyoSm3qFUurL0fPDbav](img/732c6d56a121bf6001a8ce2d86038030.png)

现在，我们标记为降价的灰色框旁边将没有方括号。如果您现在在这个灰色框中输入一些文本，然后按 Alt-Enter，文本将显示为纯文本，如下所示:

![Yb7GdouMsQTeD3136CkR0TxPBZ81w98BBUIP](img/ec0f0b9a8759fd67e152d2a78c0a1d7d.png)

If we write text in our grey box tagged as markdown, pressing Alt-Enter will render it as plain text.

您还可以探索其他一些功能。但是现在我们已经有了 Jupyter notebook，可以开始写一些代码了！

### 设置 Anaconda 环境和安装包

现在我们已经建立了我们的编码平台。但是我们要从头开始写深度学习代码吗？这似乎是一件极其困难的事情！

好消息是，许多其他人已经编写了代码，并提供给我们！有了其他人代码的贡献，我们可以在非常高的水平上玩深度学习模型，而不必担心从头实现所有这些模型。这使得我们开始编码深度学习模型变得极其容易。

对于本教程，我们将下载深度学习实践者常用的五个包:

*   Tensorflow
*   Keras
*   熊猫
*   Scikit-learn
*   Matplotlib

我们要做的第一件事是创建一个 Python 环境。一个环境就像一个独立的 Python 工作副本，所以无论您在您的环境中做什么(比如安装新的包)都不会影响其他环境。为您的项目创建一个环境是一个很好的实践。

单击左侧面板上的 Environments，您应该会看到这样的屏幕:

![wKXNPPjAOJAtpF7h2qDmHRWPJcZPSTxwXGqA](img/806c212d3a5340302d638baa8fe01781.png)

Anaconda environments

点击列表底部的“创建”按钮。应该会出现这样的弹出窗口:

![JYiSWmlNr70YIJmJIzeiwgu4ZGDnGRH1sLH2](img/a237cb1456a75ff4d8fcf4b9faa3e927.png)

A pop-up like this should appear.

命名您的环境并选择 Python 3.7，然后单击创建。这可能需要一些时间。

完成后，您的屏幕应该如下所示:

![ZP0bAkPwg2ehjpiEbfqefPLsV4N7ao6tcqMM](img/65456eedffa25f480dd6e807da32e33e.png)

请注意，我们已经创建了一个“直观-深度学习”的环境。我们可以看到我们在这个环境中安装了哪些包以及它们各自的版本。

现在让我们将一些我们需要的包安装到我们的环境中！

我们将安装的前两个包名为 Tensorflow 和 Keras，它们帮助我们即插即用深度学习的代码。

在 Anaconda Navigator 上，单击当前显示“已安装”的下拉菜单，然后选择“未安装”:

![vQgrLpY-TCpmVQNpE8Qj8lDOXZiMSrnxOxbw](img/1bd724f3ae74969eb436fdc883dbe2bc.png)

您尚未安装的软件包的完整列表将如下所示:

![1AMaD4UNcBGBOhnRYTkTOA4z7VMXsxi3nVgx](img/903fa2fabcbfd88927b5b6263d7d0c33.png)

搜索“tensorflow”，并单击“keras”和“tensorflow”的复选框。然后，单击屏幕右下角的“应用”:

![42cYM75C41sZ0XQB2VM8SCCE5Smvaa5uFRxo](img/15020d82a189c768994e028ab6248d14.png)

弹出窗口应该如下所示:

![Grmx5-30t2TXd0n20TM3ySlKEp5xStkbQsuC](img/b90e69cc506adaaaf159df9317be1f78.png)

单击应用并等待片刻。完成后，我们将在环境中安装 Keras 和 Tensorflow！

使用同样的方法，让我们安装软件包'熊猫'，' scikit-learn '和' matplotlib '。这些是数据科学家用来处理数据以及在 Jupyter notebook 中可视化漂亮图形的常用软件包。

这是您应该在 Anaconda Navigator 上看到的每个包的内容。

**熊猫:**

![VJAyfunO6Bli1IzzaYyr7tPImAmYqtpW87l8](img/6898586f264b24e887a035966d382b95.png)

Installing pandas into your environment

**Scikit-learn:**

![f7d8Fc8ijy-9aXb-nBYSh8mZyMKZedqp-xtw](img/d6760a697f3bac7695e33283ef5169f1.png)

Installing scikit-learn into your environment

**Matplotlib:**

![OsHyfrHq4ipaGbL4yzm8FrJguUuZdGTcbLLS](img/b7e58974751a9a187f860c40d899d58b.png)

Installing matplotlib into your environment

完成后，返回 Anaconda Navigator 左侧面板的“Home”。你应该会看到这样一个屏幕，上面写着“直觉深度学习的应用”:

![pNCTBZKHAKE3o1PqfPMaLo6c-adW8W7sMk70](img/4146180149e8f8b2e9907c17714d054e.png)

现在，我们必须在这种环境下安装 Jupyter 笔记本。所以点击 Jupyter 笔记本 logo 下的绿色按钮“安装”。(又)要花一些时间。安装完成后，Jupyter 笔记本面板应该如下所示:

![utwIe99nFcwxxpNPDn-1Hufq2PzvnAXrLdvG](img/8c78466f3b12baafd62bd14e27307058.png)

点击启动，Jupyter 笔记本应用程序应该会打开。

创建一个笔记本，键入这五段代码，然后单击 Alt-Enter。这段代码告诉笔记本，我们将使用您在本教程前面用 Anaconda Navigator 安装的五个包。

```
import tensorflow as tf
```

```
import keras
```

```
import pandas
```

```
import sklearn
```

```
import matplotlib
```

如果没有错误，那么恭喜您，您已经正确安装了所有东西:

![saAFY17Par0VXZyDXdi332aKEXsLIZ9527a8](img/3b604ad5005a1467465b28ae28da0c71.png)

A sign that everything works!

现在我们已经设置好了一切，我们将在这里开始构建我们的第一个神经网络:

[**用 Keras 建立你的第一个神经网络来预测房价**](https://medium.com/intuitive-deep-learning/build-your-first-neural-network-to-predict-house-prices-with-keras-eb5db60232c)
[*一步一步完整的初学者指南用几行代码建立你的第一个神经网络像一个深…*medium.com](https://medium.com/intuitive-deep-learning/build-your-first-neural-network-to-predict-house-prices-with-keras-eb5db60232c)

如果你在上面的任何步骤中有任何问题，请随时在下面评论，我会帮你解决的！

**关于作者:**

你好，我是约瑟夫！我最近从斯坦福大学毕业，在那里我和吴恩达一起在[斯坦福机器学习小组](https://stanfordmlgroup.github.io/)工作。我想让深度学习概念尽可能直观，尽可能容易被每个人理解，这激励了我的出版:[直观的深度学习](https://medium.com/intuitive-deep-learning)。