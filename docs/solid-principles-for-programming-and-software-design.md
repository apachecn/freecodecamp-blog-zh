# 编程和软件设计的坚实基础

> 原文：<https://www.freecodecamp.org/news/solid-principles-for-programming-and-software-design/>

面向对象编程的坚实原则有助于使面向对象设计更容易理解、更灵活和更易维护。

它们还使创建可读和可测试的代码变得容易，许多开发人员可以随时随地协同工作。它们让你意识到编写代码的最佳方式💪。

SOLID 是一个助记首字母缩写词，代表面向对象类设计的五个设计原则。这些原则是:

*   单一责任原则
*   **O** -开闭原理
*   **L**——利斯科夫替代原理
*   **I** -界面分离原理
*   **D**——依存倒置原则

在本文中，您将通过 JavaScript 示例了解这些原则的含义以及它们是如何工作的。即使您不完全熟悉 JavaScript，这些例子也应该没问题，因为它们也适用于其他编程语言。

## 什么是单一责任原则？

单一责任原则，或 SRP，声明一个类应该只有一个改变的理由。这意味着一个类应该只有一个任务，做一件事。

让我们来看一个恰当的例子。你总是会被诱惑把相似的类放在一起——但是不幸的是，这违背了单一责任原则。为什么？

下面的`ValidatePerson`对象有三个方法:两个验证方法，(`ValidateName()`和`ValidateAge()`)和一个`Display()`方法。

```
class ValidatePerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    ValidateName(name) {
        if (name.length > 3) {
            return true;
        } else {
            return false;
        }
    }

    ValidateAge(age) {
        if (age > 18) {
            return true;
        } else {
            return false;
        }
    }

    Display() {
        if (this.ValidateName(this.name) && this.ValidateAge(this.age)) {
            console.log(`Name: ${this.name} and Age: ${this.age}`);
        } else {
            console.log('Invalid');
        }
    }
} 
```

`Display()`方法违背了 SRP，因为它的目标是一个类应该只有一个任务并做一件事。`ValidatePerson`类完成两项工作——它验证这个人的姓名和年龄，然后显示一些信息。

避免这个问题的方法是将支持不同动作和作业的代码分开，这样每个类只执行一项作业，并且有一个更改的理由。

这意味着`ValidatePerson`类将只负责验证用户，如下所示:

```
class ValidatePerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    ValidateName(name) {
        if (name.length > 3) {
            return true;
        } else {
            return false;
        }
    }

    ValidateAge(age) {
        if (age > 18) {
            return true;
        } else {
            return false;
        }
    }
} 
```

而新的类`DisplayPerson`现在将负责显示一个人，正如您在下面的代码块中看到的:

```
class DisplayPerson {
    constructor(name, age) {
        this.name = name;
        this.age = age;
        this.validate = new ValidatePerson(this.name, this.age);
    }

    Display() {
        if (
            this.validate.ValidateName(this.name) &&
            this.validate.ValidateAge(this.age)
        ) {
            console.log(`Name: ${this.name} and Age: ${this.age}`);
        } else {
            console.log('Invalid');
        }
    }
} 
```

这样，你就完成了单一责任原则，意味着我们的类现在只有一个改变的理由。如果要更改`DisplayPerson`类，不会影响`ValidatePerson`类。

## 开闭原理是什么？

开闭原则可能会令人困惑，因为它是一个双向原则。根据 [Bertrand Meyer 在](https://en.wikipedia.org/wiki/Bertrand_Meyer) [Wikipedia](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle) 上的定义，**开闭原则(OCP)** 陈述了软件实体(类、模块、函数等。)应该对扩展开放，但对修改关闭。

这个定义可能会令人困惑，但是一个例子和进一步的澄清会帮助你理解。

OCP 有两个主要属性:

*   对于扩展来说,**是开放的——这意味着你可以扩展模块所能做的。**
*   对于修改，它是**关闭的**——这意味着您不能更改源代码，即使您可以扩展模块或实体的行为。

OCP 意味着一个类、模块、函数和其他实体可以在不修改源代码的情况下扩展它们的行为。换句话说，实体应该是可扩展的，而不需要修改实体本身。怎么会？

例如，假设您有一个数组`iceCreamFlavours`，其中包含一个可能的口味列表。在`makeIceCream`类中，`make()`方法将检查特定的味道是否存在，并记录一条消息。

```
const iceCreamFlavors = ['chocolate', 'vanilla'];

class makeIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }

    make() {
        if (iceCreamFlavors.indexOf(this.flavor) > -1) {
            console.log('Great success. You now have ice cream.');
        } else {
            console.log('Epic fail. No ice cream for you.');
        }
    }
} 
```

上面的代码不符合 OCP 原理。为什么？因为上面的代码没有对扩展开放，这意味着你可以添加新的风格，你需要直接编辑`iceCreamFlavors`数组。这意味着代码不再关闭修改。哈哈(那就很多了)。

要解决这个问题，您需要一个额外的类或实体来处理加法，因此您不再需要直接修改代码来进行任何扩展。

```
const iceCreamFlavors = ['chocolate', 'vanilla'];

class makeIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }
    make() {
        if (iceCreamFlavors.indexOf(this.flavor) > -1) {
            console.log('Great success. You now have ice cream.');
        } else {
            console.log('Epic fail. No ice cream for you.');
        }
    }
}

class addIceCream {
    constructor(flavor) {
        this.flavor = flavor;
    }
    add() {
        iceCreamFlavors.push(this.flavor);
    }
} 
```

这里，我们添加了一个新的类——`addIceCream`——来使用`add()`方法处理`iceCreamFlavors`数组的加法。这意味着您的代码不允许修改，但允许扩展，因为您可以添加新的风格，而不会直接影响数组。

```
let addStrawberryFlavor = new addIceCream('strawberry');
addStrawberryFlavor.add();
makeStrawberryIceCream.make(); 
```

另外，请注意 SRP 已经就位，因为您创建了一个新的类。😊

## 什么是利斯科夫替代原理？

1987 年，Barbara Liskov 在她的会议主题“数据抽象”中介绍了 Liskov 替代原则(LSP)。几年后，她这样定义这个原则:

> “设φ(x)是关于 T 类型的对象 x 的一个可证明的性质。那么φ(y)对于 S 类型的对象 y 应该是真的，其中 S 是 T 的子类型”。

老实说，这个定义并不是许多软件开发人员想要看到的😂—所以让我把它分解成一个与 OOP 相关的定义。

该原则定义了在继承中，超类(或父类)的对象应该可以被其子类(或子类)的对象替换，而不会破坏应用程序或导致任何错误。

简单地说，您希望子类的对象与超类的对象行为相同。

一个非常常见的例子是矩形、正方形场景。很明显，所有的正方形都是长方形，因为它们都是四边形，四个角都是直角。但并不是每个矩形都是正方形。要成为正方形，它的边必须等长。

记住这一点，假设您有一个 rectangle 类来计算矩形的面积并执行其他操作，如设置颜色:

```
class Rectangle {
    setWidth(width) {
        this.width = width;
    }

    setHeight(height) {
        this.height = height;
    }

    setColor(color) {
        // ...
    }

    getArea() {
        return this.width * this.height;
    }
} 
```

充分了解所有正方形都是矩形，您可以继承矩形的属性。因为宽度和高度必须相同，所以您可以调整它:

```
class Square extends Rectangle {
    setWidth(width) {
        this.width = width;
        this.height = width;
    }
    setHeight(height) {
        this.width = height;
        this.height = height;
    }
} 
```

看一下这个例子，它应该可以正常工作:

```
let rectangle = new Rectangle();
rectangle.setWidth(10);
rectangle.setHeight(5);
console.log(rectangle.getArea()); // 50 
```

在上面的代码中，您会注意到创建了一个矩形，并设置了宽度和高度。然后就可以算出正确的面积了。

但是根据 LSP，您希望您的子类的对象以与您的超类的对象相同的方式运行。也就是说，如果你用`Square`代替`Rectangle`，一切都应该正常工作:

```
let square = new Square();
square.setWidth(10);
square.setHeight(5); 
```

您应该得到 100，因为`setWidth(10)`应该将宽度和高度都设置为 10。但是因为有了`setHeight(5)`，这个会返回 25。

```
let square = new Square();
square.setWidth(10);
square.setHeight(5);
console.log(square.getArea()); // 25 
```

这打破了 LSP。为了解决这个问题，应该有一个通用的类来保存所有你希望子类的对象可以访问的通用方法。然后对于单独的方法，您为 rectangle 和 square 创建单独的类。

```
class Shape {
    setColor(color) {
        this.color = color;
    }
    getColor() {
        return this.color;
    }
}

class Rectangle extends Shape {
    setWidth(width) {
        this.width = width;
    }
    setHeight(height) {
        this.height = height;
    }
    getArea() {
        return this.width * this.height;
    }
}

class Square extends Shape {
    setSide(side) {
        this.side = side;
    }
    getArea() {
        return this.side * this.side;
    }
} 
```

这样，您可以设置颜色并使用 super 或 subclasses 获得颜色:

```
// superclass
let shape = new Shape();
shape.setColor('red');
console.log(shape.getColor()); // red

// subclass
let rectangle = new Rectangle();
rectangle.setColor('red');
console.log(rectangle.getColor()); // red

// subclass
let square = new Square();
square.setColor('red');
console.log(square.getColor()); // red 
```

## 什么是界面分离原理？

接口隔离原则(ISP)声明“永远不应该强迫客户端实现它不使用的接口，或者不应该强迫客户端依赖它们不使用的方法”。这是什么意思？

正如术语“隔离”的意思——这就是保持事物分离，也就是分离接口。

**注意:**默认情况下，JavaScript 没有接口，但这个原则仍然适用。所以让我们假设这个接口存在来研究它，这样你就会知道它对于其他编程语言比如 Java 是如何工作的。

典型的接口将包含方法和属性。当您在任何类中实现这个接口时，该类需要定义它的所有方法。例如，假设您有一个定义绘制特定形状的方法的接口。

```
interface ShapeInterface {
    calculateArea();
    calculateVolume();
} 
```

当任何类实现这个接口时，所有的方法都必须被定义，即使你不使用它们或者它们不适用于那个类。

```
class Square implements ShapeInterface {
    calculateArea(){
        //...
    }
    calculateVolume(){
        //...
    }  
}

class Cuboid implements ShapeInterface {
    calculateArea(){
        //...
    }
    calculateVolume(){
        //...
    }    
}

class Rectangle implements ShapeInterface {
    calculateArea(){
        //...
    }
    calculateVolume(){
        //...
    }   
} 
```

看上面的例子，你会注意到你不能计算正方形或长方形的体积。因为该类实现了接口，所以您需要定义所有方法，甚至是您不会使用或不需要的方法。

要解决这个问题，您需要隔离接口。

```
interface ShapeInterface {
    calculateArea();
}

interface ThreeDimensionalShapeInterface {
    calculateArea();
    calculateVolume();
} 
```

现在，您可以实现用于每个类的特定接口。

```
class Square implements ShapeInterface {
    calculateArea(){
        //...
    } 
}

class Cuboid implements ThreeDimensionalShapeInterface {
    calculateArea(){
        //...
    }
    calculateVolume(){
        //...
    }    
}

class Rectangle implements ShapeInterface {
    calculateArea(){
        //...
    }  
} 
```

## 依赖倒置原理是什么？

这一原则的目标是松散耦合的软件模块，以便高级模块(提供复杂的逻辑)易于重用，不受低级模块(提供实用功能)变化的影响。

根据[维基百科](https://en.wikipedia.org/wiki/Dependency_inversion_principle)的说法，这条原则声明:

1.  高级模块不应该从低级模块导入任何东西。两者都应该依赖于抽象(例如，接口)。
2.  抽象应该独立于细节。细节(具体的实现)应该依赖于抽象。

简单地说，这个原则表明你的类应该依赖于接口或抽象类，而不是具体的类和函数。这使得你的类可以扩展，遵循开闭原则。

让我们看一个例子。建立商店时，您可能希望您的商店使用 stripe 等支付网关或任何其他首选支付方式。您可能会编写与该 API 紧密耦合的代码，而不考虑未来。

但是，如果你发现了另一个提供更好服务的支付网关，比如说贝宝，该怎么办呢？然后，从 Stripe 转换到 Paypal 就成了一场斗争，这在编程和软件设计中应该不是问题。

```
class Store {
    constructor(user) {
        this.stripe = new Stripe(user);
    }

    purchaseBook(quantity, price) {
        this.stripe.makePayment(price * quantity);
    }

    purchaseCourse(quantity, price) {
        this.stripe.makePayment(price * quantity);
    }
}

class Stripe {
    constructor(user) {
        this.user = user;
    }

    makePayment(amountInDollars) {
        console.log(`${this.user} made payment of ${amountInDollars}`);
    }
} 
```

考虑到上面的例子，你会注意到如果你改变支付网关，你不仅仅需要添加类——你还需要改变`Store`类。这不仅违背了依赖倒置原则，也违背了开闭原则。

要解决这个问题，您必须确保您的类依赖于接口或抽象类，而不是具体的类和函数。对于这个例子，这个接口将包含您希望您的 API 拥有的所有行为，并且不依赖于任何东西。它充当高级模块和低级模块之间的中介。

```
class Store {
    constructor(paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    purchaseBook(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }

    purchaseCourse(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }
}

class StripePaymentProcessor {
    constructor(user) {
        this.stripe = new Stripe(user);
    }

    pay(amountInDollars) {
        this.stripe.makePayment(amountInDollars);
    }
}

class Stripe {
    constructor(user) {
        this.user = user;
    }

    makePayment(amountInDollars) {
        console.log(`${this.user} made payment of ${amountInDollars}`);
    }
}

let store = new Store(new StripePaymentProcessor('John Doe'));
store.purchaseBook(2, 10);
store.purchaseCourse(1, 15); 
```

在上面的代码中，你会注意到`StripePaymentProcessor`类是`Store`类和`Stripe`类之间的接口。在你需要使用 PayPal 的情况下，你所要做的就是创建一个可以和`PayPal`类一起工作的`PayPalPaymentProcessor`，一切都不会影响到`Store`类。

```
class Store {
    constructor(paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    purchaseBook(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }

    purchaseCourse(quantity, price) {
        this.paymentProcessor.pay(quantity * price);
    }
}

class PayPalPaymentProcessor {
    constructor(user) {
        this.user = user;
        this.paypal = new PayPal();
    }

    pay(amountInDollars) {
        this.paypal.makePayment(this.user, amountInDollars);
    }
}

class PayPal {
    makePayment(user, amountInDollars) {
        console.log(`${user} made payment of ${amountInDollars}`);
    }
}

let store = new Store(new PayPalPaymentProcessor('John Doe'));
store.purchaseBook(2, 10);
store.purchaseCourse(1, 15); 
```

您还会注意到这遵循了 Liskov 替换原则，因为您可以用同一接口的其他实现来替换它，而不会破坏您的应用程序。

## 哒哒😇

这是一次冒险🙃。我希望你注意到这些原则中的每一条都以某种方式与其他原则相关联。

例如，为了纠正一个原则，比如说依赖倒置原则，你间接地确保你的类可以扩展，但是不能修改。

在编写代码时，您应该记住这些原则，因为它们使许多人更容易在您的项目中协作。它们简化了扩展、修改、测试和重构代码的过程。所以确保你理解它们的定义，它们的作用，以及为什么你需要它们超越 OOP。

想要了解更多，你可以在 [freeCodeCamp YouTube 频道](https://www.youtube.com/c/Freecodecamp)上观看 [Beau Carnes](https://www.freecodecamp.org/news/author/beau/) 的[这个视频](https://www.youtube.com/watch?v=XzdhzyAukMM)，或者阅读[yi kit Kemal Erin](https://www.freecodecamp.org/news/author/erinc/)的[这篇文章](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/)。

祝编码愉快！