# 如何用 Docker，Nginx & Letsencrypt 建立一个简单安全的反向代理

> 原文：<https://www.freecodecamp.org/news/docker-nginx-letsencrypt-easy-secure-reverse-proxy-40165ba3aee2/>

作者:卡斯帕·西格

## 介绍

有没有试过在家里安装某种服务器？您必须为每个服务打开一个新端口。而且还得记住什么端口去什么服务，你家 ip 是什么？这绝对是有效的，而且人们已经这样做了很长时间。

然而，输入*plex.example.com*，就能即时访问你的媒体服务器，这不是很好吗？这正是反向代理将为您做的，将它与 Docker 结合起来，比以往任何时候都更容易。

## 先决条件

### 坞站&坞站-合成

你应该有 Docker 版本 17.12.0+和 Compose 版本 1.21.0+。

### 领域

您应该设置一个域，并有一个 SSL 证书与之关联。如果你没有，那么[跟随我在这里的指南](https://medium.com/devopslinks/docker-letsencrypt-dns-validation-75ba8c08a0d)学习如何用 LetsEncrypt 获得一个免费的。

## 本文将涵盖的内容

我坚信理解你在做什么。曾经有一段时间，我会遵循指南，但不知道如何排除故障。如果你想这么做，[这里有一个很棒的教程](https://medium.freecodecamp.org/docker-compose-nginx-and-letsencrypt-setting-up-website-to-do-all-the-things-for-that-https-7cb0bf774b7e)，涵盖了如何设置它。虽然我的文章很长，但你最终应该会理解它是如何工作的。

在这里，您将了解什么是反向代理，如何设置它，以及如何保护它。我尽我所能把主题分成几个部分，用标题分开，所以如果你喜欢的话，可以随意跳过一个部分。我建议在开始设置之前，先把整篇文章读一遍。

## 什么是反向代理？

### 常规代理

让我们从常规代理的概念开始。虽然这是一个在技术社区非常流行的术语，但它并不是唯一被使用的地方。代理意味着信息在到达位置之前要经过第三方。

说你不想让一个服务知道你的 IP，可以用代理。代理是专门为此目的而设置的服务器。例如，如果您使用的代理服务器位于阿姆斯特丹，将向外界显示的 IP 就是来自阿姆斯特丹服务器的 IP。唯一知道你的 IP 地址的人是控制代理服务器的人。

### 反向代理

简单来说，代理会添加一层屏蔽。这与反向代理中的概念相同，除了不是屏蔽输出连接(您访问 web 服务器)，而是屏蔽输入连接(人们访问您的 web 服务器)。你只需提供一个类似于*example.com*的 URL，当人们访问这个 URL 时，你的反向代理将会处理这个请求。

假设您在内部网络上设置了两台服务器。服务器 1 在 *192.168.1.10* 上，服务器 2 在 *192.168.1.20 上。*现在，您的反向代理正在将来自 example.com*的请求*发送到服务器 1。有一天你更新了网页。您只需在 Server2 上进行新的设置，而无需关闭网站进行维护。一旦完成，只需更改反向代理中的一行，现在请求就发送到 Server2。假设反向代理设置正确，您应该完全没有停机时间。

但拥有反向代理的最大优势可能是，您可以在多个端口上运行服务，但您只需分别打开端口 80 和 443、HTTP 和 HTTPS。所有请求都将通过这两个端口进入您的网络，反向代理将处理其余的请求。一旦我们开始设置代理，所有这些都将变得有意义。

## 设置容器

### 做什么

`docker-compose.yaml`:

```
version: '3'

services:
  reverse:
    container_name: reverse
    hostname: reverse
    image: nginx
    ports:
      - 80:80
      - 443:443
    volumes:
      - <path/to/your/config>:/etc/nginx
      - <path/to/your/certs>:/etc/ssl/private
```

首先，您应该在 docker-compose 文件中添加一个新服务。你可以随便叫它什么，在这种情况下，我选择了 *reverse* 。在这里，我刚刚选择了 *nginx* 作为镜像，然而在生产环境中，指定一个版本通常是一个好主意，以防在未来的更新中有任何突破性的变化。

然后你应该卷绑定两个文件夹。 */etc/nginx* 是存储所有配置文件的地方， */etc/ssl/private* 是存储 ssl 证书的地方。第一次启动容器时，主机上不存在 config 文件夹是非常重要的。当您通过 docker-compose 启动您的容器时，它会自动创建文件夹，并用容器的内容填充它。如果您在主机上创建了一个空的 config 文件夹，它将挂载该文件夹，并且容器中的文件夹将是空的。

### 为什么有效

这部分没什么特别的。大多数情况下，这就像用 docker-compose 启动任何其他容器一样。您应该注意的是，您正在绑定端口 80 和 443。这是所有请求进入的地方，它们将被转发到您指定的任何服务。

## 配置 Nginx

### 做什么

现在，您的主机上应该有一个 config 文件夹。转到这个目录，您应该会看到一堆不同的文件和一个名为`conf.d`的文件夹。所有的配置文件都将放在`conf.d`中。现在只有一个`default.conf`文件，你可以删除它。

仍然在`conf.d`里面，创建两个文件夹:`sites-available`和`sites-enabled`。导航到`sites-available`并创建您的第一个配置文件。在这里，我们将为 [Plex](https://plex.tv) 设置一个条目，但是如果您喜欢，可以随意使用您已经设置的另一个服务。这个文件叫什么并不重要，但是我更喜欢把它命名为`plex.conf`。

现在打开文件，输入以下内容:

```
upstream plex {
  server        plex:32400;
}

server {
  listen        80;
  server_name   plex.example.com;

  location / {
    proxy_pass  http://plex;
  }
}
```

进入`sites-enabled`目录，输入以下命令:

```
ln -s ../sites-available/plex.conf .
```

这将创建一个指向另一个文件夹中的文件的符号链接。现在只剩下一件事了，那就是更改 config 文件夹中的`nginx.conf`文件。如果您打开该文件，您应该看到以下内容作为最后一行:

```
include /etc/nginx/conf.d/*.conf;
```

将其更改为:

```
include /etc/nginx/conf.d/sites-enabled/*.conf;
```

为了让反向代理真正工作，我们需要在容器中重新加载 nginx 服务。从主机运行`docker exec <container-name> nginx -t`。这将对您的配置文件运行语法检查。这应该输出语法是正确的。现在运行`docker exec <container-name> nginx -s reload`。这将向 nginx 进程发送一个应该重新加载的信号，恭喜！你现在有了一个正在运行的反向代理，应该能够在 plex.example.com 的*访问你的服务器(假设你已经在路由器中将端口 80 转发给你的主机)。*

*尽管您的反向代理正在工作，但您是在 HTTP 上运行的，它不提供任何加密。下一部分将是如何保护你的代理，并在 SSL 实验室获得满分。*

### *为什么有效*

***配置文件***

*如您所见，`plex.conf`文件由两部分组成。一个`upstream`部分和一个`server`部分。先说`server`部分。您可以在这里定义接收传入请求的端口、该配置应该匹配的域以及它应该被发送到的位置。*

*按照这个服务器的设置方式，您应该为您希望代理请求的每个服务创建一个文件，因此显然您需要某种方式来区分哪个文件接收每个请求。这就是`server-name`指令的作用。在它下面是`location`指令。*

*在我们的例子中，我们只需要一个`location`，但是您可以拥有任意多的`location`指令。想象一下，你有一个有前端和后端的网站。根据您使用的基础设施，您可以将前端作为一个容器，将后端作为另一个容器。然后你可以使用`location / {}`向前端发送请求，使用`location /api/ {}`向后端发送请求。突然，你有多个服务运行在一个单一的难忘的领域。*

*至于`upstream`部分，可以用于负载均衡。如果你有兴趣了解更多关于它是如何工作的，你可以在这里查看[官方文件](http://nginx.org/en/docs/http/ngx_http_upstream_module.html)。对于我们这个简单的例子，您只需定义您想要代理的服务的主机名或 ip 地址，以及应该代理到哪个端口，然后在`location`指令中引用上游名称。*

***主机名与 IP 地址***

*为了理解主机名是什么，让我们举个例子。假设您在您的家庭网络 *192.168.1.0 上。*然后在 *192.168.1.10* 上设置一个服务器，并在其上运行 Plex。只要您仍在同一个网络上，您现在就可以访问 *192.168.1.10:32400* 上的丛。另一种可能是给服务器一个主机名。在这种情况下，我们将赋予它主机名 *plex* 。现在，您可以通过在浏览器中输入 *plex:32400* 来访问 Plex！*

*在版本 3 中，docker-compose 引入了同样的概念。如果您查看本文前面的 docker-compose 文件，您会注意到我给了它一个`hostname: reverse`指令。现在，所有其他容器都可以通过主机名访问我的反向代理。值得注意的一点是，服务名必须与主机名相同。这是 docker-compose 的创造者选择强加的。*

*另一个需要记住的非常重要的事情是，默认情况下 docker 容器被放在它们自己的网络上。这意味着如果你坐在主机网络上的笔记本电脑上，你将不能通过它的主机名访问你的容器。只有容器能够通过它们的主机名相互访问。*

*所以总结一下，说清楚。在 docker-compose 文件中，将`hostname`指令添加到服务中。大多数情况下，每次重启容器时，容器都会获得一个新的 IP，所以通过主机名引用它，意味着容器获得什么 IP 并不重要。*

***站点-可用&站点-启用***

*为什么我们要创建`sites-available`和`sites-enabled`目录？这不是我创造的东西。如果你在服务器上安装 Nginx，你会看到它附带了这些文件夹。然而，因为 Docker 是基于微服务构建的，一个容器应该只做一件事，所以容器中省略了这些文件夹。由于我们使用容器的方式，我们再次重现了它们。*

*是的，你完全可以创建一个`sites-enabled`文件夹，或者直接在`conf.d`中存放你的配置文件。通过这种方式，您可以进行被动配置。说自己是做维护的，不想让服务主动；您只需移除符号链接，并在希望服务再次活动时将它放回原处。*

***符号链接***

*符号链接是操作系统的一个非常强大的功能。在安装 Nginx 服务器之前，我个人从未使用过它们，但从那以后，我就在任何可能的地方使用它们。假设您正在处理 5 个不同的项目，但是所有这些项目都以某种方式使用同一个文件。您可以将文件复制到每个项目中，并直接引用它，或者您可以将文件放在一个地方，并在这 5 个项目中创建指向该文件的符号链接。*

*这带来了两个好处:你占用的空间比不占用的少 4 倍，而且是最强大的；在一个地方改变文件，它在所有 5 个项目中同时改变！这有点回避，但我认为值得一提。*

## *保护 Nginx 代理*

### *做什么*

*转到 config 文件夹，创建 3 个文件并用以下输入填充它们:*

*`common.conf`:*

```
*`add_header Strict-Transport-Security    "max-age=31536000; includeSubDomains" always;
add_header X-Frame-Options              SAMEORIGIN;
add_header X-Content-Type-Options       nosniff;
add_header X-XSS-Protection             "1; mode=block";`*
```

*`common_location.conf`:*

```
*`proxy_set_header    X-Real-IP           $remote_addr;
proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
proxy_set_header    X-Forwarded-Proto   $scheme;
proxy_set_header    Host                $host;
proxy_set_header    X-Forwarded-Host    $host;
proxy_set_header    X-Forwarded-Port    $server_port;`*
```

*`ssl.conf`:*

```
*`ssl_protocols               TLSv1 TLSv1.1 TLSv1.2;
ssl_ecdh_curve              secp384r1;
ssl_ciphers                 "ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384 OLD_TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 OLD_TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256";
ssl_prefer_server_ciphers   on;
ssl_dhparam                 /etc/nginx/dhparams.pem;
ssl_certificate             /etc/ssl/private/fullchain.pem;
ssl_certificate_key         /etc/ssl/private/privkey.pem;
ssl_session_timeout         10m;
ssl_session_cache           shared:SSL:10m;
ssl_session_tickets         off;
ssl_stapling                on;
ssl_stapling_verify         on;`*
```

*现在打开`plex.conf`文件，并将其更改为以下内容(注意第 6、9、10 & 14 行):*

```
*`upstream plex {
  server        plex:32400;
}

server {
  listen        443 ssl;
  server_name   plex.example.com;

  include       common.conf;
  include       /etc/nginx/ssl.conf;

  location / {
    proxy_pass  http://plex;
    include     common_location.conf;
  }
}`*
```

*现在回到 config 文件夹的根目录，运行以下命令:*

```
*`openssl dhparam -out dhparams.pem 4096`*
```

*这将需要很长时间才能完成，在某些情况下甚至长达一个小时。*

*如果您阅读了我关于获取 LetsEncrypt SSL 证书的文章，那么您的证书应该位于`</path/to/your/letsencrypt/config>/etc/letsencrypt/live/<domain>/`中。*

*当我帮助一个朋友在他的系统上设置这个时，我们遇到了一些问题，当文件位于那个目录中时，它无法打开文件。很可能是某些权限问题的原因。解决这个问题的简单方法是创建一个 SSL 目录，比如`</path/to/your/nginx/config>/certs`，然后将它挂载到 Nginx 容器的`/etc/ssl/private`文件夹中。在新创建的文件夹中，您应该创建符号链接，指向 LetsEncrypt 的 config 文件夹中的证书。*

*当`openssl`命令运行完后，您应该运行`docker exec <container-name> nginx -t`来确保所有的语法都是正确的，然后通过运行`docker exec <container-name> nginx -s reload`来重新加载它。此时，一切都应该运行了，您现在有了一个工作正常且非常安全的反向代理！*

### *为什么有效*

*查看`plex.conf`文件，只有一个主要的变化，那就是反向代理监听哪个端口，并告诉它这是一个 ssl 连接。然后有 3 个地方我们包含了我们制作的另外 3 个文件。虽然 SSL 本身是安全的，但这些其他文件使它更加安全。然而，如果出于某种原因你不想包含这些文件，你需要将`ssl-certificate`和`ssl-certificate-key`移到`.conf`文件中。为了使 HTTPS 连接正常工作，这些都是必需的。*

***Common.conf***

*查看`common.conf`文件，我们添加了 4 个不同的头。头是服务器在每次响应时发送给浏览器的东西。这些头告诉浏览器以某种方式行动，然后由浏览器来执行这些头。*

*[**【严-运-安(HSTS)**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security)*

*这个头告诉浏览器应该通过 HTTPS 建立连接。当这个头被添加后，浏览器不允许你与服务器进行简单的 HTTP 连接，以确保所有的通信都是安全的。*

*[**【X 帧选项】**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)*

*指定此标题时，您就指定了其他网站是否可以将您的内容嵌入到他们的网站中。这有助于避免[点击劫持](https://en.wikipedia.org/wiki/Clickjacking)攻击。*

*[**X-内容-类型-选项**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options)*

*假设你有一个用户可以上传文件的网站。没有对文件进行足够的验证，所以用户成功地上传了一个`php`文件到服务器，而服务器期望上传一个图像。然后，攻击者就可以访问上传的文件。现在服务器响应一个图像，但是文件的 MIME 类型是`text/plain`。浏览器会“嗅探”文件，然后呈现 php 脚本，使得攻击者能够进行 RCE(远程代码执行)。*

*当这个头设置为‘no sniff’时，浏览器将不会查看这个文件，而只是简单地按照服务器告诉浏览器的那样来呈现它。*

*[**X-XSS-保护**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)*

*虽然这个标题在旧的浏览器中更有必要，但是它很容易添加，你也可以这样做。一些 XSS(跨站点脚本)攻击可能非常智能，而一些非常初级。这个标题将告诉浏览器扫描简单的漏洞并阻止它们。*

***Common_location.conf***

***X-Real-IP***

*因为您的服务器在反向代理后面，如果您尝试查看请求 IP，您将总是看到反向代理的 IP。添加此报头是为了让您可以看到哪个 IP 真正在请求您的服务。*

***X-Forwarded-For***

*有时，一个用户请求在到达服务器之前会经过多个客户机。这个头包括所有这些客户端的数组。*

***X-Forwarded-Proto***

*该标题将显示客户端和服务器之间使用的协议。*

***主持人***

*这确保了可以对域名进行反向 DNS 查找。当`server_name`指令不同于你所代理的指令时使用。*

***X 转发主机***

*显示请求的真实主机，而不是反向代理。*

***X 转发端口***

*帮助识别客户端请求服务器使用的端口。*

***Ssl.conf***

*SSL 本身就是一个巨大的主题，大到无法在本文中开始解释。有很多很棒的教程介绍 SSL 握手是如何工作的，等等。如果你想研究这个特定的文件，我建议看看所使用的协议和密码，以及它们有什么不同。*

## *将 HTTP 重定向到 HTTPS*

*善于观察的人可能已经注意到，在这个安全版本中，我们只监听 443 端口。这意味着任何试图通过 *https://** 访问网站的人都可以通过，但是试图通过 *http://** 连接只会得到一个错误。幸运的是，有一个非常简单的方法可以解决这个问题。用以下内容制作一个`redirect.conf`文件:*

```
*`server {
  listen        80;

  server_name   _;

  return 301 https://$host$request_uri;
}`*
```

*现在只需确保它出现在您的`sites-enabled`文件夹中，当您在容器中重新加载 Nginx 进程时，所有到端口 80 的请求将被重定向到端口 443 (HTTPS)。*

## *最后的想法*

*现在你的网站已经启动并运行了，你可以去 [SSL 实验室](https://www.ssllabs.com/ssltest/analyze.html)进行测试，看看你的网站有多安全。在写这篇文章的时候，你应该会得到一个满分。然而，有一件重要的事情需要注意。*

*安全性和便利性之间总会有一个平衡点。在这种情况下，权重严重偏向于安全性。如果您在 SSL Labs 上运行测试并向下滚动，您会看到有多个设备无法连接到您的站点，因为它们不支持新标准。*

*因此，在设置时请记住这一点。现在我只是在家里运行一个服务器，在那里我不必担心很多人能够访问它。但是如果你对脸书进行扫描，你会发现他们不会有这么高的分数，但是他们的网站可以被更多的设备访问。*