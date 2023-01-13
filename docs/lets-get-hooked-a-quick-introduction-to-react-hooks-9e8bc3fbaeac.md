# 让我们上钩:快速介绍反应钩

> 原文：<https://www.freecodecamp.org/news/lets-get-hooked-a-quick-introduction-to-react-hooks-9e8bc3fbaeac/>

#### React 挂钩入门

React 团队在 2018 年 10 月下旬的 React Conf 上向全世界介绍了 React Hooks。2019 年 2 月初，他们终于推出了 React v16.8.0。虽然我可能和大多数其他人一样，暂时无法在生产中使用它们(直到我们决定更新 React)，但我一直在边上试验它们。

实际上，我对此非常兴奋，我将在当地的一次聚会上介绍它。此外，我将在五月亨茨维尔的 WeRockITConf 上做一个关于钩子(和其他即将到来的 React 特性)的演讲！(编辑:我现在已经做了这些演讲，你可以在我的网站上找到这些演讲和相关资源！)但是现在，这里是 React 钩子的入门方法！

# 钩子到底是什么？

React 挂钩允许您使用状态和其他 React 特性，而不必定义 JavaScript 类。这就像能够利用纯组件 **和** 状态和组件生命周期方法的干净和简单。这是因为钩子只是普通的 JavaScript 函数！这有助于产生更干净、更简洁的代码。一个简单计数组件在有和没有挂钩的情况下代码外观的并排比较:

```
import './App.css';
import React, { useState } from 'react';

const HooksExample = () => {
    const [counter, setCount] = useState(0);

    return (
        <div className="App">
            <header className="App-header">
                The button is pressed: { counter } times.
                <button
                    onClick={() => setCount(counter + 1)}
                    style={{ padding: '1em 2em', margin: 10 }}
                >
                    Click me!
                </button>
            </header>
        </div>
    )
}

export default HooksExample;
```

NoHooks.js:

```
import './App.css';
import React, { Component } from 'react';

export class NoHooks extends Component {
    constructor(props) {
        super(props;
        this.state = {
            counter: 0
        }
    }

    render() {
        const { counter } = this.state;
        return (
            <div className="App">
                <header className="App-header">
                    The button is pressed: { counter } times.
                    <button
                        onClick={() => this.setState({ counter: counter + 1 }) }
                        style={{ padding: '1em 2em', margin: 10 }}
                    >
                        Click me!
                    </button>
                </header>
            </div>
        )	
    }
}

export default NoHooks;
```

不仅代码要小得多——节省的空间肯定会为更大的组件积累起来——它的可读性*也大得多，这是钩子的一个巨大优势。对于刚刚开始使用 React 的初学者来说，阅读第一段代码更容易，也更容易看到到底发生了什么。对于第二个块，我们有一些无关的元素，这足以让你停下来想知道它是干什么用的。*

*钩子的另一个伟大之处是你可以创建自己的钩子！这意味着我们过去不得不从一个组件到另一个组件重新编写许多有状态的逻辑，现在我们可以抽象出一个定制的钩子，然后 **重用它** 。*

*我想到的一个改变生活的例子是表单的使用。对于表单的所有有状态逻辑，很难减少组件的大小。但是现在，有了钩子，复杂的表单可以变得简单得多，而不需要使用其他的表单库。*

*但是在我们开始之前，让我们看一下手头的钩子——use state。*

# *useState*

*顾名思义，useState 是一个钩子，它允许你在函数中使用状态。我们将其定义如下:*

*const [ someState，updateState]= use state(initial state)*

*让我们来分解一下:*

*   *****someState:**** 让您访问当前状态变量， **someState***
*   *****updateState:**** 函数，允许您更新状态——无论您传递给它什么，它都会变成新的 **someState***
*   *****初始状态:**** 你想要的 **某个状态** 初始渲染时*

*(如果你不熟悉数组析构语法，就此打住，阅读 [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Basic_variable_assignment) 。)*

*现在我们已经了解了 useState 的基本格式以及如何调用和使用它，让我们回到前面的例子。*

*本例中 ****，计数器**** 为状态变量， ****setCount**** 为更新函数， ****0**** 为初始状态。我们使用****set count(counter+1)****来增加按钮按下时的计数，使 ****counter + 1**** 成为 ****counter**** 的新值。或者，如果我们想使用以前的状态来更新当前状态，我们可以将旧状态传递给 setCount:*

*`setCount(prevCount => prevCount + 1)`*

*这是一个简单的例子，并不反映我们通常在实际应用中使用的内容。但是让我们来看看我们更可能使用的东西——一个简单的电子邮件和密码登录表单:*

```
*`import './App.css';
import React, { useState } from 'react';

const LoginForm = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    return (
        const { handleSubmit } = this.props;
        <div className="App">
            <header className="App-header">
                <form onSubmit={handleSubmit}>
                    <input value={ email } onChange={(e) => setEmail(e.target.value) } />
                    <input value={ password } onChange={(e) => setPassword(e.target.value) } />
                    <button type="submit">Submit</button>
                </form>
            </header>
        </div>
    )
}

export default LoginForm;`*
```

*我们有两个独立的状态字段和状态更新器。这允许我们创建非常简单的表单，而不需要创建一个完整的 JavaScript 类。*

*如果我们想进一步简化，我们可以创建一个对象作为状态。但是，useState 替换了整个状态，而不是更新对象(就像 setState 一样)，因此我们可以复制 setState 的通常行为，如下所示:*

```
*`import './App.css';
import React, { useState } from 'react';

const LoginForm = () => {
    const [login, setLogin] = useState({ email: '', password: '' });

    return (
        const { handleSubmit } = this.props;
        <div className="App">
            <header className="App-header">
                <form onSubmit={handleSubmit}>
                    <input value={ login.email } onChange={(e) => setLogin(prevState => { ...prevState, email: e.target.value }) } />
                    <input value={ login.password } onChange={(e) => setLogin(prevState => { ...prevState, password: e.target.value }) } />
                    <button type="submit">Submit</button>
                </form>
            </header>
        </div>
    )
}

export default LoginForm;`*
```

*如果您有比这更复杂的状态对象，您可以像在第一个登录示例中那样将它们分解成单独的状态，或者使用 useReducer(我们很快就会看到！).*

*所以我们把州挂在钩子上。组件生命周期方法呢？*

# *使用效果*

*useEffect 是另一个钩子，它在一次调用中处理 componentDidUpdate、componentDidMount 和 componentWillUnmount。例如，如果您需要获取数据，您可以使用 Effect 来这样做，如下所示。*

```
*`import React, { useState, useEffect } from 'react';
import axios from 'axios';
import './App.css';

const HooksExample = () => {
    const [data, setData] = useState();

    useEffect(() => {
        const fetchGithubData = async (name) => {
            const result = await axios(`https://api.github.com/users/${name}/events`)
            setData(result.data)
        }
        fetchGithubData('lsurasani')
    }, [data])

    return (
        <div className="App">
            <header className="App-header">
                {data && (
                    data.map(item => <p>{item.repo.name}</p>)
                )}
            </header>
        </div>
    )
}

export default HooksExample;`*
```

*看看 useEffect，我们看到:*

*   *第一个参数:一个函数。在其中，我们使用异步函数获取数据，然后在得到结果时设置 ****数据**** 。*
*   *第二个参数:包含 ****数据**** 的数组。这定义了组件更新的时间。正如我之前提到的，useEffect 在 componentDidMount、componentWillUnmount、 **和** componentDidUpdate 正常运行时运行。在第一个参数中，我们设置了一些状态，这通常会导致 componentDidUpdate 运行。因此，如果我们没有这个数组，useEffect 将再次运行。现在，useEffect 将在 componentDidMount、componentWillUnmount 上运行，如果 ****数据**** 被更新，componentDidUpdate。该参数可以为空，您可以选择传入一个空数组。在这种情况下，只有 componentDidMount 和 componentWillUnmount 会触发。但是，如果你在里面设置了一些状态，你必须指定这个参数。*

# *useReducer*

*对于使用 Redux 的人来说，useReducer 可能很熟悉。useReducer 接受两个参数—一个 ****缩减器**** 和一个 ****初始状态**** 。缩减器是一个您可以定义的函数，它接受当前状态和“动作”。动作有一个类型，reducer 使用一个 switch 语句根据类型决定执行哪个块。当它找到正确的块时，它返回状态，但是根据类型进行修改。我们可以将这个 reducer 传递给 useReducer，然后像这样使用这个钩子:*

*`const [ state, dispatch ] = useReducer(reducer, initialState)`*

*您可以使用 dispatch 来表示您想要执行的操作类型，如下所示:*

*`dispatch({ type: name})`*

*useReducer 通常用于管理复杂的状态，例如下面的注册表单。*

```
*`import React, { useReducer } from 'react';

const reducer = (state, action) => {
    switch (action.type) {
        case 'firstName': {
            return { ...state, firstName: action.value };
            }
        case 'lastName': {
            return { ...state, lastName: action.value };
            }
        case 'email': {
            return { ...state, email: action.value };
            }
        case 'password': {
            return { ...state, password: action.value };
            }
        case 'confirmPassword': {
            return { ...state, confirmPassword: action.value };
            }
        default: {
            return state;
        }
    }
};

function SignupForm() {
    const initialState = {
        firstName: '',
        lastName: '',
        email: '',
        password: '',
        confirmPassword: '',
    }
    const [formElements, dispatch] = useReducer(reducer, initialState);

    return (
        <div className="App">
            <header className="App-header">
                <div>
                    <input placeholder="First Name" value={ formElements.firstName} onChange={(e) => dispatch({ type: firstName, value: e.target.value }) } />
                    <input placeholder="Last Name" value={ formElements.lastName} onChange={(e) => dispatch({ type: lastName, value: e.target.value }) } />
                    <input placeholder="Email" value={ formElements.email} onChange={(e) => dispatch({ type: email, value: e.target.value }) } />
                    <input placeholder="Password" value={ formElements.password} onChange={(e) => dispatch({ type: password, value: e.target.value }) } />
                    <input placeholder="Confirm Password" value={ formElements.confirmPassword} onChange={(e) => dispatch({ type: confirmPassword, value: e.target.value }) } />
                </div>
            </header>
        </div>
    );
}

export default SignupForm;`* 
```

*这个钩子有很多额外的应用，包括允许我们在整个应用中指定一些 reducers，然后在我们的每个组件中重用它们，根据这些组件中发生的事情进行更改。在高层次上，这类似于 Redux 的功能——因此对于相对简单的应用程序，我们可以避免使用 Redux。*

# *定制挂钩*

*我们已经讨论了 3 个基本的钩子，让我们看看如何制作自己的钩子。还记得我前面提到的登录表单的例子吗？这里再次提醒你:*

```
*`import './App.css';
import React, { useState } from 'react';

const LoginForm = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    return (
        const { handleSubmit } = this.props;
        <div className="App">
            <header className="App-header">
                <form onSubmit={handleSubmit}>
                    <input value={ email } onChange={(e) => setEmail(e.target.value) } />
                    <input value={ password } onChange={(e) => setPassword(e.target.value) } />
                    <button type="submit">Submit</button>
                </form>
            </header>
        </div>
    )
}

export default LoginForm;`*
```

*我们对两者都使用 State，并为两个字段定义一个状态变量和一个更新函数。如果我们能进一步简化呢？这里有一个用于处理任何类型的输入值更改的定制钩子(注意:命名定制钩子的约定是:使用<function description="">)。</function>*

```
*`import { useState } from 'react';

export const useInputValue = (initial) => {
    const [value, setValue] = useState(initial)
    return { value, onChange: e => setValue(e.target.value) }
}`*
```

*我们使用 useState 来处理更改，就像我们在前面的例子中做的那样，但是这次我们返回值和一个 onChange 函数来更新该值。因此，登录表单现在看起来像这样:*

```
*`import React from 'react';
import { useInputValue } from './Custom'

const Form = () => {
    const email = useInputValue('')
    const password = useInputValue('')

    return (
        <div className="App">
            <header className="App-header">
                <div>
                    <input type="text" placeholder="Email" {...email} />
                </div>
                <div>
                    <input type="password" placeholder="Password" {...password} />
                </div>
            </header>
        </div>
    );
}

export default Form;`*
```

*我们用一个空字符串为两个字段初始化 useInputValue，并将结果设置为字段的名称。我们可以把它放回 input 元素中，这样 input 元素就可以动态地呈现值和 onChange 函数。*

*现在，我们已经使这个表单变得更加简单——我们的定制钩子可以在我们需要表单输入元素的任何地方重用！*

*我认为这是钩子最有用的地方之一——能够自己制作并允许以前锁定在每个组件内部的有状态逻辑被取出并重用，允许每个组件变得更简单。*

*所以我们已经过了:useState，useEffect，useReducer，最后是自定义钩子。有一些基本的东西我们还没有复习，也就是说，钩子要遵循的两个一般规则:*

1.  *****只调用 **顶层********—**不在循环、嵌套函数、条件等中。这确保了钩子在每次渲染后总是以相同的顺序被调用。这很重要，因为 React 依赖钩子的调用顺序来确定哪个状态对应于一个 useState 调用(如果使用多个)。如果你的一个钩子隐藏在一个循环、嵌套函数或者条件中，那么顺序会随着渲染而改变，从而混淆了哪个状态对应于哪个使用状态。*
2.  *****只从 React 函数调用钩子或者自定义钩子****——换句话说，不要从 JavaScript 函数调用钩子。*

*希望这能为你弄清楚如何以及何时使用钩子！您可以查看一些其他资源:*

*   *[反应文件](https://reactjs.org/docs/hooks-intro.html)*
*   *[钩子资源集合](https://github.com/rehooks/awesome-react-hooks)*

*如果您有任何问题/意见，请随时在下面提问！*