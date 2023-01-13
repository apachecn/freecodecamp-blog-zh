# 如何使用 Python 获取 IP 地址的位置信息

> 原文：<https://www.freecodecamp.org/news/how-to-get-location-information-of-ip-address-using-python/>

有时你需要知道一个 IP 地址的位置，不管它是你自己的还是你正在使用的网站的。

这方面的一个用例是，当您想要向网站用户发送登录信息时。

在本文中，我们将看到如何使用 Python 来查找 IP 地址的位置。

# **准备好你的工具**

为了实现这个目标，我们将使用下面提到的两个 API:

1.  [**ipify**](https://www.ipify.org/) :这个 API 会帮助我们知道请求来自哪里的 IP 地址。
2.  [**ipapi**](https://ipapi.co/) :这个 api 将帮助我们获取特定 IP 地址的位置信息。

为了与这些 API 交互，我们将使用 Python 中的`**requests**`库。如果你是 API 新手，一定要看看[这篇教程](https://ireadblog.com/posts/77/python-api-tutorial-how-to-interact-with-web-services)来了解它们。

您可以像这样使用`pip`命令安装这个库:

```
$ pip install requests
```

一旦库安装完毕，我们就可以开始了！

# **获取位置信息**

正如我们所讨论的，我们将首先从第一个 API 获取我们的 IP 地址。然后我们将利用这个 IP 地址来获取这个特定 IP 地址的位置信息。所以，我们有两个函数:

```
import requests

def get_ip():
    response = requests.get('https://api64.ipify.org?format=json').json()
    return response["ip"]

def get_location():
    ip_address = get_ip()
    response = requests.get(f'https://ipapi.co/{ip_address}/json/').json()
    location_data = {
        "ip": ip_address,
        "city": response.get("city"),
        "region": response.get("region"),
        "country": response.get("country_name")
    }
    return location_data

print(get_location()) 
```

在上面的代码中，我们有两个函数——`**get_ip()**`和`**get_location()**`。让我们分别讨论它们。

### **`get_ip()`功能**

根据 ipify 的 [API 文档](https://www.ipify.org/)，我们需要在 [**`https://api.ipify.org?format=json`**](https://api.ipify.org/?format=json) 上发出一个 **GET** 请求，以获得如下所示的 JSON 响应:

```
{
  "ip": "117.214.109.137"
}
```

我们将这个响应存储在一个 **`response`** 变量中，这个变量就是一种带有一个键值对的 [Python 字典](https://ireadblog.com/posts/84/everything-you-need-to-know-about-python-dictionaries)。所以我们将键`**ip**`的值返回为`**response["ip"]**`。

### **`get_location()`功能**

根据 ipapi 的 [API 文档](https://ipapi.co/api/#introduction)，我们需要在 **[`https://ipapi.co/{ip}/{format}/`](https://ipapi.co/%7Bip%7D/%7Bformat%7D/)** 上发出 **GET** 请求，以获取特定 IP 地址的位置信息。`{ip}`被替换为 IP 地址，而`{format}`可以被替换为这些地址中的任意一个—`json`、`jsonp`、`xml`、`csv`、`yaml`。

这个函数在内部调用`**get_ip()**`函数来获取 IP 地址，然后在带有 IP 地址的 URL 上发出一个 **GET** 请求。该 API 返回如下所示的 JSON 响应:

```
{
    "ip": "117.214.109.137",
    "version": "IPv4",
    "city": "Gaya",
    "region": "Bihar",
    "region_code": "BR",
    "country": "IN",
    "country_name": "India",
    "country_code": "IN",
    "country_code_iso3": "IND",
    "country_capital": "New Delhi",
    "country_tld": ".in",
    "continent_code": "AS",
    "in_eu": false,
    "postal": "823002",
    "latitude": 24.7935,
    "longitude": 85.012,
    "timezone": "Asia/Kolkata",
    "utc_offset": "+0530",
    "country_calling_code": "+91",
    "currency": "INR",
    "currency_name": "Rupee",
    "languages": "en-IN,hi,bn,te,mr,ta,ur,gu,kn,ml,or,pa,as,bh,sat,ks,ne,sd,kok,doi,mni,sit,sa,fr,lus,inc",
    "country_area": 3287590,
    "country_population": 1352617328,
    "asn": "AS9829",
    "org": "National Internet Backbone"
}
```

我们在回复中得到大量的数据。你可以使用任何对你有用的东西。对于本教程，我们将只使用`**city**`、`**region**`和**、`country`和**。这就是为什么我们创建了一个名为 **`location_data`** 的字典，并在其中存储所有数据并返回相同的数据。

最后，我们调用`**get_location()**`函数并打印输出。我们的输出将如下所示:

```
{
  "ip": "117.214.109.137", 
  "city": "Gaya", 
  "region": "Bihar", 
  "country": "India"
}
```

# **结论**

在本文中，我们学习了如何与 web 服务交互来获取特定 IP 地址的位置信息。

感谢阅读！更多这样的文章，请查看我的博客。