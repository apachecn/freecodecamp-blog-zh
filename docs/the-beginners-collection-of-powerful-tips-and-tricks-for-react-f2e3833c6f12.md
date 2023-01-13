# 初学者收集的 React 的强大提示和技巧

> 原文：<https://www.freecodecamp.org/news/the-beginners-collection-of-powerful-tips-and-tricks-for-react-f2e3833c6f12/>

> 这是我的“React 初学者”系列文章的一部分，介绍 React、它的核心特性和应该遵循的最佳实践。更多文章来了！

> [< <开始超过](https://www.freecodecamp.org/news/p/2994c09b-d550-4eb6-b281-a83e553240c7/) | [<先前的](https://www.freecodecamp.org/news/how-to-bring-reactivity-into-react-with-states-exclude-redux-solution-4827d293dfc4/)

从这篇文章的标题可以看出，它是针对初学者的。

其实几个月前就开始学 React 了。阅读 React 文档、开源项目和 Medium 文章对我帮助很大。

毫无疑问，我不是反应专家。所以我读了很多关于这个话题的书。此外，建立一个小项目帮助我更好地了解 React。一路走来，我采纳了一些最佳实践——我想在这里与您分享。所以让我们开始吧。

### 命名您的组件

要找出哪个组件有缺陷，给组件起个名字很重要。

当您开始使用 React 路由器或第三方库时更是如此。

```
// Avoid thoses notations 
export default () => {};
export default class extends React.Component {};
```

关于是使用默认导入还是命名导入存在争议。注意，**默认导入**不能确保组件的名称在项目中是一致的。再说，[撼树](https://en.wikipedia.org/wiki/Tree_shaking)就没那么有效了。

#### 无论您如何公开您的组件，请命名它

您需要定义承载组件的类名或变量名(对于功能组件)。

React 实际上会从错误消息中推断出组件名。

```
export const Component = () => <h1>I'm a component</h1>;
export default Component;

// Define user custom component name
Component.displayName = 'My Component';
```

这里是我关于进口的最后一条建议(摘自 [here](https://medium.freecodecamp.org/the-most-important-eslint-rule-for-redux-applications-c10f6aeff61d) ):如果你使用 ESLint，你应该考虑设置以下两条规则:

```
"rules": {
    // Check named import exists
    "import/named": 2, 

    // Set to "on" in airbnb preset
    "import/prefer-default-export": "off"
}
```

### 首选功能组件

如果您有许多组件的目的只是显示数据，那么利用多种方式[来定义一个 React 组件](https://reactjs.org/docs/components-and-props.html#functional-and-class-components):

```
class Watch extends React.Component {
  render () {
    return <div>{this.props.hours}:{this.props.minutes}</div>
  }
}

// Equivalent functional component
const Watch = (props) =>
  <div>{props.hours}:{props.minutes}</div>;
```

两个片段定义了同一个`Watch`组件。然而，第二种方式要短得多，甚至会丢弃`this`来访问 JSX 模板中的道具。

### 用片段替换 div

每个组件必须公开一个唯一的根元素作为模板。为了遵守这一规则，常见的解决方法是将模板包装在一个`div`中。

React 16 给我们带来了一个新功能叫做 [*碎片*](https://reactjs.org/docs/fragments.html) 。现在你可以把那些没用的`div`换成`React.Fragment`的*。*

输出模板将是没有任何包装的片段内容。

```
const Login = () => 
  <div><input name="login"/><input name="password"/></div>;

const Login = () =>
  <React.Fragment><input name="login"/><input name="password"/></React.Fragment>;

const Login = () => // Short-hand syntax
  <><input name="login"/><input name="password"/></>;
```

### 设置状态时要小心

只要 React 应用程序是动态的，就必须处理组件的状态。

使用状态似乎很简单。初始化`constructor`中的状态内容，然后调用`setState`更新状态*。*

出于某种原因，在调用`setState`来设置下一个状态的值时，您可能需要使用当前的**状态**或**道具**的值。

```
// Very bad pratice: do not use this.state and this.props in setState !
this.setState({ answered: !this.state.answered, answer });

// With quite big states: the tempatation becomes bigger 
// Here keep the current state and add answer property
this.setState({ ...this.state, answer });
```

问题是 React 不能确保`this.state`和`this.props`具有您期望的值。`setState`是异步的，因为状态更新是批量的，以优化 DOM 操作(详见 [React 文档](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)和这个[问题](https://github.com/facebook/react/issues/11527#issuecomment-360199710))。

```
// Note the () notation around the object which makes the JS engine
// evaluate as an expression and not as the arrow function block
this.setState((prevState, props) 
              => ({ ...prevState, answer }));
```

为了防止损坏的状态，您必须将`setState`与函数参数一起使用。它提供适当的状态和道具值。

### 绑定组件功能

将一个元素的事件绑定到它的组件有很多方法，有些方法不推荐。

第一个合法的解决方案出现在 [React 文档](https://reactjs.org/docs/handling-events.html)中:

```
class DatePicker extends React.Component {
   handleDateSelected({target}){
     // Do stuff
   }
   render() {   
     return <input type="date" onChange={this.handleDateSelected}/>
   }
 }
```

当你发现它不起作用时，你可能会失望。

原因是当使用 JSX 时，`this`值不绑定到组件实例。这里有三种方法可以让它发挥作用:

```
// #1: use an arrow function
<input type="date" onChange={(event) => this.handleDateSelected(event)}/>

// OR #2: bind this to the function in component constructor
constructor () { 
  this.handleDateSelected = this.handleDateSelected.bind(this); 
}

// OR #3: declare the function as a class field (arrow function syntax)
handleDateSelected = ({target}) => {
   // Do stuff
}
```

像第一个例子一样在 JSX 使用一个箭头函数看起来很吸引人。但是不要这样做。实际上，你的箭头函数会在每个组件渲染时被再次创建[，这会影响性能。](https://reactjs.org/docs/handling-events.html)

另外，要小心最后一个解决方案。它使用 class fields 语法，这只是 ECMAScript 的一个[建议。](https://github.com/tc39/proposal-class-fields)

这意味着你必须使用[巴别塔](https://babeljs.io/docs/plugins/transform-class-properties)到[传送](https://en.wikipedia.org/wiki/Source-to-source_compiler)代码。如果语法最终没有被采用，你的代码将会崩溃。

### 采用容器模式(即使有 Redux)

最后但同样重要的是，容器设计模式。这允许您在 React 组件中遵循[关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns)原则。

```
export class DatePicker extends React.Component {
  state = { currentDate: null };

  handleDateSelected = ({target}) =>
     this.setState({ currentDate: target.value });

  render = () => 
     <input type="date" onChange={this.handleDateSelected}/>
}
```

单个组件在同一位置处理模板呈现和用户操作。让我们用两个组件来代替:

```
const DatePicker = (props) => 
  <input type="date" onChange={props.handleDateSelected}/>

export class DatePickerController extends React.Component { 
  // ... No changes except render function ...
  render = () => 
     <DatePicker handleDateSelected={this.handleDateSelected}/>;
}
```

这是诀窍。必要时处理用户交互和 API 调用。然后它渲染一个`DatePicker`并提供道具。

由于这种模式，容器组件取代了表示组件。这个功能组件没有道具就变得没用了。

```
export const DatePickerContainer = 
 connect(mapStateToProps, mapDispatchToProps)(DatePickerController);
```

此外，如果您使用 Redux 作为应用程序的状态管理器，它也可以很好地适应这种模式。

`connect`函数将道具注入到组件中。在我们的例子中，它将反馈给控制器，控制器将这些道具转发给组件。

因此，两个组件都将能够访问 Redux 数据。下面是容器设计模式的完整代码(没有 Redux 或类字段语法)。

[https://codepen.io/jbardon/embed/preview/oNXOWEy?height=300&slug-hash=oNXOWEy&default-tabs=js,result&host=https://codepen.io](https://codepen.io/jbardon/embed/preview/oNXOWEy?height=300&slug-hash=oNXOWEy&default-tabs=js,result&host=https://codepen.io)

### 奖励:固定道具钻孔

在为 React 编写我的学习项目时，我注意到一个不好的模式，它用道具困扰着我。在每个页面上，我都有一个使用商店的主组件，并呈现一些嵌套的哑组件。

深度嵌套的哑组件如何访问主组件数据？事实上，它们不能——但是您可以通过以下方式解决:

*   将哑组件包装在容器中(它变得智能)
*   或者从顶部组件向下传递道具

第二个解决方案意味着顶层组件和底层组件之间的组件必须传递它们不需要的道具。

```
const Page = props => <UserDetails fullName="John Doe"/>;

const UserDetails = props => 
<section>
    <h1>User details</h1>
    <CustomInput value={props.fullName}/> // <= No need fullName but pass it down
</section>;

const inputStyle = {
   height: '30px',
   width: '200px',
	fontSize: '19px',
   border: 'none',
   borderBottom: '1px solid black'
};
const CustomInput = props => // v Finally use fullName value from Page component
   <input style={inputStyle} type="text" defaultValue={props.value}/>;
```

React 社区将这个问题命名为 **prop drilling** 。

`Page`是加载用户详细信息的主要组件。需要通过`UserDetails`将数据传递给`CustomInput`。

在这个例子中，道具只通过一个不需要它的组件。但是如果你有可重用的组件，它可以更大。例如，脸书代码库包含几千个可重用的组件！

别担心，我会教你三种方法来解决它。前两个方法出现在[上下文 API 文档](https://reactjs.org/docs/context.html#before-you-use-context)中:**子道具**和**渲染道具**。

```
// #1: Use children prop
const UserDetailsWithChildren = props => 
<section>
    <h1>User details (with children)</h1>
    {props.children /* <= use children */} 
</section>;

// #2: Render prop pattern
const UserDetailsWithRenderProp = props => 
<section>
    <h1>User details (with render prop)</h1>
    {props.renderFullName() /* <= use passed render function */}
</section>;

const Page = () => 
<React.Fragment>
    {/* #1: Children prop */}
    <UserDetailsWithChildren>
        <CustomInput value="John Doe"/> {/* Defines props.children */}
    </UserDetailsWithChildren>

    {/* #2: Render prop pattern */}
    {/* Remember: passing arrow functions is a bad pratice, make it a method of Page class instead */}
    <UserDetailsWithRenderProp renderFullName={() => <CustomInput value="John Doe"/>}/>
</React.Fragment>;
```

这些解决方案非常相似。我更喜欢使用 children，因为它在 render 方法中工作得很好。请注意，您还可以通过提供更深层次的嵌套组件来扩展这些模式。

```
const Page = () =>  
<PageContent>
  <RightSection> 
    <BoxContent>
      <UserDetailsWithChildren>
          <CustomInput value="John Doe"/>
      </UserDetailsWithChildren>
    </BoxContent>
  </RightSection>
</PageContent>
```

第三个例子使用实验上下文 API。

```
const UserFullNameContext = React.createContext('userFullName');

const Page = () => 
<UserFullNameContext.Provider value="John Doe"> {/* Fill context with value */}
    <UserDetailsWithContext/>
</UserFullNameContext.Provider>;

const UserDetailsWithContext = () => // No props to provide
<section>
    <h1>User details (with context)</h1>
    <UserFullNameContext.Consumer> {/* Get context value */}
        { fullName => <CustomInput value={fullName}/> }
    </UserFullNameContext.Consumer>
</section>;
```

我不推荐这种方法，因为它使用了一个实验性的特性。(这也是为什么 API 最近[在一个小版本](https://reactjs.org/blog/2018/03/29/react-v-16-3.html)上发生了变化。)此外，它迫使您创建一个全局变量来存储上下文，并且您的组件获得了一个不明确的新依赖项(上下文可以包含任何内容)。

#### 就是这样！

感谢阅读。我希望你学到了一些关于 React 的有趣技巧！

**如果您觉得这篇文章有用，请点击**？**按钮几下，让别人找到文章，以示支持！**？

**别忘了关注我，获取我即将发布的文章的通知**？

> 这是我的“React 初学者”系列文章的一部分，介绍 React、它的核心特性和应该遵循的最佳实践。

> [< <开始超过](https://www.freecodecamp.org/news/p/2994c09b-d550-4eb6-b281-a83e553240c7/) | [<先前的](https://www.freecodecamp.org/news/how-to-bring-reactivity-into-react-with-states-exclude-redux-solution-4827d293dfc4/)

### 查看我的其他文章

#### JavaScript

*   [如何通过编写自己的 Web 开发框架来提高自己的 JavaScript 技能](https://medium.freecodecamp.org/how-to-improve-your-javascript-skills-by-writing-your-own-web-development-framework-eed2226f190)？
*   [使用 Vue.js 时要避免的常见错误](https://medium.freecodecamp.org/common-mistakes-to-avoid-while-working-with-vue-js-10e0b130925b)

#### 提示和技巧

*   [停止痛苦的 JavaScript 调试，用源代码图拥抱 Intellij】](https://medium.com/dailyjs/stop-painful-javascript-debug-and-embrace-intellij-with-source-map-6fe68eda8555)
*   [如何毫不费力地减少庞大的 JavaScript 包](https://medium.com/dailyjs/how-to-reduce-enormous-javascript-bundle-without-efforts-59fe37dd4acd)