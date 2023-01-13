# Python 网页抓取教程——如何用 Python 从任何网站抓取数据

> 原文：<https://www.freecodecamp.org/news/how-to-scrape-websites-with-python-2/>

Web 抓取是从互联网上自动提取特定数据的过程。它有许多用例，如为机器学习项目获取数据，创建价格比较工具，或任何其他需要大量数据的创新想法。

虽然理论上您可以手动提取数据，但是互联网的大量内容使得这种方法在许多情况下不切实际。因此，知道如何建立一个网页刮刀可以派上用场。

这篇文章的目的是教你如何用 Python 创建一个 web scraper。您将学习如何检查网站以准备抓取，使用 BeautifulSoup 提取特定数据，使用 Selenium 等待 JavaScript 呈现，以及将所有内容保存在新的 JSON 或 CSV 文件中。

但是首先，我应该警告你网络抓取的合法性。虽然刮擦行为是合法的，但您可能提取的数据可能是非法使用的。确保你没有弄乱任何:

*   受版权保护的内容——因为它是某人的知识产权，所以受法律保护，你不能随便重复使用。
*   个人数据——如果您收集的信息可以用来识别一个人的身份，那么它被视为个人数据，对于欧盟公民来说，它受 GDPR 的保护。除非你有合法的理由存储这些数据，否则最好干脆跳过它。

一般来说，你应该在抓取之前阅读网站的条款和条件，以确保你没有违反他们的政策。如果您不确定如何继续，请联系网站所有者并征得同意。

## 你的铲运机需要什么？

要开始构建你自己的 web scraper，你首先需要在你的机器上安装 [Python](https://www.python.org/downloads/) 。Ubuntu 20.04 和其他版本的 Linux 预装了 Python 3。

要检查您的设备上是否已经安装了 Python，请运行以下命令:

```
python3 -v 
```

如果您安装了 Python，您应该会收到如下输出:

```
Python 3.8.2
```

此外，对于我们的 web scraper，我们将使用 Python 包 BeautifulSoup(用于选择特定数据)和 Selenium(用于呈现动态加载的内容)。要安装它们，只需运行以下命令:

```
pip3 install beautifulsoup4
```

和

```
pip3 install selenium
```

最后一步是确保你在电脑上安装了谷歌浏览器和 T2 浏览器的驱动程序。如果我们想使用 Selenium 来抓取动态加载的内容，这些将是必要的。

## 如何检查页面

现在你已经安装好了所有的东西，是时候认真开始我们的刮擦项目了。

你要根据自己的需求来选择你要刮的网站。请记住，每个网站的内容结构都不同，所以当你开始自己抓取时，你需要调整你在这里学到的东西。每个网站都需要对代码进行细微的修改。

为了这篇文章，我决定从《https://www.imdb.com/chart/top/》和《IMDb》的前 250 部电影中搜集前十部电影的信息。

首先，我们将获得标题，然后我们将通过从每部电影的页面提取信息来进一步深入研究。有些数据需要 JavaScript 呈现。

要开始理解内容的结构，您应该右键单击列表中的第一个标题，然后选择“Inspect Element”。

![e6DE3zczzQa-VSBIynK-fR4oyAjVbpx2PztpEDKbi3K0NII9_lFkFhGQmiOjc_-Y_Kg26cM3pecnSKNiPlLZGpntqVKUrcX9E4gDWaTsolWoCFzQ6EEhj3GruBvrlEIzrUffvdjU](img/ac2f7bf6fb1f7baddd9b7ab2b0b637dd.png)

按 CTRL+F，在 HTML 代码结构中搜索，会看到页面上只有一个 **<表>** 标签。这很有用，因为它为我们提供了如何访问数据的信息。

一个 HTML 选择器将为我们提供页面中的所有标题，这个选择器就是 **`table tbody tr td.titleColumn a`** 。这是因为所有的标题都在一个名为“titleColumn”的表格单元格中。

使用这个 CSS 选择器并获取每个锚点的 **innerText** 将会给出我们需要的标题。您可以在刚刚打开的新窗口中，通过使用 JavaScript 行在浏览器控制台中进行模拟:

```
document.querySelectorAll("table tbody tr td.titleColumn a")[0].innerText
```

您将看到类似这样的内容:

![T1pgLUXJHX_s3gubDKvBjwkWeK1neZxiysoneD2Q1NU3Sj_pD8defdKorTlcsiiqShlmPDEeCu3Goo5T9CgzPKCml9dq_kCCu7KUyTx7uSrU8VN9QzJZhO6AwBM-kfQ8r0uNxbn9](img/b559afe189a2d3f8e3f9adaf352985fb.png)

现在我们有了这个选择器，我们可以开始编写 Python 代码并提取我们需要的信息。

## 如何使用 BeautifulSoup 提取静态加载的内容

我们列表中的电影标题是静态内容。这是因为如果您查看页面源代码(在页面上按 CTRL+U 或右键单击，然后选择查看页面源代码)，您会看到标题已经在那里了。

静态内容通常更容易抓取，因为它不需要 JavaScript 渲染。为了提取列表中的前十个标题，我们将使用 BeautifulSoup 获取内容，然后将其打印在我们的 scraper 的输出中。

```
import requests
from bs4 import BeautifulSoup

page = requests.get('https://www.imdb.com/chart/top/') # Getting page HTML through request
soup = BeautifulSoup(page.content, 'html.parser') # Parsing content using beautifulsoup

links = soup.select("table tbody tr td.titleColumn a") # Selecting all of the anchors with titles
first10 = links[:10] # Keep only the first 10 anchors
for anchor in first10:
    print(anchor.text) # Display the innerText of each anchor 
```

上面的代码使用我们在第一步中看到的选择器从页面中提取电影标题锚点。然后它遍历前十个并显示每个的内部文本。

输出应该如下所示:

![RrmEldjCrbz7V1-o4r6UsKNuWkj_yD2cWwfyuMMbdnRn7lk9cI0yhMi85PK4NrvX7L2KY0pY8047f9CmAeXo1W51HvFENMPxxh36ACqu3kNKuoFNNfhB_WSCMntIB-UB0usEU2n5](img/dc67378370e8bb76fd05acb7126ebed7.png)

## 如何提取动态加载的内容

随着技术的进步，网站开始动态加载内容。这改善了页面的性能，用户的体验，甚至为抓取工具清除了一个额外的障碍。

不过，这使事情变得复杂了，因为从简单请求中检索到的 HTML 将不包含动态内容。幸运的是，有了 Selenium，我们可以在浏览器中模拟一个请求，并等待动态内容显示出来。

### 如何使用 Selenium 进行请求

你需要知道你的 chrome 驱动的位置。下面的代码与第二步中的代码相同，但是这次我们使用 Selenium 来发出请求。我们仍将使用 BeautifulSoup 解析页面内容，就像我们之前做的那样。

```
from bs4 import BeautifulSoup
from selenium import webdriver

option = webdriver.ChromeOptions()
# I use the following options as my machine is a window subsystem linux. 
# I recommend to use the headless option at least, out of the 3
option.add_argument('--headless')
option.add_argument('--no-sandbox')
option.add_argument('--disable-dev-sh-usage')
# Replace YOUR-PATH-TO-CHROMEDRIVER with your chromedriver location
driver = webdriver.Chrome('YOUR-PATH-TO-CHROMEDRIVER', options=option)

driver.get('https://www.imdb.com/chart/top/') # Getting page HTML through request
soup = BeautifulSoup(driver.page_source, 'html.parser') # Parsing content using beautifulsoup. Notice driver.page_source instead of page.content

links = soup.select("table tbody tr td.titleColumn a") # Selecting all of the anchors with titles
first10 = links[:10] # Keep only the first 10 anchors
for anchor in first10:
    print(anchor.text) # Display the innerText of each anchor 
```

不要忘记将“你的 chrome 驱动路径”替换为你提取 chrome 驱动的位置。另外，您应该注意到，当我们创建 BeautifulSoup 对象时，我们现在使用的是提供页面 HTML 内容的 **`driver.page_source`** ，而不是 **`page.content`** 。

### 如何使用 Selenium 提取静态加载的内容

使用上面的代码，我们现在可以通过调用每个锚点上的 click 方法来访问每个电影页面。

```
first_link = driver.find_elements_by_css_selector('table tbody tr td.titleColumn a')[0]
first_link.click() 
```

这将模拟点击第一部电影的链接。不过，在这种情况下，我建议你继续使用 **`driver.get instead`** 。这是因为当你进入另一个页面后，你将无法再使用 **`click()`** 方法，因为新页面没有其他九部电影的链接。

因此，点击列表中的第一个标题后，您需要返回到第一页，然后点击第二个，依此类推。这是对性能和时间的浪费。相反，我们将只使用提取的链接并逐个访问它们。

对于《肖申克的救赎》，电影页面将是[https://www.imdb.com/title/tt0111161/](https://www.imdb.com/title/tt0111161/)。我们将从页面中提取电影的年份和时长，但这次我们将使用 Selenium 的功能而不是 BeautifulSoup 作为示例。在实践中，你可以使用任何一个，所以选择你最喜欢的。

要检索电影的年份和持续时间，您应该重复我们在电影页面上完成的第一步。

您会注意到，您可以在第一个元素中找到带有类 **`ipc-inline-list`** ("的所有信息。ipc-inline-list”选择器)，并且列表的所有元素都包含具有值**`presentation`**`[role=’presentation’]`选择器的属性 **`role`** 。

```
from bs4 import BeautifulSoup
from selenium import webdriver

option = webdriver.ChromeOptions()
# I use the following options as my machine is a window subsystem linux. 
# I recommend to use the headless option at least, out of the 3
option.add_argument('--headless')
option.add_argument('--no-sandbox')
option.add_argument('--disable-dev-sh-usage')
# Replace YOUR-PATH-TO-CHROMEDRIVER with your chromedriver location
driver = webdriver.Chrome('YOUR-PATH-TO-CHROMEDRIVER', options=option)

page = driver.get('https://www.imdb.com/chart/top/') # Getting page HTML through request
soup = BeautifulSoup(driver.page_source, 'html.parser') # Parsing content using beautifulsoup

totalScrapedInfo = [] # In this list we will save all the information we scrape
links = soup.select("table tbody tr td.titleColumn a") # Selecting all of the anchors with titles
first10 = links[:10] # Keep only the first 10 anchors
for anchor in first10:
    driver.get('https://www.imdb.com/' + anchor['href']) # Access the movie’s page
    infolist = driver.find_elements_by_css_selector('.ipc-inline-list')[0] # Find the first element with class ‘ipc-inline-list’
    informations = infolist.find_elements_by_css_selector("[role='presentation']") # Find all elements with role=’presentation’ from the first element with class ‘ipc-inline-list’
    scrapedInfo = {
        "title": anchor.text,
        "year": informations[0].text,
        "duration": informations[2].text,
    } # Save all the scraped information in a dictionary
    totalScrapedInfo.append(scrapedInfo) # Append the dictionary to the totalScrapedInformation list

print(totalScrapedInfo) # Display the list with all the information we scraped 
```

### 如何使用 Selenium 提取动态加载的内容

web 抓取的下一个重要步骤是提取动态加载的内容。你可以在电影的每一页(比如[https://www.imdb.com/title/tt0111161/](https://www.imdb.com/title/tt0111161/))的编辑列表部分找到这样的内容。

如果您在页面上使用 inspect 查看，您会看到您可以找到一个元素，该元素的属性 **`data-testid`** 设置为 **`firstListCardGroup-editorial`** 。但是如果您查看页面源代码，您将不会在任何地方找到这个属性值。这是因为编辑列表部分是由 IMDB 动态加载的。

在下面的例子中，我们将抓取每部电影的编辑列表，并将其添加到我们当前抓取的全部信息的结果中。

为此，我们将导入更多的包，以便等待动态内容的加载。

```
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

option = webdriver.ChromeOptions()
# I use the following options as my machine is a window subsystem linux. 
# I recommend to use the headless option at least, out of the 3
option.add_argument('--headless')
option.add_argument('--no-sandbox')
option.add_argument('--disable-dev-sh-usage')
# Replace YOUR-PATH-TO-CHROMEDRIVER with your chromedriver location
driver = webdriver.Chrome('YOUR-PATH-TO-CHROMEDRIVER', options=option)

page = driver.get('https://www.imdb.com/chart/top/') # Getting page HTML through request
soup = BeautifulSoup(driver.page_source, 'html.parser') # Parsing content using beautifulsoup

totalScrapedInfo = [] # In this list we will save all the information we scrape
links = soup.select("table tbody tr td.titleColumn a") # Selecting all of the anchors with titles
first10 = links[:10] # Keep only the first 10 anchors
for anchor in first10:
    driver.get('https://www.imdb.com/' + anchor['href']) # Access the movie’s page 
    infolist = driver.find_elements_by_css_selector('.ipc-inline-list')[0] # Find the first element with class ‘ipc-inline-list’
    informations = infolist.find_elements_by_css_selector("[role='presentation']") # Find all elements with role=’presentation’ from the first element with class ‘ipc-inline-list’
    scrapedInfo = {
        "title": anchor.text,
        "year": informations[0].text,
        "duration": informations[2].text,
    } # Save all the scraped information in a dictionary
    WebDriverWait(driver, 5).until(EC.visibility_of_element_located((By.CSS_SELECTOR, "[data-testid='firstListCardGroup-editorial']")))  # We are waiting for 5 seconds for our element with the attribute data-testid set as `firstListCardGroup-editorial`
    listElements = driver.find_elements_by_css_selector("[data-testid='firstListCardGroup-editorial'] .listName") # Extracting the editorial lists elements
    listNames = [] # Creating an empty list and then appending only the elements texts
    for el in listElements:
        listNames.append(el.text)
    scrapedInfo['editorial-list'] = listNames # Adding the editorial list names to our scrapedInfo dictionary
    totalScrapedInfo.append(scrapedInfo) # Append the dictionary to the totalScrapedInformation list

print(totalScrapedInfo) # Display the list with all the information we scraped 
```

对于前面的示例，您应该得到以下输出:

![geHhbKeeP2ATtz-OnIx9MATB3UvXcrobnO4eUNOLrzQll9ebPlq_2PqKaT_oT6e-3h7NmRkRh_9mrDuSvuW3Wbs3sRi1iuM3paCa8HBpTqWrZuSQc8sIu5y4EVZ_5j-60TmPs71Z](img/53e7f6239ff6fa8cad2c82bdf9f0193b.png)

## 如何保存刮掉的内容

既然我们已经有了所有想要的数据，我们可以将它保存为. json 或. csv 文件，以便于阅读。

为此，我们将使用 Python 中的 JSON 和 CVS 包，并将我们的内容写入新文件:

```
import csv
import json

...

file = open('movies.json', mode='w', encoding='utf-8')
file.write(json.dumps(totalScrapedInfo))

writer = csv.writer(open("movies.csv", 'w'))
for movie in totalScrapedInfo:
    writer.writerow(movie.values()) 
```

## 刮痧技巧和窍门

虽然到目前为止，我们的指南已经足够先进，可以处理 JavaScript 呈现场景，但在 Selenium 中仍有许多东西需要探索。

在这一部分，我将分享一些可能会派上用场的技巧和窍门。

### 1.为你的请求计时

如果你在短时间内向服务器发送数百个请求，很有可能在某个时候会出现验证码，或者你的 IP 地址会被屏蔽。不幸的是，Python 中没有避免这种情况的变通方法。

因此，您应该在每个请求之间设置一些超时间隔，以便流量看起来更自然。

```
import time
import requests

page = requests.get('https://www.imdb.com/chart/top/') # Getting page HTML through request
time.sleep(30) # Wait 30 seconds
page = requests.get('https://www.imdb.com/') # Getting page HTML through request 
```

### 2.错误处理

由于网站是动态的，它们可以随时改变结构，如果你经常使用同一个 web scraper，错误处理可能会很方便。

```
try:
    WebDriverWait(driver, 5).until(EC.presence_of_element_located((By.CSS_SELECTOR, "your selector")))
    break
except TimeoutException:
    # If the loading took too long, print message and try again
    print("Loading took too much time!")
```

当您等待一个元素、提取它，甚至当您只是发出请求时，try and error 语法会很有用。

### 3.截图

如果您需要随时获取正在抓取的网页的屏幕截图，您可以使用:

```
driver.save_screenshot(‘screenshot-file-name.png’)
```

这有助于在处理动态加载的内容时进行调试。

### 4.阅读文档

最后但同样重要的是，不要忘记阅读来自 Selenium 的[文档。该库包含有关如何在浏览器中执行大多数操作的信息。](https://selenium-python.readthedocs.io/)

使用 Selenium，您可以填写表单、按按钮、回答弹出消息以及做许多其他很酷的事情。

如果你面临一个新问题，他们的文档可以成为你最好的朋友。

## 最后的想法

本文的目的是向您提供使用 Python 和 Selenium 以及 BeautifulSoup 进行 web 抓取的高级介绍。虽然这两种技术仍有许多特性需要探索，但您现在已经有了如何开始抓取的坚实基础。

有时候网页抓取非常困难，因为网站开始给开发者设置越来越多的障碍。这些障碍包括验证码、IP 地址块或动态内容。仅仅用 Python 和 Selenium 来克服它们可能很困难，甚至是不可能的。

所以，我也给你一个选择。尝试使用一个[网络抓取 API](https://webscrapingapi.com) 来为你解决所有这些挑战。它还使用旋转代理，因此您不必担心在请求之间添加超时。请记住，始终检查您想要的数据是否可以合法提取和使用。