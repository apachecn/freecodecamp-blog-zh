# 如何使用 Gettext 在 Phoenix 应用程序中执行本地化

> 原文：<https://www.freecodecamp.org/news/how-to-perform-localization-in-phoenix-applications-with-gettext-c38bb0f01bef/>

安娜斯塔西娅

# 如何使用 Gettext 在 Phoenix 应用程序中执行本地化

![YeVWrTAtoLW1vnKlFenMJ2vfDvwgi-SwAz5d](img/ffa417b3a81ffc33f0de8b55bded72b7.png)

在我之前的教程中，我们讨论了如何在 Rails 应用程序中引入对 I18n 的支持。今天我们将继续讨论后端框架，并在 *Gettext* 的帮助下讨论凤凰应用的*本地化。*

你可能以前没听说过凤凰城，所以让我说几句。这是一个用 [Elixir](https://elixir-lang.org/) 编写的服务器端 MVC 框架，这是一个工作在 [Erlang 虚拟机](https://en.wikipedia.org/wiki/BEAM_(Erlang_virtual_machine))上的函数式编程语言。这个框架本身还很年轻，但由于 Erlang 和 Elixir 的特性，它仍然很有前途。它非常快，可伸缩，面向并发，这对于负载很重的应用程序非常重要。

Gettext 是一个由 GNU 维护的 I18n 工具，可以用于网络、桌面应用，甚至是操作系统。

在本文中，我们将本地化一个 Phoenix 演示项目，并将看到 Gettext 的实际应用。此外，我们将讨论如何在整个请求中引入对区域切换和持久化用户偏好的支持。在进入本教程的主要部分之前，您可能还想了解一些常见的建议[，这些建议在我们最近的文章](https://blog.lokalise.co/localization-5-focus-points/)中列出。

### Hello, Gettext!

好了，让我们直接进入代码，观察 Phoenix 应用程序在实践中的本地化。创建一个没有默认 DBMS 的新项目，并将目录更改到项目中:

```
mix phx.new lokalise_demo --no-ecto cd lokalise_demo
```

似乎 Phoenix 已经支持 Gettext 开箱即用:你不需要安装任何第三方库。此外，如果您导航到`demo/lib/demo_web/templates/page/index.html.eex`文件，您会注意到以下代码行:

```
<h2><%= gettext "Welcome to %{name}!", name: "Phoenix" %></h2>
```

这是怎么回事？嗯，`gettext` [是一个函数](https://hexdocs.pm/gettext/Gettext.html#gettext/2)，它试图为字符串`"Welcome to %{name}!"`加载翻译。`%{name}`这里有一个[占位符，它将被第二个参数`name: "Phoenix"`指定的`"Phoenix"`字符串替换](https://hexdocs.pm/gettext/Gettext.html#module-interpolation)(该参数包含所谓的*绑定*)。

默认情况下，Phoenix 应用程序将英语作为默认区域设置，不支持其他区域设置。但是，您可以通过在`config/config.exs`文件中添加新的一行来轻松地更改它:

```
config :lokalise_demo, LokaliseDemoWeb.Gettext, locales: ~w(en ru)
```

现在我们支持英语和俄语两种语言环境。

下一步是为传递给`index.html.eex`文件中的`gettext`函数的字符串提供翻译。最简单的方法是将所有翻译字符串自动提取到单独的文件中:

```
mix gettext.extract mix gettext.merge priv/gettext mix gettext.merge priv/gettext --locale ru
```

这些命令将在`priv/gettext`文件夹中创建三个新文件。因此，让我们停下来，多谈一谈这些文件。

### Gettext 文件类型

上面的第一个命令`mix gettext.extract`，搜索所有需要翻译的 Gettext 消息，并将它们放到`priv/gettext/default.pot`文件中。POT 的意思是“可移植对象模板”，这样的文件可以作为特定语言翻译的模板。我们的`default.pot`有以下内容:

```
## This file is a PO Template file. ## ## msgid here are often extracted from source code. ## Add new translations manually only if they're dynamic ## translations that can't be statically extracted. ## ## Run mix gettext.extract to bring this file up to ## date. Leave msgstr empty as changing them here as no ## effect: edit them in PO (.po) files instead. msgid "" msgstr "" #, elixir-format #: lib/lokalise_demo_web/templates/page/index.html.eex:2 msgid "Welcome to %{name}!" msgstr ""
```

该模板方便地显示了提取的消息所在的行。`msgid`是要翻译的字符串(有些开发人员可能称之为“key”)。`msgstr`当然是实际的翻译。

POT 文件的名称——`default`——也是作为名称空间的 [*域名*](https://hexdocs.pm/gettext/Gettext.html#module-domains) 。最初，只有一个名称空间，但是对于具有数百种翻译的较大站点，创建多个域并因此将翻译分离到不同的文件中可能是一个好主意。

`mix gettext.merge priv/gettext --locale LOCALE_CODE_HERE`命令根据模板为给定语言创建[翻译文件](https://hexdocs.pm/gettext/Gettext.html#module-translations)。这些翻译文件的扩展名为“T1”，位于“T2”文件夹中。请记住，为了提供消息的翻译，您应该编辑这些 PO 文件，而不是直接编辑模板！

### Gettext 域

如上所述，Gettext 支持多个域或名称空间。当你使用`gettext/4`函数时，你总是假设一个`default`域。如果您想使用不同的名称空间，请使用`[dgettext/6](https://hexdocs.pm/gettext/Gettext.html#dngettext/6)` [函数](https://hexdocs.pm/gettext/Gettext.html#dngettext/6)，它接受域、消息、可选绑定和其他一些参数:

```
<%= dgettext "custom_domain", "message is ${placeholder}", placeholder: "my binding" %>
```

现在，`mix gettext.extract`命令将创建一个新的`custom_domain.pot`文件。类似地，运行`mix gettext.merge`会基于模板创建一个`custom_domain.po`文件。

再次注意，对于较小的站点，使用多个域通常是一种过度的做法。尽管如此，还是强烈推荐将它们用于大型资源，因为这样你就不会在一个文件中有上百个翻译。另一个原因是能够在不同的名称空间下拥有相同的翻译键。

### 提供翻译

因此，在讨论了一些 Gettext 的内部内容之后，我们现在可以将`Welcome to %{name}!`字符串翻译成俄语(这个消息已经是英语的了，所以当然不需要翻译成这种语言)。像这样修改`priv/gettext/ru/LC_MESSAGES/default.po`文件:

```
# ... some other stuff goes here ... #, elixir-format #: lib/lokalise_demo_web/templates/page/index.html.eex:2 msgid "Welcome to %{name}!" msgstr "Вас приветствует %{name}"
```

就是这个！目前我们没有任何切换语言的机制，所以将俄语设置为默认语言环境:

```
# config/config.exs config :lokalise_demo, LokaliseDemoWeb.Gettext, locales: ~w(en ru), default_locale: "ru" # <== modify this line
```

现在，通过运行以下命令启动服务器:

```
mix phx.server
```

在浏览器中打开`http://localhost:4000`页面，确保显示翻译后的信息！

### Gettext 多元化

我想介绍的另一个重要特征是[多元化](https://hexdocs.pm/gettext/Gettext.html#module-pluralization)。不同的语言有不同的多元化规则，Gettext 支持许多现成的规则。尽管如此，我们的工作是为所有潜在的案例提供适当的翻译。

举个很简单的例子，比如说用户有多少个苹果。假设我们不知道确切的数量，这意味着句子可能读作“1 个苹果”或“X 个苹果”。为了支持多元化，我们必须坚持使用`[ngettext/5](https://hexdocs.pm/gettext/Gettext.html#ngettext/5)` [函数](https://hexdocs.pm/gettext/Gettext.html#ngettext/5):

```
ngettext "You have 1 apple", "You have %{count} apples", 2
```

该函数接受句子的单数和复数形式，以及`count`。在引擎盖下，Gettext 利用这个计数并根据多元化规则选择适当的翻译。

接下来，您可以使用以下命令更新 POT 和 PO 文件:

```
mix gettext.extract --merge priv/gettext mix gettext.extract --merge priv/gettext --locale=ru
```

您会在 Gettext 文件中发现几行新内容:

```
msgid "You have 1 apple" msgid_plural "You have %{count} apples" msgstr[0] "" msgstr[1] ""
```

`msgstr[0]`和`msgstr[1]`分别包含单数和复数形式的翻译。对于英语，我们不需要做任何其他事情，但是俄语需要一些额外的步骤:

```
msgid "You have one message" msgid_plural "You have %{count} messages" msgstr[0] "У вас одно яблоко" msgstr[1] "У вас %{count} яблока" msgstr[2] "У вас %{count} яблок"
```

这种情况下的多元化规则有点复杂，因此我们必须提供三种而不是两种可能的选择。你可以在官方文件中找到关于主题[的更多信息。](https://hexdocs.pm/gettext/Gettext.Plural.html)

### 选择应用程序的区域设置

正如我之前提到的，目前浏览应用程序时没有办法在不同的语言环境之间切换。这是一个重要的特性，现在就来添加吧！

总而言之，我们有两个潜在的解决方案:

*   利用第三方解决方案，例如 [set_locale](https://github.com/smeevil/set_locale) 插件(简单的方法)
*   从头开始写一切(战士之路)

如果你选择坚持使用第三方插件，事情会变得非常简单。您只需要执行[三个快速步骤](https://github.com/smeevil/set_locale#setup):

1.  安装软件包
2.  向`router.ex`文件添加一个新的插头
3.  添加新的`:locale`路由范围

之后，将从 URL、cookies 或`accept-language`请求头中推断出[区域设置](https://github.com/smeevil/set_locale#fallback-chain-and-precedence)。简单。

然而，在本教程中，我建议选择一种更复杂的方式，从头开始编写这个特性。

### 从 URL 读取区域设置

指定所需地区的最常见方式是通过 URL。语言代码可以是域名的一部分，也可以是路径的一部分:

*   `[http://en.example.com/some/path](http://en.example.com/some/path)`
*   `[http://example.com/en/some/path](http://example.com/en/some/path)`
*   `[http://example.com/some/path?locale=en](http://example.com/some/path?locale=en)`

让我们坚持后一种选择，并以 GET 参数的形式提供 locale。为了读取区域设置的值并做些什么，我们需要一个[自定义插件](https://hexdocs.pm/phoenix/plug.html#module-plugs)。用以下内容创建一个新的`lib/lokalise_demo_web/plugs/set_locale_plug.ex`文件:

```
defmodule LokaliseDemoWeb.Plugs.SetLocale do import Plug.Conn # 1 @supported_locales Gettext.known_locales(LokaliseDemoWeb.Gettext) # 2 def init(_options), do: nil # 3 def call(%Plug.Conn{params: %{"locale" => locale}} = conn, _options) when locale in @supported_locales do # 4 end def call(conn, _options), do: conn # 5 end
```

让我们来讨论这段代码:

1.  在这一行，我们导入了一个行为。它要求我们履行一定的契约(见下文)。
2.  这是带有支持的区域设置列表的模块属性
3.  这是契约的实际履行:自动调用的回调。它可能返回传递给`call/2`函数的选项，或者只返回`nil`
4.  用请求的所有 GET 参数初始化`call/2`。我们只对`locale`部分感兴趣，并使用模式匹配机制获取它。同样在这一行中，我们有一个 guard 子句来确保所选择的语言确实得到支持
5.  这是当传递的区域设置不受支持时调用的回退子句。在这种情况下，我们只返回连接，不做任何修改。

我们需要做的最后一件事是充实`call/2`函数的第一个子句。它只需将选择的语言环境设置为当前语言环境:

```
def call(%Plug.Conn{params: %{"locale" => locale}} = conn, _options) when locale in @supported_locales do LokaliseDemoWeb.Gettext |> Gettext.put_locale(locale) conn end
```

注意`conn`必须由`call/2`函数返回！

塞子准备好了，你可以把它放在`:browser`管道里面了；

```
# lib/router.ex # ... pipeline :browser do plug :accepts, ["html"] plug :fetch_session plug :fetch_flash plug :protect_from_forgery plug :put_secure_browser_headers plug LokaliseDemoWeb.Plugs.SetLocale end
```

现在重新加载服务器并导航到`http://localhost:4000/?locale=en`。欢迎信息应该是英文的，这意味着自定义插头正在按预期工作！

### 将区域设置存储到 Cookie 中

我们的下一个任务是在请求中持久化所选择的语言环境，这样用户就不需要每次都提供它。这种持久性的最佳选择是 cookies:存储在用户 PC 上的小文本文件。Phoenix 确实支持开箱即用的 cookies，所以只需在您的插件中使用一个`[put_resp_cookies/4](https://hexdocs.pm/plug/Plug.Conn.html#put_resp_cookie/4)` [函数](https://hexdocs.pm/plug/Plug.Conn.html#put_resp_cookie/4):

```
def call(%Plug.Conn{params: %{"locale" => locale}} = conn, _options) when locale in @supported_locales do LokaliseDemoWeb.Gettext |> Gettext.put_locale(locale) conn |> put_resp_cookie "locale", locale, max_age: 365*24*60*60 end
```

我们通过存储一个名为`"locale"`的 cookie 来修改连接。它的生命周期为 1 年，就网络而言，这实际上意味着永恒。

这里的最后一步是从 cookie 中读取所选择的语言环境。不幸的是，我们不能再为这个任务使用一个 guard 子句，所以让我们用一个子句替换`call/2`函数的两个子句:

```
def call(conn, _options) do case fetch_locale_from(conn) do nil -> conn locale -> LokaliseDemoWeb.Gettext |> Gettext.put_locale(locale) conn |> put_resp_cookie "locale", locale, max_age: 365*24*60*60 end end
```

总而言之，逻辑保持不变:我们获取区域设置，检查它，然后或者什么都不做，或者将其存储为当前区域设置。

添加两个私有函数来完成此功能:

```
defp fetch_locale_from(conn) do (conn.params["locale"] || conn.cookies["locale"]) |> check_locale end defp check_locale(locale) when locale in @supported_locales, do: locale defp check_locale(_), do: nil
```

这里，我们从 GET param 或 cookie 中读取区域设置，然后检查所需的语言是否受支持。然后要么返回这种语言的代码，要么只返回`nil`。干得好！

另一种非常常见的设置语言环境的方法是使用`Accept-Language` HTTP 头。如果你想实现这个机制，试着利用来自 set_locale 插件的代码[，它已经提供了所有必要的正则表达式和其他有趣的东西。](https://github.com/smeevil/set_locale/blob/fd35624e25d79d61e70742e42ade955e5ff857b8/lib/headers.ex)

### 区域设置切换器控制

因此，`SetLocale`插件已经准备好了，但是我们仍然没有提供任何控件来选择网站的语言。因此，让我们在页面顶部呈现两个链接。在`lib/views/layout_view.ex`文件中定义一个新的助手:

```
defmodule LokaliseDemoWeb.LayoutView do use LokaliseDemoWeb, :view def new_locale(conn, locale, language_title) do "<a href=\"#{page_path(conn, :index, locale: locale)}\">#{language_title}</a>" |> raw end end
```

从`templates/layout/app.html.eex`模板调用这个助手:

```
<body> <div class="container"> <header class="header"> <%= new_locale @conn, :en, "English" %> <%= new_locale @conn, :ru, "Russian" %> </header> <!-- other stuff --> </div> </body>
```

重新加载页面并尝试在区域设置之间切换。一切都应该工作正常，这意味着任务完成了！

### 使用 lokalite 简化您的生活

到目前为止，您可能认为在一个大型网站上支持多种语言可能是一件痛苦的事情。老实说，你是对的。当然，翻译可以在域名的帮助下命名。但是您仍然必须确保所有的键都被翻译成适合每个地区。幸运的是，这个问题有一个解决方案:Lokalise 平台，[使得处理本地化文件更加简单](https://lokalise.co/features)。让我来指导您完成初始设置，这其实并不复杂。

*   首先，[获取您的免费试用版](https://lokalise.co/signup)
*   创建一个新项目，给它起个名字，并将英语设为基础语言
*   单击“上传语言文件”
*   上传所有语言的采购订单文件
*   继续项目，并根据需要编辑您的翻译
*   你也可以联系专业翻译为你做这项工作
*   接下来，只需将您的 PO 文件下载回来，并替换到`priv/gettext`文件夹中
*   利润！

Lokalise 有更多的功能，包括支持几十种平台和格式，甚至可以上传截图，以便从中阅读文本。所以，坚持使用 Lokalise，让你的生活更轻松！

### 结论

在今天的教程中，我们已经看到了如何在 Gettext 的帮助下执行 Phoenix 应用程序的本地化。我们已经讨论了 Gettext 是什么以及它能提供什么好东西。我们已经看到了如何提取翻译、生成模板以及基于这些模板创建 PO 文件。您还学习了什么是域，以及如何引入对多元化的支持。最重要的是，我们已经成功地创建了我们的定制插件，以根据用户的偏好获取和保存所选择的语言环境。一篇文章还不错！

要了解更多关于 Phoenix I18n 的信息，我鼓励您查看官方指南中的[，它提供了一般的解释，以及针对个别功能的文档。要更详细地了解 Gettext 及其特性，请参考](https://hexdocs.pm/gettext/Gettext.html) [GNU 的文档](https://www.gnu.org/software/gettext/manual/gettext.html)。当然，如果你有任何问题，欢迎在评论中发表！

*最初发布于 2018 年 9 月 27 日[blog.lokalise.co](https://blog.lokalise.co/localization-of-phoenix-applications/)。*