# 如何用 JavaScript 编写强大的模式

> 原文：<https://www.freecodecamp.org/news/how-to-write-powerful-schemas-in-javascript-490da6233d37/>

作者迭戈·哈兹

# 如何用 JavaScript 编写强大的模式

#### 介绍 [schm](https://github.com/diegohaz/schm) ，这是一个功能强大且高度可组合的库，用于用 JavaScript 和 Node.js 创建模式

![1*u_gVBtCyIcrWbOBv3xDCWg](img/3498c633dfb8c9b4c3ac029e7824ab58.png)

Background photo by [Willi Heidelbach](https://www.flickr.com/photos/wilhei/ "Vá para a galeria de Willi Heidelbach")

自 2002 年以来，我一直从事 HTML、CSS 和 JavaScript 方面的工作。几年前，我第一次在 JavaScript 中需要某种模式。

在使用了许多不同的库，甚至创作了[一个](https://github.com/diegohaz/querymen)和[另一个](https://github.com/diegohaz/bodymen)之后，我决定创建 [schm](https://github.com/diegohaz/schm) 。这是我所有 JavaScript 模式经验的结果。

### 什么是 [schm](https://github.com/diegohaz/schm) ？

`schm`是一组 npm 包，帮助开发者处理 JavaScript 和 Node.js 中的模式。

它受到了[猫鼬模式](http://mongoosejs.com/docs/guide.html)的高度启发。实际上，它们非常相似，以至于您可以在 Mongoose 模式中使用`schm`参数，反之亦然。虽然它不是 MongoDB 特有的。您可以在 JavaScript 中使用它。

### [schm](https://github.com/diegohaz/schm) 解决什么样的问题？

#### ？表单值的解析和验证

在客户端，您可以使用模式来定义 HTML 表单的模型。它使得转换和验证值变得更加容易。此外，如果您在服务器上使用 Node.js，您可以使用相同的模式。结果是客户端和服务器验证之间的行为一致。

#### ？查询字符串的解析和验证

考虑下面的查询字符串:`/?term=John&page=2&limit=10`。通过组合诸如 [schm-koa](https://github.com/diegohaz/schm/tree/master/packages/schm-koa) 、 [schm-express](https://github.com/diegohaz/schm/tree/master/packages/schm-express) 和/或 [schm-mongo](https://github.com/diegohaz/schm/tree/master/packages/schm-mongo) 之类的包，您将能够轻松地解析和验证带有搜索词和分页的查询字符串。

#### 客户端和服务器之间的☊通信

例如，如果您有一个使用 REST API 资源的应用程序，您可以使用模式在客户端定义您的客户端期望从服务器接收的对象结构。如果服务器上发生了变化(例如，属性被重命名)，您只需更新您的模式，这样您的整个应用程序就可以继续工作。

### 创建模式

简单的模式只是键和类型的映射:

这与使用`type`属性是一样的:

模式也可以是键和默认值之间的映射。将自动推断类型:

这相当于执行以下操作:

要了解更多关于如何编写模式的信息，请看一下[mongose 模式](http://mongoosejs.com/docs/guide.html)。

### 解析值

定义模式后，可以用它来解析值。这个过程将把值转换成适当的类型，并应用模式中定义的其他解析器。这些是可用的解析器:`type`、`default`、`set`、`get`、`trim`、`uppercase`、`lowercase`。

输出将是这样的:

```
{  name: "HAZ",  birthdate: Tue Apr 10 1990 00:00:00 GMT,}
```

### 验证值

就像在[mongose 模式](http://mongoosejs.com/docs/guide.html)中一样，您可以在模式中定义验证规则。这些是可用的验证器:`validate`、`required`、`match`、`enum`、`max`、`min`、`maxlength`、`minlength`。

您还可以通过扩展模式来创建定制的解析器和验证器。我们将在本文的后面讨论它。

默认情况下，验证错误将是描述错误的对象数组。

```
[  {    message: "age must be greater than or equal 18",    min: 18,    param: "age",    validator: "min",    value: 17,  }]
```

如果验证通过，它将返回解析后的值，就像`parse()`一样。

### 构成多个模式

假设您有描述一个`body`、`identity`和其他事物的单独模式，并且想要组合它们来构建一个`human`模式。这听起来很简单:

构建模式的另一种方式是通过嵌套。一个模式可以在另一个模式中用作`type`:

### 扩展模式

这才是`schm`真正出彩的地方。您可以添加定制的解析器和验证器，甚至通过创建模式组来替换`parse`和`validate`方法的默认行为。

一个**模式组**是一个接收前一个模式作为唯一参数的函数。除了前面的`params`、`parsers`和`validators`，模式对象还有一个`merge`方法，这对于模式组函数将新功能合并到前面的模式中很有用。

上述代码片段的输出将类似如下:

```
{  name: "Haz!!!",  age: 27,}
```

如果您想进一步了解如何创建自定义解析器，请看一下核心库[中的解析器是如何编写的。](https://github.com/diegohaz/schm/blob/master/packages/schm/src/parsers.js)

通过扩展模式，我们可以创建许多种类的东西。大部分的`schm`卫星库都是这么写的，比如 [schm-translate](https://github.com/diegohaz/schm/tree/master/packages/schm-translate) 、 [schm-computed](https://github.com/diegohaz/schm/tree/master/packages/schm-computed) 和 [schm-mongo](https://github.com/diegohaz/schm/tree/master/packages/schm-mongo) 。

我们现在就来谈谈其中的一个。

### 重命名值键

schm-translate 是`schm`中最简单但功能强大的卫星库之一。它比压缩到一个函数中的 [10 行代码](https://github.com/diegohaz/schm/blob/master/packages/schm-translate/src/index.js)多一些，这个函数允许你将值键转换成模式键。

假设您正在开发一个消耗 REST API 资源的 webapp。突然，开发人员改变了 API 的内容，这使得响应主体返回的模型与客户期望的略有不同。它现在返回的不是一个`email`属性，而是一个`emails`数组。

这可能会使你的应用程序崩溃。如果您没有模式或任何其他集中的方式来处理该对象，您将需要更新应用程序的每个部分，以符合服务器的变化。

有了`schm`和`schm-translate`，只要在一个地方改几行代码就能解决:

输出将完全是您的应用程序在更改前预期的输出:

```
{  name: "Haz",  email: "hazdiego@gmail.com",}
```

[点击此处查看所有包的列表](https://github.com/diegohaz/schm#packages)

### 这与其他模式库有何不同？

一个常见的问题是`schm`和其他库的区别，比如 [Joi](https://github.com/hapijs/joi) 和 [ajv](https://github.com/epoberezkin/ajv) (遵循 [JSON 模式](http://json-schema.org/)规范)。

与`ajv`相比，`schm`不遵循任何特定的规范。相反，它试图模仿 Mongoose 模式 API。此外，尽管`ajv`有一些解析特性，但它们仅限于[默认值](https://github.com/epoberezkin/ajv#assigning-defaults)和[类型强制](https://github.com/epoberezkin/ajv#coercing-data-types)。

例如，在`schm`中，基于模式解析值的能力使得将查询字符串转换成 MongoDB 查询成为可能。

也就是说，`Joi`和`ajv`都可以和`schm`结合。您可以轻松地扩展它以使用不同的验证方法:

### 感谢您阅读本文！

如果你喜欢它，并且觉得它很有用，你可以做以下事情来表达你的支持:

*   鼓掌吗？此条上的按钮几次(最多 50 次)
*   在 GitHub 上给一个明星⭐️:[https://github.com/diegohaz/schm](https://github.com/diegohaz/schm)
*   在 GitHub 上关注我:[https://github.com/diegohaz](https://github.com/diegohaz)
*   在推特上关注我:[https://twitter.com/diegohaz](https://twitter.com/diegohaz)