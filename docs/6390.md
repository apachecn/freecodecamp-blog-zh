# 如何在现有的 Django 应用程序上获得即时 GraphQL APIs

> 原文：<https://www.freecodecamp.org/news/how-to-get-instant-graphql-apis-on-your-existing-django-application-c8fcfdb945aa/>

作者 Karthik Venkateswaran

# 如何在现有的 Django 应用程序上获得即时 GraphQL APIs

![1*BRtPUVYOTcVmiL0GlytYAQ](img/25b6cba6c9ed9ddb0c0a29e0b92f7df8.png)

GraphQL APIs on your existing Django application

### TL；速度三角形定位法(dead reckoning)

如果你想了解更多，下面是我们将在本文中涉及的主题:

#### [**为什么是 GraphQL？**](https://medium.com/@vasanthivenkateswaran/c8fcfdb945aa#dcf2)

GraphQL 是由脸书开发的一种数据查询语言。它不依赖于任何特定的数据库。它为客户端提供了一种方法，通过请求他们需要的内容，同时从不同的数据库中进行查询。它以客户端请求的格式返回响应

#### [**构建一个 GraphQL 服务器**](#8c56)

构建 GraphQL 服务器有哪些不同的方法？我们将了解 Hasura GraphQL 引擎如何提供在现有数据库上获得 GraphQL API 的最简单方法。

#### [**设置 GraphQL 引擎**](#8668)

我们将安装 Hasura GraphQL 引擎。然后我们将通过 GraphQL API 公开表。

#### [**保护 GraphQL 服务器**](#9050)

#### [**办理迁移**](#50e3)

所以让我们开始吧！

### 为什么是 GraphQL？

在典型的 Django 应用程序中，任何新的特性需求或模式变化都需要添加或修改现有的视图。这可能会对开发人员的工作效率产生巨大的影响。这将需要在使用特定 API 的所有地方进行代码更新。

这就是 GraphQL 派上用场的地方。 [GraphQL](https://graphql.org/) 是一种用于 API 的查询语言。它抽象了多个数据源。这使得应用程序开发人员能够以他们需要的格式请求数据。它这样做不需要任何后端 API 的变化。我们调用单个端点，而不是调用单个端点来获取数据。我们得到所有我们想要的信息，完全按照我们想要的方式构建。

所以这可能会让你想知道:我如何在我现有的 Django 应用程序上获得一个 GraphQL API？

### 构建 GraphQL 服务器

要构建一个 GraphQL 服务器，您需要做的就是定义一个**模式。模式**是应用程序中数据类型的目录。**解析器函数**告诉服务器从哪里以及如何获取每种数据类型的数据。

目前的方法包括在像 django-graphene 这样的工具的帮助下从头开始编写(模式、解析器函数)。

在这篇文章中，我将使用 [Hasura GraphQL 引擎](https://hasura.io)在我现有的本地运行的 [Django 应用程序](https://github.com/karthikvt26/django-blog-app/tree/4e92155f0af0f17545a9a77dac7d744b0cc51e31)上获得一个 GraphQL API。我们将得出如下图所示的解决方案。

![1*rDlZ3Ejok1L-IvmpyUTu4w](img/d7492b1f14a8058f18d77d860a51f35b.png)

Architecture before and after integrating with Hasura GraphQL engine

Hasura GraphQL 引擎 (HGE)在现有 Postgres 的基础上为您提供即时实时的 GraphQL API。HGE 工程开箱即用您现有的:

*   [**Postgres 数据库**](#f087)**——**连接您现有的数据库，为您的数据库提供一个 GraphQL API。
*   [**认证系统**](#9050)**——**连接您现有的认证系统，以保护 GraphQL API。
*   [**迁移系统**](#50e3)**——**Hasura graph QL 引擎不干扰现有 Django 的迁移系统。模式可以通过 **models.py** 和 **django migrate** 单独管理，直到并且除非它不改变 GraphQL 引擎跟踪的模式。关于 Hasura GraphQL 引擎如何管理你的模式状态的更多信息可以在[这里](https://docs.hasura.io/1.0/graphql/manual/engine-internals/index.html)找到。

此外，它还带有一个漂亮的控制台(类似于 Django admin ),可用于调试 GraphQL APIs。

### 装置

使用下面的按钮可以将 Hasura GraphQL 引擎安装到 Heroku 上:

![1*dcgu-klnpwTWilYiGMdM6Q](img/c916dc06b59661d3e933b122aa2d488e.png)

Click this button to deploy the GraphQL engine to Heroku

或者可以运行 Docker 的任何机器上。查看[入门](https://docs.hasura.io/1.0/graphql/manual/getting-started/index.html)部分了解更多信息。

#### 使用 docker 安装并连接到现有 Postgres

在安装 Hasura GraphQL 引擎之前，我需要获取 Postgres 连接字符串，以便 Hasura GraphQL 引擎与数据库连接。我可以从应用程序的`[settings.py](https://github.com/karthikvt26/django-blog-app/blob/4e92155f0af0f17545a9a77dac7d744b0cc51e31/blog/settings.py)`中获取 Postgres 连接字符串。

```
DATABASES = { 
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'SECUREPASSWORD',
        'HOST': '172.17.0.1',
        'PORT': '5432',
    }   
}
```

数据库连接 URL 将变成:

```
postgres://postgres:SECUREPASSWORD@172.17.0.1:5432/postgres
```

一旦 Hasura GraphQL 引擎启动，访问 [http://localhost:8080](http://localhost:8080/) 打开如下 Hasura 控制台。**数据**部分显示了数据库中按模式分组的未跟踪表的列表。如果你想知道什么是未跟踪的表，前往[文档](https://docs.hasura.io/1.0/graphql/manual/schema/using-existing-database.html)获取更多信息。

![1*dJlxjx1PHK5flNXiKVy5sg](img/d7aa140c9d9e8896e5070d2296aca3f5.png)

Hasura GraphQL Engine Console

![1*S7hBC1Hefyr1ho3RcneXmw](img/31e9dbefa33468b2367bbaf2b18b9ac9.png)

Hasura Console | Data Explorer

上面的截图列出了 Django 应用程序在这个[**models . py**](https://github.com/karthikvt26/django-blog-app/blob/4e92155f0af0f17545a9a77dac7d744b0cc51e31/medium/models.py)**文件中定义的未跟踪表下创建的表。单击 add 按钮会在左侧的跟踪表列表中显示。它通过 GraphQL APIs 公开它们以供查询:**

**![1*DU43Tx-lE4AK6CtFlVWRdw](img/b40df2389e8892a10337cd05f57b96e9.png)**

**为了测试一切是否正常，让我们尝试获取表中所有可用的`authors`:**

```
`query {
  medium_author {
    id
    name
    interests
  }
}`
```

**来自 GraphQL 引擎的响应是:**

```
`{
  "data": {
    "medium_author": [
      {
        "name": "Karthik",
        "id": 2,
        "interests": "Cricket, Music, Code"
      },
      {
        "name": "Second Author",
        "id": 4,
        "interests": "Hockey"
      }
    ]
  }
}`
```

#### **对象和数组关系**

**GraphQL 引擎解析您的模式，并根据表之间定义的外键提出关系建议。**

**![1*b0o9K84Qsfy2khrriNKJxg](img/3c2c88bab0c9181ddae37a2342dabe19.png)

Suggested foreign key relations** 

**GraphQL 引擎自动为它遇到的每个外键建议两种关系。**

*   ****对象关系** : 1:1 关系。例如，一篇文章将只有一个作者。**
*   ****数组关系** : 1:多。例如，一个作者可以写很多文章。**

**在[博客模式](https://github.com/karthikvt26/django-blog-app/blob/master/medium/models.py)中，`mediumArticlesByauthorId`是一个“数组关系”它基于 t `he medium_artic` l `e. mediumAuthorByAutho` rId 中的外键`medium_article :: author_id ->` id，是基于相同外键的“对象关系”。**

**当我们跟踪这些关系时，派生的 GraphQL 模式包含关系名。可以在单个查询中查询表和关系:**

**![1*tSg46B7TF20ceET3rYLboQ](img/6a23e7144d8678bb6d312c7aa7dca317.png)

GraphQL query with array relationship** **![1*zeALwfd78Oqp2glGKG8oqw](img/09b628a9b276c435065c3c6e8c234979.png)

GraphQL query with object relationship** 

### **证明**

**默认情况下，GraphQL 引擎以开发模式安装。GraphQL 引擎跟踪的所有表/视图都可以查看/更新，无需任何检查。这很危险，不建议在生产环境中使用。**

**Hasura 允许您为 GraphQL 模式中的每个字段定义粒度访问控制，基本上是 Postgres 模式中的每个表或视图。这些访问控制规则可以使用每个请求中的动态变量。查看[文档](https://docs.hasura.io/1.0/graphql/manual/auth/index.html)了解更多信息。**

**通过配置 webhook URL，可以防止 GraphQL 引擎直接访问它。GraphQL 引擎将调用它来验证每个请求，除非请求包含有效的`access-key`。**

**![1*pX9ydrpIK9angEvmCHkKrA](img/ae390f50b382495a007f0480c6771826.png)

Architecture of how the request/response happens** 

**在我们使用`access-key` 和`auth-hook` (webhook URL)保护 GraphQL 端点之前，让我们使用 Hasura 控制台添加一个简单的访问控制规则，以限制`author`仅获取他的数据并使用 GraphQL explorer 进行查询。**

**下面是 role = `user`的`medium_author`表的访问控制规则。**

**![1*sjF-pudjQP96I8X8IMOebQ](img/aaf3030216ab19959be6d1b1378b8c0c.png)

Add access control to author table** 

**我只创建了`select` 权限，但是您可以配置所有四种类型的操作(选择、插入、更新、删除)。查看[文档](https://docs.hasura.io/1.0/graphql/manual/auth/basics.html)了解更多信息。**

**让我们从`medium_author`表中查询，看看响应是什么:**

**![1*yaSciD7j9zo12Ph1JbBYqQ](img/5cced233bf157147f22707194ad071e9.png)**

**这里请注意，`x-hasura-user-id`设置为“2”，而`x-hasura-role`设置为“用户”。这是在[生产模式](https://docs.hasura.io/1.0/graphql/manual/deployment/docker/securing-graphql-endpoint.html)下`auth-hook`将传递的`auth`数据(GraphQL 发动机用`access-key` 和`auth-hook`启动)。**

#### **安全图形 api**

**让我们用`access-key`来保护 GraphQL 引擎。让我们用认证处理程序配置`auth-hook`，在本例中是 Django 应用程序。配置好的 [**webhook**](https://github.com/karthikvt26/django-blog-app/blob/e8c966b4d9b87e3a5b6a39f3b7c6e9e02fef034d/medium/views.py#L44) 将被 GraphQL 引擎调用。webhook 将返回适当的`x-hasura-role`和`x-hasura-user-id`。**

```
`version: '3.6'
services:
  postgres:
    image: postgres
    environment:
    - "POSTGRES_PASSWORD:mysecretpassword"
    ports:
    - "5432:5432"
    restart: always
    volumes:
    - db_data:/var/lib/postgresql/data
  graphql-engine:
    image: hasura/graphql-engine:v1.0.0-alpha13
    ports:
    - "8080:8080"
    depends_on:
    - "postgres"
    restart: always
    environment:
      HASURA_GRAPHQL_DATABASE_URL: postgres://postgres:mysecretpassword@postgres:5432/postgres
    command:
      - graphql-engine
      - serve
      - --access-key=mysecretkey
      - --auth-hook=http://192.168.2.58:9090/validate_request
      - --enable-console
volumes:
  db_data:`
```

**让我们再次尝试进行查询，看看响应是什么:**

**![1*afYqLtDIyrJyR7RU7iG6_w](img/6388329648cdbfc369c795bc2bd8726d.png)**

**被配置的 [**webhook**](https://github.com/karthikvt26/django-blog-app/blob/e8c966b4d9b87e3a5b6a39f3b7c6e9e02fef034d/medium/views.py#L44) 拒绝，因为请求未通过认证。让我们尝试以用户身份登录，并使用用户的 auth 令牌发出请求。Django auth 系统解析`cookies.`它将用户信息添加到请求上下文中，然后请求处理程序可以使用这些信息。**

**为了这个博客，我写了一个简单的认证中间件。它将解析`Authorization: Bearer <tok` en >并将其解析为 Django 用户。用户将被添加到请求图标[文本](https://github.com/karthikvt26/django-blog-app/blob/e8c966b4d9b87e3a5b6a39f3b7c6e9e02fef034d/medium/authentication.py#L20)。下面是相同的代码片段。**

**![1*YTesXQ80z4olokviXp-uTA](img/8bcde73b4d5531cf7b26cafb0158ce86.png)

Login with user with id = 2** **![1*I2DOakdiY0X-3bm7UGk9ZQ](img/5e950457878fb0784c24bec70a48c5fa.png)

Querying with the logged in user** 

**用户通过 webhook 进行身份验证。webhook 返回对应的`x-hasura-user-id`和`x-hasura-role`。GraphQL 引擎以访问规则中配置的适当结果进行响应。**

### **迁移系统**

**Hasura GraphQL 引擎附带了强大的受 Rails 启发的迁移工具，帮助您跟踪对模式所做的更改。当您使用 Hasura 控制台时，Hasura CLI 将为您提供迁移文件。您可以将它们置于版本控制中，甚至可以编辑它们。**

**默认情况下，Hasura 控制台由 GraphQL 引擎提供服务。它可以用来快速测试 GraphQL 引擎提供的特性。然而，如果您正在构建一个复杂的应用程序，或者将 Hasura 添加到现有的应用程序或数据库中，您将需要存储迁移，以确保您的迭代和 CI/CD 是平滑的。**

#### **设置**

**如果您使用的是 Mac/Linux 机器，通过在您的终端上执行以下命令来安装`hasura`。否则，前往我们的[文档](https://docs.hasura.io/1.0/graphql/manual/hasura-cli/install-hasura-cli.html)在不同的环境下安装 hasura。**

```
`curl -L https://cli.hasura.io/install.sh | bash`
```

**执行以下命令将初始化一个目录，其中包含配置为使用 GraphQL 引擎的 hasura 配置文件。**

```
`$ hasura init --directory blog-hasura-app --endpoint http://localhost:8080 --access-key=mysecretkey`
```

**![1*kMA2X_j9L9alBH7w8MnjNw](img/d9e1f25cd087967a95e0f30ede0c606e.png)

hasura init** 

**将`endpoint`和`access-key`的值替换为合适的值。**

#### **禁用迁移**

**因为 Django 默认负责迁移，所以可以通过在终端上键入`hasura console`来禁用 Hasura 迁移。要打开 Hasura 控制台，导航到**数据- >迁移** ns(在左侧导航栏上)和 disab `**le Allow postgres schema chan**` **g** es。**

**我们仍然可以存储 Hasura 元数据，以确保应用程序始终处于可恢复状态:**

**![1*Wo4dKt2qPkKkPnoIbPDShg](img/e905e61a1a996fb30b3ba57540331c34.png)

Before disabling migration** **![1*5ttl3UM-iQKdHCxUt5KUdw](img/12e72fdbcdb944e0a5074e349a2c9719.png)

After disabling migration** 

#### **元数据导出**

**导出 Hasura 元数据并将其存储在 migrations 文件夹中。这将确保您的系统总是可以从任何不良状态中恢复。**

```
`hasura metadata export`
```

**上述命令将导出`metadata.yaml`并将其存储在`migrations`文件夹中。**

**![1*32kfQrUSc6qQBljLhTgFYQ](img/320ca2d29f96cfdb97db18963c127c21.png)**

**请确保仅通过 Django **models.py** 文件创建/修改表/视图，以避免不一致。**

**如果你想使用 Hasura 迁移系统，查看[文档](https://docs.hasura.io/1.0/graphql/manual/migrations/existing-project.html)了解更多信息。**

**[***Hasura***](https://goo.gl/fR68ep)*让你在任何 Postgres 数据库上即时实时 GraphQL APIs，而无需编写任何后端代码。***

***对于那些不熟悉 Hasura GraphQL 引擎的人来说， [**这个**](https://docs.hasura.io/1.0/graphql/manual/index.html) 是一个入门的好地方。***