# 客户端与服务器端渲染:为什么不全是黑白的

> 原文：<https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/>

自古以来，将 HTML 显示在屏幕上的传统方法是使用服务器端呈现。这是唯一的办法。你装上了你的。html 页面，然后服务器将它们转换成用户浏览器上的有用文档。

服务器端渲染在当时也非常有效，因为大多数网页大多只是显示静态图像和文本，几乎没有交互性。

快进到今天，情况不再是这样了。你可能会说，如今的网站更像是伪装成网站的应用程序。你可以用它们来发送信息、更新在线信息、购物等等。网络只是比过去先进了许多。

因此，服务器端呈现慢慢开始让位于日益增长的在客户端呈现网页的方法是有道理的。

那么哪种方法是更好的选择呢？和大多数开发中的事情一样，这真的取决于你打算用你的网站做什么。你需要了解利弊，然后自己决定哪条路线最适合你。

### **服务器端渲染如何工作**

服务器端呈现是在屏幕上显示信息的最常见方法。它的工作原理是将服务器中的 HTML 文件转换成浏览器可用的信息。

每当您访问网站时，您的浏览器都会向包含网站内容的服务器发出请求。该请求通常只需要几毫秒，但最终取决于多种因素:

*   你的网速
*   服务器的位置
*   有多少用户试图访问该网站
*   以及网站的优化程度等等

一旦请求处理完毕，您的浏览器就会获得完全呈现的 HTML 并将其显示在屏幕上。如果您随后决定访问网站上的不同页面，您的浏览器将再次请求新信息。每当您访问浏览器没有缓存版本的页面时，就会出现这种情况。

如果新页面只有几项与当前页面不同，这并不重要，浏览器会要求整个新页面，并从头开始重新呈现所有内容。

以这个 HTML 文档为例，它被放在一个 HTTP 地址为`example.testsite.com`的虚拟服务器中。

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Example Website</title>
  </head>
  <body>
    <h1>My Website</h1>
    <p>This is an example of my new website</p>
    <a href="http://example.testsite.com/other.html.">Link</a>
  </body>
</html>
```

如果您将示例网站的地址输入到虚拟浏览器的 URL 中，您的虚拟浏览器将向该 URL 使用的服务器发出请求，并期待一些文本的响应呈现在浏览器上。在这种情况下，你可以看到标题、段落内容和链接。

现在，假设您想要单击包含以下代码的呈现页面中的链接。

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Example Website</title>
  </head>
  <body>
    <h1>My Website</h1>
    <p>This is an example of my new website</p>
    <p>This is some more content from the other.html</p>
  </body>
</html>
```

前一页和这一页的唯一区别是，这一页没有链接，而是有另一个段落。从逻辑上讲，只有新的内容应该被呈现，其余的内容应该被保留。唉，这不是服务器端渲染的工作方式。实际发生的情况是，整个新页面将被呈现，而不仅仅是新内容。

虽然这两个例子看起来没什么大不了的，但大多数网站并没有这么简单。现代网站有数百行代码，复杂得多。现在想象一下，浏览一个网页，并且在浏览网站时必须等待每一页的渲染。如果你曾经访问过 WordPress 网站，你会看到它们有多慢。这是原因之一。

从好的方面来看，服务器端渲染对 SEO 来说非常好。你的内容在你得到它之前就存在了，所以搜索引擎能够很好地索引和抓取它。客户端渲染就不是这样了。至少不简单。

### **客户端渲染如何工作**

当开发人员谈论客户端呈现时，他们谈论的是使用 JavaScript 在浏览器中呈现内容。因此，您不是从 HTML 文档本身获得所有内容，而是获得一个带有 JavaScript 文件的基本 HTML 文档，该文件将使用浏览器呈现站点的其余部分。

这是一种相对较新的呈现网站的方法，直到 JavaScript 库开始将它纳入开发风格，它才真正流行起来。一些著名的例子是 Vue.js 和 React.js，我已经在这里写了更多。

回到之前的网站，`example.testsite.com`，假设您现在有一个 index.html 文件，包含以下代码行。

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Example Website</title>
</head>
<body>
  <div id="root">
    <app></app>
  </div>
  <script src="https://vuejs.org"type="text/javascript"></script>
  <script src="location/of/app.js"type="text/javascript"></script>
</body>
</html>
```

您可以立即看到，在使用客户端进行渲染时，index.hmtl 的工作方式发生了一些重大变化。

对于初学者来说，不是 HTML 文件中的内容，而是一个 id 为 root 的容器 div。在结束 body 标记的正上方还有两个脚本元素。一个将加载 Vue.js JavaScript 库，另一个将加载名为 app.js 的文件。

这与使用服务器端渲染截然不同，因为服务器现在只负责加载网站的基本部分。主要样板文件。其他一切都由客户端 JavaScript 库处理，在本例中是 Vue.js 和自定义 JavaScript 代码。

如果您只使用上面的代码向 URL 发出请求，您将得到一个空白屏幕。因为实际内容需要使用 JavaScript 呈现，所以不需要加载任何东西。

要解决这个问题，您可以将下面几行代码放入 app.js 文件中。

```
var data = {
        title:"My Website",
        message:"This is an example of my new website"
      }
  Vue.component('app', {
    template:
    `
    <div>
    <h1>{{title}}</h1>
    <p id="moreContent">{{message}}</p>
    <a v-on:click='newContent'>Link</a>
    </div>
    `,
    data: function() {
      return data;
    },
    methods:{
      newContent: function(){
        var node = document.createElement('p');
        var textNode = document.createTextNode('This is some more content from the other.html');
        node.appendChild(textNode);
        document.getElementById('moreContent').appendChild(node);
      }
    }
  })
  new Vue({
    el: '#root',
  });
```

现在，如果您访问 URL，您将看到与服务器端示例相同的内容。关键的区别在于，如果你点击页面上的链接来加载更多的内容，浏览器不会向服务器发出另一个请求。您使用浏览器呈现项目，因此它将使用 JavaScript 加载新内容，Vue.js 将确保只呈现新内容。其他的事情就不管了。

这要快得多，因为您只加载页面的很小一部分来获取新内容，而不是加载整个页面。

不过，使用客户端渲染也有一些利弊。由于在页面加载到浏览器之前，内容是不会呈现的，所以网站的 SEO 会受到影响。有很多方法可以解决这个问题，但是不像服务器端渲染那么容易。

另一件要记住的事情是，直到所有的 JavaScript 都下载到浏览器，你的网站/应用程序才能加载。这是有意义的，因为它包含了所有需要的内容。如果你的用户使用慢速互联网连接，这可能会使他们的初始加载时间有点长。

### 每种方法的优缺点

所以你有它。这些是服务器端和客户端渲染的主要区别。只有你这个开发者才能决定哪个选项最适合你的网站或应用程序。

下面是对每种方法的利弊的快速分析:

#### 服务器端优势:

1.  搜索引擎可以抓取网站以获得更好的 SEO。
2.  初始页面加载速度更快。
3.  非常适合静态网站。

#### 服务器端缺点:

1.  频繁的服务器请求。
2.  整体页面呈现缓慢。
3.  整页重新加载。
4.  非丰富的站点交互。

#### 客户端优势:

1.  丰富的网站互动
2.  初始加载后的快速网站渲染。
3.  非常适合 web 应用程序。
4.  强大的 JavaScript 库选择。

#### 客户端缺点:

1.  低搜索引擎优化，如果没有正确实施。
2.  初始加载可能需要更多时间。
3.  在大多数情况下，需要一个外部库。

如果你想了解更多关于 Vue.js 的信息，可以查看我在 juanmvega.com 的网站上的视频和文章！