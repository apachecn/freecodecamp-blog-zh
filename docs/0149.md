# 如何在 React 应用中使用 Firebase 身份验证

> 原文：<https://www.freecodecamp.org/news/use-firebase-authentication-in-a-react-app/>

几乎每个 web 应用程序都需要某种形式的身份验证。这可以防止未经授权的用户访问应用程序的内部工作。

在本教程中，您将学习如何使用 Firebase SDK 来验证您的 React 应用程序。

## 如何使用 Firebase 进行认证

使用 Firebase 进行身份验证对最终用户和开发人员来说都很容易。Firebase Authentication 提供后端服务、易于使用的 SDK 和现成的 UI 库。这使您可以专注于您的用户，而不是复杂的基础设施来支持他们。

Firebase 支持多种用户认证方式。它们包括通过电子邮件地址、第三方提供商如 Twitter、脸书、Github、谷歌、微软等进行认证。

## 如何设置 Firebase 身份验证

在您为 React 应用程序设置和初始化 Firebase SDK 之前，您需要使用您的 Google 帐户注册 Firebase。

如果您已经有一个 Firebase 帐户，请登录并按照提示创建一个新项目。为您的项目选择一个合适的名称，然后点击**继续。**

对于本教程，我们将我们的项目命名为 **Focus-app。**您的下一个屏幕将会提示您启用 Google Analytics。我们不需要它。你可以选择关闭它。

祝贺您，您已经成功设置了 Firebase 控制台。您的下一个屏幕将是 Firebase 控制台仪表板，如下所示:

![Screenshot--197-](img/a6ee326ff5efa956fbea282a676c1f3e.png)

使用 Firebase 认证用户有很多方法。在本教程中，我们将使用用户的电子邮件地址和密码来验证用户。

要开始使用 Firebase SDK 身份验证，请在**构建**类别中选择身份验证 SDK。

接下来，我们将设置我们的登录方法。点击**设置登录方式**，从登录提供商列表中选择**邮箱/密码**。

![Screenshot--209--3](img/4d541fb7690b398c7cb1f5cc746e20f9.png)

启用**电子邮件/密码**选项，让用户使用他们的电子邮件地址和密码注册，然后点击**保存**。

## 如何使用 React 路由器设置 React 应用程序

身份验证是大多数应用程序都需要的功能。我们将使用以下命令设置 React 应用程序:

```
$ npx create-react-app focus-app
```

在我们开始应用程序之前，我们应该设置我们的反应路由器领域。您可以通过运行以下命令来安装您的 **react-router-dom** :

```
$ npm i -D react-router-dom
```

要设置你的 React 应用程序使用 **react-router-dom** ，你可以阅读他们的[文档](https://reactrouter.com/en/6.4.2/start/tutorial)。设置好路线后，app.js 文件应该包含以下代码:

```
import React, {useState, useEffect} from 'react';
import Home from './page/Home';
import Signup from './page/Signup';
import Login from './page/Login';
import { BrowserRouter as Router} from 'react-router-dom';
import {Routes, Route} from 'react-router-dom';

function App() {

  return (
    <Router>
      <div>
        <section>                              
            <Routes>                                                                        <Route path="/" element={<Home/>}/>
               <Route path="/signup" element={<Signup/>}/>
               <Route path="/login" element={<Login/>}/>
            </Routes>                    
        </section>
      </div>
    </Router>
  );
}

export default App;
```

我们设置 React 应用程序有三个路径:**主页**、**登录**和**注册**页面。

## 如何设置和配置 Firebase SDK

您有两个选项来设置 Firebase SDK。我们将使用 **npm** 选项来安装和设置 Firebase。

在安装 Firebase SDK 之前，您的机器上应该已经安装了 [npm](https://nodejs.org/en/download/) 。您可以通过运行以下命令来安装最新的 Firebase SDK:

```
$ npm install firebase
```

要初始化 Firebase，在您的 **src** 目录中创建一个 **firebase.js** 文件。在仪表板的项目设置部分获取 Firebase 配置，并将 Firebase 配置复制到您的 **firebase.js** 文件中。您的 **firebase.js** 文件应该包含以下代码:

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries
// Your web app's Firebase configuration

const firebaseConfig = {
  apiKey: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "XXXXXXXXXXXXXXXXXXXXXXXX",
  projectId: "XXXXXXXXX",
  storageBucket: "XXXXXXXXXXXXXXXXXX",
  messagingSenderId: "XXXXXXXXXXXX",
  appId: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig); 
```

恭喜你，你已经成功初始化了 Firebase。

要开始使用 Firebase SDK，您必须导入您想要使用的产品。在导入 Firebase SDK 进行身份验证之后，您的 **firebase.js** 文件应该具有以下代码:

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries
// Your web app's Firebase configuration

const firebaseConfig = {
  apiKey: "AIzaSyAAx_knJ_qqxPkJQ_xoIZnxt_c6gb6Wdys",
  authDomain: "todoapp-eeeb7.firebaseapp.com",
  projectId: "todoapp-eeeb7",
  storageBucket: "todoapp-eeeb7.appspot.com",
  messagingSenderId: "1072574112522",
  appId: "1:1072574112522:web:65fc4e184aed9894dc90f3"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Initialize Firebase Authentication and get a reference to the service
export const auth = getAuth(app);
export default app;
```

## 如何认证您的 React 应用

Firebase 有许多内置产品，其中之一是身份验证。根据其[文档](https://firebase.google.com/docs/auth)，

> 为了对应用程序的用户进行身份验证，Firebase Authentication 提供了一些很酷的功能，如后端服务、易于使用的 SDK 和现成的 UI 库。

Firebase 身份验证允许用户使用不同的登录方法登录到您的应用程序。在本教程中，我们将了解如何使用电子邮件和密码来验证登录到您的应用程序的用户。我们不会在这篇文章中使用任何样式。

要创建用户，您应该创建一个接受输入的表单，并使用 Firebase 的**createUserWithEmailAndPassword**方法创建新用户。

该表单应该将新用户的电子邮件地址和密码作为输入，并将它们传递给**createUserWithEmailAndPassword**方法。如果用户创建成功，您将被转到登录屏幕。

下面是创建用户的完整代码:

```
import React, {useState} from 'react';
import { NavLink, useNavigate } from 'react-router-dom';
import {  createUserWithEmailAndPassword  } from 'firebase/auth';
import { auth } from '../firebase';

const Signup = () => {
    const navigate = useNavigate();

    const [email, setEmail] = useState('')
    const [password, setPassword] = useState('');

    const onSubmit = async (e) => {
      e.preventDefault()

      await createUserWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
            // Signed in
            const user = userCredential.user;
            console.log(user);
            navigate("/login")
            // ...
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode, errorMessage);
            // ..
        });

    }

  return (
    <main >        
        <section>
            <div>
                <div>                  
                    <h1> FocusApp </h1>                                                                            
                    <form>                                                                                            
                        <div>
                            <label htmlFor="email-address">
                                Email address
                            </label>
                            <input
                                type="email"
                                label="Email address"
                                value={email}
                                onChange={(e) => setEmail(e.target.value)}  
                                required                                    
                                placeholder="Email address"                                
                            />
                        </div>

                        <div>
                            <label htmlFor="password">
                                Password
                            </label>
                            <input
                                type="password"
                                label="Create password"
                                value={password}
                                onChange={(e) => setPassword(e.target.value)} 
                                required                                 
                                placeholder="Password"              
                            />
                        </div>                                             

                        <button
                            type="submit" 
                            onClick={onSubmit}                        
                        >  
                            Sign up                                
                        </button>

                    </form>

                    <p>
                        Already have an account?{' '}
                        <NavLink to="/login" >
                            Sign in
                        </NavLink>
                    </p>                   
                </div>
            </div>
        </section>
    </main>
  )
}

export default Signup 
```

Firebase 允许现有用户使用他们最初注册时使用的**电子邮件**和**密码**登录。

要允许现有用户登录，您应该创建一个收集他们的电子邮件和密码的表单。该表单有一个提交按钮，每当单击该按钮时，就会调用**signInWithEmailAndPassword**方法。

您可以使用以下代码登录:

```
import React, {useState} from 'react';
import {  signInWithEmailAndPassword   } from 'firebase/auth';
import { auth } from '../firebase';
import { NavLink, useNavigate } from 'react-router-dom'

const Login = () => {
    const navigate = useNavigate();
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    const onLogin = (e) => {
        e.preventDefault();
        signInWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
            // Signed in
            const user = userCredential.user;
            navigate("/home")
            console.log(user);
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode, errorMessage)
        });

    }

    return(
        <>
            <main >        
                <section>
                    <div>                                            
                        <p> FocusApp </p>                       

                        <form>                                              
                            <div>
                                <label htmlFor="email-address">
                                    Email address
                                </label>
                                <input
                                    id="email-address"
                                    name="email"
                                    type="email"                                    
                                    required                                                                                
                                    placeholder="Email address"
                                    onChange={(e)=>setEmail(e.target.value)}
                                />
                            </div>

                            <div>
                                <label htmlFor="password">
                                    Password
                                </label>
                                <input
                                    id="password"
                                    name="password"
                                    type="password"                                    
                                    required                                                                                
                                    placeholder="Password"
                                    onChange={(e)=>setPassword(e.target.value)}
                                />
                            </div>

                            <div>
                                <button                                    
                                    onClick={onLogin}                                        
                                >      
                                    Login                                                                  
                                </button>
                            </div>                               
                        </form>

                        <p className="text-sm text-white text-center">
                            No account yet? {' '}
                            <NavLink to="/signup">
                                Sign up
                            </NavLink>
                        </p>

                    </div>
                </section>
            </main>
        </>
    )
}

export default Login
```

Firebase 认证提供其他登录方法，包括使用 GitHub、微软、苹果或联合身份提供商，如[谷歌登录](https://firebase.google.com/docs/auth/web/google-signin)或[脸书登录](https://firebase.google.com/docs/auth/web/facebook-login)。

成功登录后，用户的信息可被访问，并可用于向你的应用添加更多功能，包括保护你的路线。

为了获得当前注册的用户，我们在 Auth 对象上设置了一个观察者。我们可以使用下面的代码在 **Home** 组件中得到一个当前注册的用户:

```
import React, { useState, useEffect } from 'react';
import { onAuthStateChanged } from "firebase/auth";
import { auth } from '../firebase';

const Home = () => {

    useEffect(()=>{
        onAuthStateChanged(auth, (user) => {
            if (user) {
              // User is signed in, see docs for a list of available properties
              // https://firebase.google.com/docs/reference/js/firebase.User
              const uid = user.uid;
              // ...
              console.log("uid", uid)
            } else {
              // User is signed out
              // ...
              console.log("user is logged out")
            }
          });

    }, [])

  return (
    <section>        
      …
    </section>
  )
}

export default Home 
```

为了完成我们的 Firebase 身份验证，在创建用户并登录之后，应该有一种方法可以注销用户。

要注销用户，从 Firebase 调用 **signOut** 方法。登录到**首页**路线后，每当点击**注销**按钮，就会出现一个注销按钮。按钮应该有一个从 Firebase auth 调用 **signOut** 方法的 **onClick** 事件。如果注销成功，控制台上将显示一条成功消息。

下面是注销用户的完整代码:

```
import React from 'react';
import {  signOut } from "firebase/auth";
import {auth} from '../../firebase';
import { useNavigate } from 'react-router-dom';

const Home = () => {
    const navigate = useNavigate();

    const handleLogout = () => {               
        signOut(auth).then(() => {
        // Sign-out successful.
            navigate("/");
            console.log("Signed out successfully")
        }).catch((error) => {
        // An error happened.
        });
    }

    return(
        <>
            <nav>
                <p>
                    Welcome Home
                </p>

                <div>
        			<button onClick={handleLogout}>
                        Logout
                    </button>
        		</div>
            </nav>
        </>
    )
}

export default Home; 
```

## 结论

Firebase 身份验证允许您创建用户的身份。它无缝认证用户。

通过这篇文章，我希望您已经获得了足够的知识来构建验证用户身份的应用程序。可以查看[文档](https://firebase.google.com/docs/auth)了解更多。

这里有一个[链接](https://github.com/IsraelChidera/focus-app)到使用 Firebase 认证的 GitHub 存储库。它还使用 **TailwindCSS** 进行造型，使用 **React Router** 进行路由。

编码快乐！