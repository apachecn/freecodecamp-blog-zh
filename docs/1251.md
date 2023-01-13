# 如何使用无服务器和节点设置 AWS Cognito 身份验证

> 原文：<https://www.freecodecamp.org/news/aws-cognito-authentication-with-serverless-and-nodejs/>

在本文中，我们将了解如何使用 AWS Cognito、AWS Serverless 和 NodeJS 创建 REST API 应用程序进行身份验证。

我们将使用 Lambda 函数、API 网关和无服务器框架来实现这一点。

让我们从设置项目开始。

## ****项目设置****

我们的项目结构将如下所示:

![aws cognito project folder structure](img/bf6def8c2eb648c7878c99e57ce7f8de.png)

如你所见，我们将所有的 lambda 函数文件存储在一个名为 *user* 的文件夹中，将所有的实用函数存储在一个名为 *functions* 的单独文件夹中。除此之外，还有一个 *serverless.yml* 文件，它是任何基于 serverless 的项目的核心文件。

如果你想了解更多关于这个文件的信息，请查看[这个](https://devswisdom.com/use-websockets-with-aws-serverless/)帖子。

## ****Serverless.yml 文件****

让我们开始编写我们的 *serverless.yml* 文件，在这里我们将定义所有的 lambda 函数。它将保存我们注册、登录等等的逻辑。

我们还将使用不同的设置和权限来定义我们的 AWS Cognito 用户池和用户池客户端。

让我们把这个文件分成不同的部分，这样我们可以分别理解每一部分。

### **如何定义 **AWS IAM 权限和设置****

我们将从定义环境变量、无服务器项目配置、设置和 AWS IAM 权限开始。

```
service: serverless-cognito-auth

provider:
  name: aws
  runtime: nodejs14.x
  environment:
    user_pool_id: { Ref: UserPool }
    client_id: { Ref: UserClient }
  iamRoleStatements:
    - Effect: Allow
      Action:
        - cognito-idp:AdminInitiateAuth
        - cognito-idp:AdminCreateUser
        - cognito-idp:AdminSetUserPassword
      Resource: "*"
```

在`provider`块下，我们定义了多种配置和设置。让我们简单讨论一下每一部分。

#### `environment`

在这个块中，我们定义了我们希望在项目中使用的所有环境变量，比如 lambda 函数等等。

我们将 AWS 认知的用户池 id 和客户端 id 设置为用户池和客户端。

我们还引用了我们稍后将在该文件中定义的资源，所以不用担心。请理解，这些引用将为我们提供所创建的用户池和客户端的 id。

#### `iamRoleStatements`

在这个块中，我们定义了我们希望给予我们的资源的所有 AWS IAM 权限，在我们的情况下，这些权限是我们的 lambda 函数所需要的，这些函数将使用 AWS Cognito API。

要了解更多关于 AWS IAM 的信息，请查看官方的[文档](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)。

### **如何定义**λ函数****

接下来，我们将定义我们的 lambda 函数。我们将需要三个，一个用于用户注册，一个用于用户登录，最后一个用于测试私有路由。

```
functions:
  loginUser:
    handler: user/login.handler
    events:
      - http:
          path: user/login
          method: post
          cors: true

  signupUser:
    handler: user/signup.handler
    events:
      - http:
          path: user/signup
          method: post
          cors: true

  privateAPI:
    handler: user/private.handler
    events:
      - http:
          path: user/private
          method: post
          cors: true
          authorizer:
            name: PrivateAuthorizer
            type: COGNITO_USER_POOLS
            arn:
              Fn::GetAtt:
                - UserPool
                - Arn
            claims:
              - email
```

在`**events**`块中，我们定义了 lambda 函数将被调用的事件。所以在我们的例子中，我们在这里添加 HTTP 事件，这将是我们的 AWS API 网关调用。

****`authorizer`****–**在这里，我们定义了在我们的主 lambda 函数被调用之前将被调用的授权者。因此，这里我们使用 AWS Cognito authorizer 作为 API 网关，它检查每个请求是否传递了有效的访问令牌。只有这样，它才允许我们的主 lambda 函数被调用。**

**我们需要传递 AWS Cognito 用户池的 ARN，因此我们引用该资源，并通过使用`:GetAtt`函数从中获取 ARN。**

**我们还使用了`claims`块，在事件对象的主 lambda 函数中，它拥有来自解码后的访问令牌对象的特定字段。**

### ****如何定义**资源******

**最后，我们将在我们的 *serverless.yml* 文件中定义我们需要的所有资源。**

```
`resources:
  Resources:
    UserPool:
      Type: AWS::Cognito::UserPool
      Properties:
        UserPoolName: serverless-auth-pool
        Schema:
          - Name: email
            Required: true
            Mutable: true
        Policies:
          PasswordPolicy:
            MinimumLength: 6
        AutoVerifiedAttributes: ["email"]

    UserClient:
      Type: AWS::Cognito::UserPoolClient
      Properties:
        ClientName: user-pool-ui
        GenerateSecret: false
        UserPoolId: { Ref: UserPool }
        AccessTokenValidity: 5
        IdTokenValidity: 5
        ExplicitAuthFlows:
          - "ADMIN_NO_SRP_AUTH"`
```

**在这里，我们正在创建我们的 AWS Cognito 用户池和客户端。现在让我们来看一些选项。如果你想看到你可以使用的所有选项，检查一下这个官方的[文档](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpool.html)和[这个](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpoolclient.html)以及用户池客户端。**

******`Schema`*******–*******我们在这里定义将在我们的用户池中创建的用户数据的模式。我们可以定义不同的属性，如电子邮件、年龄、性别等等。******

********`Policies`*******–*****在这个模块中，我们定义了我们的密码验证策略，因此基本上所有关于密码在保存到我们的用户池之前应该如何的设置。******

********`AutoVerifiedAttributes`******–在这里我们可以设置想要自动验证的字段，如电子邮件和电话号码。通常，当在 AWS Cognito 用户池中创建新用户时，该用户必须通过一个验证过程来验证他们的电子邮件或电话号码。但是在这里设置该字段将会跳过对已创建用户的验证过程。******

********`AccessTokenValidity`******–这定义了访问令牌有效的小时数。******

********`ExplicitAuthFlows`******–这定义了用户池客户端将允许的所有认证流。我们将使用`ADMIN_NO_SRP_AUTH`,它可以用来授权用户使用用户名和密码——这就是为什么我们在这里将它作为值传递。******

****我鼓励你也去看看 AWS Cognito 的官方[文档](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)。****

## ******如何编码**λ函数********

**现在是时候开始编码我们的 REST API 逻辑了，为用户注册、用户登录和我们的私有路径创建 lambda 函数来测试一切。**

### ******用户注册******

**首先，我们将在用户文件夹中创建一个新文件，并将其命名为 *signup.js* 。这个文件将保存所有与用户注册相关的逻辑。让我们把这个文件分成几个部分，看看它的代码是什么样子。**

#### ******进口******

```
`const AWS = require('aws-sdk')
const { sendResponse, validateInput } = require("../functions");

const cognito = new AWS.CognitoIdentityServiceProvider()`
```

**我们将使用`aws-sdk` NPM 与 AWS Cognito API 进行交互。我们还导入了两个实用函数(查看代码):`sendResponse`用于发送 HTTP 请求的响应，以及`validateInput`用于验证请求主体数据。**

**我们还让 Cognito 身份提供者的实例与用户池 API 进行交互。**

#### ****如何验证**请求体数据******

```
`const isValid = validateInput(event.body)
if (!isValid)
return sendResponse(400, { message: 'Invalid input' })`
```

**这里我们验证请求主体数据，并检查数据是否有效。如果无效，我们将返回响应并发送适当的消息。**

#### ****如何在**中创建**用户 **AWS 认知用户池******

```
`const {
 email,
 password
 } = JSON.parse(event.body)
const {
 user_pool_id
 } = process.env

const params = {
  UserPoolId: user_pool_id,
  Username: email,
  UserAttributes: [{
      Name: 'email',
      Value: email
    },
    {
      Name: 'email_verified',
      Value: 'true'
    }
  ],
  MessageAction: 'SUPPRESS'
}
const response = await cognito.adminCreateUser(params).promise();`
```

**在这里，我们从请求体获得电子邮件和密码，还从环境变量对象获得用户池 id。**

**之后，我们为`adminCreateUser` API 创建一个参数对象。`MessageAction`被设置为“抑制”,因为我们不想在用户池中创建新用户时发送由 AWS Cognito 发送的默认电子邮件。**

#### ****如何为**创建的用户**** 设置**密码****

```
`if (response.User) {
  const paramsForSetPass = {
    Password: password,
    UserPoolId: user_pool_id,
    Username: email,
    Permanent: true
  };
  await cognito.adminSetUserPassword(paramsForSetPass).promise()
}
return sendResponse(200, {
  message: 'User registration successful'
})`
```

**当我们的用户在用户池中被创建时，我们需要为该用户设置密码。我们这样做是因为我们不希望用户在登录时创建密码，因为他们已经在 HTTP 请求中发送了密码。**

**这还会将 Cognito 用户池中的用户状态更改为已确认。**

**我们还需要将`Permanent`作为`true`传递，因为否则将为用户生成一个临时密码。**

### ******用户登录******

**现在我们将从用户登录开始，在*用户*文件夹中创建一个名为 *login.js* 的文件。这个登录 API 将启动身份验证过程，并将身份令牌发送给用户，用户可以使用身份令牌来访问授权的路由。**

**login.js 看起来和 *signup.js* 非常相似。唯一的区别是参数和 API 调用。**

#### ****如何启动**认证过程******

```
`const {
  email,
  password
} = JSON.parse(event.body)
const {
  user_pool_id,
  client_id
} = process.env

const params = {
  AuthFlow: "ADMIN_NO_SRP_AUTH",
  UserPoolId: user_pool_id,
  ClientId: client_id,
  AuthParameters: {
    USERNAME: email,
    PASSWORD: password
  }
}
const response = await cognito.adminInitiateAuth(params).promise();
return sendResponse(200, {
  message: 'Success',
  token: response.AuthenticationResult.IdToken
})`
```

**在这段代码中需要理解的主要事情是，我们使用`AuthFlow`作为 ADMIN_NO_SRP_AUTH，它用于根据用户名和密码对用户进行身份验证。之后，我们只需调用`adminInitiateAuth` API 并将身份令牌发送给用户。**

### ******私人路线******

**我们将增加一个 lambda 函数作为私有路由。要访问这个 API 端点，我们需要在请求头中发送一个有效的身份令牌，并带有密钥“Authorization”。**

**首先在*用户*文件夹中创建一个新文件，并将其命名为 *private.js.***

```
`module.exports.handler = async (event) => {
  return sendResponse(200, {
    message: `Email ${event.requestContext.authorizer.claims.email} has been authorized`
  })
}`
```

**在这里，我们只是从请求中获取电子邮件，并发送一个简单的响应。只有当请求通过 API 网关配置中添加的授权层时，这个 lambda 函数才会被调用。**

**要查看 Nodejs SDK 提供的所有 API，请查看这些文档。**

**另外，请查看 AWS 如何计算出 [AWS Cognito 定价](https://devswisdom.com/aws-cognito-pricing-and-features-2021/),以便您只花自己想花的钱。**

## ******结论******

**现在您有了使用 AWS Cognito、AWS Serverless 和 Nodejs 进行身份验证的 REST API。恭喜你。**

**请务必查看本文末尾给出的 GitHub 代码。您可以在当前代码中添加或改进许多东西——可以增加数据验证，可以添加忘记密码，等等。我让你决定。**

**我们也可以用 DynamoDB 做到这一点，查看 [AWS DynamoDB 定价](https://devswisdom.com/aws-dynamodb-pricing-and-features/)以了解更多信息。**

## ******获取代码******

**你可以在 Github 上找到[的源代码。](https://github.com/shivangchauhan7/serverless-auth)**

**你可以在我的网站上查看更多类似的文章。**