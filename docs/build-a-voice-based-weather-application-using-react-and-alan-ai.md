# 如何使用 React、Firebase 和 Alan AI 构建基于语音的 Todo 应用程序

> 原文：<https://www.freecodecamp.org/news/build-a-voice-based-weather-application-using-react-and-alan-ai/>

React Todo 应用程序通常是非常基础的——事实上，如果你是一个 React 初学者，并且想继续提高你的技能，它们是一个很好的练习。

但是你有没有构建过一个用户可以使用语音命令添加待办事项的应用程序呢？这使它变得更加复杂和令人兴奋。

这就是我们在本教程中要做的。为了构建这个基于语音的 Todo 应用程序，我们将使用三个主要工具:

1.  反应–用于用户界面。
2.  firebase–用于数据库。
3.  Alan AI–用于执行语音命令。

那么，我们开始吧。

## 如何使用 React 创建 Todo 应用程序 UI

让我们首先创建 React 应用程序。只需键入以下命令:

```
npx create-react-app react-todo-alan-firebase
```

它将像这样初始化并创建我们的 React 应用程序。然后，我们将导航到该文件夹，并使用 npm start 启动应用程序。

现在让我们创建一个名为 components 的文件夹。它将包含我们的主要组件，名为 Todo.js。

```
import React from 'react'

export default function Todo() {
    return (
        <div>

        </div>
    )
} 
```

Todo.js

给应用程序一个标题(或名称)，比如基于语音的 Todo 应用程序，或者你选择的任何东西。

```
import React from 'react'

export default function Todo() {
    return (
        <div>
            <h2>Voice-based Todo Application</h2>
        </div>
    )
} 
```

然后，将该组件导入到 App.js 文件中。

```
import './App.css';
import Todo from './components/Todo';

function App() {
  return (
    <div>
      <Todo />
    </div>
  );
}

export default App; 
```

App.js

您将在输出屏幕上看到标题。

让我们把标题放在中间。所以，在 Todo.js 组件中给`h2` 一个 header 类名。

```
import React from 'react'

export default function Todo() {
    return (
        <div>
            <h2 className="header">Voice-based Todo Application</h2>
        </div>
    )
} 
```

然后我们将在 App.css 文件中添加一些样式，使标题居中。

```
.header{
  text-align: center;
}
```

![Screenshot-2021-11-15-215217](img/b900bb47875c39cddec2102bc5ba8c7c.png)

您将在屏幕上看到上面的输出，标题居中。

现在，让我们创建一个包含待办事项的卡片。

```
import React from 'react'

export default function Todo() {
    return (
        <div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">

            </div>
        </div>
    )
} 
```

创建一个 div 并使`className`成为`todo-card`。你会看到主父 div 有`todo-main`的`className`。这是因为我们需要中心的一切。

```
.todo-main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.header {
  text-align: center;
}

.todo-card {
  border: 1px dashed #1f133d;
  height: 50vh;
  width: 50vh;
  border-radius: 20px;
} 
```

并将上述样式添加到 App.css 中。它现在看起来像这样:

![Screenshot-2021-11-15-220125](img/be8cdedc8057854699c85b3f0f2480ab.png)

让我们现在添加列表。

```
import React from 'react'

export default function Todo() {
    return (
        <div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">
                <div className="todo-list">
                    <h3>
                        Wash the Clothes
                    </h3>
                </div>
                <div className="todo-list">
                    <h3>
                        Cook the Dinner
                    </h3>
                </div>
                <div className="todo-list">
                    <h3>
                        Code some software
                    </h3>
                </div>
            </div>
        </div>
    )
} 
```

因此，我创建了一个 div，它包含了`h3`标签中的项目。这些文本目前是静态的，但是我们很快也会从 Firebase 数据库中创建动态文本。

以下是我们最新的款式:

```
.todo-main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.header {
  text-align: center;
}

.todo-card {
  border: 1px dashed #1f133d;
  height: 50vh;
  width: 50vh;
  border-radius: 20px;
}

.todo-list{
  text-align: center;
}
```

![Screenshot-2021-11-15-220710](img/187365d92e688edf19d9fdf5f5d32ef1.png)

这就是现在的输出，我们的列表中有三个条目。

现在，让我们添加一个关闭图标，它将在我们完成后删除每个项目。

为了添加图标，我们需要一个图标包。所以，让我们用这个命令安装 React 图标:

```
npm install react-icons --save
```

安装完成后，从左侧工具条中选择任意图标包。

![Screenshot-2021-11-15-220805](img/a625f3cdc6d730b8273d7b16a6cf82bf.png)

我正在使用羽毛图标，所以我将导入它。

首先，让我们使用以下命令导入这个包:

```
import { FiX } from "react-icons/fi";
```

然后，在 h3 标签后调用它。

```
import React from 'react'
import { FiX } from "react-icons/fi";
export default function Todo() {
    return (
        <div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">
                <div className="todo-list">
                    <h3>
                        Wash the Clothes
                    </h3>
                    <FiX />
                </div>
                <div className="todo-list">
                    <h3>
                        Cook the Dinner
                    </h3>
                    <FiX />
                </div>
                <div className="todo-list">
                    <h3>
                        Code some software
                    </h3>
                    <FiX />
                </div>
            </div>
        </div>
    )
} 
```

![Screenshot-2021-11-15-221145](img/3ff892ef113cc3c75a611c79792ede39.png)

以上，这就是我们的应用程序现在的样子。但是我们希望关闭图标和待办事项在同一行。

给图标一个类名`close-icon`。

```
 <FiX className="close-icon" />
```

在 App.css 中，添加以下样式:

```
.todo-list {
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-icon {
  margin-left: 10px;
} 
```

到目前为止，我们的 Todo.js 组件将具有以下最终代码:

```
import React from 'react'
import { FiX } from "react-icons/fi";
export default function Todo() {
    return (
        <div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">
                <div className="todo-list">
                    <h3>
                        Wash the Clothes
                    </h3>
                    <FiX className="close-icon" />
                </div>
                <div className="todo-list">
                    <h3>
                        Cook the Dinner
                    </h3>
                    <FiX className="close-icon" />
                </div>
                <div className="todo-list">
                    <h3>
                        Code some software
                    </h3>
                    <FiX className="close-icon" />
                </div>
            </div>
        </div>
    )
} 
```

我们的 App.css 是这样的:

```
.todo-main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.header {
  text-align: center;
}

.todo-card {
  border: 1px dashed #1f133d;
  height: 50vh;
  width: 50vh;
  border-radius: 20px;
}

.todo-list {
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-icon {
  margin-left: 10px;
} 
```

这是我们的用户界面的样子:

![Screenshot-2021-11-15-221826](img/57cf562407d585d7260f5c857f0a7fdb.png)

## 如何将艾伦·艾加入我们的 React 项目

前往[https://alan.app/](https://alan.app/)创建您的账户。

登录后，您可以创建项目。只需点击加号按钮。

![Screenshot-2021-11-15-222106](img/8f71945f022d42be6f90365572eb5f36.png)

但在使用之前，我们需要先安装艾伦 AI 包。因此，前往[https://alan.app/docs/client-api/web/react](https://alan.app/docs/client-api/web/react)获取 React 文档。

使用以下命令安装 Alan Al:

```
npm install @alan-ai/alan-sdk-web --save
```

现在，让我们在主 App.js 文件中导入 Alan。

```
import alanBtn from "@alan-ai/alan-sdk-web";
```

然后，我们需要创建一个 useEffect 挂钩。每当我们的组件被安装或加载时，它将启动我们的 Alan 服务。

```
useEffect(() => {
    alanBtn({
        key: 'YOUR_KEY_FROM_ALAN_STUDIO_HERE',
        onCommand: (commandData) => {
            if (commandData.command === 'go:back') {
                // Call the client code that will react to the received command
            }
        }
    });
}, []);
```

这个`alanBtn`需要一个我们需要得到的密钥。因此，在您在 Alan 中创建的项目中，您应该会在顶栏中看到一个“Integrations”按钮。单击该按钮。

![Screenshot-2021-11-15-222645](img/838a8865513c64e811c23dcaba864b06.png)

在那里你会拿到你的钥匙。

将该密钥粘贴到 React 应用程序的`alanBtn`中，如下所示:

```
import './App.css';
import Todo from './components/Todo';
import alanBtn from "@alan-ai/alan-sdk-web";
import { useEffect } from 'react'
function App() {
  useEffect(() => {
    alanBtn({
      key: '86e866fbe49666abd385ee5c9f9cbf5c2e956eca572e1d8b807a3e2338fdd0dc/stage',
      onCommand: (commandData) => {

      }
    });
  }, []);
  return (
    <div>
      <Todo />
    </div>
  );
}

export default App; 
```

现在，检查输出，您会看到一个麦克风按钮。

![Screenshot-2021-11-15-222913](img/30d88a49b975790ee0445f8190ea40aa.png)

现在，我们需要在 Alan 应用程序中创建一个意图。它会以添加命令开始，像添加洗衣服，添加写一些代码等等。所以，让我们来写代码:

```
intent('Add $(item* (.*))', (p) => {
    if(p.item.value){
        p.play({ command: 'todoApp', data: p.item.value });
        p.play(`${p.item.value} added`);
    }
    else{
        p.play(`Cannot add Empty Item`);
    }
})
```

它还会将项目返回给我们，我们可以在 React 应用程序中看到这一点。这里，我们也有一个检查来阻止我们添加任何空的条目。如果我们尝试，它会回复“不能添加空项目”。

现在，我们希望将口语项接收回我们的 React 应用程序。

```
import './App.css';
import Todo from './components/Todo';
import alanBtn from "@alan-ai/alan-sdk-web";
import { useEffect } from 'react'
function App() {
  useEffect(() => {
    alanBtn({
      key: '86e866fbe49666abd385ee5c9f9cbf5c2e956eca572e1d8b807a3e2338fdd0dc/stage',
      onCommand: (commandData) => {
        console.log(commandData)
      }
    });
  }, []);
  return (
    <div>
      <Todo />
    </div>
  );
}

export default App; 
```

只需 console.log 命令数据，您将获得以下结果。别忘了点一下麦克风按钮，说点什么。你会在控制台上看到你说的话。

![Screenshot-2021-11-15-223428](img/44eb669514ec98a43675c5e1236ae171.png)

好极了，我们的艾伦·艾已经准备好了。

## 如何使用 Firebase 向 Firestore 数据库发送数据？

我们现在将把这些数据发送给 Firebase。

但首先，我们需要安装它。前往[https://firebase.google.com/](https://firebase.google.com/)并在那里创建一个项目。

要安装 Firebase，只需输入`npm install firebase`。

然后，在项目中创建一个应用程序，如下所示:

![Screenshot-2021-11-15-223909](img/e73b3a96e553958e452f629d43429598.png)

它会给我们一些配置数据。只需在 src 文件夹中创建一个文件，将其命名为`firebase-config.js`，并添加那些配置数据。

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";

// Your web app's Firebase configuration
const firebaseConfig = {
    apiKey: "AIzaSyCP8qL8z9BorGF3NZJsGb4vSaWHYyCVfc8",
    authDomain: "todo-firebase-alan.firebaseapp.com",
    projectId: "todo-firebase-alan",
    storageBucket: "todo-firebase-alan.appspot.com",
    messagingSenderId: "892581913000",
    appId: "1:892581913000:web:dbe08ac753c3adaab87d9d"
};

// Initialize Firebase
export const app = initializeApp(firebaseConfig);
```

不要忘记导出 const 应用程序。

接下来，我们还需要访问 Firestore。因此，让我们将它导入到 firebase-config.js 文件中。

```
import { getFirestore } from 'firebase/firestore'
```

```
export const database = getFirestore(app);
```

并在底部导出它。

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getFirestore } from 'firebase/firestore'

// Your web app's Firebase configuration
const firebaseConfig = {
    apiKey: "AIzaSyCP8qL8z9BorGF3NZJsGb4vSaWHYyCVfc8",
    authDomain: "todo-firebase-alan.firebaseapp.com",
    projectId: "todo-firebase-alan",
    storageBucket: "todo-firebase-alan.appspot.com",
    messagingSenderId: "892581913000",
    appId: "1:892581913000:web:dbe08ac753c3adaab87d9d"
};

// Initialize Firebase
export const app = initializeApp(firebaseConfig);
export const database = getFirestore(app);
```

这是整个 firebase-config 代码。

现在，在 App.js 中，我们需要导入这个 App 和数据库。

```
import { app, database } from './firebase-config';
```

接下来，我们需要创建一个到 Firebase Firestore 的连接。为此，我们需要 Firebase Firestore 的收藏属性。此外，我们将导入 addDoc，用于向 Firestore 添加数据。

```
import { collection, addDoc } from 'firebase/firestore';
```

现在，让我们创建到数据库的连接。

创建一个变量，并放入我们从 firebase-config 文件导入的数据库，以及我们希望为我们的集合指定的名称。因为我们希望集合是 todo-list，所以我们可以添加以下内容:

```
const databaseRef = collection(database, 'todo-list');
```

要添加数据，我们需要 addDoc 属性。

addDoc 属性将接受两个参数。第一个是我们创建的连接 databaseRef。第二个是我们要添加的数据，作为一个对象。

将 addDoc 放入 useEffect 挂钩，如下所示:

```
useEffect(() => {
    alanBtn({
      key: '86e866fbe49666abd385ee5c9f9cbf5c2e956eca572e1d8b807a3e2338fdd0dc/stage',
      onCommand: (commandData) => {
        addDoc(databaseRef, { item: commandData.data })
      }
    });
  }, []);
```

目前，我们的 Firestore 是空的。

![Screenshot-2021-11-15-225805](img/91399564fcd2bfb1e55ed6c1a3e19ca3.png)

现在，让我们试试这个。对着麦克风说点什么，以 add command 开头，它就会出现在 Firebase Firestore 中。

![Screenshot-2021-11-15-225922](img/6601cb394eb93604c7b2af3854de3a81.png)

你看，我们所说的现在被添加到 Firebase 中。

现在，让我们尝试读取并显示这些数据。

将`databaseRef`作为道具传递给 Todo 组件。

```
<Todo databaseRef={databaseRef}/>
```

然后在 Todo 组件中接收它。

```
export default function Todo({databaseRef})
```

在 Todo.js 组件中创建一个 useEffect 钩子，并在 useEffect 钩子内部创建函数`getData`。

```
useEffect(() => {
        const getData = async () => {

        }
        getData()
    }, [])
```

我们将使用 getDocs 属性从 Firebase Firestore 中读取数据。我们还需要连接 databaseRef，它是我们之前作为一个属性传递的。

```
let data = await getDocs(databaseRef);
```

```
const getData = async () => {
  let data = await getDocs(databaseRef);
  console.log(data.docs.map((item) => ({ ...item.data(), id: item.id })));
}
```

我们映射传入的数据，使其更具可读性。我们还添加了项目的唯一 id，应用程序稍后将使用它来删除该项。

现在让我们检查一下我们的控制台:

![Screenshot-2021-11-15-231500](img/ecd91075e05294b38ce920f8432ee362.png)

你看，我们正在得到它。

现在，让我们将这些数据存储在一个状态中，以便在 React 应用程序中显示。

导入 useState 挂钩，然后创建一个数组状态，如下所示:

```
 const [todoList, setTodoList] = useState([]);
```

并使用`setTodoList`功能设置数据:

```
setTodoList(data.docs.map((item) => ({ ...item.data(), id: item.id })));
```

现在，让我们绘制 todoList 数组。

```
<div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">
                {todoList.map((todo) => {
                    return (
                        <div className="todo-list">
                            <h3>
                                {todo.item}
                            </h3>
                            <FiX className="close-icon" />
                        </div>
                    )
                })}
            </div>
        </div>
```

我们将在 React UI 中看到我们的数据，它看起来像这样:

![Screenshot-2021-11-15-231945](img/788a9a45b0f908439b8e4965847f772b.png)

现在，让我们更新它，使每个 Todo 都以大写字母开头。

给 h3 作为`todo-items`的`className`。

```
<h3 className="todo-item">
 {todo.item}
</h3>
```

在 App.css 中，添加以下样式:

```
.todo-item{
  text-transform: capitalize;
}
```

你会看到每个 Todo 现在都是大写的。

![Screenshot-2021-11-15-232149](img/fd7a337ebc60027682082eb855349393.png)

现在，如果我们通过语音命令添加任何东西，我们在 React 中的列表应该会更新。因此，让我们将 useEffect 配置为在每次我们对应用程序说话时运行。

在 App.js 文件中，创建一个状态。它将是一个布尔值，初始状态为假。

```
const [update, setUpdate] = useState(false)
```

当我们说了什么，或者 App.js 中的 useEffect 运行时，这个状态就会变成 true。

```
useEffect(() => {
    alanBtn({
      key: '86e866fbe49666abd385ee5c9f9cbf5c2e956eca572e1d8b807a3e2338fdd0dc/stage',
      onCommand: (commandData) => {
        addDoc(databaseRef, { item: commandData.data })
        .then(() => {
          setUpdate(true);
        })
      }
    });
  }, []);
```

然后，我们将在 Todo.js 中传递更新状态和更新状态的函数。

```
<Todo databaseRef={databaseRef} update={update} setUpdate={setUpdate}/>
```

并在 Todo 组件中接收这两个。

```
export default function Todo({ databaseRef, update, setUpdate })
```

然后，在 Todo.js 的 useEffect 中，一旦它从 Firebase Firestore 获取了我们的数据，就使用 setUpdate 函数将 update 设置为 false。然后将更新状态放入依赖数组中。

```
useEffect(() => {
        const getData = async () => {
            let data = await getDocs(databaseRef);
            setTodoList(data.docs.map((item) => ({ ...item.data(), id: item.id })));
        }
        getData()
        setUpdate(false)
    }, [update])
```

这可能有点令人困惑，但让我解释一下。

当我们说话时，更新状态从 false 变为 true。然后，当从 Firestore 获取数据完成后，它将从 true 更改为 false。这样，状态就在不断变化。因此，每次更新状态改变时，useEffect 都会更新。

让我们现在试一试。说点什么，列表就会动态更新。

![Screenshot-2021-11-15-233617](img/d93da0c4a184542db7cce05986db9bc7.png)

现在，让我们添加删除功能，从 Firebase Firestore 和 React 应用程序中删除项目。

创建一个名为`deleteItems`的函数。

```
const deleteItems = () => {

}
```

并将该函数绑定到`close`图标，如下所示:

```
<FiX className="close-icon" onClick={() => deleteItems()}/>
```

当我们单击一个特定的关闭图标时，我们需要将该项目的 id 传递给函数，该函数将用于删除该项。

```
<FiX className="close-icon" onClick={() => deleteItems(todo.id)}/>
```

并在函数中，接收它。

让我们试着登录我们的 id:

```
const deleteItems = (id) => {
  console.log(id)
}
```

![Screenshot-2021-11-15-234631](img/9002eb95a24f8a13e63e7d33979454b7.png)

我们将在控制台中获得那个特定的 id。

现在，在删除任何待办事项之前，我们需要指定要删除的待办事项。因此，我们将使用这个 id 创建一个引用。我们将使用 Firestore 的 doc 属性。

因此，首先使用以下命令从 Firestore 导入文档:

```
import { getDocs, doc } from 'firebase/firestore';
```

然后，在函数 1deleteItems1 中，添加以下代码:

```
const data = doc(database, 'todo-list', id)
```

这个文档有三个参数——数据库、集合名和 id。这三样东西我们都有。

数据库已从 firebase-config 导入。todo-list 是 Firestore 集合的名称。我们从关闭按钮点击中得到的 id。

要删除一个项目，我们需要 Firestore 中另一个名为 deleteDoc 的属性。

```
import { getDocs, doc, deleteDoc } from 'firebase/firestore';
```

然后，只需添加以下内容:

```
const deleteItems = (id) => {
   const data = doc(database, 'todo-list', id);
   deleteDoc(data)
}
```

立即尝试——单击关闭图标，然后检查 Firestore。该项目将被删除。

但是我们遇到了在添加和读取操作中遇到的同样的问题:在我们删除一个项目后，React 应用程序没有得到更新。

所以，首先要做的是将 getData 函数移到 useEffect 钩子之外。不要担心，它仍然会工作。

```
const getData = async () => {
   let data = await getDocs(databaseRef);
   setTodoList(data.docs.map((item) => ({ ...item.data(), id: item.id })));
}

useEffect(() => {   
   getData()
   setUpdate(false)
}, [update])
```

在 deleteDoc 函数中，我们需要再次调用 getData 函数，以便在用户删除一个项目后获取更新的数据。

```
const deleteItems = (id) => {
    const data = doc(database, 'todo-list', id);
    deleteDoc(data)
    .then(() => {
       getData()
    })
}
```

以下是完整的 Todo.js 代码:

```
import React, { useEffect, useState } from 'react'
import { FiX } from "react-icons/fi";
import { database } from '../firebase-config';
import { getDocs, doc, deleteDoc } from 'firebase/firestore';
export default function Todo({ databaseRef, update, setUpdate }) {
    const [todoList, setTodoList] = useState([]);
    const getData = async () => {
        let data = await getDocs(databaseRef);
        setTodoList(data.docs.map((item) => ({ ...item.data(), id: item.id })));
    }
    useEffect(() => {
        getData()
        setUpdate(false)
    }, [update])

    const deleteItems = (id) => {
        const data = doc(database, 'todo-list', id);
        deleteDoc(data)
            .then(() => {
                getData()
            })
    }

    return (
        <div className="todo-main">
            <h2 className="header">Voice-based Todo Application</h2>

            <div className="todo-card">
                {todoList.map((todo) => {
                    return (
                        <div className="todo-list">
                            <h3 className="todo-item">
                                {todo.item}
                            </h3>
                            <FiX className="close-icon" onClick={() => deleteItems(todo.id)} />
                        </div>
                    )
                })}
            </div>
        </div>
    )
} 
```

以及 App.js 代码:

```
import './App.css';
import Todo from './components/Todo';
import alanBtn from "@alan-ai/alan-sdk-web";
import { useEffect, useState } from 'react';
import { app, database } from './firebase-config';
import { addDoc, collection } from '@firebase/firestore';
function App() {
  const databaseRef = collection(database, 'todo-list');
  const [update, setUpdate] = useState(false)
  useEffect(() => {
    alanBtn({
      key: '86e866fbe49666abd385ee5c9f9cbf5c2e956eca572e1d8b807a3e2338fdd0dc/stage',
      onCommand: (commandData) => {
        addDoc(databaseRef, { item: commandData.data })
          .then(() => {
            setUpdate(true);
          })
      }
    });
  }, []);
  return (
    <div>
      <Todo databaseRef={databaseRef} update={update} setUpdate={setUpdate} />
    </div>
  );
}

export default App; 
```

现在，我们可以使用语音命令添加项目，它将存储在我们的 Firebase 数据库中。它也会出现在我们的 React 应用程序中。

还有最后一件事要做。在我们的 App.css 文件中，我们需要将卡片的高度设置为 auto，以防止文本溢出。

```
.todo-card {
  border: 1px dashed #1f133d;
  height: auto;
  width: 50vh;
  border-radius: 20px;
}
```

而这只是一个简单的 UI 设计。如果你愿意，你可以用你自己的设计。继续，构建这个令人敬畏的应用程序。

感谢阅读！

你可以在我的 YouTube 频道[上查看我关于同一主题的视频，让我们使用 React、Firebase 和 Alan AI](https://youtu.be/BzHbI2AAXGs) 构建一个基于语音的 Todo 应用程序。

> 快乐学习。