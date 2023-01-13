# API 代表什么？用简单的英语对编码缩略词的定义。

> 原文：<https://www.freecodecamp.org/news/what-does-api-stand-for-a-definition-of-the-coding-acronym-in-plain-english/>

不，API 不代表里面的苹果派。API 代表应用程序编程接口。API 允许两个应用程序相互连接(或交互)。

API 是用于访问网站或基于网络的软件应用程序的一组编程指令和功能。API 允许其他开发人员使用您的应用程序的数据和功能。它允许您的产品与其他产品进行交互。

API 首次用于软件和硬件开发是在 20 世纪 80 年代。但是现在当人们谈论 API 时，他们通常指的是 web APIs，或者更具体地说是 RESTful APIs。在开发基于 web 的应用程序时，使用 RESTful APIs 已经成为惯例。

web API 基本上是一个完全通过 URL 进行交互的程序。通常，当你用浏览器向一个 URL 发送请求时，服务器会发回一个显示给你看的响应。当你发送一个请求到一个 API 的 URL 时，事情就不一样了。服务器发回一些只对计算机有用的信息。API 返回可以在不同网站或程序中使用的数据。

## API 是用来做什么的？

API 不是最终用户使用的。它们用于软件与其他软件进行交互。例如，一个网站可以调用 Open Weather API 来获取天气信息以显示在网站上。

API 有时也在一个公司内部使用。它们可以用来创建内部网站和系统，相互之间可以很容易地进行交互。

## API 是如何工作的？

API 通常允许其他人访问大量有组织的数据。该数据的看门人允许开发人员(以 *API 密钥*的形式)向服务器请求信息。如果请求成功，服务器会用一条消息进行响应，通常是 JSON 或 XML 格式。

通常会有你想要使用的 API 的文档，称为 API 规范。这解释了控件以及如何使用 API。

这里有一个 OpenWeather API 的 API 规范的例子，它允许你获取特定地点的当前天气:[https://openweathermap.org/current](https://openweathermap.org/current)

API 规范包含一个可以用来检索数据的 URL 列表。使用其中一个 URL 被称为 *API 请求*或 *API 调用*。通常规范会显示作为 API 一部分的每个 URL 的*参数*和*响应*。

### 因素

参数是添加到 URL 末尾的内容，用于指定希望 API 返回的信息。参数基本上是传递给 API 的变量。

从 OpenWeather API 获取天气信息的 URL 为:
`api.openweathermap.org/data/2.5/weather`。

但是，您需要添加一个城市作为参数来指定返回哪个位置的天气数据。下面是带有城市参数的 URL:
[`api.openweathermap.org/data/2.5/weather?q=London`](http://samples.openweathermap.org/data/2.5/weather?q=London,uk&appid=b6907d289e10d714a6e88b30761fae22)

有时需要参数来获得响应。有时参数是可选的。在 OpenWeather API 中，需要提供一个位置，但是除了城市名称之外，还有其他指定位置的方法。API 规范中给出了所有的方法。

参数还可以指定如下内容:

*   结果应该如何排序？
*   应该返回多少个结果？
*   结果应该是什么格式？
*   您想要哪个日期范围的结果？

### 回应

当你向一个 API 发送请求时，你将得到一个响应。您将得到您请求的数据或请求失败的原因。

下面是一个例子，当你发送下面的请求时，你得到的响应: [`api.openweathermap.org/data/2.5/weather?q=London`](http://samples.openweathermap.org/data/2.5/weather?q=London,uk&appid=b6907d289e10d714a6e88b30761fae22) 。这是一个 JSON 响应。

```
{
    "coord": {
        "lon": -0.13,
        "lat": 51.51
    },
    "weather": [
        {
            "id": 300,
            "main": "Drizzle",
            "description": "light intensity drizzle",
            "icon": "09d"
        }
    ],
    "base": "stations",
    "main": {
        "temp": 280.32,
        "pressure": 1012,
        "humidity": 81,
        "temp_min": 279.15,
        "temp_max": 281.15
    },
    "visibility": 10000,
    "wind": {
        "speed": 4.1,
        "deg": 80
    },
    "clouds": {
    	"all": 90
    },
    "dt": 1485789600,
    "sys": {
        "type": 1,
        "id": 5091,
        "message": 0.0103,
        "country": "GB",
        "sunrise": 1485762037,
        "sunset": 1485794875
    },
    "id": 2643743,
    "name": "London",
    "cod": 200
    }
```

API 响应的格式可能不像下面的例子。所有文本通常都在一行中。因为它主要是由计算机而不是人来阅读的，所以格式并不重要。

### **API 键**

如果你自己尝试上面的网址，你不会得到上面的回应。它可能看起来更像:

```
{
    "cod": 401,
    "message": "Invalid API key. Please see http://openweathermap.org/faq#error401 for more info."
}
```

大多数 API 在返回数据之前需要某种认证。这通常是以一个 *API 键*的形式。这些钥匙有点像密码。它们是一长串字母和数字，您必须随 API 请求一起发送，以便服务器知道您被允许访问这些信息。

对于 OpenWeather API，以及许多其他 API，您可以在创建帐户后免费获得一个 API 密钥。许多公司在免费 API 上使用 API 键，以确保人们不会在一天内发出很多请求。如果一个人每分钟发出数千个请求，服务器就会陷入瘫痪。

有些 API 是公共的，没有 API 键。下面是一个 API，可以让你找到押韵的单词。点按该链接，然后尝试更改 URL 中的最后一个单词来搜索不同的押韵单词。

[https://api.datamuse.com/words?rel_rhy=camp](https://api.datamuse.com/words?rel_rhy=camp)

## 想了解更多？

如果你想了解更多关于使用 API 的知识，可以看看 freeCodeCamp.org YouTube 频道下面的视频。

[https://www.youtube.com/embed/BYsTrGH6B2s?feature=oembed](https://www.youtube.com/embed/BYsTrGH6B2s?feature=oembed)