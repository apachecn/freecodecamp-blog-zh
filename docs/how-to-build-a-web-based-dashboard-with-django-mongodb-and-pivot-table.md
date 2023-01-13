# 如何用 Django、MongoDB 和 Pivot Table 构建基于 web 的仪表板

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-web-based-dashboard-with-django-mongodb-and-pivot-table/>

嗨，自由代码营社区！

在本教程中，我想与您分享一种用 Python 实现数据可视化的方法，您可以在 Django 开发中进一步应用这种方法。

如果你曾经遇到过建立一个**交互式仪表板**的必要性，或者你想尝试这样做，欢迎你来完成本教程的步骤。

如果你对这个过程有任何问题，请在评论中提问。我很乐意帮助你。

完成教程后，您将掌握以下技能:

*   如何创建一个基本的 Django 应用程序
*   如何在 **MongoDB 图集**中托管远程 **MongoDB** 数据
*   如何将 **JSON** 和 **CSV** 数据导入 **MongoDB**
*   如何将**报告工具**添加到 Django 应用程序中

开始吧！？？‍?？？‍?

## 先决条件

*   网页开发的基础知识
*   关于 **Python** 的自信知识
*   拥有使用 **NoSQL** 数据库的基本经验(例如，MongoDB)

## 工具

*   ******Django-****一个高级 Python web 框架。**
*   ******MongoDB 图册-****** 一个面向现代应用的云数据库服务。在这里，我们将托管我们的 MongoDB 数据库。
*   ******Flexmonster 透视表&图表******——一个用于报表的 JavaScript web 组件。它将在客户端处理数据可视化任务。
*   ******flex monster******的 MongoDB 连接器——数据透视表之间快速通信的服务器端工具& MongoDB。
*   PyCharm 社区版-Python 开发的 IDE。
*   **Kaggle** 数据

## 建立 Django 项目

如果你是 Django development 的新手，没关系。我们将一步一步地设置一切，使我们的应用程序出类拔萃。

*   确保你之前已经在你的机器上安装了 Django。
*   首先，打开要创建项目的目录。打开控制台并运行以下命令来创建一个新的闪亮 Django 项目:

*`django-admin startproject django_reporting_project`*

*   接下来，导航到该项目:

*T2`cd django_reporting_project`*

*   让我们检查一下是否一切正常。运行 Django 服务器:

*T2`python manage.py runserver`*

除非另有说明，开发服务器从端口 **8000** 开始。在浏览器中打开 [`http://127.0.0.1:8000/`](http://127.0.0.1:8000/) 。如果你能看到这个很酷的火箭，我们就在正确的轨道上！

![_lqtoZcHe7ESb0dKXfqmjHGvWG8pW7Bek734ML7YnIafTKncYYaRdPrZbO-Kwef8U4WcRzAYJett_QV5QWrAwND2JbxJl4x6c-HREBAvoMwvvpctdhwGHq6otm63nD8-cuN5EzE_](img/c60d16bfd0c333aed17d2224191fac25.png)

## 创建应用程序

现在是时候创建具有报告功能的应用程序了。

> 如果你对 Django 中的项目和应用之间的区别没有信心，这里有一个[快速参考](https://learndjango.com/tutorials/django-best-practices-projects-vs-apps)来帮助你弄清楚。

*   姑且称之为 *`dashboard`* :

*T2`python manage.py startapp dashboard`*

*   接下来，在您最喜欢的 IDE 中打开项目。我强烈推荐使用 **PyCharm** ,因为它让用 Python 编程的整个过程变得非常愉快。它还可以方便地管理特定于项目的隔离虚拟环境的创建。
*   创建应用程序后，需要在项目级别注册它。打开 *`django_reporting_project/settings.py`* 文件，将应用的名称追加到 *`INSTALLED_APPS`* 列表的末尾:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'dashboard',
]
```

万岁！现在，项目知道了您的应用程序的存在，我们准备好继续进行数据库配置。

## 使用 MongoDB Atlas 建立 MongoDB 数据库

让我们把应用程序放在一边，直到我们完成了数据库的整理。我建议我们通过将远程 MongoDB 数据库托管在 MongoDB Atlas 上来练习创建远程 MongoDB 数据库——这是一种面向应用程序的云数据库服务。或者，您可以准备一个本地数据库，并以任何方便的方式使用它(例如，通过 [MongoDB Compass](https://www.mongodb.com/products/compass) 或 [mongo shell](https://docs.mongodb.com/manual/mongo/) )。

*   登录 MongoDB 帐户后，创建我们的第一个项目。姑且称之为 *`ECommerceData`* :

![fySeUI8EiBjYHRWLtt9LNNEsAilie8bDcEhpOEYWca-t1t4f44CYdx9XsABCoRQogCHw1MmsX61f4w3PW78j-XQ7eYNM3TOkzAfVPhQN8N-3zb1DwgHsqq8ihaEpk7zHuo39rN93](img/7080608dc99fff3bc658fcc8da597fb2.png)

*   接下来，添加成员(如果需要)并设置权限。您可以通过电子邮件地址邀请用户参与您的项目。
*   创建集群:

![I3Z6F1uTTSJsGlylgR-Fe0Jvk6AuljINzT7DDBXiMWQJJcQ93PPJJi0xQqn3ZqqwivEHvaKN1XA5DjaM9nUJOAoFmUcGfJE0UBSgaOS1_UT5el94yqto_oGadfuEWHS7HLVT4PAN](img/638f425da54c7c981de6449192988b60.png)

*   选择计划。因为我们在学习的道路上，最简单的免费计划将足以满足我们的需求。
*   选择云提供商和地区。推荐区域是通过您的位置推断出来的，并用星星标记。
*   给我们全新的集群起一个有意义的名字。注意，以后不能改了。姑且称之为 *`ReportingData`* :

![b3Lic8LQJcVHqB2pVGUMzyca817PTI3HSGAg_RvxEE8w12-qZjG6ZNIQxy7fCTlaMBPARyOOAZ1k1iKMgu3ZL4GNhnDY3Ut9jYDJx5cl83Gpcf6qLEGGZ_TvloST6K6cZ9vRj17Z](img/1097a768da2e368d585f05fb5a2a05bd.png)

## 准备数据

在您等待集群创建的同时，让我们仔细看看我们将使用的数据。在本教程中，我们将使用[ka ggle 数据集](https://www.kaggle.com/carrie1/ecommerce-data)处理一家英国零售商的交易。使用这些数据，我们将尝试构建一个有意义的报告，该报告可以为真实组织中的*探索性数据分析*服务。

此外，我们将使用关于营销的 mock **JSON** 数据。它将帮助我们实现在同一应用程序中建立不同报告工具的目标。你可以选择你喜欢的任何数据。

## 连接到您的群集

现在我们的集群已经准备好了，让我们**将**连接到它！

*   将您当前的 IP 地址列入白名单或添加一个不同的地址。
*   创建一个 MongoDB 用户。第一个将拥有当前项目的 *atlasAdmin* 权限，这意味着拥有[以下角色和特权动作](https://docs.atlas.mongodb.com/security-add-mongodb-users/#Atlas-admin)。出于安全原因，建议自动生成一个强密码。
*   选择一种最适合自己的**连接方式**。为了测试连接，我们可以首先使用 mongo shell 的连接字符串。稍后，我们还将为应用程序使用一个连接字符串。

![SrmLY8KbbhYdm8ljnv_MlY7oCQI04PCeVVt3IcgbvoOdeg3D02UvDbiFLD53J-8sE09-j2wWx57WlCbSBy4Igv1HIdmpNkkd4RFaOEjhDxE5dhqoVwiAL-hZB76GhLyZSRYo7ETE](img/d25bacd7e80e102721e5eab02d4da274.png)

*   通过 mongo shell 连接到它。打开命令行并运行以下命令:

*T2`mongo "mongodb+srv://reportingdata-n8b3j.mongodb.net/test"  --username yourUserName`*

交互式提示将要求您输入密码进行身份验证。

## 检查集群指标

唷！我们快到了。

现在回到包含集群摘要的页面，看看它是如何实现的！从现在起，我们可以深入了解 MongoDB 数据库的读写操作、活动连接的数量、副本集的逻辑大小——所有这些统计信息都在您的掌握之中。但最重要的是，现在有可能*创建和管理数据库和收藏。*

## 创建数据库

创建您的第一个数据库和两个集合。我们把它们分别命名为*电子商务、* *交易、*和*营销*。

这是我们的工作区现在的样子:

![image-235](img/0d915041309ceefcdfad41456b7a2fe1.png)

看起来很空，不是吗？

## 将数据导入 MongoDB

*让我们用数据*填充集合。我们将从先前从 [**Kaggle**](https://www.kaggle.com/carrie1/ecommerce-data) 下载的零售数据开始。

*   解压缩归档文件，并导航到存储其内容的目录。
*   接下来，打开命令提示符，使用 *`mongoimport`* 命令和 mongo shell 的给定连接字符串，将数据导入到 ***电子商务*** 数据库的 ***事务*** 集合中:

*T2`mongoimport --uri "mongodb+srv://username:password@reportingdata-n8b3j.mongodb.net/ecommerce?retryWrites=true&w=majority" --collection transactions --drop --type csv --headerline --file data.csv`*

❗Please 记得把*用户名*和*密码*关键字换成**你的证件**。

* * *

恭喜你。*我们刚刚下载了 541909 份文档到我们的收藏中*。**下一步是什么？**

*   将带有营销指标的数据集上传到 ***营销*** 集合。这里是我们将要使用的带有样本数据的 [**JSON** 文件](https://gist.github.com/veronikaro/b631874a1681b89506ba5b9880889e8e)。

使用以下命令将 JSON 数组导入集合:

*T2`mongoimport --uri "mongodb+srv://username:password@reportingdata-n8b3j.mongodb.net/ecommerce?retryWrites=true&w=majority" --collection marketing --drop --jsonArray marketing_data.json`*

如果这些数据还不够，我们可以使用 **mongoengine / PyMongo** 模型动态生成更多数据。这就是我们这个系列的下一篇教程将致力于的。但是现在，我们将跳过这一部分，使用我们已经拥有的数据。

* * *

现在我们的集合包含了数据，我们可以研究每个集合中的文档数量以及它们的结构。为了获得更多的见解，我推荐使用 **MongoDB Compass** ，它是 MongoDB 的官方 GUI 工具。有了它，您可以探索每个集合的结构，检查字段类型的分布，构建聚合管道，运行查询，评估和优化它们的性能。首先，[下载应用程序](https://www.mongodb.com/download-center/compass)，并使用 MongoDB Atlas 提供的 Compass 连接字符串。

## 将 URL 模式映射到视图

让我们回到姜戈。

*   在 app 的文件夹里创建 *`urls.py`* (在 *`dashboard`* 里面)。在这里，我们将为我们的应用程序存储 URL 路由。这些 URL 模式将与 *`dashboard/views.py` :* 中定义的视图相匹配

```
from django.urls import path
from . import views

urlpatterns = [
    path('report/retail', views.ecommerce_report_page, name='retail_report'),
    path('report/marketing', views.marketing_report_page, name='marketing_report'),
] 
```

*   应用程序的 URL 需要在项目级别注册。打开 *`django-reporting-project/urls.py`* ，用以下代码替换内容:

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('dashboard.urls')),
]
```

## 创建视图

视图只是一个接受 web 请求并返回 web 响应的函数。响应可以是任何类型的。使用 [render()](https://docs.djangoproject.com/en/3.0/topics/http/shortcuts/#render) 函数，我们将返回一个 HTML 模板和一个组合成单个 [HttpResponse](https://docs.djangoproject.com/en/3.0/ref/request-response/) 对象的上下文。注意 Django 中的视图也可以是基于[类的](https://docs.djangoproject.com/en/3.0/topics/class-based-views/)。

*   在 *`dashboard/views.py`* 中，让我们为我们的报告创建两个简单的视图:

```
from django.shortcuts import render

def ecommerce_report_page(request):
   return render(request, 'retail_report.html', {})

def marketing_report_page(request):
   return render(request, 'marketing_report.html', {}) 
```

## 创建模板

*   首先，在你的应用目录下创建 *`templates`* 文件夹。Django 会在这里搜索你的 HTML 页面。

*   接下来，让我们设计应用程序的布局。我建议我们添加一个导航栏，它将显示在每一页上。为此，我们将创建一个名为 *`base.html`* 的基本模板，所有其他页面都将根据业务逻辑对其进行扩展。这样我们将利用**模板继承和**e——Django 模板引擎的强大部分。请在 [GitHub](https://gist.github.com/veronikaro/9bc763f5717f86872420ab30357e4573) 上找到 HTML 代码。

你可能已经注意到了，我们将使用自举风格。这是为了用现成的 UI 组件美化我们的页面。

请注意，在导航栏中，我们添加了两个重定向到报告页面的链接。可以通过将链接的 *`href`* 属性设置为 URL 模式的名称来实现，该名称由 URL 模式中的*名称*关键字指定。例如，以下面的方式:

*`href="{% url 'marketing_report' %}"`*

*   是时候创建报告所在的页面了。首先让我向您展示如何创建零售报告。遵循这些原则，您可以根据需要创建任意多的其他报告页面。

1.  在模板中，创建 *`marketing_report.html`* 。
2.  添加一个**扩展**标签来继承基本模板:`{% extends "base.html" %}`
3.  添加一个**块**标签来定义我们子模板的*内容* : `{% block content %}
    {% endblock %}`
4.  在块中，添加 Flexmonster 脚本和容器，其中将放置报告组件(即数据透视表和数据透视图):`<script src="https://cdn.flexmonster.com/flexmonster.js"></script> <div id="pivot"></div>
    <div id="pivot_chart"></div>`
5.  在 JavaScript 代码将要执行的地方添加`<script>`标签。在这些标签中，使用 [init API 调用](https://www.flexmonster.com/api/new-flexmonster/)实例化两个 Flexmonster 对象。

```
var pivot = new Flexmonster({
    container: "#pivot",
    componentFolder: "https://cdn.flexmonster.com/",
    height: 600,
    toolbar: true,
    report: {}
});
var pivot_charts = new Flexmonster({
    container: "#pivot_charts",
    componentFolder: "https://cdn.flexmonster.com/",
    height: 600,
    toolbar: true,
    report: {}
});
```

您可以放置任意数量的 Flexmonster 组件。稍后，我们将用数据填充这些组件，并编写定制报告。

## 设置 MongoDB 连接器

为了在 Flexmonster 数据透视表和 MongoDB 数据库之间建立高效的通信，我们可以使用 Flexmonster 提供的 [MongoDB 连接器](https://www.flexmonster.com/doc/how-to-connect-to-mongodb/?r=fr6)。这是一个服务器端工具，它为我们做了所有艰苦的工作，即:

1.  连接到 MongoDB 数据库
2.  获取集合的结构
3.  每次报告结构改变时查询数据
4.  发回聚合数据以在数据透视表中显示。

要运行它，让我们从 GitHub 克隆[这个示例，导航到它的目录，并通过运行 *npm install* 来安装 npm 包。](https://github.com/flexmonster/pivot-mongo)

*   在 *`src/server.ts`* 中，您可以检查连接器将在哪个端口上运行。您可以更改默认设置。在这里，您还可以指定哪个模块将处理到达端点的请求(在我们的例子中是`mongo.ts`)。
*   之后，在 *`src/controller/mongo.ts`* 中指定数据库凭证。在这里，为 MongoDB Atlas 提供的应用程序添加连接器字符串，并指定数据库的名称。

## 定义报告

现在，我们准备在客户端定义报告的配置。

*   下面是通过连接器使数据透视表与 MongoDB 数据一起工作的最小配置:

```
var pivot = new Flexmonster({
    container: "#pivot",
    componentFolder: "https://cdn.flexmonster.com/",
    height: 600,
    toolbar: true,
    report: {
        "dataSource": {
            "type": "api",
            "url": "http://localhost:9204/mongo", // the url where our connector is running
            "index": "marketing" // specify the collection’s name
        },
        "slice": {}
    }
});
```

*   指定一个**切片**——将在网格或图表上显示的一组层次结构。这是透视网格的示例配置。

```
"slice": {
        "rows": [
            {
                "uniqueName": "Country"
            }
        ],
        "columns": [
            {
                "uniqueName": "[Measures]"
            }
        ],
        "measures": [
            {
                "uniqueName": "Leads",
                "aggregation": "sum"
            },
            {
                "uniqueName": "Opportunities",
                "aggregation": "sum"
            }
        ]
    }
```

## 运行您的报告应用程序

现在我们已经配置了客户端，让我们导航到 MongoDB 连接器的目录并运行服务器:

*T2`npm run build`*

*T2`npm run start`*

*   接下来，返回到 PyCharm 项目并运行 Django 服务器:
    *`python manage.py runserver`*
*   打开 [`http://127.0.0.1:8000/report/marketing`](http://127.0.0.1:8000/report/) 。要切换到另一个报告，请单击导航栏上的报告名称。

是时候评价结果了！在这里你可以看到营销部门的报告:

![freeCodeCampMongo](img/a49a174e855036ba6e44ad5c7b39de2e.png)

尝试使用布局:

*   **切片** & **掷骰子**数据得到你独特的视角。
*   更改**汇总**功能，**过滤** & **排序**记录。
*   在**经典**和**紧凑**形式之间切换，了解哪种感觉更好。

## 享受 Python 中的分析仪表板

恭喜你！出色的工作。我们让我们的数据活了起来。现在，您有了一个强大的 **Django 应用程序**启用了**的报告**和**数据可视化**功能。

您的最终用户可能会觉得非常舒服的一件事是，可以配置一个报表，保存它，然后通过将它上传到数据透视表中来继续您的工作。报告是整洁的 JSON 文件，可以存储在本地或服务器上。此外，还可以将报告导出为 **PDF** 、 **HTML** 、**图像**或 **Excel** 文件。

请根据您的业务需求随意定制应用程序！您可以添加更复杂的逻辑，更改*数据源*(例如， *MySQL* 、 *PostgreSQL* 、 *Oracle* 、*Microsoft Analysis Services*、 *Elasticsearch* 等)，以及定制数据透视表和数据透视表图表的外观和/或功能。

## 进一步阅读

*   [**github 上的完整代码**](https://github.com/veronikaro/analytics-django-mongodb) ⭐
*   关于如何开始使用 MongoDB Atlas 的全面指南
*   【Flexmonster 数据透视表入门&图表
*   [Django 入门](https://docs.djangoproject.com/en/3.0/intro/)
*   [MongoDB 连接器简介](https://www.flexmonster.com/doc/mongodb-connector/?r=fr6)
*   [MongoDB 连接器 API](https://www.flexmonster.com/api/all-methods/?r=fr6)
*   [如何更改报表主题](https://www.flexmonster.com/demos/themes/?r=fr6)
*   [如何本地化透视表组件](https://www.flexmonster.com/demos/localization/?r=fr6)

## 美化报告的额外设置

这里有一个为好奇者准备的附加部分！

为了美化层次结构的标题并定义字段类型，我们将添加 **[映射](https://www.flexmonster.com/doc/mapping/?r=fr6)**——报表数据源配置中的一个特殊对象。**映射**通过设置标题帮助我们定义如何显示字段名称。另外，可以显式定义字段类型(数字、字符串、不同类型的日期)。每一项配置都取决于您的业务逻辑。

一般来说，映射在数据源和它的表示之间创建了一个额外的抽象层次。

下面是一个如何为零售数据集定义它的示例:

```
"mapping": {
    "InvoiceNo": {
        "caption": "Invoice Number",
        "type": "string"
    },
    "StockCode": {
        "caption": "Stock Code",
        "type": "string"
    },
    "Description": {
        "type": "string"
    },
    "Quantity": {
        "type": "number"
    },
    "InvoiceDate": {
        "type": "string",
        "caption": "Invoice Date"
    },
    "UnitPrice": {
        "type": "number",
        "caption": "Unit Price"
    },
    "CustomerID": {
        "type": "string",
        "caption": "Customer ID"
    },
    "Country": {
        "type": "string"
    }
}
```