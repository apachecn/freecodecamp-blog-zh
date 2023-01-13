# 你应该知道的 Chrome API 的特性

> 原文：<https://www.freecodecamp.org/news/features-of-the-chrome-api-you-should-know-bf5c8b6c7733/>

所以你认为你知道如何构建一个 Chrome 扩展？嗯，这很好，但是你听说过上下文菜单吗？脚本之间的消息传递？在您的扩展图标上添加徽章？如果这一切听起来很吸引人，那么你很幸运。我们将回顾 Chrome API 赋予我们的一些很酷的特性。

如果你有兴趣阅读如何构建一个 Chrome 扩展，你可以在这里阅读我之前的文章。如果你想知道如何出版一本，你可以在这里[阅读全部内容](https://medium.freecodecamp.org/chrome-extension-how-to-publish-dd8400a3d53)

### [上下文菜单](https://developer.chrome.com/extensions/contextMenus)

简而言之，上下文菜单是当你在浏览器中的任何地方右击时出现的菜单。您可以通过几个简单步骤将您的 Chrome 扩展添加到菜单中:

1.  将**上下文菜单**添加到清单中的**权限**键
2.  添加一个 16x16 的图标(因为它将在上下文菜单中使用)
3.  将以下代码添加到您的后台脚本中:

### [存储](https://developer.chrome.com/extensions/storage)

与 [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API#localStorage) 类似，Chrome API 允许将数据保存为对象，即使浏览器关闭并重新打开，这种保存也会持续。以下是在扩展中允许存储使用的必要步骤:

1.  将**存储**添加到清单中的**权限**键
2.  要将数据放入存储器，您可以使用:

3.要从您使用的存储中提取数据:

> ⚠️不把敏感的用户数据放在存储器中，因为它没有被加密

### [消息传递](https://developer.chrome.com/extensions/messaging#simple)

Chrome 还有一个很棒的特性，可以让你在脚本之间传递消息。例如，在您的扩展中，有一个 popup.js 文件来处理与弹出窗口相关的事情，还有一个后台脚本。如果您想让这两个脚本相互通信，您可以使用以下方法:

**SendMessage**

**监听收到的消息**

### 徽章

你知道他们，你爱他们，你可以把他们添加到你的扩展图标中。请务必注意，由于其尺寸较小，您要显示的文本仅限于四个字符***。***

若要设定您使用的徽章的背景颜色:

若要设定您使用的工卡的文本:

在这两种方法中，回调都是一个可选参数，在方法完成其操作后可以使用。

有其他你想了解的 Chrome APIs 吗？想问点什么？请随意联系。

如果你喜欢这篇文章，请鼓掌让其他人也能欣赏它！？