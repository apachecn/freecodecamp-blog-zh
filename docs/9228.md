# 域名注册商、DNS 和托管

> 原文：<https://www.freecodecamp.org/news/domain-registrars-dns-and-hosting-353e4163a19/>

作者:ᴋɪʀʙʏ ᴋᴏʜʟᴍᴏʀɢᴇɴ

# 域名注册商、DNS 和托管

![jYoDpn9-NIy7zkeEAylenC4WA6vYOeShGhE4](img/81a248a0c7e2ea59f7abcb31c5ff350d.png)

#### 如何以正确的方式建立你的网站

我花了一段时间来建立基础设施，以一种让我满意的方式运行我的网站和电子邮件。有很多蹩脚的域名注册商、DNS 提供商和网络主机。我终于对管道中的每个组件都满意了，所以我想我应该与世界分享我的设置。

### Namecheap

以前，我和其他许多人一样，使用 GoDaddy 作为我的主要域名注册商。我很快了解到，GoDaddy 把他们所有的钱都花在了花哨的广告[](http://fortune.com/2015/03/30/godaddy-ads-ipo/)**上，而不是雇佣 UX 的工程师或产品经理。我在那里的时候，他们的支持团队也很糟糕。**

**如今，我只使用 [**名称便宜**](http://www.namecheap.com/?aff=90584) 来满足我的域名需求。Namecheap 是迄今为止我发现的最好的域名管理界面，而且每次使用它都会变得更好。他们也总是在 [**新顶级域名**](https://www.namecheap.com/domains/new-tlds/explore.aspx?aff=90584) 上有很好的交易，在他们的网站上搜索一个伟大的域名总是一种奇妙的体验。**

### **云耀斑**

**每个域名注册商都将为您提供为您的域名配置 DNS 设置的能力，但没有任何免费的域名服务器服务能够真正与 [**CloudFlare**](https://www.cloudflare.com/) 相提并论。我不想花太多时间来解释 CloudFlare 可以为您做的所有令人惊叹的事情，因为这个视频[](https://vimeo.com/14700285)**已经做了令人难以置信的工作。****

****如果您看过该视频，那么您应该知道 CloudFlare 免费提供给用户的许多出色功能。不过，我最喜欢的功能在 CloudFlare 的网站上出人意料地被弱化了。我使用 CloudFlare 的首要原因是因为 [**我从来不需要等待 DNS 传播**](https://blog.cloudflare.com/never-deal-with-dns-propagation-again/) 。****

### ****github pages-github 页面****

****托管一个网站也是一件痛苦的事情。如果你只需要托管一个静态网站，那么 [**没有理由**](https://google.com/search?q=free+static+website+hosting) 你需要付费托管。我个人最喜欢的方案是 [**GitHub Pages**](https://pages.github.com/) 。我已经使用 *git* 来管理我的网站，所以 GitHub Pages 使得更新我的网站像 *git push* 一样简单。不再有 FTP、SSH 或任何其他三个字母的缩写。****

### ****设置它****

****到现在为止，你应该对如何设置你的网站有了一个很好的高层次的概述，但是可能有一些小的实现细节仍然使连接这些点变得困难。为了利用上述服务，您需要遵循以下所有步骤:****

#### ****1.在 Namecheap 上获得您的域名。****

****直接买还是转让都没关系，但是要在 Namecheap 上获得你的域名。****

#### ******2。将您的站点添加到 CloudFlare。******

****在 CloudFlare 上添加您的站点，扫描当前 DNS 记录，然后选择免费计划。完成后，系统会提示您两个域名服务器地址。把这些留到下一步。****

#### ******3。点到云闪。******

****回到 name price，从主“仪表板”页面点击与您的域名对应的“管理”按钮。在“域名服务器”部分点击“基本域名服务器”，然后选择“自定义域名服务器”在这里，输入您从 CloudFlare 获得的两个名称服务器，然后单击绿色复选标记。****

#### ******4。检查域名服务器。******

****太棒了，现在回到您在 CloudFlare 上的站点，单击“重新检查名称服务器”这可能需要 24 小时，但通常(尤其是使用 Namecheap)只需要几分钟。****

#### ****5.设置 GitHub 页面。****

****前往 github 并创建一个名为*用户名* .github.io 的 [**新回购**](https://github.com/new) ，其中*用户名*是你在 GitHub 上的用户名。在这里，您可以使用以下命令来提升您的静态网站:****

```
**`# from your website directory on your local machine$ git init$ git add .$ git commit -m ‘Initial commit’`**
```

```
**`# replace “<remote-url>” with the URL on your repo page on GitHub$ git remote add origin <remote-url>$ git push -u origin master`**
```

#### ****6.将您的 DNS 指向 GitHub 页面。****

****现在，您可以返回到您在 CloudFlare 上的站点，并单击顶部的“DNS”。从这里开始，您将需要添加一个 CNAME 记录。第一个值(名称)将是“@”，第二个值(域名)将是 *username* .github.io，其中 username 是您在 github 上的用户名。****

****除非您有一些子域或其他特殊情况，否则您可以从 CloudFlare 中删除任何其他 CNAME 或 A 记录。不过为了安全起见，我建议你通过点击“高级”和“导出”来备份你的 DNS 记录****

****我个人不喜欢很多域名前缀的“www”。我通过添加第一个值为“www”、第二个值为 *username* .github.io 的另一个 CNAME 记录来消除这个问题，其中 username 是您在 github 上的用户名。****

#### ****7.在 GitHub 上添加一个 CNAME 文件。****

****这个过程的最后一步是告诉 GitHub Pages 我们的域。为此，我们在 GitHub 网站的根目录下添加了一个名为“CNAME”的文件。为此，请运行以下命令:****

```
**`# from your website directory on your local machine`**
```

```
**`$ echo “<my-domain>” >;> CNAME # where “<my-domain>;” is your domain$ git add CNAME$ git commit -m ‘Add CNAME’$ git push origin master`**
```

****如果你觉得这篇文章有用，或者你有什么建议，请在这里回复。****

****在 Twitter 上关注我[这里](https://www.twitter.com/_kirbyk)。****