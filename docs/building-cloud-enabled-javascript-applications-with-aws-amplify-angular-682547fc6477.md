# 如何使用 AWS Amplify 和 Angular 构建支持云的 JavaScript 应用程序

> 原文：<https://www.freecodecamp.org/news/building-cloud-enabled-javascript-applications-with-aws-amplify-angular-682547fc6477/>

作者:纳德·达比特

# 如何使用 AWS Amplify 和 Angular 构建支持云的 JavaScript 应用程序

![LFxCKjKFsNYnMTtXIt2YLtRIglclpDLQMFXa](img/fd6bac2fabbfdf76d057154113c95afb.png)

[AWS Amplify](https://aws.github.io/aws-amplify/?utm_source=blog&utm_campaign=amplify_angular_medium_nader) 帮助您将存储、GraphQL、认证、分析、发布订阅和国际化等功能添加到您的 JavaScript 应用程序中。

虽然您可以将 AWS Amplify 集成到任何 JavaScript 框架中，但是最近添加了 Angular 组件，使得在 Angular 应用程序中启动和运行云服务比以前更容易。

在本帖中，我们将了解如何在角度应用中使用 AWS Amplify。

### 入门指南

#### 安装依赖项

首先，我们需要安装几个依赖项:AWS Amplify 和 AWS Amplify Angular:

```
$ npm install --save aws-amplify
$ npm install --save aws-amplify-angular
```

#### 创建新的 AWS 移动项目

如果您已经有一个想要使用的 AWS 项目，可以跳过这一步。如果没有，您将了解我们如何使用 AWS Mobile Hub 快速启动和运行 AWS 服务，如用于身份验证的 Amazon Cognito、用于分析的 Amazon Pinpoint、用于托管 GraphQL 的 AWS AppSync 和用于存储的 Amazon S3。

接下来，我们需要安装 AWS Mobile CLI:

```
npm i -g awsmobile-cli
```

接下来，我们需要配置 AWS Mobile CLI，以使用您首选的 IAM 凭据。

> 如果你是 AWS 新手，并且想第一次了解如何设置，请查看此视频。

```
awsmobile configure
```

现在，我们的移动 CLI 已经准备就绪，我们可以创建一个新项目。

让我们创建一个启用了分析、存储和身份验证的新项目:

```
awsmobile init

awsmobile user-signin enable
awsmobile user-files enable
awsmobile push
```

创建后端后，配置文件被复制到`/awsmobilejs/#current-backe`

#### 在 AWS 控制台中查看项目

现在您已经从 CLI 创建了您的项目，您可以通过访问[https://console.aws.amazon.com/mobilehub/home](https://console.aws.amazon.com/mobilehub/home?region=us-east-1)并点击您的项目名称，在 AWS Mobile hub 中查看您的项目。

### 以角度工作

要将配置文件导入您的 Angular 应用程序，您需要将`aws_exports.js`重命名为`aws_exports.ts`。

要将配置文件导入您的 Angular 应用程序，您需要将`aws_exports.js`重命名为`aws_exports.ts`。

```
import Amplify from 'aws-amplify'
import awsmobile from './aws-exports'
Amplify.configure(aws_exports)
```

当使用底层`aws-js-sdk`时，“节点”包应该包含在*类型*编译器选项中。确保编辑源文件文件夹中的`tsconfig.app.json`文件，例如`*src/tsconfig.app.json*`。

```
"compilerOptions": {
  "types" : ["node"]
}
```

#### 进口放大器

在您的[根模块](https://angular.io/guide/bootstrapping) `src/app/app.module.ts`中，您可以导入 AWS 放大器模块如下:

```
import { AmplifyAngularModule, AmplifyService } from 'aws-amplify-angular'

@NgModule({
  ...
  imports: [
    ...
    AmplifyAngularModule
  ],
  ...
  providers: [
    ...
    AmplifyService
  ]
  ...
})
```

> 服务提供商是可选的。您可以正常导入核心类别，即`import { Analytics } from 'aws-amplify'`或创建自己的提供商。服务提供者为您做一些工作，并将类别公开为方法。该提供程序还使用您可以在组件中订阅的观察值来管理和调度身份验证状态更改(见下文)。

#### 使用 AWS Amplify 服务

AmplifyService 是你的 Angular app 中的一个提供者，它通过依赖注入的方式提供 AWS Amplify 核心类别。

要将 *AmplifyService* 与[依赖注入](https://angular.io/guide/dependency-injection-in-action)一起使用，请将其注入到应用程序中任何位置的任何组件或服务的构造函数中。

```
import { AmplifyService } from 'aws-amplify-angular';

...
constructor(
    public navCtrl:NavController,
    public amplifyService: AmplifyService,
    public modalCtrl: ModalController
) {
    this.amplifyService = amplifyService;
}
...
```

#### 使用 AWS 放大类别

您可以通过内置服务提供商直接访问和使用 AWS Amplify 类别:

```
import { Component } from '@angular/core';
import { AmplifyService }  from 'aws-amplify-angular';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent {

  constructor( public amplify:AmplifyService ) {

      this.amplifyService = amplify;

      /** now you can access category APIs:
       * this.amplifyService.auth();          // AWS Amplify Auth
       * this.amplifyService.analytics();     // AWS Amplify Analytics
       * this.amplifyService.storage();       // AWS Amplify Storage
       * this.amplifyService.api();           // AWS Amplify API
       * this.amplifyService.cache();         // AWS Amplify Cache
       * this.amplifyService.pubsub();        // AWS Amplify PubSub
     **/
  }

}
```

#### 用法示例:订阅身份验证状态更改

将 AmplifyService 导入到您的组件中，并监听身份验证状态的变化:

```
import { AmplifyService }  from 'aws-amplify-angular';

  // ...
constructor( public amplifyService: AmplifyService ) {

    this.amplifyService = amplifyService;

    this.amplifyService.authStateChange$
        .subscribe(authState => {
        this.signedIn = authState.state === 'signedIn';
        if (!authState.user) {
            this.user = null;
        } else {
            this.user = authState.user;
            this.greeting = "Hello " + this.user.username;
        }
        });

}
```

#### 查看组件

AWS Amplify 还提供了可以在角度视图模板中使用的组件，包括验证器组件、照片拾取器组件和亚马逊 S3 相册组件。

**认证者**

Authenticator 组件为您的 Angular 应用程序创建开箱即用的签名/注册体验。要使用 Authenticator，只需在您的。html 视图:

```
<amplify-authenticator></amplify-authenticator>
```

**照片拾取器**

照片选择器组件将呈现一个文件上传控件，将允许选择一个本地图像，并将其上传到亚马逊 S3。选择图像后，将自动显示 base64 编码的图像预览。

要在角度视图中渲染照片拾取器，请使用 *`amplify-photo-picker`* 组件:

```
<amplify-photo-picker 
    (picked)="onImagePicked($event)"
    (loaded)="onImageLoaded($event)">
</amplify-photo-picker>
```

该组件将发出两个事件:

*   `(picked)` -选择图像时发出。该事件将包含可用于上传的`File`对象。
*   `(loaded)` -当图像预览已经渲染和显示时发出。

**上传图像**

使用 AWS Amplify 存储类别，使用`onImagePicked(event)`将您的照片上传到亚马逊 S3:

```
onImagePicked( file ) {

    let key = `pics/${file.name}`;

    this.amplify.storage().put( key, file, {
      'level': 'private',
      'contentType': file.type
    })
    .then (result => console.log('uploaded: ', result))
    .catch(err => console.log('upload error: ', err));

}
```

#### S3 专辑

亚马逊 S3 相册组件显示来自连接的 S3 桶的图像列表。

要渲染相册，请使用角度视图中的`*amplify-s3-album*`组件:

```
<amplify-s3-album
  path="pics" (selected)="onAlbumImageSelected($event)"
>
</amplify-s3-album>
```

`(selected)`事件可用于检索列表中被点击图像的 URL:

```
onAlbumImageSelected( event ) {
      window.open( event, '_blank' );
}
```

**自定义样式**

您可以对 AWS 放大器组件使用自定义样式。只需导入你的自定义 *styles.css* ，它会覆盖`/node_modules/aws-amplify-angular/theme.css`中的默认样式。

### 结论

AWS Amplify 是开源的，正在积极开发中，我们希望客户或用户提供任何反馈和/或问题，以帮助我们制定未来的路线图。

随意查看这里的文档，或者这里的 GitHub 回购。

我的名字是纳德·达比特。我是 [AWS Mobile](https://aws.amazon.com/mobile/) 的一名开发人员，负责像 [AWS AppSync](https://aws.amazon.com/appsync/) 和 [AWS Amplify](https://aws.github.io/aws-amplify/?utm_source=blog&utm_campaign=amplify_angular_medium_nader) 这样的项目，也是 [React Native Training](http://reactnative.training/) 的创始人。