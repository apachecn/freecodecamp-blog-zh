# 每个区块链开发者都应该知道这些 Web3 和 Metamask 用例

> 原文：<https://www.freecodecamp.org/news/every-blockchain-developer-should-know-these-web3-and-metamask-use-cases-7f93c1f139b1/>

伊戈尔·亚洛沃伊

# 每个区块链开发者都应该知道这些 Web3 和 Metamask 用例

![0*JVmq6javfae3gzTA](img/ff711ed6685dbb3868ac2bcf86c69c0a.png)

### 更新

11 月 2 日 MetaMask 和其他 dapp 浏览器将默认停止暴露用户账户。这将使本文中的一些代码被破解。我将发布带有 web3 1.0 和新元掩码接口的更新版本。

Metamask 是网络上 dApps 的事实标准。它将一个 Web3 实例注入到一个 window 对象中，使其可供 JavaScript 代码使用。

我们将使用 Web3 0.20 版本，而不是 Web3 1.0。Web3 1.0 的代码会有所不同。

每个 dApp 都有它的使命，但它们与 Metamask 交互的方式是相似的。在本文中，我们将介绍处理 Web3/Metamask 交互的十个最常见的实践。

### #1.检测元掩码并实例化 Web3

根据 [docs](https://github.com/MetaMask/faq/blob/master/DEVELOPERS.md#partly_sunny-web3---ethereum-browser-environment-check) ，下面是最好的方法。

这是怎么回事？首先，我们检查 Web3 是否被注入。如果它是注入的，我们使用注入的提供者创建一个新的实例。这是为什么呢？因为我们要用自己的库版本，而不是 Metamask 注入的那个。

如果 Web3 不存在，我们尝试连接到本地主机提供者，比如 [ganache](https://truffleframework.com/ganache) 。

### #2.检查元掩码是否被锁定

元掩码可以安装但被锁定。为了与用户帐户交互并发送交易，用户必须解锁元掩码。

### #3.检查当前网络

主网之外还有很多测试网。通常，您的合同被部署到某个网络。您希望确保用户在同一网络上运行 Metamask。

### #4.获取当前帐户

一个用户可能在 Metamask 上有多个帐户，但是他们希望 dApp 与当前的帐户进行交互。

您应该总是从 Web3 实例中获取帐户。不要保留和重复使用它，因为用户可能会随时更改他们的帐户。

### #5.获取往来账户的余额

这里我们使用#4 中的函数`getAccount`并调用`getBalance`。简单。

### #6.检测当前帐户是否已更改

用户可以随时更改他们的帐户。你应该为此做好准备，并做出适当的反应。

### #7.检测元掩码是否锁定/解锁

类似于#6。用户可以随时锁定/解锁。你的 dApp 应该能正确处理。

### #8.处理取消/确认

一旦用户与您的 dApp 交互，您必须使用 Web3 API 发送一个事务。用户可以按下元掩码弹出菜单上的取消或确认按钮。如果处理不当，这可能会导致 UI 不一致。

为了立即返回事务散列，调用`contract.methodName.sendTransaction`。

### #9.获取交易收据

一旦你的 dApp 交易被挖掘，交易收据就变得可用。然而没有事件/通知，所以我们必须实现一个轮询机制。

### #10.监听 Web3 事件

坚实的事件是伟大的。假设您的契约实现了所有必要的事件，它们允许从难看的轮询切换到仅仅是推送机制。您可以完全避免轮询，只对事件做出反应。事件回调[返回](https://github.com/ethereum/wiki/wiki/JavaScript-API#callback-return)很多数据，但是我们最感兴趣的是`args`。

### 摘要

无论你的 dApp 是关于什么的，它仍然必须执行普通的任务，比如检测 Web3，获取账户状态和余额，识别当前网络，以及处理事务和事件。我们已经讨论了如何使用十个代码片段来实现它。

### 附言

这里的很多例子使用的方法可能会因为调用时元掩码的状态或某些变量未定义而抛出错误。在生产环境中，您应该将它们包装在`try/catch`中。
为了简单起见，这里使用了 Async/await。可以换成 Promise then/catch。

### 社会的

*   在 [LinkedIn](https://www.linkedin.com/in/ylv-io/) 上与我联系。
*   在 [twitter](https://twitter.com/ylv_io) 上关注我。

### 想要更多吗？

[**如何创建和部署自己的 EOS 令牌**](https://hackernoon.com/how-to-create-and-deploy-your-own-eos-token-1f4c9cc0eca1)
[*我们要弄清楚什么是 EOS 令牌，以及如何自己创建和部署一个。*hackernoon.com](https://hackernoon.com/how-to-create-and-deploy-your-own-eos-token-1f4c9cc0eca1)[**2018 年 DApp 跑一趟要多少钱**](https://hackernoon.com/how-much-does-it-costs-to-run-dapp-in-2018-87ee11fe1d5d)
[*你觉得你的 AWS 或者你网站的数字海洋账单要了你的命？*hackernoon.com](https://hackernoon.com/how-much-does-it-costs-to-run-dapp-in-2018-87ee11fe1d5d)medium.com

*最初发布于[ylv . io](https://ylv.io/10-web3-metamask-use-cases-ever-blockchain-developer-needs/)2018 年 10 月 15 日。*