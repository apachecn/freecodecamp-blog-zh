# 通过在反应中建立井字游戏来学习推理

> 原文：<https://www.freecodecamp.org/news/learn-reasonml-by-building-tic-tac-toe-in-react-334203dd513c/>

> ***3。7.2018:更新至 reason react v 0 . 4 . 2***

你可能之前听说过[原因](https://reasonml.github.io/)。这是一种在 [OCaml](https://ocaml.org/) 之上的语法，既可以编译成可读的 JavaScript 代码，也可以编译成本机代码和字节码。

这意味着你可以使用 Reason 语法编写一个单独的应用程序，并且可以在浏览器中运行，也可以在 Android 和 iOS 手机上运行。

这也是 Reason(哎哟，双关)越来越受欢迎的原因之一。由于语法的相似性，在 JavaScript 社区中尤其如此。

如果在 Reason 出现之前你是一名 JavaScript 开发人员，并且想学习一门函数式编程(FP)语言，你还必须学习一套全新的语法和规则。这可能让许多人望而却步。

有了理性，你主要需要理解它所基于的 FP 原则——比如不变性、currying、复合和高阶函数。

在我发现 Reason 之前，我试图在 JavaScript 中尽可能多地使用 FP 原则。然而，JavaScript 在这个意义上是有限的，因为它并不是一种 FP 语言。为了有效地利用这些原则，你需要使用一些库来创建复杂的抽象，这些抽象对你来说是隐藏的。

另一方面，Reason 向所有感兴趣的 JavaScript 开发人员开放了整个 FP 领域。它为我们提供了一个机会，使用我们非常熟悉的语法来使用所有这些很酷的 OCaml 特性。

最后但同样重要的是，我们可以使用 Reason 编写我们的 [React](https://reasonml.github.io/reason-react/) 或 [React Native](https://github.com/reasonml-community/bs-react-native) 应用程序。

### 你为什么要试一试理性？

我希望当你读完这篇文章时，你会自己找到答案。

当我们浏览经典井字游戏的源代码时——用 React 用 rational 编写——我将解释这种语言的核心特性。您将看到强类型系统、不变性、模式匹配、使用管道的功能组合等的好处。与 JavaScript 不同，这些特性是 Reason 本身固有的。

### 预热

在动手之前，你需要按照本指南在你的机器上安装 Reason。

之后，你需要设置你的应用程序。为此，你可以克隆包含我们应用程序代码的[我的库](https://github.com/codinglawyer/reason-tic-tac-toe)，或者你可以使用[推理脚本](https://github.com/reasonml-community/reason-scripts)和代码建立自己的项目。

要在浏览器中查看您的应用程序，您需要首先将您的原因文件编译成 JavaScript 文件。 [BuckleScript](https://bucklescript.github.io/) 编译器会处理这个问题。

换句话说，当您运行`npm start`(在 ReasonScripts 项目中)时，您的原因代码被编译成 JavaScript。编译的结果随后呈现给浏览器。你可以通过查看你的应用程序中的`lib`文件夹来了解编译后的代码的可读性。

### 我们的第一个组件

![GBLERj-jp68NeaR1Ray8hXWUIfsItL0Gf8Y1](img/ddf5f0d4b9dd70cc8600600a229d8485.png)

正如我们已经提到的，我们的井字游戏应用程序是使用 [ReasonReact](https://github.com/reasonml/reason-react) 库编写的。这使得 Reason 对于 JavaScript 开发人员来说更容易理解，并且许多新来者都来自这个社区。

我们的应用程序具有经典的组件结构，就像任何其他 React 应用程序一样。当谈到 UI 时，我们将自顶向下地浏览组件，当描述它们的逻辑时，我们将自底向上地浏览组件。

让我们从顶级组件`App`开始。

```
let component = ReasonReact.statelessComponent("App");
let make = _children => {
  ...component,
  render: _self =>
    <div>
       <div className="title">
         (ReasonReact.string("Tic Tac Toe"))
       </div>
       <Game />
    </div>,
};
```

当您调用`ReasonReact.statelessComponent`并将组件名传递给它时，组件就被创建了。你不需要像 React 中那样的任何类关键字，因为 Reason 没有任何关键字。

该组件既不是类也不是函数——它是一个所谓的[记录](https://reasonml.github.io/docs/en/record.html)。`record`是 Reason 的数据结构之一，类似于 JavaScript 对象。然而，与后者不同的是，`record`是不可变的。

我们新的`record`组件包含各种默认属性，比如初始状态、生命周期方法和渲染。为了根据我们的需要调整组件，我们需要覆盖其中的一些属性。我们可以在返回组件的`make`函数中这样做。

由于`record`是不可变的，我们不能通过突变来覆盖它的属性。相反，我们需要返回一个新的`record`。为此，我们需要扩展组件并重新定义我们想要更改的属性。这非常类似于 JavaScript 对象扩展操作符。

由于`App`是一个非常简单的组件，我们只想覆盖默认的`render`方法，这样我们就可以将元素呈现到屏幕上。`render`方法有一个`self`参数，这个参数给了我们访问状态和 reducers 的权限，我们将在后面看到。

由于 ReasonReact 支持 [JSX](https://reactjs.org/docs/introducing-jsx.html) ，我们的`render`函数可以返回 JSX 元素。未大写的元素将被识别为 DOM 元素— `div`。大写的要素将被确认为一个组成部分— `Game`。

由于 Reason 的强类型系统，您不能像在 classic React 中那样简单地将一个字符串传递给一个元素来显示它。

相反，您需要将这样的字符串传递给一个`ReasonReact.string`助手函数，该函数将把它转换成可以呈现的`reactElement`。

因为这有点冗长，而且我们会经常使用这个助手，所以让我们将它存储在一个`toString`变量中。按理说，您可以只使用`let`关键字来做到这一点。

```
let toString = ReasonReact.string;
```

在进一步讨论之前，让我们先讨论一下`make`函数的参数。因为我们没有向`App`组件传递任何道具，所以它只接受默认的`children`参数。

然而，我们并没有使用它。我们可以通过在它前面写一个下划线来明确这一点。如果我们没有这样做，编译器会警告我们参数没有被使用。我们对`render`方法中的`self`参数做了同样的处理。

与 JavaScript 相比，可理解的错误和警告消息是另一个很酷的特性，可以改善开发人员的体验。

### 设置变体类型

![BH-VorOdK3kCiMmNYWKwfWWeSwUFBLKgYI1m](img/ff37143c039ef0b23e82d1b928e32b1e.png)

在深入应用程序本身之前，我们将首先定义我们的类型。

理性是一种静态类型的语言。这意味着它在编译时评估我们的值的类型。换句话说，你不需要运行你的应用程序来检查你的类型是否正确。这也意味着你的编辑器可以为你提供[有用的编辑支持](https://github.com/reasonml-editor/vscode-reasonml)。

然而，拥有类型系统并不意味着您需要为所有的值显式定义类型。如果你决定不这样做，理性会为你找出(推断出)类型。

我们将利用类型系统来定义我们将在整个应用程序中使用的类型。这将迫使我们在编码之前考虑应用程序的结构，我们将得到一份代码文档作为奖励。

如果你有过使用[类型脚本](https://www.typescriptlang.org/)或[流程](https://flow.org/)的经验，那么原因类型应该看起来很熟悉。但是，与这两个库不同，你根本不需要任何之前的配置(我看的是你的 Typescript)。类型是现成可用的。

按理说，我们可以区分[类型](https://reasonml.github.io/docs/en/type.html)和[变体类型](https://reasonml.github.io/docs/en/variant.html)(简称变体)。例如，类型有`bool`、`string`和`int`。另一方面，变体更复杂。可以把它们看作是可枚举的值集——或者更准确地说，是构造函数。变体可以通过模式匹配来处理，我们将在后面看到。

```
type player =
  | Cross
  | Circle;

type field =
  | Empty
  | Marked(player);
```

这里我们定义`player`和`field` **的变体**。当定义一个变量时，你需要使用一个`type`关键字。

因为我们正在构建一个井字游戏，我们需要两个玩家。因此，`player`类型将有两个可能的构造函数— `Cross`和`Circle`。

如果我们考虑一下棋盘，我们知道每个`field`类型可以有两个可能的构造器——或者是由其中一个玩家创建的`Empty`或者是`Marked`。

如果你看一下`Marked`构造函数，你会发现我们把它作为一个数据结构。我们使用一个变量来保存另一段数据。在我们的例子中，我们传递给它的是`player`变量。这种行为非常强大，因为它使我们能够将不同的变体和类型组合在一起，以创建更复杂的类型。

所以，我们有了`field`变体。然而，我们需要定义由多行字段组成的整个棋盘。

```
type row = list(field);
type board = list(row);
```

每个`row`是一个`field`列表，播放的`board`由一个`row`列表组成

`list`是 Reason 的数据结构之一——类似于 JavaScript 数组。不同的是，它是不可改变的。Reason 也有一个`array`作为可变的固定长度列表。我们稍后将回到这些结构。

```
type gameState = 
  | Playing(player)
  | Winner(player)
  | Draw;
```

我们需要定义的另一个变量是`gameState`。游戏可以有三种可能的状态。其中一个`player`可以是`Playing`，可以是`Winner`，或者我们可以有一个`Draw`。

现在，我们有了组成游戏状态所需的所有类型。

```
type state = {
  board,
  gameState,
};
```

我们组件的状态是一个由`board`和`gameState`组成的`record`。

在进一步讨论之前，我想先谈谈模块。按理说，文件是模块。例如，我们将所有的变体存储在`SharedTypes.re`文件中。这段代码自动包装在模块中，如下所示:

```
module SharedTypes {
  /* variant types code */
}
```

如果我们想在不同的文件中访问这个模块，我们不需要任何`import`关键字。我们可以使用点符号在应用程序中的任何地方轻松访问我们的模块，例如`SharedTypes.gameState`。

因为我们经常使用我们的变体，我们可以通过在我们想要访问模块的文件的顶部写`open SharedTypes`来使它更简洁。这允许我们去掉点符号，因为我们可以在文件范围内使用我们的模块。

### 建立国家

![7mmAc2Jq5I1NBrtabAzNIPOTCH66gTLcvpv3](img/b7c6f69dd9453ca372c2082770f4c914.png)

既然我们知道了应用程序的状态，我们就可以开始构建游戏本身了。

我们已经看到我们的`App`组件呈现了`Game`组件。这是所有乐趣开始的地方。我将一步一步地向您介绍代码。

`App`是一个无状态组件，类似于 React 中的功能组件。另一方面，`Game`是有状态的，这意味着它可以包含状态和归约器。理性中的减速器与你从 [Redux](https://github.com/reactjs/redux) 中了解到的减速器基于相同的原理。你调用一个动作，reducer 会捕捉它并相应地更新状态。

为了了解`Game`组件中发生了什么，让我们检查一下`make`函数(代码被缩短了)。

```
let component = ReasonReact.reducerComponent("Game");

let make = _children => {
  ...component,
  initialState: () => initialState,
  reducer: (action: action, state: state) => ...,
  render: ({state, send}) => ...,
};
```

在`App`组件中，我们只覆盖了`render`方法。这里，我们也覆盖了`reducer`和`initialState`属性。稍后我们将讨论减速器。

`initialState`是一个函数，它(令人惊讶地)返回我们存储在变量中的初始状态。

```
let initialState = {
  board: [
    [Empty, Empty, Empty],
    [Empty, Empty, Empty],
    [Empty, Empty, Empty],
  ],
  gameState: Playing(Cross),
};
```

如果你向上滚动一点，检查我们的`state`类型，你会看到`initialState`有相同的结构。它是由`field`的`row`组成的`board`组成的，游戏开始时所有字段都是`Empty`。

然而，随着游戏的进行，他们的状态可能会发生变化。状态的另一部分是最初设置为先玩的`Cross`玩家的`gameState`。

### 渲染板

让我们来看看我们的`Game`组件的`render`方法。

```
render: ({state, send}) =>
    <div className="game">
      <Board
        state
        onRestart=(_evt => send(Restart))
        onMark=(id => send(ClickSquare(id)))
      />
    </div>,
```

我们已经知道它接受了`self`参数。这里，我们使用析构来访问`state`和`send`函数。这就像在 JavaScript 中一样。

render 方法返回`Board`组件，并将`state`和两个状态处理程序作为道具传递给它。第一个负责应用程序重启，第二个在玩家标记字段时触发。

你可能已经注意到，当传递`state`属性时，我们没有写`state=state`。按理说，如果我们不改变 prop 的名称，我们可以使用这个简化的语法来传递 prop。

现在，我们可以看看`Board`组件。我暂时省略了大部分的`render`方法。

```
let component = ReasonReact.statelessComponent("Board");

let make = (~state: state, ~onMark, ~onRestart, _children) => {
  ...component,
  render: _ =>
    <div className="game-board">
      /* ... */
    </div>,
};
```

`Board`是一个无状态组件。您可能已经注意到了，`make`函数现在有几个参数。这些是我们从`Game`父组件传递过来的道具。

`~`符号表示参数被标记。用这样的参数调用函数时，我们需要在调用这个函数(组件)时显式地写出参数的名称。这就是我们在`Game`组件中将道具传递给它时所做的。

你可能也注意到了，我们正在对其中一个论点做另一件事。在上一节中，我们定义了我们的`state`类型。这里，我们告诉编译器，这个参数的结构应该和`state`类型的结构相同。你可能从心流中知道这个模式。

![G9CmB7A70emHKIGRogQ0uKXPKrrKQw3AwjcH](img/9a18bee5dfd33bbff02b3a1152f61c73.png)

让我们回到`Board`组件的`render`方法。

因为我们在那里处理列表，所以在检查其余的`render`方法之前，我们现在将更多地讨论它们。

### 游览 I:列表和数组

按理说，我们有两个类似 JavaScript 数组的数据结构— `list`和`array`。`list`是不可变的和可调整大小的，而`array`是可变的和有固定长度的。我们使用`list`是因为它的灵活性和效率，当我们递归使用它时，它真的会大放异彩。

要映射一个`list`，可以使用接收两个参数的`List.map`方法——一个函数和一个`list`。该函数从`list`中获取一个元素并映射它。这与 JavaScript `Array.map`非常相似。这里有一个简单的例子:

```
let numbers = [1, 5, 8, 9, 15];
let increasedNumbers = List.map((num) => num + 2, numbers);
Js.log(increasedNumbers);  /* [3,[7,[10,[11,[17,0]]]]] */
```

什么？你是说打印出来的结果看起来很奇怪？这是因为 Reason 中的列表是[链接的](https://en.wikipedia.org/wiki/Linked_list)。

在代码中打印列表可能会令人困惑。幸运的是，您可以使用`Array.of_list`方法将其转换为`array`。

```
Js.log(Array.of_list(increasedNumbers));  /* [3,7,10,11,17] */
```

让我们回到我们的应用程序，提醒自己我们的`state`看起来怎么样。

```
let initialState = {
  board: [
    [Empty, Empty, Empty],
    [Empty, Empty, Empty],
    [Empty, Empty, Empty],
  ],
  gameState: Playing(Cross),
};
```

在 Board 的`render`方法中，我们首先映射由一列行组成的`board`。因此，通过映射它，我们将获得对`row`的访问。然后，我们呈现`BoardRow`组件。

```
let component = ReasonReact.statelessComponent("Board");

let make = (~state: state, ~onMark, ~onRestart, _children) => {
   ...component,
   render: _ =>
      <div className="game-board">
         ( 
            ReasonReact.array(
               Array.of_list(
                  List.mapi(
                    (index: int, row: row) =>
                     <BoardRow
                        key=(string_of_int(index))
                        gameState=state.gameState
                        row
                        onMark
                        index
                     />,
                   state.board,
                 ),
             ),
           )
        )
     /* ... */
```

我们使用的是`List.mapi`方法，它为我们提供了一个`index`参数，我们需要这个参数来惟一地定义我们的 id。

当将`list`映射到 JSX 元素时，我们需要做两件额外的事情。

首先，我们需要使用`Array.of_list`将其转换成一个`array`。其次，我们需要使用`ReasonReact.array`将结果转换为`reactElement`，因为我们(如前所述)不能像 React 中那样简单地将字符串传递给 JSX 元素。

为了获得字段值，我们还需要映射每个`row`。我们在`BoardRow`组件内部做这件事。这里，`row`中的每个元素都被映射到`Square`组件。

```
let component = ReasonReact.statelessComponent("BoardRow");

let make = (~gameState: gameState, ~row: row, ~onMark, ~index: int, _children) => {
   ...component,
   render: (_) =>
      <div className="board-row">
         (ReasonReact.array(
            Array.of_list(
               List.mapi(
                  (ind: int, value: field) => {
                    let id = string_of_int(index) ++ string_of_int(ind);
                    <Square
                       key=id
                       value
                       onMark=(() => onMark(id))
                       gameState
                    />;
                 },
               row,
             ),
          ),
        ))
    </div>,
};
```

使用这两个映射，我们的电路板被渲染。你会同意我的看法，这段代码的可读性不是很好，因为所有的函数包装。

为了改进它，我们可以使用`pipe`操作符，它获取我们的`list`数据，并通过我们的函数进行传输。这是第二个映射示例——这次使用了`pipe`。

```
let component = ReasonReact.statelessComponent("BoardRow");

let make = (~gameState: gameState, ~row: row, ~onMark, ~index: int, _children) => {
   ...component,
   render: (_) =>
      <div className="board-row">
         (
            row
            |> List.mapi((ind: int, value: field) => {
               let id = string_of_int(index) ++ string_of_int(ind
               <Square 
                 key=id
                 value
                 onMark=(() => onMark(id))
                 gameState
               />;
             })
            |> Array.of_list
            |> ReasonReact.array
         )
      </div>,
};
```

这使得我们的代码可读性更好，你不觉得吗？首先，我们将`row`传递给映射方法。然后，我们将结果转换成一个`array`。最后，我们将其转换为`reactElement`。

通过映射我们的棋盘，我们将一堆`Square`组件渲染到屏幕上，通过这样做，我们创建了整个游戏棋盘。

我们将向`Square`传递一些道具。因为我们希望我们的`id`是惟一的，所以我们通过组合来自两个映射的索引来创建它。我们还传递了包含`field`类型的`value`，它可以是`Empty`或`Marked`。

最后，我们传递一个`gameState`和`onMark`处理程序，当一个特定的`Square`被点击时，它们将被调用。

### 输入字段

![GEIYAkhGbe884pLggpWM4JAVEww3pR0JQYmg](img/9a922908cc42b763a6963cdae7e1b88d.png)

```
let component = ReasonReact.statelessComponent("Square");

let make = (~value: field, ~gameState: gameState, ~onMark, _children) => {
  ...component,
  render: _self =>
    <button
      className=(getClass(gameState, value))
      disabled=(gameState |> isFinished |> Js.Boolean.to_js_boolean)
      onClick=(_evt => onMark())>
      (value |> toValue |> toString)
    </button>,
};
```

`Square`组件呈现一个按钮并传递给它一些道具。我们在这里使用了几个助手函数，但我不会详细讨论它们。你可以在[回购](https://github.com/codinglawyer/reason-tic-tac-toe)中找到它们。

按钮的类别是使用`getClass`辅助函数计算的，当其中一个玩家获胜时，该函数会将方块变成绿色。当这种情况发生时，所有的`Square`也将被禁用。

为了渲染按钮的`value`，我们使用了两个助手。

```
let toValue = (field: field) =>
  switch (field) {
  | Marked(Cross) => "X"
  | Marked(Circle) => "O"
  | Empty => ""
};
```

`toValue`将使用模式匹配将`field`类型转换为字符串。我们稍后将讨论模式匹配。现在，您需要知道我们正在将`field`数据与我们的三种模式进行匹配。因此，结果将是`X`、`O`，或者一个空字符串。然后，我们使用`toString`将其转换为`reactElement`。

唷。我们刚刚渲染了游戏板。让我们快速回顾一下我们是如何做到的。

我们的顶层`App`组件呈现保存游戏状态的`Game`组件，并将其与处理程序一起传递给`Board`组件。

然后，`Board`获取棋盘状态属性并将行映射到`BoardRow`组件，后者将行映射到`Square`组件。每个`Square`都有一个 onClick 处理程序，它将用一个正方形或一个圆形填充它。

### 让它做点什么吧！

让我们来看看我们控制游戏的逻辑是如何工作的。

因为我们有一个棋盘，我们可以允许玩家点击任何方块。当这种情况发生时，触发`onClick`处理程序并调用`onMark`处理程序。

```
/* Square component */
<button
  className=(getClass(gameState, value))
  disabled=(gameState |> isFinished |> Js.Boolean.to_js_boolean)
  onClick=(_evt => onMark())>
  (value |> toValue |> toString)
</button>
```

从`BoardRow`组件传递来了`onMark`处理程序，但是它最初是在负责状态的`Game`组件中定义的。

```
/* Game component */
render: ({state, send}) =>
    <div className="game">
      <Board
        state
        onRestart=(_evt => send(Restart))
        onMark=(id => send(ClickSquare(id)))
      />
    </div>,
```

我们可以看到`onMark` prop 是一个`ClickSquare` reducer，这意味着我们用它来更新状态(如 Redux 中)。`onRestart`处理程序的工作方式类似。

注意，我们正在将 square 的惟一的`id`传递给`BoardRow`组件中的`onMark`处理程序。

```
/* BoardRow component */
(
  row
  |> List.mapi((ind: int, value: field) => {
    let id = string_of_int(index) ++ string_of_int(ind
    <Square 
      key=id
      value
      onMark=(() => onMark(id))
      gameState
    />;
   })
  |> Array.of_list
  |> ReasonReact.array
)
```

在详细查看我们的减速器之前，我们需要定义减速器将响应的动作。

```
type action =
  | ClickSquare(string)
  | Restart;
```

与全局变量类型一样，这迫使我们在开始实现它之前考虑我们的逻辑。我们定义了两个动作变量。`ClickSquare`接受一个类型为`string`的参数。

现在，让我们看看我们的减速器。

```
let updateBoard = (board: board, gameState: gameState, id) =>
  board
  |> List.mapi((ind: int, row: row) =>
    row
      |> List.mapi((index: int, value: field) =>
        string_of_int(ind) ++ string_of_int(index) === id ?
          switch (gameState, value) {
          | (_, Marked(_)) => value
          | (Playing(player), Empty) => Marked(player)
          | (_, Empty) => Empty
          } :
          value
      )
  );

reducer: (action: action, state: state) =>
    switch (action) {
    | Restart => ReasonReact.Update(initialState)
    | ClickSquare((id: string)) =>
       let updatedBoard = updateBoard(state.board, state.gameState, id);
       ReasonReact.Update({
         board: updatedBoard,
         gameState:
            checkGameState3x3(updatedBoard, state.board, state.gameState),
       });
    },
```

`ClickSquare`减速器取特定`Square`的一个`id`。正如我们已经看到的，我们正在传入`BoardRow`组件。然后，我们的减速器计算一个新的状态。

对于`board`状态更新，我们将调用`updateBoard`函数。它使用了我们在`Board`和`BoardRow`组件中使用的相同映射逻辑。在其中，我们映射`state.board`来获取行，然后映射行来获取字段值。

因为每个方块的`id`是来自两个映射的 id 的组合，我们将使用它来查找玩家点击的字段。当我们找到它时，我们将使用模式匹配来决定如何处理它。否则，我们将保持正方形的`value`不变。

### 游览 II:模式匹配

![fiLwW5agrPnevGF1Y-ZgXjbpfmEZbmE85YV1](img/d9c65bc42e30f6edc9d4e7a72c32384b.png)

我们使用模式匹配来处理我们的数据。我们定义**模式**，我们将把它与我们的**数据**进行匹配。当在 Reason 中使用模式匹配时，我们使用一个`switch`语句。

```
switch (state.gameState, value) {
  | (_, Marked(_)) => value
  | (Playing(player), Empty) => Marked(player)
  | (_, Empty) => Empty
}
```

在我们的例子中，我们使用一个[元组](https://reasonml.github.io/docs/en/tuple.html)来表示我们的**数据**。元组是用逗号分隔数据的数据结构。我们的`tuple`包含`gameState`和`value`(包含`field`类型)。

然后我们定义多个**模式**，我们将根据我们的数据进行匹配。第一次匹配决定了整个模式匹配的结果。

通过在模式中写一个下划线，我们告诉编译器我们不关心特定的值是什么。换句话说，我们每次都想比赛。

例如，任何玩家在`value`为`Marked`时匹配第一个图案。所以，我们不关心`gameState`，也不关心玩家类型。

当这个模式匹配时，结果就是原来的`value`。此模式防止玩家覆盖已标记的`Squares`。

第二种模式处理任何玩家都在玩的情况，字段是`Empty`。这里，我们在模式中使用`player`类型，然后在结果中再次使用。我们基本上是说，我们不关心哪个玩家在玩(`Circle`或`Cross`)，但我们仍然希望根据实际在玩的玩家来标记方块。

最后一种模式作为默认模式。如果第一个或第二个模式不匹配，第三个将总是匹配的。在这里，我们不关心`gameState`。

然而，由于我们在前面的模式中检查的是`Playing`游戏状态，我们现在检查的是`Draw`或`Winner` `gameState`类型。如果是这样的话，我们就离开`Empty`这个领域。这种默认场景会阻止玩家在游戏结束后继续游戏。

关于模式匹配的一件很酷的事情是，如果你没有覆盖所有可能的模式匹配，编译器会警告你。这会省去你很多麻烦，因为你总是知道你是否已经涵盖了所有可能的场景。所以，如果编译器没有给你任何警告，你的模式匹配永远不会失败。

当模式匹配完成时，特定字段得到更新。当所有的映射完成后，我们得到一个新的板状态，并将其存储为`updatedBoard`。然后我们可以通过调用`ReasonReact.Update`来更新组件的状态。

```
ReasonReact.Update({
  board: updatedBoard,
  gameState:
    checkGameState3x3(updatedBoard, state.board, state.gameState),
```

我们使用模式匹配的结果更新`board`状态。当更新`gameState`时，我们调用`checkGameState3x3`助手为我们计算游戏的状态。

### 我们有赢家吗？

![te2QEBpX1w-XX4HghCfe543FZVCCHR5n6ma9](img/a6a781ec035b6d7fde8e9db84ea1fd47.png)

让我们来看看`checkGameState3x3`是做什么的。

首先，我们需要定义获胜场的所有可能组合(对于 3x3 棋盘)，并将它们存储为`winningCombs`。我们还必须定义`winningRows`类型。

```
type winningRows = list(list(int));

let winningCombs = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],  
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];
```

我们将这个列表作为第一个参数传递给了`checkGameState`函数。

```
let checkGameState3x3 = checkGameState(winningCombs);
```

通过这样做，我们利用了[逢迎](https://en.wikipedia.org/wiki/Currying)原则。当我们将`winningCombs`传递给`checkGameState`函数时，我们得到一个新的函数，等待其余的参数被传递。我们将这个新函数存储为`checkGameState3x3`。

这种行为非常有用，因为我们能够根据电路板的宽度和高度来配置`checkGameState`功能。

让我们看看`checkGameState`函数内部发生了什么。

```
let checkGameState =
  (
    winningRows: winningRows,
    updatedBoard: board,
    oldBoard: board,
    gameState: gameState,
  ) =>
 oldBoard == updatedBoard ?
   gameState :
   {
     let flattenBoard = List.flatten(updatedBoard);
     let rec check = (rest: winningRows) => {
       let head = List.hd(rest);
       let tail = List.tl(rest);
       switch (
         getWinner(flattenBoard, head),
         gameEnded(flattenBoard),
         tail,
       ) {
       | (Cross, _, _) => Winner(Cross)
       | (Circle, _, _) => Winner(Circle)
       | (_, true, []) => Draw
       | (_, false, []) => whosPlaying(gameState)
       | _ => check(tail)
       };
    };
    check(winningRows);
};
```

首先，我们检查电路板状态是否与前一个不同。如果不是这样，我们将返回未更改的`gameState`。否则，我们将计算新的游戏状态。

#### 计算新状态

![oanyRY03r9aMAUtxtzFR-GvsnnMoT8azntmX](img/865aecb98b4be74e1f8a3758f03d5012.png)

我们通过使用`List.flatten`将状态的`board`部分(由一列行组成)转换为简单的`list`来开始确定新的游戏状态。展平的结果将具有这种结构:

```
[Empty, Empty, Empty, Empty, Empty, Empty, Empty, Empty, Empty]
```

回到函数中，我们定义了一个`check`函数，它接收一个类型为`winningRows`的`rest`参数。定义前的`rec`关键字意味着它可以被递归调用。然而，对于递归函数调用，我们也需要递归数据。幸运的是，`list`是一种递归数据结构。

我们已经知道理性中的列表是链接的。这个特性使我们能够使用递归轻松地遍历[列表。](http://reasonmlhub.com/exploring-reasonml/ch_recursion.html)

在`checkGameState`的底部，我们第一次调用`check`函数，并传递给它`winningCombs`列表。在函数内部，我们从`list`中提取第一个元素，并将其存储为`head`。其余的`list`被存储为`tail`。

之后，我们再次使用模式匹配。我们已经知道它是如何工作的，所以我就不赘述了。但是检查我们如何定义我们的数据和模式是值得的。

```
type winner =
  | Cross
  | Circle
  | NoOne;

switch (
  getWinner(flattenBoard, head),
  gameEnded(flattenBoard),
  tail,
) { ...
```

在`switch`语句中，我们再次使用一个`tuple`来表示我们的数据。我们的`tuple`包含三个元素——winner 类型作为`getWinner`函数的结果，boolean 作为`gameEnded`函数的结果，以及剩余的`list`元素(`tail`)。

在进一步讨论之前，让我们先讨论一下这两个辅助函数。

我们先来看看`getWinner`函数的内部。

```
let getWinner = (flattenBoard, coords) =>
  switch (
    List.nth(flattenBoard, List.nth(coords, 0)),
    List.nth(flattenBoard, List.nth(coords, 1)),
    List.nth(flattenBoard, List.nth(coords, 2)),
  ) {
  | (Marked(Cross), Marked(Cross), Marked(Cross)) => Cross
  | (Marked(Circle), Marked(Circle), Marked(Circle)) => Circle
  | (_, _, _) => NoOne
  };
```

当我们第一次调用`check`递归函数时，`head`将是`winningRows`的第一个元素，也就是说`[0, 1, 2]`是一个`list`。我们将`head`和`flattenBoard`一起传递给`getWinner`函数作为`coords`参数。

同样，我们使用与`tuple`匹配的模式。在`tuple`内部，我们使用`List.nth`方法来访问`coords`坐标在展平板`list`中的等价位置。`List.nth`函数接受一个`list`和一个数字，并将列表的元素返回到那个位置。

因此，我们的`tuple`由我们使用`List.nth`访问的棋盘的三个获胜坐标组成。

现在，我们可以将我们的`tuple`数据与模式进行匹配。前两个模式检查所有三个字段是否由同一玩家标记。如果是，我们将返回获胜者— `Cross`或`Circle`。否则，我们就返回`NoOne`。

让我们看看`gameEnded`函数内部发生了什么。它检查是否所有的字段都是`Marked`并返回一个布尔值。

```
let gameEnded = board =>
  List.for_all(
    field => field == Marked(Circle) || field == Marked(Cross),
    board,
  );
```

既然我们知道我们的助手函数可以返回什么值，让我们回到我们的`check`函数。

```
switch (
  getWinner(flattenBoard, head),
  gameEnded(flattenBoard),
  tail,
  ) {
  | (Cross, _, _) => Winner(Cross)
  | (Circle, _, _) => Winner(Circle)
  | (_, true, []) => Draw
  | (_, false, []) => whosPlaying(gameState)
  | _ => check(tail)
  };
```

我们的模式匹配现在可以决定游戏是赢还是平。如果这些案例不匹配，我们将转到下一个案例。如果匹配，游戏继续进行并调用`whosPlaying`函数，另一个玩家轮一次。

```
let whosPlaying = (gameState: gameState) =>
  switch (gameState) {
  | Playing(Cross) => Playing(Circle)
  | _ => Playing(Cross)
  };
```

否则，我们将使用新的获胜字段组合递归调用`check`函数。

就是这样。现在你知道我们控制游戏逻辑的代码是如何工作的了。

### 那都是乡亲们！

我希望这篇文章能帮助你理解这种有前途且仍在发展中的语言的核心特性。然而，要在 OCaml 之上充分体会这种新语法的威力，您需要开始构建自己的东西。现在你已经准备好了。

祝你好运！

![1zDhVxvp1uDIV-5RdNDxFMYqiyQTYNdQyC4G](img/b1ae9da623e2c48d4d5905063d5f82a4.png)

如果你喜欢这篇文章，给它几个掌声。我会非常感激，更多的人也会看到这篇文章。

这篇文章最初发表在我的博客上。

如果您有任何问题、批评、意见或改进建议，请随时在下面写下评论或通过 [Twitter](https://twitter.com/coding_lawyer) 联系我。