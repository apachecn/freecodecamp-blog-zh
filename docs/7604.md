# 使用构建实时评论功能。网和推动器

> 原文：<https://www.freecodecamp.org/news/build-a-real-time-commenting-feature-using-net-and-pusher-4feada408812/>

作者奥贡迪普·塞缪尔

# 使用构建实时评论功能。网和推动器

今天，我们将使用构建一个具有实时评论功能的微型博客引擎。网和推动器。

![0*dk8lZiI7UJOq2eLs](img/067fe783dabca70a58c254835bdb6774.png)

重新加载页面来查看新的评论既无聊又费力，因为你甚至不知道对你评论的回复来了没有。你一直在重新加载，一直在浪费你的数据。长话短说，用户可能会放弃那些必须重新加载页面才能看到新评论的网站。

为了完成本教程，我们将使用 MSSQL 作为我们的数据库引擎。请确保它已启动并正在运行。

要学习本教程，请确保您熟悉以下基础知识:

### 设置推手帐户和应用程序

[Pusher](https://pusher.com/) 是一种托管服务，可以非常容易地将实时数据和功能添加到 web 和移动应用程序中。

Pusher 充当服务器和客户端之间的实时层。Pusher 保持与客户端的持久连接(如果可能的话，通过 Web-socket，并退回到基于 HTTP 的连接)，这样一旦您的服务器有了新数据，它们就可以通过 Pusher 将数据推送到客户端。

如果你还没有，去 Pusher 创建一个免费账户。

我们将在仪表板上注册一个新的应用程序。唯一必须的选项是应用程序名称和集群。一个集群代表 Pusher 服务器的物理位置，它将处理您的应用程序的请求。此外，从应用程序密钥部分复制出您的应用程序 ID、密钥和密码，因为我们稍后会用到它们。

### 在 Visual Studio 中设置 Asp.Net 项目

我们需要做的下一件事是创建一个新的`Asp.Net MVC application`。

为此，让我们:

*   打开`Visual Studio`并从工具条中选择`New Project`
*   在模板下，选择`Visual C#`
*   接下来，选择`Web`
*   在中间部分，选择`ASP.NET Web Application`
*   对于本教程，我将项目命名为:`Real-Time-Commenting`
*   现在我们差不多准备好了。下一步将是使用`NuGet Package`为`ASP.NET`安装官方的`Pusher`库

为此，我们转到顶栏上的工具，单击`NuGet Package Manager`，在下拉菜单中选择`Package Manager Console`。

我们将在 Visual Studio 的底部看到`Package Manager Console`。接下来，让我们通过运行以下命令来安装该软件包:

### 制作我们的应用程序

现在我们的环境已经设置好了，让我们开始编写代码。

默认情况下，Visual Studio 为我们创建了三个控制器。但是，我们将为应用程序逻辑使用 HomeController。

我们要做的第一件事是定义一个模型来存储数据库中的文章列表。我们姑且称这个模型为`BlogPost`。因此，让我们在 models 文件夹中创建一个名为`BlogPost.cs`的文件，并添加:

在这个代码块中，我们定义了保存博客文章的模型。我们在此定义的属性包括:

*   帖子的 id，叫做`BlogPostID`(通常是主键)。
*   我们帖子的标题，叫做`Title`(定义为一个字符串)
*   我们将要创建的文章的主体(定义为一个字符串)

接下来，让我们创建一个名为`Comment`的模型，我们之前已经提到过。让我们在 models 文件夹中创建一个名为`Comment.cs`的文件，并添加:

查看上面的代码，我们注意到我们已经声明了以下属性:

*   我们的注释的 ID 叫做`CommentID`(通常是主键)
*   评论者的姓名
*   注释的正文
*   我们正在评论的帖子的 ID

现在我们已经定义了我们的模型，让我们在默认的数据库上下文中引用它。

为此，让我们打开`models\IdentityModels.cs`文件。找到名为`ApplicationDbContext`的类，并在 create 函数后添加以下内容:

在上面的代码块中，`DbSet`类表示用于读取、更新和删除操作的实体集。

这里，我们定义了两个实体，我们的`BlogPost`和`Comment`模型。我们现在可以从`ApplicationDbContext`的实例中访问它们。

### 连接到我们的数据库

尽管我们的模型已经建立，我们仍然需要将一个数据库附加到我们的应用程序。为此，选择 Visual Studio 左侧的服务器资源管理器，右键单击数据连接并添加一个数据库。
有各种各样的轻量级数据库，可以适合我们正在构建的应用程序，例如:

*   Microsoft access 数据库
*   Sqlite 数据库
*   ms sql server

对于本教程，我使用 MSSQL 服务器。

### 创建我们的控制器

现在我们的模型和数据库都设置好了，让我们继续创建我们的索引路径。打开`HomeController`并替换为:

在上面的代码块中，我们定义了六个不同的函数:

*   `Index`函数，它显示了我们所有博客文章的快速列表
*   `Create`函数，为`GET`和`POST`请求处理新 BlogPosts 的添加
*   `Details`函数，返回我们文章的完整视图
*   `Comments`函数，返回一篇特定文章的所有评论的 JSON 数据
*   `Comment`函数，处理新注释的添加，并将数据发送给 Pusher。

在查看我们的控制器函数之前，我们注意到有一行代码将我们的 DB 上下文导入到我们的类中:

这使得访问我们在`ApplicationDbContext`类中定义的数据库模型成为可能。

在`Index`函数中，我们返回我们的视图，传入我们数据库中所有帖子的列表，这将被循环。

接下来，在处理我们的`GET`请求的`Create`函数中，我们简单地返回创建新帖子的视图。

我们转到处理我们的`POST`请求的`Create`函数，该函数接收一个名为`post`的`BlogPost`类型的参数。在这个函数中，我们将一个新的`post`添加到数据库中，之后我们返回一个对`Index`函数的重定向。

在我们的`Details`函数中，我们将一个特定的`post`的实例返回给我们的视图，该视图将被显示。该视图还将显示允许我们添加评论的表单。

在我们的`Comments`函数中，我们返回属于特定`post`的所有`comments`，其 ID 作为 JSON 提供。这个方法将通过 AJAX POST 调用。

最后，我们的`Comment`函数处理将评论添加到数据库，并将数据发送到 Pusher。我们注意到这个函数是一个`async`方法。这是因为 Pusher 库异步发送数据，我们必须等待它的响应。

此外，我们需要将`XXX_APP_CLUSTER`、`XXX_APP_ID`、`XXX_APP_KEY`和`XXX_APP_SECRET`替换为我们之前从 Pusher 获得的应用集群、ID、密钥和秘密。

### 创建我们的视图文件

为了完成我们的应用程序，我们需要 3 个不同的视图文件，我们将在下面讨论。

#### **索引视图**

让我们将位于`Views\Home\Index.cshtml`的`Index.cshtml`文件中的默认内容替换为:

查看上面的 HTML 结构，我们注意到我们已经定义了一个表，该表列出了我们所有的帖子并将它们链接到详细信息页面。

#### **创建视图**

这里，我们需要在`View\Home`文件夹中创建一个名为`Create.cshtml`的新文件，并将以下内容粘贴到其中:

在上面的 HTML 结构中，我们有三个主要输入:

*   一个文本输入元素，保存文章的标题
*   一个文本输入元素，保存文章的内容
*   一个按钮元素，用于提交新条目

**详细视图和 Vue 绑定**

这是我们需要的最终视图文件。这个文件还处理 Pusher 事件的绑定，并使用 Pusher 和 Vue 实时更新评论。
让我们在`Views\Home`文件夹中创建一个名为`Details.cshtml`的新文件，并在其中添加以下内容:

[https://gist . github . com/755 c0e 5 e 8 CBF 53 dbb 9560 deafd 50 a 21](https://gist.github.com/755c0e5e8cbf53dbb9560deafdd50a21)

在上面的代码块中，我们显示了当前帖子的标题和内容，以及它的评论数量。

我们还创建了由三个主要元素组成的意见表，它们是:

*   评论者姓名的文本输入
*   注释正文的文本区
*   按钮将新注释保存到数据库中

注意，我们使用了 Vue 的`v-for`指令来迭代和显示可用的评论。

另外，请注意，我们已经包括了一些必需的库，例如:

*   axios JavaScript 库
*   Vue js JavaScript 库
*   Pusher JavaScript 库

#### **推动器绑定和 Vue 片段**

下面是我们的示例 Vue 片段，用于处理评论提交和 Pusher 的实时更新。

在上面的代码块中，我们完成了两个主要活动，它们是:

#### **上传评论代码**

为了处理从客户端到服务器的新评论，遵循以下步骤:

*   我们在提交按钮上附加了一个 Vue 事件监听器`@click`，它触发了一个名为`submit_comment`的方法
*   我们定义了一个名为`submit_comment`的函数，它使用`axios`向我们的`comment`函数发出 POST 请求

#### **从其他客户端订阅服务器上的订阅源添加内容**

在评论被发送到服务器之后，一个请求被发送到 Pusher，以返回一个包含我们已经广播的新数据的事件。要收听这些实时事件，我们需要:

*   在传递我们的应用程序密钥和集群时初始化了一个 Pusher 对象
*   订阅我们的频道`asp_channel`
*   在我们的 Vue 代码的 listen 方法中，我们声明了一个名为`asp_event`的事件绑定。在这个绑定的回调函数中，我们将新数据推送到我们的注释列表中

就是这样！现在，一旦有了新的评论，它也会被广播，我们可以使用我们的频道来实时更新评论。

![0*d-b_bCTehgAMknrK](img/ab2a4d24a619bee1dcb27581e3e28f3e.png)

### 结论

在本文中，我们介绍了如何使用？NET 和 Pusher，并在. NET 中创建一个迷你博客引擎。

本教程的代码库可以在[公共 Github 库](https://github.com/samuelayo/Net_real_time_commenting_pusher)中找到。可以下载用于教育目的。
有任何保留或意见，请在评论中告诉我们您的反馈。

这篇文章最初由作者发表在 Pusher 的博客[这里](https://blog.pusher.com/build-a-realtime-commenting-feature-using-net-and-pusher/)