# 如何瞒过微服拦截

> 原文：<https://www.freecodecamp.org/news/disabling-browser-incognito-check-cc84288e89b3/>

依母胎

# 如何瞒过微服拦截

![1*BgAlYI-fuv9BrHQHPnNZoQ](img/b89aa549fa2f09fdcbd06810d0cd1b56.png)

最近我遇到了几个显示警告或付费墙的网站，因为我使用的是匿名模式。我认为这不公平。我应该被允许使用任何我想要的浏览器和模式。这是加强他们追踪工具的一种方式。我知道隐姓埋名不安全，但这是避免广告追踪你的最低限度。

整个过程花了我大约两个小时，我学到了很多关于浏览器扩展和作为最终用户入侵客户端代码的知识。我想这可能值得分享。

首先，我必须查找如何检测隐私模式。据我所知，没有浏览器 API 可以直接检测隐私模式，所以我很确定这是一个偷偷摸摸的小脚本。[这个](https://stackoverflow.com/a/27805491/2419215) StackOverflow 的回答给了我一个提示，所以我知道我得去找找`webkitRequestFileSystem`。我在一个私人厌恶网站的迷你 JavaScript 代码中发现了[这个](https://gist.github.com/matyasfodor/15e8863ab15baf4791a5fa4c748b64af)位。激动人心的部分来了:

我可以通过将模块粘贴到匿名和公共浏览器窗口开发控制台来测试它，并运行:

```
var module = {};incognito(null, module);module.exports.detectIncognito().then(console.log)
```

答对了。就是这样，我只要想办法不调用`window.webkitRequestFileSystem(..)`里的错误回调就行了。最简单的方法是给函数打上猴子补丁:

```
(function(webkitRequestFileSystem) {  window.webkitRequestFileSystem = function(t, s, success, error) {    webkitRequestFileSystem(t, s, success, success);  }})(window.webkitRequestFileSystem);
```

如果你不熟悉这种技术， [monkey patching](https://www.audero.it/blog/2016/12/05/monkey-patching-javascript/) 是一种在运行时添加、修改或抑制一段代码的默认行为的方法，而不改变其原始源代码。

**迂回 1:** 首先我开始用 [Extensionizr](https://extensionizr.com) 编写自己的 chrome 扩展。这是一个生成 Chrome 扩展样板代码的好工具。但最终，我找到了一个更简单的解决方案。

每当谈到定制网站，我都使用 [Tampermonkey](https://tampermonkey.net/) (例如[在堆栈溢出时隐藏招聘广告](https://gist.github.com/rjrudman/a472924d3fb078bd73bb12066e0319a0)，而我真的不应该花时间寻找新的职位)。您不必安装第 n+1 个扩展，它提供了一个很好的界面来管理您的脚本。好吧，nice 大概是夸张了吧，很丑但是很好用。

所以我添加了我上面提到的猴子补丁脚本，已经咯咯地笑着说它是多么容易，但令人失望的是，它不起作用。我尝试了一些其他的事情，例如:

```
window.foobar = 'baz';
```

但是在开发控制台中，`window`变量中没有这个属性。原来[内容脚本](https://stackoverflow.com/a/20513730/2419215)运行在一个隔离的环境中，它们只与网页脚本共享 DOM。我开始使用 SO 中引用的解决方案。但是有一件非常重要的事情，我必须在当前页面的代码之前执行这段代码。这是我想到的:

```
function injectScript(file_path, node) {        var element = document.createElement('script');        element.setAttribute('type', 'text/javascript');        element.setAttribute('src', file_path);        element.setAttribute('async', false);        node.appendChild(element);}injectScript(url, document.documentElement);
```

**迂回 2:** 当我从一个扩展开始时，加载另一个 JavaScript 文件是微不足道的。然而，Tampermonkey 脚本却不是这样(至少我不知道)。所以我决定将我的代码放在 GitHub gist 中，并尝试加载[原始文件](https://gist.githubusercontent.com/matyasfodor/ab6c92e32a35ebae0bebedff8e7cf569/raw/4f97a8fb702ae8710ba9542b5a7a8127495cf9e4/fakepublic.js)。但是后来浏览器抱怨它的 MIME 类型。最后，我最终使用了 https://rawgit.com/的，这正是合适的工具。

我意识到我应该将这几行 monkey 补丁代码作为一个字符串添加进来，并用它填充脚本元素的文本。这是我最后的解决方案:

如果你在匿名模式下使用 Tampermonkey，有一点很重要:你在匿名模式下所做的更改不会出现在正常模式下，反之亦然:如果你想尝试在公共模式下所做的最新更改，你必须关闭所有私人窗口。

**小心！如果你决定使用我的脚本，你必须知道这个*可能会*(虽然不太可能)破坏一些网页。在坦帕蒙基，你可以随时关闭这些功能。使用时风险自担。**