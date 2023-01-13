# 什么是树莓派？规格和型号(2021 指南)

> 原文：<https://www.freecodecamp.org/news/what-is-raspberry-pi-specs-and-models-2021-guide/>

## 介绍

你对 IoT(物联网)好奇吗？)你有没有一直想尝试自己做机器人，智能镜子，或者喂鸟器相机？以商用机器的一小部分成本建造一台计算机怎么样？

如果你对这些问题中的任何一个回答是肯定的，你可能会喜欢玩树莓派。

在这篇文章中，我将解释什么是树莓派(和它不是什么。)然后我会向您展示一些您可以使用它的东西，最后，我会列出所有当前的型号及其规格。

## 什么是树莓派？

Raspberry Pi 是英国的 Raspberry Pi 基金会(T1)发明的单板计算机(SBC)。这是一个“致力于将计算和数字制作的力量传递到全世界人民手中”的慈善机构

树莓 Pi 的第一个模型于 2012 年发布，截至 2021 年，已经有五代板了。一款名为 Pico 的微控制器(稍后将详细介绍)于 2021 年初发布。

### GPIO 引脚

将 Pi 与普通计算机区别开来的是一组 40 个 GPIO(通用输入输出)引脚。

GPIO 引脚和它们听起来的一样。它们被设计成输入和输出单个比特。这意味着您可以使用它们来添加各种功能到您的 Raspberry Pi，使用开关、蜂鸣器、灯、传感器等等。

### 树莓皮帽子

有许多附加板可以使用 GPIO 引脚连接到 Raspberry Pi。

树莓派帽子(硬件附在顶部)是根据特定规格设计的附加板。Raspberry Pi 可以自动检测和配置帽子，使设置变得容易。

你可以买到各式各样的帽子和其他插件板，但这里有一些值得注意的:

*   **[PoE+HAT](https://www.raspberrypi.org/products/poe-plus-hat/)**–以太网供电帽子
*   **[传感帽](https://www.raspberrypi.org/products/sense-hat/)**——专为[天文 Pi 任务](https://astro-pi.org/)设计，传感帽包括陀螺仪、加速度计、磁力计，以及温度、湿度和气压传感器。

*   **[Adafruit 电容式触摸帽子](https://www.adafruit.com/product/2340)**——类似于一个 [Makey Makey](https://makeymakey.com/) ，这个帽子允许你使用任何导电物体来触发使用 Python 的事件。

### Raspberry Pi 操作系统

Raspberry Pi 通常运行某种形式的 Linux，但是您可以使用大量的操作系统。

在官网上，你会发现一个[操作系统镜像](https://www.raspberrypi.org/software/operating-systems/)列表可供下载。其中包括官方的 Raspberry Pi OS、Debian Buster、Ubuntu(桌面、核心、服务器。)

你还会发现 RetroPie，一个专门的游戏平台操作系统，以及 LibreELEC，一个专门为开源媒体播放器 [Kodi](https://kodi.tv/) 开发的轻量级 Linux 发行版。

### 树莓派 VS Arduino

你可能听说过 Arduino 板，想知道它们和 Raspberry Pi 有什么区别。

主要区别在于 Pico 和 RP2040 除外)是带有操作系统的完整计算机。您可以将 Pi 连接到键盘、鼠标和显示器，像使用 Mac 或 PC 一样使用它。

另一方面，Arduino 是一个微控制器。它不能像计算机一样独立工作，而是用计算机编程，然后用来控制像照相机、灯、机器人等东西。

正如 Arduino 官方网站所说:“Arduino 板能够读取输入…并将其转化为输出。”

## 树莓派是用来做什么的？

搜索互联网，你会发现大量使用 Raspberry Pis 创建的项目。

常见的使用案例包括家庭自动化、游戏控制台、服务器、WiFi 扩展器、流媒体设备、气象站和家用电脑。(有趣的事实:这篇文章的大部分内容是在树莓派上写的。)

在新冠肺炎疫情期间，树莓派甚至被用来控制一些重灾区的通风设备。

## 树莓 Pi 模型

目前正在生产的所有型号的 Raspberry Pi 都有 40 个 GPIO 引脚。

该列表不包括 Raspberry Pi 微控制器、Pico 和 RP2040。

你可以在 [Raspberry Pi 网站](https://www.raspberrypi.org/products/)上找到购买这些主板的地方。

| 模型 | 处理器 | 随机存取存储 | 连通性 | 通用串行总线 | 高清晰度多媒体接口 | 力量 | 价格 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 零 | BCM2835 | 512MB | 没有人 | 微型 USB OTG | 迷你 HDMI | 微型 USB | $5 |
| 零瓦特 | BCM2835 | 512MB | 802.11 宽带/宽带/无线局域网 | 微型 USB OTG | 迷你 HDMI | 微型 USB | $10 |
| 树莓 Pi 1 型号 A+ | BCM2835 | 512MB | 没有人 | 1 个 USB 2.0 接口 | 全尺寸 HDMI | 微型 USB | $25 |
| 树莓 Pi 1 型号 B+ | BCM2835 | 512MB | 100 base 以太网 | 4 个 USB 2.0 接口 | 全尺寸 HDMI | 微型 USB | $30 |
| 树莓 Pi 3 型号 A+ | BCM2837B0 | 512MB | 双频无线，蓝牙 4.2 | 1 个 USB 2.0 接口 | 全尺寸 HDMI | 通过微型 USB 的 5V DC | $25 |
| 树莓 Pi 3 型号 B | BCM2837 | 1GB | ethernet, wireless, BLE | 4 个 USB 2.0 接口 | 全尺寸 HDMI | 2.1A 通过微型 USB | $35 |
| 树莓 Pi 3 型号 B+ | BCM2837B0 | 1GB | 双频无线，蓝牙 4.2，BLE | 4 个 USB 2.0 接口 | 全尺寸 HDMI | 通过微型 USB 和以太网供电(PoE)实现 5V DC | $35 |
| 树莓 Pi 4 型号 B | BCM2711 | 2GB、4GB 或 8GB | 千兆以太网、双频无线、蓝牙 | 2 个 USB 3.0 和 2 个 USB 2.0 | 2x 微型 HDMI | 5V DC(通过 USB-C) | $35, $55, $75 |
| 树莓派 400 | BCM2711 | 4GB | 千兆以太网、双频无线、蓝牙 | 2 个 USB 3.0 和 1 个 USB 2.0 | 2x 微型 HDMI | 5 伏直流(通过 USB) | $70 |

## 结论

Raspberry Pi 是探索电子、硬件和计算机编程的一种经济实惠的方式。它可以用于各种各样的项目，从非常愚蠢的项目(比如仓鼠驱动的仓鼠绘图机)到非常重要的项目(比如发展中国家的 T2 计算机实验室)。)

既然你知道了基本知识，为什么不出去做点什么呢？无论你是把电容式触摸帽挂在你的树莓皮上，把它变成一架香蕉钢琴，还是在上面安装 Linux，用它来做作业，我希望你能有一段很棒的时间来创造一些又酷又有用的东西。