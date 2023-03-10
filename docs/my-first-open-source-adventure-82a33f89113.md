# 我的第一次开源冒险

> 原文：<https://www.freecodecamp.org/news/my-first-open-source-adventure-82a33f89113/>

作者安东尼·吴

# 我的第一次开源冒险

![XZFttbphmnQail-9QcKM3MRMxwTAEesDgPdl](img/26f0d54fe95f4d75d3d9750b477e3073.png)

Les Alpilles, Mountain Landscape Near South-Reme by Vincent van Gogh

### 灵感来源

开源的想法总是能引起我的共鸣。人们为了公共利益自愿贡献自己的时间和知识，这有什么不好呢？

所以在读了 [Shubheksha](https://www.freecodecamp.org/news/my-first-open-source-adventure-82a33f89113/undefined) 的文章[嘿，新手开源贡献者:请写更多的博客](https://medium.freecodecamp.com/new-contributors-to-open-source-please-blog-more-920af14cffd#.rwxevlacv)之后，我决定加入这个行列。

有人告诉我，从事开源项目是最好的学习方式之一。

*   您可以看到由更有经验的开发人员编写的代码。
*   你可以使用你不会在自己的爱好项目中使用的技术。

为开源做贡献是表达你利他本性的一种方式。我想象一个罗宾汉式的人物拿着一台 MacBook(从它的 20 个标签下你几乎分辨不出来)，勇敢地与笨重的闭源企业软件作斗争。

![pXOHlz1kYkryoexWwm9K5JqW2FJtH2z8NWit](img/c1787c44f77d65caf304c2999b3e5c3d.png)

Imagine his arrows are commits.

我想成为一个开源的罗宾汉。

### ☠☠害怕☠☠

当我凝视着显示器时，许多恐惧中的第一个进入了我的脑海。如果我被这些开源项目的复杂性淹没了怎么办？

盯着一个有 2000 个提交的存储库是非常令人畏惧的——尤其是当我的爱好项目达到 40 个提交时。

当我开始在一堆我从未听说过的名字神秘的文件中导航时。snyk，. varci.yml，pm2Start.js)，冒名顶替综合征的想法很快就爬了进来。我问自己:“我真的有资格接触这些代码吗？”

我听说过一些关于恶劣社区的恐怖故事。当我到了需要一些指导，不得不实际上…和一个人交谈的时候，会发生什么？

当我想到与开源维护者的第一次对话时，恐惧让我窒息。在我的脑海中，他们的回答从“哈哈，你是认真的吗？”“也许网络开发不适合你。”

但我感到安慰的是，像网络巨魔一样，我可以把自己(和我的羞耻)藏在我的电脑后面。

我还担心我的拉请求不会被合并。可能我的工作太差了，以至于其他维护人员都懒得给我指引正确的方向。将我的时间花在可能不会用到的代码上，值得吗？

尽管有这些担心，我还是决定开始我的开源冒险。

### ？？找项目？？

第一步是找到一个开源项目。 [Shubheksha](https://www.freecodecamp.org/news/my-first-open-source-adventure-82a33f89113/undefined) 的文章[如何找到你的第一个开源 bug 修复](https://medium.freecodecamp.com/finding-your-first-open-source-project-or-bug-to-work-on-1712f651e5ba#.q0bt5j41d)推荐看[待价而沽](http://up-for-grabs.net/)寻找项目候选人。在快速浏览 JavaScript 项目列表后，我没有发现任何吸引我眼球的项目。

我想找一个我个人使用并且相信的项目。问题是这些项目中的大部分都太成熟了，对我来说太超前了。例如，我喜欢 React，但是我不知道如何修复“子树/父生命周期方法的意外交错”

我确信有一些项目我可以取得重大进展，但它们对我来说仍然是陌生的。

然后我想起来自由代码营是开源的。作为一个我使用并相信的项目，它通过了我的测试。我花了几个小时学习他们的课程，喜欢他们同时帮助新开发者和非营利组织的方式。我也向任何提到想学习 web 开发的人推荐免费代码营。

所以在浏览了他们在 GitHub 上的[开放问题列表后，我选择了自由代码营作为我的第一个开源项目。](https://github.com/FreeCodeCamp/freecodecamp/issues)

### ？？发现一个 bug？？

下一步是找到要解决的问题。理论上，这应该是最简单快捷的事情。

但是，就像在网飞看电影一样，有时候你花更多的时间去挑选要看的东西，而不是实际观看。

有超过 300 个公开的问题，我决定用标签来过滤它们。我找到了一个标有“仅限首次使用”的，听起来很友好。但这一期有 80 多条评论。我不想碰那个。

我没有看到任何我可以立即着手解决的问题，所以我继续寻找。

我找到了另一个名为“寻求帮助”的标签，浏览了一下还没有人评论过的问题，但是没有什么引起我的注意。

相反，我决定关注回购，寻找尚未有人解决的新问题。大约 15 分钟后，我发现大部分新问题都是来自那些被困在挑战中的营员。这些问题实际上不是我可以修复的错误。

然后 [issue #10989](https://github.com/FreeCodeCamp/FreeCodeCamp/issues/10989) 走进了我的生活。这与一项编码挑战中过于严格的正则表达式测试有关。

我心想，“我对 regex 略知一二。”

然后一个维护者评论说这是一个有效的问题，并添加了一个“需要帮助”的标签。我的心开始怦怦直跳，因为我重读了 3 次，以确保我没有错过任何东西。

然后我心里想:“我能做到！”

我对这个问题进行了评论，并害羞地问我是否可以在这个问题上努力。我的膝盖无力，手臂沉重。在等待回应的时候，我的手心在冒汗。我感到紧张，就好像我刚给一个女人发短信邀请她约会一样。

最后，有人回答说，“你可以在这方面努力。”

该去工作了。

### ？？？入门？？？

我从查看存储库中的 README.md 开始。幸运的是，自由代码营对如何贡献和创建拉请求有非常详细的指导。他们也有一个专门为投稿人准备的 Gitter 频道。

我分叉了存储库并开始工作。

### ？？磕磕绊绊？？

我知道建立我的环境会很困难。我的导师告诉我，在他开始第一份工作之前，他花了一周时间安装所有正确的程序。

这很困难，因为当某些东西没有正确安装或运行时，你必须在你的终端上阅读这些神秘的错误信息。果然，没过多久我就遇到了一个问题。

当我开始运行“gulp”时，gulpfile 内部出现了一个错误。它突出显示了一行使用 ES6 arrow 语法的代码，并表示它无法识别它。我查看了文件的其余部分，没有看到 ES6 arrow 语法的其他用法。我可能忘记了构建过程中的某个步骤吗？

我在谷歌上搜索解决方案，但没有得到任何有用的信息。所以我决定黑掉它。我把 ES6 箭头变成了一个常规的函数声明，看看发生了什么。“gulp”命令没有再抱怨，设置了一个本地主机。

在这次成功之后，没多久我就遇到了另一个障碍。我去了我的本地主机，迎接我的是自由代码营主页，除了它不起作用。点击通常显示所有挑战的“地图”链接没有任何作用。控制台中有许多错误，最明显的是丢失了“bundle.js”文件。

我为自己在某个地方漏掉了一步而自责，决定从头开始。我删除了回购的本地副本，并重新安装了所有东西，确保对每一次按键都格外警惕。我遇到了同样的错误，并决定是时候使用 Gitter 通道了。

当我第一次登录 Gitter 频道时，我看到了许多关于自由代码营新更新的消息。在我的右边，迎接我的是这个频道最近活动的总结，其中包括禁止四个人。这些人有没有因为没有让他们的大口文件工作而感到内疚？他们是否漫不经心地询问他们丢失的 bundle.js 文件在哪里？

我决定再读一遍投稿指南，以防遗漏什么明显的东西。我已经通读了很多遍，几乎可以背诵了。我没有发现任何对我有帮助的新东西，所以我鼓起勇气最终向 Gitter 频道询问了我的 ES6 语法问题。

有人马上回复了，但我很快意识到他们不是在和我说话。

我等了又等，直到救赎来临。一个叫 Dylan 的维护者告诉我，我必须更新我的节点和 npm。我再次看了一下贡献指南，意识到它清楚地说明了我应该使用的版本。我怎么忽略了这一点？！

我感谢迪伦的善举，我悄悄感谢吉特没有禁止我。

我立即安装了最新版本的 Node 和 npm。事情看起来已经有所好转。

我决定删除一切，从头再来。这是我第三次参与这个项目，并且已经成为一种熟悉的舞蹈。我还提取了一些前一天提交的新代码，修复了我看到的一些错误。

我在丢失“bundle.js”文件时遇到了同样的错误，我在 Gitter 频道中询问了这个问题。有人回应说应该是“gulp”命令创建的。我自己通过运行“webpack”命令创建了“bundle.js ”,最终环境看起来工作正常。我终于可以着手解决实际问题了！

实际修复花费的时间是设置我的项目所花费时间的四分之一。我写了一个正则表达式来检查元素的存在。棘手的部分是将我的正则表达式转换成一个字符串，然后记住根据需要使用反斜杠来转义字符。我更新了一行代码并运行了测试，测试通过了。

我通读了拉请求指令的文档。当你提交一个拉取请求时，自由代码营有一个令人敬畏的预填充清单。

我吸了最后一口气，发出了我的拉请求。我觉得自己就像一个送孩子上学的家长。除了等待，我别无选择。

### ？？已合并拉取请求！？？

第二天，我收到了一封关于我的[拉动请求](https://github.com/FreeCodeCamp/FreeCodeCamp/pull/11026#event-810810708)状态的电子邮件。有人评论“LGTM”

我在谷歌上快速搜索了一下，发现这代表“我觉得不错”然后我感到一种巨大的解脱感。他们合并了我的拉取请求！

我进入开源的第一步已经成功了！

### ？？我学到了什么？？

通过研究这个问题，我确实学到了一些技术知识。我在使用正则表达式方面有了更多的经验。

我还学习了如何使用反斜杠来转义字符串中的某些字符。在此之前，我只使用反斜杠来转义引号。

另一个好处是，我在使用 GitHub 时更加得心应手。

### ？？对新开源贡献者的话？？

这对我来说是一次惊人的经历，我推荐其他新开发人员尝试一下。这是我给那些有兴趣投稿的人的建议。

#### 秘诀 1:努力工作，坚持不懈

我想说的是，当你工作的时候，60%的时间里，一切都没有问题。但是当所有事情都失败时，剩下的 40%将决定你是一个怎样的开发人员。

拥抱这些障碍，战胜它们。

如果一切顺利的话，我从修复 bug 中学到的东西会比我学到的更多。

改变心态，迎接错误。把它们当作教训。

#### 技巧 2:尽可能多地交流细节。

当你通过异步通信模式(如 Gitter 或 email)提问时，这一点尤其重要。

假设您正在使用一个 API 来请求您所在城市的天气信息。你可以这样做。你问 API 天气
2。API 回应询问位置
3。你回复“纽约”
4。API 会问你是要摄氏、华氏还是开尔文
5 度。你回复“飞轮海”
6。空气污染指数以“纽约华氏 72 度”作为回应

或者你可以这样做。你问空气污染指数纽约的天气华氏温度。
2。空气污染指数以“纽约华氏 72 度”作为回应

任何人在使用 API 时选择第一个选项似乎都很愚蠢，但这正是人们在与其他人交流时所做的。简单的对话拖得很长，而每一方都要等上几分钟或几个小时才能得到对方的回应。

#### 技巧 3:不要被开源维护者吓倒。

他们可能比你更有经验，但是要意识到，在某一点上，他们也处于你的处境。他们只是比你经历了更多的错误。

只要记住，所有的开发者都在同一条漫长的道路上。我们中的一些人比其他人落后几步。重要的是继续前进。

### ？？给开源维护者的话？？

免费代码营社区非常棒，在我需要的时候非常有用。

我想感谢所有的维护者，他们不怕麻烦地帮助新的贡献者。当你告诉某人更新他们的节点和 npm 时，你可能认为你在做一些小事。你可能没有意识到的是，你可能是他们的救命恩人，让他们免于溺水和放弃。

继续努力吧！

此外，要小心你选择使用的词语。

我去日本旅行，想找一家餐馆吃饭。我路过一家很有前途的餐馆，但是它的窗户上有一个牌子，上面写着“禁止说英语”其他的都是日文。

像我这样的外国人会如何理解这个标志？这是不是意味着那里没有人会说英语？这是不是意味着我必须用菜单上我指的任何东西来玩俄罗斯轮盘赌？这是否意味着他们排外，不欢迎像我这样的外国人？我继续走着，从来没有费心去发现。

作为维护者，有时你是新贡献者交流的第一个真实的人。请注意，这些贡献者是在开源的异乡。他们可能很难讲这门语言——更不用说试图修复存储库中的一个公开问题了。

我计划继续我的开源之旅。希望能再见到你，听到你的故事。