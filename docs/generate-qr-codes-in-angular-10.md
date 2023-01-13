# 如何在 Angular 10 中生成二维码

> 原文：<https://www.freecodecamp.org/news/generate-qr-codes-in-angular-10/>

在本教程中，我们将通过构建一个简单的示例应用程序来学习如何在 Angular 10 中生成 QR 码。

但首先，什么是二维码，它有什么作用？

据[维基百科](https://en.wikipedia.org/wiki/QR_code):

> QR 码(快速响应码的缩写)是一种矩阵条形码(或二维条形码)，于 1994 年首次为日本的汽车行业设计。
> 
> 条形码是一种机器可读的光学标签，包含其所贴物品的相关信息。
> 
> 实际上，二维码通常包含指向网站或应用程序的定位器、标识符或跟踪器的数据。
> 
> 二维码使用四种标准化编码模式(数字、字母数字、字节/二进制和汉字)高效存储数据。

因此，这是一种简洁高效的数据存储方式。

现在，让我们通过创建一个示例来看看如何在 Angular 10 应用程序中生成二维码。

## 先决条件

在开始之前，您需要一些先决条件:

*   打字稿的基础知识。特别是熟悉面向对象的概念，如类型脚本类和装饰器。
*   安装了**节点 10+** 和 **NPM 6+** 的本地开发机。

像当今大多数前端工具一样，Angular CLI 需要节点。你可以直接进入官方网站的[下载页面，为你的操作系统下载二进制文件。](https://nodejs.org/downloads)

您还可以参考特定的系统说明，了解如何使用软件包管理器安装节点。不过推荐的方法是使用[NVM](https://github.com/nvm-sh/nvm)——节点版本管理器——一个 POSIX 兼容的 bash 脚本来管理多个活动的 Node.js 版本。

**注**:不想安装 Angular 开发的本地环境但仍想尝试本教程中的代码？您可以使用 [Stackblitz](https://stackblitz.com/) ，这是一个用于前端开发的在线 IDE，允许您创建与 Angular CLI 兼容的 Angular 项目。

## 步骤 1 —安装 Angular CLI 10

在这一步，我们将[安装最新的 Angular CLI 10](https://www.ahmedbouchefra.com/install-angular-cli/) (在撰写本教程时)。

Angular CLI 是用于初始化和使用 Angular 项目的官方工具。要安装它，请打开一个新的命令行界面并运行以下命令:

```
$ npm install -g @angular/cli 
```

在撰写本文时， **angular/cli v10** 将安装在您的系统上。

## **步骤 2——创建一个新的 Angular 10 应用程序**

现在让我们创建我们的项目。回到您的命令行界面，运行以下命令:

```
$ cd ~
$ ng new angular10qrcode
```

CLI 将询问您几个问题:

*   **您想要添加角度路由吗？**键入 **y** 表示是，并且
*   您想使用哪种样式表格式？选择 **CSS** 。

接下来，导航到您的项目文件夹，并使用以下命令运行本地开发服务器:

```
$ cd angular10qrcode
$ ng serve 
```

打开网络浏览器，导航至`http://localhost:4200/`地址，查看您的应用程序运行情况。

接下来，打开一个新的终端，确保导航到您的项目文件夹，并运行以下命令，使用以下命令从 npm 安装 [`ngx-qrcode`库](https://github.com/techiediaries/ngx-qrcode):

```
$ npm install @techiediaries/ngx-qrcode
```

接下来打开`src/app/app.module.ts`文件，从模块中的`@techiediaries/ngx-qrcode`导入`NgxQRCodeModule`，如下所示:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { NgxQRCodeModule } from '@techiediaries/ngx-qrcode';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    NgxQRCodeModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

一旦库被导入，您可以在您的 Angular 应用程序中使用`ngx-qrcode`组件。

> 请注意，我们还导入了`FormsModule`。

接下来，打开`src/app/app.component.ts`文件，并更新如下:

```
import { Component } from '@angular/core';
import { NgxQrcodeElementTypes, NgxQrcodeErrorCorrectionLevels } from '@techiediaries/ngx-qrcode';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  elementType = NgxQrcodeElementTypes.URL;
  correctionLevel = NgxQrcodeErrorCorrectionLevels.HIGH;
  value = 'https://www.techiediaries.com/';
}
```

接下来，打开`src/app/app.component.html`文件并添加以下代码:

```
<ngx-qrcode
  [elementType]="elementType"
  [errorCorrectionLevel]="correctionLevel"
  [value]="value"
  cssClass="bshadow"></ngx-qrcode>
```

我们使用各种属性来配置我们的 QR 码，例如:

*   类型，
*   纠错级别，
*   价值，
*   CSS 类。

您可以从官方 [`ngx-qrcode`](https://www.techiediaries.com/ngx-qrcode/) [文档](https://www.techiediaries.com/ngx-qrcode/)中找到关于这些属性和其他支持属性的更多信息。

接下来，添加一个文本区域，用于输入要编码的值:

```
<textarea [(ngModel)] = "value"></textarea>
```

最后打开`src/styles.css`文件，添加以下样式:

```
.bshadow {

  display: flex;
  align-items: center;
  justify-content: center;
  filter: drop-shadow(5px 5px 5px #222222);
  opacity: .5;

}

textarea {
    margin-top: 15px; 
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 250px;
    opacity: .5;
}
```

这是我们应用程序的屏幕截图:

![Screenshot-from-2020-08-06-20-26-36](img/67a2b50f7200598174ef28ce2ad161c9.png)

就这样，我们完成了我们的 Angular 10 示例项目，演示了如何生成 QR 码。

你可以通过 **`Techiediaries`** 访问我们，获取关于 Angular 和现代 web 开发实践的教程。

您可以在 Stackblitz 上查看我们在本文中构建的应用程序:

[https://stackblitz.com/edit/angular-ngx-qrcode-example?embed=1&file=src%2Fapp%2Fapp.component.ts](https://stackblitz.com/edit/angular-ngx-qrcode-example?embed=1&file=src%2Fapp%2Fapp.component.ts)