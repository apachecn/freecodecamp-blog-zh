# 编程范例——初学者的范例

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-programming-paradigms/>

大家好！在这篇文章中，我们将看一看编程范例，这是一个用来描述组织编程的流行方式或风格的花哨标题。

我将试着把它分成几个部分，并对每个范例给出一个简单的解释。这样，当人们说“面向对象”、“函数式”或“声明式”时，你就能理解他们在说什么。

尽管我们也将看到一些伪代码和代码示例，但这将是一个肤浅而简短的理论介绍。

我计划在未来用实际的 JavaScript 例子深入探索每一种范式，所以如果你对这类文章感兴趣，请关注我(底部的链接)。；)

我们走吧！

## 目录

*   [什么是编程范例](#what-is-a-programming-paradigm)
*   [不是什么编程范例](#what-a-programming-paradigm-is-not)
*   我为什么要在乎？
*   [流行的编程范例](#popular-programming-paradigms)
    *   [命令式编程](#imperative-programming)
    *   [程序编程](#procedural-programming)
    *   [功能编程](#functional-programming)
    *   [声明式编程](#declarative-programming)
    *   [面向对象编程](#object-oriented-programming)
*   [综述](#roundup)

# 什么是编程范例？

编程范例是组织给定程序或编程语言的不同方式或风格。每个范例都由某些结构、特性和关于如何解决常见编程问题的观点组成。

为什么会有许多不同的编程范例的问题类似于为什么会有许多编程语言。某些范例更适合于某些类型的问题，因此对不同类型的项目使用不同的范例是有意义的。

此外，构成每个范例的实践也随着时间的推移而发展。由于软件和硬件的进步，出现了以前不存在的不同方法。

最后，我认为，还有人类的创造力。作为一个物种，我们只是喜欢创造东西，改进别人在过去建造的东西，并根据我们的偏好或对我们来说更有效的东西来调整工具。

所有这些导致了这样一个事实，即今天当我们想要编写和构造一个给定的程序时，我们有许多选择。🥸

# 编程范例不是什么

编程范式不是语言或工具。你不能用一个范例来“建造”任何东西。它们更像是许多人认同、遵循和扩展的一套理想和指导方针。

编程语言并不总是绑定到特定的范式。有些语言在构建时就已经考虑到了某种范式，并且具有比其他语言更有利于这种编程的特性(Haskel 和 functional programming 就是一个很好的例子)。

但是也有“多范例”语言，这意味着你可以修改你的代码来适应某种范例(JavaScript 和 Python 就是很好的例子)。

同时，编程范例并不相互排斥，也就是说，您可以毫无问题地同时使用不同范例的实践。

# 我为什么要在乎？

![whatever-yeah-1](img/1191b9573cafc3cc01bb00f1a701e952.png)

简答:常识。

长回答:我发现理解编程的多种方式很有趣。探索这些话题是打开你思维的好方法，有助于你跳出框框，跳出你已经知道的工具去思考。

此外，这些术语在编码领域中经常使用，因此有一个基本的理解将有助于您更好地理解其他主题。

# 流行的编程范例

既然我们已经介绍了什么是编程范例，什么不是，那么让我们来看看最流行的编程范例，解释它们的主要特征，并对它们进行比较。

请记住，这个列表并不详尽。还有其他编程范例没有在这里讨论，尽管我将讨论最流行和最广泛使用的范例。

## 命令式编程

命令式编程由一组详细的指令组成，这些指令按照给定的顺序给计算机执行。之所以称之为“命令性的”,是因为作为程序员，我们以一种非常具体的方式确切地规定了计算机必须做什么。

命令式编程侧重于描述*一个程序如何*一步一步地运行。

假设你想烤一个蛋糕。你的命令式程序可能看起来像这样(我不是一个伟大的厨师，所以不要评价我😒):

```
1- Pour flour in a bowl
2- Pour a couple eggs in the same bowl
3- Pour some milk in the same bowl
4- Mix the ingredients
5- Pour the mix in a mold
6- Cook for 35 minutes
7- Let chill
```

使用一个实际的代码示例，假设我们想要过滤一个数字数组，只保留大于 5 的元素。我们的命令式代码可能如下所示:

```
const nums = [1,4,3,6,7,8,9,2]
const result = []

for (let i = 0; i < nums.length; i++) {
    if (nums[i] > 5) result.push(nums[i])
}

console.log(result) // Output: [ 6, 7, 8, 9 ]
```

请注意，我们告诉程序遍历数组中的每个元素，将项目值与 5 进行比较，如果项目大于 5，则将其推入数组。

我们在指令中变得详细而具体，这就是命令式编程所代表的。

## 过程程序设计

过程编程是命令式编程的派生，增加了函数的特性(也称为“过程”或“子例程”)。

在过程化编程中，鼓励用户将程序执行细分为多个功能，以此来提高模块性和组织性。

以 cake 为例，过程化编程可能如下所示:

```
function pourIngredients() {
    - Pour flour in a bowl
    - Pour a couple eggs in the same bowl
    - Pour some milk in the same bowl
}

function mixAndTransferToMold() {
    - Mix the ingredients
    - Pour the mix in a mold
}

function cookAndLetChill() {
    - Cook for 35 minutes
    - Let chill
}

pourIngredients()
mixAndTransferToMold()
cookAndLetChill()
```

你可以看到，由于函数的实现，我们只需阅读文件末尾的三个函数调用，就可以很好地了解我们的程序做了什么。

这种简化和抽象是过程化编程的好处之一。但是在函数中，我们仍然得到相同的旧命令代码。

## 函数式编程

函数式编程将函数的概念拓展了一点。

在函数式编程中，函数被视为**一等公民**，这意味着它们可以被赋给变量，作为参数传递，并从其他函数返回。

另一个关键概念是**纯函数**的概念。一个**纯**函数是一个仅仅依赖于它的输入来产生结果的函数。给定相同的输入，它将总是产生相同的结果。此外，它不会产生副作用(函数环境之外的任何变化)。

考虑到这些概念，函数式编程鼓励大部分用函数编写的程序(惊讶吧😲).它还支持这样一种观点，即代码模块化和没有副作用使得在代码库中识别和分离职责变得更加容易。因此，这提高了代码的可维护性。

回到数组过滤的例子，我们可以看到，在命令式范例中，我们可能会使用一个外部变量来存储函数的结果，这可以被认为是一个副作用。

```
const nums = [1,4,3,6,7,8,9,2]
const result = [] // External variable

for (let i = 0; i < nums.length; i++) {
    if (nums[i] > 5) result.push(nums[i])
}

console.log(result) // Output: [ 6, 7, 8, 9 ]
```

要将它转换成函数式编程，我们可以这样做:

```
const nums = [1,4,3,6,7,8,9,2]

function filterNums() {
    const result = [] // Internal variable

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] > 5) result.push(nums[i])
    }

    return result
}

console.log(filterNums()) // Output: [ 6, 7, 8, 9 ]
```

这几乎是相同的代码，但是我们将迭代封装在一个函数中，其中还存储了结果数组。这样，我们可以确保函数不会修改其范围之外的任何内容。它只创建一个变量来处理自己的信息，一旦执行完毕，这个变量也消失了。

## 声明式编程

声明式编程就是隐藏复杂性，让编程语言更接近人类语言和思维。这与命令式编程正好相反，程序员不给出关于计算机应该如何执行任务的指令，而是告诉 T2 需要什么样的结果。

这一点用一个例子会清楚得多。按照同样的数组过滤规则，声明性方法可能是:

```
const nums = [1,4,3,6,7,8,9,2]

console.log(nums.filter(num => num > 5)) // Output: [ 6, 7, 8, 9 ]
```

请注意，使用 filter 函数，我们并没有明确地告诉计算机迭代数组或将值存储在单独的数组中。我们只说我们想要什么(“过滤器”)和要满足的条件(“num > 5”)。

这样做的好处是更容易阅读和理解，而且通常写起来更短。JavaScript 的`filter`、`map`、`reduce`和`sort`函数是声明性代码的好例子。

另一个很好的例子是像 React 这样的现代 JS 框架/库。以这段代码为例:

```
<button onClick={() => console.log('You clicked me!')}>Click me</button>
```

这里我们有一个按钮元素，带有一个事件侦听器，当单击按钮时，该事件侦听器触发 console.log 函数。

JSX 语法(React 使用的语法)将 HTML 和 JS 混合在一起，这使得编写应用程序更加容易和快速。但这不是浏览器读取和执行的内容。React 代码随后被编译成普通的 HTML 和 JS，这就是现实中浏览器运行的内容。

JSX 是声明性的，从某种意义上说，它的目的是给开发者一个更友好、更有效的界面。

关于声明式编程，需要注意的一件重要事情是，在幕后，计算机无论如何都将这些信息作为命令性代码来处理。

以数组为例，计算机仍然像在 for 循环中一样迭代数组，但是作为程序员，我们不需要直接编写代码。声明式编程所做的就是**隐藏**程序员看不到的复杂性。

这里有一个命令式编程和声明式编程的比较。

## 面向对象编程

最流行的编程范例之一是面向对象编程(OOP)。

OOP 的核心概念是将关注点分离成编码为对象的实体。每个实体都将一组给定的信息(属性)和实体可以执行的动作(方法)组合在一起。

OOP 大量使用类(这是一种从程序员设定的蓝图或样板开始创建新对象的方法)。从类创建的对象称为实例。

按照我们的伪代码烹饪示例，现在假设我们的面包店有一名主厨(名为 Frank)和一名助理厨师(名为 Anthony ),他们每个人在烘焙过程中都有一定的职责。如果我们使用 OOP，我们的程序可能会像这样。

```
// Create the two classes corresponding to each entity
class Cook {
	constructor constructor (name) {
        this.name = name
    }

    mixAndBake() {
        - Mix the ingredients
    	- Pour the mix in a mold
        - Cook for 35 minutes
    }
}

class AssistantCook {
    constructor (name) {
        this.name = name
    }

    pourIngredients() {
        - Pour flour in a bowl
        - Pour a couple eggs in the same bowl
        - Pour some milk in the same bowl
    }

    chillTheCake() {
    	- Let chill
    }
}

// Instantiate an object from each class
const Frank = new Cook('Frank')
const Anthony = new AssistantCook('Anthony')

// Call the corresponding methods from each instance
Anthony.pourIngredients()
Frank.mixAndBake()
Anthony.chillTheCake()
```

OOP 的好处在于，通过清晰地分离关注点和责任，它促进了对程序的理解。

在这个例子中，我只是触及了 OOP 的许多特性的表面。如果你想了解更多，这里有两个很棒的视频解释 OOP 的基础:

*   [OOP 视频 1](https://www.youtube.com/watch?v=cg1xvFy1JQQ)
*   [OOP 视频 2](https://www.youtube.com/watch?v=pTB0EiLXUC8)

这里有一个命令式、函数式和面向对象编程的很好的比较。

## 综述

正如我们所见，编程范例是我们面对编程问题和组织代码的不同方式。

命令式、过程式、函数式、声明式和面向对象的范例是当今最流行和最广泛使用的范例。了解它们的基础知识有助于了解一般知识，也有助于更好地理解编码世界的其他主题。

一如既往，我希望你喜欢这篇文章，并学到一些新东西。如果你愿意，你也可以在 [linkedin](https://www.linkedin.com/in/germancocca/) 或 [twitter](https://twitter.com/CoccaGerman) 上关注我。

干杯，下期再见！=D

![200](img/d7fb567313866c699b75ffffe497475d.png)