# 如何在 HTML 电子邮件中启用黑暗模式——你需要知道的一切

> 原文：<https://www.freecodecamp.org/news/dark-mode-in-html-email-everything-you-need-to-know/>

随着新的 iOS 13 更新，苹果邮件正在获得一个黑暗的主题。这意味着它是第一个支持`prefers-color-scheme` CSS 媒体查询的主要电子邮件客户端。所以你现在可以专门为黑暗和光明的主题设计邮件。

我是黑暗模式的超级粉丝，刺眼的明亮邮件是我的克星。所以当我在 iOS 13 中了解到黑暗模式时，我做了唯一显而易见的事情，订购了一部全新的 iPhone 来测试。

在此期间，我还测试了几乎所有电子邮件客户端的黑暗模式，包括麻烦制造者 Outlook。这是我的发现。

首先，w **帽子的配色方案是什么？**
`prefers-color-scheme`CSS 媒体查询用于检测用户喜欢浅色主题还是深色主题，从而可以为两者专门设计电子邮件。

随着 iOS 13 的更新， ****在最受欢迎的电子邮件客户端中的支持从 2.3%跃升至 38.4%**** ！由于苹果邮件的流行，这是一个巨大的进步。令人惊讶的是，Outlook 是 Apple Mail 之前唯一支持这一功能的电子邮件客户端。

## 黑暗模式如何在流行的电子邮件客户端工作

为了使电子邮件本身变暗，电子邮件客户端会在后台自动反转电子邮件的颜色。对于常规的用户对用户的电子邮件，这在所有的电子邮件客户端中运行良好且一致。

然而，定制 HTML 电子邮件就没那么简单了——那些填满了我们大部分收件箱的邮件。我说的是交易和促销。

以下是我发现的电子邮件客户端处理黑暗模式电子邮件渲染的不同之处:

| 电子邮件客户端 | 流行 | 深色用户界面 | 自动反转电子邮件颜色 | 支持@media(首选颜色方案) |  |
| --- | --- | --- | --- | --- | --- |
| **苹果邮箱** iPhone + iPad | 36.1% | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/applemail-ios.png) |
| Gmail Android 10 | 27.8% * | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/gmail-android-1.png) |
| Gmail iOS 13 版 | 27.8% * | ✖没有 | ✖没有 | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/gmail-ios.png) |
| **Gmail** 网络邮件 | 27.8% * | ✔ Yes | ✖没有 | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/gmail-webmail.png) |
| **展望** iOS 13 | 9.1% * | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/outlook-ios.png) |
| **展望** Android 10 | 9.1% * | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/outlook-android-1.png) |
| **Outlook** Windows 10 | 9.1% * | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/outlook-windows-10.png) |
| **展望** macOS | 9.1% * | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/outlook-macos.png) |
| **苹果邮件**苹果电脑 | 7.5% | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/applemail-macos.png) |
| 雅虎！网络邮件 | 6.3% * | ✔ Yes | ✖没有 | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/yahoo-webmail.png) |
| 美国在线的网络邮件 | 6.3% * | ✖没有 | ✖没有 | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/aol-webmail.png) |
| **Outlook.com**网络邮件 | 2.3% | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/outlook-webmail.png) |
| **Windows 10 邮件** Windows 10 | 0.5% | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/windows-10-mail.png) |
| **Zoho Mail** webmail | 不到 0.5% | ✔ Yes | ✔ Yes | ✖没有 | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/zoho-webmail.png) |
| Mozilla 雷鸟 Windows 10 | 不到 0.5% | ✔ Yes | ✖没有 | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/thunderbird-windows-10.png) |
| **Spark** macOS | 不到 0.5% | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/spark-macos-dark-mode.png) |
| **火花** iOS 13 | 不到 0.5% | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/spark-ios-dark-mode.png) |
| **Spark** 安卓 9 | 不到 0.5% | ✔ Yes | ✔ Yes | ✔ Yes | [(展示截图)](https://sidemail.io/assets/dark-mode-in-html-email/spark-android-dark-mode.png) |

***由于无法可靠地区分流行度，因此流行度在同一电子邮件客户端的所有平台上共享。人气来源:** [**石蕊，2019 邮箱客户端市场份额**](https://litmus.com/blog/infographic-the-2019-email-client-market-share) **。**

([访问原始帖子](https://sidemail.io/articles/dark-mode-in-html-email/)查看我的测试笔记，并查看最新的测试，因为我将逐步测试更多的电子邮件客户端，并首先更新那里的文章。)

## 如何使 HTML 电子邮件的黑暗模式友好

我已经将数据投入使用，在经历了一些与 Outlook 相关的挑战后，我让我们的电子邮件黑暗模式变得友好。 ****你也可以这样做:****

> ****数据怎么说:****
> 超过 55%的邮件可能在开启黑暗模式的情况下打开。一旦 Gmail 加入黑暗面，可能在启用黑暗模式下打开的电子邮件将飙升至 83%！

### 1)调整颜色

当心苹果邮件，因为它只有在背景颜色透明或未指定的情况下才会反转颜色——****白色背景不行**** 。确保你的邮件不会蒙蔽任何人的最简单的方法是检查背景颜色是否被指定。为了更好地控制设计，这就是`prefers-color-scheme`派上用场的地方。

****语法(@ media prefers-color-scheme):****

```
<style>
	/* Your light mode (default) styles: */
	body {
		background: white;
		color: #393939;
	}

	@media (prefers-color-scheme: dark) {
		/* Your dark mode styles: */

		body {
			background: black;
			color: #ccc;
		}
	}
</style> 
```

****一个设计小技巧:**** 避免纯白色`#fff`作为文字颜色。我发现 11.5 左右的对比率对于正文来说是一个不太亮也不太暗的很好的折衷。在这里检查对比度:[https://contrast-ratio.com](https://contrast-ratio.com/)或者使用 Chrome dev 工具。

![Switching between light and dark logo version in HTML email with prefers-color-scheme media query](img/8171549a2db5c8cc0608089bd5879a03.png)

### 2)明暗标志之间的切换

深色背景上的深色文字几乎是看不见的，如果在启用了深色模式的电子邮件客户端中查看，这正是徽标的情况。

现在，一个典型的标志通常有一个透明的背景，彩色的图标和黑色的副本。看到问题了吗？因为电子邮件客户端不会反转图像颜色，所以您需要自己处理。

要解决这个问题，您可以:

1.  用白色背景而不是透明背景保存徽标(解决这个问题的最简单方法)。但我不会推荐这种方式——黑暗模式用户不会高兴的。
2.  在深色背景上放一个浅色标志，电子邮件的其余部分放在白色背景上。
3.  将黑暗模式电子邮件设为您的默认设置。Spotify 是一个很好的候选人，因为他们的应用程序中只提供黑色主题。
4.  包括您的徽标的浅色和深色版本，并通过`prefers-color-scheme`媒体查询进行切换

我最喜欢的是最后一种方法，所以你可以这样做:

一个简单的黑色标志在所有现代的电子邮件客户端都很好用。但出乎所有人意料的是，它在 Outlook 和 Windows 10 Mail 中不起作用。

在 CSS 样式中:

```
<style>
	@media (prefers-color-scheme: dark) {
		.darkLogo {
			display: none !important;
		}

		.lightLogoWrapper,
		.lightLogo {
			display: block !important;
		}
	}
</style> 
```

…以及 HTML 结构:

```
<image src="dark-logo.png" class="darkLogo" />

<!--
	To hide the light logo perfectly in Outlook and Windows 10 Mail, 
	you need to wrap the light logo image tag with a div.
-->
<div class="lightLogoWrapper" style="mso-hide: all; display: none">
	<image src="light-logo.png" class="lightLogo" style="display: none" />
</div> 
```

这种方法工作得很好，但是仍然不能全面正确地工作。支持深色模式但不支持`prefers-color-scheme`的电子邮件客户端会出现深色背景上的深色文本问题。那就是 Outlook，Windows 10 邮件，Zoho，可能还有 Gmail。

![Bulletproof method: switching between light and dark logo version in HTML email with prefers-color-scheme media query](img/703e36be457be6471e8804b1c5bf78e4.png)

因此，为了使电子邮件中的徽标完全防弹，我将结合上面的方法 1 和 4。方法 1 将涵盖所有支持黑暗模式的电子邮件客户端，但不包括`prefers-color-scheme`。第四种方法将涵盖苹果邮件、苹果电脑上的 Outlook 和 Outlook.com，这两者都支持。

此外，我将添加一个 3 像素宽的背景匹配边框，并像往常一样将其保存在透明背景上，而不是将徽标保存在白色背景上。

它开始看起来相当复杂(只是对于一个徽标来说)，所以让我们先看看 HTML 标记:

```
<!-- Default logo with 3-pixel wide background-matching border -->
<image src="dark-logo-with-background.png" class="darkLogoDefault" />

<!-- Light theme (so dark logo): 
This is for Apple Mail, Outlook on macOS, Outlook.com -->
<div class="darkLogoWrapper" style="mso-hide: all; display: none">
	<image src="dark-logo.png" class="darkLogo" style="display: none" />
</div>

<!-- Dark theme (so light logo): 
This is for Apple Mail, Outlook on macOS, Outlook.com -->
<div class="lightLogoWrapper" style="mso-hide: all; display: none">
	<image src="light-logo.png" class="lightLogo" style="display: none" />
</div> 
```

…和 CSS 样式:

```
<style>
	@media (prefers-color-scheme: light) {
		.darkLogoDefault,
		.lightLogo {
			display: none !important;
		}

		.darkLogoWrapper,
		.darkLogo {
			display: block !important;
		}
	}

	@media (prefers-color-scheme: dark) {
		.darkLogoDefault,
		.darkLogo {
			display: none !important;
		}

		.lightLogoWrapper,
		.lightLogo {
			display: block !important;
		}
	}
</style> 
```

## Gmail 中的黑暗模式即将到来

黑暗模式将在新的 Android 10 中出现，Gmail 也应该最终完全黑暗。你需要的只是 Android 10 和最新的 Gmail(至少 2019.09.01.268168002 版本)。然而，谷歌倾向于通过服务器端推送逐渐为用户启用新功能(在这种情况下是一个黑暗的主题)，我目前还没有运气。

我很好奇 Gmail 是否会支持`@media prefers-color-scheme`。据我所知，这似乎没有什么希望。我想我们得等着看了。一旦我在 Gmail 中启用了黑暗主题，我就会更新这篇文章。

## 包扎

黑暗模式即将到来的 HTML 电子邮件，我爱它！但是，这是另一件需要担心的事情——就像使用 HTML 表格进行布局是不够的。

[加入我们的邮件列表](https://hosted.sidemail.io/5d919d2fcc34a000fc97cfed)，了解最新的电子邮件黑暗模式。我们还分享了在构建和发展我们的 SaaS 产品 [Sidemail](https://sidemail.io/) 时所面临的见解和挑战。

感谢阅读！