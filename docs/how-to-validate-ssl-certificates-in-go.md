# 如何在 Go 中验证 SSL 证书

> 原文：<https://www.freecodecamp.org/news/how-to-validate-ssl-certificates-in-go/>

最近，我遇到了一个 SaaS 的产品，它可以为你的网站验证 SSL 证书。我想试着用 Go 写同样的东西——结果证明这非常简单(只有 17 行代码)。

你需要对你的网站进行三项主要检查。

1.  首先，检查你的站点是否有 SSL 证书。您还需要知道您的网站是否使用自签名 SSL 证书，该证书不被视为有效证书(它需要由证书颁发机构签名)。
2.  其次，查看 SSL 证书是否有正确的主机名。
3.  第三，您需要知道服务器证书的到期日期。

先决条件:

*   你应该在电脑上安装了`go`。

### 第一步:检查你的网站是否有 SSL 证书

首先，我们将尝试检查网站是否有 SSL 证书。

为此，我们需要与网站建立 TLS 连接。如果成功，这意味着该网站有一个有效的 TLS 证书。

要建立 TLS 连接，我们可以使用 Go `crypto/tls`包。我们将使用`Dial`方法连接到网站，如下所示:

```
package main

import (
	"crypto/tls"
)

func main() {
	conn, err := tls.Dial("tcp", "example.com:80", nil)
	if err != nil {
		panic("Server doesn't support SSL certificate err: " + err.Error())
	}
} 
```

main.go

我们将尝试在`example.com`运行我们的测试。当您运行上述代码时，您应该会得到以下错误:

```
$ go run main.go
panic: Server doesn't support SSL certificate err: tls: first record does not look like a TLS handshake

goroutine 1 [running]:
main.main()
        /Users/umesh/personal/spike/main.go:99 +0x2ca
exit status 2 
```

output in the terminal

基本上，它试图建立连接，但失败了。只有当网站拥有有效证书时,`Dial`方法才会成功(如果证书是自签名的，则该方法会失败)。

现在，在启用了 SSL 的网站上尝试相同的代码。我以自己网站的网址为例:

```
package main

import (
	"crypto/tls"
)

func main() {
	conn, err := tls.Dial("tcp", "blog.umesh.wtf:443", nil)
	if err != nil {
		panic("Server doesn't support SSL certificate err: " + err.Error())
	}
} 
```

main.go

这一次，我们应该能够成功地建立连接，而不会出现代码混乱。

在生产中，您不应该使用恐慌，而是应该优雅地处理错误。

### 步骤 2:检查 SSL 证书和网站主机名是否匹配

为了验证主机名，我们需要在由`Dial`返回的`conn`上调用`VerifyHostname`。此方法尝试将证书中指定的公用名或主题替换名与作为参数传递的域相匹配。

```
package main

import (
	"crypto/tls"
)

func main() {
	conn, err := tls.Dial("tcp", "blog.umesh.wtf:443", nil)
	if err != nil {
		panic("Server doesn't support SSL certificate err: " + err.Error())
	}

	err = conn.VerifyHostname("blog.umesh.wtf")
	if err != nil {
		panic("Hostname doesn't match with certificate: " + err.Error())
	}
} 
```

main.go

这将成功执行，不会出现任何错误，因为证书的公用名和主机名是相同的。

### 步骤 3:验证服务器 SSL 证书的到期日期

我们可以使用`conn.ConnectionState().PeerCertificates`获得证书链。然后，我们可以使用这个证书来获取服务器证书的到期日期。

我们将使用证书列表中的第一个证书，并尝试使用字段`NotAfter`获取到期日期。

```
package main

import (
	"crypto/tls"
	"fmt"
	"time"
)

func main() {
	conn, err := tls.Dial("tcp", "blog.umesh.wtf:443", nil)
	if err != nil {
		panic("Server doesn't support SSL certificate err: " + err.Error())
	}

	err = conn.VerifyHostname("blog.umesh.wtf")
	if err != nil {
		panic("Hostname doesn't match with certificate: " + err.Error())
	}
	expiry := conn.ConnectionState().PeerCertificates[0].NotAfter
	fmt.Printf("Issuer: %s\nExpiry: %v\n", conn.ConnectionState().PeerCertificates[0].Issuer, expiry.Format(time.RFC850))
} 
```

main.go

输出应该包含证书的截止日期和颁发者名称。

```
$ go run main.go
Issuer: CN=Let's Encrypt Authority X3, O=Let's Encrypt, C=US
Expiry: Wednesday, 16-Dec-20 16:20:00 UTC
```

output in the terminal

现在，我们已经成功验证了该站点的证书。

### 结论

您还可以获得详细信息，如根 CA、证书颁发日期以及所有链接的证书。

这个工具只是使用 Go 的核心库编写的(没有外部库)。一定要让我知道你对它的看法！

如果你喜欢这篇文章，你可以去我的个人博客看看我写的更多东西。

我经常写关于编程和软件的文章，所以请订阅我的时事通讯，让我的最新文章直接发送到你的收件箱。你也可以在推特上和我联系。