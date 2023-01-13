# 如何使用 Firebase 和 React(用钩子)设置 GitHub 用户认证

> 原文：<https://www.freecodecamp.org/news/github-user-authentication-using-firebase-and-reactjs-with-hooks/>

在本教程中，我将带您了解使用 Firebase 和 React(带钩子)创建 GitHub 用户认证系统的过程。

如果您以前曾经尝试过构建身份验证系统，您可能会同意这是一件痛苦的事情。这就是 Firebase 介入的地方。Firebase 提供了开箱即用的用户身份验证，因此您无需从头开始编写复杂的身份验证代码，从而节省了大量时间。

在本文中，我们将构建一个简单的 Profile Card 组件，它将显示经过身份验证的用户 GitHub 配置文件数据，如配置文件图片、显示名称和用户名。

我们还将看到如何像专家一样使用`ContextAPI`和`useReducer`钩子来管理认证用户的状态。

🚀我们开始吧！

请注意，您需要对 React 有一个基本的了解才能学习本教程。

*   [如何创建新的 React 项目](#how-to-create-a-new-react-project)
*   [🧹项目前内务管理](#-pre-project-housekeeping)
*   [如何设置 Firebase](#how-to-set-up-firebase)
*   [如何设置 Firebase Auth](#how-to-set-up-firebase-auth)
*   [如何在钩子中创建 GitHub 日志](#how-to-create-a-github-log-in-a-hook)
*   [如何构建登录功能](#how-to-build-the-login-functionality)
*   [如何构建注销功能](#how-to-build-the-log-out-functionality)
*   [如何创建认证上下文](#how-to-create-an-authentication-context)
*   [如何创建上下文提供者](#how-to-create-a-context-provider)
*   [如何创建减速器功能](#how-to-create-a-reducer-function)
*   [如何连接上下文和缩减器](#how-to-wire-up-context-and-reducer)
*   [如何更新授权上下文值](https://www.freecodecamp.org/news/p/95ce6097-8ce9-427b-afeb-a8731e785c4b/how-to-update-the-auth-context-value)
*   [如何保持授权状态](#how-to-persist-auth-state)
*   [如何增加档案卡组件](#how-to-add-a-profile-card-component)
*   [如何将用户保存到 Firebase](#how-to-save-a-user-to-firebase)
*   [🙏感谢您阅读本教程](#-thank-you-for-reading-this-tutorial)

## 如何创建新的 React 项目

开始的第一步是使用 [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html) 工具生成一个新的 react 项目。如果您还没有安装它，首先打开您的终端并运行以下命令进行全局安装:

```
npm install -g create-react-app 
```

安装完成后，您可以运行以下命令来生成一个新的 React 项目:

```
npx create-react-app react-firebase-github-auth 
```

一旦执行了这个命令，create-react-app 将需要几分钟的时间来下载和安装所有需要的依赖项。这可能看起来时间很长，但这很正常！你可以去给自己泡杯茶。

当该过程完成时，使用`cd react-firebase-github-auth`命令将 cd 放入目录。现在我们在我们的项目目录中，让我们运行`code .`在您的代码编辑器中打开项目文件夹(我使用的是 VS 代码)。

现在通过按下`ctrl + ``打开您的终端并运行`npm start`来启动开发服务器。这将打开一个新的浏览器标签，我们的应用程序在里面运行。您的浏览器将显示如下内容:

![BqGj8FM](img/3d34574337bc65ba45c927964667d571.png)

## 🧹项目前内务管理

既然您已经创建了一个新项目，我们将做一些项目前的整理工作。

我们之前运行的`create-react-app`命令创建了很多我们在项目中不需要的文件。为了保持整洁，我们将删除一些文件。

首先要做的是用下面的代码替换掉`App.js`文件的全部代码:

```
const App = () => {

  return (
    <div className="App">
      <button className="btn">
        Login With Github
      </button>
      <button className="btn">
        Log Out
      </button>
    </div>
  );
};

export default App; 
```

现在你已经替换了你的 **App.js** 文件的全部代码，让我们为你的 **App** 组件添加一些 CSS。打开您的`index.css`，用以下样式替换内容。我们不会把重点放在造型上，所以这里有一些风格供你使用:

```
@import url("https://fonts.googleapis.com/css2?family=Overpass:wght@400;700&display=swap");

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.App {
  font-family: "Overpass", sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: hsl(216, 12%, 8%);
}

.btn {
  border: none;
  background-color: hsl(25, 97%, 53%);
  cursor: pointer;
  border-radius: 6px;
  color: white;
  font-weight: bold;
  padding: 12px 14px;
  font-size: 18px;
  margin-top: 8px;
}

.btn:hover {
  background-color: hsla(25, 97%, 53%, 0.668);
  transition: all 100ms linear;
}

.github-logo {
  width: 18px;
  margin-right: 6px;
  vertical-align: middle;
} 
```

让我们继续删除一些不必要的文件。打开您的终端并运行以下命令。确保您位于项目文件夹的根目录中:

```
cd src

rm -- App.test.js App.css logo.svg serviceWorker.js setupTests.js

cd .. 
```

注意:如果您停止了服务器来执行上面提到的终端任务，您必须使用 npm start 再次启动它。

## 如何设置 Firebase

在深入 React 和编写一些好东西之前，我们需要通过 Firebase 控制台建立我们自己的 Firebase 项目。

首先，在浏览器中导航到 [Firebase 控制台](https://console.firebase.google.com/)。确保您已登录您的 Google 帐户。

现在，单击 Add project，您应该会看到下面的屏幕:

![1CquSV9Qf](img/e4b3a816b8e618a66942ba239a90cec0.png)

您可以随意命名您的项目。但对于这个例子，我们将称之为“react-firebase-github-auth”。完成后，点击**继续**按钮。您应该会看到类似这样的内容:

![SfFPlAwOu](img/650d3c70613eeb0619eb0365cd42d65d.png)

现在，你会看到一个切换按钮，为这个项目启用谷歌分析。但是我们不需要它，所以只需点击切换按钮来禁用它。如果您愿意，可以稍后启用它。

一旦你创建了你的 Firebase 项目，点击**继续**按钮进入你的项目仪表板，看起来像这样:

![dM0vleyHO--1-](img/9abe2028eb0d8e7e3496f10744e8c693.png)

现在我们已经建立了一个 Firebase 项目，让我们继续注册我们的 React 应用程序，开始使用 Firebase。

因此，要做到这一点，请单击我在上面的截图中指出的代码图标(>)。现在，给它一个名字。对于这个教程，我将它命名为“react-firebase-github-auth”并点击**注册应用**来注册我们的应用。您应该会看到以下配置代码:

![sY07iiBu6--1--2](img/d1e89617e20f06523c8cb22f6c6e1155.png)

点击复制到剪贴板按钮复制整个配置代码，并点击`Continue to console`按钮。然后，我们需要在项目中创建一个文件来存储它。

让我们转到 React 应用程序，在`src`中创建一个新文件夹，并将其命名为`firebase`。在这个 Firebase 文件夹中，创建一个新文件，命名为`config.js`，并将你的配置代码粘贴到这个文件中。

如果需要，可以从 config.js 文件中删除这些注释。您也不需要创建 app 变量。相反，您可以调用`initializeApp()`而不用将其保存到变量中。现在，您的代码应该类似于以下内容:

```
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "AIzaSyA2jZRiXP36UXbBS2xuV1UE4Yr3dYwhX24",
  authDomain: "react-firebase-github-au-eb675.firebaseapp.com",
  projectId: "react-firebase-github-au-eb675",
  storageBucket: "react-firebase-github-au-eb675.appspot.com",
  messagingSenderId: "605356741694",
  appId: "1:605356741694:web:5efdfac0ea6046e25c2d6f",
};

// Initialize Firebase
initializeApp(firebaseConfig); 
```

现在，让我们使用 npm 将 Firebase 安装到我们的项目中。为此，请打开您的终端并运行以下命令:

```
npm install firebase 
```

此工作流使用 npm，并需要模块捆绑器或 JavaScript 框架工具。这是因为 v9 SDK 经过优化，可以与模块捆绑器一起工作，以消除未使用的代码(树抖动)并减小应用程序的大小。

## 如何设置 Firebase 身份验证

在 Firebase 控制台中创建 Firebase 项目后，需要启用 GitHub 提供程序。为此，请按照下列步骤操作:

*   转到 Firebase 项目仪表板，点击侧边栏上的**身份验证**选项卡。
*   现在，点击**开始**按钮，然后点击**登录方式**选项卡，然后选择 **GitHub 登录**提供商。
*   之后，点击切换按钮启用 Github Auth。

现在，您需要从 GitHub 开发人员控制台添加`Client ID`和`Client Secret`。

要获得您的`Client ID`和`Client Secret`，首先[在 GitHub 上将您的应用](https://github.com/settings/applications/new)注册为开发者应用，您会看到一个如下所示的申请表:

![P0SjzJSFc--1--3](img/754075ff2f7ee19a0888ccfdbec5e2f2.png)

填写此表格，并确保您的 **Firebase OAuth 重定向 URI** (例如，my-app-12345.firebaseapp.com/__/auth/handler)被设置为您的**授权回调 URL** 。你可以在这里找到你的**火焰基地 OAuth 重定向 URI** :

![GltEGGRMI](img/8923f7bd48b81e3883b6ea5b0cb5cc77.png)

现在点击**注册申请**。您将看到您的`Client ID`，但是您需要通过点击**生成新的客户端密码**按钮来生成您的`Client secrets`。现在，从 GitHub 应用程序页面复制您的`Client ID`和`Client secrets`，并粘贴到 Firebase GitHub 登录提供者表单中。

点击**保存**。

现在我们已经在 Firebase 项目上启用了 GitHub Auth，是时候在前端初始化它了。

首先，打开您的`config.js`文件，从`initializeApp`导入语句下面的`firebase/auth`导入`getAuth`。

然后我们将创建一个名为`auth`的变量，并将其设置为等于`getAuth()`。最后，从这里导出这个`auth`变量。

您的代码应该如下所示:

```
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  apiKey: "AIzaSyAaHLAnc5DXxHHFtcjIO7dQVe9i9OKsFqg",
  authDomain: "fir-github-auth-b5110.firebaseapp.com",
  projectId: "fir-github-auth-b5110",
  storageBucket: "fir-github-auth-b5110.appspot.com",
  messagingSenderId: "857975576429",
  appId: "1:857975576429:web:0a1d4e6a5a3b08febcac64",
};

// Initialize Firebase
initializeApp(firebaseConfig);

// Initialize Firebase Auth
const auth = getAuth();

export { auth }; 
```

现在我们已经完成了所有的设置，我们可以在任何需要的组件中使用 Firebase 认证服务，比如**登录**和**注销**！

## 如何在钩子中创建 GitHub 日志

到目前为止，我们已经为我们的项目添加了一个 Firebase。现在，让我们使用它。

首先，让我们创建一个自定义挂钩，使用用户的 GitHub 帐户注册用户。为此，首先，在`src`文件夹中创建一个名为`hooks`的文件夹。在里面，创建一个新文件，并命名为`useLogin.js`。在该文件中添加以下代码:

```
import { GithubAuthProvider, signInWithPopup } from "firebase/auth";
import { auth } from "../firebase/config";
import { useState } from "react";

export const useLogin = () => {
  const [error, setError] = useState(false);
  const [isPending, setIsPending] = useState(false);
  const provider = new GithubAuthProvider();

  const login = async () => {
    setError(null);
    setIsPending(true);

    try {
      const res = await signInWithPopup(auth, provider);
      if (!res) {
        throw new Error("Could not complete signup");
      }

      const user = res.user;
      console.log(user);
      setIsPending(false)
    } catch (error) {
      console.log(error);
      setError(error.message);
      setIsPending(false);
    }
  };

  return { login, error, isPending };
}; 
```

我将遍历上面的代码。别担心——这并不可怕！你很快就会明白的。

前两行非常简单——它们只是从我们的 config.js 文件中导入一些我们稍后会用到的 Firebase 和 auth。在第三行，我们从 React 模块导入了`useState`钩子。

第五行是事情变得有趣的地方！我们已经创建了一个名为`useLogin`的函数，并在同一行中立即将其导出。在这个函数中，我们使用 useState 钩子创建了两个状态:`error`和`isPending`。

我们将使用错误状态来显示错误，使用 isPending state 来显示挂起状态。

例如，假设用户点击注册按钮，我们将 isPending 设置为`true`。当注册请求完成时，我们可以使用 setisPending(false)将其设置回 false。我们可以使用这个`isPending`状态来添加一个加载器，以在我们的组件中显示一个挂起状态。

之后，我们创建了 GitHub provider 对象的一个实例:

```
const provider = new GithubAuthProvider(); 
```

然后，我们创建一个登录函数，并在其中添加了这段代码:

```
setError(null);
setIsPending(true); 
```

上面的代码运行时，会自动将错误的状态设置为 null，将`isPending`的状态设置为 true。这意味着当您登录时，您不会看到任何错误，但是您的页面上会有挂起状态。

就在它下面，我们添加了一个`try-catch`块，在 try 块中，我们尝试使用`signInWithPopup`函数注册用户。但是我们有两种方法来提示用户使用他们的 GitHub 账户登录:要么通过**打开一个弹出窗口**，要么通过**重定向到登录页面**。

在这篇博客中，我们使用带两个参数的`signInWithPopup`。第一个是`auth`，第二个是`provider`。

在`catch`块中，如果错误发生在 try 块中，我们将捕获它。如果发生了，那么我们将使用`setError(error.message)`来设置它。我们还将设置`isPending`等于`false`,因为我们已经完成了我们一直试图做的事情，现在有了一个响应——一个登录的用户或一个错误。

最后，我们将导出**登录**函数、**错误**和**挂起**，以便我们可以在其他组件中使用它们。

## 如何构建登录功能

现在我们已经创建了一个`useLogin`钩子，让我们用它来登录使用 GitHub 帐户的用户。

让我们从从`useLogin.js`导出的 **App.js** 文件中导入来自`./hooks/useLogin`的`useLogin`钩子开始。然后我们在 App 组件的开头调用这个钩子，析构两个东西:`login`函数和`isPending`状态:

```
import { useLogin } from "./hooks/useLogin";
....
const App = () => {
  const { login, isPending } = useLogin();

  return (
    <div className="App">
.... 
```

现在，让我们给我们的`Login With Github`按钮添加一个`onClick`事件处理程序，以便当用户点击按钮时调用`login`函数。我们还将有条件地显示一个按钮文本——如果`isPending`为真，则显示`Loading...`，否则显示`Login With Github`。

您的按钮代码应该如下所示:

```
<div className="App">
    <button className="btn" onClick={login}>
        {isPending ? "Loading..." : "Login With Github"}
    </button>
</div> 
```

现在，让我们测试我们的登录按钮。点击`Login With Github`按钮，应该会打开一个新窗口，然后你需要授权你的 React 应用。然后，您会看到控制台上打印出一个用户对象，看起来像这样:

![2A4p7e3T3-2](img/2b94ab3ef92f2e05d7298bf75ea25f6d.png)

现在打开 Firebase 项目仪表板，并单击 Authentication 选项卡。您将看到一个登录的用户，看起来像这样:

![Gda8EuxXf](img/e4286e820bf69f45ff0419cf936acc47.png)

## 如何构建注销功能

现在我们可以登录用户了，接下来我们做什么呢？好了，现在是时候研究注销用户的功能了。这实际上比让他们登录要容易得多。

所以，首先，让我们创建一个`useLogout`定制钩子来注销用户。为此，在`hooks`文件夹中创建一个文件并命名为`useLogout.js`。在该文件中，添加以下代码:

```
import { signOut } from "firebase/auth";
import { auth } from "../firebase/config";

export const useLogout = () => {

  const logout = async () => {
    try {
      await signOut(auth);
      console.log("user logged out")
    } catch (error) {
      console.log(error.message);
    }
  };

  return { logout };
}; 
```

我将遍历上面的代码。你很快就会明白的。

前两行非常简单——我们只需从 Firebase 导入`signOut`函数，从我们的 config.js 文件导入`auth`,我们稍后会用到。

第四行是事情变得有趣的地方！我们已经创建了一个名为`useLogout`的函数，并在同一行中立即将其导出。

然后我们创建一个异步`logout`函数，并在同一行上导出它。在其中，我们添加了一个`try-catch`块。在 try 块中，我们使用由`firebase/auth`提供的`signOut`方法注销用户。在 catch 块中，如果错误发生在 try 块中，我们将捕获它。

现在，让我们像这样将这个`useLogout`挂钩导入 App.js 组件:

```
import { useLogout } from "./hooks/useLogout"; 
```

就在我们调用`useLogin()`钩子的线下面，也调用`useLogout`钩子:

```
const { logout } = useLogout(); 
```

现在，让我们给`Log Out`按钮添加一个`onClick`事件处理程序，以便在用户点击按钮时调用`useLogout`函数。您的按钮代码应该如下所示:

```
<button className="btn" onClick={logout}>
    Log Out
</button> 
```

现在，是时候测试我们的注销功能了。如果您已经登录，请点击`Log Out`按钮。如果您尚未登录，请先登录，然后单击“注销”按钮。现在，您应该会在控制台上看到一个**“用户注销”**:

![n3gT5cWkg](img/6bc99a61ffac1aca7dfcd9204dccab47.png)

## 如何创建身份验证上下文

既然我们能够注册和注销用户，我们希望在用户登录时将用户对象存储在某种全局状态中。这将使我们能够访问组件的用户数据，而无需进行适当的演练。

如果你不了解上下文，让我给你简单解释一下:

[Context API](https://reactjs.org/docs/context.html) 是一种不使用 props 在 React 组件之间共享数据的方式。这意味着您可以将数据直接传递给需要它们的组件，而不是通过中间组件。它通过为数据提供一种全局存储来做到这一点。

现在，我们将为 React 应用程序创建一个上下文。我们要做的第一件事是在`src`文件夹中为我们的上下文创建一个文件夹，我们将其命名为`contexts`。这个文件夹将是我们放置所有应用程序上下文的地方(如果我们有多个上下文，那么我们将把它们都放在这个文件夹中。但是对于这个小的 React 应用程序，我们将只为 auth 创建一个上下文。

现在，让我们在 contexts 文件夹下为我们的上下文创建一个文件，并将其命名为`AuthContext.js`。我们新的 AuthContext.js 文件当前为空！打开它，给它头两行:

```
import { createContext } from "react";

export const AuthContext = createContext(); 
```

React 提供的这个`createContext`函数基本上创建了一个上下文对象。我们将使用这个对象来消费组件中的上下文。我将在后面的部分向您展示如何使用上下文。

## 如何创建上下文提供者

我们在上面创建的 AuthContext 让我们可以访问用于为所有子元素提供上下文的`AuthContext.Provider`组件。

为了设置上下文的值，我们需要使用`<AuthContext.Provider value={/* some value */}>`上可用的`value`属性。

复制下面的代码片段，然后粘贴到`createContext()`函数下的`AuthContext.js`中。

```
const AuthContextProvider = ({ children }) => {

  return (
    <AuthContext.Provider value={/* some value */}>
      {children}
    </AuthContext.Provider>
  );
};

export default AuthContextProvider; 
```

## 如何创建减速器功能

首先，我们来说说什么是减速器。reducer 函数是一个接受两个参数的 JavaScript 函数:**状态**和**动作**。state 参数是应用程序的当前状态，而 action 参数是描述用户正在执行的操作的对象。

要创建一个 reducer 函数，我们先在`src`文件夹中创建一个文件夹，并将其命名为`reducers`。在这个 reducers 文件夹中，创建一个文件并命名为`authReducer.js`。

我们的新`authReducer.js`文件目前是空的！复制以下代码片段，然后将其粘贴到 authReducer.js 中。

```
export const authReducer = (state, action) => {
  switch (action.type) {
    case "LOGIN":
      return { ...state, user: action.payload };
    case "LOGOUT":
      return { ...state, user: null };
    case "AUTH_IS_READY":
      return { ...state, user: action.payload, authIsReady: true };
    default:
      return state;
  }
}; 
```

让我浏览一下上面的代码:

上面的代码基本上是一个带两个参数的函数:**状态**和**动作**。在这个函数内部，我们添加了一个开关案例来确定动作类型，并根据动作类型运行相应的案例。

例如，如果你点击你的网页上的一个登录按钮，它发出一个“登录”动作，那么这将由这些 case 语句之一来处理。如果其中一种情况不匹配，那么它将返回不变的状态。这意味着如果没有动作被分派，那么什么都不会发生！

你可能想知道这个动作意味着什么。动作是描述如何更新状态的对象。这些动作由用户在一些用户交互中执行，例如点击按钮或按箭头键。

这个动作对象通常有两个键:

1.  **类型:**正在执行的动作的类型。这只是一个字符串，使用描述性的字符串很重要。例如，你可以使用**【登录】**或**【注销】**，这取决于你在做什么。
2.  **有效负载:**动作的有效负载是您需要与正在执行的动作类型一起传递的任何数据。如果你是注册用户，那么这可能是用户数据。

如果你想知道我要说什么，不要担心——很快就会明白了！

## 如何连接上下文和 Reducer

让我们将我们的 **authReducer** 与我们的 **AuthContext** 连接起来。为此，首先将减速器函数从`authReducer.js`导入到`AuthContext.js`文件中。

```
import { authReducer } from "../reducers/authReducer"; 
```

现在，我们将使用一个`useReducer`钩子。`useReducer(reducer, initialState)`有两个参数:一个**减速器**和一个**初始状态**。

在本例中，我们只是传入我们的`authReducer`函数和一个初始状态对象，该对象有两个属性:**用户**和**授权就绪**:

```
{
    user: null,
    authIsReady: false,
} 
```

因此，让我们将`useReducer`钩子从 React 导入到这个文件中。更新您的第一行，使其如下所示:

```
import { createContext, useReducer } from "react"; 
```

这个 useReducer 钩子返回给我们一个包含两个条目的数组，其中既有我们当前的`state`方法，也有`dispatch`方法。(如果您熟悉 Redux，那么您已经知道这种分派方法是如何工作的。).

现在，让我们将下面的代码添加到`AuthContextProvider`组件的 return 关键字之上:

```
 const [state, dispatch] = useReducer(authReducer, {
    user: null,
    authIsReady: false,
  }); 
```

现在我们已经使用[数组析构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#examples)从数组中析构了`state`和`dispatch`值，让我们更新`AuthContext.Provider`的`value`属性，使其包含所有这些值:

```
<AuthContext.Provider value={{ ...state, dispatch }}> 
```

我们还可以在 return 语句的上方添加一个`console.log(state)`，这样我们就可以看到用户的状态发生了变化。

现在我们已经创建了一个上下文，让我们将`AuthContextProvider`组件导入到`index.js`文件中来使用它:

```
import AuthContextProvider from "./contexts/AuthContext"; 
```

现在用`AuthContextProvider`组件包装`App`组件，为整个应用程序提供一个上下文。为此，请像这样包装您的应用程序组件:

```
<AuthContextProvider>
    <App />
</AuthContextProvider> 
```

现在，让我们回到我们的应用程序组件，看看如何使用 AuthContext 对象来获取用户详细信息。首先，让我们将`AuthContext.js`中的`AuthContext`对象导入到`App.js`文件中:

```
import { AuthContext } from "./contexts/AuthContext"; 
```

我们还需要将`useContext`钩子导入到这个文件中，这样我们就可以使用 AuthContext 对象:

```
import { useContext } from "react"; 
```

一个`useContext`钩子接受一个上下文对象(从`createContext()`返回的值)并返回该上下文的当前上下文值。

当前上下文值由`<AuthContext.Provider value={{ ...state, dispatch }}>`的值属性决定。

现在让我们用我们的`AuthContext`对象来调用这个`useContext`。将这段代码添加到 App 组件的 return 语句上方

```
const { user } = useContext(AuthContext);
console.log(user); 
```

现在，让我们刷新页面。您应该看到`null`从您的`App.js`文件中打印出来。如果你尝试使用你的 GitHub 账户登录，它仍然会显示`null`。

如果您想知道为什么即使您登录了，您也只能看到`null`，这是因为，在登录或注销时，我们没有更新我们的上下文值——它总是显示我们传递的初始值。

为了解决这个问题，我们需要在登录或注销时使用`dispatch`方法更新上下文值。

## 如何更新授权上下文值

既然我们已经设置了 Auth Context 和 Auth Reducer，现在让我们使用它们来更新状态。

首先，让我们将`AuthContext.js`中的`AuthContext`对象导入到`useLogin.js`文件中:

```
import { AuthContext } from "../contexts/AuthContext"; 
```

现在将`useContext`钩子导入到这个文件中，这样我们就可以使用 AuthContext 对象了——更新您的`useState`导入行，使其如下所示:

```
import { useContext, useState } from "react"; 
```

现在让我们用 AuthContext 对象调用这个 useContext。将这段代码添加到`useLogin`函数中的`GithubAuthProvider()`下面:

```
const { dispatch } = useContext(AuthContext); 
```

现在，让我们调度`LOGIN`动作来更新状态。因此，要做到这一点，只需在 try 块中的`user`变量下面调用带有 action 对象的`dispatch`函数。您的 try 块代码应该如下所示:

```
try {
      const res = await signInWithPopup(auth, provider);
      if (!res) {
        throw new Error("Could not complete signup");
      }

      const user = res.user;
      dispatch({ type: "LOGIN", payload: user });

      console.log(user);
      setIsPending(false);
    } 
```

保存您的文件并尝试再次登录，当您登录时，您应该看到一个用户对象从 App.js 文件打印在控制台上。

现在，让我们对注销功能做同样的事情，以便在用户注销时再次更新状态。

为此，打开您的`useLogout.js`文件并从`AuthContext.js`中导入`AuthContext`对象:

```
import { AuthContext } from "../contexts/AuthContext"; 
```

现在将`useContext`钩子导入到这个文件中:

```
import { useContext } from "react"; 
```

现在让我们用 AuthContext 对象调用这个 useContext。将这段代码添加到`useLogout`函数内的`logout`函数上方:

```
const { dispatch } = useContext(AuthContext); 
```

现在，让我们调度`LOGOUT`行动。因此，要做到这一点，只需在 try 块中的`await signOut(auth)`下面调用带有 action 对象的`dispatch`函数。您的 try 块代码应该如下所示:

```
try {
      await signOut(auth);
      dispatch({ type: "LOGOUT" });
      console.log("user logged out");
    } 
```

保存您的文件并尝试注销。当您注销时，您应该会看到控制台上打印出`null`和`user logged out`。

我们现在已经成功地实现了使用 Firebase 和 ReactJS 验证用户身份背后的逻辑。

但是还有一个问题:当你试图刷新页面时，你会看到控制台上打印的`null`。这意味着当您刷新时，您会自动注销！

我们不想在更新时注销我们的用户。我们可以通过使用由`firebase/auth`提供的 [onAuthStateChanged](https://firebase.google.com/docs/auth/web/start#set_an_authentication_state_observer_and_get_user_data) 方法来持久化 auth 状态来解决这个问题。

## 如何保持身份验证状态

现在，打开您的`AuthContext.js`文件，像这样导入`onAuthStateChanged`:

```
import { onAuthStateChanged } from "firebase/auth"; 
```

现在，让我们将`useEffect`钩子从 React 导入到这个文件中。更新您的第一行，使其如下所示:

```
import { createContext, useEffect, useReducer } from "react"; 
```

现在，从`config.js`文件导入`auth`:

```
import { auth } from "../firebase/config"; 
```

然后在 return 语句的正上方，添加这段代码:

```
 useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (user) => {
      dispatch({ type: "AUTH_IS_READY", payload: user });
    });
    return unsubscribe;
  }, []); 
```

让我带你快速浏览一下上面的代码，你马上就会明白所有的事情。

在第一行中，我们使用一个名为`useEffect()`的钩子，它接受一个函数和一个空的依赖数组作为参数。

然后，我们定义一个带两个参数的`onAuthStateChanged`方法:首先，它接受我们从`config.js`文件中导出的`auth`。第二，它采用一个回调函数，在用当前认证状态注册了`onAuthStateChanged`观察器之后以及每当认证状态改变时，该函数会立即被调用**。**

在这个回调函数中，我们传递从用户对象收到的任何数据，并设置我们的 dispatch 方法来调度一个类型为`AUTH_IS_READY`的动作，并将有效负载作为我们的用户对象来更新上下文的值。

最后，onAuthStateChanged()函数返回[取消订阅函数](https://firebase.google.com/docs/reference/js/v8/firebase.auth.Auth#onauthstatechanged)来取消注册`onAuthStateChanged`观察者。我们将这个函数保存在一个变量中，并将其命名为`unsubscribe`。最后，我们返回这个`unsubscribe`函数进行清理，以避免内存泄漏。

现在，如果您在登录后进行刷新，您将看到控制台上打印出一个用户对象。

这就是认证逻辑的全部内容。在下一节中，我们将添加一个 ProfileCard 组件来显示登录的用户数据。

## 如何添加档案卡组件

让我们创建一个显示登录数据的 Profile Card 组件。为此，首先在`src`文件夹中创建一个`components`文件夹。在这个文件夹中创建一个文件，命名为`ProfileCard.js`，并添加这段代码:

```
import React from "react";
import { useLogout } from "../hooks/useLogout";

const ProfileCard = ({ user }) => {
  const { logout } = useLogout();
  return (
    <>
      <div className="profile-card">
        <img className="profile-img" src={user.photoURL} alt="" />
        <p>
          Name: <span>{user.displayName}</span>
        </p>
        <p>
          Username: <span>{user.reloadUserInfo.screenName}</span>
        </p>
        <p>
          Email: <span>{user.email}</span>
        </p>
        <p>
          User ID: <span>{user.uid}</span>
        </p>
      </div>
      <button className="btn" onClick={logout}>
        Log Out
      </button>
    </>
  );
};

export default ProfileCard; 
```

如您所见，该组件从其父组件(即`App`)中获取一个用户道具。然后我们使用对象析构来析构这个用户属性。

现在，让我们为这个 ProfileCard 组件添加一些 CSS。只需将以下 CSS 复制并粘贴到 index.css 文件中:

```
.profile-card {
  text-align: center;
  background-color: hsl(213, 19%, 18%);
  border-radius: 8px;
  padding: 16px;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  margin-bottom: 4px;
}

.profile-img {
  border-radius: 50%;
  width: 112px;
  border: 4px solid hsl(25, 97%, 53%);
  margin-bottom: 8px;
}

p {
  color: hsl(216, 12%, 54%);
  font-size: 24px;
  font-weight: 700;
}

span {
  color: white;
  font-size: 18px;
  font-weight: 400;
} 
```

现在，让我们将这个`ProfileCard`组件导入到`App.js`文件中:

```
import ProfileCard from "./components/ProfileCard"; 
```

现在，只需用这段代码替换 return 语句的内容:

```
return (
    <div className="App">
      {user ? (
        <ProfileCard user={user} />
      ) : (
        <button className="btn login-btn" onClick={login}>
          Login With GitHub
        </button>
      )}
    </div>
  ); 
```

如您所见，我们使用一个名为`user`的属性来确定用户是否登录。

如果用户登录，那么将显示`ProfileCard`组件。如果用户没有登录，那么将显示`Login With GitHub`按钮元素。

现在，您的应用程序应该如下所示:

![31YHvHfM2](img/59f24be5ed1d4380774385e6cf66ac93.png)

但是有一个问题:如果你刷新，它首先显示一个`Login With GitHub`按钮，然后显示一个概要文件组件。这是因为 Firebase 需要一些时间来检查用户是否登录。

这可能是一种糟糕的用户体验，尤其是如果你的应用程序有用户只能在登录时才能访问的私人路线。

假设您有一个网站，用户只有在登录后才能访问私有路由，如果他们没有登录，当他们试图访问私有路由时，就会被重定向到主页。

假设一个用户已经登录并在私有路由上，出于某种原因，他们进行了刷新。你认为会发生什么？

他们将被重定向到主页，因为 Firebase 将需要一些时间来验证他们是否登录。这可能会让你网站的用户感到沮丧！

您可能想知道我们如何解决这个问题？🤔

这个问题的解决方案很简单——如果您还记得，我们在初始状态中添加了一个名为`authIsReady`的属性和`user`属性。我们将使用该属性来解决这个问题。

打开您的`App.js`文件并更新您的`useContext(AuthContext)`行，使其如下所示:

```
const { user, authIsReady } = useContext(AuthContext); 
```

现在让我们在 return 关键字后添加一个检查:

```
 return authIsReady ? (
    <div className="App">
      {user ? (
        <ProfileCard user={user} />
      ) : (
        <button className="btn login-btn" onClick={login}>
          Login With Github
        </button>
      )}
    </div>
  ) : (
    <h1>Making your auth ready, please wait for a moment</h1>
  ); 
```

这里我们只是检查`authIsReady`是否是`true`，然后显示一个`App`组件。如果不是，那么我们将显示一个`Loader`组件(在我们的例子中，我们只是显示一些难看的文本，但是您可以添加一个漂亮的加载器来代替)。

注意:`authIsReady`并不意味着我们已经登录。这仅仅意味着现在我们有了用户值，它可以是空值或用户对象。

所以，现在你已经创建了一个伟大的注册流程！

但是我们还没完。当他们第一次注册时，是时候在 Firebase 上存储一些用户数据了。

我个人认为这是一个非常好的主意。假设你正在建立一个招聘网站，你想列出所有在你的网站上注册的候选人。如果您不存储个人用户的数据，您将如何做到这一点？

现在，让我们在下一节中添加一些存储用户数据的逻辑。👇

## 如何拯救用户到 Firebase

首先，让我们创建数据库。首先，进入你的 **Firebase 项目仪表板**，点击侧边栏上的 **Firebase 数据库**标签。

现在，点击**创建数据库**按钮创建一个数据库。系统会提示您选择云 Firestore 安全规则的启动模式。现在，选择测试模式，但您可以在以后更改它。然后点击**下一个**按钮。

![EFGHwYJ-R](img/1e8b10ba4841a5dd6dd5f39ca154f19f.png)

接下来，选择您的云 Firestore 位置，并单击**启用**按钮。
现在我们已经建立了数据库，我们准备写一些代码！让我们直接开始吧。

首先，打开您的`config.js`文件并从 Firebase 导入`getFirestore`模块:

```
import { getFirestore } from "firebase/firestore"; 
```

然后我们将创建一个名为`db`的变量，并将其设置为等于`getFirestore()`。最后，像这样将`db`和`auth`一起输出:

```
// Initialize Firebase Firestore
const db = getFirestore();
export { auth, db }; 
```

现在，让我们在`firebase`文件夹中创建一个文件并命名为`createUserDocument.js`。然后添加这段代码:

```
import { collection, doc, getDocs, query, serverTimestamp, setDoc, where } from "firebase/firestore";

import { db } from "./config";

export const createUserDocument = async (user) => {
  const q = query(collection(db, "users"), where("uid", "==", user.uid));
  const { docs } = await getDocs(q);

  if (docs.length === 0) {
    const { uid, displayName, email, photoURL, reloadUserInfo } = user;

    const docRef = doc(db, `users/${uid}`);
    await setDoc(docRef, {
      displayName,
      email,
      photoURL,
      username: reloadUserInfo.screenName,
      createdAt: serverTimestamp(),
    });
  }
}; 
```

我将带您浏览上面的代码。别担心——这并不可怕！你很快就会明白的。

首先，我们创建了一个名为`createUserDocument`的异步函数。该功能将用于在我们的数据库中创建新的用户文档。

接下来，我们在`db`上使用了`query`方法，用一个`where`子句查询`users`集合，以找到所有具有与所提供的用户 uid 相等的`uid`的文档。

然后我们将这个查询`q`传递给`getDocs(q)`方法。这个方法返回一个`docs`数组。

然后我们添加了一个 if 语句来检查 docs 数组是否为空。如果它是空的，那么我们将使用带两个参数的`doc`方法创建一个新的 doc ref:`db`和`document id`。该方法返回对该文档的引用。

最后，我们将调用`setDoc`方法，该方法再次接受两个参数:`docRef`和我们希望保存到数据库的用户数据。

现在，打开您的`useLogin.js`文件并导入`createUserDocument`函数:

```
import { createUserDocument } from "../firebase/createUserDocument"; 
```

就在 dispatch 语句的上面，像这样调用`createUserDocument`函数:

```
const user = res.user;
await createUserDocument(user);
dispatch({ type: "LOGIN", payload: user }); 
```

现在，再次尝试登录，应该会将新用户保存到 Firestore。

![6KFqHkwMa](img/598f682c15f6ef568719d3e5f19b3f7e.png)

🎉就是这样！你已经成功实现了一个伟大的认证流程，现在你的应用程序已经准备好在用户第一次注册时存储用户数据。

现在去让你的应用程序变得令人惊奇吧，别忘了让我知道进展如何！

## 🙏感谢您阅读本教程

感谢您花时间阅读本指南！

我希望你喜欢这个教程，并发现它很有用。如果您有任何问题或意见，请通过 [Twitter](https://twitter.com/thefierycoder) 或 [LinkedIn](https://www.linkedin.com/in/thefierycoder/) 联系！

如果你喜欢这篇文章，如果你把它分享到你最喜欢的社交媒体平台上，我会非常感激。

请看看我的 [YouTube 频道](https://www.youtube.com/c/TheFieryCoder)，如果你喜欢就订阅吧。

干杯，下期再见！🙏

感谢您的阅读。