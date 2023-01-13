# 如何使用 Cookies、会话和 Passport.js 对节点应用程序中的用户进行身份验证

> 原文：<https://www.freecodecamp.org/news/authenticate-users-node-app/>

在任何关注后端技术的课程中，学习如何认证用户进入应用程序是你首先要学的事情之一。

这是你构建社交媒体应用、在线课程学习应用等的第一步。

在本文中，我们将以初学者友好的方式了解基本的身份验证概念。

## 如何使用 Cookies 认证用户

首先，让我们构建一个简单的应用程序，使用经典的用户名和密码方法验证用户——我们现在不用担心数据库连接。

每当我们试图加载一个页面时，浏览器都会向服务器发送一个请求，服务器会做出相应的响应。

最初，当用户访问网站时，系统会提示他们输入注册的用户名和密码。一旦给定，它们就让服务器知道它们自己，因此对服务器的后续请求不涉及重新声明它们的身份。

但是服务器如何跟踪哪些用户已经被认证，哪些没有被认证呢？这就是饼干的用武之地。

根据 w3.org 的说法，饼干被定义为:

> "存储在客户端机器上的文本位，通过 HTTP 请求发送到为其创建的网站。"

Cookies 是在用户登录后创建和存储的，在满足后续请求之前会对它们进行查询。然后，它们根据指定的时间限制到期。

```
let express=require('express')
let cookie_parser=require('cookie-parser')
let app=express()
app.use(cookie_parser('1234'))
```

首先，我们设置了我们的 Express 应用程序，并包含了`cookie-parser`中间件。它解析请求的 cookie 头，并将其添加到`req.cookies`或`req.signedCookies`(如果使用了密钥)以进行进一步处理。

将密钥作为参数，它将用于创建当前 cookie 值的 HMAC。如果该值后来被篡改，则会被检测到，因为创建时的签名与当前签名不匹配。

接下来，当用户访问适当的 URL(比如/login 或类似的东西)时，我们需要执行一些检查。假设用户第一次登录。

```
let cookie_Stuff=req.signedCookies.user
//But the user is logging in for the first time so there won't be any appropriate signed cookie for usage.
if(!cookie_Stuff)//True for our case
    {
        let auth_Stuff=req.headers.authorization
        if(!auth_Stuff)//No authentication info given
        {
            res.setHeader("WWW-Authenticate", "Basic")
            res.sendStatus(401)
        }
```

我们使用 WWW-Authenticate 响应头来定义应该用来访问资源的身份验证方法(“基本”方法)。

来自客户端的响应由冒号分隔的用户名和密码组成。它是 base64 编码的，附加到请求的授权头。

系统会提示用户输入身份验证信息，并提取和检查这些信息。我们实际上需要从数据库中检查，但是为了简单起见，我们现在做一个简单的检查。

如果给出了正确的值，我们就建立一个适当的 cookie。如果没有，那么我们再次提示用户。下一个代码片段执行这些步骤:

```
else
        {
            step1=new Buffer.from(auth_Stuff.split(" ")[1], 'base64')
 //Extracting username:password from the encoding Authorization: Basic username:password
            step2=step1.toString().split(":")
//Extracting the username and password in an array
            if(step2[0]=='admin' && step2[1]=='admin')
            {
//Correct username and password given
                console.log("WELCOME ADMIN")
//Store a cookie with name=user and value=username
                res.cookie('user', 'admin', {signed: true})
                res.send("Signed in the first time")
            }
            else
            {
 //Wrong authentication info, retry
                res.setHeader("WWW-Authenticate", "Basic")
                res.sendStatus(401)
            }
        }
    }
```

下一次我们的用户提出请求时怎么办？从那时起，直到 cookie 被清除或过期，我们检查 cookie 的值以进行身份验证。

```
else
    {//Signed cookie already stored
        if(req.signedCookies.user=='admin')
        {
            res.send("HELLO GENUINE USER")
        }
        else
        {
     //Wrong info, user asked to authenticate again
            res.setHeader("WWW-Authenticate", "Basic")
            res.sendStatus(401)
        }
    }
})
```

现在您知道如何使用 cookies 来验证用户了！

您可以通过导航到浏览器开发工具的存储部分并转到 cookie 选项卡来检查存储的 cookie。cookie 值和解析后的值分别显示在两个部分中(例如在 Firefox 中)。

## 如何通过会话验证用户

让我们看一个类比来帮助我们理解会话与 cookies 的比较。想象你是一个心不在焉的人，总是忘记朋友的名字。

一个解决办法是给每个朋友一张卡片，上面有他们的名字和照片。每次你遇到他们时，只要简单地要求看一下你给他们的卡片，以此来唤起你的记忆。

问题是你的朋友可能会丢失这张卡。或者他们中的两个人可能会交换卡片，对你恶作剧。或者您的朋友没有足够的空间来存放另一张卡。

在这两种情况下，身份验证机制都显示出了弱点。但这是 cookie 的基本功能——它们存储在客户端，每次客户端向其网站发出请求时，都会访问 cookie 进行身份验证。它们会占用一些空间或者被篡改。

不使用 cookies，假设你为每个朋友制作一张卡片并随身携带。当你看到他们时，你指定一种方法将卡片与人匹配，以便于识别。这样，信息不在客户端，因此更安全。

这个场景向我们展示了会话是如何工作的。关于新通过身份验证的用户的信息驻留在服务器上，只有最少的信息被传回客户端。这样，客户端可以映射到存储的信息。

创建一个会话中间件，让你可以轻松地建立和操作会话。

默认的存储服务器端是 MemoryStore。要将会话信息存储为 JSON 文件，您需要 session-file-store。以下代码执行以下操作:

*   它设置了快速应用程序
*   如果没有指定，它告诉中间件请求认证，否则检查用户名和密码是否匹配。
*   如果不是，它需要再次进行相同的认证请求，否则会话被建立。
*   然后，它添加用户名作为用户属性，并随后检查它。

同样，这只是一个简单的例子——需要对照给定数据进行检查的信息至少应该存储在数据库中。

```
let app=express()
app.use(session({
  store: new File_Store,
  secret: 'hello world',
  resave: true,
  saveUninitialized: true
}))
app.use('/', (req,res,next)=>{
  if(!req.session.user)
  {
    console.log("Session not set-up yet")
    if(!req.headers.authorization)
    {
      console.log("No auth headers")
      res.setHeader("WWW-Authenticate", "Basic")
      res.sendStatus(401)
    }
    else
    {
      auth_stuff=new Buffer.from(req.headers.authorization.split(" ")[1], 'base64')
      step1=auth_stuff.toString().split(":")
      console.log("Step1: ", step1)
      if(step1[0]=='admin' && step1[1]=='admin')
      {
        console.log('GENUINE USER')
        req.session.user='admin'
        res.send("GENUINE USER")
      }
      else
      {
        res.setHeader("WWW-Authenticate", "Basic")
        res.sendStatus(401)
      }
    }
  }
```

## 如何用 Passport.js 中间件认证用户

到目前为止，我们已经看到了如何使用 cookies 和会话来验证用户。现在我们将看到第三种身份验证方法。

Passport.js 是一个用于 Node 的身份验证中间件，允许您使用会话和 OAuth 对用户进行身份验证。它还可以让您创建自定义策略等等。

```
let passport=require('passport')
let bcrypt=require('bcrypt-nodejs')
let User_Obj=require('./Set_Up_Database_Stuffs')
const local_strategy=require('passport-local').Strategy
```

这段代码设置了定义合适的本地策略所需的所有模块。`passport-local`策略允许仅使用用户名和密码进行身份验证。

确保用户名和密码的表单输入元素的名称分别为“用户名”和“密码”。虽然这听起来非常直观，但它给我带来了很多麻烦，因为我忽略了这一部分。现在，这是我不太可能再次忽视的事情之一(至少在不久的将来)。

您还可以通过在对`local_strategy`的调用中的回调函数之前传递 JSON 来更改字段的默认名称，其中 JSON 结构是`usernameField`:“该字段的某个新名称”，以及`passwordField`:“该字段的某个新名称”。

```
passport.use(new local_strategy(
    async (username, password, done)=>{
        console.log("Here inside local_strategy" ,username, password)

    try
    {
        let row1=await User_Obj.findOne({username: username})
        console.log(row1)
        //row1 should be the tuple from database where the username field matches the username supplied.
        if(row1==null)
        {
            console.log("NO RECORDS FOUND")
            return done(null, false)
        }
        else
        {
            console.log("Record found")
            console.log(row1)
            if(bcrypt.compareSync(password, row1.password))//Compare plaintext password with the hash
            {
                console.log("The passwords match")
                console.log("Finished authenticate local")
                return done(null, row1)
            }
            else
                {
                    console.log("The passwords don't match")
                    return done(null, false)
                }
        }

    }
    catch(err){
        console.log("Some error here")
        return done(err)}
    }
  ));
```

上面几行是`local-strategy`的一个简单实现，其中数据是从一个以用户名为字段的数据库中检查出来的。

```
app.post('/auth', passport.authenticate('local', {successRedirect: 'articles', failureRedirect: '/failurepage'}))
//Triggers the local strategy. If successful, redirect to articles page else show failure page
app.post('/donesignup', objForUrlencoded, async (req,res)=>{
    console.log(req.body)
    try
    {
        let row1=await User_Obj.findOne({username: req.body.username})
        console.log(row1)
        if(row1!=null)
        {
            console.log("That username already exists")
            res.render('signup')
        }
        else
        {
            console.log(bcrypt.hashSync(req.body.password[0], bcrypt.genSaltSync(8), null))//Get the hash of the password to store it in the database
            let save_this=User_Obj({username: req.body.username, password: bcrypt.hashSync(req.body.password[0], bcrypt.genSaltSync(8), null)})
            console.log(save_this)
            save_this.save()
            console.log("SAVED IT")//Save it to database
        }
    }
    catch(err){}
})
```

每当用户访问/auth 路由时，它都会触发本地策略，该策略会按照指定的方式执行。如果身份验证失败，它会重定向到失败页面。否则，它会重定向到一个文章页面(或您需要的任何页面)。

在对/donesignup 的 post 请求中，它检查用户名是否已经存在。如果不是，那么它将把它作为一个元组添加到数据库中，其中的字段是用户名和给定密码的散列。

## 包扎

这就是我对 Node 中不同身份验证方法的总结。

这里使用的代码并不理想，但我希望它能帮助那些刚刚开始学习身份验证并感到不知所措的人。

如果你已经读到这里，非常感谢你，尽管我尽了最大的努力，请纠正任何可能出现的错误。再次感谢你，祝你编码愉快。