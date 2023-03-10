# 我如何建立一个无服务器的创业公司

> 原文：<https://www.freecodecamp.org/news/how-i-built-a-serverless-startup-387fc6f61064/>

作者:维克拉姆·朗涅卡

# 我如何建立一个无服务器的创业公司

![I9ESsa0Hkln1KYCGAXKo9SUF3UYw3VVFG7rO](img/f87e093b9e93621fd14307ff858458ec.png)

让我开门见山地说吧，无服务器是个时髦词。但这也是一个很好的方式来提及我们如何构建软件的巨大变化。是的，仍然涉及到服务器，我们还没有摆脱它们。区别在于你不关心他们。无服务器关注的是你的代码，而不是运行它的基础设施。您可以在需要时租赁足够的计算能力来完成工作。

> 无服务器计算是从巨大的资源池中调用的能力。在你需要的时候，租赁适量的资源来完成你的工作。完事之后马上还给我。

我早在 2008 年就发现了无服务器计算，当时谷歌发布了一款名为 App Engine 的产品。我们正在努力为我们的创业和应用引擎基础设施看起来很有趣。我们最终将整个产品“Socialwok”建立在它的基础上。就在那时，我第一次意识到使用无服务器技术来构建产品有多么大的优势。

从那以后，事情发生了很大的变化，我们正在走向一个没有服务器的未来。不必担心管理基础架构，这有助于您更快地行动，更有创造力。我对此深信不疑，以至于我写了一整本书来宣扬托管基础设施的力量。

[https://www . Amazon . com/How-Build-Future-powered-software-ebook/DP/b01n 580 gjq](https://www.amazon.com/How-Build-Future-powered-software-ebook/dp/B01N580GJQ)

为了进一步发展这个想法，我决定建立一个无服务器的创业公司。一种消耗很少资源的产品，如果需要，可以扩展到数百万用户。当走向无服务器时，有几个选项可供选择。亚马逊 Lambda 和谷歌 Firebase plus 云功能是两个受欢迎的选项。我选择了谷歌，因为我更熟悉他们的技术。

我打造的产品 [Bell+Cat](https://bellpluscat.com/) 是帮助你变得井井有条的工具。把它想象成更简单的电子表格。我想让每个人都能免费使用这个产品。如果有一百万人决定使用它，那么我为他们服务的成本应该很低。

为此，我决定使用 Google Drive 来存储用户数据。它被证明是安全、可靠的，并且符合人们可能需要的所有东西。知道你的数据在某个熟悉的地方也很好。Google Drive 给了每个人 15GB 的免费存储空间，所以 [Bell+Cat](https://bellpluscat.com/) 是一个更好地利用它的好方法。我想确保非常高的安全性。为了实现这一点，我把它设计成你的数据只在 Google Drive 和你的浏览器之间流动。并且一直被加密。这使得它更加安全，我不必担心存储任何敏感的用户信息。

为了托管应用程序，我选择 Google Firebase 托管。这是一款非常棒的产品，它使用谷歌的 CDN 网络，以最快的方式向世界上的任何人提供应用。我喜欢用一个命令“firebase deploy”在几秒钟内启动新版本的 [Bell+Cat](https://bellpluscat.com/) 。我没什么别的事要做。

一旦应用程序开始获得用户，我就开始收到功能请求。更受欢迎的请求之一是寻求一种方式来与他人共享 [Bell+Cat](https://bellpluscat.com/) 文件，或者将其嵌入到他们自己的网站上。这个特性要求我处理一些后端计算。在传统方式中，这需要我运行一个服务器，但在这种情况下，我使用了云功能。这是一个真正的无服务器基础设施，并且很好地集成到 Firebase 中。云函数允许执行小代码片段。这个触发器可以是任何事件，如来自浏览器的请求。每次跑步你只需支付一小部分费用。一百万次运行的费用大约是 40 美分。重要的是要记住，这里没有需要您管理的服务器，甚至没有虚拟服务器。

任何构建软件的人都熟悉将一个大问题分解成小部分的过程。有了云功能，每个部分只有在被触发时才运行，你只需在触发时付费。我认为无服务器将帮助我们更有效地使用我们的计算资源。我们需要的服务器将会比现在少。随着技术接管一切，从长远来看，需要更少的服务器应该对环境有益。思考无服务器帮助我们用更小的团队构建更好的产品。在谷歌应用引擎上运行的 Snapchat。他们谈到他们的团队中没有任何人管理服务器。对于这种规模的产品来说，这是很不寻常的。

> "如果说我看得更远，那是因为我站在巨人的肩膀上。"
> —艾萨克·牛顿

做出好的技术选择可以在很多方面帮助你的想法。就像在 [Bell+Cat](https://bellpluscat.com/) 的案例中，它允许我保持产品的免费性，并且任何需要它的人都可以获得。它还允许单个开发人员继续专注于核心产品，并且非常高效。在谷歌云上实现无服务器给了[贝尔+猫](https://bellpluscat.com/)这么多免费的东西。世界上最快的网络非常高的安全级别，由许多非常聪明的人管理的复杂的计算基础设施。对我来说，最重要的好处是，当我结束这篇文章去睡觉时，有人会确保我的代码继续运行。

[**【Bell+Cat】一个简单的组织者在你的浏览器中**](https://bellpluscat.com/)
[*一个简单免费的工具让你井井有条。保存到 Google Drive。安全保密。使用模板或从…*bellpluscat.com](https://bellpluscat.com/)导入