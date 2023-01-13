# 如何在 Kotlin 应用程序中使用 Xodus 数据库

> 原文：<https://www.freecodecamp.org/news/how-to-use-the-xodus-database-in-kotlin-applications-3f899896b9df/>

我想向您展示如何将我最喜欢的数据库之一用于 Kotlin 应用程序。即[xodu](https://github.com/JetBrains/xodus)。为什么我喜欢用 Xodus 做 Kotlin 应用？这是它的几个卖点:

*   **交易型**
*   **嵌入式**
*   **无模式**
*   **纯基于 JVM 的**
*   拥有额外的**科特林 DSL**——[Xodus——DNQ](https://github.com/JetBrains/xodus-dnq)。

这对你意味着什么？

*   ACID 板载—所有数据库操作都是原子的、一致的、隔离的和持久的。
*   无需管理外部数据库—一切都在您的应用程序中。
*   无痛重构——如果您需要添加几个属性，就不必重新构建表。
*   跨平台数据库——xodu 可以在任何可以运行 Java 虚拟机的平台上运行。
*   Kotlin 语言的好处——在属性声明和约束描述中充分利用类型、可空值和委托。

Xodus 是来自 [JetBrains](https://www.jetbrains.com/) 的开源产品。最初它是为内部使用而开发的，但随后在 2016 年 7 月向公众发布。 [YouTrack issue tracker](https://www.jetbrains.com/youtrack) 和 [Hub team tool](https://www.jetbrains.com/hub/) 将其作为数据存储。如果你对性能很好奇，可以查看一下[基准](https://github.com/JetBrains/xodus/wiki/Benchmarks)。至于现实生活中的例子，看一看 [JetBrains YouTrack 安装](https://youtrack.jetbrains.com/issues):在撰写本文时，它已经有超过 160 万个问题，这还没有考虑所有存储在那里的评论和时间跟踪条目。

Xodus-DNQ 是一个 Kotlin 库，包含了 Xodus 的数据定义语言和查询。它也是首先作为产品的一部分开发的，然后公开发布。YouTrack 和 Hub 都使用它进行持久层定义。

### 设置

让我们编写一个存储书籍及其作者的小应用程序。

我将使用 Gradle 作为构建工具，因为它有助于简化所有的依赖管理和项目编译工作。如果你从未和 Gradle 合作过，我推荐你看看他们关于[安装](https://gradle.org/install/)和[创建新版本](https://guides.gradle.org/creating-new-gradle-builds/)的官方指南。

所以首先，我们需要为我们的示例创建一个新目录，然后在那里运行`gradle init`。这将初始化项目结构，并添加一些目录和构建脚本。

现在，在`src/main/kotlin`目录下创建一个`bookstore.kt`文件。用永不过时的经典填满它:

```
fun main() {
  println("Hello World")
}
```

然后，使用类似下面的代码更新`build.gradle`文件:

```
plugins {
  id 'application'
  id 'org.jetbrains.kotlin.jvm' version '1.3.21'
}
group 'mariyadavydova'
version '1.0-SNAPSHOT'
sourceCompatibility = 1.8
targetCompatibility = 1.8
tasks.withType(org.jetbrains.kotlin.gradle.tasks.KotlinCompile).all {
  kotlinOptions {
    jvmTarget = "1.8"
  }
}
repositories {
  mavenCentral()
}
dependencies {
  implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.3.21'
  implementation 'org.jetbrains.xodus:dnq:1.2.420'
}
mainClassName = 'BookstoreKt'
```

这里发生了一些事情:

1.  我们添加了 Kotlin 插件，并声称编译输出是针对 JVM 1.8 的。
2.  我们向科特林标准库和 Xodus-DNQ 添加了依赖项。
3.  我们还添加了应用程序插件并定义了主类。在 Kotlin 应用程序的例子中，我们没有一个像 Java 中那样具有静态方法 main 的类。相反，我们必须定义一个独立的函数`main`。不过在遮光罩下，Kotlin 还是做了一个包含这个函数的类，类名是从文件名生成的。比如`‘bookstore.kt’`制造`‘BookstoreKt’`。

我们实际上可以安全地删除`settings.gradle`，因为在这个例子中我们不需要它。

现在，执行`./gradlew run`；您应该会在控制台中看到“Hello World ”:

```
> Task :run
Hello World
```

### 数据定义

![VQdCPUo-UlHYulNuJGemzF98MzBCfgfsq3k7](img/2b8c8b187e5ea013e04d9e6ac50ad927.png)

Photo by [Alfons Morales](https://unsplash.com/@alfonsmc10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Xodus 提供了三种不同的方式来处理数据，即[环境](https://github.com/JetBrains/xodus/wiki/Environments)、[实体存储](https://github.com/JetBrains/xodus/wiki/Entity-Stores)和[虚拟文件系统](https://github.com/JetBrains/xodus/wiki/Virtual-File-Systems)。然而，Xodus-DNQ 只支持实体存储，它将数据模型描述为一组具有命名属性(attributes)和命名实体链接(relations)的类型化实体。它类似于 SQL 数据库表中的行。

因为我的目标是展示通过 Kotlin DSL 操作 Xodus 是多么简单，所以在这个故事中，我将坚持使用实体类型 API。

让我们从一个`XdAuthor`开始:

```
class XdAuthor(entity: Entity) : XdEntity(entity) {
  companion object : XdNaturalEntityType<XdAuthor>()
var name by xdRequiredStringProp()
  var countryOfBirth by xdStringProp()
  var yearOfBirth by xdRequiredIntProp()
  var yearOfDeath by xdNullableIntProp()
  val books by xdLink0_N(XdBook::authors)
}
```

在我看来，这个声明看起来很自然:我们说我们的作者总是有名字和出生年份，可能有出生国家和死亡年份(后者与目前活着的作者无关)；此外，在我们的书店里，每个作者的书可以有任意数量。

在这段代码中有几件事值得一提:

*   `companion`对象为每个类声明了`entityType`属性(由数据库引擎使用)。
*   数据字段是在委托的帮助下声明的，委托封装了这些字段的类型、属性和约束。
*   链接是值，不是变量；也就是说，你不用`=`来设置它们，而是作为一个集合来访问它们。(注意`val books`对`var name`；我花了相当多的时间试图找出为什么用`var books`编译总是失败。)

第二种是`XdBook`:

```
class XdBook(entity: Entity) : XdEntity(entity) {
  companion object : XdNaturalEntityType<XdBook>()
var title by xdRequiredStringProp()
  var year by xdNullableIntProp()
  val genres by xdLink1_N(XdGenre)
  val authors : XdMutableQuery<XdAuthor> by xdLink1_N(XdAuthor::books)
}
```

这里需要注意的是`authors`字段的声明:

*   注意，我们显式地写下了类型(`XdMutableQuery<XdAuth`或>)。对于双向链接，我们必须通过在链接的一端留下提示来帮助编译器解析类型。
*   另外，注意`XdAuthor::books`引用了`XdBook::authors`，反之亦然。如果我们希望链接是双向的，我们必须添加这些引用；所以如果你给这本书添加一个作者，这本书就会出现在这个作者的书的列表里，反之亦然。

第三种实体类型是一个`XdGenre`枚举，这很简单:

```
class XdGenre(entity: Entity) : XdEnumEntity(entity) {
 companion object : XdEnumEntityType<XdGenre>() {
   val FANTASY by enumField {}
   val ROMANCE by enumField {}
 }
}
```

### 数据库初始化

现在，当我们声明了实体类型后，我们必须初始化数据库:

```
fun initXodus(): TransientEntityStore {
  XdModel.registerNodes(
      XdAuthor,
      XdBook,
      XdGenre
  )
  val databaseHome = File(System.getProperty("user.home"), "bookstore")
  val store = StaticStoreContainer.init(
      dbFolder = databaseHome,
      environmentName = "db"
  )
  initMetaData(XdModel.hierarchy, store)
  return store
}
fun main() {
  val store = initXodus()
}
```

这段代码显示了最基本的设置:

*   我们定义数据模型。这里我们手动列出了所有的实体类型，但是也可以使用[自动扫描类路径](https://jetbrains.github.io/xodus-dnq/meta-model.html)。
*   我们初始化存储在`{user.home}/bookstore`文件夹中的数据库。
*   我们将元数据与商店联系起来。

### 填写数据

![X41x19KiXNapMX3lR5wDETktCtpl1prZQiHD](img/1f642f1d056bdd8601bb8d88b66b40cf.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现在我们已经初始化了数据库，是时候在里面放些东西了。在此之前，让我们将`toString`方法添加到实体类中。它们唯一的目的是允许我们以人类可读的格式输出数据库内容。

```
class XdAuthor(entity: Entity) : XdEntity(entity) {
  ...
  override fun toString(): String {
    val bibliography = books.asSequence().joinToString("\n")
    return "$name ($yearOfBirth-${yearOfDeath ?: "???"}):\n$bibliography"
  }
}
class XdBook(entity: Entity) : XdEntity(entity) {
  ...
  override fun toString(): String {
    val genres = genres.asSequence().joinToString(", ")
    return "$title (${year ?: "Unknown"}) - $genres"
  }
}
class XdGenre(entity: Entity) : XdEnumEntity(entity) {
  ...
  override fun toString(): String {
    return this.name.toLowerCase().capitalize()
  }
}
```

注意`books.asSequence().joinToString("\n")`和`genres.asSequence().joinToString(", ")`指令:这里我们使用`asSequence()`方法将`XdQuery`转换为 Kotlin 集合。

好的，现在让我们在主函数中添加几本我们收藏的书。我们在事务内部进行的所有数据库操作(创建、读取、更新和删除实体)都是原子数据库修改，这保证了一致性。

以我们的书店为例，有很多方法可以往里面放东西:

1.分别添加作者和图书:

```
 val bronte = store.transactional {
   XdAuthor.new {
     name = "Charlotte Brontë"
     countryOfBirth = "England"
     yearOfBirth = 1816
     yearOfDeath = 1855
   } 
 }
 store.transactional {
   XdBook.new {
     title = "Jane Eyre"
     year = 1847
     genres.add(XdGenre.ROMANCE)
     authors.add(bronte)
   }
 }
```

2.添加一位作者，并将几本书放入他们的列表中:

```
 val tolkien = store.transactional {
   XdAuthor.new {
     name = "J. R. R. Tolkien"
     countryOfBirth = "England"
     yearOfBirth = 1892
     yearOfDeath = 1973
   }
 }
 store.transactional {
   tolkien.books.add(XdBook.new {
     title = "The Hobbit"
     year = 1937
     genres.add(XdGenre.FANTASY)
   })
   tolkien.books.add(XdBook.new {
     title = "The Lord of the Rings"
     year = 1955
     genres.add(XdGenre.FANTASY)
   })
 }
```

3.添加图书作者:

```
 store.transactional {
   XdAuthor.new {
     name = "George R. R. Martin"
     countryOfBirth = "USA"
     yearOfBirth = 1948
     books.add(XdBook.new {
       title = "A Game of Thrones"
       year = 1996
       genres.add(XdGenre.FANTASY)
     })
   }
 }
```

为了检查是否创建了所有内容，我们需要做的就是打印数据库的内容:

```
store.transactional(readonly = true) {     println(XdAuthor.all().asSequence().joinToString("\n***\n"))
 }
```

现在，如果您执行`./gradlew run`，您应该会看到以下输出:

```
Charlotte Brontë (1816-1855):
Jane Eyre (1847) - Romance
***
J. R. R. Tolkien (1892-1973):
The Hobbit (1937) - Fantasy
The Lord of the Rings (1955) - Fantasy
***
George R. R. Martin (1948-???):
A Game of Thrones (1996) - Fantasy
```

### 限制

如前所述，事务保证了数据的一致性。Xodus 在保存更改之前执行的操作之一是检查约束。在 DNQ 中，其中一些编码在提供给定类型属性的委托的名称中。例如，`xdRequiredIntProp`必须总是被设置为某个值，而`xdNullableIntProp`可以保持为空。

尽管如此，Xodus-DNQ 允许定义更复杂的约束，这些约束在[官方文档](https://jetbrains.github.io/xodus-dnq/properties.html#simple-property-constraints)中有描述。我给`XdAuthor`实体类型添加了几个例子:

```
 var name by xdRequiredStringProp { containsNone("?!") }
  var country by xdStringProp {
    length(min = 3, max = 56)
    regex(Regex("[A-Za-z.,]+"))
  }
  var yearOfBirth by xdRequiredIntProp { max(2019) }
  var yearOfDeath by xdNullableIntProp { max(2019) }
```

您可能想知道为什么我将`countryOfBirth`属性的长度限制为 56 个字符。嗯，我发现最长的官方国名是“大不列颠及北爱尔兰联合王国”——正好 56 个字符！

### 问题

我们已经在上面使用了数据库查询。你还记得吗？我们使用`XdAuthor.all().asSequence()`打印了作者列表。正如您可能猜到的那样，`all()`方法返回给定实体类型的所有条目。

然而，我们更倾向于过滤数据。以下是一些例子:

```
store.transactional(readonly = true) {
  val fantasyBooks = XdBook.filter { 
    it.genres contains XdGenre.FANTASY }
  val booksOf20thCentury = XdBook.filter { 
    (it.year ge 1900) and (it.year lt 1999) }
  val authorsFromEngland = XdAuthor.filter { 
    it.countryOfBirth eq "England" }

  val booksSortedByYear = XdBook.all().sortedBy(XdBook::year)
  val allGenres = XdBook.all().flatMapDistinct(XdBook::genres)
}
```

同样，构建数据查询有很多选择，所以我强烈建议看一下[文档](https://jetbrains.github.io/xodus-dnq/queries.html)。

我希望这个故事对你和我写的时候一样有用:)任何反馈都非常感谢！

你可以在这里找到本教程的源代码。