# 计算机网络— Traceroute 解释

> 原文：<https://www.freecodecamp.org/news/computer-networking-traceroute-explained/>

根据维基百科，`traceroute`是:

> 一种计算机网络诊断工具，用于显示路由(路径)并测量数据包在互联网协议(IP)网络中的传输延迟。路由的历史被记录为从路由(路径)中的每个连续主机(远程节点)接收的分组的往返时间；每一跳的平均时间总和是建立连接所花费的总时间的量度。Traceroute 会继续执行，除非所有(三个)发送的数据包丢失两次以上，然后连接会丢失，并且无法评估路由。另一方面，Ping 只计算从目的地点开始的最终往返时间。

`traceroute`可用于查找下载数据的最快来源，通常被渗透测试人员用来收集网络信息。

## 数据如何在互联网上传输

traceroute 上的每台计算机都通过其 IP 地址或唯一的网络连接来标识。

```
- The journey from one computer to another is known as a hop.
- The amount of time it takes to make a hop is measured in milliseconds.
- The information that travels along the traceroute is known as a packet.
```

以下是 traceroute 的一些重要细节:

*   从一台计算机到另一台计算机的路径称为一跳
*   跳数以毫秒为单位
*   沿着跟踪路由传输的信息称为数据包

如果 traceroute 无法访问计算机，它会显示“请求超时”无法访问的计算机的每个跃点列将显示星号，而不是毫秒计数。

## 使用

`traceroute`的大多数实现允许用户指定每一跳发送的查询数量、等待每个响应的时间、使用的端口等等。

这里有一个简单的 Linux 例子:

```
[root@example ~]#  traceroute -w 3 -q 1 -m 16 www.google.com
traceroute to www.google.com (216.58.200.36), 16 hops max, 60 byte packets
 1  192.168.4.2 (192.168.4.2)  0.136 ms
 2  *
 3  *
 4  *
 5  *
 6  *
 7  *
 8  *
 9  *
10  *
11  *
12  *
13  *
14  *
15  *
16  *
```

在上面的示例中，选择的选项是等待三秒钟(而不是五秒钟)，对每一跳只发送一个查询(而不是三个)，在放弃之前将最大跳数限制为 16(而不是 30)，将 www.google.com 作为最终主机。

这有助于识别可能阻止 ICMP 流量或 Unix ping 中的高端口 UDP 流量到达站点的不正确的路由表定义或防火墙。请注意，防火墙可能允许 ICMP 数据包，但不允许其他协议的数据包。

## IP 子网计算器

虽然与跟踪路由没有严格的关系，但 IP 子网计算器在运行网络诊断时是一个有用的工具。

IP 子网计算器通过计算适当的网络地址、子网掩码、广播地址和主机 IP 范围，帮助将 IP 网络划分为子网。对于简单的网络(如家庭局域网)，确定合适的值可能非常容易，但对于更复杂的子网划分，IP 子网计算器是一个很好的工具。

以下是一些在线 IP 子网计算器:

*   [https://www.calculator.net/ip-subnet-calculator.html](https://www.calculator.net/ip-subnet-calculator.html)
*   [https://www . subnet online . com/pages/subnet-calculators/IP-subnet-calculator . PHP](https://www.subnetonline.com/pages/subnet-calculators/ip-subnet-calculator.php)
*   [https://www.tunnelsup.com/subnet-calculator/](https://www.tunnelsup.com/subnet-calculator/)