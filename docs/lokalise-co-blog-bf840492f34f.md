# Rails 国际化完全指南(i18n)

> 原文：<https://www.freecodecamp.org/news/lokalise-co-blog-bf840492f34f/>

安娜斯塔西娅

# Rails 国际化完全指南(i18n)

![U-JBkqIqeKtNuw7-Nex4WlA-tpf2lQ1HC8Rf](img/e38c72f7c5287ad34fec693ca67f6b57.png)

在本文中，您将学习如何将您的 [Rails 应用程序](https://rubyonrails.org/)翻译成多种语言，使用翻译，本地化日期时间，以及切换地区。我们将通过创建一个示例应用程序并逐步增强它来查看所有这些方面。到本文结束时，您将拥有在实际项目中开始实施这些概念的所有必要知识。

### 准备 Rails 应用程序

因此，正如我已经说过的，我们将看到所有的概念都在起作用，因此让我们通过运行以下命令来创建一个新的 Rails 应用程序:

```
rails new SampleApp
```

对于本教程，我使用的是 Rails 5.2.1 ，但是大多数描述的概念也适用于旧版本。

现在让我们生成一个`StaticPagesController`，它将有一个`index`动作(我们的主页):

```
rails g controller StaticPages index
```

通过添加一些示例内容来调整`views/static_pages/index.html.erb`视图:

```
<h1>Welcome!</h1> <p>We provide some fancy services to <em>good people</em>.</p>
```

此外，我想添加一个反馈页面，我们的用户将能够分享他们对公司的意见(希望是积极的)。每条反馈都有作者姓名和实际信息:

```
rails g scaffold Feedback author message
```

我们只对两个动作感兴趣:`new`(它将呈现表单以发布评论，并列出所有现有的评论)和`create`(实际验证并保存评论)。当然，理想情况下，评论应该是预先审核的，但我们今天不会为此费心。

调整`new`动作，从数据库中获取所有评论，并按照创建日期对它们进行排序:

```
# feedbacks_controller.rb # ... def new @feedback = Feedback.new @feedbacks = Feedback.order created_at: :desc end
```

我还想在处理表单并保存新记录时将用户重定向到反馈页面:

```
# feedbacks_controller.rb # ... def create @feedback = Feedback.new(feedback_params) if @feedback.save redirect_to new_feedback_path else @feedbacks = Feedback.order created_at: :desc render :new end end
```

在`new`页面上呈现反馈集合:

```
<!-- views/feedbacks/new.html.erb --> <!-- other code goes here... --> <%= render @feedbacks %>
```

最后，为个人反馈创建一个部分:

```
<!-- views/feedbacks/_feedback.html.erb --> <article> <em> <%= tag.time feedback.created_at, datetime: feedback.created_at %><br> Posted by <%= feedback.author %> </em> <p> <%= feedback.message %> </p> <hr> </article>
```

注意路线:

```
# config/routes.rb Rails.application.routes.draw do resources :feedbacks root 'static_pages#index' end
```

最后，向布局添加一个全局菜单:

```
<!-- views/layouts/application.html.erb --> <!-- other code goes here... --> <nav> <ul> <li><%= link_to 'Home', root_path %></li> <li><%= link_to 'Feedback', new_feedback_path %></li> </ul> </nav>
```

现在运行迁移并启动服务器:

```
rails db:migrate rails s
```

导航到`http://locahost:3000`并确保一切正常。既然我们已经有了要处理的东西，让我们进入主要部分并本地化我们的应用程序。

### 一点配置

在执行翻译之前，我们需要决定支持哪些语言。你可以选择任何一个，但是我将坚持使用俄语和英语，后者被设置为默认。在`config/application.rb`文件中反映这一点:

```
# ... config.i18n.available_locales = [:en, :ru] config.i18n.default_locale = :en
```

还要连接一个 rails-i18n gem(T1 ),它有不同语言(T2 和 T3)的地区数据。例如，它翻译了月份名称、复数规则和其他有用的东西。

```
# Gemfile # ... gem 'rails-i18n'
```

只需安装这块宝石，您就可以开始了:

```
bundle install
```

### 存储翻译

现在一切都配置好了，让我们来关注一下主页，并在那里翻译文本。

最简单的方法是利用[本地化视图](https://guides.rubyonrails.org/i18n.html#localized-views)。您需要做的就是创建名为`index.LANG_CODE.html.erb`的视图，其中`LANG_CODE`对应于一种受支持的语言。因此，在这个演示中，我们应该创建两个视图:`index.en.html.erb`和`index.ru.html.erb`。在里面只需放置网站的英语和俄语版本的内容，Rails 就会根据当前设置的语言环境自动选择合适的视图。方便，嗯？

然而，这种方法并不总是可行的。另一种方法是将翻译后的字符串存储在一个单独的文件中，并根据选择的语言呈现字符串的正确版本。默认情况下，Rails 使用 [YAML 文件](https://en.wikipedia.org/wiki/YAML)，这些文件必须存储在`config/locales`目录下。不同语言的翻译存储在不同的文件中，每个文件都以该语言命名。

打开`config/locales`文件夹，注意已经有一个`en.yml`文件，里面有一些样本数据:

```
en: hello: "Hello world"
```

因此，`en`是一个顶级关键字，代表这些翻译所针对的语言。接下来，有一个嵌套的键-值对，其中`hello`是*翻译键*，而`Hello world`是实际翻译的字符串。让我们用以下内容替换这一对:

```
en: welcome: "Welcome!"
```

这只是来自我们主页的欢迎信息。现在在`config/locales`文件夹中创建一个`ru.yml`文件，并在那里提供翻译好的欢迎信息:

```
ru: welcome: "Добро пожаловать!"
```

我们刚刚为我们的第一个字符串创建了翻译，这真的很棒。

### 执行简单的翻译

现在我们已经用一些数据填充了 YAML 文件，让我们看看如何在视图中使用翻译后的字符串。实际上，就像利用别名为`t`的`translate`方法一样简单。此方法有一个必需的参数:转换键的名称:

```
<!-- views/static_pages/index.html.erb --> <h1><%= t 'welcome' %></h1>
```

当页面被请求时，Rails 查找对应于所提供的键的字符串，并呈现它。如果找不到所请求的翻译，Rails 将在屏幕上呈现这个键(并将其转换成更容易阅读的形式)。

翻译键可以被命名为你喜欢的任何东西(嗯，几乎任何东西)，但是当然建议给它们一些有意义的名字，这样你就可以理解它们对应的文本。

让我们关注第二条信息:

```
en: welcome: "Welcome!" services_html: "We provide some fancy services to <em>good people</em>."
```

```
ru: welcome: "Добро пожаловать!" services_html: "Мы предоставляем различные услуги для <em>хороших людей</em>."
```

为什么我们需要这个`_html`后缀？嗯，正如你所看到的，我们的字符串有一些 HTML 标记，默认情况下，Rails 会将`em`标记呈现为纯文本。只要我们不希望这种情况发生，我们就将字符串标记为“安全 HTML”。

现在再次使用`t`方法:

```
<!-- views/static_pages/index.html.erb --> <!-- ... ---> <p><%= t 'services_html' %></p>
```

### 关于转换键的更多信息

我们的主页现在已经本地化了，但是让我们停下来想一想我们做了什么。总而言之，我们的翻译键有一个有意义的名字，但是如果我们在应用程序中有 500 条消息，会发生什么呢？这个数字其实没那么大，大型网站可能有几千个翻译。

如果我们所有的键-值对都存储在`en`(或`ru`)键下，没有任何进一步的分组，这会导致两个主要问题:

*   我们需要确保所有的键都有唯一的名称。随着应用程序的增长，这变得越来越复杂。
*   很难找到所有相关的翻译(例如，单个页面或功能的翻译)。

因此，在任意关键字下进一步分组您的翻译将是一个好主意。例如，您可能会这样做:

```
en: main_page: header: welcome: "Welcoming message goes here"
```

嵌套的级别是不受限制的(但是你应该合理对待它)，不同组中的键可以有相同的名称。

然而，遵循视图的文件夹结构是有益的(稍后我们将看到原因)。因此，按以下方式调整 YAML 文件:

```
en: static_pages: index: welcome: "Welcome!" services_html: "We provide some fancy services to <em>good people</em>."
```

```
ru: static_pages: index: welcome: "Добро пожаловать!" services_html: "Мы предоставляем различные услуги для <em>хороших людей</em>."
```

通常，在`t`方法中引用翻译键时，需要提供完整的路径:

```
<!-- views/static_pages/index.html.erb --> <h1><%= t 'static_pages.index.welcome' %></h1> <p><%= t 'static_pages.index.services_html' %></p>
```

然而，也有一个“懒惰”查找可用。如果在视图或控制器中执行转换，并且转换键的命名空间符合文件夹结构，则可以省略所有命名空间。这样，上面的代码变成了:

```
<!-- views/static_pages/index.html.erb --> <h1><%= t '.welcome' %></h1> <p><%= t '.services_html' %></p>
```

请注意，这里需要前导点。

让我们也正确地翻译我们的全局菜单和命名空间:

```
en: global: menu: home: "Home" feedback: "Feedback"
```

```
ru: global: menu: home: "Главная" feedback: "Отзывы"
```

在这种情况下，我们不能利用惰性查找，所以请提供完整路径:

```
<!-- views/layouts/application.html.erb --> <!-- ... ---> <nav> <ul> <li><%= link_to t('global.menu.home'), root_path %></li> <li><%= link_to t('global.menu.feedback'), new_feedback_path %></li> </ul> </nav>
```

### 翻译模型

现在，让我们进入反馈页面，处理表格。我们需要翻译的第一件事是输入的标签。看起来 Rails 允许我们为模型属性提供翻译，并且它们会在需要时被自动利用。您需要做的就是正确命名这些翻译:

```
en: activerecord: attributes: feedback: author: "Your name" message: "Message"
```

```
ru: activerecord: attributes: feedback: author: "Ваше имя" message: "Сообщение"
```

标签现在将被自动翻译。至于“提交”按钮，您可以通过以下方式为模型本身提供翻译:

```
en: activerecord: models: feedback: "Feedback"
```

但老实说，我不喜欢这个按钮上的“创建反馈”文本，所以让我们坚持使用一个通用的“提交”词:

```
en: global: forms: submit: Submit
```

```
ru: global: forms: submit: Отправить
```

现在利用这个翻译:

```
<!-- views/feedbacks/_form.html.erb --> <!-- ... ---> <%= form.submit t('global.forms.submit') %>
```

### 错误消息

我们可能不希望访问者发布空的反馈信息，因此提供一些简单的验证规则:

```
# models/feedback.rb # ... validates :author, presence: true validates :message, presence: true, length: {minimum: 5}
```

但是相应的错误消息呢？我们如何翻译它们？看起来我们根本不需要做任何事情，因为 rails-i18n gem 已经知道如何定位常见错误。例如，[这个文件](https://github.com/svenfuchs/rails-i18n/blob/master/rails/locale/ru.yml#L133)包含俄罗斯地区的错误消息。如果你真的*真的*想要修改默认错误信息，那么[可以查看解释如何实现的官方文档](https://guides.rubyonrails.org/i18n.html#error-message-scopes)。

然而，该表单的一个问题是错误消息副标题(即" *N* 错误禁止保存该反馈:")没有翻译。让我们现在就解决它，同时也谈谈多元化。

### 多元化规则

只要可能存在一个或多个错误消息，字幕中的“错误”一词就应该相应地用复数表示。在英语中，单词的复数形式通常是通过添加后缀“s”来实现的，但是对于俄语来说，规则要复杂一些。

我已经提到过，rails-i18n gem 包含所有受支持语言的复数规则，所以我们不需要费心从头开始编写它们。你所需要做的就是为每一种可能的情况提供合适的钥匙。因此，对于英语，只有两种可能的情况:一个错误或许多错误(当然，不可能有错误，但在这种情况下，消息根本不会显示)。

```
en: global: forms: submit: Submit messages: errors: one: "One error prohibited this feedback from being saved" other: "%{count} errors prohibited this feedback from being saved"
```

这里的`%{count}`是插值——我们将传递的值放入字符串中。

现在，请注意俄语语言环境，它有更多可能的情况:

```
ru: global: forms: submit: Отправить messages: errors: one: "Не удалось сохранить отзыв! Найдена одна ошибка:" few: "Не удалось сохранить отзыв! Найдены %{count} ошибки:" many: "Не удалось сохранить отзыв! Найдено %{count} ошибок:" other: "Не удалось сохранить отзыв! Найдена %{count} ошибка:"
```

有了这些，只需利用这些翻译:

```
<!-- views/feedbacks/_form.html.erb --> <!-- ... ---> <%= form_with(model: feedback, local: true) do |form| %> <% if feedback.errors.any? %> <div id="error_explanation"> <h2><%= t 'global.forms.messages.errors', count: feedback.errors.count %></h2> <!-- errors... --> </ul> </div> <% end %> <!-- form fields --> <% end %>
```

注意，在这种情况下，我们传递了翻译键以及变量`count`的值。Rails 将根据这个数字选择合适的翻译变量。另外，`count`的值将被插入到每个`%{count}`占位符中。

我们的下一站是`_feedback.html.erb`部分。这里我们需要本地化两个字符串:“Posted by…”和 datetime ( `created_at`字段)。至于“发布者……”，让我们再次利用插值:

```
en: global: feedback: posted_by: "Posted by %{author}"
```

```
ru: global: feedback: posted_by: "Автор: %{author}"
```

```
<!-- views/feedbacks/_feedback.html.erb --> <article> <em> <%= tag.time feedback.created_at, datetime: feedback.created_at %><br> <%= t 'global.feedback.posted_by', author: feedback.author %> </em> <p> <%= feedback.message %> </p> <hr> </article>
```

但是`created_at`呢？为了解决这个问题，我们可以利用别名为`l`的`localize`方法。它与 Ruby 的`strftime`非常相似，但是产生了日期的翻译版本(特别是，月份名称被正确翻译)。让我们使用一种叫做`:long`的[预定义格式](https://github.com/svenfuchs/rails-i18n/blob/master/rails/locale/ru.yml#L265):

```
<!-- views/feedbacks/_feedback.html.erb --> <article> <em> <%= tag.time l(feedback.created_at, format: :long), datetime: feedback.created_at %><br> <%= t 'global.feedback.posted_by', author: feedback.author %> </em> <!--... --> </article>
```

如果你想添加你自己的格式，也可以像这里解释的那样。

### 在区域设置之间切换

所以，我们的应用程序现在已经完全翻译了…但是有一件很小的事情:我们不能改变地区！仔细想想，这确实是一个相当大的问题，所以让我们现在就解决它。

在请求中设置和保持所选的语言环境有几种可能的方式。我们将坚持以下方法:

*   我们的 URL 将有一个可选的`:locale`参数，所以它们看起来像`[http://localhost:3000/en/some_page](http://localhost:3000/en/some_page)`
*   如果设置了这个参数，并且支持指定的语言环境，我们就将应用程序翻译成相应的语言
*   如果未设置此参数或不支持该区域设置，请设置默认区域设置

听起来很简单？那我们就来钻研代码吧！

首先，通过包含一个`scope`来调整`routes.rb`:

```
# config/routes.rb scope "(:locale)", locale: /#{I18n.available_locales.join("|")}/ do # your routes here... end
```

在这里，我们使用正则表达式验证指定的参数，以确保该区域设置受支持(注意，这里不允许使用像`\A`这样的锚字符)。

接下来，在`ApplicationController`中设置一个`before_action`来检查和设置每个请求的语言环境:

```
# application_controller.rb # ... before_action :set_locale private def set_locale I18n.locale = extract_locale || I18n.default_locale end def extract_locale parsed_locale = params[:locale] I18n.available_locales.map(&:to_s).include?(parsed_locale) ? parsed_locale : nil end
```

此外，为了跨请求保持所选的语言环境，设置`default_url_options`:

```
# application_controller.rb # ... private def default_url_options { locale: I18n.locale } end
```

将把`locale`参数包含到 Rails 助手生成的每个链接中。

最后一步是提供两个链接以在地区间切换:

```
<!-- views/layouts/application.html.erb --> <!-- ... --> <nav> <ul> <li><%= link_to t('global.menu.home'), root_path %></li> <li><%= link_to t('global.menu.feedback'), new_feedback_path %></li> </ul> <ul> <li><%= link_to 'English', root_path(locale: :en) %></li> <li><%= link_to 'Русский', root_path(locale: :ru) %></li> </ul> </nav>
```

作为一个练习，你可以使这些链接更漂亮，例如，重定向用户回到他正在浏览的页面。

### 使用 lokalite 简化您的生活

到目前为止，您可能认为在一个大型网站上支持多种语言可能是一件痛苦的事情。老实说，你是对的。当然，翻译可以有名称空间，如果需要的话，[甚至可以分成多个 YAML 文件](https://guides.rubyonrails.org/i18n.html#organization-of-locale-files),但是你仍然必须确保所有的键都为每个地区翻译。

幸运的是，这个问题有一个解决方案:Lokalise 平台，[使得处理本地化文件更加简单](https://lokalise.co/features)。让我来指导您完成初始设置，这其实并不复杂。

*   首先，[获取您的免费试用版](https://lokalise.co/signup)
*   安装将用于上传和下载翻译文件的 Lokalise CLI
*   打开您的[个人资料页面](https://lokalise.co/profile)，导航到“API 令牌”部分，并生成一个读/写令牌
*   创建一个新项目，给它起个名字，并将英语设为基础语言
*   在项目页面上，单击“更多”按钮并选择“设置”。在此页面上，您应该可以看到项目 ID
*   现在只需从命令行运行`lokalise --token <token> import <project_id> --lang_iso en --file config/lo` cales/en.yml，同时提供您生成的令牌和项目 ID(在 Windows 上，您可能还需要提供文件的完整路径)。这应该上传英文翻译到洛卡利斯。对俄语语言环境运行相同的命令。
*   导航回项目概述页面。您应该在那里看到所有的翻译键和值。当然，可以编辑、删除它们，也可以添加新的。在这里，您还可以过滤密钥，例如，查找未记录的密钥，这非常方便。
*   编辑完翻译后，通过运行`lokalise --token <token> export <project_id> --type yaml --bundle_structure %LANG_ISO%.yml --unzip_to E:/Supreme/docs/work/lokalise/rails/SampleApp/con` fig/locales/将它们下载回来。太好了！

Lokalise 有更多的功能，包括支持几十种平台和格式，能够从专业人士那里订购翻译，甚至可以上传截图，以便阅读其中的文本。所以，坚持使用 Lokalise，让你的生活更轻松！

### 结论

在本文中，我们已经彻底讨论了如何在 Rails 应用程序中引入国际化支持，并自己实现了它。您已经学习了如何以及在哪里存储翻译，如何查找它们，什么是本地化视图，如何翻译错误消息和与 ActiveRecord 相关的内容，以及如何在语言环境之间切换并在请求中保持所选的语言环境。今天还不错，是吧？

当然，不可能在一篇文章中涵盖 Rails I18n 的所有细节，因此我建议查看官方指南中的[部分，该指南给出了关于该主题的更多详细信息并提供了有用的示例。](https://guides.rubyonrails.org/i18n.html)

*最初发布于 2018 年 8 月 23 日[blog.lokalise.co](https://blog.lokalise.co/rails-i18n/)。*