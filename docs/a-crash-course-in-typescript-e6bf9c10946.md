# 打字稿速成班

> 原文：<https://www.freecodecamp.org/news/a-crash-course-in-typescript-e6bf9c10946/>

Typescript 是 javascript 的类型化超集，旨在简化大型 javascript 应用程序的开发。Typescript 添加了常见的概念，如类、泛型、接口和静态类型，并允许开发人员使用静态检查和代码重构等工具。

### 为什么关注 Typescript:

现在的问题仍然是为什么首先应该使用 Typescript。以下是 javascript 开发人员应该考虑学习 Typescript 的一些原因。

#### 静态类型:

Javascript 是动态类型的，这意味着在运行时实例化变量之前，它不知道变量的类型，这可能会导致项目中出现问题和错误。Typescript 为 Javascript 添加了静态类型支持，如果使用正确，它会处理由变量类型的错误假设导致的错误。您仍然可以完全控制键入代码的严格程度，或者是否使用类型。

#### 更好的 IDE 支持:

与 Javascript 相比，Typescript 的最大优势之一是强大的 IDE 支持，包括智能感知、来自 Typescript 编译器的实时信息、调试等等。还有一些很棒的扩展来进一步提升您的 Typescript 开发体验。

#### 访问新的 ECMAScript 功能:

Typescript 使您能够访问最新的 ECMAScript 特性，并将它们转录到您选择的 ECMAScript 目标。这意味着您可以使用最新的工具开发应用程序，而无需担心浏览器支持。

### 什么时候应该使用它:

到目前为止，我们应该知道 Typescript 为什么有用，以及它可以在哪些方面改善我们的开发体验。但是它不是所有问题的解决方案，当然也不能阻止你自己编写可怕的代码。所以让我们来看看你应该在哪里使用 Typescript。

#### 当您有一个大的代码库时:

Typescript 是对大型代码库的一个很好的补充，因为它可以帮助您避免许多常见错误。如果有更多的开发人员在一个项目上工作，这一点尤其适用。

#### 当您和您的团队已经了解静态类型语言时:

使用 Typescript 的另一个明显的情况是，当您和您的团队已经知道静态类型语言，如 Java 和 C#并且不想改变来编写 Javascript 时。

### 设置:

要安装 typescript，我们只需要用 npm 包管理器安装它并创建一个新的 typescript 文件。

```
npm install -g typescript
```

安装之后，我们可以继续查看 typescript 为我们提供的语法和功能。

### 类型:

现在让我们看看在 Typescript 中哪些类型是可用的。

#### 编号:

Typescript 中的所有数字都是浮点值。它们都得到数字类型，包括二进制和十六进制值。

```
let num: number = 0.222;
let hex: number = 0xbeef;
let bin: number = 0b0010;
```

#### 字符串:

与其他语言一样，Typescript 使用 String 数据类型来保存文本数据。

```
let str: string = 'Hello World!';
```

您还可以使用多行字符串，并通过用反勾号``将字符串括起来来嵌入表达式

```
let multiStr: string = `A simple
multiline string!`
let expression = 'A new expression'
let expressionStr: string = `Expression str: ${ expression }`
```

#### 布尔型:

Typescript 还支持所有数据类型中最基本的数据类型，布尔型，它只能为真或假。

```
let boolFalse: boolean = false;
let boolTrue: boolean = true;
```

### 分配类型:

现在我们已经有了基本的数据类型，我们可以看看如何在 Typescript 中分配类型。基本上，你只需要在名字和冒号后面写上变量的类型。

#### 单一类型:

下面是一个将字符串数据类型赋给变量的示例:

```
let str: string = 'Hello World'
```

这对于所有数据类型都是一样的。

#### 多种类型:

您还可以使用|运算符将多个数据类型赋给变量。

```
let multitypeVar: string | number = 'String'
multitypeVar = 20
```

这里，我们使用|操作符为变量分配两种类型。现在我们可以在其中存储字符串和数字。

### 检查类型:

现在让我们看看如何检查我们的变量是否有正确的类型。我们有多种选择，但这里我只展示两种最常用的。

#### Typeof:

命令的*类型只知道基本数据类型。这意味着它只能检查变量是否是上面定义的数据类型之一。*

```
let str: string = 'Hello World!'
if(typeof str === number){
 console.log('Str is a number')
} else {
 console.log('Str is not a number')
}
```

在这个例子中，我们创建一个字符串变量，并使用 *typeof* 命令来检查 str 是否属于 Number 类型(总是为 false)。然后我们打印它是否是一个数字。

#### Instanceof:

instanceof 运算符几乎与 typeof 相同，只是它也可以检查 javascript 尚未定义的自定义类型。

```
class Human{
 name: string;
 constructor(data: string) {
  this.name = data;
 }
}
let human = new Human('Gabriel')
if(human instanceof Human){
 console.log(`${human.name} is a human`)
}
```

这里我们创建一个自定义类型，我们将在本文后面讨论，然后创建它的一个实例。之后，我们检查它是否真的是 Human 类型的变量，如果是，就在控制台中打印出来。

### 类型断言:

有时我们还需要将变量转换成特定的数据类型。当你已经指定了一个像 any 一样的通用类型，并且你想使用具体类型的函数时，这种情况经常发生。

解决这个问题有多种选择，但在这里我只分享其中的两种。

#### 作为关键字:

我们可以很容易地在变量名称后使用 as 关键字来转换变量，并在其后添加数据类型。

```
let str: any = 'I am a String'
let strLength = (str as string).length
```

这里我们将 str 变量转换为 String，这样我们就可以使用 length 参数。(如果您的 TSLINT 设置允许，甚至可以在没有强制转换的情况下工作)

#### <>操作员:

我们也可以使用<>操作符，它与关键字的效果完全相同，只是语法不同。

```
let str: any = 'I am a String'
let strLength = (<string>str).length
```

这个代码块与上面的代码块具有完全相同的功能。这只是语法上的不同。

### 数组:

Typescript 中的数组是相同对象的集合，可以用两种不同的方式创建。

#### 创建数组

**使用[]:**

我们可以通过编写后跟[]的类型来定义一个对象的数组，以表示它是一个数组。

```
let strings: string[] = ['Hello', 'World', '!']
```

在这个例子中，我们创建了一个字符串数组，它包含三个不同的字符串值。

**使用通用数组类型:**

我们也可以通过编写 Array <type>使用泛型类型定义一个数组。</type>

```
let numbers: Array<number> = [1, 2, 3, 4, 5]
```

这里我们创建了一个数字数组，它包含 5 个不同的数值。

#### 多类型数组:

此外，我们还可以使用|操作符将多个类型分配给单个数组。

```
let stringsAndNumbers: (string | number)[] = ['Age', 20]
```

在这个例子中，我们创建了一个可以保存字符串和数字值的数组。

#### 多维数组:

Typescript 还允许我们定义多维数组，这意味着我们可以将一个数组保存在另一个数组中。我们可以通过一个接一个地使用多个[]操作符来创建多维数组。

```
let numbersArray: number[][] = [[1,2,3,4,5], [6,7,8,9,10]]
```

这里我们创建了一个数组来保存另一个数字的数组。

### 图勃:

元组基本上就像一个数组，只有一个关键区别。我们可以定义每个位置可以存储什么类型的数据。这意味着我们可以通过在方括号内枚举索引来强制索引的类型。

```
let exampleTuple: [number, string] = [20, 'https://google.com'];
```

在这个例子中，我们创建了一个简单的元组，在索引 0 上有一个数字，在索引 1 上有一个字符串。这意味着如果我们试图在这个索引上放置另一个数据类型，它将抛出一个错误。

下面是一个无效元组的示例:

```
const exampleTuple: [string, number] = [20, 'https://google.com'];
```

### 枚举:

与大多数其他面向对象编程语言一样，Typescript 中的枚举允许我们定义一组命名常量。Typescript 还提供数字和基于字符串的枚举。Typescript 中的枚举是使用 enum 关键字定义的。

#### 数字:

首先，我们将查看数字枚举，其中我们将一个键值与一个索引进行匹配。

```
enum State{
 Playing = 0,
 Paused = 1,
 Stopped = 2
}
```

上面，我们定义了一个数字枚举，其中播放用 0 初始化，用 1 暂停等等。

```
enum State{
 Playing,
 Paused,
 Stopped
}
```

我们也可以让初始化器为空，Typescript 会自动从零开始索引它。

#### 字符串:

在 Typescript 中定义字符串枚举非常简单——我们只需要用字符串初始化我们的值。

```
enum State{
 Playing = 'PLAYING',
 Paused = 'PAUSED',
 Stopped = 'STOPPED'
}
```

这里我们通过用字符串初始化我们的状态来定义一个字符串枚举。

### 对象:

Typescript 中的对象是包含一组键值对的实例。这些值可以是变量、数组甚至函数。它也被认为是代表非原始类型的数据类型。

我们可以用花括号来创建对象。

```
const human = {
 firstName: 'Frank',
 age: 32,
 height: 185
};
```

这里我们创建了一个具有三个不同键值对的人类对象。

我们还可以向我们的对象添加函数:

```
const human = {
 firstName: 'Frank',
 age: 32,
 height: 185,
 greet: function(){
  console.log("Greetings stranger!")
 }
};
```

### 自定义类型:

Typescript 还允许我们定义称为 alias 的自定义类型，以便以后重用。要创建自定义类型，我们只需要使用 type 关键字并定义我们的类型。

```
type Human = {firstName: string, age: number, height: number}
```

在这个例子中，我们定义了一个名为 Human 的自定义类型，并有三个属性。现在让我们看看如何创建这种类型的对象。

```
const human: Human = {firstName: ‘Franz’, age: 32, height: 185}
```

在这里，我们创建一个自定义类型的实例，并设置所需的属性。

### 函数参数和返回类型:

Typescript 使我们能够为函数参数和返回类型设置类型。现在让我们看看使用 Typescript 定义函数的语法。

```
function printState(state: State): void {
 console.log(`The song state is ${state}`)
}
function add(num1: number, num2: number): number {
 return num1 + num2
}
```

这里我们有两个示例函数，它们都有已定义类型的参数。我们还看到，我们在右括号后定义了返回类型。

现在我们可以像在普通 javascript 中一样调用我们的函数，但是编译器会检查我们是否为函数提供了正确的参数。

```
add(2, 5)
add(1) // Error to few parameters
add(5, '2') // Error the second argument must be type number
```

#### 可选属性:

Typescript 还允许我们为函数定义可选属性。我们可以用猫王来做吗？接线员。这里有一个简单的例子:

```
function printName(firstName: string, lastName?: string) {
if (lastName) 
 console.log(`Firstname: ${firstName}, Lastname: ${lastName}`);
else console.log(`Firstname: ${firstName}`);
}
```

在这个例子中，lastName 是一个可选参数，这意味着当我们不提供它调用函数时，编译器不会出错。

```
printName('Gabriel', 'Tanner')
printName('Gabriel')
```

这意味着这两种情况都被认为是正确的。

#### 默认值:

我们可以用来使一个属性可选的第二种方法是给它分配一个默认值。我们可以通过在函数的头部直接赋值来实现。

```
function printName(firstName: string, lastName: string = 'Tanner') {
 console.log(`Firstname: ${firstName}, Lastname: ${lastName}`);
}
```

在这个例子中，我们为 lastName 分配了一个默认值，这意味着我们不需要在每次调用函数时都提供它。

### 接口:

Typescript 中的接口用于定义与我们的代码以及项目外部的代码的契约。接口只包含我们的方法和属性的声明，但不实现它们。实现方法和属性是实现接口的类的责任。

让我们看一个例子，让这些陈述更清楚一点:

```
interface Person{
 name: string
}
const person: Person = {name: 'Gabriel'}
const person2: Person = {names: 'Gabriel'} // is not assignable to type Person
```

这里我们创建了一个接口，当我们实现接口时，需要实现它的一个属性。这就是为什么第二个人称变量会抛出一个错误。

#### 可选属性:

在 Typescript 中，并非接口的所有属性都是必需的。属性也可以设置为可选的。属性名后的运算符。

```
interface Person{
 name: string
 age?: number
}
const person: Person = {name: 'Frank', age: 28}
const person2: Person = {name: 'Gabriel'}
```

这里我们创建了一个带有一个普通属性和一个可选属性的接口。接线员。这就是为什么我们两个人的初始化都是有效的。

#### 只读属性:

我们接口的一些属性也应该只在对象第一次被创建时被修改。我们可以通过将 *readonly* 关键字放在我们的属性名之前来指定这个功能。

```
interface Person{
 name: string
 readonly id: number
 age?: number
}
const person: Person = {name: 'Gabriel', id: 3127831827}
person.id = 200 // Cannot assign to id because it is readonly
```

在本示例中，id 属性是只读的，在创建对象后不能更改。

### 桶:

桶允许我们将几个导出模块合并到一个更方便的模块中。

我们只需要创建一个新文件，它将导出我们项目的多个模块。

```
export * from './person';
export * from './animal';
export * from './human';
```

这样做之后，我们可以使用一个方便的 import 语句导入所有这些模块。

```
import { Person, Animal, Human } from 'index';
```

### 仿制药:

泛型允许我们创建兼容多种类型而不是单一类型的组件。这有助于我们使我们的组件“开放”和可重用。

现在你可能想知道为什么我们不使用 any 类型来为我们的组件接受多个单一类型。让我们看一个例子来更好地理解这种情况。

我们想要一个简单的虚拟函数，它返回传递给它的参数。

```
function dummyFun(arg: any): any {
 return arg;
}
```

虽然 any 是通用的，因为它接受所有类型的参数，但它有一个很大的不同。我们正在丢失有关函数传递和返回的类型的信息。

因此，让我们看看如何在知道它返回的类型的情况下接受所有类型。

```
function dummyFun<T>(arg: T): T {
 return arg
}
```

这里我们使用了泛型参数 T，这样我们可以捕获变量类型并在以后使用它。我们还将它用作我们的返回参数，这允许我们在检查代码时看到相应的类型。

关于泛型更详细的解释，你可以看看 [Charly Poly](https://www.freecodecamp.org/news/a-crash-course-in-typescript-e6bf9c10946/undefined) 关于[泛型和重载](https://medium.com/@wittydeveloper/typescript-generics-and-overloads-999679d121cf)的文章。

### 访问修饰符:

访问修饰符控制类成员的可访问性。Typescript 支持三种访问修饰符— public、private 和 protected。

#### **公共:**

公共成员可以从任何地方获得，没有任何限制。这也是标准的修饰符，这意味着您不需要在变量前面加上 public 关键字。

#### **私人:**

私有成员只能在定义它们的类中被访问。

#### 受保护:

受保护的成员只能在定义它们的类和每个子类中访问。

### 功能区:

TSLINT 是 Typescript 的标准 linter，可以帮助我们编写干净、可维护和可读的代码。它可以用我们自己的 lint 规则、配置和格式化程序进行定制。

#### 设置:

首先，我们需要安装 typescript 和 tslint，我们可以在本地或全局这样做:

```
npm install tslint typescript --save-dev
npm install tslint typescript -g
```

之后，我们可以使用 TSLINT CLI 在我们的项目中初始化 TSLINT。

```
tslint --init
```

现在我们已经有了我们的 *tslint.json* 文件，我们准备开始配置我们的规则。

#### 配置:

TSLINT 允许用户配置自己的规则，定制代码的外观。默认情况下，tslint.json 文件如下所示，并且只使用默认规则。

```
{
"defaultSeverity": "error",
"extends": [
 "tslint:recommended"
],
"jsRules": {},
"rules": {},
"rulesDirectory": []
}
```

我们可以通过将其他规则放入 rules 对象中来添加它们。

```
"rules": {
 "no-unnecessary-type-assertion": true,
 "array-type": [true, "array"],
 "no-double-space": true,
 "no-var-keyword": true,
 "semicolon": [true, "always", "ignore-bound-class-methods"]
},
```

对于所有可用规则的概述，您可以查看官方文档。

#### 推荐阅读:

[**JavaScript DOM 简介**](https://medium.freecodecamp.org/an-introduction-to-the-javascript-dom-512463dd62ec)
[*JavaScript DOM(文档对象模型)是一个接口，允许开发者操纵内容、结构……*medium.freecodecamp.org](https://medium.freecodecamp.org/an-introduction-to-the-javascript-dom-512463dd62ec)

### 结论

你一直坚持到最后！希望本文能帮助您理解 Typescript 的基础知识，以及如何在您的项目中使用它。

如果你想阅读更多像这篇文章一样的文章，你可以访问我的[网站](https://gabrieltanner.org/)或者开始关注我的[时事通讯](https://gabrieltanner.us20.list-manage.com/subscribe/post?u=9d67fc028348a0eb71318768e&amp;id=6845ed3555)。

如果你有任何问题或反馈，请在下面的评论中告诉我。