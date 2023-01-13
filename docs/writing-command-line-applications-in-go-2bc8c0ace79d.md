# 如何用 Golang 编写快速有趣的命令行应用程序

> 原文：<https://www.freecodecamp.org/news/writing-command-line-applications-in-go-2bc8c0ace79d/>

彼得·本杰明

# 如何用 Golang 编写快速有趣的命令行应用程序

![olgcVqPz3wC3t4kJ-PEpp5JKv4howZTQTiaZ](img/91b0ac77f44eaecf49ccdcaa79b317b5.png)

ASCII credit: [belbomemo](https://gist.github.com/belbomemo/b5e7dad10fa567a5fe8a)

不久前，我写了一篇关于“[在 NodeJS](https://medium.freecodecamp.com/writing-command-line-applications-in-nodejs-2cf8327eee2) 中编写命令行应用程序”的文章。

我喜欢 JavaScript，Node。JS，npm，以及整个生态系统。对我来说，没有什么比用 ES6 或 TypeScript 编写现代 JavaScript 应用程序更自然的了。

但是，最近，我需要利用多处理器(并行)并发。由于 NodeJS 的单线程事件循环，NodeJS 是并发的，但不是并行的。NodeJS 不支持“开箱即用”的并行并发。

#### 为什么要去？

Go 语言(通常被称为“Golang”)将默认使用机器的所有内核。Go 还带来以下好处:

*   类型安全(例如，不能将字符串传递给需要数字的函数，编译器会抱怨)
*   容易重构(例如，改变一个函数或变量名将会在整个项目中传播这种改变)
*   开箱即用的速度和性能
*   过程编程范式当然更容易推理
*   轻松部署(只需部署一个二进制文件即可完成！)
*   标准风格(Go 对格式有自己的看法，并附带了自动化工具)
*   …还有更多！

**注意:**对新开发人员来说重要的是**而不是**被新概念吓倒。拥抱当你面对新挑战时的那种不舒服的感觉。这意味着你在学习、成长和提高。成功开发人员的一个关键特质是*坚持*。

以下是跟随本文学习的内容:

1.  名称空间
2.  进口
3.  变量
4.  结构
5.  功能
6.  参考和指针
7.  ***如果*** 条件成立
8.  ***为*** 循环往复

### 入门指南

为了避免因为必须支持 3 个不同平台的不同命令而使本文臃肿，我将假设您正在跟进 [Cloud9](http://c9.io) 。Cloud9 是一个在线 IDE(集成开发环境)——基本上就是牛逼酱！

#### 安装

Go 已经预装在 Cloud9 的 *blank U* buntu 工作区中。所以，你可以跳过这一步。

如果你想在你的本地计算机上跟随，你可以[下载并安装 Go](https://golang.org/dl/) 。

#### 设置

Go 要求您以特定的方式设置环境。

*   你必须为你所有的围棋项目准备一个家。Go 称这个家为 ***工作区*** 。工作区必须包含 3 个目录: *bin* (针对二进制文件)、 *pkg* 和 *src* (针对源代码):

```
$ pwd
/home/ubuntu/workspace

$ mkdir {bin,src,pkg}
```

*   Go 假设每个项目都存在于自己的存储库中，所以我们需要进一步将我们的 *src* 目录组织成:

```
$ mkdir -p src/github.com/<your_github_username>/<project_name>
```

**注意:**如果你是 *gitlab* 或 *bitbucket* 用户，只需将*github.com*换成合适的名字(例如分别为*gitlab.com*或*bitbucket.org*)。

这种目录结构是有原因的。Go 没有像 NPM 或 RubyGems 那样的集中代码库。Go 可以直接从在线 VCS(版本控制系统)获取源代码，当它这样做时，它会将源代码下载到正确的路径中。例如，以下命令:

```
$ go get golang.org/x/tools/cmd/goimports
```

将告诉 Go 与 golang.org 联系，然后下载下面的源代码:

```
<your_go_workspace>/src/golang.org/x/tools/cmd/goimports
```

反过来，当您将第三方包和库导入到您的项目中时，它可以让 Go 找到它们。

*   最后，我们需要设置我们的 *GOPATH* 环境变量。在 Cloud9 Ubuntu 中，只需在*的末尾加上以下内容。巴沙尔*:

```
# in ~/.bashrc
...
export GOPATH="/home/ubuntu/workspace"
export PATH="$PATH:$GOPATH/bin"
```

然后，保存文件并在终端中运行以下命令:

```
source ~/.bashrc
```

*   要验证 Go 在 Cloud9 上运行，并且我们的 GOPATH 设置正确:

```
$ go version
go version go1.6 linux/amd64

$ go get golang.org/x/tools/cmd/goimports
$ goimports --help
usage: goimports [flags] [path ...]
...
```

有关 Golang 设置的更多信息，请访问官方“入门”文档。

### 我们走吧！

我们的目标是:建立一个最小的 CLI 应用程序来查询 [GitHub](https://api.github.com/) [用户](https://api.github.com/users)。

让我们为 Github.com 的这个项目创建一个回购协议。称之为 **gitgo** 。然后克隆它:

```
$ cd $GOPATH/src/github.com/<your_github_username>
$ git clone git@github.com:<your_github_username>/gitgo.git
```

#### 围棋入门

让我们创建我们的第一个文件，将其命名为 ***main.go*** ，并编写以下代码(不要担心，我们会覆盖每一行):

```
package main

import "fmt"

func main() {
    fmt.Println("Hello, World")
}
```

#### 打破它

```
package main
```

*   这是一个名称空间声明。名称空间只是我们对逻辑和功能进行分组的一种方式。稍后您将看到名称空间如何帮助我们。
*   ***这个词主*** 是一个关键词。它告诉 GO 编译器我们的代码打算作为一个 ***应用*** 运行，而不是作为一个 ***库*** 。不同的是 ***应用*** 被我们的用户直接使用，而 ***库*** 只能被其他代码段导入和使用。

```
Import “fmt”
```

*   导入语句。这将从[标准库](https://golang.org/pkg/)中导入“ [fmt](https://golang.org/pkg/) ”(“格式”的简称)包。

```
func main()
```

*   ***func*** 是在 GO 中定义或声明一个函数的关键字。
*   ***主*** 这个词在围棋中是一个特殊的关键词。它告诉 GO 编译器我们的应用程序从这里开始！

```
fmt.Println(“Hello, World”)
```

*   这是不言自明的。我们正在使用 **fmt** 包中的 **Println** 函数，它是我们之前导入到……嗯……打印线上的。

注意函数 **Println** 的第一个字母是大写的。这是 GO 导出变量、函数和其他东西的方式。如果函数或变量的第一个字母是大写的，这意味着它可以被外部的包或名称空间访问。

#### 让我们运行它！

```
$ go run main.go
Hello, World
```

厉害！您已经编写了第一个 GO 应用程序。刚刚发生了什么？好吧，GO 编译*和*在内存中执行应用！很快，是吧？

#### 让我们建造它！

```
$ go build     # generates executable binary in your local directory 
$ ./gitgo
Hello, World
```

太棒了。您刚刚构建了第一个 GO 应用程序。你可以只发送那个**一个**文件给你的朋友和家人，他们可以运行它并得到**同样的结果**。当然，如果他们运行的是 Windows，这个应用程序将无法工作，因为我们是为 Linux/Unix 构建的。所以，让我们为 Windows 构建它:

```
$ GOOS=windows go build -o forecaster.exe main.go
```

这就对了。现在，您已经为 Windows 创建了一个应用程序。很整洁，是吧？

事实上，您可以将该应用程序交叉编译到各种平台(例如 Windows、Linux、OS X)和架构(例如 i386、amd64)上。你可以在这里看到完整的名单:[https://golang.org/doc/install/source#environment](https://golang.org/doc/install/source#environment)

#### 我们来装吧！

如果您希望您的应用程序可以从系统的任何地方访问:

```
$ go install
```

就是这样。现在，您可以从任何地方调用您的应用程序:

```
$ gitgo
Hello, World
```

此时，将您的工作签入 GitHub 是一个好主意:

```
$ git add .
$ git commit -am "Add main.go"
$ git push
```

厉害！但到目前为止，我们的应用程序并没有真正做什么。这个练习只是为了让我们熟悉一下，让我们了解一下在 Go 中编写代码是什么感觉。

#### 现在，让我们深入了解我们的 CLI 应用程序！

我们设想与我们的应用程序的交互看起来像这样

```
$ gitgo -u pmbenjamin
# or...
$ gitgo --user pmbenjamin,defunkt
```

现在我们有了一个方向，让我们开始创建这些标志。

我们可以在 Go 中使用 ***标志*** 标准库，但是，通过反复试验和一点点搜索，您会发现标准的 ***标志*** 库不支持长标志语法(通过双划线)。它只支持单破折号。

幸运的是，有人已经用 GO 库解决了这个问题。让我们下载它:

```
$ go get github.com/ogier/pflag
```

现在，让我们将它导入到我们的项目中:

```
import (

    "github.com/ogier/pflag"
)
```

在 GO 中，import 语句的最后一个元素是我们用来访问库函数的名称空间:

```
func main() {
    pflag.SomeFunction()
}
```

如果我们喜欢使用不同的名称，我们可以在导入时为我们的包起别名:

```
import (

    flag "github.com/ogier/pflag"
)
```

这将使我们能够:

```
func main(){
    flag.SomeFunction()
}
```

也就是你在[官方示例](https://github.com/ogier/pflag)中看到的。

让我们创建保存用户输入数据的变量:

```
import (...)import (
...
)

// flags
var (
   user  string
)

func main() {
...
}
```

这里需要指出几件事:

*   我们已经在`func main()`的之外声明了我们的变量*。这允许我们在除了`func main()`之外的其他函数中引用这些变量。这对您来说可能感觉很奇怪，因为您不想污染全局名称空间。但是，相信我，这完全没问题。我们的范围仅限于当前的名称空间。*
*   Go 是一种静态类型语言，这意味着您必须指定存储在每个变量中的数据类型(因此有了`string`关键字)

现在您已经声明了变量，让我们声明您的标志并将每个标志绑定/映射到适当的变量:

```
import (
    ...
)

// flags
var (
    ...
)

func main() {
 flag.Parse()
}

func init() {
 flag.StringVarP(&user, "user", "u", "", "Search Users")
}
```

#### 打破它

```
func init()
```

*   **init** 是 GO 中的一个特殊函数。GO 按以下顺序执行应用:
    1 .进口报表
    2。包级变量/常量声明
    3。init()函数
    4。main()函数(如果项目被视为应用程序)
*   我们所做的只是初始化一次标志

```
flag.StringVarP(&user, "user", "u", "", "Search Users")
```

*   从**标志**包/库，我们使用了 **StringVarP()** 函数。
*   `StringVarP()`做 3 件事:
    1。它告诉 GO，我们将评估期待一个**字符串**，
    2。它告诉 GO 我们想要绑定一个**变量**到这个标志，以及
    3。它告诉 GO 我们想要一个符合 osix 的标志(例如双点划线和单点划线标志)
*   `StringVarP()`依次取 5 个参数:
    1。我们希望将此标志绑定到的变量，
    2。双划旗，
    3。
    四号旗。没有显式调用 flag 时使用的默认值，
    5。以及对这个标志的描述
*   `&user` 意味着我们正在传递一个**用户**变量的引用(也称为内存地址)。好了，在你开始担心引用和内存地址之前，让我们进一步分解这个概念…
*   在许多语言中，比如 JavaScript 和 Ruby，当你定义一个函数，它接受一个参数，然后调用这个函数并传递一个参数，你实际上是在创建一个变量的新副本，这个变量是你作为一个参数传递的。但是，有时您不想传递数据的副本。有时候，你需要对原始数据进行操作。
*   因此，如果你通过**值**传递数据，你实际上是在创建数据的另一个副本并传递该副本，而如果你通过**引用**(也就是它的内存地址)传递变量，那么你传递的是原始数据。
*   在 GO 中，你可以通过前置的&符号获得几乎任何东西的内存地址。

```
flag.Parse()
```

*   解析标志。

#### 让我们测试我们的工作…

```
$ go run main.go # nothing happens
$ go run main.go --help
Usage of /tmp/go-build375844749/command-line-arguments/_obj/exe/main:
  -u, --user string
        Search Users
exit status 2
```

太好了。它似乎在起作用。

注意到奇怪的 ***/tmp/go-build…*** 路径了吗？Go 在那里编译并动态执行我们的应用程序。让我们构建并测试它:

```
$ go install -v
$ gitgo --help
Usage of gitgo:
  -u, --user string
        Search Users
```

**Pro-Tip:** 构建或编译二进制文件时，总是优先选择`go install`而不是`go build`。`go install`会将非主包缓存到`$GOPATH/pkg`中，从而导致比`go build`更快的构建时间。

#### 核心逻辑

现在我们已经初始化了我们的标志，让我们开始实现一些核心功能:

```
func main() {
 // parse flags
 flag.Parse()

 // if user does not supply flags, print usage
 // we can clean this up later by putting this into its own function
  if flag.NFlag() == 0 {
     fmt.Printf("Usage: %s [options]\n", os.Args[0])
     fmt.Println("Options:")
     flag.PrintDefaults()
     os.Exit(1)
  }

  users = strings.Split(user, ",")
  fmt.Printf("Searching user(s): %s\n", users)

}
```

注意，Go 中的 if 条件句没有括号。

#### 让我们测试我们的工作…

```
$ go install
# github.com/pmbenjamin/gitgo
./main.go:15: undefined: fmt in fmt.Printf
./main.go:15: undefined: os in os.Args
./main.go:16: undefined: fmt in fmt.Println
./main.go:18: undefined: os in os.Exit
./main.go:21: undefined: fmt in fmt.Printf
./main.go:24: undefined: fmt in fmt.Printf
```

我有意想向您展示 Go 编译器在报错时的体验。重要的是，我们能够理解这些错误信息，以修复我们的代码。

所以，编译器抱怨我们使用了 **fmt** 包中的`Println` 函数，但是那个包是未定义的。与 **os** 包中的`Exit` 相同。

原来，我们只是忘了导入一些包！在一个普通的 IDE 中(例如 Atom，VS-Code，vim，emacs…等等)，你可以在你的编辑器中安装一些插件，这些插件会动态地自动导入任何缺失的包！因此，您不必手动导入它们。多棒啊。

现在，让我们自己添加正确的导入语句。还记得我们之前安装的`goimports`工具吗？

```
$ goimports -w main.go # write import stmts back in main.go!
```

重新构建和测试应用程序:

```
$ go install

$ gitgo
Usage: gitgo [options]
Options:
  -u, --user string
        Search Users

$ gitgo -u pmbenjamin        
Searching user(s): [pmbenjamin]
```

是啊！有用！

用户想查询多个用户怎么办？

```
$ gitgo -u pmbenjamin,defunkt
Searching user(s): [pmbenjamin defunkt]
```

那好像也管用！

现在，让我们开始获取实际数据。将不同的功能封装到不同的函数中，以保持代码库的整洁和模块化，这是一个很好的实践。您可以将该函数放在 **main.go** 或另一个文件中。我更喜欢一个单独的文件，因为这将使我们的应用程序模块化，可重用，易于测试。

为了节省时间，这里有代码和注释来解释。

[https://gist . github . com/petermbenjamin/8 aeece 9305 bb 44282799384365 ab 3a 3c # file-user-go](https://gist.github.com/petermbenjamin/8aeece9305bb44282799384365ab3a3c#file-user-go)

#### 要点是:

1.  在`user.go`中，我们用用户名发送一个 HTTP GET 请求
2.  然后，我们读取响应体并将数据存储在`resp`中。
3.  最佳实践是在函数失败或完成后，用`defer`语句关闭响应体进行清理。
4.  然后，我们用`json.Unmarshal`函数解析 JSON 数据，将解析后的用户数据存储在`user`变量中，并返回。
5.  在`main.go`中，我们循环遍历`users`数组，为每个用户执行`getUser()`，并输出我们想要的数据。

### 未来的增强

这个项目只是初学者的快速入门指南。我知道这个项目可以写得更有效率一点。

在我的下一篇文章中，我计划深入一些新概念，比如并发性(GoRoutines)、通道、测试、销售和编写 Go 库(而不是应用程序)。

同时，完整的项目代码可以在这里找到[。](https://github.com/pmbenjamin/gitgo)

欢迎通过公开 GitHub 问题或提交 PRs 做出贡献。