# React 钩子教程——use state，useEffect，以及如何创建自定义钩子

> 原文：<https://www.freecodecamp.org/news/introduction-to-react-hooks/>

钩子最早是在 React 16.8 中引入的。它们很棒，因为它们让你可以使用更多的 React 特性——比如管理组件的状态，或者在状态发生某些变化时执行 after effect，而无需编写类。

在本文中，您将学习如何在 React 中使用钩子，以及如何创建自己的定制钩子。请记住，您可以只对功能组件使用钩子。

## 什么是使用状态挂钩？

应用程序的状态在某个时候肯定会改变。这可能是变量、对象或组件中存在的任何数据类型的值。

为了能够在 DOM 中反映这些变化，我们必须使用一个名为`useState`的 React 钩子。看起来是这样的:

```
import { useState } from "react";

function App() {
  const [name, setName] = useState("Ihechikara");
  const changeName = () => {
    setName("Chikara");
  };

  return (
    <div>
      <p>My name is {name}</p>
      <button onClick={changeName}> Click me </button>
    </div>
  );
}

export default App; 
```

让我们更仔细地看看上面的代码中发生了什么。

```
import { useState } from "react";
```

为了能够使用这个钩子，你必须从 React 导入 **`useState`** 钩子。我们正在使用一个名为`app`的功能组件。

```
const [name, setName] = useState("Ihechikara");
```

之后，你必须创建你的状态，并给它一个初始值(或初始状态)，即“Ihechikara”。状态变量称为`name`，`setName`是更新其值的函数。

很好地理解 ES6 的一些特性将有助于您掌握 React 的基本功能。上面，我们使用了析构赋值来给`useState("Ihechikara")`中的状态分配一个初始名称值。

```
return (
    <div>
      <p>My name is {name}</p>
      <button onClick={changeName}> Click me </button>
    </div>
  );
} 
```

接下来，DOM 有一个包含 name 变量的段落和一个按钮，单击该按钮会触发一个函数。`changeName()`函数调用`setName()`函数，然后将 name 变量的值更改为传递给`setName()`函数的值。

您所在州的值不能硬编码。在下一节中，您将看到如何在表单中使用`useState`钩子。

对于 React 初学者，请注意，您是在 return 语句之前创建函数和变量的。

## 如何在表单中使用 useState 挂钩

本节将帮助您理解如何为表单创建状态值，并在需要时更新它们。这个过程与我们在上一节中看到的没有太大的不同。

像往常一样，导入`useState`钩子:

```
import { useState } from "react"; 
```

我们会像之前一样创建初始状态。但是在这种情况下，它将是一个空字符串，因为我们处理的是输入元素的值。对该值进行硬编码意味着无论何时重新加载页面，输入都将具有该值。那就是:

```
 const [name, setName] = useState(""); 
```

现在我们已经创建了状态，让我们在 DOM 中创建 input 元素，并将 name 变量作为它的初始值。看起来是这样的:

```
return (
    <div>
      <form>
        <input
          type="text"
          value={name}
          onChange={(e) => setName(e.target.value)}
          placeholder="Your Name"
        />
        <p>{name}</p>
      </form>
    </div>
  );
```

您会注意到，我们没有在 return 语句之上创建一个函数来更新状态的值——但是如果您决定使用该方法，也没关系。

这里，我们使用`onChange`事件监听器，它等待输入字段中的任何值变化。每当发生变化时，就会触发一个匿名函数(将事件对象作为参数)，该函数又会调用`setName()`函数来用输入字段的当前值更新 name 变量。

下面是最终代码的样子:

```
import { useState } from "react";

function App() {
  const [name, setName] = useState("");

  return (
    <div>
      <form>
        <input
          type="text"
          value={name}
          onChange={(e) => setName(e.target.value)}
          placeholder="Your Name"
        />
        <p>{name}</p>
      </form>
    </div>
  );
}

export default App; 
```

## 什么是 useEffect 钩子？

效果钩子，顾名思义，在每次有状态变化时执行一个效果。默认情况下，它在第一次渲染后以及每次更新状态时运行。

在下面的例子中，我们创建了一个初始值为零的状态变量`count`。每当单击 DOM 中的一个按钮，这个变量的值就会增加 1。useEffect 钩子将在每次`count`变量改变时运行，然后将一些信息记录到控制台。

```
import { useState, useEffect } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`You have clicked the button ${count} times`)
  });

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}

export default App; 
```

如果您打算“挂钩”这个 React 特性，那么导入所需挂钩的第一行代码总是很重要的。我们导入了上面使用的两个挂钩:

```
import React, { useState, useEffect } from "react"; 
```

请注意，您可以使用 useEffect 挂钩来实现各种效果，比如从外部 API 获取数据(您将在本文的另一部分中看到)、更改组件中的 DOM 等等。

### 使用效果依赖关系

但是，如果您希望您的效果只在第一次渲染后运行，或者如果您有多个状态，并且只希望一个 after 效果附加到其中一个状态，会发生什么呢？

我们可以通过使用一个依赖数组来做到这一点，该数组作为第二个参数被传入到`useEffect`钩子中。

#### 如何运行一次效果

对于第一个例子，我们将传入一个只允许 useEffect 钩子运行一次的数组。这是一个如何工作的例子:

```
import { useState, useEffect } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`You have clicked the button ${count} times`)
  }, []);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}

export default App; 
```

除了 useEffect 钩子接受一个空数组`[]`作为第二个参数之外，上面的代码与上一节相同。当我们让数组为空时，效果将只运行一次，不管它所附加的状态如何变化。

#### 如何将效果附加到特定状态

```
import { useState, useEffect } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`You have clicked the first button ${count} times`);
  }, [count]);

  const [count2, setCount2] = useState(0);

  useEffect(() => {
    console.log(`You have clicked the second button ${count2} times`)
  }, [count2]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={() => setCount2(count2 + 1)}>Click me</button>
    </div>
  );
}

export default App; 
```

在上面的代码中，我们创建了两个状态和两个 useEffect 挂钩。通过将状态名`[count]`和`[count2]`传递给相应的 useEffect 数组依赖项，每个状态都有一个 after effect。

所以当`count`的状态改变时，负责观察这些变化的 useEffect 钩子将执行分配给它的任何 after effect。这同样适用于`count2`。

## 如何创建自己的钩子

既然您已经看到了 React 中的一些内置钩子(查看[文档](https://reactjs.org/docs/hooks-reference.html)以查看更多钩子)，那么是时候创建我们自己的定制钩子了。

你的钩子可以做很多事情。在这一节中，我们将创建一个钩子，从外部 API 获取数据，并将数据输出到 DOM。这让您不必在不同的组件上一遍又一遍地重新创建相同的逻辑。

### 步骤 1–创建您的文件

为自定义钩子创建新文件时，一定要确保文件名以“use”开头。我会叫我的`useFetchData.js`。

### 步骤 2–创建挂钩的功能

如前所述，我们将使用这个钩子从外部 API 获取数据。它将是动态的，所以没有什么是硬编码。我们将这样做:

```
import { useState, useEffect} from 'react'

function useFetchData(url) {
    const [data, setData] = useState(null);

    useEffect(() => {
      fetch(url)
        .then((res) => res.json())
        .then((data) => setData(data))
        .catch((err) => console.log(`Error: ${err}`));
    }, [url]);

    return { data };
}

export default useFetchData 
```

为了解释上面发生的事情:

*   我们导入钩子:`import { useState, useEffect} from 'react'`。
*   我们创建一个状态来保存将要返回的数据——初始状态将为 null: `const [data, setData] = useState(null);`。返回的数据将使用`setData()`函数更新`data`变量的值。
*   我们创建了一个效果，在第一次渲染和每次`url`参数改变时运行:

```
useEffect(() => {
      fetch(url)
        .then((res) => res.json())
        .then((data) => setData(data))
        .catch((err) => console.log(`Error: ${err}`));
    }, [url]);
```

*   我们返回数据变量:`return { data };`

### 步骤 3–创建一个新文件并导入定制钩子

因此，我们创建了我们的自定义挂钩。现在让我们创建一个新组件，看看如何在其中使用`useFetchData`钩子:

```
import useFetchData from './useFetchData'

function Users() {
    const { data } = useFetchData("https://api.github.com/users");

  return (
      <div>
          {data && (
            data.map((user) =>(
                <div className="text-white" key={user.id}>
                    <h1> {user.login} </h1>
                    <p> { user.type } </p>
                </div>
            ))
          )}
      </div>
  )
}

export default Users;
```

让我们来分解一下:

*   我们将组件命名为`Users.js`，因为它将用于从 GitHub API(可以是任何 API)获取用户数据。
*   我们导入了一个自定义钩子:`import useFetchData from './useFetchData'`。
*   我们在 return 语句前引用了钩子，并传入了 URL: `const { data } = useFetchData("https://api.github.com/users");`。API 请求将被发送到您传入的任何 URL。
*   使用`&&`操作符，只有当数据变量被来自 API 请求的数据更新时，DOM 才会被更新——也就是说，当`data != null`时。
*   我们循环处理返回的数据，并将其输出到 DOM。

## 结论

如果您已经了解了这一点，那么您应该对 React 中的钩子有很好的理解，如何使用它们，以及如何创建您自己的定制钩子。完全理解这一点的最好方法是实践，所以不要只是通读。

这篇文章涵盖了钩子的核心领域，但是它不会教你所有关于钩子的知识。因此，请务必查看 React JS 文档，以便了解更多相关信息。

感谢您的阅读。可以在 Twitter 上关注我 [@ihechikara2](https://twitter.com/Ihechikara2) 。