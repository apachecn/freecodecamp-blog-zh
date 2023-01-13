# 与谷歌果汁的实践会议

> 原文：<https://www.freecodecamp.org/news/a-hands-on-session-with-google-guice-5f25ce588774/>

桑卡尔普·巴蒂亚

# 与谷歌果汁的实践会议

![NnrlpXIR2If8PGgs0JUsv90uvrywBCrhEV68](img/4227a081a75140e126357d00f928a607.png)

几个月前，我写了一篇文章解释依赖注入。我提到了一篇后续文章，其中有一篇关于 Google Guice 的实际操作。虽然我对这么晚才写这篇文章感到失望，但我也为能够写第二篇文章感到高兴。

本文假设您熟悉什么是依赖注入。我建议你浏览一下[我以前的文章](https://medium.freecodecamp.org/demystifying-dependency-injection-49d4b6fe6536)，因为我们将建立在我们在那里使用的例子之上。如果你是第一次听到这个词，这是值得的。如果熟悉的话，读起来不会花太多时间:)

如果你没怎么用过 Guice，请点击这里查看 GitHub。

开始之前，我们需要设置一些东西

1.  JDK:我们将使用 Java 来完成这项任务。所以你需要一个工作的 JDK 来在你的电脑上运行 Java 代码。要检查它是否已经安装，请在命令行上运行“java -version”。如果版本是 1.6 或更高，我们是好的。只是一个提示:如果你没有 Java *的经验，我不认为尝试这样做有什么意义。*
2.  Maven :我们将使用 Maven 作为构建工具。要安装 maven，按照这里的说明【https://maven.apache.org/install.html (相当简单)。要检查您是否已经安装了 maven，请在命令行上运行“mvn -v”。
3.  **git** (可选):[https://www . linode . com/docs/development/version-control/how-to-install-git-on-Linux-MAC-and-windows/](https://www.linode.com/docs/development/version-control/how-to-install-git-on-linux-mac-and-windows/)
4.  **克隆 hands on repository(fresh guice)**:运行下面提到的命令

```
cd folder/to/clone-into/ git clone https://github.com/sankalpbhatia/FreshGuice.git
```

### 绑定和绑定注释

我们现在准备好了。让我首先介绍 Guice 框架中的两个关键术语:**绑定和绑定注释。**

**约束:**作为 Guice 的核心概念，从字面上来看，意味着涉及不可违背的义务的协议或承诺。现在让我们将它映射到依赖注入的上下文中。当我们让 Guice *用类绑定*一个实例时，我们与 Guice 约定“当我请求 X.java 的实例时，给我这个实例”。而且这个协议是不能毁约的。

**绑定注释:**有时，您会希望同一个类型有多个绑定。注释和(类)类型一起唯一地标识一个绑定。例如，在某些情况下，您可能需要同一接口的同一类/实现的两个单独的实例。为了识别这些，我们使用绑定注释。我们将在解释绑定时看到一些例子。

#### **如何创建绑定**

Guice 的用户指南部分对此进行了完美的解释。所以我会把它复制到这里:

> 要创建绑定，扩展`AbstractModule`并覆盖它的`configure`方法。在方法体中，调用`bind()`来指定每个绑定。这些方法是经过类型检查的，因此如果您使用了错误的类型，编译器可以报告错误。一旦你创建了你的模块，将它们作为参数传递给`Guice.createInjector()`来构建一个注入器。

有许多类型的绑定:链接的、实例的、@Provides 注释的、提供者绑定、构造器绑定和非目标绑定。

对于本文，我将只讨论链接绑定、实例绑定、@Provides 注释和一个特殊的注释@Inject。我很少用其他方式装订，但是更多信息可以在[https://github.com/google/guice/wiki/Bindings](https://github.com/google/guice/wiki/Bindings)找到。

1.  **链接绑定:**链接绑定将类型/接口映射到它的实现。此示例将接口 MessageService 映射到其实现 EmailService。

简单地说:当我请求 Guice 给我一个 MessageService 实例时，它会给我一个 EmailService 实例。

但是，它如何知道创建 EmailService 的实例呢？我们稍后会看到。

```
public class MessagingModule extends AbstractModule {  @Override   protected void configure() {    bind(MessageService.class).to(EmailService.class);  }}
```

也许我们希望在我们的项目中有不止一个 MessageService 实例。在某些地方，我们会希望 SMSService 与 MessageService 相关联，而不是与 EmailService 相关联。在这种情况下，我们使用绑定注释。要创建绑定注释，您必须创建两个注释，如下所示:

```
@BindingAnnotation @Target({ FIELD, PARAMETER, METHOD }) @Retention(RUNTIME)public @interface Email {}
```

```
@BindingAnnotation @Target({ FIELD, PARAMETER, METHOD }) @Retention(RUNTIME)public @interface SMS {}
```

您不需要了解元数据注释(@Target，@ Retention)。如果有兴趣，请阅读此文:[https://github.com/google/guice/wiki/BindingAnnotations](https://github.com/google/guice/wiki/BindingAnnotations)

一旦我们有了注释，我们就可以创建两个独立的绑定，这两个绑定指示 Guice 基于 BindingAnnotation(我把它看作一个限定符)创建 MessageService 的不同实例。

```
public class MessagingModule extends AbstractModule {  @Override   protected void configure() {   bind(MessageService.class).annotatedWith(Email.class)                             .to(EmailService.class);
```

```
 bind(MessageService.class).annotatedWith(SMS.class)                             .to(SMSService.class);  }}
```

2.**实例绑定:**将类型绑定到特定的实例

```
 bind(Integer.class) .annotatedWith(Names.named(“login timeout seconds”)) .toInstance(10);
```

应该避免使用。以创建复杂的对象为例，因为它会降低应用程序的启动速度。您可以改用@Provides 方法。事实上，您甚至可以忘记我们现在提到的任何关于实例绑定的内容。

3. **@提供注释**:

这直接来自 Guice 的 wiki，因为它相当简单:

> 当你需要代码来创建一个对象时，使用一个`@Provides`方法。该方法必须在模块中定义，并且必须有一个`@Provides`注释。该方法的返回类型是绑定类型。每当注入器需要该类型的实例时，它都会调用该方法。

```
bind(MessageService.class)
```

```
.annotatedWith(Email.class)
```

```
.to(EmailService.class);
```

与相同

```
@Provides@Emailpublic MessageService provideMessageService() {  return new EmailService();}
```

其中 Email.java 是一个有约束力的注解。

依赖关系可以通过这个注释传递给一个方法，这使得它在实际项目中非常有用。例如，对于下面提到的代码，注入器将在调用方法之前对字符串参数 ***apiKey*** 进行绑定。

```
@Provides @PayPalCreditCardProcessor providePayPalCreditCardProcessor(      @Named("PayPal API key") String apiKey) {  PayPalCCProcessor processor = new PaypalCCProcessor();  processor.setApiKey(apiKey);  return processor;  }
```

4. **@ Inject annotation** (及时绑定):到目前为止我们所掩盖的都被称为*显式绑定。*如果 Guice 在尝试创建实例时没有找到显式绑定，它会尝试使用即时绑定来创建一个。

Guice 可以通过使用该类的*可注入构造函数*来创建这些绑定。这要么是一个非私有、无参数的构造函数，要么是一个带有`@Inject`注释的构造函数。

### 工作

现在让我们转到我们从 Github 克隆的项目。

与前一篇文章中的例子一样，这个 maven 项目实现了一个 BillingService，它使用信用卡向 PizzaOrder 收费并生成收据。

项目结构如下:

**接口**

*   计费服务—使用信用卡对订单收费
*   CreditCardProcessor —从信用卡中扣除一些金额
*   事务日志—记录结果

**类**

科学研究委员会

*   信用卡——代表信用卡的实体
*   PizzaOrder —代表比萨饼订单的实体
*   收据——代表收据的实体
*   RealBillingService 实现 BillingService
*   paypal 信用卡处理器实现信用卡处理器
*   银行卡处理器实施信用卡处理器
*   immemorytransaction log implements transaction log
*   juicetest —使用 billingservice 的主类
*   计费模块—所有果汁绑定都在这里
*   juiceinjectiontest:检查绑定约束的单元测试

这里的任务是在 BillingModule 中创建 Guice 绑定，以满足以下约束:

1.  BillingService 的所有实现都应该绑定到 RealBillingService。
2.  用@Paypal 注释的 CreditCardProcessor 接口应该绑定到 PaypalCreditCardProcessor 类。
3.  以字符串“Bank”命名的 CreditCardProcessor 接口应绑定到 BankCreditCardProcessor 类。
4.  injector 返回的 BillingService 实例应该有一个 BankCreditCardProcessor 实例作为其依赖项。
5.  TransactionLog 的所有实现都应绑定到 InMemoryTransactionLog。

如果满足上述条件，GuiceInjectionTests 中的所有五个单元测试都应该通过。您还应该能够在 GuiceTest 中运行 main 方法。

要测试正确性:

1.  运行单元测试

```
mvn test
```

这应该运行测试文件 GuiceInjectionTests.java。

2.运行主文件

```
mvn exec:java -Dexec.mainClass="GuiceTest"
```

这应该执行项目的主类，它完成创建订单的端到端工作，使用信用卡处理付款并生成收据。

如果你有任何问题，你可以发表意见，我会试着回答。请注意，这个练习没有唯一正确的答案。给我你的解决方案，我会添加到知识库的答案。或者更好的是，给我发送一个拉取请求:)