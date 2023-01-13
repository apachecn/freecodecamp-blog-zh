# 如何使用 Python 抓取网站

> 原文：<https://www.freecodecamp.org/news/scrap-websites-using-python-c0c7ad41d2dd/>

作者:德万舒·贾恩

这是一年中的这个时候，在印度超级联赛板球 T20 锦标赛期间，空气中充满了 4 轮和 6 轮比赛的掌声和欢呼声，随后是在英格兰举行的国际刑事法院板球世界杯。我们怎么能忘记世界上最大的民主国家印度的选举结果呢？它将在未来几周内公布。

为了了解谁将获得今年的 IPL 冠军，或者哪个国家将获得 2019 年的 ICC 世界杯，或者这个国家的未来在未来 5 年将会如何，我们需要不断关注互联网。

但是，如果你像我一样，不能在互联网上花太多时间，但是有强烈的愿望保持更新所有这些标题，那么这篇文章是给你的。所以不浪费任何时间，让我们开始吧！

![dySuWsAk2qARPtM0cOs88-YztYwzc1fWPz1F](img/170f7d21b65a971bbfb7339ab4cb886a.png)

Photo by [Balázs Kétyi](https://unsplash.com/photos/sScmok4Iq1o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们有两种方法可以获取更新的信息。一种方式是通过这些媒体网站提供的 API，另一种方式是通过 Web/内容抓取。

API 的方式太简单了，可能获得更新信息的最佳方式是调用相关的编程接口。但可悲的是，并不是所有的网站都提供可公开访问的 API。所以我们剩下的另一条路就是网刮。

### **网页抓取**

网络抓取是一种从网站中提取信息的技术。这种技术主要关注于将 web 上的非结构化数据(HTML 格式)转换成结构化数据(数据库或电子表格)。Web 抓取可能涉及使用 HTTP 直接访问 web，或者通过 web 浏览器访问 web。

在本文中，我们将使用 Python 创建一个从网站抓取内容的机器人。

### **流程工作流程**

*   获取我们要从中提取/抓取数据的页面的 URL
*   复制/下载页面的 HTML 内容
*   解析 HTML 内容并获取所需的数据

上面的流程帮助我们导航到所需页面的 URL，获取其 HTML 内容，并解析所需的数据。但有时会出现这样的情况，我们首先要登录网站，然后导航到一个特定的位置来获取所需的数据。在这种情况下，就多了一个登录网站的步骤。

### **套餐**

为了解析 HTML 内容并获得所需的数据，我们使用了 **Beautiful Soup** 库。这是一个用于解析 HTML 和 XML 文档的了不起的 Python 包。请在此查看[。](https://code.launchpad.net/beautifulsoup/)

为了登录网站，在同一会话中导航到所需的 URL，以及下载 HTML 内容，我们将使用 **Selenium** 库。 [Selenium Python](https://selenium-python.readthedocs.io/) 帮助点击按钮、在结构中输入内容等等。

### **直接进入代码**

首先，我们要导入我们将要使用的所有库。

```
# importing librariesfrom selenium import webdriverfrom bs4 import BeautifulSoup
```

接下来，我们需要给浏览器的驱动程序到 Selenium 的路径，以启动我们的 web 浏览器(Google Chrome)。如果我们不想让我们的机器人显示浏览器的 GUI，那么我们可以给 Selenium 添加 **headless** 选项。无头浏览器在类似于流行的 web 浏览器的环境中提供对网页的自动控制，但是通过命令行界面或使用网络通信来执行。

```
# chrome driver pathchromedriver = '/usr/local/bin/chromedriver'options = webdriver.ChromeOptions()options.add_argument('headless')  # for opening headless browser
```

```
browser = webdriver.Chrome(executable_path=chromedriver, chrome_options=options)
```

在通过定义浏览器和安装库建立了环境之后，我们将着手处理 HTML。导航到登录页面，找到电子邮件、密码和提交按钮的字段 id、类或名称，将我们的内容输入到页面结构中。

```
# Navigating to the login pagebrowser.get('http://playsports365.com/default.aspx')
```

```
#Finding the tags by nameemail = browser.find_element_by_name('ctl00$MainContent$ctlLogin$_UserName')
```

```
password = browser.find_element_by_name('ctl00$MainContent$ctlLogin$_Password')
```

```
login = browser.find_element_by_name('ctl00$MainContent$ctlLogin$BtnSubmit')
```

接下来，我们将通过单击 submit 按钮将凭证发送到这些 HTML 标记中，以便在页面结构中输入我们的内容。

```
# appending login credentialsemail.send_keys('********')password.send_keys('*******')
```

```
# clicking submit buttonlogin.click()
```

登录成功后，导航到所需的页面并获取页面的 HTML 内容

```
# After successful login, navigating to Open Bets Pagebrowser.get('http://playsports365.com/wager/OpenBets.aspx')
```

```
# Getting HTML content and parsing itrequiredHtml = browser.page_source
```

现在，我们已经收到了 HTML 内容，剩下的唯一工作就是解析这些内容。我们将使用漂亮的 Soup 和 html5lib 库解析内容。 [**html5lib**](http://code.google.com/p/html5lib/) 是一个 Python 包，实现了深受当前浏览器影响的 HTML5 解析算法。一旦我们获得了解析内容的规范化结构，我们就可以在 HTML 标签的任何子标签中找到我们的数据。我们的数据存在于 table 标记中，这就是为什么我们要搜索那个标记。

```
soup = BeautifulSoup(requiredHtml, 'html5lib')table = soup.findChildren('table')my_table = table[0]
```

一旦我们找到了父标签，我们只需要递归地遍历它的子标签并输出值。

```
# fetching tags and printing valuesrows = my_table.findChildren(['th', 'tr'])for row in rows:    cells = row.findChildren('td')    for cell in cells:        value = cell.text        print (value)
```

要执行上面的程序，使用 [pip](https://pip.pypa.io/en/stable/installing/) 安装 Selenium、Beautiful Soup 和 html5lib 库。安装完库之后，输入`#python <program na` me >会将值打印到控制台。

通过这种方式，我们可以从任何网站上搜集和查找数据。

现在，如果我们正在抓取一个内容变化非常频繁的网站，比如板球比分或现场选举结果，我们可以在 cron 作业中运行这个程序，并为 cron 作业设置一个时间间隔。

除此之外，我们还可以将结果显示在我们的屏幕上，而不是控制台上，方法是在特定时间间隔后，在桌面上弹出的通知选项卡中打印结果。我们甚至可以在消息客户端上共享这些值。Python 有丰富的库可以帮助我们完成所有这些。

如果你想让我解释如何设置一个 cron 任务，并让通知出现在桌面上，欢迎在评论区问我。

下次再见，我希望你喜欢这篇文章。