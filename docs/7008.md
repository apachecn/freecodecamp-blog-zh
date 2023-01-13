# 为什么我会自动删除我所有的旧推文，以及我使用的 AWS Lambda 函数

> 原文：<https://www.freecodecamp.org/news/why-im-automatically-deleting-all-my-old-tweets-and-the-aws-lambda-function-i-use-to-do-this-6d26ef517ee1/>

从现在开始，我的微博是短暂的。这就是为什么我要删除我所有的旧推文，并且我使用的 AWS Lambda 功能可以免费做到这一切。

### 材料和意见

我只做了一年半多一点的一包游牧者。在那之前，我像大多数人一样住在公寓或房子里。我拥有家具，比我严格需要的更多的衣服，和足够的“东西”来填满至少几个移动的箱子。如果我去别的地方生活，为了学校、家庭或工作而搬家，我会收拾好所有的东西随身携带。这些年来，我积累了越来越多的东西。

采纳许多人所谓的极简主义生活方式迅速改变了我长期以来的许多观点。放弃我所有的东西(这个想法我曾经认为原则上很有趣，但实际上有点可笑)已经变得很正常了。现在，对我来说，不拥有我不经常使用的东西是很正常的。我不会在墙上的架子上放满旧书、旧盘子、旧衣服或童年玩具，因为这些东西对我来说已经不再重要了。相反，我只是保留美好的回忆。

想象一下，我仍然住在一所房子里。想象一下，在那个房子里，冰箱上，有一幅我六岁时画的画。在这幅画的右下角，用绿色蜡笔潦草地写着“西兰花是哑巴——维多利亚，6 岁。”

如果你在我家看到冰箱上的那幅画，你会认为“西兰花是愚蠢的”这句话准确地表达了我对西兰花的看法吗？当然不是。我写这个的时候才六岁。我有足够的时间改变主意。

### 社交媒体不是社交媒体

我有一个朋友，我们从幼儿园就认识了。我们一起上了小学，这些年来，我们偶尔会说话和见面。我们都是成年人了。有时当我们聊天时，我们会想起年轻时的一些有趣的记忆。就记忆的本质而言，我并不幻想我们回忆起的东西会被准确地叙述出来。我们对发生的事情的印象——我们犯的错误和胜利的时刻——被我们从那时起的经历和我们学到的所有东西所影响。在学校同事的生日聚会上的尴尬时刻成为一个孩子学习社交的例子，而不是当时可能感觉的尴尬的世界末日。

这就是记忆的工作原理。从某种意义上来说，它应该更新。生活在小社区的人们记得他们的邻居很多年前做过的事情，但是在他们现在的邻居是谁以及他们目前的关系是什么的背景下回忆这些事情。这种对历史的重新着色是人们如何治愈创伤、T2 如何做出正确决定、T4 如何社交的重要组成部分。

社交媒体不会这样做。你保存完好的五天前或五年前的推文可以绝对准确地回忆起来。对于大多数人来说，这并不是特别令人担忧。我们倾向于在推特上谈论一些非常平凡的事情——当我们感到无聊并希望有人注意到我们时突然想到的事情。就个人而言，通常情况下，我们的旧推文是相当微不足道的。然而，总的来说，它们相当完整地描绘了一个人随机的、无意中说出的想法。这就是问题所在。

对社交媒体和推特上写的东西做出的假设，与你上周对某人记事本上的涂鸦做出的假设截然不同。我不想去猜测为什么——我只是看到了足够多的案例，有人因为几年前发布的东西而被公开鞭打，我知道这确实发生了。这太奇怪了。如果你不认为上周的记事本涂鸦或几十年前的蜡笔画反映了某人现在的本质，为什么你会认为一条旧推文反映了呢？

你已经不是上个月的那个你了——你所看到的、读到的、理解的和学到的东西，都以某种微小的方式改变了你。虽然一个人可能在一生中的大部分时间里都有相同的自我意识和身份，但这种意识也会随着时间的推移而增长和变化。我们改变我们的观点，我们的欲望，我们的习惯。我们不是停滞不前的存在，我们不应该让我们自己被描述成停滞不前的存在，不管是多么无意的。

### 短暂的推文

如果你今天看我的 Twitter 个人资料页面，你会发现那里的推文比你的手指还少(我希望)。我正在使用[短命](https://github.com/victoriadrake/ephemeral/)——一个我为在 [AWS Lambda](https://aws.amazon.com/lambda/) 上使用而编写的轻量级实用程序——删除我几天前的所有推文。我这样做的原因和我不保存不再使用的东西是一样的——那些东西对我来说不再相关了。也不代表我。

组成 periodic 的代码是用 Go 写的。AWS Lambda 为每个 Lambda 函数创建了一个环境，所以 periodic 为您的私人 Twitter API 密钥和您希望保留的 tweets 的最大年龄(以小时表示)利用环境变量，如`72h`。

```
var (
	consumerKey       = getenv("TWITTER_CONSUMER_KEY")
	consumerSecret    = getenv("TWITTER_CONSUMER_SECRET")
	accessToken       = getenv("TWITTER_ACCESS_TOKEN")
	accessTokenSecret = getenv("TWITTER_ACCESS_TOKEN_SECRET")
	maxTweetAge       = getenv("MAX_TWEET_AGE")
	logger            = log.New()
)
func getenv(name string) string {
	v := os.Getenv(name)
	if v == "" {
		panic("missing required environment variable " + name)
	}
	return v
}
```

这个程序使用了 [anaconda](https://github.com/ChimeraCoder/anaconda) 库。它获取您的时间线，达到 Twitter API 的每个请求 200 条推文的限制，然后将每条推文的创建日期与您的`MAX_TWEET_AGE`变量进行比较，以确定它是否足够旧，可以删除。删除所有过期推文后，Lambda 函数终止。

```
func deleteFromTimeline(api *anaconda.TwitterApi, ageLimit time.Duration) {
	timeline, err := getTimeline(api)
if err != nil {
		log.Error("Could not get timeline")
	}
	for _, t := range timeline {
		createdTime, err := t.CreatedAtTime()
		if err != nil {
			log.Error("Couldn't parse time ", err)
		} else {
			if time.Since(createdTime) > ageLimit {
				_, err := api.DeleteTweet(t.Id, true)
				log.Info("DELETED: Age - ", time.Since(createdTime).Round(1*time.Minute), " - ", t.Text)
				if err != nil {
					log.Error("Failed to delete! ", err)
				}
			}
		}
	}
	log.Info("No more tweets to delete.")
}
```

点击阅读完整代码[。](https://github.com/victoriadrake/ephemeral/blob/master/main.go)

对于这样的用例，AWS Lambda 有一个免费层，不需要任何成本。如果您是任何级别的开发人员，那么熟悉它是非常有用的工具。关于如何设置一个 Lambda 函数来为你发 tweets 的完整截图，你可以阅读这篇文章。短命的设置是相同的，它只是有相反的功能。:)

我从亚当·德雷克的[哈罗德](https://github.com/adamdrake/harold)那里获得了短命，这是一个 Twitter 工具，除了保持你的时间表不变之外，它还有许多有用的功能。如果你一次要删除超过 200 条推文，请首先使用 Harold。您可以在终端上用`deletetimeline`标志运行 Harold。

你可能想先[下载你所有的推文，然后为了情感价值](https://twitter.com/settings/your_twitter_data)删除它们。

### 为什么要使用 Twitter？

预计到这个问题，让我说，是的，除了作为我的 Lambda 函数填充和清空的桶之外，我确实使用 Twitter。它有它的好处，大部分与我认为它最初的预期目的有关:作为一种接近即时的交流手段，将简短、易理解的信息传达给广泛的人群。

我用它来跟踪现在正在发生的事情。我用它来评论、取笑和同情我现在关注的*的人发的微博。*通过将我的时间表限制在最近几天，我觉得我使用 Twitter 更像是它的本意:一种加入对话并了解世界上正在发生什么的方式*——而不仅仅是另一个积累更多“东西”的地方*

*感谢阅读！如果你想知道更多关于我如何用技术让我的生活变得更好，你可以关注我，也可以看看我的博客，在那里我用更多画得很糟糕的食物涂鸦来解释编码: [Victoria.dev](https://victoria.dev)*

*我希望你有一个真正伟大的一天！:)*