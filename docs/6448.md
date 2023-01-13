# 如何在您现有的 Laravel 应用程序上获得即时 GraphQL APIs

> 原文：<https://www.freecodecamp.org/news/instant-graphql-apis-on-your-existing-laravel-app-e4af9917148c/>

作者:Karthikeya Viswanath

# 如何在您现有的 Laravel 应用程序上获得即时 GraphQL APIs

![1*q3KiCaC7_bynx8CfVlTXNw](img/62b44f56afe6c4013526d06c33ace0ee.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/oZ61KFUQsus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

#### TL；速度三角形定位法(dead reckoning)

*   [**设置 GraphQL 引擎**](#8668) —安装 Hasura GraphQL 引擎并通过 GraphQL API 公开表格
*   [**认证并保护您的 GraphQL 服务器**](#9050)
*   [**迁徙**](#6d18)

在这篇文章中，我们将使用 [Hasura GraphQL 引擎](https://hasura.io)在我现有的本地运行的 Laravel 应用程序上获得即时 GraphQL APIs。

出于这个项目的目的，我们将使用一个使用 Laravel 5.1 构建的样本 Laravel ToDo 应用程序，并修改代码以集成 HGE。(不过请注意，Laravel 5.1 已经在 2018 年 6 月寿终正寝，如果你还在使用它，你应该迁移到一个更新的版本。)

你可以在这里找到最初的示例应用[，在这里](https://github.com/milon/laravel-todo)找到最终的存储库[，在这里](https://github.com/hasura/laravel-todo-hge)找到一个供你测试[的实时应用。](http://laravel-todo-hge.herokuapp.com/)

这是我们规划的架构的样子:

![1*hEit6SqjtIbh0gdYFE0cGw](img/0ac1f34807f6a42c381daf1c53c24d9e.png)

Architecture before and after integration with HGE

### 设置图形 SQL 引擎

Hasura GraphQL 引擎 (HGE)在你现有的 Postgres 上给你一个即时的实时 GraphQL API。HGE 工程开箱即用您现有的:

*   [**Postgres 数据库**](#f087)**——**连接您现有的数据库，为您的数据库提供一个 GraphQL API。
*   [**认证系统**](#9050)**——**连接您现有的认证系统，以保护 GraphQL API。
*   [**迁移系统**](#50e3)**——**Hasura graph QL 引擎不干扰现有的 Laravel 迁移系统。只要不改变 GraphQL 引擎跟踪的模式，模式可以在 Laravel 中单独管理。更多关于 Hasura GraphQL 引擎如何管理你的模式状态的信息[在这里](https://docs.hasura.io/1.0/graphql/manual/engine-internals/index.html)。

此外，它还带有一个漂亮的控制台，集成了 GraphQL，这在调试 GraphQL APIs 时非常有用。

### 装置

使用下面的按钮可以在 Heroku 上安装 Hasura GraphQL 引擎

![1*dcgu-klnpwTWilYiGMdM6Q](img/c916dc06b59661d3e933b122aa2d488e.png)

Click this button to deploy the GraphQL engine to Heroku

或者在任何可以运行 Docker 的机器上。查看[入门](https://docs.hasura.io/1.0/graphql/manual/getting-started/index.html)部分了解更多信息。

出于本教程的考虑，我们在这里为我们的 Laravel 应用程序[设置了一个 HGE 实例(使用访问键`helloworld`，我们将在下面解释它是如何工作的)。](https://hge-laravel-todo.herokuapp.com/console)

#### [使用 Docker 安装](https://docs.hasura.io/1.0/graphql/manual/deployment/docker/index.html)

在安装 Hasura GraphQL 引擎之前，您需要一个 postgres 连接字符串。您可以从保存存储凭证的`config/database.php`文件或`.env`文件中获得此信息。

将细节整合在一起:

```
postgres://username:SECUREPASSWORD@host:port/database_name
```

遵循此处的说明[。](https://docs.hasura.io/1.0/graphql/manual/deployment/docker/index.html)

一旦 Hasura GraphQL 引擎启动，访问 [http://localhost:8080](http://localhost:8080) 打开 Hasura 控制台。控制台提供了一个 GraphQL 实例来轻松测试所有的 graph QL 查询、变异等等。

![1*dJlxjx1PHK5flNXiKVy5sg](img/d7aa140c9d9e8896e5070d2296aca3f5.png)

Hasura GraphQL Engine Console

现在转到 Data 选项卡，跟踪所有的表来创建即时 GraphQL APIs！

### 证明

默认情况下，HGE 以开发模式安装。HGE 跟踪的所有表/视图都可以查看/更新，而无需任何检查。对于生产环境，不建议这样做。Hasura 允许您为 GraphQL 模式中的每个字段定义粒度访问控制，即 Postgres 模式中的每个表或视图。这些访问控制规则可以使用每个请求中的动态变量。查看[文档](https://docs.hasura.io/1.0/graphql/manual/auth/index.html)了解更多信息。

可以通过配置一个 webhook URL 来防止直接访问 HGE，除非请求包含有效的`access-key`，否则将调用该 URL 来验证每个请求。

我们先对用户提一个简单的要求:

并在 GraphiQL 中运行它:

![1*VVG55IL6bFoDlYnCDvk9vg](img/399975c21b4dc2293997ad9bf8825ad1.png)

请注意，`x-hasura-user-id`被设置为“2”，而`x-hasura-role`被设置为“用户”。这些是`auth`割台，需要在[生产模式](https://docs.hasura.io/1.0/graphql/manual/deployment/docker/securing-graphql-endpoint.html)中通过`auth-hook`进行设置。(GraphQL 发动机用`access-key`和`auth-hook`启动)。

#### 安全图形 api

![1*nJKLzkgQXGd7a7ILDBAEfQ](img/91c71d78f7a27d054db2b7f5a10c63ff.png)

Webhook architecture

第一步是用一个`access-key`保护 HGE 并用一个 webhook 配置`auth-hook`，在这种情况下，web hook 将由 Laravel 应用程序提供服务。这个 webhook 将由 GraphQL 引擎调用，请求附有头。webhook 将返回适当的`x-hasura-role`和`x-hasura-user-id`，这可以通过使用请求传递的`Authorization`头验证用户来获得。

在这里，如果您使用 docker，auth-hook 主机将是您在 docker 桥接网络上的 IP 地址，否则就是您的 webhook URL。如果使用外部 Postgres 数据库，可以忽略`postgres`部分。

要在 HGE 的 Heroku 实例上设置`access-key` / `auth-hook`标志，[请按照这些指令](https://docs.hasura.io/1.0/graphql/manual/deployment/heroku/securing-graphql-endpoint.html)操作。我们假设 webhook 现在在`/hge-webhook`，稍后我们将在 Laravel 上设置它。

让我们再次尝试进行查询，看看响应是什么。

![1*afYqLtDIyrJyR7RU7iG6_w](img/6388329648cdbfc369c795bc2bd8726d.png)

这是因为我们还没有配置 webhook，甚至没有设置正确的`Authorization`头。

### 设置 Laravel webhook

让我们在 Heroku 上设置我们的示例应用程序，以便我们可以轻松地部署它并测试我们的更改。

出于本教程的目的，我们已经用 webhook [在这里](http://laravel-todo-hge.herokuapp.com)部署了一个示例应用程序。相应的 HGE 实例可以在这里访问[。(进入键:`helloworld`)](https://hge-laravel-todo.herokuapp.com/console)

您可以在这个示例应用程序上注册，并在应用程序上添加/删除待办事项。

现在让我们添加一个 webhook 来认证发送到我们的 HGE 实例的请求。我们将使用中间件来实现这一点，所以让我们首先生成一个中间件类。

`php artisan make:middleware webhookMiddleware`

让我们首先将路线添加到我们的`app/Http/routes.php`文件中:

现在我们将注册我们的中间件，将它添加到`routeMiddleware`下的`app/Http/Kernel.php`文件中:

现在让我们在`app/Http/Middleware/webhookMiddleware.php`设置实际的 webhook:

这个页面简单地使用`Authorization`承载令牌来启动一个会话，然后使用 Laravel 的 Auth 来检查并获取用户 id。您可以对此进行修改，以添加您的自定义会话/令牌逻辑，并验证身份验证。

如果通过身份验证，我们将把`x-hasura-role`和`x-hasura-user-id`变量作为 JSON 返回。这将向 HGE 验证请求。

现在，我们需要一种简单的方法来获取登录用户的会话令牌，所以让我们将它添加到我们的`resources/views/users/profile.blade.php`:

现在，登录并转到用户配置文件，查看您的新会话令牌:

![1*KvPgJCAvDSq3btHtZ-xw7A](img/867ed9752a139fbef185321057c27729.png)

让我们将它提交并部署到 Heroku:

`git commit -am "Add HGE webhook"`

`git push heroku master`

一旦它被推出，让我们前往 HGE 控制台测试我们的新网页钩子！

![1*2hIa4anl3efGIqbJv2dgLw](img/0b3944090fab96be9d1bc995113e3648.png)

Authorization using Bearer token

webhook 返回相应的`x-hasura-user-id`和`x-hasura-role`，GraphQL 引擎响应访问规则中配置的适当结果。

### 迁移系统

HGE 附带了一个强大的受 Rails 启发的迁移系统，当以`hasura console`身份运行时，在 HGE 控制台中所做的更改会自动在您的文件夹中生成模式文件(您可以为此安装 [Hasura CLI](https://docs.hasura.io/1.0/graphql/manual/hasura-cli/install-hasura-cli.html) )。

不过，出于这个博客的目的，我们将让 Laravel 处理我们的迁移，只导出 HGE 元数据，以便它可以单独跟踪模式和权限。

你可以查看 [HGE 文档](https://docs.hasura.io/1.0/graphql/manual/migrations/database-with-migrations.html)以获得更详细的说明。

一旦按照上面的链接设置好了一切，您就可以将创建的文件夹添加到 Laravel 代码的版本控制存储库中。

要导出元数据，请在由迁移说明中的`hasura init`命令创建的文件夹中运行以下命令:

`hasura metadata export`

因为我们让 Laravel 处理迁移，所以避免通过 Hasura 控制台进行模式更改，这样 Laravel 迁移仍然是模式的真实来源。

就是这样！我们现在有了一个安全的 HGE 端点，它可以与 Laravel 的内部授权很好地协同工作。前进并编写代码！

[***Hasura***](https://goo.gl/fR68ep)*让你在任何 Postgres 数据库上即时实时 GraphQL APIs，而无需编写任何后端代码。*