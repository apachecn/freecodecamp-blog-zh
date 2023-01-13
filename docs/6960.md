# 见见测试模块的害羞巨人 doctests

> 原文：<https://www.freecodecamp.org/news/doctests-the-shy-giant-of-testing-modules-89454654a318/>

你用 Python 吗，连洗衣服都用？你是否发现单元测试很无聊，但是仍然不得不做，因为你在自动化测试中发现了价值？那么这篇文章就送给你了。

### 这个想法

如果你是一个 Python 爱好者，那么我相信你已经时不时地使用过 Python 控制台了。让我们假设您正在编写一些类似下面的内联函数，来试验一些东西:

```
$ python
Python 3.6.4 |Anaconda custom (64-bit)| (default, Jan 16 2018, 18:10:19) 
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> def addme(a):
...     return a + a
... 
>>> addme(2)
4
>>> addme(1.9)
3.8
>>> addme(0)
0
>>>
```

因此，您已经编写了一个内联函数，并对它进行了几次测试，以进行一些基本的验证。如果 Python 可以在运行时读取上面的输出并为您进行推理，会怎么样？这就是 doctests 背后的想法，我的朋友。

### 真的吗？

![V3es93rVCDb3SSQnkAYWHMY1T8zo38gaupW4](img/09897102b055b8a315286294d44562c6.png)

Calm down Patrick

是的，完全正确！Python 不时地引入了许多直观而令人心碎的特性，这些特性现在已经成为少数几种主要语言的主流。为什么这个不能呢？

### 文档测试的好处

doctests 的好处有很多。

首先，它们写起来简单得可笑。我的意思是，你甚至可以把它外包给你的表弟，他研究土壤原子的解剖(而不是编程)，因为他欠你修理他的计算机。

第二，除非你觉得复制粘贴很难，否则他们更乐于写作。不过，这次您必须熟悉终端环境。

第三，没有必要打开另一个文件(即使你可以把你的应用程序的所有文档测试放到一个文件中)来读取任何函数的测试代码，因为它们就在每个函数的签名下面。

最后，它们可以用 doctest 模块执行，即使不了解一点 Python 也可以阅读。

### 单元测试呢？

单元测试仍然提供了巨大的价值和先进的测试能力。虽然 doctests 对于验证简单和纯粹的函数非常有用，但是当您必须进行复杂的验证时(例如，如果在测试过程中调用了一系列命令)，它们就没有什么帮助了。

### 嘲讽呢？

Doctests 可以用一个名为 [Minimock](https://pypi.org/project/MiniMock/) 的伟大库优雅地处理嘲讽。我鼓励你试一试，并让我知道你的想法。

尽管我喜欢这个计划，但我更喜欢我的测试有单独的角色。我不希望我的 doctests 负载过重，而且千篇一律。但这真的是一个品味问题——如果你不同意也没什么错。如果是这样的话，我很乐意听听你的理由。

### 一个工作实例

光说不练，还是写点代码吧。下面是一个使用上述提示功能的工作示例。

```
addme(a):
    """
    This is a docstring, usually to explain the use of a function. Please do not confuse it with doctests. They both mean to provide forms of documentation, but doctests are executable too.
    >>> addme(4)
    8
    >>> addme('a')
    'aa'
    >>> addme(set())
    Traceback (most recent call last):
        ...
    TypeError: unsupported operand type(s) for +: 'set' and 'set'
    """
    return a + a

if __name__ == '__main__':
    import doctest
    doctest.testmod()
    print(addme(1))
```

这里有几点值得注意:

*   文档测试需要存在于文档字符串中。在那里，只需“复制-粘贴”Python 控制台快速测试的输出。
*   如果您需要模拟一个异常情况，您只需要添加异常消息的第一行和最后一行。您是否期望硬编码路径或执行任何奇怪的逻辑来获得文件的完整路径并完整地打印出来？拜托，Python 不会这么做的:)
*   您需要 doctest 库来运行测试。虽然鼻子不需要它。
*   您可以像往常一样用`python mydoctests.py`运行上面的代码

### 我需要我的测试作为 CI/CD/CT 循环的一部分运行。我有什么好处？

我不是来让你失望的，是吗？:)

除了单元测试之外， [nose](http://nose.readthedocs.io/en/latest/) 测试运行器还支持运行所有的文档测试。只要加上旗子`--with-doctest`就可以了。

### 好东西，我该何去何从？

*   阅读 Python [文档](https://docs.python.org/3.6/library/doctest.html)获得更多的抽搐和案例。
*   阅读这本神奇的[书](https://amzn.to/2sjvKoP)，尤其是第四章，它涵盖了文档测试。
*   检查其他语言的实现。例如，[节点的那个](https://github.com/davidchambers/doctest)
*   [Doctests 和 nose](http://nose.readthedocs.io/en/latest/plugins/doctests.html)

感谢您阅读这篇文章，我希望您喜欢它，并且这篇文章激发了您编写更多(更少痛苦)测试的兴趣。如果是，请用下面的按钮来传播爱。您也可以随意添加自己对 doctests 的想法和体验。

最初发表于[我的博客](https://perigk.github.io)。