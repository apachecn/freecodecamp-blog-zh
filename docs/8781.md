# 记住一个 Facepalm:我没有先测试就升级了 SDK 的版本。

> 原文：<https://www.freecodecamp.org/news/a-facepalm-to-remember-i-bumped-up-the-version-of-an-sdk-without-testing-it-first-acb16da33d41/>

作者拉胡尔·乔杜里

# 记住一个 Facepalm:我没有先测试就升级了 SDK 的版本。

![1*-hj6FOiWHQDYIWJgCmbeZQ](img/6c4e2f23a9b75c5ec2e65e1ae5b02362.png)

Image credit: [Valeriy Khan](https://unsplash.com/@valeriydmi)

这一切都始于谷歌在 Android 平台上向开发者开放其应用快捷方式 API。我非常兴奋地将它添加到我的散列标签 Android 应用程序中，[放大](https://play.google.com/store/apps/details?id=com.upcurve.magnify)。所以我开始在[他们的文档](https://developer.android.com/guide/topics/ui/shortcuts.html)中寻找实现它的步骤。

我注意到的第一件事是 Android 所需的 API 版本比我正在使用的版本高。“没什么大不了的，”我想。我把它升级到了新版本。

### “错误”

升级我的 SDK 版本不是一个错误。错误在于，我没有在最新版本的 Android 上广泛测试这款应用。我只是添加了快捷方式功能，对其进行了彻底的测试，然后推出了我的更改——并没有意识到这个新版本的 SDK 已经进行了重大的 API 更改。

Android Nougat 强制检查系统中应用程序间共享的文件范围。如果你试图使用传统的`file://`方法与一些外部应用程序共享保存在你的应用程序范围内的文件，你的应用程序将会遇到`FileUriExposedException`，导致你的应用程序出现丑陋的崩溃对话框，这是任何开发人员——当然也没有任何用户——都不想看到的。

如何修复这个异常超出了本文的范围。相反，让我分享一下这个愚蠢的错误是如何影响我的应用程序的。

### “问题”

以前，当我针对 Android 棉花糖用户时，我的应用程序在牛轧糖上运行时，总是设法通过那个被称为“兼容模式”的隐藏门溜出去。所以当我知道我的应用在最新版本的操作系统上运行良好时，我完全放松了。

```
android {     defaultConfig {         minSdkVersion 18         targetSdkVersion 23 //Targeting Marshmallow    }}
```

但是现在，对于我可怜的小应用程序来说，事情略有不同。由于它说它针对的是最新版本的 Android，该操作系统认为它已经对所有新的 API 更新进行了良好的测试，因此应该对任何违规行为进行惩罚。在我的情况下，这是`FileUriExposedException`，因为我使用传统的`file://`方法分享照片，而不是升级到一个安全可靠的解决方案。

```
android {     defaultConfig {         minSdkVersion 18         targetSdkVersion 25 //Targeting Nougat 7.1    }}
```

终极惩罚？"*不幸的是，Magnify 已经停止工作。*

### “更大的问题”

虽然崩溃本身是一个严重的问题，但我还没有发现一个更大的问题。由于当时只有大约 0.6%的安卓手机用户可以使用安卓牛轧糖，而使用我的应用程序的人只有大约 2%到 3%，这是一次可能隐藏了几周的崩溃。

幸运的是，我的一个应用程序用户有一个运行牛轧糖的谷歌 Pixel，正是她让我注意到这个应用程序坏了。我修补了它，并推出了另一个更新，修复了这个崩溃，谢天谢地，大多数用户都不知道，因为我在一两天内就被通知了这个问题。

唷！真的真的很近。

### 我是怎么解决的？

是的，是的，我说过我不会深入解决这个问题，但我很难看到一个开发伙伴为我遇到的相同问题而挣扎，因为我知道我可以帮助他们，给他们的生活增添一些快乐的时光。

我是这样做的:

[**file:// scheme 现在不允许附带意向 targetSdkVersion 24(安卓牛轧糖……**](https://inthecheesefactory.com/blog/how-to-share-access-to-file-with-fileprovider-on-android-nougat/en)
[*)安卓牛轧糖几乎是被公开发布。作为一名 Android 开发者，我们需要做好调整的准备……*inthecheesefactory.com](https://inthecheesefactory.com/blog/how-to-share-access-to-file-with-fileprovider-on-android-nougat/en)

### 这个故事的寓意

永远不要——我重复一遍，永远不要——在升级 SDK 版本之前，不经过非常、非常广泛的测试就推出软件更新。有可能存在一些您没有意识到的 API 变化——其中一些可能会永久性地破坏您的软件。

确保您仅在经过适当的测试后发布更新。花一点时间进行测试可以节省很多时间来重新赢得用户的信任。

哦，还有:

> 没有错误，除了一个:没有从错误中吸取教训。—罗伯特·弗里普

因为你不会在一篇毒品文章的结尾没有一句精彩的引言。？✌️

如果你喜欢这个故事，请点击？按钮，关注我了解更多关于编程的故事。