# 从 WordPress 到 Hugo——我如何迁移一个 250 多页的网站以及我使用的脚本

> 原文：<https://www.freecodecamp.org/news/wordpress-to-hugo-scripts/>

我最近决定把 Boot.dev 的博客从 WordPress 换成静态网站生成器。

我决定选择 Hugo，部分原因是因为我是 go 编程语言的超级粉丝，但也因为从开发体验的角度来看，这似乎是最好的选择。

以下是我决定离开 WordPress 的原因:

*   我想在 Markdown 中写并存储我所有的帖子。
*   我想对我在 Git/Github 中的所有帖子进行版本控制。
*   我希望能够使用 ctrl+shift+f 来查找和编辑我的博客上的内容。
*   我想要一种不那么容易出错的方式来处理“全局块”
*   我不想花几个小时来微调性能。我的帖子只是文字和图片，所以页面速度分数默认应该是 100！
*   我想使用直接的 CSS 样式，使我的博客和应用程序更容易拥有相同的样式。
*   我想免费托管这个网站

考虑到所有这些目标，雨果是一个完美的选择。自从做出改变后，我一直非常开心，但是在这个过程中也有一些小问题。

WordPress 会为你处理一些 SEO 调整的细微差别，如果你忘记自己重做，你最终会牺牲排名。

## 如何将 WordPress 文章导出为 Markdown

从 WordPress GUI 中手动复制粘贴你所有的帖子到 Markdown 文件中可能会花费*很长的时间。尤其是如果你像我一样有数百个帖子。*

为了加速这个过程，我使用了[这个 markdown 插件](https://wordpress.org/plugins/wp-gatsby-markdown-exporter/)将我所有的帖子一次导出为 Markdown。它被称为“盖茨比削价出口商”，但对雨果来说同样有效。

我不打算复习和 Hugo 一起跑步的基础知识，因为我假设你已经很熟悉了。但是如果你不是，你可以在这里阅读[快速入门指南](https://gohugo.io/getting-started/quick-start/)。

## 如何使用 Hugo 内置的搜索引擎优化模板

WordPress 和它的各种 SEO 插件为你处理了许多与 SEO 相关的骗局。所以当你转向静态站点生成器时，你经常需要自己做一些事情。

事实证明，Hugo 对此有一些交钥匙解决方案，所以使用正确的[内部模板](https://gohugo.io/templates/internal/)可以让您获得开箱即用的所有 open-graph、schema 和 Twitter 元标签。

对我来说，这意味着简单地将这 3 行代码添加到我的`layouts/partials/header.html`文件的`<head>`标签中:

```
{{ template "_internal/opengraph.html" . }}
{{ template "_internal/twitter_cards.html" . }}
{{ template "_internal/schema.html" . }}
```

现在我们将进入我写的脚本来帮助我完成这个过程。

## 脚本 1 —检查断开的链接

你可以通过为各种任务安装插件来解决很多 WordPress 的问题。好吧，假设那些插件不会破坏你的整个 WP 安装。

对于 Hugo，我只需编写一些文本操作或 HTTP 脚本来完成我的命令。

第一个脚本只是一个简单的迭代器，它爬行站点并报告任何断开的链接。你可以在这里看到完整的源代码。这是 Go 团队用来检查 Go 编程语言网站上断开的链接的脚本的小编辑。

## 脚本 2 —缩小图像

当使用 WordPress 时，你通常会上传一张博文图片，一些 SEO 插件会为你优化图片的大小。

在 Hugo 中，不会为你做这样的优化。它只是提供您添加到“静态”文件夹中的精确图像。

我写了一个小脚本来优化我的图像。它执行以下操作:

*   接受几乎任何类型的输入图像(。png，。jpeg，。gif 等)。
*   将其转换为`.webp`格式(一种性能格式)。
*   如果图像太大，将图像缩小到我配置的最大图像尺寸。

这个脚本是用 Node.js 和[编写的，源代码可以在这里找到](https://github.com/bootdotdev/blog/blob/main/scripts/image-min.js)。

## 脚本 3 —管理全局短代码

WordPress 有一个功能叫做“全局块”。它们对于出现在你文章中间的时事通讯注册框非常有用，但是你希望能够在一个地方更新。

在 Hugo 中，你可以使用[短码](https://gohugo.io/content-management/shortcodes/)来达到类似的目的。

不幸的是，插入和移除短代码*本身*实际上在规模上变得很繁琐。

例如，假设我在所有文章的第 5 段后添加了简讯注册短代码，但现在我希望它在第 7 段后。我必须手动复制/粘贴短代码！

嗯，不再是了。再说一次，这是以文本格式存储所有博客文章的一大好处——我们可以编写一些简单的解决方案。我写了两个脚本，它们的源代码可以在这里找到:

*   [rmshorts](https://github.com/bootdotdev/blog/blob/main/scripts/rmshorts/main.go)
*   [add short](https://github.com/bootdotdev/blog/blob/main/scripts/addshorts/main.go)的缩写

这两个脚本允许我轻松地移动我的全局块。例如，要从博客中删除`myshortcode`的所有实例:

```
rmshorts myshortcode
```

然后将`myshortcode`作为自己的段落添加到网站上每篇博文的第二部分之后:

```
addshorts myshortcode 2
```

这大大节省了时间。

## 脚本 4 —转换。docx 到 Markdown

最后但同样重要的是，我和一些非技术作家一起工作。我的作家不习惯写降价。谷歌文档是他们的首选工具。

我对此没有问题，因为我希望它们是有效的。我的解决方案是写一个[小 bash 脚本](https://github.com/bootdotdev/blog/blob/main/scripts/docxmd.sh)，它使用 [pandoc](https://pandoc.org/) 将 Google 文档转换成 Markdown 文件。

## 包扎

我希望这些脚本中的一些对您有价值，我真的不推荐您转而使用静态站点生成器。如果你愿意花几个小时为你的博客建立一个良好的工作环境，从长远来看，这会为你节省很多时间和金钱。

记住，总是自动化那些无聊的东西！