# 如何使用 Apollo/Graphene 管理 GraphQL 变异中的文件上传

> 原文：<https://www.freecodecamp.org/news/how-to-manage-file-uploads-in-graphql-mutations-using-apollo-graphene-b48ed6a6498c/>

卢卡斯·麦克加特兰

# 如何使用 Apollo/Graphene 管理 GraphQL 变异中的文件上传

![RUQEf97I4RpExO9Cx4DeZx-pNaY0dkj6Dczg](img/2ce6b81dd33449c286eb90c99649c83b.png)

Illustration made with Paper by [FiftyThree](https://www.freecodecamp.org/news/how-to-manage-file-uploads-in-graphql-mutations-using-apollo-graphene-b48ed6a6498c/undefined)

GraphQL 是查询和操作数据的一种非常好的方式。你描述你的数据，提出你想要的，得到可预测的结果。问题是，GraphQL 只处理现成的可序列化数据——没有办法直接上传文件作为您的变异的一部分。

但是，如果有一种方法可以将 GraphQL 的强大功能与在多部分请求中上传文件的便利性结合起来，那会怎么样呢？@jaydenseric 想出了一个解决方案:[**graph QL-multipart-request-spec**](https://github.com/jaydenseric/graphql-multipart-request-spec)

> 如果您只是希望代码能够实现这一点，请跳到本文末尾，找到 Apollo 和 Graphene 规范的 JavaScript 和 Python 实现。

### 用 GraphQL 处理文件上传，不需要多部分上传(老方法)

Vanilla GraphQL 不支持将原始文件放入突变中。您可以请求和操作的数据仅限于您可以通过网络序列化的数据。实际上，这看起来像基本类型:**数字、布尔值和字符串**。这些功能非常棒——您可以用基本数据类型构建几乎所有需要的东西。

但是，如果您需要运行一个以文件为参数的变异，该怎么办呢？比如:上传新的个人资料图片。下面是如何用普通的 GraphQL 处理这个问题:

#### 选项 1: Base64 编码

Base64 编码的图像，并通过网络发送它作为一个字符串在您的变化。这有几个缺点:

1.  base64 编码的文件将比原始二进制文件大约大 33%。
2.  对文件进行编码和解码的操作成本更高。
3.  要记住正确编码和解码是复杂的。

#### 选项 2:单独的上传请求

运行一个单独的服务器(或 API)来上传文件。在第一个请求中上传文件，然后将得到的存储 URL 作为变异的一部分进行传递(第二个请求)。

然而，如果您必须上传几个文件，那么您必须等到所有的上传都完成之后，才能在您的变体中传递 URL(以识别它们),这就强制了一个同步且缓慢的过程。它还增加了另一层复杂性，使它在您的客户机中单独处理所有这些请求。

1.  不是异步。
2.  管理另一个上传服务器很复杂。

> 将一个文件作为变异参数的一部分传入不是很酷吗？

### 输入多部分请求规范(新方式)

这就是多部分请求规范的由来。这个 GraphQL 扩展规范允许您将文件嵌套在 GraphQL 变体中的任何位置，如下所示:

```
{  query: `    mutation($file: Upload!) {      uploadFile(file: $file) {        id      }    }  `,  variables: {    file: File // somefile.jpg  }}
```

如您所见，在文件中添加就像在任何其他类型的突变参数中添加一样简单。要实现这个规范，您必须安装两部分:一部分运行在客户机上，另一部分运行在服务器上。他们是这样做的:

*   客户机规范定义了如何将一个变异中的任何文件对象映射到一个键中，该键定位它们在一个多部分请求中的位置。
*   服务器规范定义了如何解析该映射，并根据映射中提供的键使文件可重新访问。

因此，在 apollo-client 中，您可以运行如下所示的变异:

```
this.props.mutate({variables: {file: yourFile}})
```

### 多部分请求规范实现

如果你想用 Apollo 实现多部分请求规范，你可以很容易地把它与袁晓超·塞里奇写的这些包集成在一起。这些是针对 JavaScript 和 Apollo 生态系统的。

[**jaydenseric/apollo-upload-Client**](https://github.com/jaydenseric/apollo-upload-client)
[*Apollo-upload-Client-通过 GraphQL 突变增强 Apollo 客户端对文件上传的直观性。*github.com](https://github.com/jaydenseric/apollo-upload-client)[**jaydenseric/Apollo-upload-Server**](https://github.com/jaydenseric/apollo-upload-server)
[*Apollo-upload-Server-增强 Apollo GraphQL 服务器，通过 GraphQL 突变实现直观的文件上传。*github.com](https://github.com/jaydenseric/apollo-upload-server)

如果您通过 Graphene 和 Django 运行您的 GraphQL API，您可以通过用我在这里写的这个包替换您的 GraphQL 视图来实现 Python 中的规范:

[**lmcgartland/Graphene-file-upload**](https://github.com/lmcgartland/graphene-file-upload)
[*Graphene-file-upload-增强 Graphene Django GraphQL 服务器通过 GraphQL 突变实现直观的文件上传。*github.com](https://github.com/lmcgartland/graphene-file-upload)

### 结论

这个规范是向 GraphQL 应用程序添加文件上传功能的简单方法。少关注如何获取文件，多关注如何使用它们！

如果你想聊更多，就聊聊 GraphQL 或者伟大的字体，**在 twitter 上联系我@ [lucasmcgartland](https://twitter.com/lucasmcgartland)** 或者在下面的网站上找到我:

> [网站](http://www.lucasmcgartland.com) | [邮箱](mailto:luke@thebeeinc.com) | [LinkedIn](https://www.linkedin.com/in/lucasmcgartland/) | [推特](https://twitter.com/lucasmcgartland) | [Dribbble](https://dribbble.com/lucasmcgartland)

#### 延伸阅读:

*   [https://medium . com/@ Daniel bue chele/file-uploads-with-graph QL-and-Apollo-5502 bbf 3941 e](https://medium.com/@danielbuechele/file-uploads-with-graphql-and-apollo-5502bbf3941e)