# JavaScript 中的完整指南

> 原文：<https://www.freecodecamp.org/news/the-complete-guide-to-this-in-javascript/>

在 JavaScript 中，每个函数都有一个在声明时自动创建的`this`引用。

JavaScript 的`this`非常类似于 Java 或 C#等其他基于类的语言中的`this`引用(JavaScript 是一种基于原型的语言，没有“类”的概念):*它指向哪个对象正在调用函数*(这个对象有时称为*上下文*)。然而，在 JavaScript 中，*函数内部的`this`引用可以绑定到不同的对象，这取决于函数在哪里被调用*。

以下是 JavaScript 中关于`this`绑定的 5 条基本规则:

### **规则 1**

在全局范围内调用函数时，`this`引用默认绑定到 ****全局对象**** (浏览器中的`window`，或者 Node.js 中的`global`)。例如:

```
function foo() {
  this.a = 2;
}

foo();
console.log(a); // 2
```

注意:如果你在严格模式下声明上面的`foo()`函数，那么你在全局范围内调用这个函数，`this`将是`undefined`，赋值`this.a = 2`将抛出`Uncaught TypeError`异常。

### **规则 2**

让我们看看下面的例子:

```
function foo() {
  this.a = 2;
}

const obj = {
  foo: foo
};

obj.foo();
console.log(obj.a); // 2
```

显然，在上面的代码片段中，`foo()`函数被调用，而*上下文*是`obj`对象，`this`引用现在被绑定到`obj`。所以当一个函数被一个上下文对象调用时，`this`引用将被绑定到这个对象。

### **规则 3**

`.call`、`.apply`、`.bind`都可以在调用点显式绑定`this`。在很多 React 组件中都可以看到使用`.bind(this)`。

```
const foo = function() {
  console.log(this.bar)
}

foo.call({ bar: 1 }) // 1
```

这里有一个简单的例子来说明如何使用它们来绑定`this`:

*   `.call()` : `fn.call(thisObj, fnParam1, fnParam2)`
*   `.apply()` : `fn.apply(thisObj, [fnParam1, fnParam2])`
*   `.bind()` : `const newFn = fn.bind(thisObj, fnParam1, fnParam2)`

### **规则 4**

```
function Point2D(x, y) {
  this.x = x;
  this.y = y;
}

const p1 = new Point2D(1, 2);
console.log(p1.x); // 1
console.log(p1.y); // 2
```

你必须注意的是用`new`关键字调用的`Point2D`函数，`this`引用被绑定到`p1`对象。所以当用`new`关键字调用一个函数时，它将创建一个新的对象，而`this`引用将被绑定到这个对象。

注意:当你用`new`关键字调用一个函数时，我们也称它为*构造函数*。

### **规则 5**

JavaScript 在运行时根据当前上下文确定`this`的值。所以`this`有时候可以指向你期待之外的东西。

考虑这个带有名为`makeSound()`的方法的 Cat 类的例子，遵循规则 4(上面)中的模式，带有一个构造函数和`new`关键字。

```
const Cat = function(name, sound) {
  this.name = name;
  this.sound = sound;
  this.makeSound = function() {
    console.log( this.name + ' says: ' + this.sound );
  };
}

const kitty = new Cat('Fat Daddy', 'Mrrooowww');
kitty.makeSound(); // Fat Daddy says: Mrrooowww
```

现在让我们试着通过重复他的声音 100 次，每半秒钟一次，给猫一种方法。

```
const Cat = function(name, sound) {
  this.name = name;
  this.sound = sound;
  this.makeSound = function() {
    console.log( this.name + ' says: ' + this.sound );
  };
  this.annoy = function() {
    let count = 0, max = 100;
    const t = setInterval(function() {
      this.makeSound(); // <-- this line fails with `this.makeSound is not a function` 
      count++;
      if (count === max) {
        clearTimeout(t);
      }
    }, 500);
  };
}

const kitty = new Cat('Fat Daddy', 'Mrrooowww');
kitty.annoy();
```

这不起作用，因为在`setInterval`回调中，我们已经创建了一个全局范围的新上下文，所以`this`不再指向我们的 kitty 实例。在 web 浏览器中，`this`将会指向窗口对象，它没有`makeSound()`方法。

有几种方法可以让它工作:

1.  在创建新的上下文之前，将`this`赋给一个名为`me`或`self`的局部变量，或者任何你想叫它的名字，并在回调中使用这个变量。

```
const Cat = function(name, sound) {
  this.name = name;
  this.sound = sound;
  this.makeSound = function() {
    console.log( this.name + ' says: ' + this.sound );
  };
  this.annoy = function() {
    let count = 0, max = 100;
    const self = this;
    const t = setInterval(function() {
      self.makeSound();
      count++;
      if (count === max) {
        clearTimeout(t);
      }
    }, 500);
  };
}

const kitty = new Cat('Fat Daddy', 'Mrrooowww');
kitty.annoy();
```

1.  在 ES6 中，你可以通过使用箭头函数来避免将`this`赋给局部变量，它将`this`绑定到定义它的周围代码的上下文中。

```
const Cat = function(name, sound) {
  this.name = name;
  this.sound = sound;
  this.makeSound = function() {
    console.log( this.name + ' says: ' + this.sound );
  };
  this.annoy = function() {
    let count = 0, max = 100;
    const t = setInterval(() => {
      this.makeSound();
      count++;
      if (count === max) {
        clearTimeout(t);
      }
    }, 500);
  };
}

const kitty = new Cat('Fat Daddy', 'Mrrooowww');
kitty.annoy();
```