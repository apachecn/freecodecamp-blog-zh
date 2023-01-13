# Gatsby Starter 博客:如何在支持 Twitter 卡的帖子中添加标题图像

> 原文：<https://www.freecodecamp.org/news/gatsby-blog-header-image-twitter-card/>

如果你和我一样，你用 [Gatsby Starter Blog](https://www.gatsbyjs.com/starters/gatsbyjs/gatsby-starter-blog) 来启动你的个人博客，做了一些定制，然后就开始了。

以 markdown 的形式添加新帖子是很棒的。但这也意味着你很少有理由去看代码。所以当我决定在我的帖子中添加标题图片以支持 [Twitter 卡](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards)时，我感到很失落。

我的要求是能够添加一个大标题图像和标题到一篇文章，正如你在这里看到的:

[Working with Heterogeneous Item Collections in the DynamoDB Enhanced Client for JavaWorking with heterogeneous item collections with the Java SDKs can be tricky. Here we see how to handle them with the AWS SDK v2 for Java’s Enhanced Client.![icon-512x512](img/6367700ceaaa9514e7c217c012da3e48.png)David A Good![kevin-mueller-gGUiw8GNIFE-unsplash](img/d8e775e15a03936f85bbcbaa247e7d3c.png)](https://davidagood.com/dynamodb-enhanced-client-java-heterogeneous-item-collections/)

此外，包含帖子链接的推文应该“扩展”成带有大图的 [Twitter 摘要卡，如下所示:](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/summary-card-with-large-image)

> 在 DynamoDB 增强版 Java 客户端中使用异构项目集合[https://t.co/tDzLmLb5Sg](https://t.co/tDzLmLb5Sg)@ dynamo db# Java# AWS
> 
> — David Good (@helloworldless) [December 8, 2020](https://twitter.com/helloworldless/status/1336323721254948864?ref_src=twsrc%5Etfw)