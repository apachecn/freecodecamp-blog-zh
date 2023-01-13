# Firebase 简化了角度认证

> 原文：<https://www.freecodecamp.org/news/angular-2-authentication-made-easy-with-firebase-246c282d9ef8/>

瓦西姆·切舍姆

# Firebase 简化了角度认证

![1*oOBXowI2IO9eCTcxP1BT-Q](img/f823c5c6da93842c5cb9b48162d230da.png)

> 更新:本文中的代码已经更新到 Angular Final。我们还假设您使用的是最新的 Angular CLI。

任何严肃的 Web 应用程序都需要某种认证特性。在这篇博文中，我们将使用 Firebase 为 Angular 应用程序设置这个特性，这要感谢官方的 [AngularFire2](https://github.com/angular/angularfire2) 库。

### 创建一个新的 Firebase 项目

在我们的应用程序中使用 Firebase 之前，我们需要在我们的 [Firebase 开发控制台](https://console.firebase.google.com/)中创建一个新项目:

![1*ZzoMPcmlDgYRLCQgAFiU8Q](img/400f256530d440f91e5c19fca0bdfc15.png)

Firebase Developer Console

要使用 Firebase 身份验证特性，我们需要启用我们希望在项目中使用的**登录提供者**。在我们的例子中，我们将使用谷歌、脸书、Twitter 和 Github 来登录我们的用户。

您可以在以下位置找到身份验证页面:您的应用→身份验证→登录方法:

![1*wfIpVAzDOEVFUJR2UjUp5g](img/b006f5f5d9f67561af0532cfb711c64a.png)

一些提供商，如脸书、Twitter 和 Github，要求你提供一个 **App/Client/API ID** 和 **App/Client/API secret** 密钥，并使用给定的 **OAuth URI** 作为重定向 URI:

![1*cmB2uiqwESrHHwKo1RVkgA](img/72fa608e51ecb12c292796f8c8e48cab.png)

为了获得这些信息，你需要使用你的开发者账户为每个服务提供商创建一个应用程序(Github，脸书和 Twitter)。

### 配置您的服务

![1*M372rrSgYuiwgODlftxKDQ](img/a6c5edceb2927ff2eefa251aac732ebf.png)

Github Application Settings

对于 Github，去你的[开发者设置](https://github.com/settings/developers)，和[注册](https://github.com/settings/applications/new)一个新的应用。

在 Firebase 配置页面中使用您的应用程序**客户端 ID** 和**客户端秘密**。

您还需要使用 Firebase 的 **OAuth URI** 填写**授权回调 URL** 。

![1*UsjnM5koHUhanSlGXYR6Yg](img/97cc1f22f57fcac99df4881020fd567c.png)

Twitter Application Settings

对于 Twitter，进入你的[应用管理设置](https://apps.twitter.com/)，然后[创建一个新的](https://apps.twitter.com/app/new)应用。

在您的 Firebase 配置页面中使用您的应用程序 **API 密钥**和 **API 秘密**。

您还需要使用 Firebase 的 **OAuth URI** 填写**回调 URL** (在设置选项卡中)。

![1*blTSqtexKVeShcVPDMHKaw](img/474129d4fc583a7c0cf197b83595a3dc.png)

Facebook Developer Console

最后，对于脸书，进入你的[开发者应用](https://developers.facebook.com/apps/)主页，点击右上角的“添加新应用”绿色按钮。

在你的 Firebase 配置页面中使用你的**应用 ID** 和**应用密码**。

您还需要添加一个新的 Web 平台。点击页面底部的**+添加平台**——并在网站 URL 中填入来自 Firebase 的重定向 URI(即 **OAuth URI** )。

现在，您已经准备好使用 Google、Twitter、Github 和脸书作为您的 web 应用程序的身份验证提供者。

### 在应用中使用 AngularFire

下一步，我将使用我搭建的 Angular 应用程序，这要感谢官方 Angular CLI。

在这个应用程序中，我将使用我们在上一步中刚刚设置的身份验证提供者。

该应用程序如下所示:

![1*wmQDx_-Q87Gr-nu9iFRXGg](img/b1d61f2d20ecb4ac41e887cf8a351c36.png)

点击一个按钮将调用一个特定的提供商，验证用户，并获得他们的信息:

![1*zgPhCyiM1Z93aWXD-u3__g](img/59a50421f247480bd70414ddb7dc5381.png)

User logged in using github.com

#### 安装和设置 angularfire2

首先，我们需要安装 *firebase* 和 *angularfire2* 库:

```
$ npm install angularfire2 firebase --save
```

然后我们需要安装 *Firebase* 类型:

```
$ npm install @types/firebase
```

并将这些类型添加到您的 **src/tsconfig.json** 文件中:

```
{  "compilerOptions": {    "declaration": false,    "emitDecoratorMetadata": true,    "experimentalDecorators": true,    "lib": ["es6", "dom"],    "mapRoot": "./",    "module": "es6",    "moduleResolution": "node",    "outDir": "../dist/out-tsc",    "sourceMap": true,    "target": "es5",    "typeRoots": [      "../node_modules/@types"    ],    "types": [      "jasmine",      "firebase",      "node"    ]  }}
```

官方文件中描述了更详细的设置过程。

#### 为您的应用配置 *AngularFire*

为了将 Firebase 与 Angular 一起使用，我们需要为 AngularFire 服务提供一些配置。

让我们从默认的 Firebase 配置开始，如下所示:

```
defaultFirebase({  apiKey: "AIzaSyCk3weREVFpOIN6pL_QVVNFRl3C3keMIRU",  authDomain: "angular2-auth.firebaseapp.com",  databaseURL: "https://angular2-auth.firebaseio.com",  storageBucket: "angular2-auth.appspot.com"})
```

*注:不用担心这个暴露的 **apiKey** 。这个键仅用于标识项目的不同服务:一种项目标识符。*

您可以从您的应用程序仪表板中获得您的 Firebase **apiKey** 、**项目 Id、数据库名称、**和**存储桶 ID** 。

点击右上角的**网络设置**按钮:

![1*qzOQlZJtR6U9luzBDaVwrw](img/6cb9491f0a511b2640f1bf52cc973a0d.png)

接下来，对于我们的用例，我们想要提供一个默认的身份验证方法:

```
firebaseAuthConfig({  method: AuthMethods.Popup})
```

这里，我们选择使用一个**弹出窗口**来允许用户登录。我们也可以使用这样的重定向方法:

```
firebaseAuthConfig({  method: AuthMethods.Redirect})
```

现在让我们在 **src/app/core.module.ts** 中创建一个 *CoreModule* 来放置我们的配置。最佳实践是将核心依赖分组到一个名为[核心模块](https://angular.io/docs/ts/latest/guide/ngmodule.html#!#core-module)的独立 NgModule 中。以下是此类模块的一个示例:

```
import { BrowserModule } from '@angular/platform-browser';import { HttpModule } from '@angular/http';import { NgModule } from '@angular/core';import { FormsModule } from '@angular/forms';import {   AngularFireModule,   AuthMethods,   AuthProviders } from "angularfire2";
```

```
const firebaseConfig = {  apiKey: "AIzaSyCk3weREVFpOIN6pL_QVVNFRl3C3keMIRU",  authDomain: "angular2-auth.firebaseapp.com",  databaseURL: "https://angular2-auth.firebaseio.com",  storageBucket: "angular2-auth.appspot.com"};
```

```
@NgModule({  imports: [    BrowserModule,    FormsModule,    HttpModule,    AngularFireModule.initializeApp(firebaseConfig,{      provider: AuthProviders.Google,      method: AuthMethods.Popup    })  ],  exports: [    BrowserModule,  ]})export class CoreModule {}
```

#### 在您的组件中使用它

现在我们终于准备好为我们的 Angular 应用程序实现身份验证特性了。

我们需要在组件的构造函数中注入 **AngularFire** 服务:

```
import { Component } from '@angular/core';import { AngularFire, AuthProviders } from 'angularfire2';
```

```
@Component({  moduleId: module.id,  selector: 'app-root',  templateUrl: 'app.component.html',  styleUrls: ['app.component.css']})export class AppComponent {  user = {};
```

```
 constructor(    public af: AngularFire  ) {    this.af.auth.subscribe(user => {      if(user) {        // user logged in        this.user = user;      }      else {        // user not logged in        this.user = {};      }    });  }}
```

> 在真实的应用程序中，您需要创建一个单独的服务来处理身份验证。想 [SRP](https://en.wikipedia.org/wiki/Single_responsibility_principle) 和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)。但是对于本教程，我们将保持简单。

**AngularFire** 服务为我们提供了实现可观察 API 的 **auth** 属性。这意味着我们需要订阅这个 **auth** 属性来获取身份验证状态。 ***空值*** 表示用户未登录。

让我们在组件视图中添加几个按钮:

```
<button md-raised-button (click)="login()">  <img width="30" src="google-logo.png" />Use My Google Account</button><button md-raised-button (click)="logout()">  Logout</button>
```

我们将使用这些按钮登录和注销，以 Google provider 为例。

在***app . component . ts*T3 中，我们需要实现 ***login()*** *和 **logout()*** 方法:**

```
login() {  this.af.auth.login({    provider: AuthProviders.Google  });} logout() {  this.af.auth.logout();}
```

为了让用户登录，我们使用 Google provider 调用 **this.af.auth.login()** 方法，并调用 **this.af.auth.logout()** 让用户注销。

**AngulareFire login()** 通常需要一个认证方法。在我们的例子中，它将使用默认方法(即 **AuthMethods)。【T3 弹出)】我们在自举阶段设置。**

如果在调用 **login()** 方法时需要覆盖认证方法，只需提供一个新的认证配置:

```
login() {  this.af.auth.login({    provider: AuthProviders.Google,    method: AuthMethods.Redirect  });}
```

一旦一切都设置好并正常工作，我们就可以检查包含 Google(身份验证提供者)发送的所有用户会话令牌的 ***用户*** 对象:

![1*mQcUhIAo_anodDNp_Z0tnQ](img/d99d305950359107124cddd0621ff40b.png)

您可以从 **auth.provideData** 条目中获取用户的信息，如 **displayName** 和 **photoURL** 。

就是这样！您的用户现在可以使用他们的 Google 帐户登录。

你可以用同样的方法添加 Github 和脸书。

在 Firebase 上查看完整的工作应用[或者在 Github](https://angular2-auth.firebaseapp.com) 上阅读完整的源代码[。](https://github.com/manekinekko/angular-firebase-authentication)

> 注意:我正在将这个回购迁移到 Angular final ( [见分支](https://github.com/manekinekko/angular-firebase-authentication/tree/migration-to-angular-2.0.0))。由于 AngularFire2 中的[这一问题](https://github.com/manekinekko/angular-firebase-authentication/issues/3)，此流程处于待定状态。

[**manekinekko/angular-Firebase-authentic ation**](https://github.com/manekinekko/angular-firebase-authentication)
[*angular-Firebase-authentic ation-An angular demo Firebase 认证提供者*github.com](https://github.com/manekinekko/angular-firebase-authentication)

### 提示和参考

*关注 [@manekinekko](https://twitter.com/manekinekko) 了解更多关于网络平台的信息。*