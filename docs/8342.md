# 如何使用 async/await 和 Firebase 数据库编写漂亮的 Node.js APIs

> 原文：<https://www.freecodecamp.org/news/how-to-write-beautiful-node-js-apis-using-async-await-and-the-firebase-database-befdf3a5ffee/>

保罗·布雷斯林

# 如何使用 async/await 和 Firebase 数据库编写漂亮的 Node.js APIs

![1*6wZYofh0czXf3SO8Ubw2xg](img/95b72e69482cc53555be16d1c5269f49.png)

本教程将涵盖在编写 RESTful API 端点来读写 Firebase 数据库实例时会遇到的典型用例。

将重点关注**漂亮的**异步代码，它利用了 Node.js 中的`async/await`特性(在 7.6 及更高版本中可用)。

(挥手告别回拨地狱的时候随意甜甜一笑？)

### 先决条件

我假设您已经用 Firebase Admin SDK 设置了一个 Node.js 应用程序。如果没有，那么查看一下[官方设置指南](https://firebase.google.com/docs/admin/setup)。

### 写入数据

首先，让我们创建一个示例`POST`端点，它将把单词保存到我们的 Firebase 数据库实例:

这是一个非常基本的端点，它接受一个`userId`和一个`word`值，然后将给定的单词保存到一个`words`集合中。很简单。

但是有些不对劲。我们错过了错误处理！在上面的例子中，我们返回了一个`201`状态代码(意味着资源被创建了)，即使这个词没有正确地保存到我们的 Firebase 数据库实例中。

因此，让我们添加一些错误处理:

既然端点返回了准确的状态代码，客户端就可以向用户显示相关的消息。例如，“Word 保存成功。”或者“无法保存 word，请单击此处再试一次。”

> 注意:如果 ES2015+的一些语法对你来说不熟悉，请查看通天塔 [ES2015 指南](https://babeljs.io/learn-es2015/)。

### 阅读日期

好了，现在我们已经向 Firebase 数据库写入了一些数据，让我们试着从中读取数据。

首先，让我们看看使用最初的基于承诺的方法时`GET`端点是什么样子的:

同样，很简单。现在让我们将其与相同代码的`async/await`版本进行比较:

注意添加在函数参数`(req, res)`之前的`async`关键字和现在位于`db.ref()`语句之前的`await`关键字。

`db.ref()`方法返回一个承诺，这意味着我们可以使用`await`关键字来“暂停”脚本的执行。(`await`关键字可用于任何承诺)。

最终的`res.send()`方法将是**只在**兑现`db.ref()`承诺后运行 **。**

这一切都很好，但是当您需要链接多个异步请求时,`async/await`的真正魅力就显而易见了。

假设您必须顺序运行许多异步函数:

不漂亮。这也被称为“末日金字塔”(我们甚至还没有添加错误处理程序)。

现在看一下上面改写成使用`async/await`的代码片段:

不再有末日金字塔了！此外，所有的`await`语句都可以封装在一个`try/catch`块中，以处理任何错误:

### 并行异步/等待请求

如果您需要同时从 Firebase 数据库中获取多条记录，该怎么办？

简单。只需使用`Promise.all()`方法并行运行 Firebase 数据库请求:

### 还有一点

当创建一个端点来返回从 Firebase 数据库实例中检索的数据时，注意不要简单地返回整个`snapshot.val()`。这可能会导致客户端的 JSON 解析出现问题。

例如，假设您的客户端有以下代码:

Firebase 返回的`snapshot.val()` 可以是一个 JSON 对象，或者如果不存在记录，则返回`null`。如果返回`null`，上面代码片段中的`response.json()`将抛出一个错误，因为它试图解析一个非对象类型。

为了避免这种情况，您可以使用`Object.assign()`始终向客户端返回一个对象:

感谢阅读！

有兴趣看看基于 Firebase 和 Node.js 构建的真实项目吗？看看 [Vocabify](https://vocabifyapp.com) ，这个词汇生成器可以帮助你记住遇到的单词。