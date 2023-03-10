# Swift 中的可选类型——如何使用和打开选项

> 原文：<https://www.freecodecamp.org/news/optional-types-in-swift/>

如果您来自 Java、C++或其他面向对象的语言，那么您可能从未遇到过可选类型或“optionals”。您可能会惊讶地发现 Swift 中存在这样的概念。

选项是一个基本主题，您需要彻底理解它，以便在 Swift 中编码。如果您刚刚开始使用 Swift，并且是第一次学习可选类型，请确保阅读这篇文章直到结束。

为了理解什么是可选类型，让我们快速复习一下**常量**和**变量。**

## Swift 中的常量和变量

常量是一个数据项，它的值一旦被赋值，就不能在整个程序范围内改变。

另一方面，变量是一个其值可以无限改变的数据项。

在 Swift 中如何声明常量和变量有一些细微差别。让我们看看语法:

`<data-item-type> <var-name>:<data-type> = <value>`

这里有一个常量的例子:

`let pi:Double = 3.1415`

这里有一个变量的例子:

```
var message:String = "Hello, this is prajwal"
message = "Coding in swift is awesome" //variable value changed.
```

请注意，在我们这里用于声明常量和变量的关键字中，您使用 **let** 来声明常量，使用 **var** 来声明变量。

关键字后面是常量/变量名、冒号及其数据类型，然后是赋值。

Swift 是一种类型安全语言，这意味着您可以在不指定数据类型的情况下分配变量和常量值。它可以根据分配的值做出适当的假设，例如:

`let const:String = "String"`

也可以写成，

`let const = "String"`

而且，

`var speed:Int = 20`

可以写成，

`var speed = 35`

您可能想知道，如果您可以省略数据类型，那么还有什么必要指定它呢？嗯，你有理由怀疑这一点——但是当你使用可选类型时，你需要指定数据类型，可选类型不同于传统的数据类型。

## Swift 中的可选类型或选项

说到重点，Swift 中有哪些可选类型？

让我们看看[医生](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)对此有什么看法:

> 在可能缺少值的情况下，可以使用选项。可选表示两种可能性:要么有一个值，您可以打开可选来访问该值，要么根本没有值。

这是一个非常简单的定义。因此，可选函数基本上用于在编译时处理空值，以确保运行时不会发生崩溃。

只有当可选变量包含非空值时，才会对其执行任何操作。

可选类型可以是任何数据类型，如字符串、整数、双精度、浮点或任何用户定义的非原始数据类型(对象)。

但是，需要注意的是，可选数据类型并不等同于其基本数据类型。例如，可选字符串不同于字符串，可选整数不同于整数，等等。这是因为原始数据类型不能处理`nil`值，但是可选类型可以。

## Swift 中的可选类型语法和用法

可选类型的语法如下:

`<data-item> <var-name>:<data-type>?`

声明类似于声明常规变量，除了您添加了一个问号(？)旁边的数据类型使其成为可选类型。

启动您的 XCode 游戏，尝试运行以下代码片段:

```
let someVal:Double?
someVal = 5.6324

print(someVal)
//Output : Optional(someVal)

var str:String? = nil

str = "Hello, World!"
print(str)
//Output : Optional("Hello, World!")
```

您可以看到输出不是常规值。相反，它们是可选值。

## 如何在 Swift 中打开可选类型

对于任何操作或任务，您都不会提前使用可选类型，因为在其他地方使用可选类型之前，应该先将其转换为原语或用户定义的实例(可选字符串应该转换为字符串，可选整数应该转换为整数，等等)。

这种铸造就是所谓的**展开。**你可以通过思考薛定谔的猫来更好地理解这个概念。

在这个思维实验中，一只猫和一小瓶毒药一起被放在一个封闭的盒子里。除非你打开盒子，否则你无法确定这只猫是死是活。这意味着根据你的感知，它同时是死的和活的(零和非零)。

只有当你打开盒子(打开包装)时，你才能知道猫是否活着(零或没有)。

Swift 中的解包本质上是验证可选值是否为零，然后只有当它不为零时才执行任务。

您可以通过以下方式执行展开:

1.  使用 if else 块
2.  使用强制展开
3.  使用可选绑定
4.  使用可选链接
5.  使用零合并运算符

### 用 if-else 块展开可选类型

展开意味着确保可选值不为零。您可以通过使用一个简单的 if-else 块来实现这一点，如下所示:

```
var variable:String? //evaluates to nil

if variable != nil{
 print("Not nil")
}
else{
 print("Nil")
}
//Output : Nil
```

### 使用强制展开来展开可选类型

强制展开是非常矛盾的，因为您访问的是可选值，而不管它的值(nil 或 not nil)。如果一个 nil 可选值被展开，就会抛出一个错误，说“**在展开可选值**时意外地发现 nil。”

你应该只在预定义的环境中使用强制展开，在这种环境中，你确定可选值不会为零。

您可以使用感叹号(！)运算符是这样的:

```
var color:String?;

print(color!) // Unexpectedly found nil while unwrapping an Optional value

color = "Black";

print(color!) // Black
```

### 用可选绑定解开可选类型

可选绑定类似于使用 if-else 块。唯一细微的区别是，如果可选值不为零，则展开的值被赋给一个新的常数，并对该常数执行进一步的操作。

您可以使用 **if-let** 语句来实现这一点:

```
var password:String? = "$tr0ngp@$w0rd"

if let unwrappedpass = password {
	print("Password is \(unwrappedpass)") //Password is $tr0ngp@$w0rd
}
```

可选字符串`password`被赋予值`$tr0ngp@$$w0rd`。然后在 if-let 块中，只有当可选值`password`不为零时，可选值`password`才被展开并赋给变量`unwrappedpass`。`unwrappedpass`现在包含展开的值，可以在块的范围内使用。

```
var password:String?

if let unwrap = password{ // Block unexecuted, as optional password is nil.
	print("value is not nil")
}
```

### 用可选链接打开可选类型

在同时处理多个可选值的地方，可以使用可选链接。您可以使用它来访问和变异或分配其值取决于其他约束的牵强属性。

例如，我们从植物中获取食物，而植物又从阳光和水中获取食物。

这意味着事件之间存在连锁依赖——我们依赖植物获取食物，而植物本身依赖水和光获取食物。

可选链接包括在每个依赖项处验证实例是否为零。

让我们通过一个代码示例来看看这是如何工作的:

```
class ShipmentCar{
    var seats:Int?
    var quality:String?

    init(seatQty:Int){
        seats = seatQty
    }

    func displaySeatQuality(){
        if let seatQuality = quality{
            print("The seat covering is made of:\(seatQuality)")
        }
    }

}

class CheckSeats{
    var seatExists:ShipmentCar?

}

var obj = ShipmentCar(seatQty:4)
var obj2:CheckSeats?
obj2 = CheckSeats()
obj2?.seatExists = obj
obj2?.seatExists?.quality = "Leather" //Optional chaining, set leather quality of seats, only if seats exist.
obj.displaySeatQuality()
//Output: The seat covering is made of: Leather
```

在上面的例子中，我们有两个类，`ShipmentCar`是货物，`CheckSeats`是检查车辆中是否有座位。

我们首先创建类`ShipmentCar`的一个实例，并将参数传递为座位数 4——这意味着车辆中有 4 个座位。

此外，我们创建一个可选的类实例`CheckSeats`并实例化它。类别`CheckSeats`的属性`seatExists`是可选类型`ShipmentCar`。

接下来，我们在实例`obj2`上执行可选的链接，我们将其写为`obj2?.seatExists?.quality = "Leather"`。这意味着我们检查车内是否有座椅，如果有，我们定义座椅套的质量，即`Leather`。然后我们通过调用`displaySeatQuality`进行验证，并得到想要的结果。

**这里有一个小警告**:在上面的例子中，如果你设置`seatQty`的值为 0 而不是 4，它仍然会输出相同的结果。这意味着座椅套是由皮革制成的，即使有 0 个座位，这没有任何意义。

所以，我想提一下，这只是一个例子，这里的主要目的是强调，只有当依赖属性不为-nil 时，属性才会被赋值。

### 用 nil 合并运算符展开可选类型

nil 合并操作符作为常规 if-else 块的速记符号。如果在展开可选值时发现一个空值，将提供一个额外的默认值来代替。

```
var text:String?

var output = text ?? "Default value"

print(output) // Default value

text = "This is a string"

output = text ?? "Default String"

print(output) // This is a string
```

您还可以根据对象编写默认值。

## 包扎

你有它！在本文中，您已经了解了可选类型以及如何使用它们。

感谢您阅读本文，希望本文能帮助您理解 Swift 中的可选类型。

如果您有任何疑问，请随时通过 [Twitter](https://twitter.com/prajwalinbizz) 和/或 [LinkedIn](https://www.linkedin.com/in/prajwal-kulkarni) 联系我。我很乐意帮助你。干杯！