# 使用 Go 和 AWS 编码和部署无服务器 API

> 原文：<https://www.freecodecamp.org/news/code-and-deploy-a-serverless-api-using-go-and-aws/>

与在 Node.js、Laravel、Spring Boot 或其他后端框架中编写后端代码相比，创建无服务器 API 通常是更好的选择。

我们刚刚在 freeCodeCamp.org YouTube 频道上发布了一门课程，将教你如何使用 Go 和 AWS 编写和部署无服务器 API。

您将学习如何使用 Go 编程语言创建一个完整的 AWS 无服务器堆栈。本课程涵盖了使用 API Gateway、Lambda 和 DynamoDB 编码和部署一个简单的无服务器堆栈。

本课程包括以下几个部分:

*   项目设置
*   主文件
*   代码的其余部分
*   测试
*   部署

观看以下全部课程或在 freeCodeCamp.org YouTube 频道观看[(2 小时观看)。](https://youtu.be/zHcef4eHOc8)

[https://www.youtube.com/embed/zHcef4eHOc8?feature=oembed](https://www.youtube.com/embed/zHcef4eHOc8?feature=oembed)

## 副本

(自动生成)

在本课程中，Go 编程语言可用于将无服务器堆栈部署到 AWS。

Akhil Sharma 将教你如何使用 go 在 AWS 上用基本的 CRUD 函数编写和部署一个完整的无服务器堆栈。

这是一个完整的无服务器堆栈。

对，所以你使用 DynamoDB 无服务器，你再次使用 lambda 无服务器，然后我们将再次使用 API 网关，无服务器。

现在 API gateway 帮助我们让世界各地的任何人都可以与我们的 lambda 函数进行交互。

所以我们将部署这三样东西。

从 AWS 控制台，对，我们将使用 AWS 控制台，然后我们将对这三项进行设置。

我们将部署所有这些。

世界各地的任何人都可以使用我们的 lambda 函数。

所以这更像是真实世界的场景。

所以我现在在一个航站楼里。

你不需要把这个项目放在你的计划中。

因为是 Golang。

一点十二分。

无论如何，他们有，你知道，去 mod，这是我们无论如何要做的，在这里，去现代，它照顾一切。

我是说，你真的不需要手动做任何事情。

好吧。

所以在这里，我们会说，我们将首先制作一个目录，我们会说，Go server less，和 YT。

好吧，我们来听听这个。

请使用无服务器策略。

因为 yt 基本上就是 YouTube，所以我专门为 YouTube 做了这个项目。

好的，在这里，我说进入模式。

这基本上是 github.com/akil/go 服务器较少。

这就是我的项目。

它为我初始化了 go mod，go mod 基本上就像我的包 json 文件。

如果你来自 Java 背景，它会很容易地列出我所有的依赖项。

现在我能做的就是打开这个项目，代码编辑器，在我的例子中是 VS 代码，只是作为一个指针，对，我在 windows 中使用 Windows，我有一个叫做 WsL 的东西。

里面我用的是 Ubuntu 20.04。

我使用了多个不同版本的 open 来编写 Stan，这是我在 20.04 中使用的视频。

而在我的公开赛 20.0。

例如，我有 VS 代码设置。

好吧。

现在，我不想为调用安装许多扩展，因为我知道当我开始打字时，我将开始编写许多代码。

我会犯一些错误，就在 Golang 那里。

我会犯一些错误，但我会改正。

但是不要担心，我没有扩展，因为我不想开始，你知道，膨胀我的实例。

因为这只是我用的一个例子。

在我的 Windows 电脑上。

两年多来，我大概有 1015 个不同版本的优步，对吧。

所以如果你需要，顺便说一下，main.go 文件是你在项目中的主文件。

这就是你如何开始 main.go 文件，你说包 main，你说 import。

因为你会导入一些包，对吗？不是你将拥有的一切。

内围棋郎围棋。

Lang 是非常模块化的，你必须安装许多外部软件包，以及许多，你知道，与煤炭一起提供的软件包，你必须明确指出，你知道，这是来自可乐的软件包。

因此，我们将使用的第三方软件包将主要围绕 lambda。

好的，我们一会儿会谈到它们。

这里最重要的是，func main，因为这是你进入程序的入口，对吧，func main。

现在，在我开始在 main.go 文件中写任何东西之前，我要做的就是试着创建一个非常简单的项目结构，我需要 build 文件夹，对吧，因为我要把整个项目的构建保存在我的 build 文件夹中。

那就是，我将它压缩，然后我将继续把那个压缩文件带到我的 lambda，对不起，我的 AWS lambda 控制台。

好了，这就是我需要构建文件夹的原因。

我需要我的 CMD 文件夹，因为那是我通常保存我的主要文件的地方对不起，不是在构建中，而是在我的 CMD 文件夹中，我保存我的 main.co 文件。

好的，然后你会有一个文件夹。

所以如果你和 Golang 一起工作，在这之前，就像非常标准的那种结构，没什么，没什么新的。

如你所知，包会有处理程序，正确的处理程序来处理 API。

然后你会有一个用户文件夹。

我会向你解释我为什么这么做。

然后我有一个非常简单的验证函数，我把它放在一个名为 validators 的文件夹中。

验证器

抱歉。

就是这样。

是的，所以在验证器中，我只有一个 Pedley 的小文件，大概 5，6 行代码。

它只是作为电子邮件有效，只是为了检查我的电子邮件对用户是否有效。

在我的用户中，有一个名为 user.co 的文件。

好的，我的处理程序就是我的主要逻辑所在。

这是一个非常短、非常小的项目处理程序，我只有两个文件，我有 API response.go，因为我的 API 网关需要这个文件，我将构建它，你会明白它是什么。

然后是 handlers.co。

对吗？现在，我的 user.go 文件有点像是我的模型和我的控制器的组合，所以我有处理程序。

但是你知道，user.go 文件会有很多代码，那种会和我的数据库对话，好吧，直接和我的数据库对话。

所以我会有那些模型，就像那些结构一样，同时，我们会有模型函数，那些。

所以数据库功能，我会去你的主 go 文件。

和 func main，我想我要做的是我会从这个地方开始，对，一旦我被教导，得到环境 AWS 区域。

所以 AWS 区域非常重要，因为那是你的 lambda 将要去和设置的地方。

没错。

所以对于 AWS，如果你和 AWS 合作过很多，你知道他有不同的区域，对吧。

因此，我的特定 AWS 被认为是配置了印度，这是亚太区南部的一个。

因此，如果你一直在使用 CLI 和设置是，这将是惊人的。

不然，还有，我觉得你应该没问题吧。

但是试着试着设置一个命令行界面。

这很简单，只需要五分钟。

对你来说，跟着我的其他视频学习也很简单，因为在我的其他视频中，这是一个先决条件。

那么，如何创建会话呢？所以你会说 session dot new session。

现在，如果我说这是旧的，谁在这里，对，所以我需要我们在这里。

那是包裹，对吗？如果我在这里调用 session，session 是 AWS lambda 给我的，AWS 给我不好意思，我说 github.com/now.

查看 AWS 软件包的文档，同时，你也可以这样做，如果你是那种想知道你要安装哪个软件包的人，因为我是那种人，我想安装任何额外的软件包。

所以如果你像我一样，你，你可能想打开，就像我已经做的那样，你可能想打开 AWS SDK，它被称为所以我会说 AWS slash AWS SDK for go，你想，我们想打开文档。

在这里，您会看到它只是大幅缩短了会话时间。

好的，为了创建新的会话，你会说 AWS 点配置和括号，括号是花括号，你会说区域，AWS 绷紧的弦，说你会通过区域。

好吧，好吧。

然后，如你所知，与 Golang 有关的所有事情，每当我们没有做时，我们通常会得到两件正确的事情，我们得到我们正在寻找的东西，这是来自会话功能的耳机会话，对，或者不好意思，是新的会话功能，这是我们这里的会话包的一部分。

我们得到了错误。

所以使用 go Lang，你必须不断处理错误，这是一种非常简洁的处理错误的方式，因为在每个阶段，你都知道错误来自哪里，并且可以自己处理错误。

因此，如果有错误，只需返回 right，我想创建一个名为 Dinah client 的变量，对于我的 DynamoDB 和 dot new，以及如何创建一个新的客户端，将显示 AWS session perfect，这是您刚刚在这里创建的。

你已经创建了会话，然后我们说 lambda 点开始和处理程序。

所以你现在可能不明白这不是问题。

首先，我们想要 DynamoDB 你怎么得到它，你这样得到它，它会说 github.com/aws/aws SDK 去斜线服务斜线 DynamoDB。

好的，你有会话，你有 DynamoDB，你想要 AWS 本身，首先我们会说 github.com slash AWS SDK co slash class。

要得到这个 lambda 函数，或者不好意思，lambda 包，它有 Start 函数，你需要 Start 函数，否则你会说 github.com/aws/aws Lambda go 斜杠 Lambda。

酷毙了。

到目前为止，一切顺利。

那是我需要的，你知道。

我需要一个叫做 handler 的东西，我们会谈到的。

现在，我创建一个名为表名的常量。

稳定名称是 lambda 用户。

我会在 go 用户里说 lambda。

现在是时候创建我的句柄了，基本上是这样的。

所以我路过汉德勒那里。

在我的处理程序中，我有一个请求。

基本上，我接受一些正确的事情。

这个函数，我返回一些东西，它有一些定义。

那么它接受什么呢？它接受事件点 API，网关代理。

请求。

你想知道发生了什么事吗？对吗？那么事件基本上是什么呢？朗加油。

lamda 会给我们，所以我会来这里。

我会说 github.com/aws/aws,拉姆达去斜线事件。

完美。

它接受的另一个东西是 star events dot API。

好吧，代理回应。

对不起，我的意思是它接受一个请求，返回一个响应。

我不确定在那之前我说了什么。

我在这里想说的是，你知道，这是一个函数，它接受一些东西，然后把一些东西，显然接受请求，显然返回一个响应，对吗？没什么复杂的。

这就是你想做的。

您在这里要做的是切换 HTTP 方法。

所以你会说 switch，插入请求点 HTTP 方法。

那么你有什么方法可以发布，上传和删除呢？这就是你的全部。

所以对于每一个不同的历史方法，我们想调用不同的函数。

你是怎么做到的？所以你会说交换。

所以我们已经交换了，对吗？我们已经交换了。

就像，这里 830，所以我有点困了。

我通常在晚上 11 点犯困。

但是不知怎么的，我比较早就困了，因为我醒的真的很早。

我参加了一个武术班，你知道，这让我这些天早上 5 点就醒了。

所以你会有所谓的处理程序。

对吗？这些处理程序从何而来，基本上就是你想要导入的这些处理程序。

因此，哪些基本上是你自己的创造，你知道，你的处理器或你自己的创造会说 github.com/kill/go 服务器少，右斜线包斜线处理器。

酷毙了。

你的意思是，这是我的项目，github.com/search，不用服务器，怀特尼？比如说我有一个名为 package 的文件夹，而不是一个名为 handlers 的包。

对吗？并处理文件。

在那个文件的上面会写着包处理程序，它属于处理程序文件夹，然后 go Lang 理解，好的，这就是我们正在谈论的。

好吧。

格式就是这样。

我的意思是，文件夹名和包名必须相同。

只是让你知道，以防你不知道。

我是说，我很确定你知道，但我只是想确认一下。

所以你有请求表名和 Dinah 客户端权限，所以你把它发送给这个叫做 handlers dot get user 的函数，我们一会儿会用到它。

这就是你的案子。

现在你有了更多的例子，根据 HDB 方法，你有 post，put 和 Delete。

对于 post，您将看到返回处理程序 dot create。

用户。

同样，您将传递请求逗号，表名，逗号动态客户端。

对于 put 你会通过书面，和律师 dot 更新用户请求逗号，对不起，请求逗号，表名，逗号，就餐者客户。

对于删除，处理两个处理程序来删除用户。

同样的事情，你传递同样的事情逗号，黛娜客户端。

删除后，你只需给它一个默认值。

如果您使用 case switching case 语句，您显然知道这个缺省值，对吗？尽量保持方向不变。

不会影响的。

但作为一个好习惯，你知道，我的意思是，我不会做很多次。

但是这些天我试着去做。

好吧。

所以默认情况下，你要调用这个叫做 unhandled 的函数。

方法，它也在您的处理程序文件或处理程序包中。

这是整个画面。

我是说，我不认为我们错过了什么。

这里有操作系统包，有事件 lambda AWS 会话 DynamoDB。

是的，还有一个包裹。

实际上，如果你看 AWS SDK，文档，然后你想导入它是 SDK，去斜线，服务斜线 DynamoDB 斜线 Dynamo DB 我的脸。

就留着这些包裹吧。

我认为我们应该做的是我们应该说去 mod tidy。

所以我们会得到我们刚刚谈到的所有的包。

通常，就像很多次一样，我会在视频结束时这样做，对吗？但是这一次，我只是提前做了，因为很多人会开始害怕，嘿，你安装了所有这些包？如果不是，你还没有运行命令 go 可能会走得更整齐。

或者你在打字的时候犯了很多错误。

为什么不安装一些扩展，我已经向你解释了为什么不安装扩展，对。

所以，对于 Golang，我的意思是，发生的事情是，你知道，我们通常来自 JavaScript 类的背景，然后我们认为，哦，你知道，如果你犯了所有这些错误，整个程序将崩溃，这将很难用 go Lang 解决它。

Golang 和其他编程语言很不一样吧？想犯多少错误就犯多少。

一旦你运行这个程序，Golang 会帮你处理所有的事情，告诉你哪条线有什么问题，它非常智能。

对吗？它不像 JavaScript 这样的常规编程语言。

在这里，我们遗漏了一件事，就是首先创建数据客户端，对吧，客户端有变量，定义数据客户端是什么类型的变量，它们将是什么类型的时间。

这就是为什么我们会用这个或者如果，对，我们会说点迪纳摩。

迪纳摩 DB。

API。

只要确保你做对了。

好吧。

这就是你的主文件，你的主文件是完整的。

现在你想开始处理处理程序。

所以在这里，由于这两个文件属于同一个文件夹，调用处理程序。

我们都想成为包裹处理员。

对吗？所以我们会说，包裹处理者是你的。

没错。

那很清楚。

现在，我希望在写完这个包之后，你说导入，然后你有你的主函数，在这个例子中，这个文件的主函数叫做 API 响应，接受一些东西，返回一些东西，有一个函数定义。

它接受什么接受状态，是 int 和 body，有一个接口。

它返回什么？它返回事件开始、API 网关代理响应、逗号错误。

好吧。

对于导入，你会说和编码斜线 JSON github.com/aws/headress, lambda Gqo 斜线事件。

好吧。

我们所做的基本上就是定义响应，对吗？所以我们说响应等于事件点 API 网关响应。

和响应，你只是设置标题。

相当标准。

因为我们要设置这些头的方式，我们已经见过很多次了。

基本就是内容类型和应用 JSON。

所以我们说我们从 lambda 函数返回 JSON，不要写任何不寻常的东西。

所以我们说响应点状态码等于状态。

所以基本上，你就像 400，或 300，或类似的东西。

然后你会说字符串体逗号 JSON 点马歇尔。

所以，如果你和 Golang 一起工作过，你就会知道它是编组，编组，编组对不起，Golang 本身并不理解 JSON。

所以它需要 JSON 编组的帮助，这是这个编码 slash JSON 包的一部分。

因此，无论何时你发送一些 JSON，从 postman 到 post postman，或者可能是 terminal，无论你在哪里发送一些 JSON，基本上我们都希望冷却剂能够理解它，以及 Golang 为它生成的信息成为 JSON，并作为响应发送它，这也需要，你知道，帮助，用船去 lang 本身。

这就是所谓的编组和解组。

关于它的视频有几百个。

在 YouTube 上，你可以查看一下，没什么特别的，也没什么复杂的。

你知道，你必须在几乎所有其他语言中这样做，对，不是 JavaScript，就像基本上，Java 和 Python，所有这些语言，你都必须这样做，对吗？因为任何不是 JavaScript 的语言都不理解 JSON，默认情况下，因为 JSON 是 JavaScript 对象符号。

这就是你的 API 响应。

好吧，就这样。

我是说，这份文件里没什么内容。

但是处理程序中的另一个文件，包处理程序，这是我们要花一些时间的地方，因为显然，这里会有很多事情要做。

因此，通常会出现的主要函数将基于我们的主 go 文件。

所以主要的，go 文件需要获取用户函数，创建用户，更新用户，删除用户和匿名方法。

未处理的方法，抱歉。

这些显然是你需要创建的函数，对吗？所以我们说获取用户。

他会说创建用户。

他会说更新用户和删除用户。

更新用户和删除用户。

好吧，给你。

我们想在这里保留的最后一个函数是未处理的。

方法。

对吗？比方说，如果有人使用 patch 方法，因为我们在处理，get POST，PUT，delete，如果有人使用 patch，那么我们会说，嘿，它没有被处理。

所以我们会说未处理的方法，除了一些东西返回一些东西，所以，函数定义。

好的，这就是，一般来说，你的句柄文件应该是这样的。

我们可以做的是我们的电子邮件验证，有效的文件，所以我们会说包验证器。

你可以从 go Lang 那里得到一个包。

这叫做正则表达式。

我们只是创建了一个简单的函数，我们只需判断电子邮件是否有效，非常简单。

它所做的只是接受一个电子邮件字符串。

并返回一个布尔值，如真或假。

我们将采用一个名为 Alex email 的变量，我们将使用 like 表达式，正则表达式包。

如果你看正则表达式文档，就像我面前的一样，去 Golang 官方文档并检查 dragon 正则表达式包，我强烈建议你打开它，因为这就是我在这里所做的，在我的另一个屏幕中将使用名为 must compile 的函数。

我将在这里复制并粘贴一个正则表达式字符串。

你真的不需要理解这是做什么的，你只需要直接从文档中得到它。

但是如果你真的想知道，基本上，你只是检查你是否知道这些数字在 A 到 Zed A 到 Z 之间，和 0 到 9 之间，诸如此类的东西，对吗？这就是它所做的一切。

它也在检查 at 符号。

没错。

但显然，如果它在任何地方，它需要有一个广告，对不对。

所以这些事情，只是基本的电子邮件验证检查正在发生，如果你想真正理解正则表达式做什么，请检查正则表达式文档。

但是如果你想节省时间，就像我刚才做的那样复制粘贴它。

现在我们想检查电子邮件的长度。

如果电子邮件，如果另一个女性没有三个，或电子邮件的长度超过 254 或 Arex，电子邮件变量，不匹配字符串。

那么你将返回 false。

对，所以这个要么返回真要么返回假。

如果这些条件不满足，那么你返回假，否则，你返回真，这意味着一切都很好。

电子邮件是有效的。

对吗？显然，如果这不合适，它就不能正常工作。

如果长度大于或小于，我们会说该邮件无效。

否则，我会对你说，你可以随意改变这些数字。

但我一直把他们保持在 3 岁和 254 岁。

好吧，在你的试卷上改一下。

现在，我们将转到 user.go 文件。

我们将添加一个 10，000 英尺的高度，我将创建这个文件。

现在，我希望发生的是，对于我的处理程序，像域控制，你知道什么将进入我们的主文件，你知道，这个函数，基本上。

而这个函数正在调用获取用户函数和创建用户函数更新用户。

如你所知，所有这些函数都在处理程序中提到过。

从处理程序中，您将调用 user.go 文件中的函数。

这些函数实际上是与数据库对话的数据库函数。

所以这里的每个函数，除了未处理的方法，因为它做的不多，我们有这四个 crud 函数，它们在你的 user.co 文件中有一个等价的函数，在你的用户 go 文件中有一个完整的一对一函数，与数据库对话。

好吧，那我们就这么做吧。

让我们创建这些函数。

首先，如你所知，当我们开始一个文件时，我们导入一些东西。

然后在这里，我还想定义一些变量，一些我想定义的误差。

我想在这里放的函数和我的 user.go 文件，与这四个函数有一对一的关系，第一个函数将被称为我称它为 fetch user。

这意味着我的 Get 用户函数被我的处理程序调用。

这个函数将调用 my user.go 文件中的 fetch user 函数，该函数直接从数据库本身获取用户，数据库在本例中是 DynamoDB。

好的，它接受一些东西，返回一些东西，有一个简单的函数定义。

我想要的第二个函数实际上是获取多个用户，所以我说 func 获取用户。

同样的，会看起来很相似。

然后我想有一个创建函数。

因此，我们将创建用户，然后更新用户，您只需继续查看这里，然后创建这些函数。

然后你有法律以及删除用户，对吧，所以你会到这里来对吧，如果它没有被删除，那么你不需要返回任何特别的东西，你可以只返回一个错误。

这就是为什么从这里返回只是一个错误。

对吗？大概就是这样。

所以我们设法实现了一个功能，所有这些功能，但是有一个额外的功能就是获取用户。

好吧，那么我们来看看在哪里得到在哪里使用它。

所以从 10，000 英尺的角度来看，这是你的 user.go 文件，这是你的 handlers 文件。

因此，我们要做的是，我们将从诺拉处理程序函数开始，然后同时，我们将开始我们的 user.co 函数。

在这里，很明显，你们会引入一些东西。

我首先需要的是 net slash HTTP。

然后我需要我的 lambda 事件。

所以我会说 github.com/ad/aws,拉姆达去斜线事件。

现在，有可能我正在进口这些，但是你不明白我为什么进口这些。

所以你可能会困惑。

所以现在有两种方法。

一种是当我实际编写代码时，然后我在某个地方使用这个事件包，然后我进来，你知道，导入它。

或者我知道，你知道我需要哪一个。

所以我会在一开始就导入它们。

这样他们以后就可以使用它们了。

所以我就这么做了。

所以请不要混淆，因为我们实际上将在两三分钟内使用这些软件包。

别担心，你知道我们为什么要进口它们，我们会重新利用它们。

所以请耐心等待。

斜线 AWS。

没错。

这些就是我们已经在 main.go 文件中使用的包。

我们会说 github.com/aws/check 它去斜线服务斜线，没有数据库斜线迪纳摩数据库我面对。

太好了。

这里我需要一件事，因为我想调用这些函数，就像我告诉你的，我想调用这个函数。

这就是为什么我要导入这个用户包和我的处理程序文件。

我该怎么做？我会说 github.com/achill/go,服务器很少写斜杠包，斜杠用户。

所以在用户包的内部，我们在这里输入它。

好的，在我开始之前，我想让这些函数定义一个非常变量，叫做不允许的错误方法。

不允许等于方法。为什么我需要创建这个？因为首先，我们要研究这个函数的未处理方法，对吗？这就是为什么你想调用这个函数。

在这里，它返回一个事件 API 网关事件。

所以它会说 API，网关代理响应，逗号，错误。

对，所以我们将从这里发送响应，或者它将发送一个错误。

这里我们说 return pay API response，你要发送的响应是 HTTP dot。

当我们说 HTTP 的时候，我们说的是历史包内斯塔历史包，好吧，状态。

方法，不允许逗号。

不允许 Header 方法，这是我们刚刚一起定义的，对吗？你就是这个意思。

你的意思是，我们有一些东西可以让 post 到位，但我们没有一些东西可以用来修补，所以修补或其他任何人想要使用的方法。

所以如果你得到类似的东西，比如一个未处理的句柄方法，你会发送一个响应，说，嘿，这个音表不是很响。

好吧。

我想创建另一个变量，但它实际上是一个结构体，对吗？所以它是一个误差体。

我会经常用到它，所以请原谅我为什么要创建它。

该结构有一个名为错误信息字符串的变量。

错误逗号，省略空。

现在是你的 get 用户。

你让他接受一些东西，在我得到用户之前，你把一些东西转化，你接受什么？你从邮递员那里得到一个请求，这个请求是一个储蓄包的一部分。

所以你可以说 events dot API 网关请求，逗号表名，也就是字符串，逗号，Dyna 客户端，dot Dynamo DB API。

它返回了什么？显然会返回响应。

所以我们会说事件，点 API 网关，代理响应，逗号，爱尔兰语。

直白。

实际上，这两个东西，对，将要接受，将要返回，将要被所有的函数使用。

因此，您只需复制并粘贴到此处，以便创建用户和更新用户。

也可以删除用户。

太棒了。

所以我们做了很多繁重的工作。

现在，我想开始研究这些函数的定义。

对，所以你要说的第一件事是电子邮件，请求点查询，字符串参数。

所以，如果你已经猜到了，这里发生的事情是你想获得用户，但通过使用该用户的电子邮件 ID。

所以对于我的请求，你将有你的查询字符串参数，它将传递一封电子邮件，我们将在一个名为 email 的变量中捕获它。

然后我们会检查长度。

所以我们说，如果电子邮件的长度大于零，那么你会得到一个单一的用户。

所以我们说 user dot fetch user，这是我们刚刚一起创建的，对吗？我们还没有创建定义。

但你知道我说的是哪个功能。

它在我的用户包中，已经被导入到这里了。

因此，fetch 用户将获取电子邮件、表名和捐赠客户端。

我会在结果中捕捉到这个，或者当有错误的时候。

如果有错误，我会处理。

所以我说如果误差不等于零。

返回 API 响应必须是点、状态、错误请求。

逗号或正文。

所以每个人都是我已经定义过的结构。

而在我们体内，我要通过 AWS 点串错误点错误。

否则，你会说你好，DB 点，一切都好。

所以我们会说地位。

好吗？如果一切顺利，你将会传递这个函数的结果。

现在，如果你想要多个用户，你可以说用户点取用户说同样的话，他们将命名一个客户端。

你要做的是，再次使用结果和误差，来获取这个函数的值。

如果有错误，你会检查错误是否不等于零，你会返回 API 响应，说 HTTP 点状态，错误请求，因为有错误，对吗？你将发送错误体，就像我们之前做的一样，错误体是一个我们已经定义的结构。

这就是我们要发送的错误体，基本上就是 JSON。

这里是误差体。

在里面，我们要发送，你要发送一个 WS 点串，头点。

明白了吗？但是如果一切顺利，没有错误，那么我们将返回 API 响应和 HTTP 点状态。

好吧。

商业结果如此，基本更容易得到用户。

现在我们可以做的是，在我们的 user.co 文件中，处理我们的获取用户和获取用户这两个函数。

或者，我们可以在处理程序中创建用户函数。

因此，我想我会做前者，我会在 fetch 用户上工作，我会在 fetch 用户上工作，让我检查一下是否一切都在记录，是的，一切都在完美地记录。

我只是需要继续确保，否则，你知道，我最终会在什么都没有记录的情况下制作一个很长的视频。

这导致了一个大问题。

好的，在我对用户做任何事情之前，我想先创建一个用户权限。

就像我说的，这个文件是你通常创建的模型文件的混合，在这里你定义了用户的结构。

所以我说用户结构。

电子邮件。

现在，因为这个用户结构很小，我们没有多种股票，例如，如果这是一个电子商务，那么你会有用户和订单，你知道，所有这些不同的产品和许多不同的股票。

所以你必须为所有这些不同的模型准备一个单独的模型文件夹。

然后你会有，你会有，你知道，数据库功能分开，和控制器或类似的东西，对。

但是因为我们没有，我们有一个非常小的项目，它只有用户和用户本身，它是一个非常小的结构，因为你只有电子邮件，名字和姓氏。

这里不用太多，对，可以把控制器和模型合并到同一个文件里。

现在，email 将会是一个表示 JSON 的字符串。

你会说，就像这封 JSON 邮件，因为对于 Gulags 来说，它会用大写的 E 发邮件，而存储它的 JSON，它会用小写的 E 发邮件。

所以你需要向这样的电信公司付费，你知道，它有两个版本，一个是 JSON 版本，另一个是 Golang 理解的版本，因为 Golang 不理解 JSON，我已经告诉过你了。

这就是为什么我们还必须使用一些编组和解组。

让它理解 JSON 并与之交互。

好吧，所以你会有名字和姓氏。

然后我们要做的是创建我们的获取用户函数，它接受什么。

因此，如果你真的去处理程序，你会看到 fetch 用户函数接受电子邮件表名给客户端。

所以我们在这里说，电子邮件，和表名，也就是字符串。

你的 Dinah 客户端，类型为，你已经知道 dB，I face dot DynamoDB API。

它只是返回一个用户。

很明显，我的意思是，你获取用户，你返回特定的用户或者返回错误，如果没有任何结果。

您定义了一个名为 input 的变量。

它的类型是 DynamoDB dot get item input。

因此，我们必须定义一个键，基于这个键，我们的数据库函数将运行来查找特定的用户。

您已经知道，用户将根据电子邮件 ID 形成数据库。

因此，我们获取一个电子邮件 id，您获取一个电子邮件 id，然后我们希望找到与该电子邮件 ID 相关联的用户。

好吧，就这么简单。

我们会说 DynamoDB 点属性值。

我们将说电子邮件，因为我们想对电子邮件进行查询。

所以 email 等于 s 等于 AWS 点串，点，不好意思，那个括号里面会传 email。

现在字符串是 AWS 包 Aerospike 给我们的函数。

这是我们必须从这里引进的东西。

好吧。

DynamoDB 也是如此。

所以我们还必须导入 DynamoDB 包。

所以在你的导入中，首先调用编码斜杠 JSON，因为就像我告诉你的，Golang 默认不理解 JSON。

所以你需要编码 slash JSON 包来使用类似编组和解组的功能，我也需要 errors 包。

然后我想亲自了解这些事件。

现在我们很快就会用到它。

发送回复和通常的回复。

如你所见，我需要 DynamoDB 和 AWS。

所以我们去拿那些吧。

所以我会说 AWS slash AWS SDK，去 slash AWS，还有 github.com/aws/aws SDK 去 slash 迪纳摩 DB。

我还会做得更好的是得到这个特别的包裹。

是的，让我现在做那件事。

所以我会说 github.com/aws/aws 是一个叫做斜线服务斜线，迪纳摩的例子。

DB 斜线迪纳摩 dB。

我面对。

好吧。

现在来到这里，你已经通过了这个。

在这个括号之后，您需要指定这个函数将要运行的表名。

所以我们将再次使用常规的字符串函数来传递表名。

在这个括号之后，您希望使用您的数据客户端最终开始获取项目。

输入这个基本上是你创建的查询，对而 get 项是你得到一个错误客户端的函数。

您将捕获最终结果一个错误。

所以，标准的做法是，如果有一个错误，我们会把它放在头上，很快返回值 nil，并返回 errors dot，新的错误是无法获取记录。

所以你一定想知道，这个错误是从哪里来的？大概没见过这个错误吧？因为这是我自己创造的新错误。

我怎么能制造我自己的错误，我能像这样制造我自己的错误。

因此，无法获取记录的错误等于无法获取记录。

类似地，我可以定义任何错误，我想把这个变量定义为变量，对吗？你知道，你已经创建了一个类似于 MongoDB 的查询，对，你已经创建了一个基于电子邮件的查询，因为你想搜索那封电子邮件，这样你就可以检索用户，你已经运行了函数 get in get item for DynamoDB。

你已经把查询传递给了它，你收到了一个叫做 result 的东西，或者你收到了一个错误。

如果有错误，你处理错误。

但是如果结果很好，什么都没发生，那么你会做一些正确的事情。

但在此之前，我们将创建一个名为 item 的变量，它基本上将是一个新用户。

而这里你会说误差等于迪纳摩 DB 属性. on

军事地图。

好了，DynamoDB 属性是另一个。

顺便说一下，如果你还没有打开 DynamoDB 和 AWS SDK，去，文档，这样做，因为我已经在我的另一个屏幕上打开了它，你也可以这样做。

然后一切都会变得更有意义，因为这些是主包中的实际包。

对吗？所以我用那些。

一旦你浏览了文档，你就会明白在哪里使用，哪一个，或者如果你真的不在乎，你可以继续跟着我做。

但我只是建议你只是读它，因为你知道，一切都会更有意义。

在这里，我需要这个包正确完成，我们属性是什么？这有助于我解组用户。

首先，让我来写完整的代码。

所以我会说结果，抱歉，不在这里。

他会每条记录都会说结果，点项逗号项。

如果这个误差不等于零，我们要做的是，我们要返回零逗号，新的误差没有出现在马歇尔记录上。

所以你一定想知道这里发生了什么，对吗？当您使用 get item 函数从 DynamoDB 中获取数据时。

您希望将数据解组到一个实际的用户中，前端基本上可以将其理解为一个 JSON。

所以你使用了用户结构和，你没有把它放在一个叫做 item 的变量里。

对吗？所以 item 基本上是一个 user 类型的变量。

然后，你知道，你想要来自你的结果或项目的数据，你想要它被解组并进入项目，所以现在无论什么作为 JSON 出现都变成，你知道，类型，这是 user，它被 go Lang 理解，但是它被一个叫做 item 的变量捕获。

对吗？因此，我试图向您解释每一行，以防您不理解，以检查什么是编组和解组。

好吧。

如果你还有任何困惑，你可以在下面的评论里提出来。

我给你整理一下。

但这并不是很难，对吧，我们只是从 DynamoDB，JSON 中获取信息，然后你对它进行编组，使它成为一个结构体，对吧？其中包含电子邮件名、姓以及 ko lang 能够理解的内容。

你在一个名为 item 的变量中捕获它，它显然是 user 类型的，对吗？令人震惊的是我们已经定义了。

这是标准的做法，我在我所有的其他视频中也这样做，如果你是这个频道的新成员，我有数百个关于 ko lang 的视频，你可以全部查看。

就像上百个关于可乐的视频，对吧？看看他们，和我一起做项目。

你会很容易理解所有的事情。

所以如果一切顺利，如果有一个错误，显然，你发送了零，你发送了错误。

但是如果一切顺利，你将返回这个项目，然后你将为这个错误返回 nil。

现在，您可能还想使用 fetch users 函数，即复数 fetch users 函数。

你是怎么做的，你在那里传递表名。

如你所见，在这里，你给他一个 ANA 客户。

所以 Duolingo 已经被传递了，这显然是一种类型的字符串来传递 10 ^ 9，一个类型的客户端，你已经知道，Dynamo DB，我面对 dot DynamoDB API。

并且返回多个用户。

当用户是你定义的结构时，你会返回一部分用户，对吗？所以你使用了一个结构，你对所有这些用户做了一个切片，然后这就是你要返回的。

我希望这有意义。

如果你不知道 like，Kulang，like 切片和结构的基础知识，那么我强烈建议你在变得更加困惑之前查看一下基础教程。

所以我想用 DynamoDB 给我的一个函数，它叫做扫描输入。

来访问一个表名，这个表名包含我们已经传递到这里的 AWS 点、字符串和表名。

没错。

他会说 Dinah 客户端，思想扫描，你要扫描输入，你刚刚创建的查询。

正如您所看到的，这里我们有一个更复杂的查询，因为我们必须获得一个特定用户的电子邮件。

但是通过获取用户，你只是得到所有的用户，你没有一个特定的查询，好的，你知道，对于这封电子邮件，得到一个特定的用户说，给我所有的用户。

因此，您没有任何这样的查询，您只是传递表名。

所以当你的用餐者客户，但你能只是通过输入，就是这样。

扫描就像得到一切，你知道，你可以这么说。

如果你用过 MongoDB，你可以找到所有类似的东西。

在这里，你只需要在 MongoDB 中扫描、查找并找到一个。

所以你只需要扫描，好的，扫描并得到它。

所以你做扫描来得到所有的结果。

你要把它保存在一个叫做 result 的变量里。

显然，如果你有一个错误，你知道处理错误的标准方法是这样的。

当你返回一个错误的时候，你会说 return nil。

她会说错误点新错误，无法获取记录。

再一次，就像我们在这里做的，项目将定义项目，对吗？这里的项目不仅仅是一个用户，而是一部分用户，多个用户，因为我们从数据库中获取所有用户。

再一次，和我们刚刚做的一样。

我们会在这里做同样的事情。

所以我们会说或者说实际上只是复制整个东西。

复制并粘贴。

但是，除了结果或项目之外，只有更多的一点点变化。

您将得到结果点项目。

因为显然从 AWS，你会得到多个项目，多个用户，这就是你得到的。

你要退回你的东西。

因为这个错误，你要回来。

有道理。

我们已经做了很多，对，我们已经做了获取用户函数，获取用户函数从你的 user.co 文件中获取用户和获取用户两个函数。

现在这里还剩下三个函数，出现三个函数是因为我们已经处理了未处理的方法函数，在这里你不需要再做什么了。

所以只剩下三个函数，三个函数。

所有其他的文件都很完整。

对吗？所以，如果你走到了这一步，祝贺你自己，因为你已经走了很长一段路。

对吗？所以现在让我们开始考虑我们的创建用户功能是如何工作的。

好吧。

对于创建用户功能，让我们开始构建吧。

那里实际上没发生什么。

所有这些功能，对吗？创建用户，更新，用户，删除用户，就像，他们看起来都非常非常相似。

所以要说 user，因为你知道，你要调用创建用户函数，用户包，所以我们说 USER 点创建用户，就是这个函数，这就是你要调用的，对吗？你传递给它三样东西请求，表名，和一个有意义的客户机。

没有人会响应这个函数的返回，你想在一个叫做 error，sorry，result 的变量中捕获它，你会得到一个错误，这个错误会被捕获。

你知道，从这里开始的过程，如果有一个错误，这意味着错误，不等于零，你将返回 API 响应，HTTP 点，状态，错误请求，逗号，错误正文和 URL 正文。

顺便说一下，这个例子中的每个人都是，不好意思，我们想要定义的结构，对吗？所以在正文中，有一个 AWS 点字符串，有一个单个的，或者不好意思，字符串，或者点错误。

逗号。

好吧。

您将返回 API 响应。

HTTP 点状态已创建。

逗号结果。

因此，如果这意味着如果没有错误，一切顺利，那么你会说，他应该点状态创建。

好吧。

仅此而已。

这就是了，实际上，这就是你的创建用户功能，没有别的了。

好吧。

然后为您更新用户功能，非常类似。

显然你会调用用户，就像你在这里做的一样，用户点创建，用户点更新用户方法。

所以我们会说，更新用户。

您将传递三个请求，表名和用餐者客户。

您将在结果逗号标题中捕捉到它。

然后你会再次检查，如果误差不等于零。

返回的 API 响应必须是点，状态，错误请求，逗号，错误体。

同样的事情，它是一个字符串或一个点。

好的，但是如果一切顺利，如果用户得到了更新，那么您将返回 API 响应。

你会说 SDP 点状态，好吗？一切顺利，你会返回结果。

这意味着正在发生的事情，这两个函数创建用户，更新用户，这两个函数正在被调用。

这意味着很多逻辑，主要的逻辑，会发生在这两个函数中，对吗？因为在处理程序中发生的事情不多。

类似地，让我们来看看删除用户函数。

给你。

你会说用户点读用户，插入请求，逗号，表名逗号戴安娜客户端。

这里，你唯一能从这里学到的是错误，对吗？如果用户没有被删除，你会转错误。

不需要从删除函数返回结果和结果。

因此，如果结果不等于零，您将返回 API 响应。

现在你知道我们要说什么了，我们要说的是错误的请求，我们还将发送包含 AWS 点字符串和错误本身的错误正文。

但是如果一切顺利的话，你会希望用 status 返回响应，好吗？对于这个错误，你将返回 0，好的，就这样。

是的，就是这样，我认为整个文件是完整的。

现在，我们没有别的事可做了。

我也浏览了 AWS SDK 文档，我不认为在处理程序中需要做任何其他事情。

所以在我看来一切正常。

现在，我们将不得不处理我们的 user of go 文件以及 user.go 文件中存在的所有这些不同的函数。

对于您的创建用户功能，我们所有它将接受的取决于您从这里发送的内容，这是请求一个初步的客户端。

所以在这里，你会说请求，事件点 API 网关代理请求，逗号表名，这将是字符串，然后是客户端，这将是你已经知道，可能有一个面对点纳度 API。

那么它将返回什么，它将返回刚刚创建的用户，或者一个错误。

在这里，我们要做的第一件事是创建一个变量，它的类型是 user user，这是我们将要定义的结构，这意味着你将拥有电子邮件的名字和姓氏。

好吧。

所以让我们从那里开始。

现在我们要用到你，为什么我们要创建这个变量，因为我们想捕捉到来自邮递员的信息。

比如说，从 postman 或从任何地方的终端都会发送用户的 JSON 以及电子邮件、名字和姓氏。

而且数据需要解组到您体内，以便 Golang 能够理解它并对它执行任何操作。

所以你要做的是我们要用 JSON 编码 slash JSON 包来调用解组函数。

你要做的是把请求体传递给它。

如果误差不等于零，用逗号与你符号，这里也一样。

他为 if 语句调用了正确的语法。

如果 error 不等于 null，那么我们将返回 nil，我们将返回一些错误。

我们会说错误，无效的用户数据。

但是我们这里没有这个错误，对吗？我们没有这个错误错误无法解组记录。

我们没有错误，无效的用户数据，我们只是有错误无法获取记录。

这就是我们所定义的。

现在让我们定义所有其他的错误。

让我来定义所有其他的错误，我将需要在这个完整的文件和开始本身。

所以我们会说错误未能解组记录等于未能解组。

记录错误和添加错误和有效用户数据等于有效用户数据。

无效电子邮件也将在那里，有效电子邮件等于无效电子邮件。

那么我们将有好的不封送项目，不能删除项目，所以。

那么我们将不能放置项目，所以我们会说不能发电机放置项目等于代码不是发电机放置项目。

并且用户已经存在。

用户不存在，或者用户不存在。

那么为什么我需要所有这些错误呢？显然，到现在为止，你应该已经明白了，我只是给你一个例子，当创建一个用户时，如果我们检查那个用户是否已经存在，那么我们不需要创建那个用户。

这就是为什么我们需要这种误差。

如果您需要，如果您正在更新用户，然后或删除用户，我们可以使用这个，该用户不存在。

所以我试着更新那个用户，对吗？这就是为什么我要考虑所有类型的错误函数，我需要错误语句。

基于此，我刚刚创建了它们。

回到您的创建用户，此时此刻，我们将开始验证电子邮件。

所以我们说验证器是我们一起创建的包，对，就是这个。

我们在那个包中创建的函数是他的电子邮件有效。

你就是你定义的变量。

现在在解组之后，你在 body body 中的请求是你从比如说 postman，或者从终端 JSON 得到的请求。

你抓住了这一点。

所以现在你现在可以走了，郎很容易理解你，因为它是反编排的，对不对？它不只是一个或多个，所以你可以访问你的电子邮件。

你可以在上面运行验证功能。

所以这里的拼写是错误的。

我应该什么时候电子邮件有效？你将返回零，逗号，错误，点新的错误，无效的电子邮件。

对吗？我创建这个错误的原因是因为我想知道这个用户是否已经存在，对吗？因此，如果用户已经存在，那么您不需要创建它。

所以我们需要抛出一个错误。

所以我们将检查用户是否已经存在，对吗？所以我不会发表评论，实际上，我只是想给你看。

所以要检查用户是否真的存在，你得运行 fetch user 函数，你会说 u 点男逗号表名，逗号 Diana 客户端。

并在当前用户中捕获来自该函数的任何内容，我们不会处理该错误，因此我将在此处留空。

所以，如果当前用户不等于空，这意味着有这个用户存在，对不对？如果当前用户不等于空，并且当前用户点电子邮件的长度不等于零，则返回零逗号，错误点新错误用户已经存在。

如果用户存在，所有这些都会发生，但是如果用户不存在，他只会说用户正确，那么如何保存呢？所以首先，为了挽救它，很明显，不管你现在有什么，你想开始整理它，对吗？这样 DynamoDB 现在就能理解了。

所以你会说迪纳摩数据库，属性点马歇尔地图，你，你会在 AV 中捕捉这个，你会检查错误。

所以如果有错误，我们将返回零。

新错误不能封送你已经定义的项目，我们已经定义了这个错误，对吗？现在，您想要开始创建您的数据，并将这些数据发送到 DynamoDB。

那么你会怎么做呢？你会说 DynamoDB 点放项输入项是 Av。

逗号。

表名是 AWS 点，字符串表名。

最后，您将看到 Dana 客户端点放项目，您将发送您在此定义的输入。

所以是的，错过了就等于签错了。

类似于获取用户，对，您创建了这个输入，然后您调用 ANA 客户端函数。

类似地，你在这里做同样的事情，你调用 put item 函数。

你会在一个空的 actors 变量中捕捉到它，或者，或者，你会从这里得到一个错误。

如果有错误，你会很容易处理。

你会说 return nil，comma，errors dot new error，not timer put item right here 这个错误已经消失了。

但如果一切顺利，你只想返回用户是零。

太棒了。

这就是你的创建用户功能。

现在只剩下更新用户和删除用户了。

你所关心的其他事情都是正确的，处理程序是 go 文件是完整的 API 响应是完整的，就像电子邮件验证器是完整的一样，可能不完整。

我认为对于我们的用户文件，除了验证器之外，我们已经导入了所有的包。

所以你需要验证器。

所以你会说 github.com，你已经创建的验证器包？我们谈我说的是那个。

所以我们会说去无服务器 yt，斜线 PKG 斜线验证器。

它还会检查对于处理程序来说我还缺少什么？我想我已经导入了用户。

我不需要更多的包裹了。

用于 API 响应。

我不需要更多的包裹。

对于他的电子邮件有效，我不需要其他任何东西。

我的 main.go 文件，可能，我可能漏掉了什么。

因此，它有处理程序，甚至总是抨击 AWS 会话 DynamoDB DynamoDB，所以一切都是正确的，基于 AWS SDK，去吧，看看这个文档，你会知道我为什么要导入这些包，以及我如何使用这些函数。

你已经知道了，因为你一直在和我一起建造它，好吗？

然后，我们会来到这里，再次回到我们的 user.co 文件。

让我们开始研究更新用户功能。

那么它接受什么呢，接受事件类型的请求，认为 API 网关请求，逗号，表名，字符串逗号，Dinah 客户端，DynamoDB 类型，我面对 dot DynamoDB。

API 向用户返回更新后的用户。

或者很好，我是说，或者它返回一个错误。

我们将做许多与创建用户功能非常相似的事情，基本上就是我们创建用户。

首先我们创建了用户，右，这是变量 u，它的类型是 user。

我们将解组，就像我们之前做的那样，我们将解组您获得的请求体。

所以要求点号体，逗号和符号你错了，所以我必须在这里把括号括起来，因为这是在一起的。

这是一个可变的视图。

好吧。

还有她的航海隧道，如果你不想跟着走，可以直接复制粘贴这部分，对吧？我没有复制和粘贴，因为这种语法对许多人来说可能是新的。

因为它是 DynamoDB，有点像与 DynamoDB 一起工作。

所以我都是手写的。

如果你想复制和粘贴，如果你对 DynamoDB 很熟悉，那就去做吧。

你不需要跟我一起练习。

然后，您想获取用户并查看该用户是否存在。

所以你会说你的邮箱逗号，表名逗号，数据客户端。

您希望在当前用户添加支票或当前用户时捕捉到这一点。

因此，如果当前用户不等于零，这也正是您在 create 函数中所做的。

并且当前用户点电子邮件的长度等于零，那么您将返回 nil 逗号错误点 nao 错误，用户不存在。

因此，对于创建用户功能，我们正在检查与该电子邮件相关的用户，因为我们希望看到，你知道，这是真正存在的，我们不想添加该用户。

但是对于 update，我们正在做相反的事情，我们正在检查那个用户。

因为只有当那个用户存在时，我们才能为那个用户更新数据有意义吗？同样的事情也会发生，因为现在你想开始了。

所以现在用户，你知道，如果用户不存在，就会抛出一个错误。

但是如果用户存在，那么您希望开始用新数据更新表。

你是怎么做到的？无论您现在解组了什么，从 JSON 到解组，到 Golang 能够理解东西，现在您都希望开始将它封送到 DynamoDB 能够理解的东西。

所以你已经知道你要用什么，你要用编组图功能。

把它递给你，因为你刚刚完成了。

它是 Dynamo DB 属性包的一部分，我们将在一个名为 AV 的变量中捕获它，否则我们会得到一个错误。

现在我们可以轻松地处理错误。

因此，如果错误不等于零，返回零逗号，错误点新的错误不能封送它，这一个。

非常非常简单。

现在您只想创建您的输入项，然后您只想调用它和一个客户端函数，只剩下两个步骤。

所以你会说 input 等于 Dyna，更 dB，dot 放 item input，而 item 等于 Av 逗号，表名就等于 AWS dot string。

表名。

最后一件事是，你要用 Diana 客户端点 put 项，你要传递输入，如果错误不等于零，零逗号错误点 new error 不能发生 put 项返回&你来吧。

很快，现在连我的更新功能都完成了，一切看起来都很好。

我现在想做的就是运行 Gomati 命令来获取所有不在那里的包。

所以我想这需要一段时间，反正我找不到我的手机。

因此，当这在后台发生时，我们可以开始处理删除用户功能。

如你所见，这都是安装好的。

实际上我有几个问题。

正如你所看到的，大多数问题是因为我在这里有几个拼写错误，而不是 SDK 加了一个 S KD，所以我犯了一些小错误。

一定要把这些系紧。

你可以很容易地在 AWS SDK go 文档中找到它们，或者如果我把大部分代码放在 GitHub 上。

所以你可以从那里拿起它。

好吧，有道理吗？只是从那里拿起它，而不是自己输入，因为我犯了几个错误。

所以这需要一段时间。

但是现在它安装了所有这些软件包。

那么，你的 user.go 文件，一切都工作正常吗？就像我们不知道它是否完美，但在我看来一切都很完美。

所有的包裹都准备好了。

你现在需要做的就是处理你的 read user 函数，这实际上是最简单的函数。

所以它需要请求，事件点 API 网关请求，逗号表名，这是字符串客户端类型，这是一个已经知道的类型，你已经知道了。

dotadynamodbapi。

好吧。

所以你想在函数定义里放什么，你想要用户的邮箱，对吧？因此，请求点，查询字符串参数，和参数，电子邮件，然后您将创建您的输入到函数的类型 DynamoDB。

输入删除项，项，输入星点属性。

价值。

基本上，您传递电子邮件，然后找到用户并删除与该特定电子邮件相关联的用户。

对吗？没什么复杂的。

所以你会说 AWS 点串。

你发邮件给那个人。

在这之后，你会说表名。

AWS 点字符串，传递表名。

现在您已经准备好了输入，就像我们已经完成的所有其他函数一样，您使用数据客户端，调用 DynamoDB 函数，在本例中是 delete item，然后将输入传递给它。

put 基本上有您要删除的查询，也就是电子邮件 ID。

你会得到错误。

如果有错误将返回错误点新的错误无法删除项目。

完美。

否则，我们只返回 null，什么也不返回。

基本上，这意味着用户已经被删除。

现在，我肯定有很多错误。

因为我没有使用任何类型检查扩展或任何 go Lang 扩展。

所以这里会有很多错误。

因此，在将它部署到云之前，我需要开始逐一解决它们。

因此，为了找到这些问题并解决它们，我将前往我的终端。

我会转到 CMD 文件夹，有一个共同建设 main.co，并开始给我错误，对不对？文件上写着，第八行。

是的，我能看到这个问题，它必须加倍。

没错。

这就是它成为 or 运算符的原因。

因此，您将再次运行该命令。

现在你将得到我们所期待的所有这些漂亮的标题，对吗？现在我们要开始一个一个地解决它们。

所以从第 37 行开始，一直到第 145 行有太多的错误。

所以当它说错误太多的时候，意味着即使你解决了这些，它也会给你更多的错误，对吗？这就是为什么我不使用扩展，因为 Golang 会处理所有的事情，它会告诉你哪一行哪一部分，你错过了什么，你不喜欢这是一个简单的问题，对吗？你不需要动很多脑筋就能解决这些问题，就像围棋一样，一旦你做到了，所有的事情都会迎刃而解。

所以我一直都很放松，伙计。

所以如果我回到你的代码，在第 37 行，问题可能是我没有在这里放一个逗号，在这里放一个逗号。

基本语法问题，对吧？这就是它所说的基本语法问题。

这里你也需要放一个逗号。

让我们继续把逗号放在你知道的地方，我认为它需要逗号，所以这里需要逗号，这是第 58 行。

在我的情况下，你可以在你知道的地方查看，这是为你准备的。

为了再次创建用户，我将在这里放一个逗号以确保安全。

我会在输入部分加一个逗号。

好的，在更新用户函数中。

还是那句话，我们去输入区。

在这里，我已经把逗号或没有看到任何问题在这里。

对于删除，你肯定需要在这里，这里，还有这里，放一个逗号。

所以现在如果我们运行它，很多错误都消失了，对吗？现在你有一些其他的错误。

也有一些语法错误，比如语法错误。

这个是你不能引用这个函数。

因此，让我们同时尝试解决这些问题。

所以让我们回到我们的代码。

让我看看第一行是 88，第 88 行，等等，是的，M 很小，而 M 应该很大。

我在想什么？我是不是很蠢，你知道，我不能？我没有把它们做得很大，因为很明显，这个函数是大写 m 的军事地图。

对吗？所以这只是意味着，不管你有多蠢，去吧，郎会照顾一切，对不对？所以你不需要成为这个星球上最聪明的人。

所以让我们看看别人愚蠢的错误。

所以少了 108 个逗号？这是语法错误。

超级简单吧？它甚至告诉你期待逗号，对不对？我是说，编辑语句能有多明显？然后人们，你知道，告诉我下载这个扩展？我，我没有灭绝人类。

Go Lang 为我做了一切。

不知道我是否想知道我是否错过了你的陈述？是啊，很明显，生理信号消失了。

然后 147 147。

在这里放一个逗号，就在 147 后面。

然后出乎意料。

没错。

找到了。

现在让我们运行它。

现在。

让我们看看更多的错误。

是的，所以它发现了更多的错误。

现在你必须翻到第 66 行，第 76 行和第 140 行。

但这些不是语法错误。

所以在这里，你必须思考一下，为什么会有错误。

所以回到你的在线代码。

66.

第 66 行。

是的，所以它说，让我们看看它说的错误，不能使用结果点结果或项目。

和这个叫做元帅图的函数有关的东西。

那是因为你收到的是物品，而不是物品，你收到的是多件物品，对吗？所以你不能在这里使用解组映射函数，你只会看到使用解组映射列表。

超级简单，伙计，超级简单。

36，第 76 行。

好吧，它说什么，什么，什么，什么，什么，请求点体。

看到它说请求点体未定义。

它还说没有大写的 body。

对吗？我的意思是，你不需要，你可以是这个星球上最愚蠢的人来解决这个问题，因为你必须把它变成资本。

对吗？这就是郎的直觉。

然后你转到第 140 行。

140 号线。

我的意思是，我可以向你保证，你不能用任何其他语言做类似的事情，对吗？它说这个函数有问题，很明显，因为这里有一个拼写错误。

参数。

现在一切都解决了。

现在让我们看看。

这又给了我一个错误。

上面写着解编。

从地图上消失是错误的。

是，因为我犯了错误。

同样，它应该是地图解组列表，而不是地图。

太棒了。

现在，你的 user.go 文件被排序为一个，它首先向你显示语法错误，你已经解决了它们，然后你解决了逻辑错误。

现在它给你一些关于处理程序文件的问题。

它给你所有这些语法错误，我们会解决它们，我们会给你一些逻辑错误。

我们也会解决它们。

然后我们就可以走了。

对吗？所以我们走在正确的道路上。

现在让我们回到我们的代码。

所以每当你的代码从第 19 行开始，第 19 行，在这里放一个逗号，在这里放一个逗号，它在说它接受你的逗号，对吗？和管线 54。

逗号，第 59 行等等，等等，等等，等等。

第 62 行。

60 和 72 和 66 为逗号 32，逗号。

好吗？现在你开始得到真正的错误，对，真正的错误。

所以，第 22 行，我们刚刚在另一个文件第 22 行解决了这个问题，因为参数的拼写是错误的。

现在你要担心第九行，第九行。

因此，需要注意的一件事是，这是第九行，但另一个文件 API 响应会是。

于是打开 API 回应，转到文件。

这里我看到了一个问题。

好了，我已经解决了这个问题。

这基本上与那些支架有关。

现在它开始向我显示可能无法归档的问题，这正是我想要的。

我想解决 main.go 文件的问题。

但是在第 44 行，第 44 行。

让我看看发生了什么事。

好吧，是的，我看得出来，我看得出来，基本上所有的案子都得放在一起。

错误的是，我把它写在了括号外，也写在了开关括号外，我把它包含在了括号内。

所以这个错误也应该消失。

现在，当我这样做时，它为我创建了目标构建可能不会归档。

让我看看它有没有，我可以看到主文件，我只需要把它移到我的构建文件夹中。

我已经点击了构建文件，没有任何问题，没有错误。

现在让我们开始部署过程。

在做任何事情之前，您需要首先创建一个 zip 文件。

所以你只要来到这里，它会说 zip 减去 gr M，并建立斜线主点 zip。

从建立斜线主。

所以 build 是你的文件夹 main 是你为任何文件夹创建的 build，build slash main dot zip 是使用 zip 函数的 zip 文件。

也许在你的 Linux 中，你没有 zip，你必须使用 sudo 来安装它。

获取安装 zip。

现在，如果我们检查这里，我们可以在我的构建文件夹中看到主. zip 文件。

现在我们准备开始上传到 lambda，或者 ADA 加 lambda。

但正如你们所记得的，我告诉过你们，我在上面安装了 Windows，我在上面运行 WsL，我也在上面运行这个 open。

在此之上，我建立了这个文件夹，对吗？因此，我需要能够从我的 windows 访问这个主要的点 zip 文件，以便我的 windows 能够安装，比如将它上传到我的 AWS 代码和 lambda lambda 仪表板上。

为此，我会做点什么。

为了帮助我做到这一点，我必须运行这个命令 explorer . exe。

点。

现在希望它能打开。

是的，它已经为我打开了这个位置的浏览器，我将复制这个。

我会把这个放在我的桌面上。

太棒了。

现在我可以开始在我的 AWS 控制台上部署它了。

所以你想开始部署，但有一件事我想改变的是这个表名。

所以我将保持这个表名和我的程序名一样。

右拐无服务器。

好的，只是为了一致性，因为我们必须在 DynamoDB 中创建这个表。

让我搜索一下。

如果我使用了另一个表名，那么你只需控制 Zed。

我们会看到我是否在其他文件的其他地方使用了这个表名。

我只在一个地方用过它。

所以这里的表名必须更改。

它会说去服务器少写。

好的，现在我们开始部署。

所以登录你的控制台，前往 lambda。

您只需创建函数 go server less writing server using Golang one point x 我认为我们必须更改执行角色从 AWS 策略演示创建新角色此选项是您必须选择的，并且滚动名称，go server less，right 从模板执行，您必须选择简单微服务权限创建函数成功创建，对吗？现在你想改变处理程序。

如你所知，我们要上传的文件叫做主文件。

这就是我们的建筑所谓的权利。

因此，我们必须更改处理程序，意思是上传 zip 文件并上传，然后返回 AWS 并转到 DynamoDB。

你可以点击创建表格表格名称表格名称是你在这里选择的去服务器少白。

简单吧？因为我们保留了相同的名称和主键。

所以它说输入分区键名，我想这是主键这里是主键。

我们案例中的主要关键是电子邮件。

显然是字符串。

其他一切都一样。

我们可以创建活动数据库，它现在是活动的。

现在我们可以进入下一个阶段，即配置您的 API 网关。

因此，请前往您的 API 网关。

创建 API。

现在来构建一个 REST API。

协议，让我们在这里测试选择新的 API。

现在你想把 API 的名字，这很酷。

无服务器 ID。

然后创建 API。

从要创建的操作方法中，选择任何。

在这里，您将选择积分型 lambda 函数。

并使用 lambda 代理集成对此进行检查。

这里有 lambda 函数的名字，就是 go 无服务器 YT。

您还必须确保这是选中的默认超时。

现在我们需要部署我们的 API。

因此，从“操作”“部署 API”中，选择“新阶段”、“转移和部署”的阶段名称。

这是你在这里得到的 URL。

我们现在要用它来测试。

所以让我们来测试一下。

我，我预计会有一些错误。

但不是问题。

让我们清空屏幕。

首先，我们必须构建这个命令。

所以给我一点时间。

这是我的四个命令。

显然，这是创建用户的 post，这是获取所有用户，通过电子邮件 ID 获取特定用户。

最后一个命令用于更新。

现在我们可以用 postman，或者你可以用 curl，它们是一样的，对吗？你设置你的标题头，你告诉它是请求，是发布，你发送的数据是电子邮件，是我的电子邮件地址，我的名字，我的姓氏，这是我刚刚得到的唯一链接，这个链接是我刚刚从我的调用 URL 得到的唯一链接。

好的，我会把这些贴上来，我会把这个命令贴在某个地方，我怎么贴呢？现在吗？好的，我会把这个项目上传到 GitHub 上，然后在描述中，我想我会发布这些命令，这样你就可以快速地复制和粘贴它们。

我认为那将是最好的选择。

如果你不想使用这些命令，你也可以使用 postman。

好吧。

同样，对于更新，你说这是请求，但你有新的数据将基于此找到电子邮件，因为电子邮件是我们的主键，正如你已经知道的，这是我的 API 链接将遵循的命令，我还需要一个命令，这是删除命令。

所以我把那个改成这个。

因此，您可以在和 API 网关文档中找到关于如何调用这些 API 的所有信息，您也可以找到这些信息。

所以我只是复制那些，我只是改变那些命令。

好吧。

所以我们现在要做的是尝试并测试它。

我很确定我们会出错失败。

但不管怎样，我们还是做吧。

哦，这么说成功了。

成功了。

好吧。

所以它创造了这个用户，很神奇。

餐厅第二司令部。

对不起，这个命令不是复制粘贴。

现在返回获取所有用户命令。

我们会得到所有的用户，只有一个用户是 S，它得到了那个。

然后我们会得到特定的用户。

特定用户有这个电子邮件 id，它也会为我得到那个用户，因为现在只有一个用户。

是的，所以那也是有效的。

现在我们将研究 put 函数。

太恶心了，它拿走了我的电子邮件地址，把我的名字改成了 lalala，把我的姓改成了 blah，blah，blah，blah，对吗？所以你的更新工作已经完成了。

现在我们必须处理删除函数。

所以如果 delete 函数起作用，它不会返回任何东西。

因此，我们将检查该用户是否仍在那里，因此我们将获取所有用户。

这意味着删除工作，因为它是一个空数组，对吗？那个用户已经不在了。

完美。

这意味着一切都正常。

我希望你喜欢这个教程。

这是一个很长的视频，我知道有很多后续。

那里有很多东西要学。

我希望你真的喜欢它。

我希望你在这个项目中学到了很多。

很快，我将会有一个项目推出无服务器标签，但它更高级一些，我们将有多个 lambda 函数，我们将有 CloudFormation，我们将有那些 CloudFormation 脚本。

所以一切都要通过 YAML 文件，然后是 AWS CLI。

我们还将在整个设置中使用 AWS 和 Sam。

所以这将会非常复杂。

因此，请确保您已经完成了 AWS lambda，了解该视频，并且已经完成了无服务器视频。

然后，您会进一步了解无服务器的工作原理以及所有不同的技术。

这将在接下来的视频中对你有所帮助。

实际上会有更长的时间，他们会有更复杂的项目。

这就像是一个整体的无服务器项目，对吗？那将是无服务器微服务项目，将会有 API gateway CloudFormation。

它们将会是更多的技术。

我将使用 AWS，Sam 来编写所有的配置文件。

无论如何，所以我希望你喜欢它，如果你还没有订阅的话，请订阅这个频道。

在我的频道上有数百个关于 go Lang 的视频，请查看。

我也一直分享一些关于你的职业发展的好建议。

所以请继续关注。

好的，再见，在 LinkedIn 上和我联系。

下面我的描述框里有 LinkedIn 的链接。

谢谢你。