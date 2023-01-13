# 如何开始使用 CentOS

> 原文：<https://www.freecodecamp.org/news/getting-started-with-centos-15eac7215c99/>

作者:krasimir gatchinsky

# 如何开始使用 CentOS

![V4yHgyfsS-ZmZl92THEgwsn7zQ9lFd8zo7lf](img/b414feab078b2ea26ba95a5599bd72e5.png)

[CentOS-Logo.svg. (2018, March 2). *Wikimedia Commons, the free media repository*. Retrieved 16:11, January 5, 2019](https://commons.wikimedia.org/w/index.php?title=File:CentOS-Logo.svg&oldid=290142359)

你可以在这里 *下载 CentOS 版本 [*。*](https://wiki.centos.org/Download)*

CentOS 或 Community Enterprise OS 是基于 RHEL 或 Red Hat Enterprise Linux 的开源发行版。仅当您购买了支持包时，此功能才可用。此外，所有 RHEL 软件包都与 CentOS 完全兼容，提供了一个强大、稳定且易于管理的平台，确保了最高级别的免费操作安全性。

CentOS 开箱即可与 RHEL 二进制兼容，是服务器安装的首选平台。CentOS 最有价值的部分之一是长支持周期。例如，Fedora 的发布支持周期长达 13 个月，而 CentOS 的发布支持周期长达 7 年。这使得它非常可靠和可靠。

此外，CentOS 社区项目正在扩展其在大量平台上的可用性，如 Google、Amazon AWS 等。它也可以在通用的启用云初始化的映像中使用。

要了解更多有关 CentOS 的信息，请访问此处的 CentOS 项目。

#### 版本

![FexWtcAsU02omlWN2zMjEBD-S9O4KYaNDnv2](img/f70efa9e1cca3d9b19d8d5dc75867e82.png)

#### 例子

让我们来看一些关于 CentOS 7 安装和基本设置的详细说明。

1.  下载最新的 [CentOS。ISO](https://www.centos.org/download/)
2.  使用上述链接或使用 CentOS 官方下载页面下载 CentOS 的最新版本后，将其刻录到 DVD 或使用名为 [Unetbootin](https://unetbootin.github.io/) 的 LiveUSB Creator 创建一个可启动的 u 盘。
3.  创建安装程序可引导介质后，将 DVD/USB 放入系统相应的驱动器中，启动计算机，选择可引导单元，应会出现第一个 CentOS 7 提示符。在提示符下，选择 Install CentOS 7 并按[Enter]键。

![rk-uJQssZewGXgFP9sIRmUHGpHjUEIpz0ee2](img/c96cc1f39d551989a18a7b9d73499bc2.png)

4.系统将开始加载媒体安装程序，并出现一个欢迎屏幕。选择您的安装过程语言，这将帮助您完成整个安装过程，然后单击继续。

![21CN30c3nOpSCAb9XSlWJncObEl5fyqAIQEr](img/dd2cf083cdc4220643c5fcfb98add57f.png)![IIAp4gYwhWdcRvg0KP8de6Y7ivhgiZJ4SXvH](img/28ed9d5b72828c0ea3360e0e9ba406f4.png)

5.在下一步中，当前屏幕提示是安装摘要。它包含许多选项来完全定制您的系统。你首先要设置的是你的时间设置。单击日期和时间，从提供的地图中选择您的服务器的物理位置，并点击上方的完成按钮以应用该配置。

![zPoewuybibFqlg79nqCcDjEtKE1lxAi8RqXj](img/c33b7a2b8e1012b8d2d97a3a5ab10bc4.png)![udVF8vI0FuFqP8uFY-fXGeXIHcyVUYZcEmQg](img/ae59512db9d23d05d737afd59717853a.png)

6.接下来，选择您的语言支持和键盘设置。为你的系统选择你的主要和额外的语言，当你完成的时候，点击 Done。

![GNUOD3cBf0HukL0AdEFSaslfAXDfihcOeJxt](img/4a06d6ab51724045560df15faf17bccf.png)![0SOMnkOxnmkG4DSwnogamdTSlEOC45QFi23m](img/6be80cde7ee5bebfba84cff5641e2c50.png)

7.同样，点击加号按钮选择键盘布局，并使用正确的输入字段测试键盘配置。完成键盘设置后，您可以使用任何组合键在键盘之间切换。在我的例子中，我使用 Alt+Ctrl。选择所需的组合键后，再次按 Done 以应用更改并返回到安装摘要的主屏幕。

![YI7FkWgL8h9QTXALMgCCvqxrKgZarWNFh5gn](img/225cf2cd095c0fe13b666e3588736cbf.png)![r0NDr-cIq3UyaXoa0y3qgmE7EhiMLj8DZRS4](img/240bcb4a7afe374db0f2069c4549c83a.png)![yPi-j4-sjLXe4ShqPpR-bnKWwY2wO-keP5mv](img/e91b2e21c8f24227a5d776c97f4e0b2a.png)![aXcgQKMdphnhNTLzTcWWWt7B-3bduQ3b4AXm](img/b1bde936fed78ed7e8df271509c739eb.png)

8.现在，如果你不想使用英语，我们可以添加语言支持。点击“语言支持”打开对话框。

![caxBvCvaLqNJYHbuqCEnWz4SlzHcnjJWbSHN](img/02dc9c858088cad1c87227ef86311927.png)

9.默认情况下，CentOS 预装了英语，但我们可以轻松添加更多语言。在我的例子中，我添加了德语作为附加语言。选择后按 Done。

![p69eRIErcsIn7g1AMpUQskZ2gCN2hnpzxAI5](img/010d361f8a1473b01ca64b894623b252.png)

10.在下一步中，您可以使用本地 DVD/USB 介质以外的其他安装源来自定义您的安装，例如使用 HTTP、HTTPS、FTP 或 NFS 协议的网络位置。您甚至可以添加一些额外的存储库，但是只有在您知道自己在做什么的情况下才使用这种方法。因此，保留默认的自动检测安装介质，并点击完成继续。

![TOxIxWtUUaU6dck4gKzc2d5VPzbFaWbH7XP0](img/a83813deb18d619e3ebeb373a0952c8e.png)![5SagBXBx6G402fzVKdcQgKJXJ3vRqFv9j3Rr](img/80fa389f2c72b7b08dfc92582d819009.png)

11.接下来，您可以选择您的系统安装软件。在这一步中，CentOS 提供了许多服务器和桌面平台环境供您选择。但是如果你想要高度定制，特别是如果你打算使用 CentOS 7 作为服务器平台运行，那么我建议最小化安装，将兼容性库作为插件。这将安装一个最小的基本系统软件，以后您可以根据需要使用以下命令添加其他软件包:

![aiPGfy0I85i73SC5EYBucyDGslreAYyBUvai](img/ce0f15b8bfd765798c3add3786c1f736.png)![9VBciZwESM-gjiXOUsLz-88I7H9fLs1GghwC](img/b16b8772e52ddf206231ca50640d411a.png)![w4QVnApDtJTm1jcMIFnQ8z-Z7ZW68jdjXMxB](img/d6a4ddaf2c4bab288c8c95d0f6c2df5f.png)

12.现在是时候给你的硬盘分区了。点击安装目的地菜单，选择您的磁盘，并选择一个你想要的。我将配置分区。点击阅读更多关于选择哪个分区的信息[。](https://www.centos.org/docs/5/html/Installation_Guide-en-US/s1-diskpartitioning-x86.html)

![enMF4gpceUHqdwSWDhP6-n0Zvyb0122RP8NQ](img/7080243b8b1da2dde27221acf4b344c2.png)![7nKmF4m-peDAaJT-pJz7F4EvKAzW1fYbHIQX](img/010825c1d9d2081d6f632e4b55623287.png)

13.在下一个屏幕上，选择 LVM(逻辑卷管理器)作为分区布局，然后单击单击此处自动创建它们。该选项将使用 XFS 文件系统创建三个系统分区，自动重新分配您的硬盘空间，并将所有 LVS 收集到一个名为“centos”的大卷组中。11.

*   /boot —非 LVM
*   /(根)— LVM
*   互换——LVM

![N-pKNTOCYDUvsQjB7N5O8H2JV02iw08ODO93](img/a1492f713c8db0469beb440b40fe8068.png)![U7obhodVkpI38y4oV19VUrC4WU59fYo65s2H](img/6ec1f6efb12ab124bb4100858551a7cd.png)

14.如果您对安装程序自动创建的默认分区布局不满意，您可以完全添加、修改或调整您的分区方案。完成后，点击“完成”按钮，并在“更改摘要”提示上接受更改。

![fyZ3sVKHOs8pN77NHqcRDfwpYpuyyF4xlzsJ](img/5413c4999931ce70626395b686ed336c.png)

注意:对于那些硬盘容量超过 2TB 的用户，安装程序会自动将分区表转换为 GPT。但是如果您希望在小于 2TB 的磁盘上使用 GPT 表，那么您应该在安装程序引导命令行中使用参数 inst.gpt 来更改默认行为。

15.下一步是设置您的系统主机名并启用网络。单击网络和主机名标签，在主机名字段中键入您的系统 FQDN(完全合格的域名)，然后启用您的网络接口，将顶部的以太网按钮切换到 on。如果您的网络上有一个正常工作的 DHCP 服务器，那么它会自动为已启用的 NIC 配置所有网络设置，这应该会出现在您的活动接口下。

![aJaSFWrR0OhcclQfctFmPeShvAb4CrK-yJfO](img/1ab8d0153f15167347c03a2c0eaabb60.png)![6JGzTjzo0weuiVyPoGWOYzRRsh1Rl6C5GcHF](img/11dc909ab142f61ffb23cd0a359e09f8.png)

16.如果您的系统是服务器，最好在以太网卡上设置静态网络配置，方法是单击 Configure 按钮，然后添加所有静态接口设置，如下图所示。完成后，点击保存，通过开关按钮禁用和启用以太网卡，然后点击完成应用设置并返回主菜单。

否则:

![LSvNkE3Tc2i7Fa2q1DDT6YCtJ3C0WQpEam8G](img/15836a973245ffc3443ca80c78232fdb.png)![j44d6AGcV6OTDbjZOMx8MY8eFGVt4c5QDYOp](img/d8fb7696a241de9ccdaf2447e78327e9.png)![xNSaSz1Fe3lQ9dZHBqTha5Y4kciBIxVDs7R2](img/747c7e9131c85dc8ce5bc5a118e5b9bb.png)

17.根据您的静态 IP 环境添加地址、网络掩码和网关条目。在我的情况下，我使用地址 192.168.1.100，网络掩码 255.255.255.0，网关 192.168.1.1 和 DNS 服务器 8.8.8.8 8.8.4.4。这些值可能会因您的网络环境而异。按下保存键后。

重要提示:如果您没有 IPv6 internet 连接，请在 IPv6 选项卡上将 IPv6 从自动设置为忽略。否则，您将无法通过 IPv4 从该服务器访问互联网，因为 CentOS 似乎忽略了正确的 IPv4 设置，而是使用 IPv6，这将失败。

![zIVn34DFeRZBICltiKeJ6ZoLae4c2pN45Xdp](img/4ff0f653242ef79316d435e37e163fd6.png)

18.接下来，我们必须打开连接，如下图所示。之后，按完成。

![ZnGNcnOiXpdvvpMya2dRhJ4CaOY8pSRYLnaJ](img/56b5e3ddc2bef083f53dcfda55591f34.png)

19.现在是时候开始安装过程了，选择 Begin Installation 并为 root 帐户设置一个强密码。

![ea3kScov6z01BRxr4jOLM4NRJccCxUS3EDqN](img/00714c15ecb9e93bcd452bd132dae61b.png)

20.安装过程现在将开始，您将在下一个窗口中看到一个蓝色的小进度条。现在，我们必须设置 root 密码，并在用户创建选项中添加一个新的非 ROOT 用户。我将首先选择 root 密码。

![c5h9dW9aqyasoNX9aDwfInjkU5xnPGq4QAu2](img/3df1a0a9684b4839a0bcdfe9b9bed838.png)

21.输入您选择的安全密码，然后按 Done。

![2Ky7QzNMLccBT5QPLSqGbav50VsK3cH4JmBI](img/af65417f22a9114cf89bb6c1b37d13d2.png)

22.接下来，我们将进行用户创建。

![GeveORGJo-scuJeD1Z25sxVDSeNNeAN7Npr-](img/4cdf884cb52c8b814f9853aa34e0d9c9.png)

23.接下来，我将创建一个用户。在我的例子中，我使用了全名“管理员”和用户名“管理员”。选中使用此帐户需要密码选项，然后按完成。当然，您可以根据自己的选择使用任何值。

![3sGgDLaj28eqO4yvWYHgANUkwZn-kXz10LmY](img/6a3c358c07658e4df2f6179ea0930a8f.png)

24.按完成。请耐心等待安装完成。

![n8Qxnk5BdvJRkM6sntmR62uC4eV8GiNK7dFk](img/802e4397f494992f79f832ce9b955b1a.png)

25.安装完成后，它会要求重新启动服务器，只需按下完成配置。

![rTWWi5rPGgTBBjTN8vI6uInYwTqD9nhcb2Yy](img/a5aa101d3f015a37148f510a6d9ef678.png)

26.服务器将重新启动，随后会要求您输入用户名和密码。

![yQwZNdAYh2gDJ3cxbDzEiVFGQYnzz412Q3j8](img/d79b450a6c8ca49ad63c2b6a512e574b.png)

恭喜你！您现在已经在全新的机器上安装了最新版本的 CentOS。移除所有安装介质并重新启动计算机，以便您可以登录到新的 minimal CentOS 7 环境并执行其他系统任务，例如更新您的系统并安装运行日常任务所需的其他有用软件。

现在，我们已经准备好使用上面刚刚创建的用户进行登录，或者我们可以使用 root 凭据。

首次登录 CentOS。以 root 用户身份登录到服务器，这样我们就可以执行一些最后的安装步骤。

第一个是用 yum 安装所有可用的更新。

![yjaMZ33oDpbtjB21GtAnE-yh3GHkhHuZsEOp](img/e57304fd792de0d3488081c5fbb2ac14.png)

用“y”确认继续安装更新。我将安装两个命令行编辑器，以便能够在 shell 上编辑配置文件:

![eRLKdk7Ha3KRU3fgjZF847V9wIiHhxTpXqnO](img/8d950dcad28c8670988fc3a584afaa74.png)

#### 网络结构

CentOS 7.2 minimal 没有预装 ifconfig 命令，因此我们将按如下方式安装它:

![slhBMQEUoqGJkgEDUQOW-3rxa3WZCA7PD-V3](img/6b5eeca6925b44b988e68feb90b8b304.png)

如果您想更改或查看网络配置文件，只需编辑该文件:

![OliVYcSJvGNYWk9mDB5zqtMExcggQG5SqzbM](img/c6142f14981b65dd30e7e878d308a9a6.png)

当您配置一个静态 IP 地址时，它将是这样的:

![pofxOfE9lJnMwr4nIKs0AOyORp4k-r1eUMBI](img/aa3a1ee21f5449cc9a334e6530eb9465.png)![CNjWloceJVc8vHVn9BaiqeZ5c1hEhoYw7WuN](img/e157fb3a4f9c65eaf4afee2a0391ef54.png)

如果需要，更改这些值。

注意:上述设备名称可能会有所不同，因此请检查/etc/sysconfig/network-scripts 目录中的等效文件。

#### 调整/etc/hosts

按如下方式调整文件/etc/hosts:

![9f52fds4U3nqAdcQtDp5tv98MDuQ6nm09m3q](img/9f02b71ad5a03b9ba9acfd467c8a7771.png)

使值如下所示:

![4G8Ve0NTRP1Jx6fVcVNx6a7HGIGRiAVK02Hm](img/5fcd1aa9f56a24cd3f5d9db91013dc05.png)

恭喜你！现在我们有了基本的 CentOS 7 服务器设置。

现在您可能更喜欢使用 GUI，这里有多种风格可供您选择:

#### 安装 GNOME-Desktop:

通过输入以下内容安装 GNOME 桌面环境:

![8xrxZk0240pPvx-yslBxJXZih9jrhfOPaksU](img/c27153474915e77e74dec1c73bcf0789.png)

要启动 GUI，请在安装完成后输入:

![cZPdsALzVpA5DYKoToQ2lgMG7FDBftKHJgGY](img/3ad870e8174ce2576ad97990f507a006.png)![8O5W1tTbW4-caJbSU9qQxqIM5RaUfugFt6BJ](img/7cda7b224c68d15c36ec9a5061ce02b5.png)

#### 如何使用 GNOME Shell

CentOS 7 的默认 GNOME 桌面以经典模式启动，但是如果你想使用 GNOME Shell，可以这样设置:

选项 A:如果你用 **startx** 启动 GNOME，就这样设置:

![UGFEuBeOA5TeYZZlPE-E2iOZBJmXvjjrfFYn](img/5878969d9b23835d4de889500c8288cc.png)

选项 B:设置系统图形登录 system CTL set-default graphical . target 并重新引导系统。系统启动后:

1.  单击位于“登录”按钮旁边的按钮。
2.  在列表中选择“GNOME”。(默认为 GNOME Classic)
3.  点击“登录”并用 GNOME Shell 登录。

![N2icRiFgBxnYEXUUu8cHdfVTGPo-yGzHhlAU](img/ddb1a97d27aa317c15eb9229724b0184.png)

GNOME shell 是这样开始的:

![oiqDsqZuANvGUG2XEpJU8M9LJbaEhfBTcrAV](img/401e08ed8bec24a309c83866dae9f643.png)

#### 安装 KDE 桌面:

通过输入以下命令安装 KDE 桌面环境

![8EKvkfyxykrD7ESdx-NTZqLn1-XoAHUxqB0t](img/15960b05cc8febcf4c989d6d4d397277.png)

安装完成后，输入如下命令:

![BM7BB8s7TzICeeeYJyhw9H2XeBU5BmXcEAfb](img/fdb5170cdc468fe3fad9ca0ee03ddc59.png)

KDE 桌面环境是这样开始的:

![vLim8XR1-vl7DP2Nzbk7GPe34X7PNLjx4KcR](img/934de6f7e530ce1ade3b75c3dae8363a.png)

#### 安装 MATE 桌面环境:

通过输入以下命令安装 MATE 桌面环境:

![KQGWjK2f5QL-ESkHT71foneUyytOZgC76THw](img/f08edf771707e471975ead3bbc4d3b72.png)

安装完成后，输入如下命令:

![S-9RT00AC-D0EyVaqdsQMYbH9G1CMKf5imSZ](img/673163ec944a910598a314bf5ac6f117.png)

MATE 桌面环境启动:

![6bLYOa7ktDIkJ5-nJ7BK9mQifjizzOZK47ir](img/f43903fba271fb82fec064c167039dc1.png)

#### 安装 Xfce 桌面环境:

通过输入以下命令安装 Xfce 桌面环境:

![eajRi-cVLawzVkihC8nTDZNGXKjdEISEnn6Y](img/bf3a96026ea972356e4fdf6bd9af179d.png)

安装完成后，输入如下命令:

![nvqLmB2PqwYamPW3KOfIRf4mIKvohyGzwbgv](img/4ac65e49a4613d3a80b5863066b01c79.png)

Xfce 桌面环境是这样启动的:

![3916tM9oz0pGpYNGX5H1EfjvA93F4aSQ33qf](img/50042c3cb890fbcec578dcaa3d3d9264.png)

#### 另一种方法是:

与其利用将 startx 命令插入到. xinitrc 文件中，不如告诉 Systemd 您希望引导到图形 GUI 而不是终端。

要做到这一点，只需执行以下操作:

![clCHZwiwW12IxLg8F-1eV8MMpwww250wx8Fn](img/991305a850f2d3a7d81b9f9f904d9a32.png)

然后简单地重启。

最后一位将把 runlevel 5 目标作为 Systemd 的默认目标。

#### 用系统来做

您也可以使用 Systemd 来实现这一点。这可能是更好的方法，因为您是通过 Systemd 及其 CLI 直接管理系统状态的。

您可以看到您当前的默认目标是什么:

![GAMQwC7zZTIyvWWjnOwCxOBII0f9ZJFo6NpV](img/24c5dfffd53104bf35a1c2494d9061ae.png)

然后将其更改为图形:

![MRK-LDaeKfDAPHrq1UiP3aG6USYtZYMp0c8Z](img/47a2b97803be3f02965b813129efe458.png)![Q3WkBqFK0D7pAQCU0Kzure2BqDw5cSoA24tC](img/c27261d6a087099a9d413b6a5a884910.png)

#### 目标

在 Systemd 中，目标 runlevel5.target 和 graphical.target 是相同的。runlevel2.target 和 multi-user.target 也是如此。

![04fnoUaIaChUTVk1gI5bpdWueeHe9j6Z4AZx](img/3ebd8b9263d73380375d745b5c7f2cfd.png)

#### RHEL / CentOS Linux 安装核心开发工具 Automake、Gcc (C/C++)、Perl、Python 和调试器

问:在 shell 提示符下安装 CentOS 或 RHEL 或 Fedora Linux 之后，如何安装所有开发人员工具，如 GNU GCC C/C++编译器、make 等？

您需要在 RHEL/CentOS/Fedora/Scientific/Red Hat Enterprise Linux 上安装“开发工具”组。这些工具包括核心开发工具，如 automake、gcc、perl、python 和调试器，它们是编译软件和构建新 rpm 所必需的:

1.  弯曲
2.  gcc c/c++编译器
3.  red hat-rpm-配置
4.  失去了
5.  rpm-构建
6.  制造
7.  pkgconfig
8.  gettext
9.  自动制造
10.  史具特 64
11.  基因组数据库
12.  野牛
13.  libtool
14.  autoconf
15.  gcc-c++编译器
16.  binutils 和所有依赖项。

#### 安装:

打开终端或通过 ssh 会话登录，并以 root 用户身份键入以下命令:

![FBV3oXITAW4NDKmUlCSnqKRHkrpFxOsywEOa](img/81a2ccb850c7093e75fc47e984d3b519.png)

以下输出示例:

![eJlL3KG2asfbT7-KjJODLskpBeLTsgNbcCRt](img/1fb49a641a612c878aadc4220afde705.png)

现在，您可以在您的系统上编译和使用任何应用程序。

#### **安装验证**

要显示 Gnu gcc/c/c++编译器版本类型:

![gCh9plzh1fFHqRAxIN41R2Z024dgm-3uUICK](img/836bef9c9ac4fe9ba0d8fb421127ffc7.png)

样本输出:

![f3COT3u-Xb8zvgnrNGVvH-7rpiPqZenBdivq](img/a68681acae6101ac18fd035a9b06c947.png)

#### 如何列出 Fedora / RHEL / CentOS Linux 服务器中当前运行的所有服务？

有各种方法和工具可以找到并列出 Fedora / RHEL / CentOS Linux 系统下所有正在运行的服务。

![sR1j7IEftLUJcf3l8eA09lFf4QuW31Na3dbE](img/1cd635983c41d624aa947c8c5e77a80d.png)

对于 CentOS/RHEL 6.x 及更早版本(systemd 之前),语法如下:

![34duN-gDwYU8Voox68o0OWpyO0lcROokMeN6](img/6c8cdeb7ec1193283c73cee18bfef5e7.png)

打印任何服务的状态。要打印 apache (httpd)服务的状态:

![hrg5zAjTRW789CRAM0kLPpoEdJi4VCqdf2re](img/0ca62c00ae912fbce92799d7d36416ca.png)

列出所有已知服务(通过 SysV 配置):

![zdMHiLpBgWw-7wcHU4rtNePnTWydgPGwNVSY](img/a6b0f2d8cafc7e1c8555d441571e584d.png)

列出服务及其开放的端口:

![ONlXfdtpMuQThom-1TXCwogvZLW-CxvJsZSu](img/3a99a342026dcf231a7d58140bd2b605.png)

打开/关闭服务:

![9zwxQTHcBo1W4-bYkTdLRYkRqlxbgiwH30Fo](img/498a3ac3c4f32c0eae0e084dc7b09cf9.png)

**ntsysv** 是配置运行级服务的简单接口，也可以通过 **chkconfig** 进行配置。默认情况下，它配置当前运行级别。只需输入 **ntsysv** 并选择你想要运行的服务。

#### 关于使用 systemd 的 RHEL/CentOS 7.x 的说明

如果您使用的是基于 systemd 的发行版，如 Fedora Linux v22/23/24 或 RHEL/CentOS Linux 7.x+,请尝试使用以下命令列出使用 systemctl 命令运行的服务。它控制 systemd 系统和服务管理器。

要在 CentOS/RHEL 7.x+上列出 systemd 服务，请使用以下命令。

语法是:

![HMgfPY4TiCQZt5VqkAeVnOA6wzKA0tHb7fnM](img/65eefeef4a28c0789cb758cd5050bd94.png)

要列出所有服务:

![4X-Q7tJMtViVrTXOg1118N4RQSxK8fIIsG8q](img/fb7d82e6ef81f636ca13cbf250f5e0d4.png)

样本输出:

![8HHdKMxQvr1tP9gfoT3K602qrD3FkQ-RpAU2](img/8534ab702f2dde23e36ed950ede52e1e.png)

上图显示了安装在基于 CentOS /RHEL 7 systemd 的系统上的所有设备及其当前状态。

要查看与特定服务(cgroup)相关联的进程，可以使用 systemd-cgtop 命令。与 top 命令一样，systemd-cgtop 根据服务列出正在运行的进程:

![cnQkMU558eEjH4IQ7NY546voU0Q079Aj5iOp](img/a1e36196a9a1ce92f4ca76ba56085303.png)

样本输出:

![vOiJ6B9pCEt3sT5CrOGbkiIGf5UoSdQ7xv42](img/eeea8651958b5efe30c3b7410ec55cc6.png)

要仅在 CentOS/RHEL 7.x+上列出 SysV 服务，请使用(不包括本机 systd 服务):

![C6mSxcdwt6RdEjfLwbP0XmdjJBv7UrKEjr33](img/d8cf239ac9f8532b43d31e12ec3c8303.png)

样本输出:

![psYt44SaI963eoYVsBCFSCEFGyERv4H3V2nC](img/c0a8555d05aa87fa0ccd8b4e34dc650a.png)

#### 防火墙如何:

点击了解如何设置防火墙[。](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-oncentos-7)

**参考文献**

*   [CentOS 文档](https://wiki.centos.org/Documentation)
*   [CentOS 发行说明](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7)
*   [在 CentOS 7 / RHEL 7 上安装 Gnome GUI](https://linuxconfig.org/how-to-install-gui-gnome-on-centos-7-linux-system)
*   [使用 SYSTEMD 目标](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-managing_services_with_systemd-targets)

**文档如何指导 CentOS**

[CentOS 第 7 版](https://docs.centos.org/en-US/docs/)

CentOS 7 完全基于 RedHat 的详细文档。示例和系统管理指南位于此处: [CentOS 7 完整文档](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/)

[*最初由 Krasimir Vatchinsky 发表于存档的堆栈溢出文档— RIP 教程*](https://riptutorial.com/centos/topic/7640/getting-started-with-centos)