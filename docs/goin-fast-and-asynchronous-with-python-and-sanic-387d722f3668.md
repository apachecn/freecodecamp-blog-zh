# 如何用 Python 和 Sanic 让你的代码快速异步

> 原文：<https://www.freecodecamp.org/news/goin-fast-and-asynchronous-with-python-and-sanic-387d722f3668/>

作者:戴维·托马西恩

# 如何用 Python 和 Sanic 让你的代码快速异步

大家好。在本文中，我将讨论用 Sanic 框架构建简单的异步项目。

![F0Jro1PM0OTKI0on7Hz0sEIFNlIb9JWZ58JO](img/2b6bca63500c2e3ecfe9c942d7a8e698.png)

Goin’ Fast

### 介绍

[Sanic](https://sanicframework.org/) 是一个非常类似烧瓶的开源 Python web 服务器和 web 框架，它拥有超过[个 10K 星](https://github.com/huge-success/sanic)的速度。它允许使用 Python 3.5 中添加的`async/await`语法( [read more](https://docs.python.org/3/library/asyncio-task.html) )，让你的代码[无阻塞](https://medium.com/vaidikkapoor/understanding-non-blocking-i-o-with-python-part-1-ec31a2e2db9b)且速度快。

Sanic 有非常好的[文档](https://sanic.readthedocs.io/en/latest/),由社区维护，为社区服务。

> 该项目的目标是提供一种简单的方法来建立和运行一个高性能的 HTTP 服务器，该服务器易于构建、扩展和最终扩展。

### 要求

在我们开始之前，让我们安装一些软件包，并确保我们已经为这个项目的开发做好了一切准备。

*注意:源代码可以在我的[github.com](https://github.com/davitovmasyan/sanic-project)仓库中找到。每一步都有相应的提交。*

#### 先决条件:

*   Python3.6+版
*   [pipenv](https://github.com/pypa/pipenv) (您可以使用任何其他软件包安装程序)
*   [PostgreSQL](https://www.postgresql.org/)**(对于数据库，也可以是 MySQL 或 SQLite)**

#### 包装:

*   [secure](https://pypi.org/project/secure/) 是一个轻量级的包，为 Python web 框架添加了可选的安全头和 cookie 属性。
*   [environs](https://pypi.org/project/environs/) 是一个用于解析环境变量的 Python 库。它允许你将配置从你的代码中分离出来，按照[的十二因素应用](https://12factor.net/config)方法。
*   sanic-envconfig 是一个扩展，帮助你将命令行&环境变量引入到你的 sanic 配置中。
*   [databases](https://pypi.org/project/databases/) 是一个 Python 包，允许你使用强大的 [SQLAlchemy 核心](https://docs.sqlalchemy.org/en/latest/core/)表达式语言进行查询，并提供对 PostgreSQL、MySQL 和 SQLite 的支持。

让我们创建一个空目录，并在那里初始化一个空的`Pipfile`。

```
pipenv  -- python python3.6
```

使用下面的 **pipenv** 命令安装所有必要的软件包。

```
pipenv install sanic secure environs sanic-envconfig
```

对于数据库:

```
pipenv install databases[postgresql]
```

选择有**postgresql，mysql，sqlite。**

### 结构

现在让我们创建一些文件和文件夹，我们将在其中编写实际的代码。

```
├── .env├── Pipfile├── Pipfile.lock├── setup.py└── project    ├── __init__.py    ├── __main__.py    ├── main.py    ├── middlewares.py    ├── routes.py    ├── settings.py    └── tables.py
```

我们将使用`setup.py`文件使`project`文件夹在我们的代码中作为一个包可用。

```
from setuptools import setupsetup(    name='project',)
```

*安装…*

```
pipenv install -e .
```

在`.env`文件中，我们将存储一些全局变量，比如数据库连接网址。

创建`__main__.py`是为了让我们的`project`包可以从命令行执行。

```
pipenv run python -m project
```

### 初始化

让我们在 **__main__ 进行第一次呼叫。py** 文件。

```
from project.main import initinit()
```

这是我们应用程序的开始。现在我们需要在 **main.py** 文件中创建`init`函数。

```
from sanic import Sanicapp = Sanic(__name__)def init():    app.run(host='0.0.0.0', port=8000, debug=True)
```

简单地从 *Sanic* 类创建应用程序，我们可以指定**主机**、**端口**和可选的**调试**关键字参数来运行它。

*正在运行…*

```
pipenv run python -m project
```

![7GwhssZorNRLRt-O9V-Dlm0E9VCqTFqNeOE0](img/8f33905398ed6682e71a3fd6c26e8a6d.png)

Sanic console output

这是您的 Sanic 应用程序中成功输出的外观。如果你在浏览器上打开 http://0.0.0.0:8000 ，你会看到

> 错误:找不到请求的 URL

我们还没有创建任何**路线**，所以现在还不错。我们将在下面添加一些路线。

### 设置

现在我们可以修改环境和设置。我们需要在**中添加一些变量。env** 文件，读取它们，并传递给 Sanic app config。

**T1。env** *文件。*

```
DEBUG=TrueHOST=0.0.0.0POST=8000
```

*配置…*

```
from sanic import Sanic
```

```
from environs import Envfrom project.settings import Settings
```

```
app = Sanic(__name__)
```

```
def init():    env = Env()    env.read_env()        app.config.from_object(Settings)    app.run(        host=app.config.HOST,         port=app.config.PORT,         debug=app.config.DEBUG,        auto_reload=app.config.DEBUG,        )
```

***设置. py*** *文件。*

```
from sanic_envconfig import EnvConfigclass Settings(EnvConfig):    DEBUG: bool = True    HOST: str = '0.0.0.0'    PORT: int = 8000
```

请注意，我已经添加了一个可选的 **auto_reload** 参数，它将激活或停用自动重新加载器。

### 数据库ˌ资料库

现在是建立数据库的时候了。

在我们继续之前，有一个关于**数据库**包的小提示:

> **databases** 包使用了 [asyncpg](https://github.com/MagicStack/asyncpg) 这是一个 PostgreSQL 的异步接口库。相当快。请参见下面的结果。

![RvldCRR3gSuI4UKswmylnrJYrIDQhQ9wetEd](img/679b3da7e8c0b0d1c822ccd7fd4d4ab5.png)

我们将使用 Sanic 的两个[监听器](https://sanic.readthedocs.io/en/latest/sanic/middleware.html#listeners)，在这里我们将指定数据库连接和断开操作。以下是我们将要使用的监听器:

*   **服务器启动后**
*   **服务器停止后**

***main.py*** *文件。*

```
from sanic import Sanic
```

```
from databases import Database
```

```
from environs import Envfrom project.settings import Settings
```

```
app = Sanic(__name__)
```

```
def setup_database():    app.db = Database(app.config.DB_URL)    @app.listener('after_server_start')    async def connect_to_db(*args, **kwargs):        await app.db.connect()    @app.listener('after_server_stop')    async def disconnect_from_db(*args, **kwargs):        await app.db.disconnect()
```

```
def init():    env = Env()    env.read_env()        app.config.from_object(Settings)
```

```
 setup_database()
```

```
 app.run(        host=app.config.HOST,         port=app.config.PORT,         debug=app.config.DEBUG,        auto_reload=app.config.DEBUG,        )
```

还有一件事。我们需要在项目设置和环境中指定 **DB_URL** 。

**T1。env** *文件。*

```
DEBUG=TrueHOST=0.0.0.0POST=8000DB_URL=postgresql://postgres:postgres@localhost/postgres
```

*和 **settings.py** 文件。*

```
from sanic_envconfig import EnvConfigclass Settings(EnvConfig):    DEBUG: bool = True    HOST: str = '0.0.0.0'    PORT: int = 8000    DB_URL: str = ''
```

确保 **DB_URL** 是正确的，并且您的数据库正在运行。现在，您可以使用 **app.db.** 访问数据库，详见下一节。

### 桌子

现在我们有了到数据库的连接，我们可以尝试做一些 SQL 查询。

让我们使用 SQLAlchemy 在 **tables.py** 文件中声明一个表。

```
import sqlalchemymetadata = sqlalchemy.MetaData()books = sqlalchemy.Table(    'books',    metadata,    sqlalchemy.Column('id', sqlalchemy.Integer, primary_key=True),    sqlalchemy.Column('title', sqlalchemy.String(length=100)),    sqlalchemy.Column('author', sqlalchemy.String(length=60)),)
```

这里，我假设您已经有了一个**迁移的**数据库，其中有一个**图书**表。对于创建数据库迁移，我建议您使用 [Alembic](https://alembic.sqlalchemy.org/) ，这是一个轻量级且易于使用的工具，您可以将其与用于 Python 的 SQLAlchemy 数据库工具包一起使用。

现在我们可以使用任何 [SQLAlchemy 核心](https://docs.sqlalchemy.org/en/latest/core/)查询。看看下面的一些例子。

```
# Executing manyquery = books.insert()values = [    {"title": "No Highway", "author": "Nevil Shute"},    {"title": "The Daffodil", "author": "SkyH. E. Bates"},]await app.db.execute_many(query, values)# Fetching multiple rowsquery = books.select()rows = await app.db.fetch_all(query)# Fetch single rowquery = books.select()row = await app.db.fetch_one(query)
```

### 路线

现在我们需要设置路线。我们去 **routes.py** 添加一条新的图书路线。

```
from sanic.response import json
```

```
from project.tables import books
```

```
def setup_routes(app):    @app.route("/books")    async def book_list(request):        query = books.select()        rows = await request.app.db.fetch_all(query)        return json({            'books': [{row['title']: row['author']} for row in rows]        })
```

当然，我们需要调用 **init** 中的 **setup_routes** 来使其工作。

```
from project.routes import setup_routes
```

```
app = Sanic(__name__)
```

```
def init():    ...    app.config.from_object(Settings)    setup_database()    setup_routes(app)    ...
```

*请求…*

```
$ curl localhost:8000/books{"books":[{"No Highway":"Nevil Shute"},{"The Daffodil":"SkyH. E. Bates"}]}
```

### 中间件

检查一下**响应**头，看看我们可以在那里添加或修复什么，怎么样？

```
$ curl -I localhost:8000Connection: keep-aliveKeep-Alive: 5Content-Length: 32Content-Type: text/plain; charset=utf-8
```

如你所见，我们需要一些安全改进。有一些丢失的头文件，如 **X-XSS 保护、严格传输安全**……**，所以让我们使用[中间件](https://sanic.readthedocs.io/en/latest/sanic/middleware.html#middleware)和**安全**包的组合来处理它们。**

*****middleware . py****文件。***

```
`from secure import SecureHeaderssecure_headers = SecureHeaders()def setup_middlewares(app):    @app.middleware('response')    async def set_secure_headers(request, response):        secure_headers.sanic(response)`
```

***在 **main.py** 文件中设置中间件。***

```
`from project.middlewares import setup_middlewares`
```

```
`app = Sanic(__name__)`
```

```
`def init():    ...    app.config.from_object(Settings)    setup_database()    setup_routes(app)    setup_middlewares(app)    ...`
```

***结果是:***

```
`$ curl -I localhost:8000/booksConnection: keep-aliveKeep-Alive: 5Strict-Transport-Security: max-age=63072000; includeSubdomainsX-Frame-Options: SAMEORIGINX-XSS-Protection: 1; mode=blockX-Content-Type-Options: nosniffReferrer-Policy: no-referrer, strict-origin-when-cross-originPragma: no-cacheExpires: 0Cache-control: no-cache, no-store, must-revalidate, max-age=0Content-Length: 32Content-Type: text/plain; charset=utf-8`
```

**正如我在开始时承诺的，本文中的每个[部分](https://github.com/davitovmasyan/sanic-project/tags)都有一个 [github 存储库](https://github.com/davitovmasyan/sanic-project)。希望这个小教程能帮助你开始使用 Sanic。在 Sanic 框架中还有许多未开发的特性，您可以在[文档](https://sanic.readthedocs.io/en/latest/)中找到并查看。**

**[**davitovmasyan/sanic-project**](https://github.com/davitovmasyan/sanic-project)
[*用 Python 和 Sanic 实现快速异步！- davitovmasyan/sanic 项目*github.com](https://github.com/davitovmasyan/sanic-project)**

**如果你对此有想法，一定要留下评论。**

**如果你觉得这篇文章有帮助，给我一些掌声？**

**感谢阅读。和 Sanic 一起去吧，祝你好运！！！**