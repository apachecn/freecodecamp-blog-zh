# JavaScript 原型初学者指南

> 原文：<https://www.freecodecamp.org/news/a-beginners-guide-to-javascripts-prototype/>

[https://www.youtube.com/embed/XskMWBXNbp0?feature=oembed](https://www.youtube.com/embed/XskMWBXNbp0?feature=oembed)

如果不处理对象，你就无法在 JavaScript 中走得很远。它们几乎是 JavaScript 编程语言各个方面的基础。事实上，学习如何创建对象可能是你刚开始学习的第一件事。也就是说，为了最有效地学习 JavaScript 中的原型，我们将引导我们的 inner Jr. developer 回到基础。

对象是键/值对。创建一个对象最常见的方法是用花括号`{}`，你可以用点符号给一个对象添加属性和方法。

```
let animal = {}
animal.name = 'Leo'
animal.energy = 10

animal.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

animal.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

animal.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
} 
```

简单。现在，在我们的应用程序中，我们可能需要创建多个动物。很自然，下一步是将逻辑封装在一个函数中，当我们需要创建一个新的动物时，就可以调用这个函数。我们称这个模式为`Functional Instantiation`,我们称这个函数本身为“构造函数”,因为它负责“构造”一个新对象。

#### 功能实例化

```
function Animal (name, energy) {
  let animal = {}
  animal.name = name
  animal.energy = energy

  animal.eat = function (amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }

  animal.sleep = function (length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }

  animal.play = function (length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10) 
```

`"I thought this was an Advanced JavaScript course...?" - Your brain`

的确如此。我们会到达那里的。

现在，每当我们想要创建一个新的动物(或者更广义地说，一个新的“实例”)，我们所要做的就是调用我们的`Animal`函数，将动物的`name`和`energy`级别传递给它。这非常有效，而且非常简单。然而，您能发现这种模式的弱点吗？最大的也是我们试图解决的问题与三种方法有关- `eat`、`sleep`和`play`。这些方法不仅是动态的，而且是完全通用的。这意味着，我们没有理由像现在一样，每当创造一种新动物时，就重新创造那些方法。我们只是在浪费内存，让每个动物对象比它需要的要大。你能想到解决办法吗？如果每次我们创建一个新的动物时，我们不需要重新创建这些方法，而是将它们移动到它们自己的对象中，这样我们就可以让每一个动物都引用那个对象了。我们可以把这种模式叫做`Functional Instantiation with Shared Methods`，啰嗦但有描述性？‍♂️.

#### 共享方法的函数实例化

```
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = {}
  animal.name = name
  animal.energy = energy
  animal.eat = animalMethods.eat
  animal.sleep = animalMethods.sleep
  animal.play = animalMethods.play

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10) 
```

通过将共享方法移动到它们自己的对象中，并在我们的`Animal`函数中引用该对象，我们现在已经解决了内存浪费和过大动物对象的问题。

#### 对象.创建

让我们通过使用`Object.create`再次改进我们的例子。简单地说， **Object.create 允许您创建一个对象，该对象将在查找失败时委托给另一个对象**。换句话说，Object.create 允许您创建一个对象，每当对该对象的属性查找失败时，它可以咨询另一个对象，以查看该对象是否具有该属性。说了很多话。让我们看一些代码。

```
const parent = {
  name: 'Stacey',
  age: 35,
  heritage: 'Irish'
}

const child = Object.create(parent)
child.name = 'Ryan'
child.age = 7

console.log(child.name) // Ryan
console.log(child.age) // 7
console.log(child.heritage) // Irish 
```

所以在上面的例子中，因为`child`是用`Object.create(parent)`创建的，每当在`child`上有一个失败的属性查找时，JavaScript 将把那个查找委托给`parent`对象。这意味着即使`child`没有`heritage`属性，但`parent`有，当你登录`child.heritage`时，你会得到`parent`的遗产，即`Irish`。

现在`Object.create`在我们的工具库中，我们如何使用它来简化我们之前的`Animal`代码呢？我们可以使用 Object.create 来委托给`animalMethods`对象，而不是像现在这样一个一个地给动物添加所有的共享方法。为了听起来很聪明，我们把这个叫做`Functional Instantiation with Shared Methods and Object.create`？

#### 使用共享方法和 Object.create 的函数实例化

```
const animalMethods = {
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  },
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  },
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function Animal (name, energy) {
  let animal = Object.create(animalMethods)
  animal.name = name
  animal.energy = energy

  return animal
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)

leo.eat(10)
snoop.play(5) 
```

？所以现在当我们调用`leo.eat`时，JavaScript 会在`leo`对象上寻找`eat`方法。因为 Object.create，这个查找将会失败，它将委托给`animalMethods`对象，在那里它将找到`eat`。

到目前为止，一切顺利。尽管如此，我们仍然可以做一些改进。为了在实例间共享方法，不得不管理一个单独的对象(`animalMethods`)似乎有点“笨拙”。这似乎是一个你希望在语言本身中实现的常见特性。事实证明是的，这就是你在这里的全部原因。

那么 JavaScript 中的`prototype`到底是什么？简单地说，JavaScript 中的每个函数都有一个引用对象的`prototype`属性。虎头蛇尾，对吧？自己测试一下吧。

```
function doThing () {}
console.log(doThing.prototype) // {} 
```

如果不是创建一个单独的对象来管理我们的方法(就像我们对`animalMethods`所做的那样)，而是将这些方法放在`Animal`函数的原型上，会怎么样？那么我们要做的就是不用 Object.create 来委托给`animalMethods`，我们可以用它来委托给`Animal.prototype`。我们称这种模式为`Prototypal Instantiation`。

#### 原型实例化

```
function Animal (name, energy) {
  let animal = Object.create(Animal.prototype)
  animal.name = name
  animal.energy = energy

  return animal
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = Animal('Leo', 7)
const snoop = Animal('Snoop', 10)

leo.eat(10)
snoop.play(5) 
```

？？？希望你刚刚经历了一个重大的“啊哈”时刻。同样，`prototype`只是 JavaScript 中每个函数都有的属性，正如我们上面看到的，它允许我们在函数的所有实例中共享方法。我们所有的功能都是一样的，但是现在不用为所有的方法管理一个单独的对象，我们可以使用内置在`Animal`函数本身的另一个对象`Animal.prototype`。

* * *

## 让我们。走吧。更深。

此时我们知道三件事:

1.  如何创建构造函数？
2.  如何向构造函数的原型添加方法？
3.  如何使用 Object.create 将失败的查找委托给函数的原型。

这三项任务似乎是任何编程语言的基础。JavaScript 真的那么糟糕，没有更简单的“内置”方法来完成同样的事情吗？你可能已经猜到了，这是通过使用关键字`new`实现的。

我们采取的缓慢而有条理的方法的好处在于，您现在将对 JavaScript 中的关键字`new`到底在做什么有一个深刻的理解。

回头看看我们的`Animal`构造函数，两个最重要的部分是创建对象和返回它。如果没有用`Object.create`创建对象，我们将无法在查找失败时委托给函数的原型。如果没有`return`语句，我们永远也不会得到创建的对象。

```
function Animal (name, energy) {
  let animal = Object.create(Animal.prototype)
  animal.name = name
  animal.energy = energy

  return animal
} 
```

这里有一件关于`new`的很酷的事情——当你使用`new`关键字调用一个函数时，这两行是隐式地为你完成的(“在引擎盖下”)，并且被创建的对象被称为`this`。

使用注释来显示幕后发生的事情，并假设用关键字`new`调用`Animal`构造函数，它可以重写为这样。

```
function Animal (name, energy) {
  // const this = Object.create(Animal.prototype)

  this.name = name
  this.energy = energy

  // return this
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10) 
```

没有“引擎盖下”的评论

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10) 
```

同样，这种方法有效并且为我们创建了`this`对象的原因是因为我们用`new`关键字调用了构造函数。如果在调用函数时省略了`new`，那么这个`this`对象就不会被创建，也不会被隐式返回。我们可以在下面的例子中看到这个问题。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

const leo = Animal('Leo', 7)
console.log(leo) // undefined 
```

这种模式的名称是`Pseudoclassical Instantiation`。

如果 JavaScript 不是你的第一编程语言，你可能会有点不安。

“这家伙刚刚重新创建了一个更垃圾版本的类”——你

对于那些不熟悉的人，类允许你创建一个对象的蓝图。然后，无论何时创建该类的实例，您都会获得一个具有蓝图中定义的属性和方法的对象。

听起来熟悉吗？这基本上就是我们对上面的`Animal`构造函数所做的。然而，我们没有使用`class`关键字，而是使用一个常规的旧 JavaScript 函数来重新创建相同的功能。当然，这需要做一些额外的工作，也需要了解一些 JavaScript“幕后”发生的事情，但是结果是一样的。

好消息是。JavaScript 不是一种死亡的语言。它被 [TC-39 委员会](https://tylermcginnis.com/ecmascript/)不断改进和添加。这意味着即使 JavaScript 的初始版本不支持类，也没有理由不能将它们添加到官方规范中。事实上，TC-39 委员会正是这么做的。2015 年，EcmaScript(官方 JavaScript 规范)6 发布，支持类和`class`关键字。让我们看看上面的`Animal`构造函数在新的类语法下会是什么样子。

```
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10) 
```

很干净，对吧？

那么，如果这是创建类的新方法，为什么我们要花这么多时间重温旧方法呢？其原因是因为新的方式(带有`class`关键字)主要只是现有方式的“语法糖”,我们称之为伪经典模式。为了*完全*理解 ES6 类的便利语法，你首先必须理解伪经典模式。

* * *

至此，我们已经介绍了 JavaScript 原型的基础知识。这篇文章的其余部分将致力于理解与之相关的其他“值得了解”的话题。在另一篇文章中，我们将看看如何利用这些基础知识来理解 JavaScript 中的继承机制。

* * *

### 数组方法

我们在上面深入讨论了如果你想在一个类的实例间共享方法，你应该把这些方法贴在类(或函数)的原型上。如果我们看一下`Array`类，我们可以看到相同的模式。在历史上，您可能已经创建了这样的数组

```
const friends = [] 
```

事实证明，这只是创建了一个`Array`类的`new`实例。

```
const friendsWithSugar = []

const friendsWithoutSugar = new Array() 
```

你可能从未想过的一件事是，一个数组的每个实例是如何拥有所有内置的方法(`splice`、`slice`、`pop`等)？

正如你现在所知道的，这是因为那些方法存在于`Array.prototype`中，当你创建一个新的`Array`实例时，你使用了`new`关键字，该关键字在失败的查找中设置了对`Array.prototype`的委托。

我们可以通过简单地记录`Array.prototype`来查看数组的所有方法。

```
console.log(Array.prototype)

/*
  concat: ƒn concat()
  constructor: ƒn Array()
  copyWithin: ƒn copyWithin()
  entries: ƒn entries()
  every: ƒn every()
  fill: ƒn fill()
  filter: ƒn filter()
  find: ƒn find()
  findIndex: ƒn findIndex()
  forEach: ƒn forEach()
  includes: ƒn includes()
  indexOf: ƒn indexOf()
  join: ƒn join()
  keys: ƒn keys()
  lastIndexOf: ƒn lastIndexOf()
  length: 0n
  map: ƒn map()
  pop: ƒn pop()
  push: ƒn push()
  reduce: ƒn reduce()
  reduceRight: ƒn reduceRight()
  reverse: ƒn reverse()
  shift: ƒn shift()
  slice: ƒn slice()
  some: ƒn some()
  sort: ƒn sort()
  splice: ƒn splice()
  toLocaleString: ƒn toLocaleString()
  toString: ƒn toString()
  unshift: ƒn unshift()
  values: ƒn values()
*/ 
```

对象也存在完全相同的逻辑。在查找失败时，所有对象都将委托给`Object.prototype`，这就是为什么所有对象都有像`toString`和`hasOwnProperty`这样的方法。

### 静态方法

到目前为止，我们已经讨论了在类的实例之间共享方法的原因和方式。然而，如果我们有一个对类很重要的方法，但不需要跨实例共享，那会怎么样呢？例如，如果我们有一个函数，它接受一组`Animal`实例，并确定下一个需要喂哪一个呢？我们就叫它`nextToEat`。

```
function nextToEat (animals) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
} 
```

让`nextToEat`生活在`Animal.prototype`上是没有意义的，因为我们不想在所有实例之间共享它。相反，我们可以认为它更像是一个辅助方法。那么如果`nextToEat`不应该住在`Animal.prototype`上，我们应该把它放在哪里呢？显而易见的答案是，我们可以将`nextToEat`放在与`Animal`类相同的范围内，然后在需要时像平常一样引用它。

```
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

function nextToEat (animals) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)

console.log(nextToEat([leo, snoop])) // Leo 
```

这是可行的，但还有更好的方法。

每当您有一个特定于类本身的方法，但不需要在该类的实例之间共享时，您可以将它添加为该类的一个`static`属性。

```
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep(length) {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play(length) {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
  static nextToEat(animals) {
    const sortedByLeastEnergy = animals.sort((a,b) => {
      return a.energy - b.energy
    })

    return sortedByLeastEnergy[0].name
  }
} 
```

现在，因为我们添加了`nextToEat`作为类的`static`属性，它存在于`Animal`类本身(不是它的原型)上，并且可以使用`Animal.nextToEat`来访问。

```
const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)

console.log(Animal.nextToEat([leo, snoop])) // Leo 
```

因为我们在这篇文章中遵循了类似的模式，所以让我们看看如何使用 ES5 来完成同样的事情。在上面的例子中，我们看到了如何使用`static`关键字将方法直接放到类本身上。对于 ES5，同样的模式非常简单，只需手动将方法添加到函数对象中。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

Animal.nextToEat = function (nextToEat) {
  const sortedByLeastEnergy = animals.sort((a,b) => {
    return a.energy - b.energy
  })

  return sortedByLeastEnergy[0].name
}

const leo = new Animal('Leo', 7)
const snoop = new Animal('Snoop', 10)

console.log(Animal.nextToEat([leo, snoop])) // Leo 
```

### 获取对象的原型

不管您使用哪种模式来创建对象，都可以使用`Object.getPrototypeOf`方法来获得该对象的原型。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)
const prototype = Object.getPrototypeOf(leo)

console.log(prototype)
// {constructor: ƒ, eat: ƒ, sleep: ƒ, play: ƒ}

prototype === Animal.prototype // true 
```

从上面的代码中有两点很重要。

首先，你会注意到`proto`是一个有 4 个方法的对象，`constructor`、`eat`、`sleep`和`play`。有道理。我们使用`getPrototypeOf`传入实例，`leo`取回实例的原型，这是我们所有方法的所在。这也告诉了我们关于`prototype`的另一件我们还没有谈到的事情。默认情况下，`prototype`对象将有一个`constructor`属性，该属性指向创建实例的原始函数或类。这也意味着，因为 JavaScript 默认在原型上放置了一个`constructor`属性，所以任何实例都可以通过`instance.constructor`访问它们的构造函数。

第二个重要的收获是`Object.getPrototypeOf(leo) === Animal.prototype`。这也说得通。`Animal`构造函数有一个原型属性，我们可以在所有实例中共享方法，而`getPrototypeOf`允许我们看到实例本身的原型。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

const leo = new Animal('Leo', 7)
console.log(leo.constructor) // Logs the constructor function 
```

为了与我们之前讨论的`Object.create`联系起来，这样做的原因是因为`Animal`的任何实例都将在失败的查找中委托给`Animal.prototype`。所以当你试图访问`leo.constructor`，`leo`没有`constructor`属性，所以它会将查询委托给`Animal.prototype`，而后者确实有`constructor`属性。如果这一段没有意义，回去看看上面的`Object.create`。

您可能已经见过使用 __proto__ 来获取实例的原型。那是过去的遗迹。而是使用**object . getprototypeof(instance)**就像我们上面看到的。

### 确定属性是否存在于原型中

在某些情况下，您需要知道一个属性是存在于实例本身上，还是存在于对象委托的原型上。通过循环我们已经创建的`leo`对象，我们可以看到这一点。假设目标是循环遍历`leo`并记录它的所有键和值。使用一个`for in`循环，可能会像这样。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)

for(let key in leo) {
  console.log(`Key: ${key}. Value: ${leo[key]}`)
} 
```

你希望看到什么？很有可能，是这样的-

```
Key: name. Value: Leo
Key: energy. Value: 7 
```

然而，如果你运行代码，你会看到这个-

```
Key: name. Value: Leo
Key: energy. Value: 7
Key: eat. Value: function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}
Key: sleep. Value: function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}
Key: play. Value: function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
} 
```

这是为什么呢？一个`for in`循环将遍历对象本身及其委托的原型上的所有**可枚举属性**。因为默认情况下，添加到函数原型的任何属性都是可枚举的，所以我们不仅可以看到`name`和`energy`，还可以看到原型上的所有方法——`eat`、`sleep`和`play`。为了解决这个问题，我们或者需要指定所有的原型方法都是不可枚举的**或者**如果属性是在`leo`对象本身上，而不是在失败的查找中`leo`委托给的原型上，我们需要一种方法只访问 console.log。这就是`hasOwnProperty`可以帮助我们的地方。

`hasOwnProperty`是每个对象上的一个属性，返回一个布尔值，表明该对象是否将指定的属性作为自己的属性，而不是该对象委托给的原型的属性。这正是我们所需要的。现在有了这个新的知识，我们可以修改我们的代码来利用我们的`for in`循环中的`hasOwnProperty`。

```
...

const leo = new Animal('Leo', 7)

for(let key in leo) {
  if (leo.hasOwnProperty(key)) {
    console.log(`Key: ${key}. Value: ${leo[key]}`)
  }
} 
```

现在我们看到的只是在`leo`对象本身上的属性，而不是原型`leo`委托给它的属性。

```
Key: name. Value: Leo
Key: energy. Value: 7 
```

如果你仍然对`hasOwnProperty`有些困惑，这里有一些代码可以让你明白。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

const leo = new Animal('Leo', 7)

leo.hasOwnProperty('name') // true
leo.hasOwnProperty('energy') // true
leo.hasOwnProperty('eat') // false
leo.hasOwnProperty('sleep') // false
leo.hasOwnProperty('play') // false 
```

### 检查一个对象是否是一个类的实例

有时你想知道一个对象是否是一个特定类的实例。为此，您可以使用`instanceof`操作符。用例非常简单，但是如果你以前从未见过，实际的语法会有点奇怪。它是这样工作的

```
object instanceof Class 
```

如果`object`是`Class`的实例，上面的语句将返回 true，否则返回 false。回到我们的`Animal`例子，我们会有这样的东西。

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

function User () {}

const leo = new Animal('Leo', 7)

leo instanceof Animal // true
leo instanceof User // false 
```

`instanceof`的工作方式是检查对象的原型链中是否存在`constructor.prototype`。在上面的例子中，`leo instanceof Animal`是`true`，因为`Object.getPrototypeOf(leo) === Animal.prototype`。另外，`leo instanceof User`是`false`因为`Object.getPrototypeOf(leo) !== User.prototype`。

### 创建新的不可知构造函数

你能找出下面代码中的错误吗？

```
function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

const leo = Animal('Leo', 7) 
```

即使是经验丰富的 JavaScript 开发人员有时也会被上面的例子绊倒。因为我们使用了之前学过的`pseudoclassical pattern`，当调用`Animal`构造函数时，我们需要确保用`new`关键字调用它。如果我们不这样做，那么`this`关键字将不会被创建，也不会被隐式返回。

提醒一下，注释掉的行是在函数上使用`new`关键字时发生的事情。

```
function Animal (name, energy) {
  // const this = Object.create(Animal.prototype)

  this.name = name
  this.energy = energy

  // return this
} 
```

这似乎是一个太重要的细节，不能留给其他开发人员去记住。假设我们和其他开发人员在一个团队中工作，有没有一种方法可以确保我们的`Animal`构造函数总是被`new`关键字调用？事实证明是有的，而且是通过使用我们之前学过的`instanceof`操作符。

如果构造函数是用关键字`new`调用的，那么构造函数体内部的`this`将是构造函数本身的一个`instanceof`。那是许多大词。下面是一些代码。

```
function Animal (name, energy) {
  if (this instanceof Animal === false) {
    console.warn('Forgot to call Animal with the new keyword')
  }

  this.name = name
  this.energy = energy
} 
```

现在，不仅仅是向函数的消费者记录一个警告，如果我们重新调用函数，但是这次使用关键字`new`会怎么样？

```
function Animal (name, energy) {
  if (this instanceof Animal === false) {
    return new Animal(name, energy)
  }

  this.name = name
  this.energy = energy
} 
```

现在不管`Animal`是否被`new`关键字调用，它仍然会正常工作。

### 正在重新创建对象。创建

在这篇文章中，我们非常依赖`Object.create`来创建委托给构造函数原型的对象。在这一点上，你应该知道如何在你的代码中使用`Object.create`，但是有一件事你可能没有想到，那就是`Object.create`实际上是如何工作的。为了让你**真正**理解`Object.create`是如何工作的，我们将自己重新创建它。首先，我们对`Object.create`的运作方式了解多少？

1.  它接受一个作为对象的参数。
2.  它创建一个对象，该对象在查找失败时委托给 argument 对象。
3.  它返回新创建的对象。

让我们从第一点开始。

```
Object.create = function (objToDelegateTo) {

} 
```

很简单。

现在#2 -我们需要创建一个对象，它将在查找失败时委托给 argument 对象。这个有点复杂。为了做到这一点，我们将利用我们对关键字和原型在 JavaScript 中如何工作的了解。首先，在我们的`Object.create`实现体中，我们将创建一个空函数。然后，我们将这个空函数的原型设置为参数对象。然后，为了创建一个新对象，我们将使用`new`关键字调用我们的空函数。如果我们返回新创建的对象，这也将完成#3。

```
Object.create = function (objToDelegateTo) {
  function Fn(){}
  Fn.prototype = objToDelegateTo
  return new Fn()
} 
```

狂野。让我们走一遍。

当我们在上面的代码中创建新函数`Fn`时，它带有一个`prototype`属性。当我们用`new`关键字调用它时，我们知道我们将得到的是一个对象，它将在查找失败时委托给函数的原型。如果我们覆盖了函数的原型，那么我们就可以决定在查找失败时委托给哪个对象。所以在上面的例子中，我们用调用`Object.create`时传入的对象覆盖了`Fn`的原型，我们称之为`objToDelegateTo`。

请注意，我们只支持 Object.create 的单个参数。官方实现还支持第二个可选参数，该参数允许您向创建的对象添加更多属性。

### 箭头功能

箭头函数没有自己的`this`关键字。因此，箭头函数不能是构造函数，如果你试图用关键字`new`调用一个箭头函数，它会抛出一个错误。

```
const Animal = () => {}

const leo = new Animal() // Error: Animal is not a constructor 
```

此外，因为我们在上面演示了伪经典模式不能用于箭头函数，所以箭头函数也没有`prototype`属性。

```
const Animal = () => {}
console.log(Animal.prototype) // undefined 
```

* * *

## 这是我们 **[高级 JavaScript 课程](https://tylermcginnis.com/courses/advanced-javascript)** 的一部分。如果你喜欢这篇文章，看看吧。

* * *