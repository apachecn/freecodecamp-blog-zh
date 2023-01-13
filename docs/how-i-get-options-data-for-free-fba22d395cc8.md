# 我如何免费获得期权数据

> 原文：<https://www.freecodecamp.org/news/how-i-get-options-data-for-free-fba22d395cc8/>

哈利·绍尔斯

# 我如何免费获得期权数据

#### 面向金融的 web 抓取介绍

![3ijdvJH6eCn2kke2e555zvyrpErBZKhZN0fx](img/ae04aa2b43fe40601109729cdb52d513.png)

你是否希望可以访问历史期权数据，但却被收费墙挡住了？如果你只是想用它来做研究，娱乐，或者发展个人交易策略呢？

在本教程中，您将学习如何使用 Python 和 BeautifulSoup 从 Web 上抓取金融数据并构建自己的数据集。

### **入门**

在开始本教程之前，您至少应该具备 Python 和 Web 技术的应用知识。为了建立这些，我强烈推荐去类似于 [codecademy](https://www.codecademy.com/) 的网站学习新技能或者温习旧技能。

首先，让我们启动您最喜欢的 IDE。通常情况下，我使用 [PyCharm](https://www.jetbrains.com/help/pycharm/installation-guide.html) ，但是对于像这样的快速脚本 [Repl.it](https://repl.it/) 也可以完成这项工作。添加快速打印(“Hello world”)以确保您的环境设置正确。

现在我们需要找出一个数据源。

不幸的是， [Cboe 令人敬畏的期权链数据](http://www.cboe.com/delayedquote/quote-table)被完全锁定，即使是对于当前延迟的报价。幸运的是，雅虎财经有足够坚实的期权数据[在这里](https://finance.yahoo.com/quote/SPY/options?p=SPY)。我们将在本教程中使用它，因为 web 抓取工具通常需要一些内容意识，但它很容易适应您想要的任何数据源。

### **依赖关系**

我们不需要太多的外部依赖。我们只需要 Python 中的请求和 BeautifulSoup 模块。将这些添加到程序的顶部:

```
from bs4 import BeautifulSoupimport requests
```

创建一个`main`方法:

```
def main():  print(“Hello World!”)if __name__ == “__main__”:  main()
```

### 抓取 HTML

现在你可以开始刮了！在`main()`中，添加这些行来获取页面的完整`HTML`:

```
data_url = “https://finance.yahoo.com/quote/SPY/options"data_html = requests.get(data_url).contentprint(data_html)
```

这将获取页面的全部内容，因此我们可以在其中找到我们想要的数据。随意试一试，观察输出。

您可以随意注释掉打印语句——这些只是为了帮助您理解程序在任何给定的步骤中正在做什么。

BeautifulSoup 是在 Python 中处理`HTML`数据的完美工具。让我们将`HTML`缩小到期权定价表，以便更好地理解它:

```
 content = BeautifulSoup(data_html, “html.parser”) # print(content)
```

```
 options_tables = content.find_all(“table”) print(options_tables)
```

这仍然是相当大的一部分——我们不能从中获得太多，而且雅虎的代码对网页抓取器来说不是最友好的。让我们把它分成两个表格，分别用于买入和卖出:

```
 options_tables = [] tables = content.find_all(“table”) for i in range(0, len(content.find_all(“table”))):   options_tables.append(tables[i])
```

```
 print(options_tables)
```

雅虎的数据包含了相当多的价内和价外期权，这对于某些目的来说可能是很棒的。我只对接近实际价格的期权感兴趣，即最接近当前价格的两个看涨期权和两个看跌期权。

让我们使用 BeautifulSoup 和雅虎的价内和价外期权的差异表条目来找到这些:

```
expiration = datetime.datetime.fromtimestamp(int(datestamp)).strftime(“%Y-%m-%d”)
```

```
calls = options_tables[0].find_all(“tr”)[1:] # first row is header
```

```
itm_calls = []otm_calls = []
```

```
for call_option in calls:    if “in-the-money” in str(call_option):  itm_calls.append(call_option)  else:    otm_calls.append(call_option)
```

```
itm_call = itm_calls[-1]otm_call = otm_calls[0]
```

```
print(str(itm_call) + “ \n\n “ + str(otm_call))
```

现在，我们有了与`HTML`中的金额最接近的两个选项的表条目。让我们从第一个看涨期权中提取定价数据、交易量和隐含波动率:

```
 itm_call_data = [] for td in BeautifulSoup(str(itm_call), “html.parser”).find_all(“td”):   itm_call_data.append(td.text)
```

```
print(itm_call_data)
```

```
itm_call_info = {‘contract’: itm_call_data[0], ‘strike’: itm_call_data[2], ‘last’: itm_call_data[3],  ‘bid’: itm_call_data[4], ‘ask’: itm_call_data[5], ‘volume’: itm_call_data[8], ‘iv’: itm_call_data[10]}
```

```
print(itm_call_info)
```

将此代码用于下一个呼叫选项:

```
# otm callotm_call_data = []for td in BeautifulSoup(str(otm_call), “html.parser”).find_all(“td”):  otm_call_data.append(td.text)
```

```
# print(otm_call_data)
```

```
otm_call_info = {‘contract’: otm_call_data[0], ‘strike’: otm_call_data[2], ‘last’: otm_call_data[3],  ‘bid’: otm_call_data[4], ‘ask’: otm_call_data[5], ‘volume’: otm_call_data[8], ‘iv’: otm_call_data[10]}
```

```
print(otm_call_info)
```

试一试你的程序吧！

现在你有了两个近钱看涨期权的字典。对于同样的数据，只需整理一下看跌期权表就足够了:

```
puts = options_tables[1].find_all("tr")[1:]  # first row is header
```

```
itm_puts = []  otm_puts = []
```

```
for put_option in puts:    if "in-the-money" in str(put_option):      itm_puts.append(put_option)    else:      otm_puts.append(put_option)
```

```
itm_put = itm_puts[0]  otm_put = otm_puts[-1]
```

```
# print(str(itm_put) + " \n\n " + str(otm_put) + "\n\n")
```

```
itm_put_data = []  for td in BeautifulSoup(str(itm_put), "html.parser").find_all("td"):    itm_put_data.append(td.text)
```

```
# print(itm_put_data)
```

```
itm_put_info = {'contract': itm_put_data[0],                  'last_trade': itm_put_data[1][:10],                  'strike': itm_put_data[2], 'last': itm_put_data[3],                   'bid': itm_put_data[4], 'ask': itm_put_data[5], 'volume': itm_put_data[8], 'iv': itm_put_data[10]}
```

```
# print(itm_put_info)
```

```
# otm put  otm_put_data = []  for td in BeautifulSoup(str(otm_put), "html.parser").find_all("td"):    otm_put_data.append(td.text)
```

```
# print(otm_put_data)
```

```
otm_put_info = {'contract': otm_put_data[0],                  'last_trade': otm_put_data[1][:10],                  'strike': otm_put_data[2], 'last': otm_put_data[3],                   'bid': otm_put_data[4], 'ask': otm_put_data[5], 'volume': otm_put_data[8], 'iv': otm_put_data[10]}
```

恭喜你！你刚刚搜集了标准普尔 500 交易所交易基金所有近价期权的数据，可以这样查看:

```
 print("\n\n") print(itm_call_info) print(otm_call_info) print(itm_put_info) print(otm_put_info)
```

试着运行一下您的程序，您应该可以将这样的数据打印到控制台上:

```
{‘contract’: ‘SPY190417C00289000’, ‘last_trade’: ‘2019–04–15’, ‘strike’: ‘289.00’, ‘last’: ‘1.46’, ‘bid’: ‘1.48’, ‘ask’: ‘1.50’, ‘volume’: ‘4,646’, ‘iv’: ‘8.94%’}{‘contract’: ‘SPY190417C00290000’, ‘last_trade’: ‘2019–04–15’, ‘strike’: ‘290.00’, ‘last’: ‘0.80’, ‘bid’: ‘0.82’, ‘ask’: ‘0.83’, ‘volume’: ‘38,491’, ‘iv’: ‘8.06%’}{‘contract’: ‘SPY190417P00290000’, ‘last_trade’: ‘2019–04–15’, ‘strike’: ‘290.00’, ‘last’: ‘0.77’, ‘bid’: ‘0.75’, ‘ask’: ‘0.78’, ‘volume’: ‘11,310’, ‘iv’: ‘7.30%’}{‘contract’: ‘SPY190417P00289000’, ‘last_trade’: ‘2019–04–15’, ‘strike’: ‘289.00’, ‘last’: ‘0.41’, ‘bid’: ‘0.40’, ‘ask’: ‘0.42’, ‘volume’: ‘44,319’, ‘iv’: ‘7.79%’}
```

### 设置重复数据收集

默认情况下，雅虎只返回你指定日期的选项。就是这部分网址:[https://finance.yahoo.com/quote/SPY/options?date=**1555459200**](https://finance.yahoo.com/quote/SPY/options?date=1555459200)

这是一个 Unix 时间戳，所以我们需要生成或抓取一个，而不是在程序中硬编码。

添加一些依赖项:

```
import datetime, time
```

让我们为下一组选项编写一个快速脚本来生成和验证 Unix 时间戳:

```
def get_datestamp():  options_url = “https://finance.yahoo.com/quote/SPY/options?date="  today = int(time.time())  # print(today)  date = datetime.datetime.fromtimestamp(today)  yy = date.year  mm = date.month  dd = date.day
```

上面的代码保存了我们正在抓取的页面的基本 URL，并生成了一个`datetime.date`对象供我们将来使用。

让我们将这个日期增加一天，这样我们就不会得到已经过期的期权。

```
dd += 1
```

现在，我们需要将其转换回 Unix 时间戳，并确保它是期权合约的有效日期:

```
 options_day = datetime.date(yy, mm, dd) datestamp = int(time.mktime(options_day.timetuple())) # print(datestamp) # print(datetime.datetime.fromtimestamp(options_stamp))
```

```
 # vet timestamp, then return if valid for i in range(0, 7):   test_req = requests.get(options_url + str(datestamp)).content   content = BeautifulSoup(test_req, “html.parser”)   # print(content)   tables = content.find_all(“table”)
```

```
 if tables != []:   # print(datestamp)   return str(datestamp) else:   # print(“Bad datestamp!”)   dd += 1   options_day = datetime.date(yy, mm, dd)   datestamp = int(time.mktime(options_day.timetuple()))  return str(-1)
```

让我们修改我们的`fetch_options`方法，使用动态时间戳来获取期权数据，而不是雅虎想要给我们的默认数据。

更改此行:

```
data_url = “https://finance.yahoo.com/quote/SPY/options"
```

对此:

```
datestamp = get_datestamp()data_url = “https://finance.yahoo.com/quote/SPY/options?date=" + datestamp
```

恭喜你！你只是从网上搜集了现实世界的期权数据。

现在我们需要做一些简单的文件 I/O，并设置一个计时器来记录每天收盘后的数据。

### 改进计划

将`main()`重命名为`fetch_options()`,并将这些行添加到底部:

```
options_list = {‘calls’: {‘itm’: itm_call_info, ‘otm’: otm_call_info}, ‘puts’: {‘itm’: itm_put_info, ‘otm’: otm_put_info}, ‘date’: datetime.date.fromtimestamp(time.time()).strftime(“%Y-%m-%d”)}return options_list
```

创建一个名为`schedule()`的新方法。我们将利用这一点来控制我们在收盘后每 24 小时争取期权的时间。添加以下代码，以便在下次收市时安排我们的第一个作业:

```
from apscheduler.schedulers.background import BackgroundScheduler
```

```
scheduler = BackgroundScheduler()
```

```
def schedule():  scheduler.add_job(func=run, trigger=”date”, run_date = datetime.datetime.now())  scheduler.start()
```

在您的`if __name__ == “__main__”:`语句中，删除`main()`并添加一个对`schedule()`的调用来设置您的第一个计划作业。

创建另一个名为`run()`的方法。这是我们处理大部分业务的地方，包括实际保存市场数据。将此添加到`run()`的正文中:

```
 today = int(time.time()) date = datetime.datetime.fromtimestamp(today) yy = date.year mm = date.month dd = date.day
```

```
 # must use 12:30 for Unix time instead of 4:30 NY time next_close = datetime.datetime(yy, mm, dd, 12, 30)
```

```
 # do operations here “”” This is where we’ll write our last bit of code. “””
```

```
 # schedule next job scheduler.add_job(func=run, trigger=”date”, run_date = next_close)
```

```
 print(“Job scheduled! | “ + str(next_close))
```

这使得我们的代码可以在未来调用自己，所以我们可以把它放在服务器上，每天建立我们的选项数据。添加此代码以实际获取`“”” This is where we’ll write our last bit of code. “””`下的数据

```
options = {}
```

```
 # ensures option data doesn’t break the program if internet is out try:   if next_close > datetime.datetime.now():     print(“Market is still open! Waiting until after close…”)   else:     # ensures program was run after market hours     if next_close < datetime.datetime.now():      dd += 1       next_close = datetime.datetime(yy, mm, dd, 12, 30)       options = fetch_options()       print(options)       # write to file       write_to_csv(options)except:  print(“Check your connection and try again.”)
```

### 保存数据

您可能已经注意到`write_to_csv`还没有实现。别担心，让我们在这里解决这个问题:

```
def write_to_csv(options_data):  import csv  with open(‘options.csv’, ‘a’, newline=’\n’) as csvfile:  spamwriter = csv.writer(csvfile, delimiter=’,’)  spamwriter.writerow([str(options_data)])
```

### **清理**

由于期权合同是时间敏感的，我们可能希望添加一个字段来表示它们的到期日期。我们收集的原始 HTML 中不包含这一功能。

添加这行代码以保存到期日期并将其格式化到`fetch_options()`的顶部:

```
expiration =  datetime.datetime.fromtimestamp(int(get_datestamp())).strftime("%Y-%m-%d")
```

将`‘expiration’: expiration`添加到每个`option_info`字典的末尾，如下所示:

```
itm_call_info = {'contract': itm_call_data[0],  'strike': itm_call_data[2], 'last': itm_call_data[3],   'bid': itm_call_data[4], 'ask': itm_call_data[5], 'volume': itm_call_data[8], 'iv': itm_call_data[10], 'expiration': expiration}
```

试一试您的新程序——它会抓取最新的选项数据，并将其作为字典的字符串表示形式写入. csv 文件。的。csv 文件将准备好被回溯测试程序解析或通过 webapp 提供给用户。恭喜你！