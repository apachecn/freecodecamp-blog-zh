# HTML a href 属性举例说明

> 原文：<https://www.freecodecamp.org/news/the-a-href-attribute-explained/>

`<a href>`属性指的是由链接提供的目的地。没有了`<href>`属性，`a`(锚)标签就死了。

## 如何使用

有时在你的工作流程中，你不想要一个实时链接，或者你还不知道链接的目的地。在这种情况下，将`href`属性设置为`"#"`来创建一个死链接很有用。

`href`属性可用于链接到本地文件或互联网上的文件。

例如:

```
<html>
  <head>
    <title>Href Attribute Example</title>
  </head>
  <body>
    <h1>Href Attribute Example</h1>
      <p>
        <a href="https://www.freecodecamp.org/contribute/">The freeCodeCamp Contribution Page</a> shows you how and where you can contribute to freeCodeCamp's community and growth.
      </p>
    </h1>
  </body>
</html>
```

所有浏览器都支持`<a href>`属性。

## 更多 HTML 属性:

`hreflang`:指定链接资源的语言。

`target`:指定将在其中打开链接资源的上下文。

`title`:定义链接的标题，作为工具提示显示给用户。

### **例题**

```
<a href="#">This is a dead link</a>
<a href="https://www.freecodecamp.org">This is a live link to freeCodeCamp</a>
<a href="https://html.com/attributes/a-href/">more with a href attribute</a>
```

### **页面内锚点**

也可以在页面的某个地方设置一个锚点。要做到这一点，你应该首先在页面上的位置放置一个标签

```
<a name="top"></a>
```

标签之间不需要任何描述。之后，你可以在同一个页面的任何地方放置一个指向这个锚的链接。要做到这一点，你应该使用标签 

```
<a href="#top">Go to Top</a>
```

### ****图片链接****

`<a href="#">`也可以应用于图像和其他 HTML 元素。

### ****例如****

```
<a href="#"><img itemprop="image" style="height: 90px;" src="http://www.chatbot.chat/assets/images/header-bg_y.jpg" alt="picture">  </a>
```

### **href 的更多例子**

```
<base href="https://www.freecodecamp.org/a-href/">This gives a base url for all further urls on the page</a>
<link href="style.css">This is a live link to an external stylesheet</a>
```

## 你还能用

更多定制！让我们来看一个具体的使用案例:

mailto 链接是一种带有特殊参数的超链接(`<a href=""></a>`)，允许您指定附加收件人、主题行和/或正文。

### **收件人的基本语法是:**

```
<a href="mailto:friend@something.com">Some text</a>
```

### 向该邮件添加主题:

如果你想给邮件添加一个特定的主题，注意在主题行的任何地方添加`%20`或`+`。确保它被正确格式化的一个简单方法是使用一个 [URL 解码器/编码器](https://meyerweb.com/eric/tools/dencoder/)。

### 添加正文:

类似地，您可以在电子邮件的正文部分添加特定的消息:同样，空格必须由`%20`或`+`替换。在主题参数之后，任何附加参数必须以`&`开头

例如:假设您希望用户向他们的朋友发送电子邮件，告知他们在自由代码营的进展:

地址:空

主题:好消息

正文:我正在成为一名开发人员

您现在的 html 链接:

```
<a href="mailto:?subject=Great%20news&body=I%20am%20becoming%20a%20developer">Send mail!</a>
```

这里，我们让 mailto 为空(mailto:？).这将打开用户的电子邮件客户端，用户将自己添加收件人地址。

### 添加更多收件人:

同样，您可以添加 CC 和 bcc 参数。用逗号分隔每个地址！

附加参数必须以`&`开头。

```
<a href="mailto:firstfriend@something.com?subject=Great%20news&cc=secondfriend@something.com,thirdfriend@something.com&bcc=fourthfriend@something.com">Send mail!</a>
```

#### **更多信息:**

[MDN -电子邮件链接](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#E-mail_links)