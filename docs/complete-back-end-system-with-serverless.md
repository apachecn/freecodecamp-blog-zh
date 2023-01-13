# 如何用无服务器构建一个完整的后端系统

> 原文：<https://www.freecodecamp.org/news/complete-back-end-system-with-serverless/>

本文将教您如何构建和部署为应用程序构建后端所需的一切。我们将使用 AWS 来托管所有这些，并使用无服务器框架来部署它们。

完成本文后，您将知道如何:

*   [设置您的 AWS 帐户以使用无服务器框架](#setup)
*   [建立一个无服务器项目并部署一个 Lambda](#firstlambda)
*   [使用 S3 桶创建私有云存储，并从您的计算机上传文件](#s3)
*   [使用 API 网关和 AWS Lambda 部署 API](#api)
*   [用 AWS DynamoDB 创建一个无服务器数据库表](#dynamo)
*   [创建一个 API 从 DynamoDB 表中获取数据](#dynamoGet)
*   [创建一个 API，将数据添加到 DynamoDB 表中](#dynamoPut)
*   [创建 API 来存储文件并从您的 S3 桶中获取文件](#s3API)
*   [用 API 密钥保护所有 API 端点](#l9apikey)

能够做所有这些事情使您能够从大多数应用程序后端创建所有需要的功能。

# ****带 AWS 的无服务器设置****

无服务器框架是一个工具，开发人员可以使用它从我们的计算机上配置和部署服务。有一点设置允许所有这些一起工作，这一节将告诉你如何做。

[https://www.youtube.com/embed/videoseries?list=PLmexTtcbIn_gP8bpsUsHfv-58KsKPsGEo](https://www.youtube.com/embed/videoseries?list=PLmexTtcbIn_gP8bpsUsHfv-58KsKPsGEo)

要允许无服务器使用您的帐户，您需要为其设置一个用户。为此，导航到 AWS 并搜索“IAM”(身份和访问管理)。

一旦进入 IAM 页面，点击左侧列表中的 **用户** 。这将打开您帐户上的用户列表。从这里我们将点击 **添加用户。**

我们需要创建一个拥有 **编程访问** 的用户，我已经将我的用户命名为**server less account**，但是名称并不重要。

![createUser-1](img/0ea19e8940fd12e145b3e1651d609d22.png)

接下来，我们需要给用户一些权限。在权限屏幕上，选择 **直接附加现有策略** ，然后选择 **管理员访问** 。这将允许无服务器框架创建它需要的所有资源。

我们不需要添加任何标签，因此我们可以直接进入 **查看** 。

![credential](img/2516c11b584ff8a6315b14238c2cd4ff.png)

在查看窗口中，您会看到用户已经获得了一个 **访问密钥 ID** 和一个 **秘密访问密钥** 。我们将在下一部分中需要这些，所以请保持此页面打开。

### ****无服务器安装和配置****

现在我们已经创建了我们的用户，我们需要在我们的机器上安装无服务器框架。

打开终端并运行此命令，在您的计算机上全局安装 Serverless。如果您还没有安装 NodeJS，请查看此页面。

```
npm install -g serverless
```

现在我们已经安装了无服务器，我们需要设置无服务器使用的凭证。运行该命令，将您的 **访问密钥 ID** 和 **秘密访问密钥** 放在正确的位置:

```
serverless config credentials --provider aws --key ${Your access key ID} --secret ${Your secret access key} --profile serverlessUser
```

一旦运行了这个程序，你就可以设置无服务器了。

# ****部署你的第一个 AWS Lambda****

在没有设置 serverlessUser 的情况下，我们希望使用 Serverless 框架部署一些东西。我们可以使用无服务器模板来设置一个可以部署的基本项目。这将是整个无服务器项目的基础。

[https://www.youtube.com/embed/sku9Rrci-tE?feature=oembed](https://www.youtube.com/embed/sku9Rrci-tE?feature=oembed)

在您的终端中，我们可以从模板创建一个无服务器项目。该命令将在`myServerlessProject`文件夹中创建一个 NodeJS 无服务器项目:

```
serverless create --template aws-nodejs --path myServerlessProject 
```

如果您现在在代码编辑器中打开该文件夹，我们可以看到我们创建的内容。

![folderStruct](img/8db59d97f69ce4a167122c3a220cab54.png)

我们有两个值得讨论文件:`handler.js`和`serverless.yml`

### ****handler . js****

该文件是一个函数，将作为 Lambda 函数上传到您的 AWS 帐户。Lambda 函数很棒，我们将在本系列的后面更多地使用它们。

### ****无服务器。yml****

这对我们来说是一份非常重要的文件。这是我们部署的所有配置所在的位置。它告诉 Serverless 使用什么运行时，部署到哪个帐户，以及部署什么。

我们需要对该文件进行更改，以便我们的部署能够正常工作。在`provider`对象中，我们需要添加一行新的`profile: serverlessUser`。这告诉 Serverless 使用我们在上一节中创建的 AWS 凭证。

我们可以向下滚动到`functions`，看到我们有一个名为`hello`的函数，它指向`handler.js`文件中的函数。这意味着我们将部署这个 Lambda 函数作为这个项目的一部分。

在本文的后面，我们将了解更多关于这个`serverless.yml`文件的信息。

## ****部署我们的项目****

现在我们已经看了文件，是时候进行第一次部署了。打开一个终端，导航到我们的项目文件夹。部署非常简单，只需输入:

```
serverless deploy 
```

这需要一段时间，但当它完成时，我们可以检查一切都已成功部署。

打开浏览器，导航到您的 AWS 帐户。搜索`Lambda`，你会看到所有 Lambda 函数的列表。(如果您没有看到任何，请检查您的区域是否设置为`N. Virginia`)。您应该看到`myserverlessproject-dev-hello` Lambda，它包含项目文件夹中`handler.js`文件中的确切代码。

# ****部署 S3 桶上传文件****

在本节中，我们将学习如何部署亚马逊 S3 存储桶，然后从我们的计算机同步文件。这就是我们如何开始使用 S3 作为我们的文件云存储。

[https://www.youtube.com/embed/8dc72i41r1A?feature=oembed](https://www.youtube.com/embed/8dc72i41r1A?feature=oembed)

打开`serverless.yml`文件，删除所有注释掉的行。滚动到文件底部，添加以下代码以包含我们的 S3 资源:

```
resources:
    Resources:
        DemoBucketUpload:
            Type: AWS::S3::Bucket
            Properties:
                BucketName: EnterAUniqueBucketNameHere 
```

更改存储桶的名称，我们就可以再次部署了。再次打开您的终端并运行`serverless deploy`。您可能会得到一个错误，指出 bucket 名称不唯一，在这种情况下，您需要更改 bucket 名称，保存文件并重新运行命令。

如果成功了，我们就可以通过浏览器在 AWS 控制台上看到我们的新 S3 桶了。搜索`S3`，然后您应该会看到您新创建的 bucket。

## ****同步你的文件****

有一个桶很好，但是现在我们需要把文件放入桶中。我们将使用一个名为 S3 同步的无服务器插件来完成这项工作。要将这个插件添加到我们的项目中，我们需要定义插件。在 provider 对象之后，添加以下代码:

```
plugins:
    - serverless-s3-sync
```

这个插件还需要一些自定义配置，所以我们在我们的`serverless.yml`文件中添加了另一个字段，为您更改了 bucket 名称:

```
custom:
    s3Sync:
        - bucketName: YourUniqueBucketName
          localDir: UploadData 
```

这段代码告诉 S3 同步插件将`UploadData`文件夹的内容上传到我们的桶中。我们目前没有该文件夹，所以我们需要创建它并添加一些文件。你可以添加一个文本文件，一张图片，或者任何你想上传的东西，只要确保文件夹里至少有一个文件。

我们需要做的最后一件事是安装插件。幸运的是，所有无服务器插件也是 npm 包，所以我们可以通过在我们的终端运行`npm install --save-dev serverless-s3-sync`来安装它。

正如我们之前所做的，我们现在可以运行`serverless deploy`并等待部署完成。一旦完成，我们可以返回到我们的浏览器和我们的桶中，我们应该看到我们放在项目的`UploadData`文件夹中的所有文件。

![Screenshot-2019-12-10-at-07.12.07](img/f16f92d0cec1826dc252d63d29f5394b.png)

# ****用 Lambda 和 API 网关**创建 API**

在这一节中，我们将学习无服务器最有用的事情之一:创建一个 API。创建一个 API 允许你做很多事情，从从数据库、S3 存储中获取数据，访问其他 API，等等！

[https://www.youtube.com/embed/Jruqo0KVOWk?feature=oembed](https://www.youtube.com/embed/Jruqo0KVOWk?feature=oembed)

为了创建 API，我们首先需要创建一个新的 Lambda 函数来处理请求。我们将制作几个 Lambdas，所以我们将在项目中创建一个`lambdas`文件夹，其中包含两个子文件夹`common`和`endpoints`。

![Screenshot-2019-12-10-at-07.47.27](img/09efb2364e54b9d49db40cf478add6f9.png)

在端点文件夹中，我们可以添加一个名为`getUser.js`的新文件。这个 API 将允许某人基于用户的 ID 发出请求并获取数据。这是 API 的代码:

```
const Responses = require('../common/API_Responses');

exports.handler = async event => {
    console.log('event', event);

    if (!event.pathParameters || !event.pathParameters.ID) {
        // failed without an ID
        return Responses._400({ message: 'missing the ID from the path' });
    }

    let ID = event.pathParameters.ID;

    if (data[ID]) {
        // return the data
        return Responses._200(data[ID]);
    }

    //failed as ID not in the data
    return Responses._400({ message: 'no ID in data' });
};

const data = {
    1234: { name: 'Anna Jones', age: 25, job: 'journalist' },
    7893: { name: 'Chris Smith', age: 52, job: 'teacher' },
    5132: { name: 'Tom Hague', age: 23, job: 'plasterer' },
}; 
```

如果请求不包含 ID，那么我们返回一个失败的响应。如果有该 ID 的数据，那么我们返回该数据。如果没有该用户 ID 的数据，那么我们也返回一个失败响应。

您可能已经注意到，我们需要来自`API_Responses`的`Responses`对象。这些响应对于我们制作的每个 API 来说都是常见的，所以让这些代码可导入是一个明智的举动。在`common`文件夹中创建一个名为`API_Responses.js`的新文件，并添加以下代码:

```
const Responses = {
    _200(data = {}) {
        return {
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Methods': '*',
                'Access-Control-Allow-Origin': '*',
            },
            statusCode: 200,
            body: JSON.stringify(data),
        };
    },

    _400(data = {}) {
        return {
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Methods': '*',
                'Access-Control-Allow-Origin': '*',
            },
            statusCode: 400,
            body: JSON.stringify(data),
        };
    },
};

module.exports = Responses; 
```

这组函数用于简化在使用带有 API 网关的 Lambda 时所需的正确响应的创建(我们将在一秒钟内完成)。这些方法添加标头、状态代码和需要返回的任何数据的字符串。

现在我们有了 API 的代码，我们需要在我们的`serverless.yml`文件中设置它。滚动到`serverless.yml`文件的`functions`部分。在本指南的最后一部分，我们部署了`hello`功能，但是我们不再需要它了。删除 functions 对象并替换为:

```
functions:
    getUser:
        handler: lambdas/endpoints/getUser.handler
        events:
            - http:
                  path: get-user/{ID}
                  method: GET
                  cors: true 
```

这段代码正在创建一个名为`getUser`的新 Lambda 函数，它在`lambdas/getUser`的文件中，在`handler`的方法上。然后我们定义可以触发这个 lambda 函数运行的事件。

为了让 Lambda 成为 API，我们可以添加一个`http`事件。这告诉 Serverless 向该帐户添加一个 API 网关，然后我们可以使用`path`定义 API 端点。在这种情况下，`get-user/{ID}`意味着 URL 将是`https://${something-provided-by-API-Gateway}/get-user/{ID}`，其中 ID 作为路径参数传递给 Lambda。我们还将方法设置为`GET`，并启用 CORS，这样我们就可以从前端应用程序访问这个端点。

我们现在可以再次部署，这一次我们可以使用简写命令`sls deploy`。这只会节省一些字符，但有助于避免大量的错别字。完成后，我们将得到一个输出，其中还包括一个端点列表。我们可以复制我们的端点，然后用浏览器来测试它。

![Screenshot-2019-12-10-at-07.19.00](img/74523ace54351cdc901af71f2c674c09.png)

如果我们将 API URL 粘贴到浏览器中，然后在 5132 的末尾添加一个 ID，我们应该会得到一个响应`{ name: 'Tom Hague', age: 23, job: 'plasterer' }`。如果我们输入不同的 ID，如 1234，我们将得到不同的数据，但是输入 ID 7890 或不输入 ID 将返回错误。

如果我们想向我们的 API 添加更多的数据，我们可以简单地向`getUser.js`文件中的数据对象添加一个新行。然后我们可以运行一个特殊的命令，该命令只部署一个功能`sls deploy -f ${functionName}`。所以对我们来说就是:

```
sls deploy -f getUser 
```

如果您现在使用新数据的 ID 发出请求，API 将返回新数据而不是错误。

# ****在 AWS 上创建数据库****

DynamoDB 是 AWS 上完全托管的非关系数据库。这是存储您需要定期访问和更新的数据的完美解决方案。在这一节中，我们将学习如何使用无服务器创建 DynamoDB 表。

[https://www.youtube.com/embed/1de8NkTseqM?feature=oembed](https://www.youtube.com/embed/1de8NkTseqM?feature=oembed)

在我们的`serverless.yml`文件中，我们将向`Resources`部分添加一些配置:

```
resources:
    Resources:
        DemoBucketUpload:
            Type: AWS::S3::Bucket
            Properties:
                BucketName: ${self:custom.bucketName}
        # New Code
        MyDynamoDbTable:
            Type: AWS::DynamoDB::Table
            Properties:
                TableName: ${self:custom.tableName}
                AttributeDefinitions:
                    - AttributeName: ID
                      AttributeType: S
                KeySchema:
                    - AttributeName: ID
                      KeyType: HASH
                BillingMode: PAY_PER_REQUEST 
```

在这段代码中，我们可以看到我们正在创建一个新的 DynamoDB 表，它的`TableName`为`${self:custom.tableName}`，定义了一个属性`ID`，并将计费模式设置为按请求付费。

这是我们第一次看到`serverless.yml`文件中变量的使用。我们可以因为一些原因使用变量，它们可以使我们的工作更容易。在这种情况下，我们引用变量`custom.tableName`。然后，我们可以从多个位置引用这个变量，而不必复制和粘贴表名。为了让这个工作，我们还需要添加`tableName`到自定义部分。在我们的例子中，我们将添加行`tableName: player-points`来创建一个存储玩家点数的表。这个表名只需要对您的帐户是唯一的。

定义表时，您需要定义至少一个字段，该字段将是您的唯一标识字段。因为 DynamoDB 是非关系数据库，所以不需要定义完整的模式。在我们的例子中，我们已经定义了`ID`，声明它有一个 string 属性类型和一个 key 类型`HASH`。

定义的最后一部分是计费模式。有两种支付 DynamoDB 的方式:

*   按请求付费
*   提供的资源。

Provisioned resources 允许您定义要向表中读取和写入多少数据。这样做的问题是，如果您开始使用更多的资源，您的请求就会受到限制，即使没有人使用它，您也要为资源付费。

按请求付费简单多了，因为你只需按请求付费。这意味着如果你没有人使用它，那么你不用支付任何费用，如果你有数百人同时使用它，所有的请求都有效。对于这种增加的灵活性，您需要为每次请求支付稍微多一点的费用，但是从长远来看，这通常会更便宜。

一旦我们再次运行`sls deploy`，我们就可以打开 AWS 控制台，搜索 DynamoDB。我们应该能够看到我们的新表，我们可以看到那里什么也没有。

要向表中添加数据，单击`Create item`，给它一个唯一的 ID，单击加号按钮和`append`到一个新字段，并选择字符串的类型。我们需要给它一个字段`name`和值`Jess`。增加一个设置为`12`的`score`数字域。单击 save，现在 dynamo 表中就有数据了。

![Screenshot-2019-12-10-at-07.57.54](img/efc7d458ff203eb1a09ae59a1140a15d.png)

# ****从 DynamoDB 表中获取数据****

现在我们已经创建了 Dynamo 表，我们希望能够获取数据并向表中添加数据。我们将从使用 get 端点从表中获取数据开始。

[https://www.youtube.com/embed/CpDFfSXRG04?feature=oembed](https://www.youtube.com/embed/CpDFfSXRG04?feature=oembed)

我们将在我们的`endpoints`文件夹中创建一个名为`getPlayerScore.js`的新文件。这个 Lambda 端点将为用户处理请求，并从 Dynamo 表中获取数据。

```
const Responses = require('../common/API_Responses');
const Dynamo = require('../common/Dynamo');

const tableName = process.env.tableName;

exports.handler = async event => {
    console.log('event', event);

    if (!event.pathParameters || !event.pathParameters.ID) {
        // failed without an ID
        return Responses._400({ message: 'missing the ID from the path' });
    }

    let ID = event.pathParameters.ID;

    const user = await Dynamo.get(ID, tableName).catch(err => {
        console.log('error in Dynamo Get', err);
        return null;
    });

    if (!user) {
        return Responses._400({ message: 'Failed to get user by ID' });
    }

    return Responses._200({ user });
}; 
```

这里使用的代码与`getUser.js`文件中的代码非常相似。我们检查 ID 为的路径参数是否存在，获取用户数据，然后返回用户。主要的区别是我们如何获得用户。

我们已经导入了`Dynamo`函数对象，并正在调用`Dynamo.get`。我们传入 ID 和表名，然后捕捉任何错误。我们现在需要在公共文件夹中一个名为`Dynamo.js`的新文件中创建那个`Dynamo`函数对象。

```
const AWS = require('aws-sdk');

const documentClient = new AWS.DynamoDB.DocumentClient();

const Dynamo = {
    async get(ID, TableName) {
        const params = {
            TableName,
            Key: {
                ID,
            },
        };

        const data = await documentClient.get(params).promise();

        if (!data || !data.Item) {
            throw Error(`There was an error fetching the data for ID of ${ID} from ${TableName}`);
        }
        console.log(data);

        return data.Item;
    },
};
module.exports = Dynamo; 
```

读写 Dynamo 需要合理数量的代码。每当我们想使用 Dynamo 时，我们都可以编写这样的代码，但是用函数来简化我们的过程要干净得多。

该文件首先导入 AWS，然后创建 DynamoDB 文档客户机的一个实例。文档客户端是我们与 Lambdas 中的 Dynamo 一起工作的最简单的方式。我们用异步 get 函数创建了一个`Dynamo`对象。我们发出请求只需要一个 ID 和一个表名。我们将它们格式化为 DocumentClient 的正确参数格式，等待一个`documentClient.get`请求，并确保在末尾添加了`.promise()`。这就把请求从回叫变成了更容易处理的承诺。我们检查我们设法从迪纳摩得到一个项目，然后我们返回那个项目。

我们现在有了我们需要的所有代码，我们还必须更新我们的`serverless.yml`文件。要做的第一件事是通过将我们的新 API 端点添加到我们的函数列表中来添加它。

```
 getPlayerScore:
        handler: lambdas/endpoints/getPlayerScore.handler
        events:
            - http:
                  path: get-player-score/{ID}
                  method: GET
                  cors: true 
```

为了让我们的端点正常工作，我们还需要做两个更改:

*   环境变量
*   许可

您可能已经注意到在`getPlayerScore.js`文件中我们有这样一行代码:

```
const tableName = process.env.tableName; 
```

这就是我们从 Lambda 的环境变量中获取表名的地方。为了用正确的环境变量创建我们的 Lambda，我们需要在提供者中设置一个名为`environment`的新对象，其字段为`tableName`，值为`${self:custom.tableName}`。这将确保我们请求更正表。

我们还需要给我们的 Lambdas 访问 Dynamo 的权限。我们必须向提供者添加另一个名为`iamRoleStatements`的字段。它有一系列策略，可以允许或禁止对某些服务或资源的访问:

```
provider:
    name: aws
    runtime: nodejs10.x
    profile: serverlessUser
    region: eu-west-1
    environment:
        tableName: ${self:custom.tableName}
    iamRoleStatements:
        - Effect: Allow
          Action:
              - dynamodb:*
          Resource: '*' 
```

由于所有这些都已添加到 provider 对象中，因此它将应用于所有 Lambdas。

我们现在可以再次运行`sls deploy`来部署我们的新端点。完成后，我们应该得到一个带有新端点`https://${something-provided-by-API-Gateway}/get-player-score/{ID}`的输出。如果我们将该 URL 复制到浏览器选项卡中，并添加我们在上一节中创建的播放器的 ID，我们应该会得到一个响应。

![Screenshot-2019-12-10-at-07.59.02](img/220d55c8d33e0f303b5dedde51b3e00c.png)

# ****向 DynamoDB 添加新数据****

能够从 Dynamo 获取数据很酷，但是如果我们不能同时向表中添加新数据，那就没什么用了。我们将创建一个 POST 端点，以便在 Dynamo 表中创建新数据。

[https://www.youtube.com/embed/AguTaMQGACE?feature=oembed](https://www.youtube.com/embed/AguTaMQGACE?feature=oembed)

首先在我们的端点文件夹中创建一个名为`createPlayerScore.js`的新文件，并添加以下代码:

```
const Responses = require('../common/API_Responses');
const Dynamo = require('../common/Dynamo');

const tableName = process.env.tableName;

exports.handler = async event => {
    console.log('event', event);

    if (!event.pathParameters || !event.pathParameters.ID) {
        // failed without an ID
        return Responses._400({ message: 'missing the ID from the path' });
    }

    let ID = event.pathParameters.ID;
    const user = JSON.parse(event.body);
    user.ID = ID;

    const newUser = await Dynamo.write(user, tableName).catch(err => {
        console.log('error in dynamo write', err);
        return null;
    });

    if (!newUser) {
        return Responses._400({ message: 'Failed to write user by ID' });
    }

    return Responses._200({ newUser });
}; 
```

这段代码与`getPlayerScore`代码非常相似，只是有一些变化。我们从请求体中获取用户，将 ID 添加到用户，然后将其传递给一个`Dynamo.write`函数。在将事件传递给 Lambda 之前，我们需要解析 API Gateway 字符串形式的事件体。

我们现在需要修改公共的`Dynamo.js`文件来添加`.write`方法。这执行与`.get`函数非常相似的步骤，并返回新创建的数据:

```
 async write(data, TableName) {
        if (!data.ID) {
            throw Error('no ID on the data');
        }

        const params = {
            TableName,
            Item: data,
        };

        const res = await documentClient.put(params).promise();

        if (!res) {
            throw Error(`There was an error inserting ID of ${data.ID} in table ${TableName}`);
        }

        return data;
    } 
```

我们已经创建了端点和公共代码，所以我们需要做的最后一件事是修改`serverless.yml`文件。因为我们在上一节中添加了环境变量和权限，所以我们只需要添加函数和 API 配置。这个端点与前两个不同，因为方法是`POST`而不是`GET`:

```
 createPlayerScore:
        handler: lambdas/endpoints/createPlayerScore.handler
        events:
            - http:
                  path: create-player-score/{ID}
                  method: POST
                  cors: true 
```

使用`sls deploy`部署它将创建三个端点，包括我们的`create-player-score`端点。测试一个`POST`端点比测试一个`GET`请求更复杂，但是幸运的是有工具可以帮助我们。我使用 [Postman](https://www.getpostman.com/) 来测试我所有的端点，因为它让测试变得快速而简单。

创建一个新请求并粘贴到您的`create-player-score` URL 中。您需要将请求类型更改为`POST`，并在 URL 末尾设置 ID。因为我们正在进行 POST 请求，所以我们可以在请求体中发送数据。点击`body`，然后点击`raw`，选择`JSON`作为身体类型。然后，您可以添加要放入表中的数据。当您点击`Send`时，您应该会得到一个成功的响应:

![postman](img/2c83e23bc31f4bd0d7e27d8a370e7535.png)

为了验证您的数据已经被添加到表中，您可以使用刚刚创建的新数据的 ID 发出 get-player-score 请求。您也可以进入 Dynamo 控制台，查看表中的所有项目。

# ****创建 S3 获取和发布端点****

Dynamo 是一个出色的数据库存储解决方案，但有时它不是最好的存储解决方案。如果你有不会改变的数据，你想节省一些钱，或者如果你想存储 JSON 以外的文件，那么你可以考虑亚马逊 S3。

[https://www.youtube.com/embed/MlKpK0WqTSs?feature=oembed](https://www.youtube.com/embed/MlKpK0WqTSs?feature=oembed)

在 S3 创建端点来获取和创建文件与 DynamoDB 非常相似。我们需要创建两个端点文件，一个公共 S3 文件，并修改`serverless.yml`文件。

我们将从向 S3 添加一个文件开始。在端点文件夹中创建一个`createFile.js`文件，并添加以下代码:

```
const Responses = require('../common/API_Responses');
const S3 = require('../common/S3');

const bucket = process.env.bucketName;

exports.handler = async event => {
    console.log('event', event);

    if (!event.pathParameters || !event.pathParameters.fileName) {
        // failed without an fileName
        return Responses._400({ message: 'missing the fileName from the path' });
    }

    let fileName = event.pathParameters.fileName;
    const data = JSON.parse(event.body);

    const newData = await S3.write(data, fileName, bucket).catch(err => {
        console.log('error in S3 write', err);
        return null;
    });

    if (!newData) {
        return Responses._400({ message: 'Failed to write data by filename' });
    }

    return Responses._200({ newData });
}; 
```

该代码与`createPlayerScore.js`代码几乎相同，但使用了`filename`代替了`ID`和`S3.write`代替了`Dynamo.write`。

现在我们需要创建我们的`S3`公共代码来简化对 S3 的请求:

```
const AWS = require('aws-sdk');
const s3Client = new AWS.S3();

const S3 = {
    async write(data, fileName, bucket) {
        const params = {
            Bucket: bucket,
            Body: JSON.stringify(data),
            Key: fileName,
        };
        const newData = await s3Client.putObject(params).promise();
        if (!newData) {
            throw Error('there was an error writing the file');
        }
        return newData;
    },
};
module.exports = S3; 
```

同样，这个文件中的代码与`Dynamo.js`中的代码非常相似，只是请求的参数有一些不同。

为了写入 S3，我们需要做的最后一件事是更改`severless.yml`文件。我们需要做四件事:添加环境变量，添加权限，添加函数，以及添加一个 S3 桶。

在 provider 中，我们可以添加一个新的环境变量`bucketName: ${self:custom.s3UploadBucket}`。

要向 S3 添加读写权限，我们可以向现有策略添加新权限。在`- dynamodb:*`之后，我们可以添加一行`- s3:*`。

添加函数与我们对所有其他函数所做的一样。确保路径有一个参数`fileName`，因为这是您在端点代码中检查的内容:

```
 createFile:
        handler: lambdas/endpoints/createFile.handler
        events:
            - http:
                  path: create-file/{fileName}
                  method: POST
                  cors: true 
```

最后，我们需要创建一个新的 bucket 来上传这些文件。在`custom`部分，我们需要添加一个新字段`s3UploadBucket`，并将其设置为一个唯一的 bucket 名称。我们还需要配置资源。在 Dynamo 表配置之后，我们可以添加它来为我们的文件上传创建一个新的 bucket:

```
 s3UploadBucket:
            Type: AWS::S3::Bucket
            Properties:
                BucketName: ${self:custom.s3UploadBucket} 
```

有了这个设置，是时候再次部署了。再次运行`sls deploy`将部署新的上传桶以及 S3 写端点。为了测试写端点，我们需要回到 Postman。

复制您在无服务器完成部署时获得的`create-file` URL，并将其粘贴到 Postman 中，并将请求类型更改为`POST`。接下来，我们需要做的是添加我们上传的文件名。在我们的例子中，我们将上传`car.json`。我们需要做的最后一件事是将数据添加到请求中。选择`Body`，然后选择`JSON`类型的`raw`。您可以添加任何想要的 JSON 数据，但这里有一些示例数据:

```
{
	"model": "Ford Focus",
	"year": 2018,
	"colour": "red"
}
```

当您发布这些数据时，您应该得到一个带有文件引用的`ETag`的`200`响应。进入控制台和你的新 S3 桶你应该能看到`car.json`。

## ****从 S3 获取数据****

现在，我们可以上传数据到 S3，我们希望能够得到它回来了。我们首先在端点文件夹中创建一个`getFile.js`文件:

```
const Responses = require('../common/API_Responses');
const S3 = require('../common/S3');

const bucket = process.env.bucketName;

exports.handler = async event => {
    console.log('event', event);

    if (!event.pathParameters || !event.pathParameters.fileName) {
        // failed without an fileName
        return Responses._400({ message: 'missing the fileName from the path' });
    }

    const fileName = event.pathParameters.fileName;

    const file = await S3.get(fileName, bucket).catch(err => {
        console.log('error in S3 get', err);
        return null;
    });

    if (!file) {
        return Responses._400({ message: 'Failed to read data by filename' });
    }

    return Responses._200({ file });
};
```

这应该与我们之前创建的`GET`端点非常相似。不同之处在于使用了`fileName`路径参数、`S3.get`和返回文件。

在普通的`s3.js`文件中，我们需要添加`get`函数。这与从迪纳摩获得的主要区别在于，当我们从 S3 获得时，结果不是 JSON 响应，而是`Buffer`。这意味着，如果我们上传一个 JSON 文件，它不会以 JSON 格式返回，所以我们检查我们是否正在获取一个 JSON 文件，然后将它转换回 JSON:

```
 async get(fileName, bucket) {
        const params = {
            Bucket: bucket,
            Key: fileName,
        };
        let data = await s3Client.getObject(params).promise();
        if (!data) {
            throw Error(`Failed to get file ${fileName}, from ${bucket}`);
        }
        if (fileName.slice(fileName.length - 4, fileName.length) == 'json') {
            data = data.Body.toString();
        }
        return data;
    } 
```

回到我们的`serverless.yml`文件，我们可以添加一个新的函数和端点来获取文件。我们已经配置了权限和环境变量:

```
 getFile:
        handler: lambdas/endpoints/getFile.handler
        events:
            - http:
                  path: get-file/{fileName}
                  method: GET
                  cors: true
```

当我们创建一个新的端点时，我们需要使用`sls deploy`再次进行完整部署。然后，我们可以将新的`get-file`端点粘贴到浏览器或邮递员中。如果我们将`car.json`添加到请求的末尾，我们将会收到我们在本节前面上传的 JSON 数据。

# ****使用 API 密钥保护您的端点****

能够使用无服务器快速轻松地创建 API 端点对于启动项目和创建概念验证来说是非常棒的。当创建应用程序的生产版本时，您需要开始更加小心谁可以访问您的端点。你不希望任何人能够攻击你的 API。

[https://www.youtube.com/embed/n5aSq1L5nIw?feature=oembed](https://www.youtube.com/embed/n5aSq1L5nIw?feature=oembed)

为了保护您的 API，有许多方法，在这一节中，我们将实现 API 密钥。如果您没有将 API 密钥与请求一起传递，那么请求会失败，并显示一条未经授权的消息。然后，您可以控制将 API 密钥交给谁，以及谁可以访问您的 API。

您还可以将使用策略添加到您的 API 密钥中，这样您就可以控制每个人使用您的 API 的程度。这允许您为您的服务创建分层使用计划。

首先，我们将创建一个简单的 API 键。为此，我们需要进入我们的`serverless.yml`文件，并向提供者添加一些配置。

```
 apiKeys:
		myFirstAPIKey
```

这将创建一个新的 API 密钥。现在我们需要告诉 Serverless 要用 API 密钥保护哪些 API 端点。这样做是为了让我们可以保护一些 API，同时让一些 API 保持公开。我们通过添加选项`private: true`来指定需要保护的端点:

```
 getUser:
        handler: lambdas/endpoints/getUser.handler
        events:
            - http:
                  path: get-user/{ID}
                  method: GET
                  cors: true
                  private: true
```

然后，您可以将该字段添加到任意数量的 API 中。要部署它，我们可以再次运行`sls deploy`。完成后，您将在返回值中获得一个 API 键。这非常重要，我们很快就会用到它。如果您尝试向您的`get-user` API 发出请求，您应该会得到一个 401 未授权错误。

为了让请求成功，现在需要在请求的头中传递一个 API 键。为此，我们需要使用 Postman 或另一个 API 请求工具，并在 get 请求中添加头。我们通过使用`API type`选择`Authorisation`来做到这一点。密钥需要是`X-API-KEY`，值是您从无服务器部署中获得的输出密钥:

![Screenshot-2019-12-20-at-20.05.53](img/bfaa65ede1204f18a2c1c96f5872d76b.png)

现在，当我们发出请求时，我们会得到一个成功的响应。这意味着唯一可以访问你的 API 的人是你已经给了你的 API 密匙的人。

这很好，但我们可以做得更多。我们可以向这个 API 键添加一个使用策略。在这里，我们可以限制一个月的请求数量以及请求的速率。这对于运行 SAAS 产品非常有用，因为您可以提供一个 API 键，为用户提供一定数量的 API 调用。

要创建使用计划，我们需要在提供程序中添加一个新对象。`quota`部分定义了使用这个 API 键可以发出多少个请求。如果更适合您的应用，您可以将周期更改为`DAY`或`WEEK`。

`throttle`部分允许您控制 API 端点被点击的频率。添加一个节流器`rate limit`设置每秒的最大请求数。这非常有用，因为它可以阻止人们发起拒绝服务攻击。`burstLimit`允许 API 比你的`rateLimit`更频繁地被攻击，但只持续很短的时间，通常只有几秒钟:

```
 usagePlan:
        quota:
            limit: 10
            period: MONTH
        throttle:
            burstLimit: 2
            rateLimit: 1
```

如果我们再次部署它，部署将会失败，因为我们将会尝试部署相同的 API 键。API 键需要是唯一的，所以我们必须更改 API 键的名称。当我们部署它并将新的 API 密钥复制到 Postman 中时，我们将能够像平常一样发出请求。如果我们尝试每秒发出太多请求或者达到最大请求数，那么我们将得到一个 429 错误

```
{
    "message": "Limit Exceeded"
}
```

这意味着下个月之前你不能再使用这个 API 密匙了。

虽然创建一个使用计划是很好的，但是你经常想给不同的人不同级别的访问权限。你可以给免费用户每月 100 个请求，付费用户每月 1000 个。你可能想要不同的付款计划，给出不同数量的请求。您可能还希望自己有一个主 API 密钥，它有无限的请求！

为此，我们可以设置多组 API 键，每组都有自己的使用策略。我们需要更改`apiKeys`和`usagePlan`部分:

```
 apiKeys:
        - free:
              - MyAPIKey3
        - paid:
              - MyPaidKey3
    usagePlan:
        - free:
              quota:
                  limit: 10
                  period: MONTH
              throttle:
                  burstLimit: 2
                  rateLimit: 1
        - paid:
              quota:
                  period: MONTH
                  limit: 1000
              throttle:
                  burstLimit: 20
                  rateLimit: 10
```

一旦您保存并部署了它，您将获得两个新的 API 键，每个键对您的 API 端点具有不同的访问级别。

感谢您阅读本指南！如果你觉得它有用，请订阅我的 Youtube 频道,在那里我每周发布关于无服务器和软件开发的视频。