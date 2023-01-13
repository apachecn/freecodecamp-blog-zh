# 如何创建你的第一个 Hugo 博客:实用指南

> 原文：<https://www.freecodecamp.org/news/your-first-hugo-blog-a-practical-guide/>

如果你想开一个博客，Hugo 是一个很好的工具。

我自己用 Hugo 写博客，[flaviocopes.com](https://flaviocopes.com/)，我已经用了两年多了。我有几个喜欢雨果的理由。

首先是**简单**、**枯燥**、**灵活**、**快速**。

主要原因是**简单**。入门不需要学太多东西。

你用 Markdown 写内容，这种格式让我可以用我最喜欢的编辑器(Bear)写帖子。

雨果是**无聊**。不要误会，这是一件非常积极的事情。作为一名开发人员，我总是想在这里或那里调整一些东西。雨果背后没有什么奇特的技术。它是用我最喜欢的语言之一 Go 构建的，但这并不意味着我想深入 Hugo 的内部并改变它的工作方式。

而且它不像许多 JavaScript 框架那样呈现任何酷的或下一代的东西。

因此它很无聊，这给了我很多时间去做在博客上真正有用的事情:**写内容**。我关注的是内容，而不是内容容器。

也就是说，Hugo 非常灵活。我以开源主题开始了自己的博客，然后随着时间的推移彻底改变了它。有时候，我想在我的网站上做一些超出简单博客范围的事情，Hugo 允许我创建这些事情。

最后，我喜欢 Hugo 的另一个原因是它的速度很快。为什么？首先，它的核心是 Go，这是一种速度非常快的语言。在 Go 生态系统中，没有 100 兆字节依赖性的概念。事情被做得越快越好。此外，Hugo 不需要做一些使用尖端技术时需要的尖端工作。这是无聊的副产品。

总之，说够了。

Hugo 很棒，尤其是如果你是一个开发者，并且你愿意用 Markdown 来写的话。非技术人员可能会拒绝使用 Markdown，这完全可以理解。

此外，您必须为以 Git 为中心的工作流做好准备，以使事情真正顺利进行。

这是写博客的过程:

*   用 Markdown 写一篇文章，
*   然后将您的更改提交到 Git 存储库，最常见的是在 GitHub 上，
*   然后，一些粘合技术将更改部署在托管站点的服务器上。

## 主持 Hugo 网站

Hugo 博客是完全静态的。这意味着你不需要托管自己的服务器，或者为它使用特殊的服务。

Netlify、Now 和 GitHub 页面是三个可以免费托管 Hugo 博客的好地方。

唯一的成本是你必须维持的域名。我怎么强调拥有自己域名的重要性都不为过。没有`.github.io`或`.netlify.com`或`.now.sh`的网站，请。

我自己的 Hugo 网站托管在 Netlify 上。

## 选择一个域

把你的博客放在你自己的域名下。选一个。用你自己的名字。并使用`.com`或`.blog`。不要试图通过使用本地化领域来变得聪明——例如，不要使用`.io`。`.com`只是给人一个更好的印象，它可以在你未来的所有项目中重复使用，而不仅仅是托管你的博客。我选了那个。

哦，如果你有一个旧域名，就使用它。为什么？你的域名越老越好。

子域注意:每个子域，对谷歌来说，是一个不同的网站。因此，如果你的域名是`flaviocopes.com`，你在`blog.flaviocopes.com`创建你的博客，那么对谷歌来说，这是一个全新的网站，它将有自己的排名，独立于主域名。

我的建议是完全避免子域。

## 安装 Hugo

要在 macOS 上安装 Hugo，从您的终端运行

```
brew install hugo 
```

**Mac 上没有`brew`命令？查看[自制指南](https://flaviocopes.com/homebrew/)* 。*

对于 Windows 和 Linux，查看[官方安装指南](https://gohugo.io/getting-started/installing/)。

## 创建一个 Hugo 网站

一旦安装了 Hugo，您就可以通过运行

```
hugo new site myblog 
```

我建议您将它运行到主目录中的一个`www`文件夹中，因为该命令将在您运行它的地方创建一个新的`myblog`文件夹。

![Screen%20Shot%202020-01-03%20at%2018.40.28](img/a4b833f98566caccb10b717e71e334fd.png)

## 选择一个主题

在你开始之前，你需要选择一个主题。我希望 Hugo 包含一个默认主题，让事情变得简单明了，但是它没有。

在 [https://themes.gohugo.io](https://themes.gohugo.io/) 上有很多选择。我个人的建议是从 https://themes.gohugo.io/ghostwriter/的[开始，以后再调整。](https://themes.gohugo.io/ghostwriter/)

我还建议你避开他们在页面上建议的`git clone`工作流程。将来你肯定会调整主题，我发现最好是有一个内容和主题的单一存储库。它简化了部署。

所以，去[https://github.com/jbub/ghostwriter/archive/master.zip](https://github.com/jbub/ghostwriter/archive/master.zip)下载当前版本的主题。

然后在您新创建的 Hugo 网站的`themes/ghostwriter`文件夹中将其解包:

![Screen%20Shot%202020-01-03%20at%2019.00.41](img/1f75289761802351c245e4ce20829fc6.png)

注意在`themes/ghostwriter`中有一个`exampleSite`文件夹。打开它，并打开它的`content`子文件夹。在那里，你可以看到`page`、`post`和`project`子文件夹。

![Screen%20Shot%202020-01-03%20at%2019.03.28](img/6fd30bdfdc77e0afc0eff74bce6c6b8c.png)

将`page`和`post`复制到站点的`content`文件夹中；

![Screen%20Shot%202020-01-03%20at%2019.03.39](img/f154375c66611befdb29d9adbd1aae9b.png)

## 配置

示例数据还在`themes/ghostwriter/exampleSite/config.toml`中提供了一个示例`config.toml`文件。这是 Hugo 配置文件，它告诉 Hugo 一些配置的细节，而不需要你在主题中硬编码信息。

我建议你不要复制它，因为它有太多的东西，而是使用这个:

```
baseurl = "/"
title = "My blog"
theme = "ghostwriter"

[Params]
    mainSections = ["post"]
    intro = true
    headline = "My headline"
    description = "My description"
    github = "https://github.com/XXX"
    twitter = "https://twitter.com/XXX"
    email = "XXX@example.com"
    opengraph = true
    shareTwitter = true
    dateFormat = "Mon, Jan 2, 2006"

[Permalinks]
    post = "/:filename/" 
```

您可以在以后自由定制该文件中的信息。

现在，从命令行运行:

```
hugo serve 
```

![Screen%20Shot%202020-01-03%20at%2019.06.43](img/4716ee2cbb5feabe5a402bc234873554.png)

在你的浏览器中打开`http://localhost:1313`，你应该可以看到现场直播！

![Screen%20Shot%202020-01-03%20at%2019.24.20](img/f0d17d7122efb57a151950af0d958f99.png)

这是该网站的主页。

这里有一个从你网站的`content/post`文件夹中提取的帖子列表:

![Screen%20Shot%202020-01-03%20at%2019.08.20](img/bc44f7c4a4ae86c1342fabb231719ef0.png)

单击名为“创建新主题”的第一个主题:

![Screen%20Shot%202020-01-03%20at%2019.24.22](img/115a19d07a0564e4d334d00f28fda87e.png)

您可以打开文件`content/post/creating-a-new-theme.md`来更改帖子中的任何内容。

![Screen%20Shot%202020-01-03%20at%2019.09.47](img/d2561de129838cdb21cb3e25e3208300.png)

如果保存，网站将自动更新新内容。

![Screen%20Shot%202020-01-03%20at%2019.24.29](img/45f91dcd2f59d273fd7082c824c839dd.png)

这太棒了，对吧？

你可以通过创建一个新的`.md`文件来创建一个新的帖子，在它前面加上你想要的前缀。如果您愿意，可以使用递增的号码。或者使用日期。

如果有些东西看起来不像你想要的样子，你可以打开`themes/ghostwriter/layouts`文件夹并调整它。

“发布”模板在`themes/ghostwriter/layouts/post/single.html`中定义:

![Screen%20Shot%202020-01-03%20at%2019.13.23](img/89673f7eb9410a96c7913a9dfec3d55b.png)

Hugo 使用 go 模板。语法可能很陌生，但 Hugo 网站在这个 [Go 模板介绍](https://gohugo.io/templates/introduction/)中做了很好的解释。

然而，现在不要试图定制你的模板。

如果你想调整颜色，在`themes/ghostwriter/layouts/partials/header.html`中添加一个带有 CSS 的`<style>`标签。

例如，将链接设为黑色:

```
<style>
.site-title a, .button-square {
    background: black;
}
a {
    color: black;
}
</style> 
```

而是关注内容。

删除现有文件，开始写 2-3 篇文章。

把事情做得尽善尽美太容易了，但重要的是内容。

你的网站越干净，对你的读者越好。

现在让我写一点关于部署的内容。

## 将 Hugo 站点部署到 Netlify

我想展示如何在我最喜欢的两个服务中部署 Hugo 站点:Netlify 和 Now。

首先，我将创建一个 GitHub 存储库来托管站点。

我打开 GitHub Desktop，这是一款我每天都在使用的应用，也是我工作流程的一部分。这是使用 Git 最简单的方法。

从文件菜单中，我按下了“新建存储库”选项:

![Screen%20Shot%202020-01-03%20at%2019.40.29](img/0ba80f25cfcff246ec20cacaf8ec2761.png)

只需将`myblog`文件夹拖入应用程序，即可生成相同的屏幕。

我给存储库起了`myblog`名，并为回购选择了正确的路径。

该过程自动进行第一次提交:

![Screen%20Shot%202020-01-03%20at%2019.43.06](img/8415653eb9bd549d3dd9ff24ddb08d79.png)

现在，我们可以单击“发布存储库”按钮将 repo 推送到 GitHub:

![Screen%20Shot%202020-01-03%20at%2019.43.47](img/b61a0a5fecae8ccec2700d21d3fdc304.png)

当然，你可以将回购保密。

一旦回购进入 GitHub:

![Screen%20Shot%202020-01-03%20at%2019.44.43](img/d37b16ceaf48031bfb85cf79ddc36cd0.png)

我们可以搬到 Netlify。

在我的 Netlify 仪表板上，我按下了“从 Git 新建网站”按钮:

![Screen%20Shot%202020-01-03%20at%2019.45.40](img/f1346fd43912404d4486c256c59fecbe.png)

我按下 GitHub，授权 Netlify 访问我的私人存储库，然后我选择了我刚刚创建的回购:

![Screen%20Shot%202020-01-03%20at%2019.46.40](img/8f1c562600b864526b09ebb6d19c963b.png)

Netlify 自动将其识别为 Hugo repo，并自动输入构建命令:

![Screen%20Shot%202020-01-03%20at%2019.47.07](img/30f348f946a950f404a2a754dfd27af0.png)

单击“部署站点”启动部署流程:

![Screen%20Shot%202020-01-03%20at%2019.47.53](img/f8f72c95d3eea8b83588fae494a5478e.png)

在一个真实的网站上，我会建立一个自定义域。Netlify 可以选择通过他们购买域名，这是一个非常简单的过程。我强烈推荐。该网站可以在购买域名后几分钟内上线。

一个随机的`.netlify.com`子域被附加到站点，在这个例子中是`pedantic-engelbart-500c9a.netlify.com`，HTTPS 被自动启用。

因此，我们可以立即看到现场直播:

![Screen%20Shot%202020-01-03%20at%2019.52.38](img/a7ffeb9f02db388c0c2c584582a20a83.png)

现在，如果你试图在你的本地版本中编辑一些东西，你只需将更改推送到 GitHub，Netlify 就会自动更新网站。您可以在网站的“概述”面板中看到它正在构建网站:

![Screen%20Shot%202020-01-03%20at%2019.50.39](img/98bfbcfc4c60bd070bd22b7a5818480d.png)

要了解更多关于网络生活的知识，我建议你看看我的[网络生活教程](https://flaviocopes.com/netlify/)。

## 立即将 Hugo 站点部署到 Zeit

另一个很棒的平台是 Zeit Now。

![Screen%20Shot%202020-01-03%20at%2019.54.59](img/783825411cb9dfb21b8f030a19907968.png)

一旦你注册了，在仪表盘上按下**新项目**按钮。

![Screen%20Shot%202020-01-03%20at%2019.58.59](img/3e901ef4ce40ab1063166579bd97f6b7.png)

第一次从 GitHub 部署时，您必须首先通过单击“立即安装 GitHub”来安装 GitHub 应用程序:

![Screen%20Shot%202020-01-03%20at%2019.59.22](img/edecfb2a057762fb102dbdf0f64e1ae2.png)

这将把你带到应用程序的 GitHub 页面，在这里你可以为你的所有回购授权，或者只是为一些:

![Screen%20Shot%202020-01-03%20at%2019.59.58](img/c7ebf68549f79ae89a5d3499b6d5a4be.png)

返回后，点击“来自 GitHub 的新项目”按钮:

![Screen%20Shot%202020-01-03%20at%2020.00.15](img/c33b8555a5f12e7aee07bf675c7e1153.png)

选择项目并点击“导入”:

![Screen%20Shot%202020-01-03%20at%2020.01.41](img/9eb06a83f4581c83e5267d7dc6c1022a.png)

同时，进入`mysite`的主文件夹，添加一个`package.json`文件，内容如下:

```
{
  "scripts": {
    "build": "hugo"
  }
} 
```

这告诉我们现在如何部署站点。

当您回到仪表板时，新的部署应该很快就会开始，您将看到站点正在运行:

![Screen%20Shot%202020-01-03%20at%2020.04.18](img/eb803b10082583cc5c333972628e9781.png)![Screen%20Shot%202020-01-03%20at%2020.05.40](img/b6d78c4cbb8f2dfc312db48154edbc87.png)

请注意，现在您有三个可以用来访问网站的 URL:

*   `myblog.flaviocopes.now.sh`
*   `myblog-alpha-swart.now.sh`
*   `myblog-git-master.flaviocopes.now.sh`

你可以选择你喜欢的那个。

另外，每个部署也有自己的 URL。在这种情况下，我有`myblog-h8xks5jhn.now.sh`，但它会随着每次部署而变化。

当然，你也可以添加你的域名。Zeit 有一项很棒的服务，可以直接从他们那里购买你的域名，可以在[https://zeit.co/domains](https://zeit.co/domains)买到。

如果您喜欢使用命令行，`now`命令也可以让您从那里购买域。

我强烈推荐你查看我的 [Zeit Now 教程](https://flaviocopes.com/zeit-now/)来了解更多关于这个平台的信息。

## 包扎

如果你正打算开一个新博客，希望这篇教程能给你一点指导。Hugo 是我最喜欢的平台，但它当然不是唯一的。Ghost(支持 freeCodeCamp 的平台)也很棒，当然还有 WordPress 和 Gatsby。

挑你最喜欢的。在我看来，平台没有你的内容重要。所以，选一个，开始写吧！