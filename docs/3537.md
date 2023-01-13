# JavaScript 初学者手册(2020 版)

> 原文：<https://www.freecodecamp.org/news/the-complete-javascript-handbook-f26b2c71719c/>

JavaScript 是世界上最流行的编程语言之一。

我相信对于你的第一门编程语言来说，这是一个很好的选择。

我们主要使用 JavaScript 来创建

*   网站
*   网络应用
*   使用 Node.js 的服务器端应用程序

但是 JavaScript 并不局限于这些东西，它还可以用来

*   使用 React Native 等工具创建移动应用程序
*   为微控制器和物联网创建程序
*   创建智能手表应用

它基本上什么都能做。它如此受欢迎，以至于任何新出现的东西在某个时候都会有某种 JavaScript 集成。

JavaScript 是一种编程语言，它:

*   高层:它提供了一些抽象概念，允许你忽略运行它的机器的细节。它使用垃圾收集器自动管理内存，因此您可以专注于代码，而不是像其他语言(如 C)一样管理内存，并提供许多允许您处理强大变量和对象的构造。
*   动态:与静态编程语言相反，动态语言在运行时执行静态语言在编译时做的许多事情。这有利也有弊，它给了我们强大的特性，比如动态类型、后期绑定、反射、函数式编程、对象运行时变更、闭包等等。如果你不知道这些东西，也不要担心——在课程结束时，你会知道所有的东西。
*   **动态类型化**:变量不强制类型。您可以将任何类型重新分配给变量，例如，将整数分配给保存字符串的变量。
*   松散类型:与强类型相反，松散(或弱)类型语言不强制对象的类型，允许更大的灵活性，但是拒绝我们的类型安全和类型检查(这是构建在 JavaScript 之上的 TypeScript 提供的)
*   **解释型**:它通常被称为解释型语言，这意味着它在程序运行之前不需要编译阶段，这与 C、Java 或 Go 等语言不同。实际上，出于性能原因，浏览器确实会在执行 JavaScript 之前对其进行编译，但是这对于您来说是透明的——不需要额外的步骤。
*   多范例(multi-paradigm):这种语言不强制任何特定的编程范例，不像 Java 那样强制使用面向对象编程，也不像 C 那样强制使用命令式编程。您可以使用面向对象的范例，使用原型和新的(从 ES6 开始)类语法来编写 JavaScript。你可以用函数式编程风格，用它的一级函数，甚至是命令式风格(类似 C 语言)来编写 JavaScript。

如果你想知道， *JavaScript 与 Java* 没有任何关系，这是一个糟糕的名称选择，但我们不得不接受它。

## 手册摘要

1.  [一点历史](#alittlebitofhistory)
2.  [只是 JavaScript](#justjavascript)
3.  [JavaScript 语法简介](#abriefintrotothesyntaxofjavascript)
4.  [分号](#semicolons)
5.  [值](#values)
6.  [变量](#variables)
7.  [类型](#types)
8.  [表情](#expressions)
9.  [操作员](#operators)
10.  [优先规则](#precedencerules)
11.  [比较运算符](#comparisonoperators)
12.  [条件句](#conditionals)
13.  [数组](#arrays)
14.  [琴弦](#strings)
15.  [循环](#loops)
16.  [功能](#functions)
17.  [箭头功能](#arrowfunctions)
18.  [物体](#objects)
19.  [对象属性](#objectproperties)
20.  [对象方法](#objectmethods)
21.  [类](#classes)
22.  [继承](#inheritance)
23.  [异步编程和回调](#asynchonousprogrammingandcallbacks)
24.  [承诺](#promises)
25.  [异步等待](#asyncandawait)
26.  [变量范围](#variablescope)
27.  [结论](#conclusion)

> 更新:[你现在可以得到这本 JavaScript 初学者手册](https://flaviocopes.com/page/javascript-handbook/)的 PDF 和 ePub 版本。

## 一点历史

自 1995 年诞生以来，JavaScript 已经走过了漫长的道路。

它是第一种由 web 浏览器原生支持的脚本语言，由于这一点，它比任何其他语言都具有竞争优势，今天它仍然是我们可以用来构建 Web 应用程序的唯一脚本语言。

其他语言也存在，但都必须编译成 JavaScript——或者最近的 WebAssembly，但这是另一个故事了。

一开始，JavaScript 远没有今天这么强大，它主要用于花哨的动画和当时被称为动态 HTML 的奇迹。

随着网络平台不断增长的需求，JavaScript *也有*的责任增长，以适应世界上使用最广泛的生态系统之一的需求。

JavaScript 现在也广泛应用于浏览器之外。Node.js 在过去几年的兴起解放了后端开发，这曾经是 Java、Ruby、Python、PHP 和更传统的服务器端语言的领域。

JavaScript 现在也是驱动数据库和更多应用程序的语言，甚至有可能开发嵌入式应用程序、移动应用程序、电视应用程序等等。最初只是浏览器中的一种小语言，现在已经成为世界上最流行的语言。

## 只有 JavaScript

有时很难将 JavaScript 与其使用环境的特性分开。

例如，你在许多代码示例中可以找到的`console.log()`行并不是 JavaScript。相反，它是浏览器提供给我们的庞大 API 库的一部分。

同样，在服务器上，有时很难将 JavaScript 语言特性与 Node.js 提供的 API 分开。

React 或 Vue 是否提供了特定的功能？或者是“普通 JavaScript”，或者通常所说的“普通 JavaScript”？

在这本书里，我谈论 JavaScript，这种语言。

不要让你的学习过程因外界的事物而变得复杂，这些事物是由外部生态系统提供的。

## JavaScript 语法的简要介绍

在这个简短的介绍中，我想告诉你 5 个概念:

*   空格
*   区分大小写
*   文字
*   标识符
*   评论

### 空格

JavaScript 认为空白没有意义。空格和换行符可以以你喜欢的任何方式添加，至少在理论上是这样的。

在实践中，你很可能会保持一种定义良好的风格，坚持人们通常使用的风格，并使用 linter 或风格工具如*pretty*来加强这一点。

例如，我总是为每个缩进使用 2 个空格字符。

#### 区分大小写

JavaScript 是区分大小写的。名为`something`的变量不同于`Something`。

这同样适用于任何标识符。

### 文字

我们将**文字量**定义为一个写在源代码中的值，例如，一个数字、一个字符串、一个布尔值或者更高级的结构，如对象文字量或数组文字量:

```
5
'Test'
true
['a', 'b']
{color: 'red', shape: 'Rectangle'} 
```

### 标识符

一个**标识符**是一个字符序列，可以用来识别一个变量、一个函数或一个对象。它可以以字母、美元符号`$`或下划线`_`开头，并且可以包含数字。使用 Unicode，字母可以是任何允许的字符，例如表情符号？。

```
Test
test
TEST
_test
Test1
$test 
```

美元符号通常用于引用 DOM 元素。

有些名字是留给 JavaScript 内部使用的，我们不能用它们作为标识符。

### 评论

在任何编程语言中，注释都是任何程序最重要的部分之一。它们之所以重要，是因为它们让我们能够对代码进行注释，并添加重要的信息，否则阅读代码的其他人(或我们自己)将无法获得这些信息。

在 JavaScript 中，我们可以使用`//`在单行上写一个注释。JavaScript 解释器不会将`//`之后的所有内容视为代码。

像这样:

```
// a comment
true //another comment 
```

另一种类型的注释是多行注释。以`/*`开始，以`*/`结束。

介于两者之间的一切都不被视为代码:

```
/* some kind
of 
comment 

*/ 
```

## 分号

JavaScript 程序中的每一行都可以用分号结束。

我说可选，因为 JavaScript 解释器足够聪明，可以为您引入分号。

在大多数情况下，您可以在程序中完全省略分号，甚至不用考虑它。

这个事实很有争议。有些开发人员总是使用分号，有些人从不使用分号，您总是会发现使用分号的代码和不使用分号的代码。

我个人的偏好是避免分号，所以我在书中的例子不会包括它们。

## 价值观念

一个`hello`字符串是一个**值**。
像`12`这样的数字是一个**值**。

`hello`和`12`是数值。`string`和`number`是那些值的**类型**。

**类型**是值的种类，它的类别。JavaScript 中有许多不同的类型，稍后我们会详细讨论它们。每种类型都有自己的特点。

当我们需要引用一个值时，我们将它赋给一个**变量**。
变量可以有名字，值是存储在变量中的，所以我们以后可以通过变量名访问那个值。

## 变量

变量是赋给标识符的值，因此您可以在程序中引用和使用它。

这是因为 JavaScript 是松散类型的，这个概念你会经常听说。

变量必须在使用前声明。

我们有两种主要的方法来声明变量。首先是使用`const`:

```
const a = 0 
```

第二种方法是使用`let`:

```
let a = 0 
```

有什么区别？

`const`定义一个常量引用值。这意味着引用不能被改变。您不能为其重新分配新值。

使用`let`你可以给它分配一个新值。

例如，您不能这样做:

```
const a = 0
a = 1 
```

因为会得到一个错误:`TypeError: Assignment to constant variable.`。

另一方面，您可以使用`let`来完成:

```
let a = 0
a = 1 
```

不像其他一些语言如 C 语言那样表示“常数”。特别是，这并不意味着值不能改变——这意味着它不能被重新分配。如果变量指向一个对象或数组(我们将在后面看到更多关于对象和数组的内容),对象或数组的内容可以自由改变。

`const`变量必须在声明时初始化:

```
const a = 0 
```

但是`let`值可以在以后初始化:

```
let a
a = 0 
```

您可以在同一语句中一次声明多个变量:

```
const a = 1, b = 2
let c = 1, d = 2 
```

但是不能多次重新声明同一个变量:

```
let a = 1
let a = 2 
```

否则会出现“重复声明”错误。

我的建议是总是使用`const`,只有当你知道你需要给那个变量重新赋值的时候才使用`let`。为什么？因为我们的代码功能越少越好。如果我们知道一个值不能被重新赋值，那么就少了一个 bug 源。

既然我们已经看到了如何使用`const`和`let`，我想提一下`var`。

直到 2015 年，`var`是我们在 JavaScript 中声明变量的唯一方式。今天，一个现代的代码库很可能只使用`const`和`let`。我在本帖中详细介绍了一些基本差异[，但是如果你刚刚开始，你可能不会关心它们。用`const`和`let`就行了。](https://flaviocopes.com/javascript-difference-let-var/)

## 类型

JavaScript 中的变量没有附加任何类型。

它们是*非类型化的*。

一旦将某个类型的值赋给变量，以后就可以重新分配该变量来托管任何其他类型的值，而不会有任何问题。

在 JavaScript 中我们有两种主要的类型:**原始类型**和**对象类型**。

### 原始类型

原始类型有

*   数字
*   用线串
*   布尔运算
*   标志

以及两种特殊类型:`null`和`undefined`。

### 对象类型

任何非原始类型的值(字符串、数字、布尔值、空值或未定义值)都是一个**对象**。

对象类型有**属性**，也有**方法**，可以作用于这些属性。

我们稍后会详细讨论对象。

## 公式

表达式是 JavaScript 引擎可以计算并返回值的一个 JavaScript 代码单元。

表达式的复杂程度各不相同。

我们从最简单的开始，叫做基本表达式:

```
2
0.02
'something'
true
false
this //the current scope
undefined
i //where i is a variable or a constant 
```

算术表达式是接受一个变量和一个运算符(稍后将详细介绍运算符)并产生一个数字的表达式:

```
1 / 2
i++
i -= 2
i * 2 
```

字符串表达式是产生字符串的表达式:

```
'A ' + 'string' 
```

逻辑表达式利用逻辑运算符并解析为布尔值:

```
a && b
a || b
!a 
```

更高级的表达式涉及到对象、函数、数组，我后面会介绍。

## 经营者

运算符允许您获得两个简单的表达式，并将它们组合成一个更复杂的表达式。

我们可以根据操作数对操作符进行分类。有些运算符使用一个操作数。大多数使用两个操作数。只有一个运算符处理 3 个操作数。

在第一次介绍运算符时，我们将介绍您最熟悉的运算符:有两个操作数的运算符。

我在谈论变量的时候已经介绍过一个:赋值操作符`=`。使用`=`给变量赋值:

```
let b = 2 
```

现在让我们介绍另一组二元运算符，你们已经从基础数学中熟悉了。

### 加法运算符(+)

```
const three = 1 + 2
const four = three + 1 
```

如果您使用字符串，那么`+`操作符也会执行字符串连接，所以请注意:

```
const three = 1 + 2
three + 1 // 4
'three' + 1 // three1 
```

### 减法运算符(-)

```
const two = 4 - 2 
```

### 除法运算符(/)

返回第一个运算符和第二个运算符的商:

```
const result = 20 / 5 //result === 4
const result = 20 / 7 //result === 2.857142857142857 
```

如果除以零，JavaScript 不会产生任何错误，但会返回`Infinity`值(如果该值为负，则返回`-Infinity`)。

```
1 / 0 //Infinity
-1 / 0 //-Infinity 
```

### 余数运算符(%)

在许多使用案例中，余数是非常有用的计算方法:

```
const result = 20 % 5 //result === 0
const result = 20 % 7 //result === 6 
```

被零除的余数总是`NaN`，这是一个特殊的值，表示“不是一个数”:

```
1 % 0 //NaN
-1 % 0 //NaN 
```

### 乘法运算符(*)

将两个数相乘

```
1 * 2 //2
-1 * 2 //-2 
```

### 取幂运算符(**)

将第一个操作数提升到第二个操作数的幂

```
1 ** 2 //1
2 ** 1 //2
2 ** 2 //4
2 ** 8 //256
8 ** 2 //64 
```

## 优先规则

在同一行中包含多个操作符的每个复杂语句都会引入优先级问题。

举个例子:

```
let a = 1 * 2 + 5 / 2 % 2 
```

结果是 2.5，但是为什么呢？

哪些操作先执行，哪些需要等待？

有些操作比其他操作更优先。下表列出了优先规则:

| 操作员 | 描述 |
| --- | --- |
| `*``/` | 乘法/除法 |
| `+` `-` | 加法/减法 |
| `=` | 作业 |

同一级别的操作(如`+`和`-`)按照找到的顺序从左到右执行。

遵循这些规则，上面的操作可以这样解决:

```
let a = 1 * 2 + 5 / 2 % 2
let a = 2 + 5 / 2 % 2
let a = 2 + 2.5 % 2
let a = 2 + 0.5
let a = 2.5 
```

## 比较运算符

在赋值运算符和数学运算符之后，我要介绍的第三组运算符是条件运算符。

您可以使用以下运算符来比较两个数字或两个字符串。

比较运算符总是返回布尔值，即值`true`或`false`。

这些是 **disequality 比较运算符**:

*   `<`意为“小于”
*   `<=`意为“小于或等于”
*   `>`意为“大于”
*   `>=`意为“大于或等于”

示例:

```
let a = 2
a >= 1 //true 
```

除此之外，我们还有 4 个**相等运算符**。它们接受两个值，并返回一个布尔值:

*   检查是否相等
*   `!==`检查不平等

注意，我们在 JavaScript 中也有`==`和`!=`，但是我强烈建议只使用`===`和`!==`，因为它们可以防止一些微妙的问题。

## 条件式

有了比较操作符，我们就可以讨论条件句了。

一个`if`语句用于让程序根据表达式求值的结果选择一条或另一条路径。

这是一个最简单的例子，它总是执行:

```
if (true) {
  //do something
} 
```

相反，这是永远不会执行的:

```
if (false) {
  //do something (? never ?)
} 
```

条件检查您传递给它的表达式的值是真还是假。如果你传递一个数字，除非它是 0，否则它的值总是为真。如果你传递一个字符串，它总是计算为真，除非它是一个空字符串。这些是将类型转换为布尔值的一般规则。

你注意到花括号了吗？这被称为一个**块**，它被用来对一系列不同的语句进行分组。

块可以放在任何有一个语句的地方。如果在条件语句之后有一个要执行的语句，可以省略这个块，只需编写语句:

```
if (true) doSomething() 
```

但是我总是喜欢用花括号，这样更清楚。

您可以为`if`语句提供第二部分:`else`。

您附加了一个语句，如果`if`条件为假，将执行该语句:

```
if (true) {
  //do something
} else {
  //do something else
} 
```

因为`else`接受一个语句，所以可以在其中嵌套另一个 if/else 语句:

```
if (a === true) {
  //do something
} else if (b === true) {
  //do something else
} else {
  //fallback
} 
```

## 数组

数组是元素的集合。

JavaScript 中的数组本身不是一个*类型*。

数组是**对象**。

我们可以用两种不同的方法初始化一个空数组:

```
const a = []
const a = Array() 
```

第一种是使用**数组文字语法**。第二种使用数组内置函数。

您可以使用以下语法预填充数组:

```
const a = [1, 2, 3]
const a = Array.of(1, 2, 3) 
```

数组可以保存任何值，甚至是不同类型的值:

```
const a = [1, 'Flavio', ['a', 'b']] 
```

因为我们可以将数组添加到数组中，所以我们可以创建多维数组，这有非常有用的应用(例如矩阵):

```
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]

matrix[0][0] //1
matrix[2][0] //7 
```

您可以通过引用数组的索引(从零开始)来访问数组的任何元素:

```
a[0] //1
a[1] //2
a[2] //3 
```

您可以使用以下语法用一组值初始化一个新数组，首先初始化一个包含 12 个元素的数组，并用数字`0`填充每个元素:

```
Array(12).fill(0) 
```

您可以通过检查它的`length`属性来获得数组中元素的数量:

```
const a = [1, 2, 3]
a.length //3 
```

请注意，您可以设置数组的长度。如果您分配的数字大于阵列的当前容量，则不会发生任何变化。如果指定一个较小的数字，数组将在该位置被剪切:

```
const a = [1, 2, 3]
a //[ 1, 2, 3 ]
a.length = 2
a //[ 1, 2 ] 
```

### 如何将项目添加到数组中

我们可以使用`push()`方法在数组末尾添加一个元素:

```
a.push(4) 
```

我们可以使用`unshift()`方法在数组的开头添加一个元素:

```
a.unshift(0)
a.unshift(-2, -1) 
```

### 如何从数组中移除一个项目

我们可以使用`pop()`方法从数组末尾移除一个项目:

```
a.pop() 
```

我们可以使用`shift()`方法从数组的开头移除一个项目:

```
a.shift() 
```

### 如何连接两个或多个数组

您可以使用`concat()`加入多个阵列:

```
const a = [1, 2]
const b = [3, 4]
const c = a.concat(b) //[1,2,3,4]
a //[1,2]
b //[3,4] 
```

您也可以这样使用**展开**运算符(`...`):

```
const a = [1, 2]
const b = [3, 4]
const c = [...a, ...b]
c //[1,2,3,4] 
```

### 如何在数组中查找特定的项目

你可以使用数组的`find()`方法:

```
a.find((element, index, array) => {
  //return true or false
}) 
```

返回第一个返回 true 的项，如果没有找到该元素，则返回`undefined`。

一个常用的语法是:

```
a.find(x => x.id === my_id) 
```

上面的行将返回数组中第一个有`id === my_id`的元素。

`findIndex()`的工作方式与`find()`类似，但返回第一个返回 true 的项目的索引，如果没有找到，则返回`undefined`:

```
a.findIndex((element, index, array) => {
  //return true or false
}) 
```

另一种方法是`includes()`:

```
a.includes(value) 
```

如果`a`包含`value`，则返回 true。

```
a.includes(value, i) 
```

如果位置`i`后的`a`包含`value`，则返回 true。

## 用线串

字符串是一系列字符。

它也可以定义为用引号或双引号括起来的字符串文字:

```
'A string'
"Another string" 
```

我个人一直比较喜欢单引号，只在 HTML 中使用双引号来定义属性。

将字符串值赋给变量，如下所示:

```
const name = 'Flavio' 
```

您可以使用字符串的`length`属性来确定字符串的长度:

```
'Flavio'.length //6
const name = 'Flavio'
name.length //6 
```

这是一个空字符串:`''`。其长度属性为 0:

```
''.length //0 
```

可以使用`+`操作符连接两个字符串:

```
"A " + "string" 
```

您可以使用`+`运算符对*变量进行插值*:

```
const name = 'Flavio'
"My name is " + name //My name is Flavio 
```

定义字符串的另一种方法是使用模板文字，在反斜线内定义。它们对于简化多行字符串特别有用。用单引号或双引号你不能轻易定义一个多行字符串——你需要使用转义字符。

一旦用反勾号打开了模板文字，您只需按 enter 键创建一个新行，不包含特殊字符，它将按原样呈现:

```
const string = `Hey
this

string
is awesome!` 
```

模板文字也很棒，因为它们提供了一种将变量和表达式插入字符串的简单方法。

您可以通过使用`${...}`语法来实现:

```
const var = 'test'
const string = `something ${var}` 
//something test 
```

在`${}`中，你可以添加任何东西，甚至是表达式:

```
const string = `something ${1 + 2 + 3}`
const string2 = `something 
  ${foo() ? 'x' : 'y'}` 
```

## 环

循环是 JavaScript 的主要控制结构之一。

通过一个循环，我们可以自动化并重复一段代码，无论我们希望它运行多少次，甚至是无限期地运行。

JavaScript 提供了许多遍历循环方法。

我想着重讲三个方面:

*   while 循环
*   对于循环
*   为..循环的

### `while`

while 循环是 JavaScript 提供给我们的最简单的循环结构。

我们在关键字`while`后添加了一个条件，并提供了一个运行的块，直到条件的值为`true`。

示例:

```
const list = ['a', 'b', 'c']
let i = 0
while (i < list.length) {
  console.log(list[i]) //value
  console.log(i) //index
  i = i + 1
} 
```

您可以使用`break`关键字中断`while`循环，如下所示:

```
while (true) {
  if (somethingIsTrue) break
} 
```

如果您决定在一个循环的中间跳过当前的迭代，您可以使用`continue`跳到下一个迭代:

```
while (true) {
  if (somethingIsTrue) continue

  //do something else
} 
```

与`while`非常相似，我们有`do..while`循环。它与`while`基本相同，除了在代码块执行后*评估条件。*

这意味着该块总是至少执行一次*。*

示例:

```
const list = ['a', 'b', 'c']
let i = 0
do {
  console.log(list[i]) //value
  console.log(i) //index
  i = i + 1
} while (i < list.length) 
```

### `for`

JavaScript 中第二个非常重要的循环结构是循环的**。**

我们使用关键字`for`并传递一组 3 条指令:初始化、条件和增量部分。

示例:

```
const list = ['a', 'b', 'c']

for (let i = 0; i < list.length; i++) {
  console.log(list[i]) //value
  console.log(i) //index
} 
```

就像使用`while`循环一样，您可以使用`break`中断`for`循环，并使用`continue`快进到`for`循环的下一次迭代。

### `for...of`

该循环相对较新(2015 年推出)，是`for`循环的简化版本:

```
const list = ['a', 'b', 'c']

for (const value of list) {
  console.log(value) //value
} 
```

## 功能

在任何适度复杂的 JavaScript 程序中，一切都发生在函数内部。

函数是 JavaScript 的核心和基本部分。

什么是函数？

函数是一个独立的代码块。

这里有一个**函数声明**:

```
function getData() {
  // do something
} 
```

一个函数可以在任何时候通过调用来运行，就像这样:

```
getData() 
```

一个函数可以有一个或多个参数:

```
function getData() {
  //do something
}

function getData(color) {
  //do something
}

function getData(color, age) {
  //do something
} 
```

当我们可以传递一个参数时，我们调用传递参数的函数:

```
function getData(color, age) {
  //do something
}

getData('green', 24)
getData('black') 
```

注意，在第二个调用中，我传递了字符串参数`black`作为参数`color`，但是没有传递`age`。在这种情况下，`age`里面的函数就是`undefined`。

我们可以使用以下条件来检查一个值是否未定义:

```
function getData(color, age) {
  //do something
  if (typeof age !== 'undefined') {
    //...
  }
} 
```

`typeof`是一个一元运算符，允许我们检查变量的类型。

你也可以这样检查:

```
function getData(color, age) {
  //do something
  if (age) {
    //...
  }
} 
```

尽管如果`age`是`null`、`0`或空字符串，条件也将为真。

参数可以有默认值，以防它们没有被传递:

```
function getData(color = 'black', age = 25) {
  //do something
} 
```

您可以将任何值作为参数传递:数字、字符串、布尔值、数组、对象以及函数。

函数有返回值。默认情况下，函数返回`undefined`，除非您添加一个带有值的`return`关键字:

```
function getData() {
  // do something
  return 'hi!'
} 
```

我们可以在调用函数时将返回值赋给一个变量:

```
function getData() {
  // do something
  return 'hi!'
}

let result = getData() 
```

`result`现在保存一个带有`hi!`值的字符串。

您只能返回一个值。

要返回多个值，可以返回一个对象或数组，如下所示:

```
function getData() {
  return ['Flavio', 37]
}

let [name, age] = getData() 
```

函数可以在其他函数中定义:

```
const getData = () => {
  const dosomething = () => {}
  dosomething()
  return 'test'
} 
```

不能从封闭函数的外部调用嵌套函数。

你也可以从一个函数返回一个函数。

## 箭头功能

箭头函数是 JavaScript 的最新介绍。

它们经常被用来代替“常规”函数，也就是我在前一章描述的那些函数。你会发现这两种形式到处都在使用。

从视觉上看，它们允许您用更短的语法编写函数，从:

```
function getData() {
  //...
} 
```

到

```
() => {
  //...
} 
```

但是..注意这里我们没有名字。

箭头函数是匿名的。我们必须将它们赋给一个变量。

我们可以将一个常规函数赋给一个变量，就像这样:

```
let getData = function getData() {
  //...
} 
```

当我们这样做时，我们可以从函数中删除该名称:

```
let getData = function() {
  //...
} 
```

并使用变量名调用函数:

```
let getData = function() {
  //...
}
getData() 
```

我们对箭头函数也是这样做的:

```
let getData = () => {
  //...
}
getData() 
```

如果函数体只包含一条语句，可以省略括号，将所有内容写在一行中:

```
const getData = () => console.log('hi!') 
```

参数在括号中传递:

```
const getData = (param1, param2) => 
  console.log(param1, param2) 
```

如果有一个(且只有一个)参数，可以完全省略括号:

```
const getData = param => console.log(param) 
```

箭头函数允许你有一个隐式的返回值，而不需要使用`return`关键字。

当函数体中有一行语句时，它就工作了:

```
const getData = () => 'test'

getData() //'test' 
```

与常规函数一样，我们可以为参数设置默认值，以防它们没有被传递:

```
const getData = (color = 'black', 
                 age = 2) => {
  //do something
} 
```

和常规函数一样，我们只能返回一个值。

箭头函数还可以包含其他箭头函数，甚至是常规函数。

这两种类型的函数非常相似，所以你可能会问为什么要引入箭头函数。常规函数的最大区别是当它们被用作对象方法时。这是我们很快就会调查的事情。

## 目标

任何非原始类型的值(字符串、数字、布尔值、符号、空值或未定义值)都是一个**对象**。

我们是这样定义一个对象的:

```
const car = {

} 
```

这是**对象文字**语法，是 JavaScript 中最好的东西之一。

您也可以使用`new Object`语法:

```
const car = new Object() 
```

另一种语法是使用`Object.create()`:

```
const car = Object.create() 
```

也可以在大写字母的函数前使用`new`关键字初始化对象。该函数充当该对象的构造函数。在那里，我们可以初始化作为参数接收的变量，以设置对象的初始状态:

```
function Car(brand, model) {
  this.brand = brand
  this.model = model
} 
```

我们使用以下方法初始化一个新对象:

```
const myCar = new Car('Ford', 'Fiesta')
myCar.brand //'Ford'
myCar.model //'Fiesta' 
```

对象总是通过引用传递。

如果给一个变量赋予与另一个变量相同的值，如果它是一个基本类型，如数字或字符串，则它们通过值传递:

举个例子:

```
let age = 36
let myAge = age
myAge = 37
age //36 
```

```
const car = {
  color: 'blue'
}
const anotherCar = car
anotherCar.color = 'yellow'
car.color //'yellow' 
```

即使是数组或函数，本质上也是对象，所以理解它们是如何工作的非常重要。

## 对象属性

对象有**属性**，这些属性由一个与值相关联的标签组成。

属性值可以是任何类型，这意味着它可以是数组、函数，甚至可以是对象，因为对象可以嵌套其他对象。

这是我们在前一章看到的对象文字语法:

```
const car = {

} 
```

我们可以这样定义一个`color`属性:

```
const car = {
  color: 'blue'
} 
```

这里我们有一个属性名为`color`、值为`blue`的`car`对象。

标签可以是任何字符串，但是要注意特殊字符——如果我想在属性名中包含一个无效的字符作为变量名，我必须用引号将它括起来:

```
const car = {
  color: 'blue',
  'the color': 'blue'
} 
```

无效的变量名字符包括空格、连字符和其他特殊字符。

如您所见，当我们有多个属性时，我们用逗号分隔每个属性。

我们可以使用两种不同的语法来检索属性值。

第一个是**点符号**:

```
car.color //'blue' 
```

第二种方法(这是我们唯一可以用于无效名称属性的方法)是使用方括号:

```
car['the color'] //'blue' 
```

如果你访问一个不存在的属性，你将得到`undefined`值:

```
car.brand //undefined 
```

如前所述，对象可以将嵌套对象作为属性:

```
const car = {
  brand: {
    name: 'Ford'
  },
  color: 'blue'
} 
```

在这个例子中，您可以使用

```
car.brand.name 
```

或者

```
car['brand']['name'] 
```

定义对象时，可以设置属性的值。

但是您可以在以后随时更新它:

```
const car = {
  color: 'blue'
}

car.color = 'yellow'
car['color'] = 'red' 
```

您还可以向对象添加新属性:

```
car.model = 'Fiesta'

car.model //'Fiesta' 
```

给定对象

```
const car = {
  color: 'blue',
  brand: 'Ford'
} 
```

您可以使用从该对象中删除属性

```
delete car.brand 
```

## 对象方法

我在前一章谈到了函数。

函数可以被分配给一个函数属性，在这种情况下，它们被称为**方法**。

在这个例子中，`start`属性被分配了一个函数，我们可以通过使用用于属性的点语法来调用它，并在末尾加上括号:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta',
  start: function() {
    console.log('Started')
  }
}

car.start() 
```

在使用`function() {}`语法定义的方法中，我们可以通过引用`this`来访问对象实例。

在下面的例子中，我们可以使用`this.brand`和`this.model`来访问`brand`和`model`属性值:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta',
  start: function() {
    console.log(`Started 
      ${this.brand} ${this.model}`)
  }
}

car.start() 
```

注意常规函数和箭头函数之间的区别很重要——如果我们使用箭头函数，我们就不能访问`this`:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta',
  start: () => {
    console.log(`Started 
      ${this.brand} ${this.model}`) //not going to work
  }
}

car.start() 
```

这是因为**箭头函数没有绑定到对象**。

这就是正则函数经常被用作对象方法的原因。

像常规函数一样，方法可以接受参数:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta',
  goTo: function(destination) {
    console.log(`Going to ${destination}`)
  }
}

car.goTo('Rome') 
```

## 班级

我们讨论了对象，这是 JavaScript 最有趣的部分之一。

在这一章中，我们将通过引入类来更上一层楼。

什么是类？它们是为多个对象定义公共模式的一种方式。

让我们以一个人为对象:

```
const person = {
  name: 'Flavio'
} 
```

我们可以创建一个名为`Person`(注意大写`P`，使用类时的约定)的类，它有一个`name`属性:

```
class Person {
  name
} 
```

现在从这个类中，我们像这样初始化一个`flavio`对象:

```
const flavio = new Person() 
```

`flavio`被称为 Person 类的一个*实例*。

我们可以设置`name`属性的值:

```
flavio.name = 'Flavio' 
```

我们可以使用

```
flavio.name 
```

就像我们处理对象属性一样。

类可以保存属性，比如`name`，以及方法。

方法是这样定义的:

```
class Person {
  hello() {
    return 'Hello, I am Flavio'
  }
} 
```

我们可以在类的实例上调用方法:

```
class Person {
  hello() {
    return 'Hello, I am Flavio'
  }
}
const flavio = new Person()
flavio.hello() 
```

当我们创建一个新的对象实例时，有一个叫做`constructor()`的特殊方法可以用来初始化类属性。

它是这样工作的:

```
class Person {
  constructor(name) {
    this.name = name
  }

  hello() {
    return 'Hello, I am ' + this.name + '.'
  }
} 
```

注意我们如何使用`this`来访问对象实例。

现在我们可以从类中实例化一个新对象，传入一个字符串，当我们调用`hello`时，我们将得到一条个性化的消息:

```
const flavio = new Person('flavio')
flavio.hello() //'Hello, I am flavio.' 
```

当对象初始化时，调用`constructor`方法并传递任何参数。

通常方法是在对象实例上定义的，而不是在类上。

您可以将一个方法定义为`static`来允许它在类上执行:

```
class Person {
  static genericHello() {
    return 'Hello'
  }
}

Person.genericHello() //Hello 
```

这有时非常有用。

## 遗产

一个类可以扩展另一个类，使用那个类初始化的对象继承两个类的所有方法。

假设我们有一个类:

```
class Person {
  hello() {
    return 'Hello, I am a Person'
  }
} 
```

我们可以定义一个新的类`Programmer`，它扩展了`Person`:

```
class Programmer extends Person {

} 
```

现在，如果我们用类`Programmer`实例化一个新对象，它可以访问`hello()`方法:

```
const flavio = new Programmer()
flavio.hello() //'Hello, I am a Person' 
```

在子类中，可以通过调用`super()`来引用父类:

```
class Programmer extends Person {
  hello() {
    return super.hello() + 
      '. I am also a programmer.'
  }
}

const flavio = new Programmer()
flavio.hello() 
```

以上程序打印*你好，我是人。我也是程序员。*。

## 异步编程和回调

大多数时候，JavaScript 代码是同步运行的。

这意味着执行一行代码，然后执行下一行，以此类推。

一切如你所料，在大多数编程语言中是如何工作的。

然而，有时候你不能只是等待一行代码执行。

你不能只等 2 秒钟就加载一个大文件，然后完全停止程序。

你不能等一个网络资源下载完了再做别的。

JavaScript 通过使用**回调**解决了这个问题。

如何使用回调的一个最简单的例子是定时器。定时器不是 JavaScript 的一部分，而是浏览器和 Node.js 提供的，我来说一个我们有的定时器:`setTimeout()`。

`setTimeout()`函数接受两个参数:一个函数和一个数字。该数字是函数运行前必须经过的毫秒数。

示例:

```
setTimeout(() => {
  // runs after 2 seconds
  console.log('inside the function')
}, 2000) 
```

包含`console.log('inside the function')`行的功能将在 2 秒后执行。

如果在函数前添加一个`console.log('before')`，在函数后添加一个`console.log('after')`:

```
console.log('before')
setTimeout(() => {
  // runs after 2 seconds
  console.log('inside the function')
}, 2000)
console.log('after') 
```

您将在控制台中看到这种情况:

```
before
after
inside the function 
```

回调函数是异步执行的。

在浏览器中处理文件系统、网络、事件或 DOM 时，这是一种非常常见的模式。

我提到的所有东西都不是“核心”JavaScript，所以在这本手册中没有解释，但是你会在我的其他手册中找到很多例子，这些手册可以在[https://flaviocopes.com](https://flaviocopes.com)找到。

下面是我们如何在代码中实现回调。

我们定义一个接受`callback`参数的函数，这是一个函数。

当代码准备好调用回调时，我们通过传递结果来调用它:

```
const doSomething = callback => {
  //do things
  //do things
  const result = /* .. */
  callback(result)
} 
```

使用该函数的代码将如下使用它:

```
doSomething(result => {
  console.log(result)
}) 
```

## 承诺

承诺是处理异步代码的另一种方法。

正如我们在上一章中看到的，使用回调函数，我们将把一个函数传递给另一个函数调用，当这个函数完成处理时，这个函数调用就会被调用。

像这样:

```
doSomething(result => {
  console.log(result)
}) 
```

当`doSomething()`代码结束时，它调用作为参数接收的函数:

```
const doSomething = callback => {
  //do things
  //do things
  const result = /* .. */
  callback(result)
} 
```

这种方法的主要问题是，如果我们需要在其余代码中使用这个函数的结果，我们所有的代码都必须嵌套在回调函数中，如果我们必须进行 2-3 次回调，我们就进入了通常被定义为“回调地狱”的地方，其中许多级别的函数都缩进到其他函数中:

```
doSomething(result => {
  doSomethingElse(anotherResult => {
    doSomethingElseAgain(yetAnotherResult => {
      console.log(result)
    })
  }) 
}) 
```

承诺是解决这个问题的一种方法。

而不是做:

```
doSomething(result => {
  console.log(result)
}) 
```

我们以这种方式调用基于承诺的函数:

```
doSomething()
  .then(result => {
    console.log(result)
  }) 
```

我们首先调用函数，然后我们有一个在函数结束时调用的`then()`方法。

缩进并不重要，但是为了清晰起见，您会经常使用这种样式。

通常使用`catch()`方法来检测错误:

```
doSomething()
  .then(result => {
    console.log(result)
  })
  .catch(error => {
    console.log(error)
  }) 
```

现在，为了能够使用这个语法，`doSomething()`函数的实现必须有一点特殊。它必须使用承诺 API。

不要将其声明为普通函数:

```
const doSomething = () => {

} 
```

我们宣布它为一个许诺对象:

```
const doSomething = new Promise() 
```

我们在 Promise 构造函数中传递一个函数:

```
const doSomething = new Promise(() => {

}) 
```

这个函数接收两个参数。第一个是我们用来解决承诺的函数，第二个是我们用来拒绝承诺的函数。

```
const doSomething = new Promise(
  (resolve, reject) => {

}) 
```

解决一个承诺意味着成功地完成它(这导致在任何使用它的方法中调用`then()`方法)。

拒绝一个承诺意味着以一个错误结束它(这导致在任何使用它的方法中调用`catch()`方法)。

方法如下:

```
const doSomething = new Promise(
  (resolve, reject) => {
    //some code
    const success = /* ... */
    if (success) {
      resolve('ok')
    } else {
      reject('this error occurred')
    }
  }
) 
```

我们可以向 resolve 和 reject 函数传递一个我们想要的任何类型的参数。

## 异步和等待

异步函数是承诺的更高层次的抽象。

异步函数返回一个承诺，如下例所示:

```
const getData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => 
      resolve('some data'), 2000)
  })
} 
```

任何想要使用这个函数的代码都将在函数前面使用关键字`await`:

```
const data = await getData() 
```

这样，promise 返回的任何数据都将被赋给变量`data`。

在我们的例子中，数据是“一些数据”字符串。

有一个特别的警告:每当我们使用`await`关键字时，我们必须在定义为`async`的函数中这样做。

像这样:

```
const doSomething = async () => {
  const data = await getData()
  console.log(data)
} 
```

async/await duo 允许我们有一个更清晰的代码和一个简单的心智模型来处理异步代码。

正如你在上面的例子中看到的，我们的代码看起来非常简单。将其与使用承诺或回调函数的代码进行比较。

这是一个非常简单的例子，当代码非常复杂时，主要的好处就会出现。

例如，下面是如何使用 Fetch API 获取 JSON 资源，并使用 promises 解析它:

```
const getFirstUserData = () => {
  // get users list
  return fetch('/users.json') 
    // parse JSON
    .then(response => response.json()) 
    // pick first user
    .then(users => users[0]) 
    // get user data
    .then(user => 
      fetch(`/users/${user.name}`)) 
    // parse JSON
    .then(userResponse => response.json()) 
}

getFirstUserData() 
```

下面是使用 await/async 提供的相同功能:

```
const getFirstUserData = async () => {
  // get users list
  const response = await fetch('/users.json') 
  // parse JSON
  const users = await response.json() 
  // pick first user
  const user = users[0] 
  // get user data
  const userResponse = 
    await fetch(`/users/${user.name}`)
  // parse JSON
  const userData = await user.json() 
  return userData
}

getFirstUserData() 
```

## 变量作用域

当我引入变量时，我谈到了使用`const`、`let`和`var`。

作用域是对程序的一部分可见的一组变量。

在 JavaScript 中，我们有全局作用域、块作用域和函数作用域。

如果一个变量是在一个函数或块之外定义的，它就被附加到全局对象上，并且有一个全局范围，这意味着它在程序的每个部分都是可用的。

`var`、`let`和`const`声明之间有一个非常重要的区别。

在函数内部定义为`var`的变量只在该函数内部可见，类似于函数的参数。

另一方面，定义为`const`或`let`的变量仅在定义它的**块**内可见。

块是由一对花括号组成的一组指令，就像我们可以在一个`if`语句、`for`循环或函数中找到的那样。

重要的是要理解一个块并没有为`var`定义一个新的范围，但是它为`let`和`const`定义了一个新的范围。

这具有非常实际的意义。

假设您在一个函数中的`if`条件内定义了一个`var`变量

```
function getData() {
  if (true) {
    var data = 'some data'
    console.log(data) 
  }
} 
```

如果你调用这个函数，你将得到`some data`打印到控制台。

如果您尝试在`if`之后移动 console.log(数据),它仍然可以工作:

```
function getData() {
  if (true) {
    var data = 'some data'
  }
  console.log(data) 
} 
```

但是如果你把`var data`切换到`let data`:

```
function getData() {
  if (true) {
    let data = 'some data'
  }
  console.log(data) 
} 
```

你会得到一个错误:`ReferenceError: data is not defined`。

这是因为`var`是函数作用域的，这里有一个特殊的事情叫做提升。简而言之，`var`声明在运行代码之前被 JavaScript 移动到最近的函数的顶部。这个函数在 JS 内部看起来差不多是这样的:

```
function getData() {
  var data
  if (true) {
    data = 'some data'
  }
  console.log(data) 
} 
```

这就是为什么你也可以在一个函数的顶部`console.log(data)`，甚至在它被声明之前，你将得到`undefined`作为那个变量的值:

```
function getData() {
  console.log(data) 
  if (true) {
    var data = 'some data'
  }
} 
```

但是如果你切换到`let`，你会得到一个错误`ReferenceError: data is not defined`，因为提升不会发生在`let`声明中。

`const`遵循与`let`相同的规则:它是块范围的。

一开始可能会很棘手，但是一旦你意识到这种差异，你就会明白为什么与`let`相比,`var`现在被认为是一种不好的做法——它们的活动部分更少，并且它们的范围仅限于块，这也使得它们非常适合作为循环变量，因为在循环结束后它们就不再存在了:

```
function doLoop() {
  for (var i = 0; i < 10; i++) {
    console.log(i)
  }
  console.log(i)
}

doLoop() 
```

当你退出循环时，`i`将是一个值为 10 的有效变量。

如果你切换到`let`，当你尝试`console.log(i)`时会导致一个错误`ReferenceError: i is not defined`。

## 结论

非常感谢你阅读这本书。

我希望它能启发你学习更多关于 JavaScript 的知识。

关于 JavaScript 的更多内容，请查看我的博客[flaviocopes.com](https://flaviocopes.com)。

> 注意:[你可以得到这本 JavaScript 初学者手册的 PDF 和 ePub 版本](https://flaviocopes.com/page/javascript-handbook/)