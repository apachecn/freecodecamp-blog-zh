# 代码评审清单:如何解决 Java 并发性问题

> 原文：<https://www.freecodecamp.org/news/code-review-checklist-java-concurrency-49398c326154/>

作者:罗曼·莱文托夫

# 代码评审清单:如何解决 Java 并发性问题

![GzQlMvHCGYcEKGRtS5pZxvzPyXHKTZPHf6cr](img/7e586b34ac409fa3bf6b45ff5bf5205a.png)

Photo by [J. Kelly Brito](https://unsplash.com/photos/PeUJyoylfe4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/checklist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 [Apache Druid](http://druid.apache.org/) 社区，我们目前正在准备一份详细的清单，将在代码审查期间使用。我决定将清单的一部分作为帖子发布在 Medium 上，以收集更多关于清单项目的想法。希望有人会发现它在实践中有用。

顺便说一下，在我看来，为代码审查创建特定于项目的清单应该是一个强大的想法，但是我在大型开源项目中没有看到任何现有的例子。

这篇文章包含了关于多线程 Java 代码出现的问题的清单项目。

感谢[马尔科·托波尔尼克](https://stackoverflow.com/users/1103872/marko-topolnik)、[马特科·梅登贾克](https://github.com/mmedenjak)、[克里斯·维斯特](https://github.com/chrisvest)、[西蒙·威尔诺尔](https://github.com/s1monw)、[本·马涅斯](https://github.com/ben-manes)、[格莱布·斯米尔诺夫](https://github.com/gvsmirnov)、[安德烈·萨塔林](https://github.com/asatarin)、[本尼迪克特·金](https://github.com/asdf2014)和[彼得·扬内切克](https://stackoverflow.com/users/1273080/petr-jane%C4%8Dek)对本文的评论和贡献。清单并不完整，欢迎评论和建议！

更新:该清单现已在 Github 上[发布。](https://github.com/code-review-checklists/java-concurrency)

#### 1.设计

1.1.如果补丁引入了具有并发代码的新子系统，那么**在补丁描述**中是否合理化了并发的必要性？是否讨论了可以简化代码并发模型的替代设计方法(见下一条)？

1.2.有没有可能应用一个或几个设计模式(下面列出了其中的一些)来显著地**简化代码的并发模型，同时不显著地损害其他质量方面**，例如整体的简单性、效率、可测试性、可扩展性等等？

**不变性/快照。**当一些状态应该被更新时，一个新的不可变对象(或可变对象中的快照)被创建、发布和使用，而一些并发线程可能仍然使用较旧的副本或快照。参见本检查表中的【EJ 第 17 项】、【JCIP 3.4 项】、4.5 项、9.2 项、`CopyOnWriteArrayList`、`CopyOnWriteArraySet`、[持久数据结构](https://en.wikipedia.org/wiki/Persistent_data_structure)。

**分而治之。**工作被分割成几个独立处理的部分，每个部分在一个单独的线程中。然后组合处理的结果。平行流(见第 14 节)或`ForkJoinPool`(见第 10.4 和 10.5 项)可用于应用该模式。

生产者-消费者。工作通过队列在工作线程之间传输。参见【JCIP 5.3】，本检查表第 6.1 项， [CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes) ， [SEDA](https://en.wikipedia.org/wiki/Staged_event-driven_architecture) 。

**实例坐月子。一些根类型的对象封装了一些复杂的层次子状态。根对象单独负责多线程访问和修改子状态的安全性。换句话说，合成对象是同步的，而不是合成同步对象。见[JCIP 4.2，10.1.3，10.1.4]。**

**线程/任务/串行线程限制。**使用自顶向下的传递参数或`ThreadLocal`使一些状态成为线程的本地状态。参见【JCIP 3.3】。任务限制是线程限制思想的一种变体，与分治模式结合使用。它通常以 lambda 捕获的“上下文”参数或每线程任务对象中的字段的形式出现。串行线程限制是生产者-消费者模式线程限制思想的扩展，参见[JCIP 5.3.2]。

#### 2.证明文件

2.1.对于每个有线程安全迹象的类、方法和字段，比如关键字`synchronized`、字段上的`volatile`修饰符、使用来自`java.util.concurrent.*`的任何类、第三方并发原语或并发集合:它们的 Javadoc 注释是否包括

*   线程安全的理由:是否解释了为什么一个特定的类、方法或字段必须是线程安全的？
*   **并发控制流文档**:枚举了线程安全类的每个具体方法是从什么方法，在什么线程(执行器，线程池)的上下文中调用的吗？

当一些逻辑被并行化或者执行被委托给另一个线程时，是否有注释解释为什么顺序地或者在同一个线程中执行逻辑更糟糕或者不合适？关于并行`Stream`的使用，参见本清单中的第 14.1 项。

2.2.如果补丁引入了一个使用线程或线程池的新子系统，那么在某个地方是否有线程模型、子系统的并发控制流(或数据流)的**高级描述，例如在`package-info.java`中的包的 Javadoc 注释中或子系统的主类中？当添加新的线程或线程池或者从系统中删除一些旧的线程或线程池时，这些描述是否保持最新？**

线程模型的描述包括在子系统中创建和管理的线程和线程池的枚举，以及在子系统中使用的外部池(如`ForkJoinPool.commonPool()`)，它们的大小和其他重要特征，如线程优先级，以及托管线程和线程池的生命周期。

并发控制流的高级描述应该是一个概述，并将各个类的并发控制流文档联系在一起，请参见上一项。如果使用生产者-消费者模式，并发控制流是微不足道的，应该记录数据流。

描述线程模型和控制/数据流极大地提高了系统的可维护性，因为在缺乏描述或图表的情况下，开发人员会花费大量的时间和精力在脑海中创建和刷新这些模型。放下模型也有助于发现瓶颈和简化设计的方法(见 1.2)。

2.3.对于作为项目的公共 API 或扩展 API 的一部分的类和方法:是否在它们的 Javadoc 注释中指定了它们是否是**不可变的、线程安全的或者非线程安全的**(或者对于为扩展中的子类化而设计的接口和抽象类，它们是否应该被实现为)**？对于线程安全的类和方法(或者应该被实现为线程安全的),是否精确地记录了它们可以被多线程并发调用的其他方法(或者它们自己)?另见[EJ 项目 82]和[JCIP 4.5]。**

如果使用`@com.google.errorprone.annotations.Immutable`注释来标记不可变的类，那么[容易出错的](https://errorprone.info/)静态分析工具能够检测出一个类何时实际上不是不可变的(参见相关的[错误模式](https://errorprone.info/bugpattern/Immutable))。

2.4.对于使用一些并发设计模式的子系统、类、方法和字段，无论是高级的(如本清单中 1.2 项所提到的)还是低级的(如双重检查锁定，参见本清单的第 8 部分):所使用的**并发模式是否在各个子系统、类、方法和字段的设计或实现注释**中声明？这有助于读者更快地理解代码。

2.5.`ConcurrentHashMap`和`ConcurrentSkipListMap`对象是否存储在`ConcurrentHashMap`或`ConcurrentSkipListMap`或`**ConcurrentMap**`T6 类型的字段和变量中，而不仅仅是`Map`？

这很重要，因为在如下代码中:

```
ConcurrentMap<String, Entity> entities = getEntities();if (!entities.containsKey(key)) {  entities.put(key, entity);} else {  ...}
```

很明显，可能存在竞争条件，因为实体可能被调用`containsKey()`和`put()`之间的并发线程放入映射中(参见第 4.1 条关于这种类型的竞争条件)。而如果`entities`变量的类型仅仅是`Map<String, Enti` ty >，这就不那么明显了，读者可能会认为这只是稍微次优的代码而忽略掉。

在 IntelliJ IDEA 中，可以将这个建议变成一个检查。

2.6.前一项的扩展:调用`compute()`、`computeIfAbsent()`、`computeIfPresent()`或`merge()`方法的并发哈希表是否存储在`ConcurrentHashMap`类型的字段和变量中，而不是`ConcurrentMap`？这是因为`ConcurrentHashMap`(不同于一般的`ConcurrentMap`接口)保证传递到类似`compute()`的方法中的 lambdas 是按键自动执行的，并且类的线程安全性可能依赖于这种保证。

这个建议可能看起来过于迂腐，但是如果与一个静态分析规则结合使用，该规则禁止在非并发哈希表的`ConcurrentMap`类型的对象上调用类似`compute()`的方法(在 IntelliJ IDEA 中也可以创建这样的检查),它可以防止一些错误:例如，**在`ConcurrentSkipListMap`上调用`compute()`可能是一个竞争条件**,对于习惯于依赖`ConcurrentHashMap`中的`compute()`的强语义的人来说，很容易忽略这一点。

2.7.`**@GuardedBy**` **标注是否使用了**？如果对一些字段的访问应该被一些锁保护，那么这些字段是否被标注了`@GuardedBy`？从其他方法的关键部分调用的私有方法是否标注了`@GuardedBy`？如果项目不依赖任何包含此注释的库(由`jcip-annotations`、`error_prone_annotations`、`jsr305`和其他库提供)，并且由于某种原因不希望添加此类依赖，则应该在 Javadoc 注释中提及，因为访问和调用它们的各个字段和方法应该受到一些指定锁的保护。

有关`@GuardedBy`的更多信息，请参见【JCIP 2.4】。

使用`GuardedBy`特别有利于与易错一起使用，它能够[静态地检查对带有`@GuardedBy`注释](https://errorprone.info/bugpattern/GuardedBy)的字段和方法的无保护访问。

2.8.如果在一个线程安全的类中，一些**字段既可以从临界区内部访问，也可以从临界区**外部访问，注释中解释了为什么这是安全的吗？例如，对一个不可变对象的无保护只读访问可能是良性的。

2.9.关于每一个带`volatile`修饰符的字段:**真的需要是`volatile`** 吗？字段的 Javadoc 注释是否解释了为什么字段需要`volatile`字段读写的语义(如 [Java 内存模型](https://docs.oracle.com/javase/specs/jls/se11/html/jls-17.html#jls-17.4)中所定义的)？

2.10.对于线程安全类中既没有使用`volatile`也没有使用`@GuardedBy` 注释的每个可变字段，在 **Javadoc 注释中有解释吗，为什么是安全的？也许，该字段只能从单个方法或一组指定只能从单个线程顺序调用的方法中访问和变异(如 2.1 项所述)。这个建议也适用于存储非线程安全类对象的`final`字段，当这些对象可以从封闭线程安全类的方法中变异出来时。请参见本清单中的第 4.2–4.4 项，了解此类代码可能会出现的问题。**

#### 3.过度的线程安全

3.1.过度线程安全的一个例子是一个类，其中每个可修改的字段都是`volatile`或`AtomicReference`或其他原子的，并且每个集合字段都存储一个并发集合(例如`ConcurrentHashMap`)，尽管对这些字段的所有访问都是同步的。

代码中不应该有任何“额外”的线程安全，应该有足够的线程安全。线程安全的重复让读者困惑，因为他们可能认为额外的线程安全预防措施是(或曾经是)需要的，但却找不到目的。

这个原则的例外是[安全本地双重检查锁定模式](http://hg.openjdk.java.net/code-tools/jcstress/file/9270b927e00f/tests-custom/src/main/java/org/openjdk/jcstress/tests/singletons/SafeLocalDCL.java#l71)中的惰性初始化字段上的`volatile`修饰符，这是实现双重检查锁定的推荐方式，尽管当惰性初始化对象具有所有`final`字段 [*](https://shipilev.net/blog/2014/safe-public-construction/#_safe_initialization) 时`volatile`对于正确性来说是[过多的。如果没有这个`volatile`修饰符，双重检查锁定的线程安全性很容易被惰性初始化对象类中的一个变化(添加一个非`final`字段)所破坏，尽管这个类不应该意识到微妙的并发含义。如果惰性初始化对象的类被*指定*为不可变的(参见 2.3 项)，那么`volatile`仍然是不必要的，并且](https://shipilev.net/blog/2014/safe-public-construction/#_correctness) [UnsafeLocalDCL](http://hg.openjdk.java.net/code-tools/jcstress/file/9270b927e00f/tests-custom/src/main/java/org/openjdk/jcstress/tests/singletons/UnsafeLocalDCL.java#l71) 模式可以安全地使用，但是某个类具有所有`final`字段的事实并不一定意味着它是不可变的。

参见本文第 8 节关于双重检查锁定的内容。

3.2.不是有`**AtomicReference**`**`AtomicBoolean``AtomicInteger`或者`AtomicLong`字段，在这些字段上只调用`get()`和`set()`方法吗？**可以使用带有`volatile`修饰符的简单字段，但也可能不需要`volatile`；见第 2.9 项。

#### 4.竞赛条件

4.1.难道**不是用多个单独的`containsKey()`、`get()`、`put()`和`remove()`调用**而不是一个对`compute()` / `computeIfAbsent()` / `computeIfPresent()` / `replace()`的调用来更新并发哈希表吗？

4.2.对非线程安全集合(如`HashMap`或`ArrayList` )的临界区之外的`Map.get()`、`containsKey()`或`List.get()`等点读访问是否存在**，而新的条目可以并发地添加到集合中，即使在某些条目被放入集合的时刻与相同条目在临界区之外被点查询的时刻之间存在发生前边缘？**

问题是，当新条目可以添加到集合中时，它会不断增长并改变其内部结构(`HashMap`重新散列散列表，`ArrayList`重新分配内部数组)。在这样的时刻，竞争可能会发生，无保护的点读访问可能会因`NullPointerException`、`ArrayIndexOutOfBoundsException`或返回`null`或一些随机条目而失败。

注意，这个问题也适用于`ArrayList`,即使元素只是被添加到列表的末尾。然而，在 OpenJDK 中对`ArrayList`的实现做一点小小的改变，就可以在这种情况下以很小的代价禁止数据竞争。如果你订阅了并发兴趣邮件列表，你可以通过恢复[这个帖子](http://cs.oswego.edu/pipermail/concurrency-interest/2018-September/016526.html)来帮助引起对这个问题的关注。

4.3.前一项的变体:非线程安全的集合，如`HashMap`或`ArrayList` **不是在临界区**之外迭代的吗，尽管它可以被并发地修改？当集合上的`Iterable`、`Iterator`或`Stream`从线程安全类的方法返回时，这可能会意外发生，即使迭代器或流是在临界区内创建的。

和前一条一样，这一条也适用于增长的数组列表。

4.4.更一般地说，**不是可以从线程安全类的 getter**中并发变异的非平凡对象吗？

4.5.如果一个线程安全类中有多个变量同时被**更新，但是有单独的 getter**，那么在调用这些 getter 的代码中难道没有竞争条件吗？如果有的话，应该将变量设为专用 POJO 中的`final`字段，作为更新状态的快照。POJO 直接或作为一个`AtomicReference`存储在线程安全类的一个字段中。单个字段的多个 getter 应该替换为返回 POJO 的单个 getter。这允许通过一次读取状态的一致快照来避免客户端代码中的竞争情况。

这种模式对于编写安全且相当简单的非阻塞代码也非常有用:参见清单中的第 9.2 项和[JCIP 15.3.1]。

4.6.如果某个临界区中的某些逻辑依赖于某些数据，这些数据主要是类的内部可变状态的一部分，但是在临界区之外或在不同的临界区中被读取，难道不存在竞争条件吗，因为当进入临界区时**数据的本地副本可能变得与内部状态不同步？这是一个典型的“先检查后行动”竞争条件的变体，参见[JCIP 2.2.1]。**

4.7.在代码(即程序运行时动作)和外部世界的某些动作或机器上运行的其他程序执行的动作之间，难道没有**竞争条件吗？例如，如果某些配置或凭证是从某些文件或外部注册表热重新加载的，则在与文件或注册表的单独事务中读取单独的配置参数或单独的凭证(如用户名和密码)可能会与更新这些配置或凭证的系统操作员竞争。**

另一个例子是检查文件是否存在，然后分别读取、删除或创建它，而另一个程序或用户可以在检查和操作之间删除或创建该文件。处理这样的竞争条件并不总是可能的，但是记住这样的可能性是有用的。对于文件系统操作，更喜欢使用来自`[java.nio.file.Files](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html)`类的静态方法，而不是来自旧的`java.io.File`的方法。来自`Files`的方法对文件系统竞争条件更敏感，倾向于在不利的情况下抛出异常，而`File`上的方法吞下错误，甚至很难检测到竞争条件。

#### 5.用并发实用程序替换锁

5.1.是否可以使用来自`java.util.concurrent.*`和**的并发集合和/或实用程序，避免使用带有`Object.wait()` / `notify()` / `notifyAll()`** 的锁？围绕并发集合和实用程序重新设计的代码通常比实现带有内在锁、`Object.wait()`和`notify()`的等价逻辑的代码更清晰，更不容易出错(带有`await()`和`signal()`的`Lock`对象在这方面没有什么不同)。有关更多信息，请参见[EJ 项目 81]。

5.2.有没有可能通过使用 Guava 的`[Monitor](https://google.github.io/guava/releases/27.0.1-jre/api/docs/com/google/common/util/concurrent/Monitor.html)`而不是来**简化使用固有锁或带有条件等待的`Lock`对象的代码？**

#### 6.避免死锁

6.1.如果实现了一个线程安全的类，使得存在由不同锁保护的嵌套临界区，**是否有可能重新设计代码以去除嵌套临界区**？有时，一个类可以被分成几个不同的类，或者在一个线程中完成的一些工作可以被分成几个线程或任务，它们通过并发队列进行通信。见[JCIP 5.3]关于生产者-消费者模式的更多信息。

6.2.如果重构一个线程安全的类来避免嵌套的临界区是不合理的，那么是否有意检查了在整个类的代码中锁是以相同的顺序获得的？存储锁定对象的字段的锁定顺序是否记录在 Javadoc 注释中？

6.3.如果存在由几个(潜在不同的)**动态确定的锁保护的嵌套的关键部分(例如，与一些业务逻辑实体相关联)，这些锁是否在获取**之前排序？更多信息见【JCIP 10.1.2】。

6.4.不是有**调用一些回调(监听器等。)可以通过类的关键部分**内的公共 API 或扩展接口调用来配置？使用这样的调用，系统可能天生就容易出现死锁，因为在关键部分中执行的外部逻辑可能不知道锁定考虑事项，并回调到项目的逻辑中，在那里可能会获得更多的锁，从而潜在地形成可能导致死锁的锁定循环。更不用说外部逻辑可能只是执行一些耗时的操作，从而损害系统的效率(见下一条)。更多信息见[JCIP 10.1.3]和[EJ 项目 79]。

#### 7.提高可扩展性

7.1.**临界截面是否越小越好？**对于每一个关键的节:节的开头和结尾的一些语句不能移出它吗？最小化关键部分不仅提高了可伸缩性，还使检查它们和发现竞争条件和死锁变得更加容易。

这个建议同样适用于传递给`ConcurrentHashMap`的`compute()`类方法的 lambdas。

另见[JCIP 11.4.1]和[EJ 项目 79]。

7.2.有没有可能**增加锁定粒度**？如果一个线程安全的类封装了对 map 的访问，那么是否有可能将临界区变成 lambdas，并传递给`ConcurrentHashMap.compute()` 或`computeIfAbsent()`或`computeIfPresent()`方法，以享受有效的每键锁定粒度？不然有没有可能用 [**芭乐的`Striped`**](https://github.com/google/guava/wiki/StripedExplained) 或者等效的？有关锁分条的更多信息，请参见[JCIP 11.4.3]。

7.3.有没有可能使用非阻塞集合而不是阻塞集合？以下是 JDK 一些可能的替代者:

*   `Collections.synchronizedMap(HashMap)`、`Hashtable` → `ConcurrentHashMap`
*   `Collections.synchronizedSet(HashSet)` → `ConcurrentHashMap.newKeySet()`
*   `Collections.synchronizedMap(TreeMap)` → `ConcurrentSkipListMap`。顺便说一句，`ConcurrentSkipListMap`并不是最先进的并发排序字典实现。`[SnapTree](https://github.com/nbronson/snaptree)`比`ConcurrentSkipListMap`更有效吗[比](https://github.com/apache/incubator-druid/pull/6719)更有效吗？已经有一些研究论文提出了声称比 SnapTree 更有效的算法。
*   `Collections.synchronizedSet(TreeSet)` → `ConcurrentSkipListSet`
*   `Collections.synchronizedList(ArrayList)`、`Vector` → `CopyOnWriteArrayList`
*   `LinkedBlockingQueue` → `ConcurrentLinkedQueue`
*   `LinkedBlockingDeque` → `ConcurrentLinkedDeque`

是否考虑过**使用来自[JCTools 库](https://www.baeldung.com/java-concurrency-jc-tools)的基于数组的队列之一，而不是`ArrayBlockingQueue`** ？来自 JCTools 的那些队列被归类为阻塞，但是它们在许多情况下避免了锁获取，并且通常比`ArrayBlockingQueue`快得多。

7.4.**有没有可能用`[ClassValue](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ClassValue.html)`代替`ConcurrentHashMap<Class, .`。**。>？但是，请注意，u`nlike ConcurrentH`ashMap wit`h its computeIfAb`sent()m`ethod Clas`s value 并不能保证每个类的值只计算一次，`i. e. ClassValue.computeV`value()可能会被多个并发线程执行。因此，如果计算 I`nside computeV`value()不是线程安全的，它应该单独同步。另一方面，sValue 保证总是返回相同的值。get()(调用 u `nless re` move())。

7.5.有没有考虑过用`ReadWriteLock` 代替**一个简单的锁？然而，要注意的是，获取和释放一个`ReentrantReadWriteLock`比简单的内在锁更昂贵，因此可伸缩性的增加是以降低吞吐量为代价的。如果要在一个锁下执行的操作很短，或者如果一个锁已经被分条(见 7.2 项)，因此竞争很小，**用一个`ReadWriteLock`替换一个简单的锁可能会对应用程序性能产生净负面影响**。详见[本评论](https://medium.com/@leventov/interesting-perspective-thanks-i-didnt-think-about-this-before-e044eec71870)。**

7.6.当不需要重入时，是否可以用`[**StampedLock**](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/locks/StampedLock.html)` **代替`ReentrantReadWriteLock`** ？

7.7.对于“热字段”(见【JCIP 11.4.4】)是否可以使用`[**LongAdder**](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/atomic/LongAdder.html)` **而不是`AtomicLong`或`AtomicInteger`，在这些字段上只调用了`incrementAndGet()`、`decrementAndGet()`、`addAndGet()`和(很少)`get()`这样的方法，而没有调用`set()`和`compareAndSet()`？**

#### 8.惰性初始化和双重检查锁定

8.1.对于每一个延迟初始化的字段:**初始化代码是线程安全的吗？它可能被多个线程同时调用吗？**如果答案为“否”和“是”，则应该使用双重检查锁定或者应该立即进行初始化。

8.2.如果一个字段在简单锁下被延迟初始化，是否可以使用双重检查锁来提高性能？

8.3.双重检查锁定是否遵循本清单第 3.1 项中提到的 [SafeLocalDCL](http://hg.openjdk.java.net/code-tools/jcstress/file/9270b927e00f/tests-custom/src/main/java/org/openjdk/jcstress/tests/singletons/SafeLocalDCL.java#l71) 模式？

如果初始化的对象是不可变的，也可以使用更有效的[unsafelocalcl](http://hg.openjdk.java.net/code-tools/jcstress/file/9270b927e00f/tests-custom/src/main/java/org/openjdk/jcstress/tests/singletons/UnsafeLocalDCL.java#l71)模式。然而，如果延迟初始化的字段不是`volatile`，并且存在绕过初始化路径对该字段的访问，则**字段的值必须小心地缓存在局部变量**中。例如，下面的代码有问题:

```
private MyClass lazilyInitializedField;
```

```
void foo() {  if (lazilyInitializedField != null) { // (1)    // Can throw NPE!    lazilyInitializedField.bar();     // (2)  }}
```

这可能会导致一个`NullPointerException`，因为虽然第一次读取第 1 行的字段时观察到一个非空值，但是第二次读取第 2 行时可能会观察到空值。

上述代码可以修正如下:

```
void foo() {  MyClass lazilyInitialized = this.lazilyInitializedField;  if (lazilyInitialized != null) {    lazilyInitialized.bar();  }}
```

更多信息参见“[如意算盘:发生-之前是实际排序](https://shipilev.net/blog/2016/close-encounters-of-jmm-kind/#wishful-hb-actual)”。

8.4.在每种特定情况下，双重检查锁定和惰性字段初始化对性能和复杂性的净影响难道没有超过惰性初始化的好处吗？急切地初始化字段最终不是更好吗？

8.5.如果一个字段在简单锁下或者使用双重检查锁被惰性初始化，那么它真的需要锁吗？如果两个线程同时进行初始化，并使用初始化状态的不同副本，不会发生任何不好的事情，那么良性竞争就是允许的。初始化的字段仍然应该是`volatile`(除非初始化的对象是不可变的)，以确保在执行初始化和读取字段的线程之间有一个先发生边缘。

参见【EJ 第 83 项】和“[安全发布本和 Java 中的安全初始化](https://shipilev.net/blog/2014/safe-public-construction/)”。

#### 9.非阻塞和部分阻塞代码

9.1.如果有一些非阻塞或半对称阻塞的代码改变了线程安全类的状态，是否有意检查了如果非阻塞突变路径上的**线程在每个语句后被抢占，对象仍然处于有效状态**？是否有足够的注释，也许在几乎每一个状态改变的语句之前，使代码的读者相对容易地重复和验证检查？

9.2.有没有可能通过**将所有可变状态限制在一个不可变的 POJO 中，并通过比较和交换操作**来更新它，从而简化一些非阻塞代码？第 4.5 项中也提到了这种模式。如果状态的所有部分都是可以容纳 64 位的整数，则可以使用单个`long`值来代替 POJO。另见[JCIP 15.3.1]。

#### 10.线程和执行器

10.1.线程在创建时是否被命名为？ExecutorServices 是用命名线程的线程工厂创建的吗？

似乎不同的项目对于`Thread`创建的其他方面有不同的策略:是否让它们成为`setDaemon()`的守护进程，是否设置线程优先级，以及是否应该指定一个`ThreadGroup`。许多这样的规则可以通过[禁用 API](https://github.com/policeman-tools/forbidden-apis)有效执行。

10.2.在一些可能被重复调用的方法中，不是有线程被创建和启动，但没有存储在字段 a-la `**new Thread(...).start()**`中吗？是否可以将工作委托给缓存或共享的`ExecutorService`来代替？

10.3.**一些网络 I/O 操作不是在`Executors.newCachedThreadPool()`创建的`ExecutorService`中执行的吗？**如果运行应用程序的机器出现网络问题或由于负载增加而耗尽网络带宽，执行网络 I/O 的缓存线程池可能会开始不受控制地创建新线程。

10.4.**调度到`ForkJoinPool`** 的任务中不存在阻塞或 I/O 操作吗(通过`[managedBlock()](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ForkJoinPool.html#managedBlock(java.util.concurrent.ForkJoinPool.ManagedBlocker))` 调用执行的除外)？并行`Stream`操作在公共`ForkJoinPool`中隐式执行，传递给`CompletableFuture`方法的 lambdas 以“Async”结尾。

这个建议不应该走得太远:偶尔的短暂 IO(比如在日志记录期间可能发生的)和可能很少阻塞的操作(比如`ConcurrentHashMap.put()`调用)通常不应该取消所有调用方在`ForkJoinPool`或并行`Stream`中执行的资格。参见[并行流指导](http://gee.cs.oswego.edu/dl/html/StreamParallelGuidance.html)了解这些权衡的更详细的讨论。

另请参见本清单中关于平行流的第 14 节。

10.5.与前一项相反:**通过将任务提交给`ForkJoinPool.commonPool()`或通过并行流而不是使用自定义线程池**(例如由`ExecutorServices`中的一个静态工厂方法创建的)，非阻塞计算可以并行化或异步执行吗？除非自定义线程池配置了为线程指定非默认优先级的`ThreadFactory`或自定义异常处理程序(参见第 10.1 项)，否则没有理由在系统中创建更多线程，而不是重用公共`ForkJoinPool`的线程。

#### 11.线程中断和`Future`取消

11.1.如果一些代码将`InterruptedException`包装到另一个异常中(例如`RuntimeException`)，那么**在抛出包装异常之前是否恢复了当前线程的中断状态？**

将`InterruptedException`封装到另一个异常中是一种有争议的做法(尤其是在库中),它可能在某些项目中被完全禁止，或者在特定的子系统中被禁止。

11.2.如果某个方法**在捕获一个`InterruptedException`** 后正常返回，这与该方法的(文档化的)语义一致吗？在捕获一个`InterruptedException` 后正常返回通常只在两种方法中有意义:

*   `Runnable.run()`或`Callable.call()`本身，或者打算作为任务提交给某些执行者作为方法引用的方法。假设`Executor`中线程的中断策略是未知的，那么在从方法返回之前`Thread.currentThread().interrupt()`仍然应该被调用。
*   具有“尝试”或“尽力”语义的方法。这种方法的文档应该清楚，当线程被中断时，它们停止尝试做某事，恢复线程的中断状态并返回。

如果一个方法不属于这两个类别中的任何一个，它应该直接传播`InterruptedException`或者包装到另一个异常中(参见上一条)，或者它不应该在捕捉到`InterruptedException`后正常返回，而是在某种重试循环中继续执行，保存中断状态并在返回前恢复它(参见来自 JCIP 的[示例](http://jcip.net/listings/NoncancelableTask.java))。幸运的是，在大多数情况下，不需要编写这样的样板代码:**可以使用 Guava 的`[Uninterruptibles](https://google.github.io/guava/releases/27.0.1-jre/api/docs/com/google/common/util/concurrent/Uninterruptibles.html)`实用程序类中的一个方法。**

11.3.如果一个`**InterruptedException**` **或者一个`TimeoutException`被一个`Future.get()`调用**捕获，并且未来后面的任务没有副作用，即`get()`被调用只是为了在当前线程的上下文中获得和使用结果，而不是实现一些副作用，那么未来[是否被取消](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Future.html#cancel(boolean))？

有关线程中断和任务取消的更多信息，请参见[JCIP 7.1]。

#### 12.时间

12.1.从`**System.nanoTime()**` **返回的值是否以溢出感知的方式**进行比较，如该方法的[文档](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#nanoTime())中所述？

12.2.比较从`**System.currentTimeMillis()**` **返回的值的代码是否有预防“时间倒流”**的措施？这可能是由于服务器上的时间校正造成的。从`currentTimeMillis()`返回的值小于已经看到的一些其他值，应该被忽略。否则，应该有注释解释为什么这个问题与代码无关。

或者，可以用`System.nanoTime()`代替`currentTimeMillis()`。从`nanoTime()`返回的值永远不会减少(但可能会溢出——见上一条)。警告:`nanoTime()`直到 8u192 年才在 OpenJDK 中坚持这一保证(参见 [JDK-8184271](https://bugs.openjdk.java.net/browse/JDK-8184271) )。确保使用最新的发行版。

在分布式系统中，[闰秒](https://en.wikipedia.org/wiki/Leap_second)调整会导致类似的问题。

12.3.存储时间限制和周期的**变量是否有识别其单位**的后缀，例如，“timeoutMillis”(也是秒，-微秒，-毫微秒)，而不仅仅是“超时”？在方法和构造函数参数中，另一种方法是在“超时”参数后提供一个`[TimeUnit](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/TimeUnit.html)`参数。这是公共 API 的首选选项。

12.4.**有“timeout”和“delay”参数的方法会把负参数当作零吗？**这是为了遵守最小惊讶原则，因为从`java.util.concurrent.*`开始，类中所有的定时阻塞方法都遵循这个惯例。

#### 13.清理器和本机代码的线程安全

13.1.如果一个类管理本地资源并使用`java.lang.ref.Cleaner`(或`sun.misc.Cleaner`)；或者覆盖`Object.finalize()`，以确保当该类的对象被垃圾收集时资源被释放，并且该类使用直接从`close()`而不是通过`Cleanable.clean()`(或`sun.misc.Cleaner.clean()`)执行的相同清理逻辑来实现`Closeable`，以便能够区分显式`close()`和通过清理器进行的清理(例如，`clean()`可以在释放资源之前记录关于对象未被显式关闭的警告)，是否确保即使从多个线程并发调用**清理逻辑，实际的清理也只执行一次**？这类中的清理逻辑显然应该是幂等的，因为它通常会被调用两次:第一次从`close()`方法调用，第二次从清理器或`finalize()`调用。问题是清理*必须是并发幂等的，即使`close()`从未在类*的对象上被并发调用。这是因为垃圾收集器可能会认为对象在`close()`调用结束之前变得不可访问，并在`close()`仍在执行时通过清理器或`finalize()`启动清理。

或者，`close()`可以简单地委托给`Cleanable.clean()` ( `sun.misc.Cleaner.clean()`)，这是线程安全的。但是要区分显式清理和自动清理是不可能的。

另见 [JLS 12.6.2](https://docs.oracle.com/javase/specs/jls/se11/html/jls-12.html#jls-12.6.2) 。

13.2.在一个具有某种本机状态的类中，该类有一个清理器或覆盖器`finalize()`，所有与本机状态交互的方法的**体是否都用**
**`try { ... } finally { Reference.reachabilityFence(this); }`** ，
包装，包括构造函数和`close()`方法，但不包括`finalize()`？这是必要的，因为在执行与本机状态交互的方法时，对象可能变得不可访问，本机内存可能从清理程序中释放，这可能导致释放后使用或 JVM 内存损坏。

`close()`中的`reachabilityFence()`也消除了`close()`和通过清理器或`finalize()`执行的清理之间的竞争(见上一条)，但是在清理过程中保留线程安全预防措施可能是个好主意，尤其是当所讨论的类属于项目的公共 API 时，因为否则如果`close()`被意外或恶意地从多个线程并发调用，JVM 可能会由于双倍内存释放而崩溃，或者更糟的是， 内存可能会被悄悄地破坏，而 Java 平台的承诺是，无论某些代码有什么错误，只要它通过了字节码验证，抛出异常应该是最糟糕的结果，但虚拟机不应该崩溃。 第 13.4 条也强调了这一原则。

`Reference.reachabilityFence()`已加入 JDK 9。如果项目针对 JDK 8 和 Hotspot JVM，[任何带有空体的方法都是对`reachabilityFence()`](http://mail.openjdk.java.net/pipermail/core-libs-dev/2018-February/051312.html) 的有效仿真。

更多信息请参见并发兴趣邮件列表中的`[Reference.reachabilityFence()](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ref/Reference.html#reachabilityFence(java.lang.Object))`和[本讨论](http://cs.oswego.edu/pipermail/concurrency-interest/2015-December/014609.html)的文档。

13.3.不是有一些类有**清理器或者覆盖`finalize()`来释放本地资源**，而仅仅是将堆对象返回到一些池，或者仅仅是报告一些堆对象没有返回到一些池吗？这是一个反模式，因为正确使用 cleaners 和`finalize()`的巨大复杂性(参见前两项)和对性能的负面影响(尤其是`finalize()`)，这可能比不将对象返回到某个池的影响更大，从而略微增加了应用程序中的垃圾分配率。如果后一个问题变得很重要，最好在分配分析模式(`-e alloc`)下使用[异步分析器](https://github.com/jvm-profiling-tools/async-profiler)进行诊断，而不是通过注册清除程序或覆盖`finalize()`。

这个建议也适用于被缓冲区或本机内存块的其他 Java 包装器直接管理的池化对象。在这种情况下，可以使用`[async-profiler -e malloc](https://stackoverflow.com/questions/53576163/interpreting-jemaloc-data-possible-off-heap-leak/53598622#53598622)`来检测直接内存泄漏。

13.4.如果一些**类在本机内存中有一些状态，并在并发代码中被积极使用，或者属于项目的公共 API，是否考虑过让它们成为线程安全的**？如 13.2 项所述，如果在没有正确同步的情况下，无意中从多个线程访问了这些类的对象，可能会导致内存损坏和 JVM 崩溃。这就是为什么 JDK 中的类如`[java.util.zip.Deflater](http://hg.openjdk.java.net/jdk/jdk/file/a772e65727c5/src/java.base/share/classes/java/util/zip/Deflater.java)`在内部使用同步，尽管`Deflater`对象并不打算在多线程中同时使用。

注意，在本机内存中创建具有某种状态的类是线程安全的，这也意味着**本机状态应该安全地发布在构造函数**中。这意味着要么本机状态应该专门存储在`final`字段中，要么在本机状态完全初始化后在构造函数中调用`[VarHandle.storeStoreFence()](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/invoke/VarHandle.html#storeStoreFence())`。如果项目目标是 JDK 9，而`VarHandle`不可用，那么将建造者的尸体包裹在`synchronized (this) { ... }`中也可以达到同样的效果。

#### 14.平行流

14.1.对于通过`Collection.parallelStream()`或`Stream.parallel()` : **的并行流的每一次使用，是否解释了为什么在流操作之前的注释中使用并行`Stream`？**有没有粗略的计算或基准测试表明并行计算的总 CPU 时间成本超过了 [100 微秒](http://gee.cs.oswego.edu/dl/html/StreamParallelGuidance.html)？

根据第 10.4 条，注释中是否提到并行操作通常是无 I/O 和无阻塞的？后者可能明显是暂时的，但是随着代码库的发展，从并行流操作中调用的逻辑可能会意外阻塞。没有注释，很难注意到差异和计算不再适合并行流的事实。可以通过再次调用逻辑的非阻塞版本或者使用简单的顺序`Stream`而不是并行`Stream`来修复它。

奖励:项目是否配置了[禁止使用的 API](https://github.com/policeman-tools/forbidden-apis)，是否禁止使用`java.util.StringBuffer`、`java.util.Random`和`Math.random()`？`StringBuffer`和`Random`是线程安全的，它们的所有方法都是`synchronized`，这在实践中从来没有用，只会抑制性能。在 OpenJDK 中，`Math.random()`委托给一个全局静态`Random`实例。应使用`StringBuilder`代替`StringBuffer`，应使用`ThreadLocalRandom`或`SplittableRandom`代替`Random`。

### 参考书目

*   【JLS】Java 语言规范，[内存模型](https://docs.oracle.com/javase/specs/jls/se11/html/jls-17.html#jls-17.4)和`[final](https://docs.oracle.com/javase/specs/jls/se11/html/jls-17.html#jls-17.5)` [字段语义](https://docs.oracle.com/javase/specs/jls/se11/html/jls-17.html#jls-17.5)。
*   [EJ]“有效的 Java”约书亚·布洛赫，第 11 章。并发性。
*   [JCIP]“实践中的 Java 并发”，作者 Brian Goetz、Tim Peierls、Joshua Bloch、Joseph Bowbeer、大卫·福尔摩斯和 Doug Lea。
*   阿列克谢·shipilёv:的帖子
    [Java 中的安全发布和安全初始化](https://shipilev.net/blog/2014/safe-public-construction/)
    [Java 内存模型语用学](https://shipilev.net/blog/2014/jmm-pragmatics/)
    [Java 内存模型类的亲密接触](https://shipilev.net/blog/2016/close-encounters-of-jmm-kind/)
*   Doug Lea 在 Brian Goetz、Paul Sandoz、Aleksey Shipilev、Heinz Kabutz、Joe Bowbeer、…