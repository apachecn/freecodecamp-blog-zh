# 没有 API？没问题！通过模拟 API 快速开发

> 原文：<https://www.freecodecamp.org/news/rapid-development-via-mock-apis-e559087be066/>

#### 用 Node.js 通过三个快速步骤创建一个真实的模拟 API

在这个面向服务开发的时代，您需要让 JSON 进出服务器，以使您的前端活跃起来。所以 API 是必不可少的。

但是，好消息是:您不需要创建真正的 web 服务来开始。相反，只需建立一个模拟 API。

**注意:**我说 API 是为了简洁。相关术语包括 Web API、Web 服务、JSON API 和 RESTful API。

#### 为什么是模拟 API？

以下是使用模拟 API 的四个原因:

1.  **还没有 API**—可能你还没有创建 API。模拟 API 允许您开始开发，而不需要等待 API 团队来构建您需要的服务。如果你还没有决定如何设计你的 web 服务，mocking 可以让你快速地原型化不同的潜在响应形状，看看它们如何与你的应用程序一起工作。
2.  **缓慢或不可靠的 API** —您的开发或 QA 环境中的现有 API 是否缓慢、不可靠或调用成本高昂？如果是这样，模拟 API 可以为快速反馈开发提供一致的即时响应。如果您现有的 web 服务出现故障，模拟 API 允许您继续工作。
3.  **消除团队间的依赖** —是否有一个独立的团队在创建你的应用程序的 web 服务？模拟 API 意味着您可以立即开始编码，并在真正的 web 服务准备好时切换到它们。只要同意 API 提议的设计，并相应地嘲笑它。
4.  **离线工作** —最后，也许你需要在飞机上、路上或者其他连接性差的地方工作。模拟允许您离线工作，因为您的呼叫保持在本地。

### 让我们创建一个模拟 API

我发现最简单的方法是使用 Node.js。下面是我创建一个真实模拟 API 的三步过程:

1.  声明模式
2.  生成随机数据
3.  提供随机数据

让我们走完这三步。

#### **步骤 1 —声明模式**

首先，让我们使用 [JSON Schema Faker](https://github.com/json-schema-faker/json-schema-faker) 为我们的模拟 API 声明模式。这将允许我们声明我们的伪 API 应该是什么样子。我们将声明它将公开的对象和属性，包括数据类型。有一个[方便的在线 REPL](http://json-schema-faker.js.org/) ，让学习变得容易。

JSON Schema faker 通过三个开源库支持生成真实的随机数据。 [Faker.js](https://github.com/marak/Faker.js/) 、 [chance.js](http://chancejs.com/) 、 [randexp.js](https://fent.github.io/randexp.js/) 。Faker 和 chance 很像。两者都提供了各种各样的随机数据生成功能，包括真实的姓名、地址、电话号码、电子邮件等等。Randexp 基于正则表达式创建随机数据。JSON 模式 faker 允许我们在模式定义中使用 faker、chance 和 randexp。通过这种方式，您可以准确地声明您的模拟 API 中的每个属性应该如何生成。

下面是一个生成真实的随机用户数据的示例模式。我将该文件保存为 mockDataSchema.js:

```
var schema = {
  "type": "object",
  "properties": {
    "users": {
      "type": "array",
      "minItems": 3,
      "maxItems": 5,
      "items": {
        "type": "object",
        "properties": {
          "id": {
            "type": "number",
            "unique": true,
            "minimum": 1
          },
          "firstName": {
            "type": "string",
            "faker": "name.firstName"
          },
          "lastName": {
            "type": "string",
            "faker": "name.lastName"
          },
          "email": {
            "type": "string",
            "faker": "internet.email"
          }
        },
        "required": ["id", "type", "lastname", "email"]
      }
    }
  },
  "required": ["users"]
};

module.exports = schema; 
```

mockDataSchema.js

这个模式使用 faker.js 生成一组具有真实姓名和电子邮件的用户。

#### **步骤 2——生成随机数据**

一旦我们定义了模式，就该生成随机数据了。为了自动化构建任务，我更喜欢使用 npm 脚本，而不是囫囵吞枣。[下面是为什么](https://medium.freecodecamp.com/why-i-left-gulp-and-grunt-for-npm-scripts-3d6853dd22b8#.2cqrvlxhf)。

我在 package.json 中创建了一个 npm 脚本，它调用一个单独的节点脚本:

```
"generate-mock-data": "node buildScripts/generateMockData"
```

上面的脚本正在调用一个名为 generateMockData 的节点脚本。以下是 generateMockData.js 中的内容:

```
/* This script generates mock data for local development.
   This way you don't have to point to an actual API,
   but you can enjoy realistic, but randomized data,
   and rapid page loads due to local, static data.
 */

var jsf = require('json-schema-faker');
var mockDataSchema = require('./mockDataSchema');
var fs = require('fs');

var json = JSON.stringify(jsf(mockDataSchema));

fs.writeFile("./src/api/db.json", json, function (err) {
  if (err) {
    return console.log(err);
  } else {
    console.log("Mock data generated.");
  }
});
```

generateMockData.js

我在第 11 行调用了 [json-schema-faker](https://www.npmjs.com/package/json-schema-faker) ，并向它传递了我们在步骤 1 中设置的模拟数据模式。这最终将 JSON 写入 db.json，如上面第 13 行所示。

#### **步骤 3——提供随机数据**

既然我们已经向 db.json 中写入了随机的、真实的数据，那么就让我们开始吧！ [JSON 服务器](https://github.com/typicode/json-server)使用我们创建的静态 JSON 文件创建一个逼真的 API。因此，让我们将 JSON server 指向我们在步骤 2 中动态生成的模拟数据集。

```
"start-mockapi": "json-server --watch src/api/db.json --port 3001"
```

这将启动 json-server，并在端口 3001 上提供 db.json 中的数据。每个顶级对象都在 HTTP 端点上公开。

最精彩的部分是:JSON Server 通过保存对我们在步骤 2 中创建的 db.json 文件的更改来模拟真实的数据库。

> JSON 服务器的优点:它处理创建、读取、更新和删除，所以感觉非常真实。

模拟 API 的操作就像一个真正的 API 一样，但是不需要进行真正的 HTTP 调用或者建立一个真正的数据库！滑头。

这意味着我们可以在不创建真正的 API 的情况下进行开发。我们只需要就调用和数据形式达成一致，然后 UI 团队就可以继续前进，而不必等待服务团队来创建相关的服务。

总之，要将所有这些放在一起，需要 package.json 中的 3 行代码:

```
"generate-mock-data": "node buildScripts/generateMockData",
"prestart-mockapi": "npm run generate-mock-data",
"start-mockapi": "json-server --watch src/api/db.json --port 3001"
```

start-mockapi 脚本运行 json-server，并告诉它观察我们在步骤 2 中生成的 db.json。在启动模拟 API 之前，会生成模拟数据。在 start-mockapi 之前调用 prestart-mockapi 脚本，因为它以“pre”为前缀。这是 npm 脚本约定。通过这种设置，每次我们启动应用程序时，都会生成新的逼真模拟数据！

好了，我们准备好了。

键入以下内容:

```
npm run start-mockapi
```

并且加载这个:

[http://localhost:3001/users](http://localhost:3001/users)。

您应该会看到以 JSON 形式返回的用户列表。成功！

为了了解这一切是如何组合在一起的，[下面是 GitHub](https://github.com/coryhouse/mock-api-example) 上的一个工作演示。

另外，我的新“[构建 JavaScript 开发环境](https://app.pluralsight.com/library/courses/javascript-development-environment)”课程从头开始构建这个和更多内容。([免费试用](https://billing.pluralsight.com/individual/checkout))

![1*buNt_4s0mdYgZVU9ojz6Ew](img/cf862bb2a24dd92d7f85724853ca5f48.png)

最后**、**考虑一下[莫奇.欧](https://www.mocky.io)或者[fakejson.com](https://fakejson.com)简单的替代方案，不需要设置。

#### 冰山一角…

本文只讨论了从头开始创建新的 JavaScript 开发环境所需的 40 多个决策中的一个:

![1*zFePRtYWlugmbOxrzOYivQ](img/903009b1a5010e91d5de2ba2a2d2b5fc.png)

Overwhelmed yet?

我完成了所有这些决策，并从零开始构建了一个丰富的 JavaScript 开发环境。

你今天生成模拟 API 吗？有一个可供分享的替代设置吗？我很想在评论中听到你的经历。

[Cory House](https://twitter.com/housecor) 是[plur sight](http://pluralsight.com/author/cory-house)多门课程的作者，也是 reactjsconsulting.com[的首席顾问](http://www.reactjsconsulting.com)。他是微软 MVP 公司 VinSolutions 的软件架构师，在软件实践方面对软件开发人员进行国际培训，比如前端开发和干净编码。