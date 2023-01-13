# 我如何使用 Create React App DevOps 自动完成工作中所有无聊的部分

> 原文：<https://www.freecodecamp.org/news/lets-write-create-react-app-devops-together-dc19512c6fbb/>

詹姆斯·劳胡特

# 我如何使用 Create React App DevOps 自动完成工作中所有无聊的部分

![TncB0VfTHD1KExrx6UrEE5pDC2BCJd1bjN6L](img/7b1310a607dffcfe1bc0423ea05d8e8a.png)

[Image credit](https://www.pexels.com/photo/space-ship-launching-under-blue-sky-during-daytime-57769/)

当你作为团队中唯一的设计者之一——也可能是开发人员——承担责任时，自动化就成了你最好的朋友。

在工作中，我既有设计师的职责，有时也是一名单独的开发人员。这意味着没有太多的时间来配置我正在工作的开发环境。当我不得不手动将应用程序更新到他们的在线环境时，时间也被浪费了。

值得庆幸的是，有一些免费的工具可以帮助我们快速原型化和发布:Create React App、Bluemix、GitHub 和 Travis CI。我将与您分享我如何使用所有这些来自动化我的工作中所有无聊的部分，使用 [Create React App DevOps](https://github.com/seejamescode/create-react-app-devops) 。

> 2017 年 3 月 3 日更新:感谢一位评论者的提醒，我被警告不要在生产中使用 Babel-Node(在 Bluemix 上)。[创建 React App DevOps](https://github.com/seejamescode/create-react-app-devops) 现在在 v1.1.0 中反映了这一点！

有三种方法可以让你自己适应这个过程:

*   跟随这篇文章，我们一起写这个项目
*   检查初始 Create React 应用程序使用和最终提交之间的比较: [Github 第一次和最后一次提交之间的比较](https://github.com/seejamescode/create-react-app-devops/compare/0dbaf64a02f0eeedba2e5a134d472a58b3fc55a5...master)
*   派生回购并遵循以下说明: [Fork Create React App DevOps](https://github.com/seejamescode/create-react-app-devops#fork-destination-box)

请访问:https[://create-react-app-devo PS . mybluemix . net/](http://create-react-app-devops.mybluemix.net/)

如果你想知道这个项目的内幕，那就继续阅读和我一起做吧！将有六个部分:

1.  使用 Create React App 启动用户界面
2.  用 Node、Express 和 Babel 设置您的服务器
3.  使用 blue mix 在网络上运行应用程序
4.  使用 Travis CI 从 Github 自动部署
5.  获取 API 数据，同时保持密钥安全
6.  创建一个用于实验的分期应用程序

### [使用 Create React App 打开用户界面](https://github.com/seejamescode/create-react-app-devops/commit/0dbaf64a02f0eeedba2e5a134d472a58b3fc55a5)

刚开始用 React 做前端项目的时候，浪费了很多时间。很多时间都在配置 Webpack 和各种插件。幸运的是，Create React App 的创建是为了保持您的项目配置正确。有一个选项可以“弹出”您的 Create React 应用程序项目，但我避免弹出。这样我就可以继续收到脸书对项目的更新。

1.  [安装节点](https://docs.npmjs.com/getting-started/installing-node)来管理我们使用的包和服务器。
2.  [装纱](https://yarnpkg.com/lang/en/docs/install/#mac-tab)(可选)加快包的安装。如果您选择不这样做，请记住像`yarn run ---`这样的终端命令通常是`npm run ---`。
3.  是时候打开你的终端了。全局安装 Create React 应用:`yarn global add create-react-app`
4.  现在让 Create React App 制作你的项目或你自己的项目，并导航到其中:`create-react-app <app-na`me>App-name>

**补充说明**:任何时候你在这篇文章中看到“< app-name >，你都可以为你的项目取一个独特的名字，比如“超级酷 app”。

#### ？？？？？

您现在可以处理所有客户端(用户界面)代码了！运行`yarn start`并创建 React App 会在你的浏览器中打开一个标签页，向你展示 UI。在`<app-name>`中编辑客户端代码的任何时候；/src/，浏览器将根据更改进行刷新！
？？？？？

### [用 Node、Express 和 Babel 设置您的服务器](https://github.com/seejamescode/create-react-app-devops/commit/aafd7e34b43906814b7bb49e0a3d33e211e81281)

现在让我们运行一个服务器，以便您可以在线托管应用程序。控制自己的节点服务器对于以后使用 API 从 Github 等服务获取数据也很重要。

1.  让我们添加一个节点服务器的所有包。巴别塔相关的包将允许你使用最新的 Javascript 功能:`yarn add babel-cli babel-preset-es2015 babel-preset-stage-2 compression express`
2.  现在在项目文件夹的根目录下创建一个`index.js`文件来表示我们的节点服务器:

```
import compression from 'compression';import express from 'express'; const app = express();const port = process.env.PORT || 8080;app.use(compression()); app.use(express.static('./build')); app.listen(port, (err) => { if (err) {   console.log(err);   return; }
```

```
 console.log(`Server is live at http://localhost:${port}`);});
```

3.您现在可以看到我们在`package.json`中安装的所有依赖项。让我们添加一个名为“bluemix”的脚本来运行服务器，添加一个名为“babel”的部分来配置 Babel:

```
"scripts": {  "bluemix": "babel-node index.js",  "start": "react-scripts start",  "build": "react-scripts build",  "test": "react-scripts test --env=jsdom",  "eject": "react-scripts eject",},"babel": {  "presets": [    "es2015",    "stage-2"  ]}
```

4.`yarn build && yarn bluemix`将构建应用程序并运行服务器。然而，我们希望向服务器添加一个类似于客户端代码的开发模式。通过这种方式，我们可以在编码时看到仅仅保存`index.js`所带来的变化。让我们添加一些可以让我们这样做的依赖:`yarn add babel-watch concurrently --dev`

5.现在更新 package.json 中的“start”脚本，以便我们运行 Create React App 的开发模式和我们的服务器。我们还将添加一个“代理”行。这一行告诉 Create React App 服务器可以接受客户端代码中没有的请求:

```
"proxy": "http://localhost:8081","scripts": {  "bluemix": "babel-node index.js",  "start": "concurrently \"PORT=8080 react-scripts start\" \"PORT=8081 babel-watch index.js\"",  "build": "react-scripts build",  "test": "react-scripts test --env=jsdom",  "eject": "react-scripts eject",},
```

#### ？？？？？

您现在可以在`index.js`中处理服务器端代码了！运行【the Create React 应用程序开发模式和我们的服务器将对保存的更改做出响应！
？？？？？

### [使用 Bluemix 在网络上运行应用](https://github.com/seejamescode/create-react-app-devops/commit/3d61ec57acdbd0988c4aadf402415d290cf9c064)

因为我在 IBM 工作，所以 Bluemix 是我们的首选托管平台。我们不仅在 Bluemix 上托管我们的最终产品，而且还托管任何原型来与同行和用户测试共享。我也将 Bluemix 用于像这样的个人项目，因为它有一个坚实的免费层。

1.  在 [Bluemix](https://www.bluemix.net) 上创建一个免费帐户。
2.  安装 [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) 。由于 Bluemix 建立在一个叫做 Cloud Foundry 的开源项目之上，你会在我们的很多命令中看到“cf”。
3.  类似于`.gitignore`文件，我们要做一个文件，防止不必要的文件上传到 Bluemix。让项目的根文件夹中的`.cfignore`来做这件事:

```
/node_modules .DS_Store npm-debug.log* yarn-debug.log* yarn-error.log*
```

4.现在，我们可以用项目根目录中的一个`manifest.yml`文件告诉 Bluemix 我们应用程序的所有部署设置:

```
---applications:- name: <app-name>  buildpack: https://github.com/cloudfoundry/nodejs-buildpack  command: npm run bluemix  disk_quota: 256MB  memory: 128MB
```

5.最后，用`cf login -a https://api.ng.bluemix.net`从你的终端登录 Bluemix，用`yarn build`构建你的 app，然后用`cf push`把你的 app 推送到这个世界。

#### ？？？？？

大约五分钟后，您的终端应该会在 <app-name>.mybluemix.net 显示该应用程序正在运行！现在全世界都能看到了。一个常见的错误是你的应用名称已经在 Bluemix 上被占用了。简单地选择一个更独特的名字，它应该工作！
？？？？？</app-name>

### [使用 Travis CI 从 Github 自动部署](http://Automagically Deploy from Github with Travis CI)

管理一个应用程序最乏味的部分之一是每次准备好更改时部署它。我甚至会变得懒惰，每当我最终想做的时候就批量部署。多亏了 Travis CI(持续集成)，部署可以变得像管理 Github repo 一样简单。

1.  首先，你需要创建一个 [Github 账户](https://github.com/join)，并在你的电脑上设置 [Git。](https://help.github.com/articles/set-up-git/)
2.  接下来，在[Github.com](https://github.com/new)上创建一个新的 repo，然后按照提供的终端指示将您的项目推送到 Github:

```
git initgit add .git commit -m 'Initial commit'git remote add origin https://github.com/<github-username/<app-name>.gitgit push -u origin master
```

3.现在前往 [Travis CI](https://travis-ci.org/) 用你的 Github 凭证登录。点击“+”图标激活您的新回购。如果您没有看到您刚刚创建的回购，请单击“同步帐户”，然后它应该会出现。

4.然后在 Travis 中点击项目的设置来选择以下选项:

仅当. travis.yml 存在时才构建= On
构建推送= On
限制并发作业= Off
构建拉取请求= On(这将允许 Github 仍然运行您将来为打开的 PRs 添加的任何自动化测试。)
环境变量:`BLUEMIX_PASSWORD = <Your-bluemix-passwo` rd >

5.这里最大的一步是将 Travis 的蓝图作为一个`.travis.yml`文件添加到项目的根目录中:

```
sudo: truelanguage: node_jsnode_js:- '5.7'cache:  yarn: true  directories:    - node_modulesenv:  global:  - CF_API=https://api.ng.bluemix.net/  - CF_USERNAME=<Your-bluemix-email>  - CF_ORG=<Your-bluemix-email>  - CF_SPACE=devbefore_deploy:  - wget https://s3.amazonaws.com/go-cli/releases/v6.12.4/cf-cli_amd64.deb -qO temp.deb && sudo dpkg -i temp.deb  - rm temp.deb  - cf login -a ${CF_API} -u ${CF_USERNAME} -p ${BLUEMIX_PASSWORD} -o ${CF_ORG} -s ${CF_SPACE}  - cf install-plugin autopilot -r CF-Community  - yarn builddeploy:  - edge: true    provider: script    script: if [ "$TRAVIS_PULL_REQUEST" = "false" ]; then cf zero-downtime-push <My-app> -f ./manifest.yml; else echo "PR skip deploy"; fi    skip_cleanup: true    on:      branch: master
```

**重要提示:**请注意，您可以在两个地方插入您的 Bluemix 电子邮件，在一个地方插入 Bluemix 上的应用程序名称！

这里发生了很多事情。所以我试着总结一下:在`before_deploy`部分，Travis 正在构建 app，以你的身份登录 Bluemix，然后下载一个名为 Autopilot 的 Cloud Foundry 插件。然后在 deploy 部分，Travis 决定部署是一个开放的 pull 请求还是对 Github 主分支的实际提交。如果是实际提交，运行 Autopilot 来部署应用程序。

自动驾驶练习蓝绿色部署。这意味着你的应用程序的新版本将在 Bluemix 上被命名为`<my-app>-ven`elerable。如果新版本成功构建并运行，旧版本的应用程序将被删除，新版本将自己重命名为原始名称。如果展开 f`ails, <my-app&g`t；-revestive 保持运行，以便您可以调试日志，旧版本的应用程序保持运行，以便您的用户看到零停机时间！

#### ？？？？？

笨蛋德文普斯，蝙蝠侠！导航到`https://travis-ci.org/<github-username>/<app-nam` e > /builds，你应该会看到一个 Travis 构建即将发生。如果你点击它，你可以看到它开始，并按照它为你部署！
？？？？？

### [获取 API 数据，同时保持密钥安全](https://github.com/seejamescode/create-react-app-devops/commit/2a4fe33006f46b4f036f1846874ef869243d5743)

大多数应用程序使用不同来源的数据来整合他们的报价。对于获取外部数据的应用程序，它们使用 API 来获取数据。为了让 API 确保正确的应用程序获取数据，应用程序被赋予一个密钥来标识自己。不过，我们需要对 Github 回购保守这个关键秘密！

1.  首先，让我们向 [Github API 请求一个密钥](https://github.com/settings/tokens/new)。在“选择范围”下，我们将检查`public_repo`以获取您的回购信息。点击“生成令牌”，然后你会得到一个绿色勾号旁边的关键！
2.  是时候在本地将密钥作为`keys.json`添加到我们项目的根中了:

```
{  "github": "<your-github-key>"}
```

但是，我们不希望您宝贵的密钥被上传到您的 Github repo。因此，将这个文件添加到您的`.gitignore`文件中:

```
# misc.DS_Store.envnpm-debug.log*yarn-debug.log*yarn-error.log*keys.json
```

3.现在我们有了您的密钥，我们可以添加一个服务器请求。使用`yarn add request`将请求安装到您的项目中，然后编辑您的服务器的`index.js`:

```
import compression from 'compression';import express from 'express';import fs from 'fs';import request from 'request';
```

```
const app = express();const port = process.env.PORT || 8080;app.use(compression());
```

```
let keys;if (fs.existsSync('./keys.json')) {  keys = require('./keys.json');} else {  keys = JSON.parse(process.env.VCAP_SERVICES)['user-provided'][0].credentials;}
```

```
app.get('/github', (req, res) => {  request({    url: `https://api.github.com/user/repos?affiliation=owner,collaborator&access_token=${keys.github}`,    headers: {      'user-agent': 'node.js',    },  }, (err, response, body) => {    if (!err && response.statusCode === 200) {      res.send(body);    }  });});
```

```
app.use(express.static('./build'));
```

```
app.listen(port, (err) => {  if (err) {    console.log(err);    return;  }  console.log(`Server is live at http://localhost:${port}`);});
```

您将首先注意到一个 if 语句，它检查我们是否有本地的`keys.json`文件。该声明中的“其他”将涵盖该应用程序稍后在 Bluemix 上的时间。然后，我们有了一个端点，pinging】将返回您的个人资料的回复！

4.打开`src/App.js`从你的服务器获取数据到你的用户界面。添加这些内容后，`yarn start`应该会显示您项目的所有回购:

```
import React, { Component } from 'react';import logo from './logo.svg';import './App.css';
```

```
class App extends Component {
```

```
 state = {    repos: [],  }
```

```
 componentDidMount() {    fetch('/github')    .then(response => response.json())    .then((data) => {      const repos = data.map((repo) =>        <p key={repo.id}>{repo.name}</p>      );
```

```
 this.setState({ repos })    });  }
```

```
render() {    return (      <div className="App">        <div className="App-header">          <img src={logo} className="App-logo" alt="logo" />          <h2>Welcome to React</h2>        </div>        <p className="App-intro">          To get started, edit <code>src/App.js&lt;/code> and save to reload.        </p>        <h3>App Creator's Repos:</h3>        {this.state.repos}      </div>    );  }}
```

```
export default App;
```

5.现在我们可以在开发模式下安全地使用 Github API，让我们确保您的 Bluemix 应用程序也可以获得 API 密钥。我们将在终端中的 Bluemix 上创建一个用户提供的服务:`cf cups keys -p keys.json`。然后告诉`manifest.json`应用程序应该始终将自己绑定到该服务:

```
---applications:- name: <app-name>  buildpack: https://github.com/cloudfoundry/nodejs-buildpack  command: npm run bluemix  disk_quota: 256MB  memory: 128MB  services:     - keys
```

旁注:如果你需要更新 Bluemix 上的密钥，你可以运行`cf uups keys -p keys.json`！

#### ？？？？？

Travis 更新了你的 Bluemix 应用程序后，你应该会看到 UI 在网上实时获取你的所有回复！我们费了很大劲才把钥匙从 github.com 身上拿走。这是因为如果其他开发人员获得了大量的 API 键，他们可能会滥用你的 API 键。
？？？？？

### [创建一个分期应用程序进行实验](https://github.com/seejamescode/create-react-app-devops/commit/e792b417e6a6b843516fd485668587bc9f513c04)

现在，我们的应用程序生产版本已经设置了 DevOps，让我们构建一个分期应用程序。这将有助于我们分享用户测试和同行评审的进展！

1.  我们现在需要为 Bluemix 创建一个清单文件，指定我们新的 staging 应用程序。在项目的根目录下，创建一个`manifest-staging.yml`文件:

```
---applications:- name: <my-app>-staging  buildpack: https://github.com/cloudfoundry/nodejs-buildpack  command: npm run bluemix  disk_quota: 256MB  memory: 128MB  services:     - keys
```

2.继续使用新的清单文件:`cf push -f manifest-staging.yml`将这个临时应用程序直接部署到 Bluemix

3.然后我们将编辑`.travis.yml`中的部署脚本。当我们向 master 提交以更新新的 staging 应用程序时，我们需要更改原始的部署脚本。我们还需要为原始生产应用程序添加一个新的部署脚本:

```
deploy:  - edge: true    provider: script    script: if [ "$TRAVIS_PULL_REQUEST" = "false" ]; then cf zero-downtime-push <my-app>-staging -f ./manifest-staging.yml; else echo "PR skip deploy"; fi    skip_cleanup: true    on:      branch: master  - edge: true    provider: script    script: if [ "$TRAVIS_PULL_REQUEST" = "false" ]; then cf zero-downtime-push <my-app> -f ./manifest.yml; else echo "PR skip deploy"; fi    skip_cleanup: true    on:      tags: true
```

那么现在我们通过提交到 Github master 分支来更新分期 app，那么如何更新生产 app 呢？你将使用 [Github 的发布功能](https://help.github.com/articles/creating-releases/),就好像你的应用是一个真正的产品。？

4.继续将您的最新更改推送到 Github！然后导航到 Github repo 上的“Releases”并创建一个新版本。

#### ？？？？？

根据上一步，您应该在 Travis CI 的队列中看到两个构建。一个是由于最近的提交而进行的升级应用程序更新。另一个将是您的生产应用程序更新，因为新版本！
？？？？？

### DevOps 最后也是最重要的价值

我想通过强调 DevOps 最重要的部分来结束这篇文章:**自动化测试**。每当 Travis 运行时，包括在打开的 pull 请求中，它将在采取行动之前检查命令`yarn test`是否通过。Create React App 已经有`yarn test`配置了 [Jest](https://facebook.github.io/jest/) 。但是，您可以添加可访问性、林挺和您熟悉的任何其他测试框架！

概括一下我们所做的:首先，由于创建了 React 应用程序，我们很快配置好了 React 项目。然后我们构建了一个简单的服务器。我们把这个应用推向了世界。接下来，我们让 Travis 为我们的任何更改部署应用程序(零宕机)。然后我们使用 Github API，同时不让公众看到我们的密钥。最后，我们还设置了一个应用程序，以便我们可以测试预发布。

我希望这个项目有助于使你的工作流程更容易，因为你使史诗网络应用程序！一个大的？献给 B [abel、](https://github.com/babel/babel/graphs/contributors) C [reate React App、](https://github.com/facebookincubator/create-react-app/graphs/contributors) E [xpress、](https://github.com/expressjs/express/graphs/contributors) N [ode、](https://github.com/nodejs/node/graphs/contributors)以及其他所有使用的软件包的贡献者。此外，所有的❤️️去 Bluemix，Github 和 Travis CI 的免费层。

如果这对你有所帮助，请在评论中分享，或者[发微博给我](https://twitter.com/seejamescode)！我也希望听到不同的工作流程！

你也可以通过留言、[给我发邮件](mailto:james@seejamescode.com)或者发微博给 [@seejamescode](https://twitter.com/seejamescode) 来联系我。我在 ATX 的 IBM Design 工作，总是喜欢和网页设计社区交流。

你可能也喜欢…

[**如何将你的渐进式网络应用的谷歌灯塔得分提高到 100 分**](https://medium.freecodecamp.com/how-to-crank-your-progressive-web-apps-google-lighthouse-score-up-to-100-cfc053eb7661)
[*如果 Chrome 开发团队想向开发者传达一个信息，那就是:性能至关重要。*medium.freecodecamp.com](https://medium.freecodecamp.com/how-to-crank-your-progressive-web-apps-google-lighthouse-score-up-to-100-cfc053eb7661)

#### 查看[在 Github 上创建 React 应用 devo PS](https://github.com/seejamescode/create-react-app-devops)