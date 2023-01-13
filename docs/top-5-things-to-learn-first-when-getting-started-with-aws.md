# AWS Cheatsheet:开始使用 Amazon Web Services 首先要学习的 5 件事

> 原文：<https://www.freecodecamp.org/news/top-5-things-to-learn-first-when-getting-started-with-aws/>

AWS 席卷了整个科技界。它很容易作为最可靠的提供商之一出售，拥有从对象存储到机器学习的详尽服务列表。

但是对于刚接触云计算的人来说，这很容易让人不知所措。尝试学习 AWS 应该从哪里开始？

*   [使用 AWS S3 的对象存储](#object-storage-with-aws-s3)
*   [使用 AWS S3 和 CloudFront 托管和部署静态网站](#host-and-deploy-a-static-website-with-aws-s3-and-cloudfront)
*   [使用 AWS Lambda 创建无服务器功能](#create-a-serverless-function-with-aws-lambda)
*   [使用 AWS EC2 启动托管服务器](#spin-up-a-managed-server-with-aws-ec2)
*   [学习 AWS 首字母缩写词](#learn-the-aws-acronyms)(认真地)

## 使用 AWS S3 的对象存储

S3 是 AWS 的对象存储解决方案。从一个非常简单的角度来看，S3 存储桶有点像云中存储静态文件的硬盘驱动器。这意味着，虽然你可以上传几乎任何东西到 S3，但是一旦它在那里，你除了下载或覆盖它之外，你真的不能做任何事情。

但是 S3 很便宜，存储是几乎每个网站都需要的服务。这使得 S3 成为真正有价值的服务，事实上是任何现代建筑的一部分。

你想储存一些简单的图像吗？把它们扔进 S3 桶里。有一些您为报告生成的 pdf 吗？从 S3 桶中存储和访问它们。

虽然您不能执行代码，但浏览器通过下载 JavaScript 等文件，然后自己执行这些文件来工作，因此它是静态 web 资源或照片的完美组合。

### 学习资源

*   亚马逊 S3(aws.amazon.com)
*   如何将文件和文件夹上传到 S3 存储桶？(docs.aws.amazon.com)

## 通过 AWS S3 和 CloudFront 托管和部署静态网站

让我们把刚刚学到的关于 AWS S3 的知识再向前推进一点。由于浏览器最终下载文件来使用它们，我们可以使用 S3 作为一种方式来托管静态网站与一个简单的复选框！

S3 提供了一个[配置选项](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/static-website-hosting.html),允许我们从一个简单的桶中服务一个网站。它设置了一个桶来允许浏览器可以识别的 HTTP 请求，这使得您刚刚编译的 [Gatsby](https://www.gatsbyjs.org/) 应用程序，甚至是一个 HTML 文件，成为 S3 的完美候选。

CloudFront 位于末端，为我们的网站提供 CDN(或内容交付网络)。S3 允许我们在一个桶中托管网站，CloudFront 位于桶的前面，作为一个缓存的分布式网络层，允许我们的网站比直接从 S3 更快地到达我们的访问者的浏览器。

### 学习资源

*   亚马逊 S3(aws.amazon.com)
*   [亚马逊云锋](https://aws.amazon.com/cloudfront/)(aws.amazon.com)
*   [如何托管和部署静态网站或 JAMstack 应用到 AWS S3 和 CloudFront](https://www.freecodecamp.org/news/how-to-host-and-deploy-a-static-website-or-jamstack-app-to-s3-and-cloudfront/)(freecodecamp.org)
*   [如何在 youtube.com S3&CloudFront](https://www.youtube.com/watch?v=1lDGDzmbQWg)上托管和部署静态网站或 JAMstack 应用
*   在亚马逊 S3(docs.aws.amazon.com)上托管一个静态网站

## 用 AWS Lambda 创建一个无服务器函数

如果你是无服务器世界的新手，这个想法并不是说实际上没有服务器。只是作为客户，你不必管理那些服务器。

大多数云提供商都有某种无服务器服务的解决方案，但最流行的是使用 AWS 的 Lambda 函数。

Lambdas 函数听起来像是一个函数，但它们运行在云中。您不必担心使该函数运行的任何资源，只需担心您想要在其中编写的环境，如 node 或 python。

这个功能强大而且便宜！它有助于将逻辑抽象为云中的单个功能，该功能可以根据您的需要进行扩展(考虑到它所接触的任何第三方服务都可以扩展那么多)。

你能用 Lambda 做什么？以下是几个例子:

*   向 S3 或数据库读写数据
*   用复杂的逻辑处理数据
*   使用 [Express](https://expressjs.com/) 创建 web 应用程序

熟悉 AWS Lambda 可以释放很多潜力！

### 学习资源

*   [AWS Lambda](https://aws.amazon.com/lambda/)(aws.amazon.com)
*   [从零开始学习 AWS Lambda](https://egghead.io/playlists/learn-aws-lambda-from-scratch-d29d?af=atzgap)(egghead . io)
*   [Cron Job AWS Lambda 函数教程–如何安排任务](https://www.freecodecamp.org/news/using-lambda-functions-as-cronjobs/)(freecodecamp.org)

## 使用 AWS EC2 启动受管服务器

AWS 的一大卖点是我们可以在云中完成所有的计算。这适用于任何事情！

AWS 的核心是 Amazon [EC2](https://aws.amazon.com/ec2/) (弹性计算云)，这是云中最简单的服务器。

使用 EC2，您可以使用各种可用的配置来启动服务器，在这里您几乎可以做任何您想做的事情。如果您只想执行简单的操作，可以从小规模开始，也可以纵向和横向扩展，为数据密集型操作提供强大的处理能力。

使用 EC2 可以做的一些事情包括:

*   创建一个新的 Wordpress 实例或者你最喜欢的 CMS
*   管理自定义 web 服务器
*   科学数据的计算机大量处理

### 学习资源

*   亚马逊 EC2(aws.amazon.com)
*   [如何在 AWS 上启动远程服务器](https://www.freecodecamp.org/news/getting-started-with-server-administration-on-aws/)(freecodecamp.org)
*   运行虚拟的 Ubuntu 桌面(ubuntu.com)
*   [从零到 AWS EC2 数据科学](https://medium.com/@junseopark/from-zero-to-aws-ec2-for-data-science-62e7a22d4579)(medium.com)

## 了解 AWS 首字母缩写

说真的。作为一名长期从事网站建设的前端工程师，对我来说，与团队中的其他人一起工作最有价值的事情之一就是学习各种 AWS 服务的缩写。

S3？EC2？CF？我可能会得出很多错误的答案，但是能够简单地知道这些事情的意思让我能够跟上对话。

尽管我主要从事前端工作，但我应该对静态应用程序的存储和托管位置有所了解。这意味着，我应该知道 S3 是一个简单的存储服务，它就像云中的硬盘驱动器一样存储静态文件。

但是学习缩写并不一定意味着你必须知道服务是如何工作的。

虽然我上面列出的要学习的东西实际上是很好的，但如果你只关注应用程序的前端，就不应该期望你理解 ELB(弹性负载平衡器)或 EMR(弹性 MapReduce)服务是如何工作的。但是当你和团队的其他成员一起工作时，能够知道它们是什么是非常有帮助的。

### 学习 AWS 缩略语的资源

有很多方法可以学习缩略语。你可以简单地进入 AWS 网站，通读所有服务的列表，但这可能不是最有效的方式。

有大量资源可用于学习 AWS 的基础知识，包括来自 freecodecamp.org 的 AWS 认证云从业者课程的[4 小时免费课程。](https://www.freecodecamp.org/news/aws-certified-cloud-practitioner-training-2019-free-video-course/)

虽然你不必参加考试来学习材料，但如果你想投资，这绝对是一个额外的奖励，因为这是你可以添加到简历中的另一件事，最终使你更有价值。

但是从关注你最常听到的服务或者你的团队使用的服务开始。这将有助于你成为团队中更有效率的一员。

## 帮助您管理 AWS 的其他工具

虽然您可以通过单独使用每个 AWS 服务来提高工作效率，但是有许多工具可以帮助您更轻松地使用这些服务。

### AWS SDK

直接来自 AWS 的是 [AWS SDK](https://aws.amazon.com/tools/) 。它有几种不同的语言版本，比如用于 Javascript 的[AWS SDK](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)，你可以通过 npm 使用[。](https://www.npmjs.com/package/aws-sdk)

借助 SDK，您可以直接在应用中与 AWS 服务进行交互。

### 无服务器框架

围绕无服务器世界的工具还很年轻，但是[无服务器框架](https://www.serverless.com/)是帮助构建无服务器应用的时间最长的工具之一。

不要与这个概念相混淆，无服务器将通过管理您想要的 AWS 服务的部署来帮助您加速 web 应用程序。

### AWS Lightsail

虽然你可以运行 EC2 实例来做任何你想做的事情，但是 AWS Lightsail 可以为你处理一些繁重的工作。

Lightsail 提供一些点击启动服务，比如构建新的 Wordpress 服务器或 LAMP 环境。

### AWS 放大器

如果您想要一个工具来管理您的服务并提高工作效率， [AWS Amplify](https://aws.amazon.com/amplify/) 可以帮助您快速构建包含各种服务的 web 和移动应用程序。

这包括身份验证、数据存储、分析、机器学习等等。

## 你已经在 AWS 工作了吗？

你最喜欢的服务是什么？在推特上告诉我！

[![Follow me for more Javascript, UX, and other interesting things!](img/1c93a38209f03fa2ee013e5b17071f07.png)](https://twitter.com/colbyfayock)

*   [？在 Twitter 上关注我](https://twitter.com/colbyfayock)
*   [？️订阅我的 Youtube](https://youtube.com/colbyfayock)
*   [✉️注册我的简讯](https://www.colbyfayock.com/newsletter/)