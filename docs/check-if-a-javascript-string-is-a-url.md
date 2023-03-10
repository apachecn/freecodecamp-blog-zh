# 如何检查 JavaScript 字符串是否是有效的 URL

> 原文：<https://www.freecodecamp.org/news/check-if-a-javascript-string-is-a-url/>

URL——或统一资源定位符——是用于识别互联网上的网页、图像和视频等资源的文本。

我们通常将 URL 称为网站地址，它们用于文件传输、电子邮件和其他应用程序。

URL 由多个部分组成——协议、域名等——告诉浏览器如何以及在哪里检索资源。

在 JavaScript 中，您可能需要在锚标签或按钮中使用 URL 来将用户链接到另一个网页。在这种情况下，必须验证该 URL 字符串，以确保它是有效的 URL。

本教程将教你一些方法来检查一个 JavaScript 字符串是否是一个有效的 URL。

要了解如何在 JavaScript 中获取当前 URL，您可以阅读这篇关于如何在 JavaScript 中获取当前 URL 的文章。

## 如何使用正则表达式检查一个字符串是否是有效的 URL

正则表达式(regex)是匹配 JavaScript 字符串中字符组合的模式。在 JavaScript 中，[正则表达式](https://www.freecodecamp.org/news/a-quick-and-simple-guide-to-javascript-regular-expressions-48b46a68df29/)也被称为提供不同方法来执行各种操作的对象。

有两种方法可以构造正则表达式:

*   使用正则表达式文字
*   使用正则表达式构造函数

**注意:**当您只想检查一个字符串是否是有效的 URL，并且不想创建任何其他额外的对象时，使用正则表达式方法是合适的。

让我们来学习这两种方法是如何工作的。

### 如何使用正则表达式文字

在正则表达式文本中，模式被括在斜线之间，如下所示。

该模式包括`URL`中所需部件的验证。例如，一个协议、`https`、一个`//`等等。

```
const urlPattern = /(?:https?):\/\/(\w+:?\w*)?(\S+)(:\d+)?(\/|\/([\w#!:.?+=&%!\-\/]))?/; 
```

### 如何使用正则表达式构造函数

要使用构造方法创建正则表达式，使用`RegExp()`构造函数并将模式作为参数传递。

```
const urlPattern = new RegExp('(?:https?):\/\/(\w+:?\w*)?(\S+)(:\d+)?(\/|\/([\w#!:.?+=&%!\-\/]))?'); 
```

为了演示如何验证一个字符串是否是一个`URL`，让我们创建一个方法，使用正则表达式构造函数验证一个 JavaScript `String`，并基于匹配的模式返回`True`或`False`。

```
 const isValidUrl = urlString=> {
	  	var urlPattern = new RegExp('^(https?:\\/\\/)?'+ // validate protocol
	    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|'+ // validate domain name
	    '((\\d{1,3}\\.){3}\\d{1,3}))'+ // validate OR ip (v4) address
	    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*'+ // validate port and path
	    '(\\?[;&a-z\\d%_.~+=-]*)?'+ // validate query string
	    '(\\#[-a-z\\d_]*)?

### 如何使用正则表达式验证 URL 字符串

下面的代码演示了如何使用上述方法验证不同的 URL 字符串:

```
 var url = "invalidURL";
	console.log(isValidUrl(url));      //false

	var url = "htt//jsowl";            //false
	console.log(isValidUrl(url));

    var url = "www.jsowl.com";         //true
    console.log(isValidUrl(url));

    var url = "https://www.jsowl.com"; //true
    console.log(isValidUrl(url));

    var url = "https://www.jsowl.com/remove-an-item-from-an-array-in-javascript/";
    console.log(isValidUrl(url));      //true 
```

## 如何使用 URL 构造函数检查字符串是否是有效的 URL

您可以使用 URLConstructor 来检查字符串是否是有效的 URL。

[URLConstructor](https://developer.mozilla.org/en-US/docs/Web/API/URL) ( `new URL(url)`)返回由 URL 参数定义的新创建的 URL 对象。

如果给定的 URL 无效，则抛出 JavaScript [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 异常。

**注意:**当你想在你的程序中构造一个 URL 对象以备后用时，使用这个方法是合适的。

### URL 构造函数语法

以下语法解释了如何用 JavaScript 字符串创建 URL 对象。

```
new URL(url);
new URL(url, base); 
```

在哪里，

*   `url`是一个字符串或任何带有代表绝对或相对 URL 的[字符串符](https://developer.mozilla.org/en-US/docs/Glossary/Stringifier)的对象。如果 **URL** 是绝对 URL，则 **base** 将被忽略。如果 **URL** 是相对 URL，则需要 **base** 。
*   `base`(可选)是表示基本 URL 的字符串。当 URL 是相对的时，必须传递它。忽略时默认为*未定义的*。

### URL 构造函数方法的示例

为了演示 URL 构造函数方法是如何工作的，让我们用 JavaScript 创建一个 lambda 函数，用传递的字符串构造一个新的 URL。

*   如果该字符串是有效的 URL，则创建一个 URL 对象，并返回`true`
*   如果该字符串不是有效的 URL，则抛出`Tyeperror`异常，并返回`false`

```
const isValidUrl = urlString=> {
      try { 
      	return Boolean(new URL(urlString)); 
      }
      catch(e){ 
      	return false; 
      }
  } 
```

### 如何使用`isValidURL()`方法

让我们为不同的字符串类型调用`isValidURL()`方法，看看结果。

```
 var url = "invalidURL";
  console.log(isValidUrl(url));     //false

  var url = "htt//jsowl";
  console.log(isValidUrl(url));     //false

  var url = "www.jsowl.com";
  console.log(isValidUrl(url));     //false

  var url = "tcp://www.jsowl.com";
  console.log(isValidUrl(url));     //true

  var url = "https://www.jsowl.com/remove-an-item-from-an-array-in-javascript/";
  console.log(isValidUrl(url));     //true 
```

在前三种情况下，您可以看到*传递了一个无效的 URL 字符串*。结果，URL 对象创建失败，返回一个`TypeError`和`false`。

在最后两种情况下，*有效的 URL 字符串*被传递。因此成功创建了一个`URL`对象，并返回了`True`，确认了正确的 URL。

让我们再看一个验证特定 URL 部分的例子。

在本例中，您正在验证 URL 中的特定协议。URL 必须包含`http`或`https`协议。

```
 const isValidUrl = urlString=> {
		let url;
		try { 
	      	url =new URL(urlString); 
	    }
	    catch(e){ 
	      return false; 
	    }
	    return url.protocol === "http:" || url.protocol === "https:";
	} 
```

### 如何验证部分 URL 的示例

让我们为不同的字符串类型和协议调用`isValidURL()`方法，看看结果。

```
var url = "tcp://www.jsowl.com";
console.log(isValidUrl(url));      //false

var url = "https://www.jsowl.com";
console.log(isValidUrl(url));      //true 
```

第一种情况，URL 字符串 *(tcp://www.jsowl.com)* 是有效的，但是不包含特定的协议(`HTTP` / `HTTPS`)。所以它返回*假*。

在第二种情况下，URL 字符串*[【https://www.jsowl.com】](https://www.jsowl.com)*是*有效的*，并且包含特定的协议。所以它返回*真*。

## 如何使用 Input 元素检查字符串是否是有效的 URL

HTML 支持类型为 [`url`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/url) 的输入元素，专门用于表示 URL 值。

在提交表单之前，包含字符串的`<input>`元素的`value`属性通过匹配 URL 语法(*具有空的或格式正确的 URL* )被自动验证。

[`HTMLInputElement.checkValidity()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/checkValidity) 方法用于检查`<input>`元素的 value 属性中的字符串是否为`URL`。如果值是正确的 URL，则`checkvalidity()`方法返回`true`，如果输入不是正确的 URL，则返回`false`。

让我们创建一个方法，该方法创建一个输入元素类型`URL`并使用`checkValidity()`方法验证输入。

```
 const isValidUrl = urlString =>{
      var inputElement = document.createElement('input');
      inputElement.type = 'url';
      inputElement.value = urlString;

      if (!inputElement.checkValidity()) {
        return false;
      } else {
        return true;
      }
    } 
```

现在让我们使用这个方法并验证不同的字符串，看看它们是否是有效的 URL。

```
 var url = "invalidURL";
    console.log(isValidUrl(url));     //false

    var url = "htt//jsowl";
    console.log(isValidUrl(url));     //false

    var url = "www.jsowl.com";
    console.log(isValidUrl(url));     //false

    var url = "https://www.jsowl.com";
    console.log(isValidUrl(url));     //true

    var url = "https://www.jsowl.com/remove-an-item-from-an-array-in-javascript/";
    console.log(isValidUrl(url));     //true 
```

这就是如何使用输入类型方法来检查一个字符串是否是有效的 URL。

## 如何使用锚标记方法检查字符串是否是有效的 URL

本节教你如何使用 [HTMLAnchorElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement) 来检查一个 JavaScript 字符串是否是一个 URL。

**注意:**当你想给你的网页的`anchor`标签分配一个 URL，并确保 URL 字符串有效并被正确分配给`anchor`标签时，使用这个方法是合适的。

`HTMLAnchorElement`界面表示超链接元素。它为操作这些元素的布局和表示提供了特殊的属性和方法。它也被称为锚标记。

您可以使用`href`属性将 URL 分配给锚标记。分配时，

*   如果传递了有效的 URL 字符串，它将被分配给锚标记
*   如果一个无效的 URL 被传递，当前浏览器位置被分配给锚标签
*   默认情况下，锚标记将有一个空的 URL(" ")

一旦分配了 URL，您可以使用下面解释的属性提取 URL 的特定部分。

| HTMLAnchorElement 属性 | 使用 |
| --- | --- |
| `host` | 代表主机名和端口的字符串 |
| `hostname` | 代表主机名的字符串 |
| `href` | 包含有效 URL 的字符串 |
| `origin` | 返回一个包含原点、其模式、域名和端口的字符串 |
| `port` | 表示端口的字符串(如果指定) |
| `protocol` | 代表协议的字符串，包括结尾(`:`') |
| `pathname` | 包含起始(/)的路径 URL 的字符串，不包括查询字符串 |

现在，让我们看看如何检查分配的字符串是否是正确的 URL。

如果它是一个正确的 URL，它将被分配给锚标记。否则，当前浏览器位置将被分配给锚标记。

因此，要检查它是否是一个正确的 URL，您可以使用语句`a.host != window.location.host`检查锚标记的`host`是否不等于当前位置。

让我们看看代码。

我们创建一个 lambda 函数，并将其赋给下面代码中的常量`isValidUrl`。

该函数创建一个 anchor 标记元素，并将 URL 字符串分配给 anchor 标记。之后，它检查元素的`host`属性是`null`还是未定义。

如果不为空，则检查`host`属性是否不等于当前浏览器 URL，如果不等于则返回`True`。

这是因为如果传递的 URL 是有效的，那么锚标记将具有 URL 值。但是如果传递的 URL 无效，锚标记将具有当前的浏览器位置。在这种情况下，lambda 函数返回`False`。

```
const isValidUrl = urlString =>{	
  	var a  = document.createElement('a');
   	a.href = urlString;
   	return (a.host && a.host != window.location.host);
  } 
```

下面的代码片段使用不同的输入调用 lambda 函数`isValidUrl()`，并在控制台中相应地打印输出。

```
 var url = "invalidURL";
  console.log("1.AnchorTag:  " +isValidUrl(url));    //false

  var url = "htt//jsowl";
  console.log("22.AnchorTag:  "+isValidUrl(url));    //false

  var url = "www.jsowl.com";  
  console.log("3.AnchorTag:  " +isValidUrl(url));    //false  

  var url = "https://www.jsowl.com";  
  console.log("4.AnchorTag:  " +isValidUrl(url));    //true 

  var url = "https://www.jsowl.com/remove-an-item-from-an-array-in-javascript/";
  console.log("5.AnchorTag:  " +isValidUrl(url));    //true 
```

本教程可在[this](https://jsfiddle.net/jsowl/mvzqh4of/266/)jsdild 中获得。

## 结论

在本文中，您已经学习了如何使用不同的方法检查 JavaScript 字符串是否为`URL`，以及何时适合使用每种方法。

如果你喜欢这篇文章，请随意分享。

你可以在我的博客 JS Owl 上查看我的其他教程。,'i'); // validate fragment locator
	  return !!urlPattern.test(urlString);
	} 
```

### 如何使用正则表达式验证 URL 字符串

下面的代码演示了如何使用上述方法验证不同的 URL 字符串:

[PRE3]

## 如何使用 URL 构造函数检查字符串是否是有效的 URL

您可以使用 URLConstructor 来检查字符串是否是有效的 URL。

[URLConstructor](https://developer.mozilla.org/en-US/docs/Web/API/URL) ( `new URL(url)`)返回由 URL 参数定义的新创建的 URL 对象。

如果给定的 URL 无效，则抛出 JavaScript [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 异常。

**注意:**当你想在你的程序中构造一个 URL 对象以备后用时，使用这个方法是合适的。

### URL 构造函数语法

以下语法解释了如何用 JavaScript 字符串创建 URL 对象。

[PRE4]

在哪里，

*   `url`是一个字符串或任何带有代表绝对或相对 URL 的[字符串符](https://developer.mozilla.org/en-US/docs/Glossary/Stringifier)的对象。如果 **URL** 是绝对 URL，则 **base** 将被忽略。如果 **URL** 是相对 URL，则需要 **base** 。
*   `base`(可选)是表示基本 URL 的字符串。当 URL 是相对的时，必须传递它。忽略时默认为*未定义的*。

### URL 构造函数方法的示例

为了演示 URL 构造函数方法是如何工作的，让我们用 JavaScript 创建一个 lambda 函数，用传递的字符串构造一个新的 URL。

*   如果该字符串是有效的 URL，则创建一个 URL 对象，并返回`true`
*   如果该字符串不是有效的 URL，则抛出`Tyeperror`异常，并返回`false`

[PRE5]

### 如何使用`isValidURL()`方法

让我们为不同的字符串类型调用`isValidURL()`方法，看看结果。

[PRE6]

在前三种情况下，您可以看到*传递了一个无效的 URL 字符串*。结果，URL 对象创建失败，返回一个`TypeError`和`false`。

在最后两种情况下，*有效的 URL 字符串*被传递。因此成功创建了一个`URL`对象，并返回了`True`，确认了正确的 URL。

让我们再看一个验证特定 URL 部分的例子。

在本例中，您正在验证 URL 中的特定协议。URL 必须包含`http`或`https`协议。

[PRE7]

### 如何验证部分 URL 的示例

让我们为不同的字符串类型和协议调用`isValidURL()`方法，看看结果。

[PRE8]

第一种情况，URL 字符串 *(tcp://www.jsowl.com)* 是有效的，但是不包含特定的协议(`HTTP` / `HTTPS`)。所以它返回*假*。

在第二种情况下，URL 字符串*[【https://www.jsowl.com】](https://www.jsowl.com)*是*有效的*，并且包含特定的协议。所以它返回*真*。

## 如何使用 Input 元素检查字符串是否是有效的 URL

HTML 支持类型为 [`url`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/url) 的输入元素，专门用于表示 URL 值。

在提交表单之前，包含字符串的`<input>`元素的`value`属性通过匹配 URL 语法(*具有空的或格式正确的 URL* )被自动验证。

[`HTMLInputElement.checkValidity()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/checkValidity) 方法用于检查`<input>`元素的 value 属性中的字符串是否为`URL`。如果值是正确的 URL，则`checkvalidity()`方法返回`true`，如果输入不是正确的 URL，则返回`false`。

让我们创建一个方法，该方法创建一个输入元素类型`URL`并使用`checkValidity()`方法验证输入。

[PRE9]

现在让我们使用这个方法并验证不同的字符串，看看它们是否是有效的 URL。

[PRE10]

这就是如何使用输入类型方法来检查一个字符串是否是有效的 URL。

## 如何使用锚标记方法检查字符串是否是有效的 URL

本节教你如何使用 [HTMLAnchorElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement) 来检查一个 JavaScript 字符串是否是一个 URL。

**注意:**当你想给你的网页的`anchor`标签分配一个 URL，并确保 URL 字符串有效并被正确分配给`anchor`标签时，使用这个方法是合适的。

`HTMLAnchorElement`界面表示超链接元素。它为操作这些元素的布局和表示提供了特殊的属性和方法。它也被称为锚标记。

您可以使用`href`属性将 URL 分配给锚标记。分配时，

*   如果传递了有效的 URL 字符串，它将被分配给锚标记
*   如果一个无效的 URL 被传递，当前浏览器位置被分配给锚标签
*   默认情况下，锚标记将有一个空的 URL(" ")

一旦分配了 URL，您可以使用下面解释的属性提取 URL 的特定部分。

| HTMLAnchorElement 属性 | 使用 |
| --- | --- |
| `host` | 代表主机名和端口的字符串 |
| `hostname` | 代表主机名的字符串 |
| `href` | 包含有效 URL 的字符串 |
| `origin` | 返回一个包含原点、其模式、域名和端口的字符串 |
| `port` | 表示端口的字符串(如果指定) |
| `protocol` | 代表协议的字符串，包括结尾(`:`') |
| `pathname` | 包含起始(/)的路径 URL 的字符串，不包括查询字符串 |

现在，让我们看看如何检查分配的字符串是否是正确的 URL。

如果它是一个正确的 URL，它将被分配给锚标记。否则，当前浏览器位置将被分配给锚标记。

因此，要检查它是否是一个正确的 URL，您可以使用语句`a.host != window.location.host`检查锚标记的`host`是否不等于当前位置。

让我们看看代码。

我们创建一个 lambda 函数，并将其赋给下面代码中的常量`isValidUrl`。

该函数创建一个 anchor 标记元素，并将 URL 字符串分配给 anchor 标记。之后，它检查元素的`host`属性是`null`还是未定义。

如果不为空，则检查`host`属性是否不等于当前浏览器 URL，如果不等于则返回`True`。

这是因为如果传递的 URL 是有效的，那么锚标记将具有 URL 值。但是如果传递的 URL 无效，锚标记将具有当前的浏览器位置。在这种情况下，lambda 函数返回`False`。

[PRE11]

下面的代码片段使用不同的输入调用 lambda 函数`isValidUrl()`，并在控制台中相应地打印输出。

[PRE12]

本教程可在[this](https://jsfiddle.net/jsowl/mvzqh4of/266/)jsdild 中获得。

## 结论

在本文中，您已经学习了如何使用不同的方法检查 JavaScript 字符串是否为`URL`，以及何时适合使用每种方法。

如果你喜欢这篇文章，请随意分享。

你可以在我的博客 JS Owl 上查看我的其他教程。