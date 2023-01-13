# 用一个故事解释 JavaScript 的 var、let 和 const 变量

> 原文：<https://www.freecodecamp.org/news/javascripts-var-let-and-const-variables-explained-with-a-story-2038e3c6b2f9/>

普拉塔纳·s·桑纳马尼

# 用一个故事解释 JavaScript 的 var、let 和 const 变量

![dqtsGiSRlWg950TkvenjDVMhKMWgjl3OWxKF](img/04e274a99bb8f1c3e82c60c66ca12309.png)

“Assorted woodwork boxes collection with varied floral art designs on red fabric surface in Cambridge” by [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将探索 JavaScript 中`var`的历史，对`let`和`const`的需求，以及它们之间的区别。

这篇文章由两部分组成:虚构部分和技术解释。

这篇虚构的文章旨在帮助初学者理解概念，但是几个部分被简化了，并不总是呈现精确的 1:1 类比。

开始吧！

### 三个变量的故事

JavaScript town 是一个繁华的海边小镇，有一个商业区，到处都是高楼大厦。

自古以来，JavaScript 小镇的居民就用盒子来存放他们的贵重物品，尤其是他们珍贵的黄金弹珠。为此，居民有两个选择:

1.  他们可以把金球直接放在盒子里(按值传递)
2.  如果他们有大量的金弹珠，以致放不进盒子里，他们可以在盒子里放一张特殊的纸，表明他们把它们存放在哪里。例如，这张纸可以说“储物柜中的第二个抽屉”(通过引用传递)

由于该镇以法律和秩序为荣，他们建立了一些规则和程序。

#### ***店铺规则***

1.  为了保持城镇的宁静，商店只能建在山上(功能创造了他们自己的地方范围)
2.  规则 1 的唯一例外是海平面上的特殊商店(全球范围)。
3.  商店可以有内部商店来帮助支付租金(嵌套功能)。然而，每个内部商店都被要求位于比地主商店的山丘更高的山丘上(本地功能范围)。
4.  一家商店可以有“特价”柜台，比如“`If`你已经超过 20 岁了，在这里买一盒特价商品。”以及“`For`(每)子`of`你家，在这里买个小孩的盒子”(其他街区如`if`和 loops)。
5.  每个商店都必须有一个“申报-初始化”柜台，在入口处有一名警卫，负责维护一本登记日志(悬挂在相应范围的顶部)。
6.  每个商店都可以有无限的“分配”柜台，店员会把居民的金弹珠放在盒子里。

#### 盒子市场监管规则

1.  这些盒子只能从海平面特殊商店或山上的商店购买(变量可以具有全球或本地范围)。
2.  在海平面或任何山上，居民只能拥有一个单色的`Vary`盒子(不允许重复的标识符)。
3.  从创建的那一刻起，`Vary`盒子就不能是空的。它必须始终包含棉花(`undefined`)或金弹珠(提升的效果)。
4.  一旦居民离开商店(并因此下山)，他们在商店购买的所有盒子都消失了(变量范围的结尾)。

#### 居民购买“Vary”盒子的程序

在本文中，我们将跟随居民约翰的旅程。

1.  约翰走进商店，在“申报-初始化”柜台申报他想要购买的盒子的颜色。警卫在他的登记簿上记下了这一点。
2.  警卫变出一个彩色的盒子，装满棉花，递给约翰。
3.  约翰得到了轮到他的一张票，当它到达时，他走向“分配”柜台。在那之前，他可以拿着他的盒子，但是不能把他的金球放进去。
4.  在柜台，约翰把他的盒子和金弹珠交给店员，店员拿走棉花，把金弹珠放进去，然后还给他。

自然，这些规则带来了特殊的问题。

1.  在等待“分配”柜台的漫长时间里，约翰会忘记他还没有把他的金弹珠放进他的盒子里。他会打开它向他的朋友吹嘘，发现只有棉花。真扫兴。
2.  通常，约翰会忘记他已经在商店里买了某种颜色的盒子，并重新注册了同样颜色的盒子。这将立即导致他现有的盒子消失(和金弹珠！！)，接着警卫变戏法似的拿出一个装满棉花的新盒子。没有预警！这在“特价商品”柜台尤其普遍。

你可以想象这种情况有多令人沮丧。随着 JavaScript 镇的居民失去理智，镇议会决定采取行动。

在 2015 年的一次盛大的镇民大会上，他们自豪地推出了两款新盒子:`Lety`和`Consty`。

他们还介绍了另一个重大变化:从`Lety`和`Consty`商店移除“特价”柜台。相反，这些柜台被升级为内部商店，建在商店内的一座小山上。

#### 购买“Lety”和“Consty”盒子的规则

1.  约翰走进商店，在“申报”柜台申报他想要购买的盒子的类型和颜色。警卫在他的登记簿上记下了这一点。这些信息朦胧地出现在巨大的挂钟上，可以看到，但不能使用，被称为“时间死区”。
2.  轮到约翰时，他得到一张票。由于箱子不是在申报时创建的，所以不能使用。

这就是`Lety`和`Consty`购买规则的不同之处。

#### “Lety”规则:

1.  一旦轮到约翰，他就走向“初始化”柜台。
2.  在柜台，约翰可以选择买一个空的`Lety`盒子，或者买一个`Lety`盒子，然后立刻把他的金弹珠放在里面。
3.  根据他的选择，店员变出一个盒子，在里面装满棉花，或者把它交给“分配”柜台，在那里约翰的金弹珠被放在里面。

#### “常数”规则

`Consty`箱子**极其**特别。这些盒子内衬一层黄金，用一把锁密封着，对店员来说是如此珍贵，以至于他们在不知道里面到底会放些什么的情况下拒绝出售。

1.  一旦轮到约翰，他走向“初始化-分配”柜台。
2.  约翰**被要求**把他的金弹珠交给店员，店员召唤出彩色`Consty`盒子，把金弹珠放进去，**永远锁上盒子**。

如果你记得的话，约翰可以直接把他的金弹珠放在盒子里，或者放一张特殊的纸来标明他的金弹珠的位置。

1.  如果他把他的金弹珠放在盒子里，他就不能再添加或移除它们了。它们被永远锁住了。
2.  然而，如果他把那张特殊的纸放在哪里，那就有点不同了。虽然他不能替换这张纸，但他可以在纸上指定的位置添加或移除他的金球。

让我们回到促使发明`Lety`和`Consty`盒子的特殊问题，并决定它们是否得到解决。

> 在等待“分配”柜台的漫长时间里，约翰会忘记他还没有把他的金弹珠放进他的盒子里。他会打开它向他的朋友吹嘘，发现只有棉花。真扫兴。

因为`Lety`和`Consty`盒子直到 John 分别走向“初始化”或“初始化-赋值”计数器时才被创建，他知道他没有盒子，因此没有试图使用它。即使他这样做了，安装在商店里的响亮的警报器开始响起来提醒他这个事实。

> 通常，约翰会忘记他已经在商店里买了某种颜色的盒子，并重新注册相同颜色的盒子。这将立即导致他现有的盒子消失(和金弹珠！！)，接着警卫变戏法似的拿出一个装满棉花的新盒子。没有预警！这在“特价商品”柜台尤其普遍。

这是通过取消“特别优惠”计数器并引入以下规则来实现的:

***居民一旦在`Lety`或`Consty`店铺的“申报”柜台登记了某个彩盒，就不能再在该店铺重新登记相同的彩盒！如果他这样做，响亮的警报将开始响起。***

这些奇妙的新盒子和规则再次给 JavaScript 小镇带来了和平与宁静，从此每个人都过上了幸福的生活。

### 深入技术细节

让我们回顾一下`var`、`let`和`const`的技术层面来理解这个故事。

如果你对提升和范围(函数级和块级)不熟悉，我推荐你看我之前的文章 [**这里**](https://codeburst.io/hoist-your-knowledge-of-javascript-hoisting-59b73124b430) **。**

以下是理解我在上面使用的希尔斯类比的摘录:

> 为了增加我们对块级和功能级范围的理解，让我们考虑一下山的类比。假设全局范围是海平面上的陆地，局部范围是丘陵。如果你站在山顶上，你可以看到海拔以下的变量。但是，如果你是海平面，你就看不到(访问)更高海拔的变量。

> 在 C++中，每一个块`{}`都会导致一个新山丘(局部范围)的形成，其高度比它所在的高度高一级。嵌套块导致多级山丘。

> 在 JavaScript 中，只有一个函数会导致一个新山丘的形成(局部范围)。其他区块如`if`区块出现在同一高度。

> 因此，如果在某个山头(块)上声明了一个变量，就可以从那个山头(块)和它上面的所有山头(块)上访问它。

### 变量的生命周期

**声明阶段**:变量在其作用域内注册，可以是全局/函数/块作用域。在这个阶段，还没有分配内存。

**初始化阶段**:为变量分配内存，创建绑定，用`undefined`初始化变量。

**赋值阶段**:给变量赋值。

需要注意的是，变量声明和声明阶段是不一样的！

变量声明是一个像`var a`这样的语句。

声明阶段是 JavaScript 编译器执行的一个步骤。在这一步中，当编译器遇到一个变量声明时，它会在其对应的作用域中声明/注册它(如果该声明尚不存在)。稍后，编译器生成的代码由 JavaScript 引擎执行。

### 定义变量

1.  全局范围或函数范围
2.  值可以更新
3.  可以重新申报
4.  提升:在作用域中注册，用`undefined`初始化

下面是一个简单的例子，我们初始化一个变量，更新它的值，并重新声明它。

```
// Hoistedconsole.log(a); // undefined
```

```
var a = 10;console.log(a); // 10
```

```
a = 20; // value updated: OKconsole.log(a); // 20
```

```
var a = 30; // re-declared: OKconsole.log(a); // 30
```

在作用域的顶部，所有变量都在它们对应的作用域中声明，并用值`undefined`初始化。注册和初始化是耦合的。因此，变量`a`可以从作用域的顶部使用。因此，当我们试图在声明`a`之前访问它的值时，它不会抛出错误。毋宁说，`undefined`是打印出来的。这就是所谓的可变提升。

下面的例子展示了`var`的功能范围。

```
function outerFunc() {  var a = 10;  if (a > 5) {    var a = 20;    console.log(a); // 20  }  console.log(a); // 20}
```

变量`a`最初是在`outerFunc`的范围内声明的。由于`if`块没有创建新的作用域，当我们重新声明变量`a`时，先前的变量`a`被清除，一个新的变量`a`被创建，其值为`20`。

意外地重新声明`var`变量是开发人员经常犯的错误，这是由于无声的重新声明和对函数范围理解的混乱。

### 让

1.  块范围
2.  值可以更新
3.  不能重新声明
4.  托管但未初始化

下面是一个简单的例子，我们初始化一个变量，更新它的值，并尝试重新声明它。

```
console.log(a); //   ReferenceError: a is not defined
```

```
let a = 10;console.log(a); // 10
```

```
a = 20;console.log(a); // 20
```

```
let a = 30; // SyntaxError: Identifier 'a' has already been declared
```

允许更新一个`let`变量。但是，如果您尝试重新声明它，您会遇到一个`SyntaxError`。这保护了开发人员免于对变量进行无声的和意外的重新声明。

`let`变量吊起来了吗？

这是一个棘手的问题。互联网在这个问题上存在分歧:双方都有自己的观点。一些开发人员认为`let`(和`const`)变量没有被提升，因为在它们的声明语句到达之前不能被访问，不像`var`。不过这个答案真的要看你对吊装的定义了。如果提升是一个变量的声明和初始化阶段在其对应范围顶部的耦合，那么`let`和`const`变量不被提升。

然而，在阅读了几个观点后，我决定采用 MDN 对提升的定义，因为这与事实并不接近。

> `let`绑定创建在包含声明的(块)范围的顶部，通常称为“提升”。(MDN)

根据这个定义，我们这个问题的答案是肯定的。`let`变量被提升，但没有用`undefined`初始化。因此，它们存在于一个称为“时间死区”的时间段内，从块的开始直到它们的定义被评估。试图在 TDZ 访问它们会抛出一个`ReferenceError`，如示例所示。

下面的例子显示了`let`的块范围。

```
function outerFunc() {  let a = 10;  if (a > 5) {    let a = 20;    console.log(a); // 20  }  console.log(a); // 10}
```

变量`a`的第一次声明在`outerFunc`的范围内。`if`块创建了一个新的作用域，当我们第二次声明变量`a`时，它被注册到新的作用域中。这与`outerFunc`的范围无关。因此，创建了一个单独的变量`a`，我们可以观察到内部变量`a`的变化不会影响外部变量`a`。

这使得开发人员可以轻松地在条件和循环块中创建临时变量，而不必搜索函数中是否已经存在该变量。

### 常数

1.  块范围
2.  绑定是不可变的(但是值可以改变也可以不改变)
3.  不能重新声明
4.  托管但未初始化

下面是一个简单的例子，我们初始化一个变量，尝试更新它的值，并尝试重新声明它。

```
console.log(a); //  ReferenceError: a is not defined
```

```
const a = 10;console.log(a); // 10
```

```
a = 20; // TypeError: Assignment to constant variable.
```

```
const a = 30; // SyntaxError: Identifier 'a' has already been declared
```

```
const b; // SyntaxError: Missing initializer in const declaration
```

与`let`变量类似，`const`变量被提升，但没有用`undefined`初始化。试图在时间死区访问它们会抛出一个`ReferenceError`。

如果我们试图在没有赋值的情况下初始化一个`const`变量，就像上面的例子中的`const b;`，我们会遇到一个`SyntaxError: Missing initializer in const declaration`。同样，我们不能重新声明`const`变量。这就引出了一个`SyntaxError`。

让我们暂时推迟更新`const`变量的讨论。

下面是`const`变量块级范围的一个例子:

```
function outerFunc() {  const a = 10;  if (a > 5) {    const a = 20;    console.log(a); // 20  }  console.log(a); // 10}
```

上述行为类似于`let`变量，其中为`if`块创建了一个新的作用域，因此，对内部变量`a`的更改不会影响外部变量`a`。

让我们回到更新`const`变量的讨论。

有一个常见的误解，即`const`变量保存常量值，并且永远不能更新。但是，`const`的工作原理不同。

初始赋值后，`const`变量的**绑定**为**不可变**。，因此，对存储在`const`变量中的内容的**引用**不能被修改。用最简单的话来说，这意味着你不能有一个左边只有**变量**的语句，后面跟着一个等号`=`，右边是一个新值。

但是，值是否可以更新取决于其中存储了什么。让我们考虑两种情况:

1.  原始数据类型:布尔值、空值、未定义、数字、字符串、符号
2.  目标

如果一个变量被赋予了一个原始数据类型，那么这个数据类型将通过**值**传递。因此，如果我们有一个语句`let x = 10`，我们可以想象`x`包含数字`10`。

如果一个变量被赋予了一个对象，那么这个对象将通过**引用**传递。因此，如果我们有一个语句`let x = [1,2,3]`，`x`不包含数组`[1,2,3]`。相反，它包含数组`[1,2,3]`在创建后在内存中存储位置的引用(地址)。因此，我们可以想象`x`包含一个地址，比如`5274621`。

让我们看看原始数据类型和对象数据类型的例子:

```
// Booleanconst a = true;a = false; // TypeError: Assignment to constant variable.
```

```
// Nullconst b = null;b = 10; // TypeError: Assignment to constant variable.
```

```
// Undefinedconst c = undefined;c = 10; // TypeError: Assignment to constant variable.
```

```
// Numberconst d = 50;d = 100; // TypeError: Assignment to constant variable.
```

```
// Stringconst e = 'hello';e = 'world'; // TypeError: Assignment to constant variable.
```

```
// Symbolconst f = Symbol('foo');f = 100; // TypeError: Assignment to constant variable.
```

正如我们在上面看到的，试图更新任何原始数据类型的值都会导致一个`TypeError`。

```
/* Arrays are stored by reference.Hence, although the binding is immutable, the values are not. */
```

```
const c = [1,2,3];
```

```
c.push(10); // No errorconsole.log(c); // [1,2,3,10]
```

```
c.pop(); // No errorconsole.log(c); // [1,2,3]
```

```
c = [4,5,6]; // TypeError: Assignment to constant variable.
```

正如我们在上面看到的，我们可以从数组中推送和弹出项目，因为这只会修改`const`变量所指向的内容，而不会试图覆盖`const`变量本身的内容。然而，如果我们试图通过重新分配一个全新的数组`c = [4,5,6]`来更新`const`变量的绑定，它会抛出一个`TypeError`。

```
/* Objects are stored by reference.Hence, although the binding is immutable, the values are not. */
```

```
const d = { name: 'John Doe', age: 35};
```

```
d.age = 40; // Modifying a property: No errorconsole.log(d); // { name: 'John Doe', age: 40};
```

```
d.zipCode = '52534'; // Adding a property: No errorconsole.log(d); // { age: 40, name: "John Doe", zipCode: '52534; }
```

```
d = { name: 'Mary Jane', age: 25}; // TypeError: Assignment to constant variable.
```

正如我们在上面看到的，我们可以修改和添加对象的属性，因为这只是修改了`const`变量指向的内容，而不是试图覆盖`const`变量本身的内容。然而，如果我们试图通过重新分配一个全新的对象`d = { name: 'Mary Jane', age: 25 };`来更新`const`变量的绑定，它会抛出一个`TypeError`。

### 什么时候该用什么？

JavaScript 现在有三种变量，一个自然的问题是什么时候使用什么。

在引入了块范围的`let`之后，通常不鼓励使用`var`，以避免与函数级范围混淆、意外的重声明以及用`undefined`值提升 bug。除非你有令人信服的理由使用`var`的功能范围，否则就使用`let`。

使用`const`保存事实值，比如`const PI = 3.14`，或者在程序的整个执行过程中严格保持不变的值。

一种常见的编程方法是，开发人员从用`const`声明所有变量开始，如果需要的话，逐步将它们转换成`let`变量。就我个人而言，我从`let`变量开始，如果我看到需要，就把它们转换成`const`变量。没有固定的方法，您应该使用最适合您的代码的方法。

如果你有时间，我强烈建议你再读一遍这篇虚构的文章，因为它会巩固你头脑中与额外技术知识的联系。

感谢您的阅读！我希望你学到了新的东西，我也希望收到反馈。

在 Twitter 上关注我[这里](https://twitter.com/prar_s)，在 LinkedIn 上关注我[这里](https://www.linkedin.com/in/prarthana-sannamani/)。

参考资料:

1.  [*https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/let*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
2.  [*https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/const*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
3.  [*https://dmitripavlutin . com/variables-life cycle-and-why-let-is-not-hanged/*](https://dmitripavlutin.com/variables-lifecycle-and-why-let-is-not-hoisted/)
4.  [*https://github.com/getify/You-Dont-Know-JS*](https://github.com/getify/You-Dont-Know-JS)