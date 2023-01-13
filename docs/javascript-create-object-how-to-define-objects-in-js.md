# JavaScript 创建对象——如何在 JS 中定义对象

> 原文：<https://www.freecodecamp.org/news/javascript-create-object-how-to-define-objects-in-js/>

对象是面向对象编程中封装的主要单位。在本文中，我将描述几种用 JavaScript 构建对象的方法。它们是:

*   对象文字
*   Object.create()
*   班级
*   工厂功能

## 对象文字

首先，我们需要区分数据结构和面向对象的对象。数据结构有公共数据，没有行为。这意味着他们没有方法。

我们可以很容易地使用对象字面语法创建这样的对象。看起来是这样的:

```
const product = {
  name: 'apple',
  category: 'fruits',
  price: 1.99
}

console.log(product);
```

JavaScript 中的对象是键值对的动态集合。该键始终是一个字符串，并且在集合中必须是唯一的。这个值可以是一个原语，一个对象，甚至是一个函数。

我们可以用点或正方形符号来访问一个属性。

```
console.log(product.name);
//"apple"

console.log(product["name"]);
//"apple"
```

下面是一个例子，其中的值是另一个对象。

```
const product = {
  name: 'apple',
  category: 'fruits',
  price: 1.99,
  nutrients : {
   carbs: 0.95,
   fats: 0.3,
   protein: 0.2
 }
}
```

`carbs`属性的值是一个新对象。下面是我们如何访问`carbs`属性。

```
console.log(product.nutrients.carbs);
//0.95
```

### 速记属性名

考虑这样一种情况，我们将属性值存储在变量中。

```
const name = 'apple';
const category = 'fruits';
const price = 1.99;
const product = {
  name: name,
  category: category,
  price: price
}
```

JavaScript 支持所谓的简写属性名。它允许我们只使用变量的名字来创建一个对象。它将创建一个同名的属性。下一个对象文字等同于前一个。

```
const name = 'apple';
const category = 'fruits';
const price = 1.99;
const product = {
  name,
  category,
  price
}
```

## 对象.创建

接下来，我们来看看如何用行为实现对象，面向对象的对象。

JavaScript 拥有所谓的原型系统，允许对象间共享行为。主要思想是用一个公共行为创建一个称为原型的对象，然后在创建新对象时使用它。

原型系统允许我们创建从其他对象继承行为的对象。

让我们创建一个原型对象，它允许我们添加产品并从购物车中获取总价。

```
const cartPrototype = {
  addProduct: function(product){
    if(!this.products){
     this.products = [product]
    } else {
     this.products.push(product);
    }
  },
  getTotalPrice: function(){
    return this.products.reduce((total, p) => total + p.price, 0);
  }
}
```

注意，这次属性 `addProduct`的值是一个函数。我们也可以使用一种更短的形式，称为速记方法语法，来编写前面的对象。

```
const cartPrototype = {
  addProduct(product){/*code*/},
  getTotalPrice(){/*code*/}
}
```

`cartPrototype`是原型对象，它保持由两个方法`addProduct`和`getTotalPrice`表示的共同行为。它可用于构建继承此行为的其他对象。

```
const cart = Object.create(cartPrototype);
cart.addProduct({name: 'orange', price: 1.25});
cart.addProduct({name: 'lemon', price: 1.75});

console.log(cart.getTotalPrice());
//3
```

`cart`对象以`cartPrototype`为原型。它继承了它的行为。`cart`有一个指向原型对象的隐藏属性。

当我们在一个对象上使用一个方法时，首先在对象本身而不是它的原型上搜索这个方法。

### 这

注意，我们使用一个名为`this`的特殊关键字来访问和修改对象上的数据。

记住函数是 JavaScript 中独立的行为单位。它们不一定是对象的一部分。当它们存在时，我们需要一个引用来允许函数访问同一个对象上的其他成员。`this`是函数上下文。它提供对其他属性的访问。

### 数据

您可能想知道为什么我们没有在原型对象本身上定义和初始化`products`属性。

我们不应该那样做。原型应该用于共享行为，而不是数据。共享数据将导致几个购物车对象上有相同的产品。考虑下面的代码:

```
const cartPrototype = {
  products:[],
  addProduct: function(product){
      this.products.push(product);
  },
  getTotalPrice: function(){}
}

const cart1 = Object.create(cartPrototype);
cart1.addProduct({name: 'orange', price: 1.25});
cart1.addProduct({name: 'lemon', price: 1.75});
console.log(cart1.getTotalPrice());
//3

const cart2 = Object.create(cartPrototype);
console.log(cart2.getTotalPrice());
//3
```

从`cartPrototype`继承通用行为的`cart1`和`cart2`对象也共享相同的数据。我们不想那样。原型应该用于共享行为，而不是数据。

## 班级

原型系统不是构建对象的常见方式。开发人员更熟悉在类之外构建对象。

类语法允许以一种更熟悉的方式创建具有共同行为的对象。它仍然在幕后创建相同的原型，但是语法更加清晰，并且我们还避免了之前与数据相关的问题。类提供了一个特定的位置来定义每个对象的不同数据。

下面是使用类 sugar 语法创建的同一个对象:

```
class Cart{
  constructor(){
    this.products = [];
  }

  addProduct(product){
      this.products.push(product);
  }

  getTotalPrice(){
    return this.products.reduce((total, p) => total + p.price, 0);
  }
}

const cart = new Cart();
cart.addProduct({name: 'orange', price: 1.25});
cart.addProduct({name: 'lemon', price: 1.75});
console.log(cart.getTotalPrice());
//3

const cart2 = new Cart();
console.log(cart2.getTotalPrice());
//0
```

请注意，该类有一个构造函数方法，它为每个新对象初始化不同的数据。构造函数中的数据不在实例之间共享。为了创建一个新的实例，我们使用了`new`关键字。

我认为类的语法对大多数开发者来说更加清晰和熟悉。然而，它做了类似的事情，它用所有的方法创建了一个原型，并用它来定义新的对象。原型可以通过`Cart.prototype`访问。

事实证明，原型系统足够灵活，允许类语法。因此，可以使用原型系统来模拟类系统。

### 私有财产

唯一不同的是，新对象上的`products`属性在默认情况下是公共的。

```
console.log(cart.products);
//[{name: "orange", price: 1.25}
// {name: "lemon", price: 1.75}]
```

我们可以使用散列前缀`#`将其设为私有。

私有属性是用`#name`语法声明的。`#`是属性名本身的一部分，应该用于声明和访问属性。这里有一个宣布`products`为私有财产的例子:

```
class Cart{
  #products
  constructor(){
    this.#products = [];
  }

  addProduct(product){
    this.#products.push(product);
  }

  getTotalPrice(){
    return this.#products.reduce((total, p) => total + p.price, 0);
  }
}

console.log(cart.#products);
//Uncaught SyntaxError: Private field '#products' must be declared in an enclosing class
```

### 工厂功能

另一种选择是将对象创建为闭包的集合。

闭包是一个函数从另一个函数访问变量和参数的能力，即使在外部函数已经执行之后。看看用所谓的工厂函数构建的`cart`对象。

```
function Cart() {
  const products = [];

  function addProduct(product){
    products.push(product);
  }

  function getTotalPrice(){
    return products.reduce((total, p) => total + p.price, 0);
  }

  return {
   addProduct,
   getTotalPrice
  }
}

const cart = Cart();
cart.addProduct({name: 'orange', price: 1.25});
cart.addProduct({name: 'lemon', price: 1.75});
console.log(cart.getTotalPrice());
//3
```

`addProduct`和`getTotalPrice`是两个内部函数，从它们的父级访问变量`products`。在父变量`Cart`执行后，它们可以访问`products`变量事件。`addProduct`和`getTotalPrice`是共享同一个私有变量的两个闭包。

`Cart`是一个工厂函数。

用工厂函数创建的新对象`cart`具有私有变量`products`。它不能从外面进入。

```
console.log(cart.products);
//undefined
```

工厂函数不需要关键字`new`,但是如果你愿意，你可以使用它。不管你是否使用它，它都会返回相同的对象。

## 概述

通常，我们使用两种类型的对象，有公共数据但没有行为的数据结构和有私有数据和公共行为的面向对象的对象。

使用对象文字语法可以很容易地构建数据结构。

JavaScript 提供了两种创建面向对象对象的创新方法。第一种是使用原型对象来共享公共行为。对象从其他对象继承。类提供了一个很好的语法来创建这样的对象。

另一种选择是将对象定义为闭包的集合。

关于闭包和函数编程技术的更多内容，请查看我的系列丛书[用 JavaScript 和 React 进行函数编程](https://www.amazon.com/gp/product/B08BW8BY1H)。

**JavaScript****书中的** [**函数式编程即将问世。**](https://www.amazon.com/dp/B08CZZ4FQQ)