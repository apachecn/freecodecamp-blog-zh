# 动画如何在 React Native 中工作

> 原文：<https://www.freecodecamp.org/news/how-react-native-animations-work/>

React Native 是一个很棒的框架，可以让你创建跨平台的移动应用。如果您是一名 web 开发人员，并且想要一个快速、低成本的解决方案来开发可以在 Android 和 iOS 上运行的原生移动应用程序，这将非常有帮助。额外收获:你不必花费$$$在独立的 iOS、Android 和 web 团队上。

这不仅降低了你的成本，还允许你在所有三个版本(Android、iOS 和 Web)之间共享你的代码库，从而更容易做出改变和推出更新。

React Native 还允许您非常接近实际操作系统地工作，这对于动画等性能关键型任务非常重要。

动画是任何应用程序不可或缺的一部分，因为它们使程序具有交互性，对最终用户更有吸引力。但是很多时候，在处理动画的时候，你可能会觉得你在处理一个黑盒。

因此，让我们了解一下 React Native 中的动画是如何工作的。你可以从[这个免费的 YouTube 播放列表](https://www.youtube.com/playlist?list=PLYxzS__5yYQmdfEyKDrlG5E0F0u7_iIUo)开始学习 React Native 中的动画。

# 动画 API

React Native 公开了一个名为 Animated 的 API。它由许多奇妙的东西组成，如动画值、弹簧/定时动画和事件。但是我们不在这里讨论 API——我会把它留给官方文档和我的 T2 YouTube 播放列表。他们在这方面做得更好。

事实上，我们想在这里讨论的是 React Native 如何在屏幕上制作动画，以及幕后发生了什么。

# 制作动画的两种方式

你应该从我的另一篇文章中知道[如何反应本地作品。(如果你还没有，请快速阅读。)由于 React Native 使用 React 和 JavaScript，因此屏幕上的动画有两种精确的实现方式。](https://www.freecodecamp.org/news/how-react-native-constructs-app-layouts-and-how-fabric-is-about-to-change-it-dd4cb510d055/)

首先让我们明确一个事实。React Native 在屏幕上构建实际的本地视图，而不是像 Ionic 那样通过嵌入式 web 浏览器呈现的视图。因此，如果您想以任何方式激活视图，最终都必须在本地视图上完成。

JavaScript 必须以某种方式与操作系统通信，以便更新视图。JavaScript 运行在与 UI 线程(主线程)不同的线程中，只有这个 UI 线程可以更新视图。因此 JS 需要使用 React Native 提供的桥来序列化数据并将其传递给操作系统。

## 在 JS 中工作并更新本地视图

这种方法意味着您获取一个已经在用户屏幕上可见的视图，并处理它在 JavaScript 线程中的下一个位置需要做的事情。步骤大致如下:

1.  动画开始
2.  JavaScript 运行`requestAnimationFrame`函数——一个**尝试**以 60 次调用/秒(60 FPS)运行的函数
3.  JavaScript 计算下一个位置/不透明度/变换/你在视图上动画的任何东西。
4.  JavaScript 序列化这个值并通过桥发送它。
5.  在 bridge 的另一端，Java (Android)或 Objective C (iOS)反序列化这个值，并在提到的视图上应用给定的转换。
6.  屏幕上的框架被更新。

你看到那里发生了什么吗？没有一个步骤实际上重新呈现 React 本机组件。这意味着动画 API 实际上“绕过”了 React 不改变“状态”变量的哲学。

好消息是:这实际上对动画很有帮助，因为如果我们让 React 每秒 60 次重新渲染一个组件，速度会太慢，对性能也不好！

这一切都很好，但这里有一个非常基本的问题。JavaScript 是单线程的。所以 JavaScript 的异步特性在这里不起作用，因为动画是一个 CPU 绑定的任务。让我们来看看这种方法的利弊。

### 赞成的意见

1.  你可以用 JS 编写非常复杂的动画，并且可以作为本地动画看到。
2.  对动画状态的更多控制

### 骗局

1.  如果您的 JavaScript 线程非常繁忙，将会带来巨大的性能损失。
2.  如果网桥繁忙，当 OS/JS 想要相互通信时，性能会下降。

老实说，这个骗局是个大骗局。这篇文章后面有一个视频可以实时向你展示这个问题。您将看到当 JavaScript 线程变得繁忙时，JS 是如何在动画上浪费时间的。

### JS 动画为什么会滞后？

在 JS 中完成的动画会在动画进行的时候开始滞后，并且用户(或应用程序)请求一些必须由 JS 线程处理的其他动作。

例如，想象有一个动画正在发生。这意味着 JS 正忙于运行`requestAnimationFrame`函数。假设更新本身不太重，假设用户开始点击屏幕上的一个按钮，使计数器递增。

现在随着`requestAnimationFrame`被频繁调用，你的 React 虚拟树也在一次又一次地重新渲染，以解决计数器增加的问题。

它们都是在单线程上运行的 CPU 绑定任务，因此会有性能影响。`requestAnimationFrame`将开始丢帧，因为 JS 线程正在做额外的工作。

所有这些意味着你会得到一个非常不稳定的动画。

## 本地驱动程序动画

不用担心，因为 React Native 实际上也允许你在本地驱动上运行动画！你可能会问，我这么说是什么意思？

简单地说，这意味着 React Native 将动画工作从 JS 线程卸载到 UI 线程(操作系统)，并让它处理对象的动画。这有几个好处:

1.  JS 线程(和 React Native bridge)现在可以自由处理其他密集型任务，比如用户的重复点击。
2.  动画流畅得多，因为没有序列化/反序列化和桥接通信开销。

React Native 是如何做到这一点的？React Native 允许开发人员在构建动画对象时提供一个名为`useNativeDriver`的属性作为布尔值。

当设置为 true 时，React Native，在开始动画之前，序列化整个动画状态以及将来需要做的事情。然后，它通过网桥将它传输到操作系统。

从那时起，Java (Android)或 Objective C (iOS)代码运行动画，而不是 JavaScript 计算下一个动画帧并通过桥一次又一次地发送数据。

这大大加快了动画的速度，动画运行更加流畅，尤其是在低端设备上。让我们来看看 React Native 中本地动画与基于 JS 的动画的视频演示:

[https://www.youtube.com/embed/lsRf_PspjSs?feature=oembed](https://www.youtube.com/embed/lsRf_PspjSs?feature=oembed)

在这种情况下，步骤大致如下:

1.  动画开始
2.  JS 序列化动画信息并通过桥发送它。
3.  另一方面，操作系统接收这些信息并本地运行动画。

就是这样！现在 JS 线程的动画要轻得多。不再无休止地奔跑。

然而，这种方法有它自己的利弊。

### 优点:

1.  更快的动画
2.  非阻塞 JS 线程
3.  不那么臃肿的桥

### 缺点:

1.  对动画的控制更少(一旦“自动”动画开始，JS 就看不到屏幕上发生了什么)
2.  需要操作的属性更少——本地驱动程序不支持所有需要动画的属性。例如，`width`或`height`本身是不可动画化的，但是`opacity`和`transform`可以。

在很多情况下，你会发现你可以使用`useNativeDriver: true`来创建一个类似的效果，而不设置`useNativeDriver: false`是无法实现的。

正如我们所说的，React Native 团队正在努力增加对更多属性的支持，但就目前而言，我认为它目前工作得很好。

## 结论

本文向您展示了 React 本地动画实际上是如何工作的，以及它们为什么如此美丽。

你觉得这篇文章怎么样？请通过我的 [Instagram](https://instagram.com/mehulmpt) 和 [Twitter](https://twitter.com/mehulmpt) 账户联系我来告诉我！新来的反应原生？开始在 [codedamn](https://codedamn.com) 上学习吧——一个开发者学习和交流的平台！

和平！