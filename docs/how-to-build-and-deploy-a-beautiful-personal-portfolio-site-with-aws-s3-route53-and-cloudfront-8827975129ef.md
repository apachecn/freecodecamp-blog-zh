# 如何用 AWS S3、Route53 和 CloudFront 构建和部署一个漂亮的个人作品集网站？

> 原文：<https://www.freecodecamp.org/news/how-to-build-and-deploy-a-beautiful-personal-portfolio-site-with-aws-s3-route53-and-cloudfront-8827975129ef/>

尼古拉斯·文森特·希尔

# 如何用 AWS S3、Route53 和 CloudFront 构建和部署一个漂亮的个人静态站点？

这是一个分步指南，旨在创建一个响应性静态广告牌/组合网站，通过 AWS 将其部署到网络上，并通过 HTTPS 安全地提供服务。

本指南是为初级和中级软件工程师和 web 开发人员设计的，他们正在寻找一种简单的方法来创建一个比 SquareSpace 或其他网站建设者的付费服务更便宜的个人网站。基本的技术要求包括 HTML/CSS/JS、npm 和 Git 的知识(高级站点创建过程使用 Gatsby.js，它需要 React.js 和 GraphQL 的知识)。

唯一的经济要求是 AWS Route53 托管区费用(每月 0.50 美元)和获得一个域名的成本(我从 get.tech 以 25 美元的价格租用了 nickvh.tech，为期五年)。网络生活也是一个免费的选择。根据个人技能和经验，预计时间承诺为四到五个小时。

本指南将使您(开发人员)能够控制网站设计、开发和部署的所有方面。

#### **第一步——创建你的个人网站(简单还是困难)**

这里有一个简单的方法:去[这里](https://html5up.net/)挑选一个完全响应的、超级可定制的、100%免费的(在[知识共享](https://html5up.net/license)下)HTML/CSS/JS 模板。下载它，定制/构建它，直到你爱上它！

通过调用您选择的项目的根目录中的`npm init`来创建一个`package.json`。确保`git init`，将所有非公开信息添加到`.gitignore`，并编写出色的提交消息。

**这里有一个困难的方法:**使用 Gatsby.js 从头开始构建一个速度极快的静态站点和 PWA。这不是一个 Gatsby.js 教程(很多很棒的教程可以在[这里](https://www.gatsbyjs.org/tutorial/)找到)，需要 React.js 和 GraphQL 知识。在这里可以找到一些入门模板(我使用“strata”html 5 up 模板作为 [nickvh.tech](https://www.nickvh.tech/) 的起点)。

测试和开发你的网站，直到你对它感到满意，并想与世界分享。我们将在以后用 Grunt 或`gatsby-plugin-s3`插件建立一个持续的开发管道，这样将来的修改/部署将变得快速和容易。

#### **第二步——获取域名**

像 [https://get.tech](https://get.tech/) 或【https://www.namecheap.com】T2 这样的网站为域名提供非常诱人的价格。我等着升职，用 25 美元得到了 [https://nickvh.tech](https://get.tech/) 五年。或者从谷歌购买一个. dev 域名。

#### **步骤 3——创建一个 AWS 账户**

创建一个 [AWS 账户](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)。这一步相当简单。新账户获得一年的免费服务——在这里概述[。这要求您在 AWS 上有一张有效的信用卡。**要意识到安全隐患，不要不小心把你的 AWS 凭证推到公共网站上。**](https://aws.amazon.com/free/)

#### **步骤 4 —配置 AWS S3 存储桶**

> 亚马逊简单存储服务(亚马逊 S3)是一种对象存储服务，提供行业领先的可扩展性、数据可用性、安全性和性能。这意味着客户可以使用它来存储和保护一系列使用案例中的任意数量的数据，例如网站、移动应用程序、备份和恢复、归档、企业应用程序、物联网设备和大数据分析。

AWS S3 是你的静态站点的所在地；为了正确配置，需要做一些事情。

首先创建一个桶；AWS S3 和 Route53 只有在您的 bucket 名称和域名完全匹配的情况下才能正常工作—将您的 bucket 命名为 mysite.domain(即 nickvh.tech)。请确保正确设置权限；公众应该能够从桶中读取，但不能向桶中写入。

![1*nDKOVzwkcQamFyaTVWYDHw](img/98359cd2ce9f460d1b579c21fd1ce50b.png)

Make sure to give public read access to your bucket

接下来导航到“属性”并将 AWS S3 配置为静态服务`index.html`(见下文)。

![1*jU6929oqVXSINz3o9LHJZg](img/c536342b1155d909c64b2e4049eaadc6.png)

创建一个名为 www.mysite.domain(在我的例子中为 [www.nickvh.tech](http://www.nickvh.tech) )的 bucket(也支持公共读取访问),并重定向到主 bucket 地址(见下文)。

![1*3r7p-HOEkXp97fOOdA58dA](img/37e603ea16feeb3bdca1d795b976a9a4.png)

接下来，在项目的根目录下创建`aws-keys.json`:

这里有一个[指南](https://help.bittitan.com/hc/en-us/articles/115008255268-How-do-I-find-my-AWS-Access-Key-and-Secret-Access-Key-)来找到你的 AWS 访问密钥 ID 和秘密密钥。

**记得给你的** `.gitignore` **加上** `aws-keys.json` **！我的一个学生不小心将他们的 AWS 凭据推送到 Github，被编程攻击，几个小时内产生了约 1.3k 美元的费用。**

![1*Vb3KlOUa_yZJ_yyZEz9zEg](img/7ec82535e0f10e2966cb71c4b9ed639b.png)

Don’t let this happen to you!

#### 第 5 步——用 Grunt 或`gatsby-plugin-s3`将您的网站上传到 AWS S3

> Grunt 是一个 JavaScript 任务运行器，一个用于自动执行频繁任务的工具，例如缩小、编译、单元测试和林挺。它使用命令行界面来运行文件中定义的自定义任务。

如果你选择简单的方法来创建你的站点，你可以使用 Grunt 来上传你完成的站点文件到你的 AWS S3 桶。

```
npm install grunt grunt-aws-s3 --save-dev
```

创建一个`Gruntfile.js`:

将它添加到您的`package.json`的脚本对象中。

```
"deploy": "grunt deploy"
```

现在，无论何时您想要部署一个新的站点产品版本，只需执行`npm run deploy`。这将仅向 AWS S3 发布/上传已更改的文件，从而减少您需要付费的请求数量(新 AWS 帐户第一年每月获得 2，000 个免费上传请求)。

或者——如果你用 Gatsby.js 构建你的站点，忘记 Grunt，只使用`gatsby-plugin-s3`进行静态站点部署。我写了一个 npm 脚本`npm run ship`，它构建了产品包并上传到 AWS S3 - `gatsby build && gatsby-plugin-s3 deploy`。我的盖茨比网站的代码可以在这里找到。

#### **步骤 6——用 AWS CloudFront 创建一个发行版**

> *Amazon CloudFront 是一种快速内容交付网络(CDN)服务，能够以低延迟、高传输速度向全球客户安全交付数据、视频、应用和 API，所有这些都在一个对开发人员友好的环境中完成。*

创建一个新的 CloudFront 发行版(大致遵循这些 [AWS 文档](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-https-requests-s3/))。确保正确指定**备用域名****【CNAMEs】**，并请求自定义 SSL 证书以启用 HTTPS。

![1*dKBOI5qkk-JZUoTs4KOrHQ](img/8ac596f2799b55e4341a4cf59558c688.png)

同样指定**默认根对象**为`index.html`。

![1*YdRYM7Ygu_pHCViM6ARgYA](img/2e84e8882f28d8cf0163bcb8345e5a4f.png)

Remember to specify Default Root Object

点击 Create Origin 将您的 S3 bucket 链接到您的 CloudFront 发行版。将**原域名**指定为`YOUR_S3_BUCKET_NAME.s3.amazonaws.com`。不要限制铲斗的进出。

![1*Q2B1TcFuw2o9vvkXOXwnjQ](img/f0a14747b25c2ee1df1653820a9e3d61.png)

CloudFront Distribution Origin

创建一个新行为，并将**查看器协议策略**设置为“将 HTTP 重定向到 HTTPS”

![1*y7BAWIzgm-TCa_jdCBTKqA](img/0f40f3af926614fe9e491ca529ecf702.png)

CloudFront Distribution Behaviors

您还可以定制错误响应——我将 404 错误重定向到一个定制的 404.html 页面。

![1*CyDZcVNq6o3FtA1fDGOjDw](img/ca8ff3b8b15cf56f8cf47cecf053382d.png)

CloudFront Error Pages

#### **第 7 步—使用 AWS Route53** 将流量重定向到您的新 CDN

> *Amazon Route 53 是一个高度可用和可扩展的云[域名系统(DNS)](https://aws.amazon.com/route53/what-is-dns/) web 服务。它旨在为开发者和企业提供一种极其可靠且经济高效的方式，通过将像[www.example.com](http://www.example.com)这样的名字翻译成像 192.0.2.1 这样的数字 IP 地址，让计算机相互连接，从而将最终用户路由到互联网应用程序。*

现在，我们需要将您的域名(mysite.domain)连接到您的 CloudFront 发行版，以便当用户向您的域发出请求时，他们会收到您的`index.html`和您的静态站点的其余部分(通过点击您新创建的 CDN)。

打开 AWS Route53 控制台并创建一个新的**托管区域**。这里有一些帮助你开始的 [AWS 文档](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-inactive.html)。

确保更新你所购买域名的域名服务器。使用您购买的域的注册商提供的方法来更改该域的名称服务器(使用在 NS —名称服务器记录集中找到的四个 Route53 名称服务器)。

创建一个新的记录集，并将别名目标设置为您的 AWS CloudFront 发行版(见下文)。确保将其命名为 yoursite.domain。

![1*gDQ-77A2FQ0n3Qlr47vcaQ](img/e16ec1922b2bf615c0f1f53f8a96373f.png)

Alias your CloudFront distribution

对域名服务器的更改需要时间来传播，因此在尝试通过新域访问您的站点之前，请确保等待几分钟。

#### 第 8 步—享受新部署站点的荣耀！(或解决问题)

你做到了！享受部署到万维网上的美丽的新投资组合网站。查看[我的网站](http://nickvh.tech)我用这个技术栈构建和部署的；我用[这个指南](https://dev.to/adnanrahic/building-a-serverless-contact-form-with-aws-lambda-and-aws-ses-4jm0)把联系表格和 AWS Lambda 连接起来。

![1*lkRw8tZ_QI_tDfz0Tagufg](img/e560b1dd3a175a3e678dc8eb75ac5e40.png)

Enjoy your new beautiful portfolio site! [html5up.com](http://html5up.com)

> *阅读下一条:*

> [scrabbblr——一款 React 游戏，有 react-dnd 和 react-flip-move](https://hackernoon.com/scrabblr-a-react-game-with-react-dnd-and-react-flip-move-40cfaac786e2)