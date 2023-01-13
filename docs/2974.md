# 如何使用 React、TypeScript、NodeJS 和 MongoDB 构建 Todo 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/>

在本教程中，我们将在两端(服务器和客户端)使用 TypeScript，用 React、NodeJS、Express 和 MongoDB 从头开始构建一个 Todo 应用程序。

所以，让我们从规划 API 开始。

*   [具有 NodeJS、Express、MongoDB 和 TypeScript 的 API](#api-with-nodejs-express-mongodb-and-typescript)
*   [设置](#setting-up)
*   [创建待办事项类型](#create-a-todo-type)
*   [创建一个待办事宜模型](#create-a-todo-model)
*   [创建 API 控制器](#create-api-controllers)
*   [获取、添加、更新和删除待办事项](#get-add-update-and-delete-todos)
*   [创建 API 路线](#create-api-routes)
*   [创建服务器](#create-a-server)
*   [带有 React 和 TypeScript 的客户端](#client-side-with-react-and-typescript)
*   [设置](#setting-up)
*   [创建待办事项类型](#create-a-todo-type-1)
*   [从 API 获取数据](#fetch-data-from-the-api)
*   [创建组件](#create-the-components)
*   [添加待办事宜表单](#add-todo-form)
*   [显示待办事宜](#display-a-todo)
*   [获取并显示数据](#fetch-and-display-data)
*   [资源](#resources)

让我们开始吧。

## 具有 NodeJS、Express、MongoDB 和 TypeScript 的 API

### 正在设置

如果你是这方面的新手，你可以从[一个实用的 TypeScript 指南](https://www.ibrahima-ndaw.com/blog/a-practical-guide-to-typescript/)或者[如何用 Node JS、Express 和 MongoDB](https://www.ibrahima-ndaw.com/blog/graphql-api-express-mongodb/) 从头构建一个 API 开始，以便从本教程中获得最大收益。否则，我们开始吧。

要创建新的 NodeJS 应用程序，您需要在终端上运行以下命令:

```
 yarn init 
```

它会问几个问题，然后初始化应用程序。您可以通过在命令中添加一个`-y`标志来跳过它。

接下来，按如下方式构建项目:

```
├── dist
├── node_modules
├── src
   ├── app.ts
   ├── controllers
   |  └── todos
   |     └── index.ts
   ├── models
   |  └── todo.ts
   ├── routes
   |  └── index.ts
   └── types
      └── todo.ts
├── nodemon.json
├── package.json
├── tsconfig.json 
```

如您所见，这个文件结构相对简单。一旦代码编译成普通 JavaScript，`dist`目录将作为输出文件夹。

我们还有一个`app.ts`文件，它是服务器的入口点。控制器、类型和路径也位于各自的文件夹名称中。

现在，我们需要配置`tsconfig.json`文件来帮助编译器遵循我们的偏好。

*   tsconfig.json

```
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "dist/js",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["src/types/*.ts", "node_modules", ".vscode"]
} 
```

在这里，我们有四个主要属性需要强调:

`outDir`:告诉编译器将编译好的代码放到`dist/js`文件夹中。

`rootDir`:通知 TypeScript 编译位于`src`文件夹中的每个`.ts`文件。

`include`:告诉编译器包含`src`目录和子目录中的文件。

`exclude`:将排除编译时数组中传递的文件或文件夹。

我们现在可以安装依赖项以在项目中启用 TypeScript。因为默认情况下，这个应用会使用 JavaScript。

在 NodeJS 应用程序中使用 TypeScript 有两种方式。无论是项目中的本地，还是我们机器中的全局。基于个人喜好，我会选择后者，但是如果你也想的话，你也可以坚持用当地的方式。

现在，让我们在终端上执行以下命令来安装 TypeScript。

```
 yarn add typescript -g 
```

这个`g`标志允许全局安装 TypeScript，这使得它可以从计算机上的任何地方访问。

接下来，让我们添加一些依赖项，以便使用 Express 和 MongoDB。

```
 yarn add express cors mongoose 
```

我们还需要将它们的类型作为开发依赖项来安装，以帮助 TypeScript 编译器理解这些包。

```
 yarn add -D @types/node @types/express @types/mongoose @types/cors 
```

现在，TypeScript 不会再对你大喊大叫了——它将使用这些类型来定义我们刚刚安装的库。

我们还需要添加其他依赖项，以便能够同时编译 TypeScript 代码和启动服务器。

```
 yarn add -D concurrently nodemon 
```

准备就绪后，我们现在可以用启动服务器所需的脚本更新`package.json`文件。

*   package.json

```
 "scripts": {
    "build": "tsc",
    "start": "concurrently \"tsc -w\" \"nodemon dist/js/app.js\""
  } 
```

`concurrently`将帮助编译 TypeScript 代码，保持对变化的观察，并且还同时启动服务器。也就是说，我们现在可以启动服务器了——然而，在这方面我们还没有创造出什么有意义的东西。因此，让我们在下一节中解决这个问题。

### 创建待办事项类型

*   types/todo.ts

```
import { Document } from "mongoose"

export interface ITodo extends Document {
  name: string
  description: string
  status: boolean
} 
```

这里，我们有一个 Todo 接口，它扩展了由`mongoose`提供的`Document`类型。我们稍后将使用它与 MongoDB 进行交互。也就是说，我们现在可以定义 Todo 模型应该是什么样子。

### 创建 Todo 模型

*   models/todo.ts

```
import { ITodo } from "./../types/todo"
import { model, Schema } from "mongoose"

const todoSchema: Schema = new Schema(
  {
    name: {
      type: String,
      required: true,
    },

    description: {
      type: String,
      required: true,
    },

    status: {
      type: Boolean,
      required: true,
    },
  },
  { timestamps: true }
)

export default model<ITodo>("Todo", todoSchema) 
```

正如您在这里看到的，我们从导入接口`ITodo`和来自`mongoose`的一些实用程序开始。后者帮助定义 Todo 模式，并在导出之前将`ITodo`作为类型传递给`model`。

这样，我们现在可以在其他文件中使用 Todo 模型来与数据库进行交互。

### 创建 API 控制器

#### 获取、添加、更新和删除待办事项

*   控制器/todo/index . ts

```
import { Response, Request } from "express"
import { ITodo } from "./../../types/todo"
import Todo from "../../models/todo"

const getTodos = async (req: Request, res: Response): Promise<void> => {
  try {
    const todos: ITodo[] = await Todo.find()
    res.status(200).json({ todos })
  } catch (error) {
    throw error
  }
} 
```

这里，我们首先需要从`express`导入一些类型，因为我想显式地输入值。如果您愿意，可以让 TypeScript 为您推断。

接下来，我们使用函数`getTodos()`来获取数据。它接收一个`req`和`res`参数并返回一个承诺。

在前面创建的`Todo`模型的帮助下，我们现在可以从 MongoDB 获取数据，并返回一个包含 todos 数组的响应。

*   控制器/todo/index . ts

```
const addTodo = async (req: Request, res: Response): Promise<void> => {
  try {
    const body = req.body as Pick<ITodo, "name" | "description" | "status">

    const todo: ITodo = new Todo({
      name: body.name,
      description: body.description,
      status: body.status,
    })

    const newTodo: ITodo = await todo.save()
    const allTodos: ITodo[] = await Todo.find()

    res
      .status(201)
      .json({ message: "Todo added", todo: newTodo, todos: allTodos })
  } catch (error) {
    throw error
  }
} 
```

如您所见，函数`addTodo()`接收包含用户输入数据的 body 对象。

接下来，我使用类型转换来避免输入错误，并限制`body`变量匹配`ITodo`，然后基于模型创建一个新的 Todo。

这样，我们现在可以将 todo 保存在 DB 中，并返回一个响应，其中包含创建的 Todo 和更新的 todos 数组。

*   控制器/todo/index . ts

```
const updateTodo = async (req: Request, res: Response): Promise<void> => {
  try {
    const {
      params: { id },
      body,
    } = req
    const updateTodo: ITodo | null = await Todo.findByIdAndUpdate(
      { _id: id },
      body
    )
    const allTodos: ITodo[] = await Todo.find()
    res.status(200).json({
      message: "Todo updated",
      todo: updateTodo,
      todos: allTodos,
    })
  } catch (error) {
    throw error
  }
} 
```

为了更新 todo，我们需要从`req`对象中提取 id 和主体，然后将它们传递给`findByIdAndUpdate()`。该实用程序将在数据库中查找 Todo 并更新它。一旦操作完成，我们现在可以将更新后的数据返回给用户。

*   控制器/todo/index . ts

```
const deleteTodo = async (req: Request, res: Response): Promise<void> => {
  try {
    const deletedTodo: ITodo | null = await Todo.findByIdAndRemove(
      req.params.id
    )
    const allTodos: ITodo[] = await Todo.find()
    res.status(200).json({
      message: "Todo deleted",
      todo: deletedTodo,
      todos: allTodos,
    })
  } catch (error) {
    throw error
  }
}

export { getTodos, addTodo, updateTodo, deleteTodo } 
```

功能`deleteTodo()`允许您从数据库中删除待办事项。这里，我们从 req 中取出 id，并将其作为参数传递给`findByIdAndRemove()`以访问相应的 Todo 并将其从 DB 中删除。

接下来，我们导出函数，以便能够在其他文件中使用它们。也就是说，我们现在可以为 API 创建一些路由，并使用这些方法来处理请求。

### 创建 API 路线

*   路线/索引. ts

```
import { Router } from "express"
import { getTodos, addTodo, updateTodo, deleteTodo } from "../controllers/todos"

const router: Router = Router()

router.get("/todos", getTodos)

router.post("/add-todo", addTodo)

router.put("/edit-todo/:id", updateTodo)

router.delete("/delete-todo/:id", deleteTodo)

export default router 
```

正如您在这里看到的，我们有四条路线来从数据库中获取、添加、更新和删除 todos。因为我们已经创建了函数，所以我们唯一要做的就是导入方法，并将它们作为参数传递来处理请求。

到目前为止，我们已经谈了很多。但是我们还是没有服务器可以启动。因此，让我们在下一节中解决这个问题。

### 创建服务器

在创建服务器之前，我们需要首先添加一些环境变量，这些变量将在`nodemon.json`文件中保存 MongoDB 凭证。

*   nodemon.json .你好

```
{
    "env": {
        "MONGO_USER": "your-username",
        "MONGO_PASSWORD": "your-password",
        "MONGO_DB": "your-db-name"
    }
} 
```

您可以通过在 [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) 上创建一个新集群来获得凭证。

*   应用程序

```
import express, { Express } from "express"
import mongoose from "mongoose"
import cors from "cors"
import todoRoutes from "./routes"

const app: Express = express()

const PORT: string | number = process.env.PORT || 4000

app.use(cors())
app.use(todoRoutes)

const uri: string = `mongodb+srv://${process.env.MONGO_USER}:${process.env.MONGO_PASSWORD}@clustertodo.raz9g.mongodb.net/${process.env.MONGO_DB}?retryWrites=true&w=majority`
const options = { useNewUrlParser: true, useUnifiedTopology: true }
mongoose.set("useFindAndModify", false)

mongoose
  .connect(uri, options)
  .then(() =>
    app.listen(PORT, () =>
      console.log(`Server running on http://localhost:${PORT}`)
    )
  )
  .catch(error => {
    throw error
  }) 
```

这里，我们从导入`express`库开始，该库允许我们访问帮助处理 Todos 路由的`use()`方法。

接下来，我们使用`mongoose`包连接到 MongoDB，方法是将保存在`nodemon.json`文件中的凭证附加到 URL 上。

也就是说，现在如果我们成功连接到 MongoDB，服务器就会启动。如果合适，将抛出一个错误。

我们现在已经用 Node、Express、TypeScript 和 MongoDB 完成了 API 的构建。现在让我们开始用 React 和 TypeScript 构建客户端应用程序。

![excited](img/0b567a447e4a3a13aae39330a28effb9.png)

## 带有 React 和 TypeScript 的客户端

### 安装

要创建一个新的 React 应用程序，我将使用 create-React-app——如果你愿意，也可以使用其他方法。

所以，让我们在终端中运行以下命令:

```
 npx create-react-app my-app --template typescript 
```

接下来，安装 Axios 库，以便能够获取远程数据。

```
 yarn add axios 
```

安装完成后，让我们按如下方式构建我们的项目:

```
├── node_modules
├── public
├── src
|  ├── API.ts
|  ├── App.test.tsx
|  ├── App.tsx
|  ├── components
|  |  ├── AddTodo.tsx
|  |  └── TodoItem.tsx
|  ├── index.css
|  ├── index.tsx
|  ├── react-app-env.d.ts
|  ├── setupTests.ts
|  └── type.d.ts
├── tsconfig.json
├── package.json
└── yarn.lock 
```

这里，我们有一个相对简单的文件结构。要注意的主要事情是`src/type.d.ts`将保存类型。因为我将在几乎每个文件中使用它们，所以我添加了扩展名`.d.ts`来使这些类型全局可用。现在我们不再需要导入它们了。

### 创建待办事项类型

*   src/type.d.ts

```
interface ITodo {
  _id: string
  name: string
  description: string
  status: boolean
  createdAt?: string
  updatedAt?: string
}

interface TodoProps {
  todo: ITodo
}

type ApiDataType = {
  message: string
  status: string
  todos: ITodo[]
  todo?: ITodo
} 
```

这里，`ITodo`接口需要反映来自 API 的数据的形状。由于这里没有`mongoose`，我们需要添加额外的属性来匹配 API 上定义的类型。

接下来，我们对`TodoProps`使用相同的接口，它是 props 的类型注释，将由负责呈现数据的组件接收。

我们现在已经定义了我们的类型——现在让我们开始从 API 获取数据。

### 从 API 获取数据

*   src/API.ts

```
import axios, { AxiosResponse } from "axios"

const baseUrl: string = "http://localhost:4000"

export const getTodos = async (): Promise<AxiosResponse<ApiDataType>> => {
  try {
    const todos: AxiosResponse<ApiDataType> = await axios.get(
      baseUrl + "/todos"
    )
    return todos
  } catch (error) {
    throw new Error(error)
  }
} 
```

如您所见，我们需要导入`axios`来从 API 请求数据。接下来，我们使用函数`getTodos()`从服务器获取数据。它将返回一个类型为`AxiosResponse`的承诺，其中包含需要匹配类型`ApiDataType`的 Todos。

*   src/API.ts

```
export const addTodo = async (
  formData: ITodo
): Promise<AxiosResponse<ApiDataType>> => {
  try {
    const todo: Omit<ITodo, "_id"> = {
      name: formData.name,
      description: formData.description,
      status: false,
    }
    const saveTodo: AxiosResponse<ApiDataType> = await axios.post(
      baseUrl + "/add-todo",
      todo
    )
    return saveTodo
  } catch (error) {
    throw new Error(error)
  }
} 
```

该函数接收用户输入的数据作为参数，并返回一个承诺。这里，我们需要省略`_id`属性，因为 MongoDB 将动态创建它。

*   src/API.ts

```
export const updateTodo = async (
  todo: ITodo
): Promise<AxiosResponse<ApiDataType>> => {
  try {
    const todoUpdate: Pick<ITodo, "status"> = {
      status: true,
    }
    const updatedTodo: AxiosResponse<ApiDataType> = await axios.put(
      `${baseUrl}/edit-todo/${todo._id}`,
      todoUpdate
    )
    return updatedTodo
  } catch (error) {
    throw new Error(error)
  }
} 
```

要更新 Todo，我们必须传入更新的数据和对象的`_id`。这里，我们需要更改 Todo 的`status`,这就是为什么我在向服务器发送请求之前只选择我们需要的属性。

*   src/API.ts

```
export const deleteTodo = async (
  _id: string
): Promise<AxiosResponse<ApiDataType>> => {
  try {
    const deletedTodo: AxiosResponse<ApiDataType> = await axios.delete(
      `${baseUrl}/delete-todo/${_id}`
    )
    return deletedTodo
  } catch (error) {
    throw new Error(error)
  }
} 
```

这里，我们还有一个函数，它接收属性`_id`作为参数，并返回一个承诺。

准备就绪后，我们现在可以转到`components`文件夹，向它的文件中添加一些有意义的代码。

### 创建组件

#### 添加待办事项表单

*   components/AddTodo.tsx

```
import React from "react"

type Props = TodoProps & {
  updateTodo: (todo: ITodo) => void
  deleteTodo: (_id: string) => void
}

const Todo: React.FC<Props> = ({ todo, updateTodo, deleteTodo }) => {
  const checkTodo: string = todo.status ? `line-through` : ""
  return (
    <div className="Card">
      <div className="Card--text">
        <h1 className={checkTodo}>{todo.name}</h1>
        <span className={checkTodo}>{todo.description}</span>
      </div>
      <div className="Card--button">
        <button
          onClick={() => updateTodo(todo)}
          className={todo.status ? `hide-button` : "Card--button__done"}
        >
          Complete
        </button>
        <button
          onClick={() => deleteTodo(todo._id)}
          className="Card--button__delete"
        >
          Delete
        </button>
      </div>
    </div>
  )
}

export default Todo 
```

如您所见，这里我们有一个类型为`React.FC`的功能组件(FC 代表功能组件)。它接收方法`saveTodo()`作为道具，该方法允许我们将数据保存到 DB。

接下来，我们有一个需要匹配`ITodo`类型的`formData`状态，以满足编译器。这就是为什么我们把它传给了`useState`钩子。我们还需要添加一个替代类型(`{}`)，因为初始状态将是一个空对象。

这样，我们现在可以向前移动并显示获取的数据。

### **显示待办事宜**

*   components/TodoItem.tsx

```
import React from "react"

type Props = TodoProps & {
  updateTodo: (todo: ITodo) => void
  deleteTodo: (_id: string) => void
}

const Todo: React.FC<Props> = ({ todo, updateTodo, deleteTodo }) => {
  const checkTodo: string = todo.status ? `line-through` : ""
  return (
    <div className="Card">
      <div className="Card--text">
        <h1 className={checkTodo}>{todo.name}</h1>
        <span className={checkTodo}>{todo.description}</span>
      </div>
      <div className="Card--button">
        <button
          onClick={() => updateTodo(todo)}
          className={todo.status ? `hide-button` : "Card--button__done"}
        >
          Complete
        </button>
        <button
          onClick={() => deleteTodo(todo._id)}
          className="Card--button__delete"
        >
          Delete
        </button>
      </div>
    </div>
  )
}

export default Todo 
```

这里，我们需要扩展`TodoProps`类型并添加函数`updateTodo`和`deleteTodo`来适当地处理组件收到的道具。

现在，一旦 Todo 对象传入，我们将能够显示它并添加更新或删除 Todo 所需的函数。

太好了！我们现在可以转到`App.tsx`文件，添加拼图的最后一块。

### 获取和显示数据

*   App.tsx

```
import React, { useEffect, useState } from 'react'
import TodoItem from './components/TodoItem'
import AddTodo from './components/AddTodo'
import { getTodos, addTodo, updateTodo, deleteTodo } from './API'

const App: React.FC = () => {
  const [todos, setTodos] = useState<ITodo[]>([])

  useEffect(() => {
    fetchTodos()
  }, [])

  const fetchTodos = (): void => {
    getTodos()
    .then(({ data: { todos } }: ITodo[] | any) => setTodos(todos))
    .catch((err: Error) => console.log(err))
  } 
```

这里，我们首先需要导入保存在`API.ts`上的组件和实用函数。接下来，我们向`useState`传递一个`ITodo`类型的数组，并用一个空数组初始化它。

方法`getTodos()`返回一个承诺——因此，我们可以访问`then`函数，用获取的数据更新状态，或者在出现错误时抛出一个错误。

有了这些，我们现在可以在组件成功安装后调用函数`fetchTodos()`。

*   App.tsx

```
const handleSaveTodo = (e: React.FormEvent, formData: ITodo): void => {
  e.preventDefault()
  addTodo(formData)
    .then(({ status, data }) => {
      if (status !== 201) {
        throw new Error("Error! Todo not saved")
      }
      setTodos(data.todos)
    })
    .catch(err => console.log(err))
} 
```

一旦提交了表单，我们就使用`addTodo()`将请求发送到服务器，然后如果 Todo 已经成功保存，我们就更新数据，否则就会抛出一个错误。

*   App.tsx

```
const handleUpdateTodo = (todo: ITodo): void => {
  updateTodo(todo)
    .then(({ status, data }) => {
      if (status !== 200) {
        throw new Error("Error! Todo not updated")
      }
      setTodos(data.todos)
    })
    .catch(err => console.log(err))
}

const handleDeleteTodo = (_id: string): void => {
  deleteTodo(_id)
    .then(({ status, data }) => {
      if (status !== 200) {
        throw new Error("Error! Todo not deleted")
      }
      setTodos(data.todos)
    })
    .catch(err => console.log(err))
} 
```

更新或删除 Todo 的函数非常相似。它们都接收一个参数，发送请求，然后得到一个响应。然后，它们检查请求是否成功，并相应地进行处理。

*   App.tsx

```
 return (
    <main className='App'>
      <h1>My Todos</h1>
      <AddTodo saveTodo={handleSaveTodo} />
      {todos.map((todo: ITodo) => (
        <TodoItem
          key={todo._id}
          updateTodo={handleUpdateTodo}
          deleteTodo={handleDeleteTodo}
          todo={todo}
        />
      ))}
    </main>
  )
}

export default App 
```

这里，我们遍历`todos`数组，然后将预期的数据传递给`TodoItem`。

现在，如果您浏览包含服务器端应用程序的文件夹(并在终端中执行以下命令):

```
yarn start 
```

在客户端应用程序上:

```
yarn start 
```

您应该会看到我们的 Todo 应用程序按预期工作。

![Image of working todo app](img/77a40a3094018611c12764a1363d4d1b.png)

太好了！至此，我们已经使用 TypeScript、React、NodeJs、Express 和 MongoDB 完成了 Todo 应用程序的构建。

你可以在这里找到[的源代码。](https://github.com/ibrahima92/fullstack-typescript-mern-todo)

你可以在[我的博客](https://www.ibrahima-ndaw.com)上找到类似这样的精彩内容，或者在 Twitter 上关注我[以获得通知。](https://twitter.com/ibrahima92_)

感谢阅读。

## 资源

[反应打字稿备忘单](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet)

[高级打字稿类型 cheatsheet(附示例)](https://www.ibrahima-ndaw.com/blog/advanced-typescript-cheat-sheet/)

[打字稿备忘单](https://github.com/typescript-cheatsheets)