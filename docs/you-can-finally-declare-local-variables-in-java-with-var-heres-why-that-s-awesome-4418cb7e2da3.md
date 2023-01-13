# 最后，您可以用 var 在 Java 中声明推断类型的局部变量——这就是为什么这么棒的原因

> 原文：<https://www.freecodecamp.org/news/you-can-finally-declare-local-variables-in-java-with-var-heres-why-that-s-awesome-4418cb7e2da3/>

作者:javinpaul

# 最后，您可以用 var 在 Java 中声明推断类型的局部变量——这就是为什么这么棒的原因

![jar5Flf4rKVjeXeXkxurRe-b-RRVQZMo8kTg](img/dba1abc393e0e4674a882a894101ef9c.png)

“black ceramic cup with saucer and cappuccino on brown wooden surface” by [Carli Jeen](https://unsplash.com/@carlijeen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大家好！在本文中，我将讨论 **Java 10 的新特性。**具体来说，我将谈论可能是最流行和最有用的:Java 中对 var 关键字的**介绍。**嗯，这不是一个真正的关键词，但我稍后会告诉你。

### 终于…

最后，Java 得到了 **var** 关键字来声明**局部变量。**这允许你声明一个没有类型的变量。例如，不这样做:

`String str = “Java”`

你现在可以这么说:

`var str = “Java”.`

当你声明一个 String 或 int 变量时，这听起来可能没什么好处，但是想想复杂类型的泛型。这肯定会节省大量的输入，也会提高代码的可读性。

#### 一点背景

Java 开发人员长期以来一直抱怨样板代码和编写代码时涉及的礼仪。在像 [Python](http://javarevisited.blogspot.sg/2018/03/top-5-courses-to-learn-python-in-2018.html) 、 [Groovy](http://javarevisited.blogspot.sg/2017/08/top-5-books-to-learn-groovy-for-java.html) 或 [JavaScript](http://javarevisited.blogspot.sg/2017/02/top-5-javascript-books-to-learn-best-of-lot-must-read.html) 这样的语言中，许多只需要 5 分钟的事情在 Java 中可能会因为冗长而需要 30 多分钟。

如果你已经用 Scala、Kotlin、Go、C#或任何其他的 JVM 语言编写过代码，那么你知道它们都有某种已经内置到语言中的局部变量类型推理。

例如，JavaScript 有 **let** 和 **var** ， [Scala](http://javarevisited.blogspot.sg/2018/01/10-reasons-to-learn-scala-programming.html#axzz550Ppgfxg) 和 [Kotlin](http://www.java67.com/2017/12/10-programming-languages-to-learn-in.html) 有 **var** 和 **val** ，C++有 **auto** ，C#有 **var、**，Go 通过用 **:=** 运算符声明来支持这一点。

在 [Java 10](http://javarevisited.blogspot.sg/2018/03/java-10-released-10-new-features-java.html) 之前，Java 是唯一没有本地变量类型推断或支持 var 关键字的语言。

尽管随着 lambda 表达式、方法引用和流的引入，类型推理在 Java 8 中得到了很大的改进，但局部变量仍然需要用正确的类型来声明。但是现在已经没有了！ [Java 10](http://javarevisited.blogspot.sg/2018/03/java-10-released-10-new-features-java.html#axzz5ALJyiIAt) 有一个特性， [**JEP 286:局部变量类型推断**](http://openjdk.java.net/jeps/286) **，**它允许在没有类型信息的情况下，只使用 var 关键字来声明局部变量。

让我们仔细看看。

### Java 10 var 关键字示例

以下是 Java 10 的 var 关键字的一些例子:

```
var str = "Java 10"; // infers String 
```

```
var list = new ArrayList<String>(); // infers ArrayList<String>
```

```
var stream = list.stream(); // infers Stream<String>
```

正如我所说的，在这一点上，您可能没有完全理解 var 为您所做的一切。但是看看下一个例子:

```
var list = List.of(1, 2.0, "3")
```

在这里，列表将被推断成**列表<？扩展可序列化的&可比的&l**t；..> >这是一个交集类型，但你不必键入完整的类型信息。在这种情况下，var 使代码更容易编写和阅读。

在下一节中，我们将看到更多的例子，帮助您学习如何在 Java 10 中使用 var 编写简洁的代码。

### 在 Java 中使用 var 关键字编写简洁的代码

使用 var 保留字还可以减少重复，从而使代码简洁，例如，类的名称同时出现在赋值语句的左侧和右侧，如下例所示:

```
ByteArrayOutputStream bos = new ByteArrayOutputStream();
```

这里 [ByteArrayOutputStream](https://docs.oracle.com/javase/10/docs/api/java/io/ByteArrayOutputStream.html) 重复了两次，我们可以通过使用 Java 10 的 var 特性来消除它，如下所示:

```
var bos = new ByteArrayOutputStream();
```

我们可以在 Java 中使用 [try-with-resource](https://javarevisited.blogspot.com/2011/09/arm-automatic-resource-management-in.html) 语句时做类似的事情，例如

```
try (Stream<Book> data = dbconn.executeQuery(sql)) {    return data.map(...) .filter(...) .findAny(); }
```

可以写成如下:

```
try (var books = dbconn.executeQuery(query)) {   return books.map(...) .filter(...) .findAny(); }
```

这些只是几个例子。有很多地方可以使用 var 让你的代码更加简洁易读，很多地方你可以在 Sander 的 Pluarlsight 课程[**Java 10 新特性**](https://pluralsight.pxf.io/c/1193463/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Fcourses%2Fwhats-new-java-10-local-variable-type-inference) 上看到。

这是一门付费课程，但你可以试试这个 [10 天免费试用](http://pluralsight.pxf.io/c/1193463/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Flearn)。

![unYRUbTTXMNdA-id-rw25UbXpaksermD2GOc](img/d319661080dc02f5cbfb31f699ee9ff4.png)

对于那些使用过 Groovy 或 T2 Scala 的程序员来说，var 的引入让 Java 看起来像是在走 Scala 的路……但只有时间能证明。

现在，我们可以高兴地看到, **var 使得在 Java 10 中声明复杂的局部变量变得更加容易。**

而且一定要注意:Java 10 var 关键字的局部变量类型推断只能用来声明[局部变量](http://javarevisited.blogspot.sg/2012/02/difference-between-instance-class-and.html#axzz5ArdIFI7y)(比如方法体或者代码块内部的任何变量)。

### Java 中可以用 var 声明成员变量吗？

你**不能**使用 var 来声明类内部的成员变量、方法形参或方法的返回类型。

比如这个 var 的例子就可以了:

```
public void aMethod(){ var name = "Java 10"; } 
```

但是，下面的例子是不行的:

```
class aClass{   var list; // compile time error }
```

所以，即使这个【Java 10 新特性很抢眼，看起来也不错，但它还有很长的路要走。不过，您可以开始使用它来进一步简化您的代码。更少的样板代码总是意味着更好、更可读的代码。

### 关于这个新的 var 关键字的要点

既然您已经知道在 Java 10 中可以声明局部变量而不用声明类型，那么在您开始在生产代码中使用它之前，是时候了解一些关于这个特性的重要事情了:

1.这个特性是在 **JEP 286:局部变量类型推理**下构建的，作者不是别人，正是[布莱恩·戈茨](https://www.freecodecamp.org/news/you-can-finally-declare-local-variables-in-java-with-var-heres-why-that-s-awesome-4418cb7e2da3/undefined)。他是《实践中的 [Java 并发性](http://www.amazon.com/dp/0321349601/?tag=javamysqlanta-20)的作者，这是 Java 开发人员最受欢迎的书籍之一。

2.var 关键字允许局部变量类型推断，这意味着编译器将推断出[局部变量](https://javarevisited.blogspot.com/2012/02/difference-between-instance-class-and.html)的类型。现在不需要申报了。

3.局部变量类型推断或 Java 10 var 关键字只能用于声明**局部变量**，例如在初始化器代码块上的方法内部、[中的索引增强 for 循环](http://javarevisited.blogspot.sg/2017/01/difference-between-for-loop-and-enhanced-forlop-in-java.html#axzz4pp42TeHu)、 [lambda 表达式](http://www.java67.com/2017/06/10-points-about-lambda-expressions-in-java-8.html)，以及在传统 for 循环中声明的局部变量。

您不能使用它来声明形式变量和方法的返回类型、声明成员变量或字段、构造函数形式变量以及任何其他类型的变量声明。

4.尽管引入了 var，Java 仍然是一种静态类型语言，应该有足够的信息来推断局部变量的类型。否则，编译器将抛出一个错误。

5.var 关键字类似于 C++的 auto 关键字，C#的 var， [JavaScript](http://javarevisited.blogspot.sg/2018/01/top-10-udemy-courses-for-java-and-web-developers.html) ， [Scala](http://javarevisited.blogspot.sg/2017/03/top-30-scala-and-functional-programming.html) ， [Kotlin](http://javarevisited.blogspot.sg/2018/02/5-courses-to-learn-kotlin-programming-java-android.html) ， [Groovy](http://javarevisited.blogspot.sg/2018/02/top-3-jvm-languages-java-programmer-learn.html#axzz56WXxxAC0) 和 [Python](http://www.java67.com/2018/02/5-free-python-online-courses-for-beginners.html) 的 def(某种程度上)，以及 Go 编程语言的:=运算符。

6.需要知道的一件重要事情是，尽管 var 看起来像一个关键字，但它并不是一个真正的关键字。相反，它是一个保留的类型名。这意味着使用 var 作为变量、方法或包名的代码不会受到影响。

7.另一个需要注意的是，使用 var 作为类名或接口名的代码会受到这次 Java 10 变化的影响。但正如 JEP 所说，这些名字在实践中很少见，因为它们违反了通常的命名惯例。

8.Java 10 中还不支持本地变量或最终变量 val 和 let 的[不可变](http://javarevisited.blogspot.sg/2018/02/java-9-example-factory-methods-for-collections-immutable-list-set-map.html)等价物。

### 包扎

以上就是 Java 10 中的 **var！**这是一个有趣的 Java 10 特性，它允许你声明局部变量而不用声明它们的类型。这也将帮助 Java 开发人员快速掌握其他语言，如 [Python](https://javarevisited.blogspot.com/2018/05/10-reasons-to-learn-python-programming.html) 、 [Scala](https://javarevisited.blogspot.com/2018/01/10-reasons-to-learn-scala-programming.html#axzz550Ppgfxg) 或 [Kotlin](https://hackernoon.com/should-android-developers-learn-kotlin-or-java-ee391902736f) ，因为他们大量使用 var 来声明可变变量，使用 val 来声明不可变局部变量。

即使 **JEP 286:局部变量类型推断**只支持 **var** 不支持 **val** ，但还是很有用的，感觉更像是用 Java 编写 Scala。

#### **进一步学习**

[Sander Mak](https://pluralsight.pxf.io/c/1193463/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Fcourses%2Fwhats-new-java-10-local-variable-type-inference)
[Java 10 中的新特性 Java 中局部变量类型推断的风格指南](http://openjdk.java.net/projects/amber/LVTIstyle.html)
[JEP 286:局部变量类型推断](http://openjdk.java.net/jeps/286)
[2018 年 Java 开发者应该学会的 10 件事](http://javarevisited.blogspot.sg/2017/12/10-things-java-programmers-should-learn.html#axzz53ENLS1RB)
[完整的 Java MasterClass 更好地学习 Java](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fjava-the-complete-java-developer-course%2F)

感谢阅读这篇文章。如果你喜欢这个【Java 10 的新特性，那么请与你的朋友和同事分享。

如果您有任何问题或反馈，请留言并在这里关注更多的 Java 10 教程和文章。

*原载于 2018 年 3 月 27 日[javarevisited.blogspot.com](https://javarevisited.blogspot.com/2018/03/finally-java-10-has-var-to-declare-local-variables.html)。*