# 如何建立一个 Twitter 情绪分析工具

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-twitter-sentiment-analysis-tool/>

这个周末，我有一些空闲时间，决定建立一个 Twitter 情绪分析工具。

这个想法是，你输入一个搜索词，该工具将搜索最近的推文。然后，它将使用情绪分析来确定 Twitter 对该主题的积极或消极程度。

例如，你可以搜索“唐纳德·特朗普”来获得推特上对总统的看法。

让我们开始吧！

## 获取 Twitter API 密钥

我们需要做的第一件事是创建一个 Twitter 应用程序，以便获得一个 API 密钥。

前往 [Twitter 应用页面](https://developer.twitter.com/en/apps)创建一个新应用。您必须拥有开发者帐户才能创建应用程序。

如果您没有开发者帐户，您可以申请一个。大多数请求都会被立即批准。？

记下您在 Twitter 应用程序中找到的`API Key`和`API Key Secret`。

## 创建 NodeJS 项目

我将使用 NodeJS 来创建这个应用程序。

要创建我运行的新项目:

```
npm init
npm install twitter-lite
```

这将创建一个新的 NodeJS 项目并安装`twitter-lite`包。这个包使得与 Twitter API 的交互变得非常简单。

为了验证我们的请求，我们将使用 OAuth2.0 承载令牌。`twitter-lite`包有一个处理 Twitter 认证的简单方法。

让我们创建一个新的`index.js`文件，并向其中添加以下代码:

```
const Twitter = require('twitter-lite');

const user = new Twitter({
    consumer_key: "YOUR_API_KEY",
    consumer_secret: "YOUR_API_SECRET",
});

// Wrap the following code in an async function that is called
// immediately so that we can use "await" statements.
(async function() {
    try {
        // Retrieve the bearer token from twitter.
        const response = await user.getBearerToken();
        console.log(`Got the following Bearer token from Twitter: ${response.access_token}`);

        // Construct our API client with the bearer token.
        const app = new Twitter({
            bearer_token: response.access_token,
        });
    } catch(e) {
        console.log("There was an error calling the Twitter API.");
        console.dir(e);
    }
})();
```

运行此命令时，控制台会输出以下内容:

```
Got the following Bearer token from Twitter: THE_TWITTER_BEARER_TOKEN
```

太棒了，到目前为止一切正常。？

## 获取最近的推文

下一部分是从 Twitter API 中检索最近的 tweets。

在 [Twitter 文档](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets)中，您可以看到有一个端点用于搜索最近的推文。

为了实现这一点，我将以下代码添加到`index.js`文件中:

```
const Twitter = require('twitter-lite');

(async function() {
    const user = new Twitter({
        consumer_key: "YOUR_API_KEY",
        consumer_secret: "YOUR_API_SECRET",
    });

    try {
        let response = await user.getBearerToken();
        const app = new Twitter({
            bearer_token: response.access_token,
        });

        // Search for recent tweets from the twitter API
        response = await app.get(`/search/tweets`, {
            q: "Lionel Messi", // The search term
            lang: "en",        // Let's only get English tweets
            count: 100,        // Limit the results to 100 tweets
        });

        // Loop over all the tweets and print the text
        for (tweet of response.statuses) {
            console.dir(tweet.text);
        }
    } catch(e) {
        console.log("There was an error calling the Twitter API");
        console.dir(e);
    }
})();
```

运行这个程序时，你可以看到很多关于莱昂内尔·梅西的 twitter 评论，这意味着它运行得非常完美！⚽

```
"RT @TheFutbolPage: Some of Lionel Messi's best dribbles."

"RT @MagufuliMugabe: Lionel Messi ? didn't just wake up one day  and become the best player in the world no  HE trained. So if your girl is…"

""RT @goal: The boy who would be King ? Is Ansu Fati the heir to Lionel Messi's throne?"

and many more... 
```

## 执行情感分析

为了执行情感分析，我将使用谷歌云的自然语言 API。有了这个 API，你可以通过一个简单的 API 调用获得文本的情感分数。

首先，前往[谷歌云控制台](https://console.cloud.google.com/)创建一个新的云项目。

接下来，转到[自然语言 API](https://console.cloud.google.com/apis/api/language.googleapis.com) 并为项目启用它。

最后，我们需要创建一个服务帐户来验证我们自己。前往[创建服务帐户页面](https://console.cloud.google.com/apis/credentials/serviceaccountkey)创建服务帐户。

创建服务帐户时，您需要下载包含该服务帐户私钥的`json`文件。将此文件存储在项目文件夹中。

Google 有一个 NodeJS 包来与自然语言 API 交互，所以让我们使用它。要安装它，请运行:

```
npm install @google-cloud/language
```

为了让语言包工作，它需要知道私钥文件在哪里。

该包将试图读取一个应该指向该文件的`GOOGLE_APPLICATION_CREDENTIALS`环境变量。

为了设置这个环境变量，我更新了`package.json`文件中的`script`键。

```
"scripts": {
  "start": "GOOGLE_APPLICATION_CREDENTIALS='./gcloud-private-key.json' node index.js"
}
```

*注意，为了让它工作，您必须通过运行`npm run start`来启动脚本。*

有了这些，我们终于可以开始编码了。

我在`index.js`文件中添加了一个新的`getSentiment`函数:

```
const language = require('@google-cloud/language');
const languageClient = new language.LanguageServiceClient();

async function getSentiment(text) {
    const document = {
        content: text,
        type: 'PLAIN_TEXT',
    };

    // Detects the sentiment of the text
    const [result] = await languageClient.analyzeSentiment({document: document});
    const sentiment = result.documentSentiment;

    return sentiment.score;
}
```

这个函数调用 Google 自然语言 API 并返回一个介于-1 和 1 之间的情感分数。

让我们用几个例子来测试一下:

```
getSentiment("I HATE MESSI");
```

返回以下内容。

```
The sentiment score is -0.40
```

类似地:

```
getSentiment("I LOVE MESSI");
```

回报更高的情绪。？

```
The sentiment score is 0.89
```

## 将这一切结合在一起

最后要做的是用 tweets 中的文本调用`getSetiment`函数。

不过有一个问题:只有前 5000 个 API 请求是免费的，之后谷歌将对后续的 API 请求收费。

为了尽量减少 API 调用的数量，我将把所有的 tweets 合并成一个字符串，如下所示:

```
let allTweets = "";
for (tweet of response.statuses) {
	allTweets += tweet.text + "\n";
}

const sentimentScore = await getSentimentScore(allTweets);
console.log(`The sentiment about ${query} is: ${sentimentScore}`);
```

现在我只需要调用 API 一次，而不是 100 次。

最后一个问题当然是:Twitter 怎么看待莱昂内尔·梅西？运行该程序时，它会给出以下输出:

```
The sentiment about Lionel Messi is: 0.2
```

因此，Twitter 对莱昂内尔·梅西持积极态度。

## 结论

我们已经创建了一个 NodeJS 程序，它与 Twitter API 交互以获取最近的 tweets。然后，它将这些推文发送到谷歌云自然语言 API，以执行情感分析。

你可以在这里找到这个[情绪分析的现场版。](https://coffeecoding.dev/twitter-sentiment-analysis)

你也可以在 Github 这里查看完整的代码[。](https://github.com/Dirk94/twitter-sentiment-analysis)