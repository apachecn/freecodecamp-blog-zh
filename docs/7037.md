# 如何解决生成器模式的样板问题

> 原文：<https://www.freecodecamp.org/news/how-to-solve-the-builder-patterns-boilerplate-problem-2ea97001dbe6/>

哈什迪普·S·贾旺达

# 如何解决生成器模式的样板问题

![0*pWJ2kCkdoFaXrlK6](img/b9159a64c6bdf43d6a15a980622ce3dc.png)

Photo by [Sai Kiran Anagani](https://unsplash.com/@_imkiran?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

人们普遍认为，Java 的主要缺点之一是编写“好的”(惯用的)Java 需要大量样板代码。随便问个 Java 从业者就知道了。这一点在 [Builder 模式](http://www.informit.com/articles/article.aspx?p=1216151&seqNum=2)中表现得最为明显。在本文中，我提出了两种解决这个问题的方法，并讨论了它们的优点和缺点。

### 构建器模式介绍

builder 模式非常适合初始化复杂的类。它通常由多个变量的设置值组成，其中一些是必需的，其余的是可选的。使用[静态工厂方法](http://www.informit.com/articles/article.aspx?p=1216151&seqNum=1)或构造函数可以导致**伸缩构造函数模式**，由[约书亚·布洛克](https://twitter.com/joshbloch)描述(正如他的推特所说，“有效的 Java 作者，API 设计者，了不起的家伙”):

> …在这种情况下，您只提供一个带有必需参数的构造函数，另一个带有一个可选参数，第三个带有两个可选参数，依此类推，最终得到一个带有所有可选参数的构造函数。

即使只有五个必需的参数，但都是相同的**类型**，开发人员也很容易在传递参数的顺序上犯错误。这可能导致微妙的、通常难以发现的错误。

构建器模式为所有这些问题提供了一个优雅的解决方案。此外，这种模式可以帮助开发人员确保在构造对象之前**满足对象的所有先决条件。这保证了构造的对象处于一致的状态。**

抛开所有这些美好的优势不谈，实际上编写一个构建器可能是一项相当乏味且重复的任务。“那又怎么样！?"你说，“至少我得到了所有这些优势！”你当然是…但是当你在同一个项目上写你的第十个构建器的时候，回来和我谈谈。

所以让我们看看其他的选择。

### 动态地做:龙目岛

[Lombok](https://projectlombok.org) 是很多 Java 开发者熟知的工具/库(虽然还不够，IMHO)。根据它的网站，它“是一个 java 库，可以自动插入你的编辑器和构建工具，为你的 Java 增添趣味。” [Lombok 通过动态代码生成](https://projectlombok.org/features/all)添加了如此多的功能，以至于在此无法一一介绍。我们将关注`[@Builder](https://projectlombok.org/features/Builder)`注释。

`@Builder`注释——顾名思义——可以用来为任何 POJO 生成一个构建器。可以通过将它放在类、构造函数或静态方法上来使用它([详见文档](https://projectlombok.org/features/Builder))。只需将注释放在`Person`类上:

通过 Lombok 的魔力，一个`PersonBuilder`类瞬间自动生成。我们现在可以使用构建器模式实例化一个`Person`对象:

```
Person.builder().firstName("Adam").lastName("Savage")    .city("San Francisco").jobTitle("TV Personality").build();
```

将`@Builder`注释放在`Person`类上

1.  为`Person`类的所有属性创建一个带有[流畅设置器](https://en.wikipedia.org/wiki/Fluent_interface)的`PersonBuilder`类，并创建一个`build()`方法来使用值集构造一个`Person`对象，
2.  向返回`PersonBuilder`新实例的`Person`类添加一个`public static builder()`方法。

将`@Builder`注释放在构造函数或静态方法上做的事情和上面一样，但是只为构造函数/静态方法中列出的参数生成 setters。

将`@NonNull`注释添加到`Person`类的字段中，使其成为**必需的**参数。不使用`PersonBuilder`的相应 setter 方法设置它的值将会在调用`build()`方法时抛出一个`NullPointerException`。

Lombok 的美妙之处在于，这些生成的代码在源文件中是不可见的(尽管 Eclipse 会在 Outline 视图中显示生成的类、方法和字段)，使它非常干净整洁。开发人员只需要关注 POJO 的重要部分，而不是其(通常)平凡实现的繁琐细节。在 POJO 中做任何更改，Lombok 会立即更新/生成相关代码。

#### 龙目岛的缺点

尽管有这些神奇的好处，龙目岛并不完美。

通过 Lombok 生成构建器的最大缺点是，如果您在构建器中需要比非空检查更复杂的验证，那么您就不走运了。你可以 [delombok](https://projectlombok.org/features/delombok) ，复制代码，并手动修改它，但这是非常繁琐的，并且首先否定了使用 lombok 的所有便利。

[给 POJO 的字段分配默认值](https://reinhard.codes/2016/07/13/using-lomboks-builder-annotation-with-default-values/)过去非常繁琐:

1.  编写正确命名的构建器类的框架。
2.  编写相应的/正确命名的字段，并将其设置为默认值。

然而，从 Lombok 的 v1.16.16 开始，通过使用`@Builder.Default`注释，分配默认值变得更加容易:

另一个缺点——尽管不是每个人都同意这一点——是空值检查(是否设置了**所需的**值)和 NPE 抛出是在 POJO 的构造器中完成的**,而不是在构造 POJO 的**之前的**。如果它的先决条件没有得到满足，我宁愿根本不构造 POJO。**

Lombok 的一个更普遍的问题是，它不能很好地与 IDE 重构工具兼容。考虑以下(公认的愚蠢)代码:

如果我们将`Person`类重命名为`HumanBeing`，相应的构建器生成的名称将变成`HumanBeingBuilder`。但是 IDE 的重构工具将无法将上面的`PersonBuilder`更新到`HumanBeingBuilder`(至少在 Eclipse 中是这样的:-))。

然而，这个问题有一个简单的解决方法:只需使用`Builder`注释的`builderClassName`选项为构建器类设置一个固定的名称:`@Builder(builderClassName = "Builder")`。然后所有对构建器类的引用将变成`Person.Builder`，这将通过重构为`HumanBeing.Builder`而被正确地改变。

### 静态完成:Eclipse Spark 插件

Eclipse Spark 插件允许您通过单击工具栏中的一个按钮来为 POJO 生成构建器代码:就这么简单！点击按钮将(可选地)显示字段列表，供您选择在构建器中使用。代码就在你眼前生成，并且在文件中是可读的。

用 Spark 插件生成一个构建器使得`Person`类看起来像这样:

现在整个生成的代码就在你面前，你可以定制它(或者不定制！)尽情地。但这还不是 Spark 故事中最精彩的部分。

Spark 还允许您生成[分阶段构建器](http://blog.crisp.se/2013/10/09/perlundholm/another-builder-pattern-for-java):第一次使用分阶段构建器时，您会喜出望外，并且您的 IDE 会提供完全正确的自动完成建议！解释分阶段构建器超出了本文的范围，所以请阅读上面链接的文章以了解更多信息。

上面提到的 Lombok 的缺点都不适用于 Spark 插件。

#### Spark 插件的缺点

总之，使用 Spark 插件确实会产生大量可见的代码。尽管你可能不需要手写，但它确实增加了源文件的视觉权重，使它看起来又大又复杂。许多开发人员可能不喜欢这样对龙目岛。

另一个问题是，Spark 是一匹只会一招的小马:生成构建器是它唯一能做的——相比之下[Lombok 能做的所有事情](https://projectlombok.org/features/all)。

### 那到底是哪个？

您应该使用的工具取决于您的独特情况和您的偏好。在绝大多数情况下，我将继续首选 Lombok，只要我需要细粒度的控制，就切换到 Spark。

你会选择哪个？

### 最后但同样重要的是…

如果你觉得这篇文章有帮助，请不要忘记鼓掌；-)!

非常欢迎建设性的讨论和纠正。