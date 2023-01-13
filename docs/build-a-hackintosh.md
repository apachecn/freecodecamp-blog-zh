# 如何构建一个 Hackintosh——使用 OpenCore 在 PC 上安装 MacOS Big Sur

> 原文：<https://www.freecodecamp.org/news/build-a-hackintosh/>

Hackintosh 是一种非 Mac 电脑系统，由 PC 部件制成，运行 macOS 操作系统。在本教程中，您将学习如何创建一个 Hackintosh。

您将学习如何使用 OpenCore 安装 macOS Big Sur(或任何其他版本的 macOS)。

与正式的 Macintosh 电脑相比，Hackintosh 电脑的主要优势在于它便宜得多。我制作了一台符合 Mac Pro 规格的 Hackintosh 电脑，价格大约是 Mac Pro 的 1/3。有些人已经能够花不到 100 美元就创造出一个黑客电脑。

本教程将主要关注如何在你的硬件上安装 macOS。我还创建了一个视频版本，展示如何构建一台完整的机器，然后在其上安装 macOS。

该视频展示了如何使用装有 macOS 的电脑为您的 Hackintosh 创建 macOS 安装程序。在本书面教程中，您将学习如何使用 macOS 或 Windows 创建 macOS 安装程序。

你可以在这里观看视频:

[https://www.youtube.com/embed/Gaosub7FRf4?feature=oembed](https://www.youtube.com/embed/Gaosub7FRf4?feature=oembed)

创建供个人使用的 Hackintosh 并不违法，但它确实违反了苹果的最终用户许可协议。所以不要打算把这个拿到苹果店去维修。在许多地方，出售黑客电脑是违法的。

## 硬件

许多计算机零件为黑客服务。但有些人没有。查看这个网站,看看什么硬件可以兼容一台 Hackintosh。

在上面的视频中，我一步一步地演示了如何构建一台可以作为黑客的电脑。在视频描述是我使用的具体部分的列表。

如果你想安全，你可以使用我在我的构建中使用的精确部件，但是在各种各样的硬件上安装 macOS 是可能的。

## 下载 MacOS 并创建可引导的 USB 安装程序

对于这一步，您需要一个至少 16GB 的 USB 驱动器。根据您是使用 macOS 还是 Windows 设置可引导 USB 安装程序，该过程会有所不同。这个过程在 macOS 上要简单得多，但在 Windows 上仍然可行。

如果可以，找一台 Mac 机器来创建可引导 USB 安装程序。但是我将介绍 macOS 和 Windows 的步骤。

### 使用 MacOS 创建 MacOS 安装程序

在这个过程中，你会用到一些程序，所以从下载它们开始吧。以下是您需要的链接，后面是下载说明。

*   [ProperTree](https://github.com/corpnewt/ProperTree) -点击“代码”按钮，然后“下载 Zip”
*   [MountEFI](https://github.com/corpnewt/MountEFI) -点击“代码”按钮，然后“下载 Zip”
*   [OC_GEN-X](https://github.com/Pavo-IM/OC-Gen-X/releases) -下载最新版本的 zip 文件。

在 macOS 上打开 App store。搜索“大苏尔”。点击“获取”，然后点击“下载”。

![image-69](img/3e012fafa6ba33cae30eac92da9f3cc5.png)

使用“磁盘工具”格式化您的 USB 驱动器。若要进入“磁盘工具”,只需点按放大镜并键入“磁盘工具”。

打开“磁盘工具”后，请确定视图已设定为显示所有设备。

![image-70](img/4178b9e10be25a0ce0ec8d8552962ac4.png)

单击 USB 驱动器，然后单击顶部菜单中的“擦除”。

将驱动器命名为“MyVolume”。确保格式为 Mac OS 扩展(日志式),并且方案为 GUID 分区图。然后点击“擦除”按钮。

![image-71](img/732d044d7913b520a8148e22c3fb3489.png)

准备好 USB 驱动器后，在 MacOS 中打开终端。您将在终端中使用一个命令，使 USB 驱动器成为 macOS 的可引导安装程序。

如果您正在安装 macOS Big Sur，请键入以下命令:

`sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

如果你正在安装一个不同版本的 macOS，你可以在这里找到[你正在安装的版本的命令](https://support.apple.com/en-us/HT201372)。

你需要等一会儿才能安装。完成这些后，打开您之前下载的 OC_Gen-X 程序。

要打开该程序，您必须右键单击图标，选择“打开”，然后再次选择“打开”。

这是一个软件向导，可以帮助我们轻松地准备在特定的硬件设置上安装 MacOS 所需的东西。它将获取除 SSDTs 之外我们需要的所有内容，并将其放入一个文件夹中。

您也可以通过遵循 [OpenCore 安装指南](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#downloading-macos-modern-os)这是一种更加手动的方法。但是这个程序为我们简化了很多事情，而且它不适用于 Windows。

![image-72](img/ea1e3dd1eec00985928aea223d3b1013.png)

在“系统类型”下的第一个屏幕上，选择您拥有的处理器类型。查看处理器文档，确定它使用的微体系结构的名称。做好这一点非常重要。

我用的处理器类型是“咖啡湖”。

对于该程序中的大多数选项卡，您可以保留默认设置。

在“图形”下选择“WhateverGreen”，在“音频”下选择“AppleALC”。在“以太网”下，选择“IntelMausi”。这些是非常常用的选项，但根据您的硬件和具体使用情况，您的设置可能会有所不同。

![image-73](img/0de00143ad91e21a63c0db29a4edbf03.png)

SMBIOS 很重要，您必须在该选项卡上指定正确的系统型号。对于我的设置，我使用的是“iMac19，1 ”,但如果您使用的是不同操作系统版本的不同处理器，它可能会有所不同。

要确定使用什么系统型号，请访问 [Open Core 安装指南](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html)。

为您的处理器类型选择左侧的部分(在我的例子中是“Coffee Lake”)。然后找到标题“PlatformInfo”。向下滚动一点，您将看到一个包含要使用的 SMBIOS 的表格。

![image-74](img/ffa7ffaa6212203ca584ebe2dc5a7313.png)

选择适当的系统型号后，单击底部的“生成 EFI”按钮。

![image-76](img/8386ea5f66626e4989cd91d802ade631.png)

现在，您的桌面上已经创建了一个 EFI 文件夹。我们现在将对内容进行一些修改。

你需要拿到 SSDT 的文件。这取决于您的处理器。

你可以在这个链接找到你[需要的 SSDT 的清单。只需选择您的处理器类型并下载每个所需的固态硬盘。](https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-prebuilt.html)

以下是我的咖啡湖系统所需的 SSDTs 的链接。

*   [SSDT-塞-德尼亚](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml)
*   [SSDT-EC-USBX 桌上型电脑](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
*   SSDT 预警飞机
*   [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PMC.aml)

一旦所有这些文件下载完毕，将它们移动到您的 EFI 文件夹中。它们应该被移动到这个子文件夹:`EFI/ACPI/OC`

现在，您将使用之前下载的 MountEFI 程序在 USB 驱动器上安装隐藏的 EFI 分区。

右键点击`MountEFI.command`，点击“打开”，然后再次“打开”。

选择您的 USB 驱动器。它应该有一个类似“安装 macOS 大苏尔”的名字，因为我们把它变成了 MacOS 的可引导安装程序。在下面的截图中，是选项 2。

![image-77](img/d35bf8379731024a7d7e1bf08fc3fa5c.png)

现在您已经有了一个挂载的 EFI 分区和一个来自 OC Gen-X 的 EFI 文件夹。

打开你之前下载的`ProperTree.command`。像以前一样，您可以通过右键单击并选择“打开”来打开它。

一旦 ProperTree 正在运行，转到“文件->打开”。选择 EFI 分区，然后选择“OC”文件夹，然后打开“config.plist”文件。

我们需要做的第一件事是将 EFI 文件夹中的所有文件注入到“config.plist”文件中。

所以转到“文件”，然后选择“OC 快照”。确保您在 EFI 分区上。转到“EFI”文件夹，然后是“OC”文件夹。并点击“选择”按钮。

这里可能会弹出一个对话框，提示使用哪个版本。如果出现这种情况，请单击“是”。

现在回到“文件”，然后选择“OC 清理快照”，并选择“选择”。

OC Gen-X 程序帮助简化了所有必需的设置。此时，您应该根据官方安装指南验证所有设置是否正确。

这是咖啡湖的指南。如果您使用不同类型的处理器，只需在左侧菜单中选择您的类型。

您可以验证在`config.plist`文件中是否正确设置了怪圈。应该都是对的。

![image-78](img/de286580f29ee828936023fee65d5240.png)

您需要在 config.plist 文件中进行额外的设置，以确保板载显卡正常工作。找到“DeviceProperties”部分，然后复制以下要添加的字符。

`PciRoot(0x0)/Pci(0x2,0x0)`

请注意，如果您的处理器不是 Coffee Lake，您需要在“设备属性”下添加的确切内容可能会有所不同。根据您的处理器类型，在 OpenCore 指南中搜索“设备属性”,以确认在`config.plist`文件的“设备属性”下添加什么。

在“设备属性”下，单击“添加”。然后右击并选择“在‘添加’(+)下新建孩子”。

![image-79](img/3d2b344dd5c4f9a339d8c81c47d87e18.png)![image-80](img/ae9e231ad5869773dab5b012da792a3f.png)

双击显示“新字符串”的地方，将文本粘贴到字段中，然后按回车键。然后在下一列中选择“字符串”,并确保将其设置为“字典”。

![image-81](img/7c6ee908c37c7fe7c87f7e13f416a8a5.png)

接下来，我们需要在它下面添加更多的孩子，它最终应该是这样的:

![image-82](img/7e04dad13b9b169fffa811da7a2bb29f.png)

下面是您需要添加的上图中的文本(如果您的系统是 Cofee Lake)。

| 名字 | 类型 | 价值 |
| --- | --- | --- |
| AAPL，ig-平台-id | 数据 | 07009B3E |
| 帧缓冲区修补启用 | 数据 | 01000000 |
| 帧缓冲区-stolenmem | 数据 | 00003001 |

现在，在`config.plist`文件中，找到 NVRAM 部分。

更新“boot-args”，使文本为“-v keepsyms=1 debug=0x100 alcid=1”。

![image-83](img/369019889b921314205b54c0f087d0d4.png)

现在我们将把语言改为英语。因此，在“prev-lang:kbd”旁边，将“data”更改为“String ”,并将值设置为“en-US:0 ”,然后按 enter 键。

![image-84](img/cd5ca4919642608fa1d0fa833fbeeadf.png)

如果您想要不同的语言，只需进入[此链接](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)查找要使用的语言代码。

config.plist 文件现在完成了。所以去“文件”，然后“保存”。现在，您已经完全完成了可引导驱动器的设置。所以只要把硬盘弹出来，然后你就可以把它插到你的电脑上了。

跳过下一个 Windows 部分，转到标题“BIOS 设置”。

### 使用 Windows 创建 macOS 安装程序

在 Windows 上创建 macOS 安装程序的第一步是下载 [OpenCore](https://github.com/acidanthera/opencorepkg/releases) 。确保下载最新版本的 zip 文件。

解压 OpenCore，然后转到`/Utilities/macrecovery/`。接下来复制`macrecovery`文件夹的路径:

![image-10](img/9e3cf60954129122bebe3ad1bbb8a2c0.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

打开一个命令提示符，使用命令`cd [PASTE_FOLDER_NAME]`将目录切换到您刚刚复制的`macrecovery`文件夹。

它应该是这样的:

![image-12](img/823655db83555f9d5feb05c7ee968bd6.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

现在，在命令提示符下，根据您想要的 macOS 版本运行以下命令之一。如果你还没有 Python，你就必须先[安装它](https://www.python.org/downloads/)。

```
# Mojave(10.14)
python macrecovery.py -b Mac-7BA5B2DFE22DDD8C -m 00000000000KXPG00 download

# Catalina(10.15)
python macrecovery.py -b Mac-00BE6ED71E35EB86 -m 00000000000000000 download

# Big Sur(11)
python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download
```

这将需要一些时间，但是一旦完成，您应该会得到基本系统或恢复映像文件:

![image-13](img/2903820e7b897fb92515f8a3239918ff.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

现在打开磁盘管理，将 USB 驱动器格式化为 FAT32。遵循 [OpenCore 文档](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)中的以下步骤:

1.  右键单击任务栏上的开始按钮，然后选择磁盘管理。
2.  您应该会看到所有的分区和磁盘。在下半部分，你会看到你的设备。找到你的 USB。
3.  你需要格式化 USB，使其有一个 FAT32 分区。
4.  如果 USB 上有多个分区，右键单击每个分区，然后单击删除 USB 的卷。
5.  右键单击未分配的空间并创建一个新的简单卷。确保它是 FAT32，并且至少有一两千兆字节大。命名为“EFI”。
6.  否则，右键单击 USB 上的分区，然后单击 Format 并将其设置为 FAT32。

![DiskManagement.aac12f25](img/916ec955407c5870fb262cea6fd11875.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

接下来，转到这个 USB 驱动器的根目录，创建一个名为`com.apple.recovery.boot`的文件夹。然后移动下载的 BaseSystem 或 RecoveryImage 文件。请确保您复制了。dmg 和。将文件分块列表到此文件夹:

![com-recovery.805dc41f](img/785cdee17b4165bda8833694c8a756de.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

现在抓取你之前下载的 OpenCorePkg 并打开它:

![base-oc-folder.9a1a058a](img/d49dc05056e4765ed67d455254cd4773.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

这里我们看到 IA32(32 位 CPU)和 X64(64 位 CPU)文件夹，选择最适合您的硬件的一个并打开它。接下来，抓取 EFI 文件夹，并将其放在 USB 驱动器的根目录下，沿着 com.apple.recovery.boot。一旦完成，它应该看起来像这样:

![com-efi-done.a6fb730e](img/5dce9900ecd057c02c72c5102570f0e7.png)

Source: [OpenCore Docs](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)

此时，您必须继续设置您的 EFI 文件夹。由于这一步的复杂性以及取决于您的设置的所有不同的可能选项，您应该遵循官方文档来完成接下来的几个步骤。

以下是使用 Windows 创建可引导 USB 安装程序时的后续步骤说明的链接。请注意，文档中的屏幕截图显示的是 mac，但这些步骤也适用于 Windows。对于使用 mac 的设置，您不必经历这些步骤，因为有一个向导可以自动完成所有这些步骤。

[添加基础 OpenCore 文件](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/opencore-efi.html)

[收集文件](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#firmware-drivers)

[ACPI 入门](https://dortania.github.io/Getting-Started-With-ACPI/)

## BIOS 设置

我将向您展示我如何为我的 Hackintosh 设置 BIOS。BIOS 软件是我的主板专用的，你的可能看起来有点不同。如果你的看起来不同，你最好在你的软件中找到相同的设置。注意，BIOS 设置很容易试验，你不需要和我一样的设置就能让一切正常工作。

启动计算机，然后按“删除”键进入 BIOS。

大多数设置可以保留为默认设置。以下是应该更新的设置:

![image-14](img/c87b81cd10c15a74c72afbb676f98a69.png)

在高级下，应启用“4G 以上解码”。

![image-15](img/5c16b0f7fa983aa1943fae3400a7f382.png)

在“高级”下，然后在“串行端口配置”下，将“串行端口”关闭。

![image-16](img/81fe45ba2bbf52b241db51a813745e58.png)

在高级下，然后在 USB 配置下，将“XHCI 切换”设置为启用。

![image-17](img/eb6e788670b450d0ce7d462056324c4e.png)

在“启动”下，然后在“启动配置”下，将“快速启动”设置为“禁用”

![image-18](img/2ae389d9e1665783dddc102191cdfe5f.png)

在“启动”下，然后在“安全启动”下，将“操作系统类型”设置为 Windows UEFI 模式。

![image-19](img/58911300568d61b6db5ffe532d0fefad.png)

你需要做的最后一件事是进入引导>安全引导>密钥管理。然后选择“清除安全启动密钥”。

现在，请退出并选择“保存更改并重置”。

## MacOS 设置

计算机重新启动后，按 F12 进入启动菜单。选取“安装 MacOS Big Sur(外部)”。

> 接下来的几张截图有点模糊，因为我在拍摄我的显示器，没有正确对焦。很抱歉。

![image-21](img/d84fd79d07a94ca57ba7dcaf7d88c416.png)

加载后，macOS 实用程序屏幕应该会出现。选择“磁盘工具”。

![image-22](img/cdfea28c19043801860c59382720b2cd.png)

单击顶部的下拉菜单，然后单击“显示所有设备”。然后选择你的硬盘，点击顶部的“擦除”。

![image-23](img/2df1146e6e89835ee230958db7641f59.png)

你可以给这个驱动器起任何你喜欢的名字。对于格式，请确保选择“Mac OS 扩展(日志式)”，对于方案，请选择“GUID 分区图”

抹掉驱动器后，关闭“磁盘工具”并选择“安装 MacOS Big Sur”。您必须选择刚刚格式化的硬盘。然后你将不得不等待 macOS 被安装。

计算机应该重新启动，回到启动菜单。选择“MacOS 安装程序”。

此时，您将像安装一台全新的 Mac 电脑一样安装电脑。设置完成后，将加载 macOS Big Sur。

还有一件事要做。你必须把 u 盘上隐藏的 EFI 分区的 EFI 文件夹复制到你安装 macOS 的硬盘上的 EFI 分区。

在新的 Hackintosh 上，进入网络浏览器并[下载 MountEFI](https://github.com/corpnewt/MountEFI) 。如果您在 mac 上创建了安装程序，这与您之前使用的程序相同。点击链接后，点击“代码”按钮，然后“下载 ZIP”。

进入下载文件夹，右击`MountEFI.command`并打开它。

使用该程序从 Hackintosh 的硬盘和名为“Install MacOS Big Sur”的 USB 驱动器上挂载 EFI 分区。首先选择一个，然后选择另一个。

在两个分区都被挂载后，您需要将 EFI 文件夹从 USB EFI 分区复制到硬盘 EFI 分区。

![image-24](img/14e7b62e070c6fabb019581039636c45.png)

此时，您可以重启计算机并取出 USB 驱动器。黑客帝国已经完成了！