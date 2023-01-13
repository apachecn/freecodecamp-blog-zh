# 面向 Android 开发人员的函数式编程—第 3 部分

> 原文：<https://www.freecodecamp.org/news/functional-programming-for-android-developers-part-3-f9e521e96788/>

作者:阿努普·考库尔

# 面向 Android 开发人员的函数式编程—第 3 部分

![E7XOleTvTpgFH6eK8m3DAnbSop2sMwY4SCqO](img/07cd1cd46e883874ada172ce0091327b.png)

在上一篇文章中，我们学习了*不变性*和*并发性*。在这个例子中，我们将看看*高阶函数*和*闭包。*

如果您尚未阅读第 2 部分，请在此阅读:

[**面向 Android 开发者的函数式编程—第二部分**](https://medium.freecodecamp.com/functional-programming-for-android-developers-part-2-5c0834669d1a)
[*在上一篇帖子中，我们学习了纯度、副作用和排序。在这一部分，我们来谈谈不变性和…*medium.freecodecamp.com](https://medium.freecodecamp.com/functional-programming-for-android-developers-part-2-5c0834669d1a)

### 高阶函数

高阶函数是可以将函数作为参数，并将函数作为结果返回的函数。很酷吧。

但是为什么会有人想这么做呢？

我们举个例子。假设我想压缩一堆文件。我想用两种方式来实现——使用 ZIP 或 RAR 格式。为了在传统 Java 中做到这一点，我们将使用类似于[策略模式](https://en.wikipedia.org/wiki/Strategy_pattern)的东西。

首先，我会创建一个定义策略的界面:

```
public interface CompressionStrategy {    void compress(List<File> files);}
```

然后我会像这样实施两个策略:

```
public class ZipCompressionStrategy implements CompressionStrategy {    @Override public void compress(List<File> files) {        // Do ZIP stuff    }}
```

```
public class RarCompressionStrategy implements CompressionStrategy {    @Override public void compress(List<File> files) {        // Do RAR stuff    }}
```

然后在运行时，我可以使用以下策略之一:

```
public CompressionStrategy decideStrategy(Strategy strategy) {    switch (strategy) {        case ZIP:            return new ZipCompressionStrategy();        case RAR:            return new RarCompressionStrategy();    }}
```

那是一大堆代码和仪式。

我们在这里尝试做的是，根据一些变量，尝试做两个不同的业务逻辑。由于业务逻辑在 Java 中不能独立存在，我们必须在类和接口中对其进行修饰。

如果能直接传入业务逻辑岂不是很棒？也就是说，如果我们可以将函数视为变量，我们是否可以像传递变量和数据一样简单地传递业务逻辑？

这正是**高阶函数的作用！**

让我们看一个高阶函数的例子。我将在这里使用 [Kotlin](https://kotlinlang.org/) ，因为 Java 8 lambdas 仍然涉及[一些创建函数接口](https://stackoverflow.com/a/13604748/1369222)的仪式，这是我们想要避免的。

```
fun compress(files: List<File>, applyStrategy: (List<File>) -> CompressedFiles){    applyStrategy(files)}
```

`compress`方法有两个参数——一个文件列表和一个名为`applyStrategy`的函数，后者是一个类型为`List<File> -> Compres` sedFiles 的函数。也就是说，它是一个接受文件列表和 `returns Compre` ssedFiles 的函数。

现在我们可以用任何一个函数调用`compress`,这个函数接受一个文件列表并返回压缩文件:

```
compress(fileList, {files -&gt; // ZIP it})
```

```
compress(fileList, {files -&gt; // RAR it})
```

好多了。好多了。

所以高阶函数允许我们传递逻辑并将代码视为数据。干净利落。

### 关闭

闭包是捕获它们的环境的函数。我们用一个例子来理解这个。假设我在一个视图上有一个点击监听器，我们想在里面打印一些值:

```
int x = 5;view.setOnClickListener(new View.OnClickListener() {    @Override public void onClick(View v) {        System.out.println(x);    }});
```

Java 不允许我们这样做，因为`x`不是最终版本。`x`在 Java 中必须是 final，因为 click listener 可以在任何时候被执行，`x`可能已经不存在了，或者它的值可能已经改变了。Java 迫使我们将这个变量设为 final，以有效地使其成为不可变的。

一旦它是不可变的，Java 就会知道无论何时执行 click listener,`x`总是为`5`。这个系统并不完美，因为`x`可以指向一个可以被改变的列表，即使这个列表的引用是相同的。

Java 没有一种机制能让函数捕获并响应超出其作用域的变量。Java 函数不能在其环境中捕获或关闭。

让我们试着在科特林做同样的事情。我们甚至不需要匿名内部类，因为我们在 Kotlin 中有一级函数:

```
var x = 5view.setOnClickListener { println(x) }
```

这在科特林完全成立。Kotlin 中的函数是*闭包。*他们可以跟踪并响应环境中的更新。

第一次触发 click listener 时，它将打印`5`。如果我们改变`x`的值，说`x = 9`并再次触发点击监听器，这次它将打印`9`。

#### 那么我能用这些闭包做什么呢？

闭包有许多漂亮的用例。任何时候你想让业务逻辑响应环境中的某种状态，你都可以使用闭包。

假设您在一个按钮上有一个点击监听器，该按钮向用户显示一个带有一串消息的对话框。如果没有闭包，那么每次消息改变时，都必须用新的消息列表初始化新的侦听器。

使用闭包，您可以将消息列表存储在某个地方，并将对列表的引用传递给监听器，就像我们上面所做的那样，监听器将总是显示最新的一组消息。

闭包也可以用来完全替换对象。这通常用在函数式语言中，你可能需要一些类似 OOP 的行为，但语言不支持它们。

让我们看一个例子:

```
class Dog {    private var weight: Int = 10    fun eat(food: Int) {        weight += food    }    fun workout(intensity: Int) {        weight -= intensity    }}
```

我有一只狗，当我们喂它的时候，它的体重会增加；当它运动的时候，它的体重会减少。我们可以用闭包来描述同样的行为吗？

```
fun main(args: Array<String>) {   dog(Action.feed)(5)}
```

```
val dog = { action: Action ->    var weight: Int = 10
```

```
when (action) {        Action.feed -> { food: Int -> weight += food; println(weight) }        Action.workout -> { intensity: Int -> weight -= intensity; println(weight) }    }}
```

```
enum class Action {    feed, workout}
```

`dog`功能接受一个`Action`，并根据动作，要么喂狗，要么让它锻炼。当我们在`main`函数中调用`dog(Action.feed)(5)`时，结果将是`15`。`dog`函数正在执行一个`feed`动作，并返回另一个将喂狗的函数。当我们将值`5`传递给这个返回的函数时，它会将狗的体重增加到`10 + 5 = 15`并打印出来。

> 所以结合闭包和高阶函数，我们可以不用 OOP 就能得到对象。

![3kHTFLmBq02uXvhIh4MNZingIOakRfXUKnNK](img/1df61ecd29afc0d7142b917daff14ab2.png)

你可能不想用真正的代码来做这件事，但是知道这是可以做到的是很有趣的。的确，被封者被称为 [*穷人的对象*](http://wiki.c2.com/?ClosuresAndObjectsAreEquivalent) *。*

### 摘要

在许多情况下，高阶函数允许我们比 OOP 更好地封装业务逻辑，我们可以传递它们，把它们当作数据。闭包捕捉它们周围的环境，帮助我们有效地使用高阶函数。

在下一部分，我们将学习函数式的错误处理。

*如果您喜欢，请点击？下面。我注意到了每一个人，我感谢他们中的每一个人。*

如果你想了解更多关于编程的想法，请关注我，这样当我写新帖时你会得到通知。