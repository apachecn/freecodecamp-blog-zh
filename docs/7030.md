# Angular 6 及其新特性——三分钟解释

> 原文：<https://www.freecodecamp.org/news/angular-what-is-the-new-briefly-e6837348dd3a/>

[Angular](https://angular.io) 在[6 . 0 . 0](https://angular.io/)版本中推出了一些令人惊叹的新功能，尤其是 Angular-cli。现在，有了 Angular 6，您可以轻松地更新您的旧包，使用 Angular 元素创建原生 web 元素，以及许多其他事情。我们来看看吧！

### ng 添加

![1*u8BWLIWdkabEzp0QSmMUgg](img/4979cdb099579aebe03a3afef18c60ab.png)

`**ng add**`是 Angular-cli 中的一个新命令，可帮助您在 Angular 应用程序中安装和下载新的软件包。它的工作原理与 npm 相同，但并不取代它。

### ng 更新

![1*mxQPMNmblN_8t_ky1r5G8w](img/a62c47e30a10ec70209aec690068d285.png)

也是一个新的 Angular-cli 命令。它用于更新和升级您的软件包。这真的很有帮助，例如，当你想从 Angular 5 升级到 Angular 6，或者你的 Angular 应用程序中的任何其他包。

### 在服务本身内部声明提供者

在这次更新之前，您必须在`**app.module.ts**`中声明 providers 数组

现在有了 Angular 6，您可以通过将`**providedIn:root**` 属性放在“`**@injectable"**` **装饰器中，在管理器内部提供服务。**

![1*3Huej4et-8LIfrAEhzY3pQ](img/2c25385e68c0e3c8d6569e2a1c4a5850.png)

### 使用 ng-template 代替 template 指令

你可以使用 `**ng-template**`来呈现 HTML，而不是新版本 Angular 中的`**template**`标签。`**ng-template**`是一个角元素，当它与[结构指令](https://angular.io/guide/structural-directives)一起使用时起作用，如`***ngFor**`和`***ngIf**`

![1*6RjvjuP6weX0bPrYBbjQ8Q](img/e472a584cf9852c8bdfd656ecc9ab9a5.png)

### 角度元素

角 6 向我们介绍了角元素。您可以将 Angular 元素呈现为原生 web 元素，它们被解释为可信的 HTML 元素。

您可以通过运行以下命令添加角度元素:

![1*u8BWLIWdkabEzp0QSmMUgg](img/4979cdb099579aebe03a3afef18c60ab.png)

在组件中导入`**createCustomElement**`。

![1*YP2ej1AXVAO9GURmbGnFcQ](img/ebc09560d1b52e789a315b93a9c68e81.png)

然后创建您的定制元素！

![1*F1WAYYCRzJSyfr8PSWsMRg](img/c8cb5a3273a3dcd8e978eb3934eb13de.png)

`**MyElemComponent.ts**`

![1*S4Ib01DNgO67jh_-habKmQ](img/f93d850bad1d908b9e2ca0242dac5413.png)

结果是:

![1*lgl-OcKiFKVLF7A9KdrImA](img/57563f5b730620d9b64d74cd5950ea37.png)

**注意:**您必须实现`@angular/platform-browser`中的`**DomSanitizer**`方法，使您的定制元素成为可信的 HTML 标签。

你可以在这里了解更多关于角元素

### 升级到 RxJS 6.0.0

Angular 6 使用最新版本的 Rxjs 库。现在，您可以在 Angular 应用程序中享受 RxJS 6 的最新功能:)

### 包扎

Angular 本身在 Angular 内核上并没有太多突破性的改变，但是 Angular-cli 真的很让人激动。Angular 团队更加关注性能，轻松地构建 PWAs，提供一个良好的工作环境，以便以一种简单的方式与 Angular 一起工作。

你可以在推特上找到我。

> 顺便说一下，我最近和一个强大的软件工程师团队合作开发了一个移动应用程序。这个组织非常棒，产品交付得非常快，比我合作过的其他公司和自由职业者快得多，我想我可以诚实地向他们推荐其他项目。如果你想联系我，请发邮件给我—[said@devsdata.com](mailto:said@devsdata.com)。