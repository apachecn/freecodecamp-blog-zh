# Android 开发中要避免的关键错误

> 原文：<https://www.freecodecamp.org/news/first-5-mistakes-to-avoid-in-android-development-51177007a4f6/>

作者:瓦伦·巴拉德

# Android 开发中要避免的关键错误

![TfphkrI2K5lzYzd5L7akveQXCS0zBufua3uF](img/b9ad4d2b4d3f6cfd3cf426f3c77df0b0.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正如不同领域的许多先驱和领导者所解释的那样:

> 在任何努力中，重要的是要知道哪些是最需要正确完成的事情。但是，同样重要的是，如果不是更重要的话，知道人们应该不惜一切代价避免的前几件事。

到目前为止，我的帖子都是关于如何在 Android 上执行特定任务的。注意到上面的说法，今天我将写下我认为 Android 开发者应该避免的前五个错误。

#### 没有将所有要显示的字符串放入`strings.xml`

这提供了很差的国际化体验，因为您必须根据用户的语言环境设计自己的方式来显示消息的正确版本。

如果消息在 strings.xml 中，那么它们可以很容易地被翻译并集成到应用程序中。然后，Android 操作系统会根据用户在设备上设置的语言环境无缝地处理要使用的字符串资源。

以下是用户给出的一些不使用字符串资源的理由:

*   **需要上下文来访问:**如果您希望在 UI 中显示该字符串，那么您将不可避免地需要/拥有某种上下文。只需使用相同的上下文来获取字符串。
*   但是我只在这个地方需要它:不知道明天什么时候你可能需要在其他文件中有相同的字符串。最好多投资一分钟，为将来的问题提供保障
*   **运行时数据的复杂字符串:**朋友们，Android 已经覆盖了你们。平台支持参数化字符串，其语法类似于 Java 的`String.format()`中使用的语法。不仅如此，还支持复数字符串(根据数量使用不同的字符串)。查看[这个 StackOverflow 帖子](https://stackoverflow.com/questions/2397613/are-parameters-in-strings-xml-possible)中的参数化字符串和[官方文档](https://developer.android.com/guide/topics/resources/string-resource.html)中的复数字符串。

#### 不使用数据绑定

谁喜欢编写繁琐的`findViewById`调用，然后在当前名称空间中维护对这些视图的引用？此外，在这种情况下，我们需要保留我们的视图 id，这样我们就可以确定在`findViewById`中使用的是哪个视图 id。这是因为 Android Studio 中的自动完成功能会提示每个 id(来自所有布局)，但只有那些出现在当前布局树中的 id 对`findViewById`可用。不存在的会返回`null`(大概造成一个`NullPointerException`)。

Google 使得将数据绑定集成到任何应用程序(新的/现有的)变得非常容易，并且消除了所有那些讨厌的样板视图引用的东西。

使用数据绑定(相对于不使用它)的一些好处是:

*   仅引用当前可用的视图(在 AS 中编辑文件时，尝试引用不存在的组件将显示错误。它还会抛出一个编译时错误，而不是在运行时咬你。).
*   由于它只需要遍历整个布局树一次，而不是每次调用`findViewById`时都要遍历，所以速度稍快。
*   您的工作名称空间(类/函数)保持干净，您不必保存对所有视图的引用。
*   你可以在数据绑定中使用尽可能少的功能，就像使用它来消除对更高级功能的`findViewById`调用一样(就像在[这篇文章](https://medium.com/google-developers/android-data-binding-recyclerview-db7c40d9f0e4)中，Google 的 George Mount 试图为一个应用程序中的所有回收器视图编写一个适配器)。

#### 不隐藏 API 键

这是一个常见的问题，它与领域无关，主要由几乎所有领域的初级开发人员造成。一旦您将某段代码提交给版本控制，它将永远保留在那里。即使您在以后的提交中删除了这个 API 键，任何能够访问这个存储库的人都可以从它的历史中查看这个键，所有的问题都会随之而来。

你可以看看这篇文章中的[来弄清楚如何从你的库中隐藏你的 API 键，同时仍然将它们包含在构建过程中，并使它们在你的代码中可用。](https://medium.com/code-better/hiding-api-keys-from-your-android-repository-b23f5598b906)

#### 不考虑活动的生命周期

任何类型的配置更改都会导致当前活动被销毁并重新创建。为了确保用户的过渡是无缝的，我们需要存储应用程序在配置更改之前的状态。然后，在配置更改后重新创建活动后，我们可以按照用户期望的状态重新创建它。

在这个问题上，当我们当前的活动进入停止状态时，我们还应该存储应用程序的状态。之后，我们的应用程序可能会根据系统对资源的需求而被终止。

#### 没有学习 Android Studio 中的键盘快捷键

这可能不会反映在您编写的代码中，但它会极大地影响您的整个工作流程。Android Studio 建立在 IntelliJ Idea(一种以键盘友好而闻名的 IDE)之上。这意味着，只需花一点时间学习不同的键盘快捷键，就可以大大提高开发人员的工作效率。这里有一些我最喜欢的资源可以帮助你:

*   这是一个 IntelliJ 插件(在 AS 中可用),它会显示一个巨大丑陋的对话框，显示你刚刚执行的动作的快捷命令，无论何时你使用鼠标做某事。相信我，这一条会让你烦透了，并迫使你去学习那些捷径。你可以在 Android Studio 设置的插件部分找到并下载。
*   这是 Jetbrains(IntelliJ 背后的公司)为键盘快捷键设计的官方可打印的备忘单。Windows 和 Mac 的版本都有。
*   **官方指南** — [这是 Jetbrains 提供的在 IntelliJ 平台上掌握键盘快捷键的官方指南](https://www.jetbrains.com/help/idea/mastering-intellij-idea-keyboard-shortcuts.html)。
*   也来看看[这](https://www.youtube.com/watch?v=hdrAlhRI5vM) [两个](https://www.youtube.com/watch?v=eOV2owswDkE)视频

#### 这是所有的乡亲

这是我认为任何从事 Android 开发的人应该首先关注的五件事。如果你对这些或天空下的任何其他话题有任何其他建议，请通过 Twitter 联系我，电话: [@varun_barad](https://twitter.com/varun_barad) 。