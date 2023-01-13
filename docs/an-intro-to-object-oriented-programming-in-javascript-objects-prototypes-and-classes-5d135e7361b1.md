# JavaScript 中面向对象编程的介绍:对象、原型和类

> 原文：<https://www.freecodecamp.org/news/an-intro-to-object-oriented-programming-in-javascript-objects-prototypes-and-classes-5d135e7361b1/>

作者:安德里亚·库提法里斯

# JavaScript 中面向对象编程的介绍:对象、原型和类

![NQGYTcJBmsBc1qx3tm8Nf3iQ4FntP9dOWRZP](img/29c98eccbebf280062c776ff6b222504.png)

Objects in a class(room).

在许多编程语言中，类是一个定义明确的概念。在 JavaScript 中，情况并非如此。或者至少事实并非如此。如果你搜索 O.O.P .和 JavaScript，你会看到很多文章，里面有很多不同的方法，告诉你如何在 JavaScript 中模拟一个`class`。

有没有一种简单的，K.I.S.S .的方法来定义 JavaScript 中的类？如果是这样，为什么有这么多不同的配方来定义一个类？

在回答这些问题之前，让我们更好地理解什么是 JavaScript `Object`。

### **JavaScript 中的对象**

让我们从一个非常简单的例子开始:

```
const a = {};
a.foo = 'bar';
```

在上面的代码片段中，创建了一个对象，并用属性`foo`对其进行了增强。向现有对象添加内容的可能性是 JavaScript 不同于 Java 等经典语言的地方。

更详细地说，对象可以被增强的事实使得创建“隐式”类的实例成为可能，而不需要实际创建该类。让我们用一个例子来阐明这个概念:

```
function distance(p1, p2) {
  return Math.sqrt(
    (p1.x - p2.x) ** 2 + 
    (p1.y - p2.y) ** 2
  );
}

distance({x:1,y:1},{x:2,y:2});
```

在上面的例子中，我不需要一个点类来创建一个点，我只是扩展了一个添加了`x`和`y`属性的`Object`实例。函数距离不关心参数是否是类`Point`的实例。直到你用两个具有类型为`Number`的`x`和`y`属性的对象调用`distance`函数，它才会正常工作。这个概念有时被称为*鸭式打字*。

到目前为止，我只用过一个数据对象:只包含数据而不包含函数的对象。但是在 JavaScript 中，可以向对象添加函数:

```
const point1 = {
  x: 1,
  y: 1,
  toString() {
    return `(${this.x},${this.y})`;
  }
};

const point2 = {
  x: 2,
  y: 2,
  toString() {
    return `(${this.x},${this.y})`;
  }
};
```

这一次，代表 2D 点的对象有了一个`toString()`方法。在上面的例子中，`toString`代码被复制了，这并不好。

有许多方法可以避免这种重复，事实上，在关于 JS 中对象和类的不同文章中，您会找到不同的解决方案。你听说过“揭示模块模式”吗？包含“格局”“揭示”两个字，听起来很酷，“模块”是必须的。所以这一定是创建对象的正确方法…除了它不是。在某些情况下，揭示模块模式可能是正确的选择，但它绝对不是创建具有行为的对象的默认方式。

我们现在准备引入类。

### **JavaScript 中的类**

什么是课？从字典来看:类是“一组或一类事物，它们具有一些共同的性质或属性，并通过种类、类型或质量与其他事物相区别。”

在编程语言中，我们经常说“一个对象是一个类的实例”。这意味着，使用一个类，我可以创建许多对象，它们都共享方法和属性。

因为对象可以被增强，正如我们前面看到的，有许多方法可以创建共享方法和属性的对象。但是我们想要最简单的。

幸运的是，ECMAScript 6 提供了关键字`class`，使得创建一个类变得非常容易:

```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return `(${this.x},${this.y})`;
  }
}
```

因此，在我看来，这是用 JavaScript 声明类的最好方式。类通常与继承有关:

```
class Point extends HasXY {
  constructor(x, y) {
    super(x, y);
  }

  toString() {
    return `(${this.x},${this.y})`;
  }
}
```

正如您在上面的例子中看到的，要扩展另一个类，使用关键字`extends`就足够了。

您可以使用`new`操作符从一个类中创建一个对象:

```
const p = new Point(1,1);
console.log(p instanceof Point); // prints true
```

一个好的面向对象的定义类的方法应该提供:

*   声明类的简单语法
*   一种访问当前实例的简单方法，又名`this`
*   扩展类的简单语法
*   一种访问超类实例的简单方法，又名`super`
*   这可能是判断一个对象是否是一个特定类的实例的简单方法。如果该对象是该类的实例，`obj instanceof AClass`应该返回`true`。

新的`class`语法提供了上述所有要点。

在`class`关键字引入之前，JavaScript 中定义类的方式是什么？

另外，JavaScript 中的类到底是什么？为什么我们经常说*原型*？

### **JavaScript 5 中的类**

来自 [Mozilla MDN 关于类的页面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes):

> ECMAScript 2015 中引入的 JavaScript 类主要是 JavaScript 现有的**基于原型的继承**的语法糖。类语法没有给 JavaScript 引入新的面向对象的继承模型。

这里的关键概念是**基于原型的继承**。由于对那种继承是什么有很多误解，我将一步一步地进行，从`class`关键字移动到`function`关键字。

```
class Shape {}
console.log(typeof Shape);
// prints function
```

看来`class`和`function`是有关系的。`class`只是`function`的别名吗？不，不是的。

```
Shape(2);
// Uncaught TypeError: Class constructor Shape cannot be invoked without 'new'
```

所以，似乎引入`class`关键字的人想告诉我们，类是一个必须使用`new`操作符调用的函数。

```
var Shape = function Shape() {} // Or just function Shape(){}
var aShape = new Shape();
console.log(aShape instanceof Shape);
// prints true
```

上面的例子表明我们可以使用`function`来声明一个类。然而，我们不能强迫用户使用`new`操作符调用函数。如果没有使用`new`操作符来调用函数，就有可能抛出异常。

无论如何，我建议你不要在每个作为类的函数中做检查。取而代之的是使用这个约定:任何名字以大写字母开头的函数都是一个类，必须使用`new`操作符来调用。

让我们继续，看看什么是*原型*:

```
class Shape {
  getName() {
    return 'Shape';
  }
}
console.log(Shape.prototype.getName);
// prints function getName() ...
```

每次在类中声明一个方法时，实际上是将该方法添加到相应函数的原型中。JS 5 中的对应项是:

```
function Shape() {}
Shape.prototype.getName = function getName() {
  return 'Shape';
};
console.log(new Shape().getName()); // prints Shape
```

有时类函数被称为*构造函数*，因为它们的行为就像普通类中的构造函数。

您可能想知道如果声明一个静态方法会发生什么:

```
class Point {
  static distance(p1, p2) {
    // ...
  }
}

console.log(Point.distance); // prints function distance
console.log(Point.prototype.distance); // prints undefined
```

因为静态方法与类是一对一的关系，所以静态函数被添加到构造函数中，而不是原型中。

让我们用一个简单的例子来概括所有这些概念:

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function toString() {
  return '(' + this.x + ',' + this.y + ')';
};
Point.distance = function distance() {
  // ...
}

console.log(new Point(1,2).toString()); // prints (1,2)
console.log(new Point(1,2) instanceof Point); // prints true
```

到目前为止，我们已经找到了一种简单的方法:

*   声明一个作为类的函数
*   使用`this`关键字访问类实例
*   创建实际上是该类实例的对象(`new Point(1,2) instanceof Point`返回`true`)

但是遗传呢？访问超级类呢？

```
class Hello {
  constructor(greeting) {
    this._greeting = greeting;
  }

  greeting() {
    return this._greeting;
  }
}

class World extends Hello {
  constructor() {
    super('hello');
  }

  worldGreeting() {
    return super.greeting() + ' world';
  }
}

console.log(new World().greeting()); // Prints hello
console.log(new World().worldGreeting()); // Prints hello world
```

上面是使用 ECMAScript 6 的简单继承示例，下面是使用所谓的**原型继承**的相同示例:

```
function Hello(greeting) {
  this._greeting = greeting;
}

Hello.prototype.greeting = function () {
  return this._greeting;
};

function World() {
  Hello.call(this, 'hello');
}

// Copies the super prototype
World.prototype = Object.create(Hello.prototype);
// Makes constructor property reference the sub class
World.prototype.constructor = World;

World.prototype.worldGreeting = function () {
  const hello = Hello.prototype.greeting.call(this);
  return hello + ' world';
};

console.log(new World().greeting()); // Prints hello
console.log(new World().worldGreeting()); // Prints hello world
```

这种声明类的方式在 Mozilla MDN 示例[这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create#Examples)中也有建议。

使用`class`语法，我们推断出创建类涉及到改变函数的原型。但是为什么会这样呢？要回答这个问题，我们必须理解`new`操作符实际上是做什么的。

### JavaScript 中的新运算符

在 Mozilla MDN 页面[这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)对`new`操作符做了很好的解释。但是我可以给你提供一个相对简单的例子，它模拟了`new`操作符的作用:

```
function customNew(constructor, ...args) {
  const obj = Object.create(constructor.prototype);
  const result = constructor.call(obj, ...args);

  return result instanceof Object ? result : obj;
}

function Point() {}
console.log(customNew(Point) instanceof Point); // prints true
```

注意真正的`new`算法更复杂。上面例子的目的只是为了解释当你使用`new`操作符时会发生什么。

当你写`new Point(1,2)`时，发生的是:

*   `Point`原型用于创建一个对象。
*   调用函数构造函数，并将刚刚创建的对象作为上下文(也称为`this`)与其他参数一起传递。
*   如果构造函数返回一个对象，那么这个对象就是新的结果，否则从原型创建的对象就是结果。

那么，**原型继承**是什么意思呢？这意味着您可以创建继承在用`new`操作符调用的函数原型中定义的所有属性的对象。

如果你想一想，在传统语言中会发生同样的过程:当你创建一个类的实例时，该实例可以使用`this`关键字来访问该类(和祖先)中定义的所有函数和属性(public)。与属性相反，一个类的所有实例可能共享对类方法的相同引用，因为不需要复制方法的二进制代码。

### 函数式编程

有时人们会说 JavaScript 不太适合面向对象编程，你应该使用函数式编程。

虽然我不同意 JS 不适合 O.O.P，但我确实认为函数式编程是一种非常好的编程方式。在 JavaScript 中，函数是[第一类公民](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)(例如，你可以将一个函数传递给另一个函数)，它提供了像`bind`、`call`或`apply`这样的特性，这些特性是函数式编程中使用的基本构造。

此外，RX 编程可以被视为函数式编程的一种发展(或专门化)。看一看 [RxJs 这里](https://rxjs-dev.firebaseapp.com/)。

### 结论

尽可能使用 ECMAScript 6 `class`语法:

```
class Point {
  toString() {
    //...
  }
}
```

或者使用函数原型来定义 ECMAScript 5:

```
function Point() {}
Point.prototype.toString = function toString() {
  // ...
}
```

希望你喜欢阅读！