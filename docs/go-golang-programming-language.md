# Go (Golang)编程语言

> 原文：<https://www.freecodecamp.org/news/go-golang-programming-language/>

****Go**** (或 ****golang**** )是 2007 年由 Robert Griesemer、Rob Pike 和 Ken Thompson 在谷歌创造的一种编程语言。它是一种继承了 Algol 和 c 语言传统的编译型静态语言。

Go 增加了垃圾收集、有限的结构化类型、内存安全和 CSP 风格的并发编程特性。最初由 Google 开发的编译器和其他语言工具都是免费开源的。

围棋的受欢迎程度正在迅速提高。这是构建 web 应用程序的绝佳选择。

欲了解更多信息，请访问 Go 的主页。想快速浏览一下围棋吗？在这里查看文件。

现在让我们看看如何安装和开始使用 Go。

## **安装**

### 用自制软件安装 Golang:

```
$ brew update
$ brew install golang
```

### **使用 tarball 在 MacOS 上安装 Go**

#### **链接到 tarball**

你可以从 [golang 下载页面](https://golang.org/dl/)的最新稳定部分获得 MacOS tarball 存档的链接。

#### **安装过程**

在这个安装过程中，我们将使用撰写本文时的最新稳定版本(go 1.9.1)。对于较新或较旧的版本，只需替换第一步中的链接。查看 [golang 下载页面](https://golang.org/dl/)查看当前可用的版本。

##### **安装 Go 1.9.1**

```
$ curl -O https://storage.googleapis.com/golang/go1.9.1.darwin-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.9.1.darwin-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
```

### **用 apt** 在 Ubuntu 上安装 Golang

使用 Ubuntu 的源码包管理器(apt)是安装 Go 最简单的方法之一。你不会得到最新的稳定版本，但为了学习这个应该足够了。

```
$ sudo apt-get update
$ sudo apt-get install golang-go
```

#### **检查 Go 的安装和版本**

要检查 go 是否安装成功，请运行:

```
$ go version
> go version go1.9.1 linux/amd64
```

这将打印安装到控制台的 Go 版本。如果你看到 Go 的一个版本，你就知道安装很顺利。

## 设置工作空间

### 添加环境变量:

首先，你需要告诉 Go 你工作空间的位置。

我们将在 shell 配置中添加一些环境变量。其中一个文件位于您的主目录 bash_profile、bashrc 或。zshrc(哦，我的 Zsh 军队)

```
$ vi .bashrc
```

然后添加这些行来导出所需的变量

#### 这其实是你的。bashrc 文件

```
export GOPATH=$HOME/go-workspace # don't forget to change your path correctly!
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
```

## 创建您的工作空间

### 创建工作区目录树:

```
$ mkdir -p $GOPATH $GOPATH/src $GOPATH/pkg $GOPATH/bin
$GOPATH/src : Where your Go projects / programs are located
$GOPATH/pkg : contains every package objects
$GOPATH/bin : The compiled binaries home
```

## 快速启动

对于快速启动和样板 Go 项目，尝试 [Alloy](https://www.growthmetrics.io/open-source/alloy) 。

1.  克隆合金库

```
git clone https://github.com/olliecoleman/alloy
cd alloy
```

2.安装依赖项

```
glide install
npm install
```

3.启动开发服务器

```
go install
alloy dev
```

4.在`http://localhost:1212`访问网站

*Alloy 使用 Node、NPM 和 Webpack。*

## 戈朗游乐场

学习如何在本地机器上安装 Go 非常重要。但是如果你想在你的浏览器中开始玩 Go right，那么 Go Playground 是一个完美的沙盒，可以马上开始！

只需打开一个新的浏览器窗口，进入[游乐场](https://play.golang.org/)。

一旦到了那里，你就会得到按钮:

1.  奔跑
2.  格式
3.  进口
4.  分享

**Run** 按钮只是向运行 Golang 后端的 Google 服务器发送编译您编写的代码的指令。

**Format** 按钮实现了该语言惯用的格式化风格。你可以在这里阅读更多关于[的信息。](https://golang.org/pkg/fmt/)

**Imports** 只需检查您在 import()中声明了哪些包。导入路径是唯一标识包的字符串。包的导入路径对应于它在工作区或远程存储库中的位置(下面将会解释)。这里可以阅读更多[。](https://golang.org/doc/code.html#ImportPaths)

使用 **Share** 你会得到一个保存你刚刚写的代码的 URL。这在请求帮助显示代码时很有用。

你可以对 Go here 进行更深入的[之旅，并在 Go 游乐场](https://tour.golang.org/welcome/4)内的[一文中了解更多关于游乐场的信息。](https://blog.golang.org/playground)

## Go 地图

一个映射，在其他语言中称为*字典*，将键“映射”到值。地图是这样声明的:

```
var m map[Key]Value
```

此地图没有关键字，也不能添加关键字。要创建地图，使用`make`功能:

```
m = make(map[Key]Value)
```

任何东西都可以用作键或值。

## 修改地图

以下是一些常见的地图操作。

### 插入/更改元素

在地图`m`中创建或更改元素`foo`:

```
m["foo"] = bar
```

### 获取元素

获取映射`m`中关键字为`foo`的元素:

```
element = m["foo"]
```

### 删除元素

删除映射`m`中关键字为`foo`的元素:

```
delete(m, "foo")
```

### 检查是否使用了钥匙

检查键`foo`是否已在地图`m`中使用:

```
element, ok = m["foo"]
```

如果`ok`为`true`，则该键已被使用，`element`保持在`m["foo"]`的值。如果`ok`为`false`，则该键未被使用，并且`element`保持其零值。

## 地图文字

您可以直接创建地图文字:

```
var m = map[string]bool{
	"Go": true,
	"JavaScript":false,
}

m["Go"] // true
m["JavaScript"] = true // Set Javascript to true
delete(m, "JavaScript") // Delete "JavaScript" key and value
language, ok = m["C++"] // ok is false, language is bool's zero-value (false)
```

## 关于 Go 的更多信息:

*   [通过这个免费视频课程在 7 小时内学会围棋](https://www.freecodecamp.org/news/go-golang-course/)
*   如何用 Go 构建一个 Twitter 机器人