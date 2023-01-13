# 如何使用 Travis CI 在 GitHub 页面上托管 Angular 应用程序

> 原文：<https://www.freecodecamp.org/news/host-an-angular-application-on-github-pages-with-travis-ci/>

在本文中，我们将使用最新版本的 Angular 创建一个应用程序。然后我们将它托管在 GitHub Pages 静态网站服务上，使用持续集成工具 Travis CI 来部署应用程序。

## 先决条件

在开始之前，您需要安装和配置下面的工具来创建 Angular 应用程序。

*   Git 是一个分布式版本控制系统，我们将使用它来同步存储库。
*   [Node.js 和 npm](https://nodejs.org/) : Node.js 是基于 Google 的 V8 引擎的 JavaScript 代码运行时软件。npm 是 Node.js 的包管理器(节点包管理器)。我们将使用这些来构建和运行 Angular 应用程序并安装库。
*   Angular CLI : Angular CLI 是 Angular 的命令行实用工具，我们将使用它来创建 Angular 应用程序的基础结构。
*   IDE(像 [Visual Studio Code](https://code.visualstudio.com/) 或者[WebStorm](https://www.jetbrains.com/webstorm/)):IDE(集成开发环境)是一个带有图形界面的工具，可以帮助我们开发应用程序。这里，我们将使用一个来开发角度应用程序。

## 入门指南

### 在 GitHub 上创建和配置您的帐户

GitHub 是一个源代码和文件存储服务，使用 Git 工具进行版本控制。而 [GitHub Pages](https://pages.github.com/) 是一个使用公共存储库的静态文件托管服务。

首先，你需要在 GitHub 上创建一个账户，如果你还没有的话。访问[https://github.com/](https://github.com/)，点击*按钮报名*。

![github-step1](img/84d96339ebae639edb6efb2c1aa0171a.png)

填写用户名、电子邮件地址和密码字段，单击“验证”按钮解决问题，然后单击“创建帐户”按钮。

![github-step2](img/e7c8f8da0342d6421c7d5507edc767a9.png)

接下来，我们将生成将在 Travis CI 中使用的令牌。点击有头像的菜单，点击菜单设置。

![github-step3](img/73bc7e25243c47b61c2459e4ffbe6a29.png)

点击菜单开发者设置。

![github-step4](img/fa64be696a8ad9a2fec5f215bb356776.png)

单击菜单个人访问令牌。

![github-step5](img/b9d227206ad13ed80e5f344abb8efd08.png)

点击按钮生成新令牌。

![github-step6](img/60f76af933edd9c203e0977d58a7986e.png)

填写“注释”字段，选择“回购”选项，然后单击“创建令牌”按钮。

![github-step7](img/6cc1fde4445c9b64d415415fc9d5314e.png)

复制生成的令牌。在我的例子中，生成了令牌`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`,因为这个令牌将在 Travis CI 中配置。

![github-step8](img/171bf61a336ab64f4c37ae4b922f4a58.png)

让我们创建存储库。点击带有头像的菜单，然后点击你的仓库菜单。

![github-step9](img/3651e92fc6b993c167bf7d95e69c0306.png)

点击新建按钮。

![github-step10](img/20d8444964892bc764bac68a5396b0c9.png)

填写“存储库名称”字段，然后单击“创建存储库”按钮。

![github-step11](img/d75c04f4229e0731f0e8337c0308cf8b.png)

准备好了。帐户已创建，令牌已生成，存储库 [`https://github.com/rodrigokamada/angular-travisci`](https://github.com/rodrigokamada/angular-travisci) 已创建。

![github-step12](img/9eac706c0b2be1deb9dbce5ba4cf006a.png)

### 在 Travis CI 上创建和配置您的帐户

[Travis CI](https://www.travis-ci.com/) 是与 GitHub 集成的部署服务。

首先，如果您还没有 Travis CI 帐户，您需要创建一个。访问[https://travis-ci.com/](https://travis-ci.com/)并点击按钮注册。

![travisci-step1](img/15d8856905826b9c5f46166f07b4889f.png)

点击按钮使用 GitHub 登录，使用您的 GITHUB 帐户登录。

![travisci-step2](img/370534b4fa9f97803df4592731eaee10.png)

如果 Travis CI 请求允许列出 GitHub 存储库，接受这个请求。单击存储库链接 angular-travisci。

![travisci-step3](img/7c8455f7cdade1b3f559ad5a6d2e0dd2.png)

让我们设置 GitHub 访问令牌。点选选单更多选项，然后点选选单设定。

![travisci-step4](img/c443d899303d6b07c4967335928b6986.png)

用值 GITHUB_TOKEN 填充字段名，用您在 GITHUB 上生成的令牌的值填充 value，然后单击 Add 按钮。

![travisci-step5](img/88cbfb905f5a6808661c6c49f7a0227d.png)

准备好了。帐户已创建，存储库已配置。

![travisci-step6](img/1946e05245cc2a99056a11391d2068d6.png)

### 创建您的角度应用

[Angular](https://angular.io/) 是一个使用 HTML、CSS 和 TypeScript (JavaScript)构建 Web、移动和桌面应用的开发平台。目前，Angular 处于版本 13，Google 是该项目的主要维护者。

让我们使用带有路线文件和 SCSS 风格格式的`@angular/cli`来创建具有角度基础结构的应用程序。

```
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
CREATE angular-travisci/README.md (1061 bytes)
CREATE angular-travisci/.editorconfig (274 bytes)
CREATE angular-travisci/.gitignore (604 bytes)
CREATE angular-travisci/angular.json (3267 bytes)
CREATE angular-travisci/package.json (1078 bytes)
CREATE angular-travisci/tsconfig.json (783 bytes)
CREATE angular-travisci/.browserslistrc (703 bytes)
CREATE angular-travisci/karma.conf.js (1433 bytes)
CREATE angular-travisci/tsconfig.app.json (287 bytes)
CREATE angular-travisci/tsconfig.spec.json (333 bytes)
CREATE angular-travisci/src/favicon.ico (948 bytes)
CREATE angular-travisci/src/index.html (301 bytes)
CREATE angular-travisci/src/main.ts (372 bytes)
CREATE angular-travisci/src/polyfills.ts (2820 bytes)
CREATE angular-travisci/src/styles.scss (80 bytes)
CREATE angular-travisci/src/test.ts (743 bytes)
CREATE angular-travisci/src/assets/.gitkeep (0 bytes)
CREATE angular-travisci/src/environments/environment.prod.ts (51 bytes)
CREATE angular-travisci/src/environments/environment.ts (658 bytes)
CREATE angular-travisci/src/app/app-routing.module.ts (245 bytes)
CREATE angular-travisci/src/app/app.module.ts (393 bytes)
CREATE angular-travisci/src/app/app.component.scss (0 bytes)
CREATE angular-travisci/src/app/app.component.html (23809 bytes)
CREATE angular-travisci/src/app/app.component.spec.ts (1087 bytes)
CREATE angular-travisci/src/app/app.component.ts (221 bytes)
✔ Packages installed successfully.
    Successfully initialized git. 
```

创建`.travis.yml`文件。

```
touch .travis.yml 
```

用以下内容配置`.travis.yml`文件:

```
notifications:
  email:
    recipients:
      - rodrigo@kamada.com.br

language: node_js

node_js:
  - 16

before_script:
  - npm install

script:
  - npm run test:headless

before_deploy:
  - npm run build:prod

deploy:
  provider: pages
  skip_cleanup: true
  github_token: $GITHUB_TOKEN
  local_dir: dist/angular-travisci
  on:
    branch: main 
```

更改`package.json`文件并添加下面的脚本。用您的 GitHub 用户名替换`rodrigokamada`值。

```
 "build:prod": "ng build --prod --base-href https://rodrigokamada.github.io/angular-travisci/",
  "test:headless": "ng test --watch=false --browsers=ChromeHeadless" 
```

更改`src/app/app.component.spec.ts`文件并移除测试`should have as title 'angular-travisci'`和`should render title`。

使用以下命令运行测试:

```
npm run test:headless

> angular-travisci@1.0.0 test:headless
> ng test --watch=false --browsers=ChromeHeadless

⠋ Generating browser application bundles (phase: setup)...Compiling @angular/core/testing : es2015 as esm2015
Compiling @angular/compiler/testing : es2015 as esm2015
Compiling @angular/platform-browser/testing : es2015 as esm2015
Compiling @angular/common/testing : es2015 as esm2015
Compiling @angular/platform-browser-dynamic/testing : es2015 as esm2015
Compiling @angular/router/testing : es2015 as esm2015
⠸ Generating browser application bundles (phase: building)...05 09 2021 19:40:04.329:INFO [karma-server]: Karma v6.3.4 server started at http://localhost:9876/
05 09 2021 19:40:04.331:INFO [launcher]: Launching browsers ChromeHeadless with concurrency unlimited
05 09 2021 19:40:04.369:INFO [launcher]: Starting browser ChromeHeadless
✔ Browser application bundle generation complete.
05 09 2021 19:40:09.704:INFO [Chrome Headless 92.0.4515.159 (Linux x86_64)]: Connected on socket NUtJhjavb1JvssqOAAAB with id 25989029
Chrome Headless 92.0.4515.159 (Linux x86_64): Executed 1 of 1 SUCCESS (0.068 secs / 0.042 secs)
TOTAL: 1 SUCCESS 
```

使用下面的命令运行应用程序。访问 URL `http://localhost:4200/`并检查应用程序是否工作。

```
npm start

> angular-travisci@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files | Names         |      Size
vendor.js           | vendor        |   2.39 MB
polyfills.js        | polyfills     | 128.51 kB
main.js             | main          |   8.89 kB
runtime.js          | runtime       |   6.63 kB
styles.css          | styles        |   1.18 kB

                    | Initial Total |   2.53 MB

Build at: 2021-09-05T22:35:38.010Z - Hash: a4cfc9149589386eca5b - Time: 39997ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **

✔ Compiled successfully. 
```

使用下面的命令构建应用程序:

```
npm run build:prod

> angular-travisci@1.0.0 build:prod
> ng build --configuration production --base-href https://rodrigokamada.github.io/angular-travisci/

✔ Browser application bundle generation complete.
✔ Copying assets complete.
✔ Index html generation complete.

Initial Chunk Files           | Names         |      Size
main.c678fa8750e7c769.js      | main          | 177.63 kB
polyfills.6d7801353e02e327.js | polyfills     |  36.21 kB
runtime.b136bda8a38c4f2e.js   | runtime       |   1.06 kB
styles.ef46db3751d8e999.css   | styles        |   0 bytes

                              | Initial Total | 214.90 kB

Build at: 2021-09-05T22:42:19.525Z - Hash: 83bfffc079b083727ca4 - Time: 26030ms 
```

同步您创建的 GitHub 存储库上的应用程序。

准备好了。在 GitHub 存储库上同步应用程序之后，Travis CI 构建应用程序并在分支`gh-pages`上同步。

访问网址 [`https://rodrigokamada.github.io/angular-travisci/`](https://rodrigokamada.github.io/angular-travisci/) ，检查应用程序是否工作。用您的 GitHub 用户名替换`rodrigokamada`值。

就是这样！应用程序存储库可在[https://github.com/rodrigokamada/angular-travisci](https://github.com/rodrigokamada/angular-travisci)获得。

## 结论

总结本文所涵盖的内容:

*   我们在 GitHub 上创建了一个账户。
*   我们在 GitHub 上创建了一个访问令牌。
*   我们在 GitHub 上创建了一个知识库。
*   我们在特拉维斯. CI 上建了一个账户。
*   我们在 Travis CI 上配置了 GitHub 访问令牌。
*   我们创建一个角度应用程序。

你可以用这篇文章来创建你的个人网站，并拥有一个在线作品集。

感谢您的阅读，我希望你喜欢这篇文章！

为了在我发布新文章时保持更新，请在 Twitter 上关注我。

这篇教程用葡萄牙语发布在我的博客上。