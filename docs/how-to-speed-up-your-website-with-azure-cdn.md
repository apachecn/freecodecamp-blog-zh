# 如何用 Azure CDN 加速你的网站

> 原文：<https://www.freecodecamp.org/news/how-to-speed-up-your-website-with-azure-cdn/>

## 什么是 CDN？

内容交付网络(CDN)帮助您更快地交付内容。您可以提供在一段时间内保持不变的任何类型的内容，如图像、视频、CSS、JavaScript、HTML 文件、pdf 等。

CDN 是一组分布在世界各地的服务器，从*边缘服务器*传送内容。边缘服务器是离发出请求的地方最近的服务器。

根据请求，边缘服务器可以从其缓存中返回内容，也可以从*源服务器*获取内容。提供实际内容的服务器称为源服务器。

![CDN Overview](img/f281a8a5b18659590930d87ad38c00c0.png)

在上图中，边缘服务器位于世界各地，源服务器位于美国加利福尼亚州。当发出请求时，位于印度孟买的边缘服务器可能会联系源服务器，如果它不能提供内容的话。

## CDN 是如何工作的？

cdn 有四个主要部分:消费者、DNs、边缘服务器和源服务器。

![CDN Detail](img/f679a68b03ec0ceb3d82eb48d6a81b70.png)

当消费者提出请求时，它首先被其互联网服务提供商(ISP)接受。然后，ISP 将点击内容提供商的*权威 DNS* 。

> 权威 DNS 将 DNS 请求转换为 IP 请求。

建立权威 DNS 后，它会返回最近的边缘服务器的 IP 地址。然后，边缘服务器将在其自己的缓存中检查所请求的内容是否可用。

如果是，那么它返回内容。如果内容不可用，它从源服务器请求内容，并在检索时缓存它。

## CDN 的优势

### 低带宽消耗

许多 web 主机每月的带宽分配有限。如果你超过那个，你将被额外收费。

有了 CDN，你的大部分带宽将被节省，因为内容将由边缘服务器提供。

### 低延迟

边缘服务器缓存内容。因此，无论何时请求缓存内容，延迟都会显著降低。这是因为请求不会一直到达源服务器。

### 针对 DDoS 的安全性

几乎所有流行的 CDN 都有能力保护你的网络服务器免受分布式拒绝服务(DDos)攻击。

### 提高搜索引擎优化

加载时间是影响网站 SEO 排名的因素之一。如果你通过 CDN 提供大部分内容，加载时间会大大减少，有助于提高你的搜索引擎优化。

## 深入了解 Azure CDN

假设您已经创建了一个 Azure 存储帐户并托管了一个非常简单的站点，该站点将 Hello World 显示为 h1。现在你知道了 CDN 的好处，你想通过 CDN 服务你的简单站点。

您将拥有一个类似于*[【https://demostorageaccountarjav.z29.web.core.windows.net/】](https://demostorageaccountarjav.z29.web.core.windows.net/)*的端点(而不是 demostorageaccountarjav，它将是您的存储帐户的名称)。这里有更多关于如何[建立一个静态网站](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)的细节。

登录你的 Azure 门户，点击*从你的仪表板创建一个资源*。搜索 *CDN* ，将在市场中打开资源，如下所示。

![CDN Create Resource](img/64f9135a2a11c820c6b2e7bc28ba1ea9.png)

Create CDN

这将打开一个创建 CDN 配置文件的表单。CDN 配置文件是一组 CDN 端点。除了名称、资源组和定价层之外，这里没有什么需要填写的。

接下来，选中复选框以创建 CDN 端点。端点是消费者请求内容的地方。因此，如果您有多个站点，也可以创建多个端点。

我附上了一个截屏供您参考，在什么价值投入。由于 CDN 是一项全球服务，因此区域选择将被禁用。

![CDN Details](img/0e667b2a28e2e5c52388ac96f659f3ca.png)

CDN Details

您现在可以点击*创建*来生成轮廓和端点。创建需要几分钟时间。创建后，当您转到主屏幕时，您将拥有以下 4 种资源:

![Azure Resources](img/28bf8adefd4d3c4fae3d53a1b4318604.png)

Azure Resources

如前所述，CDN 配置文件是一组*端点*。要查看详细信息，请单击* *端点*资源。您将看到一个概述，其中有一个到*端点主机名*的链接。

![Endpoint Overview](img/3b3523e9cd2e2488bb2f66fda03083d2.png)

Endpoint Overview

当您打开端点主机名时，最初可能会显示“404 未找到”。你可能需要再等 10-15 分钟才能看到你的网站。

正如在一节中所讨论的，您可以配置端点的安全性、缓存、路由和许多其他功能。你可以在这里探索更多选项[。](https://docs.microsoft.com/en-us/azure/cdn/cdn-how-caching-works)

## 如何通过 SAS 令牌访问

您可能想知道，如果您的资源位于私有容器中，并且只能通过*共享访问签名* (SAS)令牌来访问，该怎么办。你很幸运！查询字符串按原样传递，因为 SAS 是一个查询字符串，所以很好。

继续创建新的存储帐户(禁用静态网站)。在 CDN 配置文件中添加一个指向新创建的存储帐户的新端点。

出于演示的目的，我创建了一个名为 *site* 的容器，具有私有访问级别。我上传了一个名为 Photo.jpeg*的 Blob 到一个 URL 为 https://demostorageaccountarjav.blob.core.windows.net[的存储账户中。](https://demostorageaccountarjav.blob.core.windows.net)*

你当然可以直接从 Azure 门户获取 SAS 令牌进行测试，但这不是你在现实世界中通常会做的。为此，您将在下面找到在 Node.js 中创建 SAS 令牌的简单代码片段。

```
const azureSasToken = require('azure-sas-token');

// default token validity is 7 days
let sasToken = azureSasToken.createSharedAccessToken('https://<service namespace>.servicebus.windows.net/<topic name or queue>',
                                '<signature key name>',
                                '<signature hash>');
console.log(`sasToken: ${sasToken}`);

// Specify your own validity in secs, two hours in this example
sasToken = azureSasToken.createSharedAccessToken('https://<service namespace>.servicebus.windows.net/<topic name or queue>',
                                '<signature key name>',
                                '<signature hash>', 
                                60 * 60 * 2);
console.log(`sasToken: ${sasToken}`); 
```

我们使用了一个简单的名为 [azure-sas-token](https://www.npmjs.com/package/azure-sas-token) 的 npm 包。生成 SAS 后，您的 URL 将类似于以下内容:

```
https://demostorageaccountarjav.blob.core.windows.net/site/Photo.jpeg?sp=r&st=2021-03-25T07:28:45Z&se=2022-02-02T15:28:45Z&spr=https&sv=2020-02-10&sr=b&sig=PD4HlRI8bDEirMevpYQgpx6drwh%2BE5EpILfXkQOMlvw%3D 
```

上面的 URL 直接指向存储帐户。所以继续改变原点，使它使用原点端点。

```
https://demowebsitearjav.azureedge.net/site/Photo.jpeg?sp=r&st=2021-03-25T07:28:45Z&se=2022-02-02T15:28:45Z&spr=https&sv=2020-02-10&sr=b&sig=PD4HlRI8bDEirMevpYQgpx6drwh%2BE5EpILfXkQOMlvw%3D 
```

当您访问该网站时，您将能够通过 CDN 查看受保护的资源。您可能需要等待几分钟和/或清除您的端点才能看到更新的内容。

## 结论

在我看来，每个人都应该使用内容交付网络。还有许多其他提供商，如 Cloudflare、S3 等。但是微软是提供各种服务的主要参与者之一。

如果你和我一样是 Azure 的粉丝，你绝对应该试试 Azure CDN。

对于任何反馈或问题，您可以与我联系。

查看[此处](https://arjavdave.com)获取更多类似的教程。