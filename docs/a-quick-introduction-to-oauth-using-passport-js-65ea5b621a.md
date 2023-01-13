# 使用 Passport.js 快速介绍 OAuth

> 原文：<https://www.freecodecamp.org/news/a-quick-introduction-to-oauth-using-passport-js-65ea5b621a/>

### OAuth 是什么？

OAuth(开放授权)是一种授权协议。第三方应用程序可以使用它从一个网站(如 Google 或 Twitter)访问用户数据，而不会泄露他们的密码。像 Quora、Medium、AirBnb 和许多其他网站都提供使用 OAuth 的认证。

OAuth 让我们的生活变得更加简单，因为它不需要记住你在任何网站上创建的每个账户的密码。你只需要记住你的 OAuth 提供者的主账户密码。

### Passport.js 是什么？

Passport 是一个中间件，在基于 Express 的 web 应用程序上实现认证。它提供了 500 多种策略。这些策略是什么？策略用于验证请求。每个策略都有自己的 npm 包(比如 [passport-twitter](https://www.npmjs.com/package/passport-twitter) 、 [passport-google-oauth20](https://www.npmjs.com/package/passport-google-oauth20) )。策略必须在使用前进行配置。

### 为什么要用 Passport.js？

这里有六个理由说明为什么你应该使用 Passport:

*   它很轻
*   易于配置
*   支持持久会话
*   提供认证
*   为每种策略提供单独的模块
*   使您能够实施定制策略

### 让我们建造一些东西

首先，我们需要安装 NPM 的 passport:

```
npm install passport 
```

我们将建立一个简单的应用程序，只有当用户登录时，它才授予用户访问秘密路线的权限。我将在本教程中使用 [passport-google-oauth20](https://www.npmjs.com/package/passport-google-oauth20) 策略。你可以随意使用你喜欢的任何其他策略，但是一定要检查一下[文档](http://www.passportjs.org/packages/)，看看它是如何配置的。

在继续之前，我们需要一个 clientID 和 clientSecret。要得到一个，去 https://console.developers.google.com 创建一个新项目。然后转到启用 API 和服务，启用 Google+ API。选择 API 并单击 create credentials。

填写表单，并在表单和文件上使用相同的回调 URL。请务必阅读代码上的注释，以弄清楚所有内容是如何组合在一起的。

**app.js**

```
// Required dependencies 
const express = require('express');
const app = express();
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20');
const cookieSession = require('cookie-session');

// cookieSession config
app.use(cookieSession({
    maxAge: 24 * 60 * 60 * 1000, // One day in milliseconds
    keys: ['randomstringhere']
}));

app.use(passport.initialize()); // Used to initialize passport
app.use(passport.session()); // Used to persist login sessions

// Strategy config
passport.use(new GoogleStrategy({
        clientID: 'YOUR_CLIENTID_HERE',
        clientSecret: 'YOUR_CLIENT_SECRET_HERE',
        callbackURL: 'http://localhost:8000/auth/google/callback'
    },
    (accessToken, refreshToken, profile, done) => {
        done(null, profile); // passes the profile data to serializeUser
    }
));

// Used to stuff a piece of information into a cookie
passport.serializeUser((user, done) => {
    done(null, user);
});

// Used to decode the received cookie and persist session
passport.deserializeUser((user, done) => {
    done(null, user);
});

// Middleware to check if the user is authenticated
function isUserAuthenticated(req, res, next) {
    if (req.user) {
        next();
    } else {
        res.send('You must login!');
    }
}

// Routes
app.get('/', (req, res) => {
    res.render('index.ejs');
});

// passport.authenticate middleware is used here to authenticate the request
app.get('/auth/google', passport.authenticate('google', {
    scope: ['profile'] // Used to specify the required data
}));

// The middleware receives the data from Google and runs the function on Strategy config
app.get('/auth/google/callback', passport.authenticate('google'), (req, res) => {
    res.redirect('/secret');
});

// Secret route
app.get('/secret', isUserAuthenticated, (req, res) => {
    res.send('You have reached the secret route');
});

// Logout route
app.get('/logout', (req, res) => {
    req.logout(); 
    res.redirect('/');
});

app.listen(8000, () => {
    console.log('Server Started!');
}); 
```

**索引. ejs】t1**

```
<ul>
    <li><a href="/auth/google">Login</a></li>
    <li><a href="/secret">Secret</a></li>
    <li><a href="/logout">Logout</a></li>
</ul>
```

如您所见，我们已经创建了一个`/secret`路由，并且只有在用户通过身份验证的情况下才允许访问它。为了验证用户是否通过了身份验证，我们创建了一个中间件来检查请求中是否包含用户对象。最后，为了注销，我们使用了 passport 提供的`req.logout()`方法来清除会话。

### 这里有一些资源可以帮助您了解有关 passport 的更多信息

[https://www.youtube.com/embed/videoseries?list=PL4cUxeGkcC9jdm7QX143aMLAqyM-jTZ2x](https://www.youtube.com/embed/videoseries?list=PL4cUxeGkcC9jdm7QX143aMLAqyM-jTZ2x)

[**passport . js 的官方文档**](http://www.passportjs.org/)
[*对 Node.js 进行简单、不引人注目的认证*www.passportjs.org](http://www.passportjs.org/)

### 结论

我们在这里只看到了一个策略。还有 500+多。我强烈建议你浏览一下 Passport 的官方文档，看看他们还提供什么。感谢您花时间阅读本文。请随时在 [LinkedIn](https://www.linkedin.com/in/arun4033622) 、 [Twitter](https://twitter.com/Arun4033622) 和 [GitHub](https://github.com/Arun4033622) 上与我联系。祝你好运！

![0*mgwlLYxIDy5weT3_](img/6f0d021b4342872c1db58a549167371d.png)

“Do what is great, written on a computer monitor.” by [Martin Shreder](https://unsplash.com/@martinshreder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 前一篇文章

[**材料设计运用快速入门**](https://medium.freecodecamp.org/an-quick-introduction-to-material-design-using-materialize-8a9b223c64f1)
[*什么是材料设计？*medium.freecodecamp.org](https://medium.freecodecamp.org/an-quick-introduction-to-material-design-using-materialize-8a9b223c64f1)