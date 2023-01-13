# 类型检查 React、Redux 和 React-Redux with Flow 的综合指南

> 原文：<https://www.freecodecamp.org/news/a-comprehensive-guide-to-type-checking-react-redux-and-react-redux-with-flow-4219bdc4ef89/>

作者:费边·泰尔

# 类型检查 React、Redux 和 React-Redux with Flow 的综合指南

![LwOjDA5I0tNxHZPOuhTS9abq4Bc3FxMr1SJQ](img/2869c88f1306e8e4ad088e2500d79ca1.png)

本文分为 4 个部分:

1.  类型检查还原操作、操作创建者和还原者
2.  安装流库定义
3.  类型检查应用程序状态
4.  类型检查冗余存储和调度

虽然在第一部分有一堆非常有用的指南，但我发现只有少量关于第三和第四部分的文章。在长时间的 Google 搜索、钻研源代码、反复试验之后，我决定将我所学到的东西放在一起，编写本教程，作为使用 Flow 对 React + Redux + React-Redux 应用程序进行类型检查的一站式指南。

### 1.类型检查还原操作、操作创建者和还原者

#### 行动

Redux 动作本质上是普通的 Javascript 对象，带有强制的`type`属性:

```
// This is an action{  type: 'INCREASE_COUNTER',  increment: 1}
```

遵循最佳实践，您可能想要定义并使用[动作类型常量](https://redux.js.org/basics/actions)来代替。如果是这样的话，上面的代码片段可能看起来像这样:

```
const INCREASE_COUNTER = 'INCREASE_COUNTER';
```

```
// This is an action{  type: INCREASE_COUNTER,  increment: 1}
```

类型检查很容易(我们在这里处理的是普通的 JavaScript):

```
type $action = {  type: 'INCREASE_COUNTER',  increment: number};
```

请注意，您不能用常量`INCREASE_COUNTER`替换[文字字符串类型](https://flow.org/en/docs/types/literals/)。这是流量本身的一个[限制](https://flow.org/try/#0PTACDMBsHsHcCh6QKYBcAEAjAhgJ3QLzoDkOuxA3EmuuNNAFxZ6ElmVA)。

#### 动作创建者

因为动作创建者只是返回动作的函数，所以我们仍然在处理普通的 Javascript。这是类型检查操作创建者的样子:

```
function increaseCounter(by: number): $action {  return {    type: INCREASE_COUNTER, // it's okay to use the constant here    increment: by  };}
```

#### 还原剂

减速器是*处理动作*的功能。它们接收一个状态和一个动作，并返回新的状态。在这个节骨眼上，重要的是要想好你的州会是什么样子(州形)。在这个非常简单的示例中，状态形状只包含一个键`counter`，它采用一个`number`类型的值:

```
// State shape{  counter: <number>}
```

所以你的减速器看起来像这样:

```
const initialState = { counter: 0 };
```

```
function counter(state = initialState, action) {  switch (action.type) {    case INCREASE_COUNTER:      return Object.assign({}, state, {        counter: action.increment + state.counter      });        default:      return state;  }};
```

*注意:在这个特殊的例子中，* `Object.assign({}, state, { ... })` *是多余的，因为存储只包含一个键/值对。我可以很容易地将最后一个参数返回给函数。然而，为了正确起见，我包含了完整的实现。*

键入状态和缩减器非常简单。下面是上面代码片段的版本:

```
type $state = {  +counter: number};
```

```
const initialState: $state = { counter: 0 };
```

```
function counter(  state: $state = initialState,  action: $action): $state {    switch (action.type) {    case INCREASE_COUNTER:      return Object.assign({}, state, {        counter: action.increment + state.counter      });        default:      return state;  }};
```

### 安装流库定义

流[库定义](https://flow.org/en/docs/libdefs/)(或 libdefs)为第三方模块提供类型定义。在这种情况下，我们使用 React、Redux 和 React-Redux。您可以使用`flow-typed`安装它们的类型定义，而不是手动输入这些模块及其函数:

```
npm install -g flow-typed
```

```
// Automatically download and install all relevant libdefsflow-typed install
```

```
// Orflow-typed install <package>@<version> // e.g. redux@4.0.0
```

库定义被安装到`flow-typed`文件夹中，这使得流可以开箱即用，无需任何进一步的配置([细节](https://github.com/flowtype/flow-typed/wiki/Importing-And-Using-Type-Definitions))。

### 类型检查应用程序状态

之前，我们已经输入了这样的状态:

```
type $state = {  +counter: number};
```

虽然这对于像上面这样的简单例子是有效的，但是一旦你的状态变得非常大，它就失效了。每次引入新的减速器或修改现有减速器时，您都必须手动编辑`type $state`。你也不希望将所有的 reducers 保存在同一个文件中。相反，您想要做的是将 reducers 重构为单独的模块，并使用 Redux 的`combineReducers`函数。

由于本文的重点是在 React/Redux/React-Redux 应用程序的*类型检查*上，而不是构建一个应用程序，所以我假设您熟悉`combineReducers`函数。如果没有，去 [Redux 的教程](https://redux.js.org/basics/reducers)了解一下吧。

假设我们在一个单独的模块中引入一个新的 action/reducer 对:

```
// playSong.js
```

```
export const PLAY_SONG = 'PLAY_SONG';
```

```
// Typing the actionexport type $playSongAction = {  type: 'PLAY_SONG',  song: ?string};
```

```
// Typing the action creatorexport function playSong(song: ?string): $playSongAction {  return {    type: PLAY_SONG,    song: song  };};
```

```
// Typing arg1 and the return value of the reducer [*1]export type $song = ?string;
```

```
// Typing the state [*1]export type $songState = {  +song: $song};
```

```
// [*1][*2]const initialValue: $song = null;
```

```
// Typing the reducer [*1][*3]function song(  state: $song = initialValue,  action: $playSongAction): $song {    switch (action.type) {    case PLAY_SONG:      return action.song;        default:      return state;  }};
```

[*1]:如果我们使用的是`combineReducers`函数，需要注意的是**你的减速器不应该再返回状态，而是将*值返回到状态*** 中的键。在这方面，我认为 [Redux 的教程](https://redux.js.org/basics/reducers)有点缺乏清晰度，因为它没有明确地说明这一点，尽管从示例代码片段中可以清楚地看到。

[*2]:减速器不允许返回`undefined`，只好退而求其次`null`。

[*3]:由于 reducer 不再以`{ song: string }`的形式接收和返回状态，而是将*值*发送给状态对象中的`song`键，我们需要将其第一个参数和返回值的类型从`$songState`更改为`$song`。

我们也修改和重构了`increaseCounter`:

```
// increaseCounter.js
```

```
export const INCREASE_COUNTER = 'INCREASE_COUNTER';
```

```
export type $increaseCounterAction = {  type: 'INCREASE_COUNTER',  increment: number};
```

```
export function increaseCounter(by: number): $action {  return {    type: INCREASE_COUNTER,    increment: by  };};
```

```
export type $counter = number;
```

```
export type $counterState = {  +counter: $counter};
```

```
const initialValue: $counter = 0;
```

```
function counter(  state: $counter = initialValue,  action: $increaseCounterAction): $counter {    switch (action.type) {    case INCREASE_COUNTER:      return action.increment + state;        default:      return state;  }};
```

现在我们有两个动作/减速器对。

我们可以创建一个新的`State`类型来存储我们的应用程序状态的类型:

```
export type State = $songState & $counterState;
```

这是一个流量[交集类型](https://flow.org/en/docs/types/intersections/)，相当于:

```
export type State = {  song: $song,  counter: $counter};
```

如果你不想创建`$songState`和`$counterState`只用于应用程序状态`State`的交集输入，那也很好——使用第二个实现。

### 类型检查冗余存储和调度

我发现 Flow 在我的容器中报告错误(在[容器/组件范例](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)的上下文中)。

```
Could not decide which case to select. Since case 3 [1] may work but if it doesn't case 6 [2] looks promising too. To fix add a type annotation to dispatch [3].
```

这是关于我的`mapDispatchToProps`功能。案例 3 和案例 6 如下:

```
// flow-typed/npm/react-redux_v5.x.x.js
```

```
// Case 3declare export function connect<  Com: ComponentType<*>,  A,  S: Object,  DP: Object,  SP: Object,  RSP: Object,  RDP: Object,  CP: $Diff<$Diff<ElementConfig<Com>;, RSP>, RDP>  >(  mapStateToProps: MapStateToProps<S, SP, RSP>,  mapDispatchToProps: MapDispatchToProps<A, DP, RDP>): (component: Com) => ComponentType<CP & SP & DP>;
```

```
// Case 6declare export function connect<  Com: ComponentType<*>,  S: Object,  SP: Object,  RSP: Object,  MDP: Object,  CP: $Diff<ElementConfig<Com>, RSP>  >(  mapStateToProps: MapStateToProps<S, SP, RSP>,  mapDispatchToPRops: MDP): (component: Com) => ComponentType<$Diff<CP, MDP> & SP>;
```

**不知道为什么会出现这个错误。**但是正如错误提示的那样，输入`dispatch`就可以修复它。如果我们输入`dispatch`，我们也可以输入`store`。

我找不到太多关于输入 Redux/React-Redux 应用程序的文档。我通过钻研 libdefs 并查看其他项目(尽管是一个演示项目)的[源代码来学习。如果你有任何见解，请让我知道，这样我就可以更新这个帖子(当然，要有正确的归属)。](https://github.com/reduxjs/redux/tree/master/examples/todos-flow/src)

与此同时，我发现这很有效:

```
import type {  Store as ReduxStore,  Dispatch as ReduxDispatch} from 'redux';
```

```
// import any other variables and types you may need,// depending on how you organized your file structure.
```

```
// Reproduced from earlier onexport type State = {  song: $song,  counter: $counter};
```

```
export type Action =   | $playSongAction  | $increaseCounterAction
```

```
export type Store = ReduxStore<State, Action>;
```

```
export type Dispatch = ReduxDispatch<Action>;
```

进入您的容器模块，然后您可以继续输入如下的`mapDispatchToProps`:`const mapDispatchToProps = (dispatch: Dispatch) => { ...`}；

### 包扎

这是一篇相当长的文章，我希望你觉得有用。我写这篇文章部分是因为缺乏关于本文后面部分的资源(部分是为了组织我的想法和巩固我所学到的)。

我不能保证本文中的指导遵循了最佳实践，或者在概念上是合理的，甚至是 100%正确的。如果您发现任何错误或问题，请让我知道！