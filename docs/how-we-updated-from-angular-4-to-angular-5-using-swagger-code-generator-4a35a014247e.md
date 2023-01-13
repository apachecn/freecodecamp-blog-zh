# 我们如何使用 swagger 代码生成器从 Angular 4 更新到 Angular 5

> 原文：<https://www.freecodecamp.org/news/how-we-updated-from-angular-4-to-angular-5-using-swagger-code-generator-4a35a014247e/>

马克·格里沙尼克

# 我们如何使用 swagger 代码生成器从 Angular 4 更新到 Angular 5

![1*c5MXOqXBhsy0nVXEdP89og](img/0fbf15f795a92dbbf4a446b92e9e75a4.png)

作为一家企业公司的全栈开发人员，我有机会将我们的客户端升级到 **Angular 5** 。这个过程本身并不简单，有许多事情需要考虑。在本文中，我概述了轻松简单地更新 Angular 版本所需的必要步骤。

> *按照 Angular 团队的说法，直接从 4 版更新到 6 版是不明智的。这就是为什么本文将重点关注版本 5 的更新。*

> 作为一个提醒，Angular 团队承诺从 Angular 6 和更高版本升级会更轻更容易操作。

### 先决条件

确保您使用的是最新的 swagger 版本。Angular 5 放弃了 OpaqueToken，现在使用 InjectionToken。

OpaqueToken 是一个惟一且不可变的值，它允许开发人员避免 DI 令牌 id 的冲突。 [InjectionToken](https://angular.io/api/core/InjectionToken) < T >是“opaqueeToken”的参数化且类型安全的版本 [n。](https://github.com/angular/angular/commit/d169c2434e3b5cd5991e38ffd8904e0919f11788)

> *我们用的是 swagger-codegen-maven-plugin，版本 2.2.2。由于上述问题，我们不得不升级到 2.3.1。*

> 在 maven pom.xml 文件中，我们必须将 swagger yml 的语言属性从“typescript-angular2”更改为“typescript-angular ”,这将从现在开始支持所有的 angular 版本。

在 Swagger 2.2.2 中，BASE_PATH 生成为:

与 Angular 5:

请注意，关于生成的文件，Swagger 版本之间还有另一个主要区别。例如，如果我们有一个包含各种 REST 调用的 dogResource.java 文件， **Swagger 2.2.2** 将生成 dogApi.ts，而 **Swagger 2.3.1** 将生成一个服务。含义，dog.service.ts

> *升级到最新的 swagger 版本后，您必须重构您的导入以使用 dog.service.ts 而不是 dogapi . ts。*

![0*KHF_L94Z1iOA6jxA](img/c9eadce60dccf26657b4a299d3d1f3b1.png)

by [Bruno Nascimento](https://unsplash.com/@bruno_nascimento?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 我们开始吧

我们的服务器端是用 Java 编写的，而我们的客户端是用 Angular 4 编写的。我们使用 [swagger](https://github.com/swagger-api/swagger-codegen) 创建一个 yaml 文件，该文件使用 [OpenApi](https://www.openapis.org/) 规范。

这是 RESTful APIs 的一个标准接口，允许在不访问源代码的情况下理解服务的功能。([https://swagger.io/specification/](https://swagger.io/specification/))。

接下来，我们从原始 yaml 文件生成 Typescript 文件(包含对象和 API)。你可以在这里玩他们的发电机。

***第一步:*** 将您的节点和 npm 更新到最新版本。我们使用 npm 6.1 和 node 10.0。你可以从[这里](https://nodejs.org/en/)下载。

***第二步*** :使用以下命令更新 package.json 文件:

### 升级 NgRx 和需要重构

不强制使用 NgRx，但我强烈建议使用。如果您没有使用 NgRx，可以跳到下一步。

> NgRx 团队创建了一个非常有用的迁移指南。它们一步一步地描述了为了成功迁移需要做哪些修改

在版本 5 中，Action 接口中的“payload”属性已被移除，因为它是类型安全问题的一个来源。

NgRx 团队建议为您拥有的每个动作创建一个新类，并将有效负载作为输入传递给该类的构造函数。

我们已经决定通过创建我们自己的动作接口来使转换变得更容易，这个接口叫做，***【actionwithpail，*** ，它扩展了常规的动作接口。ActionWithPayload 扩展了较新的接口，但保留了较旧的有效负载属性。

我们还注意到，我们不能使用可观察的美元。选择，我们必须用“管道”操作来包装它:

### 固定单元测试

为了使用新的存储机制，您需要创建一个模拟存储类:

为了在测试中使用它，请执行以下操作:

Store 将是所提供状态的模拟实例！我们可以用它来模拟各种状态！

### 最后的话

虽然一开始看起来令人望而生畏，但我概述的步骤很简单，也不是很复杂。如果你遇到任何问题，随时给我写信，地址:[*markgrichanik[at]Gmail[dot]com*](mailto:markgrichanik@gmail.com)。

在用 Swagger 和 NgRx 升级 Angular 应用程序时，我也很乐意听到您的任何反馈。

> 如果你喜欢这篇文章？以便其他人也能阅读