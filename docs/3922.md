# 用 Go 制作自己的 Web 服务器:快速指南

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-own-web-server-with-go/>

Go 编程语言因其内置的 web 服务器而闻名。在这篇文章中，你将学习如何用 Go 轻松地创建自己的 web 服务器。除了已经内置的包之外，你不需要任何其他的包！

首先，进入你的文本编辑器。然后创建一个名为`webserver.go`的文件，并输入以下代码:

```
package main

import (
  "net/http"
  "io"
)

func main() {
  http.HandleFunc("/", servePage)
	http.ListenAndServe(":8080", nil)
}

func servePage(writer http.ResponseWriter, reqest *http.Request) {
  io.WriteString(writer, "Hello world!")
}
```

让我们分解上面的代码块。我们导入`net/http`包:这个包包含 web 服务器本身。然后我们还导入了`io`包，稍后我们将利用它为客户端提供服务。

在`main`函数中，我们做两件事。首先，我们指示服务器让名为`servePage`的函数处理所有进入`/`的流量——在这种情况下，这意味着它处理对 *any* `URL`的请求。

我们做的第二件事实际上是激活服务器。我们使用一个名为`ListenAndServe`的函数来实现这一点。这个函数需要两个参数:T1(作为 T2)，在这里是 T3，T4(作为 T5)，然而最后一个并不重要。我们会让它`nil`一切都会好的。

目前，我们只做一件简单的事情。使用`io`包和它包含的`WriteString`函数，我们可以用文本`Hello world!`(当然也可以是任何其他字符串)来响应客户的请求。

您可能还注意到了`servePage`函数有两个参数:`writer`和`request`。有了 writer，你实际上可以响应一个`HTTP`请求，而有了`request`，你可以获得关于请求本身的更多信息。

恭喜你！您刚刚创建了您的第一个 web 服务器！如果你想测试它:只需运行`go run webserver.go`，启动浏览器并导航到`http://localhost:8080`！