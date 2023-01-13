# 如何用 Selenium 和 Python 编写一个抓取机器人

> 原文：<https://www.freecodecamp.org/news/how-to-code-a-scraping-bot-with-selenium-and-python/>

Selenium 是一个旨在帮助您在 web 应用程序中运行自动化测试的工具。它有几种不同的编程语言版本。

虽然不是它的主要用途，但是 Selenium 在 Python 中也用于 web 抓取，因为它可以访问 JavaScript 渲染的内容(这是 BeautifulSoup 等常规抓取工具做不到的)。

当您需要在收集数据之前以某种方式与页面进行交互时，比如单击按钮或填写字段，Selenium 也很有用。这是本文将要涉及的用例。

作为一个例子，我们将使用 investing.com 来提取美元对一种或多种货币汇率的历史数据。

如果你在网上搜索，你会发现 API 和 Python 包使得收集金融数据变得更加容易(而不是手动搜集)。然而，这里的想法是探索 Selenium 如何帮助您进行一般的数据提取。

## 我们要刮的网站

首先，我们需要了解网站。这个网站包含了美元对欧元汇率的历史数据。

在这个页面上，您可以看到一个表格，其中包含数据和设置我们想要的日期范围的选项。这就是我们要用的。

要查看其他货币对美元的数据，只需在 URL 中将“ *eur* ”替换为其他货币代码。

此外，这还假设您只需要货币对美元的汇率。如果不是这样，只需替换 URL 中的“usd”即可。

## 刮刀的代码

我们从进口开始，我们不需要太多。让我们从 Selenium 导入一些有用的项目:在代码中插入一些暂停的`sleep`函数，以及在必要时操纵日期的 Pandas。

```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from time import sleep
import pandas as pd
```

接下来，我们将编写一个函数来抓取数据。该功能将接收:

*   货币代码列表
*   开始日期
*   结束日期
*   一个布尔值，通知我们是否要将数据导出为*。csv* 文件。我将使用 False 作为默认值。

此外，这里的想法是构建一个能够收集多种货币数据的 scraper，我们还将初始化一个空列表来存储每种货币的数据。

```
def get_currencies(currencies, start, end, export_csv=False):
    frames = []
```

由于该函数现在有一个货币列表，您可能会想象我们将迭代这个列表并按货币获取货币数据。这正是我们的计划。

因此，对于货币列表中的每种货币，我们将创建一个 URL，实例化一个驱动程序对象，并使用它来获取页面。然后我们将最大化窗口，但是只有当你保持`option.headless` 为假时才可见。否则，Selenium 将完成所有工作，而不向您显示任何内容。

```
for currency in currencies:
    my_url = f'https://br.investing.com/currencies/usd-{currency.lower()}-historical-data'
    option = Options()
    option.headless = False
    driver = webdriver.Chrome(options=option)
    driver.get(my_url)
    driver.maximize_window()
```

此时，我们已经在查看历史数据，我们可以获得包含数据的表格。但是，默认情况下，我们只能看到最近 20 天的数据。我们希望获得任何时间段的数据。

为此，我们将使用一些有趣的 Selenium 功能与网站进行交互。这就是硒发光的时候！

我们在这里要做的是单击日期，在开始日期和结束日期字段中填入我们想要的日期，然后点击“应用”。

为此，我们将使用`WebDriverWait`、`ExpectedConditions`和`By`来确保 web 驱动程序等待我们想要交互的元素变为可点击的。

这一点很重要，因为如果潜水员试图在某个东西变得可点击之前与之交互，将会引发一个异常。

等待时间将是二十秒，但是由你来设置你认为合适的时间。首先，让我们通过 Xpath 选择日期按钮，然后单击它。

```
date_button = WebDriverWait(driver, 20).until(
              EC.element_to_be_clickable((By.XPATH,
              "/html/body/div[5]/section/div[8]/div[3]/div/div[2]/span")))

date_button.click()
```

现在，我们需要填写开始日期字段。让我们首先选择它，然后使用`clear`删除默认日期，使用`send_keys`填充我们想要的日期。

```
start_bar = WebDriverWait(driver, 20).until(
            EC.element_to_be_clickable((By.XPATH, 
	        "/html/body/div[7]/div[1]/input[1]")))

start_bar.clear()
start_bar.send_keys(start) 
```

现在我们对结束日期字段重复这个过程。

```
end_bar = WebDriverWait(driver, 20).until(
          EC.element_to_be_clickable((By.XPATH, 
          "/html/body/div[7]/div[1]/input[2]")))

end_bar.clear()
end_bar.send_keys(end)
```

完成后，我们将选择“应用”按钮并单击它。然后我们使用`sleep`暂停代码几秒钟，确保新页面被完全加载。

```
apply_button = WebDriverWait(driver, 20).until(
	           EC.element_to_be_clickable((By.XPATH,  
               "/html/body/div[7]/div[5]/a")))

apply_button.click()
sleep(5)
```

如果你将`option.headless` 设为 False，你会看到整个过程就发生在你面前，就好像有人真的点击了页面一样。当 Selenium 单击 Apply 时，您将看到表被重新加载，以显示您指定的时间段的数据。

我们现在使用`pandas.read_html` 函数来选择页面上的所有表格。这个函数将接收页面的源代码。终于可以退出驱动了。

```
dataframes = pd.read_html(driver.page_source)
driver.quit()
print(f'{currency} scraped.')
```

## 如何处理 Selenium 中的异常

收集数据的过程已经完成。但是我们需要考虑到，Selenium 有时会有点不稳定，最终可能会在我们在这里执行的所有操作的某个时刻无法加载页面。

为了防止这种情况，我们将整个代码放在一个无限循环的`try`子句中。一旦 Selenium 设法收集了我上面描述的数据，这个循环就会被打破。但是每次它发现一个问题，就会激活一个`expect`条款。

在这种情况下，代码将:

*   退出驱动程序——这样做总是很重要的，这样我们就不会运行几十个消耗内存的网络驱动程序
*   打印指示错误的消息
*   睡三十秒
*   再次转到循环的起点

这一过程将一直重复，直到正确收集到每种货币的数据。这是这一切的代码:

```
 for currency in currencies:
        while True:
            try:
                # Opening the connection and grabbing the page
                my_url = f'https://br.investing.com/currencies/usd-{currency.lower()}-historical-data'
                option = Options()
                option.headless = False
                driver = webdriver.Chrome(options=option)
                driver.get(my_url)
                driver.maximize_window()

                # Clicking on the date button
                date_button = WebDriverWait(driver, 20).until(
                            EC.element_to_be_clickable((By.XPATH,
                            "/html/body/div[5]/section/div[8]/div[3]/div/div[2]/span")))

                date_button.click()

                # Sending the start date
                start_bar = WebDriverWait(driver, 20).until(
                            EC.element_to_be_clickable((By.XPATH,
                            "/html/body/div[7]/div[1]/input[1]")))

                start_bar.clear()
                start_bar.send_keys(start)

                # Sending the end date
                end_bar = WebDriverWait(driver, 20).until(
                            EC.element_to_be_clickable((By.XPATH,
                            "/html/body/div[7]/div[1]/input[2]")))

                end_bar.clear()
                end_bar.send_keys(end)

                # Clicking on the apply button
                apply_button = WebDriverWait(driver,20).until(
                		EC.element_to_be_clickable((By.XPATH,
                		"/html/body/div[7]/div[5]/a")))

                apply_button.click()
                sleep(5)

                # Getting the tables on the page and quiting
                dataframes = pd.read_html(driver.page_source)
                driver.quit()
                print(f'{currency} scraped.')
                break

            except:
                driver.quit()
                print(f'Failed to scrape {currency}. Trying again in 30 seconds.')
                sleep(30)
                continue 
```

不过，还有最后一步。如果您还记得，到目前为止，我们所拥有的是一个包含页面上所有存储为数据帧的表的列表。我们需要选择一个包含所需历史数据的表。

对于这个数据帧列表中的每个数据帧，我们将检查它的列名是否与我们期望的相匹配。如果他们这样做，那就是我们的框架，我们打破了循环。现在我们终于准备好将这个数据帧添加到开始时初始化的列表中。

```
for dataframe in dataframes:
    if dataframe.columns.tolist() == ['Date', 'Price', 'Open', 'High', 'Low', 'Change%']:
        df = dataframe
        break

frames.append(df)
```

是的，如果参数`export_csv`被设置为真，我们将需要导出一个*。csv* 文件。但是这远不是问题，因为`DataFrame.to_csv`方法可以很容易地完成这项工作。

然后我们可以通过返回数据帧列表来结束这个函数。当然，这最后一步是在遍历完货币列表之后完成的。

```
if export_csv:
        df.to_csv('currency.csv', index=False)
        print(f'{currency}.csv exported.')

# Outside of the loop
return frames
```

就是这样！下面是最后两个步骤的完整代码:

```
 # Selecting the correct table            
        for dataframe in dataframes:
            if dataframe.columns.tolist() == ['Date', 'Price', 'Open', 'High', 'Low', 'Change%']:
                df = dataframe
                break
        frames.append(df)

        # Exporting the .csv file
        if export_csv:
            df.to_csv('currency.csv', index=False)
            print(f'{currency}.csv exported.')

  return frames
```

## 后续步骤和总结

到目前为止，这段代码获取了一系列货币对美元汇率的历史数据，并返回一系列数据帧和几个*。csv* 文件。

但是总有改进的空间。只要多几行代码，就不难让函数返回并导出一个包含列表中每种货币的数据的 DataFrame。

另一个建议是使用相同的 Selenium 功能编写一个`update` 函数，接收现有的数据帧并将历史数据更新到当前日期。

此外，用来刮擦货币的完全相同的逻辑可以用来刮擦股票、指数、商品、期货等等。你可以刮这么多页。

然而，如果这是目标，那么在代码中插入更多的停顿以避免服务器过载是很重要的。你也应该利用一个代理提供者，比如 [Infatica](https://infatica.io/) ，来确保只要还有页面需要清理，你的代码就会一直运行，并且你和你的连接会受到保护。

最后，Selenium 在其他一些情况下也很有用，比如登录网站、填写表单、在下拉列表中选择项目等等。当然，这不是解决此类问题的唯一方法，但是根据不同的用例，它肯定是有用的。

我希望你喜欢这篇文章，它能帮到你。如果您有问题或建议，请随时[联系](https://www.linkedin.com/in/otavioss28/)。