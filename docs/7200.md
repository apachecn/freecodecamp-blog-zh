# 揭开依赖注入的神秘面纱，通过这个快速介绍来看看它的实际应用

> 原文：<https://www.freecodecamp.org/news/demystifying-dependency-injection-49d4b6fe6536/>

桑卡尔普·巴蒂亚

# 揭开依赖注入的神秘面纱，通过这个快速介绍来看看它的实际应用

![SEN9zD1pGC78YEzPQDK7LP6TOuB3MzbqXLe7](img/760ca2a4408ec4b19e4a73e851e2b955.png)

Photo by [Fabien Butazzi](https://unsplash.com/photos/ALMp3vMzPig?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/mist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

依赖注入(DI)是一个话题，在我作为软件开发人员的最初几天，我发现这个话题有点难以理解。我就是找不到这个术语的好定义。

在这篇文章中，我试图用相当简单的语言表达我对它的理解。它是为那些从 DI 开始的人，或者那些只想获得更多一点洞察力的人准备的。

### **什么是依赖注入？**

首先:这是什么？依赖注入，像许多其他软件开发术语一样，是一个相当简单的概念的花哨术语。

我发现最有用的定义是:

***依赖注入就是给一个对象赋予它的实例变量。***

就是这样。为类提供(注入)依赖关系。简单吧？确实如此。

现在，在 Java 中有三种方法可以提供一个类的依赖关系。他们都实现了……(咳嗽)依赖注射。

它们是:

*   构造器
*   安装员
*   直接设置公共字段

### 让我们看看依赖注入的实际应用

我有一个名为 MyMessagePublisher 的应用程序类，它依赖于某个 EmailService 类。

#### **使用构造函数的依赖注入:**

```
public class MyMessagePublisher {    private EmailService emailService = null;        public MyMessagePublisher(EmailService emailService){        this.emailService = emailService;    }}
```

看到构造函数在那里做什么了吗？它告诉 MyMessagePublisher 使用它提供的 EmailService。实例化 MyMessagePublisher 的类应该提供(注入)一个由 MyMessagePublisher 使用的 EmailService 实例(使用构造函数)。大概是这样的:

```
EmailService emailService = new EmailService();MyMessagePublisher myMessagePublisher =                            new MyMessagePublisher(emailService);
```

干得好，建造师！

#### **使用 Setter 的依赖注入:**

```
public class MyMessagePublisher {    private EmailService emailService = null;    public MyMessagePublisher() {    }    public setEmailService(EmailService emailService) {        this.emailService = emailService;    }}
```

这是怎么回事？使用 MyMessagePublisher 的类现在可以设置它们想要使用的 EmailService。大概是这样的:

```
MyMessagePublisher myMessagePublisher = new MyMessagePublisher();myMessagePublisher.setEmailService(new EmailService());
```

好样的，二传手！

#### **通过直接设置公共字段进行依赖注入**

千万不要这样！！如果你读到这里，我相信你知道这种方法是邪恶的。

### **依赖注入的好处**

我将从解释如果我们不使用 DI 我们会错过什么开始这一部分。

考虑这段代码。我已经定义了 EmailService 和 MyMessagePublisher 类。MyMessagePublisher 本身实例化一个 EmailService 对象，而不是使用上面提到的 DI 技术。

```
public class EmailService {        public void sendEmail(String message, String receiver){        System.out.println("Email sent to " + receiver);    }}
```

```
public class MyMessagePublisher {    private EmailService emailService = new EmailService();    public void processMessages(String message, String receiver){        this.emailService.sendEmail(message, receiver);    }}
```

上面的代码有一些限制:

1.  如果 EmailService 初始化逻辑发生变化(它需要一个构造函数参数来初始化)，我们将需要对 MyMessagePublisher 类以及代码库中使用 EmailService 而不使用 DI 的其他地方进行更改。
2.  它具有紧密耦合性。假设我们想不再发送电子邮件，而是开始发送短信。然后我们将不得不编写一个新的 publisher 类。
3.  这段代码不可测试。在对 MyMessagePublisher 类进行单元测试时，我们将向每个人发送电子邮件。

这是我提出的解决方案:

```
public class EmailService implements MessageService {    @Override    public void sendMessage(String message, String receiver) {        //logic to send email    }}
```

```
public class SMSService implements MessageService {    @Override    public void sendMessage(String message, String receiver) {        //logic to send SMS    }
```

```
public interface MessageService {    void sendMessage(String message, String receiver);}
```

```
public class MyMessagePublisher {    private MessageService messageService;        public MyMessagePublisher(MessageService messageService){        this.messageService = messageService;    }        @Override    public void processMessages(String msg, String rec){        this.service.sendMessage(msg, rec);    }}
```

这种实现克服了上述所有三个限制。

*   EmailService(或 MessageService)初始化逻辑移至初始化 MyMessagePublisher 的模块
*   我们可以转移到不同的 MessageService 实现，它在不改变 MyMessagePublisher 中的代码的情况下发送 SMS
*   代码是可测试的。在对 MyMessagePublisher 进行单元测试时，我们可以使用 MessageService 的虚拟实现

太好了。我们已经使用类的构造函数实现了 DI。简单对吗？但是这里也有一些缺点。

### 普通依赖注入的问题

这什么时候成问题了？**代码增长时**。

那么有哪些选择呢？**依赖注入容器**(弹簧、导轨、匕首等)。

让我们试着更详细地回答上面的问题。

考虑下面的代码。我们正在设计一个 AllInOneApp，它提供多种服务，如预订电影票、充值预付费连接、转账和在线购物。

```
public class BookingService {    private SlotsManager slotsManager;    private MyMessagePublisher myMessagePublisher; //Looks familiar?}
```

```
public class AllInOneApp  {    BookingService bookingService; // Class above    RechargeService rechargeService;    MoneyTransferService moneyTransferService;    ShoppingService shoppingService;}
```

AllInOneApp 需要初始化四个依赖项。让我们在这里使用构造函数来使用普通依赖注入，并实例化 AllInOneApp 类。

```
public static void main(String[] args) {
```

```
AllInOneApp allInOneApp = new AllInOneApp(                new BookingService(new SlotsManager(),                                   new MyMessagePublisher(                                              new EmailService())),                 new RechargeService(...),                 new MoneyTransferService(..),                new ShoppingService(...));
```

```
}
```

这看起来很乱。我们能发现这里的问题吗？其中一些是:

*   初始化 AllInOneApp 的类也需要知道构造所有依赖类的逻辑。在任何适当规模的项目中编写代码时，这都是很麻烦的。在编写特定于业务的代码时，我们不想自己管理所有这些实例。
*   虽然我看到人们更喜欢这种 DI，但我个人认为这段代码的可读性不如下面这段代码:

```
AllInOneApp myApp = SomeDIContainer.getInstance(AllInOneApp.class);
```

如果所有的组件都使用 DI，那么在系统中的某个地方，某个类或工厂必须知道将什么注入到所有这些组件中(AllInOneApp、BookingService、MessageService 等)。

这就是依赖注入容器出现的地方。它被称为容器而不是工厂，因为容器通常承担更多的责任，而不仅仅是实例化对象和注入依赖关系。

DI 容器让我们定义哪些组件将被启动，哪些依赖项将被注入到这些组件中。我们还可以配置其他实例化特性，比如对象是单例的，或者每次被请求时都被创建。

我将会写另一篇文章来解释一个普遍使用的 DI 容器 Google Guice 的用法，它也将会有一个实践部分。我将很快更新这个故事的链接。现在，我将把这本[用户指南](https://github.com/google/guice/wiki/GettingStarted)留给大家，这是一个很好的起点。

感谢您的阅读:)