# 面向对象的 JavaScript 编程初学者

> 原文：<https://www.freecodecamp.org/news/object-oriented-javascript-for-beginners/>

大家好！在本文中，我们将通过实际的 JavaScript 例子来回顾面向对象编程(OOP)的主要特征。

我们将讨论 OOP 的主要概念，为什么以及什么时候它会有用，并且我将给出大量使用 JS 代码的例子。

如果你不熟悉编程范例，我建议你在深入本文之前先看看我最近写的简介。

放马过来。

![160cf1a4201c53b015bfcccb9398e9ab](img/f315d18fbe47d4600a303c33d0837722.png)

## 目录

*   [面向对象编程简介](#intro-to-object-oriented-programming)
*   [如何创建对象-类](#how-to-create-objects-classes)
    *   [关于课程需要记住的一些事情](#some-things-to-keep-in-mind-about-classes-)
*   [面向对象的四个原则](#the-four-principles-of-oop)
    *   [继承](#inheritance)
        *   关于继承需要记住的一些事情
    *   [封装](#encapsulation)
    *   [抽象](#abstraction)
    *   [多态性](#polymorphism)
*   [物体构成](#object-composition)
*   [综述](#roundup)

# 面向对象编程简介

正如我在关于编程范例的前一篇文章中提到的，OOP 的核心概念是将关注点和职责分离成 T4 实体。

实体被编码为**对象**、**和**，并且每个实体将分组一组给定的信息(**属性**)和实体可以执行的动作(**方法**)。

OOP 在大规模项目中非常有用，因为它促进了代码的模块化和组织。

通过实现实体的抽象，我们能够以类似于我们的世界工作的方式来思考程序，不同的参与者执行特定的动作并相互交互。

为了更好地理解我们如何实现 OOP，我们将使用一个实际的例子，在这个例子中我们将编写一个小的视频游戏。我们将把重点放在角色的创造上，看看 OOP 如何帮助我们。👽 👾 🤖

# 如何创建对象-类

所以任何电子游戏都需要角色，对吧？而且所有角色都有某些**特征**(属性)如肤色、身高、名字等等和**能力**(方法)如跳跃、奔跑、出拳等等。对象是用来存储这种信息的完美数据结构。👌

假设我们有 3 个不同的角色“物种”,我们想要创建 6 个不同的角色，每个物种 2 个。

创建我们的角色的一种方法是使用[对象文字和](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)手工创建对象，如下所示:

```
const alien1 = {
    name: "Ali",
    species: "alien",
    phrase: () => console.log("I'm Ali the alien!"),
    fly: () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}
const alien2 = {
    name: "Lien",
    species: "alien",
    sayPhrase: () => console.log("Run for your lives!"),
    fly: () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}
const bug1 = {
    name: "Buggy",
    species: "bug",
    sayPhrase: () => console.log("Your debugger doesn't work with me!"),
    hide: () => console.log("You can't catch me now!")
}
const bug2 = {
    name: "Erik",
    species: "bug",
    sayPhrase: () => console.log("I drink decaf!"),
    hide: () => console.log("You can't catch me now!")
}
const Robot1 = {
    name: "Tito",
    species: "robot",
    sayPhrase: () => console.log("I can cook, swim and dance!"),
    transform: () => console.log("Optimus prime!")
}
const Robot2 = {
    name: "Terminator",
    species: "robot",
    sayPhrase: () => console.log("Hasta la vista, baby!"),
    transform: () => console.log("Optimus prime!")
}
```

看到所有的角色都有`name`和`species`属性以及`sayPhrase`方法。而且每个物种都有只属于那个物种的方法(比如外星人有`fly`方法)。

如你所见，有些数据是所有角色共享的，有些数据是每个物种共享的，有些数据是每个个体角色独有的。

这种方法行得通。请注意，我们可以像这样完美地访问属性和方法:

```
console.log(alien1.name) // output: "Ali"
console.log(bug2.species) // output: "bug"
Robot1.sayPhrase() // output: "I can cook, swim and dance!"
Robot2.transform() // output: "Optimus prime!"
```

这样做的问题是，它根本不能很好地扩展，而且容易出错。想象一下，我们的游戏可能有数百个角色。我们需要为它们中的每一个手动设置属性和方法！

为了解决这个问题，我们需要一种编程方式来创建对象，并在给定的条件下设置不同的属性和方法。这就是**类**的好处。😉

类设置了一个蓝图，用预定义的属性和方法创建对象。通过创建一个类，您可以稍后从该类实例化(创建)对象，这将继承该类的所有属性和方法。

重构我们之前的代码，我们可以为每个角色种类创建一个类，如下所示:

```
class Alien { // Name of the class
    // The constructor method will take a number of parameters and assign those parameters as properties to the created object.
    constructor (name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    // These will be the object's methods.
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}

class Bug {
    constructor (name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "bug"
    }
    hide = () => console.log("You can't catch me now!")
    sayPhrase = () => console.log(this.phrase)
}

class Robot {
    constructor (name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "robot"
    }
    transform = () => console.log("Optimus prime!")
    sayPhrase = () => console.log(this.phrase)
}
```

然后我们可以从这些类中实例化我们的角色，就像这样:

```
const alien1 = new Alien("Ali", "I'm Ali the alien!")
// We use the "new" keyword followed by the corresponding class name
// and pass it the corresponding parameters according to what was declared in the class constructor function

const alien2 = new Alien("Lien", "Run for your lives!")
const bug1 = new Bug("Buggy", "Your debugger doesn't work with me!")
const bug2 = new Bug("Erik", "I drink decaf!")
const Robot1 = new Robot("Tito", "I can cook, swim and dance!")
const Robot2 = new Robot("Terminator", "Hasta la vista, baby!")
```

话说回来，我们可以这样访问每个对象的属性和方法:

```
console.log(alien1.name) // output: "Ali"
console.log(bug2.species) // output: "bug"
Robot1.sayPhrase() // output: "I can cook, swim and dance!"
Robot2.transform() // output: "Optimus prime!"
```

这种方法和使用类的好处在于，我们可以使用这些“蓝图”来创建新的对象，比我们“手动”创建更快、更安全。

此外，我们的代码组织得更好，因为我们可以清楚地识别每个对象属性和方法的定义位置(在类中)。这使得未来的改变或适应更容易实现。

### 关于类需要记住的一些事情:

根据这个定义，用更正式的术语来说，

> 程序中的类是自定义数据结构“类型”的定义，包括数据和对数据进行操作的行为。类定义了这种数据结构是如何工作的，但是类本身并不是具体的值。要获得可以在程序中使用的具体值，必须实例化一个类(用“new”关键字)一次或多次。

*   记住，类不是实际的实体或对象。类是我们用来创建实际对象的蓝图或模型。
*   按照惯例，类名用大写首字母和大小写来声明。class 关键字创建了一个常量，因此以后不能重新定义它。
*   类必须总是有一个构造函数方法，该方法将在以后用于实例化该类。JavaScript 中的构造函数只是一个返回对象的普通函数。唯一特别的是，当用“new”关键字调用时，它将其原型指定为返回对象的原型。
*   “this”关键字指向类本身，用于在构造函数方法中定义类属性。
*   只需定义函数名及其执行代码，就可以添加方法。
*   JavaScript 是一种基于原型的语言，在 JavaScript 中，类仅用作语法糖。这在这里并没有很大的区别，但是知道并记住这一点是有好处的。如果你想了解更多关于这个话题的信息，你可以阅读这篇文章。

# 面向对象的四个原则

OOP 通常用 4 个关键原则来解释，这些原则决定了 OOP 程序如何工作。这些是**继承、封装、抽象和多态**。让我们逐一回顾一下。

## 遗产

继承是基于其他类创建类的能力。通过继承，我们可以定义一个**父类**(具有某些属性和方法)，然后定义**子类**，它将继承父类的所有属性和方法。

让我们看一个例子。想象一下我们之前定义的所有角色都将是我们主角的敌人。而作为敌人，他们都将拥有“力量”属性和“攻击”方法。

实现这一点的一种方法是给我们所有的类添加相同的属性和方法，就像这样:

```
...

class Bug {
    constructor (name, phrase, power) {
        this.name = name
        this.phrase = phrase
        this.power = power
        this.species = "bug"
    }
    hide = () => console.log("You can't catch me now!")
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Robot {
    constructor (name, phrase, power) {
        this.name = name
        this.phrase = phrase
        this.power = power
        this.species = "robot"
    }
    transform = () => console.log("Optimus prime!")
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

const bug1 = new Bug("Buggy", "Your debugger doesn't work with me!", 10)
const Robot1 = new Robot("Tito", "I can cook, swim and dance!", 15)

console.log(bug1.power) //output: 10
Robot1.attack() // output: "I'm attacking with a power of 15!"
```

但是你可以看到我们在重复代码，这不是最佳的。更好的方法是声明一个父“敌人”类，然后由所有敌人物种扩展，就像这样:

```
class Enemy {
    constructor(power) {
        this.power = power
    }

    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power) {
        super(power)
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}

...
```

注意敌人的职业看起来和其他职业一样。我们使用构造函数方法接收参数并将它们作为属性分配，方法像简单函数一样声明。

在子类中，我们使用`extends`关键字来声明我们想要继承的父类。然后在构造函数方法中，我们必须声明“power”参数，并使用`super`函数来表明 property 是在父类中声明的。

当我们实例化新对象时，我们只需传递在相应的构造函数中声明的参数，然后*瞧！*我们现在可以访问父类中声明的属性和方法。😎

```
const alien1 = new Alien("Ali", "I'm Ali the alien!", 10)
const alien2 = new Alien("Lien", "Run for your lives!", 15)

alien1.attack() // output: I'm attacking with a power of 10!
console.log(alien2.power) // output: 15
```

现在，假设我们想要添加一个新的父类，它将我们所有的角色分组(不管他们是不是敌人)，并且我们想要设置一个属性“speed”和一个“move”方法。我们可以这样做:

```
class Character {
    constructor (speed) {
        this.speed = speed
    }

    move = () => console.log(`I'm moving at the speed of ${this.speed}!`)
}

class Enemy extends Character {
    constructor(power, speed) {
        super(speed)
        this.power = power
    }

    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(power, speed)
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}
```

首先，我们声明新的“Character”父类。然后我们把它扩展到敌人的职业上。最后，我们将新的“速度”参数添加到 Alien 类中的`constructor`和`super`函数中。

我们像往常一样实例化传递参数，并且再次 *voilà* ，我们可以从“祖父”类访问属性和方法。👴

```
const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)
const alien2 = new Alien("Lien", "Run for your lives!", 15, 60)

alien1.move() // output: "I'm moving at the speed of 50!"
console.log(alien2.speed) // output: 60
```

现在我们对继承有了更多的了解，让我们重构代码，尽可能避免代码重复:

```
class Character {
    constructor (speed) {
        this.speed = speed
    }
    move = () => console.log(`I'm moving at the speed of ${this.speed}!`)
}

class Enemy extends Character {
    constructor(name, phrase, power, speed) {
        super(speed)
        this.name = name
        this.phrase = phrase
        this.power = power
    }
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

class Bug extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "bug"
    }
    hide = () => console.log("You can't catch me now!")
}

class Robot extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "robot"
    }
    transform = () => console.log("Optimus prime!")
}

const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)
const alien2 = new Alien("Lien", "Run for your lives!", 15, 60)
const bug1 = new Bug("Buggy", "Your debugger doesn't work with me!", 25, 100)
const bug2 = new Bug("Erik", "I drink decaf!", 5, 120)
const Robot1 = new Robot("Tito", "I can cook, swim and dance!", 125, 30)
const Robot2 = new Robot("Terminator", "Hasta la vista, baby!", 155, 40)
```

请注意，我们的物种类现在看起来小多了，这要归功于我们将所有共享的属性和方法移到了一个公共的父类中。这就是继承可以帮助我们实现的效率。😉

### 关于继承要记住的一些事情:

*   一个类只能继承一个父类。你不能扩展多个类，尽管有一些方法可以解决这个问题。
*   您可以任意扩展继承链，设置父类、祖父类、曾祖父类等等。
*   如果一个子类从父类继承了任何属性，那么在分配自己的属性之前，它必须首先分配调用`super()`函数的父属性。

一个例子:

```
// This works:
class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

// This throws an error:
class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        this.species = "alien" // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        super(name, phrase, power, speed)
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}
```

*   继承时，所有父方法和属性都将被子方法和属性继承。我们无法决定从父类继承什么(就像我们无法选择从父母那里继承什么美德和缺陷一样。😅我们在讲构图的时候再回到这个)。
*   子类可以重写父类的属性和方法。

举个例子，在我们之前的代码中，Alien 类扩展了敌人类，它继承了记录`I'm attacking with a power of ${this.power}!`的`attack`方法:

```
class Enemy extends Character {
    constructor(name, phrase, power, speed) {
        super(speed)
        this.name = name
        this.phrase = phrase
        this.power = power
    }
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)
alien1.attack() // output: I'm attacking with a power of 10!
```

假设我们希望`attack`方法在我们的 Alien 类中做一件不同的事情。我们可以通过再次声明来覆盖它，就像这样:

```
class Enemy extends Character {
    constructor(name, phrase, power, speed) {
        super(speed)
        this.name = name
        this.phrase = phrase
        this.power = power
    }
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    attack = () => console.log("Now I'm doing a different thing, HA!") // Override the parent method.
}

const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)
alien1.attack() // output: "Now I'm doing a different thing, HA!"
```

## 包装

封装是 OOP 中的另一个关键概念，它代表了一个对象“决定”哪些信息向“外部”公开，哪些不公开的能力。封装是通过**公共和私有属性和方法**实现的。

在 JavaScript 中，默认情况下，所有对象的属性和方法都是公共的。“Public”仅仅意味着我们可以从对象本身的外部访问它的属性/方法:

```
// Here's our class
class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

// Here's our object
const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)

// Here we're accessing our public properties and methods
console.log(alien1.name) // output: Ali
alien1.sayPhrase() // output: "I'm Ali the alien!"
```

为了使这一点更清楚，让我们看看私有属性和方法是什么样子的。

假设我们希望我们的 Alien 类有一个`birthYear`属性，并使用该属性执行一个`howOld`方法，但是我们不希望除了对象本身之外的任何地方都可以访问该属性。我们可以这样实现:

```
class Alien extends Enemy {
    #birthYear // We first need to declare the private property, always using the '#' symbol as the start of its name.

    constructor (name, phrase, power, speed, birthYear) {
        super(name, phrase, power, speed)
        this.species = "alien"
        this.#birthYear = birthYear // Then we assign its value within the constructor function
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    howOld = () => console.log(`I was born in ${this.#birthYear}`) // and use it in the corresponding method.
}

// We instantiate the same way we always do
const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50, 10000)
```

然后我们可以访问`howOld`方法，就像这样:

```
alien1.howOld() // output: "I was born in 10000"
```

但是如果我们试图直接访问这个属性，我们会得到一个错误。如果我们记录对象，私有属性就不会显示出来。

```
console.log(alien1.#birthYear) // This throws an error
console.log(alien1) 
// output:
// Alien {
//     move: [Function: move],
//     speed: 50,
//     sayPhrase: [Function: sayPhrase],
//     attack: [Function: attack],
//     name: 'Ali',
//     phrase: "I'm Ali the alien!",
//     power: 10,
//     fly: [Function: fly],
//     howOld: [Function: howOld],
//     species: 'alien'
//   }
```

当我们需要某些属性或方法来实现对象的内部工作，但又不想对外公开时，封装是很有用的。拥有私有属性/方法可以确保我们不会“意外”暴露我们不想要的信息。

## 抽象

抽象是这样一个原则，即一个类应该只表示与问题的上下文相关的信息。简单地说，只向外部公开您将要使用的属性和方法。如果不需要，就不要曝光。

这个原则与封装密切相关，因为我们可以使用公共和私有的属性/方法来决定什么公开，什么不公开。

## 多态性

然后就是多态性(听起来真的很复杂，不是吗？OOP 名字是最酷的...🙃).多态性意味着“多种形式”，实际上是一个简单的概念。它是一个方法根据特定条件返回不同值的能力。

比如我们看到敌方类有`sayPhrase`法。我们所有的物种类都继承了敌人类，这意味着它们都有`sayPhrase`方法。

但是我们可以看到，当我们对不同的物种调用这个方法时，我们得到不同的结果:

```
const alien2 = new Alien("Lien", "Run for your lives!", 15, 60)
const bug1 = new Bug("Buggy", "Your debugger doesn't work with me!", 25, 100)

alien2.sayPhrase() // output: "Run for your lives!"
bug1.sayPhrase() // output: "Your debugger doesn't work with me!"
```

这是因为我们在实例化时给每个类传递了不同的参数。那是一种多态性，**基于参数的**。👌

另一种多态性是**基于继承的**，这指的是当我们有一个父类设置了一个方法，而子类覆盖了那个方法以某种方式修改它。我们之前看到的例子在这里也完全适用:

```
class Enemy extends Character {
    constructor(name, phrase, power, speed) {
        super(speed)
        this.name = name
        this.phrase = phrase
        this.power = power
    }
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    attack = () => console.log("Now I'm doing a different thing, HA!") // Override the parent method.
}

const alien1 = new Alien("Ali", "I'm Ali the alien!", 10, 50)
alien1.attack() // output: "Now I'm doing a different thing, HA!"
```

这个实现是多态的，因为如果我们注释掉 Alien 类中的`attack`方法，我们仍然能够在对象上调用它:

```
alien1.attack() // output: "I'm attacking with a power of 10!"
```

我们得到了同样的方法，它可以做这样或那样的事情，取决于它是否被覆盖。多态。👌👌

# 对象组成

对象组合是一种替代继承的技术。

当我们谈到继承时，我们提到子类总是继承所有的父方法和属性。嗯，通过使用组合，我们可以以一种比继承更灵活的方式给对象分配属性和方法，所以对象只能得到它们需要的，而不能得到其他的。

我们可以非常简单地实现这一点，通过使用接收对象作为参数的函数，并为它分配所需的属性/方法。让我们看一个例子。

假设现在我们想给我们的虫子角色增加飞行能力。正如我们在代码中看到的，只有外星人才有`fly`方法。因此，一种选择是在`Bug`类中复制完全相同的方法:

```
class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

class Bug extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "bug"
    }
    hide = () => console.log("You can't catch me now!")
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!") // We're duplicating code =(
}
```

另一个选择是将`fly`方法移到`Enemy`类，这样它就可以被`Alien`和`Bug`类继承。但是这也使得不需要这个方法的类可以使用这个方法，比如`Robot`。

```
class Enemy extends Character {
    constructor(name, phrase, power, speed) {
        super(speed)
        this.name = name
        this.phrase = phrase
        this.power = power
    }
    sayPhrase = () => console.log(this.phrase)
    attack = () => console.log(`I'm attacking with a power of ${this.power}!`)
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
}

class Alien extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "alien"
    }
}

class Bug extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "bug"
    }
    hide = () => console.log("You can't catch me now!")
}

class Robot extends Enemy {
    constructor (name, phrase, power, speed) {
        super(name, phrase, power, speed)
        this.species = "robot"
    }
    transform = () => console.log("Optimus prime!")
	// I don't need the fly method =(
}
```

如您所见，当我们对类的开始计划改变时，继承会引起问题(这在现实世界中是经常发生的)。对象组合提出了一种方法，在这种方法中，对象只在需要时才分配属性和方法。

在我们的示例中，我们可以创建一个函数，它唯一的职责是将 flying 方法添加到任何作为参数接收的对象:

```
const bug1 = new Bug("Buggy", "Your debugger doesn't work with me!", 25, 100)

const addFlyingAbility = obj => {
    obj.fly = () => console.log(`Now ${obj.name} can fly!`)
}

addFlyingAbility(bug1)
bug1.fly() // output: "Now Buggy can fly!"
```

对于我们希望怪物拥有的每种能力，我们可以有非常相似的功能。

正如您肯定能看到的，这种方法比让父类继承固定的属性和方法要灵活得多。每当一个对象需要一个方法的时候，我们只需要调用相应的函数就可以了。👌

这里有一个很好的视频，比较了继承和合成。

# 综述

OOP 是一个非常强大的编程范例，它可以通过创建实体的抽象来帮助我们处理大型项目。每个实体将负责某些信息和动作，并且实体也将能够彼此交互，就像真实世界是如何工作的一样。

在这篇文章中，我们学习了类、继承、封装、抽象、多态和组合。这些都是 OOP 世界中的关键概念。我们也看到了各种各样的例子，说明如何在 JavaScript 中实现 OOP。

一如既往，我希望你喜欢这篇文章，并学到一些新东西。如果你愿意，你也可以在 [LinkedIn](https://www.linkedin.com/in/germancocca/) 或 [Twitter](https://twitter.com/CoccaGerman) 上关注我。

干杯，下期再见！✌️

![98OvjJ](img/96d59756ecb122a38f0ac4a781f973fb.png)