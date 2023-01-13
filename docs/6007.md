# 如何实现 Chrome 扩展

> 原文：<https://www.freecodecamp.org/news/how-to-implement-a-chrome-extension-3802d63b5376/>

我们都喜欢网上冲浪。我们都喜欢触手可及的东西。为什么不创造出迎合这两个绝对真理的东西呢？

在本文中，我将解释 Chrome 扩展的构建模块。之后，你只需要想出一个好主意，作为创造一个好主意的借口。

### 为什么选择 Chrome？

Chrome 是目前最受欢迎的网络浏览器。[有的估计高达 **59%**](https://en.wikipedia.org/wiki/Usage_share_of_web_browsers) 。所以，如果你想接触尽可能多的人，开发 Chrome 扩展是最好的方法。

⚠️为了能够发布 Chrome 扩展，你需要有一个开发者账户，需要支付 5 美元的一次性注册费。

每个 Chrome 扩展应该有这些组件:清单文件、【popup.html】和**以及**和**后台**脚本。让我们来看看它们。

### Chrome 扩展由什么组成？

#### 清单文件

什么是清单文件？它是一个 JSON([JavaScript Object Notation](https://en.wikipedia.org/wiki/JSON))格式的文本文件，包含了关于您将要开发的扩展的某些细节。

当你发布你的扩展时，Google 使用这个文件来获取关于你的扩展的细节。有**必选**、**推荐**和**可选**字段。

#### 需要

```
{
  "manifest_version": 2,
  "name": "My Chrome Extension",
  "version": "1.0",
  "default_locale": "en"
}
```

*   `manifest_version` -版本的清单文件格式。从 Chrome 18 开始，版本 1 已被弃用
*   `name` -最多 45 个字符。用于在以下位置显示你的扩展名称:安装对话框，扩展管理界面，Chrome 网络商店
*   你的 Chrome 扩展版本。最多可以是由点分隔的四位数(例如，1.0.0.0)
*   `default_locale` -这个文件夹位于`_locals`文件夹中，它包含默认的字符串文字。`_locals`文件夹用于国际化(允许您的扩展支持多种语言)。如果`_locals`文件夹存在，则为必填字段，否则不应出现

如果你想支持多种语言，在这里阅读更多。

#### 被推荐的

```
 "description": "A plain text description",
  "author": "Your Name Here",
  "short_name": "shortName",
  "icons": {
      "128":"icon128.png",
       "48":"icon48.png",
       "16":"icon16.png"
    },
```

*   `description` -您可以使用最多 132 个字符来描述扩展名
*   `short_name` -限于 12 个字符，用于没有足够空间显示扩展的全名(应用程序启动器和新标签页)的情况
*   `icons` -代表扩展名的图标。**总是包含一个 128X128 的图标**，因为它被 Chrome 网上商店和你的扩展安装过程中使用

可选字段是特定于案例的，所以我们不会在本文中深入讨论它们。

在介绍了清单文件所需的数据之后，我们现在可以继续为我们的扩展、**弹出窗口和背景**编写代码了。

#### 弹出窗口和背景

弹出窗口指的是用户在使用您的扩展时看到的主页面。它由两个文件组成:**Popup.html**和一个 JavaScript 文件， **Popup.js** 。

Popup.html 是你的扩展外观的布局文件。根据您的扩展将做什么，这个文件的标记将会改变。

后台脚本是唯一可以与发生的事件交互并使用 Chrome API 的脚本。要使用后台脚本，您需要将以下内容添加到 manifest.json 文件中:

```
{
  "manifest_version": 2,
  "name": "My Chrome Extension",
  "version": "1.0",
  "background":{
    	"scripts": ["yourScript.js"],
    	"persistent": false
    }
}
```

键`scripts`有一个脚本名数组的值。

`persistent`是一个带有布尔值的键，表示使用 chrome.webRequest API 的扩展来阻止或修改网络请求。Chrome.webRequest API 不适用于非持久背景页面。

![0*mFgdmSgmiXKQmhZ_](img/2f1be8a0fbb88356fe3298a65a63f2e7.png)

“open signage on door” by [Artem Bali](https://unsplash.com/@belart84?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 您的扩展将如何打开

每个 Chrome 扩展都会在浏览器顶部的工具栏上添加一个小图标。一旦用户单击该图标，您就可以选择希望扩展在浏览器中的打开方式:

1.  它可以覆盖一个新的标签，以便不中断当前用户的活动

2.您可以在用户的当前选项卡中打开一个小窗口，从而使用户保持在同一个选项卡中

每个选择都有其后果，由你来决定什么是对你最好的选择。

下面是实现每个选项所需的代码。它们都将使用下面列出的同一个 popup.html 文件:

```
<html>

	<head>

		<title>Chrome Extension Example</title>
	</head>

	<body>

		<h1>Hello From Extension</h1>

	</body>

</html>
```

### 把所有的放在一起

#### 覆盖新标签

```
//In manifest.json
{
    "name": "ChromeExampleNewTab",
    "version": "1.0",
    "manifest_version": 2,
    "chrome_url_overrides": {
    	"newtab": "popup.html"
    },
    "browser_action": {}, 
    "permissions":[        
    	"tabs"
    ],
    "background":{        
    	"scripts": ["background.js"],
    	"persistent": false
    }
}

//In background.js
chrome.browserAction.onClicked.addListener(function(tab) {
	chrome.tabs.create({'url': chrome.extension.getURL('popup.html')}, function(tab) {
		// Tab opened.
	});
});
```

#### 在当前标签页中打开

```
//In manifest.js
{
    "name": "ChromeExample",
    "version": "1.0",
    "manifest_version": 2,
    "browser_action": {         
      "default_popup": "popup.html"
    }
}
```

注意我们在两个例子中都覆盖了`browser_action`键。

我们必须这样做，因为我们不希望浏览器以常规方式打开一个新标签。

此外，因为如果我们想用我们的扩展打开一个新的选项卡，我们必须向清单添加一个 permissions 键并指定 tabs 值。这让浏览器知道我们需要用户的许可来覆盖打开新标签页的默认行为。

Chrome 扩展还有很多功能(消息、上下文菜单和存储等等)。我希望给你一些关于 Chrome 扩展的见解。也许足够让你自己做一个了！

当你在做的时候，检查一下我在这里做的。