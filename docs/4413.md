# 如何在 Elm 中使用 Netlify 函数

> 原文：<https://www.freecodecamp.org/news/how-to-use-netlify-functions-in-elm/>

这个工作示例创建了一个简单的 [Netlify 函数](https://functions.netlify.com/)，并将其与一个 Elm 应用程序集成在一起。

Netlify 函数提供了一种非常方便的使用 AWS Lambdas 的方式，它们有一系列令人印象深刻的用例，比如发送电子邮件、处理支付和登录。

这个例子从环境变量中读取秘密(以避免它们暴露在浏览器中)，但是它主要是通用的，并且可以很容易地适用于其他用例。

## 步骤 1 -先决条件

*   为代码创建一个存储库，可能在 GitHub 上
*   `npm i -g elm`
*   `npm install -g netlify-cli`
*   `npm i -g create-elm-app`

## 步骤 2 -创建香草榆树应用程序

*   `create-elm-app elm-app-with-netlify-function`
*   `cd elm-app-with-netlify-function`
*   `elm-app start`

这将启动一个开发服务器，并在浏览器中加载应用程序。

您可以查看配套存储库中的[提交，以检查一切是否正常。](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/d976b2391f98f07113d1e41a64b0359caddf3452)

## 步骤 2 -在网络上部署

*   `npm init`(并填写合理值)
*   `npm i create-elm-app --save-dev`(这将 create-elm-app 添加到 netlify 使用的 package.json 中)
*   把代码推给 GitHub

您可以在同伴库中的[处看到这个提交的结果](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/aa52ccfabacae69591a920f0675eedf620ae8b03)

*   登录/注册/向 [Netlify](https://www.netlify.com/) 注册
*   在 Netlify 上创建一个新站点
*   选择您的存储库
*   将“构建命令”设置为`elm-app build`
*   将“公共目录”设置为`build`
*   点击“部署站点”

Netlify 现在将部署站点，安装 package.json 中指定的依赖项，然后运行`elm-app build`，然后服务于 dist 目录。

从现在开始，每次你推送 GitHub，Netlify 都会尝试部署最新的代码。

## 步骤 3 -链接网络效率开发

*   `netlify login`
*   `netlify link`并选择“使用当前 git 远程 url”选项
*   添加”。/netlify "到。gitignore
*   添加一个 netlify.toml 文件(从伴随库中的[这个提交)](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/6514012000ea82fb6625fa3686adafa321723d28)
*   `netlify dev`

这将启动一个本地开发服务器，并在您的浏览器中加载应用程序，与步骤 1 类似。

## 步骤 4 -添加网络功能

运行`netlify functions:create`创建一个新的网络功能。选择“js-token-hider”模板，命名为“call-api”。

这将为该函数创建一个 javascript 文件，并在“functions/call-api”中为其依赖项创建一个 package.json。

将 functions/call-api/call-api.js 替换为[配套库](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/79381b9c1a7731b01f0c81b58a772d9576f76732)中的这个

现在，如果你运行`netlify dev`，这个功能将和应用程序一样，尽管是在不同的端口上。您可以在浏览器中查看该函数以检查它是否在工作(可能在[http://localhost:34567/call-API](http://localhost:34567/call-api)或 [http://localhost:34567/。网络生命/函数/调用 api](http://localhost:34567/.netlify/functions/call-api)

## 步骤 5 -从 Elm 调用 netlify 函数

安装依赖项

*   `elm install elm/json`
*   `elm install elm/http`
*   `elm install krisajenkins/remotedata`

更新 Main.elm 以调用该函数并显示结果(从[伴随库](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/4dc9e8e4b60d061b5d5ef0fb2ce6ab856741236f))。

指示 create-elm-app 通过添加 elmapp.config.js 来代理对该函数的 api 调用，如[中的配套库](https://github.com/ceddlyburge/netlify-functions-with-elm/commit/90a63178e38f2919770e37fcc94e7ee0bec343ab)所示。

此时，应用程序运行，并成功调用 api，但是还没有秘密/环境变量，所以 UI 显示一个错误。

## 第 6 步-添加秘密

转到 Netlify 网站上针对您的应用程序的“站点设置”-“构建和部署”-“持续部署”-“环境变量”部分。

为 API_TOKEN 和 API_URL 添加环境变量

现在，当您运行“netlify dev”时，应用程序应该会加载到浏览器中，并调用本地托管的 netlify 函数，该函数将返回您在 netlify 上设置的 API_TOKEN 和 API_URL 环境变量。

Netlify 上的实时部署也应该如此。您可能需要在 Netlify 上手动“触发部署”,以便它使用新的环境变量。

您可以在[https://netlify-functions-with-elm.netlify.com](https://netlify-functions-with-elm.netlify.com)看到配套存储库的部署

## 结论

网络/无服务器功能对于创建/连接前端所需的后端服务非常有用。他们也非常东成立，因为这 artcile(希望！)显示。

Create-elm-app 是开发 elm 应用程序的一个很好的工具，在开发 Netlify 函数时，它的简单代理特性工作得很好。

Netlify Dev 非常适合在本地开发时复制生产 Netlify 设置(在这种情况下，通过自动提供环境变量)。