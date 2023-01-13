# 不要在运行时这样做。在设计的时候做。

> 原文：<https://www.freecodecamp.org/news/dont-do-it-at-runtime-do-it-at-design-time-c4f59d1775e4/>

很久以前，一位睿智的老开发人员给了我一条建议，直到最近我才完全理解。

我们在一次代码审查中查看了一些要求程序输出从 A 到 Z 的字母列表的特性(想象一下带有一组按钮的联系人列表，这些按钮允许您向下跳转到以某个字母开头的姓名——诸如此类)。

于是，出现了一些年轻的大人物。(好吧——是我。)我认为，与其对所有字母的数组进行硬编码，不如编写一个从 65°到 90°迭代的 for 循环，然后使用这些值从字符代码中生成字母。

对应的 JavaScript 代码应该是这样的:

```
for (let i = 65; i <= 90; i++) {  letters.push(String.fromCharCode(i))}
```

聪明的老开发人员看着我，问我为什么不直接对数组进行硬编码。这并不是说字母表在不同的时段会有所不同。所以为什么每次都要计算呢？

我惊呆了。“你不会真的指望我像个孩子一样把每一个字母都打出来吧。我是专业的软件开发人员！我有算法和数据结构，还有一个数学协处理器！”

“很好，”他说。然后在设计时使用它们为您生成数组，然后将其复制/粘贴到生产代码中

然后他说:

> “避免在运行时做设计时可以做的事情”

现在，让我们说实话。我的小 for 循环不会让应用程序陷入停滞。今天的机器处理代码的速度会很快，甚至没有人会注意到。但作为一般原则，这是明智的建议。

我们经常编写代码，在每次请求时将很少变化的数据从一种格式转换成另一种格式。

想想从一个可能一年变化一两次的数据库中获取一段内容、格式化它并将其转发给浏览器的所有往返行程，这不必要地降低了我们的应用程序的速度。

对于依赖于内容管理系统的网站来说尤其如此。

这就是为什么我认为 Wordpress、Drupal 等老牌公司将在未来几年面临来自静态站点生成器的可信挑战，如 [Gatsby](https://www.gatsbyjs.org) 、 [Hugo](https://gohugo.io) 或 [Jekyll](https://jekyllrb.com) 以及平滑的构建过程、 [headless CMS](https://css-tricks.com/what-is-a-headless-cms/) 、廉价 CDNs 和快速持续集成工作流。

这种模式被称为 [JAMstack](https://jamstack.org) ，代表“JavaScript、API 和标记堆栈”而[的成绩也相当可观](https://jamstack.org/examples/)。

睿智的老开发人员的建议在我耳边回响:“避免在运行时做那些你可以在设计时做的事情。”随着时间的推移，我意识到这条建议有着深远的影响。不仅对于软件开发，对于生活也是如此。

最近，我一直在读一本很棒的书，书名是雷伊·达里奥的《原则:工作与生活》。这本书的一个中心主题是，T2 类型的问题比 T4 实际问题要少得多。因此，如果你提前做好工作，并想好如何处理你可能面临的特定类型的问题，那么当它真的到来时，你会更好地准备好处理它。

本质上，你可以通过在“设计时”整理不同问题类型的方法来更快地做出更好的决定，当你冷静地思考生活时，而不是在“运行时”，当你面临一个实际的问题并惊慌失措时。

Dalio 通过将他的方法编目为一组原则来实现这种技术。他甚至将自己的决策过程编成了一套计算机算法，可以用大量的历史数据进行测试。

鉴于他是一个亿万富翁，经营着一家非常成功的投资公司，我想说这是可行的。

事实上，华尔街开始雇佣比股票交易员更多的电脑程序员。因此，如果你对自己是否选择了正确的职业有任何怀疑，那么有更多的证据表明软件正在吞噬这个世界。

我在最近的一次关于 Developer On Fire 播客的采访中分享了我自己的建议和经验教训，你可以在这里听。

[**第 299 集| Bill Sourour -向前支付**](https://dev.to/developeronfire/episode-299--bill-sourour--paying-it-forward)
[*Bill Sourour 与 Dave Rael 讨论了艰难的教训、使教训变得可及、软件咨询…* 开发到](https://dev.to/developeronfire/episode-299--bill-sourour--paying-it-forward)

你可以在 Jamstack.org 学到更多关于 JAMstack 的知识

[**JAMstack | JavaScript、API 和标记**](http://www.JAMstack.org)
[*当我们谈论“堆栈”时，我们不再谈论操作系统、特定的 web 服务器、后端编程……*www.jamstack.org](http://www.JAMstack.org)

Netlify 博客上还有一篇关于静态站点生成器的精彩综述，网址是:

[**2017 十大静态网站生成器| Netlify**](https://www.netlify.com/blog/2017/05/25/top-ten-static-site-generators-of-2017/)
[*在这个倒计时中，我们来回顾一下到目前为止的 2017 十大静态网站生成器。*www.netlify.com](https://www.netlify.com/blog/2017/05/25/top-ten-static-site-generators-of-2017/)

这里有一篇关于我最近评论和推荐的特定堆栈的文章，它使用 Gatsby、Contentful、Netlify 和 Algolia 的组合作为文档站点的传统 CMS 的替代方案:

[**【Gatsby+Contentful+Netlify(和 Algolia)**](https://www.gatsbyjs.org/blog/2017-12-06-gatsby-plus-contentful-plus-netlify/)
[*乔希韦弗(Josh Weaver)在 By the Book，Inc .的开发者，喜欢科技、写作和演奏音乐。不能拒绝一个像样的董事会……*www.gatsbyjs.org](https://www.gatsbyjs.org/blog/2017-12-06-gatsby-plus-contentful-plus-netlify/)

这篇文章最初出现在 **Dev Mastery 时事通讯**中，我定期将它发送给全世界成千上万的开发者。在下面注册，将更多类似的内容直接发送到您的收件箱。

如果你喜欢读这篇文章，请通过多次点击那个掌声图标来传播这个信息。提前感谢！