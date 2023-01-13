# 如何在 Windows 上安装 Node.js 和 npm

> 原文：<https://www.freecodecamp.org/news/how-to-install-node-js-and-npm-on-windows/>

在 Windows 上安装 Node.js 和 npm 非常简单。

首先从 [Node.js 网站](https://nodejs.org/)下载 Windows installer。你可以选择**(长期支持)或者 ****当前**** 版本。**

*   ******当前**** 版本接收最新功能，更新更快**
*   **为了提高稳定性，****【LTS】****版本放弃了功能变化，但接受了诸如错误修复和安全更新等补丁**

**一旦您选择了一个满足您需要的版本，运行安装程序。按照提示选择安装路径，并确保 ****npm 包管理器**** 特性与 ****Node.js 运行时**** 一起包含在内。这应该是默认配置。**

**安装完成后，重新启动计算机。**

**如果您在默认配置下安装，Node.js 现在应该会添加到您的路径中。运行命令提示符或 powershell，并输入以下内容进行测试:**

```
`> node -v`
```

**控制台应该用版本字符串进行响应。对 npm 重复该过程:**

```
`> npm -v`
```

**如果这两个命令都有效，那么您的安装就成功了，您可以开始使用 Node.js 了！**

## **关于 Node.js 的更多信息**

**根据其 [GitHub 库](https://github.com/nodejs/node)，Node.js 为:**

> **Node.js 是一个开源、跨平台的 JavaScript 运行时环境。它在浏览器之外执行 JavaScript 代码。有关使用 Node.js 的更多信息，请参见 [Node.js 网站](https://nodejs.org/)。**

### **Node.js 事实的细分:**

*   **Node.js 是基于 Chrome 的 V8 JavaScript 引擎构建的 JavaScript 运行时。每个浏览器都内置了一个 JavaScript 引擎来处理网站中包含的 JavaScript 文件。谷歌 Chrome 使用 V8 引擎，该引擎是使用 C++构建的。Node.js 也使用这个超快的引擎来解释 JavaScript 文件。**
*   **Node.js 使用事件驱动的模型。这意味着 Node.js 等待某些事件发生。然后，它对这些事件进行操作。事件可以是从点击到 HTTP 请求的任何事情。我们还可以声明自己的自定义事件，并让 Node.js 监听这些事件。**
*   **Node.js 使用非阻塞 I/O 模型。我们知道 I/O 任务比处理任务花费的时间要长得多。Node.js 使用回调函数来处理这样的请求。**

**让我们假设一个特定的 I/O 任务需要 5 秒钟来执行，并且我们希望在我们的代码中执行这个 I/O 两次。**

******Python******

```
`import time

def my_io_task():
  time.sleep(5)
  print("done")

my_io_task()
my_io_task()`
```

******Node.js******

```
`function my_io_task() {
    setTimeout(function() {
      console.log('done');
    }, 5000);
}

my_io_task();
my_io_task();`
```

**两者看起来相似，但是执行时间不同。Python 代码需要 10 秒来执行，而 Node.js 代码只需要 5 秒。**

**Node.js 花费的时间更少，因为它采用了非阻塞 I/O 模型。对`my_io_task()`的第一次调用启动了计时器，并把它留在那里。它不等待函数的响应。相反，它继续调用第二个`my_io_task()`，启动计时器，并将其留在那里。**

**当定时器用 5 秒钟完成它的执行时，它调用函数并在控制台上打印`done`。由于两个定时器一起启动，它们一起完成，因此花费相同的时间。**

## ****Socket.io****

**Socket.io 是一个 Node.js 库，用来帮助实现计算机之间的实时通信。为了确保这个 Socket.io 使用 WebSockets 在客户端的浏览器和服务器之间建立连接。本库使用[引擎。IO](https://github.com/socketio/engine.io) 用于建立连接。**

### ****演示****

**为了体验一下什么是可能的，Socket.io 提供了两个演示来展示它可能的用例。您可以在[https://socket.io/demos/chat/](https://socket.io/demos/chat/)找到演示，并在左侧找到白板演示的链接。**

### ****开始使用****

**因为 Socket.io 是一个 Node.js 库，所以您必须确保安装了 Node.js。如果尚未安装，请在[Nodejs.org](https://nodejs.org/)获取最新版本**

#### ****苹果电脑****

**Node.js 也可以通过 macOS 的包管理器 [Homebrew](https://brew.sh/) 安装。**

**只需键入`brew install node`来安装 Node.js。**

**在 Socket.io 的页面上也可以找到一个[入门](https://socket.io/get-started/chat/)指南。它展示了如何用几行代码轻松构建实时聊天。**

#### ****更多信息****

**有关 Socket.io 及其文档的更多信息，请访问:**

*   **[Socket.io](https://socket.io/)**
*   **[Socket.io 文档](https://socket.io/docs/)**

### **关于 Node.js 的更多信息**

*   **[官方 Node.js 站点](https://nodejs.org/)**
*   **[节点版本管理器](https://github.com/nvm-sh/nvm)**
*   **[n: Interactive Node.js 版本管理器](https://github.com/tj/n)**
*   **[Node.js 文档](https://nodejs.org/en/docs/)**