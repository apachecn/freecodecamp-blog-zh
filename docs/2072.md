# 子网定义

> 原文：<https://www.freecodecamp.org/news/subnet-definition/>

子网是较大网络中的较小网络。

![image-116](img/e5b5082d3fd1fbdd9ba13875061a7578.png)![home-network-diagram](img/759decf9ba2d79786219d58e7c4f8903.png)

A simple network / subnet. Source: [What Is My IP Address?](https://www.popularmechanics.com/technology/a32729384/how-to-find-ip-address/)

子网通过简化从一台计算机或服务器到另一台计算机或服务器的路由，使网络更加高效。

此外，由于子网允许我们将网络分成更小的网络，它们极大地增加了可以连接到互联网的设备数量。

如果没有子网，每台互联网连接设备都需要自己的 IP 地址。在目前的 IPv4 系统下只有大约 42 亿个可能的 IP 地址，所以我们在很多年前就会用完。

子网通常使用子网掩码，子网掩码充当互联网流量的软过滤器。使用 IP 地址和子网掩码，设备可以判断出另一台设备是同一个网络的一部分(连接到同一个路由器或在同一个公司内)，或者在其他地方在线。

![network-and-host-bits](img/5c980ced29f410c13c24a9ac35b61259.png)

An IP address and subnet mask. Source: [IPv4](https://support.huawei.com/enterprise/en/doc/EDOC1100145159)

查看本文了解更多关于子网、子网掩码及其工作原理的信息。

## 相关技术术语:

*   [子网掩码定义](https://www.freecodecamp.org/news/subnet-mask-definition/)