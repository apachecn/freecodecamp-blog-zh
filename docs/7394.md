# 现代 JavaScript 中的优雅模式:Ice Factory

> 原文：<https://www.freecodecamp.org/news/elegant-patterns-in-modern-javascript-ice-factory-4161859a0eee/>

从 90 年代末开始，我就断断续续地研究 JavaScript。起初我并不喜欢它，但在推出 ES2015(又名 ES6)后，我开始欣赏 JavaScript 作为一种杰出的动态编程语言，具有巨大的表达能力。

随着时间的推移，我采用了几种编码模式，这些模式产生了更干净、更易测试、更具表现力的代码。现在，我与你分享这些模式。

我在下面的文章中提到了第一种模式——“RORO”。如果你没看过也不用担心，你可以按任意顺序阅读这些。

[**现代 JavaScript 中优雅的模式:RORO**](https://medium.freecodecamp.org/elegant-patterns-in-modern-javascript-roro-be01e7669cbd)
[*我在 JavaScript 语言被发明不久后写了我的前几行。如果你当时告诉我，我……*medium.freecodecamp.org](https://medium.freecodecamp.org/elegant-patterns-in-modern-javascript-roro-be01e7669cbd)

今天，我要向大家介绍“冰工厂”模式。

Ice Factory 就是**一个创建并返回冻结对象**的函数。我们一会儿将解开这个陈述，但是首先让我们探索为什么这个模式如此强大。

### JavaScript 类就不那么经典了

将相关功能组合到一个对象中通常是有意义的。例如，在一个电子商务应用程序中，我们可能有一个公开了一个`addProduct`函数和一个`removeProduct`函数的`cart`对象。然后我们可以用`cart.addProduct()`和`cart.removeProduct()`调用这些函数。

如果你来自一个以类为中心、面向对象的编程语言，比如 Java 或 C#，这可能感觉很自然。

如果你是编程新手——现在你已经看到了类似于`cart.addProduct()`的语句。我怀疑将功能组合在一个对象下的想法看起来很不错。

那么我们如何创建这个漂亮的小`cart`对象呢？对于现代 JavaScript，您的第一反应可能是使用`class`。类似于:

```
// ShoppingCart.js
```

```
export default class ShoppingCart {  constructor({db}) {    this.db = db  }    addProduct (product) {    this.db.push(product)  }    empty () {    this.db = []  }
```

```
 get products () {    return Object      .freeze([...this.db])  }
```

```
 removeProduct (id) {    // remove a product   }
```

```
 // other methods
```

```
}
```

```
// someOtherModule.js
```

```
const db = [] const cart = new ShoppingCart({db})cart.addProduct({   name: 'foo',   price: 9.99})
```

> ***注意*** *:为了简单起见，我对`db`参数使用了一个数组。在真实代码中，这将是类似于与实际数据库交互的[模型](http://mongoosejs.com/docs/models.html)或[回购](https://reallyshouldblogthis.blogspot.ca/2016/02/writing-pure-javascript-repository.html)的东西。*

不幸的是——尽管这看起来不错 JavaScript 中的类的行为与您预期的完全不同。

如果你不小心，JavaScript 类会咬你一口。

例如，使用`new`关键字创建的对象是可变的。所以，你实际上可以给重新分配一个方法:

```
const db = []const cart = new ShoppingCart({db})
```

```
cart.addProduct = () => 'nope!' // No Error on the line above!
```

```
cart.addProduct({   name: 'foo',   price: 9.99}) // output: "nope!" FTW?
```

更糟糕的是，使用`new`关键字创建的对象继承了用于创建它们的`class`的`prototype`。因此，对一个类'`prototype`的更改会影响从那个`class`创建的所有对象——即使更改是在对象创建后**进行的！**

看看这个:

```
const cart = new ShoppingCart({db: []})const other = new ShoppingCart({db: []})
```

```
ShoppingCart.prototype  .addProduct = () => ‘nope!’// No Error on the line above!
```

```
cart.addProduct({   name: 'foo',   price: 9.99}) // output: "nope!"
```

```
other.addProduct({   name: 'bar',   price: 8.88}) // output: "nope!"
```

此外，JavaScript 中的`this`是动态绑定的。所以，如果我们传递我们的`cart`对象的方法，我们可能会失去对`this`的引用。这非常违背直觉，会给我们带来很多麻烦。

一个常见的陷阱是将实例方法分配给事件处理程序。

考虑我们的`cart.empty`方法。

```
empty () {    this.db = []  }
```

如果我们将这个方法直接分配给网页上按钮的`click`事件…

```
<button id="empty">  Empty cart</button>
```

```
---
```

```
document  .querySelector('#empty')  .addEventListener(    'click',     cart.empty  )
```

…当用户点击空的`button`时，他们的`cart`将保持满的状态。

它**会无声地**失败，因为`this`现在会引用`button`而不是`cart`。因此，我们的`cart.empty`方法最终为我们的`button`分配了一个名为`db`的新属性，并将该属性设置为`[]`，而不是影响`cart`对象的`db`。

这是一种会让你发疯的错误，因为控制台中没有错误，你的常识会告诉你它应该工作，但它没有。

为了让它发挥作用，我们必须做到:

```
document  .querySelector("#empty")  .addEventListener(    "click",     () => cart.empty()  )
```

或者:

```
document  .querySelector("#empty")  .addEventListener(    "click",     cart.empty.bind(cart)  )
```

我认为[马蒂亚斯·皮特·约翰逊](https://www.freecodecamp.org/news/elegant-patterns-in-modern-javascript-ice-factory-4161859a0eee/undefined)T2 说得最好:

> *“`new`和`this`(在 JavaScript 中)是某种不直观的、怪异的云彩虹陷阱。”*

### 冰工厂来了

如前所述，**Ice Factory 只是一个创建并返回冻结对象**的函数。对于制冰厂，我们的购物车示例如下所示:

```
// makeShoppingCart.js
```

```
export default function makeShoppingCart({  db}) {  return Object.freeze({    addProduct,    empty,    getProducts,    removeProduct,    // others  })
```

```
 function addProduct (product) {    db.push(product)  }    function empty () {    db = []  }
```

```
 function getProducts () {    return Object      .freeze([...db])  }
```

```
 function removeProduct (id) {    // remove a product  }
```

```
 // other functions}
```

```
// someOtherModule.js
```

```
const db = []const cart = makeShoppingCart({ db })cart.addProduct({   name: 'foo',   price: 9.99})
```

请注意，我们的“奇怪的云彩虹陷阱”不见了:

*   **我们不再需要`new`。**
    我们只是调用一个普通的 JavaScript 函数来创建我们的`cart`对象。
*   **我们不再需要`this`。**
    我们可以直接从我们的成员函数中访问`db`对象。
*   我们的`cart`对象是完全不可变的。
    `Object.freeze()`冻结`cart`对象，使其不能添加新属性，不能删除或更改现有属性，也不能更改原型。只要记住`Object.freeze()`是**浅**，所以如果我们返回的对象包含一个`array`或另一个`object`，我们必须确保它们也是`Object.freeze()`。同样，如果你在一个 [ES 模块](https://hacks.mozilla.org/2015/08/es6-in-depth-modules/)之外使用一个冻结的对象，你需要在[严格模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)下确保重新赋值会导致一个错误，而不仅仅是无声的失败。

### 请给我一点隐私

冰厂的另一个好处是可以有私人会员。例如:

```
function makeThing(spec) {  const secret = 'shhh!'
```

```
 return Object.freeze({    doStuff  })
```

```
 function doStuff () {    // We can use both spec    // and secret in here   }}
```

```
// secret is not accessible out here
```

```
const thing = makeThing()thing.secret // undefined
```

这之所以成为可能，是因为 JavaScript 中的闭包，你可以在 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) 上读到更多。

### 请稍微承认一下

尽管工厂函数一直存在于 JavaScript 中，但是 Ice 工厂模式受到了道格拉斯·克洛克福特在 T2 的视频 T3 中展示的一些代码的极大启发。

下面是克罗克福德用一个他称之为“构造函数”的函数演示对象创建:

![1*KHVORQsCaE5EJFCb9FsfGA](img/1af86f52bbcfbebb4b2075d051b0ebd1.png)

Douglas Crockford demonstrating the code that inspired me.

我的 Crockford 示例的 Ice Factory 版本将如下所示:

```
function makeSomething({ member }) {  const { other } = makeSomethingElse()     return Object.freeze({     other,    method  }) 
```

```
 function method () {    // code that uses "member"  }}
```

我利用函数提升将我的 return 语句放在靠近顶部的地方，这样读者在深入细节之前会有一个很好的摘要。

我还在`spec`参数上使用了析构。我将该模式重命名为“Ice Factory ”,这样更容易记忆，也不容易与 JavaScript `class`中的`constructor`函数混淆。但基本上是一回事。

所以，该表扬就表扬，谢谢你，克罗克福德先生。

> ***注意:*** *可能值得一提的是，Crockford 认为函数“提升”了 JavaScript 的“坏的部分”，并且很可能认为我的版本是异端。我在[上一篇文章](https://medium.freecodecamp.org/constant-confusion-why-i-still-use-javascript-function-statements-984ece0b72fd)中讨论了我对此的感受，更具体地说，在[这篇评论](https://medium.com/@BillSourour/thank-you-for-your-thoughtful-response-7542ba5ff94e)中。*

### 遗传呢？

如果我们坚持构建我们的小型电子商务应用程序，我们可能很快就会意识到添加和删除产品的概念会在所有地方一次又一次地出现。

除了购物车，我们可能还有一个目录对象和一个订单对象。并且所有这些都可能公开“addProduct”和“removeProduct”的某个版本。

我们知道重复是不好的，所以我们最终会试图创建一个类似产品列表的对象，我们的购物车、目录和订单都可以继承它。

但是，与其通过继承产品列表来扩展我们的对象，不如采用有史以来最有影响力的编程书籍之一中提供的永恒原则:

> "优先选择对象组合而不是类继承."设计模式:可重用的面向对象软件的元素。

事实上，那本书的作者——俗称“四人帮”——接着说:

> “……我们的经验是，设计者过度使用继承作为重用技术，通过更多地依赖对象组合，设计通常变得更可重用(和更简单)。”

这是我们的产品清单:

```
function makeProductList({ productDb }) {  return Object.freeze({    addProduct,    empty,    getProducts,    removeProduct,    // others  )}   // definitions for   // addProduct, etc…}
```

这是我们的购物车:

```
function makeShoppingCart(productList) {  return Object.freeze({    items: productList,    someCartSpecificMethod,    // …)}
```

```
function someCartSpecificMethod () {  // code   }}
```

现在，我们可以将产品列表放入购物车，就像这样:

```
const productDb = []const productList = makeProductList({ productDb })
```

```
const cart = makeShoppingCart(productList)
```

并通过“items”属性使用产品列表。比如:

```
cart.items.addProduct()
```

通过将其方法直接合并到购物车对象中来包含整个产品列表可能很有诱惑力，如下所示:

```
function makeShoppingCart({   addProduct,  empty,  getProducts,  removeProduct,  …others}) {  return Object.freeze({    addProduct,    empty,    getProducts,    removeProduct,    someOtherMethod,    …others)}
```

```
function someOtherMethod () {  // code   }}
```

事实上，在本文的早期版本中，我就是这样做的。但后来有人向我指出这有点危险(如这里的[所解释的](https://www.reddit.com/r/programming/comments/5dxq6i/composition_over_inheritance/da8bplv/))。所以，我们最好坚持正确的物体构成。

太棒了。我被卖了！

![1*PfWy93k2QgidbBFVGySgwA](img/64d15511020b07ad47c107589544a5be.png)

Careful

每当我们学习新的东西时，特别是像软件架构和设计这样复杂的东西，我们倾向于想要硬性的规则。我们希望听到类似于“*总是*这样做”和“*从不*那样做”这样的话

我花在这方面的时间越长，我越意识到没有什么事情是*永远*和*永远不会的。*这是关于选择和权衡。

与使用类相比，使用冰工厂制作对象速度较慢，并且占用更多内存。

在我描述的用例类型中，这无关紧要。即使比班慢，冰厂还是挺快的。

如果您发现自己需要一次创建数十万个对象，或者如果您处于内存和处理能力非常宝贵的情况下，您可能需要一个类来代替。

请记住，首先分析您的应用程序，不要过早优化。大多数时候，对象创建不会成为瓶颈。

尽管我之前咆哮过，但课堂并不总是很糟糕。你不应该仅仅因为一个框架或者库使用了类就抛弃它。事实上，在他的文章[如何利用课堂和晚上睡觉](https://medium.com/@dan_abramov/how-to-use-classes-and-sleep-at-night-9af8de78ccb4)中非常雄辩地描述了这一点。

最后，我需要承认，在我向您展示的代码示例中，我做了一些固执己见的风格选择:

*   [我用函数语句代替函数表达式](https://medium.freecodecamp.org/constant-confusion-why-i-still-use-javascript-function-statements-984ece0b72fd)。
*   我将 return 语句放在顶部附近(这是通过使用函数语句实现的，见上文)。
*   我用`makeX`来命名我的工厂函数，而不是用`createX`或者`buildX`或者别的什么。
*   我的工厂函数接受一个单一的、被析构的参数对象。
*   我不使用分号([克罗克福德也不会赞同那个](https://github.com/twbs/bootstrap/issues/3057))
*   诸如此类…

你可以选择不同的风格，没关系！风格不是模式。

Ice Factory 模式只是:**使用一个函数创建并返回一个冻结的对象**。具体如何编写该函数取决于您。

如果你觉得这篇文章有用，请多次点击那个掌声图标来帮助传播。如果你想学习更多类似的东西，请在下面注册我的开发掌握时事通讯。谢谢！

#### 更新 2019:这里有一个我用这个模式的视频，很多！