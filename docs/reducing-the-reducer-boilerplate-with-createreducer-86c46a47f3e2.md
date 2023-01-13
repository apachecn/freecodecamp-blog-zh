# 用 createReducer()减少 Reducer 样板文件

> 原文：<https://www.freecodecamp.org/news/reducing-the-reducer-boilerplate-with-createreducer-86c46a47f3e2/>

布万·马利克

# 用 createReducer()减少 Reducer 样板文件

![1*qBvXcdtU2MeWhsdD9Cpu0g](img/d20d101f676221be5c33fcb4a5b397d1.png)

This article is a short and simple walk-through for reducing the reducer boilerplate by using a more functional approach.

首先，快速回顾一下 Redux 中的 reducers 是什么:

归约器只不过是接受先前状态和动作并返回新状态的纯函数。

需要记住的两件事是，它们是纯态的，因此 T2 不会改变 T3 的状态。

说完了，让我们进入正题。

当我们从 redux 开始时，我们是这样写一个 reducer 的:

我们有一个搜索缩减器，它可以根据不同的操作更新状态，比如设置搜索结果、更新搜索字符串或改变装载器/旋转器的状态。让我们假设它是一个[切片缩减器](http://redux.js.org/docs/recipes/reducers/SplittingReducerLogic.html)，稍后我们可以使用`combineReducers(reducers)`函数组合它。

现在，如果你像我一样，你可能不喜欢转换语句？

他们自带了太多的样板文件。使用开关盒处理许多动作类型的减速器会很长。这看起来不太妙，不是吗？！这个想法是抛弃开关，转向功能性更强的方法。

### **让我们稍微反思一下**

我们能做的是将所有的开关 case 逻辑抽象成“case 函数”,并创建一个对象，将动作类型映射到它们对应的 case 函数。我们将这个对象称为“actionHandlers”。
下面是对象:

如您所见，我们现在有了从动作类型到 case 函数的映射。

> Case 函数就像一个小的 reducer 函数，它将状态和输入动作作为参数，并返回状态树的一个新切片。

现在我们必须创建一个“reducer creator”函数来使用我们的`**actionHandlers**`。这个函数将返回另一个函数，这个函数实际上是我们传递给`combineReducers()`的 reducer。请看:

如您所见，`createReducer()`是一个返回另一个函数的闭包。这个返回的函数满足形式`(previousState, action) => newSt` ate，因此将成为我们实际的切片缩减器。

由于闭包，返回的 reducer 函数可以访问其封闭函数的`actionHandlers`和`initialState`参数。`initialState`被用作`state`的默认参数。在 reducer 函数中，我们检查我们的`actionHandlers`是否有一个属性匹配传入的动作类型。如果是这样，我们执行`actionHandlers`中 case 函数，传入状态和动作。如果动作类型不是`actionHandlers`中的属性，我们返回之前的状态。

你也可以在[官方冗余文件](http://redux.js.org/docs/recipes/ReducingBoilerplate.html)中找到`createReducer()`。

这个 create reducer 函数现在可以导入到不同的 reducer 文件中来创建我们所有的切片 reducer！

出于解释的角度，上面的函数现在是冗长的。让我们来点刺激的吧！下面是新的和改进的 create reducer 文件。？

我已经使用 lambdas 和 Ramda 库的' [propOr](http://ramdajs.com/docs/#propOr) '函数缩短了所有内容。propOr 函数的作用是用第二个参数(一个键)来检查第三个参数(一个对象)，如果找到了就返回它的值。否则，它返回第一个参数提供的默认值。第一个参数， [identity](http://ramdajs.com/docs/#identity) ，是一个只返回提供给它的参数的函数。

因此，如果在`actionHandlers`中找到一个函数，则返回该函数，并使用`(state, action`执行该函数。如果没有找到动作，propOr 返回 identity，它使用相同的`(state, action)`参数执行，并返回提供的第一个参数，即`state`(在这种情况下是前一个状态)。

你可以创建你自己的‘propOr’和‘identity’函数，我用的就是 Ramda。

让我向您展示新的搜索缩减器文件，让您全面了解我们如何将`createReducer`函数与`actionHandlers`一起使用。

`createReducer`函数被部分应用并返回我们的最终切片缩减器，并被导出到我们使用`combineReducers`函数的文件中。

好了，这就是创建 reducers 和减少总体样板文件的好方法。我希望这对你有所帮助:)

![1*AhRFvNgwGHEH_Zzm-3MI_w](img/0c11efc68d21807f1219d61ddb0faec7.png)

以下是我之前文章的一些链接:

[**JavaScript ES6 函数:好的部分**](https://medium.freecodecamp.com/es6-functions-9f61c72b1e86)
[*ES6 提供了一些很酷的新功能特性，使 JavaScript 编程更加灵活。再来说说…*medium.freecodecamp.com](https://medium.freecodecamp.com/es6-functions-9f61c72b1e86)[**JavaScript 变量提升指南？使用 let 和 const**](https://medium.freecodecamp.com/what-is-variable-hoisting-differentiating-between-var-let-and-const-in-es6-f1a70bb43d)
[N*ew JavaScript 开发人员通常很难理解变量/函数提升的独特行为。m*edium.freecodecamp.com](https://medium.freecodecamp.com/what-is-variable-hoisting-differentiating-between-var-let-and-const-in-es6-f1a70bb43d)

和平✌️