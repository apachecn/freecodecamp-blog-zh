# 我如何为我的苹果收藏创建一个网站

> 原文：<https://www.freecodecamp.org/news/creating-a-website-for-my-apple-collection/>

不久前，我开始收集苹果。我从十几岁就开始关注苹果的硬件(及其美学)，但那时我还没有钱拥有一台 Mac 电脑。

我 19 岁时有了第一台 Mac 电脑。这是一台 iBook 700 Mhz，从巴西一个类似易贝的网站上购得。这笔钱来自一个 Flash 项目。

在加拿大住了几年后，我有一些额外的钱花在一个爱好上。大多数时候，我从 Craigslist 上的人那里购买设备。

在使用了几台笔记本电脑和电子设备后，我决定开始收集关于我的想法的信息。一开始，我创建了一个包含型号、序列号、我如何获得设备、最低/最高操作系统等等的 Gist。

列表越来越大，内容开始变得杂乱。我认为在网站上展示这些内容会很完美，我不需要雇佣一个开发人员:D

首先，我决定将我的数据组织在 SQL 数据库中，信息分布在不同的列和表中。之后，我会创建一个 graphQL API 来为我提供填充 UI 所需的数据——可能用 React 编写，用 Babel 编译，用 Webpack 打包。

大声读前面一段，你可以看到有很多技术，我甚至忽略了后端语言和 UI 细节，如 SASS 或 styled-components。当我的最终目标是在一个漂亮的设计中显示一个项目列表时，这一切听起来有点让人不知所措。

话虽如此，我还是考虑了如何在没有以下内容的情况下提供这些内容:

*   API 或任何后端工作
*   任何 JS 框架/库
*   任何 JS 工具(Webpack，Babel 等。)
*   任何 CSS 作品

除了这些限制，我还有几个延伸目标:

*   创建一个易于访问的网站
*   创建一个可以在旧浏览器上运行的网站，因为我的电脑运行的是 Mac OS 9.2，我的设备运行的是 iOS 3

接受挑战。一个 index.html，几个普通的 JS 文件，没有定制的 CSS。我想和你分享一下建设网站的经验。

TL，DR:

*   [最终网站](https://bit.ly/collection-website)
*   [源代码](https://bit.ly/collection-source)

让我们逐点讨论这些限制:

## 没有 API 或任何后端工作

前阵子我看到一个 SaaS 的产品叫做 [Stein](https://steinhq.com/) 。你在 Google Sheets 文档中创建数据，他们会给你一个数据端点。他们的库像把手一样工作，看起来非常适合我的用例:

```
<div data-stein-url="https://api.steinhq.com/v1/storages/5cc158079ec99a2f484dcb40/Sheet1" data-stein-limit="2">
  <div>
    <h1>{{title}}</h1>
    <h6>By {{author}}</h6>

    {{content}}

    Read on <a href="{{link}}">Medium</a>
  </div>
</div> 
```

## 没有 JS 框架/库和工具

我决定避免在这个项目中添加框架或库，因为用例不需要。这个页面上的所有 JS 交互都非常简单(显示/隐藏菜单，打开模态屏幕，处理永久链接)。

因为我没有使用框架/库，所以我可以避免添加 Webpack 和 Babel。无需钻研预置和加载器。

P.S .你可以争辩说我本可以选择 create-react-app 或者 Next.js 来解决所有这些问题，但是没有。

## 没有 CSS 工作

我喜欢写 CSS，尤其是当我可以使用 SASS 时，但是我决定不在这里写任何 CSS。我有几个很好的理由来避免这样做:

*   我心中没有任何设计，尽管事实上我可以做一些看起来不错的东西，但我不想投入时间和精力
*   我想使用[顺风 CSS](https://tailwindcss.com)

如果你从来没有听说过 Tailwind CSS，请不要只是认为，“它只是 Bootstrap 的一个替代品。”下面是他们网站上一个很好的简短解释:

> 大多数 CSS 框架做的太多了。与固执己见的预设计组件不同，Tailwind 提供了底层实用程序类，让您无需离开 HTML 就能构建完全定制的设计。

这几乎是真的。快速搜索会给你许多用 Tailwind CSS“重建”的网络应用:

*   [Whatsapp](https://tailwindcomponents.com/component/whatsapp-web-clone)
*   [电报](https://tailwindcomponents.com/component/telegram-desktop-using-tailwindcss)
*   [脸书](https://tailwindcomponents.com/component/facebook-clone)
*   [Reddit](https://tailwindcomponents.com/component/reddit-clone)
*   [Youtube](https://tailwindcomponents.com/component/youtube-clone)
*   [松弛](https://tailwindcomponents.com/component/slack-clone-1)
*   [比特币基地](https://tailwindcomponents.com/component/coinbase-clone)
*   [Github](https://tailwindcomponents.com/component/github-profile-clone)
*   特雷罗
*   [推特](https://codepen.io/drehimself/full/vpeVMx/)
*   [Netlify](https://www.youtube.com/watch?v=_JhTaENzfZQ)

## 创建一个易于访问的网站

上个月，我开始在德克大学学习无障碍课程。他们的内容很棒，这让我想起了默认情况下可以访问 **HTML。通过使用语义 HTML 结构和测试基本的东西，如键盘导航和颜色对比，你可以消除一些障碍，使残疾人远离你的内容。**

我不是无障碍专家，但这里有一些我为这个网站做的与无障碍相关的事情:

*   禁用样式表:通过禁用样式表，您可以确保您的内容遵循逻辑/结构方式。
*   VoiceOver: VoiceOver 包含在 macOS 和 iOS 中。使用非常简单，通过试验，你可以更好地了解人们如何使用这个特性。
*   情态动词:情态动词可能有问题。我决定遵循 Ire Aderinokun 的方法。
*   axe :该扩展是 WCAG 新协议和 Section 508 可访问性规则的可访问性检查器。

它并不完美——有一些东西我没有在我的网站上做，比如在主要内容上添加一个跳转链接。如果您很好奇，[这里是包含所有更改](https://github.com/leonardofaria/collection/pull/1)的拉请求。

## 创建一个可以在旧浏览器上运行的网站

我无法实现这个目标，因为我无法控制剧本和风格。不过，似乎也不是不可能。我注意到一些事情:

*   [Expedite](https://github.com/SteinHQ/Expedite) (Stein 客户端)使用 [fetch](https://github.com/SteinHQ/Expedite/blob/master/index.js#L51-L54) ，这是 Safari 10 中唯一增加的[。对他们服务器的请求可能会被 XMLHttpRequest 替换。](https://caniuse.com/#feat=fetch)
*   Tailwind 在很多元素中使用了 Flexbox。Safari 在 iOS 7 才开始支持 Flexbox。也许我可以为它们现有的元素编写一些属性来获得一个像样的外观。
*   SSL 证书可能是旧浏览器的一个问题。

## 结论

制作这个网站非常有趣。拥有这种宠物项目给了我一个很好的理由去使用我在工作中不使用的技术。也许在未来，Stein 和/或 TailwindCSS 将有助于构建一个功能原型或构建一个黑客马拉松项目。

事实上，我在我的项目中添加了“约束条件”,这让我跳出框框思考。尽管我没有实现所有的目标，但它帮助我越来越明白所有的部分是如何联系在一起的。

我完全推荐这样做，让你有机会尝试不同的技术。它不一定是苹果收藏——你可以创建一个网站，列出你最喜欢的书籍或你做过的最好的徒步旅行。在这种情况下，过程比目标更重要。

出于好奇，我用 [Clockify](https://clockify.me) 记录了我的时间，在编码、创建数据、测试和写这篇文章之间，我已经工作了 13 个小时。

*也发表在[我的博客](http://bit.ly/collection-post)上。关注我的[推特](https://twitter.com/leozera)*