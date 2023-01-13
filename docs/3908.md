# 如何用 Falcon 构建 RESTful APIs

> 原文：<https://www.freecodecamp.org/news/build-restful-apis-with-falcon/>

## **简介**

RESTful APIs 是任何架构良好的堆栈的主要组成部分，Python 恰好有一些出色的框架来快速组合 API。

其中一个框架叫做[Falcon](https://falconframework.org/)——非常棒！本质上是一个微框架，它具有相当多的优势:

1.  它很快。非常快。点击查看基准[。](https://falconframework.org/#sectionBenchmarks)
2.  HTTP 资源被定义为类，类方法用于这些资源上不同的 REST 操作。这有助于维护一个干净的代码库。
3.  它是相当可扩展的——在他们的 wiki 上查看这一部分,感受一下。
4.  它基于 WSGI——web 应用程序的 Python 标准——所以它适用于 Python 2.6、2.7 和 3.3+。如果您需要更高的性能，请使用 PyPy 运行它！

## **入门**

首先，让我们准备好我们的环境。就个人而言，在虚拟环境中工作总是很棒——你可以使用`virtualenv`、`virtualenvwrapper`或`venv`。接下来，使用`pip` : `pip install falcon`安装猎鹰。

我们将开发一个小的示例 API，为我们完成非常基本的时区操作。它将以 UTC 显示当前时间，以及相应的纪元时间。为此，我们将抓取一个叫做`arrow` : `pip install arrow`的漂亮库。

你可以在[https://github . com/rudimk/freecodecamp-guides-rest-API-falcon](https://github.com/rudimk/freecodecamp-guides-rest-api-falcon)找到完成的样本。

## **资源**

把资源想象成一个 API 需要操作的实体。在我们的例子中，最好的资源是一个`Timestamp`。我们的路由通常是这样的:

```
GET /timestamp
```

这里，`GET`是用来调用这个端点的 HTTP 动词，`/timestamp`是 URL 本身。现在我们已经解决了这个问题，让我们创建一个模块！

`$ touch timestamp.py`

导入 Falcon 库的时间:

```
import json

import falcon

import arrow
```

注意，我们还导入了`json`包和`arrow`库。现在，让我们为我们的资源定义一个类:

```
class Timestamp(object):

	def on_get(self, req, resp):
		payload = {}
		payload['utc'] = arrow.utcnow().format('YYYY-MM-DD HH:mm:SS')
		payload['unix'] = arrow.utcnow().timestamp

		resp.body = json.dumps(payload)
		resp.status = falcon.HTTP_200
```

让我们来看看这个片段。我们已经定义了一个`Timestamp`类，并定义了一个名为`on_get`的类方法——这个函数告诉 Falcon，当一个`GET`请求被发送到该资源的一个端点时，运行`on_get`函数并提供请求和响应对象作为参数。

之后就一帆风顺了——我们创建一个空字典，用当前的 UTC 和 UNIX 时间戳填充它，将其转换为 JSON 并附加到响应对象。

很简单，对吧？但可悲的是，这还不是全部。我们现在需要创建一个 Falcon 服务器，并将我们刚刚定义的资源类连接到一个实际的端点。

`$ touch app.py`

现在，添加下面的代码:

```
import falcon

from timestamp import Timestamp

api = application = falcon.API()

timestamp = Timestamp()

api.add_route('/timestamp', timestamp)
```

这里，我们定义了一个 Falcon API，并初始化了我们之前创建的资源类的一个实例。然后，我们将`/timestamp`端点与类实例连接起来——现在我们可以开始了！为了测试这个 API，安装`gunicorn` ( `pip install gunicorn`)，并运行`gunicorn app`。使用 Postman 或简单的`cURL`来测试这个:

```
$ curl http://localhost:8000/timestamp                                                    
{"utc": "2017-10-20 06:03:14", "unix": 1508479437}
```

这就行了！

## **继续前进**

一旦掌握了 Falcon，编写与数据库或消息队列交互的强大 RESTful APIs 就非常容易了。一定要查看 Falcon docs 和 PyPI 中不断出现的有趣的 Falcon 模块。