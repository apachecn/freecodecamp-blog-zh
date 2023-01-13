# OpenSSL 命令清单

> 原文：<https://www.freecodecamp.org/news/openssl-command-cheatsheet-b441be1e8c4a/>

阿列克谢·萨莫什金

当涉及到与安全相关的任务时，比如生成密钥、CSR、证书、计算摘要、调试 TLS 连接以及其他与 PKI 和 HTTPS 相关的任务，您很可能最终会使用 OpenSSL 工具。

OpenSSL 包含了大量的特性，涵盖了广泛的用例，很难记住所有这些特性的语法，而且很容易丢失。页面在这里并没有太大的帮助，所以我们经常谷歌“openssl how to [use case here]”或寻找某种“openssl cheatsheet”来回忆命令的用法并查看示例。

这篇文章是我个人收集的`openssl`命令片段和例子，按用例分组。

### 用例

以下是我将涉及的用例列表:

1.  [使用 RSA 和 ECDSA 密钥](#0507)
2.  [创建证书签名请求(CSR)](#bdf8)
3.  [创建 X.509 证书](#b723)
4.  [验证 CSR 或证书](#2322)
5.  [计算消息摘要和 base64 编码](#4fe9)
6.  [TLS 客户端连接到远程服务器](#4d47)
7.  [测量 TLS 连接和握手时间](#0f65)
8.  [在编码(PEM，DER)和容器格式(PKCS12，PKCS7)之间转换](#ce64)
9.  [列出密码套件](#dcab)
10.  [手动检查来自 OCSP 响应者的证书撤销状态](#e48c)

当然，这不是一个完整的列表，但是它涵盖了最常见的用例，包括我一直在使用的用例。例如，我跳过加密和解密，或者使用 openssl 进行 CA 管理。`openssl`就像一个宇宙。你永远不知道它在哪里结束。？

### 使用 RSA 和 ECDSA 密钥

在下面的命令中，用密钥大小替换`[bits]`(例如，2048、4096、8192)。

生成 RSA 密钥:
`openssl genrsa -out example.key [bits]`

仅打印公钥或模数:
`openssl rsa -in example.key -pubout`
`openssl rsa -in example.key -noout -modulus`

打印 RSA 密钥的文本表示:
`openssl rsa -in example.key -text -noout`

生成新的 RSA 密钥，并使用基于 AES CBC 256 加密的通行短语进行加密:
`openssl genrsa -aes256 -out example.key [bits]`

请检查您的私钥。如果密钥有密码短语，系统会提示您输入:
`openssl rsa -check -in example.key`

从密钥中删除密码:
`openssl rsa -in example.key -out example.key`

使用密码短语
`openssl rsa -des3 -in example.key -out example_with_pass.key`加密现有私钥

生成 ECDSA 密钥。`curve`将替换为:`prime256v1`、`secp384r1`、`secp521r1`或任何其他支持的椭圆曲线:
、`openssl ecparam -genkey -name [curve] | openssl ec -out example.ec.key`

打印 ECDSA 密钥文本表示:
`openssl ec -in example.ec.key -text -noout`

列出 OpenSSL 库支持的可用 EC 曲线:
`openssl ecparam -list_curves`

生成给定长度的 DH 参数:
`openssl dhparam -out dhparams.pem [bits]`

### 创建证书签名请求(CSR)

在下面的命令中，将`[digest]`替换为支持的哈希函数的名称:`md5`、`sha1`、`sha224`、`sha256`、`sha384`或`sha512`等。最好避开`md5`和`sha1`这样的弱功能，坚持`sha256`及以上。

从现有私钥创建 CSR。
`openssl req -new -key example.key -out example.csr -[digest]`

在一个命令中创建 CSR 和不带密码短语的私钥:
`openssl req -nodes -newkey rsa:[bits] -keyout example.key -out example.csr`

在命令行上提供 CSR 主题信息，而不是通过交互式提示。
`openssl req -nodes -newkey rsa:[bits] -keyout example.key -out example.csr -subj "/C=UA/ST=Kharkov/L=Kharkov/O=Super Secure Company/OU=IT Department/CN=example.com"`

从现有证书和私钥创建 CSR:
`openssl x509 -x509toreq -in cert.pem -out example.csr -signkey example.key`

通过提供 openssl 配置文件:
`openssl req -new -key example.key -out example.csr -config req.conf`为多域 SAN 证书生成 CSR

其中`req.conf`:

```
[req]prompt=nodefault_md = sha256distinguished_name = dnreq_extensions = req_ext
```

```
[dn]CN=example.com
```

```
[req_ext]subjectAltName=@alt_names
```

```
[alt_names]DNS.1=example.comDNS.2=www.example.comDNS.3=ftp.example.com
```

### 创建 X.509 证书

从头开始创建自签名证书和新的私钥:
`openssl req -nodes -newkey rsa:2048 -keyout example.key -out example.crt -x509 -days 365`

使用现有 CSR 和私钥创建自签名证书:
`openssl x509 -req -in example.csr -signkey example.key -out example.crt -days 365`

使用您自己的“CA”证书及其私钥签署子证书。如果您是一家 CA 公司，这是一个非常简单的例子，说明了如何颁发新证书。
`openssl x509 -req -in child.csr -days 365 -CA ca.crt -CAkey ca.key -set_serial 01 -out child.crt`

打印证书的文本表示
`openssl x509 -in example.crt -text -noout`

打印证书指纹为 md5，sha1，sha256 摘要:
`openssl x509 -in cert.pem -fingerprint -sha256 -noout`

### 验证 CSR 或证书

验证 CSR 签名:
`openssl req -in example.csr -verify`

验证私钥与证书和 CSR 匹配:
`openssl rsa -noout -modulus -in example.key | openssl sha256`
`openssl x509 -noout -modulus -in example.crt | openssl sha256`
`openssl req -noout -modulus -in example.csr | openssl sha256`

验证证书，前提是您的计算机上有根证书和任何配置为可信的中间证书:
`openssl verify example.crt`

当您有中间证书链时，验证证书。根证书不是捆绑包的一部分，应该在您的计算机上配置为受信任的。
`openssl verify -untrusted intermediate-ca-chain.pem example.crt`

当您有中间证书链和根证书时，请验证未配置为可信证书的证书。
`openssl verify -CAFile root.crt -untrusted intermediate-ca-chain.pem child.crt`

验证远程服务器提供的证书包含给定的主机名。检查您的多域证书是否正确覆盖了所有主机名非常有用。
`openssl s_client -verify_hostname [www.example.com](http://www.example.com) -connect example.com:443`

### 计算消息摘要和 base64 编码

计算`md5`、`sha1`、`sha256`、`sha384`、`sha512`摘要:
、`openssl dgst -[hash_function] <input.f` i `le`、
、`cat input.file | openssl [hash_functi` on】

Base64 编解码:
`cat /dev/urandom | head -c 50 | openssl base64 | openssl base64 -d`

### TLS 客户端连接到远程服务器

连接支持 TLS 的服务器:
`openssl s_client -connect [example.com:443](http://www.google.com:443)`
`openssl s_client -host example.com -port 443`

连接到服务器并显示完整的证书链:
`openssl s_client -showcerts -host example.com -port 443 </dev/n` ull

提取证书:
`openssl s_client -connect example.com:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' > certif`ite . PEM

用另一个服务器名覆盖 SNI(服务器名指示)扩展。当多个安全站点托管在同一个 IP 地址:
`openssl s_client -servername [www.](http://www.foobbz.site)example.com -host example.com -port 443`时对测试有用

强制使用特定的密码套件测试 TLS 连接，例如`ECDHE-RSA-AES128-GCM-SHA256`。用于检查服务器是否可以通过不同的已配置密码套件正常通话，而不是它喜欢的密码套件。
`openssl s_client -host example.com -port 443 -cipher ECDHE-RSA-AES128-GCM-SHA256 2>&1 </de` v/null

### 测量 TLS 连接和握手时间

不使用/使用会话重用测量 SSL 连接时间:
`openssl s_time -connect example.com:443 -new`
`openssl s_time -connect example.com:443 -reuse`

使用`curl`:
:`curl -kso /dev/null -w "tcp:%{time_connect}, ssldone:%{time_appconnect}\n" https://example.com`粗略检查 TCP 和 SSL 握手次数

衡量各种安全算法的速度:
`openssl speed rsa2048`
`openssl speed ecdsap256`

### 在编码和容器格式之间转换

在 DER 和 PEM 格式之间转换证书:
`openssl x509 -in example.pem -outform der -out example.der`
`openssl x509 -in example.der -inform der -out example.pem`

将 PKCS7 (P7B)文件中的几个证书合并:
`openssl crl2pkcs7 -nocrl -certfile child.crt -certfile ca.crt -out example.p7b`

从 PKCS7 转换回 PEM。如果 PKCS7 文件有多个证书，PEM 文件将包含其中的所有项目。
`openssl pkcs7 -in example.p7b -print_certs -out example.crt`

将 PEM 证书文件和私钥组合到 PKCS#12(。pfx .p12)。此外，您可以向 PKCS12 文件添加证书链。
`openssl pkcs12 -export -out certificate.pfx -inkey privkey.pem -in certificate.pem -certfile ca-chain.pem`

转换 PKCS#12 文件(.pfx .p12)包含私钥和证书返回给 PEM:
`openssl pkcs12 -in keystore.pfx -out keystore.pem -nodes`

### 列出密码套件

列出可用的 TLS 密码套件，openssl 客户端能够:
`openssl ciphers -v`

枚举所有单独的密码套件，这些套件由一个简写的 OpenSSL 密码列表字符串描述。这在你配置服务器(比如 Nginx)时很有用，你需要测试你的`ssl_ciphers`字符串。


### 从 OCSP 响应程序手动检查证书吊销状态

这是一个多步骤的过程:

1.  从远程服务器检索证书
2.  获取中间 CA 证书链
3.  从证书中读取 OCSP 端点 URI
4.  向远程 OCSP 响应者请求证书吊销状态

首先，从远程服务器检索证书:
`openssl s_client -connect example.com:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' >` cert.pem

您还需要获得中间 CA 证书链。使用`-showcerts` 标志显示完整的证书链，并将所有中间证书手动保存到`chain.pem`文件:
`openssl s_client -showcerts -host example.com -port 443 </dev/n`满

从证书中读取 OCSP 端点 URI:
`openssl x509 -in cert.pem -noout -ocsp_uri`

使用上述步骤中的 URI 向远程 OCSP 响应方请求证书吊销状态(例如 http://ocsp . stg-int-x1 . lets encrypt . org)。
`openssl ocsp -header "Host" "ocsp.stg-int-x1.letsencrypt.org" -issuer chain.pem -VAfile chain.pem -cert cert.pem -text -url [http://ocsp.stg-int-x1.letsencrypt.org](http://ocsp.stg-int-x1.letsencrypt.org)`

### 资源

我收集了一些关于 OpenSSL 的资源，您可能会觉得有用。

OpenSSL Essentials:使用 SSL 证书、私钥和 CSR | digital ocean—[https://www . digital ocean . com/community/tutorials/OpenSSL-Essentials-使用 SSL-Certificates-Private-Keys-and-CSR](https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs)

最常见的 OpenSSL 命令—[https://www . SSL shopper . com/article-Most-Common-OpenSSL-Commands . html](https://www.sslshopper.com/article-most-common-openssl-commands.html)

OpenSSL:使用 SSL 证书、私钥和 CSR—[https://www.dynacont.net/documentation/linux/openssl/](https://www.dynacont.net/documentation/linux/openssl/)