# 如何在 AWS Lambda 上构建无服务器 NodeJS 微服务

> 原文：<https://www.freecodecamp.org/news/building-a-nodejs-microservice-on-aws-lambda-6adb6da53cbb/>

保罗·马修·贾沃斯基

# 如何在 AWS Lambda 上构建无服务器 NodeJS 微服务

![A7mlNGUYhiI21MyRC2sgfiTVwSvQwjDVam7Y](img/689764be8581317f6d85071e2b51015f.png)

### **已弃用**

不幸的是，自从我写这篇文章以来，[无服务器框架](https://serverless.com/)的 v1.0 已经发布，同时还有一些突破性的变化。我相信您只需添加以下内容就可以迁移到新版本:

```
 integration: lambda
```

你的每一个资源。例如:

```
createPet:    handler: handler.create    events:      - http:          path: pets          method: POST          cors: true          integration: lambda
```

然而，我已经决定暂时离开无服务器，主要是因为身份验证、授权的问题，以及 DynamoDB 的挫折，所以我不会更新这篇文章。我将在后面的故事中探讨这些问题以及我切换回“传统”REST API 的决定。

现在，我建议参考 API Gateway 上的官方无服务器文档[来开始，并可能使用这篇文章的其余部分作为参考，记住无服务器文档中的任何信息都优先于这里写的任何内容。](https://serverless.com/framework/docs/providers/aws/events/apigateway/#api-gateway)

### 从这里开始要小心:

在本文中，我将分享我使用 AWS Lambda、API Gateway 和 DynamoDB 实现“无服务器”和构建 CRUD API“微服务”的经验。这将为您使用这些工具制作自己的微服务提供指导。

### **入门**

我将假设您已经安装了 AWS 帐户和 NodeJS。如果没有，现在就处理。

接下来，您需要安装无服务器 npm 包，它提供了一种轻松创建、编辑和部署作为 AWS Lambda 函数的微服务的方法:

```
npm install -g serverless
```

然后按照 Amazon 的[说明创建一个 IAM 用户并配置无服务器](https://github.com/serverless/serverless/blob/master/docs/02-providers/aws/01-setup.md)来使用这些凭证。

导航到要存储新项目的目录，然后运行:

```
serverless create --template aws-nodejs --path pets-service
```

现在是在您的项目中设置林挺的好时机。由于这不是对 ESLint 的介绍，我就不详细介绍了，但是我建议你现在就安装并设置你的**。eslintrc** 这样:

```
{  “plugins”: [“node”],  “extends”: [“eslint:recommended”, “plugin:node/recommended”],  “env”: {    “node”: true,    “mocha”: true  },  “rules”: {    “no-console”: 0,    “node/no-unsupported-features”: [2, {“version”: 4}]  }}
```

这里需要注意的重要一点是节点插件中的“无不支持功能”规则。AWS Lambda 使用 Node v4.3，知道哪些 ES6 特性可用可能是件痛苦的事。这就很容易了。

### **创建处理程序**

使用 npm 安装 aws-sdk 和 lodash:

```
npm i -S aws-sdk lodash
```

现在转到 **handler.js** 并将这些依赖项添加到文件的顶部:

```
const aws = require(‘aws-sdk’);const _ = require(‘lodash/fp’);
```

注意，我们使用 lodash 的“函数式编程”变体，因为它的合并函数不会改变原始对象。

接下来，设置您的文档客户机与 DynamoDB 通信:

```
const dynamo = new aws.DynamoDB.DocumentClient();
```

现在让我们使用 **create()** 函数在数据库中创建一个新宠物:

```
exports.create = function(event, context) {  const payload = {    TableName: 'Pets',    Item: event.body  };
```

```
 const cb = (err, data) => {    if (err) {      console.log(err);      context.fail(‘Error creating pet’);    } else {      console.log(data);      context.succeed(data);    }  }
```

```
 dynamo.put(payload, cb);};
```

很容易看出这里发生了什么:

我们得到一个带有键**主体**的**事件**对象，它包含了我们想要存储的数据。DocumentClient 要求至少有一个带有键**表名**和**项**的对象被传递到 put()。

我们还提供了一个回调函数，它做两件重要的事情:

如果有错误，我们运行 **context.fail()** ，基本上只是 AWS 提供的一个 onError 回调。

如果条目创建成功，我们运行 **context.succeed()** ，传递作为 Lambda 函数结果返回的数据。

关于 DynamoDB 的一个重要警告是，我们必须在创建时自己提供主键。在这种情况下，我们必须将 **petId** 作为关键字包含在 event.body 对象中。

为什么 DynamoDB 缺少这样一个基本特性？你的猜测和我的一样好。

我很幸运，在我的应用程序中，Auth0 为我生成了一个惟一的 ID，我将它用于我的 auth/user 管理。如果你做不到，你必须用其他方法解决这个问题。

对于其余的 CRUD 操作，我们将遵循相同的基本模式:

```
exports.show = function(event, context) {  const payload = {    TableName: 'Pets',    Key: {      petId: event.params.path.petId    }  }
```

```
 const cb = (err, data) => {    if (err) {      console.log(err);      context.fail('Error retrieving pet');    } else {      console.log(data);      context.done(null, data);    }  }
```

```
 dynamo.get(payload, cb);};
```

```
exports.list = function(event, context) { const payload = {  TableName: 'Pets' }
```

```
 const cb = (err, data) => {    if (err) {      console.log(err);      context.fail('Error getting pets');    } else {      console.log(data);      context.done(null, data);    }  }
```

```
 dynamo.scan(payload, cb);}
```

```
exports.update = function(event, context) {  const payload = {    TableName: 'Pets',    Key: {      petId: event.params.path.petId    }  };
```

```
 dynamo.get(payload, (err, data) => {    if (err) {      console.log(err);      context.fail('No pet with that id exists.');    } else {      const item = _.merge(data.Item, event.body);      payload.Item = item;
```

```
 dynamo.put(payload, (putErr, putData) => {        if (putErr) {          console.log('Error updating pet.');          console.log(putErr);          context.fail('Error updating pet.');        } else {          console.log('Success!');          console.log(putData);          context.done(null, item);        }      });    }  });}
```

```
exports.delete = function(event, context) {  const payload = {    TableName: 'Pets',    Key: {      petId: event.params.path.petId    }  };
```

```
 const cb = (err, data) => {    if (err) {      console.log(err);      context.fail('Error retrieving pet');    } else {      console.log(data);      context.done(null, data);    }  }
```

```
 dynamo.delete(payload, cb);}
```

这里有几件事需要注意:

我们希望能够进行部分更新，这意味着您不需要发送整个宠物对象及其更改，您可以只发送更改。为了实现这一点，我们首先在 **update()** 函数中调用一个 **get** ，然后将我们的更改合并到该操作的结果中。

我们的 **petId** 作为参数传递给 API Gateway，然后通过 event.params.path.petId 在 Lambda 中提供给我们。

### **配置无服务器**

这里我们差不多完成了，所以现在让我们设置无服务器配置文件。打开 **serverless.yml** 并编辑它，如下所示:

```
service: pets-service
```

```
provider:  name: aws  runtime: nodejs4.3
```

```
defaults:  stage: dev  region: us-west-2
```

```
functions:  createPet:    handler: handler.create    events:      - http:          path: pets          method: POST  showPet:    handler: handler.show    events:      - http:          path: pets/{petId}          method: GET  listPets:    handler: handler.list    events:      - http:          path: pets          method: GET  updatePet:    handler: handler.update    events:      - http:          path: pets/{petId}          method: PUT  deletePet:    handler: handler.delete    events:      - http:          path: pets/{petId}          method: DELETE
```

我认为这很容易理解。我们只是指定 Lambda 函数的名称，然后将它们映射到我们的 **handler.js** 函数以及我们希望它们响应的 HTTP 方法和路径。

我已经更改了默认设置，将“美国-西部-2”作为我的地区，并将“dev”作为我的舞台。用无服务器设置不同的阶段，我还没有完全探索过。

文档现在非常缺乏，但是这个配置将导致一个名为“dev-pets-service”的 API 网关被创建，尽管这并不是我们真正想要的。

API 网关不应该在它们的名字中引用环境，因为它们可以拥有多个环境或“阶段”

希望我能找到解决这个问题的方法，并在未来的编辑中发布它；)

### **部署和测试我们的服务**

现在我们准备好部署了！只需要跑步:

```
serverless deploy
```

大约一分钟后，您的 Lambda 函数就应该部署好了，API 网关也应该创建好了。

创建一个名为“Pets”的 DynamoDB 表(或者您称之为资源的任何东西)。然后前往 API 网关。找到您的“dev-pets-service”，并导航到 POST 方法。

通过点击带有闪电的“测试”按钮并使用以下数据来测试您的 API:

```
{ petId: "029340", name: "Fido", type: "dog" }
```

您应该已经成功地在数据库中创建了一个新项目！

### 下一步是什么？

您的下一步可能是为您的资源启用 CORS，为您的 API 使用自定义域名，并设置您的前端应用程序与这些端点通信。

这超出了本文的范围，应该很简单，但是如果您有问题，请在评论中告诉我。

**编辑**

Reddit 上的用户 jcready 建议改进我们的更新方法:

```
exports.update = function(event, context) {  const payload = _.reduce(event.body, (memo, value, key) => {    memo.ExpressionAttributeNames[`#${key}`] = key    memo.ExpressionAttributeValues[`:${key}`] = value    memo.UpdateExpression.push(`#${key} = :${key}`)    return memo  }, {    TableName: 'Pets',    Key: { petId: event.params.path.petId },    UpdateExpression: [],    ExpressionAttributeNames: {},    ExpressionAttributeValues: {}  })  payload.UpdateExpression = 'SET ' + payload.UpdateExpression.join(', ')  dynamo.update(payload, context.done)}
```

我们当前实现的问题是，如果一次发送两个更新请求，用户可能会覆盖另一个用户的更改。

DocumentClient 为我们提供了一个 **update** 方法，允许我们指定想要更新的字段，但是语法有点奇怪，需要生成一个“UpdateExpression”来实现这些更改。

这段代码基于传入的键构建表达式，并解决了在用户共享资源的应用程序中覆盖更新的问题。