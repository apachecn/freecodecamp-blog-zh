# 如何像程序员❤️一样表白

> 原文：<https://www.freecodecamp.org/news/send-a-romantic-message-every-hour-to-your-valentine/>

今天是情人节！？

如果你每小时给你爱的人发一条浪漫的信息，那该有多好？但是更好的是...

如果使用 Node.js 脚本自动完成，该有多棒？我们毕竟是...程序员吧？？

在这个简短的教程中，我将向你展示如何去做。

对那些懒惰的人，这里有一个视频教程:

[https://www.youtube.com/embed/HxqMPlyZC3w?feature=oembed](https://www.youtube.com/embed/HxqMPlyZC3w?feature=oembed)

## 创建 CRON 作业

首先，我们需要创建一个 CRON 作业，每小时运行一个函数。

为此，让我们将`node-cron`包安装到 NodeJS 应用程序中:

`npm install node-cron`

接下来，我们将安排一个函数每小时运行一次:

```
const cron = require('node-cron');

cron.schedule('0 * * * *', () => {
	sendMessage();
}); 
```

完美。我们还没有`sendMessage()`函数，但我们稍后会创建它。

此外，如果你不知道 CRON 字符串是如何工作的，这里有一个[很棒的网站](https://crontab.guru/#*_*_*_*_*)，你可以在那里测试一下。

基本上`'0 * * * *'`的意思是:`Run every hour at 0 minutes`，所以会运行在:`00:00, 01:00, 02:00`等...你明白了！

## 连接到 Twilio

我们需要一个 Twilio 账户，所以去[Twilio.com](https://www.freecodecamp.org/news/send-a-romantic-message-every-hour-to-your-valentine/www.twilio.com/referral/79CRPu)创建一个吧。您需要验证您的电子邮件地址，并验证您想要发送邮件的号码。(为了验证号码不得不“偷”老婆手机？)

在那里，他们会问你几个问题，比如:“你用的是什么编程语言？”你可以选择 Node.js，然后你会到达`/console`页面。

这里你会得到`ACCOUNT SID`和`AUTH TOKEN`。我们需要这些来调用 Twilio API，所以我们将它们存储在一个`config.js`文件中。

**警告:**不要共享**认证令牌**。这是一个密钥，所以我们将它们存储在这个“秘密”的 config.js 文件中。

太好了。

下一步将是创建一个试用号(您可以在`/console`页面上找到该按钮)。此号码将用于发送来自的消息。

现在我们一切就绪，让我们回到我们的代码！

我们需要安装 Twilio 包:`npm install twilio`并且我们需要使用存储在`./config.js`文件中的数据。

除了`ACCOUNT_SID`和`AUTH_TOKEN`之外，我们还可以存储我们所爱的人的`PHONE_NR`，因为我们将使用它来告诉 Twilio 将消息发送到哪里。

让我们这样做，并且让我们创建`sendMessage()`函数，它将...发消息？。

```
const config = require('./config');
const accountSid = config.ACCOUNT_SID;
const authToken = config.AUTH_TOKEN;
const client = require('twilio')(accountSid, authToken);

function sendMessage() {
	client.messages
		.create({
			body: 'Your Message here',
			from: '+19166191713',
			to: config.PHONE_NR
		})
		.then(message => {
			console.log(message);
		});
} 
```

您可以看到,`client.messages.create()`函数需要三样东西:

1.  正文/信息
2.  起始编号(这是上面创建的试验编号)
3.  收件人号码(这是我们要将消息发送到的号码)

## 获取消息

我们需要一个 24 条浪漫信息的列表，因此让我们创建一个`messages.js`文件并将所有信息放入一个数组中。

```
module.exports = [
	`If I could give you one thing in life, I'd give you the ability to see yourself through my eyes, only then would you realize how special you are to me.`,
	`If you were a movie, I'd watch you over and over again.`,
	`In a sea of people, my eyes always search for you.`
]; 
```

我在上面只添加了 3 条消息，但是你可以填充数组直到你得到 24 条消息。

## 结合一切

现在我们有了所有的 3 个组件:

*   CRON 工作
*   Twilio sendMessage()调用
*   这些信息

我们可以将它们组合成最终的代码:

```
const cron = require('node-cron');

const config = require('./config');
const accountSid = config.ACCOUNT_SID;
const authToken = config.AUTH_TOKEN;
const client = require('twilio')(accountSid, authToken);

const messages = require('./messages');

const currentMessage = 0;

function sendMessage() {
	client.messages
		.create({
			body: messages[currentMessage],
			from: '+19166191713',
			to: config.PHONE_NR
		})
		.then(message => {
			currentMessage++;
			console.log(message);
		});
}

cron.schedule('0 * * * *', () => {
	console.log('Message sent!');
	sendMessage();
}); 
```

你可以看到我添加了一个`currentMessage`计数器，它会在我们每次发送消息时递增，这样我们就可以遍历整个消息数组。

就是这样！？

现在你可以运行这个脚本，它会每小时给你爱的人发送一条浪漫的信息！

情人节快乐！？

*原贴于[www.florin-pop.com](https://www.florin-pop.com/blog/declare-your-love-as-a-programmer/)*