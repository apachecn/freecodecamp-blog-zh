# 我如何从 C++到 Python:一个概念上的改变

> 原文：<https://www.freecodecamp.org/news/how-i-went-from-c-to-python-a-conceptual-change-8bf29d059428/>

由阿霞 f

# 我如何从 C++到 Python:一个概念上的改变

![Yj09MUqBA6jsSjn-No3PFfq9Rcu6z07rkqo4](img/3678938ad7fff9cddc49a4e7a35465ca.png)

Photo by [Pop & Zebra](https://unsplash.com/photos/xDxiO7sldis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 介绍

人们说用 Python 编码非常简单，甚至 6 岁的孩子都能做。这是我在工作中开始用 Python 编程时的想法。那时我已经做了 4 年的全职软件开发人员，主要在 Linux 上用 C++编写，大量使用 QT 库。但是，一开始我写的 Python 代码很烂。

自从我做出这样的改变已经有大约 3 年了，我想现在是总结我在这段时间里所取得的进步的时候了。回想起来，我不仅改变了我的主要编程语言，还改变了我的工作环境和我思考代码的方式。

我不会深入讨论 C++和 Python 之间的细节和区别，因为网上有大量的资源，而是描述我自己的经历。我希望这篇文章对和我经历同样转变的人有用。

![gJvyoRQiZQyfjylGkcHimNA5Kef78j0DsMxB](img/31d1388fe5c66178c9c26755e6eed697.png)

Jumping from C++ to Python (Photo by [Erik Dungan](https://unsplash.com/photos/TZ-D7A7Oy0s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/deep-dive?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText))

### C++是潜水，Python 是浮潜

C++感觉就像潜入海洋的神奇奥秘——它很美，但需要更多的学习和实践，而且总的来说，你走过的路程并不是很长。Python 有点像浮潜——你一把头伸进水里就能看到美景，但不会再往下走多远。你继续在浅水区游泳，可以轻松游很长一段距离。从这个描述中可以清楚地看出，这些语言中的每一种都应该在正确的时间和地点使用。

#### 潜入 C++并幸存下来

C++更严格，对你的错误惩罚更严厉。如果你至少一次都没有遇到令人惊讶的*分段错误*，那么这就不是一次有效的编码会议。因此，它需要对计算机、编译器和语言有更深的理解。当你更深入的时候，你真的可以看到并被美好的东西所打动，比如编译过程和内存管理。

作为一名 C++程序员，我更关心语法调整和奇怪的例子。我总是知道在哪里分配内存以及如何释放内存。我写的程序更加独立，因为我更喜欢知道我的代码内部发生了什么。主要思想是，其他人编写的代码不太可靠，更容易出错，而且可能会占用大量内存。

我的主要日常工具是 **Vim** 和大量用于编写代码的插件、 **GDB** 用于调试，以及 **Valgrind** 用于分析我的内存使用和错误。我用 **g++** 编译，写了自己的**makefile**。当时，我并不觉得 IDE 对我有什么好处，而是宁愿拖慢速度，让我与代码失去联系。*回想起来，我非常依赖编译器来发现我的类型错误*。

![qq2GvVEIPB16B3NDqru9LJb-vhitY-Ofkzpo](img/8110d1224b368dcd9d83fba449f628ab.png)

Photo by [Jakob Boman](https://unsplash.com/photos/Td9FnTMHu0A?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/diving?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

#### Python 中的浅水游泳

切换到 Python 首先需要学习的一件事是如何放手——你不知道引擎盖下发生了什么，内存是在哪里分配和释放的，没关系。我们也鼓励你使用其他人编写的打包到库中的代码，因为这样可以节省你的时间，帮助你更快地编写代码。这并不意味着您需要编写非常慢的代码，并且依赖于无人维护和无功能的库，但是重点肯定是不同的。

当我开始用 Python 编码时，我第一次用 Python 写 C++代码。它起作用了，但是我没有从这门语言中获得任何好处。当我开始以更“Pythonic”的方式写作并开始使用库，以及更高级的概念如[生成器](http://book.pythontips.com/en/latest/generators.html)、[装饰器](http://book.pythontips.com/en/latest/decorators.html)和[上下文](http://book.pythontips.com/en/latest/context_managers.html)时，我的编码得到了改进。

作为一名 Python 开发人员，我倾向于首先寻找解决手头问题的库。Python 有丰富的[库生态系统](https://pypi.org/)和支持它的社区。有图书馆可以做几乎任何事情。下面是我日常使用的一些比较方便的: [**NumPy**](http://www.numpy.org/) 用于数值计算， [**OpenCV**](https://opencv.org/) 用于计算机视觉， [**json**](https://docs.python-guide.org/scenarios/json/) 用于读取 json 文件，[**SciPy**](https://www.scipy.org/scipylib/index.html)**用于科学计算， [**sqlite3**](https://docs.python.org/3.7/library/sqlite3.html) 用于数据库。**

**我的日常工具是带有 **IdeaVim** 插件的 **PyCharm** (是的，一个 IDE)。我开始使用它主要是因为它是一个强大的调试器，比默认的 Python 调试器 **pdb** 友好得多。我还使用 **pip** 来安装我需要的库。除非真的需要，否则我不再监控我的内存使用情况。**

**![RZNJFo27RiO1Azs7Mc-slNCDxvsWrV0A64He](img/f4617b2b6dbbf0bc19dd4b15736c5128.png)

Photo by [Channey](https://unsplash.com/photos/fYjIBVvUuRM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/snorkeling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)** 

### **一些实用技巧**

**如果你是一名 C++开发人员，并且考虑开始用 Python 编程，这里是我给你的建议:**

*   ****改掉旧习惯** —停止使用 C++编译器作为调试器。不要过度优化内存使用。避免编写类似 C++的代码。无论如何，尽量不要依赖类型。**
*   ****养成新习惯** —开始使用图书馆。编写 Pythonic 代码(但不要过度)。保持东西的可读性。使用更复杂的概念，比如生成器/装饰器/上下文。试试 PyCharm。**
*   ****使用 C++和 Python 通用库** —一些 C++库，如 OpenCV 和 QT，有一个 Python 接口。在 Python 中开始使用同一个库比从头开始学习一个新库更容易。**
*   ****不要忘记你的出身**——有时候 Python 太慢了，或者不是任务的最佳选择。这是你的 C++知识发挥作用的时候。在 Python 中使用 C++代码有很多种方式( **SIP** 、 **ctypes** 等)。**

### **结果**

**不管别人怎么说，切换到一种不同的编程语言，尤其是一种与你习惯的语言有根本不同的语言，并不容易。花时间去学习，去钻研，去发现。但最重要的是，要明白不仅语言要改变，你的编码风格和工作方法论也要改变。**

**祝你好运！**