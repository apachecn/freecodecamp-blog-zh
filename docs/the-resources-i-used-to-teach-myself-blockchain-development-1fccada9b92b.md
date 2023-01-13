# 我用来自学区块链开发的资源

> 原文：<https://www.freecodecamp.org/news/the-resources-i-used-to-teach-myself-blockchain-development-1fccada9b92b/>

我从去年开始投资加密货币，并一直从那里掉进了区块链的兔子洞。尤其是在我居住的地方，区块链社区的大部分人都专注于加密货币的交易和投资。虽然一开始投资很有趣，但我对此并不感兴趣。因此，我成立了自己的本地聚会小组，专注于区块链的发展。

meetup 群组让我能够和社区成员一起联系和学习，我用它来编制一个我和其他成员都觉得有用的资源列表。这些资源从最基本的区块链解释到底层系统以及在区块链之上构建应用程序都有安排。

外面有很多噪音。如果你有兴趣成为区块链的专业人士，我希望这能帮助你理解这一切。

### **目录:**

1.  学习基础知识
2.  用以太坊开发 Dapp
3.  博弈论
4.  密码系统
5.  音频/辅助材料
6.  其他类型的区块链开发
7.  研究

### **基础知识——区块链技术如何工作**

你可能需要一分钟来理解区块链技术的复杂性。这项技术涵盖了许多不同的领域:计算机科学、博弈论、密码学和经济学等等。因此，一开始很难了解它是如何工作的。

这里有一些资源，我认为它们很好地、清晰地概述了区块链是如何运作的。

1.  **从这个视频开始，分解它是如何工作的:**

**2。在这里观看两个视频(与之前的资源有一些重叠，但它会巩固你脑海中的概念)并在网站上播放演示:**

[**区块链演示**](https://anders.com/blockchain/)
[*一个浏览器中的区块链直播演示。*anders.com](https://anders.com/blockchain/)

3.**阅读[GitHub 书中的章节“什么是以太坊](https://github.com/ethereumbook/ethereumbook/blob/develop/what-is.asciidoc)”、[掌握以太坊](https://github.com/ethereumbook/ethereumbook)、**

### **用以太坊开发 Dapp】**

现在有许多不同的区块链，允许您创建应用程序和智能合同。以太坊是目前最受欢迎的选择，Solidity 是它的主要编程语言。我建议先尝试用这些技术构建 dapps。

到目前为止，学习编写可靠代码的最好方法是 T2 隐型僵尸 T3。这是一个交互式编码环境，教你如何一步一步地编写 Solidity 程序，同时构建一个僵尸游戏！它还与新版本的 Solidity 保持同步，这在不断变化的区块链空间中很难实现。

**如果你想要除了隐型僵尸之外的东西，这里有我对学习坚固性的另外两个建议:**

1.  用于 dapp 开发的 Youtube 视频系列 —这个频道很好地解释了事情，但是语法并不是完全最新的，所以如果你遇到错误，你可能必须谷歌一些东西。他用的混音编辑器会给你提示你需要改什么，所以应该没问题。
2.  Udemy 上的 Stephen Grider——这是一门付费课程，但你可以花大约 9.99 美元就能买到，它有很好的例子和内容。

在你完成 Cryptozombies 之后，学习如何使用 [Remix IDE](http://remix.solidity.com) 来创建、调试和部署契约是一个好主意。[的文档有一个快速的开始和许多带截图的逐步说明](https://media.readthedocs.org/pdf/remix/latest/remix.pdf)让你开始。

你还应该了解以太坊[客户端](https://github.com/ethereumbook/ethereumbook/blob/3812a5dfa5b851a1aaa52b14c5aea6c74629e5a0/clients.asciidoc)和[钱包](https://github.com/ethereumbook/ethereumbook/blob/3812a5dfa5b851a1aaa52b14c5aea6c74629e5a0/wallets.asciidoc)。这些链接会解释你需要知道的一切。Metamask 是一个浏览器插件，也是一个很好的入门方式(它适用于 Chrome 或 Firefox，但 Chrome 的效果似乎好得多)。

接下来，学习更高级的智能合约开发。从阅读[固体文件](https://solidity.readthedocs.io/en/v0.4.24/solidity-by-example.html)开始。它深入到更高级的概念，也有一些很好的例子。Ethereum.org 也有一些很好的 dapp 的例子，比如[这个](https://www.ethereum.org/dao)。你可以把例子直接复制到 Remix IDE 中，自己测试一下。

在您很好地掌握了可靠性和智能契约之后，开始浏览一些开源示例。默认的做法似乎是 [Crypto Kitties](https://etherscan.io/address/0x06012c8cf97bead5deae237070f9587f8e7a266d#code) (你可以在 [etherscan.io](https://etherscan.io) 的任何以太坊地址看到合同代码)，但还有许多可以成为很好的学习工具。你可以搜索 GitHub 和 Etherscan 来找到更多。

围绕开发者工具和安全性，以太坊领域正在进行大量开发。这里有一些很棒的库和工具，你可以看看:

*   [开齐柏林飞艇](https://github.com/OpenZeppelin/openzeppelin-solidity)
*   [块菌发展框架](https://www.truffleframework.com/)
*   [ConsenSys —智能合同最佳实践](https://github.com/ConsenSys/smart-contract-best-practices)

### 博弈论

区块链试图解决的一些问题来自博弈论，最著名的是拜占庭将军的问题。这个问题处理许多不同方之间的共识，而不必相信任何个人是没有恶意的。

[The Great Courses Plus 提供了一个关于博弈论各种主题的优秀讲座系列](https://www.thegreatcoursesplus.com/game-theory-in-life-business-and-beyond)。他们有一个月订阅模式，有两周的免费试用期。24 次 30 分钟的讲座涵盖了博弈论的广泛主题，我认为这对全面理解这个主题很有帮助。

### 密码系统

我肯定不是这方面的专家，但我一直在学习密码学的工作原理，以及如何将它应用到区块链。这个领域确实需要深入数学，因为以太坊和许多其他区块链使用椭圆曲线加密。

作为这一领域的新手，这里有一些我认为有用的资源:

*   [Coursera Cryptography I](https://www.coursera.org/learn/crypto) —免费旁听课程；如果你想要一个证书就付钱。
*   [以太坊大师书中关于密码学的章节](https://github.com/ethereumbook/ethereumbook/blob/3812a5dfa5b851a1aaa52b14c5aea6c74629e5a0/keys-addresses.asciidoc)

### 音频辅助材料

*   [**播客:**软件工程日报，区块链](https://itunes.apple.com/us/podcast/blockchain-software-engineering-daily/id1230807219) —这是我最喜欢的区块链播客。他们在解释复杂话题方面做得非常好，节目中有各种行业领袖。
*   [**播客:** CryptoDisrupted](https://cryptodisrupted.com/) —主持人带来了许多来自区块链空间有趣项目的客人。我已经享受了这个播客的大部分内容。

### 其他类型的区块链开发

到目前为止，以太坊社区拥有最多的开发人员和学习资源，所以这是开始区块链开发的好地方。然而，我认为如果你不去探索这个领域的其他创新，那将是你的失职。下面是一些有趣的项目。

[**Lisk**](https://lisk.io/) —让区块链开发更容易，因为一切都是用 JavaScript 构建的。

[**EOS**](https://www.eos.io/) —创建者丹·拉里默(Dan Larimer)在开始这个项目之前已经建立了其他几个成功的区块链解决方案。EOS 应该可以解决以太坊的一些问题，比如扩展性和安全性。它有时被称为“以太坊黑仔”。

**链间协议** —这些解决方案有助于促进不同区块链之间的交易，也有助于区块链扩展的有趣解决方案:

1.  宇宙
2.  [Polkadot](https://polkadot.network/)
3.  [插页机](https://interledger.org/)

[**Hyperledger**](https://www.hyperledger.org/) —一项开源合作项目，旨在推进跨行业的区块链技术。它由 Linux 基金会主办。

[](https://holo.host/)**—一种后区块链技术，试图解决当今区块链技术中的可扩展性和集中化问题。**

### **研究和当前发展**

**一旦你学会了基础知识，阅读研究论文对掌握区块链空间是非常重要的。以下是我取得成功的一些地方:**

*   **[晨报—区块链文章](https://blog.acolyer.org/?s=blockchain)**
*   **【ICOs 白皮书集**
*   **[http://blockchain.mit.edu/](http://blockchain.mit.edu/)**
*   **[https://www.blockchainresearchinstitute.org/](https://www.blockchainresearchinstitute.org/)**

### **结论**

**我将继续研究区块链的发展，并试图找到新的和有趣的解决方案。如果我在这里遗漏了什么，请给我留言。**

**现在，我正计划写更多关于区块链空间中感兴趣的公司、项目和人物的文章。如果你对这些感兴趣，请跟我来。**