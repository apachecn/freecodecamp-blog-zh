# vue vs React——如何从一个框架过渡到另一个框架

> 原文：<https://www.freecodecamp.org/news/vue-vs-react-comparison-of-frameworks/>

这些天来，一个新的趋势前端框架不时发布。但是 React 和 Vue.js 仍然是最受欢迎的选择。

尽管两者都是表演性的、优雅的，并且可以说是容易学习的，但是它们在某些事情应该如何做上有一些不同的观点，以及实现相同最终结果的不同方法。

我相信使用前端框架变得舒适和高效主要是关于学习做常规事情的模式。

你知道，如何监听参数/数据的变化，并对其执行一些操作。以及如何将事件监听器或数据绑定到动作对象(按钮、复选框等)等等。

当我用 React 做一个附带项目时，我注意到我的想法是这样的:“是的，我可以在 Vue 中这样做，我会从子进程中发出一个事件，然后在父进程中监听它并更新这个数据”。然后我在谷歌上搜索如何在 React 里做类似的事情。

在本文中，我将向您展示如何在 React 和 Vue 中应用一些您在日常前端工作中会遇到的常见模式。然后，您可以使用这些方法轻松地从一个框架过渡到另一个框架。

无论您是需要从事 React 项目的有经验的 Vue 开发人员，还是相反，这都是有帮助的。我将使用现代反应与挂钩和 Vue 选项 API (Vue 2)。

我建议克隆包含我在本文中使用的所有代码的[库](https://github.com/yigiterinc/VueVsReact),并在每个部分中呈现各自的组件，并使用它们来真正理解它们是如何工作的。

克隆完成后，需要在 React 和 Vue 文件夹中运行 **npm install** 。然后你可以用 **npm start** 启动 React 项目，用 **npm run serve** 启动 Vue 项目。

```
https://github.com/yigiterinc/VueVsReact
```

GitHub link

### 目录

*   [React vs Vue 中的组件结构](#1)
*   [如何在 React 和 Vue 中使用状态](#2)
*   [如何在 Vue 中使用道具并做出反应](#3)
*   [如何在 Vue 和 React 中创建方法/函数](#4)
*   [造型选项](#5)
*   [如何将表单输入绑定到数据(状态)](#6)
*   [如何处理事件(用户输入)](#7)
*   [条件样式](#8)
*   [条件渲染](#9)
*   [渲染数组(列表)](#10)
*   [孩子对父母的交流](#11)
*   [对数据/状态变化做出反应](#12)
*   [计算属性与使用备忘录](#13)
*   [Vue 插槽 vs 渲染道具](#14)

## 组件结构

让我们鸟瞰一下这两个框架中的一些非常基本的组件。我们将在接下来的部分中对此进行扩展。

在 Vue 中，单个文件组件包含 3 个部分:模板、脚本和样式。

模板是将要呈现的部分。它包含组件的 HTML，可以访问脚本和样式中的数据(和方法)。

您可以在 Vue 的这些部分中找到关于组件的所有信息。

```
<template>
  <div id="structure">
    <h1>Hello from Vue</h1>
  </div>
</template>

<script>
export default {}
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
#structure {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style> 
```

要在没有路由器或其他复杂东西的情况下渲染这个组件，您可以将它添加到 **App.vue.** 我建议您在进行过程中渲染每个组件，以便您可以看到它们的运行:

```
<template>
  <div id="app">
    <structure />
  </div>
</template>

<script>
import Structure from './Structure/Structure.vue'

export default {
  name: 'App',
  components: { Structure },
}
</script> 
```

App.vue

您将更改**组件**部分中的导入组件和**模板**部分中的标签名称。

在 React 中，功能组件是返回 [JSX](https://reactjs.org/docs/introducing-jsx.html) 的函数(JavaScript 的一个扩展，允许在 JS 代码中使用 HTML 标签)。你可以认为它是在返回 HTML 来简化事情。如果你对基于类的 React 比较熟悉的话，那么将要呈现的部分通常是写在函数`render()`中的。

随着本教程的深入，在每一部分中，您都可以将各自的组件放入 App.js 中，使它们呈现如下:

```
import React from 'react'

function Structure() {
  return <div>Render me App</div>
}

export default Structure 
```

Component

```
import './App.css'

import Structure from './Structure/Structure'

function App() {
  return (
    <div className="App">
      <Structure />
    </div>
  )
}

export default App 
```

App.js

因此，您将更改 div 内部的导入和组件。

## 如何使用状态

在 Vue 中，我们了解到脚本标签包含与组件相关的数据和方法。Vue Options API 有特殊的关键字(选项)，比如我们可以使用的*数据、方法、道具、计算、观察、*和*混合*，还有生命周期方法，比如*创建*和*挂载*。

我们将使用`data` 选项来使用我们组件中的状态。数据应该被定义为一个函数，它返回一个包含我们状态的对象。

为了访问我们的 HTML(模板)内部的状态，我们必须使用双花括号并写出变量的名称。请记住，如果在 HTML 中使用(引用)了数据变量，那么对该变量的任何更改都会导致渲染。

```
<template>
  <div>
    <h1>Hello {{ currentFramework }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentFramework: ' Vue!',
      alternative: ' React!',
    }
  },
}
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style></style> 
```

UsingState.vue

在 React 中，功能组件曾经是无状态的。但是多亏了钩子，我们现在有了`useState`钩子来存储组件内部的状态。要使用 useState 挂钩，我们必须导入它，语法是:

```
import React, { useState } from 'react';

function App() {
    const [stateName, setStateName] = useState('default value'); 
}
```

useState syntax

我们在括号中定义状态变量的名称及其 setter 函数的名称，然后将变量的默认值传递给 useState 钩子。

为了更好地理解语法，您可以像这样想象钩子:它就像一个函数，创建一个变量，将其值设置为传递的值，然后返回一个包含变量及其 setter 函数的数组。

请注意，您应该使用一对花括号切换到 JavaScript 范围，并在 JSX 中打印一个变量，而不是像 Vue 那样使用双括号。

```
import { React, useState } from 'react'

function TestUseState() {
  const [frameworkName, setFrameworkName] = useState('React')

  return (
    <div>
      <h1>useState API</h1>
      <p>Current Framework: {frameworkName}</p>
    </div>
  )
}

export default TestUseState 
```

## 如何使用道具

在 Vue 中，我们通过在脚本字段中导出的对象内添加 props 选项来定义 props，就像我们对 data 选项所做的那样。将道具定义为对象是一种最佳实践，这样我们可以更好地控制它们的使用方式。

例如，指定它们的类型、默认值，并在必要时使它们成为必需的。如果你错误地使用了组件，Vue 会显示一个警告，比如没有传递一个必需的属性就调用它。

假设我们有一个 Address 子组件，它将从`UserInfo`父组件调用。

```
<template>
  <div class="address">
    <p>City: {{ city }}</p>
    <p>Street: {{ street }}</p>
    <p>House No: {{ houseNumber }}</p>
    <p>Postal Code: {{ postalCode }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  props: {
    city: {
      type: String,
      default: 'Munich',
    },
    street: {
      type: String,
      required: true,
    },
    houseNumber: {
      type: Number,
      required: true,
    },
    postalCode: {
      type: Number,
      required: true,
    },
  },
}
</script>

<style></style> 
```

Child Component (Address.vue)

我们可以像访问数据变量一样访问我们的 props 使用模板中的双括号。我们可以像这样传递来自父母的道具:

```
<template>
  <div class="address">
    <p>Name: Yigit</p>
    <Address
      street="randomStrasse"
      :postalCode="80999"
      :houseNumber="32"
    ></Address>
  </div>
</template>

<script>
import Address from '@/components/Address.vue'

export default {
  data() {
    return {}
  },
  components: {
    Address,
  },
}
</script>

<style></style> 
```

Parent Component (UserInfo.vue)

注意我们如何使用 v-bind 简写`:`并写下`:postalCode`和`:houseNumber`来表明这些不是字符串而是数字类型的对象。每当我们需要传递除字符串以外的任何东西(数组、对象、数字等等)时，我们都必须使用这种语法。

如果您来自 React，这可能会让您感到困惑，因此您可能希望阅读更多关于 v-bind 的内容，以便更好地理解它是如何工作的。

在 React 中，我们不需要显式定义将在子组件中传递什么属性。我们既可以使用对象析构来给变量分配属性，也可以使用 props 对象来访问它们。我们在 JSX 访问我们的道具，就像我们访问国家一样。

```
import React from 'react'

function Address({ city, street, postalCode, houseNumber }) {
  return (
    <div>
      <p>City: {city}</p>
      <p>Street: {street}</p>
      <p>Postal Code: {postalCode}</p>
      <p>House Number: {houseNumber}</p>
    </div>
  )
}

export default Address 
```

Child Component (Address.js)

我们可以像这样传递来自父母的道具:

```
import React from 'react'

function UserInfo() {
  return (
    <div>
      <p>Name: Yigit</p>
      <Address
        city="Istanbul"
        street="Ataturk Cad."
        postalCode="34840"
        houseNumber="92"
      ></Address>
    </div>
  )
}

export default UserInfo
```

Parent Component (UserInfo.vue)

## 如何创建方法/函数

在 Vue 中，我们定义方法类似于数据——我们只需在数据下放置一个方法选项并定义方法。我们可以从模板中调用这些方法，方法可以访问/修改我们的数据。

```
<template>
  <div>
    {{ sayHello() }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      to: 'Methods',
    }
  },
  methods: {
    sayHello() {
      return 'Hello ' + this.to
    },
  },
}
</script>

<style></style> 
```

sayHello.vue

当试图从导出的对象内部(脚本标记内的代码)访问组件方法或数据属性时要小心。如果你不包括`this` 关键字，Vue 会显示一个错误，说它不知道那个属性/方法在哪里。

在 React 中，事情要简单一些。如果你愿意，这只是普通的 JS 函数定义，带有 ES6 语法。

```
import React from 'react'

function HelloFunctions() {
  const to = 'Functions'

  function sayHello() {
    return 'Hello ' + to
  }

  const sayHelloModern = () => 'Hello ' + to

  return (
    <div>
      {sayHello()}
      <br />
      {sayHelloModern()}
    </div>
  )
}

export default HelloFunctions 
```

SayHello.js

## 样式选项

设计 Vue 组件的样式非常简单。我们只需要在`style` 标签中编写普通的 CSS 类和选择器。

Vue 还通过使用`scoped`关键字支持作用域 CSS。它有助于避免由于在不同的组件中分配相同的类名而导致的可视化错误。例如，您可以将所有组件中的主容器命名为`main-container`，只有该组件文件中的样式才会应用到每个主容器中。

```
<template>
  <div class="main-container">
    <h3 class="label">I am a styled label</h3>
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
}
</script>

<style scoped>
.main-container {
  position: absolute;
  left: 50%;
  top: 45%;
  margin: 0;
  transform: translate(-50%, -50%);
  text-align: center;
}

.label {
  font-size: 30px;
  font-weight: 300;
  letter-spacing: 0.5rem;
  text-transform: uppercase;
}
</style> 
```

Styling.vue

不过，在 React 中，我们在样式方面有更多的选择，这基本上取决于您的个人偏好，因为有多种方式来样式化您的组件。我会在这里推荐几个不错的选择。

### 1)在. css 文件中编写常规 CSS 并导入它

这可能是将样式应用于 React 组件的最基本和最直接的方法。这并不意味着这是一个坏方法，因为它允许你写普通的老 CSS。如果你是一个刚刚开始使用 React 的 CSS 专家，这是一个很好的方法。

```
import React from 'react'
import './styles.css'

function Styled() {
  return (
    <div>
      <h3 class="title">I am red</h3>
    </div>
  )
}

export default Styled 
```

Styled.js

```
.title {
  color: red;
  font-size: 30px;
} 
```

styles.css

### 2)使用材质 UI (useStyles/makeStyles)

Material UI 是一个 CSS 框架，有很多可重用的组件。它还提供了一种样式化组件的方法，这种方法在 JS 中使用 CSS，因此有它的优点，比如作用域 CSS。

`makeStyles`钩子接收一个对象中的类列表，然后你可以通过将这些类分配给对象来使用它们。

```
import React from 'react';
import { makeStyles } from '@material-ui/core/styles';
import Button from '@material-ui/core/Button';

const useStyles = makeStyles({
  root: {
    background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
    border: 0,
    borderRadius: 3,
    boxShadow: '0 3px 5px 2px rgba(255, 105, 135, .3)',
    color: 'white',
    height: 48,
    padding: '0 30px',
  },
});

export default function Hook() {
  const classes = useStyles();
  return <Button className={classes.root}>Hook</Button>;
} 
```

### 3)使用样式组件(JS 中的 CSS)

样式化组件现代且易于使用，它允许你利用普通 CSS 的所有特性。

在我看来，比 MaterialUI 更易用，功能更强大(也可以用这个来样式 MaterialUI 组件，而不是用`makeStyles`)。这也比导入 CSS 文件要好，因为它是有作用域的，并且样式化的组件是可重用的。

```
import React from 'react'
import styled, { css } from 'styled-components'

// Use Title and Wrapper like any other React component – except they're styled!
const Title = styled.h1`
  font-size: 2em;
  text-align: center;
  color: palevioletred;
`

// Create a Wrapper component that'll render a <section> tag with some styles
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
  height: 100vh;
`

function StyledComponent() {
  return (
    <Wrapper>
      <Title>Hello World!</Title>
    </Wrapper>
  )
}

export default StyledComponent 
```

Styled Components

## 如何将表单输入绑定到数据(状态)

我们已经知道了如何在组件中设置状态，但是我们还需要一种方法将用户输入绑定到状态。例如，在登录表单中，我们可能需要在组件状态中存储用户的用户名和密码输入。React 和 Vue 有不同的方法保持用户输入与状态同步。

在 Vue 中，我们对此操作有一个特殊的指令，称为 [v-model](https://vuejs.org/v2/guide/forms.html) 。要使用它，您需要像我们之前所学的那样，通过使用`data`属性来创建一个状态。然后将 v-model 关键字添加到输入中，并指定哪个数据变量负责存储该输入(这适用于表单输入、textarea 和 select 元素)。

这是一种高级的、干净的数据连接方式，不需要创建额外的 lambda 函数或处理程序。

```
<template>
  <div>
    <input v-model="inputState" type="text" />
    <br />
    {{ inputState }}
    <br />
    <button @click="changeInputState()">Click to say goodbye</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputState: 'Hello',
    }
  },
  methods: {
    changeInputState: function () {
      this.inputState = 'Goodbye'
    },
  },
}
</script>

<style></style> 
```

这里有一个小例子:我们有一个文本输入，我们通过使用 v-model 关键字将它连接到`inputState`变量。因此，每当用户输入文本时，`inputState`变量将自动反映这些变化。

然而，有一件特别的事情你需要知道:与 React 中的**单向绑定**相比，v-model 实现了**双向数据绑定**。

这意味着，不仅当您更改输入时，数据也会更改，而且如果您更改数据，输入值也会更改。

为了演示这一点，我创建了一个按钮并将其连接到一个方法。先不要担心事件处理，我们将在下一节中看到。当单击按钮时，inputState 变量的值会发生变化，当这种情况发生时，输入也会发生变化。

我鼓励您通过运行代码亲自尝试一下。另外，注意你的输入框的初始值是“Hello”——它没有被初始化为空字符串或 null，因为我们将`inputState`变量设置为“Hello”。

现在让我们看看 React:

```
import { React, useState } from 'react'

function FormInputBinding() {
  const [userInput, setUserInput] = useState('Hello')

  return (
    <div>
      <input type="text" onChange={(e) => setUserInput(e.target.value)} />
      <button onClick={() => setUserInput('Goodbye')}>
        Click to say goodbye
      </button>
      {userInput}
    </div>
  )
}

export default FormInputBinding 
```

本主题与处理用户事件重叠，因此如果您有不明白的地方，请等到完成下一节。这里，我们手动处理`onChange`事件，并调用`setUserInput`函数将状态设置为事件的值。

正如我们前面提到的，React 使用了一个**单向绑定**模型。这意味着改变 userInput 状态不会影响我们在文本输入中看到的值——最初我们不会在输入框中看到 Hello。同样，当我们单击按钮时，它将更新状态，但框内的输入将保持其值。

## 处理事件(用户输入)

让我们看看 Vue 中更接近真实案例的另一种形式，并作出反应:

```
<template>
  <div>
    <input
      v-model="username"
      id="outlined-basic"
      label="Username"
      variant="outlined"
    />
    <input
      v-model="password"
      id="outlined-basic"
      type="password"
      label="Password"
      variant="outlined"
    />

    <input
      v-model="termsAccepted"
      id="outlined-basic"
      type="checkbox"
      label="Password"
      variant="outlined"
    />

    <Button variant="contained" color="primary" @click="submitForm">
      Submit
    </Button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      termsAccepted: false,
    }
  },
  methods: {
    submitForm: function () {
      console.log(this.username, this.password, this.termsAccepted)
    },
  },
}
</script>

<style></style> 
```

Vue Form

如您所见，我们正在使用刚刚学习的 v-model 属性将所有输入连接到数据属性(state)。因此，只要输入发生变化，Vue 就会自动更新相应的变量。

要查看我们如何处理按钮上的点击事件，请检查提交按钮。我们使用 [v-on](https://vuejs.org/v2/guide/events.html) 关键字来处理点击事件。`@click`只是`v-on:click`的简称。

每当点击事件发生时，它简单地调用`submitForm`方法。您可以通过浏览链接的文档来熟悉可能事件的列表。

在 React 中，我们可以有这样一种形式:

```
import { React, useState } from 'react'

import {
  TextField,
  Checkbox,
  FormControlLabel,
  Button,
} from '@material-ui/core'

function EventHandling() {
  let [username, setUsername] = useState('')
  let [password, setPassword] = useState('')
  let [termsAccepted, setTermsAccepted] = useState(false)

  const submitForm = () => {
    console.log(username, password, termsAccepted)
  }

  const formContainer = {
    display: 'flex',
    flexDirection: 'column',
    alignItems: 'center',
    gap: '20px',
  }

  return (
    <div style={formContainer}>
      <TextField
        onInput={(e) => setUsername(e.target.value)}
        id="outlined-basic"
        label="Username"
        variant="outlined"
      />
      <TextField
        onInput={(e) => setPassword(e.target.value)}
        id="outlined-basic"
        type="password"
        label="Password"
        variant="outlined"
      />

      <FormControlLabel
        control={
          <Checkbox
            type="checkbox"
            checked={termsAccepted}
            onChange={(e) => setTermsAccepted(e.target.checked)}
          />
        }
        label="Accept terms and conditions"
      />

      <Button variant="contained" color="primary" onClick={() => submitForm()}>
        Submit
      </Button>
    </div>
  )
}

export default EventHandling
```

React Form

我们为每个输入创建状态变量。然后，我们可以监听输入上的事件，并且可以在处理函数内部访问该事件。作为对这些事件的响应，我们调用状态设置器来更新我们的状态。

## 条件样式

条件样式意味着如果条件为真，则将类或样式绑定到元素。

在 Vue 中，可以这样实现:

```
<template>
  <div>
    <button @click="toggleApplyStyles"></button>
    <p :class="{ textStyle: stylesApplied }">
      Click the button to {{ stylesApplied ? 'unstyle' : 'style' }} me
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      stylesApplied: false,
    }
  },
  methods: {
    toggleApplyStyles: function () {
      this.stylesApplied = !this.stylesApplied
    },
  },
}
</script>

<style>
.textStyle {
  font-size: 25px;
  color: red;
  letter-spacing: 120%;
}
</style> 
```

Vue Conditional Styling

我们创建了一个段落，并希望仅当`stylesApplied`数据属性的值为真时才应用`textStyle`类。我们可以使用 v-bind 来实现这一点。冒号是 v-bind 的简写，因此`:class`与`v-bind:class`相同。

我们使用 v-bind 对象语法来绑定类。我们向类属性传递一个对象:{textStyle: stylesApplied}，这意味着如果`stylesApplied`为真，则应用`textStyle`类。

这在开始时有点复杂，但它帮助我们避免使用多个连锁的 if 语句来确定将应用哪个类，并以一种干净的方式处理样式绑定:左边是类名(对象键)，右边是条件(对象值)。

作为回应，我们必须更原始地做事。

```
import { React, useState } from 'react'
import './styles.css'

function ConditionalStyling() {
  let [stylesApplied, setStylesApplied] = useState(false)

  return (
    <div>
      <button onClick={() => setStylesApplied(!stylesApplied)}>Click me</button>
      <p style={{ color: stylesApplied ? 'red' : 'green' }}>Red or Green</p>
      <p className={stylesApplied ? 'styleClass' : ''}>Red with class</p>
    </div>
  )
}

export default ConditionalStyling 
```

React Conditional Styling

这里我们使用普通的 JavaScript 将 styles 对象或类名绑定到元素。我认为这使代码有点复杂，我不是这种语法的忠实粉丝。

## 条件渲染

有时，我们希望在某个操作完成后呈现一个组件——比如从 API 获取数据。或者，如果有错误，我们可能希望显示一条错误消息；如果没有，我们可能希望显示一条成功消息。

对于这种情况，我们使用条件呈现来改变 HTML 以编程方式呈现。

在 Vue 中，也有专门的指令。我们可以根据条件使用`v-if`和`v-else`甚至`v-else-if`来渲染模板。

```
<template>
  <div>
    <h2 v-if="condition1">condition1 is true</h2>
    <h2 v-else-if="condition2">condition2 is true</h2>
    <h2 v-else>all conditions are false</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      condition1: false,
      condition2: false,
    }
  },
}
</script>

<style></style>
```

这种语法允许我们创建复杂的条件呈现链，而不会用 if else 语句使模板代码复杂化。

以下是使用 React 实现相同输出的方法之一:

```
import React from 'react'

function ConditionalRendering() {
  const condition1 = false
  const condition2 = false

  function getMessage() {
    let message = ''

    if (condition1) {
      message = 'condition1 is true'
    } else if (condition2) {
      message = 'condition2 is true'
    } else {
      message = 'all conditions are false'
    }

    return <h1>{message}</h1>
  }

  return <>{getMessage()}</>
}

export default ConditionalRendering 
```

这只是普通的 JS 和 JSX，这里没有魔法。

## 呈现数组(列表)

现在让我们看看如何呈现列表数据。在 Vue 中，有`v-for`关键字可以做到这一点。names 中的语法 **name 意味着 name 变量将始终保持当前名称。随着 index 的变化，如果 index 为 0，则为`names[0]`以此类推。我们也可以通过在括号内指定索引来访问它。 **v-for** 指令也需要一个密钥。**

```
<template>
  <div>
    <h1>Names</h1>
    <ul>
      <li v-for="(person, index) in people" :key="index">{{ person.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      people: [
        {
          id: 0,
          name: 'Yigit',
        },
        {
          id: 1,
          name: 'Gulbike',
        },
        {
          id: 2,
          name: 'Mete',
        },
        {
          id: 3,
          name: 'Jason',
        },
        {
          id: 4,
          name: 'Matt',
        },
        {
          id: 5,
          name: 'Corey',
        },
      ],
    }
  },
}
</script>
```

注意，我们也可以使用 **v-for** 指令来迭代对象的属性。记住，在 JS 中，数组只是带有数字键的对象的一个特殊子集。

在 React 中，我们将使用`Arrays.map`函数迭代数组，并为每个元素返回一个 JSX 标签。

```
import React from 'react'

function RenderingLists() {
  const cities = [
    'Istanbul',
    'München',
    'Los Angeles',
    'London',
    'San Francisco',
  ]

  return (
    <div>
      <h1>Cities</h1>
      {cities.map((city, index) => (
        <h4 key={index}>{city}</h4>
      ))}
    </div>
  )
}

export default RenderingLists 
```

## 孩子与父母的交流

假设您有一个表单组件和一个按钮子组件。您希望在单击按钮时执行一些操作，比如调用 API 提交数据——但是您不能访问按钮组件中的表单数据，因为它存储在父组件中。你是做什么的？

嗯，如果你在 Vue 中，你想从子进程中发出一个自定义事件，并在父进程中对其进行操作。在 React 中，您将创建将在父组件中执行(当单击按钮时)的函数，该函数可以访问表单数据并将其传递给按钮组件，以便它可以调用父组件的函数。

让我们看看 Vue 中的一个例子:

```
<template>
  <div>
    <button @click="buttonClicked">Submit</button>
  </div>
</template>

<script>
export default {
  methods: {
    buttonClicked: function () {
      this.$emit('buttonClicked') // Emits a buttonClicked event to parent
    },
  },
}
</script>

<style></style> 
```

Child.vue

在我们的子组件中，我们有按钮，点击时我们发出一个`buttonClicked`事件。我们也可以在这个调用中发送数据。例如，如果这是一个文本输入框而不是一个按钮，并且有自己的数据，我们可以用 emit 将数据发送给父节点。

在父组件中，我们需要监听刚刚创建的定制`buttonClicked`事件。

```
<template>
  <form action="#">
    <input v-model="username" type="text" />
    <child @buttonClicked="handleButtonClicked" />
  </form>
</template>

<script>
import Child from './Child.vue'

export default {
  components: { Child },
  data() {
    return {
      username: '',
    }
  },
  methods: {
    handleButtonClicked: function () {
      console.log(this.username)
    },
  },
}
</script>

<style></style> 
```

Parent.vue

我们只是在子组件调用中添加了一个`@buttonClicked`事件来处理这个自定义事件。

在 React 中，我们可以通过向子组件传递一个处理函数来获得相同的结果。这个概念对我来说有点复杂，但语法比 Vue 的例子简单，也没有什么魔力。

```
import React from 'react'

function Child({ handleButtonClicked }) {
  return (
    <div>
      <button onClick={() => handleButtonClicked()}>Submit</button>
    </div>
  )
}

export default Child 
```

Child.js

我们访问`handleButtonClicked` prop 并在按钮被点击时调用它。

```
import { React, useState } from 'react'

import Child from './Child.js'

function Parent() {
  const [username, setUsername] = useState('')

  const submitForm = () => {
    console.log(username)
    // Post form data to api...
  }

  return (
    <div>
      <input onChange={(e) => setUsername(e.target.value)} type="text" />
      <Child handleButtonClicked={submitForm} />
    </div>
  )
}

export default Parent 
```

在父类中，我们将`submitForm`函数作为完成工作的`handleButtonClicked`道具来传递。

## 对数据/状态变化做出反应

在某些情况下，我们需要对数据变化做出反应。例如，当您想要执行异步或昂贵的操作来响应数据的变化时。

在 Vue 中，我们有`watch` 属性或属性来表示。如果你熟悉 React 中的`useEffect`，这是你能在 Vue 中找到的最接近的东西，它们的用例或多或少是相同的。

让我们看一个例子:

```
<template>
  <div>
    <input v-model="number1" type="number" name="number 1" />
    <input v-model="number2" type="number" name="number 2" />
    {{ sum }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      number1: 0,
      number2: 0,
      sum: 0,
    }
  },
  watch: {
    number1: function (val) {
      this.sum = parseInt(val) + parseInt(this.number2)
    },
    number2: function (val) {
      this.sum = parseInt(this.number1) + parseInt(val)
    },
  },
}
</script>

<style></style> 
```

这里，我们在数据属性中定义了`number1`和`number2`。我们有两个各自的输入，我们打印这些数字的总和，我们希望当任何输入改变时，总和更新。

在`watch`属性中，我们写下我们希望**观察**的变量的名称。在这种情况下，我们既想看数字 1 又想看数字 2。如果用户输入一个输入，`v-model`将改变相应的数据变量，当这种情况发生时，该变量的**手表**功能将被触发，sum 的值将被重新计算。

请注意，在实际应用中，您不需要使用 watch 来完成这个简单的事情，您只需将 sum 放在`computed` 中即可。这是一个虚构的例子来演示`watch`是如何工作的。

在将`watch`用于更复杂的事物之前，比如对象、数组和嵌套结构，我建议阅读[这篇文章](https://michaelnthiessen.com/how-to-watch-nested-data-vue/)，因为你可能需要学习一些`watch`选项，比如`deep`和`immediate`。

在 React 中，我们使用内置的`useEffect`钩子来观察变化。

```
import { React, useState, useEffect } from 'react'

function ReactToDataChanges() {
  const [number1, setNumber1] = useState(0)
  const [number2, setNumber2] = useState(0)
  const [sum, setSum] = useState(0)

  useEffect(() => {
    console.log('I am here!')
    setSum(parseInt(number1) + parseInt(number2))
  }, [number1, number2])

  return (
    <div>
      <input
        onChange={(e) => setNumber1(e.target.value)}
        type="number"
        name="number 1"
      />
      <input
        onChange={(e) => setNumber2(e.target.value)}
        type="number"
        name="number 2"
      />
      {sum}
    </div>
  )
}

export default ReactToDataChanges 
```

`useEffect`期望函数在依赖关系改变时作为第一个参数运行，依赖关系列表作为第二个参数运行。

请记住，这也是一个虚构的示例来演示`useEffect`(我们可以通过将 sum 变量从状态中取出来，在没有`useEffect`的情况下实现相同的效果)。

我想展示这个钩子的一个非常常见的用例:在组件加载后从 API 获取数据:

```
useEffect(() => {
    fetchUserData()
}, [])

const fetchUserData = async () => {
    const url = '';
    const response = await axios.get(url);
    const user = response.data;
    setUser(user);
}
```

我们可以指定空数组在组件渲染时运行一次`useEffect`。在 Vue 中，我们会在一个生命周期钩子中做同样的操作，比如`created`或者`mounted`。

## 计算属性与使用备忘录

Vue 有一个被称为计算属性的概念，它的目的是缓存相当复杂的计算，并在其中一个依赖关系改变时重新评估它们的值(类似于`watch`)。

通过将逻辑转移到 **JavaScript** 中，这也有助于保持模板的干净和简洁。在这种情况下，它们的行为就像我们不想成为状态的常规变量。

```
<template>
  <div>
    <h3>Yigit's Favorite Cities are:</h3>
    <p v-for="city in favCities" :key="city">{{ city }}</p>
    <h3>Yigit's Favorite Cities in US are:</h3>
    <p v-for="town in favCitiesInUS" :key="town">{{ town }}</p>
    <button @click="addBostonToFavCities">
      Why is Boston not in there? Click to add
    </button>
  </div>
</template>

<script>
export default {
  computed: {
    favCitiesInUS: function () {
      return this.favCities.filter((city) => this.usCities.includes(city))
    },
  },
  data() {
    return {
      favCities: [
        'Istanbul',
        'München',
        'Los Angeles',
        'Rome',
        'Florence',
        'London',
        'San Francisco',
      ],
      usCities: [
        'New York',
        'Los Angeles',
        'Chicago',
        'Houston',
        'Phoenix',
        'Arizona',
        'San Francisco',
        'Boston',
      ],
    }
  },
  methods: {
    addBostonToFavCities() {
      if (this.favCities.includes('Boston')) return

      this.favCities.push('Boston')
    },
  },
}
</script>

<style></style> 
```

Computed Example

这里，我们不想把`favCitiesInUS` 函数放在我们的模板里面，因为它太多逻辑了。

可以把它想象成一个输出将被缓存的函数。只有当`favCities`或`usCities`(其依赖关系)改变时，才会重新评估该功能。要尝试这样做，您可以单击按钮，看看模板是如何变化的。请记住，计算函数不接收任何参数。

我们可以使用`useMemo`钩子在 React 中实现同样的结果。我们将函数包装在 useMemo 钩子中，并在第二个参数中提供依赖项列表。每当其中一个依赖关系改变时，React 将再次运行该函数。

```
import { React, useMemo, useState } from 'react'

function UseMemoTest() {
  const [favCities, setFavCities] = useState([
      'Istanbul',
      'München',
      'Los Angeles',
      'Rome',
      'Florence',
      'London',
      'San Francisco',
    ]),
    [usCities, setUsCities] = useState([
      'New York',
      'Los Angeles',
      'Chicago',
      'Houston',
      'Phoenix',
      'Arizona',
      'San Francisco',
      'Boston',
    ])

  const favCitiesInUs = useMemo(() => {
    return favCities.filter((city) => usCities.includes(city))
  }, [favCities, usCities])

  return (
    <div>
      <h3>Yigit's Favorite Cities are:</h3>
      {favCities.map((city) => (
        <p key={city}>{city}</p>
      ))}
      <h3>Yigit's Favorite Cities in US are:</h3>
      {favCitiesInUs.map((town) => (
        <p key={town}>{town}</p>
      ))}
      <button onClick={() => setFavCities([...favCities, 'Boston'])}>
        Click me to add
      </button>
    </div>
  )
}

export default UseMemoTest 
```

## Vue 插槽 vs 渲染道具

我们有时希望创建可以在其中显示其他组件的通用组件，例如可以在其中显示任何类型项目的网格组件。

为此，Vue 有一个叫做[插槽](https://twitter.com/caglarcilara/status/1448905192045531143?s=20)的机制。插槽背后的逻辑非常简单:在组件中打开一个插槽，该插槽应该接收另一个组件来呈现(让我们称之为消费者，因为它消费所提供的元素)。

在生产者中，您传递消费者应该在其标签中呈现的组件——您可以将它们视为填充您在消费者中打开的槽。第一个元素将在第一个槽中呈现，第二个元素在第二个槽中呈现，依此类推。

如果有多个插槽，您还必须设置名称。让我们看一个例子:

```
<template>
  <div>
    <h3>Component in slot 1:</h3>
    <slot name="slot1"></slot>
    <h3>Hello slot1</h3>
    <slot name="slot2"></slot>
    <h3>Component in slot 3:</h3>
    <slot name="slot3"></slot>
  </div>
</template>

<script>
export default {}
</script>

<style></style> 
```

Consumer.vue

这是我们的消费者部分。它可能是从多个组件创建一个布局或一些组合。我们创建这些插槽，并给它们不同的名称。

```
<template>
  <div>
    <consumer>
      <custom-button
        text="I am a button in slot 1"
        slot="slot1"
      ></custom-button>
      <h1 slot="slot2">I am in slot 2, yayyy</h1>
      <custom-button text="I am in slot 3" slot="slot3"></custom-button>
    </consumer>
  </div>
</template>

<script>
import CustomButton from './CustomButton.vue'
import Consumer from './Consumer.vue'

export default {
  components: {
    CustomButton,
    Consumer,
  },
}
</script>

<style></style> 
```

Producer.vue

这里，生产者通过将组件的插槽名称指定为属性，将组件传递给消费者的插槽。

如果您对此感兴趣，这里有一个简单的`CustomButton`组件:

```
<template>
  <button>{{ text }}</button>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: 'I am a button component',
    },
  },
}
</script>

<style></style> 
```

不运行代码能猜出输出吗？这可能是一个很好的练习，以确保你理解插槽。

在 React 中，要简单得多。我认为老虎机使事情过于复杂。由于 React 使用 JSX，我们可以只传递要渲染的组件作为道具。

```
import React from 'react'
import Child from './Child'

function Parent() {
  const compToBeRendered = (
    <div>
      <h1>Hello</h1>
      <button>Im button</button>
    </div>
  )

  return (
    <div>
      <Child compToBeRendered={compToBeRendered}></Child>
    </div>
  )
}

export default Parent 
```

Parent.js

```
import React from 'react'

function Child({ compToBeRendered }) {
  return (
    <div>
      <h1>In child:</h1>
      {compToBeRendered}
    </div>
  )
}

export default Child 
```

Child.js

## 最后的想法

在本文的最后一部分，我将分享我对这些框架的两点看法。

正如我们在文章中看到的，Vue 通常有自己的做事方式，并且对大多数事情有不同的构造。在我看来，有时这让开始变得有点困难。

另一方面，React 就像纯 JS，混合了 JSX:没有太多的魔法，也没有太多需要学习的特殊关键字。

虽然它可能不是对初学者最友好的框架，但我相信 Vue 提供的抽象/关键字(如 v-for、v-if 和 Options API)允许您在更高的抽象级别上编写代码(想想添加一个简单的语句来迭代组件，而不是一个低级的多行 map 函数)。

这些特性也使你的 Vue 代码更加结构化和干净，因为框架是固执己见的。所以它有自己的做事方式，如果你用那种方式做事，你最终会得到易于阅读和理解的代码。

另一方面，React 对事情不是很固执己见，并为开发人员提供了很大的自由来决定他们自己项目的结构。

但是这种自由是有代价的:如果你是一个不知道最佳实践的初学者，很容易以混乱的代码和糟糕的项目结束。

在我看来，另一个重要的区别是:如果你要用 React 构建一个非基础的项目，你将需要使用大量的外部库以正常的速度进行开发，这意味着你也需要学习那些东西是如何工作的。

## 结论

感谢您的阅读，我希望这个比较是有用的。如果您想联系、提问或进一步讨论，请随时在 [LinkedIn](https://www.linkedin.com/in/yigit-erinc/) 上给我发消息。