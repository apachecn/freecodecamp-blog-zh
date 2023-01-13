# 如何在互联网信息亭中实现 PWA 和 Barba.js

> 原文：<https://www.freecodecamp.org/news/how-to-implement-a-pwa-and-barba-js-into-internet-kiosks-854d4895fd04/>

by Nino Mihovilić

# 如何在互联网信息亭中实现 PWA 和 Barba.js

![AkDSI3Y5Em7hdTQlTJe2apPOpBSuiMYlspFy](img/50a659bfdbc6c4dd33f4128fac04bcc0.png)

Photo by [rawpixel](https://unsplash.com/photos/VEJOYqTQTjs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/internet-cafe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们将在这里描述的项目是一个交互式互联网亭，它被用作 LikeUs 移动应用程序的扩展。LikeUs 是一款移动应用，用户可以轻松选择外出、喝咖啡或听音乐会的地点。因为萨格勒布的 tkal ci EVA 街是很多年轻人聚集的地方，所以我们决定在这里进行 LikeUs 应用的线下推广。

#### 实施 Kiosk 模式

我们面临的第一个挑战是在我们的设备上实现 kiosk 浏览器模式。这是一个 Android 盒子，我们使用 Chrome 作为网络浏览器来运行应用程序。Kiosk 浏览器模式是一种全屏运行应用程序的模式，没有任何浏览器用户界面，或者在我们的例子中，没有任何 Android 用户界面。

其目的是防止用户运行除基于浏览器的内容之外的任何内容。由于有数百个 kiosk 模式应用程序，我们决定使用其中一个可用的应用程序，而不是从头开始构建一个。

经过一番研究，我们决定使用 [Kiosk 浏览器锁定](https://www.android-kiosk.com/)应用。它拥有我们一直在寻找的所有功能:

*   将设备锁定到单个 URL
*   隐藏工具栏选项
*   隐藏通知屏幕
*   隐藏 Android 用户界面

下一步是在 Android 环境和 Kiosk 浏览器应用程序中测试 PWA。这时候我们才发现事情不会像我们计划的那么顺利！

我们遇到的第一个问题是在前端——最终的网站看起来像是初始设计的升级版本，这是由于一些屏幕限制和不同的渲染环境。随着截止日期的临近，我们没有足够的时间来调整 CSS 中的每一项以适应最初的设计大纲，所以我们决定缩减整个文档。

考虑到我们所有的投入，这是一个合理的方法。不得不再次测试所有东西是一个很大的缺点，但是我们必须确保所有东西在这种环境下都能工作。

第二个问题是，像谷歌地图这样的外部脚本无法通过 PWA 加载到 Kiosk 浏览器应用程序中，所以我们做了一点小小的调整。我们启动了 Kiosk 浏览器应用程序，它删除了 Android 用户界面，然后退出 Kiosk 浏览器应用程序，并在 Kiosk 浏览器应用程序之外启动 PWA。通过这种方式，我们成功地移除了 Android 界面，并加载了所有外部脚本。

![HpUfeFTjyE05czMLe-X7GgCVO0uacIPiAjrE](img/878f6174cc89d6bea81bfa595a76101d.png)

#### 开发渐进式 Web 应用程序

在浏览了项目简介和规范之后，我们想到的第一件事是…我们应该做一个 PWA(渐进式 Web 应用程序)。渐进式 Web 应用程序是一种提供与原生移动应用程序类似的功能的应用程序:

*   服务人员允许应用程序几乎即时可靠地显示内容，因为他们缓存了每个请求
*   可以像普通的本地应用程序一样将应用程序添加到主屏幕
*   推送通知可以实现多种用途
*   这个应用程序又快又流畅
*   它使用 HTTPS，易于实现。

在评估了客户的要求后，PWA 的所有功能都符合我们的要求。

我们的要求是:

*   构建一个可以在交互式屏幕上使用的应用程序
*   该应用程序应该使用我们为 LikeUs 移动应用程序构建的现有 API
*   使用的设备将是一个安卓盒子
*   互联网访问将受到限制，因为该应用程序将连接到公共网络(这将在以后改变)
*   该应用程序应该有一个横幅和横幅管理系统的附加部分

我们可以使用现有的 API 构建一个 web 应用程序，而不必实现额外的功能，我们还可以构建一个简单的 CMS(内容管理系统),用于横幅管理和推送内容重载通知。由于互联网访问会受到限制和不稳定，我们可以使用 PWA 功能来缓存页面，甚至在应用程序离线时也能提供服务。

请务必查看本[深度教程](https://medium.com/@jewbre/service-workers-6a5c13c9a123)和对服务人员的解释。

#### 调整横幅管理系统

该应用程序分为两个部分。顶部是横幅部分，底部是分成选项卡的主要部分。

我们有两种类型的横幅——Youtube 视频和图片。由于横幅可以改变，我们需要开发一个 CMS。我们开发了一个简单的 CMS，客户端可以将 Youtube 视频和图像输入到滑块中。

我们在这里遇到的问题是刷新应用程序以重新加载新的横幅内容。你看，因为当时 app 用的是 Barba.js，所以从来不刷新。为了让它工作，我们使用了 PWA 的一个很酷的功能——推送通知。推送通知是一种使用通知 API 和推送 API 从服务器向客户端发送消息的功能。

推送通知如何帮助解决我们的内容重载问题？解决方案非常简单明了。当用户更改 CMS 中的横幅内容时，我们向 PWA 发送推送，然后 PWA 刷新两次。PWA 需要刷新两次，以删除缓存并重新加载新内容。

#### 处理外部障碍

互联网信息亭通常放置在室外环境中，那里的互联网连接有时不稳定且速度较慢。当互联网连接是公共的并且在相当拥挤的街道上时，当使用实时通信和外部 API 时，你会面临许多问题。

一种常见的“黑客”方法是延长延迟时间，并希望一切正常。尽管这不是首选方式，但如果其他方式都失败了，它可以作为备用方式。

谷歌地图是让我们头疼的外部 API 之一。我们必须重新加载并添加新的 pin，但在慢速连接上，这有时是不可能的。

![hyvg705AuzUl8rybWXHDDk9stgbs7vsnTmec](img/6e28c1c6f496231e591be6447328b949.png)

#### 固定内容和动态内容之间的平衡

优化不仅应用于高级缓存技术和内容交付网络领域。智能布局放置和理解可以从页面重新加载流程中“推出”的元素可以减少请求的数量，并加快整个导航流程。

亭中的广告内容托管在 Youtube 上——它是一个视频滑块，在所有页面上重复播放。在那之下，我们有内联导航的主要内容。当选择不同的导航项目时，默认的浏览器行为是重新加载整个页面，包括固定的广告区域。这是一个性能噩梦，尤其是当有 Youtube API 这样的外部脚本时。

这里的问题是——如何只重新加载页面的一个特定部分？嗯，不会有浏览器重新加载，唯一能做的是在后台改变内容，而不离开页面。

由于实施了分析，我们不得不相应地更新 URL。我们通过使用 PJAX (Push State Ajax)技术做到了这一点。这项技术允许在后台预取和交换目标 DOM 节点。

为了避免内容闪烁，请创建一个在内容更改时触发的简单渐变过渡。由于手动管理内容交换的所有状态非常耗时，我们使用了一个名为 Barba.js 的外部库。该库允许高级过渡管理，并与所有动画框架兼容。

![yzC0x5NH-VvltNXi-nOOXA871NO2gcJnlcQy](img/37a86a7f7958addafc7a9d76d8faed66.png)

Barba.js 具有内部状态缓存，可用于利用浏览器缓存和优化加载时间。Barba cache 是一个全局 Javascript 对象，其中每个值都是一个必须解决的承诺。

#### 实施分析和虚拟浏览量

我们想测量用户互动和页面浏览量。因为我们用的是 Barba.js，这基本上是一个没有页面重载的单页 app，所以衡量这类 app 浏览量的诀窍就是使用虚拟浏览量。它们是发送到 Google Analytics 的页面点击，并没有真正重新加载页面。

第一步是包含 [Google Tag manager](https://developers.google.com/tag-manager/quickstart) 代码，然后实际发送虚拟页面视图到数据层。我们可以用下一段代码来做:

```
dataLayer.push({ 'event': 'VirtualPageview', 'virtualPageURL': currentUrl, 'virtualPageTitle': title });
```

需要在每个新页面上调用这个代码片段。在每次用户交互打开一个新的“页面”时，我们称之为将页面 URL 和页面标题发送到 Google Analytics 的代码片段。通过这种方式，我们可以跟踪使用 Barba.js 或任何其他 PJAX 技术的单页面应用程序的页面浏览量。

![pkJtXRjiySB4CfjP6v8gC1ujMJZFTSGe2qjI](img/46171d6c997ae3faf2e4de6609eed46a.png)

#### 最后

在特定的环境中工作时，有时“照章办事”的解决方案并不是你唯一的解决方案。在一个不太标准的环境中，通常有机会在一系列特定的挑战中创新和使用一些常用的工具和库。

*最初发表于[www.bornfight.com](https://www.bornfight.com/blog/how-to-implement-pwa-and-barba-js-into-internet-kiosks/)。*