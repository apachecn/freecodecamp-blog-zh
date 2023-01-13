# 如何设置自己的一次性电子邮件服务器

> 原文：<https://www.freecodecamp.org/news/how-to-setup-your-own-disposable-email-server-b4cfd297afa7/>

作者柳文欢·杰瓦

# 如何设置自己的一次性电子邮件服务器

![1*--gyEd4Bg3ZTAwGztCg8_Q](img/68d42e24ee7bf245ca374f97ba65b4e5.png)

[AHEM — Ad Hoc Email Server](https://www.ahem.email)

一次性电子邮件服务是提供临时电子邮件地址的在线服务，用于在需要电子邮件验证的网站上注册或签约。

这些服务的目的是让您避免将自己的电子邮件地址暴露给潜在的垃圾邮件，尤其是如果您只是在短时间内需要该服务。

一次性电子邮件服务在软件开发和测试中也很有用，因为许多软件产品本身需要电子邮件验证。在软件开发或测试环境中使用真实的电子邮件地址既麻烦又烦人。世界上许多团队使用临时的一次性电子邮件服务来测试他们自己的软件产品。

[咳咳——临时电子邮件](https://www.ahem.email)就是这些服务之一。您可以发送电子邮件到@咳咳. email 地址，并检查[咳咳](https://www.ahem.email)邮箱来检索和阅读该电子邮件。

许多类似的服务，如 [Mailinator](http://www.mailinator.com/) 、[一次性邮件](https://www.throwawaymail.com/)、[临时邮件](https://temp-mail.org)和 [Yopmail](http://www.yopmail.com) 都可以在网上找到，仅举几例。

每个人都有自己对主题的解释，但让[咳咳](https://www.ahem.email)独特的一点是[咳咳的](https://www.ahem.email)代码在 GitHub 上免费开放，允许用户下载并设置自己的临时邮件服务器。

但是为什么有人想要建立自己的一次性邮件服务器呢？在软件测试的环境中，虽然大多数情况下一个在线的一次性电子邮件服务就足够了，但是在某些情况下，你可能想要在现场托管一个临时的电子邮件服务器:

*   一些组织阻止访问一次性电子邮件，甚至只是未知的网站
*   一些质量保证实验室不提供外部互联网接入
*   一些产品需要测试多个或可控的电子邮件域

咳咳满足了所有这些需求，甚至更多。

要安装[咳咳](https://www.ahem.email)，你需要一台具有管理权限的 Linux 或 Windows 机器，并安装 Node.js 版本和 MongoDB。

设置 Node.js、npm 和 MongoDB 超出了本指南的范围，但是如果您迷路了，可以在 [Node.js 下载](https://nodejs.org/en/download/)和 [MongoDB 下载](https://www.mongodb.com/download-center/community)页面找到关于如何设置它们的详细信息。

### 安装咳咳

以下部分详细介绍了安装和运行[咳咳一次性邮件](https://www.ahem.email)服务器所需的步骤。[咳咳](https://www.ahem.email)可以在任何支持 Node.js 的系统上运行
这些步骤是在 Ubuntu Linux 服务器上执行和测试的，可能需要稍加修改才能与其他系统兼容。

#### 安装角度指示器

[咳咳](https://www.ahem.email)使用 Angular 进行前端交付，因此您需要在全球范围内安装 angular-cli:

```
npm install -g @angular/cli
```

#### 并发安装

Concurrently 是一个 JavaScript 库，允许并发运行多个脚本。在此配置中，[咳咳](https://www.ahem.email)使用并发运行后端节点 API 和电子邮件服务器，并通过 angular-cli 直接为前端提供服务:

```
npm install -g concurrently
```

#### 克隆咳咳 GitHub 存储库

```
git clone https://github.com/o4oren/ahem-server.git
```

#### 在创建的文件夹中安装依赖项

```
cd ahem-servernpm install
```

#### 更新配置

名为 properties.json 的配置文件位于项目的根目录中。编辑它以适合您的喜好。

```
vim properties.json
```

properties.json 文件将如下所示:

下面是对属性文件中参数的解释:

*   **server base uri**-API 服务器的基址。
*   **mongoConnectUrl**-MongoDB 连接 Url。
    例如:“MongoDB://localhost:27017/咳咳”。
*   **appListenPort** -节点 app 绑定的端口。
*   **SMTP port**-SMTP 服务器的端口。请注意，默认情况下，它被设置为 2525 —这是出于测试目的，因为在许多系统上，只有系统帐户可以侦听端口 25。要接收标准 SMTP 电子邮件，请将其更改为 25。
*   **emailDeleteInterval** -清除旧邮件的年龄检查间隔时间(秒)。
*   **emailDeleteAge** -以秒为单位的最长期限，超过该期限的电子邮件将被删除。
*   **allowedDomains** -一组允许的电子邮件域。服务器将允许这些域作为 RCPT TO:条目。这也使得服务器不能充当开放中继。格式:["my.domain.com "，" my.second-domain.com"]。
*   **customText** -一个 html 字符串，它将替换登录页面中的默认文本。
*   **allowAutocomplete** -如果设置为 false，将阻止用户在 UI 中自动完成。

#### **构建项目**

```
npm run build:ssr
```

这可能需要一段时间…

#### 跑咳咳

此时，确保您的 MongoDB 服务器已经启动并正在运行，并且您的 properties.json 文件已经正确配置。

运行咳咳的最简单方法是使用以下命令运行项目:

```
node ahem.js
```

该命令将在端口 3000 上启动(默认情况下)后端服务器，前端将在端口 4200 上运行。然后你可以在 [http://localhost:4200 访问](http://localhost:4200.)[咳咳](https://www.ahem.email) web 界面。

如果你觉得有用，请鼓掌或开始使用 [GitHub Repo](https://github.com/o4oren/Ad-Hoc-Email-Server) ！