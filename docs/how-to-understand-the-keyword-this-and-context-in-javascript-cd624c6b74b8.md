# 如何理解 JavaScript 中的关键字 this 和 context

> 原文：<https://www.freecodecamp.org/news/how-to-understand-the-keyword-this-and-context-in-javascript-cd624c6b74b8/>

由 lukas GIS der-Dube 撰写

# 如何理解 JavaScript 中的关键字 this 和 context

![1*4Ufc1CWbaLhjEMhPgVV3Qw](img/a12a6064767190f8de1ddb13ee0b52a0.png)

Photo by [beasty .](https://unsplash.com/photos/7aUgMBxhVNU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

正如在[我之前的一篇文章](https://levelup.gitconnected.com/10-things-to-learn-on-the-way-to-become-a-javascript-master-f4fc632b2bb7)中提到的，完全掌握 JavaScript 可能是一个漫长的旅程。作为一名 JavaScript 开发人员，你可能在你的旅程中遇到过`this`。当我开始的时候，我第一次看到它是在使用`eventListeners`和 jQuery 的时候。后来，我不得不经常和 React 一起使用它，我相信你也一样。这并不意味着我真的明白它是什么，以及如何完全控制它。

然而，掌握背后的概念是非常有用的，当以清晰的头脑接近时，也不是很难。

#### 深究此事

> *解释`this`会导致很多混乱，仅仅是通过关键字的命名。*

与你在程序中所处的环境紧密相关。让我们从头开始。在我们的浏览器中，如果你在控制台中输入`this`，你将得到`window`-对象，你的 JavaScript 的最外层上下文。在 Node.js 中，如果我们这样做:

```
console.log(this)
```

我们以`{}`结束，一个空的对象。这有点奇怪，但似乎 Node.js 就是这样做的。如果你做了

```
(function() {
  console.log(this);
})();
```

然而，您将接收到`global`对象，即最外层的上下文。在该上下文中，`setTimeout`、`setInterval`被存储。请随意使用它，看看您能做些什么。从这里开始，Node.js 和浏览器几乎没有区别。我将使用`window`。请记住，在 Node.js 中，它将是`global`对象，但实际上并没有什么不同。

![1*ucforPRGm6kR3bdZMHzHKQ](img/4a9258b46c64b75a0c8398ef1e160271.png)

Photo by [Chor Hung Tsang](https://unsplash.com/photos/e_5NhSomvS4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/top-level?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

记住:上下文只在函数内部有意义

想象一下，你写了一个没有在函数中嵌套任何东西的程序。你可以简单地一行接一行地写，而不用具体的结构。这意味着你不必知道你在哪里。你总是在同一水平线上。

当你开始有函数时，你可能有不同层次的程序，而`this`代表你在哪里，什么对象调用了函数。

#### 跟踪调用者对象

让我们看看下面的例子，看看`this`是如何根据上下文变化的:

```
const coffee = {
  strong: true,
  info: function() {
    console.log(`The coffee is ${this.strong ? '' : 'not '}strong`)
  },
}

coffee.info() // The coffee is strong
```

因为我们调用了一个在`coffee`对象中声明的函数，所以我们的上下文正好更改为该对象。我们现在可以通过`this`访问该对象的所有属性。在上面的例子中，我们也可以通过`coffee.strong`直接引用它。当我们不知道我们所处的环境和对象，或者事情变得更复杂时，事情就变得更有趣了。看看下面的例子:

```
const drinks = [
  {
    name: 'Coffee',
    addictive: true,
    info: function() {
      console.log(`${this.name} is ${this.addictive ? '' : 'not '} addictive.`)
    },
  },
  {
    name: 'Celery Juice',
    addictive: false,
    info: function() {
      console.log(`${this.name} is ${this.addictive ? '' : 'not '} addictive.`)
    },
  },
]

function pickRandom(arr) {
  return arr[Math.floor(Math.random() * arr.length)]
}

pickRandom(drinks).info()
```

#### 类别和实例

类可以用来抽象你的代码和共享行为。总是重复上一个例子中的`info`函数声明并不好。因为类和它们的实例实际上是对象，所以它们的行为是相同的。然而，需要注意的一点是，在构造函数中声明`this`实际上是对未来的预测，那时将会有一个实例。

让我们来看看:

```
class Coffee {
  constructor(strong) {
    this.strong = !!strong
  }
  info() {
    console.log(`This coffee is ${this.strong ? '' : 'not '}strong`)
  }
}

const strongCoffee = new Coffee(true)
const normalCoffee = new Coffee(false)

strongCoffee.info() // This coffee is strong
normalCoffee.info() // This coffee is not strong
```

#### 陷阱:无缝嵌套的函数调用

有时候，我们会在一个我们没有真正预料到的环境中结束。当我们不知不觉地在另一个对象上下文中调用函数时，就会发生这种情况。一个非常常见的例子是使用`setTimeout`或`setInterval`时:

```
// BAD EXAMPLE
const coffee = {
  strong: true,
  amount: 120,
  drink: function() {
    setTimeout(function() {
      if (this.amount) this.amount -= 10
    }, 10)
  },
}

coffee.drink()
```

你觉得`coffee.amount`是什么？

...

..

。

还是`120`。首先，我们在`coffee`对象内部，因为`drink`方法是在它内部声明的。我们只做了`setTimeout`，其他什么都没做。正是如此。

正如我前面解释的，`setTimeout`方法实际上是在`window`对象中声明的。当调用它时，我们实际上再次将上下文切换到`window`。这意味着我们的指令实际上试图改变`window.amount`，但是由于`if`语句，它最终什么也没做。为了解决这个问题，我们必须`bind`我们的函数(见下文)。

#### 反应

使用 React，多亏了 Hooks，这有望很快成为过去。目前，我们仍然必须以这样或那样的方式解决所有的问题(稍后会有更多的介绍)。当我开始的时候，我不知道我为什么要这样做，但在这一点上，你应该已经知道为什么它是必要的。

让我们看看两个简单的 React 类组件:

```
// BAD EXAMPLE
import React from 'react'

class Child extends React.Component {
  render() {
    return <button onClick = {
      this.props.getCoffee
    } > Get some Coffee! < /button>
  }
}

class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      coffeeCount: 0,
    }
    // change to turn into good example – normally we would do:
    // this._getCoffee = this._getCoffee.bind(this)
  }
  render() {
    return ( <
      React.Fragment >
      <
      Child getCoffee = {
        this._getCoffee
      }
      /> < /
      React.Fragment >
    )
  }

  _getCoffee() {
    this.setState({
      coffeeCount: this.state.coffeeCount + 1,
    })
  }
}
```

当我们现在点击由`Child`呈现的按钮时，我们将收到一个错误。为什么？因为 React 在调用`_getCoffee`方法时改变了我们的上下文。

我假设 React 确实在另一个上下文中调用了我们组件的 render 方法，通过 helper 类或类似的方式(尽管我需要更深入地挖掘才能确定)。因此，`this.state`是未定义的，我们试图访问`this.state.coffeeCount`。你应该会收到类似`Cannot read property coffeeCount of undefined`的东西。

为了解决这个问题，你必须`bind`(我们将到达那里)在我们的类中的方法，只要我们把它们从定义它们的组件中传递出去。

![1*nkgR5atcQtIeHQLeY8XMRQ](img/38eddad1bb046d84ae56bf8b17e3f48d.png)

How many coffees did you drink so far? / Photo by [Ozgu Ozden](https://unsplash.com/photos/7X96RNhpxBc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/coffee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

让我们看一个更一般的例子:

```
// BAD EXAMPLE
class Viking {
  constructor(name) {
    this.name = name
  }

  prepareForBattle(increaseCount) {
    console.log(`I am ${this.name}! Let's go fighting!`)
    increaseCount()
  }
}

class Battle {
  constructor(vikings) {
    this.vikings = vikings
    this.preparedVikingsCount = 0

    this.vikings.forEach(viking => {
      viking.prepareForBattle(this.increaseCount)
    })
  }

  increaseCount() {
    this.preparedVikingsCount++
    console.log(`${this.preparedVikingsCount} vikings are now ready to fight!`)
  }
}

const vikingOne = new Viking('Olaf')
const vikingTwo = new Viking('Odin')

new Battle([vikingOne, vikingTwo])
```

我们正在把`increaseCount`从一个班级传到另一个班级。当我们在`Viking`中调用`increaseCount`方法时，我们已经改变了上下文，而`this`实际上指向了`Viking`，这意味着我们的`increaseCount`方法不会像预期的那样工作。

#### 解决方案—绑定

对我们来说，最简单的解决方案是`bind`将从我们的原始对象或类中传递出来的方法。有不同的方法可以绑定函数，但是最常用的方法(也是在 React 中)是在构造函数中绑定它。所以我们必须在第 18 行之前的`Battle`构造函数中添加这一行:

```
this.increaseCount = this.increaseCount.bind(this)
```

您可以将任何函数绑定到任何上下文。这并不意味着您必须始终将函数绑定到声明它的上下文中(然而，这是最常见的情况)。相反，您可以将其绑定到另一个上下文。使用`bind`，你总是**为函数声明**设置上下文。这意味着对该函数的所有调用都将接收绑定的上下文作为`this`。还有另外两个设置上下文的助手。

> 箭头函数`()=> {} `自动将函数绑定到声明上下文

![1*acg_8b0T63Aiv8TAbtn5_w](img/d124f8461276ce411b813521e37d8406.png)

Photo by [Mario Klassen](https://unsplash.com/photos/kMMY3V6IUrw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/pointing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

#### 申请并致电

它们做的事情基本相同，只是语法不同。对于这两种情况，都将上下文作为第一个参数传递。`apply`接受其他参数的数组，使用`call`你可以用逗号分隔其他参数。现在他们做什么？这两种方法都为**一个特定的函数调用**设置了上下文。在没有`call`的情况下调用函数时，上下文被设置为默认上下文(甚至是绑定上下文)。这里有一个例子:

```
class Salad {
  constructor(type) {
    this.type = type
  }
}

function showType() {
  console.log(`The context's type is ${this.type}`)
}

const fruitSalad = new Salad('fruit')
const greekSalad = new Salad('greek')

showType.call(fruitSalad) // The context's type is fruit
showType.call(greekSalad) // The context's type is greek

showType() // The context's type is undefined
```

你能猜出最后一次`showType()`通话的背景吗？

…

..

。

你说的没错，就是最外层的范围，`window`。因此，`type`就是`undefined`，没有`window.type`

这就是了，希望你现在对如何在 JavaScript 中使用`this`有一个清晰的理解。欢迎在评论中留下对下一篇文章的建议。

*关于作者:Lukas Gisder-Dubé作为首席技术官共同创立并领导了一家初创公司 1 年半，建立了技术团队和架构。离开这家初创公司后，他在 T2 的 Ironhack 担任首席讲师，教授编程，现在正在柏林建立一家初创机构咨询公司。查看 [dube.io](https://dube.io) 了解更多信息。*

![1*p-l0Cee1IHvX0RQkVTOceQ](img/65e37b79b649b51075cca3751bdeaf5b.png)