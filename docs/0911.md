# 什么是 iframe？HTML iframe 示例

> 原文：<https://www.freecodecamp.org/news/what-is-an-iframe-html-example/>

iframe 是一个 HTML 文档，嵌入在网站上的另一个 HTML 文档中。把它想象成“网页中的网页”

也许你看过电影《盗梦空间》,它讲述了梦中的梦。或者玩那些俄罗斯嵌套娃娃。这是同一个概念。

![1280px-Matryoshka_transparent](img/bb4351f2e6762069b7cc07dacc685940.png)

[Photo by BrokenSphere (CC BY-SA 3.0)](https://commons.wikimedia.org/w/index.php?curid=3773186)

## HTML iframe 标记示例

iframe HTML 标记用于指定要嵌入的文档的 URL。

Iframes 通常用于在网页上嵌入视频、地图和其他媒体。您还可以使用它们将另一个网页嵌入到一个网页中。下面是一些使用`iframe`嵌入外部资源的代码示例:

```
<iframe src="http://www.example.com/">

<iframe src="http://www.example.com/" width="400" height="300">

<iframe src="http://www.example.com/" style="border: 0;"> 
```

这里有几个在 HTML 中嵌入交互资源的例子。这段代码将嵌入一个 Vimeo 视频播放器:

```
<iframe src="https://player.vimeo.com/video/76979872" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
```

Iframes 在网络早期就已经存在了。开发人员最初使用它们在网页上嵌入外部内容，如 YouTube 上的视频。

也有像 Stumbleupon 这样的“工具栏”类型的网站会在网站顶部添加工具栏。Stumbleupon 将通过使用 iframe 在自己的页面上呈现网站来做到这一点。

## 为什么开发人员大多不再在他们的网站中使用 iframes

如今，开发者仍然使用 iframes 在网页上嵌入媒体和其他内容。但是出于安全考虑，许多 web 开发框架不鼓励这样做。

归根结底，在另一个网页中运行 iframe 并不是理想的用户体验。Iframes 在手机上看起来特别糟糕，破坏了网页设计布局。这就是 iframes 在很大程度上失宠的原因。

我希望这对你有所帮助。如果你想学习更多的编程和技术知识，可以试试 [freeCodeCamp 的核心编码课程](https://www.freecodecamp.org/learn)。它是免费的。