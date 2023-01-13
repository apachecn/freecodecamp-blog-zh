# 如何用 Netlify 在子域上托管 Git 存储库

> 原文：<https://www.freecodecamp.org/news/how-to-host-a-git-repository-on-a-subdomain-with-netlify-b8afb5fca96e/>

假设你有自己的作品集，比如`[www.glynlewington.com](http://www.glynlewington.com)`，托管在 Netlify 上，你想把你的项目添加到同一个域中。它们都是独立的 git 库，Netlify 是为从一个库托管而设计的……但是不要害怕！我们只需要做一点点工作就可以把它们放在像`project.glynlewington.com`这样的子域名上。

Netlify 让免费托管你的静态网站变得非常容易。我最近把我的作品集从 VPS 转移到了他们那里，每次你推送到你的 git 库，他们都会自动更新你的站点，这很棒。

过去，我所有的个人项目都存放在子目录中，例如`www.glynlewington.com/project`。对于网络生活来说，这要么很难，要么不可能。Netlify 主要是为您从一个 git 存储库中托管一个站点中的所有内容而设置的。

我达成的妥协是把它们放在子域名上，比如`project.glynlewington.com`。这也没有很好的记录，但这是可能的。

*   前往[www.netlify.com](http://www.netlify.com,)并登录或注册。
*   选择“从 git 新建网站”。
*   选择您的提供商(例如 GitHub) —您可能需要在此处进行鉴定。
*   选择要从中创建站点的 git 存储库。
*   选择要部署的分支。
*   选择任何需要运行的命令。— *如果是 React 应用，你需要运行一个构建命令。*
*   选择您将发布的目录。它将包含文件，如 index.html。— *如果是 React 应用，这可能是构建文件夹。*
*   选择“构建站点”。

此时，你应该有一个运行正常的应用程序托管在一个免费的域名上，比如`https://hungry-bose-fb0e6d.netlfiy.com`。如果这不起作用，请检查构建过程中是否有错误，如果有，请修复这些错误。

现在来设置一个自定义域。

*   在 Netlify 上查看您的应用概述。
*   它会说您的站点部署成功，您可以设置一个自定义域。
*   点击设置自定义域，输入你想要的域，包括子域，然后点击验证。如`project.glynlewington.com`。

接下来，登录到您的域名托管提供商。我使用 Cloudflare，但使用其他产品会有相同或相似的效果。

*   转到 DNS 设置。
*   选择一项新的 CNAME 记录。
*   输入一个“名称”——这是子域，应该与您之前在 Netlify 上选择的相同。例如`project`
*   在“IPv4 地址”下输入您的网络站点的自由域。如`hungry-bose-fb0e6d.netlify.com`。
*   如果您也在使用 Cloudflare，请关闭通过 Cloudflare 的流量路由。这扰乱了 Netlify。
*   添加记录。

搞定了。一旦你这样做了，你就可以在子域上访问你的站点。

Netlify 还会自动为你的站点添加 https 安全，这点不用担心。