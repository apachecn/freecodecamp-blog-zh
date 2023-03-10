# 事件查看器–如何访问 Windows 10 活动日志

> 原文：<https://www.freecodecamp.org/news/event-viewer-how-to-access-the-windows-10-activity-log/>

Windows 10 事件查看器是一个显示日志的应用程序，该日志详细记录了计算机上重大事件的信息。这些信息包括自动下载的更新、错误和警告。

在本文中，您将了解什么是事件查看器，它有不同的日志，最重要的是，如何在 Windows 10 计算机上访问它。

## 什么是事件查看器？

您在 Windows 10 电脑上打开的每个程序都会向事件查看器中的特定活动日志发送通知。

所有其他活动(如操作系统更改、安全更新、驱动程序异常、硬件故障等)也会记录到特定的日志中。因此，您可以将事件查看器视为记录计算机上每一项活动的数据库。

使用事件查看器，您可以解决不同的窗口和应用程序问题。

如果您深入研究事件查看器，您会看到不同的信息、警告和大量错误。不要惊慌，这很正常。即使是维护得最好的计算机也会显示大量的错误和警告。

## 如何访问 Windows 10 活动日志

在 Windows 10 上，有三种主要方式可以访问事件查看器——通过开始菜单、运行对话框和命令行。

### 如何通过开始菜单访问 Windows 10 活动日志

**步骤 1** :点击开始或按键盘上的 WIN (Windows)键
**步骤 2** :搜索“事件查看器”
**步骤 3** :点击第一个搜索结果或按`ENTER`
![ss-1-5](img/df5cce6a61a6549703139032bfa43d47.png)

你会看到这个页面:
![ss-2-1](img/2abe6f1ac7bed1b6f507fa131fc29151.png)

### 如何通过运行对话框访问 Windows 10 活动日志

**第一步**:右击开始(Windows log)选择“运行”，或者在键盘上按`WIN` (Windows 键)+`R`
![ss-3-4](img/24bcc4342cba6c68ff45a746865c8dba.png)

**第二步**:在编辑器中键入“eventvwr”，点击“确定”或点击`ENTER`
![ss-4-5](img/12ff0d2694ba20ad13fbb2cf25f8146c.png)
![ss-5-5](img/7b71436de256f248bfbd81451e87c72f.png)

### 如何通过命令提示符访问 Windows 10 活动日志

**第一步**:点击开始(Windows logo)，搜索“cmd”
**第二步**:回车或点击第一个搜索结果(应该是命令提示符)启动命令提示符
![ss-6-3](img/339f3d8ca1053d5e04f9099d9dba2258.png)

**第三步**:输入“eventvwr”，点击`ENTER`
![ss-7-2](img/dc6ee55317c42e714fef9fba8c438f0b.png)
![ss-8-2](img/062d62030a5f0e65af41859d04514476.png)

## 事件查看器活动日志

当您打开事件查看器查看计算机的活动日志时，会自动向您显示事件查看器(本地)选项卡。但是这可能不包含您需要的详细信息，因为它只是一个当您打开事件查看器时看到的页面。

事件查看器的功能远不止这些。

### 管理事件日志

您可以展开自定义视图选项卡来查看您计算机的管理事件，如下:
![ss-9](img/1c3ef91051370bd82c4d75c92beaab0c.png)

### Windows 活动日志

您还可以展开 Windows 日志以显示各种活动，例如:

*   应用事件:程序活动
    ![ss-10](img/a3f48a1c32fd67ba369ca6512a366bed.png)的信息、错误和警告报告

*   安全事件:这显示了各种安全操作的结果。它们被称为审计，每个审计都可能是成功或失败的
    ![ss-11](img/36ddbfea401caf1ace4bb6c0b80c5685.png)

*   Setup 事件:这与域控制器有关，域控制器是在计算机网络上验证用户的服务器。你不应该每天都担心他们。
    ![ss-12](img/5a6b06265c164a0f1f3b9c842ca22c36.png)

*   系统事件:这些是来自系统文件的报告，详细描述了它们遇到的错误
    ![ss-13-1](img/cbb1ddf593036e09fe07a2dd4d5c2584.png)

*   转发的事件:这些事件从同一网络中的其他计算机发送到您的计算机。它们帮助您跟踪同一网络中其他计算机的事件日志。
    ![ss-14-1](img/9ac2cb0ec8dd3f37658824dc469dfd24.png)

此外，还有应用程序和服务日志，显示硬件和 Internet Explorer 活动，以及 Microsoft Office apps 活动。

您可以双击某个错误来检查其属性，并在线查找该错误的事件 ID。这可以帮助您发现有关错误的更多信息，以便您可以在需要时修复它。
![ss-15](img/b26465ae9ecd36d739eb02b92da40b79.png)

## 结论

在本文中，您了解了 Windows 10 事件查看器，这是一个非常强大的工具，Windows 用户应该知道如何使用。

除了查看各种活动日志，它还可以帮助您了解计算机上发生了什么。

感谢您的阅读。如果你认为这篇文章有帮助，请与你的朋友和家人分享。