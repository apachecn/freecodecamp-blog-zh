# 如何在 Windows 上安装 C 和 C++编译器

> 原文：<https://www.freecodecamp.org/news/how-to-install-c-and-cpp-compiler-on-windows/>

如果你想在你的 Windows 操作系统中运行 C 或 C++程序，那么你需要有合适的编译器。

MinGW 编译器是一个众所周知的和广泛使用的软件，用于安装 C 和 C++编程语言的 GCC 和 G++编译器。

但是许多开发人员在安装编译器时会遇到困难，所以我将在本文中通过截图向您展示安装编译器的所有步骤，帮助您完成安装。

我将使用 Windows 11，但同样的过程适用于所有其他 Windows 操作系统，除非您使用 Windows XP(您需要更改 Windows XP 中的一些步骤)。

如果你也想看我做的关于这个话题的视频，这里是:

[https://www.youtube.com/embed/c7FjV8Gwk_M?feature=oembed](https://www.youtube.com/embed/c7FjV8Gwk_M?feature=oembed)

## 安装 MSYS2

首先，我们需要从 MSYS2 下载一个可执行文件。去 MSYS2 官网:[https://www.msys2.org/](https://www.msys2.org/)。截至今天，该网站如下所示。

![Screenshot--8-](img/f7ae4665220c40919799551ea37186a6.png)

向下滚动一点，直到找到可执行文件的下载按钮。

![Screenshot--9-](img/f115e506fc6067edc976971d2fe89771.png)

只需点击安装按钮，并保存安装文件在任何地方你想要的。

![Screenshot--10--1](img/02bc580b8e9cce7ccf477332c0aa0744.png)

完成下载可执行文件。根据你的网速，应该不会花太多时间。

![Screenshot--11-](img/1663335d944c34e51d84e6c4e9677e8f.png)

下载完文件后，我们会得到这个可执行文件。

![Screenshot--12-](img/cc87d267444e0e19eb2092868e1af140.png)

双击可执行文件。然后点击`Next`。

![Screenshot--13-](img/48c90258c7669d8e1bf329a778b5c17d.png)

保持名称不变，点击`Next`。

![Screenshot--14--1](img/ab6e90bd20a1af2585e030e34c01a344.png)

保持这一切不变，点击`Next`。

![Screenshot--15-](img/099d63dc199334897f9330e2732558af.png)

给它一些时间来完成安装过程。

![Screenshot--16-](img/82b913e2c2ca21b5771bb69a971218f7.png)

如果您保持复选标记，那么一旦您点击`Finish`，MSYS2 终端将打开。

![Screenshot--17-](img/cbd5899040858b5dda54db4932b53f1f.png)

我更喜欢这样做，但是如果你想以后再做剩下的任务，那么你需要自己从开始菜单打开终端。

在这种情况下，您必须点击开始按钮>搜索`MSYS2`，并点击终端，如下图所示:

![Screenshot--26-](img/94a6e206207329492289335f1ab3054f.png)

假设我们已经成功打开了 **MSYS2 MSYS** 终端。

应用命令`pacman -Syu`更新包数据库和基础包。

![Screenshot--19-](img/b078c4a84f1d2809c8bd2e75304bf1e8.png)

如果出现这种类型的安装提示，请键入`Y`并按 enter 键。

![Screenshot--20-](img/2eec59299960019f0f3eff595d02b846.png)![Screenshot--21-](img/e5bd080c46f26344185f2166c5850ef9.png)![Screenshot--22-](img/d0379f01c43ee78162085a452cef153e.png)

键入`Y`并按回车键。

![Screenshot--23-](img/5dc2d92b77daac6ed65fe38eba1b7cea.png)![Screenshot--24-](img/8703c2dbab0a9dc168c6e535cdfe212c.png)

航站楼将关闭。我们必须手动打开终端并更新其余的包。

单击开始按钮。

![Screenshot--25-](img/167707a0f589f08150ec865699c3aec9.png)

搜索名为 **MSYS2 64bit** 的文件夹。点击文件夹展开，得到终端。点击 **MSYS2 MSYS** 打开终端。

![Screenshot--26--1](img/a17461c51d169c640e0b0825a08160c2.png)

通过应用命令`pacman -Su`更新其余的包。

![Screenshot--27-](img/94c5c7a2545850867e0a2545829c74ab.png)

如果您得到任何安装提示，那么您需要键入`Y`或`y`并按回车键。

![Screenshot--28-](img/292bcfcab8b0b909467068d1b06d6da3.png)![Screenshot--29-](img/6bf0098fb22b620e1e7cf9ab41f43dc5.png)

稍等片刻，完成安装。

![Screenshot--30-](img/4ee700bd78d5cb96f3cb0dd36bf5eb52.png)![Screenshot--31-](img/4aeb4a892448c9027222de339534eaa0.png)

完成安装后关闭窗口。

## 安装 G++和 G++编译器

单击开始按钮。找到 **MSYS2 64bit** 文件夹。单击该文件夹将其展开。

![Screenshot--32-](img/8b449f9e6046eb90845b61ac3094ae0a.png)

如果你像我一样使用的是 **64 位**操作系统，那么我们需要使用 **MSYS2 MinGW x64** 终端。点击终端打开它。

![Screenshot--33-](img/c9dcb5df032684aa0727dd3d88ba118c.png)

⚠️但是，如果你使用的是 **32 位**操作系统，那么你必须使用 **MSYS2 MinGW x86** 终端。然后，你需要打开终端。

![Screenshot--34-](img/f8a21d165a2935e802e061dab4a240f4.png)

由于我使用的是 **64 位**操作系统，所以我打开了 64 位的终端。应用命令`pacman -S mingw-w64-x86_64-gcc`安装编译器。

⚠️:如果你使用的是 32 位操作系统，那么你必须在你的 32 位终端中使用命令。

![Screenshot--35-](img/b0148e692160097b15e9b7ab1e527963.png)

等一会儿。

![Screenshot--36-](img/c5684046a2c589d00a08d9d1c5ba55ae.png)

如果出现安装提示，键入`Y`或`y`并按 enter 键。

![Screenshot--37-](img/05abfb3f7c04d568b0a9c59cda771288.png)![Screenshot--38-](img/b0d3ef6dfa3ff1389b2722b2c5315271.png)

给它一些时间来完成安装过程。

![Screenshot--39-](img/c24f7cf83484809ac78e42ba72b42653.png)![Screenshot--39--1](img/e009f7412e6671b733de0d1362a67113.png)

现在，您已经完成了编译器的安装。

## 如何安装调试器

如果你像我一样使用的是 **64 位**操作系统，那么你必须使用`pacman -S mingw-w64-x86_64-gdb`命令。

⚠️:如果你使用的是 32 位操作系统，那么你必须在你的 32 位终端中使用命令。

![Screenshot--41-](img/e133c8dae1951c330c7602759bd6a5c0.png)

如果出现任何安装提示，只需键入`Y`或`y`并按回车键。

![Screenshot--42-](img/1ea138135e374d92bb17e117ae269c84.png)![Screenshot--38--1](img/739a451a750d35a61fc5f88e25ab4a2e.png)

给它一些时间来完成安装。

![Screenshot--44-](img/37f620b0dd84f61bd209175da846b9a2.png)![Screenshot--45-](img/5290427cf5938fc753cfb7e34a5d5cd8.png)

你可以关闭终端。

## 如何将目录添加到环境变量的路径中

打开文件浏览器。

![Screenshot--46-](img/c7d5e3c56d428d8b06a7bc6c71c67f98.png)

我假设您已经像我一样将 MSYS 安装到了默认目录中。如果您使用自定义目录，那么您需要转到您安装它的目录。

![Screenshot--47-](img/513d19679849f2dd1dfbc64d242927c3.png)

如果你像我一样使用 64 位操作系统，那么进入 **mingw64** 文件夹。

⚠️如果你使用的是 32 位操作系统，那么进入 **mingw32** 文件夹。

![Screenshot--48-](img/e3e8e94c234004ebe259470e1006cc48.png)

我们现在必须去二进制文件夹。转到 **bin** 文件夹。

![Screenshot--49-](img/2742f4350d13a0126485f5936dccc244.png)

⚠️如果你使用的是 32 位操作系统，那么你必须进入你的 **mingw32** 文件夹>bin 文件夹。

复制目录。

![Screenshot--51-](img/6a20211a13b282741b81b5094242753f.png)

⚠️如果您使用的是 32 位操作系统，并且您还在默认目录中安装了 MSYS2，那么您的目录应该如下所示:

```
C:\msys64\mingw32\bin
```

打开**高级系统设置**。你可以用很多方法做到这一点。一个简单的方法是简单地点击开始按钮，然后像下面的截图一样搜索它。

![Screenshot--52-](img/ca8038fe96c670a3e5ad7c8069f0e2e6.png)

从高级选项卡中点击**环境变量**。

![Screenshot--54-](img/218cdafae545cfc38b17d0f02763b116.png)

点击**路径**并选择。然后点击**编辑**。

![Screenshot--57-](img/8e7ae1b177a6901633b809533a011bbe.png)

将出现如下窗口:

![Screenshot--58-](img/f7dc1eccdc3b5b55dfd36d2c6c1ad481.png)

点击**新建**。

![Screenshot--59-](img/37574e9364a17e6583682575d488423a.png)

将出现一个空白框。

![Screenshot--60-](img/cccbd9bf8d300abd1606af75163ab0c0.png)

将目录粘贴到这里。

![Screenshot--61-](img/6e871f637e70efade962e8224be52121.png)![Screenshot--62-](img/39aa01fa50c041ba866de4a427ac902e.png)

点击**确定**。

![Screenshot--63-](img/830b92ddeeb06ce322a29cb28b6aab3d.png)

点击**确定**。

![Screenshot--65-](img/7519302eff8b9d07fe7d2face8312601.png)

点击**确定**。

![Screenshot--66-](img/45b410343ed33a72f2de19f93847e164.png)

如果你想获得视频中的所有步骤，那么你也可以观看[这个视频](https://www.youtube.com/watch?v=0HD0pqVtsmw)。

## 检查安装

现在是时候检查我们是否已经成功安装了以上所有内容。

打开终端/ PowerShell / CMD 并连续应用命令:

用于检查 **GCC** 版本:

```
gcc --version
```

![Screenshot--68-](img/d4a9884f76fc1bf78c04820890ae3213.png)

用于检查 **G++** 版本:

```
g++ --version
```

![Screenshot--69-](img/f7ced4bf8839d90f64d12c29618de114.png)

用于检查 **GDB** 版本:

```
gdb --version
```

![Screenshot--70-](img/712957be0394746d12c667315affc470.png)

## 结论

我希望本文能帮助您在 Windows 操作系统上安装 C 和 C++程序的编译器。

非常感谢你阅读整篇文章。如果你有任何建议，那么你可以通过我的 [twitter](https://twitter.com/Fahim_FBA) 或 [LinkedIn](https://www.linkedin.com/in/fahimfba/) 账户与我联系。

如果你对开源感兴趣，那么你也可以在 [GitHub](https://github.com/FahimFBA) 上关注我，因为我非常积极地为开源项目做贡献。

如果你喜欢编程相关的教程，那么我有两个 YouTube 频道。在一个频道中，[我定期发布孟加拉语内容](https://www.youtube.com/channel/UCEF4lxmpBKV2oYCSFH6ExIQ)，而我在另一个频道中定期发布英语内容。

祝你有美好的一天！