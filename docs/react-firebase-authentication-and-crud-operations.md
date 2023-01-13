# React 身份验证教程–如何使用 Firebase V9 和 React Router V6 设置身份验证

> 原文：<https://www.freecodecamp.org/news/react-firebase-authentication-and-crud-operations/>

大家好，在本教程中，我们将使用 React 和 Firebase V9 来设置应用程序的身份验证。

我们将为登录和注册创建 Firebase 函数，我们将为错误添加 toast 消息，我们将使用基于会话的身份验证添加私有路由。会很有趣的。

我们将使用以下包或依赖项:

1.  Firebase V9。
2.  React 路由器 V6。
3.  材质 UI。
4.  反应僵硬。

那么，我们开始吧。

## 如何设置项目

让我们首先创建一个 React 应用程序。确保你已经安装了节点，如果没有，从[https://nodejs.org/en/download/](https://nodejs.org/en/download/)安装。

要创建 React 应用程序，我们将使用下面的命令:

```
npx create-react-app react-firebase-v9
```

Creating a React application

然后，进入项目文件夹，键入 npm start 启动项目。

![Screenshot-2021-11-06-181635](img/9c22a52a31982eed862ad6c39c89e49a.png)

我们将看到这个屏幕。但是让我们为登录和注册表单清除它。

## 如何创建登录和注册表单

对于这些表单，我们来安装 Material UI。前往 https://mui.com/阅读文档。

要安装它，只需使用下面的命令:

```
// with npm
npm install @mui/material @emotion/react @emotion/styled

// with yarn
yarn add @mui/material @emotion/react @emotion/styled
```

Installing Material UI

同时，让我们在 src 文件夹中创建一个文件夹，名为`components`。在该组件中，我们将创建另一个名为`common`的文件夹。这将包含我们将使用的所有常见组件，如`Forms`、`Buttons`等等。

在公共文件夹中创建一个名为`Form.js`的文件。使其成为一个功能组件。我们将使用材质 UI 中的表单。

![Select-this.-1](img/19cedffcc194e19e564d88ad6f260ef8.png)

选择此文本字段以获取输入字段，并选取您想要的任何输入字段类型。

```
import * as React from 'react';
import Box from '@mui/material/Box';
import TextField from '@mui/material/TextField';

export default function BasicTextFields() {
    return (
        <Box
            component="form"
            sx={{
                '& > :not(style)': { m: 1, width: '25ch' },
            }}
            noValidate
            autoComplete="off"
        >
            <TextField id="outlined-basic" label="Outlined" variant="outlined" />
        </Box>
    );
} 
```

The Form Component

然后，将该组件导入到`App.js`文件中:

```
import './App.css';
import Form from './Components/Common/Form'

function App() {
  return (
    <div className="App">
      <Form />
    </div>
  );
}

export default App; 
```

App.js

这是我们将会看到的:

![Screenshot-2021-11-06-182936](img/d85ace94860f7669c9c23cb06f2eea99.png)

现在，让我们创建我们的登录和注册表单。我们将使用单个组件，但数据会有所不同。我们将传递数据作为道具。

让我们也创建一个表单标题，如下所示:

```
import * as React from 'react';
import Box from '@mui/material/Box';
import TextField from '@mui/material/TextField';

export default function BasicTextFields() {
    return (
        <div>
            <div className="heading-container">
                <h3>
                    Login Form
                </h3>
            </div>

            <Box
                component="form"
                sx={{
                    '& > :not(style)': { m: 1, width: '25ch' },
                }}
                noValidate
                autoComplete="off"
            >
                <TextField id="email" label="Enter the Email" variant="outlined" />
                <TextField id="password" label="Enter the Password" variant="outlined" />
            </Box>
        </div>
    );
} 
```

Form component

表单标题现在是静态的，但是我们将通过 props 来改变它。

现在，让我们添加一个按钮来执行某些操作——在我们的例子中是登录和注册。

在公共文件夹中创建一个名为`Button.js`的组件。

```
import * as React from 'react';
import Button from '@mui/material/Button';

export default function BasicButtons() {
    return (
        <Button variant="contained">Log in</Button>
    );
} 
```

并将其导入 Form.js，如下所示:

```
import * as React from 'react';
import Box from '@mui/material/Box';
import TextField from '@mui/material/TextField';
import Button from './Button';
export default function BasicTextFields() {
    return (
        <div>
            <div className="heading-container">
                <h3>
                    Login Form
                </h3>
            </div>

            <Box
                component="form"
                sx={{
                    '& > :not(style)': { m: 1, width: '25ch' },
                }}
                noValidate
                autoComplete="off"
            >
                <TextField id="email" label="Enter the Email" variant="outlined" />
                <TextField id="password" label="Enter the Password" variant="outlined" />
            </Box>

            <Button />
        </div>
    );
} 
```

这里，在 Form.js 中，我们有两个字段——电子邮件和密码。我们也有一个按钮来触发登录和注册，这取决于场景。

这是我们用户界面的样子:

![Screenshot-2021-11-06-184136](img/e136844c3feeaf9ab513249ee726f954.png)

## 如何创建 Firebase 应用程序

现在，让我们安装一些我们需要的额外的东西——React 路由器和 Firebase。但是在安装这两个之前，我们需要在 Firebase 中创建一个项目。

所以，去 https://firebase.google.com/创建一个项目。

在 Firebase 控制台中单击“添加项目”。

![Screenshot-2021-11-06-184523](img/991c6956329c780868db2b73ed7ee947.png)

在 Firebase 中创建新项目后，您需要创建一个应用程序。

![Screenshot-2021-11-06-184708](img/8bd5142580ab7d7e0a74b44cb751a6cb.png)

点按“添加应用程序”,然后选取“Web”。

![Screenshot-2021-11-06-184807](img/a7b5ea241da9c8691e57f263e44ce939.png)

给应用程序起一个你选择的名字，并注册应用程序。

![Screenshot-2021-11-06-184906](img/2c60fa609170523fbfff645e9342bb3f.png)

你会看到上面的屏幕。我们需要一个文件来存储所有这些配置数据，因此我们将继续创建它。

创建一个名为`firebase-config.js`的文件，并将所有这些配置数据存储在该文件中。

```
import { initializeApp } from "firebase/app";

const firebaseConfig = {
    apiKey: "AIzaSyBtRIMLkSVfptH4ASinlEfnKhP-mBwUV24",
    authDomain: "react-register-12564.firebaseapp.com",
    projectId: "react-register-12564",
    storageBucket: "react-register-12564.appspot.com",
    messagingSenderId: "1074586181097",
    appId: "1:1074586181097:web:47236fd450006cd1fabf78",
    measurementId: "G-JSN76LC2EC"
};

export const app = initializeApp(firebaseConfig); 
```

现在，让我们使用下面的命令安装 React 路由器和 Firebase。

```
npm install firebase react-router-dom
```

Installing React Router and Firebase

## 如何为注册和登录屏幕创建路线

现在，我们需要为注册和登录屏幕创建单独的路径。

在 App.js 文件中，导入`BrowerRouter`作为路由器，并将整个 div 包装在路由器中，如下所示:

```
import './App.css';
import Form from './Components/Common/Form'
import { BrowserRouter as Router } from 'react-router-dom'

function App() {
  return (
    <Router>
      <div className="App">
        <Form />
      </div>
    </Router>
  );
}

export default App; 
```

这样做可以确保我们的整个应用程序现在能够使用路由，因为我们在根级别添加了这个路由器。

现在，让我们为登录和注册页面创建路由。

```
import './App.css';
import Form from './Components/Common/Form'
import {
  BrowserRouter as Router,
  Routes,
  Route
} from "react-router-dom";

function App() {
  return (
    <Router>
      <div className="App">
        <>
          <Routes>
            <Route path='/login' element={<Form />} />
            <Route path='/register' element={<Form />} />
          </Routes>
        </>
      </div>
    </Router>
  );
}

export default App; 
```

如果我们去地址栏的`/login`路线，就会看到登录页面。在`/register`上，我们将看到注册页面。

这里，我们使用相同的表单组件进行登录和注册。让我们将道具传递给组件，使它们成为登录和注册屏幕。

```
import './App.css';
import Form from './Components/Common/Form'
import {
  BrowserRouter as Router,
  Routes,
  Route
} from "react-router-dom";

function App() {
  return (
    <Router>
      <div className="App">
        <>
          <Routes>
            <Route path='/login' element={<Form title="Login" />} />
            <Route path='/register' element={<Form title="Register" />} />
          </Routes>
        </>
      </div>
    </Router>
  );
}

export default App; 
```

我们将在 Form.js 组件中接收 props，作为函数参数中的标题。

然后，使用标题来定义哪个组件是哪个组件。所以，用 **`{{title}}`** 表单替换登录表单。还有按钮的标题。

```
import * as React from 'react';
import Box from '@mui/material/Box';
import TextField from '@mui/material/TextField';
import Button from './Button';
export default function BasicTextFields({title}) {
    return (
        <div>
            <div className="heading-container">
                <h3>
                    {title} Form
                </h3>
            </div>

            <Box
                component="form"
                sx={{
                    '& > :not(style)': { m: 1, width: '25ch' },
                }}
                noValidate
                autoComplete="off"
            >
                <TextField id="email" label="Enter the Email" variant="outlined" />
                <TextField id="password" label="Enter the Password" variant="outlined" />
            </Box>

            <Button title={title}/>
        </div>
    );
} 
```

我们将道具传递给按钮组件。我们将把它接收到按钮组件中。

```
import * as React from 'react';
import Button from '@mui/material/Button';

export default function BasicButtons({title}) {
    return (
        <Button variant="contained">{title}</Button>
    );
} 
```

现在，如果我们转到`/login`，我们将看到这样的登录屏幕:

![Screenshot-2021-11-06-193109](img/fb33e366820ee04f71315dd049fff89e.png)

在`/register`上，我们将看到注册屏幕:

![Screenshot-2021-11-06-193147](img/28bb50907316887e8330f9679521a6d9.png)

## 如何在状态中存储数据

现在，让我们为存储表单数据的输入字段创建状态。

在 App.js 文件中，使用 useState 钩子创建两个状态——电子邮件和密码。

```
import { useState } from 'react';

const [email, setEmail] = useState('');
const [password, setPassword] = useState('')
```

在上面的代码中，我们从 React 导入了 useState 并创建了两个状态。然后我们将函数`setEmail`和`setPassword`传递给表单组件。

```
<Routes>
            <Route
              path='/login'
              element={
                <Form
                  title="Login"
                  setEmail={setEmail}
                  setPassword={setPassword} />}
            />
            <Route
              path='/register'
              element={
                <Form
                  title="Register"
                  setEmail={setEmail}
                  setPassword={setPassword} />}
            />
          </Routes>
```

在 Form.js 组件中，我们将使用`onChange`事件设置电子邮件和密码。现在，你知道道具在反应中的作用了。

现在，我们需要一个函数来触发登录或注册功能，所以让我们创建它。

```
const handleAction = () => {

}
```

并将该函数作为道具传递给表单组件。

```
<Routes>
            <Route
              path='/login'
              element={
                <Form
                  title="Login"
                  setEmail={setEmail}
                  setPassword={setPassword}
                  handleAction={() => handleAction()}
                />}
            />
            <Route
              path='/register'
              element={
                <Form
                  title="Register"
                  setEmail={setEmail}
                  setPassword={setPassword}
                  handleAction={() => handleAction()}
                />}
            />
          </Routes>
```

和以前一样，在 Form.js 组件中接收它。

```
import * as React from 'react';
import Box from '@mui/material/Box';
import TextField from '@mui/material/TextField';
import Button from './Button';
export default function BasicTextFields({ title, setPassword, setEmail, handleAction }) {
    return (
        <div>
            <div className="heading-container">
                <h3>
                    {title} Form
                </h3>
            </div>

            <Box
                component="form"
                sx={{
                    '& > :not(style)': { m: 1, width: '25ch' },
                }}
                noValidate
                autoComplete="off"
            >
                <TextField
                    id="email"
                    label="Enter the Email"
                    variant="outlined"
                    onChange={(e) => setEmail(e.target.value)}
                />
                <TextField
                    id="password"
                    label="Enter the Password"
                    variant="outlined"
                    onChange={(e) => setPassword(e.target.value)}
                />
            </Box>

            <Button title={title} />
        </div>
    );
} 
```

然后将`handleAction`作为道具传入按钮组件。

```
<Button title={title} handleAction={handleAction}/>
```

并在 Button.js 组件中接收它。

```
import * as React from 'react';
import Button from '@mui/material/Button';

export default function BasicButtons({title, handleAction}) {
    return (
        <Button variant="contained" onClick={handleAction}>{title}</Button>
    );
} 
```

因此，当用户点击登录或注册按钮时，就会触发`handleAction`功能。

现在，回到 handleAction 函数中的 App.js 文件。我们需要确定我们将要采取的操作，是登录还是注册。因此，我们需要为登录和注册传递一个惟一的标识符，作为函数参数。

因此，假设我们正在传递“1”作为登录，传递“2”作为注册。

```
<Routes>
            <Route
              path='/login'
              element={
                <Form
                  title="Login"
                  setEmail={setEmail}
                  setPassword={setPassword}
                  handleAction={() => handleAction(1)}
                />}
            />
            <Route
              path='/register'
              element={
                <Form
                  title="Register"
                  setEmail={setEmail}
                  setPassword={setPassword}
                  handleAction={() => handleAction(2)}
                />}
            />
          </Routes>
```

而在函数中，我们来接收一下。我们还将在控制台中检查我们得到了什么输出。

```
const handleAction = (id) => {
    console.log(id)
  }
```

所以，填写表格并点击按钮。如果我们在登录屏幕上，我们将在控制台中得到‘1’，如果我们在注册屏幕上，我们将得到‘2’。

![Screenshot-2021-11-06-201657](img/ccad2e98731a7941605b6a67e62a8a64.png)

现在，我们的表单准备好了。

## 如何使用 Firebase 函数登录和注册

为了使用 Firebase 函数，我们需要将我们的`firebase-config`文件导入 App.js。

```
import { app } from './firebase-config';
```

现在，我们还需要一些其他的东西。那么，让我们导入它们:

```
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword } from 'firebase/auth'
```

我们使用`getAuth`进行认证。我们使用`signInWithEmailAndPassword`和`createUserWithEmailAndPassword`分别使用电子邮件和密码登录和注册。

在 Firebase 控制台中，转到身份验证，然后登录方法，并启用电子邮件/密码方法。

![Screenshot-2021-11-06-202403](img/8e0a2392a0d3fbf3c9a1aeeda40f1154.png)

现在，我们需要析构我们导入的`getAuth`。所以，在`handleAction`函数里，就这么做吧。

```
 const handleAction = (id) => {
    const authentication = getAuth();
  }
```

现在，让我们使用`createUserWithEmailAndPassword`函数创建一个用户。

它需要我们之前创建的三个参数:身份验证、电子邮件状态和密码状态。

```
const handleAction = (id) => {
    const authentication = getAuth();

    createUserWithEmailAndPassword(authentication, email, password)
}
```

现在，我们需要使用 id 参数创建一个检查。如果是‘1’，我们就触发登录功能，如果是‘2’，我们就触发注册功能。

我们首先创建了 Register 函数，因此我们将使用“2”作为 id。

```
const handleAction = (id) => {
    const authentication = getAuth();
    if (id === 2) {
      createUserWithEmailAndPassword(authentication, email, password)
    }
 }
```

现在，为了检查整个操作是否有效，我们将使用一个`then`声明——也就是承诺。

```
const handleAction = (id) => {
    const authentication = getAuth();
    if (id === 2) {
      createUserWithEmailAndPassword(authentication, email, password)
        .then((response) => {
          console.log(response)
      })
   }
}
```

现在填写电子邮件和密码，并点击注册按钮。

![Screenshot-2021-11-06-203439](img/e1a227a9d7efb762304ce76ec355c623.png)

你可以看到我们的控制台上有一大堆数据。如果你检查 Firebase 用户，你会看到我们创建帐户时使用的电子邮件。

![Screenshot-2021-11-06-203554](img/bd582b57243ff437a5f7a4b3c0af5fb4.png)

如果我们试图用相同的电子邮件创建一个帐户，我们会得到一个错误。

![Screenshot-2021-11-06-203723](img/bea9231d723d9e2635b53e0da6031c35.png)

## 如何在会话存储中存储令牌

现在，让我们将令牌响应存储在会话存储中。我们这样做是因为我们现在要创建私有路由。

因此，如果该令牌存在于会话存储中，我们将位于该私有路径上，比如说主页。但是如果令牌不存在，我们将被推回到注册或登录屏幕。

我选择了会话存储，因为它会在浏览器关闭时被销毁。

```
const handleAction = (id) => {
    const authentication = getAuth();
    if (id === 2) {
      createUserWithEmailAndPassword(authentication, email, password)
        .then((response) => {
          sessionStorage.setItem('Auth Token', response._tokenResponse.refreshToken)
        })
    }
  }
```

会话存储中的设置是这样的。

然后，创建一个名为 Home.js 的组件，并给它一些标题或文本。

```
import React from 'react'

export default function Home() {
    return (
        <div>
            Home Page
        </div>
    )
} 
```

然后，如果我们注册成功，我们应该在主页组件。为此，我们将使用 React 路由器中的`useNavigate`。

```
import {
  BrowserRouter as Router,
  Routes,
  Route,
  useNavigate 
} from "react-router-dom";
```

```
const navigate = useNavigate(); 
```

以及`handleAction`功能中的导航。

```
navigate('/home')
```

但在此之前，我们需要做一些改变。由于我们使用的是 React 路由器 v6，所以我们不能在当前配置中使用`useNavigate`，因为 useNavigate 也需要在 Routes 中。因此，将路线移动到 index.js 文件中。

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import {
  BrowserRouter as Router,
} from "react-router-dom";
ReactDOM.render(
  <React.StrictMode>
    <Router>
      <App />
    </Router>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals(); 
```

index.js

现在，让我们为主页设置路线。

```
<Route
            path='/home'
            element={
              <Home />}
          />
```

以下是此时的整个 App.js 文件:

```
import { useState } from 'react';
import './App.css';
import Form from './Components/Common/Form';
import Home from './Components/Home';
import {
  Routes,
  Route,
  useNavigate
} from "react-router-dom";
import { app } from './firebase-config';
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword } from 'firebase/auth'
function App() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  let navigate = useNavigate();
  const handleAction = (id) => {
    const authentication = getAuth();
    if (id === 2) {
      createUserWithEmailAndPassword(authentication, email, password)
        .then((response) => {
          navigate('/home')
          sessionStorage.setItem('Auth Token', response._tokenResponse.refreshToken)
        })
    }
  }
  return (
    <div className="App">
      <>
        <Routes>
          <Route
            path='/login'
            element={
              <Form
                title="Login"
                setEmail={setEmail}
                setPassword={setPassword}
                handleAction={() => handleAction(1)}
              />}
          />
          <Route
            path='/register'
            element={
              <Form
                title="Register"
                setEmail={setEmail}
                setPassword={setPassword}
                handleAction={() => handleAction(2)}
              />}
          />

          <Route
            path='/home'
            element={
              <Home />}
          />
        </Routes>
      </>
    </div>
  );
}

export default App; 
```

使用地址栏转到回家路线。您将看到以下内容:

![Screenshot-2021-11-06-210037](img/edd345d2382a55b8a8e0ad3cbe9f53c2.png)

现在，尝试使用不同的电子邮件和密码注册。您将被推至主页。

## 如何创建专用路由功能

但问题是，回家的路线不是私人的。所以让我们改变这一点。我们将使用会话存储中存储的令牌。

在 Home.js 组件中，创建一个`useEffect`钩子。useEffect 是一个函数，每次我们的组件加载或挂载时都会运行。

```
import React, { useEffect } from 'react'

export default function Home() {
    useEffect(() => {

    }, [])
    return (
        <div>
            Home Page
        </div>
    )
} 
```

在 useEffect 内部，我们将创建一个令牌检查。

```
import React, { useEffect } from 'react'
import { useNavigate } from 'react-router-dom'

export default function Home() {
    let navigate = useNavigate();
    useEffect(() => {
        let authToken = sessionStorage.getItem('Auth Token')

        if (authToken) {
            navigate('/home')
        }

        if (!authToken) {
            navigate('/login')
        }
    }, [])
    return (
        <div>
            Home Page
        </div>
    )
} 
```

如果令牌存在，我们将留在主页上。否则，我们将被推回到登录屏幕，除非我们找到销毁令牌的方法。

现在，我们将在 App.js 文件中创建相同的检查。

```
useEffect(() => {
    let authToken = sessionStorage.getItem('Auth Token')

    if (authToken) {
      navigate('/home')
    }
  }, [])
```

注册成功后尝试回到`/register`路线。你将被推回到`/home`页面(反之亦然，`/home`到`/register`)。

## 如何创建登录功能

现在，让我们创建登录功能。很简单。就像我们使用`createUserWithEmailAndPassword`一样，我们将使用`signInWithEmailAndPassword`函数。

```
if (id === 1) {
      signInWithEmailAndPassword(authentication, email, password)
        .then((response) => {
          navigate('/home')
          sessionStorage.setItem('Auth Token', response._tokenResponse.refreshToken)
        })
    }
```

如果 id 为“1”，将触发该功能。

让我们尝试使用注册时使用的电子邮件和密码登录。你会看到我们在主页上。

## 如何使用 Catch 块和 React Toastify 处理错误

如果我们试图用不正确的电子邮件或密码登录，我们会得到错误。

![Screenshot-2021-11-06-213046](img/9a692ee0953dc9f6b28d3d80075c9742.png)

因此，让我们使用一个`catch`块来处理这些错误。

首先，让我们处理注册方法错误。我们将使用另一个名为 React Toastify 的包来处理 toast 错误消息。使用以下命令安装它:

```
npm i react-toastify
```

然后，在 Login 和 Register 这两个函数中创建一个 catch 块。

```
.catch((error) => {
         console.log(error)
})
```

首先，尝试输入错误的电子邮件和密码。您将得到以下错误:

![Screenshot-2021-11-06-214356-1](img/d26ef70f8cdade2d8962174ec921af11.png)

如果电子邮件 ID 是正确的，但密码是错误的，您将得到以下错误:

![Screenshot-2021-11-06-214322-1](img/55ac8e9471a2e5ef2b866defced2f2ac.png)

现在让我们为我们的错误添加祝酒辞消息。

首先，我们需要导入一些东西:

```
import { ToastContainer, toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
```

然后在函数返回中添加`<ToastContainer />`。

现在，我们将检查错误代码是基于什么。如果是`auth/wrong-password`，我们会显示**的 toast 错误，请检查密码。**

或者如果是`auth/user-not-found`，我们会显示**请检查邮件**错误。

```
.catch((error) => {
          if(error.code === 'auth/wrong-password'){
            toast.error('Please check the Password');
          }
          if(error.code === 'auth/user-not-found'){
            toast.error('Please check the Email');
          }
        })
```

![Screenshot-2021-11-06-215839](img/7a34312605de8c6b695f363cc4ddb0db.png)

Error for Wrong Email

![Screenshot-2021-11-06-215922](img/253b162883d9c800e466f5509cb67e45.png)

Error for Wrong Password

我们可以对 Register 函数做同样的事情。如果我们尝试用同一个电子邮件注册两次，它会向我们抛出以下错误:

```
.catch((error) => {
          if (error.code === 'auth/email-already-in-use') {
            toast.error('Email Already in Use');
          }
        })
```

![Screenshot-2021-11-06-220420-1](img/e234993c8ff2fe8d64de60c3f5427e10.png)

## 如何创建注销功能

让我们创建让用户注销的功能。

在 Home 组件中，创建一个按钮。并创建一个名为`handleLogout`的函数。

用函数`handleLogout`为按钮分配一个“onClick”事件。所以当有人点击按钮时，这个按钮就会被触发。

在函数体中，除了从会话中销毁令牌之外，我们什么也不做，它会将我们推回到登录页面。

```
import React, { useEffect } from 'react'
import { useNavigate } from 'react-router-dom'

export default function Home() {
    const handleLogout = () => {
        sessionStorage.removeItem('Auth Token');
        navigate('/login')
    }
    let navigate = useNavigate();
    useEffect(() => {
        let authToken = sessionStorage.getItem('Auth Token')
        console.log(authToken)
        if (authToken) {
            navigate('/home')
        }

        if (!authToken) {
            navigate('/register')
        }
    }, [])
    return (
        <div>
            Home Page

            <button onClick={handleLogout}>Log out</button>
        </div>
    )
} 
```

尝试一下，确保它能工作——应该能。

## ******结论******

现在您知道了如何使用 Firebase 向 React 应用程序添加身份验证。

你可以在我的 YouTube 频道上使用 React 在同一个 [Firebase 认证和 CRUD 操作上查看我的播放列表。](https://www.youtube.com/playlist?list=PLWgH1O_994O8B_HVG2iuyqBEWPGa5Lhoj)

而这里有 [GitHub](https://github.com/nishant-666/React-Firebase-Auth-V2) 上的完整代码供你参考。

> 快乐学习。