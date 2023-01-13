# 如何使用 Ionic 4 和 Firebase 获得推送通知

> 原文：<https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/>

菲利普·耶尔加

# 如何使用 Ionic 4 和 Firebase 获得推送通知

#### 一个完整的循序渐进的教程，将带你进入 iOS 和 Android 的正确轨道

![1*EciBTQhuzIrkzA12dfsQJQ](img/88c53b657423861b70a9f7093cbe5763.png)

设置推送通知确实令人沮丧且耗时。所以我检查了所有的设置，为你准备了这个教程。

### 先决条件

Ionic 4 应该已经安装了。

### 部分导航

1.  [包安装](https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/#1-package-installation)
2.  【Android 和 iOS 的 Firebase 设置
3.  [推送通知代码实现](https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/#3-push-notification-code-implementation)
4.  [在 Android 上测试推送通知](https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/#4-test-push-notifications-on-android)
5.  [iOS 推送通知预设置](https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/#5-ios-push-notification-pre-setup)
6.  [在 iOS 上测试推送通知](https://www.freecodecamp.org/news/how-to-get-push-notifications-working-with-ionic-4-and-firebase-ad87cc92394e/#6-test-push-notification-on-ios)

### 1.软件包安装

在您选择的编码编辑器中打开您的 Ionic 项目，并打开您的终端。导航到您的项目文件夹。

首先，我们将安装所有必需的软件包。

**我们需要安装什么:**

*   【firebase 的 Cordova 插件: `ionic cordova plugin add cordova-plugin-firebase`
*   **Firebase 原生包:**由于 Ionic 4 处于测试阶段，请在`package.json` 中检查您的 Ionic 原生包，并安装与其他 Ionic 原生包相同的版本。最后，我们来输入:`npm install --save @ionic-native/firebase@5.0.0-beta.14`

![1*4R0G8E3rY6N8xrwRYpWO8A](img/c2146852f8cefdd6610c078085f308d0.png)

I have version of beta.14

*   最后一包， **AngularFire 2** 。这是一个角状和火基的库:`npm install --save angularfire2 firebase`

软件包安装，完成！让我们移到第二部分。

### 2.iOS 和 Android 的 Firebase 设置

在我们开始所有的设置之前，我需要警告你，你不能在 iOS 模拟器上测试你的推送通知。为了测试它，你需要有一个苹果开发者账户，每年大约花费 99 美元。我建议你无论如何都要完成 iOS 设置，这样会让你对未来的项目有更好的理解。

**注意:**这里开始的步骤非常重要，请耐心等待。慢慢读，确保一切都正确。相信我，在所有的设置之后寻找问题是非常令人沮丧的——这是我自己的经验之谈。

#### ios

[导航至 Firebase 页面](https://firebase.google.com)并登录控制台。如果您还没有创建项目，现在就创建。您应该看到这个屏幕。

![1*YKEMKgcl7nukC-1QOOqmAw](img/4d45c8800356e524da22e694c100f9c2.png)

点击 iOS 按钮，您会看到:

![1*yzwF8xD5qUFfy5yBWmmkZw](img/da9a01935354c44d00f4561667d45ac9.png)

现在我们需要提供我们的 iOS 包 ID，这必须与您的 Ionic 项目中的 ID 相同。假设我想要包名`com.filipjerga.angularcourse`，那么我需要做以下事情:

打开 Ionic 项目并导航到“config.xml”文件。让我们检查一下 widget 元素。 **Id** 属性保存您的应用程序**的唯一标识符。**我之前说过，如果你在 Firebase 中将你的包名指定为`com.filipjerga.angularcourse`，那么你的 Ionic 项目中的 **id** 必须是相同的！你也可以把 id**留在你的 Ionic 项目中，但是你需要在 Firebase 中修改它。**

![1*nKaEn3rqKjQQSK5DbOTx1Q](img/f4d676ce34995e1ebb0da64d08c22186.png)

widget element id is what we need to specify in Firebase

在获得 **id 的值之后，**不要忘记将它作为包 id 提供给 Firebase 应用程序。

![1*80_WthW7_5CQbd5Dcg6BvA](img/535bda6bb4d0468b9d68cfab4b230f37.png)

Bundle ID has to be a same as your widget id

这应该是注册应用程序的第一步。这一步至关重要，所以要仔细检查小部件上的 **id** 的值和 Firebase 应用程序**的包 id。**

将其他字段留空，然后单击“注册应用程序”。

现在我们需要下载“GoogleService-Info.plist”。

![1*OdG0hBlzpiMMvYT7w6gPxg](img/11a04869bececadcfad1c5911945b27f.png)

下载后，将其粘贴到项目的基本文件夹中。你可以在[我的项目](https://github.com/Jerga99/heartStoneLib)中看到一个文件夹结构。

我们可以跳过所有进一步的步骤，因为 Ionic 项目设置不需要这些步骤。您应该已经准备好了 IOS 应用程序。

![1*HVdgpvS2Rv2a1dfuNDPgtw](img/4a340a1ae5748ad346072f70da56424f.png)

Skip this step

#### 机器人

Android 的以下步骤与 iOS 设置几乎相同:

*   在 Android 上点击“添加应用”，就像我们之前在 iOS 上做的一样。
*   Android 包名需要与我们的 widget id 相同，在我的例子中:`com.filipjerga.angularcourse`
*   接下来，下载`google-services.json` **。**和之前的 iOS 文件一样，我们需要将它复制到应用程序的基本文件夹中
*   点击“下一步”,直到您进入最后一步，您可以跳过这一步，您应该已经创建了两个应用程序。

耶！恭喜你。但现在欢呼还为时过早。

![1*QN0VI93SeNd5aJ1-yDHJSg](img/f1ed41c50c54d137df36873aebf11d57.png)

Both iOS and Angular apps created.

### 3.推送通知代码实现

#### 包导入

是时候通过键入一些代码来最终温暖我们的手指了。我们将从导入之前安装的包开始。

1.  转到`app.module.ts`
2.  您的文件应该如下所示:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouterModule, RouteReuseStrategy } from '@angular/router';

import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

import { AngularFireModule } from 'angularfire2';
import { AngularFirestoreModule } from 'angularfire2/firestore';
import { Firebase } from '@ionic-native/firebase/ngx';

const config = {
    apiKey: "AIzaSyD-K6SlFECXKmd8iHwEvggVtavKgyPF2k8",
    authDomain: "angular2-course-9270e.firebaseapp.com",
    databaseURL: "https://angular2-course-9270e.firebaseio.com",
    projectId: "angular2-course-9270e",
    storageBucket: "angular2-course-9270e.appspot.com",
    messagingSenderId: "443316848633"
  };

@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [
    BrowserModule,
    IonicModule.forRoot(),
    AppRoutingModule,
    IonicStorageModule.forRoot(),
    AngularFireModule.initializeApp(config),
    AngularFirestoreModule],
  providers: [
    StatusBar,
    SplashScreen,
    Firebase,
    { provide: RouteReuseStrategy, useClass: IonicRouteStrategy }
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

app.module.ts

您可以在 providers 数组中看到`Firebase`，在 imports 中看到`AngularFirestoreModule`和`AngularFireModule`。

但是`config`物体是从哪里来的呢？你可以在那里看到很多信息，比如“apiKey，authDomain”等等。

要回答这个问题，我们需要回到 Firebase 控制台，创建一个 **web 应用**。

![1*js9Izqbu8Bceb2kIZSDzRg](img/0096962ffd116dcfabf9e95b57a87a8a.png)

Select web platform

你需要点击 Android 图标右边的一个网络平台图标(见上图)。当选择 web 应用程序时，您将看到自己的**配置**对象。

![1*5a3ry984v5Mer9Ob63axZQ](img/d483d723c8ebc31a0b5b585ed1c5afe2.png)

My config object for Firebase after selecting web app.

现在是时候在我们的 Ionic 项目中将整个**配置**对象复制到`app.module.ts`了。请**确保**你为你的配置对象改变它！我的不适合你。

![1*Gkp_0kiAs4f_t3UL_hUNVw](img/22ccfdd994c03c5aa2db6eafe12a65f1.png)

Provide config to app.module.ts

现在我们可以开始实现推送通知服务了。

#### 推送通知服务

让我们创建一个新的服务。随便你怎么称呼。我会叫我的`fcm.service.ts`。FCM 代表 Firebase 云消息传递。

首先，让我们看看服务实现。我将逐行解释它。

```
import { Injectable } from '@angular/core';
import { Firebase } from '@ionic-native/firebase/ngx';
import { Platform } from '@ionic/angular';
import { AngularFirestore } from 'angularfire2/firestore';

@Injectable()
export class FcmService {

  constructor(private firebase: Firebase,
              private afs: AngularFirestore,
              private platform: Platform) {}

  async getToken() {
    let token;

    if (this.platform.is('android')) {
      token = await this.firebase.getToken();
    }

    if (this.platform.is('ios')) {
      token = await this.firebase.getToken();
      await this.firebase.grantPermission();
    }

    this.saveToken(token);
  }

  private saveToken(token) {
    if (!token) return;

    const devicesRef = this.afs.collection('devices');

    const data = {
      token,
      userId: 'testUserId'
    };

    return devicesRef.doc(token).set(data);
  }

  onNotifications() {
    return this.firebase.onNotificationOpen();
  }
}
```

fcm.service.ts

如果我们想发送一个推送通知给一个设备，我们需要得到这个设备的唯一标识符。在这种情况下，它被称为`token`。

我们需要检查特定于平台的设备，因为在 iOS 设置中有一个额外的步骤。iOS 需要显式权限来接收推送通知。

现在我们需要将这个令牌存储在某个地方，但是在哪里呢？我们将在 **Firebase 数据库**中存储令牌。你可以看到，我正在创建**设备**集合，并用包含`token`和一个测试`UserId`的`data` 填充它们。完美！现在，我们已经存储了令牌，可以订阅通知了。

订阅通知其实很简单。我们只需要调用`this.firebase.onNotificationOpen()`

太神奇了。服务已检查！

所有这些都很好，但有点没用，因为我们还没有使用我们的服务。让我们修理它！

移动到您的`app.component.ts` 并写下以下内容:

```
import { Component } from '@angular/core';

import { Platform } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

import { FcmService } from './shared/service/fcm.service';
import { ToastService } from './shared/service/toast.service';
import { ToastController } from '@ionic/angular';

@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html'
})
export class AppComponent {

  constructor(
    private platform: Platform,
    private splashScreen: SplashScreen,
    private statusBar: StatusBar,
    private fcm: FcmService,
    private toastr: ToastService,
    public toastController: ToastController

  ) {
    this.initializeApp();
  }

  private async presentToast(message) {
    const toast = await this.toastController.create({
      message,
      duration: 3000
    });
    toast.present();
  }

  private notificationSetup() {
    this.fcm.getToken();
    this.fcm.onNotifications().subscribe(
      (msg) => {
        if (this.platform.is('ios')) {
          this.presentToast(msg.aps.alert);
        } else {
          this.presentToast(msg.body);
        }
      });
  }

  initializeApp() {
    this.platform.ready().then(() => {
      this.statusBar.styleDefault();
      this.splashScreen.hide();
      this.notificationSetup();
    });
  }
}
```

app.component.ts

这里特别重要的是函数`notificationSetup`。

我们首先获得设备的唯一令牌。

我们还订阅来自 Firebase 的接收通知。

当收到消息时，我们需要再次检查特定的平台。在 iOS 上，您的信息文本位于`aps.alert`下。在 Android 上，它在`body`下。

然后，我们将简单地将收到的消息显示为一个`Toast`。

现在代码设置完成了。我们非常接近了！是时候检验一下了。

### 4.在 Android 上测试推送通知

Android 所需的所有设置现在都应该完成了。您可以通过以下方式开始模拟您的应用:

`ionic cordova emulate android`

或者

在 Android Studio 中手动打开你的版本。

让我们启动应用程序并转到主菜单，我们将在那里看到一个推送通知。确保您的应用程序在模拟器中正确启动，并且没有错误。

回到你的浏览器，回到你的 Firebase 应用程序。现在是时候检查我们的 Firebase 数据库了。您可以在左侧面板的**开发**类别下找到数据库选项。

![1*ev05fxLy5yySp_Xa-uA4JQ](img/85a96083584326bde3e228ada0d90f4d.png)

Firebase Database

在模拟器中启动您的应用程序后，我们刚才编写的来自 **app.component.ts** 的代码运行了。难怪我们的数据库被填充。在“saveToken”函数中，我们指定了“devices”集合，并将带有用户 id 的令牌作为文档保存到该设备集合中。这就是我们在数据库中看到的。

在我的例子中，我的数据库中有多个令牌，但你应该只有一个，因为我们第一次运行我们的应用程序。您将为运行应用程序的每个唯一设备/仿真器创建一个新文档。

现在是时候复制这个令牌了，以便向特定设备发送推送通知。

![1*tGtpXyvp46jGuGr9SzsBTg](img/4fc6c0d472a7bae637339ed25f75613f.png)

Firebase database collection, You will see only one document

**导航至左侧面板的增长选项卡**，点击云消息。

![1*_oO9cu4Gw3IAXbvGuWeWOQ](img/e64fdb620e1c2a98a8359aa6ba1b193a.png)

Click on Cloud Messaging

现在我们需要填写必要的数据。输入您的消息文本，并提供我们刚刚复制的数据库中的设备令牌。

![1*vdXEcVntcKkfqr8ll49r6Q](img/789d7d536511a3885b16c1dd46a6046d.png)

当您发送消息时，同时打开您的仿真设备并观察会发生什么。

发送一条消息，AAAA 和祝贺！现在你的 Android 设置完成了，你可以发送推送通知了！是不是很牛逼？

![1*HwQKVZAHeF6Tb1QZV46Zzw](img/027535648ad121246e1ccba79d51ed35.png)

Push notification on Android.

### 5.iOS 推送通知预设置

各位系好安全带，iOs 设置来了。让我们把这个设置分成多个步骤，这样我们就不会恐慌了。让我们开始吧！

**首先，**为 iOS 构建你的应用:`ionic cordova build ios`

打开您的 Xcode，并在`platforms/ios/nameofyourapp.xcodeproj`中找到您构建的应用程序。打开它。

![1*2J27mF3dE36yro4TDuDbZA](img/48123bc8fbca6280d75dee40445cb593.png)

Open your .xcodeproj

这将在左侧打开应用程序的树形结构。双击此结构的根文件。这将打开一个附加菜单，为您的应用程序提供更多设置。

![1*CwCbrVvqy_lYJhOHahe5qQ](img/9133392591c1587f01f2ee1d53454e47.png)

More settings

使用您的开发者帐户登录。

![1*AD1U9EEfHRozZHoUn3d5Ng](img/fe16829ef99adc90f852dfa5c78f9dbd.png)

Sign in with a developer account

5.打开顶部的“功能”选项卡，并启用“推送通知”。

![1*4lOdzB6p9BWGiVkYuC1G1w](img/bfa663c416d29266a9707de4664ad92d.png)

Enable Push Notifications

6.导航到你的[苹果开发者账户页面](https://developer.apple.com)。在“证书”下选择“全部”，点击`+"`到**添加**一个新的证书。

![1*M_sJ-0Y6UcfCSXvZexoh9g](img/4d9479f9bd9545b856f4f7c88f105165.png)

Click the plus sign.

启用 Apple 推送通知服务，并继续下一步。

![1*sUsADfJUdEJcyi2xblFjwg](img/2e991a98558fa54340a0eec648399c51.png)

现在让我们选择您的应用。

![1*nXick1xCKeQt8Xrv5LSdvw](img/83f10f78e095309b9b13a9efc1cc247b.png)

我们需要**请求证书**。在 Mac 上，前往“钥匙串访问”-“T2”-“证书助理”-“T3”-“向证书颁发机构请求证书”。

![1*rhTf5N0TL1AmKoXcWbhfoQ](img/8de0c0bbd25ca416d4edd94cfa414808.png)

填写所有必要的信息—您的电子邮件和常用名—并将其保存到磁盘。

![1*ytVFGho1inBYnv2pYYLLbA](img/66a07238ad164aa2efab91a167ef7a95.png)

在 Apple 控制台中，继续下一步并**上传**您的证书请求。

![1*Qlz9edFubew4V3ozdt2X7A](img/3e2c7662108bfb5ec83541ae92c7e9ad.png)

在下一步中，应该会创建您的证书，您可以下载它。你以后会需要它的。

![1*n5YZm8E6QYxNCDb9Ns7tyQ](img/e8364d0b32e2ae94f4b8dc2c537fe2f6.png)

现在我们需要**创建一个服务密钥**来启用苹果推送通知。在“密钥”下，选择“全部”。选择您的密钥名。启用“苹果推送通知服务(APNs)”。

![1*8lXVFTpEjwrApP2FArRaew](img/92add46e035883d9481135fa96c6b079.png)

点击“继续”并确认您的密钥。切勿与他人分享此类数据。您现在可以下载您的密钥了。

![1*S_gRFhnecm29gSTaSWpfiA](img/4584f8926e5e96f8eb55e2dc9e2e7d5c.png)

现在我们需要回到火基地。

在 Firebase 中，打开您的 iOS 应用程序，导航到“云消息”。

![1*10JoxYxq7LPUNv_magHS9A](img/22f2f464dbb50bf9201be36fc21fa7af.png)

我们需要**上传**我们不久前生成的 APN 认证密钥。点击“上传”。

![1*jZtNNpy7gTjsv4tjZfsy9A](img/9952e237ac93143e1977cca7ba74aec9.png)

提供所有信息并上传密钥。

首先，上传你的“. p8”文件，这个文件是之前从 Apple console 上下载的。输入您的密钥 ID。你可以从苹果控制台的“标识符”->“应用 ID”->“你的应用”->“前缀”中获取应用 ID 前缀。

![1*TKeh5ZvgKRN6MXZvw7e0qw](img/8b4a25a4daead00561d01360f6219bfd.png)![1*l7g5BPKU9GvG0c9RJFWTUA](img/bd7062f840eb485af7950b2180742736.png)

Upload p8 file and get keyID and ID prefix

就是这样。喜悦的泪水从我的脸上流下。

我们可以在 iOS 上测试推送通知。别忘了我们需要使用真正的设备。

### 6.在 iOS 上测试推送通知

首先，我们需要构建我们的应用程序，所以让我们运行:`ionic cordova build ios`

在 Xcode 中，您可以在通过 USB 与电脑连接的设备上运行应用程序。

![1*f4NOHT2xCJPgd2SXCXl-EA](img/c42f9d246a5ff16638bbc56a0b67e366.png)

Choose your device

等一切都启动了再说吧。我们现在可以重复从 [**第 4 节**](https://medium.com/p/ad87cc92394e#0d9c)**发送推送通知的步骤，因为这与 Android 上的相同。**

**记住**您现在需要使用一个新的令牌**，它是为您的 iOS 设备生成的。转到数据库，获取新令牌，并发送推送通知。您的结果应该是这样的。**

**![1*ZOE5hMQpOWxUIfE9zjDyfg](img/8b8c872858ee28ef2757ef1d49163795.png)****![1*zBEkbPOB6EEurPt5j3qzrw](img/5ff9d94149d0250c32fa48e8bb88b355.png)**

**我希望你已经成功设置了推送通知。做好每件事需要一些时间和耐心，但是结果和好处是惊人的。**

**如果你喜欢我的教程，并且你对更多感兴趣，可以在 Udemy 上查看我的课程: [**Ionic 4 速成班，有心石 API 和 Angular。**](http://bit.ly/2Ne2PhK)**

**完整项目见 [my Github Repo](https://github.com/Jerga99/heartStoneLib) 。**

**编码快乐！**

**菲利普**