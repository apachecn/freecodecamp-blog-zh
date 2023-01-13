# 如何将静态 Gatsby 应用程序部署到 Heroku

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-a-static-gatsby-app-to-heroku-3362e3ecda0f/>

克里斯汀·鲍曼

# 如何将静态 Gatsby 应用程序部署到 Heroku

![7A-6qjzSvZnR-Vdg2H7asQ3T5Ot9g0nW0Ef7](img/25b1bab80f42f227914d9ec8632bdbac.png)

Photo by [Vinícius Henrique](https://unsplash.com/photos/SlUrsOx78kw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本教程解释了如何使用 Heroku 和 Github 设置静态 GatsbyJS 项目的部署。您将了解如何为您的应用程序创建一个试运行和生产环境，以便为安全的持续部署做好准备。

完成本教程后，…

*   ✈️:你将能够**构建和部署一个静态的 Gatsby 应用程序**。
*   ？你将能够通过推送你的 git repo 来测试 **rigger 自动部署**到你的**标记环境**。(您可以查看分期应用程序，如果合适，将其推广到您的**生产网站。**)

**要求:**

*   你的项目将基于 [GatsbyJS](https://www.gatsbyjs.org/) (一个静态站点生成器)。用 Gatsby 或者 React 编码不需要任何知识，但是要安装[节点](https://nodejs.org/en/download/)和 [GatsbyJS](https://www.gatsbyjs.org/docs/) 。
*   你需要一个 [Github](https://github.com/) 和 [Heroku](https://heroku.com/) 账户(两者都是免费的)。需要在您的机器上设置 Git。

### 1.)创建一个新的盖茨比项目

首先，你需要一个新的盖茨比项目。

*   通过在我们的控制台中执行以下操作，您可以在文件夹`test-project`中创建一个新项目:

```
gatsby new test-project https://github.com/gatsbyjs/gatsby-starter-hello-world
```

这从一个启动包中为静态 Gatsby 应用程序创建了必要的文件。您可以通过使用`cd test-project`进入项目目录，然后运行`gatsby develop`，在本地启动开发服务器。您的应用程序现已在`localhost:8000`上线。

### 2.)建立一个 git 回购

随着项目在本地运行，您现在可以为您的 Gatsby 项目设置一个 git 存储库。

*   登录 Github，创建一个新的 repo。
*   使用以下命令初始化项目中的 git repo:

```
git init
```

*   使用以下命令将本地 git repo 连接到远程 repo:

```
git remote add origin <remoteURL>
```

*   初次提交 Gatsby 项目时，请使用:

```
git add .git commit -m "Initial commit"git push origin master
```

您的 Gatsby 项目中的变更现在可以通过 Github 进行跟踪，这将为以后启动部署提供触发。

### 3.)设置 Heroku 应用程序

下一步，您可以在 Heroku 上配置连续部署环境。

*   在 Heroku 应用仪表板中创建一个名为`test-project`的新管道
*   在这个管道中，为暂存环境创建一个名为`test-project-staging`的新应用，为生产环境创建一个名为`test-project-prod`的新应用
*   将管道(不是每个应用程序单独)与您之前创建的 Github repo 连接起来
*   从主分支为暂存应用程序启用自动部署(但不为生产应用程序启用！)
*   将两个应用程序的构建包设置为:

```
"heroku/nodejs" 
```

```
"https://github.com/heroku/heroku-buildpack-static"
```

这些构建包是在部署应用程序时运行的脚本，并且特定于静态 Gatsby 项目。您可以在下一步配置静态构建包。

![qNtbliSCD21VxlTI5LE69J5KajixsfdmMSth](img/ece3d1d943cef357acf9a1ca345b179e.png)

Your Heroku setup including a staging and production environment

### 4.)准备您的 Gatsby 项目以部署到 Heroku

*   将您的代码复制到 Heroku 并安装了必要的依赖项之后，需要构建 Gatsby 项目并将其存储在 static /public 文件夹中。因此，在您的`package.json`文件中添加一个构建脚本:

```
{     // ...
```

```
 scripts: {         // ...
```

```
 “heroku-postbuild”: “gatsby build”
```

```
 },
```

```
 // ...}
```

*   在项目的根目录下创建一个名为`app.json`的文件。该文件包含在 Heroku 上运行应用程序所需的一般信息。在我们的例子中，我们再次说明了两个构建包的用法:

```
{
```

```
 "buildpacks": [
```

```
 { "url": "heroku/nodejs" },
```

```
 { "url": "https://github.com/heroku/heroku-buildpack-static" }
```

```
 ]
```

```
}
```

*   在项目的根目录下创建一个名为`static.json`的文件。`static.json`文件用于静态构建包的配置。点击可以查看更多配置选项[。在这种情况下，我们只定义我们构建的应用程序的文件夹:](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-static)

```
{
```

```
 "root": "public/"
```

```
}
```

*   (可选)当您的项目目录中有一个`package-lock.json`和一个`yarn.lock`文件时，Heroku 的部署将会失败。在这种情况下，决定一个。例如，删除`package-lock.json`文件，以防你使用 yarn。

### 5.)测试您的设置

恭喜，差不多完成了！？

现在，您可以通过将上一步中的更改提交到 Github 来测试您的设置:

```
git add .git commit -m "Prepared Heroku deployment of Gatsby app"git push origin master
```

这应该会触发您的 Gatsby 项目到登台环境的自动构建和部署。然后，您可以查看暂存应用程序，如果合适，将其推广到您的生产网站。

*感谢您阅读这篇文章！请留下问题或反馈，关注我在 [Twitter](https://twitter.com/kristin_baumann) 上的更多 JavaScript 和 React 相关帖子。*