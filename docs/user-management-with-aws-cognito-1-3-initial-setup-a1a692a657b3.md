# 使用 AWS Cognito 的用户管理— (1/3)初始设置

> 原文：<https://www.freecodecamp.org/news/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3/>

作者:黄

# 使用 AWS Cognito 的用户管理— (1/3)初始设置

#### 完整的 AWS 网络样板——教程 1A

![e1CBg1j1AJmx1aoLC5kX0KWTfCpBhk2evIa9](img/086ea97dbbc37b036ab0542443a12d96.png)

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

点击下载 Github [。](https://github.com/kangzeroo/Kangzeroos-AWS-Cognito-Boilerplate)

### 介绍

设置用户认证可能需要很长时间，但它是任何生产应用程序的重要基石。有一些选项，比如 AuthO 和 PassportJS，但是它们要么有很难的学习曲线，需要持续的维护，要么容易出现程序员错误，因为它们需要自我设置。要是云上有一个无需干预、可定制、安全且高度可扩展的用户管理服务就好了。

介绍 Amazon Cognito 和联合身份。Cognito 是用于管理用户配置文件的 AWS 解决方案，联合身份有助于在多次登录时跟踪用户。AWS Cognito 集成到 AWS 生态系统中，为高级前端开发开辟了一个可能性的世界，因为 Cognito+IAM 角色为您提供了对其他 AWS 服务的选择性安全访问。想只允许特定的注册用户访问 S3 存储桶吗？只需连接一个具有 IAM 角色的 Cognito 登录名，就可以访问 bucket，现在您的 bucket 是安全的了！最重要的是，免费层为您提供了 50，000 个月活跃用户，因此您不必担心支付更多费用，直到您准备好蓬勃发展。

这个样板文件是一个 React-Redux web 应用程序，它预集成了 AWS Cognito 和联邦身份的全部功能。如果您有一个应用程序，希望从一开始就使用生产就绪的身份验证服务来开发，请使用此样板文件。事实上，这是你下一个伟大想法的强大发射台。

从 AWS 控制台上的 AWS Cognito 开始！

![L-eUlNJZNfWnJdq5Fpiv6KZ1GXgyJrl871Zd](img/435bbe5798aad558ba46d25d29c04f0b.png)

### 初始设置—认知

![tkMhJH8RcXnUWHfhkoJdvMzJxm4Y2JKsPUaQ](img/3c4b9c7281cc8141e02f41c5a2317a12.png)

我们将设置 AWS Cognito，这是一个自定义的登录池(如使用电子邮件登录)。Cognito 不是任何类型登录(如脸书和 Gmail)的登录管理器，只用于自定义登录。

让我们首先通过单击“管理您的用户池”来创建一个用户池。用户池是一组完成相同任务的用户。如果你要克隆优步，你需要创建两个用户池——一个给司机，一个给乘客。现在，让我们创建一个名为“App_Users”的新用户池。设置屏幕应该如下所示:

![SfXQUIUYNXLW0RUY8iwP4ZBic6Jrb6yIz24s](img/44c61e06b1366e56223767fd00e5d138.png)

User Pool Name

我们将逐步完成此过程，因此输入“App_Users”池名称，然后单击“逐步完成设置”。下一步是“属性”，我们定义“App_Users”将拥有的属性。

![O7QhmNjg9m9RpNwLUz2hjfbzv9sxolLuP7n4](img/0c829d92a213b55f9f11fefa1bfa0aae.png)

User Attributes

我们现在，我们只想有一个电子邮件，密码和“代理名称”。电子邮件是用户的唯一标识符，密码是必填字段(这就是为什么您在标准属性列表中看不到它)。我们希望用户能够有一个代号，所以让我们设置“agentName”是一个自定义属性。我们仅使用“agentName”来展示如何添加自定义属性。向下滚动，您将看到添加自定义属性的选项。

![Scp0n-wf0VsNHQQ03J1xo0tXPwRDJ8CwFu6a](img/1d81fd9018d7d4f0425a192fd1043c60.png)

Custom Attributes

截至本教程编写之日，您无法返回并更改自定义属性(尽管 AWS 似乎可以)，所以请确保第一次就做对了！如果您需要更改属性，您必须创建一个新的用户池。希望 AWS 尽快解决这个问题。无论如何，继续帐户政策！

![EzZmDaVOQKLsGxqQb-TF6KfNNG43y7GeDtPo](img/d57440912a9a377ca3f0ae3af022db36.png)

Account Policies

因此，我们可以看到，我们的密码可以强制要求某些字符。显然，要求混合各种字符类型会更安全，但用户通常不喜欢这样。作为折中，我们只要求密码长度为 8+字符，并且至少包含 1 个数字。我们也希望用户能够自己注册。其他部分并不重要，所以让我们进入下一步:验证。

![ZSesvPppQzQtylalxAt8kYO3ptalae1VCJa1](img/fc437903697ab7020b685c790cb12d05.png)

Account Verifications

这部分很酷，我们可以轻松集成多因素认证(MFA)。这意味着用户必须使用电子邮件以及电话号码等其他形式的身份验证来注册。PIN 将被发送到该电话号码，用户将使用它来验证他们的帐户。我们不会在本教程中使用 MFA，只是电子邮件验证。将 MFA 设置为“关闭”,并仅选中“电子邮件”作为验证方法。我们可以保留已填写的“AppUsers-SMS-Role”(IAM Role)，因为我们不会使用它，但将来可能会使用它。Cognito 使用该 IAM 角色来授权发送在 MFA 中使用的 SMS 文本消息。因为我们没有使用 MFA，所以我们可以继续:消息定制。

![7GSg9dIVav372e4QPuRJ9bXcTfmIGruwibQz](img/76f7743df48ba2470b0eab04742713cb.png)

Custom Account Messages

当用户收到他们的帐户验证电子邮件时，我们可以指定该电子邮件的内容。在这里，我们制作了一个自定义电子邮件，并以编程方式将它放入表示为`{####}`的验证 PIN 中。不幸的是，我们不能传入其他变量，如验证链接。为此，我们必须结合使用 AWS Lambda 和 AWS SES。

![z-bUgpq6-KqndouSQACi5OpsRb8EpV3KSFcD](img/a9fc297f04099628fd8320774c01a668.png)

Custom Account Messages

在邮件定制步骤中向下滚动页面，我们可以添加自己的默认发件人和回复地址。为了做到这一点，我们需要在 AWS SES 中验证一封电子邮件，这很容易设置，而且非常快速。在新选项卡中，通过单击左上角的橙色立方体转到 AWS 控制台主页。在控制台主页中，搜索 SES(简单电子邮件服务)。单击进入 SES 页面，然后单击左侧菜单中的电子邮件地址链接。

![A5RdmoIRfu75F4pAlnHf0uykAvpwE3wkpSk0](img/3f606eb412770f4caddf48775e41e05a.png)

接下来点击“验证新地址”，并输入您想要验证的电子邮件。

![QDUaZrmT73RnSATgkl8onbn8ln21LJWvLe3j](img/2768f5578f60227e6313fef874f41478.png)

现在登录你的邮箱，从 AWS 打开邮件。单击电子邮件中的链接进行验证，您将再次被重定向到 AWS SES 页面。您已经成功验证了一封电子邮件！那很容易。

现在已经完成了，让我们回到 AWS Cognito，继续讨论:Tags。

![3--4yXNLjHuDWWdAP5ELZwePidQoZeloH4Hq](img/146c06346cc2777115adb7642d4f4dab.png)

User Pool Tags

向用户池添加标签并不是强制性的，但是对于管理许多 AWS 服务来说，这绝对是有用的。让我们为“AppName”添加一个标记，并将其设置为值“MyApp”。我们现在可以继续:设备。

![wJYvzItugAwEyXSY9cXI9NAhKcDfut4rPEK1](img/6349f14acd796b24b3d439590420e85c.png)

Devices

我们可以选择记住用户的设备。我通常选择“总是”，因为记住用户设备是免费的，不需要我们编写代码。这些信息也很有用，为什么不呢？下一步:应用程序。

![2UfKm3CYYbsQa6QdKvGPwPILUvk4j1h7io0a](img/d7e4b52022cf8fba8e09b0f259b76482.png)

Apps

我们希望某些应用程序能够访问我们的用户池。这些应用程序在 AWS 生态系统的任何其他地方都不存在，这意味着当我们创建一个“应用程序”时，它只是一个认知标识符。应用程序是有用的，因为我们可以有多个应用程序访问同一个用户池(想象一个优步克隆应用程序，和一个免费的驾驶考试练习应用程序)。我们将刷新令牌设置为 30 天，这意味着每次登录尝试都将返回一个刷新令牌，我们可以使用该令牌进行身份验证，而不是每次登录。我们取消单击“生成客户端密码”，因为我们打算从前端而不是后端登录到我们的用户池(因此，我们不能在前端保存密码，因为这是不安全的)。点击“创建应用”,然后点击“下一步”进入:触发器。

![3Z8IPzD9uRGcLI3aATFoAqgCoSHK3lY4hQiT](img/76238b90ac89f137ea11f3eedaa21069.png)

Triggers

我们可以在用户认证和设置流程中触发各种动作。还记得我们说过我们可以使用 AWS Lambda 和 AWS SES 创建更复杂的帐户验证电子邮件吗？这就是我们要设置的地方。对于本教程的范围，我们将不使用任何 AWS Lambda 触发器。下面进入最后一步:复习。

![jlPgyxBt0tMrpuc0jldofVdeSypisGWCocDI](img/f93faedbb5c1753b5772583095c9c830.png)

Review

在这里，我们回顾一下我们所做的所有设置配置。如果您确定此信息，请单击“创建库”，我们的 Cognito 用户库将会生成！

记下“Pool details”选项卡中的池 Id `us-east-1_6i5p2Fwao`。

![QqbMbVl7GvHh083UvY39YXbkeTYqv0D0biOp](img/6fdf8580e0b1513a5cf59d986360ac78.png)

Notice the Pool Id

以及应用标签中的应用客户端 id `5jr0qvudipsikhk2n1ltcq684b`。我们将需要这两个在我们的客户端应用程序。

![-8p3fPT8mAl-wVuEFRAjztUKA18j6QPe0mrv](img/38b91cb07a552e2cc7977754b50dd4c3.png)

Notice the App client id

既然已经设置了 Cognito，我们就可以为多个登录提供者设置联合身份了。在本教程中，我们不涉及 FB 登录的细节，因为它不在本系列教程的范围之内。然而，集成 FB 登录是非常容易的，我们将在下面的部分展示如何完成。

### 初始设置—联合身份

![90IsJWecVCVPebDqABxVaAxqJ81CbSN80qwd](img/ac5163a1722344134b3bb33992225a54.png)

接下来，我们要设置“联合身份”。如果我们有一个应用程序，允许多个登录提供商(亚马逊认知，脸书，Gmail..等等)，我们将使用联合身份来集中所有这些登录。在本教程中，我们将使用我们的亚马逊 Cognito 登录，以及一个潜在的脸书登录。转到联合身份并开始创建新身份池的过程。给它一个合适的名字。

![Z4DaYZSFEvuzVONWBSu7zNIJrEcvFY3hjFFJ](img/cf01693a34855b9ca7a95899fe345d6e.png)

现在展开“身份验证提供者”部分，您将看到下面的屏幕。在 Cognito 下，我们将添加刚刚创建的 Cognito 用户池。复制并粘贴我们之前记下的用户池 ID 和应用程序客户端 ID。

![2aWx2T6c3Ek3SdCLvsUqiNhkusJSTUJGu0hc](img/54e7600dd53ee03413cf47c60be01502.png)

如果我们希望脸书登录同一个用户身份池，我们可以转到脸书选项卡，只需输入我们的脸书应用程序 ID。这就是 AWS 控制台上的全部内容！

![SspjVsav0UYlQCQEwAh0Y7vWbJQrGIET4Rm8](img/c8233e1977afb63ef87a41fc233ddc7d.png)

保存身份池，您将被重定向到下面的屏幕，在这里创建 IAM 角色来表示联合身份池。未经身份验证的 IAM 角色适用于非登录用户，经过身份验证的版本适用于登录用户。我们可以授予这些 IAM 角色访问其他 AWS 资源的权限，如 S3 存储桶等。这就是我们如何通过在整个 AWS 生态系统中集成我们的应用程序来实现更高的安全性。继续完成此身份池的创建。

![QF9nJpmbHzNiblaOlNxJ7N6FxJqvI0EeInIS](img/078d2b6dd7d4307dd20a52a5e6acb409.png)

成功创建身份池后，您现在应该会看到下面的屏幕。您现在只需要注意一件事，即身份池 ID(即我们将在后面的代码中使用它。太好了！

![2ZmhE7019DoVa-o4efQtPW7UeGeLVuWQZ2SI](img/21a9a418012b27e936818dfaa0733c31.png)

退出所有程序，返回 AWS Cognito 主屏幕。如果我们进入 Cognito 部分或 Federated Identities 部分，我们会看到已经设置了两个必要的池。AWS Cognito 和 AWS 联合身份已准备就绪！

![FI4-abtFRmp4r9eszvErYlUSlAgFZyTBuPBy](img/8f28c8ca2dd2b50ecb456a202d03cf0e.png)

AWS Cognito

![9OogLw3GvEXUarO55p4KaILnCl27aF52fYMw](img/c2b987ced3296940f8e00480b4cfb835.png)

AWS Federated Identities

这就是全部的设置！有了这两个池，我们可以将其余代码集成到 Amazon 的完整认证服务中，并实现顶层用户管理。这比自定义 OAuth+Passport.js 简单多了！如果你喜欢你目前所看到的，请继续阅读！记住你学会这一次以后，以后就超级容易了，所以时间投入绝对值得。下一节再见！

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

> 这些方法在 [renthero.ca](http://renthero.ca) 的部署中被部分使用