# 如何在 30 分钟内构建一个简单的搜索机器人

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-simple-search-bot-in-30-minutes-eb56fcedcdb1/>

奎因·兰吉尔

# 如何在 30 分钟内构建一个简单的搜索机器人

![1*pMm3_L9RmFcb0KLJT1SirQ](img/7ebbc5045bd5628b5bd2b5bec8997d0f.png)

找房子真糟糕，尤其是在蒙特利尔。本指南将告诉你如何建立一个机器人，停留在顶部的狩猎为您服务。这样，你就再也不用无休止地刷新你的搜索了。

### 语境

与其他城市不同，在蒙特利尔租公寓的大多数人都是按同样的租期。新租约从 7 月开始，持续 12 个月，到 6 月 30 日结束。虽然有人可能会认为这简化了很多事情——比如可用性和期望值——但这也意味着竞争非常激烈。

每天我都会醒来，刷新我打开的 10 个 Kijiji 页面，并发送电子邮件询问所有的新广告。我会在午餐、晚餐和睡觉前再做一次。我的回复率很低——远低于 10%。当有人回复时，他们的回答通常是冷酷的。

我的下一步是加大赌注，真的拿起电话。打电话让我的机会多了一点。房东反应更快，这次我前面通常只有不到 10 个人。但肯定还是超过 5。回到绘图板。

一天，当我向一位同事抱怨我所有的时间都被找房子的事情占用时，我突然明白了。我可以用我的电脑解决这个问题。

回家后，我写了一个小程序来监视 Kijiji 搜索变化。当它看到它们时，它会向我的手机发送一条包含相关信息的短信。本文的其余部分将解释我是如何做到这一点的。

**注:**对于不关心教程的人，我已经把 Kijiji scraper 放上去做开源回购[这里](https://github.com/quinnlangille/pad-patrol):？

### 建筑平台-巡逻

当我下班回家时，我拿出笔记本电脑，启动了我的终端。我知道这个程序应该是轻量级的，因为我将全天候运行它——或者至少在我找到公寓之前。我决定只构建一个简单的节点脚本，可以从我的终端执行。

#### 设置

假设您已经安装了`node`和`npm`，任何节点项目的第一步都是在项目目录中初始化 npm。

接下来，让我们创建一个`src`目录，我们的代码将存放在这里。

在`src`目录中，创建一个`index.js`文件，我们的脚本将放在那里。

你可以这样做:

```
$ npm init // this will ask a few questions$ mkdir src$ cd src && touch index.js
```

#### 写剧本

当制作一个单独的项目时，我倾向于自由式——打破东西，然后修复它(可以说是最好的学习方式)。我将尝试用下面的指令来模拟我最初的思维过程，但是如果它们看起来到处都是，请告诉我。

我们要做的第一件事是向 Kijiji 发出一个成功的请求。为了确保我们能够得到正确的响应，让我们进行一个非常基本的获取。

为此，我们需要安装一个请求库:

```
$ npm install request-promise
```

然后将以下内容添加到`index.js`:

一旦保存好，我们就可以运行`$ node src/index.js`了，我们应该会在控制台中看到一些 HTML 标记。第一步完成—简单！

因为我们只关心内容何时改变，所以让我们对响应做一个简单的散列。这样，我们可以比较响应和散列。如果我们需要记录我们的结果，这比原始标记要简单得多。

为此，我们可以使用一个名为`checksum`的散列工具:

```
$ yarn add checksum
```

然后:

好酷，这个成功了！我们的 1500 行 HTML 已经被削减到 32 位。现在，让我们将它包装在一个可重用的函数中:

上面的代码将从获取的值中创建一个散列。然后在接下来的获取中，它将比较原始的和新的散列。

如果它们不同，它将返回`true`。这太棒了…有点太棒了。你会看到，它每次都返回`true`？

在进一步检查 fetch 的响应后，我们可以看到 Kijiji 在头中有一个时间戳。这意味着每次提取时散列都不同。值得注意的是，这种情况也会因循环广告和一堆其他动态内容而发生。

从上面的疏忽中得到的启示是，在处理一个不是你写的 API 时，总是要仔细检查你的反应。

这意味着我们需要访问标记的粒度，所以让我们安装一个第三方包来帮助解析响应。 [Cheerio](https://cheerio.js.org/) 是一个可以接收 HTML 标记并将其转换成可访问的 JavaScript API 的库。它的预期目的是帮助`jQuery`开发者不使用`jQuery`，但是意图被高估了。

对我们来说，这将是一套假的 Chrome 开发者工具！

作为以这种方式使用 Cheerio 的先决条件，我们需要知道在我们的标记中寻找什么。因此，让我们打开 Chrome，检查我们的网址。

如果我们检查广告，我们可以看到所有的搜索响应都有类别`.search-item`和`.regular-ad`。完美！

我们可以像这样选择那些有麦片的:

正如我们计划的那样，这将产生一系列组织有序的对象。根据 Cheerio 的文档，一个元素的所有属性都嵌套在一个名为`attribs`的键中。如果我们回到 Chrome 开发者工具，我们可以看到每个广告都有一个唯一的数据属性，称为 ID。让我们以此为目标—用下面的代码替换您的`checkURL`函数中的代码:

```
rp(siteToCheck).then(response => {  const $ = co.load(HTMLresponse);  let apartmentString = "";
```

```
 // use cheerio to parse HTML response and find all search results  $(".search-item.regular-ad").each((i, element) => {    console.log(element.attribs["data-ad-id"]);  });});
```

好极了，我们得到了一个独特的身份证号码列表。这些 ID 是页面上我们唯一关心的信息。

因此，让我们回到我们最初比较散列的计划，除了我们将只散列唯一的 id:

完美！它完全按照预期工作。当有人发布新广告(或删除旧广告，注意 id 的顺序)时，我们在控制台中打印`true`。剩下要做的就是设置我们的短信工具。

#### 从终端发送短信

这实际上比看起来容易得多。为此，我们将使用名为 [Twilio](https://www.twilio.com/) 的第三方软件。它做了很多，但它的核心功能之一是发送短信。另外，它还有很棒的 JavaScript API！为了完成教程，你需要一个他们的[账户](https://www.twilio.com/try-twilio)——免费试用就足够了——甚至可能得到一套新公寓。

好的，首先我们需要运行:

```
$ yarn add twilio
```

从这里开始，在`index.js`中，我们添加 Twilio 并定义一个名为`SMS`的新函数:

```
const twilio = require(twilio);
```

```
// you'll need to get your own credentials for this oneconst client = new Twilio("accountID", "authKey");
```

```
function SMS({ body, to, from }) {  client.messages    .create({      body,      to,      from    })    .then(() => {      console.log(`? Success! Message has been sent to ${to}`);    })    .catch(err => {      console.log(err);    });} 
```

这个简单的函数接受两个电话号码(`to`和`from`)和一条消息(`body`)。代替控制台记录我们的`checkURL`函数的结果，我们可以用我们想要的任何消息调用`SMS`:

你有它！每次我们的脚本看到站点哈希值之间的变化，它就会发送一条带有 URL 的短信到你的手机上？。

### 狩猎愉快！

我构建的实际脚本比上面的例子稍微复杂一点——我把它作为开源 repo 放在了 [GitHub](https://github.com/quinnlangille/pad-patrol) 上。

最后，我想对它做一些补充——首先是使它更加通用，而不仅仅是一个 Kijiji 刮刀。这是非常基本的，所以这将是一个伟大的新贡献者的第一次项目。

请随意以任何你认为合适的方式做出贡献？

另外，如果有人想知道，我上周日刚签了一份租约。我最终租下的公寓来自巡逻队发给我的第一份更新——那是命运✨

我目前在蒙特利尔的一家奢侈品时尚公司做软件开发。在去年夏天完成了一个网络开发训练营之后，我已经这样做了大约一年。我利用空闲时间学习热门的新技术，直到几天前，我还在寻找公寓。