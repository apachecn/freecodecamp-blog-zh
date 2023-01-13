# 在它开始之前结束—如何不退回输入错误的用户电子邮件

> 原文：<https://www.freecodecamp.org/news/over-before-it-started-how-to-not-bounce-mistyped-user-emails-69be32408f21/>

亚历克斯·彼得森

# 在它开始之前结束—如何不退回输入错误的用户电子邮件

![1*2Oo1Din32plTMdI8GypK4Q](img/ba8f314bd5b8a4ae0bd14ad4dfa2bc0d.png)

Photo by [Mathyas Kurmann](https://unsplash.com/photos/fb7yNPbT0l8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我输入我的电子邮件地址如此之多，它已经成为肌肉记忆。每当我注册一份时事通讯或者在网站上开一个新账户，我就用这个。我几乎没有仔细检查过我输入的地址是否正确。

如果我的手指滑了，或者我丢了一把钥匙怎么办？

这种情况并不少见。对于依靠电子邮件地址来验证新用户的网站来说，这是一个大问题。但对于不依赖电子邮件地址验证的网站来说，这是一个更大的问题。

这导致公司发出的电子邮件被退回。

“硬退回”率是他们发出的无效电子邮件地址的数量占有效电子邮件地址的百分比。

### 问题有多大？

根据 MailChimp 的[研究](https://mailchimp.com/resources/research/email-marketing-benchmarks/)，发送电子邮件的公司的“硬反弹”率可以攀升**超过 1%** ，而平均比率为 **0.53%** 。这些听起来可能不是很大的数字，但反弹可以累加。

想一想您可能会发送给客户的以下电子邮件，

*   欢迎电子邮件
*   验证电子邮件
*   滴灌运动
*   产品公告
*   活动通知
*   应用程序错误警报
*   每周博客综述

这样的例子不胜枚举…

随着你的网站或应用程序的扩展，被退回的电子邮件数量也会随之增长。

持续退回的邮件会开始损害你作为发件人的声誉。这将使你将来发送的电子邮件不太可能被信任。

如果你……联系不到用户，你也不能给他们发邮件，要求他们在你的数据库里修改他们的信息。

除非你事先建立了第二个沟通渠道*，否则用无效电子邮件地址注册的用户很可能会永远失去。*

*我们应该解决这个问题。*

#### *能做些什么？*

*一般来说，在用户不知情的情况下编辑用户的输入是个坏主意。*

*首先，没有机器能 100%完美地猜出人类的豆子真正想要什么。但是，如果你看到了什么，就说出来。*

*我发现，当应用程序注意到有些事情*可能*出错时，给用户一个温和的建议往往是最好的。这条经验法则在我以前的创业工作和现在正在创建的创业公司中都很有效。*

*在我的[在线写作工作室](https://www.penmob.com)的网站上，有一个简单的插件可以帮助用户在注册电子邮件地址时防止输入错误:*

*你可以在[登录页面](https://www.penmob.com/login)亲自尝试一下。我将向你展示如何在你自己的网站上设置这个。*

#### *黄铜大头针*

*本文假设你正在使用 [Browserify](https://medium.com/@christopherphillips_88739/a-beginners-guide-to-browserify-1170a724ceb2) 、 [webpack](https://webpack.js.org/) 或者类似的东西在你网站的前端构建 NPM 包。例如，Penmob 使用 [VueJS webpack](https://vuejs.org/v2/guide/installation.html#CLI) 安装。*

*首先，安装 [Mistyep 包](https://www.npmjs.com/package/mistyep):*

```
*`npm install mistyep --save`*
```

*然后将其添加到您的登录/注册页面:*

```
*`var mistyep = require('mistyep');`*
```

*Mistyep 是一个软件包，可以根据最常见的电子邮件提供商检查用户的输入。下面的代码确保了将“gmail”打错为“gnail”的人仍然可以进入。*

```
*`<input type="email" id="emailInput" />`*
```

```
*`<!-- ... -->`*
```

```
*`<script>// Get your user's email input from the login form.var emailInput = document.getElementById('emailInput').value;`*
```

```
*`// Mistyep returns the original value if no correction is found.var correctedEmail = mistyep.email(emailInput);`*
```

```
*`if (emailInput !== correctedEmail) {  // suggest the alternative spelling to the user.}</script>`*
```

*由您决定如何处理向用户提出的建议。在上面的 GIF 中，当`emailInput`不等于`correctedEmail`时，我显示一个方框。该框包含文本`Did you mean {{ correctedEmail }}?`。点击那个框会将原来的`emailInput`字段设置为等于`correctedEmail`，然后隐藏这个框。*

*注意，在这个例子中，实际应用建议需要用户手动点击。用 Mistyep 找到的建议自动更新数据库记录会让你很快陷入麻烦。*

*您还可以使用 Mistyep 来检查任意单词列表，而不仅仅是电子邮件地址。请随意查看[代码示例](https://www.npmjs.com/package/mistyep#for-custom-words)和玩[现场演示](https://penmob.github.io/mistyep/)。*

#### *保持简单*

*验证电子邮件地址是一个黑洞，我不建议太深入。Mistyep 不能捕捉到每一个可能的错误(没有什么可以)。但是，它将为大多数情况下输入错误电子邮件地址的用户提供正确的建议。*

*希望你觉得这对你自己的 app 或者网站有用。我在 Twitter 上发布工程见解、写作技巧和产品公告。对于(不经常)更新，[跟我来](https://twitter.com/pen_mob)。快乐大厦！*