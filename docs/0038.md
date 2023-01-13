# 如何使用 Python 和 API 自动化您的业务策略

> 原文：<https://www.freecodecamp.org/news/how-to-automate-e-commerce-operations-with-python/>

当你从事电子商务运营时，你每天都要执行许多任务来实现公司的商业战略。

但是这些经常重复的任务可能很费时，并且容易出错。其中一些错误会让您的公司损失金钱、声誉和时间。

幸运的是，Python 和 API 可以帮助你避免这样的错误，同时节省时间和金钱。然后，你的团队可以将这些时间和金钱投入到其他活动中，比如开发新的策略或方法来提高效率。

## 你可能以前听说过 API

如果你在科技行业工作，你可能一生中至少听说过一次 API。如果你从未听说过，不要担心，我会在这里给你一个简单的解释。

**“API”**代表**应用编程接口**。它们帮助服务或应用程序相互通信。

举个例子，如果你是一个前端开发者，你会被要求通过 API 调用一个后端服务来获取你想要在屏幕上打印的信息。这就是后端和前端发送和接收数据的方式。当您调用一个 API 时，您会得到一个 JSON 或 XML 文件作为响应。

API 也可以是**“RESTful”**，代表**表象状态转移**。Rest 是一组架构约束，帮助您设计高效、安全的 API。

如果你想了解更多关于 REST APIs 的知识，我建议你[在 freeCodeCamp 的出版物上阅读 Ihechikara](https://www.freecodecamp.org/news/what-is-rest-rest-api-definition-for-beginners/) 的这篇文章。

## API 如何帮助你完成工作？

正如我所说的，API 是关于交换信息的。即使您没有开发前端应用程序，您也可以检索您需要的数据，并操纵或处理它们以获得您需要的信息。然后，您可以要求另一个服务执行您希望它执行的操作。

我将给出一个日常生活中的例子，向您展示如何使用 API 来自动化任务，以帮助实现您的业务战略。

### API 用例示例

假设我们的公司销售雨伞，我们想根据我们商店所在城市的天气更新我们的价目表。下雨时，我们希望雨伞的价格上涨，而天气好时，我们希望价格下降。在任何其他情况下，我们都希望价格保持不变。

当然，这是一个过于简化的场景，但我认为这是一个很好的例子来展示 API 可以多么强大地帮助你在线自动化你的商业策略。

如果没有任何自动化，您将不得不每天查看天气预报并手动更新商品价格。正如我在本文开始时所说的，这种类型的手动过程非常耗时，并且容易出错。此外，不要忘记你需要有人一年 365 天执行这些任务。

谢天谢地，我们可以自动化这个过程。这是我们需要的:

*   Python (3.10 及以上版本)
*   一个公开 API 的 CMS/电子商务(我在本教程中使用的是 Woocommerce)
*   天气预报宣传短片

### 如何使用 Python 和 API 自动更新

我们将实现一个用 Python 编写的批处理——每 24 小时执行一次——根据我们调用的天气预报 API 的响应来更新雨伞的价格。

让我们深入脚本，一步一步地分析。

首先，我声明环境变量。这是列表:

```
API_BASEURL #The baseurl of the weather forecast service I'm calling

API_CITY #The city I want to get weather information about

API_COUNTRY #The country where API_CITY is located 

API_KEY # The secret Key I must pass to retrieve the weather information

CONSUMER_KEY #The key I need to work with Woocommerce APIs 

CONSUMER_SECRET #The secret I need to work with Woocommerce APIs

DOMAIN #The domain of my ecommerce
```

然后，我开始导入我需要的库:

```
import os
from dotenv import load_dotenv
```

我导入了 **os** ，还导入了 **load_dotenv** 来处理环境变量。

然后我导入请求和 JSON 来处理 API 的响应。

```
import requests
import json
```

然后我从 Woocommerce Python 库中导入 API 模块。您可以在其丰富的[文档](https://woocommerce.github.io/woocommerce-rest-api-docs/?python#libraries-and-tools)中找到更多信息。

```
from woocommerce import API
```

一旦导入了我需要的所有库和模块，我将实例化我将在脚本中使用的变量:

```
load_dotenv()
product_to_sell_id = "1736" #The Id related to umbrellas in my e-commerce
raised_price = "12" #The price I want to set when it rains
lowered_price = "8" #The price I want to set when weather is good
regular_price = "10" #The regular price of the item 
```

然后我声明 **changePrice** 函数:

```
def changePrice(price, idProduct):

    wcapi = API(
        url= os.getenv('DOMAIN'), 
        consumer_key= os.getenv('CONSUMER_KEY'), 
        consumer_secret= os.getenv('CONSUMER_SECRET'), 
        wp_api=True, 
        version="wc/v3" 
    )

    data = {
        "regular_price": price
    }

    wcapi.put("products/" + idProduct, data).json()

    print("New price set to " + data["regular_price"])
```

该函数有两个参数:我要设置的价格(price)和我要更新的产品的 id(id product)。

在函数内部，我将 api 方法存储在变量 **wcapi** 中，该方法包含我需要的信息以到达我的电子商务(baseurl、消费者密钥和秘密)。

我在**数据**变量中存储了我想要传递的有效负载:我想要用参数“price”的值来更新键“regular_price”的值。

之后，我用 PUT 方法更新电子商务数据库，方法是传递我要更新的产品的路径(它是字符串“products/”和参数“idProduct”的值的串联)和数据变量值。

最后，我打印了一条成功消息。

一旦 **changePrice** 函数准备就绪，我需要定义一个新的函数来使脚本工作。但是在我们继续之前，我们需要看到我们从 API 调用中获得的数据。

我们作为响应返回的 JSON 文件返回了很多信息。但是，正如我在开头所说的，我们只是想知道天气怎么样。

我们用“代码”键获取这些数据。每种天气状况都与一个数字相关联。在我们的例子中，我们关注这两个代码:

```
502 #rainy
800 #sunny
```

现在我们有了我们需要的东西，我们定义了 **getWeather** 函数:

```
def getWeather():

    url = os.getenv('API_BASEURL')

    headers = {
    "Accept": "application/json"
    }

    payload = {
    "key": os.getenv('API_KEY'),
    "city": os.getenv('API_CITY'),
    "country": os.getenv('API_COUNTRY')
    }

    response = requests.request(
    "GET",
    url,
    params=payload,
    headers=headers  
    )

    data = response.text

    parse_json = json.loads(data)

    get_parse_result = parse_json["data"][0]["weather"]["code"]

    match get_parse_result:
        case 502:
            changePrice(raised_price, product_to_sell_id)

        case 800:
            changePrice(lowered_price, product_to_sell_id)

        case _:
            changePrice(regular_price, product_to_sell_id)
```

在 **url** 变量中，我们定义了我们调用的 API 的基本 url。在 payload 变量中，我传递了我们的密钥、城市以及我想了解天气的国家。

我通过传递 URL、有效负载和头来执行 GET 请求。一旦我得到响应，我就解析它，并用 3 种情况实现一个 switch 语句:

*   **get_parse_result** 等于 502:我们调用 changePrice 函数，将 raised_price 变量作为第一个参数传递，以提高产品价格
*   **get_parse_result** 等于 800:我们调用 changePrice 函数，将 lowered_price 变量作为第一个参数传递，以降低产品价格
*   **get_parse_result** 有一个不同的值:我们调用 changePrice 函数，将 regular_price 变量作为第一个参数传递，以保持产品价格不变

在脚本的最后，我调用了 getWeather 函数:

```
getWeather() 
```

这就对了——只需一点代码，价格就会自动更新。

## 包扎

因此，这只是一个简单的例子，说明了如何使用 API 来自动化您的业务策略，节省时间，并避免会给公司带来金钱损失的错误。

这是一个非常基本的例子，但是我认为它展示了 API 的潜力和它们的好处。在这里你可以找到我的[回购](https://github.com/mventuri/automate-ecom-prices)的完整脚本。