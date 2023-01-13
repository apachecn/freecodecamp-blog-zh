# 你会喜欢的 10 个外部 Python 包

> 原文：<https://www.freecodecamp.org/news/these-python-packages-will-help-accelerate-your-development-process-d4b3f170b1ea/>

作者:亚当·戈德施密特

# 你会喜欢的 10 个外部 Python 包

![Tehd4MeGX2yYQUFUtdcWWNbUE7Qk9qFsZ9-Z](img/fef3392ef93e6439d5305dff8d3df299.png)

Photo by [Brina Blum](https://unsplash.com/@brina_blum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> Python 是程序员需要多大自由的一个实验。太多的自由，没人能读懂别人的代码；太少，表现力受到威胁。吉多·范·罗苏姆

Guido 所说的这种自由是 Python 如此受欢迎的部分原因。这种流行吸引了越来越多的开发人员使用这种语言——最终导致了一些令人惊叹的开源项目。

我通常每天都会在 GitHub 上搜索一次项目。在整篇文章中，我将尝试涵盖 10 个您可能熟悉也可能不熟悉的精彩包。我将从不太流行的开始，以……嗯，烧瓶结束。

### 我们开始吧！

![oGpPuDrsAM6KYONzQCrIZXv1xAEIv-oVuIUT](img/7ad3b05ca23c7d5f8094f7f22468b052.png)

#### [Loguru](https://github.com/Delgan/loguru) —让记录变得简单

![DWrohhPZvoWbH4s8apMbg8nXZOtf3m0lAhvk](img/b733e1779be4e86e48ad99b3db026129.png)

这是一个非常棒的包，我经常在我的项目中使用。它将自己描述为“一个旨在用 Python 带来愉快的日志记录的库”。这个包可以让你轻松地配置你的日志。

安装后，您只需导入模块:

```
from loguru import logger
```

您可以开箱即用:

```
logger.debug("Hello, cool debugger")
```

文档很好，并且有许多定制选项。

#### [更多工具](https://github.com/erikrose/more-itertools)

各种有趣的方法有时会非常有用，比如`peekable`:

```
>>> p = peekable(['a', 'b'])>>> p.peek()'a'>>> next(p)'a'
```

或者`chunked`:

```
>>> list(chunked([1, 2, 3, 4, 5, 6], 3))[[1, 2, 3], [4, 5, 6]]
```

#### [MonkeyType](https://github.com/Instagram/MonkeyType) —静态类型注释生成器

```
monkeytype run myscript.py
```

这个包通过收集运行时类型，在存根文件或源代码本身中自动为您生成类型注释。没错，Python 并没有强迫你使用注释——但是我相信它们对于代码的可读性非常重要(有时是为了避免错误),这也是为什么这个列表中还有 2 个包处理类型注释:)

#### [Pyright](https://github.com/Microsoft/pyright) —静态类型检查器

![B5KVRNqA90q0PqVY18dvfvc7m7rbjYYVf1EP](img/7f9f4393d262f39aed9374509d4f64c4.png)

来自微软的令人兴奋的新软件包。第一次提交是在 17 天前！这个包是 Mypy 的竞争对手(也在这个列表上)。老实说，我还没有机会使用它，但我肯定打算这样做。我目前使用 mypy 作为类型检查器，但是我将尝试使用它！

#### [请求-异步](https://github.com/encode/requests-async)——支持`requests`的`async` / `await`语法

这是我前几天在 GitHub 上发现的一个新包，看起来很有前途。我们都知道[请求](https://github.com/kennethreitz/requests)包，它让我们在代码中轻松处理 HTTP 请求。这个包为这些请求实现了`async`和`await`字:

```
import requests_async as requests​response = await requests.get('https://example.org')print(response.status_code)print(response.text)
```

很酷吧？

#### [HTTPie](https://github.com/jakubroztocil/httpie) —现代命令行卷曲

![UAD--5ZtcqjDRRKA4Y1oXEWzob6GTM94sXGa](img/68b700c9a76c971933bde336e8a43cdd.png)

以前用过 cURL 的人，一定知道它没那么好玩。必须记住参数名，确保你的数据被封装…嗯，HTTPie 的目标是让这变得更容易。这是他们提交表单数据的一个例子:

```
http -f POST example.org hello=World
```

#### [pipenv](https://github.com/pypa/pipenv) —更好的 Python 打包

当我开始一个新项目时，我总是会创建一个新的`virtualenv`并安装一些带有`pip`的基本包。然后我需要将这些包名保存在一个文件中，不管是`setup.py`还是`requirements.txt`。你们当中与`npm`共事过的人知道，那里要简单得多。你只需要写下`npm —save`，包名就会保存在你的`package.json`中。这就是为什么我首先创建了 [pypkgfreeze](https://github.com/AdamGold/pypkgfreeze) ，一个简单的包来将你当前使用的`pip`包的版本“冻结”到`setup.py`。

无论如何，pipenv 是一个有趣的解决方案，旨在合并这两个世界——他们在自己的回购页面中对此进行了最佳描述:

它会自动为您的项目创建和管理一个 virtualenv，并在您安装/卸载软件包时从您的`Pipfile`中添加/删除软件包。它还生成非常重要的`Pipfile.lock`，用于产生确定性的构建。

你可以在这里尝试一下[。](https://rootnroll.com/d/pipenv/)

#### [mypy](https://github.com/python/mypy) —静态类型检查器

正如我之前说过的，这是我目前用作标准静态类型检查器的包。它帮助我保持代码的可读性和优雅性(我认为)。

#### [黑色](https://github.com/ambv/black)

![dQoUny7l5N6sWs2GCECZKHALf59t9398hNNp](img/d4ff86c8cbc48dee9bea7cd1e760b0dd.png)

我尝试过许多 Python 格式化程序，而`black`显然是我的最爱。语法看起来很简洁，命令行运行很快，可以检查文件或实际编辑它们——这对 CI/CD 非常有用。你甚至可以在这里尝试一下[！](https://www.freecodecamp.org/news/these-python-packages-will-help-accelerate-your-development-process-d4b3f170b1ea/%5Bhttps://black.now.sh%5D(https://black.now.sh/))

#### [烧瓶](https://github.com/pallets/flask)

不确定我是否有以前没有写过的东西要写。你可能对这个惊人的微观框架很熟悉，如果你不熟悉..你绝对应该去看看。

### 在你走之前…

感谢阅读！你可以关注我的 [GitHub](https://github.com/AdamGold) 账号，获得更多酷炫的回复。我倾向于在我看到的每一个很酷的东西上打星号:)

如果你喜欢这篇文章，请按住鼓掌按钮？帮助其他人找到它。你拿的时间越长，你给的掌声就越多！

不要犹豫，在下面的评论中分享你的想法。