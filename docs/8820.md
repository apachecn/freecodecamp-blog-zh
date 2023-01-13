# 使用 AWS Cognito 的用户管理— (2/3)核心功能

> 原文：<https://www.freecodecamp.org/news/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4/>

作者:黄

# 使用 AWS Cognito 的用户管理— (2/3)核心功能

#### 完整的 AWS 网络样板——教程 1B

![d7d793oXovpHSua0AKqYS5ebesZR56-i7gXF](img/3d656462ce2ab35d8843c5d66f24ca68.png)

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

点击下载 Github [。](https://github.com/kangzeroo/Kangzeroos-AWS-Cognito-Boilerplate)

### Javascript Cognito SDK

太好了！只有在完成 Cognito 和 Federated Identities 的初始设置后，您才应该出现在这里。现在我们已经设置好了所有的东西，是时候遍历 Javascript 代码了。在 Github 上下载 [Kangzeroo 样板文件，并确保进入我们将要工作的 Cognito 分支。](https://github.com/kangzeroo/Kangzeroos-ES6-React-Redux-Boilerplate)

```
$ git clone https://github.com/kangzeroo/Kangzeroos-Complete-AWS-Web-Boilerplate.git$ cd Kangzeroos-Complete-AWS-Web-Boilerplate$ git checkout Cognito$ cd App
```

样板文件使用在 github 上找到的 [Amazon-Cognito-Identity-JS 库。这个库使得使用 programatic AWS Cognito 变得非常容易，但是在原生的`aws-sdk`中也可以找到相同的功能。因此，让我们去安装我们的依赖关系，并加载应用程序。](https://github.com/aws/amazon-cognito-identity-js)

```
$ npm install$ npm run start
```

#### AWS 配置文件设置

导航到`App/src/components/Auth`，在这里我们将找到所有与 Cognito 认证相关的 React 组件。是的，本教程使用 React，但是您可以轻松地将相同的课程应用于其他 JS 框架。转到`App/src/api/aws/aws-cognito.js`，这是 AWS Cognito 代码的主要部分。让我们看看依赖关系，以及如何设置我们自己的 AWS 概要文件。

```
// aws-cognito.js
```

```
import { CognitoUserPool, CognitoUserAttribute, CognitoUser, AuthenticationDetails, CognitoIdentityCredentials, WebIdentityCredentials } from 'amazon-cognito-identity-js';
```

```
import { userPool, LANDLORD_USERPOOL_ID, LANDLORD_IDENTITY_POOL_ID, TENANT_IDENTITY_POOL_ID } from './aws_profile'
```

```
import uuid from 'node-uuid'
```

我们从`amazon-cognito-identity-js`和`./aws_profile.js`导入了各种函数。`amazon-cognito-identity-js`中的函数将在我们进行的过程中解释。我们想要关注的是`./aws_profile.js`的东西。这里是我们放置认知参数的地方，比如我们的 userPoolId 和 AppIds。让我们来看看`./aws_profile.js`。

```
import { CognitoUserPool } from 'amazon-cognito-identity-js';import 'amazon-cognito-js'
```

```
const REGION = "us-east-1"const USER_POOL_ID = 'us-east-1_6i5p2Fwao'const CLIENT_ID = '5jr0qvudipsikhk2n1ltcq684b'
```

```
AWS.config.update({ region: REGION})
```

```
const userData = {    UserPoolId : USER_POOL_ID,    ClientId : CLIENT_ID}
```

```
export const userPool = new CognitoUserPool(userData);
```

```
export const USERPOOL_ID = 'cognito-idp.'+REGION+'.amazonaws.com/'+USER_POOL_ID
```

```
export const IDENTITY_POOL_ID = 'us-east-1:65bd1e7d-546c-4f8c-b1bc-9e3e571cfaa7'
```

在这里，我们正在设置我们的 AWS `region`、Cognito `USER_POOL_ID`和 Cognito App `CLIENT_ID`。我们还从我们的`USER_POOL_ID`和`CLIENT_ID`创建了一个`CognitoUserPool`对象，它包含了我们大部分的认知功能。`CognitoUserPool`具有从密码重置到认证新用户的各种功能。我们在这里创建`CognitoUserPool`，这样我们就不必为每个函数重新实例化它。我们还设置了我们的`USERPOOL_ID`，它是一些 Cognito 函数中需要的 url，由`USER_POOL_ID`和`region`组成。最后，我们还导出了联邦身份池的 ARN，这也是一些 Cognito 函数所需要的。总而言之，这只是设置，你只需要复制和粘贴你自己的值。

上次设置时，我们将在`aws_cognito.js`文件的顶部创建一个用户属性数组。用您自己的用户属性填充数组，不要忘记在任何自定义属性前面加上前缀`custom:`来表示`const attrs`。`const landlordAttrs`不需要`custom:`前缀。

```
// we create an array of all attributes, without the `custom:` prefix. // This will be used for building the React-Redux object in plain JS, hence no AWS Cognito related name requirementsconst landlordAttrs = ["email", "agentName", "id"]
```

```
// we create an array of all our desired attributes for changing, and we loop through this array to access the key name.// This will be used for AWS Cognito related name requirementsconst attrs = ["custom:agentName"]
```

现在，在设置完成之后，开始实际代码之前，我应该说这个样板文件已经完成了。你不一定需要知道幕后发生了什么。你可以只使用这个样板文件，所有的功能都将开箱即用:注册，登录，电子邮件验证，密码重置，用户属性的变化和保存登录使用 JWT。请注意，您在 Cognito 中使用的任何电子邮件都必须经过 AWS 验证，直到您[请求离开 AWS SES 沙箱](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html)(在这种情况下，您可以发送任何电子邮件)。如果你来这里只是为了工作样本，这是你需要阅读的。想知道怎么回事，继续看！

#### 注册用户

让我们看看第一个名为`SignUp.js`的 auth 组件。我不会花时间解释事物的反应方面，因为这不是本教程的目的。去找这个函数:

```
signup(){
```

```
...
```

```
// call the AWS Cognito function that we named `signUpUser`    signUpUser(this.state)     .then(({email})=>{      // if successful, then save the email to localStorage so we can pre-fill the email form on the login & verify account screens      localStorage.setItem('User_Email', email)      // re-route to the verify account screen      browserHistory.push('/auth/verify_account')     })
```

```
...
```

```
}
```

这里我们调用`signUpUser()`并传入 React 组件状态，如下所示:

```
this.state = {   email: "",   agentName: "",   password: "",   confirmPassword: "",   errorMessage: null,   loading: false}
```

当我们将状态传递给`signUpUser()`时，我们将只使用状态的电子邮件和密码属性。`signUpUser()`功能位于`App/src/api/aws/aws-cognito.js`。

```
export function signUpUser({email, agentName, password}){ const p = new Promise((res, rej)=>{
```

```
 const attributeList = []
```

```
 const dataEmail = {      Name : 'email',      Value : email  }  const dataAgentName = {      Name : 'custom:agentName',      Value : agentName  }
```

```
 const attributeEmail = new CognitoUserAttribute(dataEmail)  const attributeAgentName = new CognitoUserAttribute(dataAgentName)
```

```
 attributeList.push(attributeEmail, attributeAgentName)
```

```
 userPool.signUp(email, password, attributeList, null, function(err, result){      if (err) {          rej(err)          return      }      res({email})  }) }) return p}
```

`signUpUser()`接受一个应该有三个属性的对象:`email`、`password`和`agentName`。为了将它保存为我们的 Cognito 用户的属性，我们必须为每个用户创建一个`CognitoUserAttribute`对象。我们通过用属性的名称和它的值创建一个`dataEmail`和`dataAgentName`对象来做到这一点。这些对象将被传递到`CognitoUserAttribute`函数中，该函数将其转换成 AWS Cognito 可读的对象，我们将其命名为`attributeEmail`和`attributeAgentName`。请注意，`dataAgentName.name`以`custom:`为前缀，以指定认知到`agentName`是一个自定义用户属性。现在我们有了我们的`CognitoUserAttribute`对象，我们将把它们推入`attributeList`数组。

下一行代码实际注册了这个新用户。我们使用从`./aws_profile.js`导入的`userPool`对象，并调用它的`signUp`函数。前 3 个参数是唯一标识符`email`、`password`和`attributeList`数组。第四个参数是 null，第五个是回调。在回调中，如果出现错误，我们拒绝承诺，如果没有错误，那么我们解决承诺。在样板文件中，我们将要保存的电子邮件返回到 React 组件中的 localStorage，但这不是强制性的。你可以什么都不用做就解决承诺。新的 Cognito 用户已创建。但是为了使用新用户，他们需要能够验证他们的帐户。AWS 基础设施已经设置好了，所以现在我们要做的就是遍历验证函数的代码。

#### 验证帐户

我们不会在这部分代码上花费太多时间，所以我将首先从较高的层次上解释 React-Redux 组件，然后详细介绍 AWS 的内容。

在一个高层次上，用户注册后发生的事情是，他们被`react-router`重定向到出现`App/src/components/Auth/VerifyAccount.js`组件的`/verify_account` url。当组件被安装时，通过访问`localStorage`，电子邮件字段被自动填充。然后，我们可以选择输入发送到用户电子邮件的验证 PIN，或者选择重置&重新发送验证 PIN。让我们来看看`App/src/api/aws/aws-cognito.js`中的`resetVerificationPIN()`功能。

```
export function resetVerificationPIN(email){ const p = new Promise((res, rej)=>{  const userData = {   Username: email,   Pool: userPool  }  const cognitoUser = new CognitoUser(userData)  cognitoUser.resendConfirmationCode(function(err, result) {         if (err) {          rej(err)          return         }         res()     }) }) return p}
```

当我们调用这个函数时，我们只需要传入邮件。Cognito 将自动检查电子邮件是否存在，如果不存在，将抛出一个错误。像在`SignUp.js`中一样，我们创建一个`userData`对象，包含我们用户的电子邮件和从`./aws_profile.js`导入的用户池，以便创建一个 vliad `CognitoUser`对象。使用`CognitoUser`对象，我们可以调用`resendConfirmationCode()`来再次发送 PIN。就是这样！

现在我们来看看`verifyUserAccount()`函数:

```
export function verifyUserAccount({email, pin}){ const p = new Promise((res, rej)=>{  const userData = {   Username: email,   Pool: userPool  }  const cognitoUser = new CognitoUser(userData)  cognitoUser.confirmRegistration(pin, true, function(err, result) {         if (err) {             console.log(err);             rej(err)             return;         }         if(result == "SUCCESS"){          console.log("Successfully verified account!")          cognitoUser.signOut()          res()         }else{          rej("Could not verify account")         }     }) }) return p}
```

`verifyUserAccount()`接受一个对象作为它的唯一参数，包含两个基本属性`email`和`pin`。我们创建另一个`userData`对象来创建一个`CognitoUser`，以便调用`confirmRegistration()`函数。在`confirmRegistration()`中，我们传入`pin`、`true`和一个回调。如果确认成功，那么我们注销 cognitoUser(以便我们可以再次登录并刷新用户)。如果失败了，那么我们拒绝这个承诺。非常简单，因为 SDK 已经抽象了很多细节。验证成功后，React 组件会将您重定向到登录页面。

#### 登录用户

我们来看下一个组件`App/src/components/Auth/Login.js`。找到以下函数:

```
signin(){  this.setState({loading: true})  signInUser({   email: this.state.email,   password: this.state.password  }).then((userProfileObject)=>{   localStorage.setItem('User_Email', this.state.email)   this.props.setUser(userProfileObject)   browserHistory.push('/authenticated_page')  })  .catch((err)=>{   this.setState({    errorMessage: err.message,    loading: false   })  }) }
```

这里发生了什么事？首先，我们调用`signInUser()` Cognito 函数登录并从 Cognito 获取用户详细信息。在承诺链中的下一步，我们将用户电子邮件保存到`localStorage`,以便我们可以自动设置下次登录的电子邮件。我们还使用`this.props.setUser()`将用户保存到 Redux 状态，这是一个位于`App/src/actions/auth_actions.js`的 Redux 动作函数。我们不会讨论 React-Redux 的内容，因为这不是本教程的重点。让我们看看 AWS Cognito 函数。

在`App/src/api/aws/aws-cognito.js`找到`signInUser()`。看起来是这样的:

```
export function signInUser({email, password}){ const p = new Promise((res, rej)=>{
```

```
 const authenticationDetails = new AuthenticationDetails({   Username: email,   Password: password  })
```

```
 const userData = {   Username: email,   Pool: userPool  }  const cognitoUser = new CognitoUser(userData)
```

```
 authenticateUser(cognitoUser, authenticationDetails)   .then(()=>{    return buildUserObject(cognitoUser)   })   .then((userProfileObject)=>{    res(userProfileObject)   })   .catch((err)=>{    rej(err)   })
```

```
 }) return p}
```

我们创建一个包含用户`email`和`password`的`AuthenticationDetails`认知对象。我们还创建了一个用于其`authenticateUser()`函数的`CognitoUser`对象，但是注意`authenticateUser()`没有在函数中的任何地方声明，也没有在我们列出依赖项的页面顶部声明。这是因为`authenticateUser()`是页面下方声明的另一个函数。页面中声明的另一个函数是`buildUserObject()`，它从 Cognito 获取用户属性，并将其格式化为我们希望在 Redux 状态下使用的用户对象。在承诺链的末端，我们返回`buildUserObject()`输出的`userProfileObject`。让我们从`authenticateUser()`开始浏览承诺链。

```
function authenticateUser(cognitoUser, authenticationDetails){ const p = new Promise((res, rej)=>{
```

```
 cognitoUser.authenticateUser(authenticationDetails, {
```

```
 onSuccess: function (result) {             localStorage.setItem('user_token', result.accessToken.jwtToken)             const loginsObj = {                 [USERPOOL_ID]: result.getIdToken().getJwtToken()             }       AWS.config.credentials = new AWS.CognitoIdentityCredentials({                 IdentityPoolId : IDENTITY_POOL_ID,                  Logins : loginsObj             })             AWS.config.credentials.refresh(function(){              console.log(AWS.config.credentials)             })             res()         },
```

```
 onFailure: function(err) {             rej(err)         }
```

```
 })
```

```
 }) return p}
```

`authenticateUser()`接受`cognitoUser`和`authenticationDetails`参数，并用它做两件事。`cognitoUser`中有一个名为`authenticateUser()`的功能，我们将调用它来登录 AWS Cognito。我们传入的第一个参数是`authenticationDetails`(包含 email+密码)，第二个参数是一个带有`onSuccess`和`onFailure`回调的对象。很简单，`onFailure`会直接拒绝承诺链。`onSuccess`将包含`result`，它将有一个 JWT 令牌用于将来的身份验证，无需输入密码。我们将 JWT 保存到`localStorage`中，并在需要时检索它(资源的后端认证或自动登录)。接下来，我们创建一个`loginsObj`，它包含我们的`USER_POOL_ID`的键值和 JWT 令牌。我们使用`new AWS.CognitoIdentityCredentials()`将这个`loginsObj`传递给`AWS.config.credentials`的一个实例，连同`IdentityPoolId`。这是用 AWS 联合身份注册一个登录。回想一下，联合身份用于管理来自多个来源的登录，因此我们使用联合身份来记录每次登录的成功是有意义的。

设置好`AWS.config.credentials`之后，我们现在可以使用 Cognito 身份验证来请求其他 Amazon 服务。当然，这些服务需要配置为将某个 Cognito auth 列入白名单(并拒绝其他请求)，但这将在未来的教程中以每个服务为基础进行展示。无论如何，在我们设置好`AWS.config.credentials`之后，使用`AWS.config.credentials.refresh`刷新凭证是很重要的，这样 AWS 将使用我们刚刚添加的最新凭证。

现在让我们进入`signInUser()`承诺链的下一步:`buildUserObject()`。

```
function buildUserObject(cognitoUser){ const p = new Promise((res, rej)=>{  cognitoUser.getUserAttributes(function(err, result) {         if (err) {              rej(err)              return         }         let userProfileObject = {}         for (let i = 0; i < result.length; i++) {           if(result[i].getName().indexOf('custom:') >= 0){              let name = result[i].getName().slice(7, result[i].getName().length)              userProfileObject[name] = result[i].getValue()           }else{              userProfileObject[result[i].getName()] = result[i].getValue()           }         }         res(userProfileObject)     }) }) return p}
```

首先，`cognitoUser`对象被传入并用于调用它的`getUserAttributes()`方法。像往常一样，如果出现错误，我们拒绝承诺。如果成功，那么我们继续创建一个空的`userProfileObject`，它将有一个与我们在 React-Redux 前端想要的相匹配的结构。我们从成功回调中得到的`result`对象是一个 CognitoUserAttribute 对象数组(回想一下`signUpUser()`中的`AttributeList`数组)。我们使用 for 循环遍历这个数组，获得每个属性的名称，如果需要的话去掉前缀`custom:`。然后我们还包括属性的值，并将键-值对添加到`userProfileObject`。在循环结束时，我们将完成普通 JS 格式的`userProfileObject`。我们返回`userProfileObject`并完成`signInUser()`承诺链。让我们再次查看`signInUser()`流量，并观察高液位的流量。

```
export function signInUser({email, password}){ const p = new Promise((res, rej)=>{
```

```
const authenticationDetails = new AuthenticationDetails({   Username: email,   Password: password  })
```

```
const userData = {   Username: email,   Pool: userPool  }  const cognitoUser = new CognitoUser(userData)
```

```
authenticateUser(cognitoUser, authenticationDetails)   .then(()=>{    return buildUserObject(cognitoUser)   })   .then((userProfileObject)=>{    res(userProfileObject)   })   .catch((err)=>{    rej(err)   })
```

```
}) return p}
```

当我们最终解析`signInUser()`承诺时，我们将`userProfileObject`返回给 React 组件。在 React 组件`Login.js`中，观察我们在`signInUser()`之后做什么。

```
signin(){  this.setState({loading: true})  signInUser({   email: this.state.email,   password: this.state.password  }).then((userProfileObject)=>{   localStorage.setItem('User_Email', this.state.email)   this.props.setUser(userProfileObject)   browserHistory.push('/authenticated_page')    })  .catch((err)=>{   this.setState({    errorMessage: err.message,    loading: false   })  }) }
```

我们将用户的电子邮件保存到`localStorage`以备将来使用，并将`userProfileObject`添加到 Redux 状态。如果在整个过程中出现任何错误，都会被捕获并显示在`this.state.errorMessage`中。就是这样！需要指出的是:`this.props.setUser()`是一个 Redux 动作，它将设置`userProfileObject`在整个 Redux 应用程序中可访问，我们还将一个布尔值`state.auth.authenticated`切换到`true`。Redux 应用程序使用`state.auth.authenticated`作为确定某个页面是否应该被渲染的手段。例如，如果有用户登录，我们只想显示用户资料页面。

#### 结束第 2 部分

哇，这篇文章真长！但是我们还没完。我们还需要讨论一些话题，包括`updateUserInfo()`、`forgotPassword()`、`retrieveUserFromLocalStorage()`、`signOutUser()`以及受限资源的 JWT 令牌的后端认证。我说过这是一个完整的 AWS 教程，不是吗？不管怎样，如果你觉得你需要知道引擎盖下发生了什么，请继续阅读。请记住，在任何时候，你都可以停止阅读，直接使用样板文件，这样就可以了。希望到目前为止，您已经发现这个系列很有用。认知第 3 部分再见！

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

> 这些方法在 [renthero.ca](http://renthero.ca) 的部署中被部分使用