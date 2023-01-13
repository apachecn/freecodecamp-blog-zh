# 我用 Python 和 Django 在我的网站上建了一个会员区。以下是我学到的。

> 原文：<https://www.freecodecamp.org/news/i-built-a-members-area-on-my-website-with-python-and-django-heres-what-i-learned/>

我决定是时候升级我的个人网站了，以便允许访问者通过一个新的门户网站购买和访问我的课程。

具体来说，我想为访问者注册一个帐户，查看我的可用课程，并购买这些课程的地方。一旦用户购买了课程，他们将能够永远访问课程中的所有内容。

这在理论上听起来可能很简单。然而，如果不使用 Shopify 这样的电子商务网站，会员网站会变得异常复杂。

在本文中，我将向您介绍我所做的决定以及我用来构建这个新网站的技术，包括:

1.  如何开始？
2.  启动 Django 项目
3.  如何设置 Django 模型
4.  整合[条纹](https://stripe.com/en-ca)支付
5.  在 AWS EC2 实例上部署我的新站点
6.  如何从现有页面克隆 CSS

![VcTBavKxXlNR907_Gc8lRLWvXPC_8avJogGeEGw7ykM6d9WwZskU6GPvsZjAepF4glzURlMkeKBjdw5yhAcjupP_9AFZEdWVEJEEGRgJs33VbIY2gME4MOk5imvszBkpxBmPBeXJ](img/e3c6bb93c5475f836eea34f8f0e69cb9.png)

## 如何开始？

当在你的网站上添加一个全新的功能集时，把这个网站组织成你原来网站的一个子域是合乎逻辑的。

一个子域就是它听起来的样子。它是*一个域是另一个(主)域的一部分。*子域名显示为主域名 URL**前*的一个新部分。***

**更具体地说:**

*   **我的*主*域是:【https://nickmccullum.com】T2**
*   **我的新*课程*子域是:【https://courses.nickmccullum.com】T2**

**子域的主要优点是它们是免费的！更不用说，一个已经排名很好的网站的子域名会很快被索引，并从其母网站的成功中受益。**

**我知道我需要一台服务器来托管我的新网站。我还需要用一个弹性的 IP 地址连接那个服务器。**

**弹性 IP 地址是永远不会改变的静态 IP。这意味着公众可以全天候访问它。**

**如今，让服务器启动并运行的最快方式是将其托管在云中。云计算有很多选择，包括亚马逊的 AWS、T2 的数字海洋的水滴和 T4 的 Azure 的容器。就价格而言，所有可用的选项都相当平等——所以这并没有过多地影响我的决定。**

**我以前有过使用 AWS(亚马逊网络服务)的经验，这是一种基于云的托管基础设施的服务。很自然，我选择在这里托管我的服务器。更具体地说，我在一个 [EC2](https://aws.amazon.com/ec2/) 实例上托管站点。我们稍后会详细讨论这一点。**

**好了，现在我知道我想在哪里托管我的新网站，接下来呢？是时候考虑一下网站的技术了。当考虑用什么技术来建立你的网站时，重要的是要考虑以下几个主题:** 

1.  **你精通什么**
2.  **选择能够很好地结合在一起的前端和后端技术**
3.  **网站的表现**

**您应该回答这些问题，并尝试选择适合您的需求和能力的技术堆栈。对我来说，我最精通 Python，所以自然选择了 Django 3.0。**

**我以前做过 Django 应用程序，所以我对它的基础设施非常熟悉。然而，我从未从头开始构建 Django 项目。**

**因此，我有一些阅读要做。随着我对这个流行的框架了解得越来越多，我不断地将它与流行的 web 编程工具 PHP 进行比较。我过去曾在多个 Wordpress 网站上工作过，Wordpress 是建立在 PHP 之上的，所以这是一个自然的比较(至少对我来说)。**

**根据他们的文档和 Stackoverflow 上的各种帖子，以下是 Django 框架和主要 PHP 框架之间的主要区别:**

*   **Django 在默认情况下更注重安全性，并提供内置的安全实践来帮助程序员在开发过程中节省时间。**
*   **姜戈专注于速度。众所周知，它是一个帮助开发者尽快起步的框架。**
*   **与大多数基于 PHP 的框架相比，Django 的性能稍低。**

**我想谈谈最后一点。Python 是一种解释型语言，与其他编程语言相比，它的性能通常较低。当一个新的程序员听到类似这样的话时，他们可能会认为 Python 比其他语言更糟糕，因为性能在计算中非常重要。**

**尽管与其他语言相比，Python 确实有较低的性能标准，但这是一个非常模糊的说法。事实上，Django 和 Laravel(一种流行的基于 PHP 的框架)之间的差异非常小，几乎可以忽略不计。**

**为了让这种性能差异对您有意义，您需要编写一个拥有数百万用户的高度依赖性能的应用程序。得知世界上许多最大的 web 应用程序都是基于 Django 构建的，我很受鼓舞。换句话说，如果 Django 对 Instagram 来说足够好，那么它对我的网站来说肯定足够好。**

**最后，我决定用 Django 建立我的课程网站，主要是因为我有使用 Python 的经验。学习一个新的 web 框架是一个不错的收获。**

**接下来，我知道我将需要这个网站的数据库。有了 MySQL 和 [PostgreSQL](https://nickmccullum.com/sql/sql-installation/) 的经验，我原本打算在这里再次使用它。然而，Django 默认提供了一个 SQLite3 数据库服务，只需要很少的设置。我从未使用过 SQLite，所以我做了更多的研究。**

**基于性能和数据存储需求，Django 附带的默认数据库对于我的站点来说已经足够强大了。我震惊地发现一个轻量级版本的数据库服务竟然如此强大！**

**对于任何不熟悉这项技术的人(像我一样)，SQLite3 是一个关系数据库，对于低到中等流量水平的站点(每天大约 100K 的点击量)具有很好的性能。SQLite3 可以与网站运行在同一台服务器上，而不会影响性能。这意味着我不需要构建单独的 Amazon RDS 实例，这在部署阶段节省了一些资金。**

## **启动 Django 项目**

**![d5I_NnBMYltwX71zwpcZJ6Fx54w-AU-WR2GRRzrJgw70jY5Xd3oTAEZnmfnslPwM-EeZ-na8_KHOjrlk-Z9VH9gGKvuF8UXz4fOgewIA8dD1-baCKsEMwRaxIRUszVS1z9Ggdux6](img/7664cc169b4eb5c1b4f985aaa4dcbb17.png)**

**Django 是一个高级 python web 框架，主要目标是允许**快速开发**并默认提供安全性。它解决了 web 开发的许多麻烦，减少了重复的编码实践。**

**使用 Django 最好的一点是它是完全免费的。**

**Django 旨在帮助开发人员快速启动他们的网站(这是我选择在这个项目中使用它的主要原因之一)。这个框架(和大多数其他框架一样)我最喜欢的特性之一是它们的前端模板系统。

[Django 模板](https://docs.djangoproject.com/en/3.0/topics/templates/)允许你编写动态代码，然后生成想要的 HTML 和 CSS。这使您能够使用诸如循环和 if 语句之类的结构来创建动态 HTML 代码(这意味着它为每个用户呈现不同的内容)，然后可以作为静态文件使用。
以
为例:**

```
`# course_titles_template.html
{% for course in courses_list %}
<h1>{{ course.course_title }} </h1>
{% endfor %}`
```

**会为在`courses_list`对象中找到的每个课程变量创建一个标题。这可能会呈现一个带有`<h1>`标签的 HTML 文件，其中包含每个课程的标题，如下所示:**

```
`<h1> Python Fundamentals </h1>
<h1> Advanced Python for Finance and Data Science</h1>
<h1> How to Run Python Scripts </h1>
<h1> How to Make A Python Class </h1>`
```

**模板系统将您从大量的手工劳动中解放出来。允许 HTML 动态呈现可以省去每次添加新对象时更新代码的麻烦。

这个模板系统还允许网络应用随着我添加更多内容而更新。因此，在这种情况下，如果我要向我的数据库添加一门新课程，这个模板不需要更改。它只是在一个新的标题标签中呈现我的新课程的标题。

Django 也让一个项目的起步变得极其容易。一旦你安装了 Django，你就可以使用`django-admin`来启动一个项目，甚至设置你的应用程序。**

**等等，应用程序？项目？有什么区别？

app 是执行某种功能的网络应用。它可以是一个博客，*，一个登录系统，*，甚至是一个文件服务器。项目是构成网站的应用程序和配置的集合。**

### **安装 Django:**

**最简单的安装方法是使用 Python 包管理器 pip。**

```
`python -m pip install Django`
```

**要获得完整的安装指南，请查看 Django 的官方文档。**

### **开始一个项目:**

**一旦你安装了 Django，你就可以使用`django-admin`工具，它可以帮助开发者设置项目和应用程序，也可以提供其他方便的工具。

运行`django-admin startproject myproject`将会在当前目录下创建一个新的文件夹，你的项目将存放在这个文件夹中。它还将创建许多您开始工作所需的必要文件。**

**运行此命令后，您的目录看起来会是这样:**

> **我的项目/
> manage.py
> 我的项目/
> __init__。py
> 设置. py
> 网址. py
> asgi.py
> wsgi.py**

**在`myproject`文件夹中，你会发现一个`manage.py`文件，它非常有用，提供了许多便利的工具。将会有另一个名为`myproject`的文件夹，你可以在那里设置你的项目配置。

外部的 myproject/ root 目录是你的项目的容器，它的名字实际上并不重要，如果你觉得这很混乱，你可以把它重命名为你喜欢的任何名字。

内部的 myproject/目录是项目的实际 Python 包。它的名字是 Python 包名，您需要使用它来导入包中的任何内容。**

**这里需要注意的重要文件是`myproject/settings.py`，其中配置了 Django 和 app 的特定设置，还有`myproject/urls.py`。**

**`urls.py`文件用于在您的网站中创建 URL，并将它们指向一个服务请求的位置。这张图片很好地解释了 Django 如何处理请求:**

**![sSrqigM9D-JRg3XQg_sh9WvtehX-UKJnnOmygeIn4u8-Ht2xcv_y0jZ1toPcg8mgoA4BIC_7Y14AG4DKKJ6CIV3IzON06BtmjLp-cTyL8GJ7CvHRr-odmE1Yofo0cGYzhuPbgXQI](img/fda9c2ea1025713ab7107fe3b47b1a59.png)**

**瑞安·内维斯创造了如此精彩的视觉效果，值得称赞。**

**文件概述了整个网站的 URL 解析。您添加到网站的每个应用程序都将包含自己的`urls.py`文件，该文件概述了特定应用程序内的 URL 解析。**

**现在您已经掌握了这些文件的用途，让我们开始使用 manager 脚本的命令来启动项目。**

**需要注意的一个命令是`startapp`命令，它用于在您的项目中创建一个应用程序，就像您创建应用程序一样。`python manage.py startapp myapp`将创建一个新文件夹和一些在项目中创建新应用程序所需的文件。**

> **myapp/
> __init__。py
> admin . py
> apps . py
> migrations/
> _ _ init _ _。py
> 模型. py
> 测试. py
> 视图. py**

**这里的主要区别是模型和视图文件的存在，它们分别用于定义数据库和应用程序的前端功能。**

**模型是定义数据库表的类。我们将在本教程的后面更详细地讨论模型。**

**视图控制应用程序的前端结构和功能，接收 web 请求并返回 web 响应。

可能需要记住的最重要的命令是 runserver 命令:
`python manage.py runserver`。这将在本地主机的默认端口 8000 上运行您的项目。**

**就是这样！通过三个简单的步骤，您将看到一个欢迎登录页面，显示安装成功。**

**![przKJhywBTWAxqF7H1D7YbreiryXuAE4k1a_3ZmGQ0zu7ByEFTI-LBW-PsyBJlT7sUSmWXvycmlZcwAkK0QoOe-bi3zmPmF61KbGsUfNtUE1WVlkSnaCjIgE_00Kn0i8osx1Ipb_](img/6576e60ccbd5f330584b8249f5e81042.png)**

**在 Django 的文档中有一个写得非常好的教程，提供了一个关于开始一个项目的更深入的介绍。可以在这里找到:[开始一个项目](https://docs.djangoproject.com/en/3.0/intro/tutorial01/)**

## **如何建立模型**

**像许多其他 web 框架一样，Django 有一个对象关系映射( **ORM** )概念的实现。在 Django 中，这种实现被称为模型。**

**在 Django 中开发项目时，模型是需要理解的一个非常重要的主题。在最基本的形式中，模型可以被认为是数据库表的包装器。

换句话说，Django 模型是用来定义你的数据的。它包含您存储的数据的字段和行为。每个模型映射到数据库中的单个表，模型中的字段映射到数据库中的字段。**

**在编写模型时，您可以访问强大的内置字段类型，这些类型为您做了大量繁重的工作。忘记手动编写 SQL 代码来构建数据库。您可以简单地编写一个模型类并运行迁移命令，将一个功能完整的 SQL 脚本加载到您的数据库中。** 

**![CVP_WO8s02EEdmC8_A0w6gbAFmH4AbZ5Z65XatAvx9j31rLjJ1oQMgW4EmBMTlOz0hRWjiybiqnYgadFVX4rlCKLBZGX8pEbhYKbsYwCXwEClo444j6eRt3UeTYurZPg047u5-LH](img/4cd68b4edb15d6bfa8396504fc2288b6.png)**

**Django 提供了一个[用户模型](https://docs.djangoproject.com/en/3.0/ref/contrib/auth/#user-model)作为其内置认证系统的一部分，允许你忽略所有登录/注册和密码处理的后端。**

**在为我的新网站设计模型时，我需要以下三个模型:**

*   **Profile——一个围绕用户模型的包装类，用于添加与身份验证无关的信息(通常称为[概要模型](https://docs.djangoproject.com/en/3.0/topics/auth/customizing/#extending-the-existing-user-model))**
*   **课程-存储每个课程的所有信息**
*   **文档——一种模型，用于存储每个课程对应哪些文件的信息。我特别想上传降价文档，因为我的公共博客就是这样建立的**

**这里有一个模型的例子:**

```
`class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)    			
    enrolled_courses = models.ManyToManyField(Course)`
```

**配置文件模型是扩展现有用户模型功能的有用工具，以便存储关于用户的信息，而不仅仅是身份验证数据。在我的例子中，我创建了一个名为 profile 的 *profile 模型*,用于存储用户注册了哪些课程。**

**这是我的课程模型:**

```
`class Course(models.Model):   
    course_title = models.CharField(max_length=200)   
    course_description = models.CharField(max_length=500)   
    course_price = models.DecimalField(max_digits=10, decimal_places=2)`
```

**我的课程模式相当简单。我只需要存储 3 条关于每个后勤课程的信息，而课程的实际内容由文档模型处理。**

```
`class Document(models.Model):   
    course = models.ForeignKey(Course,on_delete=models.PROTECT)   
    file = models.FileField (
upload_to=markdown_upload_location,
default='default.md'
)`
```

**这里我利用了 python 的一些内置功能，我将函数`markdown_upload_location`传递给了`FileField`构造函数。**

**此函数用于确定上传的文件将存储在文件系统的什么位置。将函数传递到构造函数中允许函数在每次上传新文件时运行，而不是只运行一次，然后再次使用相同的结果。**

**本质上，当一个管理员(我)上传一个新的课程到网站时，会为该课程创建一个新的文件夹，并且该课程的所有降价文件都存储在那里。文档模型记录将这些文件链接到数据库中的课程记录。**

**我从建立这些模型中学到的一件事是设计我的数据库的过程变得多么容易。MySQL Workbench 和 ERR 图，或者一行一行地编写 SQL 并对模式进行痛苦的更新的时代已经一去不复返了。**

## **整合条纹支付**

**Stripe 是一个平台，世界各地的许多网站都使用它来收取客户的付款。它是安全的，易于客户使用，最重要的是，它易于我们开发人员设置！**

**与他们的竞争对手相比，价格也相当公平，目前每笔交易的价格为 2.9% + 0.30 加元。此价格适用于一次性付款及其订阅注册。**

**为了使用 Stripe 作为开发人员，您必须创建一个帐户，并查看他们的开发人员页面以查看选项。他们有预建的签出，完整的库和 SDK 来构建您自己的自定义签出。Stripe 还为 web 框架(Wordpress、Drupal 等)提供了预先存在的插件。)**

**我决定使用他们的 [Checkout](https://stripe.com/docs/payments/checkout) 工具，这是一个安全的、条纹托管的支付页面，它让我不必构建支付页面。这不仅节省了开发用于收集支付信息的前端页面的时间，还减少了在后端保护支付的麻烦。**

**如今，安全是一个巨大的话题，客户对他们在哪里提供信用卡信息很警惕，所以对我来说，使用 Stripe 是显而易见的。我不存储任何用户的详细信息。相反，它们会被直接发送到条带区，在那里可以得到安全处理。**

**只需几行代码，我就可以导入 Stripe 预建的 Javascript checkout 模块。以下是脚本标签:**

```
`<script 
src="https://checkout.stripe.com/checkout.js"    
class="stripe-button"    
data-key="{{ key }}"    
data-description="Payment: {{ course.course_title }}"    
data-amount="{% multiply course.course_price 100 %}"    
data-locale="auto">
</script>`
```

**这里，data-key 被设置为 Stripe 公钥，类似于任何开发人员 API 密钥。描述是收到的付款将出现在您的条纹仪表板上，金额是购买的美分数。这个简单的包含将这个支付页面作为网站上的一个模式导入:**

**![ZoLSCpczDEDUwno0TaWc8jYVl4dTnBxnL1bZBMNjyVJ46UDPbP271hBWancYSPPWA9IDPZHxg6YWSEQEyrcIyNkb4eMpg5iIMHnl0NSJ1oiF4QZluZ8YarjqWStmUc3c0-2hvY3c](img/25d2b868c6a6193074e273cc8568f91c.png)**

**一旦客户填写了支付信息，您只需将支付信息捆绑到一个请求中，并将其发送给 Stripe。接下来，Stripe 能够处理信息并在几秒钟内批准付款。**

```
`# Send the charge to Stripe
charge = stripe.Charge.create(    
amount=amount,
currency=currency,    
description=f"Payment for course: {courseTitle}",    source=self.request.POST['stripeToken']
)`
```

## **在 EC2 实例上部署我的新站点**

**![uRaS5Pqadp9Z-dGeRfXf12DaLLZLpcfQum70RzmQRNUWfcN9JM3Gc46pjCj8KIReqmP922jI9hWQRYDWG0i8r_eBOb5zNh6a0zrO7PXm4qzPPBVRRAsHsjjok2IyxcmsCG1UofJM](img/955535d708fabd67fdde65f46540d962.png)**

**一旦我在本地主机上完成了新站点的开发，我需要找到一个地方来部署它。我有一些 AWS 的经验，已经有了一个帐户，所以它很容易作出决定。亚马逊的弹性计算云——通常被称为 EC2——允许大量的配置，我只是选择了最简单的设置。更确切地说，运行在 T2 微服务器上的 Ubuntu 机器对这个网站来说已经足够了。**

**设置服务器是部署中最简单的部分，我在不到 10 分钟的时间里就设置好了服务器。接下来，我必须为实例附加一个弹性 IP 地址，并更新我在 [Route53](https://aws.amazon.com/route53/) (我的域所在的位置)中的 DNS 记录。

设置好服务器后，我必须弄清楚如何为访问者提供网站服务。我过去有过一些使用 Apache 的经验，所以这是一个自然的选择。事实证明，Apache 和 Django 配合得非常好。**

**Django 是通过它的 WSGI(网络服务器网关接口)提供服务的——这是一个 Python 的快速 CGI 接口，如果你熟悉的话，它类似于 PHP 的 FPM。简单地说，WSGI 是 Django 和 web 服务器之间的一个层，充当服务于 web 页面的接口。**

**您可能已经知道，Python 通常在`virtualenv`中运行。这就创建了一个虚拟环境，在这个环境中，特定项目的依赖项可以共存，而不会干扰系统的 python 版本。**

**如果你想学习更多关于 virtualenv 的知识，请查看 Python 的搭便车指南。**

**基本上，这只对配置 Apache 配置很重要。为了正确地提供文件，您需要为 Django 项目创建一个 WSGI 守护进程，如下所示:**

```
`# /etc/apache2/sites-available/mysite.conf:

WSGIProcessGroup courses.nickmccullum.com

WSGIDaemonProcess course python-path=/home/ubuntu/django/courses-website python-home=/home/ubuntu/django/courses-website-venv

WSGIProcessGroup course

WSGIScriptAlias / /home/ubuntu/django/courses-website/courses-website/wsgi.py

<VirtualHost *:80>
        ServerName courses.nickmccullum.com
</VirtualHost>`
```

**这告诉 Apache 利用 WSGI 守护进程来正确地服务来自 Django 项目的文件。设置好之后，我需要重启 Apache，等待 24 小时 DNS 记录更新，然后——瞧:** 

**![rPc_HYj7M-kwBTan4LUTk4pqkYBYsU4PKksK6gfYo0tV9WzX128fl67Iu0m_xz2-TVmfhRNYvMx4OWzrJP_nr5DE2ECQUXQ6Ym1P1gVFhVpLYH2_3_PE6_JcMLNBZBvWemwNDCuY](img/0a59b9a1c6a09d6f05fa1e235a94fd62.png)**

**最后一步，我需要用 SSL(安全套接字层)来保护我的站点。毕竟，我要求人们在我的网站上付款，所以客户会希望该网站是安全的！**

**在我看来，在网站上启用 SSL 最简单的方法是通过[加密](https://letsencrypt.org/)。他们免费提供一款名为 [Certbot](https://certbot.eff.org/) 的工具，可以在你的服务器上启用该工具来自动更新服务器证书，并保持你的服务器全年全天候运行。**

**就像下面三步这么简单:

1。安装证书机器人:**

**`sudo apt-get install certbot python3-certbot-apache`**

****注意**:这个脚本会查看 apache 配置文件中的 ServerName 设置来创建证书，所以在运行它之前要确保已经设置好了。**

**2.获取证书并告诉 certbot 自动更新 apache 配置:**

**`sudo certbot --apache`**

**3.测试自动续订:**

**`sudo certbot renew --dry-run`**

**一旦您配置了 SSL，您就可以通过检查这个网站来测试以确保证书安装正确:[https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)。**

**用 SSL 保护我的站点后，我打开了 EC2 实例的安全规则，允许站点公开。随着我的新站点在 EC2 实例上的建立和运行，我现在能够安全地向希望了解软件开发中各种主题的客户销售我的课程。**

## **最后的想法**

**我很感激我在这个项目中获得的所有经验，从导航一个新的 web 框架到集成 Stripe API——我确实学到了很多！**

**学习像 Django 这样的新主题可能会让人不知所措，但是我觉得与我读过的其他文档相比，他们的文档非常强大。**

**如果我给你一个建议，我会告诉你任何工具最有价值的资源就是官方文档。写得好的时候尤其如此。但是不管你使用什么工具，永远不要害怕文档，习惯于阅读它们来找到问题的答案。**

**本文由尼克·麦卡勒姆撰写，他在自己的网站上教授 [Python](https://nickmccullum.com/python-course/) 、 [JavaScript](https://nickmccullum.com/javascript/) 和[数据科学](https://nickmccullum.com/advanced-python/)课程。**