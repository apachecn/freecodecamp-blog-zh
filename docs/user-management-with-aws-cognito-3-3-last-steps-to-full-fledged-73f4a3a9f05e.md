# 使用 AWS Cognito 的用户管理— (3/3)走向成熟的最后一步

> 原文：<https://www.freecodecamp.org/news/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e/>

作者:黄

# 使用 AWS Cognito 的用户管理— (3/3)走向成熟的最后一步

#### 完整的 AWS Web 样板文件—第 1C 部分

![638LdmUDcLc3rZMddMwajeKtKnW5y7Ta5Wbx](img/d23f7429b3d78f1716d86eef46623532.png)

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

点击下载 Github [。](https://github.com/kangzeroo/Kangzeroos-AWS-Cognito-Boilerplate)

### 最后的步骤

这个宏大模式的最后一部分包括收尾工作和后端认证。我们所说的收尾工作包括:

> - updateUserInfo()

> - forgotPassword()

> -注销用户( )

> -retrieveuserfromlocalstorage()

> -后端认证

后端认证意味着检查从科宁托或脸书收到的 JWT 令牌，以确认访问受保护资源的权限。在涵盖了这些特性之后，我们将拥有一个完全基于 AWS 的成熟的用户管理系统。哇！我们开始吧。

#### 更新用户信息

任何值得尊敬的用户管理系统都会有改变用户属性的能力，AWS Cognito 也不例外。React 组件可以在`App/src/components/auth/ProfilePage.js`找到。我们的 Cognito 代码在`App/src/api/aws/aws-cognito.js`里面，找函数`updateUserInfo()`。

```
export function updateUserInfo(editedInfo){ const p = new Promise((res, rej)=>{  const attributeList = []  for(let a = 0; a<attrs.length; a++){    if(editedInfo[attrs[a]]){      let attribute = {          Name : attrs[a],          Value : editedInfo[attrs[a]]      }      let x = new CognitoUserAttribute(attribute)      attributeList.push(x)    }  }  const cognitoUser = userPool.getCurrentUser()  cognitoUser.getSession(function(err, result) {      if(result){        cognitoUser.updateAttributes(attributeList, function(err, result) {          if(err){            rej(err)            return          }          cognitoUser.getUserAttributes(function(err, result) {            if(err){              rej(err)              return            }            buildUserObject(cognitoUser)             .then((userProfileObject)=>{              res(userProfileObject)             })          })        });      }    }); }) return p}
```

我们传入从`buildUserObject()`获得的用户对象，但是对其进行编辑以包含更新后的值(在本例中为`agentName`)。在我们的承诺中，我们创建了一个空的`attributeList`数组来保存变量，感觉很像`signUpUser()`。那是因为它共享相同的过程！我们循环遍历`editedInfo`对象，为每个属性创建一个`CognitoUserAttribute`对象添加到`attributeList`数组中。

所有这些都完成后，我们从导入的`userPool`创建一个`CognitoUser`对象来刷新会话，这样我们就可以用`attributeList`数组适当地调用`updateAttributes`。这将用最新的属性更新我们的 Cognito 用户，在回调中我们可以再次`getUserAttributes`。有了更新的属性，我们调用`buildUserObject`在 React-Redux 应用程序中使用。就是这样！几乎是`signUpUser()`的完全重复。

#### 忘记密码

这个也非常简单。像往常一样，用来自`userData`的数据创建您的`CognitoUser`。现在我们可以称之为`forgotPassword`。

```
export function forgotPassword(email){ const p = new Promise((res, rej)=>{
```

```
 const userData = {     Username: email,     Pool: userPool   }  const cognitoUser = new CognitoUser(userData)
```

```
 cognitoUser.forgotPassword({      onSuccess: function (result) {        res({          cognitoUser: cognitoUser,          thirdArg: this        })      },      onFailure: function(err) {         rej(err)      },      inputVerificationCode: function(data) {         res({            cognitoUser: cognitoUser,            thirdArg: this         })      }  })
```

```
 }) return p}
```

`forgotPassword()`基本上初始化流程并返回一个`CognitoUser`对象用于 React-Redux。在回调对象中，我们只使用了`onSuccess`和`onFailure`。`inputVerificationCode`在这里不像在 [Github 文档(见案例 12)](https://github.com/aws/amazon-cognito-identity-js) 中那样使用，因为我们想制作一个更漂亮的界面，而不是使用`prompt()`来请求输入。在`onSucccess`中，我们将 CognitoUser 返回给 React 组件，因为我们希望密码重置页面包含预先填充的信息(例如`email`)。提交新密码时，`confirmPassword()`只需要接受一个来自 AWS api 调用的`PIN`、`password`和`this`声明。这是它在 React-Redux 应用程序中的样子，来自`App/src/components/Auth/ResetPassword.js`。

```
verifyPin(){  if(this.props.password == this.props.confirm_password){     this.state.cognitoUserPackage.cognitoUser       .confirmPassword(this.state.pin, this.state.password, this.state.cognitoUserPackage.thirdArg)     setTimeout(()=>{       browserHistory.push("/auth/login")     }, 500)  } }
```

这就是重置密码的全部内容——不太复杂，没有太多代码。

#### 注销用户

注销用户真的很简单。我们调用`getCurrentUser()`来实例化我们的`CognitoUser`对象，这样我们就可以使用它的`signOut()`函数。现在您的用户已注销！

```
export function signOutUser(){ const p = new Promise((res, rej)=>{  const cognitoUser = userPool.getCurrentUser()  cognitoUser.signOut() }) return p}
```

我们如何从用户界面执行这个？在我们的 React-Redux 样板文件中，应用程序路由由`react-router`处理，而应用程序状态由 Redux 处理。我们必须将这两者结合起来，集成可视化用户认证。我们的目标是:

> —为经过身份验证或未经身份验证的访问者显示不同的屏幕

> —限制未经授权的访问者访问应用程序

好了，我们开始吧。先来观察一下我们在`App/src/reducers/AuthReducer.js`中表达的 Redux 状态。我们的状态模型如下所示:

```
const INITIAL_STATE = {  authenticated: false,  user: null}
```

当我们登录时，我们将 Redux 状态`authenticated`设置为 true，并将`user`设置为`App/src/api/aws/aws-cognito.js`中`buildUserObject()`的返回值。因此`INITIAL_STATE.authenticated`将在我们的应用程序中普遍用作检查，以确定用户是否经过身份验证。在我们的应用样板中，我们将这个 Redux 状态变量作为`this.props.authenticated`来访问。因此，在我们位于`App/src/components/SideMenu/SideMenu.js`的侧菜单组件中，找到这段代码:

```
<div id='mainview' style={comStyles(this.props.sideMenuVisible).mainview}>        <SideHeader />        <SideOption text='Home' link='/' />        {           this.props.authenticated           ?           <SideOption text='Sign Out' link='/auth/signout' />           :           <SideOption text='Login' link='/auth/login' />        }</div>
```

在`<SideOption text=’Home’ link=’/’` / >之后，在卷毛 bra `cke` ts { }内部，我们有一个三元运算(又名条件运算符)。这将检查第一个参数`ument this.props.authent`是否为真，如果为真，将判断`splay <SideOption text=’Sign Out’ link=’/auth/si` gnout' / >。如果为假，w `ill display <SideOption text=’Login’ link=’` /auth/login' / >。我们做到了！经过认证的&未经认证的访客的不同视图。

接下来，转到位于`App/src/index.js`的表示我们的`react-router`的代码，并找到以下代码片段:

```
<Route path='/' component={App}>        <IndexRoute component={Home} />        <Route path='auth'>          <Route path='login' component={Login}></Route>          <Route path='signup' component={SignUp}></Route>          <Route path='signout' component={SignOut}></Route>          <Route path='verify_account' component={VerifyAccount}></Route>          <Route path='forgot_password' component={ResetPassword}></Route>          <Route path='authenticated_page' component={RequireAuth(AuthenticatedPage)}></Route>        </Route></Route>
```

快速浏览一下我们应用的 url 树。在`[http://ourApp.com/](http://ourApp.com/)`处，我们转到表示为`App/src/components/home.js`的 Home 组件。在`[http://ourApp.com/auth/login](http://ourApp.com/auth/login)`是表示为`App/src/components/Auth/Login.js`的登录组件。但是在 T4，我们有一条稍微不同的路线

```
<Route path=’authenticated_page’ component={RequireAuth(AuthenticatedPage)}></Route>
```

这条路线有`RequireAuth()`包裹着`AuthenticatedPage`组件。如果我们转到 App/src/components/auth/RequireAuth . js 上的 require auth()，我们会发现另一个组件，但是没有生成 HTML(即没有 UI)。这是一个高阶组件(HOC ),仅增加功能。在这种情况下，特设检查是否 Redux 状态变量`this.props.authenticated`是真的。如果不是真的，我们只需将 url 重定向到不同的 url 路径(在本例中，`[http://ourApp.com/auth/login](http://ourApp.com/auth/login).)` [)。](http://ourApp.com/auth/login).)完成，这就是认证检查器！

```
componentWillMount(){   if(!this.props.authenticated){    browserHistory.push('/auth/login')   }}
```

现在回到`App/src/index.js`来实现授权检查。我们简单地将可视组件像函数一样包装在非可视 HCO 中:

```
<Route path='authenticated_page' component={RequireAuth(AuthenticatedPage)}></Route>
```

这是第二个目标完成。最后一部分是我们的通用签出 HCO。转到`App/src/components/auth/SignOut.js`并找到这段代码:

```
componentWillMount(){    signOutUser()  // signoutLandlord() is a function from `actions` coming from index.js  this.props.logoutUserFromReduxState()  setTimeout(()=>{   browserHistory.push('/auth/login')  }, 500) }
```

`signOutUser()`函数就是我们在`App/src/api/aws/aws-cognit.js`中写的那个。接下来`this.props.logoutUserFromReduxState()`将我们的 Redux 状态变量`authenticated`设置为假。最后，半秒钟后，我们使用`browserHistory.push(‘/auth/login’)`更改 url 地址和应用程序视图(这样我们就可以显示一条再见消息)。

就是这样！您现在可以控制您的应用程序的每一个视觉视图，并牢记身份验证！让我们继续下一部分。

#### 从本地存储中检索用户

我们不希望用户每次访问 web 应用程序时都必须重新登录。理想情况下，我们希望他们的登录被保存并在每次访问中自动记录，直到他们注销。因为保存用户密码是不安全的，所以我们将存储 JWT 令牌。这实际上是由 AWS Cognito 为我们管理的。转到`App/src/components/Auth/Login.js`并找到以下代码行:

```
componentDidMount(){  const savedEmail = localStorage.getItem('User_Email')  if(savedEmail){   this.setState({    email: savedEmail   })  }  retrieveUserFromLocalStorage()   .then((data)=>{    this.props.setUserToReduxState(data)   }) }
```

在组件装载到 web 页面上之后,`componentDidMount()`函数将运行一次，此时我们希望检查用户是否已经保存了登录信息。我们可以忽略第一部分，它只是检查保存的电子邮件，并将其设置为 React 组件的状态。这里重要的部分是`retrieveUserFromLocalStorage()`，它返回一个`userProfileObject`，我们可以将它保存到 Redux 状态。回想一下，web 应用程序使用`userProfileObject`来表示用户是谁，以及他们的所有属性，比如姓名、年龄、身高..等等。简单明了，所以让我们看看有趣的东西:AWS 函数。去`App/src/api/aws/aws-cognito.js`找到功能`retrieveUserFromLocalStorage()`。

```
export function retrieveUserFromLocalStorage(){ const p = new Promise((res, rej)=>{     const cognitoUser = userPool.getCurrentUser();     if (cognitoUser != null) {         cognitoUser.getSession(function(err, session) {             if (err) {                rej(err)                return             }             localStorage.setItem('user_token', session.getAccessToken().getJwtToken());             const loginsObj = {                 [USERPOOL_ID] : session.getIdToken().getJwtToken()             }         AWS.config.credentials = new AWS.CognitoIdentityCredentials({                 IdentityPoolId : IDENTITY_POOL_ID,                 Logins : loginsObj             })             AWS.config.credentials.refresh(function(){              console.log(AWS.config.credentials)              res(buildUserObject(cognitoUser))             })         });     }else{      rej('Failed to retrieve user from localStorage')     } }) return p}
```

这里发生了什么？首先，我们使用从`aws_profile.js`导入的`userPool`创建一个`CognitoUser`对象。然而，这一次我们使用的是`getCurrentUser()`函数，它将从以前的会话内存中提取数据——这是 AWS Cognito 为我们处理的一个有用的特性！如果我们从`getCurrentUser()`收到一个非空值，那么我们可以假设它是一个有效的`CognitoUser`对象，并调用`getSession()`来访问最新的会话变量。我们关心的变量是`session.getAccessToken().getJwtToken()`中的 JWT 令牌，我们将把它保存到`localStorage`并放入我们的`loginsObj`(回想一下第 2 部分，登录)。这将把我们的登录注册到联合身份。现在我们所要做的就是在调用`buildUserObject()`并将其返回给 React-Redux 应用程序之前，使用它来设置我们的 AWS 凭证并刷新它们。通过从`buildUserObject()`返回的`userProfileObject`，我们登录了！

#### 后端认证

好的，所有这些前端的东西都很棒，但是要得到完整的包，我们还需要后端认证。假设我们的后端有一个资源，我们只想显示给登录的用户。要请求该资源，我们不会使用电子邮件+密码，因为为每个请求发送密码是不安全的。相反，我们将使用科宁托提供给我们的 JWT 令牌。我们只需在后端解密令牌，并对照 Cognito 令牌引用对其进行检查。让我们按照一般流程来看看如何做到这一点:

我已经包含了用 NodeJS 编写的后端代码，但是一般过程适用于任何后端。代码见`/Bonus_Backend/`。现在让我们从前端开始这个过程(`/App/`)。

首先，我们在 HTTP 报头中从客户端前端发送 JWT 令牌。我们为`http`请求使用的库是`axios`，但是您可以使用任何您喜欢的库，只要您知道如何包含一个 header 属性。在 axios 中，您只需将一个带有 headers 键值的对象作为第三个参数放入`POST`函数中。在`App/src/api/myAPI.js`中找到这个动作。

```
const API_URL = '24.74.347.34' // your backend IP
```

```
export function getBackendResource(){  const jwtConfig = {headers: {"jwt": localStorage.getItem("user_token")}}   const p = new Promise((res, rej)=>{    axios.post(API_URL+"/auth_test", null, jwtConfig)     .then((data)=>{      res(data.data)     })     .catch((err)=>{       rej(err)     })   })   return p}
```

接下来，我们必须下载 AWS Cognito 为我们提供的 JWT 集。转到 AWS 控制台中的 Cognito 页面，找到您所在的地区(如`us-east-1`)和您的用户池 Id(如`us-east-1_Fa9dl8sWt`)。现在用它们来替换下面的占位符，并跟随链接。

```
https://cognito-idp.{region}.amazonaws.com/{userPoolId}/.well-known/jwks.json
```

您应该会看到这样一个文本页面:

```
{     "keys":[        {           "alg":"RS256",         "e":"AQAB",         "kid":"I7Kw/O0QymLQ8A0pPaXNcv5je7BNYXMCW1HdziUTyrQ=",         "kty":"RSA",         "n":"uIqZqU64ytLpQr3J86NMpjxZBRubRzovkQv22oAeHoxO_w4EZuvEeodCV7WxVatHwcVyH0VrkRsqcoigajJO5Xz3s-Ttz_ozhE8wP-BI3DUPOUNtGiKZirNLf9jluScrCUsyyim2UrF4ub-hsxGSt32GFRMfqrkvz0Ral4K4oeIiBNnX8cu_pbSlDgriBLAh8ago41XhqqSFtWwlP-x_KHJc13RBgETj7HOfEm5tr6ibJlMazL3FOoXehfXQw9Yr0752A2hTKAB8reUJXuAwcyTUa8ZEO6IcnhQiaPmIgltxdm-SHdoPqwR_SQxYzZfQzU9uE78ogWT-xP29Gr08Xw",         "use":"sig"      },      {           "alg":"RS256",         "e":"AQAB",         "kid":"fxyn6hg0ziTNer+mBzqmxqGe38uh4neQPorXo3GAa/s=",         "kty":"RSA",         "n":"hMAECS0ALyFaP7OY4ZN5SXqPpkKOdp_RfNAmeCXhK98rmEnD_9Zzqb5oVviZZoqQ5xEZQBRR7a2JOZxL_JZWX7ObteHMSfNZywk8E9FN4XPMJxStZk5JSceKBd5SPYdLzTR58LFMg4OKONA5aJ1sYUu11zq6yMdUBvEJlwBjBrH4lfSkJ_jg4zSeKxsRcM72oAQ_yCnzO5giPoMjyY8VtqCj7NW_7njyQ-bD1WiGaNCkgBxWwYL_13zCxMJxNopa2vHoca0xn9bct-ysS8zIaB3DjNo_8-GGp_HJ4kNW0TczcILtl4mrl81srGzulvuK-mGF0T31IDY-tZWS3IgQYQ",         "use":"sig"      }   ]}
```

在您的后端(`Example_Backend/App/api/jwt_set.json`)中将其保存为自己的文件`jwt_set.json`，以便您的认证过程可以引用它。身份验证过程(功能)应该在访问任何受保护的资源之前运行。认证过程类似于在`Example_Backend/App/api/authCheck.js`找到的代码:

```
const jwt = require('jsonwebtoken');const jwkToPem = require('jwk-to-pem');const jwt_set = require('./jwt_set.json')
```

```
const userPool_Id = "https://cognito-idp.us-east-1.amazonaws.com/us-east-1_6i5p2Fwao"
```

```
const pems = {}for(let i = 0; i<jwt_set.keys.length; i++){ const jwk = {  kty: jwt_set.keys[i].kty,  n: jwt_set.keys[i].n,  e: jwt_set.keys[i].e } // convert jwk object into PEM const pem = jwkToPem(jwk) // append PEM to the pems object, with the kid as the identifier pems[jwt_set.keys[i].kid] = pem}
```

```
exports.authCheck = function(req, res, next){ const jwtToken = req.headers.jwt ValidateToken(pems, jwtToken)   .then((data)=>{    console.log(data)    next()   })   .catch((err)=>{    console.log(err)    res.send(err)   })}
```

```
function ValidateToken(pems, jwtToken){ const p = new Promise((res, rej)=>{  const decodedJWT = jwt.decode(jwtToken, {complete: true})  // reject if its not a valid JWT token  if(!decodedJWT){   console.log("Not a valid JWT token")   rej("Not a valid JWT token")  }  // reject if ISS is not matching our userPool Id  if(decodedJWT.payload.iss != userPool_Id){   console.log("invalid issuer")   rej({    message: "invalid issuer",    iss: decodedJWT.payload   })  }  // Reject the jwt if it's not an 'Access Token'  if (decodedJWT.payload.token_use != 'access') {         console.log("Not an access token")         rej("Not an access token")     }     // Get jwtToken `kid` from header  const kid = decodedJWT.header.kid  // check if there is a matching pem, using the `kid` as the identifier  const pem = pems[kid]  // if there is no matching pem for this `kid`, reject the token  if(!pem){   console.log('Invalid access token')   rej('Invalid access token')  }  console.log("Decoding the JWT with PEM!")  // verify the signature of the JWT token to ensure its really coming from your User Pool  jwt.verify(jwtToken, pem, {issuer: userPool_Id}, function(err, payload){   if(err){    console.log("Unauthorized signature for this JWT Token")    rej("Unauthorized signature for this JWT Token")   }else{    // if payload exists, then the token is verified!    res(payload)   }  }) }) return p}
```

好吧，这有点长，让我们一个一个来分析。首先，我们导入我们想要的 nodeJS 依赖项并安装它们。

```
$ npm install jsonwebtoken --save$ npm install jwk-to-pem --save
```

然后我们包括`jwt_set.json`以及我们的身份池 Id。这是 3 个依赖项和 1 个常量。

```
const jwt = require('jsonwebtoken');const jwkToPem = require('jwk-to-pem');const jwt_set = require('./jwt_set.json')
```

```
const userPool_Id = "https://cognito-idp.us-east-1.amazonaws.com/us-east-1_6i5p2Fwao"
```

我们再创建一个名为`pems`的常量，它是由加载文件时执行的`for`循环创建的。

```
const pems = {}for(let i = 0; i<jwt_set.keys.length; i++){ // take the jwt_set key and create a jwk object for conversion into PEM const jwk = {  kty: jwt_set.keys[i].kty,  n: jwt_set.keys[i].n,  e: jwt_set.keys[i].e } // convert jwk object into PEM const pem = jwkToPem(jwk) // append PEM to the pems object, with the kid as the identifier pems[jwt_set.keys[i].kid] = pem}
```

在高层次上，对于`jwt_set.json`中的每个键，我们创建一个`PEM`对象作为 jwt_set 键的`kid`的值。我们不需要确切地知道发生了什么，但基本上我们正在创建一个`PEM`，它可以用来匹配来自传入 http 请求头部的`jwt` `kid`，作为验证身份验证的一种方式。如果你觉得你需要知道这些术语的确切含义，可以看看 Chris Sevilleja 的《JSON Web Token 剖析》。

不管怎样，我们继续讨论使用`ValidateToken()`验证来自头部的 jwt 令牌的`authCheck`函数。很简单。

```
exports.authCheck = function(req, res, next){ const jwtToken = req.headers.jwt ValidateToken(pems, jwtToken)   .then((data)=>{    console.log(data)    next()   })   .catch((err)=>{    console.log(err)    res.send(err)   })}
```

`authCheck()`在你后端的其他地方被用作访问受保护资源之前调用的函数。对于一个`NodeJS` `Express`后端，它看起来会是这样的:

```
const authCheck = require('./api/authCheck').authCheck
```

```
// auth routeapp.get('/auth_test', authCheck, function(req, res, next){ console.log("Passed the auth test!") res.send("Nice job! Your token passed the auth test!")});
```

最后让我们看看`ValidateToken()`，它可以被分解成 6 个更小的部分。从`http`请求头传入`pems`常量和`jwt`。现在遵循验证过程的 6 个步骤，如果全部通过，JWT 令牌被接受！该承诺将得到解决，T4 将允许进入我们受保护的资源。

```
function ValidateToken(pems, jwtToken){ const p = new Promise((res, rej)=>{
```

```
 // PART 1: Decode the JWT token  const decodedJWT = jwt.decode(jwtToken, {complete: true})
```

```
 // PART 2: Check if its a valid JWT token  if(!decodedJWT){   console.log("Not a valid JWT token")   rej("Not a valid JWT token")  }
```

```
 // PART 3: Check if ISS matches our userPool Id  if(decodedJWT.payload.iss != userPool_Id){   console.log("invalid issuer")   rej({    message: "invalid issuer",    iss: decodedJWT.payload   })  }
```

```
 // PART 4: Check that the jwt is an AWS 'Access Token'  if (decodedJWT.payload.token_use != 'access') {     console.log("Not an access token")     rej("Not an access token")  }
```

```
 // PART 5: Match the PEM against the request KID  const kid = decodedJWT.header.kid  const pem = pems[kid]  if(!pem){   console.log('Invalid access token')   rej('Invalid access token')  }  console.log("Decoding the JWT with PEM!")
```

```
 // PART 6: Verify the signature of the JWT token to ensure its really coming from your User Pool  jwt.verify(jwtToken, pem, {issuer: userPool_Id}, function(err, payload){   if(err){    console.log("Unauthorized signature for this JWT Token")    rej("Unauthorized signature for this JWT Token")   }else{    // if payload exists, then the token is verified!    res(payload)   }  }) }) return p}
```

这就是我们的后端认证检查器。要集成它，请转到`Example_Backend/App/router.js`，在这里我们接收传入的 http 请求。

```
// routesconst Authentication = require('./routes/auth_routes');
```

```
// router middlewearconst authCheck = require('./api/authCheck').authCheck
```

```
module.exports = function(app){ // Auth related routes app.get('/auth_test', authCheck, Authentication.authtest);}
```

我们所要做的就是将它作为第二个参数添加到我们的 ExpressJS 路由中。如果您有一个不同的后端，同样的一般过程适用。

就是这样，后端认证使用我们相同的 AWS Cognito 环境。多么强大！

#### 结论

祝贺您阅读这篇关于 AWS Cognito 和联合身份的长篇教程！完成以上步骤后，您现在可以享受由全球最大的云服务提供商设计的顶级用户管理。如果您要定制一个系统，那么您从 AWS 获得的许多功能将需要数周时间和大量专业知识来实现。也不能保证您可以很好地实现定制的用户管理系统，而不会让您的用户暴露在安全缺陷和漏洞之下。通过使用亚马逊，你可以在晚上休息，因为你知道有一家价值数十亿美元的互联网公司在为你打理一切。

所以我们有了它:一个完整的用户管理系统，可以在现实世界中使用。如果你认为你从这个教程系列中受益或学到了很多，请分享和订阅！在接下来的几个月里，我会发布更多实用的 AWS 教程，所以请保持关注。下次见！

> [**主目录点击这里**](https://medium.com/@kangzeroo/the-complete-aws-web-boilerplate-d0ca89d1691f#.uw0npcszi)

> **A 部分:** [初始设置](https://medium.com/@kangzeroo/user-management-with-aws-cognito-1-3-initial-setup-a1a692a657b3#.pgxyg8q8o)

> **B 部分:** [核心功能](https://medium.com/@kangzeroo/user-management-with-aws-cognito-2-3-the-core-functionality-ec15849618a4)

> **C 部分:** [羽翼丰满的最后一步](https://medium.com/@kangzeroo/user-management-with-aws-cognito-3-3-last-steps-to-full-fledged-73f4a3a9f05e#.v3mg316u5)

> 这些方法在 [renthero.ca](http://renthero.ca) 的部署中被部分使用