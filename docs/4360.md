# 如何在互联网上保持安全:一直以来都是代理服务器

> 原文：<https://www.freecodecamp.org/news/how-apps-stay-safe/>

这篇文章概述了代理服务器是如何构成在线匿名的基础的。我们将讨论如何使用代理来帮助用户和 web 应用程序。

在线隐私的一个核心方面是代理服务器的使用——尽管这一基本构件在其更易识别的形式下可能不可见。

如今，对于开发者、软件产品所有者以及互联网上的普通人来说，代理服务器是一个需要了解的有用的东西。

让我们来探索是什么让代理服务器成为网络安全支持的重要组成部分。

![Internet_dog](img/9d7806842d6f996445f8cc7a33b9352e.png)

“On the Internet, nobody knows you’re a dog.” Comic by Peter Steiner of The New Yorker.

当彼得·斯坦纳的漫画《T1》于 1993 年首次在《纽约客》上发表时，据报道它基本上没有引起注意。直到后来，网络匿名这一不祥且有点可怕的暗示才以未知事物冰冷的手指触动了公众意识。

随着互联网的使用变得越来越流行，用户开始担心其他人可以以他们选择的任何方式在网上表现自己，而其他任何人都不知道他们的真实身份。

简而言之，这种情况已经不复存在。由于[追踪 cookie](https://support.mozilla.org/en-US/kb/enable-and-disable-cookies-website-preferences)、[浏览器指纹](https://robertheaton.com/2017/10/17/we-see-you-democratizing-de-anonymization/)、[互联网服务提供商(ISP)将你的浏览日志卖给广告商](https://www.privacypolicies.com/blog/isp-tracking-you/)，以及你自己莫名其妙地倾向于将你的名字和面孔放在社交网络上，在线匿名就像去年的 LaCroix 风味一样过时了。

你的隔壁邻居可能不知道如何在网上找到你(嗯，除了通过你正在使用的基于位置的二手市场应用程序)，但你可以肯定，至少有一家大型广告公司在某个地方有一系列 0 和 1，代表你、你的市场人口统计的具体细节以及你的所有在线习惯。包括你喜欢的拉克鲁瓦口味。

有很多方法可以增加一些模糊层，比如使用隐藏你 IP 的公司防火墙，或者使用 Tor T3 的 T2。

这两种方法的基本机制是相同的。就像藏在洋葱的各层中一样，你使用一个或多个代理服务器来保护自己免受第三方的追踪。

#### 代理服务器到底是什么？

在传统的英语定义中，代理是“代表另一个人的权威或权力。”([韦氏词典](https://www.merriam-webster.com/dictionary/proxy))在计算环境中，代理服务器是代表另一个服务器或用户机器的服务器。

例如，通过使用代理浏览互联网，用户可以推迟被个人识别。所有用户的互联网流量似乎都来自代理服务器，而不是他们的机器。

#### 代理服务器是为用户服务的

作为客户，当您上网时，有几种方法可以使用代理服务器隐藏您的身份。重要的是要知道这些方法提供不同级别的匿名，没有一种方法能真正提供真正的匿名。

如果其他人主动在互联网上寻找您，无论出于何种原因，您都应该采取进一步措施，使您的活动更难被识别。(这些步骤超出了本文的范围，但是您可以从[电子前沿基金会(EFF)的监视自我防御](https://ssd.eff.org/)资源开始。)

然而，对于互联网上的普通狗来说，这里有一个很小的选项菜单，范围从最少到最多匿名。

#### 在 web 浏览器中使用代理

某些 web 浏览器，包括 Mac 上的 Firefox 和 Safari，允许您配置它们通过代理服务器发送互联网流量。

代理服务器试图通过用代理服务器自己的 IP 地址替换您的原始 IP 地址来匿名化您的请求。这为您提供了一些匿名性，因为您试图访问的网站不会看到您的原始 IP 地址。但是，您选择使用的代理服务器将确切地知道是谁发出了请求。

这种方法也不一定会加密流量，阻止 cookies，或者阻止社交媒体和跨网站追踪者跟踪你；从好的方面来说，这是最不可能阻止使用 cookies 的网站正常运行的方法。

![0_07snWvA_wsI_deAm](img/d86c580629c9445d6698b399fd012ee3.png)

公共代理服务器就在那里。决定你是否应该使用它们中的任何一个就像决定你是否应该吃一颗微笑的陌生人递给你的糖果一样。

如果你的学术机构或公司提供了一个代理服务器地址，它(希望)是一个安全的私有服务器。

如果你有一点时间和每月几美元投资于安全性，我的首选方法是与亚马逊网络服务公司或 T2 数字海洋公司建立自己的虚拟实例，并将其用作代理服务器。

要通过浏览器使用代理，你可以[在 Firefox](https://support.mozilla.org/en-US/kb/connection-settings-firefox) 中编辑你的连接设置，或者[在 Mac 上使用 Safari 设置一个代理服务器](https://support.apple.com/guide/safari/set-up-a-proxy-server-ibrw1053/mac)。

关于选择浏览器，我很乐意向任何想要增强开箱即用浏览体验安全性的互联网用户推荐 [Firefox](https://www.mozilla.org/en-US/firefox/new/) 。

自从我听说了隐私第一以来，Mozilla 一直是隐私第一的倡导者，最近对 Firefox 浏览器中的[增强跟踪保护进行了一些广受欢迎的更改，默认情况下，这些更改会阻止社交媒体跟踪器、跨站点跟踪 cookies、指纹打印机和密码矿工。](https://blog.mozilla.org/blog/2019/06/04/firefox-now-available-with-enhanced-tracking-protection-by-default/)

#### 在您的设备上使用 VPN

为了将代理服务器用于所有的互联网使用，而不仅仅是通过一个浏览器，您可以使用虚拟专用网络(VPN)。

VPN 是一种服务，通常是付费的，通过他们的服务器发送你的互联网流量，从而充当代理。VPN 可以在您的笔记本电脑以及电话和平板设备上使用，因为它包含了您的所有互联网流量，所以除了确保您的设备连接外，它不需要太多额外的工作。

使用 VPN 是防止好管闲事的 ISP 窥探你的请求的有效方法。

![1_B8XYVrDTlI8KXcm27dFmUQ](img/60a626a1d928b7219d2fc7ee9b03b0dd.png)

要使用付费的第三方 VPN 服务，你通常需要在他们的网站上注册并下载他们的应用。重要的是要记住，无论你选择哪家提供商，你都是在将你的数据委托给他们。

VPN 提供商匿名化你在互联网上的活动，但是他们自己可以看到你所有的请求。提供商在他们的隐私政策和他们选择记录的数据方面有所不同，因此有必要做一些研究来确定你可以放心地信任哪一个(如果有的话)。

你也可以通过使用虚拟实例和 [OpenVPN](https://openvpn.net/) 推出自己的 VPN 服务。OpenVPN 是一个开源的 VPN 协议，可以和少数虚拟实例提供商一起使用，比如[亚马逊 VPC](https://openvpn.net/amazon-cloud/) 、[微软 Azure](https://openvpn.net/microsoft-azure/) 、[谷歌云](https://openvpn.net/google-cloud-vpn/)和[数字海洋水滴](https://openvpn.net/digital-ocean-vpn/)。

我之前写过一篇关于使用 EC2 实例用 AWS 设置你自己的个人 VPN 服务的教程。我已经亲自运行这个解决方案大约一个月了，它总共花费了我将近 4 美元，这个价格让我很放心。

#### 用于

Tor 利用代理服务器提供的匿名性，并通过由其他服务器组成的[中继网络](https://en.wikipedia.org/wiki/Relay_network)转发你的请求，每个服务器被称为一个“节点”

您的流量在到达目的地的途中会经过三个节点:守卫*节点*、*中间节点*和*出口节点*。在每一步，请求都被加密和匿名，这样当前节点只知道将它发送到哪里，而不知道请求包含什么。

这种知识分离意味着，在所讨论的选项中，Tor 提供了最完整的匿名版本。(要获得更完整的解释，请参见罗伯特·希顿关于 Tor 如何工作的文章，这篇文章写得太棒了，我真希望是我自己写的。)

![1_S-lzVbWxv3Y_iskGZ3aCwQ](img/a5401e48b955c770fbf7ab679b87d48d.png)

也就是说，这种程度的匿名是有代价的。不是金钱，因为 [Tor 浏览器](https://www.torproject.org/download/)是免费下载和使用的。然而，它比通过浏览器使用 VPN 或简单的代理服务器要慢，因为你的请求要走迂回的路线。

#### 代理服务器也适用于服务器

现在，您已经熟悉了在保护用户上网时使用代理服务器的情况。然而，代理不仅仅是为客户服务的。网站和互联网连接的应用程序也可以使用[反向代理服务器](https://en.wikipedia.org/wiki/Reverse_proxy)进行混淆。“反向”部分仅仅意味着代理代表服务器，而不是客户端。

为什么网络服务器会关心匿名？一般不会。至少不会像某些用户那样。

出于几个不同的原因，Web 服务器可以从使用代理中获益。例如，他们通常通过[缓存](https://en.wikipedia.org/wiki/Web_cache)或[压缩](https://en.wikipedia.org/wiki/HTTP_compression)内容来优化交付，从而为用户提供更快的服务。

然而，从网络安全的角度来看，反向代理可以通过混淆底层基础设施来改善应用程序的安全状况。

![1_9SL4Lh-A5dFsQSaCHT2Ekw](img/cdae9bf7cbfd7af1eaccbc550ef287da.png)

基本上，通过在直接访问所有文件和资产的 web 服务器前面放置另一个 web 服务器(“代理”)，您使得攻击者更难找到您的“真正的”web 服务器并破坏您的东西。

就像当你想要见商店经理，和你说话的店员说，“我代表经理说话”，你甚至不确定*是不是经理，不管怎样，但是你成功地把他们卖给你的粉色小马换成了*紫红色的*小马，非常感谢，所以现在你不再关心经理是谁，他们是否真的存在， 如果你在街上遇到他们，你不能阻止他们，也不能叫他们出去，因为他们把粉红色冒充成紫红色，经理对此没意见。*

一些常见的 web 服务器也可以充当反向代理，通常只需简单的配置更改。虽然我不知道您的特定架构的最佳选择，但我将在这里提供几个常见的示例。

#### 使用 NGINX 作为反向代理

NGINX 使用其[配置文件](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)(默认为`nginx.conf`)中的`proxy_pass`指令将自己变成一个反向代理服务器。该设置要求在配置文件中放置以下行:

```
location /requested/path/ {proxy_pass http://www.example.com/target/path/;}
```

这指定了对路径`/requested/path/`的所有请求都被转发到`http://www.example.com/target/path/`。目标可以是域名或 IP 地址，后者可以有也可以没有端口。

使用 NGINX 作为反向代理的完整指南是 NGINX 文档的一部分。

#### 使用 Apache httpd 作为反向代理

Apache httpd 同样需要一些简单的配置来充当反向代理服务器。在[配置文件](https://httpd.apache.org/docs/current/configuring.html)，通常是`httpd.conf`，设置如下指令:

```
ProxyPass "/requested/path/" "http://www.example.com/target/path/"ProxyPassReverse "/requested/path/" "http://www.example.com/target/path/"
```

`ProxyPass`指令确保对路径`/requested/path/`的所有请求都被转发到`http://www.example.com/target/path/`。`ProxyPassReverse`指令确保 web 服务器发送的头被修改为指向反向代理服务器。

Apache HTTP server 的完整的[反向代理指南可以在他们的文档中找到。](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html)

#### 代理服务器*大部分*一路向下

我承认我的标题有点滑稽，因为网络安全最佳实践并不真的是某种永恒的无限回归之谜(尽管它们有时看起来可能是)。

无论如何，我希望这篇文章有助于你理解什么是代理服务器，它们如何有助于客户端和服务器的在线匿名，以及它们是网络安全实践中不可或缺的组成部分。

如果你想了解更多关于个人在线安全的最佳实践，我强烈推荐你浏览由 [EFF](https://www.eff.org/) 提供的文章和资源。关于保护网站和应用程序的指南， [OWASP 备忘单系列](https://github.com/OWASP/CheatSheetSeries)是一个极好的资源。