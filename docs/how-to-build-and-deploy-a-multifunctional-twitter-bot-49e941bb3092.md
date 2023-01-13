# 如何构建和部署多功能 Twitter 机器人

> 原文：<https://www.freecodecamp.org/news/how-to-build-and-deploy-a-multifunctional-twitter-bot-49e941bb3092/>

> ******更新 20190507:****** *这个教程可能不再相关了，因为 Twitter 贬低了 API 的部分，这个会越来越不相关。我不会再更新这个了。？*
> 
> ***更新 20171105:*** *为了便于导航，我已经将这个故事的所有内容编译成了一个 [GitBook](https://spences10.gitbooks.io/twitter-bot-playground/content/) 它几乎是这个故事的精确表示，但会随着对 [GitHub](https://github.com/spences10/twitter-bot-playground/) 库所做的任何更改而保持更新。谢了。*

我又忙着造推特机器人了！

如果你看一下我的 [GitHub 简介](https://github.com/spences10?utf8=%E2%9C%93&tab=repositories&q=twitt&type=source&language=javascript)，你会发现我有很多关于 Twitter 机器人的回复。

我最近的项目是从决定将我的一个测试回购重新定位为如何使用 npm `twit`包的文档开始的。但是当我添加新的例子时，它很快就变成了另一个 Twitter 机器人。

这个机器人是由三个例子拼凑而成的，我们将在这里讨论。我还将详细介绍我如何使用 Zeit 的`now`平台将机器人部署到服务器上。

特别感谢[蒂姆](https://twitter.com/timneutkens)帮助我完成`now`部署。到[汉娜·戴维斯](https://twitter.com/ahandvanish)领取 [egghead.io](https://egghead.io/courses/create-your-own-twitter-bots) 课程资料。它有一些非常简洁的例子，我已经在相关章节中链接了这些例子。

### 开始

这篇文章旨在为我和其他对使用`Node.js`的 JavaScript Twitter 机器人感兴趣的人提供参考。注意，这里所有的例子都使用了 [npm](https://www.npmjs.com/) 包 [twit](https://www.npmjs.com/package/twit) 。

Bot 示例 1:用 NASA 每日图片发布媒体

> 木卫三:最大的卫星[pic.twitter.com/6ir3tp1lRM](https://t.co/6ir3tp1lRM)
> 
> — Botland Mc Bot ?‍?? (@DroidScott) [May 14, 2017](https://twitter.com/DroidScott/status/863823681788817408?ref_src=twsrc%5Etfw)