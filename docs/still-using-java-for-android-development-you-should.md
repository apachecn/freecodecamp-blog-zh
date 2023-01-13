# 还在用 Java 开发你的安卓应用？试试科特林。

> 原文：<https://www.freecodecamp.org/news/still-using-java-for-android-development-you-should/>

在过去的两年里，Kotlin 是一个巨大的突破，它在任何地方都是一个热门话题，并且它的受欢迎程度还在上升。谷歌也正式推动 Kotlin 成为 Android 应用程序开发的官方语言。

但是即使这样，很多人仍然喜欢用 Java 而不是 Kotlin 来开发 Android，这是为什么呢？其中一个主要原因是，人们仍然不习惯将他们选择的主要语言从 Java 改为 Kotlin，人们害怕改变成新事物。

## 现在就开始使用 Kotlin，最简单的方法

事实上，从 Java 切换到 Kotlin 是非常容易的，因为它是一种非常灵活的语言，你可以很容易地编写 Kotlin，但习惯上，它是 Java！

![meme](img/c4d099811f6695a665b8f14e821d4e5e.png)

It looks kotlin, but actually it's Java. But I assure you, it's all gonna be okay!!

现在，当从 Java 切换到 Kotlin 时，需要注意以下几点:

**1。Java 互操作性**
Kotlin 与 Java 完全互操作，反之亦然。这意味着什么，这意味着你的 Kotlin 可以调用你的 Java 代码，这使得应用程序移植变得非常容易，你已经用 Java 编写了一半的代码？无所谓，反正代码科特林。

**2。没有更多的原始数据类型**
还记得在 java 中选择使用 **int** 还是 **Int** 要花你半天时间吗？Kotlin 中现在没有更原始的数据类型，所有东西都被包装在 Object 中。

**3。稍微改变了语法(不过不用担心很容易追上)**
像变量、函数和类声明等几个语法已经稍微改变了，但是如果你来自 Java 等 OOP 背景，就不会是这样的问题。还有，void 关键字和分号('；'))已被移除，感谢 Kotlin 移除分号！！
我将向您展示 Android 类的基本示例:

```
//Inheritance and implementation is using ':' now
class BasicActivity : AppCompatActivity() {
    //variables declaration is now variable name first then type
    var a :Int = 10 //var is for mutable
    val b :Int = 15 //val is for immutable

    /*lateinit keyword must be added for initialisation of non-
    primitive type without initial value in Kotlin*/
    lateinit var someTextView:TextView

    //Also, no more new keyword here
    var gameList:List<Integer> = ArrayList()

    //overriding is using keyword instead of 
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_basic)
    }
} 
```

4.**类型推断**
Kotlin 通过为变量提供类型推断，让生活变得更简单:

```
var a = 5 //will automatically infer this as integer
var b = "John" // will automatically infer this as String 
```

5. **[空安全](https://kotlinlang.org/docs/reference/null-safety.html)检查**
Kotlin 为了避免空指针异常提供了空安全检查。每一个可能返回空值的变量都将被标记(并且必须被标记)为`?`符号。

```
var a:String? = null
var b:String = null //not allowed 
```

它还提供空安全调用，以避免空指针异常(Java Billon 美元错误)

```
//not allowed, a might be null
a.function() 

// this will not throw null pointer, instead will continue the flow
a?.function()

//force function call, can cause NPE, use this only if 100% sure that this value will not be null
a!!.function()
```

6.**不再有静态关键字**
静态关键字现在已经被替换为[对象](https://kotlinlang.org/docs/reference/object-declarations.html)关键字，对于类和方法都是如此，更具体地说[伴随对象](https://kotlinlang.org/docs/tutorials/kotlin-for-py/objects-and-companion-objects.html)用于方法。

7。**切换用例修改**
切换用例现在已经被修改成一个新的关键字，叫做[当](https://kotlinlang.org/docs/reference/control-flow.html)的时候。哪个看起来更干净、更灵活:

```
when (x) {
    1 -> println("One")
    2 -> print("Two")
	else -> print("Others")
}
```

8.**没有更多的[检查异常](https://www.geeksforgeeks.org/checked-vs-unchecked-exceptions-in-java/)**
还记得你在 Java 里做一些操作或者造型的时候，那个红色的警告出来，告诉你在那里增加异常检查吗？现在在科特林已经被移除了。现在，你的 IDE 不会再强迫你做任何异常了！！

这就是你需要开始用 Kotlin 开发的部分，但是是地道的 Java。
现在考科特林不难吧？现在 Kotlin 看起来和 Java 没那么大区别。

* * *

## Kotlin 提供的功能

![download](img/1e4ab5d46535c69cc9b85d806c90b8a9.png)

现在，在了解了 Kotlin 和 Java 的区别之后，我们还需要了解 Kotlin 提供的特性，以及如何慢慢开始使用 Kotlin，Kotlin 的方式:

**1。灵活性**
我喜欢科特林的一个主要原因是语言的灵活性。你是 OOP 纯粹主义者吗？你是不是跃跃欲试，想试试当前热门话题[函数式编程](https://en.wikipedia.org/wiki/Functional_programming)？看在对*编码*上帝的份上，你可以在 Kotlin 两者兼得！尽管没有完全提供 FP 提供的所有特性，但足以支持它。

**2。 [Lambda 支持](https://kotlinlang.org/docs/reference/lambdas.html)**
是的，我知道现在 Java 8 支持 Lambda，但 Kotlin 是第一个这样做的，它在这方面做得很好。当然，开始时过渡到使用 Lambda 函数会很困难，但是如前所述，Kotlin 在这方面非常灵活。所以嘿，你说了算！
一些 Android 库也启用了 Lambda 支持，比如`View.OnClickListener`(如果你来自 Android 背景，我敢肯定这个方法你已经很熟悉了) :

```
val randomButton : Button

//using lambda function as argument
randomButton.setOnClickListener{v -> doRandomStuff(v) }
```

**3。[数据类](https://kotlinlang.org/docs/reference/data-classes.html)**
Kotlin 已经提供了模型/实体类的替代品，称为[数据](https://kotlinlang.org/docs/reference/data-classes.html)类。它消除了对多余的 setter getter 的需要，并且还内置了 equal 方法和 toString 函数，您不必为它创建一个。它也很容易利用:

```
data class Person(
    val id:String,
    val name:String = "John Doe" //Default Value
)

//Initialisation block
var person = Person("id","Not John")
```

4. **[扩展函数](https://kotlinlang.org/docs/reference/extensions.html)**
Kotlin 允许使用扩展函数，使得代码更具可读性。此外，它将使您能够在不修改类本身的情况下为类提供功能:

```
fun Int.superOperation(anotherInt:Int):Int {
    val superNumber = this * anotherInt + anotherInt
    return superNumber
}

//you can now call the functions
val someNumber = 5
val superNumber = someNumber.superOperation(10) //65
```

5. **[命名和默认参数](https://kotlinlang.org/docs/reference/functions.html)**
和 C#一样，Kotlin 也支持为其方法命名和默认参数。它完全消除了在每个参数中传递值的需要。你可以选择你想要修改的值，瞧！不再有为 10 个函数调用传递相同的特定值的麻烦！

```
//variable declaration
fun someOperations(var x: Int, var y:Int, var z:Int = 1){
	return x+y+z
}

someOperations(1,2) //return 4
someOpeartions(y = 1, x = 1) //return 3
```

6. [**引用相等**](https://kotlinlang.org/docs/reference/equality.html)
Kotlin 也附带了引用相等(' === ')，它会检查两个变量当前是否引用同一个对象。

```
var a = 10
var b = 10
a == b //true
a === b //true also, primitive type object equation will only check its value

data class Person (val name: String)

val p1 = Person("John")
val p2 = Person("John")

p1 == p2 //true
p1 === p2 //false
```

7. [**智能转换**](https://kotlinlang.org/docs/reference/typecasts.html)
kot Lin 现在提供了更简单、更易读的语法来进行转换或类型检查，而不是使用实例:

```
//type checking
//kotlin smart cast mechanism will also automatically change the object into corresponding type if it passes the type checking
if (shape is Circle) {
    println(“it’s a Circle”)
    shape.radius //automatically be casted as Circle
} else if (shapeObject is Square) {
    println(“it’s a Square”)
} else if (shapeObject is Rectangle) {
    print(“it’s a Rectangle”)
}
```

Kotlin 也有`as`关键字来支持显式转换:

```
//It's considered unsafe since it behaves simillarly like static casting
var anotherShape = shape as Circle

//This can be used instead for safe casting
var safeShape = shape as? Circle
```

8. **[协程](https://kotlinlang.org/docs/reference/coroutines-overview.html)**
异步调用在 Java 中一直是个麻烦事。制作 Thread 占用了太多的代码空间，使得代码不可读。协程已经取代了传统的线程操作，提供了一个干净而健壮的代码:

```
import kotlinx.coroutines.*

fun main() {
    // launch a new coroutine in background and continue
    GlobalScope.launch { 
        // non-blocking delay for 1 second (default time unit is ms)
        delay(1000L) 

        // print after delay
        println("World!") 
    }
    // main thread continues while coroutine is delayed
    println("Hello,") 

    // block main thread for 2 seconds to keep JVM alive
    Thread.sleep(2000L) 
}
```

协程还支持更复杂的操作，如作业加入、通道、共享上下文和父类。详情及更多用法可在此阅读[。](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)

## 包装它

总而言之，开始使用 Kotlin 非常容易，你可以像编写 Java 一样编写 Kotlin。另外， **Kotlin 提供了几个特性，可以让你在使用 Java 时更有优势。**

当然，Java 目前仍然很强大。但是开始学习新的东西永远不会错。作为程序员，我们需要拥抱终生学习，永远不要停止学习。Kotlin 最大的优点是从 Java 中移植非常容易。

感谢阅读。我希望你喜欢，并希望这将是一个很好的使用！祝你探索科特林好运，下次再见。