# 用例子解释 JavaScript 原型

> 原文：<https://www.freecodecamp.org/news/javascript-prototype-explained-with-examples/>

## **原型**

JavaScript 是基于原型的语言，因此理解原型对象是 JavaScript 从业者需要了解的最重要的概念之一。本文将通过各种示例向您简要介绍 Prototype 对象。在阅读本文之前，您需要对 JavaScript 中的 [`this`引用有一个基本的了解。](https://guide.freecodecamp.org/src/pages/javascript/this-reference/index.md)

### **原型物体**

为了清楚起见，让我们来看看下面的例子:

```
function Point2D(x, y) {
  this.x = x;
  this.y = y;
}
```

当声明了`Point2D`函数时，将为它创建一个名为`prototype`的默认属性(注意，在 JavaScript 中，函数也是一个对象)。`prototype`属性是包含`constructor`属性的对象，其值是`Point2D`函数:`Point2D.prototype.constructor = Point2D`。当你用`new`关键字调用`Point2D`时，*新创建的对象将继承* `Point2D.prototype`的所有属性。为了检查这一点，您可以将名为`move`的方法添加到`Point2D.prototype`中，如下所示:

```
Point2D.prototype.move = function(dx, dy) {
  this.x += dx;
  this.y += dy;
}

var p1 = new Point2D(1, 2);
p1.move(3, 4);
console.log(p1.x); // 4
console.log(p1.y); // 6
```

`Point2D.prototype`被称为 ****原型对象**** 或`p1`对象的 ****原型**** 以及用`new Point2D(...)`语法创建的任何其他对象。您可以随意添加更多属性到`Point2D.prototype`对象。常见的模式是将方法声明为`Point2D.prototype`，其他属性将在构造函数中声明。

JavaScript 中的内置对象也是以类似的方式构造的。例如:

*   用`new Object()`或`{}`语法创建的对象的原型是`Object.prototype`。
*   用`new Array()`或`[]`语法创建的数组的原型是`Array.prototype`。
*   与其他内置对象如`Date`、`RegExp`以此类推。

`Object.prototype`被所有对象继承，它没有原型(它的原型是`null`)。

### **原型链**

原型链机制很简单:当您访问对象`obj`上的属性`p`时，JavaScript 引擎将在`obj`对象中搜索该属性。如果搜索失败，则继续在`obj`对象的原型中搜索，以此类推，直到到达`Object.prototype`。如果搜索完成后，没有发现任何东西，结果将是`undefined`。例如:

```
var obj1 = {
  a: 1,
  b: 2
};

var obj2 = Object.create(obj1);
obj2.a = 2;

console.log(obj2.a); // 2
console.log(obj2.b); // 2
console.log(obj2.c); // undefined
```

在上面的代码片段中，语句`var obj2 = Object.create(obj1)`将用原型`obj1`对象创建`obj2`对象。换句话说，`obj1`默认成为`obj2`的原型，而不是`Object.prototype`。如您所见，`b`不是`obj2`的属性，您仍然可以通过原型链访问它。然而，对于`c`属性，您会得到`undefined`值，因为在`obj1`和`Object.prototype`中找不到它。

### **类**

在 ES2016 中，我们现在可以使用`Class`关键字以及上面提到的方法来操作`prototype`。JavaScript `Class`吸引了来自 OOP 背景的开发人员，但它本质上做的和上面一样。

```
class Rectangle {
  constructor(height, width) {
    this.height = height
    this.width = width
  }

  get area() {
    return this.calcArea()
  }

  calcArea() {
    return this.height * this.width
  }
}

const square = new Rectangle(10, 10)

console.log(square.area) // 100
```

这基本上与以下内容相同:

```
function Rectangle(height, width) {
  this.height = height
  this.width = width
}

Rectangle.prototype.calcArea = function calcArea() {
  return this.height * this.width
}
```

类中的`getter`和`setter`方法将一个对象属性绑定到一个函数，当查找该属性时将调用该函数。这只是语法上的糖，帮助*更容易查找*或*设置*属性。

## 关于 JS 原型的更多信息:

*   [理解 JavaScript 原型所需的一切](https://www.freecodecamp.org/news/all-you-need-to-know-to-understand-javascripts-prototype-a2bff2d28f03/)