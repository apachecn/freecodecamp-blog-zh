# 对非 PWA 人群的渐进式网络应用程序的解释

> 原文：<https://www.freecodecamp.org/news/an-explanation-of-progressive-web-apps-for-the-non-pwa-crowd-8a400e275ea1/>

不久前，应用程序的世界被分为两类。你要么是在为 Android 设备创建应用程序，要么是在为 iOS 创建应用程序。进入 PWAs，或拉长，P 前进 **W** eb **A** 应用程序。在过去的几年里，你可能已经听说过他们了，但是除了一个好听的首字母缩略词，你根本不知道什么是 PWA。随着它们越来越受欢迎，了解一下这些大惊小怪的事情是什么可能是个好主意。

在本文中，我将带您了解什么是 PWA，它由哪些组件构成，并向您展示如何自己制作一个 PWA。

#### 基础知识

渐进式 web 应用程序是将网站转变为应用程序。这意味着，你不必用 Java 或 Objective-C(或更新的移动编码语言)来编码，你可以为应用程序写代码，就像你为一个网站写代码一样。您已经有了 html 文件、样式表和脚本。

为什么要构建 PWA 而不是原生应用？首先，想象一下一旦你发布了一个 PWA，你可以不断地修改它，而不必重新发布你的应用程序。由于所有代码都托管在服务器上，而不是 APK/IPA 的一部分，所以你所做的任何更改都是实时发生的。

如果您曾经使用过依赖于网络连接的应用程序，那么您一定很熟悉这种无能为力的挫败感。借助 PWAs，您可以在网络出现问题时为用户提供离线体验。

为了在顶部添加樱桃，可以提示用户将你的 PWA 添加到他们的主屏幕上。本机应用程序不具备的东西。

#### 成分

有一个关于 PWA 的标准，如果你想发布它，你必须遵守它。每个 PWA 由以下组件构成:

*   web 应用程序清单
*   服务人员
*   安装体验
*   HTTPS
*   创建 APK
*   灯塔审计

#### 清单

这纯粹是一个配置文件( ***)。JSON*** )，使您能够更改您的 PWA 的各种设置以及它将如何呈现给用户。下面是一个例子:

您必须设置名称/简称键。设置两者时，短名称将在主屏幕和启动器上使用。名称值将用于添加到主屏幕体验(或应用程序安装提示)。

显示可以有四个不同的值:

*   全屏 -这允许你的应用程序在打开时占据整个屏幕
*   **独立** -你的应用看起来像一个本地应用，隐藏了浏览器元素
*   **minimal-ui** -提供一些浏览控件(仅支持 Chrome mobile)
*   浏览器——顾名思义，你的应用程序的外观将与浏览体验完全相同

您还可以设置应用程序的**方向**和应用程序中页面的**范围**。

不要忘记将下面的 meta 标记放入 head 标记中，将清单添加到主 html 文件中:

![-sgj8knyKimbaSIeLGhmo5oflTKZzHunce4V](img/d5cf0483bf83d2e7da794e425af93f7e.png)

Photo by [sol](https://unsplash.com/@solimonster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

#### 服务人员

服务人员是运行在浏览器网站后台的一个组件。它具有广泛的功能，包括推送通知、缓存资产并提供离线体验，以及推迟操作直到用户稳定连接到互联网的能力。一个服务人员本身就可以成为一篇完整的媒体文章，所以我不会深究其工作原理的内部细节。但是，我会提供一个普通的例子，供您在 PWA 中使用。

习惯上将与服务人员相关的代码保存在一个名为 ***sw.js*** 的文件中。

> ✋服务工作者的位置很重要，因为它只能访问与自己在同一目录或子目录中的文件。

服务人员的生命周期可以归纳为以下几个阶段:

*   登记
*   安装/激活
*   应对各种事件

#### 安装体验

PWA 的独特之处之一是它的安装体验。这意味着提示用户安装你的应用程序。为了允许我们向用户展示这种能力，我们需要在 installprompt 之前监听一个名为 ***的事件。下面是一个代码示例，展示了从向用户提供添加应用程序的选项到根据他们的选择激活逻辑的流程。***

![LG4XqHneeagI9dGNOJ28F2oYInSR6vjQRTvy](img/8bcb33b47c6e5c03b24d3a9d0f8c217d.png)

Photo by [James Sutton](https://unsplash.com/@jamessutton_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

#### HTTPS

不久以前，网站仍然可以使用非常普遍的 http 协议。由于最近在安全和 [Chrome](https://searchengineland.com/effective-july-2018-googles-chrome-browser-will-mark-non-https-sites-as-not-secure-291623) 方面的变化，所有不在 https 协议下运行的网站将被标记为不安全。即使您的网站不处理用户数据或敏感通信，切换到 https 仍然是一个好的做法。

就像我前面提到的，如果你想发布 PWA，它必须使用 https 协议。如果你不想陷入获取域名、为其找到合适的主机然后启用 SSL 的麻烦中，你可以选择 Github 这个简单的选项。如果你有一个帐户，你可以打开一个存储库并建立一个 GitHub 页面。这个过程相当简单明了，额外的好处是让 HTTPS 成为 Github 的一部分。

#### 创建 APK

为了让我们的 PWA 在谷歌 Play 商店可用，我们需要创建一个 APK。你可以使用流行的工具， [PWA2APK](https://pwa2apk.com/?ref=steemhunt) ，它会为你完成这项艰巨的工作。但是如果你更喜欢自己学习如何做，请继续阅读。

谷歌推出了一种新的方式来将你的 PWA 整合到 Play store 中，使用所谓的*生锈的***W***EB***A***activity，或 TWA。只需几个简单的步骤，你将学会如何创建一个 TWA，然后你可以上传到播放商店。*

1.  *打开 Android Studio 并创建一个空活动*
2.  *转到项目的 build.gradle 文件并添加 jitpack 存储库*

*3.转到 ***模块级*** build.gradle 文件，并添加以下代码行来启用 Java8 兼容性*

*4.添加 TWA 支持库作为依赖项*

*5.将 activity XML 添加到 AndroidManifest 文件中的应用程序标记之间*

*6.我们需要使用数字资产链接来创建从应用程序到网站的关联。将以下内容粘贴到您的 ***strings.xml*** 文件中*

*7.将下一个 meta 标记作为子标记添加到 AndroidManifest.xml 内的应用程序标记中*

*8.[创建上传密钥和密钥库](https://developer.android.com/studio/publish/app-signing#generate-key)*

*9.使用以下命令提取 SHA-256*

*10.转到[资产链接生成器](https://developers.google.com/digital-asset-links/tools/generator)，提供 SHA-256 指纹、应用程序包和网站域名*

*11.将结果放在位置 ***/下名为 ***assetlinks.json*** 的文件中。知名*** 在你网站的目录里。Chrome 会专门寻找这个目的地。*

*12.[生成签名的 APK 并上传至播放商店](https://medium.freecodecamp.org/how-to-publish-an-application-in-the-play-store-8ddcc6dc3587)*

*![mp3eDdZW9F9StMhoajqbVozrN3FPeyDgQw8s](img/b12b8925a761df5053692dc44c7b735f.png)

Photo by [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)* 

#### *灯塔*

*到目前为止，我相信你已经忘记了你的 PWA 需要什么，所以它将有效地发布。要考虑的事情太多了，以至于很容易忘记需求是什么。*

*幸运的是，谷歌创造了[灯塔](https://developers.google.com/web/tools/lighthouse/#devtools)。它可以在 Chrome 开发者工具中找到(来自 Chrome 版本 60)。通过在浏览器中单击鼠标右键并选择 inspect，可以很容易地访问它。当新窗格打开时，您会在右上角看到一个 ***审计*** 选项卡。*

*![iUXU9aPKpNWuJnHTDj6gfjsMpDewFzo4Zvy4](img/f0fdb5b667a20e00c1a9c438959122fc.png)

The Audits Tab* 

*保持此选项卡中的设置不变，您现在可以通过单击“运行审计”按钮来运行审计。这将需要一两分钟的时间，但在结束时，您会收到一份信息丰富的图形演示，显示您的 PWA 在三个属性方面的排名:*

*   *表演*
*   *易接近*
*   *最佳实践*

*每个属性都有一个细分，说明您的应用程序在哪里通过了需求，在哪里没有通过。这可以让你看到哪里需要调整，哪里符合标准。如果你有兴趣，你可以在这里找到清单[的细目。](https://developers.google.com/web/progressive-web-apps/checklist#baseline)*

#### *把它举起来*

*我们已经到了旅程的终点，希望你已经做好了更好的准备，可以在 PWAs 的世界里航行。这篇文章的灵感来自于我最近创建一个的过程。您可以在下面查看:*

*[**Android 菜单 XML 生成器 Google Play 上的应用**](https://play.google.com/store/apps/details?id=com.tomerpacific.androidmenugenerator)
[*为你的 Android 应用生成你需要的任何类型的菜单。从选项、上下文或弹出菜单中选择，然后…*play.google.com](https://play.google.com/store/apps/details?id=com.tomerpacific.androidmenugenerator)*