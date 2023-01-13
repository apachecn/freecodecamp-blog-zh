# 看看 Java 中可选的数据类型和使用它时的一些反模式

> 原文：<https://www.freecodecamp.org/news/optional-in-java-and-anti-patterns-using-it-7d87038362ba/>

作者:默文·麦克赖特

# 看看 Java 中可选的数据类型和使用它时的一些反模式

*由* [默文](https://www.freecodecamp.org/news/optional-in-java-and-anti-patterns-using-it-7d87038362ba/undefined) *和* [穆罕默德·艾明·托克](https://www.freecodecamp.org/news/optional-in-java-and-anti-patterns-using-it-7d87038362ba/undefined)

![K2LKxHKECSsGZxGXipMfxYpRKElJA1Lp1Goj](img/9c38fff1d061142f0e746e57bc987ff4.png)

#### 概观

在本文中，我们将讨论我们在使用 Java 的*可选*-数据类型时收集的经验，这是 Java 8 中引入的。在我们的日常业务中，我们遇到了一些*【反模式】*想分享一下。我们的经验是，如果您严格避免在代码中使用这些模式，那么您很有可能会得到一个更干净的解决方案。

#### 可选——明确表示可能缺少值的 Java 方式

*Optional* 的目的是用一个数据类型来表达一个值的潜在缺失，而不是仅仅因为 Java 中存在*空引用*而隐含一个值缺失的可能性。

如果你看看其他没有空值的编程语言，它们通过数据类型描述了潜在的值缺失。例如，在 Haskell 中，它是通过使用[或者](https://hackage.haskell.org/package/base-4.11.1.0/docs/Data-Maybe.html)来完成的，在我看来，这已经被证明是处理可能的“无值”的有效方法。

```
data Maybe a = Just a              | Nothing
```

上面的代码片段显示了 Haskell 中[可能是](https://hackage.haskell.org/package/base-4.11.1.0/docs/Data-Maybe.html)的定义。如你所见，*可能是由类型变量 *a* 参数化的，这意味着你可以将它用于任何你想要的类型。使用数据类型声明缺少值的可能性，例如在函数中，迫使作为函数用户的您考虑调用函数的两种可能结果——实际存在有意义的内容的情况和不存在有意义的内容的情况。*

在*可选的*被引入 java 之前，如果你想描述什么都不是，可以使用的“Java 方式”是*空引用*，它可以被赋给任何类型。因为一切都可以为空，所以如果某些东西应该为空(例如，如果您希望某些东西要么表示一个值，要么什么都不表示)，要么不为空(例如，如果某些东西*可以*为空，因为在 Java 中一切都可以为空，但在应用程序的流程中，它在任何时候都不应该为空)。

如果你想明确地说明某样东西背后有某种特定的语义，那么这个定义看起来是一样的，就像你希望某样东西一直存在一样。空引用的发明者 [*东尼·霍尔爵士*](https://en.wikipedia.org/wiki/Tony_Hoare) 甚至为空引用的引入道歉。

> 我称之为我的十亿美元错误……那时，我正在用面向对象语言设计第一个用于引用的综合类型系统。我的目标是确保所有引用的使用都是绝对安全的，由编译器自动执行检查。但是我无法抗拒放入空引用的诱惑，仅仅是因为它太容易实现了。这导致了数不清的错误、漏洞和系统崩溃，在过去的四十年里，这可能造成了数十亿美元的痛苦和损失。(东尼·霍尔，2009 年— [伦敦 QCon】](https://qconlondon.com/)

为了克服这种问题，开发人员发明了许多方法，如注释*(可空，非空)、*命名约定(例如，在方法前加上 *find* 而不是 *get* )或者只是使用代码注释来暗示方法可能有意返回 *null* ，调用程序应该关心这种情况。一个很好的例子是 Java 的 map-interface 的 get-function。

```
public V get(Object key);
```

上面的定义形象化了这个问题。仅仅通过隐含的可能性，即一切都可以是一个空引用，你不能使用方法的签名来传达这个函数的结果可以是 T2 什么都不是的选项。如果这个函数的用户查看它的定义，他们没有机会知道这个方法可能会有意返回一个*空引用*——因为在 map-instance 中可能不存在到所提供的键的映射。这正是这个方法的文档告诉你的:

> 返回指定键映射到的值，如果此映射不包含键的映射，则返回`null`。

了解这一点的唯一机会是深入查看文档。你必须记住——不是所有的代码都像这样被很好地记录。假设您的项目中有平台内部代码，它没有任何注释，但在其调用栈深处的某处返回了一个*空引用*，这让您大吃一惊。这就是用数据类型来表达值的潜在缺失的地方。

```
public Optional<V> get(Object key);
```

如果你看一下上面的类型签名，它清楚地传达了这个方法可能不返回任何东西——它甚至强迫你处理这种情况，因为它是用一个特殊的数据类型表示的。

因此，在 Java 中使用可选的很好，但是如果你在代码中使用可选的 T2，我们会遇到一些陷阱。否则使用*可选的*可能会使你的代码可读性和直观性更差。下面的部分将涵盖一些模式，我们发现这些模式是 Java 的*可选的*的某种“反模式”。

#### 在集合或流中可选

我们在代码中遇到的一个模式是将空的可选项存储在一个集合中，或者作为一个流中的中间状态。通常接下来会过滤掉空的可选选项，甚至还会调用 *Optional::get* ，因为您并不真的需要可选选项的集合。下面的代码示例显示了上述情况的一个非常简单的例子。

```
private Optional<IdEnum> findValue(String id) {   return EnumSet.allOf(IdEnum.class).stream()      .filter(idEnum -> idEnum.name().equals(id)      .findFirst();};
```

```
(...)
```

```
List<String> identifiers = (...)
```

```
List<IdEnum> mapped = identifiers.stream()   .map(id -> findValue(id))   .filter(Optional::isPresent)   .map(Optional::get)   .collect(Collectors.toList());
```

如您所见，即使在这种简化的情况下，也很难理解这段代码的意图。您必须看一看 findValue 方法，以了解它的全部意图。现在想象 findValue 方法比将字符串表示映射到枚举类型的值更复杂。

还有一篇有趣的文章是关于为什么应该避免集合中有*null*[[UsingAndAvoidingNullExplained](https://github.com/google/guava/wiki/UsingAndAvoidingNullExplained)]。一般来说，你不需要在集合中有一个空的可选的。这是因为空的可选表示“无”。想象一下，有一个包含三个条目的列表，它们都是空的选项。在大多数情况下，空列表在语义上是等价的。

那么我们能做些什么呢？在大多数情况下，在映射之前先*过滤的计划会产生更可读的代码，因为它直接陈述了你想要实现的目标，而不是隐藏在一连串的*之后，比如映射*、*过滤，然后映射*。*

```
private boolean isIdEnum(String id) {   return Stream.of(IdEnum.values())      .map(IdEnum::name)      .anyMatch(name -> name.equals(id));};
```

```
(...)
```

```
List<String> identifiers = (...)
```

```
List<IdEnum> mapped = identifiers.stream()   .filter(this::isIdEnum)   .map(IdEnum::valueOf)   .collect(Collectors.toList());
```

如果你想象这个 *isEnum-method* 归 IdEnum 自己所有，那就更清楚了。但是为了有一个可读的代码示例，它没有出现在示例中。但是仅仅通过阅读上面的例子，你就可以很容易地理解发生了什么，甚至不必真的跳入引用的 *isIdEnum-method* 。

所以，长话短说——如果你不需要在列表中缺少一个值，你就不需要 Optional——你只需要它的内容，所以 Optional 在集合中已经过时了。

#### 在方法参数中可选

我们遇到的另一种模式，尤其是当代码从使用*空引用*的“老式”方式迁移到使用*可选类型时，*在函数定义中有可选类型的参数。如果您发现一个函数对它的参数进行空检查，然后应用不同的行为，这通常会发生——在我看来，这在以前是一种不好的做法。

```
void addAndUpdate(Something value) {    if (value != null) {      somethingStore.add(value);   }    updateSomething();}
```

如果您“天真地”重构这个方法来利用可选类型，那么您可能会得到这样的结果，使用一个可选类型的参数。

```
void addAndUpdate(Optional<Something> maybeValue) {   if (maybeValue.isPresent()) {      somethingStore.add(maybeValue.get());   }   updateSomething();}
```

在我看来，函数中有可选类型的参数在任何情况下都显示了设计缺陷。无论哪种方式，你都要做出一些决定，如果参数存在，你就用它做些什么，如果不存在，你就做些别的，这个流程隐藏在函数内部。在上面的例子中，将函数分成两个函数并有条件地调用它们会更清楚(这也恰好符合“每个函数一个意图”的原则)。

```
private void addSomething(Something value) {   somethingStore.add(value);}
```

```
(...)
```

```
// somewhere, where the function would have been calledOptional.ofNullable(somethingOrNull).ifPresent(this::addSomething);updateSomething();
```

根据我的经验，如果我在实际代码中遇到过类似上面的例子，总是值得重构“直到最后”，这意味着我没有带有可选类型参数的函数或方法。我最终得到了一个更干净的代码流，更容易阅读和维护。

说到这里——在我看来，带有可选参数的函数或方法甚至没有意义。我可以有一个带参数的版本和一个不带参数的版本，并决定在调用点做什么，而不是决定它隐藏在一些复杂的函数中。所以对我来说，这以前是一个反模式(有一个参数可以**故意**为空，如果为空，则处理方式不同)，现在仍然是一个反模式(有一个**可选类型的**参数)。

#### Optional::isPresent 后跟 Optional::get

Java 中进行空安全编程的旧思维方式是对值应用空检查，在这种情况下，您不能确定它们是否实际保存了一个值或者引用了一个空引用。

```
if (value != null) {   doSomething(value);}
```

为了明确表示*值*实际上可以是某个值或者什么都不是，可能需要重构这段代码，这样就有了可选类型的 value 版本。

```
Optional<Value> maybeValue = Optional.ofNullable(value);
```

```
if (maybeValue.isPresent()) {   doSomething(maybeValue.get());}
```

上面的例子展示了重构的“幼稚”版本，这是我在几个代码示例中经常遇到的。这种*是 Present* 后跟一个 *get* 的模式可能是由旧的空校验模式导致的。写了这么多空头支票，不知何故训练我们自动以这种模式思考。但是*可选的*被设计成以另一种方式使用，以达到更可读的代码。同样的语义可以通过使用 *ifPresent* 以更易读的方式简单地实现。

```
Optional<Value> maybeValue = Optional.ofNullable(value);maybeValue.ifPresent(this::doSomething);
```

“但是，如果我想做别的事情，如果值不存在”可能是你现在的想法。因为 Java-9 *Optional* 为这种流行的情况提供了一个解决方案。

```
Optional.ofNullable(valueOrNull)    .ifPresentOrElse(        this::doSomethingWithPresentValue,        this::doSomethingElse    );
```

鉴于上述可能性，使用 *isPresent* 后跟 *get* 来实现没有的空检查**的典型用例，使这种模式成为一种反模式。可选的是 API 被设计成以另一种方式使用，在我看来这种方式更具可读性。**

#### 复杂的计算、对象实例化或状态突变

Java 的 Optional-API 能够从可选的。这是通过*或*来完成的，如果您试图解包的可选内容实际上是空的，那么您就有机会定义一个默认值。每当你想为*可以*在那里，但不是必须做的事情指定一个默认行为时，这是很有用的。

```
// maybeNumber represents an Optional containing an int or not.int numberOr42 = maybeNumber.orElse(42);
```

这个基本例子说明了*或*的用法。在这一点上，你保证要么得到你放入可选的数字，要么得到缺省值 42。就这么简单。

但是有意义的默认值并不总是简单的常量值。有时，可能需要以复杂和/或耗时的方式来计算有意义的默认值。这将引导您将这个复杂的计算提取到一个函数中，并像这样将其作为参数传递给 orElse。

```
int numberOrDefault = maybeNumber.orElse(complexCalculation());
```

现在你要么得到数字，要么得到计算出的默认值。看起来不错。是吗？现在你必须记住，Java 是通过值调用的概念来传递参数的。这样做的一个结果是，在给定的例子中，函数 *complexCalculation* 将**总是**被求值，即使 orElse 不会被调用。

现在想象一下这个*复杂的计算*真的很复杂，因此很耗时。T2 总是会得到评估。这将导致性能问题。另一点是，如果你在这里将更复杂的对象作为整数值来处理，这也是对内存的**浪费，因为你总是会创建一个默认值的实例。需要与否。**

但是因为我们是在 Java 的环境中，这并没有在这里结束。假设你没有一个耗时的函数，但是有一个改变状态的函数，你想在可选的为空的情况下调用它。

```
int numberOrDefault = maybeNumber.orElse(stateChangingStuff());
```

这实际上是一个更危险的例子。记住——像这样，函数**总是**被求值，不管是否需要。这意味着你总是在改变状态，即使你实际上并不想这样做。我个人对此的看法是，要不惜一切代价避免函数出现这样的状态突变。

为了能够处理上述问题，Optional-API 提供了另一种使用*或*来定义回退的方法。这个函数实际上采用了一个将被调用来生成默认值的*供应商*。

```
// without method referenceint numberOrDefault = maybeNumber.orElseGet(() -> complex());
```

```
// with method referenceint numberOrDefault = maybeNumber.orElseGet(Something::complex);
```

像这样，通过调用 *complex* 来实际生成默认值的供应商只有在*或 ElseGet* 被实际调用时才会被执行——也就是可选选项为空的情况下。像这样的*复合体*是**而不是**在不需要的时候被调用。任何复杂的计算都需要实际使用其结果。

何时使用*或*以及何时使用*或*的一般规则可以是:
如果你满足**所有三个标准**

1.  一个简单的默认值，并不难计算(像一个常数)
2.  一个不太消耗内存的默认值
3.  一个不随状态变化的默认值函数

然后使用*或其他*。
否则用 *orElseGet* 。

#### 结论(TL；博士)

*   使用 Optional 来传达预期可能缺少的值(例如，函数的返回值)。
*   避免在集合或流中使用选项。直接用现值填就行了。
*   避免将选项作为函数的参数。
*   避免 Optional::isPresent 后跟 Optional::get。
*   在 orElse 中避免复杂或状态变化的计算。为此使用 orElseGet。

#### 反馈和问题

到目前为止，你使用 Java Optional 的经验是什么？欢迎分享你的经验，讨论我们在评论区提出的观点。