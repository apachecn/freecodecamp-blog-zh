# 铬是什么？Chromium 网络浏览器与 Chrome 有何不同

> 原文：<https://www.freecodecamp.org/news/what-is-chromium-how-the-chromium-web-browser-is-different-from-chrome/>

铬...听起来像是元素周期表上的东西([是](https://en.wikipedia.org/wiki/Chromium))或者超级英雄用来打败敌人的东西。

有趣的事实:这也是一款听起来很像谷歌 Chrome 的浏览器。有趣的是，这两者在某些方面非常接近。

所以让我们多了解一些关于 Chrome 的知识——它是什么，以及它与 Chrome 有什么不同。

## 铬是什么？

Chromium 是由 Chromium 项目运行的开源网络浏览器，于 2008 年首次发布。任何开发人员都可以修改或更新源代码(但是只有少数 Chromium 开发人员可以添加他们自己的代码)。

Chromium 有一个相当活跃的支持它的贡献者社区。因此，浏览器经常更新，这很好。它的不同部分被注册在各种不同的许可证下，像 [BSD 许可证](https://en.wikipedia.org/wiki/BSD_licenses)(对于谷歌写的部分-下面会有更多的描述)和[麻省理工](https://en.wikipedia.org/wiki/MIT_License)、 [LGPL](https://en.wikipedia.org/wiki/GNU_Lesser_General_Public_License) 以及其他的。

如果你想看看，请在这里下载。

## 什么是 Chrome？

Chrome 是谷歌的网络浏览器。谷歌的开发人员开发、维护和发布它。但这里有一个很酷的事实:Chrome 是建立在 Chrome 的开源代码之上的。

那么这是如何工作的呢？嗯，Google 开发人员利用 Chromium 代码，在它的基础上构建他们自己的专有特性。这给了我们铬。所以 Chrome 有一些额外的功能，比如自动更新、浏览器数据跟踪和对 Flash 的原生支持。

这些特性中有些是有帮助的，有些(比如数据跟踪)让开发人员感到紧张。

如果你是粉丝，你可以在这里下载。

## 它们有多相似

因为谷歌的 Chrome 实际上是建立在 Chrome 的源代码之上的，它们共享相同的骨骼，正如我们已经确定的那样。

他们的 logos 也挺像的。Chrome 的是谷歌主题的多色，chrome 的是几个深浅不一的蓝色。

但是您可能更关心这些差异，所以让我们来看看它们。

## 铬和铬的最大区别

### 他们处理更新的方式不同

Chromium 一直在更新，因为开发者在不断修改源代码。麻烦(还是很酷的事？)是，你需要手动更新浏览器，因为它不会自己更新。

另一方面，Chrome 偶尔会自动更新。所以你不用记得更新你的浏览器。但是它不像 Chromium 那样经常更新。

因此，如果你想在 Chromium 项目发布时就看到最新最棒的源代码，那么最好使用 Chromium 浏览器。

### 隐私问题

当然，Chrome 超级好用。但是如果 Chrome 是你的选择，谷歌将会跟踪你的信息。铬不会这样。

如果这让你不舒服，但你仍然喜欢 Chrome 作为浏览器，也许 Chrome 是你更好的选择。你的隐私将得到更多的保护，但你仍然可以获得基于 Chromium 源代码的体验。

### 支持 Adobe Flash

谷歌 Chrome 内置了对 Adobe Flash player 的支持。但是由于 Flash 的代码不是开源的，Chromium 项目不会使用它。因此，如果你使用 Chromium 并想启用 Flash，你必须[通过一些关卡](https://helpx.adobe.com/flash-player/kb/flash-player-chromium.html)。

然而，请注意，由于 Flash 将在 2020 年被淘汰，取而代之的是 HTML5，这在不久的将来应该不是一件大事。

### 我的编解码器王国

你可能会问，什么是编解码器？一个[编解码器](https://en.wikipedia.org/wiki/Codec)(编码和解码的组合)是一个计算机程序，它在压缩和收缩大文件格式的同时，在模拟和数字声音之间进行转换。

由于音乐和视频文件很大，编解码器被创建来编码(或缩小)这些文件，然后在准备观看或编辑时解码它们。

那么我们为什么需要这些呢？如果你度过了漫长的一周，只是想今晚去网飞放松一下，你需要这些编解码器来播放这些内容。

虽然 Chrome 自带内置媒体编解码器(如 AAC、H.264 和 MP3)，但 Chrome 没有。因此，如果你想观看一些节目，你要么需要使用 Chrome，要么在 Chrome 中手动安装这些编解码器。

### Google Play 商店与外部扩展

如果你想在 Chrome 中下载一个扩展，你只能从[谷歌 Play 商店](https://play.google.com/store?utm_source=na_Med&utm_medium=hasem&utm_content=Mar0519&utm_campaign=Evergreen&pcampaignid=MKT-DR-na-us-1000189-Med-hasem-py-Evergreen-Mar0519-Text_Search_BKWS-id_100566_%7cEXA%7cONSEM_kwid_43700023142506782&gclid=EAIaIQobChMI-u3I97zx5AIVkcVkCh2ZTAAkEAAYASAAEgKTvPD_BwE&gclsrc=aw.ds)中下载(在 Mac 和 Windows 上)。如果你想得到一些外部扩展，你必须启用[开发者模式](https://developer.chrome.com/extensions/faq)。

另一方面，如果你使用的是 Chromium，可以随意获取任何你想要的扩展。

### 安全沙箱模式

当你在 Chrome 中使用插件时，浏览器会限制它们的功能，使它们只能执行你下载的功能。也就是说，它们被“沙箱化”或仅限于此目的。Chrome 会自动限制这些插件。

出于安全原因，这通常是件好事。但是 Chromium 并不总是立刻启用沙盒模式。你可以在这里了解更多关于那个[。](https://chromium.googlesource.com/chromium/src/+/master/docs/design/sandbox_faq.md)

## 哪个浏览器适合你？

最后，这取决于你想从你的浏览体验中得到什么。

如果你想要一个简单的开箱即用的浏览器，不需要太多的关注，Chrome 可能适合你。

但是如果你担心隐私问题，并且不介意深入研究和做一些工作，Chromium 可以提供有益的体验。

浏览愉快！