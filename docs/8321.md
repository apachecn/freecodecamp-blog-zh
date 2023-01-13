# 通过删除类名和使用范围隔离，将 CSS 包的大小减少了 70%

> 原文：<https://www.freecodecamp.org/news/reducing-css-bundle-size-70-by-cutting-the-class-names-and-using-scope-isolation-625440de600b/>

by Gajus Kuizinas

# 通过删除类名和使用范围隔离，将 CSS 包的大小减少了 70%

#### 就像谷歌做的那样

今年年初，我已经放弃了咨询工作，开始着手打造[go 2 cinema](https://go2cinema.com/)——一种在英国快速、简单、安全的电影票预订方式。我在使*变得快速、简单和安全方面做了出色的工作。在这个过程中的某个地方，我痴迷于关键的渲染路径优化⚡️.*

我已经使用[sus](https://github.com/gajus/usus)解决了 HTML 的预渲染。sus 呈现单页应用程序(SPA)的 HTML，而[内嵌用于呈现页面](https://medium.com/@gajus/pre-rendering-spa-for-seo-and-improved-perceived-page-loading-speed-47075aa16d24)的 CSS。然而，我不喜欢将 70 KB 内联到每个 HTML 文档中，尤其是。其中大部分是 CSS 类名。

#### 就像谷歌做的那样

你有没有偷看过[https://www.google.com/](https://www.google.com/)的源代码？您将注意到的第一件事是 CSS 类名不超过几个字符长。

![1*mGuDYFM56iyLi1MgZPC8bw](img/aaccb07d80d289694fe2130da6c0b6a9.png)

google.com HTML

但是怎么做呢？

#### CSS 缩小程序的缺点

有一件事迷你化器不能做——改变选择器名称。这是因为 CSS minifier 不控制 HTML 输出。同时，CSS 名称可能会变得很长。

如果你使用 CSS 模块，你的 CSS 模块可能会包含样式表文件名，本地标识符名和一个随机散列。类名模板使用 [css-loader `localIdentName`](https://github.com/webpack-contrib/css-loader) 配置描述，例如`[name]___[local]___[hash:base64:5]`。因此，一个生成的类名看起来会像这样`.MovieView___movie-title___yvKVV`；如果你喜欢描述性的名字，它可以变得更长，例如`.MovieView___movie-description-with-summary-paragraph___yvKVV`。

#### 在编译时重命名 CSS 类名

但是，如果你用的是[的 webpack](https://webpack.js.org/) 和[的 babel-plugin-react-CSS-modules](https://github.com/gajus/babel-plugin-react-css-modules)，那你就走运了？–您可以在编译时使用 c [ss-loader g `etLocalIdent`](https://github.com/webpack-contrib/css-loader) 配置和等效的 babel-plugin-react-CSS-modules g`[enerateScopedName](https://github.com/gajus/babel-plugin-react-css-modules#configuration)` 配置来重命名类名。

关于`generateScopedName`很酷的一点是，函数的同一个实例可以用于 Babel 和 webpack 构建过程:

#### 使名字简短

由于`babel-plugin-react-css-modules`和`css-loader`共享相同的逻辑来生成 CSS 类名，我们可以随意更改类名，甚至是随机散列。然而，我想要尽可能短的类名，而不是随机散列。

为了生成最短的类名，我创建了类名索引，并使用`[incstr](https://github.com/grabantot/incstr)`模块为索引中的每个条目生成增量 id。

这保证了简短和唯一的类名。现在，我们的班级名字不再是`.MovieView___movie-title___yvKVV`和`.MovieView___movie-description-with-summary-paragraph___yvKVV`，而是变成了`.a_a`、`.b_a`等。

这使得 [GO2CINEMA](https://go2cinema.com/) CSS 包的大小从 140 KB 减少到 53KB。

#### 使用范围隔离来进一步减小包的大小

我将`_`添加到 CSS 类名中是有原因的，它将组件名和本地标识符名分开——这种区别有助于缩小。

[csso](https://github.com/css/csso) (CSS minifier)有[范围](https://github.com/css/csso#scopes)配置。作用域定义了在某些标记上专门使用的类名列表，即来自不同作用域的选择器不匹配相同的元素。该信息允许优化器更积极地移动规则。

为了利用这一点，使用 [csso-webpack-plugin](https://github.com/zoobestik/csso-webpack-plugin) 对 CSS 包进行后处理:

这使得 [GO2CINEMA](https://go2cinema.com/) CSS 包的大小从 53 KB 减少到 47 KB。

#### 值得吗？

反对这种缩小的第一个理由是压缩算法会为你做到这一点。与使用长类名的原始包相比，使用 [Brotli](https://en.wikipedia.org/wiki/Brotli) 算法压缩的 GO2CINEMA CSS 包只节省了 1 KB。

另一方面，设置这种缩小是一次性投资，它减少了需要解析的文档的大小。它还有其他好处，比如阻止依赖 CSS 类名导航的 scapers，或者意外匹配广告拦截器黑名单的 CSS 选择器。

与此同时，你可以在 GO2CINEMA 电影和场馆页面上看到这种缩小的演示

*   [https://go2cinema.com/movies/wonder-woman-2017-1305237](https://go2cinema.com/movies/wonder-woman-2017-1305237)
*   [https://go 2 cinema . com/venies/Odeon-Oxford-Magdalen-ST-1001053](https://go2cinema.com/venues/odeon-oxford-magdalen-st-1001053)

#### 你能帮我解决 GO2CINEMA 吗？

[GO2CINEMA](https://go2cinema.com/) 是我的宝贝。我喜欢为它工作？。然而，这是我十年来的第一次创业，有很多事情我需要帮助。

如果你能给出反馈，一个 SEO 建议，一个商业建议，认识一个天使投资人，认识一个能写一篇关于 [GO2CINEMA](https://go2cinema.com/) 的文章的人，发一条推特，邀请我参加一个会议，一个电台脱口秀等等。或者只是想表达你的支持/好奇，说声“嗨！”，给我发电子邮件在 gajus@gajus.com 或通过 Twitter 的 DM，[https://twitter.com/kuizinas](https://twitter.com/kuizinas)。

### 你喜欢阅读，我喜欢写作

你可以通过[给我买杯咖啡](https://www.buymeacoffee.com/gajus)和 [Patreon](https://www.patreon.com/gajus) 来支持我的[开源工作](https://github.com/gajus)和我写技术文章。我永远感激你？