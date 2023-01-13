# 如何免费托管轻量级应用

> 原文：<https://www.freecodecamp.org/news/how-to-host-lightweight-apps-for-free-a29773e5f39e/>

阿巴亚西里酒吧

# 如何免费托管轻量级应用

![1*m8zEyzBZkPlhZlNZ-w1gaw](img/d0b628da26f3a9ec9d87343d621a73c8.png)

Courtesy of [Luis Llerena](https://unsplash.com/@albertosaure)

当你寻找虚拟主机服务时，有许多免费的选择。但是没有多少地方可以托管涉及 API、CGI 或 AJAX 后端查询的全栈 web 应用程序——特别是如果您想使用 PHP 之外的东西。

这篇文章是一个简单但有意义的“运行之前先走一步”指南，指导你如何开始在云服务器上托管你的脚本。

### **何时使用云应用平台**

云应用程序平台在需要一点代码在服务器上运行的场景中工作得很好。这些平台中的许多都提供了一系列基于 Linux 的应用程序容器(看起来像虚拟机)，您可以在本地计算机上使用一组命令行关键字部署您开发的代码。

Heroku 就是这样一个服务，你可以用它来相对容易地托管你的代码(各种语言)。它提供了一个免费增值模式，让你免费使用大约 500 小时的计算时间(他们的全价是[这里是](https://www.heroku.com/pricing))。

![1*5r9A_yT2n4zTeBVXAJRD_w](img/0581e4582194f70722e7bc5639ee9dff.png)

As of May 2017, the programming languages you can host under Heroku

一旦在本地桌面上编写了代码，就可以执行命令将代码部署到 Heroku 中的工作区。然后，代码根据触发器执行。触发器可以是一个计划好的作业，一个通过网页请求触发的 web 服务器，或者一些持续运行并处理数据的东西——这可能会非常昂贵。

真正的好处是，您不必担心操作系统(内存、存储、CPU、安全补丁),因为这些都是为您管理的——但同时这意味着您的灵活性有限，因为您不能直接分配资源。

Heroku 可以很好地为您工作的一些具体例子:

*   托管你自己的网站，你想写你自己的网络服务器
*   定期从网站上抓取数据，然后存储在数据库中进行分析
*   为特定任务提供 API 服务器。比如提供天气数据，存储物联网传感器数据，或者对机器学习模型的 web 服务调用
*   数据库服务(尽管 Firebase 之类的服务可能更适合)

### **Heroku 建筑**

Heroku 为您提供了一个轻量级虚拟机(VM)来部署您的代码。请注意，在免费选项下，您最多可以部署 5 个被视为 5 个轻量级虚拟机的应用程序。对于您的实际应用，Heroku 下为您提供了一个单独的 URL 子域。因此，您的项目名称需要是唯一的。

这些工作区有自己的组件空间，例如:代码和资源文件(不是动态数据文件)、数据库(Postgres)和日志文件。

在您的本地桌面上，Heroku 使用您的目录名来定义您的项目，并且让 Heroku 了解您的上下文。所以你可以在不同的目录下有多个项目，当你运行 Heroku 命令时——只要确保你在正确的文件夹下运行。

您需要知道的一件关键事情是(这是我经过几个小时的调试才发现的——我希望我更多地关注文档),一切都是从内存中运行的。没有持久存储。我再说一遍—你不能在文件服务器上存储任何文件！对于持久性，Heroku 提供了一个 postgress SQL 数据库，您可以根据需要添加记录。

### **一个简单的例子——网站变更检测**

这里有一个逐步的指南，让你得到一个简单服务的工作示例，如果一个网站已经改变，它会给你发电子邮件——基本上是 www.changedetection.com 的克隆。这将包括几个关键部分:

1.  一个数据库将存储:(a)电子邮件地址，以通知一个改变的网站；(b)要跟踪的网站；(c)网站的最后“副本”
2.  一段代码将从#1 (Python 脚本)中的数据库检查给定的网站
3.  将在#2 中运行程序的作业调度程序(相当于 cron 作业)
4.  一个网络用户界面，您可以在其中添加/删除网站，以监控#1 中提到的数据库
5.  发送电子邮件的机制

理解这些组件将使你具备做很多事情的技能。我通过多种渠道了解了所有这些事情，所以这是一篇综合文章。

### **假设**

以下指南做出了以下假设:

*   您有一个 GitHub 帐户——如果没有，请在这里创建一个[。你也应该读一下这个简单的](https://github.com/)[指南](http://readwrite.com/2013/09/30/understanding-github-a-journey-for-beginners-part-1/)。
*   您已经有一个 Heroku 帐户，如果您没有，请在此处创建一个。
*   您运行的是 Windows 机器——如果不是，那也没关系，在其他环境中的说明非常相似
*   您已经安装了 Python 如果您没有，请转到[这里](https://www.continuum.io/downloads)安装它
*   你已经可以用 Python 编程了——如果没有，那么我建议你先学习一些基础知识。有些导游是[这里](https://medium.mybridge.co/19-free-ebooks-to-learn-programming-with-python-8f6f0ad4a7f8)。
*   你知道 SQL，如果你不知道，请点击[这里](https://www.w3schools.com/sql/DEfaULT.asP)。

### **步骤概述**

我发现经历“先走后跑”的方法有助于学习过程。它也为你如何解决更大过程的每一部分提供了文档。这样，如果你未来的努力中出现了问题，你就有更好的机会找出问题出在哪里。

**第一步**:开发 web 用户界面——首先构建 Hello World

第二步:持久化——创建一个数据库

**第三步**:检查网站变化

**第四步:**发送电子邮件通知

**第五步**:在网页上列出输出

**第六步:部署**

### **第一步:开发网络用户界面——首先构建 Hello World】**

首先，让我们在 Heroku 上部署一个简单的程序来开始。该程序将是上述组件列表中 web 用户界面(第 4 项)的前身。为了提供一个页面，我们只需要一个 HTML 页面，但是我们需要一个 web 服务器来提供这个文件。换句话说，当你输入网站的 URL 时，程序需要解释请求，然后提供 HTML 文件的内容。你可以用 Flask Python 库创建你自己的迷你 web 服务器，这就是我们要做的。

*   创建一个名为 webchecker 的文件夹，并进入该目录(该目录名不必与 Heroku 应用程序名相同)
*   安装烧瓶库。输入命令:npm Flask
*   创建以下 Python 程序，并将其命名为 showchecks.py:

在部署到 Heroku 之前，测试它在本地 PC 上的工作情况。您可以通过以下步骤进行测试:

*   运行程序:python webchecker.com
*   在本地电脑上打开浏览器，打开页面:[http://localhost:5000/hello](http://localhost:5000/hello)

![1*GXR1TOFAk6wIUCm_mVTWLQ](img/9e038da244183f8d4e8a4b857309ff76.png)

Running the script will return a static page with ‘hello world’ output

接下来，让我们将它部署到 Heroku。在部署之前，还需要包含一些文件来帮助 Heroku 更好地了解您的应用程序。

一、requirements.txt

第二个文件告诉 Heroku 当发出一个 webrequest 时应该运行什么:

最后，要使用的 Python 的运行时版本(默认为 2.7，但我们希望指定 Python 的最新版本):

因此，您应该有四个文件:

1.  showchecker.py 哪个是代码
2.  requirements.txt 获取非标准库依赖项列表。每当您有不属于 Python 标准库的新库时(即，您需要使用诸如“pip”之类的工具来安装它们)，请在此处添加这些库。您可以通过在命令行中运行命令:`pip show Flask`来查找已安装的库(如 Flask)的版本
3.  Procfile 是调用网站时运行的实际 Python 脚本-如果更改 Python 文件，请确保更新该文件
4.  这是要使用的 python 的实际版本

您可以从命令行通过以下步骤进行部署:

1.  heroku 创建 web checker 01-build pack heroku/python
2.  去把它给我。* *
3.  git status
4.  git commit -m "所有文件"
5.  饭桶推 heroku 主人

![1*nLcnlwU1UocIvqbiHhviXQ](img/a4b62de765fa235b9c47a4032376df68.png)

Once you run the “git push heroku master”, the deployment logs will be shown including the url where it is deployed

对于命令# 1(`heroku create…`),“webe checker 01”部分是您需要为应用程序名称提供的唯一名称。

对于命令#3 ( `git status`)，这将告诉您什么文件准备好被部署。确保所有文件都在那里，如果没有的话，使用`git add <filena` me >添加它们。

现在你可以查看你的网站: <application name="">.herokuapp.com/hello</application>

![1*tSMmTHoxgA2-xXQ0RaTrHg](img/d0206c3ca140ec3254b86a00766a3345.png)

The hello world program running on the web

我们还要确保可以看到日志，因为这是查看应用服务器运行情况的好方法。在您的 PC 和 webchecker 目录中，运行命令:`heroku logs`

![1*GQkthW50XPatw-J7fnrDlA](img/940f782ff03247ca3f613f2180acaf71.png)

“heroku logs” is a crucial command to run to see what’s going on within your app server

您将看到最后的执行步骤。如果事情不像预期的那样运行，这是你寻找更多细节的第一站。

您也可以访问 Heroku 仪表盘查看您的消费情况:

[https://dashboard.heroku.com](https://dashboard.heroku.com)

### **步骤 2:持久性——创建数据库**

为了制作更有用的程序，你需要有一个各种各样的数据存储。这就是 Postgres 数据库服务发挥作用的地方。您首先需要部署 Heroku 数据库服务，然后创建您的表，最后能够从您的代码本地连接到数据库(用于测试)。

要部署数据库服务，首先使用以下命令创建它:

`heroku addons:create heroku-postgresql:hobby-dev`

接下来，从命令行访问数据库并创建您的表。数据库是在 Heroku 云服务上创建的，而不是在本地。但是，您可以通过命令行访问它。要通过控制台登录数据库，运行命令`heroku pg:psql`。请记住，您必须在您的 webchecker 文件夹中执行此操作，以便 Heroku 知道它是 webchecker 站点的数据库。

要查看表格列表，请键入命令`\d`

![1*sctwYakzP69ePN-W50Rezw](img/9aba890a4784c52b7024fbd23d3093ad.png)

要创建一个表，您需要使用普通的 SQL 语句。对于我们的 webchecker 程序，让我们创建一个包含以下各列的表格:

*   ID —为每个条目自动生成的 ID(这将是主键)。这是通过使用“串行”类型来完成的
*   网站—要监控的网站
*   电子邮件地址—发送变更通知的电子邮件地址
*   last hashcode——我们不会存储整个网页的副本，而是根据页面的 HTML 生成一个哈希，然后每次都进行比较。这在存储方面效率更高，但不会告诉我们实际发生了什么变化
*   lastchangedate —网站上次更改的日期。因此，我们将让数据库默认为当前日期

要创建该表，请在 Heroku Postgres 数据库控制台中输入以下命令:

```
CREATE TABLE webcheckerdb (id serial, website varchar(250), emailaddress varchar(250), lasthashcode varchar(32), lastchangedate timestamp DEFAULT current_date );
```

(确保在末尾包含分号！)

![1*DJFH7kef_tDJhIse7sazWA](img/47d2eb094e65b64689bf1b2008f89b3c.png)

Creating the database, then using \d switch to see the list of tables and then “\d webecheckerdb” to see the columns

接下来，让我们在数据库中插入一条记录，以确保在启动和运行我们的 web UI 之前有一些东西可以使用(您可以使用自己的电子邮件地址，以便将来可以使用):

```
INSERT into webcheckerdb values(DEFAULT, 'news.google.com', 'email@me.com', '', DEFAULT);
```

(确保在末尾包含分号！)

![1*lzafM9Y_USmlAaY-FPIjGw](img/f8dfbfd46f1439f9e5d0f42ad292fd06.png)

You can then do a select statement (include the semicolon at the end!) to see the data

可以用`\q`退出。

### **第三步**:检查网站变化

首先，让我们获取一段代码，至少检查是否可以检索硬编码的站点(遵循先走后跑的概念)。

因此，第一步是看看我们是否可以检索一个网页，散列它，然后将其与硬编码的散列进行比较。创建一个名为 checkwebsite.py 的新 Python 文件。代码如下:

运行此命令将输出以下内容:

![1*mnR-Yfax_dWTE3GeoffaaQ](img/086e3a45b9b93efc02d6cd2dfda2b8df.png)

如果您有任何关于缺失库的错误，您可以从命令行通过:`pip install <libra` ry >添加它们。

接下来，让我们使用以下代码连接到数据库:

当您试图运行这段代码时，您很可能会得到一个形式为`KeyError: ‘DATABASE_URL’`的错误。这是因为您的 Python 代码试图定位 Heroku 上托管的 Postgres 数据库的网址。这将自动更新到 Heroku 服务器中的环境变量 DATABASE_URL。但是，在您的本地 PC 上，您必须手动执行此操作:

1.  heroku 配置
2.  set DATABASE_URL=<the database="" string="" listed="" from="" config=""></the>

![1*Q4i73mFPaKPuDobdX9G_ZA](img/55ec62e459770a85a0ef24b8cf86684e.png)

If you get the DATABASE_URL error, then set the environment variable

### 步骤 4:发送电子邮件通知更改

最后一步是发送电子邮件。要做到这一点，你需要安装一个能够发送电子邮件的插件——你可以通过 Heroku marketplace 找到这些插件:[https://elements.heroku.com/addons](https://elements.heroku.com/addons)

在这里，有一个插件叫做 send grid:[https://elements.heroku.com/addons/sendgrid](https://elements.heroku.com/addons/sendgrid)

您可以通过在命令行中键入以下命令将 SendGrid 添加到您的应用程序中:

`heroku addons:create sendgrid:starter`

当你进入你的仪表板时，你可以在资源部分看到新的插件:

![1*c15ERSHkLS2tpByYFEJM1g](img/773b3c2e45a794d7dcd686bc5b09c5b8.png)

SendGrid addon that can enable sending emails will be at the bottom

在使用它之前，您需要创建一个 API 密钥。双击上面的 SendGrid 组件，进入设置->API 密钥->创建密钥(右上角的蓝色按钮)。

![1*DENX5_vY3u5ibFsMNW0Dug](img/bf3ca8c68a508d360c1c174e910d7e6b.png)

Click on the Create Key button at top right

创建密钥后，复制它并返回到命令提示符并输入:

【T0 和】T1

这将仅在服务器上注册它，您需要使用以下命令将其本地添加到您的桌面:

>中的`set SENDGRID_API_KEY=<API Key from above aga`

完成后，您可以在一个名为 sendmail.py 的新 Python 脚本中测试您的代码。

要确认电子邮件已发送并送达，您可以返回 SendGrid 仪表板并查看统计信息概览屏幕:

![1*i3v-OiFWlQhSEY_1y2VNoQ](img/b508ed12bf6e5d179b68acc3b4889f21.png)

当检查你的电子邮件时，记得检查你的垃圾邮件。

一旦它开始工作，您只需要将两行代码添加到您的主 checkwebsite.py 脚本中。它是:

```
import sendmail #import the send email subroutine you wrote above
```

```
...
```

```
#call the subroutine after find the hashcode has changedsendmail.sendemail(webrecord['emailaddress'], 'Website changed', webrecord['website'] + ' changed')
```

完整的代码在这里:

### **步骤 5** :在网页上列出输出，安排作业

下一步是在网页上列出输出。

这包括查询数据库，然后在屏幕上循环显示数据。因此，它采用上面的“Hello World”代码，并进行修改。我还为此创建了一个不同的路径，所以要测试它，您需要转到 URL: http://localhost:5000/list

这是输出结果:

![1*Cr3hYo0DKRwBiXhfgVLE0g](img/c3351d7a8e1ac50b1e144200d3a663b6.png)

### 第六步:部署

最后一步是将所有东西都部署到 Heroku，然后调度作业，让它检查电子邮件。

您应该有以下文件:

1.  Procfile —指向 showchecker.py 的文件
2.  requirements.txt —包含库依赖项的文件
3.  runtime . txt—python 的版本
4.  showchecker.py —通过 <your appname="">.herokuapp.com/list 在 web 上显示数据库输出的 python 代码</your>
5.  checkwebsite.py 检查网站上的任何更改的 python 代码

对于 requirements.txt，您需要进行修改以添加最新的库:

将这些全部部署到 Heroku:

1.  去把它给我。* *
2.  git commit -m“部署”
3.  饭桶推 heroku 主人

测试每个组件:

1.  前往 <your app="" name="">.herokuapp.com/hello</your>
2.  前往 <your app="" name="">.herokuapp.com/list</your>

如果有任何错误，那么在命令行中运行`heroku logs`看看发生了什么。

接下来，直接在 Heroku 上运行 checkwebsite.py，确保没有问题。为此，您可以键入:

`heroku run python checkwebsite.py`

![1*eTeK6EvsZjWvMw-IYnDGuA](img/ce10fecf3b15c578b4d1735aa71a925b.png)

This is a great way to ensure your code is running as well on the heroku cloud as locally

最后，您现在可以安排您的工作。同样，你需要包括一个插件来做到这一点。

`heroku addons:create scheduler:standard`

![1*6O4ealFi6wtJ2OuE-wXa8w](img/0cd61195902bde3682cd9871b65893a4.png)

您应该能够在您的资源页面中看到调度程序:

![1*RG3zOR_MfVjM2fjHStyZmg](img/7f956fd108cef86aad428a4bec70328b.png)

The scheduler addon has been included as well which you can double click

您可以简单地使用命令行来运行程序，在我们的例子中是:`python checkwebsite.py`(这与我们上面用`heroku run`命令测试的一样)。

![1*zfwYHHgSwOl4WHIcY1DJTw](img/6473c466be03faf5a14f513e4d486fa4.png)

You can schedule this with a few options.

### 摘要

就是这样…第一次有点复杂，但希望上面的渐进步骤能帮助你理解引擎盖下发生了什么。Heroku 上有更多的资源，还有大量关于 T2 堆栈溢出的信息。在经历了以上这些之后，这些资源应该会变得更有意义。

祝你好运！

*感谢阅读！如果你喜欢你所读的，点击下面的 the❤按钮，这样其他人也可以找到这个(你也可以在 [Twitter](http://www.twitter.com/pubs12) )* 上找到我)