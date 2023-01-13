# Haskell 编程语言——如何安装和使用 Haskell 教程

> 原文：<https://www.freecodecamp.org/news/haskell-programming-language-introduction/>

哈斯克尔是什么？它是用来做什么的？为什么 Haskell 程序员相对较少？怎样才能入门 Haskell？

如果你在问自己这些问题，那么这篇文章是给你的。在这篇文章中，我将回答你关于 Haskell 编程语言的问题，并为你揭开它的神秘面纱。

您将了解 Haskell 生态系统以及如何为开发而设置它。您还将了解 Haskell 的美妙之处，以及该语言在解决现实世界问题中的应用。

鉴于 Haskell 的复杂性，在深入研究 Haskell 之前，您应该了解编程的基础知识。如果您非常熟悉另一种函数式编程语言，它也有助于您更好地理解 Haskell 语法。

# 我们将涵盖的内容

*   haskell——一个恰当的介绍
*   函数式编程
*   强静态类型编程
*   哈斯克尔生态系统
*   如何设置 Haskell 开发环境
*   代码编辑器
*   黑进哈斯克尔的美丽
*   `ghci`编译器
*   python vs Haskell——最容易的 vs 最难的
*   Haskell 的主要用例
*   Web 开发:后端使用 Spock，前端使用 Elm
*   与普路托斯合作开发 Cardano 区块链

# haskell——一个恰当的介绍

Haskell 是一种全功能编程语言，支持惰性求值和类型类。

Haskell 迫使开发人员编写非常正确的代码，这是该语言的精髓。

## 函数式编程

计算机编程的世界允许不同的编程风格:函数式、命令式、面向对象。

函数式编程风格将函数视为一等公民——程序中最重要的部分。

在函数式编程语言中，函数可以作为值或数据类型传递。函数可以作为参数传递给其他函数，作为函数的结果返回，并赋给变量。这促进了单个代码库中的代码重用。

Haskell 是一种函数式编程语言，它支持这些特性。现代的 Java、C++、Go 和 C#都被函数式编程风格所束缚。

## 强静态类型语言

编程语言可以有动态或静态类型系统。在动态类型中，值在执行过程中被标记为数据类型。这在 Python 和 JavaScript 等允许数据类型之间隐式转换的语言中很常见。

在静态类型中，标记是在编译期间完成的，在低级语言中很常见。在静态类型语言中，程序在被编译成机器或字节码并运行之前，由编译器进行评估。

Haskell 是静态类型的，因为它的程序在编译和执行之前必须进行类型检查。与 Java 和 C#不同，Haskell 编译器只做一次类型检查，这提高了性能。

此外，Haskell 的类型系统被称为强类型系统是因为编译时的错误安全性。因此，Haskell 开发人员常说的一句话是，“一旦编译成功，就能工作。”

# 哈斯克尔生态系统

开始一门新语言最具挑战性的方面是完美地配置开发环境。

要安装和设置 Haskell，您需要掌握整个 Haskell 生态系统。Haskell 生态系统包含:

*   这个编译器叫 Glasgow Haskell 编译器(GHC)
*   这位口译员叫格拉斯哥·哈斯克尔口译员，(GHCi)
*   管理 Haskell 项目的堆栈工具
*   其他 Haskell 包

你可以从[www.haskell.org/downloads#platform](http://www.haskell.org/downloads#platform)那里得到一个万能的软件包。Haskell，像其他被广泛采用的编程语言一样，有一个库数据库，叫做 [Hackage](http://hackage.haskell.org/) 。

## 如何设置 Haskell 开发环境

### Linux 环境

如果您使用 Linux 机器，运行 shell 命令会更容易。下面的命令将在您的机器上安装 Haskell 平台。

```
$ sudo apt-get install haskell-platform 
```

接下来，在 Linux 命令行上键入`ghc`并点击**回车**。这将提示您是否安装 GHCi 解释器。键入 Y 并点击`Enter`。您还应该通过运行以下命令链来安装 Cabal 构建工具:

```
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:hvr/ghc
$ sudo apt install cabal-install 
```

安装后，当您在 shell 上重新运行`ghci`时，您应该会看到以下输出:

```
$ ghci
GHCi, version 8.8.4: <https://www.haskell.org/ghc/>  :? for help
Prelude> 
```

运行一个简单的算法来确认`ghci`工作正常。

### Windows 和 Mac OS

Haskell 平台可以从 Windows 和 macOS 的官方下载页面获得。

你可以在这里安装 Cabal libraries 工具。你可以在这里为 macOS [安装。](https://downloads.haskell.org/~cabal/cabal-install-3.6.2.0/cabal-install-3.6.2.0-x86_64-darwin.tar.xz)

## 代码编辑器

Haskell 没有特别适合编写程序的代码编辑器。您可以在这些代码编辑器中编写 Haskell 代码:

*   安装了 [Haskell 插件](https://plugins.jetbrains.com/plugin/8258-intellij-haskell)的 IntelliJ IDEA
*   安装了 Haskell 插件的 Visual Studio 代码
*   Haskell 模式下的 Emacs
*   Neovim

或者，你也可以在一个“愚蠢的”代码编辑器上写 Haskell 代码，比如 Notepad++和 Sublime Text，然后用 GHC 编译。

Haskell 所做的是让你以位或模块的形式编写代码，反复检查以确保每个模块都是正确的，对产品来说是完美的。因此，智能或不智能的代码编辑器对最终代码的影响微乎其微。

随意检查代码编辑器市场中的扩展，这将使编写 Haskell 源文件更加容易，如 Haskero 或 VSCode 的 Haskell Runner。

# 黑进哈斯克尔的美丽

Haskell 的妙处在于:

*   逻辑
*   像数学表达式一样阅读 Haskell 代码的便利性
*   您可以为程序指定可能的输出，剩下的工作由语言来完成
*   其自我记录的性质
*   出色的 GHCi 编译器
*   纯洁的概念

## `ghci`编译器

与其他编程语言不同，`ghci`编译器允许你交互式地使用编译器。

另外，其他编译器不允许的多行编码在`ghci`中是允许的。例如，如果您想用 Python IDLE 编写一个完整的脚本，您必须一步一步地编写，每一行都要完整。但是 Haskell 的编译器使得这样的多行编码成为可能:

```
$ ghci
GHCi, version 8.8.4: <https://www.haskell.org/ghc/>  :? for help
Prelude> :{
Prelude| 60 +
Prelude| 30
Prelude| :}
90
Prelude> 
```

## python vs Haskel——最容易的 vs 最难的

Haskell 被认为是一种很难学习和掌握的语言。另一方面，Python 被认为是最容易使用和最有用的编程语言。

鉴于许多程序员都熟悉 Python 编程，用 Python 来解释 Haskell 是合乎逻辑的:

1.  如前所述，Haskell 是一种函数式语言，而 Python 是过程式、面向对象和函数式编程风格的混合。Haskell 支持过程化编程，但是这种语言的副作用使它变得不容易。
2.  Python 和 Haskell 拥有强大的类型系统，这意味着必须进行显式转换。然而，Python 是动态类型的，而 Haskell 是静态类型的。
3.  Python 比 Haskell 慢很多。
4.  如前所述，Python 比 Haskell 更容易学习。Haskell 的学习曲线很陡，尤其是对于那些以前没有函数式编程经验的人。
5.  在库支持方面，Python 比 Haskell 有更多的库和用例。

# Haskell 的主要用例

Haskell 语言目前的主要用途包括 Web 开发和 Cardano 区块链开发。

## Haskell 用于 Web 开发

您可以使用 Haskell 进行 web 开发。就像 Python 有 Flask 和 Django，go 有 Gin、Echo 和 Bevel 一样，Haskell 有 Scotty、Servant 和 Yesod，它们都构建在 Wai 之上。

Wai 是用于管理 HTTP 请求/响应的 Haskell 包。在三个流行的 Haskell 框架中，Yesod 比其他框架更完整。

Haskell 也有用于构建 HTML 文件的`blaze-html`包，类似于`gohtml`。

## **用于 Cardano 区块链开发的 Haskell】**

Cardano 是一个新的区块链平台，它采用了利害关系证明共识算法。它是第一个允许同行评议研究的机构，创建它是为了解决比特币和以太坊的缺点。

卡达诺加密货币 ADA 是日本流行的硬币，他们在东京安装了 ADA 自动取款机。

卡尔达诺区块链系统是用普路托斯写的，这是一种基于 Haskell 的图灵完全编程语言。

普路托斯利用几种工具在卡尔达诺区块链建立智能合约。它有普路托斯应用程序后端，提供用于与智能合约交互的环境和工具。普路托斯还为内部成本计算提供了费用估算工具。

你可以在[普路托斯游乐场](https://playground.plutus.iohkdev.io/)预览和运行普路托斯代码。

由于 Haskell 是为金融行业用户构建的高保证语言，它解决了由于错误代码导致的交易交换失败的问题，以及使黑客能够窃取数字货币的多重签名失败。

# 最后的话

感谢您阅读这篇关于 Haskell 及其生态系统和主要用途的介绍。我希望你受到启发，开始学习更多的知识。

在我以后的文章中，您将能够学习 Haskell 编程的基础知识，以及更多关于它的主要用例。