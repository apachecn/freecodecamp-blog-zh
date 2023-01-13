# 如何在一台服务器上托管多个域名和项目

> 原文：<https://www.freecodecamp.org/news/how-you-can-host-multiple-domain-names-and-projects-in-one-vps-7aed4f56e7a1/>

作者洪斌·李

# 如何在一台服务器上托管多个域名和项目

#### NGINX 是一个神奇的工具

![0*FT9uL7NRg2iep6-e](img/7796fb743513cd0af4819e41f555f74d.png)

Photo by [imgix](https://unsplash.com/@imgix?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我拥有多个域名，每个域名都有一个不同的兼职项目。很长一段时间，所有需要“托管”的东西都是在 Heroku 上托管的。但是他们的免费等级可能是相当有限的，如果你为每个单独的项目付费，它也可能很快变得昂贵。因此，我决定尝试使用 NGINX(简·满春·王向我推荐)将它们整合在一起。

### 所需资源

#### 虚拟专用服务器

你将需要一个虚拟服务器，如[数字海洋](https://www.freecodecamp.org/news/how-you-can-host-multiple-domain-names-and-projects-in-one-vps-7aed4f56e7a1/undefined)或 [AWS](https://www.freecodecamp.org/news/how-you-can-host-multiple-domain-names-and-projects-in-one-vps-7aed4f56e7a1/undefined) 的 EC2。就我个人而言，我使用的是 [Vultr](https://www.vultr.com/?ref=7358373) (这里是[非推荐链接](http://vultr.com/))，大约每月 2.50 美元。

#### 域名

你需要注册几个域名。假设您可能已经有了它们，请确保您的域名指向 VPS 的名称服务器。在你的域名服务面板中应该有一个 DNS 部分，你可以选择“自定义 DNS”或类似的东西。如果你不确定你的 VPS 的名称服务器是什么，你应该能够通过简单的搜索“名称服务器”+ VPS 服务名称很容易地找到那个信息。

### 设置 NGINX

#### 安装和基本设置

*参考自[如何在 Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)T3 上安装 Nginx*

通过 SSH-ing 进入 VPS 运行以下命令。它将安装 NGINX，设置允许它的防火墙规则，并设置 NGINX 在引导时自动启动。

#### 配置设置

*引用自[用 Apache 或 nginx 在一个服务器/IP 上托管多个域](https://geekflare.com/multiple-domains-on-one-server-with-apache-nginx/)*

默认的 virtual.conf 位置应该是/etc/nginx/conf.d/virtual.conf，我建议在进行任何更改之前备份默认文件。(如果不存在，可以直接创建。)编辑该文件，如下所示:

以下是一些需要注意的事项:

*   *服务器*块—其中的每一个都应该代表正在使用的每个不同的域或子域。
*   *根目录* —这是加载(HTML)文件的位置。
*   *服务器名称*——应该加载这些特定文件的(子)域名。
*   *proxy_redirect* —在将特定子域重定向到活动服务器的情况下，您会希望添加该子域并在其后放置 IP 位置。(对于本地服务器，无论是[*http://127 . 0 . 0 . 1:port*](http://127.0.0.1:port)还是[*http://localhost:port*](http://localhost:port)都应该正常工作。)

```
sudo systemctl restart nginx
```

完成后，重新启动服务器，以便加载和应用新的配置。

### 克隆和链接

现在请记住，由于您的目录指向/opt/htdocs/ *websiteName* ，您最初的想法可能是将您的项目克隆到这些文件夹中。这可以工作，但并不理想，因为这些文件夹中的许多操作需要 root 访问权限才能真正做任何事情。

相反，您可以像平常一样将它们克隆到您的用户文件夹或任何其他地方，然后创建一个软链接来连接到您的存储库文件夹的路径。大概是这样的:

```
git clone git@github.com:binhonglee/binhonglee.github.io ~/websitesudo ln -s ~/website /opt/htdocs/binhong
```

当然，当您克隆 Node.js 静态站点文件夹(ReactJS、Angular 或 Vue.js)时，您会希望安装(`npm install`)并构建(`npm run-script build`)它们。然后链接*。/build* 文件夹，而不是克隆存储库的基础级别。(同样适用于 Jekyll 网站，但使用了*。/_ 输出*文件夹代替。)至于活动服务器，只需确保您的服务器运行在与配置文件中列出的端口相同的端口上。

### 用 certbot 设置 HTTPS

感谢让我们加密，你现在可以获得免费和简单的 HTTPS 证书。随着 certbot 的推出，一切变得更加简单！

*参考自[如何在 Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04) 上用 Let's Encrypt 保护 Nginx】*

只需对您的所有域名和子域名运行上述操作，certbot 就会处理好一切。如果您要更新证书，您可以运行以下命令，这样证书机器人将帮助您更新 SSL 证书。

```
sudo certbot renew --dry-run
```

### 更新一切

现在你已经设置好了所有的东西，你可能会想，如果/当我需要更新的时候，似乎有太多的东西需要记住。不幸的是，这是真的，但是我们总是可以通过添加一个脚本来简化它。

这是一个人的样子:

感谢阅读！如果你有任何问题，请在下面的评论中告诉我。

### 关于我

在撰写本文时，我作为 AdvantisGlobal 的独立承包商，在苹果公司担任 Siri 语言工程师。我花了很多空闲时间用我觉得有趣的技术来试验和构建新的东西。在此或在 [GitHub](https://github.com/binhonglee) 上跟随我的探索之旅[。](https://binhong.me/blog)

### 其他参考文献

*   [nginx 代理通过重定向忽略](https://serverfault.com/questions/363159/nginx-proxy-pass-redirects-ignore-port)[服务器上的端口](https://serverfault.com)故障
*   在[超级用户](https://superuser.com/)上关闭 SSH 时继续 SSH 后台任务/作业