# Node.js 网络抓取终极指南

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-web-scraping-with-node-js-daa2027dcd3/>

那么什么是网络抓取呢？它包括自动化从网站上收集信息的繁重任务。

web 抓取有很多使用案例:您可能想从各种电子商务网站收集价格，用于价格比较网站。或者你可能需要一个旅游网站的航班时间和酒店/AirBNB 列表。也许你想从各种目录中收集电子邮件来寻找销售线索，或者使用互联网上的数据来训练机器学习/AI 模型。或者你甚至想建立一个像谷歌一样的搜索引擎！

开始使用网页抓取很容易，这个过程可以分为两个主要部分:

*   使用 HTML 请求库或无头浏览器获取数据，
*   并解析数据以获得您想要的确切信息。

本指南将带你了解流行的 Node.js [请求-承诺](https://github.com/request/request-promise)模块、 [CheerioJS](https://github.com/cheeriojs/cheerio) 和[木偶师](https://github.com/GoogleChrome/puppeteer)的流程。通过阅读本指南中的示例，您将学到成为专业人士所需的所有技巧和诀窍，以便使用 Node.js 收集所需的任何数据！

我们将从维基百科收集所有美国总统的姓名和生日，以及 Reddit 首页上所有帖子的标题。

首先:让我们安装我们将在本指南中使用的库(Puppeteer 需要一段时间来安装，因为它也需要下载 Chromium)。

### 提出你的第一个请求