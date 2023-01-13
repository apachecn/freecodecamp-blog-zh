# 用脚本 src 属性将 JavaScript 链接到 HTML

> 原文：<https://www.freecodecamp.org/news/link-javascript-to-html-with-the-src/>

标记中的“src”属性是指向要链接到 HTML 文档的外部文件或资源的路径。

例如，如果您有自己的名为“script.js”的自定义 JavaScript 文件，并希望将其功能添加到您的 HTML 页面中，您可以像这样添加它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Script Src Attribute Example</title>
  </head>
  <body>

  <script src="./script.js"></script>
  </body>
</html>
```

这将指向名为“script.js”的文件，该文件与。html 文件。您也可以使用“..”链接到其他目录在文件路径中。

```
<script src="../public/js/script.js"></script>
```

这将向上跳一个目录级别，然后进入“public”目录，再到“js”目录，然后到“script.js”文件。

您还可以使用“src”属性链接到外部。由第三方托管的 js 文件。如果您不想下载文件的本地副本，可以使用此选项。请注意，如果链接改变或网络访问中断，那么您链接的外部文件将无法工作。

这个例子链接到一个 jQuery 文件。

```
<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"></script>
```

#### **更多信息:**

[HTML 上的 MDN 文章](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-src)