# 从 0–60 开始学习 Scala:基础知识

> 原文：<https://www.freecodecamp.org/news/learning-scala-from-0-60-part-i-dc095d274b78/>

作者:难近母·普拉萨纳

# 从 0–60 开始学习 Scala:基础知识

![6Y5SO3-umRdwDXeMVVnCr7oanap9xf6RF8FF](img/8ae1b77dbf160e9d43d31966b71a89fb.png)

Photo by [Sebastian Grochowicz](https://unsplash.com/photos/p1IU4lBVW-4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/combat-plane-formation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Scala 是一种通用的高级编程语言，它在开发函数式程序和面向对象程序之间提供了一种平衡。

函数式编程是什么？简单来说，函数是函数式编程中的一等公民。为了扩展一个程序的核心功能集，我们倾向于编写额外的类来扩展某些准则/接口。在函数式编程中，函数帮助我们实现同样的目标。

我们将使用[Scala REPL](https://docs.scala-lang.org/overviews/repl/overview.html)进行所有解释。对于学习 Scala 来说，这是一个非常方便和有用的工具。它记录了关于我们的代码是如何被解释和执行的可爱的小消息。

我们先从基础开始。

#### **1。变量**

我们可以使用`val`来定义不可变变量:

```
scala> val name = "King"name: String = King
```

可变变量可以使用`var`来定义和修改:

```
scala> var name = "King"name: String = King
```

```
scala> name = "Arthur"name: String = Arthur
```

我们使用`def`给一个不可变的值分配一个标签，这个值的赋值被推迟到以后。这意味着标签的值在每次使用时都会被延迟评估。

```
scala> var name = "King"name: String = King
```

```
scala> def alias = namealias: String
```

```
scala> aliasres2: String = King
```

你观察到有趣的事情了吗？

在定义`alias`时，没有给`alias: String`赋值，因为当我们调用它时，它是延迟关联的。如果我们改变`name`的值会发生什么？

```
scala> aliasres5: String = King
```

```
scala> name = "Arthur, King Arthur"name: String = Arthur, King Arthur
```

```
scala> aliasres6: String = Arthur, King Arthur
```

#### 2.控制流

我们使用控制流语句来表达我们的决策逻辑。

你可以写一个如下的`if-else`语句:

```
if(name.contains("Arthur")) {  print("Entombed sword")} else {  print("You're not entitled to this sword")}
```

或者，可以使用`while`:

```
var attempts = 0while (attempts < 3) {  drawSword()  attempts += 1}
```

#### 3.收集

Scala 从包名称空间本身(`scala.collection.immutable`或`scala.collection.mutable`)明确区分了不可变集合和可变集合。

与不可变集合不同，可变集合可以就地更新或扩展。这使我们能够改变、添加或删除元素。

但是对不可变集合执行添加、移除或更新操作会返回一个新的集合。

不可变集合总是通过`scala._` (它也包含`scala.collection.immutable.List`的别名)自动导入。

然而，要使用可变集合，您需要显式导入`scala.collection.mutable.List`。

本着函数式编程的精神，我们将主要把我们的例子建立在语言的不可变方面，在可变方面走一点弯路。

#### **列表**

我们可以通过多种方式创建列表:

```
scala> val names = List("Arthur", "Uther", "Mordred", "Vortigern")
```

```
names: List[String] = List(Arthur, Uther, Mordred, Vortigern)
```

另一种简便的方法是使用 cons `::`操作符定义一个列表。这将一个 head 元素与列表的剩余尾部连接起来。

```
scala> val name = "Arthur" :: "Uther" :: "Mordred" :: "Vortigern" :: Nil
```

```
name: List[String] = List(Arthur, Uther, Mordred, Vortigern)
```

这相当于:

```
scala> val name = "Arthur" :: ("Uther" :: ("Mordred" :: ("Vortigern" :: Nil)))
```

```
name: List[String] = List(Arthur, Uther, Mordred, Vortigern)
```

我们可以通过索引直接访问列表元素。记住 Scala 使用从零开始的索引:

```
scala> name(2)
```

```
res7: String = Mordred
```

一些常见的助手方法包括:

`list.head`，返回第一个元素:

```
scala> name.head
```

```
res8: String = Arthur
```

`list.tail`，返回列表的尾部(除了头部之外的所有内容):

```
scala> name.tail
```

```
res9: List[String] = List(Uther, Mordred, Vortigern)
```

#### **设置**

`Set`允许我们创建一组不重复的实体。`List`默认不消除重复。

```
scala> val nameswithDuplicates = List("Arthur", "Uther", "Mordred", "Vortigern", "Arthur", "Uther")
```

```
nameswithDuplicates: List[String] = List(Arthur, Uther, Mordred, Vortigern, Arthur, Uther)
```

这里，“亚瑟”重复了两次，“乌瑟”也是。

让我们创建一个具有相同名称的集合。请注意它是如何排除重复项的。

```
scala> val uniqueNames = Set("Arthur", "Uther", "Mordred", "Vortigern", "Arthur", "Uther")
```

```
uniqueNames: scala.collection.immutable.Set[String] = Set(Arthur, Uther, Mordred, Vortigern)
```

我们可以使用`contains()`检查集合中特定元素的存在:

```
scala> uniqueNames.contains("Vortigern")res0: Boolean = true
```

我们可以使用+方法向集合中添加元素(使用`varargs`即可变长度参数)

```
scala> uniqueNames + ("Igraine", "Elsa", "Guenevere")res0: scala.collection.immutable.Set[String] = Set(Arthur, Elsa, Vortigern, Guenevere, Mordred, Igraine, Uther)
```

类似地，我们可以使用`-`方法删除元素

```
scala> uniqueNames - "Elsa"
```

```
res1: scala.collection.immutable.Set[String] = Set(Arthur, Uther, Mordred, Vortigern)
```

#### **地图**

`Map`是一个可迭代的集合，包含从`key`元素到相应的`value`元素的映射，可以创建为:

```
scala> val kingSpouses = Map( | "King Uther" -> "Igraine", | "Vortigern" -> "Elsa", | "King Arthur" -> "Guenevere" | )
```

```
kingSpouses: scala.collection.immutable.Map[String,String] = Map(King Uther -> Igraine, Vortigern -> Elsa, King Arthur -> Guenevere)
```

可以通过以下方式访问 map 中特定键的值:

```
scala> kingSpouses("Vortigern")res0: String = Elsa
```

我们可以使用`+`方法向 Map 添加一个条目:

```
scala> kingSpouses + ("Launcelot" -> "Elaine")res0: scala.collection.immutable.Map[String,String] = Map(King Uther -> Igraine, Vortigern -> Elsa, King Arthur -> Guenevere, Launcelot -> Elaine)
```

要修改现有的映射，我们只需重新添加更新的键值:

```
scala> kingSpouses + ("Launcelot" -> "Guenevere")res1: scala.collection.immutable.Map[String,String] = Map(King Uther -> Igraine, Vortigern -> Elsa, King Arthur -> Guenevere, Launcelot -> Guenevere)
```

注意，由于集合是不可变的，每个编辑操作都返回一个新的集合(`res0`，`res1`)，其中应用了更改。原来的收藏`kingSpouses`保持不变。

#### 4.函数组合子

既然我们已经学习了如何将一组实体组合在一起，那么让我们看看如何使用函数组合子在这样的集合上生成有意义的转换。

用约翰·休斯简单的话来说:

> 组合子是一个从程序片段构建程序片段的函数。

深入研究组合子是如何工作的超出了本文的范围。但是，无论如何，我们将尝试触及对这个概念的高层次理解。

我们举个例子。

假设我们想要使用我们创建的`kingSpouses`集合地图找到所有皇后的名字。

我们希望按照检查地图中每个条目的思路做一些事情。如果`key`有一个国王的名字，那么我们对它配偶的名字感兴趣(例如女王)。

我们将在 map 上使用`filter`组合子，它的签名如下:

```
collection.filter( /* a filter condition method which returns true on matching map entries */)
```

总的来说，我们将执行以下步骤来找到皇后:

*   找到以国王的名字作为键的(键，值)对。
*   仅为这样的元组提取值(queen 的名称)。

`filter`是一个函数，当给定一个(键，值)时，返回真/假。

1.  找到与国王相关的地图条目。

让我们定义我们的过滤谓词函数。由于`key_value`是(key，value)的元组，我们使用`._1`提取键(猜猜`._2`返回什么？)

```
scala> def isKingly(key_value: (String, String)): Boolean = key_value._1.toLowerCase.contains("king")
```

```
isKingly: (key_value: (String, String))Boolean
```

现在我们将使用上面定义的过滤函数来`filter` kingly 条目。

```
scala> val kingsAndQueens = kingSpouses.filter(isKingly)
```

```
kingsAndQueens: scala.collection.immutable.Map[String,String] = Map(King Uther -> Igraine, King Arthur -> Guenevere)
```

2.从过滤的元组中提取各个皇后的名字。

```
scala> kingsAndQueens.values
```

```
res10: Iterable[String] = MapLike.DefaultValuesIterable(Igraine, Guenevere)
```

让我们使用`foreach`组合符打印出皇后的名字:

```
scala> kingsAndQueens.values.foreach(println)IgraineGuenevere
```

其他一些有用的组合子有`foreach`、`filter`、`zip`、`partition`、`find`。

在学习了如何定义函数以及如何将函数作为参数传递给高阶函数中的其他函数之后，我们将再次讨论其中的一些函数。

让我们回顾一下我们所学的内容:

*   定义变量的不同方式
*   各种控制流语句
*   关于各种收藏的一些基本知识
*   对集合使用函数组合子概述

我希望这篇文章对你有用。这是学习 Scala 系列文章的第一篇。

在第二部分，我们将学习定义类、特征、封装和其他面向对象的概念。

请随时让我知道您对我如何改进内容的反馈和建议。在那之前，❤编码。