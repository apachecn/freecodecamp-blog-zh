# 如何使用设计模式构建 E2E 测试框架

> 原文：<https://www.freecodecamp.org/news/build-an-e2e-test-framework-with-design-patterns/>

端到端或 E2E 测试是关于模拟用户体验的。它不处理函数、变量、类或数据库。相反，它处理按钮、点击、预期消息、链接等等。

你可能会说 E2E 测试是“终极”测试，因为它检查产品作为一个整体是否如预期的那样运行。

一般来说，E2E 测试很难自动化。首先，你需要能够与被测应用程序交互的工具——填写表格，等待页面完全加载，诸如此类。

您还需要从用户界面获得结果。您没有返回对象的函数，而是包含信息的 HTML 元素。模仿一个真实的用户可能很有挑战性，并且可能需要大量的维护。

在本文中，我将谈谈我自己构建 E2E 测试框架的经验。我应用了一些很酷的设计模式，所以我认为这可能会让你感兴趣，即使你与 E2E 测试自动化没有任何关系。

这篇文章与语言和工具无关。这意味着我不会引用特定的编程语言或特定的 E2E 工具，如 Selenium、木偶师或剧作家。顺便说一下，这些都是自动化 E2E 测试的好工具。此外，这篇文章主要关注网站的 E2E 测试。

## 我必须解决的问题

我必须设计一个框架，在不同的网站上执行不同的 E2E 测试。更准确地说，我需要对这些网站中特定的 React 组件进行一些测试。

无论是哪个网站，每个组件都有相同的结构和 CSS 选择器，只是从一个网站到另一个网站略有不同。我需要对每个可能的视口(手机、平板电脑和桌面)进行测试，当视口改变时，组件必须改变它们的结构。

在这个场景中，我对开发人员一无所知。所以我需要准备好相对容易地管理界面中一些不可预见的变化。换句话说，框架易于维护至关重要。

那么，我该如何创建一个 E2E 测试框架，它不会太在意开发人员是否更改了在测试中被点击的按钮的 id 属性呢？我怎么能为还没有创建的组件编写测试呢？我怎样才能让每个测试都易于阅读和理解呢？

通过应用一些抽象和设计模式，我能够实现所有这些目标。让我们看看我是怎么做到的。

## 页面对象模型

我们需要做的第一件事是为页面创建一个抽象。这很重要，原因有几个。

首先，它会增加可读性。例如，您不希望在您的测试中有一行显示`tool.getByCssSelector("button.btn.btn-submit").click()`。取而代之的是，您希望有这样一行代码:`page.clickSubmitLoginFormButton()`或者类似的代码。

你还需要把所有的 CSS 选择器和 DOM 相关的东西放在一个地方。这样，当界面中的某些东西改变时，你只需要修改一个文件(或者两个，但不是更多；-) ).

这个抽象被称为页面对象模型。您可以创建一个类，该类只表示页面中您感兴趣的元素。您将所有与 DOM 相关的东西都放在这些类中。

在我的例子中，我做的略有不同。我为每个页面创建了两个类，一个**页面模型**和一个**页面对象**。

在第一个示例中，我将页面的元素。例如，假设我们正在测试一个登录页面，那么我的 **LoginPageModel** 应该是这样的:

```
 class LoginPageModel

    constructor(tool)

        this.tool = tool

    loginUsernameInput()

        return this.tool.getById('username-input')

    loginPasswordInput()

        return this.tool.getById('password-input)

    loginSubmitButton()

        return this.tool.getById('submit-login-button') 
```

如果将来这些元素中的任何一个发生变化，我们只需要修改相应的 **PageModel** 类。

在 **PageObject** 类中，我添加了您可以在页面上执行的操作。一个 **LoginPageObject** 类的例子是:

```
 class LoginPageObject

    constructor(pageModel)

        this.model = pageModel

    typeUsername(username)

        this.model.loginUsernameInput().type(username)

    typePassword(password)

        this.model.loginPasswordInput().type(password)

    clickLoginSubmitButton()

        this.model.loginSubmitButton().click() 
```

在这里，我们可以利用一种静态类型语言，它可以在编译时获取模型类的所有方法。这样，一些智能感知工具可以提醒我们表示页面元素的每个方法的名称。

我们也得到更多的编译错误和更少的运行时错误，这对我们和我们的心理健康非常好。

为什么我们需要将页面元素和页面动作分开？包含元素和动作的单个类可能非常大。

我们可以说，通过这样做，我们应用了单一责任原则，这将是很酷的。但是在这种情况下，除了可读性和保持类简单之外，这没有太多实际意义。

有了**页面对象**抽象，我们可以进行只依赖于页面对象的测试，而不是在测试代码中间编写一些复杂的 CSS 选择器。

我们把所有与 DOM 相关的东西都放在一个地方，这样我们的测试就更有表现力，也更容易理解。

## 编写测试——门面模式

现在，我们有许多包含几个页面的所有元素和动作的类。我们现在需要做的是构建我们的测试。

这些测试将提供一个简单的接口，向客户端展示`run`功能。该功能返回测试结果。

客户端不必担心访问任何元素或执行任何操作，它只需要实例化测试并运行它。

当我们提供一个简单的接口来隐藏一个更复杂的基础设施时，我们就应用了 Facade 模式。我知道这只是我们需要做的事情的一个好听的名字。

继续我们的登录页面测试示例， **LoginTest** 应该是这样的:

```
 class LoginTest

    constructor(loginPageObject)

        this.pageObject = loginPageObject

    run()

        this.pageObject.typeUsername("TestUser")

        this.pageObject.typePassword("TestPassword")

        this.pageObject.clickLoginSubmitButton()

        assert that the login was successful 
```

方法的最后一行是断言。根据您使用的断言的复杂性，您可以单独定义它们，也可以在 Page 对象中定义它们。

通过选择第一个选项，您可以重用和扩展断言。但是，如果您的断言对于每种情况都非常具体并且足够简单，那么第一个选项可能有些过头了，您可能会很好地使用第二个选项。

我们还在测试中注入了页面对象依赖。我们不是在做`this.pageObject = new LoginPageObject()`，而是在构造函数中接收依赖项作为参数。这被称为*依赖注入*。这样，我们可以为另一个页面实例化相同的测试。

我们还在页面对象实例中注入页面模型。然后，我们可以将同一个页面对象用于另一个模型(例如:同一个 LoginPageObject 实例使用 LoginMobilePageModel，而不是常规的 LoginPageModel)。

但是现在，要实例化一个测试，我们需要实例化一个或多个页面模型，然后是一个或多个页面对象，最后是测试。这似乎是太多的工作。这正是使用依赖注入的缺点之一——但是这个问题是可以解决的！

## 工厂模式

让我们将责任委托给另一个抽象。这样的话，我们就搞一些工厂。

工厂是用来实例化其他类的类。每个工厂类将负责实例化一个特定的测试。这就是工厂模式在起作用。

因此，我们可以为我们的 LoginTest 创建一个 **LoginTestFactory** :

```
 import tool

class LoginTestFactory

    create(config)

        if config.viewport == 'mobile'
            then return new LoginTest(new LoginPageObject(new LoginMobilePageModel(tool)))
        else
            return new LoginTest(new LoginPageObject(new LoginPageModel(tool))) 
```

在这里，我们用`tool`代表任何可能的技术，你可以用它来获得页面的元素并与它们交互。

也许您没有按原样传递导入的工具，但是您使用该工具创建了一些对象，然后将这些对象作为参数传递。

但是这个想法是所有相对复杂的逻辑都被封装在一个工厂对象中。

要运行我们的测试，我们只需要这样做:

```
 runLoginTestDesktop()

    factory = new LoginTestFactory()

    config = new ConfigObject(viewport = 'desktop')

    test = factory.create(config)

    test.run()

runLoginTestMobile()

    factory = new LoginTestFactory()

    config = new ConfigObject(viewport = 'mobile')

    test = factory.create(config)

    test.run() 
```

现在，在结论部分，我们将检查我们是否完成了最初的目标

## 结论

像这样构建您的测试框架可以显著降低用户界面更改的成本。所有依赖于用户界面的代码都被隔离在抽象页面概念的特定类中。

这种抽象还允许您为下周编写测试。(我指的是尚未创建的组件的测试。)您只需创建所需的新 PageModels 和 PageObjects 来模拟页面上将要创建的元素，并且您可以用我们到目前为止所看到的相同方式构建流程的其余部分。

当界面上有特定元素时，您可以更改页面模型并验证应用程序是否按预期运行。

你也有非常容易阅读和理解的测试，因为你可以做出像`this.pageObject.clickLoginSubmitButton()`这样的表情动作。因此，您的测试可以描述您的应用程序的需求，并且易于维护。

E2E 测试自动化很难，因为很难保持简单。复杂的测试不是测试。

在这篇文章中，我展示了一些设计模式和良好的实践，你可以用它们来使它更流畅。我试图使它与语言和工具无关，这样无论你使用什么语言或技术，你都可以在你的项目中应用这些实践。我只是假设了一种面向对象的编程语言。

不管你是否在做一个 E2E 测试框架，我认为这篇文章仍然对你有用。这些技巧中的一些可以应用于相对广泛的各种问题。

您可以访问我的[个人博客](https://jj.hashnode.dev)并在[推特](https://twitter.com/josejorgexl)上关注我，了解更多计算机科学相关内容。