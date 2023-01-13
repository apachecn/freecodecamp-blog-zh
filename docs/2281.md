# 我如何用漂亮的汤做了一个网络刮刀，并用它找到了我的第一份工作

> 原文：<https://www.freecodecamp.org/news/how-i-used-a-side-project-to-land-a-job/>

找到任何一份工作都是一个艰难的过程，更别说是第一份工作了。雇主经常告诉你，你没有足够的经验让他们雇用你。但这意味着你也不会有机会获得这种经验(比如一份工作)。

找一份科技行业的工作更具挑战性。一方面，像其他工作一样，你必须很好地回答面试问题。另一方面，你必须证明你的技能可以胜任你面试的工作。

这些障碍可能很难克服。在这篇文章中，我将分享我是如何建立一个 web scraper 来帮助我在技术领域找到第一份工作的。我将解释我到底构建了什么以及我学到的关键经验。最重要的是，我会分享我是如何利用这些经验赢得面试和工作机会的。

## 我建造的东西

![william-iven-gcsNOsPEXfs-unsplash](img/ca494551937a069e2d8b7f4c5f4d9a8b.png)

当我找第一份工作时，我已经快上大四了。我被安排提前一个学期毕业，所以我把在 12 月前找到一份全职工作作为目标。考虑到这一点，我知道我必须想出创造性的方法来在我的同龄人中脱颖而出。

我有过几次很棒的实习经历(其中一次是在脸书)，但我知道我需要一些东西来充实我的简历。

我发现副业在这方面有很大的潜力。我寻找具有挑战性但可行的项目。这些项目需要展示我的编程技能和对软件的热情。但他们也需要展示我处理零碎数据争论的诀窍(我想进入数据分析类的角色)。

经过进一步的研究，我找到了一个类似于[的有用的教程，它向我展示了如何从网站上搜集数据。该教程启发我建立自己的网络刮刀。但我想收集股票数据，而不是随机的网站。下面是我如何着手建立我的副业项目的细目分类。](https://realpython.com/beautiful-soup-web-scraper-python/)

## 我是如何构建 Web 刮刀的

我做的第一件事是仔细考虑我想要收集的数据类型。当时，我对金融数据很感兴趣。我做了一些研究，发现纳斯达克网站是一个很好的起点。前一年夏天我在脸书实习，所以我想尝试搜集脸书的股票数据:

```
#import libraries
import urllib.request
from bs4 import BeautifulSoup

#specify the url
quote_page = 'https://www.nasdaq.com/symbol/fb/after-hours'

# query the website and return the html to the variable 'page'
page = urllib.request.urlopen(quote_page) 

# parse the html using beautiful soup and store in the variable 'soup'
soup = BeautifulSoup(page, 'html.parser') 

# Take out the <div> of name and get its value
name_box = soup.find('h1')

#define variable for where we'll store the name of our stock
name = name_box.text.strip() # strip() is used to remove starting and trailing
print(name)

# get the index price
price_box = soup.find('div', attrs={'class':'qwidget-dollar'})
# define variable for where we'll store the price of our stock
price = price_box.text
print(price)
```

上面的脚本打印出了当天脸书股票的价格。但我知道我不能就此止步。我需要展示我可以收集股票数据，并利用这些数据做一些事情。我不想让这个项目过于复杂。但是我也想包含足够的分析来打动潜在的雇主。

经过一个月的工作，我想出了一个 Python 脚本，它完成了以下任务:

*   在 30 天内将三家公司的股票价格抓取并附加到一个 CSV 文件中
*   将 CSV 作为数据框架导入，并计算过去 30 天每只股票的平均价格
*   使用 matplotlib 可视化每个公司的股票价格变化

当我买了一台新电脑时，我忘了保存实际的脚本。这里有一些伪代码，以防你们中的任何人想要复制我所做的:

```
#import libraries
import pandas as pd
from datetime import date, datetime, timedelta
import math
import numpy as np

#Scrape and append three companies' stock prices to a pandas dataframe over the course of 30 days

#1) Scrape stock prices for Company A, Company B, and Company C
#2) Append each stock price for the day to a separate column within a CSV using ExcelWriter (pandas function)
#3) Include a single column in the CSV for the date
#4) Repeat until you have 30 days' worth of data for each company

#Calculate the average price for each stock over the course of the 30 days in the dataframe
#1) Import CSV file back into the script as a dataframe
#2) Generate basic statistics (describe() function) for each column or use the .mean() function if you're looking for just the average

#Visualize the average stock price over the last 30 days using matplotlib
#1) Create a different Time Series line plot for Company A, Company B, and Company C 
```

在我完成了剧本的写作和测试之后，我写了一份关于我所做的事情的简要报告。我总结了 scraper 可以做的一切，以及它如何应用于不同的用例。

当我向雇主提及这份报告时，他们很少要求阅读。但我还是把它留在了手边，以备不时之需。

## 我学到的关键经验

![moren-hsu-8mifpgpiyBs-unsplash](img/f0e331d8fbd8f0391aa42e4c081ecdd4.png)

我从这个项目中学到了三个关键的经验。

1.  美丽的 Soup library 是一个有趣而有条理的资源，用于从公共网站上抓取数据(尽管它有点被人瞧不起)。
2.  对于那些想在技术领域，尤其是数据领域开创事业的人来说，有一个有用的框架。该框架完全是关于获取数据(查询数据库、web 抓取等)、格式化数据(Excel、DataFrames 等)以及从中获得洞察力(关键建议、统计数据等)。
3.  构建辅助项目是容易的部分。你必须能够谈论你做了什么，为什么这么做，以及你所做的如何适用于潜在的雇主。

## 我是如何利用这些经验赢得面试和第一份工作的

![linkedin-sales-navigator-W3Jl3jREpDY-unsplash](img/ec9f701fcedab79eb8692233d0c24d87.png)

在我写完 web scraper 和分析的脚本后，我觉得已经准备好开始申请工作了。我需要关注两个关键领域:我的简历和面试。

### 我如何改进我的简历

我需要把重点放在如何在我的简历上展示这个网页抓取器上。我想说明的是，web scraper 可以增加价值，对我申请的公司有益。

首先，我在简历中留出一部分作为副业。然后我列举了我用 Python 用漂亮的 Soup 库搭建了一个 web scraper。

也就是说，我不能只说我做了一个网页抓取器，然后就这样离开简历。我还确保列出了描述我收集的数据类型的要点。我还列出了脚本的组件以及我对数据做了什么。以下是我写下的一些要点:

*   根据年初至今的回报，在纳斯达克指数中排名前十的股票
*   生成一个月的纳斯达克数据，并将其写入 CSV 文件
*   使用纳斯达克数据进行深入的自动化统计分析

### 我是如何准备面试的

我想确定我能向采访者解释 web scraper。我知道我必须首先解释我为什么要创建 web scraper。但是我也需要准备好解释为什么 web scraper 对这份工作很有价值。

这需要两个步骤:确定我在副业项目中使用的硬技能，并将这些技能与工作描述中的关键职责联系起来。

我花了一些时间来思考我做的每一件事，来构建 web scraper。经过头脑风暴，我想出了以下这些硬技能:

*   计算机编程语言
*   擅长
*   网页抓取
*   数据争论
*   数据自动化
*   数据分析
*   统计分析
*   财务预测

对于关键职责，我确保准备好面试答案。我通过将我为 web scraper 使用的一项硬技能与相应的职责联系起来实现了这一点。

让我们从 Hulu 的数据分析师角色中接过这一关键职责:

> 构建数据模型、数据可视化和自动化分析，以便在整个业务中实现可操作性洞察

为此，我准备了一个故事，讲述我如何利用纳斯达克数据创建一个财务预测可视化。我还准备了一个关于我如何自动抓取和分析网页的故事。

## 然后我得到了一份工作

![stefan-stefancik-Ue2-23uBwNw-unsplash](img/dbef04c8efd8c8163d174e1b35063a86.png)

几个星期后，我在一家娱乐公司找到了一份数据分析师的工作。

他们联系了我的申请，并带我经历了面试过程。我点了我的 I，并越过了我的 t。我确保我能把网上搜集的每一项硬技能与工作描述中的关键职责联系起来。

每个采访者都问到了 web scraper 的不同部分。他们对这个附带项目的数据争论和自动化分析部分非常感兴趣。我确信我能解释 web scraper 的这些方面。我的每个面试答案都采用了 S.T.A.R(情境、任务、行动和结果)格式。

四轮面试之后，公司给了我一份全职的有薪工作。录取通知书是在 12 月中旬发出的。

## 去找你的下一个副业吧

![sigmund-Fv2J-aK0Acs-unsplash](img/c517a05e2206176a5dfa73032cf2a91d.png)

当我回顾我获得第一份工作机会的过程时，我很高兴我创建了 web scraper。经历行为和技术方面的面试令人紧张。但我很高兴我有办法准备并通过这些面试。

获得第一份工作可能会很难，因为在你实际做之前，你需要以某种方式获得经验。这是一个潜在的恶性循环，很难打破。

也就是说，请放心，通往成功的道路是明确的。你有一个不可思议的机会来获得创造性的经验(比如建立一个网页抓取器)并获得第一份工作。