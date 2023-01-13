# 我建了这个-现在呢？如何在 DigitalOcean Droplet 上部署 React 应用程序？

> 原文：<https://www.freecodecamp.org/news/i-built-this-now-what-how-to-deploy-a-react-app-on-a-digitalocean-droplet-662de0fe3f48/>

安德烈·斯特灵汉姆

# 我建了这个-现在呢？如何在 DigitalOcean Droplet 上部署 React 应用程序？

![ZuIBTudJzFcboPgjNVD5b9my0hxkRgJye-pQ](img/e8a66942b1b4142c96f0e3d66cb25f1f.png)

Photo by [Thomas Kvistholt](https://unsplash.com/photos/oZPwn40zCK4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大多数有抱负的开发人员以前都上传过静态 HTML 网站。这个过程并不令人畏惧，因为本质上你只是把文件从一台计算机转移到另一台计算机，然后砰！网站。

但是那些致力于学习 React 的人经常花费数百甚至数千个小时来学习组件、道具和状态，结果只剩下一个问题“我该如何管理它？”别害怕，开发伙伴。部署您最新的杰作稍微深入一点，但并不过分困难。方法如下:

### 生产准备

为了让您的应用程序做好部署准备，您需要做一些事情。

#### 关闭服务人员

如果你已经使用了类似 create-react-app 这样的东西来引导你的项目，如果你还没有专门集成它来与你的应用一起工作，你会想要关闭内置的服务工作器。虽然通常是无害的，但它可能会导致一些问题，所以最好在一开始就消除它。在你的`src/index.js`文件中找到这几行并删除它们:`registerServiceWorker();` `import registerServiceWorker from ‘register-service-worker’`

#### 准备好您的服务器

为了获得最大的回报，产品版本将会缩减代码，删除多余的空白和注释，以便尽可能快地下载。它创建了一个名为`/build`的新目录，我们需要确保我们告诉 Express 使用它。在您的服务器页面上，添加这一行:`app.use( express.static( `${__dirname}/../build` ) );`

接下来，您需要确保您的路线知道如何到达您的 index.html 文件。为此，我们需要创建一个端点，并将其放在服务器文件中所有其他端点的下面。它应该是这样的:

```
const path = require('path')app.get('*', (req, res)=>{  res.sendFile(path.join(__dirname, '../build/index.html'));})
```

#### 创建生产版本

既然 Express 知道使用`/build`目录，那么是时候创建它了。打开您的终端，确保您在您的项目目录中，并使用命令`npm run build`

#### 保护好你的秘密

如果您正在使用 API 键或数据库连接字符串，希望您已经将它们隐藏在一个`.env`文件中。部署和本地之间的所有不同配置也应该放入这个文件中。标签不能被代理，所以当使用 React dev 服务器时，我们必须在后端地址中硬编码，但是我们希望在生产中使用相对路径。您得到的`.env`文件可能看起来像这样:

```
REACT_APP_LOGIN="http://localhost:3030/api/auth/login"REACT_APP_LOGOUT="http://localhost:3030/api/auth/logout"DOMAIN="user4234.auth0.com"ID="46NxlCzM0XDE7T2upOn2jlgvoS"SECRET="0xbTbFK2y3DIMp2TdOgK1MKQ2vH2WRg2rv6jVrMhSX0T39e5_Kd4lmsFz"SUCCESS_REDIRECT="http://localhost:3030/"FAILURE_REDIRECT="http://localhost:3030/api/auth/login"
```

```
AWS_ACCESS_KEY_ID=AKSFDI4KL343K55L3AWS_SECRET_ACCESS_KEY=EkjfDzVrG1cw6QFDK4kjKFUa2yEDmPOVzN553kAANcy
```

```
CONNECTION_STRING="postgres://vuigx:k8Io13cePdUorndJAB2ijk_u0r4@stampy.db.elephantsql.com:5432/vuigx"NODE_ENV=development
```

#### 推动你的代码

到`[http://localhost:3030](http://localhost:3030)`本地测试你的应用，用你的服务器端口替换 3030，以确保一切仍然顺利运行。记住用 node 或 nodemon 启动本地服务器，这样当您检查它时，它就启动并运行了。一旦一切看起来不错，我们就可以把它推送到 Github(或者 Bit Bucket 等)。

**重要！**在你这么做之前，仔细检查你的`.gitignore`文件是否包含`.env`和`/build`，这样你就不会发布敏感信息或不必要的文件。

### 设置数字海洋

[DigitalOcean](https://www.digitalocean.com/) 是一个领先的托管平台，使部署 React 站点变得相对容易且经济高效。他们使用 Droplets，这是他们用于服务器的术语。在我们创建水滴之前，我们还有一点工作要做。

#### 创建 SSH 密钥

服务器是拥有公共 IP 地址的计算机。因此，我们需要一种方式来告诉服务器我们是谁，这样我们就可以做一些我们不希望别人做的事情，比如对我们的文件进行修改。你的日常密码不够安全，而且要记住一个足够长且复杂的密码来保护你的 Droplet 几乎是不可能的。相反，我们将使用 SSH 密钥。

![j4ZwaQlN-vlDVs20wxu3Xxq9QccGKA-gLdLp](img/33536b420e8b22dc13bc47f3c2dfb0bd.png)

Photo by [Brenda Clarke](https://www.flickr.com/photos/37753256@N08/)

要创建您的 SSH 密钥，请在您的终端中输入以下命令:`ssh-keygen -t rsa`

这将启动 SSH 密钥生成过程。首先，会要求您指定保存新密钥的位置。除非您已经有了需要保留的密钥，否则您可以保留默认位置，只需按 enter 键继续。

作为额外的安全措施，以防有人得到你的电脑，你必须输入密码来保护你的钥匙。当您键入这个密码时，您的终端不会显示您的击键，但它会跟踪它。一旦你按下回车键，你必须再次输入来确认。如果成功，您现在应该会看到类似这样的内容:

```
Generating public/private rsa key pair.Enter file in which to save the key (/Users/username/.ssh/id_rsa):Enter passphrase (empty for no passphrase):Enter same passphrase again:Your identification has been saved in demo_rsa.Your public key has been saved in demo_rsa.pub.The key fingerprint is:cc:28:30:44:01:41:98:cf:ae:b6:65:2a:f2:32:57:b5 user@user.localThe key's randomart image is:+--[ RSA 2048]----+|=*+.             ||o.               || oo              ||  oo  .+         || .  ....S        ||  . ..E          || . +             ||*.=              ||+Bo              |+-----------------+
```

#### 发生了什么事？

您的电脑上已经创建了两个文件— `id_rsa`和`id_rsa.pub`。`id_rsa`文件是您的私钥，当您使用`id_rsa.pub`文件或公钥时，它用于验证您的签名。我们需要把我们的公钥给数字海洋。要得到它，输入`cat ~/.ssh/id_rsa.pub`。您现在应该看到一长串字符，这是您的`id_rsa.pub`文件的内容。它看起来像这样:

```
ssh-rsaAABC3NzaC1yc2EAAAADAQABAAABAQDR5ehyadT9unUcxftJOitl5yOXgSi2Wj/s6ZBudUS5Cex56LrndfP5Uxb8+Qpx1D7gYNFacTIcrNDFjdmsjdDEIcz0WTV+mvMRU1G9kKQC01YeMDlwYCopuENaas5+cZ7DP/qiqqTt5QDuxFgJRTNEDGEebjyr9wYk+mveV/acBjgaUCI4sahij98BAGQPvNS1xvZcLlhYssJSZrSoRyWOHZ/hXkLtq9CvTaqkpaIeqvvmNxQNtzKu7ZwaYWLydEKCKTAe4ndObEfXexQHOOKwwDSyesjaNc6modkZZC+anGLlfwml4IUwGv10nogVg9DTNQQLSPVmnEN3Z User@Computer.local
```

现在*那是*的一个密码！手动复制字符串，或者使用命令`pbcopy < ~/.ssh/id_rsa.` pub 让终端为您复制它。

#### 将您的 SSH 密钥添加到数字海洋

登录您的 DigitalOcean 帐户，如果您还没有注册，请注册。转到您的[安全设置](https://cloud.digitalocean.com/settings/security)，点击添加 SSH。粘贴你复制的密钥，并给它一个名字。您可以随意命名，但是最好引用保存密钥的计算机，尤其是如果您经常使用多台计算机。

#### 创建一个液滴

![XhAxSEJtF-5YqybAqqiqlvHcgjl1nhU11ybt](img/46acd8569367113f61c6bb0e614b38c7.png)

Photo by [M. Maddo](https://www.flickr.com/photos/14141796@N05/)

有了钥匙，我们终于可以创建我们的水滴。若要开始，请点按“创建 Droplet”。你会被要求选择一个操作系统，但是对我们来说，默认的 Ubuntu 就可以了。

你需要选择你想要使用的水滴大小。在许多情况下，最小的液滴也可以。但是，请检查可用的选项，并选择最适合您的项目的选项。

接下来，为您的 Droplet 选择一个数据中心。选择一个位于预期游客群中心的位置。DigitalOcean 在不同时间在不同的数据中心推出了新功能，但除非你知道你想使用仅在特定位置可用的特殊功能，否则这无关紧要。

如果您想要为 Droplet 添加额外的服务，如备份或专用网络，您可以在此处选择。请注意，这些服务是有相关费用的。

最后，确保您的 SSH 键被选中，并为您的 Droplet 命名。在一个 Droplet 上可以托管多个项目，所以您可能不想给它一个特定于项目的名称。单击页面底部的“创建”按钮提交您的设置。

#### 连接到您的 Droplet

创建好 Droplet 后，我们现在可以通过 SSH 连接到它。复制您的 Droplet 的 IP 地址，然后返回到您的终端。输入 ssh，然后输入 root@youripaddress，其中 youripaddress 是 Droplet 的 IP 地址。应该是这样的:`ssh root@123.45.67.8`。这将告诉您的计算机，您希望以 root 用户的身份连接到您的 IP 地址。或者，如果你不想以 root 用户身份登录，你可以[设置用户账户](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)，但这不是必须的。

#### 安装节点

![yuVxVs5VxSTn0vkLxPKclZXDgs60-xuMWuE1](img/c86f753309945c0588a41beca6979f06.png)

要运行 React，我们需要 Node 的更新版本。首先，我们想运行`apt-get update && apt-get dist-upgrade`来更新 Linux 软件列表。接下来，输入`apt-get install nodejs -y`、`apt-get install npm -y`和`npm i -g n`来安装 Nodejs 和 npm。

您的 React 应用程序依赖项可能需要特定版本的 Node，因此通过在项目目录中运行`node -v`来检查您的项目正在使用的版本。您可能想在不同的终端标签中这样做，这样您就不必再次通过 SSH 登录。

一旦您知道您需要什么版本，返回到您的 SSH 连接并运行`n 6.11.2`，用您特定的版本号替换 6.11.2。这可以确保 Droplet 的 Node 版本与您的项目相匹配，并最大限度地减少潜在的问题。

### 将您的应用程序安装到 Droplet

所有的基础都已经打好了，终于到了安装我们 React app 的时候了！当仍然通过 SSH 连接时，确保您在您的主目录中。不确定的话可以输入`cd ~`带你去。

要将这些文件放到 Droplet 中，您需要从 Github repo 中克隆它们。从 Github 抓取 HTTP 克隆链接，在你的终端输入`git clone [https://github.com/username/my-react-project.git](https://github.com/username/my-react-project.git)`。就像您的本地项目一样，使用`cd my-react-project`将 cd 放入您的项目文件夹，然后运行`npm install`。

#### 不要忽略您忽略的文件

还记得我们告诉 Git 忽略`.env`文件，所以它不会包含在我们刚刚拉下的代码中。我们现在需要手动添加它。`touch .env`将创建一个空的`.env`文件，然后我们可以使用`nano .env`在 nano 编辑器中打开它。复制本地`.env`文件的内容并粘贴到 nano 编辑器中。

我们还告诉 Git 忽略构建目录。这是因为我们刚刚测试了生产版本，但现在我们将在 Droplet 上再次构建它。使用`npm run build`再次运行该程序。如果您得到一个错误，检查以确保您的`package.json`文件中列出了您的所有依赖项。如果有任何缺失，npm 安装这些软件包。

#### 启动它！

用`node server/index.js`(或者你的服务器文件的名字)运行你的服务器，确保一切正常。如果它抛出一个错误，再次检查任何可能在构建过程中没有被捕获的缺失的依赖项。如果一切都启动了，您现在应该能够访问 ipaddress:serverport 来查看您的站点:`123.45.67.8:3232`。如果你的服务器运行在端口 80，这是一个默认端口，你可以只使用 IP 地址而不指定端口号:`123.45.67.8`

![Lmb7jLrh1rmpnZFyV0lHZhRpwxCj-4b58LJk](img/ad9ae0ff16bf11cba6b8e733db95a052.png)

Photo by [John Baker](https://unsplash.com/photos/3To9V42K0Ag?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/key?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你现在在网上有一个属于你自己的空间了！如果你已经购买了一个你想用来代替 IP 地址的域名，你可以按照 [DigitalOcean 的说明](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)来设置。

#### 保持运转

你的站点是活动的，但是一旦你关闭终端，你的服务器就会停止。这是一个问题，所以我们需要安装更多的软件来告诉服务器在连接终止后不要停止。这里有一些选项，但是为了这篇文章，让我们使用程序管理器 2。

如果你还没有关闭你的服务器，运行`npm install -g pm2`。一旦安装完毕，我们可以告诉它使用`pm2 start server/index.js`来运行我们的服务器

### 更新您的代码

在某个时候，你可能想要更新你的项目，但是幸运的是上传变更是快速和容易的。一旦您将代码推送到 Github，ssh 进入 Droplet，cd 进入项目目录。因为我们最初是从 Github 克隆的，所以这次不需要提供任何链接。只需运行`git pull`就可以下载新代码。

要合并前端更改，您需要使用`npm run build`再次运行构建过程。如果您已经对服务器文件进行了更改，运行`pm2 restart all`重新启动 PM2。就是这样！您的更新现在应该是实时的。