# 如何使用 Travis CI 和 GitHub 来提升您的 web 开发工作流程

> 原文：<https://www.freecodecamp.org/news/how-to-wire-travis-ci-to-do-the-heavy-lifting-in-your-workflow-72693c855696/>

作者:Vijayabharathi Balasubramanian

# 如何使用 Travis CI 和 GitHub 来提升您的 web 开发工作流程

![E6zYw2NdHFyAyHQgPeCItLA-V7DCvTT0boJs](img/ec89c4a660088bc693e6dfee34c8872d.png)

It all becomes easier with continuous integration via Travis CI

刚起步时，在 [CodePen](https://codepen.io/) 上拼凑应用程序是很常见的。但总有一天，你会希望向全世界发布自己的网络应用。它们可能是附带项目，也可能是客户的项目。在这一点上，一个好的开发工作流程将会有很大的不同。

我们将使用以下工具建立一个开发工作流程:

*   创建-反应-应用
*   npm 脚本
*   特拉维斯-CI

我们会把特拉维斯·CI 和 GitHub 连在一起。最后，我们将得到一个漂亮闪亮的徽章，如下图所示。

但除了外观，徽章还象征着一种功能。这是 Travis CI 测试、构建和发布我们对 GitHub 的承诺的标志。Travis CI 仅在测试通过时发布。

准备好将这些徽章放入您的存储库中:

![0OKbYVdWQeqFIG3PGgeAT-xXvZyhfVQFhsQc](img/0bdb905915ca5092c9b3c94382df5973.png)

我已经分阶段组织了整个工作流程。每个阶段坐一次就够了。大约在 50 分钟内。

### 阶段 1:在本地运行 create-react-app

#### 准备好您的 git 库

第一件事是在 [GitHub](https://github.com/) 上创建一个新的资源库。如果您还没有帐户，现在是注册一个的时候了。公共知识库是免费的。当你创建一个新的资源库时，GitHub 允许你为`.gitignore`、`license`和`README.md`创建文件。

如果你是第一次使用 Git，你可以在线阅读这本免费的[书](https://git-scm.com/book/en/v2)。GitHub 上还有一个[帮助](https://help.github.com/)版块。

这是我们的新存储库的外观:

![b-nypK9R614MkibgvmEpSx0hXhnQqz5eXSws](img/d08afc4677fd6cf54a9aeab91d732d4f.png)

好吧，让我们把它放到终端上。你看到上图中那个亮绿色的按钮了吗，显示的是**克隆或者下载**。这会给我们仓库的 URL。在您的终端上，运行以下命令:

```
git clone git@github.com:pineboat/react-continuous-deployment.git
```

该命令会将存储库的内容下载到一个新目录中。它将目录命名为与存储库相同的名称。在我们的例子中，目录名将是**react-continuous-deployment**。

如果要确保到原始存储库的链接已准备好，请使用以下命令:

`git remote -v`

既然我们已经准备好将我们的更改推送到 GitHub，那么让我们启动并运行 React。

#### 使用`create-react-app`启动并运行

从头开始一个新的 react 项目可能比你预期的要长。尤其是当您没有使用任何预定义的支架时。有几种解决方案可供我们使用。我选择了官方的 [Create React App](https://github.com/facebookincubator/create-react-app) ，因为我已经先试了一次，并坚持了下来。当你不必微观管理你的配置时，它给你一个干净的开始，这样你就可以开始编码了。

正如存储库`README.md`文件所示，您只需全局安装一次。然后，您可以用它来搭建您喜欢许多项目。

要安装它，请键入:

```
npm install -g create-react-app
```

安装完成后，你可以从任何目录运行它来创建一个新的应用程序。让我们将它命名为我们的存储库:

```
create-react-app react-continuous-deployment
```

这将**而不是**创建一个新文件夹，因为我们已经有了 Git 创建的文件夹。相反，它将开始在现有文件夹中安装必要的`node_modules`和 scaffold 脚本。

如果您想要一个全新的应用程序，可以使用:

`create-react-app fancy-app-name`

然后，您需要创建一个 Git 存储库，并将其连接到 GitHub。这并不太难。你可以使用 GitHub 的这个[帮助](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)页面。

安装完成了吗？应该不会超过 5 分钟。我花了大约 15 分钟。不要让这吓到你。除非你和我一样，用的是 150 mb/s 的 4G 数据卡，最好的时候能有 512kbps 的下载速度。

好消息是，安装程序将我们的旧文件移到了一边。终端显示这条甜蜜的信息:

```
You had a `README.md` file, we renamed it to `README.old.md`
```

终端还应该向您展示了树状结构中巨大的包列表。这看起来很吓人。但是大多数都是包之间的依赖。他们在那里帮助你开发你的应用。最终产品将只有必要的 JavaScript 文件，如`react.js`和`react-dom.js`。我们一会儿去那里。

现在，让我们唤醒我们的应用程序。一旦安装完成，
`create-react-app`会给我们一个方便的命令列表。

这里有一个目录供参考:

*   `npm start`
    启动开发服务器
*   `npm run build`
    将 app 捆绑成静态文件用于生产
*   `npm test`
    启动测试运行器
*   `npm run eject`
    从你的项目中移除单个依赖构建参见[此处](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject)
*   `npm run deploy`
    将构建部署到 gh-pages 分支

默认识别`npm start`和`npm test`内置节点命令。您需要使用一个额外的运行标志来运行其他脚本，如构建、弹出和部署:
`npm run script_name`

我们会在这个过程中再添加一些。现在是时候在浏览器上加载我们的站点了。进入应用程序目录并运行:

```
npm start
```

像魔术一样，一个新的浏览器标签打开，你会看到一个漂亮的反应轮转动。这种观点肯定会挑战你，让你去构建互联网即将见证的下一个最好的应用。

这就是我们得到的结果:

![sUEp8EOb2VUO72SH2iNiaj3-utJkqX3uUwyv](img/f80863b47def59308ebaec927a9b2387.png)

这是一个非常漂亮的 GIF(图形交换格式)不是吗？除了我的光标进来。我用一个叫做 [Peek 的工具捕捉到了这张 GIF。](https://github.com/phw/peek)需要的时候就去查。

干得好！我希望没有超过 45 分钟。如果您遇到问题，请注意错误消息并尝试修复它们。

当你快要沮丧的时候，去[栈溢出](https://stackoverflow.com/)寻求帮助。或者在下面的评论中留下你的问题。

在我们侵入任何文件之前，让我们提交代码并推送到 GitHub 库。这些命令将会:

```
git status git add --all git commit -m "Initial Scaffold, this is your own message"
```

*   `git status`
    显示我们所做的更改列表
*   `git add`
    将这些更改添加到一个名为`stage`的临时位置，然后将它推送到云端

如果您再次运行`git status`，它将报告

```
Your branch is ahead of 'origin/master' by 1 commit. (use "git push" to publish your local commits) nothing to commit, working directory clean
```

没错，我们已经在本地做出并提交了更改。没有未提交的更改。但是我们已经领先于云版本的存储库。是时候把它们发布到一个安全屋了，就是 GitHub。

你只需要运行:
`git push origin master`

您将得到一个很好的报告，显示类似于`fb74259..045ec7a`的散列，这是我们提交的参考。当然你的 hash 会不一样。

检查你的 GitHub 库。看到一个长`README.md`你惊讶吗？记住`create-react-app`已经用它自己的代替了我们小小的`README.md`。它很大而且很有帮助，所以在我们写自己的之前，我们暂时保留它。

### 阶段 2:将站点移植到 GitHub 页面

#### 构建静态站点

让我们来看看最终的构建。

你只需要跑:

`npm run build`

确保您在所有命令的应用程序目录中。我们将得到这个漂亮的文本，向我们展示发生了什么以及我们还能做什么:

```
$ npm run build > react-continuous-deployment@0.1.0 build /home/weebee/Projects/blog_projects/react-continuous-deployment> react-scripts buildCreating an optimized production build...Compiled successfully.File sizes after gzip:  48.12 KB  build/static/js/main.9fdf0e48.js  288 B     build/static/css/main.cacbacc7.cssThe project was built assuming it is hosted at the server root.To override this, specify the homepage in your package.json.For example, add this to build it for GitHub Pages:  "homepage" : "http://myname.github.io/myapp",The build folder is ready to be deployed.You may serve it with a static server:  npm install -g serve  serve -s build
```

`npm run build`命令完成了我们的要求。它构建我们的应用程序，优化和缩小我们的资产(T2)。它把所有东西都放在一个名为 **build** 的文件夹中。

接近底部，建议安装`serve` npm 包来启动本地服务器。大多数时候，如果你在 Linux 上，你已经安装了 Python。如果你有 Python，启动一个本地服务器是相当容易的。

进入`**build**`目录并启动服务器，参见以下命令:

```
cd buildpython -m SimpleHTTPServer
```

默认情况下，Python 命令在端口 8000 上启动服务器。所以，`http://localhost:8000`将服务于网站的生产版本。它使用我们刚刚创建的本地**构建**目录中的资产。

如果看起来不错，我们将把它发送到 GitHub 页面。

#### GitHub 页面入门

GitHub 页面是 GitHub 为存储库提供的托管解决方案。有几个地方可以托管您的站点，所有都在一个存储库中:

*   你可以用**主**分支(默认的一个)来托管你的网站
    如果你有`index.html`，它就会出现。不然你的`README.md`就出现了。
*   你也可以使用主分支中的 **docs** 文件夹来托管你的站点
    用例是当你有一个在 GitHub 上开发的软件或库时。您可能希望将文档存放在同一个存储库中。
*   你可以使用 **gh-pages** 分支来托管你的站点

有一个例外。您的存储库名称不应该是`<your_user_name>.git` hub。`io or <orgname&g`t；. github.io

这些是特殊的名字，它们限制你只能使用主分支。

一旦你托管了你的网站，你就可以把它加载到以下网址。这取决于您的存储库是在您的帐户下还是在组织帐户下:

```
https://<your_user_name>.github.io/<your_repository_name>/ 
```

```
https://<organization_name>.github.io/<your_repository_name>/
```

有了这样的理解，让我们来装备我们的存储库以投入使用。

#### 发布到 GitHub 页面

`create-react-app`给我们的新`README.md`在 GitHub 页面上有一个单独的部分。我们需要做的事情很少。

#### **检查添加到`package.json`文件的内容**

```
"homepage": "http://<user_name>.github.io/<your_repository_name", "scripts": { "predeploy": "npm run build", "deploy": "gh-pages -d build"}
```

**注意**:通常，JSON 中的最后一个部分或条目不需要逗号，所有其他部分都应该有一个。

#### 安装 gh 页面包

这个很简单。在项目目录中运行以下命令即可:

`npm install --save gh-pages`

`--save`标志将把`gh-pages`作为一个依赖项添加到`package.json`中。这是为了确保任何克隆你的项目的人在运行`npm install`时也能得到它。

这里是`git diff`命令的快照，显示了自从`package.json`创建以来我们添加的所有内容。

![zhiLdMIfyW2ecDys15MdpTwOLdJPN9Y1kXBJ](img/73b85e7216d2fd145c9be83b78afc53b.png)

#### 部署到 gh 页面分支

我们跑`npm run deploy`吧。它将自动运行`predeploy`，生成我们之前看到的生产版本。然后，它会将构建部署到我们的仓库中一个名为 **gh-pages** 的新分支下。

检查最后一条语句是否为`Published`状态。如果是这样，那么您已经成功地将生产版本部署到了 GitHub。以下是输出:

![tG4iWR7jDJbMuVMzVpWNSnEESUNDVuHr7XsI](img/2ff65f5fb6219a83e9e9484378ae731a.png)

#### 选择要发布的 gh 页分支

让我们前往 GitHub 库并发布我们的站点。打开存储库并转到顶部的设置选项卡。看起来像下面这张图，等一下！GitHub 已经自动发布了 **gh-pages** 分支。没什么可做的了。它还显示了我们可以访问该网站的 URL。

上面的副标题其实应该是说**什么都不做**。都做好了，等着我们去消费。

![gbqqTF9QNBdmD1iyzhkiLvA12ntRAD3dXh0f](img/2c189e988c62f296ac9dfe7792de3d05.png)

**注意**:为我的存储库显示的 URL 可能会误导您。这是因为我在一个名为 PineBoat 的组织下创建了这个知识库，这是我的博客。GitHub 将它放在我的自定义域下，这不是我尝试时所期望的。你的会不同。

到目前为止，一切顺利。如果你以前有过 Git 和 Node 包的经验，那么到目前为止应该没有问题。事实上，默认的`README.md`已经足够帮助我了。如果你没有经历过，我希望你喜欢这次旅行。

但是我们渴望一个持续的部署工作流程。我们开始在未知的水域航行。有人会说，在互联网上没有什么是未知的。我同意，但创建我自己的地图。

### 阶段 3:持续部署

这就是我们引入机器人来完成我们在阶段 2 中所做的大部分部署的地方。

#### Travis CI 中用于自动构建的连线

让特拉维斯. CI 为我们做部署。独立构建和部署您的站点没有坏处。正如我们已经看到的，它所需要的只是我们宝贵时间的几分钟。

然而，当你遇到更大规模的项目时，最好让可信的机器人来做一些工作。特拉维斯 CI 就是这样一种服务。

每当我们将代码提交到存储库时，我们都可以利用 Travis CI 进行构建和部署。

#### 注册 Travis CI

如果我现在就以“如果你有 GitHub 账号”开头会很烦。我敢肯定你现在有一个，我们可以用它来注册特拉维斯 CI。

#### 连接到 GitHub 存储库

注意权限。如果您的存储库没有列出，请单击 **sync** 按钮并刷新页面。在我能看到仓库之前，我必须授予“PineBoat”组织许可。

特拉维斯 CI 向您展示步骤。轻击存储库上开关以连接它。

单击存储库名称将其打开。它将显示一个构建状态为**未知**和一个更大的注释，说明**没有这个库的构建**。

![VRqkHvHEBLL5KY2SUDCUZ534Cxoc8Hvfujyi](img/91f6e5ddc486b0ca567a7a9854f3dd28.png)

不会太久的。让我们改变它。

#### 将`.travis.yml`添加到存储库中

这里是需要添加的`.travis.yml`。看一看，和我呆在一起，当我弄清楚你可能有的一些问题。

```
language: node_js
```

```
node_js: - "node"
```

```
after_success: - git config --global user.name "vijayabharathi" - git config --global user.email "[email protected]" - git remote rm origin - git remote add origin https://vijayabharathib:${GH_TOKEN}@github.com/pineboat/react-continuous-deployment.git - npm run deploy
```

小心这个`git remote add origin`，这是一条很长的线。`YAML` 的语法与`JSON`略有不同。[本页](http://docs.ansible.com/ansible/latest/YAMLSyntax.html)可能会有帮助。现在是时候分解它了。你可能已经理解了这些信息中的大部分。

这里用简单的英语说就是:

*   这是一个节点项目。获取最新的节点版本
*   因为 Travis 默认运行`npm test`，所以我们要求它在测试成功后做一些事情
*   添加您的 Git 用户名和电子邮件
*   然后为存储库
    添加`git remote origin`,使用您的用户名和生成的 GH_TOKEN 作为凭证
*   最后，运行`npm run deploy`命令
    如果你还记得，这将在运行`npm run deploy`之前运行`npm run predeploy`

#### 提交并观看 Travis CI 构建

保持 Travis CI 存储库页面打开。在您的终端上添加更改，提交并推送到 GitHub。

如果您需要提醒，以下是命令列表:

```
git add --all git commit -m "add .travis.yml configuration for automatic build" git push origin master
```

如果您切换到 Travis CI 页面，您会看到页面在`git push`结束后或几秒钟内变得生动起来。构建会自动开始，您会知道它是否成功。

这是我的特拉维斯 CI 页面显示良好的绿色状态。

![IczxuHQPRW8kvJPJza0WYmij24QUcTkf-tfr](img/a253be9363320061b72962d4ba809595.png)

显示的日志不少于 2500 行。我很高兴 Travis-CI 只展示了我们需要看的东西。

如下图所示，清晰地显示了遵循的步骤:

![XUh1uyAQUpPSni0UyPkrPDbUoFP8ZKOsCIyi](img/d44a738fc30ef789ffbc7c07432c53c1.png)

### 抽查，我们真的成功了吗？

这就是运行在产品上的自动化测试可以派上用场的地方。

但那是以后的事了。Selenium WebDriver 可以等到我们完成这个连接。让我们手动检查 Travis CI 是否真的发布到了 GitHub pages。

#### 另一个试验，这次是修改代码

上次，我们在部署后的应用程序中看不到任何差异。那是因为我们没有做。所以没有办法知道构建是否成功。您可以加载 gh-pages 分支并查看提交，但是我离题了。

现在，让我们做一些小的改变。是时候把反应轮收回来了。

我们只做两处改动。

在`src/App.css`文件中，有一个名为`@keyframes App-logo-spin`的动画部分。把那个`360deg`改成`-360deg`。这是逆时针旋转轮子。

加载`src/logo.svg`文件，将填充颜色从`#61DAFB`改为`#DA61FB`。如果你的服务器正在通过`npm start`运行，你已经可以看到一个紫色的轮子逆时针运行。如果没有，将变更添加到存储库中，提交并将其推送到存储库中。观察 Travis-CI 中的构建是否成功，然后前往您的 Github 页面。

加载`your_user_name.github.io/repository_name`。你应该看到紫色的轮子，而不是蓝色的。

唉，我没看到那个紫色的轮子。我还是看到默认的蓝色。

#### 修复丢失的 GH_TOKEN

尽管 Travis CI 报告称一切正常，但事实并非如此。如果您打开 **gh-pages** 分支，您将看到我们从本地终端进行的原始提交。没有其他提交。这意味着，`after_success`命令并不成功。

如果您展开 Travis 构建日志中的`npm run deploy`部分，您会看到一些**认证错误。**那是因为我们还没有允许 Travis CI 写入我们的存储库。

您可以从 GitHub.com 的[个人访问令牌](https://github.com/settings/tokens)页面创建新令牌。记住只允许访问公共存储库。只需在`public_repo`上打一个勾就可以了。**不要错过这个。**一旦你生成了一个令牌，复制它。GitHub 正确地警告说，你将再也看不到它了。

前往 Travis CI，为您的存储库点击**更多选项**，并选择**设置**。它将显示几个部分，但是**环境变量**是要查找的部分。

将令牌命名为`GH_TOKEN`,并通过值字段下的令牌。单击添加。**不要**打开**日志中的显示值**，因为如果你发送日志，人们可能会看到。令牌相当于您的密码。

就这样，现在 Travis-CI 可以写入我们的存储库了。

回到存储库的**当前**选项卡，点击**重启构建**。一旦构建完成，你就可以查看日志和 GitHub 上的 **gh-pages** 分支。您应该会看到一个新的提交。

恭喜你。这是我们的第一次自动化部署。`github.io`网站本身怎么样？再多的更新也不会带来急需的紫色车轮。不要失去希望。

#### 请服务人员休息一下

车轮仍在滴血。但是存储库中的 **gh-pages** 分支显示了第二次提交。让我们比较一下存储库和 web 页面源代码上的`index.html`。他们指向不同的 CSS 和 JavaScript 文件。哈希后缀是我们的线索。

这似乎是精力充沛的 JavaScript 服务人员的结果。它缓存了该页面以供脱机使用。但是这个结论需要更多的研究。与此同时，让我们让服务人员停下来，清理仓库。

如果你在 Chrome 上，**开发者工具**可以通过菜单或者按`F12`进入。Chrome DevTools 上的**应用**标签有**清空存储**部分。检查所有条目，最后点击**清除站点数据**。

刷新轰！这是我们亮紫色的反向轮。现在是庆祝的时候了。

![h2r8cWq2PGEZppd9LJX6gzGDIWqOCfoi1ad5](img/271d9470e8874f5fea2ffc4c1d7c4bf1.png)

**注意**:一定有更好的方法来自动清理存储空间。如果我们每次都必须停下来清理服务人员和存储设备，以便在线进行更改，那将是一件痛苦的事情。这是一个需要更多研究的话题。

### 第四阶段:特拉维斯 CI 荣誉徽章

我们面前还有最后一项任务。那就是把一个闪亮的 Travis CI 构建状态徽章放到我们的存储库的`README.md`文件中。

打开 Travis CI 并点击 **build:passing** 徽章。它会显示一个对话框，里面有移植图像的选项。作为**主人**离开分支。选择**降价**而不是**图片网址**。抄写给你的课文。

粘贴到之前`create-react-app` 留给我们的`README.old.md`里。自己写内容。

您可以从存储库中删除默认的`README.md`，并将`README.old.md`重命名为`README.md`。

将更改添加到 Git staging，提交并推送到云。现在存储库应该显示您一直想要的批处理。这是我们最喜欢的项目的 URL。

```
https://travis-ci.org/pineboat/react-continuous-deployment.svg?branch=master
```

您可以将该 url 添加到顶部的`README.md`中。这是图片:

![wvSWCLbsrzYzZa5arMQ3CX5flVEQRptZoF7Z](img/635ec7a8258d5b13bfe0bd77d1d0ea0c.png)

We are done here! Time to celebrate.

我想留给你一个问题。如果你在一个使用类似工作流程的大型团队中工作，你会面临什么样的挑战，你会如何解决它们？写个评论让我知道。

非常感谢您的阅读。希望你觉得有用。

这篇文章最初发表在的 [pineboat.in。](https://www.pineboat.in/post/travis-to-deploy-react-to-github-pages/)

鼓掌表明你有多欣赏这篇文章。