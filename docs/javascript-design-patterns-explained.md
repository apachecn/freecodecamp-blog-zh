# JavaScript 设计模式——用例子解释

> 原文：<https://www.freecodecamp.org/news/javascript-design-patterns-explained/>

大家好！在这篇文章中，我将解释什么是设计模式，以及它们为什么有用。

我们还将浏览一些最流行的设计模式，并为每种模式给出例子。我们走吧！

## 目录

*   什么是设计模式？
*   [创意设计模式](#creational-design-patterns)
    *   [单例模式](#singleton-pattern)
    *   [工厂方法模式](#factory-method-pattern)
    *   [抽象工厂模式](#abstract-factory-pattern)
    *   [构建器模式](#builder-pattern)
    *   [原型图案](#prototype-pattern)
*   [结构设计模式](#structural-design-patterns)
    *   [适配器模式](#adapter-pattern)
    *   [装饰图案](#decorator-pattern)
    *   [立面图案](#facade-pattern)
    *   [代理模式](#proxy-pattern)
*   [行为设计模式](#behavioral-design-patterns)
    *   [责任链模式](#chain-of-responsibility-pattern)
    *   [迭代器模式](#iterator-pattern)
    *   [观察者模式](#observer-pattern)
*   [综述](#roundup)

# 什么是设计模式？

设计模式是由四名 C++工程师于 1994 年出版的《设计模式:可重用面向对象软件的元素》一书推广开来的。

这本书探讨了面向对象编程的能力和缺陷，并描述了 23 种可以用来解决常见编程问题的有用模式。

这些模式**不是算法或具体实现**。它们更像是**想法、观点和抽象**，在某些情况下可以用来解决特定类型的问题。

模式的具体实现可能因许多不同的因素而异。但重要的是它们背后的概念，以及它们如何帮助我们实现更好的问题解决方案。

话虽如此，请记住这些模式是在考虑 OOP C++编程的情况下想出来的。当涉及到更现代的语言，如 JavaScript 或其他编程范式时，这些模式可能不是同样有用，甚至可能给我们的代码添加不必要的样板文件。

尽管如此，我认为把它们作为一般的编程知识来了解是有好处的。

补充说明:如果你不熟悉[编程范例](https://www.freecodecamp.org/news/an-introduction-to-programming-paradigms/)或 [OOP](https://www.freecodecamp.org/news/object-oriented-javascript-for-beginners/) ，我最近写了两篇关于这些主题的文章。😉

无论如何...现在我们已经介绍完了，设计模式被分为三个主要类别:**创建、结构和行为模式**。让我们简单地探讨一下它们。🧐

# 创造性设计模式

创建模式由用于创建对象的不同机制组成。

## 单一模式

**Singleton** 是一种确保一个类只有一个不可变实例的设计模式。简单地说，单例模式由一个不能被复制或修改的对象组成。当我们希望我们的应用程序有一些不变的单一*事实点*时，这通常是有用的。

比方说，我们希望在一个对象中包含应用程序的所有配置。我们不允许对该对象进行任何复制或修改。

实现这种模式的两种方法是使用对象文字和类:

```
const Config = {
  start: () => console.log('App has started'),
  update: () => console.log('App has updated'),
}

// We freeze the object to prevent new properties being added and existing properties being modified or removed
Object.freeze(Config)

Config.start() // "App has started"
Config.update() // "App has updated"

Config.name = "Robert" // We try to add a new key
console.log(Config) // And verify it doesn't work: { start: [Function: start], update: [Function: update] }
```

Using an object literal

```
class Config {
    constructor() {}
    start(){ console.log('App has started') }  
    update(){ console.log('App has updated') }
}

const instance = new Config()
Object.freeze(instance)
```

Using classes

## 工厂方法模式

**工厂方法**模式提供了一个创建对象的接口，这些对象在创建后可以被修改。最酷的是，创建对象的逻辑集中在一个地方，简化并更好地组织了代码。

这种模式被大量使用，也可以通过两种不同的方式实现，通过类或工厂函数(返回对象的函数)。

```
class Alien {
    constructor (name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}

const alien1 = new Alien("Ali", "I'm Ali the alien!")
console.log(alien1.name) // output: "Ali"
```

Using classes

```
function Alien(name, phrase) {
    this.name = name
    this.phrase = phrase
    this.species = "alien"
}

Alien.prototype.fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
Alien.prototype.sayPhrase = () => console.log(this.phrase)

const alien1 = new Alien("Ali", "I'm Ali the alien!")

console.log(alien1.name) // output "Ali"
console.log(alien1.phrase) // output "I'm Ali the alien!"
alien1.fly() // output "Zzzzzziiiiiinnnnnggggg"
```

Using a factory function

## 抽象工厂模式

**抽象工厂**模式允许我们在不指定具体类的情况下产生相关对象的系列。当我们需要创建只共享一些属性和方法的对象时，这很有用。

它的工作方式是通过呈现一个抽象的工厂，客户端与之交互。那个**抽象工厂**调用相应的**具体工厂**给出相应的逻辑。这个具体的工厂是返回最终对象的工厂。

基本上，它只是在工厂方法模式上添加了一个抽象层，这样我们就可以创建许多不同类型的对象，但仍然可以与单个工厂函数或类进行交互。

让我们看一个例子。假设我们正在为一家汽车公司建模，这家公司当然生产汽车，但也生产摩托车和卡车。

```
// We have a class or "concrete factory" for each vehicle type
class Car {
    constructor () {
        this.name = "Car"
        this.wheels = 4
    }
    turnOn = () => console.log("Chacabúm!!")
}

class Truck {
    constructor () {
        this.name = "Truck"
        this.wheels = 8
    }
    turnOn = () => console.log("RRRRRRRRUUUUUUUUUMMMMMMMMMM!!")
}

class Motorcycle {
    constructor () {
        this.name = "Motorcycle"
        this.wheels = 2
    }
    turnOn = () => console.log("sssssssssssssssssssssssssssssshhhhhhhhhhham!!")
}

// And and abstract factory that works as a single point of interaction for our clients
// Given the type parameter it receives, it will call the corresponding concrete factory
const vehicleFactory = {
    createVehicle: function (type) {
        switch (type) {
            case "car":
                return new Car()
            case "truck":
                return new Truck()
            case "motorcycle":
                return new Motorcycle()
            default:
                return null
        }
    }
}

const car = vehicleFactory.createVehicle("car") // Car { turnOn: [Function: turnOn], name: 'Car', wheels: 4 }
const truck = vehicleFactory.createVehicle("truck") // Truck { turnOn: [Function: turnOn], name: 'Truck', wheels: 8 }
const motorcycle = vehicleFactory.createVehicle("motorcycle") // Motorcycle { turnOn: [Function: turnOn], name: 'Motorcycle', wheels: 2 }
```

## 构建器模式

**构建器**模式用于在“步骤”中创建对象。通常我们会有一些函数或方法来为我们的对象添加某些属性或方法。

这个模式很酷的一点是，我们将属性和方法的创建分离到不同的实体中。

如果我们有一个类或一个工厂函数，我们实例化的对象将总是拥有在该类/工厂中声明的所有属性和方法。但是使用构建器模式，我们可以创建一个对象，并且只对它应用我们需要的“步骤”，这是一种更灵活的方法。

这与[物体构图](https://www.youtube.com/watch?v=wfMtDGfHWpA&t=3s)有关，这个话题我在这里讲过[。](https://www.freecodecamp.org/news/object-oriented-javascript-for-beginners/#object-composition)

```
// We declare our objects
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

const bug2 = {
    name: "Martiniano Buggland",
    phrase: "Can't touch this! Na na na na..."
}

// These functions take an object as parameter and add a method to them
const addFlyingAbility = obj => {
    obj.fly = () => console.log(`Now ${obj.name} can fly!`)
}

const addSpeechAbility = obj => {
    obj.saySmthg = () => console.log(`${obj.name} walks the walk and talks the talk!`)
}

// Finally we call the builder functions passing the objects as parameters
addFlyingAbility(bug1)
bug1.fly() // output: "Now Buggy McFly can fly!"

addSpeechAbility(bug2)
bug2.saySmthg() // output: "Martiniano Buggland walks the walk and talks the talk!"
```

## 原型模式

**原型**模式允许您使用另一个对象作为蓝图来创建一个对象，继承它的属性和方法。

如果你接触 JavaScript 已经有一段时间了，你可能熟悉[原型继承](https://www.freecodecamp.org/news/prototypes-and-inheritance-in-javascript/)以及 JavaScript 如何围绕它工作。

最终的结果与我们通过使用类得到的结果非常相似，但是有更多的灵活性，因为属性和方法可以在对象之间共享，而不依赖于同一个类。

```
// We declare our prototype object with two methods
const enemy = {
    attack: () => console.log("Pim Pam Pum!"),
    flyAway: () => console.log("Flyyyy like an eagle!")
}

// We declare another object that will inherit from our prototype
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

// With setPrototypeOf we set the prototype of our object
Object.setPrototypeOf(bug1, enemy)

// With getPrototypeOf we read the prototype and confirm the previous has worked
console.log(Object.getPrototypeOf(bug1)) // { attack: [Function: attack], flyAway: [Function: flyAway] }

console.log(bug1.phrase) // Your debugger doesn't work with me!
console.log(bug1.attack()) // Pim Pam Pum!
console.log(bug1.flyAway()) // Flyyyy like an eagle!
```

# 结构设计模式

结构模式指的是如何将对象和类组装成更大的结构。

## 适配器模式

**适配器**允许两个接口不兼容的对象相互交互。

比方说，您的应用程序查询一个返回 [XML](https://www.freecodecamp.org/news/what-is-an-xml-file-how-to-open-xml-files-and-the-best-xml-viewers/) 的 API，并将该信息发送给另一个 API 来处理该信息。但是处理 API 期望 [JSON](https://www.freecodecamp.org/news/what-is-json-a-json-file-example/) 。由于两个接口不兼容，您不能发送收到的信息。你需要先*适应它*。😉

我们可以用一个更简单的例子来想象同样的概念。假设我们有一个城市数组和一个函数，该函数返回这些城市中最大数量的居民。我们的数组中的居住者数量是以百万计的，但是我们有一个新的城市要添加，它的居住者没有百万转换:

```
// Our array of cities
const citiesHabitantsInMillions = [
    { city: "London", habitants: 8.9 },
    { city: "Rome", habitants: 2.8 },
    { city: "New york", habitants: 8.8 },
    { city: "Paris", habitants: 2.1 },
] 

// The new city we want to add
const BuenosAires = {
    city: "Buenos Aires",
    habitants: 3100000
}

// Our adapter function takes our city and converts the habitants property to the same format all the other cities have
const toMillionsAdapter = city => { city.habitants = parseFloat((city.habitants/1000000).toFixed(1)) }

toMillionsAdapter(BuenosAires)

// We add the new city to the array
citiesHabitantsInMillions.push(BuenosAires)

// And this function returns the largest habitants number
const MostHabitantsInMillions = () => {
    return Math.max(...citiesHabitantsInMillions.map(city => city.habitants))
}

console.log(MostHabitantsInMillions()) // 8.9
```

## 装饰图案

**Decorator** 模式允许您将新的行为附加到对象上，方法是将它们放在包含这些行为的包装对象中。如果您对 React 和高阶元件(HOC)有些熟悉，这种方法可能会让您想起一些东西。

从技术上讲，React 中的组件是函数，而不是对象。但是，如果我们考虑一下 React Context 或 [Memo](https://www.freecodecamp.org/news/memoization-in-javascript-and-react/) 我们可以看到，我们正在将一个组件作为子组件传递给这个 HOC，由于这个子组件能够访问某些特性。

在此示例中，我们可以看到 ContextProvider 组件正在接收作为道具的子组件:

```
 import { useState } from 'react'
import Context from './Context'

const ContextProvider: React.FC = ({children}) => {

    const [darkModeOn, setDarkModeOn] = useState(true)
    const [englishLanguage, setEnglishLanguage] = useState(true)

    return (
        <Context.Provider value={{
            darkModeOn,
            setDarkModeOn,
            englishLanguage,
            setEnglishLanguage
        }} >
            {children}
        </Context.Provider>
    )
}

export default ContextProvider
```

然后，我们围绕它包装整个应用程序:

```
export default function App() {
  return (
    <ContextProvider>
      <Router>

        <ErrorBoundary>
          <Suspense fallback={<></>}>
            <Header />
          </Suspense>

          <Routes>
              <Route path='/' element={<Suspense fallback={<></>}><AboutPage /></Suspense>}/>

              <Route path='/projects' element={<Suspense fallback={<></>}><ProjectsPage /></Suspense>}/>

              <Route path='/projects/helpr' element={<Suspense fallback={<></>}><HelprProject /></Suspense>}/>

              <Route path='/projects/myWebsite' element={<Suspense fallback={<></>}><MyWebsiteProject /></Suspense>}/>

              <Route path='/projects/mixr' element={<Suspense fallback={<></>}><MixrProject /></Suspense>}/>

              <Route path='/projects/shortr' element={<Suspense fallback={<></>}><ShortrProject /></Suspense>}/>

              <Route path='/curriculum' element={<Suspense fallback={<></>}><CurriculumPage /></Suspense>}/>

              <Route path='/blog' element={<Suspense fallback={<></>}><BlogPage /></Suspense>}/>

              <Route path='/contact' element={<Suspense fallback={<></>}><ContactPage /></Suspense>}/>
          </Routes>
        </ErrorBoundary>

      </Router>
    </ContextProvider>
  )
}
```

稍后，使用`useContext`钩子，我可以从我的应用程序中的任何组件访问上下文中定义的状态。

```
 const AboutPage: React.FC = () => {

    const { darkModeOn, englishLanguage } = useContext(Context)

    return (...)
}

export default AboutPage
```

同样，这可能不是本书作者在描述这种模式时想到的精确实现，但我相信想法是相同的。将一个对象放在另一个对象中，以便它可以访问某些功能。；)

## 立面图案

**Facade** 模式为一个库、一个框架或任何其他复杂的类集提供了一个简化的接口。

良好的...我们可能会举出很多例子，对吧？我的意思是，React 本身或任何用于与软件开发相关的大量库。特别是当我们想到[声明式编程](https://www.freecodecamp.org/news/an-introduction-to-programming-paradigms/#declarative-programming)的时候，它都是关于提供抽象来隐藏开发者眼中的复杂性。

一个简单的例子是 JavaScript 的`map`、`sort`、`reduce`和`filter`函数，它们都像引擎盖下的`for`循环一样工作。

另一个例子可以是现在用于 UI 开发的任何库，比如 [MUI](https://mui.com/) 。正如我们在下面的例子中看到的，这些库为我们提供了组件，这些组件带来了内置的特性和功能，帮助我们更快更容易地构建代码。

但是所有这些在编译时都会变成简单的 HTML 元素，这是浏览器唯一能理解的东西。这些组件只是为了让我们的生活变得更简单而抽象的。

![thewolfofwallstreet-fairydust](img/adc309c0154e29125825ae14046942a6.png)

A facade...

```
import * as React from 'react';
import Table from '@mui/material/Table';
import TableBody from '@mui/material/TableBody';
import TableCell from '@mui/material/TableCell';
import TableContainer from '@mui/material/TableContainer';
import TableHead from '@mui/material/TableHead';
import TableRow from '@mui/material/TableRow';
import Paper from '@mui/material/Paper';

function createData(
  name: string,
  calories: number,
  fat: number,
  carbs: number,
  protein: number,
) {
  return { name, calories, fat, carbs, protein };
}

const rows = [
  createData('Frozen yoghurt', 159, 6.0, 24, 4.0),
  createData('Ice cream sandwich', 237, 9.0, 37, 4.3),
  createData('Eclair', 262, 16.0, 24, 6.0),
  createData('Cupcake', 305, 3.7, 67, 4.3),
  createData('Gingerbread', 356, 16.0, 49, 3.9),
];

export default function BasicTable() {
  return (
    <TableContainer component={Paper}>
      <Table sx={{ minWidth: 650 }} aria-label="simple table">
        <TableHead>
          <TableRow>
            <TableCell>Dessert (100g serving)</TableCell>
            <TableCell align="right">Calories</TableCell>
            <TableCell align="right">Fat&nbsp;(g)</TableCell>
            <TableCell align="right">Carbs&nbsp;(g)</TableCell>
            <TableCell align="right">Protein&nbsp;(g)</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map((row) => (
            <TableRow
              key={row.name}
              sx={{ '&:last-child td, &:last-child th': { border: 0 } }}
            >
              <TableCell component="th" scope="row">
                {row.name}
              </TableCell>
              <TableCell align="right">{row.calories}</TableCell>
              <TableCell align="right">{row.fat}</TableCell>
              <TableCell align="right">{row.carbs}</TableCell>
              <TableCell align="right">{row.protein}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

## 代理模式

**代理**模式为另一个对象提供了一个替代或占位符。其思想是控制对原始对象的访问，在请求到达实际的原始对象之前或之后执行某种操作。

同样，如果你熟悉 ExpressJS ,这可能会让你想起什么。Express 是一个用来开发 NodeJS APIs 的框架，它拥有的一个特性就是中间件的使用。中间件只不过是我们可以在任何请求到达我们的端点之前、中间或之后执行的代码片段。

让我们看一个例子。这里我有一个验证认证令牌的函数。不要太在意它是怎么做到的。只需知道它接收令牌作为参数，一旦完成，它就调用`next()`函数。

```
const jwt = require('jsonwebtoken')

module.exports = function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization']
    const token = authHeader && authHeader.split(' ')[1]

    if (token === null) return res.status(401).send(JSON.stringify('No access token provided'))

    jwt.verify(token, process.env.TOKEN_SECRET, (err, user) => {
      if (err) return res.status(403).send(JSON.stringify('Wrong token provided'))
      req.user = user
      next()
    })
}
```

这个函数是一个中间件，我们可以通过以下方式在我们的 API 的任何端点中使用它。我们只是将中间件放在端点地址之后，端点函数声明之前:

```
router.get('/:jobRecordId', authenticateToken, async (req, res) => {
  try {
    const job = await JobRecord.findOne({_id: req.params.jobRecordId})
    res.status(200).send(job)

  } catch (err) {
    res.status(500).json(err)
  }
})
```

这样，如果没有提供令牌或提供了错误的令牌，中间件将返回相应的错误响应。如果提供了有效的令牌，中间件将调用`next()`函数，接下来将执行端点函数。

我们可以在端点内部编写相同的代码，并在那里验证令牌，而不用担心中间件或任何东西。但是现在我们有了一个可以在许多不同的端点中重用的抽象。😉

再说一次，这可能不是作者心中的精确想法，但我相信这是一个有效的例子。我们正在控制一个对象的访问，这样我们就可以在特定的时刻执行操作。

# 行为设计模式

行为模式控制着不同对象之间的交流和职责分配。

## 责任链模式

**责任链**沿着处理程序链传递请求。每个处理程序决定要么处理请求，要么将请求传递给链中的下一个处理程序。

对于这种模式，我们可以使用与前面完全相同的例子，因为 Express 中的中间件是某种处理程序，要么处理请求，要么将其传递给下一个处理程序。

如果你想再举一个例子，想象一下任何一个系统，在这个系统中，你有特定的信息需要按照许多步骤进行处理。在每个步骤中，不同的实体负责执行一个动作，并且只有在满足特定条件的情况下，信息才会被传递给另一个实体。

使用 API 的典型前端应用程序可以作为一个例子:

*   我们有一个负责呈现 UI 组件的函数。
*   一旦呈现，另一个函数向 API 端点发出请求。
*   如果端点响应符合预期，则信息被传递给另一个函数，该函数以给定的方式对数据进行排序，并将其存储在一个变量中。
*   一旦该变量存储了所需的信息，另一个函数就负责在 UI 中呈现这些信息。

我们可以看到这里有许多不同的实体协作执行某项任务。他们每个人负责该任务的一个“步骤”，这有助于代码模块化和关注点分离。👌👌

## 迭代器模式

**迭代器**用于遍历集合中的元素。在当今使用的编程语言中，这听起来可能微不足道，但情况并非总是如此。

无论如何，我们可以使用的任何 JavaScript 内置函数来迭代数据结构(`for`、`forEach`、`for...of`、`for...in`、`map`、`reduce`、`filter`等等)都是迭代器模式的例子。

与任何[遍历算法](https://www.freecodecamp.org/news/introduction-to-algorithms-with-javascript-examples/#traversing-algorithms)一样，我们编写代码来遍历更复杂的[数据结构，如树或图](https://www.freecodecamp.org/news/data-structures-in-javascript-with-examples/)。

## 观察者模式

**observer** 模式允许您定义一个订阅机制，通知多个对象它们正在观察的对象发生的任何事件。基本上，这就像在一个给定的对象上有一个事件监听器，当那个对象执行我们正在监听的动作时，我们做一些事情。

React 的 useEffect 钩子可能是一个很好的例子。useEffect 所做的是在我们声明的时候执行一个给定的函数。

钩子分为两个主要部分，可执行函数和一组依赖项。如果数组为空，如下例所示，则每次呈现组件时都会执行该函数。

```
 useEffect(() => { console.log('The component has rendered') }, [])
```

如果我们在依赖数组中声明了任何变量，那么只有当这些变量改变时，函数才会执行。

```
 useEffect(() => { console.log('var1 has changed') }, [var1])
```

甚至普通的旧 JavaScript 事件监听器也可以被认为是观察者。此外，用于处理系统异步信息和事件的反应式编程和库 [RxJS](https://rxjs.dev/) 就是这种模式的很好例子。

# **综述**

如果你想更多地了解这个话题，我推荐这个 g [reat Fireship 视频](https://www.youtube.com/watch?v=tv-_1er1mWI)和[这个很棒的网站](https://refactoring.guru/)，在那里你可以找到非常详细的解释，并附有插图来帮助你理解每个模式。

一如既往，我希望你喜欢这篇文章，并学到一些新东西。如果你愿意，你也可以在 [LinkedIn](https://www.linkedin.com/in/germancocca/) 或 [Twitter](https://twitter.com/CoccaGerman) 上关注我。

干杯，下期再见！✌️

![See-ya-GIF](img/012a28a3e07592e1b48d7fefa0b708b3.png)