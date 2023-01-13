# 如何在 Go 中编写编译器:快速指南

> 原文：<https://www.freecodecamp.org/news/write-a-compiler-in-go-quick-guide-30d2f33ac6e0/>

约瑟夫·利夫尼

# 如何在 Go 中编写编译器:快速指南

![1*xwPzWlZJoBbgrtEvwslRdg](img/dc9ca7ffeb12bbfd8a52a544fd5e6d8b.png)

**Photo by [DAVID ILIFF](https://www.flickr.com/photos/diliff/). License: [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/)**

编译器太棒了！？？？他们将理论和应用结合起来，涉及许多软件相关的主题，如解析和语言构造。就其核心而言，编译器是一种让计算机可读的程序。

这个灵感来自于我去年秋天参加的一个编译器课程和我对围棋的热爱。

这是我希望在开始编译器之旅时拥有的指南。有很多关于如何创建编译器的书籍、视频和教程。这篇文章的目标是在提供一个编译器可以做的一些事情的非平凡的例子和避免陷入困境之间取得平衡。？

结果将是一个编译器，可以执行一个小的组成语言。要签出并运行最终项目，请参见下面的说明。？

**注意:**记住，在运行这个函数时，Go 对绝对路径有严格的要求

```
cd $GOPATH/src/github.com/Lebonescogit clone https://github.com/Lebonesco/go-compiler.gitcd go-compilergo test -vgo run main.go ./examples/math.bx
```

#### 编译器概述

*   **词法分析器/语法分析器**
*   **AST 发电机**
*   **类型检查器**
*   **代码生成**

#### 语言

这篇文章的目标是让你尽快熟悉编译器，所以我们会保持语言的简单。对于**类型**，我们将使用`strings`、`integers`和`bools`。它将有**语句**，包括`func`、`if`、`else`、`let`和`return`。这应该足够让你在处理一些复杂的编译器时感到有趣了。

我构建的第一个编译器，我花了两个月的时间完成，占用了 **1000 行**的代码。为了向你展示关键的基本原理，我在这篇文章中采取了一些捷径。

我们的语言缺少的两个常见成分是`classes`和`arrays`。这些增加了额外的复杂性，我们现在没有时间。如果人们真的想知道如何处理这些元素，我会写一篇后续文章。

一些示例代码:

```
func add(a Int, b int) Int {    return a + b;}
```

```
func hello(name String) String {    return "hello:" + " " + name;}
```

```
let num = add(1, 2);let phrase = string hello("Jeff");let i = int 0;let result = "";
```

```
if (i == 2) {    result = hello("cat");} else {    result = hello("dog");}
```

```
PRINT(result);
```

#### 快速设置

我们唯一需要的外部包是`**gocc**` **，**，它将帮助构建词法分析器和解析器。

要让它运行:

```
go get github.com/goccmack/gocc
```

确保 gocc 所在的 bin 文件夹在你的`**PATH**` **:** 中

```
export PATH=$GOPATH/bin:$PATH
```

**注意:**如果你在这个阶段有问题，试着运行`go env`来确保你的`$GOROOT`和`$GOPATH`被正确分配。

很好，让我们深入研究一些代码。

#### 建造 Lexer

lexer 的工作是读取程序并输出一个由解析器使用的令牌流。每一个`Token`都包含了该令牌在语言中表示的`type`和该令牌的字符串`Literal`。

为了识别程序的各个部分，我们将使用正则表达式。然后 gocc 会把这些正则表达式转换成一个理论上可以线性时间运行的 **DFA** ( *确定性有限自动机*)。

我们将使用的符号是 **BNF** ( *巴克斯-诺尔形式*)。不要把这个和 **EBNF** ( *扩展的巴科斯-诺尔形式*)或者 **ABNF** ( *扩展的巴科斯-诺尔形式*)相混淆，它们有一些额外的特征。当在网上查看其他例子时，请记住这一点，这些例子可能使用了提供更多语法糖的其他形式。

让我们从基础开始，定义`strings`和`integers`的样子。

请注意:

*   所有标记名都必须小写
*   任何以“！”开头的键将被忽略
*   任何以' _ '开头的密钥都不会被转换成令牌
*   用' {' `expression` '} '括起来的任何表达式都可以重复 0 次或更多次

在下面的例子中，我们忽略了所有的空白，定义了一个`int`和`string_literal` `token`。

每种语言都有内置的`keywords`保留字，提供特殊的功能。但是，lexer 如何知道一个`string`是一个`keyword`还是一个用户创建的`identifier`？它通过优先考虑定义令牌的顺序来处理这个问题。因此，接下来我们来定义一下`keywords`。

我们将通过添加表达所必需的标点符号来结束这一部分。

酷！让我们通过一些**单元测试**来看看这是否真的如我们所愿。请随意将这一部分粘贴到您的 IDE 中。？

**注意:**将测试文件放在相关的子目录中通常是个好习惯，但是为了简单起见，我将所有的测试放在根目录中。

要测试我们的**扫描仪**运行:

```
$ gocc grammer.bnf$ go test -v=== RUN   TestToken--- PASS: TestToken (0.00s)PASSok      github.com/Lebonesco/compiler       0.364s
```

太好了，我们现在有一个工作的`**Lexer**`？

#### 构建解析器

建造`**Parser**`与我们建造`**Lexer**`的方式相似。我们将构建一组元素序列，当正确匹配一串标记时，它们将产生一个语法。这也将通过在线性时间内将我们的 **NFA** ( *非确定性自动机*)语法转换成 **DFA** ( *确定性有限自动机*)来运行。

让我们把事情简单化。我们的计划到底是什么？嗯，它是一堆`Statements`，其中每个`Statement`可以包含一组`Statements`和/或`Expressions`。这似乎是我们开始学习语法的好地方。

下面是程序的开始递归层次。`Statements`是零个或更多个`Statements`的序列，`Functions`是函数列表。我们的语言要求函数在其他`Statement`类型之前定义。这将减少类型检查阶段的一些麻烦。`empty`是 **BNF** 中的一个关键字，代表一个空的空间。

但是等等！什么是`Statement`？它是一个不返回值的代码单元。这包括:`if`、`let`和`return`语句。这与返回值的`Expressions`相反。我们接下来会谈到这些。

下面是我们对`Statement`和`Function`的语法。一个`StatementBlock`是一个更大的抽象，它用大括号`{` `}`封装了一列`Statements`。

接下来让我们构建我们的`Expression`，它处理所有中缀操作，例如`+`、`-`、`*`、`&` lt `;`、`&g`t；`, =` =，与`,` 与或。

几乎完成了一个完整的工作语法！让我们通过定义参数插入来总结一下。每个`function`可以接受任意数量的值。当**定义函数**时，我们需要在签名中标注参数类型，而被称为函数的**可以接受任意数量的`Expressions`。**

现在我们的解析器已经完成了，让我们给我们的驱动程序`main.go`添加一些代码。

随着编译器的进展，我们将为这个驱动程序添加更多的功能。

构建解析器的挑战之一是有许多不同的方法来定义语法。？

在进入下一节使用我们刚刚生成的输出之前，我们不会真正知道我们做得有多好。构建静态类型检查器的难度将受到我们的语法设计的强烈影响。祈祷好运吧。。

#### 测试分析器

我们将保持简单，因为此时我们的解析器仍然会产生误报。一旦我们开始研究 AST，我们就可以检查它的准确性。

```
go test -v=== RUN   TestParser--- PASS: TestParser (0.00s)=== RUN   TestToken--- PASS: TestToken (0.00s)PASSok      github.com/Lebonesco/go-compiler        7.295s
```

恭喜你。？？！现在，您已经有了一个完全正常工作的词法分析器和语法分析器。是时候进入 AST **(一个** bs *域语法树)了。*

### 抽象语法树

理解抽象语法树的最好方法是联系我们在上一篇文章中生成的解析树。一棵解析树代表了程序中与我们的语法相匹配的每一部分。

> 相比之下，ast 只包含与类型检查和代码生成相关的信息，并跳过解析文本时使用的任何其他额外内容。

如果这个定义现在没有意义，不要担心。我总是通过实际编码学得最好，所以让我们开始吧！

创建一个新目录和两个新文件。`ast.go`将包含我们的 AST 生成函数，`types.go`将拥有*树节点类型*。

```
mkdir astcd asttouch ast.gotouch types.go
```

像解析树一样，我们将从上到下定义我们的结构。每个`node`要么是一个`Statement`要么是一个`Expression`。Go 不是面向对象的，所以我们将使用一个复合模式，利用`interface`和`struct`来表示我们的`node`类别。我们的 AST 将返回一个包含程序其余部分的`Program`节点。从现在开始，我们用`TokenLiteral()`方法创建的任何结构都是`node`。如果`node`有一个`statementNode()`方法，它将适合`Statement`接口，如果它有一个`expressionNode()`方法，它就是一个`Expression`。

此外，我们将向每个 struct 字段添加`json`标记，以便在我们出于测试目的将 AST 转换为`json`时更加容易。

现在让我们开始构建基于`Statement`和`Node`接口的`Statement`结构。

***注:*** `json:"-"`表示该字段将从我们的 json 中省略。

接下来我们需要一些钩子来连接我们的`nodes`和`parser`。下面的代码允许我们的`Statement`节点适应`Node`和`Statement`接口。

然后我们需要将被解析器调用的钩子。

如你所见，**我们的大部分代码**都在检查和转换我们的输入类型。

这些钩子将被解析器在`grammar.bnf`中调用。要访问这些功能，我们需要`import "github.com/Lebonesco/go-compiler/ast`。

现在，当一段语法被识别时，它调用一个 AST 钩子并传递必要的数据来构造一个`node`。

可以想象，在如何生成 AST 方面有很大的灵活性。有一些**设计策略**可以减少 AST 中独特节点的数量。然而，当我们到达`typechecker`和`code generation`时，拥有更多的节点类型将使您的生活变得更容易。？

这部分代码很多。然而，这并不是很难，而且大部分都一样。Go 中的错误处理可能感觉有点乏味，但从长远来看，当我们犯了一个愚蠢的错误时，这是值得的。安全第一？

我们不会在错误处理上做任何疯狂的事情，因为我想节省代码行。然而，如果你愿意，你可以添加更多描述性的有用的错误。

让我们继续进行`Expressions`！

将它们装入`Node`和`Expression`接口。

并创建`Expression`钩子。

最后，将挂钩插入`parser`。

#### 测试 AST 发生器

AST 生成器的测试用例是最乏味的。但相信我，这不是你想搞砸的部分。如果您在这里遇到问题，这些错误将会转移到您的`type checker`和`code generator`中。？

在我看来，如果代码没有测试，它就是坏的

在我们的 AST 测试中，我们将构建最终的结果。为了避免比较像`tokens`这样可能产生假阴性的元素，我们在使用**深度比较**函数`reflect.DeepEqual()`进行比较之前，将我们的结果和预期输出转换成 json。

运行测试:

```
go test -v=== RUN   TestAST--- PASS: TestAST (0.00s)=== RUN   TestParser--- PASS: TestParser (0.00s)=== RUN   TestToken--- PASS: TestToken (0.00s)PASSok      github.com/Lebonesco/go-compiler        9.020s
```

通往工作编译器的半程！？而你给这个帖子一些？？？别忘了为自己走到这一步助一臂之力。

让我们给驱动程序添加更多的代码。

现在到了我最喜欢的部分，**类型检查器**。

### 类型检查器

我们的类型检查器将确保用户不会编写与我们的静态类型语言相冲突的代码。

我们的**类型检查器**的核心骨干将是跟踪标识符类型、初始化和有效类型操作的数据结构的组合。一旦我们开始处理不同级别的作用域和类，事情会变得更加复杂。然而，我们尽可能保持简单。？

首先:

```
touch checker_test.gomkdir checkercd checkertouch checker.gotouch environment.go
```

`environment.go`将包含我们所有的助手函数，这些函数将被我们的**检查器**和**代码生成器**用来访问和操作我们的变量集和相应的类型。我们的环境很简单，所以这很简单。

我们将开始设置我们所有的常量值，包括**操作类型**、**变量类型**，以及每种类型到其有效方法的**映射。**

注意:如果我们有自己语言的类，我们的检查器会处理这第三部分，而不是我们手工做。

其余的`environment.go`是基本的**获取器**和**设置器**，它们处理标识符和函数。

我们的类型检查器的基础将是一个单一的**分派**函数`checker()`，它接受一个`Node`并触发相应的检查器函数

我想节省代码行，这样我们将使用一个全局环境来存储我们的变量类型。

**注意:**如果我们允许`Block Statements`和`Functions`有自己的范围，或者如果我们遵守最佳实践，这是不可能的。

现在评估`Statements`。`Block Statements`是我们返回一个类型的唯一语句，以便处理它在函数中的情况。如果在`Block Statement`中有一个`Return Statement`，那么返回它的类型。否则，返回`Nothing_Type`。

开始评估`Expressions`。最复杂的部分将是`evalFunctionCall()`,因为它可能是一个内置函数，如`PRINT()`,也可能是用户定义的。

**注:**目前只有一个**内置**功能。然而，考虑到我们已经建立的框架，可以很容易地添加更多。

厉害！让我们给我们的驱动程序添加一小段代码。

#### 测试类型检查器

```
go test -v=== RUN   TestAST--- PASS: TestAST (0.00s)=== RUN   TestOperations--- PASS: TestOperations (0.00s)=== RUN   TestIdents--- PASS: TestIdents (0.00s)=== RUN   TestFunctions--- PASS: TestFunctions (0.00s)=== RUN   TestParser--- PASS: TestParser (0.00s)=== RUN   TestToken--- PASS: TestToken (0.00s)PASSok      github.com/Lebonesco/go-compiler        9.020s
```

我特意选择不使用这种语言，比如`classes`、`for loops`和`scope`函数。由此产生的大部分复杂性发生在`checker`和`code generator`。如果我添加这些元素，这篇文章会长很多。？

### 代码生成

我们终于到了最后阶段！？？？

为了以最少的麻烦处理最多的情况，每个`expression`将被分配给一个临时变量。这可能会使我们生成的代码看起来臃肿，但它将解决任何嵌套表达式。

臃肿的代码不会对最终的程序速度产生任何影响，因为优化器会在我们编译最终生成的 C++代码时删除任何冗余。

我们的生成器看起来类似于类型检查器。主要的区别是我们将传递一个`buffer`来存储生成的代码。

虽然我选择编译成 C++，但你可以用任何语言来替代。这本 **Go 编译器指南**的主要目的是让你能够很好地理解这些部分，然后去创建你自己的。

首先:

```
touch gen_test.gomkdir gencd gentouch gen.go
```

我们将首先导入所有必需的包，并定义三个**实用函数，** `write()`将生成的代码写入缓冲区，`check()`进行错误处理，`freshTemp()`为我们动态创建的临时变量生成**唯一的**变量名。

**注意:**在 Go 中使用`panic()`进行正常的错误处理一般是不好的做法，但是我已经厌倦了写`if statements`。

类似于**检查器**，我们的**生成器**有一个核心调度函数，它接受一个`Node`并调用相应的 **gen** 函数。

我们来生成一些`Statements`。`genProgram()`生成必要的头和`main()`函数。

生成的`Expressions`看起来与上面的代码非常相似。主要区别在于返回了一个代表该表达式的`temp`变量。这有助于我们处理更复杂的`Expression`嵌套。

最后一段代码将是我们的 C++ **内置类型。没有这个，什么都不会起作用。**

#### 测试代码生成器

### 将这一切结合在一起

我们现在将把我们的**词法分析器**、**解析器**、 **AST 生成器**、**类型检查器**和**代码生成器**组合成一个最终的可运行程序`main.go`。

**注意:**我在 Windows 上运行这个程序，所以我的 C++会编译成`main.exe`。如果这不起作用，请移除`.exe`扩展。

要查找一些要运行的测试程序，请转至`github.com/Lebonesco/go-compiler/examples`。

```
go run main.go ./example/function.bxhello Jeff3
```

现在你知道了！我们已经在 Go 中完成了一个完全正常工作的编译器！

感谢您花时间阅读这篇文章。

如果你觉得它有帮助或者有趣，请告诉我。？？。