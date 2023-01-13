# 将 Typescript 与 React 一起使用的最佳实践

> 原文：<https://www.freecodecamp.org/news/effective-use-of-typescript-with-react-3a1389b6072a/>

克里斯托弗·迪金斯

# 将 Typescript 与 React 一起使用的最佳实践

![NWzcNzdUlOZrJ8FguMaiBqh5RT7LWNB3a79u](img/180b28e62da3206e259fd69eed4e73dd.png)

Type theory for the win!!!

有许多工具和教程可以帮助开发人员开始用 TypeScript 编写简单的 React 应用程序。然而，在更大的 React 应用程序中使用 TypeScript 的最佳实践并不清楚。

当与第三方库的生态系统集成时尤其如此，这些库用于解决诸如主题化、样式化、国际化、日志记录、异步通信、状态管理和表单管理等问题。

在 [Clemex](http://www.clemex.com) ，我们开发计算显微镜应用。我们最近为我们的一个应用程序从 JavaScript 迁移了一个 React 前端到 TypeScript。总的来说，我们对最终结果非常满意。共识是我们的代码库现在更容易理解和维护。

也就是说，我们的过渡并非没有一些挑战。这篇文章深入探讨了我们面临的一些挑战，以及我们是如何克服这些挑战的。

挑战主要与理解 React API 的类型签名有关，尤其是那些高阶组件。我们如何正确地解决类型错误，同时保留 TypeScript 的优点？

本文试图解决如何最有效地将 TypeScript 与 React 以及支持库的生态系统结合使用。我们还将解决一些常见的混淆领域。

### 查找库的类型定义

TypeScript 程序可以很容易地导入任何 JavaScript 库。但是如果没有对导入的值和函数的类型声明，我们就不能获得使用 TypeScript 的全部好处。

幸运的是，TypeScript 使得以[类型声明文件](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)的形式定义 JavaScript 库的类型注释变得很容易。

如今只有少数项目直接随项目提供 TypeScript 类型定义。然而，对于许多库，您通常可以在`@types` organization 名称空间中找到最新的类型定义文件。

例如，如果您查看 TypeScript React 模板`package.json`，您可以看到我们使用了以下类型定义文件:

```
"@types/jest": "^22.0.1","@types/node": "^9.3.0","@types/react": "^16.0.34","@types/react-dom": "^16.0.3","@types/redux-logger": "^3.0.5"
```

使用外部类型声明的唯一缺点是，追踪由于版本不匹配导致的错误，或者类型声明文件本身的细微错误，可能会有点麻烦。最初的库作者并不总是支持类型声明文件。

### 属性和状态字段的编译时验证

在 React 应用程序中使用 TypeScript 的一个主要优点是，编译器(以及 IDE，如果配置正确的话)可以验证提供给组件的所有必要属性。

它还可以检查它们的类型是否正确。这取代了由`[prop-types](https://www.npmjs.com/package/prop-types)`库提供的运行时验证的需要。

下面是一个具有两个必需属性的组件的简单示例:

```
import * as React from ‘react’;
```

```
export interface CounterDisplayProps {  value: number;  label: string;}
```

```
export class CounterDisplay extends React.PureComponent<CounterDisplayProps> {   render(): React.ReactNode {   return (     <div>       The value of {this.props.label} is {this.props.value}      </div>    );}
```

### 作为类或函数的组件

有了 React，你可以用两种方式定义一个新的组件:作为一个函数或者作为一个类。这两种组件的类型是:

1.  组件类::`React.ComponentClass<`；P >
2.  无状态功能组件(SFC)::`React.StatelessComponent<`；P >

#### 组件类别

对于 C++/C#/Java 背景的开发人员来说，类类型是一个新概念。一个类有一个特殊的类型，它独立于类的实例类型。它是根据构造函数定义的。理解这一点是理解类型签名和一些可能出现的类型错误的关键。

一个`ComponentClass`是一个构造函数的类型，它返回一个对象，这个对象是一个`Component`的实例。省略了一些细节，`ComponentClass`类型定义的本质是:

```
interface ComponentClass<P = {}> {  new (props: P, context?: any): Component<P, ComponentState>;}
```

#### 无状态组件(SFC)

一个`StatelessComponent`(也称为`SFC`)是一个函数，它接受一个属性对象、可选的子组件列表和可选的上下文对象。它返回一个`ReactElement`或`null`。

不管名称暗示什么，一个`StatelessComponent`和一个`Component`类型没有关系。

一个`StatelessComponent`和`SFC`别名的类型定义的简化版本是:

```
interface StatelessComponent<P = {}> {  (props: P & { children?: ReactNode }, context?: any):   ReactElement<any> | null;}
```

```
type SFC<P = {}> = StatelessComponent<P>;
```

在 React 16 之前，sfc 非常慢。[显然这已经随着 React 16](https://medium.com/missive-app/45-faster-react-functional-components-now-3509a668e69f) 得到了改善。然而，由于我们的代码库需要一致的编码风格，我们继续将组件定义为类。

#### 纯组分和非纯组分

`Component`有两种不同的类型:纯种和非纯种。

术语“纯”在 React 框架中有非常特殊的含义，与计算机科学中的术语无关。

一个`PureComponent`是一个组件，它提供了一个`shouldComponentUpdate`函数的默认实现(它对`this.state`和`this.props`做了一个简单的比较)。

与常见的误解相反，a `StatelessComponent`不是纯的，a `PureComponent`可能有状态。

#### 有状态组件可以(也应该)从 React 派生。纯组件

如上所述，根据 React 的术语，具有状态的 React 组件仍然可以被视为`Pure component`。事实上，从`React.PureComponent.`派生具有内部状态的组件是一个好主意

以下内容基于 [Piotr Witek 的流行打字指南](https://github.com/piotrwitek/react-redux-typescript-guide)，但有以下小的修改:

1.  根据 React 文档，`setState`函数使用一个回调函数根据之前的状态更新状态。
2.  我们从`React.PureComponent`派生，因为它没有覆盖生命周期功能
3.  类型被定义为一个类，这样它就可以有一个初始化器。
4.  我们不在渲染函数中给局部变量分配属性，因为这违反了 DRY 原则，并且增加了不必要的代码行。

```
import * as React from ‘react’;export interface StatefulCounterProps {  label: string;}
```

```
// By making state a class we can define default values.class StatefulCounterState {  readonly count: number = 0;};
```

```
// A stateful counter can be a React.PureComponentexport class StatefulCounter  extends React.PureComponent<StatefulCounterProps, StatefulCounterState>{  // Define  readonly state = new State();
```

```
 // Callbacks should be defined as readonly fields initialized with arrow functions, so you don’t have to bind them  // Note that setting the state based on previous state is done using a callback.  readonly handleIncrement = () => {    this.setState((prevState) => {       count: prevState.count + 1 } as StatefulCounterState);  }
```

```
 // We explicitly include the return type  render(): React.ReactNode {    return (      <div>        <span>{this.props.label}: {this.props.count} </span>        <button type=”button” onClick={this.handleIncrement}>           {`Increment`}        </button>      </div>     );  }}
```

#### React 无状态功能组件不是纯粹的组件

尽管有一个常见的误解，无状态功能组件(SFC)并不是纯粹的组件，这意味着无论属性是否改变，它们每次都会被呈现。

### 键入高阶组件

React 应用程序使用的许多库都提供了接受组件定义并返回新组件定义的函数。这些被称为**高阶元件**(或简称为 **HOC** s)。

高阶组件可能返回一个`StatelessComponent`或一个`ComponentClass`，这取决于它是如何定义的。

#### 出口违约的困惑

JavaScript React 应用程序中的一个常见模式是定义一个组件，用一个特定的名称(比如说`MyComponent`)将它保存在一个模块的本地。然后，默认导出用一个或多个 HOC 包装它的结果。

匿名组件作为`MyComponent`导入整个应用程序。这是一种误导，因为程序员在两个完全不同的事情上重复使用同一个名字！

为了提供正确的类型，我们需要认识到从一个高阶组件返回的组件通常与文件中定义的组件不是同一类型。

在我们的团队中，我们发现为保存在文件本地的已定义组件提供名称(例如`MyComponentBase`)和用导出组件显式命名常量(例如`export const MyComponent = injectIntl(MyComponentBase);`)都很有用。

除了更加明确之外，这还避免了定义别名的问题，这使得理解和重构代码更加容易。

#### 注入属性的 hoc

大多数 hoc 将不需要由组件消费者提供的属性注入到组件中。我们在应用中使用的一些示例包括:

*   来自物料界面:`withStyles`
*   来自 redux-form: `reduxForm`
*   来自 react-intl: `injectIntl`
*   来自 react-redux: `connect`

#### 内部、外部和注入属性

为了更好地理解从特设函数返回的组件和定义的组件之间的关系，请尝试以下有用的心理模型:

将预期由组件的客户端提供的属性视为*外部属性*，将组件定义可见的所有属性(例如，render 函数中使用的属性)视为*内部属性*。这两组属性的区别在于*注入属性*。

#### 类型交集运算符

在 TypeScript 中，我们可以使用一个称为[交集操作符](http://www.typescriptlang.org/docs/handbook/advanced-types.html) ( `&`)的类型级操作符，按照我们想要的方式为属性组合类型。交集运算符将一种类型的字段与另一种类型的字段组合在一起。

```
interface LabelProp {  label: string;}
```

```
interface ValueProp {  value: number;}
```

```
// Has both a label field and a value fieldtype LabeledValueProp = LabelProp & ValueProp;
```

对于那些熟悉集合论的人来说，你可能想知道为什么这不被认为是一个联合运算符。这是因为它是满足两个类型约束的所有可能值的集合的交集。

### 定义包装组件的属性

当定义一个将被一个更高阶组件包装的组件时，我们必须向基本类型提供内部属性(例如`React.PureComponent<`；P >。

然而，我们不希望在一个导出的接口中定义所有这些内容，因为这些属性与组件的客户端无关:它们只需要外部属性。

为了最大限度地减少样板文件和重复，我们选择使用交集操作符，在我们需要引用内部属性类型的时候，我们将它作为泛型参数传递给基类。

```
interface MyProperties {  value: number;}
```

```
class MyComponentBase extends React.PureComponent<MyProperties & InjectedIntlProps> {  // Now has intl as a property  // ...}
```

```
export const MyComponent = injectIntl(MyComponentBase); // Has the type React.Component<MyProperties>;
```

### React-Redux 连接函数

React-Redux 库的`connect`函数用于从 Redux 存储中检索组件所需的属性，并将一些回调映射到 dispatcher(它触发触发存储更新的动作)。

因此，忽略可选的合并函数参数，我们可能有两个要连接的参数:

1.  `mapStateToProps`
2.  `mapDispatchToProps`

这两个函数都为组件定义提供了自己的内部属性子集。

然而，`connect`的类型签名是一个特例，这是因为该类型的编写方式。它可以推断注入的属性的类型，也可以推断剩余的属性的类型。

这给我们留下了两个选择:

1.  我们可以将接口分成`mapStateToProps`的内部属性和`mapDispatchToProps`的另一个内部属性。
2.  我们可以让类型系统为我们推断类型。

在我们的例子中，我们必须将大约 50 个*连接的*组件从 JavaScript 转换成 TypeScript。

他们已经有了从原始 PropTypes 定义生成的正式接口(感谢我们从 Lyft 使用的一个[开源工具)。](https://github.com/lyft/react-javascript-to-typescript-transform)

将这些接口分成外部属性、映射的状态属性和映射的分派属性的价值似乎并没有超过成本。

最后，正确地使用`connect`允许客户端正确地推断类型。我们现在很满意，但可能会重新考虑选择。

#### 帮助 React-Redux 连接函数推断类型

TypeScript 类型的推理引擎有时似乎需要一点微妙的东西。`connect`函数似乎就是其中之一。现在希望这不是一个货物邪教编程的情况，但这里是我们采取的步骤，以确保编译器可以工作的类型。

*   我们没有给`mapStateToProps`或`mapDispatchToProps`函数提供类型，我们只是让编译器推断它们。
*   我们将`mapStateToProps`和`mapDispatchToProps`定义为分配给`const`变量的箭头函数。
*   我们使用`connect`作为最外层的高阶元件。
*   我们不使用`compose`函数来组合多个高阶分量。

在`mapStateToProps`和`mapDispatchToProps`中连接到存储的属性不能被声明为可选的，否则你会在推断的类型中得到类型错误。

### 最后的话

最后，我们发现使用 TypeScript 使我们的应用程序更容易理解。它有助于加深我们对 React 和我们自己的应用程序架构的理解。

在为扩展 React 而设计的附加库的上下文中正确使用 TypeScript 需要额外的努力，但绝对值得。

如果您刚刚开始使用 React 中的 TypeScript，下面的指南会很有用:

*   [微软 TypeScript React Starter](https://github.com/Microsoft/TypeScript-React-Starter)
*   [微软的 TypeScript React 转换指南](https://github.com/Microsoft/TypeScript-React-Conversion-Guide)
*   [Lyft 的 React JavaScript to TypeScript 转换器](https://github.com/lyft/react-javascript-to-typescript-transform)

之后，我建议通读以下文章:

*   James Ravenscroft 的 TypeScript 中的 React 高阶组件模式
*   Piotr Witek 的 React-Redux 打字指南

#### 承认

非常感谢 Clemex 团队成员在本文中的合作，他们共同努力找出如何在 React 应用程序中最大限度地使用 TypeScript，并在 GitHub 上开发了开源的 [TypeScript React 模板项目](https://github.com/Clemex/typescript-react-template)。