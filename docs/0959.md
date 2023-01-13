# 如何从 PowerShell 中删除开始文本

> 原文：<https://www.freecodecamp.org/news/how-to-remove-starting-text-from-powershell/>

如果您使用的是 Windows 操作系统，您可能至少使用过一次最新的 Windows PowerShell。

每当您使用 Windows 终端打开 PowerShell 时，您都会在终端中收到一条文本消息，显示 PowerShell 版本、下载最新 PowerShell 的链接等等。

![Screenshot--3--1](img/a84b5f1b933b43293f8ddb23f6994f9c.png)

有时这可能很烦人，您可能想要删除该文本，这样该消息就不会再出现。有一种方法可以做到这一点，在本文中，我将向您展示如何从终端中一劳永逸地删除起始文本！✌️

首先，在 Windows 终端打开 PowerShell。您将照常获得起始文本。

![Screenshot--3--2](img/8684bb5692eaf2d5329fcc65cf599349.png)

单击下拉按钮以获得其下的菜单。

![Screenshot--4-](img/5f9bc09d55df77687a2de096453bb1bf.png)

转到**设置**。

![Screenshot--5-](img/aa314959fcbe0e609b5c025c6d673362.png)

你会得到如下界面:

![Screenshot--6-](img/6982b1b87d5c11bac8337c462f3e73d4.png)

点击**打开 JSON 文件**。

![Screenshot--7-](img/baecc8678d1f3210220c3f24f734071b.png)

JSON 填充将在文本编辑器中打开。对我来说，它是记事本——但对你来说，它可能是 VS 代码或任何你想要的文本编辑器。

![Screenshot--9-](img/ceb3a2299f50c4c78b5fca146caa76ae.png)

向下滚动，直到找到如下所示的 PowerShell 块。

![Screenshot--12-](img/b8f921eeea02cc795c622f880e222720.png)

如下图添加`"commandline": "pwsh.exe -nologo",`。

![Screenshot--14--1](img/691b14dfbbf9188c86061eb3e8c621dd.png)

对于 PowerShell 块，该命令应该如下所示:

![Screenshot--15-](img/805a9d428805de1b831f52c7e0c1cb46.png)

然后保存文件。您也可以使用快捷键`Ctrl` + `S`来完成此操作。

点击**保存**。

![Screenshot--16-](img/dcdd6ac0cc4842b9d534dd7aa76682fe.png)

关闭所有选项卡。

![Screenshot--17--1](img/68e2858847af3403938cbe7d1832b7ff.png)

重新打开终端，看看神奇吧！🪄

![Screenshot--19--1](img/77b47e97666b59ed012107e28c665b73.png)

## 结论

感谢您阅读整篇文章。如果对你有帮助，你还可以在 [freeCodeCamp](https://www.freecodecamp.org/news/author/fahimbinamin/) 查看我的其他文章。

如果你想和我联系，那么你可以使用 [Twitter](https://twitter.com/Fahim_FBA) ， [LinkedIn](https://www.linkedin.com/in/fahimfba/) ， [GitHub](https://github.com/FahimFBA) ，[英语 YouTube 频道](https://www.youtube.com/channel/UCG97GCUifMS2Vm28tgXQi0Q)，或者[孟加拉语 YouTube 频道](https://www.youtube.com/channel/UCEF4lxmpBKV2oYCSFH6ExIQ)。

💫如果你想查看我的精彩部分，那么你可以在我的 [Polywork 时间轴](https://www.polywork.com/fahimbinamin)上查看。

非常感谢！

横幅图像取自[故事集](https://storyset.com/worker)(故事集的工人插图)，并使用 Adobe Photoshop 进行了修改。