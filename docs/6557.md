# 如何使用创建-反应-应用程序和定制服务人员构建 PWA

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-pwa-with-create-react-app-and-custom-service-workers-376bd1fdc6d3/>

**注意:这不是关于 create-react-app 或什么是服务人员的初级读本。这篇文章假设你对这两者都有所了解。**

因此，我最近有机会参与一个 React 项目，该项目涉及将最终的 web 应用程序发布为一个渐进式 Web 应用程序(PWA)。

我意识到在 Create React App (CRA)版本中配置带有自定义路线的 PWA 是一件多么困难的事情。希望这能帮助陷入类似困境的人。

### **创建-反应-应用程序中的 PWAs**

我们如何让 PWA 在我们的 CRA 外壳中运行？

现在，CRA shell 默认绑定了一个服务人员。您应该已经注意到，在一个基本的 CRA shell 中，在`index.js`文件内部，有一个对`registerServiceWorker`的调用:

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

你可以创建一个新的 CRA 应用程序并查看`registerServiceWorker`文件。

这看起来很复杂，但它实际上只是检查环境变量是否为生产版本设置，以及当前浏览器是否支持`serviceWorker`。

如果您使用命令`yarn build`运行构建，您可以打开构建文件夹并在里面查看是否已经生成了一个`service-worker.js`文件。这是 CRA 为您生成的默认服务人员文件。

文件的格式是内联的 ES5 JavaScript，这使得阅读起来有点困难。但是你可以把它转储到任何一个编辑器中，你会看到一个更清晰的文件。

查看上面的文件，您会发现它只是创建了一个静态缓存，缓存名如下:`sw-precache-v3-sw-precache-webpack-plugin-+(selg.registration ? self.registration.scope)`。然后它缓存所有的静态文件，比如在缓存中的`index.html`和`js`和`css`文件。

您还应该看到一个`fetch`事件监听器，它捕捉一个获取事件，并检查应用程序是否正在请求一个先前缓存的静态资产。

现在来了一个百万美元的问题:如果你想为一个特定的路由配置一个动态缓存怎么办？本质上，当用户访问指定的路线时，缓存将使用从服务器发送的数据更新自身。请注意，这意味着数据在构建时将不可用，因此不能被生成的默认服务工作线程缓存。

### **CRA 违约公共福利援助的限制**

不幸的是，在使用 CRA 的时候，要做到这一点并不容易。当然，除非你愿意。

看看这些 GitHub 的问题，看看为什么 CRA 的团队不支持定制默认的服务人员。

[**自定义 ServiceWorker 配置问题# 2237 Facebook/create-react-App**](https://github.com/facebook/create-react-app/issues/2237)
[*1 . 0 . 0 增加了渐进式 Web App 支持，近期有可能支持自定义配置吗？我真的不想…*github.com](https://github.com/facebook/create-react-app/issues/2237)[**通过 piotr-cz Pull Request 在 Service Worker 中导入脚本# 2714 Facebook/create-react-app**](https://github.com/facebook/create-react-app/pull/2714)
[*此 PR 增加了使用 SWPrecacheWebpackPlugin 的 Import scripts 选项的能力。如何:创建一个名为…*github.com](https://github.com/facebook/create-react-app/pull/2714)的文件

那么，鉴于我们似乎无法定制默认的服务人员，我们该如何解决这个问题呢？

### **了解 CRA 如何培养服务人员**

找到构建系统解决方案的第一步是真正理解构建系统是如何工作的。

因此，让我们从构建系统用来生成服务工作者文件的[库](https://github.com/GoogleChromeLabs/sw-precache)开始。

`sw-precache`是一个库，允许您基于模板生成服务工作者文件。模板文件是使用下划线的模板引擎编写的。

[这里的](https://github.com/GoogleChromeLabs/sw-precache/blob/master/service-worker.tmpl)是`sw-precache`源代码中模板文件的链接。

同样，模板文件看起来很复杂，但是一旦你理解了模板语言，它就非常简单了。

因此，当您在 CRA shell 中运行构建过程时，与生成服务工作器相关的情况是:

1.  `sw-precache`库在内部执行
2.  选项对象被提供给`sw-precache`,以允许从模板生成服务工作者文件
3.  服务人员文件在名为`service-worker.js`的`build`文件夹中生成

### **覆盖默认服务工作者**

现在，我们如何覆盖上述过程以允许生成我们自己的定制服务工作者文件？

答案基于杰夫·波斯尼克(Jeff Posnick)`sw-precache`[stack overflow 答案](https://stackoverflow.com/questions/47636757/add-more-service-worker-functionality-with-create-react-app?rq=1)。

首先，我们需要在正常的构建过程之后运行`sw-precache` CLI。

通过运行以下命令安装`sw-precache`库:`npm install --save-dev sw-precache`

现在，`sw-precache`库在一个配置文件上运行，该文件是通过 CLI 上的一个选项提供的。这是命令:`sw-precache --config=sw-precache-config.js`，其中`sw-precache-config.js`是配置文件的名称。

下面是一个示例配置文件。

```
module.exports = {
  staticFileGlobs: [
    'build/static/css/**.css',
    'build/static/js/**.js'
  ],
  swFilePath: './build/service-worker.js',
  templateFilePath: './service-worker.tmpl',
  stripPrefix: 'build/',
  handleFetch: false,
  runtimeCaching: [{
    urlPattern: /this\\.is\\.a\\.regex/,
    handler: 'networkFirst'
  }]
}
```

**注意:**将 swFilePath 指定为`./build/service-worker.js`是很重要的，这样定制构建过程生成的服务工作器会覆盖 CRA 创建的服务工作器(在这个实例中，它们共享相同的名称)。否则，您将在您的构建目录中得到两个服务工作者文件！

在`sw-precache`的 [GitHub 页面](https://github.com/GoogleChromeLabs/sw-precache)上有大量关于对象属性以及可以分配给它们的有效值的文档。

特别感兴趣的是 runtimeCaching 选项，因为这是一个非常可扩展的解决方案，允许您为服务人员定义自定义规则来响应动态内容。

templateFilePath 是一个选项，用于在您希望 CLI 选取您的自定义服务工作程序模板文件时使用。但是您几乎总是要使用库本身提供的模板文件。

最后，我们需要提供脚本来通知 CRA 构建系统，我们希望生成我们的定制服务工作器。继续安装`sw-precache`库。

接下来，使用以下内容更新 package.json 构建脚本:

`build: react-scripts build && sw-precache --config=sw-precache-config.js`

一旦您使用`npm run build`运行构建过程，您就可以打开构建文件夹并查看您生成的服务工作者。

在有和没有定制服务工作者的情况下运行构建过程，并注意两者之间的差异。

### **结论**

虽然对于像定制服务人员这样简单的事情来说，这似乎是一种非常冗长的方法，但是这种方法的好处是将您牢牢地控制在 create-react-app shell 中。

还有其他方法来生成定制服务工作者(使用 [react-app-rewire](https://github.com/timarney/react-app-rewired) 和[工具箱](https://github.com/GoogleChrome/workbox)的组合)。我会试着把这种方法贴在帖子里。