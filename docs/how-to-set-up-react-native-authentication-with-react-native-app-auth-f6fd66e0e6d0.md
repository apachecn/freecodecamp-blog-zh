# 如何使用 react-native-app-auth 设置 React 本机身份验证

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-react-native-authentication-with-react-native-app-auth-f6fd66e0e6d0/>

作者:梅利赫·尤马克

# 如何使用 react-native-app-auth 设置 React 本机身份验证

![hRcfAQmBWLzhYNYmPfg8GQewd9adlfrX79JC](img/94197b99f1f5e11120282db1ffaa8e47.png)

Photo by [CMDR Shane](https://unsplash.com/@cmdrshane?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

#### 版本

开始之前，请确保您安装了以下版本:

" react": "16.8.3 "，
"react-native": "0.59.1 "，
" react-native-contacts ":" 3 . 1 . 5 "，

**如果你想了解一下的话，这里有 Github 回购的链接**:【https://github.com/FormidableLabs/react-native-app-auth 

**React-native-app-auth** 用于在 React-native 应用程序中提供身份验证。在我的情况下，我试图使用它与谷歌，所以这里有一个解释，你可以安装和使用它的上述版本。

在他们的文档中，它也被解释为用于与 [OAuth 2.0](https://tools.ietf.org/html/rfc6749) 和 [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) 提供者通信的 [AppAuth-iOS](https://github.com/openid/AppAuth-iOS) 和[AppAuth-Android](https://github.com/openid/AppAuth-Android)SDK 的 React 本地桥。

### 经过测试的 OpenID 提供商:

这些提供者符合 OpenID，这意味着您可以使用[自动发现](https://openid.net/specs/openid-connect-discovery-1_0.html):

*   [身份服务器 4](https://demo.identityserver.io/) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/identity-server-4.md))
*   [身份服务器 3](https://github.com/IdentityServer/IdentityServer3.md) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/identity-server-3.md))
*   [谷歌](https://developers.google.com/identity/protocols/OAuth2) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/google.md))
*   [Okta](https://developer.okta.com/) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/okta.md))
*   [键盘锁](http://www.keycloak.org/) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/keycloak.md))
*   [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory)([示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/azure-active-directory.md))
*   [AWS 认知](https://eu-west-1.console.aws.amazon.com/cognito) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/aws-cognito.md))

### 经过测试的 OAuth2 提供程序:

这些提供者实现 OAuth2 规范，但不是 OpenID 提供者，这意味着您必须自己配置授权和令牌端点。

*   [优步](https://developer.uber.com/docs/deliveries/guides/three-legged-oauth.md) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/uber))
*   [Fitbit](https://dev.fitbit.com/build/reference/web-api/oauth2/) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/fitbit.md))
*   [Dropbox](https://www.dropbox.com/developers/reference/oauth-guide) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/dropbox.md))
*   [Reddit](https://github.com/reddit-archive/reddit/wiki/oauth2) ( [示例配置](https://github.com/FormidableLabs/react-native-app-auth/blob/master/docs/config-examples/reddit.md))

#### **安装**

```
npm install react-native-app-auth --savereact-native link react-native-app-auth
```

**IOS**
在文档中，有三种方法可以实现这种状态，但我更喜欢 **CocoaPods。**

如果您是第一次使用 CocoaPods，请完成以下步骤:

```
sudo gem install cocoapods
```

从您的根文件夹打开

```
cd ios
```

```
pod init
```

pod init 命令将初始化 iOS 目录中的 pod 文件。

然后在你的 Podfile 中的**目标“你的应用程序”do** 之后添加下面这一行

```
pod 'AppAuth', '0.95.0'
```

#### **注册重定向 URL 方案**

如果您打算支持 iOS 10 和更早版本，您需要在您的`Info.plist`中定义支持的重定向 URL 方案，如下所示:

注意:您将从 **oauth 提供者**处获得这些值。对于谷歌:[https://console.developers.google.com/](https://console.developers.google.com/)

```
<key>CFBundleURLTypes</key><array>  <dict>    <key>CFBundleURLName</key>    <string>com.your.app.identifier</string>    <key>CFBundleURLSchemes</key>    <array>      <string>io.identityserver.demo</string>    </array>  </dict></array>
```

*   `CFBundleURLName`是任何全局唯一的字符串。一个常见的做法是使用你的应用标识符。
*   是你的应用程序需要处理的一系列 URL 方案。方案是 OAuth 重定向 URL 的开始，直到方案分隔符(`:`)字符。

#### **在 AppDelegate** 中定义 openURL 回调

您需要保留 auth 会话，以便从重定向继续授权流。请遵循以下步骤:

对`AppDelegate.h`做如下修改，使`AppDelegate`与`RNAppAuthAuthorizationFlowManager`一致:

```
+ #import "RNAppAuthAuthorizationFlowManager.h"
```

```
- @interface AppDelegate : UIResponder <UIApplicationDelegate>+ @interface AppDelegate : UIResponder <UIApplicationDelegate, RNAppAuthAuthorizationFlowManager>
```

```
+ @property(nonatomic, weak)id<RNAppAuthAuthorizationFlowManagerDelegate>authorizationFlowManagerDelegate;
```

从`AppDelegate.m`中的`UIApplicationDelegate`开始改变以下方法:

```
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *, id> *)options { return [self.authorizationFlowManagerDelegate resumeExternalUserAgentFlowWithURL:url];}
```

**安卓**

成功链接后，您应该添加**Android/app/build . grandle**文件 defaultConfig 值作为您的标识符重定向 url。

```
manifestPlaceholders = [
```

```
appAuthRedirectScheme: “io.identityserver.demo”
```

```
]
```

#### 使用

```
import { authorize } from 'react-native-app-auth';
```

```
// base configconst config = {  issuer: '<YOUR_ISSUER_URL>',  clientId: '<YOUR_CLIENT_ID>',  redirectUrl: '<YOUR_REDIRECT_URL>',  scopes: ['<YOUR_SCOPES_ARRAY>'],};
```

```
// use the client to make the auth request and receive the authStatetry {  const result = await authorize(config);  // result includes accessToken, accessTokenExpirationDate and refreshToken} catch (error) {  console.log(error);}
```

**编码快乐！**

谢谢你读到这里。如果你喜欢这篇文章，请分享，评论，并按下那个？几次(最多 50 次)。。。也许会对某个人有帮助。

**如果你对未来类似这些更深入、信息更丰富的文章感兴趣，请在 Medium 或 [Github](https://github.com/hadnazzar) 上关注我。**？