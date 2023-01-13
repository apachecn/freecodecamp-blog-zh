# 如何使用名为 Reno Expo 的简单入门套件构建全栈应用

> 原文：<https://www.freecodecamp.org/news/reno-expo-full-stack-starter/>

从头开始构建任何新项目都可能令人生畏。在你开始编码测试你的想法之前，有很多事情要做决定。

你是如何构建前端的？纯 CSS，还是框架？香草 HTML 和 JS，还是 Vue、React、Angular、Svelte 之类的框架或库？

你将使用什么样的后端语言？JavaScript，Ruby，PHP，Python 还是别的？也许是“无服务器”？

数据库呢？关系？MySQL，Postgresql？NoSQL？蒙哥？也许一些简单的东西，比如 Firebase？

你将如何处理认证？也许是一个整合了脸书、谷歌、Github 和 LinkedIn 登录界面的 Passport？

每当我有一个很酷的想法，想自己开发一个小应用程序时，我总是被要做出的一系列选择和决定弄得筋疲力尽。

因此，我花了一些时间来思考我的理想堆栈，包括身份验证和部署方面的考虑，并将其打包成一个相当容易安装的包:Reno Expo。

# 雷诺博览会是什么？

Reno Expo 代表 React、NodeJS、Express 和 Postgresql。它还使用 Sequelize 作为数据库的 ORM。

其核心是一个非常简单的快速应用程序，前端捆绑了一个 Create React 应用程序。它被设计用来部署到 Heroku，有一个非常简单的界面来注册新用户和登录，使用 JWT 进行身份验证。

除此之外，这是一个完全空白的石板。就 CSS 样式而言，我故意把它留得很空，这样我就可以插入任何我想要的样式库，或者根据需要编写自己的 CSS。

除了 Reno Expo 的完全原始版本，我还使用这个堆栈制作了一个 freeCodeCamp 项目，个人图书馆。它是如何集成 CSS 框架的一个例子，在这个例子中是 Ant 设计，还提供了一些用顺序迁移扩展数据库的例子。

## 我能在哪里得到它？

这两个应用程序的代码可以在这里找到:

*   [雷诺世博会入门套件](https://github.com/JacksonBates/reno-expo)
*   [个人图书馆，与雷诺博览会一同建造](https://github.com/JacksonBates/reno-expo-books)

每一个都有一个详细的 README.md 文件，但我也将解释如何开始使用这两个文件，并解释后一个应用程序如何在本文的初学者工具包的基础上构建。

## 这些应用程序看起来像什么？

这两款应用的示例可在此处找到:

*   [雷诺博览会，Heroku 举办的例子](http://renoexpo.herokuapp.com/)
*   [Heroku 上的个人图书馆](https://reno-expo-books.herokuapp.com/)

坦率地说，初学者工具包很难看。正如我前面所说的，它有最小的风格-我不是在开玩笑。个人库展示了如何集成 CSS 框架，以最小的努力获得一些简单的样式。

## 雷诺博览会入门

要使用 Reno Expo，您需要在本地计算机上安装以下软件: [git](https://git-scm.com/) 、[节点](https://nodejs.org/en/download/)、npm(与您的节点下载捆绑在一起)和 [Postgresql](https://www.postgresql.org/) 。

如果您是从零开始，请使用每种软件的最新版本，但是如果您的系统上已经有了这些软件的其他版本，它们可能会工作得很好。

声明一下，我一直在用这些版本进行开发:Node 8.16、npm 6.14 和 Postgres 10，我的 Heroku 部署是所有这些版本的最新稳定版本。

如果您在使用不同版本时遇到问题，要么使用这些版本，要么尝试在适当的变更日志中寻找差异，以帮助您摆脱困境。

您的 Postgres 安装还需要一个拥有数据库创建权限的有效用户。设置这一点超出了本文的范围，但是您可以通过在线搜索“postgres windows/mac/ubuntu 入门”等来找到适用于您的环境的相关指南。

### 安装初学者工具包

为了初始化初学者工具包，我们将使用两个终端。稍后，我将分享一个从单个终端加速开发环境的技巧，但是现在我们将前端和后端分开。

在终端一中，从您希望创建新应用程序的目录中:

`git clone git@github.com:JacksonBates/reno-expo.git`

然后导航到新文件夹:`cd reno-expo`

制作一份。环境文件:`cp .env.example .env`

您需要在新的中调整发展变量。环境文件:

```
DEVELOPMENT_DATABASE=database_development
DEVELOPMENT_DATABASE_USERNAME=sequelize
DEVELOPMENT_DATABASE_PASSWORD=password
```

开发数据库可以是您喜欢的任何数据库，但是用户名和密码必须与您为本地 Postgres 安装配置的任何数据库相匹配。

现在安装后端的 npm 包:`npm i`

接下来，我们将使用 Sequelize 创建数据库。请注意，如果您的 Postgres 安装没有正确设置，这是第一个会出错的地方...

`npx sequelize-cli db:create`

这将使用您在中设置的名称创建数据库。环境文件。

现在，我们可以为用户创建表格:

`npx sequelize-cli db:migrate`

如果这有效，您应该会看到一些终端输出，例如:

```
== 20200606113054-create-user: migrating =======
== 20200606113054-create-user: migrated (0.074s)
```

如果所有这些都工作了，您现在可以用`npm start`启动后端服务器。

设置前端应该更简单。在第二个终端中，导航至客户端文件夹:`reno-expo/client`

安装节点模块:`npm i`

现在用`npm start`运行 React app。

### 单终端发射

如果一切初始化正常，将来您可以通过单个终端的一个命令轻松启动 React 应用程序和 Express 服务器:

`npm run dev`

### 检查它是否工作

在您选择的浏览器中，访问 localhost:3000，您应该会看到一个非常基本的“主页”和一些到管理和登录页面的链接。

管理员应该被锁定，直到你登录。

登录将要求您首先创建一个用户帐户。点击“我没有帐户”,通过注册表创建一个帐户。您现在可以登录并测试您对管理页面的访问。

如果一切正常，你就可以开始开发你的应用了！

## 用里诺博览会建造一些更实在的东西

要创建个人图书馆，有 3 件主要的事情要做:

1.  安装并实现 Ant Design CSS 框架
2.  创建新的前端路线/视图
3.  用新的数据库模型和控制器扩展 api

在用`npm i antd`安装了 Ant Design 之后，我在`client/styles`文件夹中现有的 App.css 文件中添加了下面一行:`@import"~antd/dist/antd.css";`

这确保了 Ant 设计样式将在整个应用程序中可用。

个人库的 repo 包含所有的修改，但是这里有一些你可以使用的模式的例子。当然，你可以开发你自己的 CSS，或者使用其他框架，比如 Material-UI，Bootstrap 或者其他，下面只是举例说明。

### 实现布局

```
import React from "react";
import { Layout, Menu } from "antd";
import { Link } from "react-router-dom";
import { useAuth } from "../../context/auth";

const { Content, Sider, Footer } = Layout;

export default function AppLayout(props) {
  const { setAuthTokens } = useAuth();

  const logout = (e) => {
    e.preventDefault();
    setAuthTokens();
    localStorage.removeItem("tokens");
  };

  return (
    <Layout>
      <Sider breakpoint="md" collapsedWidth="0">
        <Menu theme="dark" mode="inline">
          <Menu.Item key="1">
            <Link to={"/"}>
              <span className="nav-text">Home</span>
            </Link>
          </Menu.Item>
          <Menu.Item key="2">
            <Link to={"/personal"}>
              <span className="nav-text">Personal Library</span>
            </Link>
          </Menu.Item>
          <Menu.Item key="3">
            <Link to={"/public"}>
              <span className="nav-text">Public Library</span>
            </Link>
          </Menu.Item>
          <Menu.Item key="4">
            <Link to={"/login"} onClick={logout}>
              <span className="nav-text">Log out</span>
            </Link>
          </Menu.Item>
        </Menu>
      </Sider>
      <Layout style={{ minHeight: "100vh" }}>
        <Content style={{ margin: "24px 16px 0" }}>
          <div style={{ padding: 24, background: "#fff", minHeight: "80vh" }}>
            {props.children}
          </div>
        </Content>
        <Footer>Reno Expo Books</Footer>
      </Layout>
    </Layout>
  );
} 
```

reno-expo-books/client/src/components/layouts/AppLayout.js

除了一个处理 auth 令牌的小函数之外，其余部分使用 Ant Design 提供的组件来创建一个具有持久侧栏的导航应用程序，并根据活动组件动态呈现内容。

活动组件在哪里加载？

```
import React from "react";
import { Route } from "react-router-dom";
import { AppLayout } from "../layouts";

export default function PublicRoute({ children, ...rest }) {
  return (
    <Route {...rest}>
      <AppLayout>{children}</AppLayout>
    </Route>
  );
}
```

reno-expo/client/src/components/routes/PublicRoute.js

上面我们有一个 PublicRoute 组件的例子。我还使用了一些其他的路由组件，但是基于这个组件，理解它们应该很简单。

我们的 PublicRoute 是一个从上面包装布局的 React-Router `<Route>`。

App.js 显示了正在使用的这些公共路线的示例，例如:

```
<PublicRoute exact path="/">
  <Home />
</PublicRoute> 
```

所以在前两个文件中我们可以看到对`children`的引用。

`children`是 React 中的内置属性，它引用嵌套在父组件中的子组件。

在上面的例子中，我们看到`<Home>`组件是`PublicRoute`的子组件。在 PublicRoute.js 文件中，我们看到了对 children 的引用，既在 props 中，也直接传递给了`<AppLayout>`组件。最后，在 AppLayout.js 中,`<Content>`组件也包含子组件。在所有这些情况下，children 指的是从 App.js 传递过来的那个`<Home>`组件

实际上，这意味着在公共或私有路径上从 App.js 传递的任何组件都将被呈现到我们布局上的内容区域中，而导航侧栏保持不变。

client 文件夹中的其他文件应该给出足够的例子，说明如何在做了一些小的修改后，用 Ant 设计框架替换登录表单之类的东西。

### 对后端进行更改

使用 Reno Expo 时要开发的另一个主要东西是 api 本身——毕竟，能够注册用户并让他们登录是有用的，但大多数应用程序需要更多的东西才能真正有用。

对于我的个人图书馆版本，我们需要实现一些新的 api 端点，并创建一些新的数据库表来存储书籍和评论数据。

这里值得强调的是，在这些例子中，我是逆向工作的。通常我会首先创建数据库迁移和模型，然后构建控制器方法和 api 路由。我在这里“向后”展示它们，这样我们就可以从我们的目标开始，一点一点地追溯它是如何实现的。

文件`reno-expo-books/app/router/router.js`包含了项目的所有路线，但是我将在这里分享两个例子来说明。

```
// Public route
app.get("/api/books", booksController.getBooks);

// Private route
app.get(
    "/api/user/books",
    [authJwt.verifyToken],
    booksController.getUserBooks
  ); 
```

添加公共路由非常简单，我们只需要定义 http 方法、api 端点和控制器方法来处理请求，例如对 booksController `getBooks`方法处理的`api/books`的 GET 请求。

我们已经从里诺世博会的发起人那里得到了 JWT 的授权，这使得私人路线也变得非常简单。我们所需要做的就是包括验证令牌的中间件，在上面的例子中是`[authJwt.verifyToken]`。

这些端点的控制器，即处理请求的代码，也相当简单，尽管第一次使用 Sequelize 可能需要一点学习过程。

下面是上面提到的公共“getBooks”方法的一个示例:

```
const db = require("../../models/index");
const Sequelize = require("sequelize");
const Book = db.Book;
const BookComment = db.BookComment;

// Public routes

exports.getBooks = (req, res) => {
  Book.findAll({
    where: { userId: null },
    attributes: {
      include: [
        [
          Sequelize.fn("COUNT", Sequelize.col("BookComments.id")),
          "commentcount",
        ],
      ],
      exclude: ["createdAt", "updatedAt"],
    },
    include: {
      model: BookComment,
      attributes: [],
    },
    group: ["Book.id"],
  })
    .then((data) => res.status(200).json(data))
    .catch((error) => res.send(error));
}; 
```

reno-expo-books/app/controller/booksController.js

文件顶部的导入提供了数据库模型和序列库。

方法看起来很复杂，但它是由几个相对简单的部分组成的。

首先我们称之为`Book`模型。这个模型是 books 表的 ORM 表示——我们很快就会看到如何创建这个表。

与大多数 ORM 一样，Sequelize 不仅提供了表的模式或描述，还提供了可以在模型上调用的方法。在这种情况下，我们调用`Book.findAll({...})`，它将返回它所能找到的与我们传递给它的特定参数相匹配的所有书籍。

在这个特殊的例子中，我希望收到这样的内容:

```
[
  {
    "id": 1,
    "title": "The Hobbit",
    "commentcount": 3
  },
  {
    "id": 2,
    "title": "The Lord of the Rings",
    "commentcount": 2
  }
]
```

在 findAll 方法中，首先我们传递`where`参数。如果您熟悉 SQL，应该很清楚这是做什么的。在上面的例子中，我们想要 userId 为空的所有书籍。这是因为我们只想从这个控制器返回公共书籍，所以只返回那些没有用户的书籍。

接下来，`attributes`描述了响应的形状，或者我们期望返回的数据。exclude 部分比较好理解，我先解释一下。books 表中有每个记录的 created_at 和 updated_at 时间戳列。因为我们不希望在 json 响应中包含这些内容，所以我们可以在 exclude 部分显式地省略它们。

属性的 include 部分更加复杂。在原始 SQL 中，我们会像这样计算相关注释的数量:

```
SELECT "Books"."id", "Books"."title", COUNT("BookComments"."id") AS "commentcount"
FROM "Books"
JOIN "BookComments" ON "BookComments"."bookId" = "Books"."id"
GROUP BY "Books"."id";
```

SQL COUNT 函数对 BookComments.id 列中的所有记录进行计数，GROUP BY 函数根据 id 将计数的注释限制在每本书中。

值得指出的是，commentcount 不是 books 表中的一列，而是从我们请求数据库为我们计数的内容中导出的一个计算列。

Sequelize 让我们可以通过它的库访问 count 函数。

相关函数包含在上述属性中，如下所示:

```
[Sequelize.fn("COUNT", Sequelize.col("BookComments.id")),"commentcount"] 
```

即“调用 Sequelize 函数‘COUNT’，对 BookComments.id 的列进行计数，并将生成的列命名为‘comment COUNT’

这提供了我们的计数功能，就像在 SQL 版本中一样。剩下的就是将`group: ['Book.id']`作为图书模型上 findAll 方法的额外参数。

您可能已经注意到 findAll 方法参数中的另一部分是:

```
include: {
  model: BookComment,
  attributes: [],
},
```

没错，*另一个*包括。注意，这一个没有嵌套在属性中，而是它的对等体。这个 include 的行为与上面 SQL 中的 JOIN 语句非常相似。这意味着我们想要包含 BookComment 模型，但是我们不需要添加任何属性，因为我们不想直接引用它拥有的任何列——我们只是在 count 函数中使用它。

### 对数据库进行更改

我们需要理解的最后一件事是，为了使用 Sequelize 提高效率，需要进行数据库变更的迁移。

可以将迁移视为数据库的源代码控制。

虽然您可以通过创建新表、添加列、引入约束、更改数据类型或其他方式直接修改数据库，但是使用迁移允许您进行实验性的更改，并且能够轻松地回滚这些更改，并且能够与其他人共享您在数据库上的开发，而不必费力地保持他们的本地数据库和 prod 同步。

迁移本质上是告诉您的数据库如何更改，以及如何在必要时撤销引入的更改的代码。

下面是创建 books 表的迁移:

```
'use strict';
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('Books', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      title: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: (queryInterface, Sequelize) => {
    return queryInterface.dropTable('Books');
  }
};
```

reno-expo-books/migrations/20200609123224-create-book.js

这个迁移是一个导出两个函数的模块:`up`和`down`

`up`函数进行修改，而`down`函数撤销任何引入的修改。

这里我们看到 up 创建了一个名为“Books”的表，其中包含列 id、title、created_at 和 updated_at。每一列都有一些相关的属性，比如数据类型，以及它是否可以包含空值。

down 函数只是删除表格。

我不会分享下一次迁移的全部内容，即创建 Book Comment 表的迁移，但我会展示定义 bookId 列的 up 函数的一个片段:

```
bookId: {
  type: Sequelize.INTEGER,
  onDelete: "CASCADE",
  references: {
    model: { tableName: "Books" },
    key: "id",
  },
  allowNull: false,
}, 
```

这里要注意的有趣的事情是设置为“CASCADE”的`onDelete`属性和通过 id 列链接 books 表的`references`属性。这就建立了一本书和它的评论之间的关系。onDelete 属性告诉数据库如果一本书被删除了该怎么办:删除操作应该级联到所有相关的评论。也就是说，如果我删除《霍比特人》,所有与《霍比特人》相关的评论也会被删除。

最后需要注意的是，这些迁移也受模型的支持。BookComment 模型如下所示:

```
"use strict";
module.exports = (sequelize, DataTypes) => {
  const BookComment = sequelize.define(
    "BookComment",
    {
      comment: DataTypes.TEXT,
      bookId: {
        type: DataTypes.INTEGER,
        references: {
          model: "Books",
          key: "id",
        },
      },
    },
    {}
  );
  BookComment.associate = function (models) {
    // associations can be defined here
    BookComment.belongsTo(models.Book, {
      foreignKey: "bookId",
    });
  };
  return BookComment;
}; 
```

您会注意到这和迁移之间的一些相似之处。它有两个部分，模型定义和模型关联。这些有助于在必要时加强各个表之间的关系。

要从头开始创建表，可以使用如下命令:

`npx sequelize-cli model:generate --name Post --attributes post:text`

这将自动为带有“post”列的`posts`表生成一个模型框架和一个迁移框架。然后，您可以用任何其他相关的列细节或关联来填充迁移和模型。

如果您只想修改现有的表，例如，更改数据类型或添加列，您可以使用命令仅生成迁移:

`npx sequelize-cli migration:generate --name add-userId-to-posts`

然后，您可以根据需要对现有模型和新的迁移文件进行更改。

### 应用数据库更改

简单地编写代码来更新数据库是不够的，您还需要为您的代码工作的每个数据库运行迁移——例如，您的本地开发机器，可能是您的临时服务器(如果有)以及您的生产服务器。

运行这些程序的命令是:

`npx sequelize-cli db:migrate`

您也可以回滚迁移:

`npx sequelize-cli db:migrate:undo`

## 编码快乐！

就是这样！正如我上面提到的，我亲自将我用这些制作的一切部署到 Heroku，在 Reno Expo 的 README.md 中有关于将它们部署到 Heroku 的细节的详细说明。这也包括在 Heroku 的服务器上运行迁移的命令。

这里有很多东西要吸收。但是，如果您从根本上理解 Express 和 React，并且愿意在需要时深入研究 Sequelize 文档，那么您可以使用这个初学者工具包构建几乎任何您可以想象的受益于关系数据库的东西。

它不像 Rails、Laravel、Sails 或 Nest 这样的 MVC 框架那样功能齐全，但我碰巧喜欢它内部隐藏的更少的 cruft 和更少的`magic`。毕竟，它只是一个捆绑了轻量级服务器和 ORM 包的 Create React 应用程序。剩下的就看你的了。

* * *

如果你读到这篇文章的结尾，特别是如果你在雷诺博览会上做了什么，我很乐意收到你的来信。可以在推特上联系我: [@JacksonBates](https://twitter.com/JacksonBates)