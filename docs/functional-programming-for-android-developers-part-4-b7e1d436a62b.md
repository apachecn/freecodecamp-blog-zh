# 面向 Android 开发人员的函数式编程—第 4 部分

> 原文：<https://www.freecodecamp.org/news/functional-programming-for-android-developers-part-4-b7e1d436a62b/>

作者:阿努普·考库尔

# 面向 Android 开发人员的函数式编程—第 4 部分

![PxbbOh1XAyXlTZmQN34-RDjsXBFGSGHYCSlC](img/32eb92da9bf567aa6a4ac8365a1455dd.png)

在上一篇文章中，我们学习了*高阶函数*和 c *损失*。在这一篇中，我们将讨论*功能性错误处理。*

如果你还没看过第 3 部分，请在这里看一下[。](https://medium.freecodecamp.org/functional-programming-for-android-developers-part-3-f9e521e96788)

### 功能错误处理

如果你一直在关注这个系列，你可能已经注意到了 FP 中一个反复出现的主题:*一切都是一个计算或者一个值。*

我们创建的函数可以选择取一些值。然后，它们执行一些有用的计算并返回其他值。

在大多数 OOP 编程中，我们将错误视为*异常——不属于计算结果的*事物。FP 的最大区别在于，我们对误差的建模就像对任何其他值一样。*错误是计算的自然组成部分，而不是凭空出现的外来邪恶力量。*

下面是 OOP 中一些典型错误处理的例子:

```
try {  result = readFromDB()} catch (e: Exception) {  result = null}
```

函数`readFromDb`的结果是一个错误，但它并不表示为计算的结果。相反，这是在 catch 块的其他地方处理的副作用。

当一个序列中有多个操作会产生错误时(这是很常见的情况)，这种代码很难解析和理解:

```
try {   data = readFromDB()   newData = doSomethingWithData(data)   isSuccess = putModifiedDataInDb(newData)} catch (e: Exception) {   // Which one of the operations caused this exception?   isSuccess = false}
```

如果这些操作中的任何一个失败了，除非在每个操作周围放置一个 try catch，否则很难发现哪个失败了。此外，如果将来的操作依赖于以前操作的结果，那么在执行将来的操作之前，您必须保持以前的操作是否失败的状态:

```
fun updateDbData(): Boolean {    try {       data = readFromDb()    } catch (e: Exception) {       data = null    }        if(data == null) {        return false    }        try {       newData = doSomethingWithData(data)    } catch (e: Exception) {        newData = null    }        if(newData == null) {        return false    }        try {       return putModifiedDataInDb(newData)    } catch (e: Exception) {        return false    }     }
```

呃。

#### 那么，我们如何以一种功能性的方式来改进它呢？

简单来说，让我们将故障建模为操作本身的结果的一部分，而不是带外过程。

许多函数式语言也有异常处理，但是通常鼓励忽略它们。对于那些失败和成功都是计算本身的自然组成部分的代码，推理起来要容易得多。

让我们看看那是什么样子。让我们创建一个 [Kotlin 数据类](https://kotlinlang.org/docs/reference/data-classes.html)来封装我们的操作结果。该类将保存成功/失败状态以及结果数据:

```
data class Result(val data: Data = Data(), val success: Boolean = false)
```

会告诉我们操作是成功还是失败。`data`是我们从计算中需要的数据。只有当`success`值为`true`(如果操作成功)时，我们才能访问它。

让我们用这个来模拟前面的例子:

```
fun updateDbData(): Boolean {       val result1 = readFromDb()       if(!result1.success) {        return false    }        val result2 = doSomethingWithData(result1.data)       if(!result2.success) {        return false    }        val result3 = putModifiedDataInDb(result2.data)       if(!result3.success) {        return false    }        return true}
```

好多了。

我们在这里所做的只是将计算错误封装为自然计算的一部分。

恭喜你！我们刚刚造了一个 ***也许是*** 单子！

一个 Maybe monad 表示一个值的存在或不存在(因此得名 *maybe* )，这使得它可以完美地表示只有成功时才有结果数据的计算。

但是等等！我们到底为什么需要`success`变量？如果我们使用 Kotlin 的可空类型来表示`Data`对象，我们可以确保只有在计算成功时数据才存在。

所以我们的`Result`类现在是:

```
data class Result(val data: Data?)
```

我们的功能变成了:

```
fun updateDbData(): Boolean {       val result1 = readFromDb()       if(result1.data == null) {        return false    }        val result2 = doSomethingWithData(result1.data)       if(result2.data == null) {        return false    }        val result3 = putModifiedDataInDb(result2.data)       if(result3.data == null) {        return false    }        return true}
```

你看到了吗？科特林的可选类型只是 ***也许是*** 单子！

![EMAzcG7SugWj0cXAYPibjcmvZJ1eUYFc9Unc](img/f6a761174c73a8750b6c84cbf78ef24a.png)

如果我们完全去掉数据类，直接使用可选类型，我们可以让代码更好:

```
fun updateDbData(): Boolean {    readFromDb()?. let {        doSomethingWithData(it)?.let {            putModifiedDataInDb(it)?.let {                return true            }        }    }        return false}
```

我们使用 Kotlin 的`?.let`语法，该语法只在 lambda 所应用的变量不为空时才执行它。

整洁吧？

### 是的，但是单子是什么？

单子是计算的构建者。它们封装了类型、组合这些类型的特定方式以及对这些类型的操作。

例如，在我们上面的函数中，Kotlin 的可选类型可以用来封装一个操作结果。当与`?.let`操作符结合使用时，它只允许 lambda 在操作结果为非空时被执行。它允许我们*表达计算结果，并根据定义的规则集以特定的方式组合它们。*

单子来自范畴理论，为了真正理解它们的数学意义，我们必须研究这个意义。然而，好消息是，我们不需要数学硕士来使用它们。一个基本的理解将满足我们的目的，随着我们对 FP 的了解越来越多，我们可以继续增加你的知识。

### 单子上的‘要么’

*也许是*单子让我们模拟一个结果的存在或不存在。但是如果一个计算可以有两个有效的结果路径呢？例如，我们可能希望为计算返回一个默认值，而不是像 null 这样的缺失值。

这就是 ***或者*** 单子出现的地方。它可以代表一个`good`和`bad`值。通常`left`用来表示失败，`right`用来表示成功。

让我们看一个例子。假设我们有一个函数`getUser`。如果没有用户，它会给我们一个匿名用户。否则它会给我们当前登录的用户。

我们可以把它建模成这样的一个*或者*:

```
data class Either(val left: AnonUser?, val right: CurrentUser?)
```

我们可以用它来调用如下代码:

```
fun updateProfileData(): Boolean {    val either = getUser()        if(either.left) {        // can't update profile for anon user        return false    }    else {        updateProfile(either.right)        return true    }}
```

### 更多资源

你可以在网上找到很多关于单子的解释，比如[这个](http://blog.jorgenschaefer.de/2013/01/monads-for-normal-programmers.html)。如果你想深入学习 FP 和单子，我推荐优秀的免费书籍[为了伟大的利益](http://learnyouahaskell.com/chapters)学你一个 Haskell。

我在这里描述的 monad 实现是不完整的，主要是为了理解概念。您可以在像 [kotlin-monads](https://github.com/h0tk3y/kotlin-monads) 这样的项目中找到更健壮的实现。

### 摘要

函数式编程鼓励将错误视为计算本身的一部分，而不是常规控制流程的异常。这使得控制流更容易理解、处理和测试。通过使用 Kotlin 的可选类型或我们自己的自定义 monad 实现来表示计算结果，我们很容易实现这一点。

在本系列的下一个也是最后一个部分，我们将通过把我们到目前为止学到的所有概念结合起来，来看看在 Android 上实现一个功能架构。

*如果您喜欢，请点击？下面。我注意到了每一个人，我感谢他们中的每一个人。*

如果你想了解更多关于编程的想法，请关注我，这样当我写新帖时你会得到通知。