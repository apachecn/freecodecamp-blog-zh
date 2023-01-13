# 如何使用 create-react-app 定制服务人员

> 原文：<https://www.freecodecamp.org/news/how-to-customize-service-workers-with-create-react-app-4424dda6210c/>

这是我之前关于用 create-react-app (CRA)构建 PWA 的帖子的后续。在链接的帖子中，我讨论了我们如何在 create-react-app shell 中构建一个定制服务工作器(SW)。

如果你一直关注这篇文章(希望它能正常工作)，你会注意到一个严重的缺陷。在开发环境中开发一个软件仍然非常困难。本质上，你必须修改你的软件代码，运行一个构建过程，测试它，排除任何错误，刷新并重复。从经验来说，这是一个令人筋疲力尽的过程。

让我们继续研究如何解决那个问题。

### **在开发模式下工作**

好的，那么我们如何让一个软件在开发模式下运行，这样我们就可以快速地编写一些糟糕的代码，并找出哪些可以，哪些不可以？

首先，让我们弄清楚为什么它在 dev 模式下不起作用。创建一个新的 CRA 项目，打开`src`目录下的`registerServiceWorker.js`。

在上面的要点中，我只有相关的代码。您会注意到一个条件检查`process.env.NODE_ENV === 'production'`。这将检查您是否正在运行生产版本。如果您运行的不是生产版本，SW 将被一个无操作文件替换。

这个决定背后的理由在这个 [GitHub 问题](https://github.com/facebook/create-react-app/issues/2396)中提供。

首先，尝试在你的应用上运行`yarn start`,并在工具栏窗口中检查一个 SW 文件。如果您点击工具栏中的`service-worker.js`链接，您将看到以下文件:

幸运的是，有一个简单的修复方法。这是一个简单的两步过程。

首先，在`registerServiceWorker.js`文件中，查找`window.addEventListener('load')`函数调用。第一行是`swUrl`的声明，上面写着:

```
const swUrl = `${process.env.PUBLIC_URL}/service-worker.js`;
```

用其他名字重命名它的`service-worker`部分。我要给我的取名`service-worker-custom.js`。

其次，在你的公共目录中创建一个文件，它的名字**与你刚刚想出的自定义名字**完全相同。因此，我将在公共目录中创建一个名为`service-worker-custom.js`的文件。

现在，在`service-worker-custom.js`中，放置一个简单的日志语句。大概是:`console.log('My custom service worker')`。

现在，使用`yarn start`再次运行您的应用程序，您应该会看到日志语句在您的浏览器控制台中弹出。如果您在此之前运行过 yarn start，您可能需要注销以前的服务人员。

所以，你有它。您可以在开发模式下安全运行的定制服务工作器。

**注意:在浏览器的私人浏览模式之外的开发环境中测试服务人员是不明智的。此外，当在开发模式下测试时，一定要确保在开发工具窗口中选择了重新加载时更新。**

### **结合开发和生产**

现在，我们已经知道如何在开发模式下测试软件。然而，我们还需要找到一种方法，将我们的定制代码注入到生产版本中由 CRA 生成的软件中。

如果您保持我们到目前为止所做的配置，运行构建过程并在浏览器中检查构建，您会注意到生成的 SW 文件是我们创建的自定义文件。这是一个问题，因为我们希望能够将 CRA 提供给我们的优点与我们自己的代码结合起来。

我们可以用`sw-precache`库做到这一点。我在我的[上一篇文章](https://medium.freecodecamp.org/how-to-build-a-pwa-with-create-react-app-and-custom-service-workers-376bd1fdc6d3)中介绍了这个库。这里是 [GitHub 链接](https://github.com/GoogleChromeLabs/sw-precache)到`sw-precache`库。

用`yarn add sw-precache`安装库。完成后，在根目录下创建一个`sw-precache-config.js`文件。这是我的文件:

我已经在[的上一篇文章](https://medium.freecodecamp.org/how-to-build-a-pwa-with-create-react-app-and-custom-service-workers-376bd1fdc6d3)中介绍了这个文件的大部分内容。唯一的新位是`importScripts`选项。这是不言自明的，它只是导入路径指定的文件，我们正在尝试导入我们的自定义 SW 文件。

您会注意到文件的路径缺少前缀`./public`，尽管文件存在于我们的`public`目录中。我稍后会解释这一点。

现在，用对`build`命令的修改来更新您的`package.json`文件。让你的`build`命令如下:

`react-scripts build && sw-precache --config=sw-precache-config.js`

现在，让我们回到我们提供给 importScripts 选项的文件路径。如果您注意到了，`sw-precache`实际上是作为一个后构建过程运行的。现在，如果您只是运行一个构建过程，并打开所创建的构建目录，您将会注意到构建文件夹中的自定义服务工作器文件。当我们提供到`importScripts`选项的路径时，我们提供的是相对于构建目录的路径！

一旦你完成了所有这些，继续运行你的应用程序的构建版本，你会注意到日志语句再次在你的浏览器控制台中弹出。

好了，现在你知道了！您现在可以将一些自定义软件代码注入到由 CRA 生成的默认软件中！