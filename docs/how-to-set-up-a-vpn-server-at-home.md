# 如何在家里免费架设 VPN 服务器

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-a-vpn-server-at-home/>

在本文中，我将一步一步地指导您在 Linux 服务器上设置 WireGuard VPN。它可以让你从咖啡店等不安全的地方访问安全的互联网资源。

## 但是为什么是 VPN 呢？为什么是 WireGuard？

每当您从远程位置连接到(比如说)您的银行网站时，您就有可能将密码和其他敏感信息暴露给网络上的任何侦听者。

当然，希望银行网站本身将被加密，这意味着在银行和你的个人电脑或智能手机之间流动的关键数据将无法被沿途监听的任何人读取。

如果您是从家里或办公室连接呢？有了 VPN，您可以合理地确定，那些没有被常规加密隐藏的数据元素不会被错误的人看到。

但是如果你在机场或咖啡店通过公共 WiFi 路由器连接呢？你确定网络没有被入侵或者没有黑客在暗中监视吗？

为了应对这种非常现实的威胁，您可以在您的笔记本电脑或手机上打开一个到 VPN 服务器的连接。这样，所有的数据传输都通过虚拟隧道进行。您的敏感连接的每个部分都将对您所连接的本地网络上的任何人不可见。

WireGuard 是开源 VPN 世界三大玩家中最新的一个，另外两个是 IPsec 和 OpenVPN。

WireGuard 比其他产品更简单、更快速、更灵活。它是这个街区的新成员，但它很快就结交了一些重要的朋友。在 Linux 创建者 Linus Torvalds 本人的敦促下，WireGuard 最近被并入了 Linux 内核。

# 在哪里建立你的 VPN 服务器？

当然，你可以在家里组装一个 VPN 服务器，并通过 ISP 的路由器配置端口转发。但是在云中运行通常更有实际意义。

别担心。我向您保证，这种方式将更接近于一种快速、轻松的“设置好就忘了”配置。你在家里构建的任何东西都不太可能像 AWS 等大型云提供商提供的基础设施那样可靠或安全。

然而，如果你碰巧在家里有一个专业安全的互联网服务器(或者你愿意冒险用一个备用的树莓派)，那么它会以同样的方式工作。

感谢 WireGuard，无论是在云中还是在物理服务器上，让您自己的家庭 VPN 变得前所未有的简单。整个设置可以在半小时内完成。

# 做好准备

让您的云实例启动并运行，也许可以使用这里的教程。

确保端口 **51820** 对您的服务器开放。这是通过 AWS 上的*安全小组*和谷歌云上的 *VPC 网络防火墙*完成的。

在现代 Debian/Ubuntu 版本中，Wireguard 可以从包管理器中安装，如下所示:

```
sudo apt install wireguard 
```

或者和百胜一起，来自 EPEL 仓库:

```
sudo yum install kmod-wireguard wireguard-tools 
```

# 第一步:创建加密密钥

在服务器上要创建包含公钥和私钥的文件的任何目录中，使用以下命令:

```
umask 077; wg genkey | tee privatekey | wg pubkey > publickey 
```

在不同的目录中或在您的本地机器上对客户机进行同样的操作。请确保您稍后能够区分不同的键集。

为了快速设置，你可以使用一个在线密钥生成器。然而，我建议第一次手动操作。确保创建的文件中包含密钥哈希，因为您将在下一步中使用它们。

# 第二步:创建服务器配置

你需要做一个*。/etc/wireguard 目录中的 conf* 文件。您甚至可以使用不同的端口同时运行多个 VPN。

将以下代码粘贴到新文件中:

```
sudo nano /etc/wireguard/wg0.conf 
```

```
[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
# use the server PrivateKey
PrivateKey = GPAtRSECRETLONGPRIVATEKEYB0J/GDbNQg6V0s=

# you can have as many peers as you wish
# remember to replace the values below with the PublicKey of the peer

[Peer]
PublicKey = NwsVexamples4sBURwFl6HVchellou6o63r2B0s=
AllowedIPs = 10.0.0.2/32

[Peer]
PublicKey = NwsexampleNbw+s4sBnotFl6HrealxExu6o63r2B0s=
AllowedIPs = 10.0.0.3/32 
```

### 启动虚拟专用网

```
sudo systemctl start wg-quick@wg0 
```

如果您没有 systemd(如果您的实例运行的是 Amazon Linux，这可能是真的)，您可以使用`sudo wg-quick up wg0`。

# 第三步:创建客户端配置

首先在你的客户端机器上安装 Wireguard，如果你使用的是 Windows、macOS、Android 或 iPhone，可以在 Linux 上以同样的方式安装，或者通过 app store 安装。

如果您在第一步中使用了在线密钥生成器或 QR 脚本，那么您可以通过拍摄 QR 码来连接您的手机。

在客户端上安装 WireGuard 后，使用以下值对其进行配置:

```
# Replace the PrivateKey value with the one from your client interface
[Interface]
Address = 10.0.0.2/24
ListenPort = 51820
PrivateKey = CNNjIexAmple4A6NMkrDt4iyKeYD1BxSstzer49b8EI=

#use the VPN server's PublicKey and the Endpoint IP of the cloud instance
[Peer]
PublicKey = WbdIAnOTher1208Uwu9P17ckEYxI1OFAPZ8Ftu9kRQw=
AllowedIPs = 0.0.0.0/0
Endpoint = 34.69.57.99:51820 
```

根据您的使用情况，您可能需要许多可选的附加组件，例如为额外的安全层指定 DNS 或预共享密钥。

如果您在 Linux 上，启动客户机的方式与启动服务器的方式相同，或者在其他系统上通过应用程序本身启动客户机。

# 测试您的 VPN

在浏览器中键入“我的 ip”以发现您的公共 IP 地址。如果您获得的 IP 地址不同于您的计算机在启动 VPN 之前拥有的地址，那么您就成功了！

(如果您忘记了之前是什么，请尝试`sudo systemctl stop wg-quick@wg0`，检查并再次启动它。)

# 故障排除指南

确保您的服务器配置了 IP 转发。检查/etc/sysctl.conf 文件，或者运行:

```
echo 1 > /proc/sys/net/ipv4/ip_forward 
```

你的联系经常中断？将此添加到客户端配置的对等部分:

```
PersistentKeepalive = 25 
```

不确定它为什么不起作用？尝试使用客户端时，在服务器上尝试`sudo tcpdump -i eth`。

## 感谢您阅读本指南。

如果你想深入了解，可以考虑参加我在 WireGuard VPN 上的[付费曼宁课程。](https://www.manning.com/liveproject/secure-business-infrastructure-with-a-custom-vpn?a_aid=bootstrap-it&a_bid=b9d7d398&chan=VPN)