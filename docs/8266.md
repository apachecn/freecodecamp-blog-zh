# 使用 jQuery 和 Regex 的 JavaScript 客户端 web 抓取

> 原文：<https://www.freecodecamp.org/news/client-side-web-scraping-with-javascript-using-jquery-and-regex-5b57a271cb86/>

by Codemzy

# 使用 jQuery 和 Regex 的 JavaScript 客户端 web 抓取

![gcxvePjAjVax5nkRXLCABF4q5e-tJIUyTiZA](img/a5ad28ade950dbf92c7d8f09ebef932a.png)

当我在构建我的第一个开源项目 codeBadges 时，我认为从所有主要的代码学习网站获取用户档案数据会很容易。

我熟悉 API 调用和 get 请求。我想我可以使用 jQuery 从各种 API 中获取数据并使用它。

```
var name = 'codemzy';                $.get('https://api.github.com/users/' + name, function(response) {                      var followers = response.followers;});
```

嗯，那很简单。但事实证明，并不是每个网站都有一个公共 API，你可以从那里获取你想要的数据。

![zTDeTtBr0aKKlCARmCqeZ1fnRGso-ewAdAnd](img/d51b9cc90c325494771a6c1c1078bf19.png)

404: API not found

但是没有公共 API 不代表你需要放弃！你可以使用网络抓取来抓取数据，只需要**做一点额外的工作**。

让我们看看如何通过 JavaScript 使用客户端 web 抓取。

例如，我将从我的公共 freeCodeCamp 个人资料中获取我的用户信息。但是您可以在任何公共 HTML 页面上使用这些步骤。

抓取数据的第一步是使用 jQuery `.get`请求抓取整个页面 html。

```
var name = "codemzy";$.get('https://www.freecodecamp.com/' + name, function(response) {  console.log(response);});
```

厉害了，整页源代码刚登录控制台。

注意:如果你在这个阶段遇到了类似`No ‘Access-Control-Allow-Origin’ header is present on the requested resource` 的错误，不要担心。向下滚动到这篇文章的**不要让 CORS 阻止你**部分。

那很容易。使用 JavaScript 和 jQuery，上面的代码像浏览器一样从[www.freecodecamp.org](http://www.freecodecamp.org)请求一个页面。freeCodeCamp 用这个页面来回应。我们得到的是 HTML 代码，而不是浏览器运行代码来显示页面。

这就是网络抓取，从网站中提取数据。

好吧，响应并不像我们从 API 中得到的数据那样简洁。

![u8HicVak3E1qSTq9eYYtyTdHlbmylt6zkw4A](img/4a3af5bcc64bde98fa73c4b6ac7e7079.png)

但是…我们有数据，在那里的某个地方。

一旦我们有了源代码，我们需要的信息就在那里，我们只需要获取我们需要的数据！

我们可以通过搜索响应来找到我们需要的元素。

假设我们想从我们得到的用户档案响应中知道用户已经完成了多少挑战。

在撰写本文时，营员已完成的挑战被组织在用户档案的表格中。因此，要获得完成的挑战总数，我们可以计算行数。

一种方法是将整个响应包装在一个 jQuery 对象中，这样我们就可以使用类似于`.find()`的 jQuery 方法来获取数据。

```
// number of challenges completedvar challenges = $(response).find('tbody tr').length;
```

这很好——我们得到了正确的结果。但是这不是得到我们想要的结果的好方法。将响应转换成 jQuery 对象实际上加载了整个页面，包括来自该页面的所有外部脚本、字体和样式表……啊哈！

我们需要一些数据。我们真的不需要页面负载，当然也不需要随之而来的所有外部资源。

我们可以去掉脚本标签，然后通过 jQuery 运行其余的响应。为此，我们可以使用 Regex 在文本中查找脚本模式并删除它们。

或者更好的是，为什么不首先使用 Regex 来找到我们要找的东西呢？

```
// number of challenges completedvar challenges = response.replace(/<thead>[\s|\S]*?<\/thead>/g).match(/<tr>/g).length;
```

而且很管用！通过使用上面的 Regex 代码，我们去掉了表标题行(不包含任何挑战)，然后匹配所有的表行来计算完成的挑战的数量。

如果您想要的数据就在纯文本的响应中，那就更容易了。在写这篇文章的时候，html 中的用户点数就像`<h1 class=”flat-top text-primary”>[ 1498` </h1>一样等待被抓取。

```
var points = response.match(/<h1 class="flat-top text-primary">\[ ([\d]*?) \]<\/h1>/)[1];
```

在上面的正则表达式模式中，我们匹配我们正在寻找的 h1 元素，包括包围这些点的`[ ]`，并用`([\d]*?).`将里面的任何数字分组，我们得到一个数组，第一个`[0]`元素是整个匹配，第二个`[1]`是我们的分组匹配(我们的点)。

Regex 对于匹配字符串中的各种模式非常有用，对于搜索我们的响应以获得我们需要的数据也非常有用。

您可以使用相同的 3 步流程从各种网站收集个人资料:

1.  使用客户端 JavaScript
2.  使用 jQuery 抓取数据
3.  使用正则表达式来过滤相关信息的数据

直到我遇到一个问题，CORS。

![2uAuWzx-z3PkOAM5ESjcfsVdghMHXZStBlcu](img/bf95df8fb574b0f49bb72b43057baa92.png)

CORS: Access Denied

### 不要让 CORS 阻止你！

CORS 或跨源资源共享，可能是客户端 web 抓取的一个真正问题。

出于安全原因，浏览器限制从脚本内部发起跨源 HTTP 请求。因为我们在前端使用客户端 Javascript 进行网页抓取，所以可能会出现 CORS 错误。

这是一个试图从 CodeWars 中获取个人资料的例子…

```
var name = "codemzy";$.get('https://www.codewars.com/users/' + name, function(response) {  console.log(response);});
```

在撰写本文时，运行上述代码会产生一个与 CORS 相关的错误。

如果您刮擦的地方没有`Access-Control-Allow-Origin`割台，您可能会遇到问题。

坏消息是，您需要在服务器端运行这类请求来解决这个问题。

Whaaaaaaaat，这应该是客户端网络抓取？！

好消息是，由于许多其他优秀的开发人员也遇到了同样的问题，你不必自己去碰后端。

在我们的前端脚本中，我们可以使用跨域工具，比如[任意原点](http://anyorigin.com/)、[任意原点](http://www.whateverorigin.org/)、[所有原点](http://multiverso.me/AllOrigins/)、[交叉原点](https://crossorigin.me/)等等。我发现你经常需要测试其中的几个来找到一个能在你试图抓取的网站上工作的。

回到我们的 CodeWars 示例，我们可以通过跨域工具发送请求，绕过 CORS 问题。

```
var name = "codemzy";var url = "http://anyorigin.com/go?url=" + encodeURIComponent("https://www.codewars.com/users/") + name + "&callback=?";$.get(url, function(response) {  console.log(response);});
```

就像魔法一样，我们有我们的回应。

![mEAI00WFdz-ExqkAA2PYZKPsFusHpqn-yYXG](img/0b6fda1e8d0e44e3ee21e4ad8009856c.png)