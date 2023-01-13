# 如何使用版本控制来保持您的 web 应用程序最新

> 原文：<https://www.freecodecamp.org/news/versioning-your-web-apps-38d9d1ccec05/>

卡姆莱什·钱德纳尼

# 如何使用版本控制来保持您的 web 应用程序最新

![tTjxWxIeP0CN2rxfK6t2f06pGTuK2VYjlo9w](img/b6be099799f6a399b668b5044f84a7a8.png)

版本控制有助于您跟踪哪些用户正在使用您的应用程序的哪个版本。

对于原生应用程序，您必须在每次构建时维护应用程序的版本。然后，只有您能够将您的应用程序的新版本发布到 App Store/Play Store。

但是，您将如何维护 web 应用程序的版本控制呢？

故事时间！

在 90 年代早期，有像 PHP、Java 和 JSP 这样的服务器端语言，它们帮助你的所有用户总是获得你的 web 应用的最新版本。

但是现在网络应用已经达到了一个新的水平。一切都是客户端！因此，我们可以利用预缓存、按需加载、同时呈现有意义的数据等概念。

但是，如果用户总是访问我们的 web 应用程序的缓存副本，这也会带来问题。

想象一下，一家 SaaS 公司的最终用户不知道如何正确使用 web 应用/下一代 web 应用/PWAs。

当涉及到像 PWAs 这样的现代网络应用程序时，你不能确保你的所有用户都在使用你的应用程序代码的最新版本。

假设您第一次发布了您的 web 应用程序，并且用户已经开始使用它。第一次访问后，应用程序会被缓存，此后在每次重复访问时，用户都会获得应用程序的缓存副本，直到新版本的应用程序代码可用。一切顺利。

但是现在假设过了一段时间，在下一次迭代中，您向现有的 web 应用程序添加了一些新功能，并部署了新的代码/包。

***吊杆* * *

您如何确保您的用户使用最新版本的 web 应用程序？

你如何确定有多少用户仍在使用你的旧版本应用？

所有这些问题都鼓励你维护和存储你的 web 应用程序的当前版本，这样当用户使用你的应用程序时，应用程序版本也存储在数据库服务器中。

但是“如何”维护版本的秘密仍然没有解决！

如果你使用 Webpack 捆绑你的代码，Git Revision Webpack 插件将会拯救你。

它是一个简单的 [webpack](http://webpack.github.io/) 插件，在基于本地 [Git](https://www.git-scm.com/) 库的构建过程中生成`VERSION`和`COMMITHASH`文件。

### 使用

1.  向您的提交添加一个标记。

```
syntax: git tag <tag-name>git tag v1.0
```

2.将以下内容添加到 webpack 配置文件中:

```
const GitRevisionPlugin = require("git-revision-webpack-plugin");
```

```
const gitRevisionPlugin = new GitRevisionPlugin();
```

3.将 webpack [定义插件](http://webpack.github.io/docs/list-of-plugins.html#defineplugin)添加到插件数组中。

```
const plugins = [.....new webpack.DefinePlugin({APP_VERSION_INFO: {  VERSION: gitRevisionPlugin.version(), //returns the output of git- describe command  COMMITHASH: gitRevisionPlugin.commithash(), // returns last commit hash  BRANCH: gitRevisionPlugin.branch() // returns the branch name from which the build was run};})...]
```

4.现在，您可以在应用程序中的任何地方使用`APP_VERSION_INFO`,因为它将在全球范围内提供。

```
console.log('Check App Version ', APP_VERSION_INFO);
```

你喜欢这个故事吗？
*推荐(通过点击❤按钮)或分享这个故事，以便其他人可以阅读它！*