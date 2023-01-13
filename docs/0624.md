# 如何在 JavaScript 中使用 Call、Apply 和 Bind 函数——代码示例

> 原文：<https://www.freecodecamp.org/news/understand-call-apply-and-bind-in-javascript-with-examples/>

在本文中，我将通过简单的例子解释如何在 JavaScript 中使用调用、应用和绑定。

我们还将实现一个示例，展示如何使用 apply 函数创建自己的 map 函数。

事不宜迟，我们开始吧。

## 目录

*   [先决条件](#prerequisites)
*   [定义](#definitions)
*   [如何使用 JavaScript 中的 call 函数](#how-to-use-the-call-function-in-javascript)
*   [如何使用 JavaScript 中的 apply 函数](#how-to-use-the-apply-function-in-javascript)
*   [如何使用 JavaScript 中的 bind 函数](#how-to-use-the-bind-function-in-javascript)
*   [如何创建自己的地图功能](#how-to-create-your-own-map-function)
*   [总结](#summary)

## 先决条件

为了从本文中获得最大收益，您应该了解以下几点:

*   [功能](https://www.freecodecamp.org/news/what-is-a-function-javascript-function-examples/)
*   [功能原型](https://www.freecodecamp.org/news/all-you-need-to-know-to-understand-javascripts-prototype-a2bff2d28f03/)
*   [这个关键字](https://www.freecodecamp.org/news/what-is-this-in-javascript/)

## 定义

让我们更仔细地看看我们将在这里学习的函数，以理解它们做什么。

**调用**是一个帮助你改变调用函数上下文的函数。用外行人的话来说，它帮助你把函数内部的`this`的值替换成你想要的任何值。

**应用**与`call`功能非常相似。唯一的区别是在`apply`中，你可以传递一个数组作为参数列表。

**Bind** 是一个帮助您创建另一个函数的函数，您可以稍后使用提供的新上下文`this`来执行该函数。

现在，我们将看看调用、应用和绑定函数的一些基本示例。然后，我们将看一个例子，在这个例子中，我们将构造自己的类似于 map 函数的函数。

## 如何使用 JavaScript 中的 Call 函数

`call`是一个函数，用来改变函数中`this`的值，并使用提供的参数执行它。

下面是`call`函数的语法:

```
 func.call(thisObj, args1, args2, ...) 
```

在哪里，

*   **func** 是一个需要用不同的`this`对象调用的函数
*   **thisObj** 是一个需要用函数`func`中的`this`关键字替换的对象或值
*   **args1、args2** 是传递给带有已更改的`this`对象的调用函数的参数。

注意，如果您调用一个没有任何`thisObj`参数的函数，那么 JavaScript 认为这个属性是一个全局对象。

现在我们已经对`call`函数有了一些了解，让我们通过一些例子来更详细地理解它。

### 如何在 JS 中调用不同上下文的函数

考虑下面的例子。它包括三个等级—`Car`、`Brand1`和`Brand2`。

```
function Car(type, fuelType){
	this.type = type;
	this.fuelType = fuelType;
}

function setBrand(brand){
	Car.call(this, "convertible", "petrol");
	this.brand = brand;
	console.log(`Car details = `, this);
}

function definePrice(price){
	Car.call(this, "convertible", "diesel");
	this.price = price;
	console.log(`Car details = `, this);
}

const newBrand = new setBrand('Brand1');
const newCarPrice = new definePrice(100000);
```

[https://www.canva.com/design/DAFD4b369JM/watch?embed](https://www.canva.com/design/DAFD4b369JM/watch?embed)

如果你仔细观察，你会发现我们在两种情况下使用了`call`函数来调用`Car`函数。首先在`setBrand`中，然后在`definePrice`功能中。

在这两个函数中，我们用代表各自函数本身的`this`对象来调用`Car`函数。例如，在`setBrand`内部，我们用属于其上下文的`this`对象调用`Car`函数。`definePrice`的情况类似。

### 如何在 JS 中调用不带参数的函数

考虑下面的例子:

```
const newEntity = (obj) => console.log(obj);

function mountEntity(){
	this.entity = newEntity;
	console.log(`Entity ${this.entity} is mounted on ${this}`);
}

mountEntity.call();
```

在这个例子中，我们调用了没有参数`thisObj`的函数`mountEntity`。在这种情况下，JavaScript 引用全局对象。

## 如何在 JavaScript 中使用 Apply 函数

`Apply`功能与`Call`功能非常相似。`call`和`apply`之间唯一的区别是传递参数的方式不同。

在`apply`，arguments 你可以传递一个参数作为一个数组文本或者一个新的数组对象。

下面是`apply`函数的语法:

```
func.apply(thisObj, argumentsArray);
```

在哪里，

*   **func** 是一个需要用不同的`this`对象调用的函数
*   **thisObj** 是一个需要用函数`func`中的`this`关键字替换的对象或值
*   **argumentsArray** 可以是一个参数数组，数组对象，或者是 [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments) 关键字本身。

正如你在上面看到的，`apply`函数有不同类型的语法。

第一个语法很简单。您可以传入一组参数，如下所示:

```
func.apply(thisObj, [args1, args2, ...]);
```

第二个语法是我们可以将新的数组对象传递给它的地方:

```
func.apply(thisObj, new Array(args1, args2));
```

第三种语法是我们可以传入 arguments 关键字的地方:

```
func.apply(thisObj, arguments); 
```

[参数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)是一个在函数内部可用的特殊对象。它包含传递给函数的参数值。您可以使用这个关键字和`apply`函数来获取任意数量的参数。

关于`apply`最好的部分是我们不需要关心传递给调用函数的参数的数量。由于其动态和多用途的性质，您可以在复杂的情况下使用它。

让我们看看上面的例子，但是这次我们将使用`apply`函数。

```
function Car(type, fuelType){
	this.type = type;
	this.fuelType = fuelType;
}

function setBrand(brand){
	Car.apply(this, ["convertible", "petrol"]); //Syntax with array literal
	this.brand = brand;
	console.log(`Car details = `, this);
}

function definePrice(price){
	Car.apply(this, new Array("convertible", "diesel")); //Syntax with array object construction
	this.price = price;
	console.log(`Car details = `, this);
}

const newBrand = new setBrand('Brand1');
const newCarPrice = new definePrice(100000);
```

这里有一个例子展示了如何使用`arguments`关键字:

```
function addUp(){
		//Using arguments to capture the arbitrary number of inputs
    const args = Array.from(arguments); 
    this.x = args.reduce((prev, curr) => prev + curr, 0);
    console.log("this.x = ", this.x);
}

function driverFunc(){
    const obj = {
        inps: [1,2,3,4,5,6]
    }
    addUp.apply(obj, obj.inps);
}

driverFunc();
```

## 如何在 JavaScript 中使用绑定函数

函数`bind`创建了一个函数的副本，并为调用函数中的函数`this`添加了一个新值。

下面是`bind`函数的语法:

```
func.bind(thisObj, arg1, arg2, ..., argN);
```

在哪里，

*   **func** 是一个需要用不同的`this`对象调用的函数
*   **thisObj** 是一个需要用函数`func`中的`this`关键字替换的对象或值
*   **arg1，arg 2…argN**–可以向调用函数传递一个或多个参数，类似于`call`函数。

然后，`bind`函数返回一个新函数，该函数包含一个新的上下文到调用函数中的`this`变量:

```
func(arg1, arg2);
```

现在这个函数`func`可以在以后用参数执行。

让我们看一个如何在基于类的 React 组件的帮助下使用`bind`函数的经典例子:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      counter: 1
    };
  }
  handleCode() {
    console.log("HANDLE CODE THIS = ", this.state);
  }
  render() {
    return <button onClick={this.handleCode}>Click Me</button>;
  }
}
```

考虑上面的 App 组件。它由以下内容构成:

*   `constructor`是一个被调用为类的函数，并用一个`new`关键字实例化。
*   `render`是一个执行/渲染 JSX 代码的函数。
*   `handleCode`是一个记录组件状态的类方法。

如果我们点击`Click Me`按钮，我们将收到一条错误消息:`Cannot read properties of undefined (reading 'state')`。

你有没有想过为什么会出现这个问题？🤔🤔

您可能希望我们能够访问类的状态，因为`handleCode`是一个类方法。但是这里有一个问题:

*   `handleCode`内的`this`与该类的`this`不同。
*   在一个类中，`this`是一个普通的对象，它拥有非静态的类方法作为它的属性。但是`handleCode`里面的`this`将指代不同的上下文。
*   老实说，这个场景中`this`的值取决于从哪里调用函数。如果你看到，`handleCode`在`onClick`事件中被调用。
*   但是在这个阶段，我们将在`handleCode`函数中为`this`的上下文获取`undefined`。
*   我们试图调用一个未定义值的`state`属性。所以，这就导致了上面的错误。

我们可以通过在`handleCode`方法中提供`this`的正确上下文来解决这个问题。你可以用`bind`方法做到这一点。

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      counter: 1
    };
   this.handleCode = this.handleCode.bind(this); //bind this function
  }
  handleCode() {
    console.log("HANDLE CODE THIS = ", this.state);
  }
  render() {
    return <button onClick={this.handleCode}>Click Me</button>;
  }
}
```

`bind`将创建一个新函数，并用一个新属性`handleCode`将其存储在`this`对象中。`Bind`将确保类的`this`上下文被应用到`handleCode`函数中的`this`。

## 如何创建自己的`map`函数

现在我们已经有了所有必要的东西，让我们开始创建我们的`own` map 函数。让我们首先了解构建我们的`own`地图函数所需的东西。

下面是`map`函数的语法:

```
arr.map(func)
```

在哪里，

*   **arr** 是调用 map 的数组。
*   **func** 是需要在数组的每个元素上运行的函数。

`map`功能的基本功能很简单:

它是一个遍历数组中每个元素的函数，并应用作为参数传递的函数。map 的返回类型也是一个数组，每个元素上都应用了`func`。

现在我们理解了需求，所以我们可以继续创建我们自己的`map`函数。下面是我们新的`map`函数的代码:

```
function newMap(func){
  let destArr = [];
  const srcArrLen = this.length;
  for(let i = 0; i < srcArrLen; i++){
    destArr.push(func.call(this, this[i]));
  }

  return destArr;
} 
```

让我们一点一点地理解上面的函数:

*   这个函数接受一个名为`func`的参数。它只是一个需要在数组的每个元素上调用的函数。
*   代码的其他部分非常简单明了。我们将重点关注下面一行:`destArr.push(func.call(this, this[i]));`
*   这一行做了两件事:
    1。将更改推入`destArr`2 的
    中。借助`call`方法执行`func`。这里，`call`方法(如前几节所述)将执行`func`方法，并为`func`方法中的`this`对象赋予一个新值。

现在让我们看看我们将如何执行我们的`newMap`函数。下面的方法是向现有的原始数据类型添加一个新的方法，这是不推荐的，但是为了这篇文章，我们还是会这样做。

**注意:**不要在您的生产代码中遵循以下方法。这可能会对现有代码造成损害。

```
Object.defineProperty(Array.prototype, 'newMap', {
  value: newMap
}); 
```

`defineProperty`我们在`Array.prototype`中创建新的属性。

一旦完成，我们就可以在数组上执行新的 map 函数了。

```
const arr = [1,2,3];
const newArr = arr.newMap(item => item + 1);
console.log(newArr);
```

## 摘要

本文通过示例向您展示了 call、apply 和 bind 函数的功能。

简单地说一下这些功能:

*   Call、apply 和 bind 是帮助您改变调用函数中出现的关键字`this`的上下文的函数。
*   我们看到了如何以不同的方式调用每个函数——例如，使用`apply`你可以执行一个带有参数数组的函数，而使用`call`你可以执行相同的函数，但是参数通过逗号分开。
*   这些函数在 React 的基于类的组件中非常有用。

感谢您的阅读！

在 [Twitter](https://twitter.com/keurplkar) 、 [GitHub](https://github.com/keyurparalkar) 和 [LinkedIn](https://www.linkedin.com/in/keyur-paralkar-494415107/) 上关注我。