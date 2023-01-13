# 别再叫 PostgREST“神奇”了！

> 原文：<https://www.freecodecamp.org/news/stop-calling-postgrest-magic-8f3e1d5e5dd1/>

by Ruslan Talpă

# 别再叫 PostgREST“神奇”了！

![VaeEEzr3WELlhev42HXCXIDo-c7XyeN24L3m](img/aa1cba20dce25a62d0ceb775a9a95d7c.png)

Doug Henning

虽然让“魔法”与他们的工作联系起来对开发者来说是一种恭维，但这可能会对采用 [PostgREST](https://github.com/begriffs/postgrest) 造成损害。魔法意味着未知，人们害怕未知。让我们花 10 分钟来了解它的内部工作原理。

我[两年前开始给 PostgREST core 贡献](https://github.com/begriffs/postgrest/graphs/contributors?from=2015-08-22&to=2017-06-27&type=a)。从那以后，我一直在调查人们对此的看法。有两个阵营。那些称之为“魔法”并准备联合起来对付非信徒的死忠粉丝们。然后是怀疑论者。

这种叙述回应了怀疑论者的阵营。源于人们不了解 PostgREST 是如何工作的！

一切都很简单，也很美好。

### (REST) URL 中有什么？

让我们举这个例子

```
GET /items/1
```

如果去掉所有东西，比如身份验证和授权，剩下的本质就是这个

```
SELECT * FROM items WHERE id=1
```

PostgREST 接收一个 HTTP 请求，查看它，将其转换为 SQL 并执行它。这几乎就是全部了。这是一个很大的纯函数。我希望我不会被 Haskell 的人钉死在十字架上，因为他们滥用了符号并忽略了实际上正在发生的 IO。

```
postgrest :: Schema -> HTTP -> SQL
```

这里有一个稍微复杂一点的例子:

```
GET /items?select=id,name&id=gt.10&order=name
```

被翻译成

```
SELECT id, name FROM items WHERE id > 10 ORDER BY name
```

### ✨的三种神奇成分

PostgREST 建立在三个核心概念之上，围绕它们的一切都是华而不实的。如果你理解它们，你就理解了 PostgREST。

#### JSON 编码

Joe Nelson 的第一个绝妙想法是认识到 PostgreSQL 可以对响应进行 JSON 编码。现在你可能会想“为什么要这样做，把更多的负载放在最难扩展的组件上？”

嗯，原因如下。

PostgreSQL JSON 编码器很快，是 C 的速度。你的回应越大，收获越大。它可以比 [ActiveRecord](https://dockyard.com/blog/2014/05/27/avoid-rails-when-generating-json-responses-with-postgresql) 快 2 倍/10 倍，对于大型响应甚至快 160 倍。

让我们想一想。除了让数据库做更多的工作之外，让数据库返回最终响应还意味着什么？

无论数据库前面是什么，都不再需要对来自数据库的响应进行反序列化，并将其转换为该数据的内部语言表示。然后，在数据进入内存后，它不需要序列化回 JSON。这意味着在退出时，您的代码不需要做任何事情，只需传递它从数据库中获得的任何内容。

我们将一些负载从应用程序代码转移到了数据库。对于强大的 PostgreSQL 来说，这没什么，这不是繁重工作发生的地方。如果我们看整个系统，我们已经通过删除一个序列化/反序列化步骤减少了需要完成的工作总量。这也是 PostgREST 为什么这么快的秘密之一。它让大家伙做它的事情。

让我们看看这在 SQL 中实际上是什么样子。这个查询类似于 PostgREST 生成的查询。

```
WITH essence AS (  SELECT id, name FROM items WHERE id > 10 ORDER BY name)SELECT   coalesce(    array_to_json(array_agg(row_to_json(response))),    '[]'  )::character varying AS BODYFROM (SELECT * FROM essence) response
```

我们已经包装了`essence`查询。另一方面，我们得到的结果是单行，其中有一个名为`BODY`的列，包含 JSON 响应。轻松点。

#### 认证/授权

这是大部分新人第一次遇到 PostgREST 都有困扰的部分。他们有一个(非常自然的)问题:“PostgREST 如何实现认证/授权？”。

简短的回答是，**不**。

这又是 Joe 的一个绝妙想法，我甚至可以说它比 JSON 编码更基础。

PostgreSQL 有丰富的[角色系统](https://www.postgresql.org/docs/current/static/user-manag.html)。结合视图和[行级安全性](https://www.postgresql.org/docs/9.6/static/ddl-rowsecurity.html) (RLS)等特性，您可以细粒度地控制对数据的访问，直到单个单元格。然而，人们将他们的数据库视为“愚蠢的数据存储”。更糟糕的是，他们使用具有管理员权限的角色连接到这些服务器。

如果应用程序连接到数据库的角色只对特定的表和行拥有特权，那么 SQL 注入攻击的目的是什么？最坏的情况是，攻击者会删除自己的数据。但是我跑题了…

当第一个 PostgREST 连接到数据库时，它使用一个我们通常称之为`authenticator`的角色来连接数据库。除了能够登录之外，该角色没有任何特权。

每当一个经过身份验证的请求进入，并且有一个包含 JWT 令牌的`Authorization`报头，PostgREST 将解码该令牌，使用秘密密钥验证它的有效性，并查看它的有效载荷中一个名为`role.` 的特定字段，假设`role` 的值为`alice`。

这意味着这个请求需要用`alice`的特权来执行。为此，我们使用了一个 PostgreSQL 的小技巧。你可以**切换**当前用户，有点像做`sudo alice.` 甚至更好，你可以在事务的上下文中这样做，这样一个请求就不会干扰另一个请求。我们没有创建新的数据库连接，这发生在同一个连接中。虽然`alice` 是一个数据库角色，但是它没有登录权限。

事情发生的顺序如下:

```
BEGIN;SET LOCAL role TO 'alice';-- the main query goes hereCOMMIT;
```

这并不意味着 PostgREST 可以随心所欲地切换。要做到这一点，您必须通过以下方式明确声明`authenticator`角色有权承担`alice`角色:

```
GRANT alice TO authenticator;
```

就这样，PostgREST 通过利用底层数据库角色系统获得了授权的能力。这种方法的意义和优势是巨大的。

PostgREST 代码库仍然很小很简单。您可以使用 SQL 为每个用户声明权限，而不必到处使用难看的命令式代码。而且，不管将来有什么代码访问数据库，这些规则都会被一致地应用。

还有一个不那么明显的优势。

在构建 API 的传统方式中，当请求进来时，您开始检查当前用户是否有权访问该信息。这意味着额外的查询。这些类型的查询在请求总延迟和数据库负载中起着很大的作用。这给人的印象是数据库很慢，伸缩性也不好。

通过让 PostgreSQL 全面了解发出查询的人(角色)、他的特权(授权)和限制(RLS)，查询规划器可以做得更好，利用所有的索引，甚至随着时间的推移变得更快。

提出的第一个异议是，每个应用程序用户都需要有一个数据库角色，这不是一个很好的设计。他们可能是对的，但这不是 PostgREST 要求你做的。您可以使用数据库角色来设置用户组(管理员、员工、客户)，然后使用 RLS 根据用户名、id 或电子邮件来指定[用户规则](https://blog.2ndquadrant.com/application-users-vs-row-level-security/)。

#### 资源嵌入

您可能会有这样的印象，尽管 PostgREST 有一个很好的认证和公开 REST 端点的解决方案，但它只是您的基本 CRUD 功能。这对于现代 API 来说是不够的。毕竟，这就是为什么我们有 GraphQL 这样的东西。

你是对的，如果它所做的只是基本的 CRUD，那么它将是原型开发或简单项目的一个很好的工具。但是 PostgREST 还有最后一招，我很自豪地称之为我最大的[贡献](https://github.com/begriffs/postgrest/issues/218)。诀窍在于`&select=`参数。它不仅允许您指定希望从表中返回哪些列，还可以请求相关的资源:

```
GET /items?select=id,name,subitems(id,name)
```

如果把`()`换成`{}`，稍微眯一下眼睛，几乎可以看到 GraphQL。当从数据库中获取数据时，PostgREST 接口在表达能力上与 GraphQL 不相上下。

我们花了一整年的时间重写了核心内容，然后花了一年的时间让所有路径的接口都统一起来。但我们最终还是到了那里。

那么这是如何工作的呢？

在第一次启动时，PostgREST 在你的数据库上运行一系列的[查询](https://github.com/begriffs/postgrest/blob/master/src/PostgREST/DbStructure.hs)。这是为了根据您定义的外键来理解哪些实体存在以及它们之间的关系。此后，每当您说`subitems(...)`时，它就知道表`items`通过一个名为`item_id`的外键与表`subitems`相关。基于这些信息，它知道如何生成正确的连接查询。这同样适用于父关系和多对多关系。

一个简化的(本质的)查询如下所示

```
SELECT    items.id, items.name,    COALESCE(         (            SELECT array_to_json(array_agg(row_to_json(subitems)))            FROM (                SELECT subitems.id, subitems.name                FROM subitems                WHERE subitems.item_id = items.id             ) subitems         ),         '[]'    ) AS subitemsFROM items
```

乍一看，人们可能会认为这是非常低效的，因为它似乎对每个项目行都进行了子选择，但我们使用的是 PostgreSQL，它是一个非常棒的查询规划器，它知道您实际上在要求什么，它会支持您。不要害怕连接和子选择。

有一次，在展示了这个查询并解释了查询规划器知道如何正确处理它之后，我收到了这样的回复

“哈哈，是的，你依赖于查询优化器的仁慈。”

我哪天都愿意，非常感谢！

我将依赖一个由拥有博士学位的人在 T2 开发了 20 年的软件。我没有自大到认为我的`for`循环是比 PostgreSQL 查询[优化器](https://momjian.us/main/writings/pgsql/optimizer.pdf)更高级的技术。

### 但是为什么(我们需要 PostgREST)？？

现在，如果你跟随并理解事物是如何结合在一起的，你可能会想:

> “PostgREST 所做的只是生成一个 SQL 查询，用某种上下文将其封装在一个事务中并执行它。为什么我不跳过中间人，编写一个脚本，以原始 SQL 作为输入，做同样的事情？”

你可以这么做。事实上，没有人能够访问他们不应该访问的数据，事情暂时会好起来。然后，有一天晚上，你醒来时会被警告说你的数据库坏了。经过一番调查，您会发现这个查询是罪魁祸首:

```
SELECT   crypt(    encode(digest(gen_random_bytes(1024), 'sha512'), 'base64'),       gen_salt('bf', 20)  )FROM   generate_series(1, 1000000)
```

WT#！

更糟糕的是，我不需要任何表的特权来运行这个查询！

尽管 PostgreSQL 可能无法抵御拒绝服务(DoS)攻击。这就是 PostgREST 拯救你的地方。它在向客户机提供大量有用的 SQL 灵活性和限制防止恶意查询的能力之间取得了平衡。它不允许**随意**连接，只允许那些由外键定义的应该有正确索引的连接。

### 做好一件事？

所以你被卖了！您已经下载了二进制文件，将它指向您的数据库，然后嘭！你有一个 REST API！事情进展得很顺利。您的大多数 API 需求都得到了满足。然后你到了要实现的最后一点，突然你停下来了！

“我怎么用这个东西发邮件呢？当用户登录时，我如何调用这个第三方 API？我如何写我的测试？

该死，我就知道这不是个好主意！嘿 PostgREST 开发者，你能帮我实现这个功能吗？"

你最有可能得到的答案是“你不会需要它”(YAGNI)。那不是因为我们是 d#$%#。是因为 PostgREST 是你栈的一个组件，不是“栈”。它只有一项工作，并且努力做好它。就其本身而言，它不会完全取代你喜欢的后端 MVC 框架。但是在 PostgreSQL、OpenResty、RabbitMQ 等朋友的帮助下，它会帮你做到这一点，而且效果很好。看一看[初学者工具包](https://github.com/subzerocloud/postgrest-starter-kit)，看看它是如何放入堆栈中的。

您将不再编写 API，而是定义和配置它们。

### 超越休息？

最近，前端社区被 React & GraphQL 热潮所接管，这是有原因的。似乎休息很快就会被抛在身后，带走了波斯特。然而这里的想法超越了 REST，它是你用来和 PostgREST 对话的协议。

你可能听说过 [PostGraphQL](https://github.com/postgraphql/postgraphql) 。它基于相同的思想，但实现方式完全不同。《T2》的作者也是 PostgREST 的主要贡献者之一。受到 PostgREST 和 PostgREST 社区中关于 GraphQL 的讨论的启发，他决定加入自己的观点。

我决定走一条不同的路线。我使用 PostgREST 并在其上构建 GraphQL，而不是用另一种语言重新实现相同的逻辑。毕竟，这是我从一开始就有的目标。将 PostgREST 功能构建到可以支持 GraphQL 的程度。

为数据库开发 graph QL&REST API[sub zero](https://subzero.cloud/)，这是一个漫长的旅程。但是一路走来我学到了很多。

在零度以下兜一圈。我希望我们一路上开发的额外的[软件](https://github.com/subzerocloud)对你有用。

尽情享受吧！

如果你有任何问题，你可以通过电子邮件或[联系我。](https://slack.subzero.cloud/)