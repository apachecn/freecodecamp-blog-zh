# 用 Chromebook 编码

> 原文：<https://www.freecodecamp.org/news/coding-with-a-chromebook/>

Chromebooks 太棒了。它们是相对简单和便宜的设备，运行谷歌开发的基于 Linux 的操作系统 Chrome OS。

虽然它们非常适合只需要一个网络浏览器来执行基本任务的人，但如果你只准备开发 Chromebook，你可能会想投资一台 PC 是否更好。

但是随着所有基于云的技术和 Chrome OS 的最新更新，你有了很多选择。我们将在这里讨论几个流行的。

## 基于云的解决方案

如果你完全是网络开发的新手，当然还有 freeCodeCamp.org。整个课程可以完全在浏览器中完成，并利用工具如 [CodePen](https://codepen.io/) 、 [CodeSandbox](https://codesandbox.io/) 、 [Glitch](https://www.freecodecamp.org/news/p/633c27ad-f2a2-4b7a-a56b-e85620d957dc/glitch.me) 和 [Repl.it](https://repl.it/) 进行更复杂的项目。

即使是有经验的开发人员也可以使用上面列出的一个或多个网站来快速制作原型，并轻松地与他人分享他们的项目，所有这些都是免费的。虽然它们可能比本地开发环境慢，但事实上，您可以从任何连接互联网的设备访问它们，并且所有内容都保存到它们的服务器上，这是一个很大的优势。

## Chromebooks 的 Linux 系统

从 [Chrome OS v.69](https://9to5google.com/2018/09/18/google-chrome-os-69-stable-release/) 开始，你可以为 Chrome book 启用 Linux，并在[选择 Chrome book](https://www.chromium.org/chromium-os/chrome-os-systems-supporting-linux)上安装 Linux shell 的测试版。虽然支持的设备列表很短，但大多数(如果不是全部)未来的 Chromebooks 都有望[支持这个功能](https://www.zdnet.com/article/all-chromebooks-will-also-be-linux-laptops-going-forward/)。

基本上发生的是 Chrome OS 在虚拟机中运行一个版本的 Debian。因为 Debian 是流行的 Linux 发行版/操作系统 Ubuntu 的基础，所以你应该可以在 Chromebook 上安装任何东西，就像在 Debian/Ubuntu 机器上一样。

比如你想安装 Firefox，你需要做的就是打开终端，输入`sudo apt install firefox`。

不过，也有一些缺点。目前这项功能还在测试阶段，非 Chrome 操作系统的应用程序仍然不支持硬件加速。

像 Firefox 或 VSCode 这样的软件在其他运行 Linux 发行版如 Ubuntu 的机器上会运行得更慢。这也意味着视频解码会慢一点，回放可能会受到影响。此外，还不支持麦克风和网络摄像头等设备。

查看[该页面](https://support.google.com/chromebook/answer/9145439?hl=en)以了解更多关于尚不支持的内容的信息。