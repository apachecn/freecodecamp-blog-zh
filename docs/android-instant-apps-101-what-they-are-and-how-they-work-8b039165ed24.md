# Android 即时应用 101:它们是什么以及如何工作

> 原文：<https://www.freecodecamp.org/news/android-instant-apps-101-what-they-are-and-how-they-work-8b039165ed24/>

by Tomislav Smrečki

![IOXBr10BPwIBeUprd1rbMKeixeXDB8N3aIbh](img/01ed0b16a003b97c7eee9324ea43d1e6.png)

Photo by [rawpixel](https://unsplash.com/photos/-WqoSoviKwc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/instant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Android 即时应用是一种无需预先安装即可使用原生应用的酷新方式。只有部分应用程序被下载和启动，在几秒钟内给用户一个本地的外观和感觉。

#### 它们是如何工作的？

首先，不要将它们与渐进式网络应用混淆，渐进式网络应用是通过 Chrome 浏览器的启动器图标打开网络应用。一个即时应用程序实际上会安装在你的手机上，但不需要在 Play Store 上搜索。

Web URLs 将触发手机上的谷歌 Play 商店，并只获取与请求的 URL 相关的应用程序部分。应用程序的其余部分不会下载。这样用户可以快速享受你的 Android 应用的原生体验。

#### 有什么背景？

嗯，你需要把你的 Android 项目分成几个模块。其中一个是基本模块，其基本代码用于所有其他模块(API 连接、数据库、共享首选项等)。).另一个是功能模块，包含可以通过相关 URL 访问的特定功能和活动。

假设您有一个 web 应用程序，其中有一个产品列表和一个产品页面。例如，您可以链接来启动 ProductsListActivity，链接来启动 ProductActivity。

要使它们作为即时应用程序活动可访问，需要将它们打包到单独的功能模块中，并且需要在它们的模块清单中定义相关的应用程序链接。我们称之为产品和产品列表模块。

现在，当用户试图打开**[【https://example.domain/products/12】](https://example.domain/products/12,)**时，产品和基础模块都将开始下载，并且 ProductActivity 将被启动。

#### 什么是 app 链接，如何定义？

你可能听说过深层链接。它们在应用程序清单中定义，并将注册到操作系统。当用户试图打开此类链接时，操作系统会要求用户选择是在网络浏览器还是在你的应用程序中打开链接。但是，这对于即时应用来说还不够，你需要更进一步——[应用链接](https://developer.android.com/training/app-links/)。您需要包含 **autoVerify="true"** 属性。

```
<activity android:name=".ProductActivity"> <intent-filter android:autoVerify="true" android:order="100"> 
```

```
<action android:name="android.intent.action.VIEW" /> <category android:name="android.intent.category.DEFAULT" /> <category android:name="android.intent.category.BROWSABLE" /> 
```

```
<data android:scheme="http"      android:host="example.domain"       android:pathPrefix="/products" /> <data android:scheme="https"/> 
```

```
</intent-filter> </activity>
```

您的应用程序将[验证](https://developer.android.com/training/app-links/verify-site-associations)您指定的链接是否确实与您的域名相关联。为此，您需要将 **assetlinks.json** 文件包含到您的域根目录的以下文件夹中:

[**https://example.domain/.well-known/assetlinks.json.**](https://example.domain/.well-known/assetlinks.json.)

**另外，请注意**Android:order = " 100"**属性。在这种情况下，这实际上是一个优先事项。如果你有一个产品列表和一个产品单对应同一个路径**(/产品和/产品/10)** ，如果在**/产品**路径后有一个 id，就会发起产品单活动。如果没有，则启动产品列表活动。**

**定义这一点非常重要。如果有两个活动对应于相同的路径，Play Store 将不会知道应该获取应用程序的哪个部分。**

#### **将您的应用程序与您的域相关联**

****assetlinks.json** 将需要包含 SHA256 密钥库散列。relation 字段设置为下面的默认值，目标对象需要填充应用程序特定的数据和您的密钥库 SHA256 哈希。**

```
`[{   "relation": ["delegate_permission/common.handle_all_urls"],  "target": {   "namespace": "android_app",   "package_name": "com.example.app",   "sha256_cert_fingerprints":["00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00"]   } }]`
```

**当 **autoVerify=true** 施展魔法时，所有关联的应用程序链接将直接启动你的应用程序。如果您没有安装该应用程序，将会下载即时应用程序。**

**这是我们最近做的一个演示应用的例子。当点击相关链接时，一个类似这样的屏幕打开，并提供使用即时应用程序代替。注意应用程序打开的速度，在奥利奥上甚至更快。**

#### **如何定义 Android 即时模块？**

**对于即时应用程序，您的项目将包含至少三个不同的模块。这个需要用 Android Studio 3.0。如果您正在从头开始创建应用程序，可以选择为您的项目启用即时应用程序支持。**

**以下所有模块将自动初始化。如果你正在修改一个旧的应用程序，你需要把旧的应用程序模块分成一个基本模块和几个功能模块。此外，您需要创建一个应用程序和一个即时应用程序模块，您将使用它们来构建常规和即时应用程序 apk。**

****App 模块****

**首先，您必须创建一个应用程序模块，它定义了所有其他模块的依赖关系(基本模块+功能模块)。在本模块的 build.gradle 文件中，您需要定义以下内容:**

```
`apply plugin: 'com.android.application' ...`
```

```
`dependencies {   implementation project(':product')   implementation project(':productlist')   implementation project(':base') }`
```

****基础模块****

**在本模块中，您将定义以下依赖关系语句。另外，确保这里应用了**‘com . Android . feature’**插件。**

```
`apply plugin: 'com.android.feature' android {  baseFeature true   ... }` 
```

```
`dependencies {   api 'com.android.support:appcompat-v7:26.0.1'   api 'com.android.support.constraint:constraint-layout:1.0.2'  implementation 'com.google.firebase:firebase-appindexing:11.0.4'  application project(':app')   feature project(':product')   feature project(':productlist') }`
```

**注意，在这里，编译语句变成了我们之前使用的常规依赖项的 API 语句。应用程序项目和功能项目是分开定义的。**

****功能模块****

**这个模块将有以下设置，也应用了 **com.android.feature** 插件。**

```
`apply plugin: 'com.android.feature' ... dependencies {   implementation project(':base')   ... }`
```

**您需要说明哪个模块是您的基础模块，并将其包含在实施项目声明中。接下来，您可以包含这个特定模块所需的依赖项。例如，如果您使用的动画库在任何其他模块中都没有使用。**

****即时应用模块****

**最后，现在 instantapp 模块的 **build.gradle** 文件中包含了一个 **com.android.instantapp** 插件。**

```
`apply plugin: 'com.android.instantapp' dependencies {   implementation project(':product')   implementation project(':productlist')   implementation project(':base') }`
```

**在本模块中，我们将定义哪些模块将构建为即时应用程序。instantapp 模块构建的结果是一个带有即时应用 APKs 的 zip 文件，您可以在 Android 即时应用发布管理器中将它单独上传到谷歌 Play 商店。这些 apk 的处理方式与常规 apk 类似，它们有自己的首次展示历史和版本。**

**就是这样！开始开发 Android 即时应用程序相当简单。但是，总有但是！**

#### **Android 即时应用的挑战是什么？**

**首先，即时应用目前默认不启用。如果你想试试，你需要在谷歌帐户下检查你的手机设置，并启用即时应用程序设置。**

**接下来，我们发现按照以下格式指定应用链接数据极其重要:**

```
`<intent-filter android:autoVerify="true"> ... <data android:scheme="http"   android:host="example.domain"   android:pathPrefix="/products" /> <data android:scheme="https"/> </intent-filter>`
```

**http 和 https 方案都需要定义，如下面的代码片段所示。任何其他方式都会导致链接验证失败，应用程序无法正确链接。**

**此外，建议将以下代码片段包含到您的应用程序清单中的一个活动中。这注释了在从设置或系统启动器启动即时应用程序的情况下应该启动哪个活动。**

```
`<meta-data  android:name="default-url" android:value="https://example.domain" />`
```

**官方文档指出，谷歌搜索将默认提供即时应用注释(小雷霆图标)，但我们有这个问题。对于我们的演示应用程序，情况并非如此。谷歌搜索结果没有将我们的演示链接标注为即时应用，这些链接指向网页。只有当我们试图从另一个应用程序(如 Gmail)打开相关链接时，整个即时应用程序流程才会被触发，即时应用程序才会启动。你遇到过类似的问题吗？**

#### **结论**

**两年前首次发布时，我对 Android 即时应用程序非常感兴趣。他们回应了用户必须在商店上搜索应用程序并等到下载后才能开始使用它们的问题。在这方面，网络应用程序更容易访问，而且更容易发现。**

**即时应用非常接近于填补网络和本地移动应用之间的鸿沟。他们已经表现得很好了，我认为随着时间的推移，他们会越来越受欢迎。我们遇到的主要问题是一个相当小的社区和缺乏适当的文件，但这方面的情况也在好转。**

**如果您尝试过使用它们或者在实现它们的过程中遇到任何挑战，我们很乐意收到您的来信！**

***最初发表于[www.bornfight.com](https://www.bornfight.com/blog/android-instant-apps-101/)。***