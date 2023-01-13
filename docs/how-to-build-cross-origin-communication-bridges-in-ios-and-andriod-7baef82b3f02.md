# 如何在 iOS 和 Android 中搭建跨原点的沟通桥梁

> 原文：<https://www.freecodecamp.org/news/how-to-build-cross-origin-communication-bridges-in-ios-and-andriod-7baef82b3f02/>

我正在做一个项目，在这个项目中，我需要通过消息连接几个不同的组件。每个都有自己的逻辑和代码语言。这让我想了解不同平台实现交流的所有方式。

这篇文章的目的是解释这些跨来源的沟通桥梁，并提出简单而有益的例子来实现它们。

也会有很多桥段双关语？

你被警告过。

如果您只是想接触一下代码，在本文的底部有到 GitHub 库的链接。

通常，您编写的 JavaScript 将在浏览器中运行。在 **iOS** 、**T3 上，可以是 UIWebView，也可以是 WKWebView。在 **Android** 上，一个 WebView。**

由于 iOS 可能是更令人恼火的平台，我将首先描述那里的沟通桥梁。

### 伦敦桥正在倒塌(iOS)

从 iOS 8 开始，苹果推荐使用 WKWebView 而不是 UIWebView，所以下面将只解决一个 **WKWebView** 上的桥。

如需 UIWebView 参考，请前往[此处](https://stackoverflow.com/questions/5671742/send-a-notification-from-javascript-in-uiwebview-to-objectivec)。

要从 WKWebView 向 JavaScript 发送消息，请使用以下方法:

```
 - (void)evaluateJavaScript:(NSString *)javaScriptString 
         completionHandler:(void (^)(id, NSError *error))completionHandler;
```

要在 WKWebView 中接收来自 JavaScript 的消息，您必须执行以下操作:

1.  创建一个实例 [WKWebViewConfiguration](https://developer.apple.com/documentation/webkit/wkwebview/1414979-configuration?language=objc)
2.  创建一个 [WKUserContentController](https://developer.apple.com/documentation/webkit/wkusercontentcontroller?language=objc) 的实例
3.  向您的配置中添加一个脚本消息处理程序(这一部分弥补了这一缺陷)。此操作还会在窗口对象上的以下路径下注册您的消息处理程序:**window . WebKit . message handlers . msg _ HANDLER _ NAME**
4.  通过在文件顶部添加<wkscriptmessagehandler>,使该类实现消息处理程序协议</wkscriptmessagehandler>
5.  实现[userContentController:didReceiveScriptMessage](https://developer.apple.com/documentation/webkit/wkscriptmessagehandler/1396222-usercontentcontroller?preferredLanguage=occ)(该方法处理从 JavaScript 接收消息)

### 建造桥梁

假设我们设置了以下 HTML 页面:

```
<html>

  <head>
    <title>Javascript-iOS Communication</title>
  </head>

  <body>

    <script>
      window.webkit.messageHandlers.myOwnJSHandler.postMessage("Hello World!");
    </script>
  </body>

</html>
```

在我们的本机代码中，我们实现了上述步骤:

```
#import <UIKit/UIKit.h>
#import <WebKit/WebKit.h>

// 4
@interface ViewController : UIViewController <WKScriptMessageHandler>

@property(nonatomic, strong) WKWebView *webview; 
```

还有维奥莱！现在您已经有了完整的 JavaScript - iOS 通信！

![1*EosjstTDed_5cYeD7Fa-mQ](img/887ef9bbbac63e382ad8002c6b011cd4.png)

Photo by [Todd Diemer](https://unsplash.com/photos/OHMg0Hgetn4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/bridge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 过桥(安卓)

这里的事情简单多了，也友好多了。为了建立我们的沟通桥梁，只有几个步骤:

1.  创建一个 [WebView](https://developer.android.com/reference/android/webkit/WebView) 对象的实例
2.  在此 WebView 中启用 JavaScript(**set JavaScript enabled**)
3.  设置您自己的 JavaScript 接口(它将包含对您的 JavaScript 可见的方法)
4.  任何要向您的 JavaScript 公开的方法在其声明之前都必须有**@ JavaScript interface***注释*

*像以前一样，假设我们已经创建了这个 HTML 文件:*

*我们已经创建了以下简单的 Android 应用程序:*

*这就对了。*

*你现在可以认为自己是一个原生通信忍者！*

*以下是存储库的链接:*

*[AndroidtoJS Repository](https://github.com/TomerPacific/MediumArticles/tree/master/AndroidtoJSNativeCommunicator)[iOStoJS Repository](https://github.com/TomerPacific/MediumArticles/tree/master/iOStoJSNativeCommunicator)

### ⚠️关于 iOS ⚠️的重要说明

当你到了想要破坏你的 WKWebView 的地步，那就是**命令式**移除你的脚本消息处理程序。如果您不这样做，脚本消息处理程序仍将保存对您的 WKWebView 的引用，并且在创建新的 wk webview 时会发生内存泄漏。*