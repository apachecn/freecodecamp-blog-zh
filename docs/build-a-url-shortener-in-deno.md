# 如何在 Deno 中构建一个网址缩写器

> 原文：<https://www.freecodecamp.org/news/build-a-url-shortener-in-deno/>

在这篇文章中，我们将学习 Deno 的基础知识，比如如何运行一个程序和拥抱安全性。

Deno 是用 Rust 编写的新的 JavaScript 和 TypeScript 运行时。它提供了严密的安全性、现成的 TypeScript 支持、运行它的单个可执行文件以及一组经过审查和审计的标准模块。

像 Node.js 中的 [npm](https://npmjs.com) 一样，Deno 中的包在一个名为 [X](https://deno.land/x/) 的集中式包存储库中进行管理。我们将使用其中的一个库 Oak 在 Deno 中构建一个基于 REST API 的服务器。

在通过使用类似 Express 的路由器包 [Oak](https://deno.land/x/oak@v6.3.0) 学习了基础知识之后，我们将跳入 Deno 的深水区，构建一个完整的应用程序。

下面是我们将在该应用程序中设置的内容:

1.  使用基于 JSON 的配置文件将 URL 短代码映射到端点。
2.  为每个 URL 附加截止日期，以便这些重定向只在有限的时间内有效。

## 0.先决条件

1.  从[这个环节](https://deno.land/#installation)安装 Deno。
2.  确保你了解 JavaScript 的基础知识。

虽然不一定要跟着这篇文章走，但你可以看看下面的 YouTube 视频，以视频格式了解 Deno。

[https://www.youtube.com/embed/VQ8Jb7GLHgk?feature=oembed](https://www.youtube.com/embed/VQ8Jb7GLHgk?feature=oembed)

A video tutorial for setting up Deno

那么，我们开始吧。？

## 1.如何构建路由器

为了编写应用程序的服务器端代码，我们将使用 Oak 模块。它有一个类似 Express 的语法来定义 API 路由。

如果我们在这里看一下它的[文档，“](https://deno.land/x/oak)[基本用法](https://deno.land/x/oak#basic-usage)部分几乎涵盖了我们路由器中需要的所有用例。因此，我们将扩展该代码来构建我们的应用程序。

为了测试这段代码，您可以在一个文件夹中创建一个名为`index.ts`的文件，并将“基本用法”代码复制到其中。

要理解如何在 Deno 中运行 TypeScript 或 JavaScript 文件，首先需要理解 Deno 如何运行文件。

您可以通过运行命令`deno run file_name.ts`或`file_name.js`来运行一个文件，后面跟着一组为您的应用程序提供某些系统权限的标志。

为了测试这一点，使用命令`deno run index.ts`运行我们刚刚创建的包含“基本用法”代码的文件。

您将看到 Deno 抱怨您没有为您的应用程序提供网络访问。因此，要做到这一点，您需要将`-allow-net`添加到运行命令中。该命令将类似于`deno run index.ts -allow-net`。

写在“基本用法”代码中的路由器如下所示:

```
router
  .get("/", (context) => {
    context.response.body = "Hello world!";
  })
  .get("/book", (context) => {
    context.response.body = Array.from(books.values());
  })
  .get("/book/:id", (context) => {
    if (context.params && context.params.id && books.has(context.params.id)) {
      context.response.body = books.get(context.params.id);
    }
  }); 
```

为了分解上面的代码，首先定义了一个`router`对象。然后在路由器上调用`get`函数，为我们的应用程序定义不同的端点。相应的逻辑在回调函数中定义。

例如，对于“/”端点，已经定义了一个在响应正文中返回“Hello World”的回调函数。我们可以保持这个端点不变，通过接收这个响应来测试我们的应用服务器是否在运行。

我们不需要已经定义的“/book”URL，因此可以安全地删除它的定义。此时，您的路由器应该具有以下结构:

```
router
  .get("/", (context) => {
    context.response.body = "Hello world!";
  })
  .get("/book/:id", (context) => {
    if (context.params && context.params.id && books.has(context.params.id)) {
      context.response.body = books.get(context.params.id);
    }
  });
```

在下一节中，我们将集中精力开始构建实际的应用程序。

## 2.如何建立网址缩写

现在让我们开始构建真正的 URL 缩写程序。

它应该根据一个`shortcode`重定向到一个目的地(`dest`)。重定向也应该只对一个`expiryDate`有效，它可以以年-月-日的格式提供。

基于这些假设，让我们创建名为`urls.json`的配置文件。文件的格式将是:

```
{
  "shortcode": {
    "dest": "destination_url_string",
    "expiryDate": "YYYY-MM-DD"
  }
} 
```

你可以[在这里](https://github.com/akash-joshi/deno-url-shortener/blob/master/urls.json)查看 JSON 文件。

要在您的代码中读取这个 JSON 文件，请将以下内容添加到您的`index.ts`的顶部:

```
import { Application, Router } from "<https://deno.land/x/oak/mod.ts>";

const urls = JSON.parse(Deno.readTextFileSync("./urls.json"));

console.log(urls); 
```

现在，要运行您的`index.ts`，您将需要另一个标志`—allow-read`，否则 Deno 将抛出“未提供读取权限”错误。你的最终命令变成了`deno run —allow-net —allow-read index.ts`。

运行该命令后，您将看到 JSON 文件在终端窗口中打印出来。这意味着您的程序能够正确读取 JSON 文件。

如果我们回到上面看到的“基本用法”示例，路径“/book/:id”正是我们需要的。

我们可以使用“/shrt/:urlid”而不是“/book/:id”，在这里我们将根据 URL ID ( `:urlid`)获得各个 URL。

用以下代码替换“/book/:id”路径中的现有代码:

```
.get("/shrt/:urlid", (context) => {
    if (context.params && context.params.urlid && urls[context.params.urlid]) {
      context.response.redirect(urls[context.params.urlid].dest);
    } else {
      context.response.body = "404";
    }
  }); 
```

路线中的`if`条件执行以下操作:

1.  检查参数是否附着到路线
2.  检查参数`urlid`是否在参数表中。
3.  检查`urlid`是否与 JSON 中的任何 URL 匹配。

如果与所有这些匹配，用户将被重定向到正确的 URL。如果没有，则返回主体上的 404 响应。

为了测试这一点，将这条路线复制到`index.ts`中。路由器现在看起来像这样:

```
router
  .get("/", (context) => {
    context.response.body = "Hello world!";
  })
	.get("/shrt/:urlid", (context) => {
	    if (context.params && context.params.urlid && urls[context.params.urlid]) {
	      context.response.redirect(urls[context.params.urlid].dest);
	    } else {
	      context.response.body = "404";
	    }
	  }); 
```

并使用`deno run —allow-net —allow-read index.ts`运行该文件。

如果您从示例中复制了 JSON 文件，并且如果您转到`http://localhost:8000/shrt/g`，您将被重定向到 Google 的主页。

另一方面，如果你使用一个随机的短码，在我们的 URL 配置中不起作用，它会把你带到 404 页面。

然而，您将会看到我们的 shortener 并没有对 JSON 文件中的变化做出实时反应。为了测试这一点，尝试添加一个新的重定向到`urls.json`，格式如下:

```
"shortcode": {
    "dest": "destination_url_string",
    "expiryDate": "YYYY-MM-DD"
  }
```

其原因是`urls.json`在开始时仅被读取一次。所以，现在我们将添加实时重装到我们的服务器。

## 3.如何添加实时重装

为了让`urls`对象对 JSON 文件中的变化做出实时反应，我们只需将 read 语句移动到我们的 route 中。这应该如下所示:

```
.get("/shrt/:urlid", (context) => {
  const urls = JSON.parse(Deno.readTextFileSync("./urls.json"));

  if (context.params && context.params.urlid && urls[context.params.urlid]) {
    context.response.redirect(urls[context.params.urlid].dest);
  } else {
    context.response.body = "404";
  }
}); 
```

请注意我们是如何在路由器内部移动 URLs 对象的。现在，在这种情况下，每次调用 route 时都会读取配置文件，因此它可以对`urls.json`文件中的任何更改做出实时反应。因此，即使我们动态地添加或删除其他重定向，我们的代码也会对其做出反应。

## 4.如何给 URL 添加过期时间

为了让我们的 URL 在某个特定的日期过期，我们将使用流行的 Moment.js 库，这使得处理日期变得很容易。

幸运的是，它也被[移植为 Deno](https://deno.land/x/moment) 。要了解它是如何工作的，请查看前面链接中的文档。

要在我们的代码中使用它，直接通过它的 URL 导入它，如下所示:

```
import { Application, Router } from "<https://deno.land/x/oak/mod.ts>";
import { moment } from "<https://deno.land/x/moment/moment.ts>";

const router = new Router(); 
```

为了检查 URL 过期的日期，我们检查了我们的`urls`对象上的`expiryDate`键。这将使代码看起来像这样:

```
if (context.params && context.params.urlid && urls[context.params.urlid]) {
  if (
    urls[context.params.urlid].expiryDate > moment().format("YYYY-MM-DD")
  ) {
    context.response.redirect(urls[context.params.urlid].dest);
  } else {
    context.response.body = "Link Expired";
  }
} else {
  context.response.body = "404";
} 
```

在`moment().format("YYYY-MM-DD")`中，我们使用`moment()`获取当前日期和时间。我们可以使用函数`.format("YYYY-MM-DD")`将其转换为“YYYY-MM-DD”(年-月-日)格式。

通过与我们的`expiryDate`键进行比较，我们可以检查 URL 是否已经过期。

就是这样！您已经在 Deno 中构建了一个功能齐全的 URL 缩短器。你可以在 GitHub repo 中找到最终代码[这里](https://github.com/akash-joshi/deno-url-shortener)。

通过将`expiryDate`设置为当前日期并对`urls.json`和我们的代码进行其他更改来测试它。

### 我对德诺的看法

作为文章的总结，我将在 deno.land 上提出我的最终想法。

虽然看到服务器端语言考虑到安全性并支持开箱即用的 TypeScript 令人耳目一新，但 Deno 在用于生产系统之前还有很长的路要走。

例如，TypeScript 编译仍然非常慢，编译时间约为 20 秒，即使是像我们刚刚开发的简单程序也是如此。

在错误报告方面，描述错误仍然很糟糕。例如，当在函数本身中嵌入读取`urls.json`的代码时，Deno 不能报告`-allow-read`标志没有被设置。相反，它只是抛出一个内部服务器错误，而没有在终端上显示正确的错误。

### 接下来呢？

你可以通过构建更复杂的应用程序来提高你的 Deno 或打字技能，比如聊天应用程序 T1 或 T2 维基百科克隆 T3。

你也可以浏览位于 [deno.land](http://deno.land/) 的 Deno 文档，以便更加熟悉基础知识。

谢谢你看了这么远，开心编程？！！

### 重要链接

Deno-https://Deno . land
Deno X(包仓库)——
Oak(REST 框架)——[https://deno.land/x/oak](https://deno.land/x/oak)
Oak 基本用法——[https://deno.land/x/oak@v6.3.1#basic-usage](https://deno.land/x/oak@v6.3.1#basic-usage)
Final GitHub Repo——[https://github.com/akash-joshi/deno-url-shortener](https://github.com/akash-joshi/deno-url-shortener)