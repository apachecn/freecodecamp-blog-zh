# 下一次开发人员面试的权威 JavaScript 手册

> 原文：<https://www.freecodecamp.org/news/the-definitive-javascript-handbook-for-a-developer-interview-44ffc6aeb54e/>

古斯塔沃·阿泽维多

![1*FfREbc94Ge3K3DYG_tEqaQ](img/11b7826fec585a6e4bf85cbaca949a47.png)

根据[栈溢出调查](https://insights.stackoverflow.com/survey/2017#most-popular-technologies)，JavaScript 是最受欢迎的编程语言，自 2014 年以来一直如此。难怪超过 1/3 的开发人员工作需要一些 JavaScript 知识。所以，如果你打算在不久的将来从事开发工作，你应该熟悉这种极其流行的语言。

这篇文章的目的是将开发者访谈中经常提到的所有 JavaScript 概念汇集在一起。编写它是为了让您可以在一个地方回顾您需要了解的关于 JavaScript 的一切。

### **类型&胁迫**

内置 7 种类型:`null`、`undefined`、`boolean`、`number`、`string`、`object` 、`symbol` (ES6)。

所有这些类型都被称为原语，除了`object`。

```
typeof 0              // number
typeof true           // boolean
typeof 'Hello'        // string
typeof Math           // object
typeof null           // object  !!
typeof Symbol('Hi')   // symbol (New ES6)
```

*   **Null vs. Undefined**

**未定义**是没有定义。它用作未初始化变量、未提供的函数参数以及缺少对象属性的默认值。当没有显式返回时，函数返回`undefined`。

**Null** 是没有值。它是一个赋值值，可以作为“无值”的表示赋给变量。

*   **隐性强制**

看一下下面的例子:

```
var name = 'Joey';
if (name) {
  console.log(name + " doesn't share food!")  // Joey doesn’t share food!
}
```

在这种情况下，字符串变量`name`被强制为真，这样就有了“乔伊不分享食物！”打印在我们的控制台上。但是你怎么知道什么会被强制为真，什么会被强制为假呢？

当强制布尔强制时，假值是将被强制为`false`的值。

虚假值:`""`、`0`、`null`、`undefined`、`NaN`、`false`。

任何没有明确列在假列表上的都是真的— **布尔被强制为真**。

```
Boolean(null)         // false
Boolean('hello')      // true 
Boolean('0')          // true 
Boolean(' ')          // true 
Boolean([])           // true 
Boolean(function(){}) // true
```

是的。你没看错。空数组、对象和函数被强制为真！

*   **字符串&号码强制**

首先需要注意的是`+`操作符。这是一个棘手的操作符，因为它对数字加法和字符串连接都有效。

但是，*、/和`-`运算符是数字运算的专用运算符。当这些运算符与字符串一起使用时，它会将字符串强制转换为数字。

```
1 + "2" = "12"
"" + 1 + 0 = "10"
"" - 1 + 0 = -1
"-9\n" + 5 = "-9\n5"
"-9\n" - 5 = -14
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
null + 1 = 1
undefined + 1 = NaN
```

*   **== vs. ===**

广为流传的是`==`检查相等，`===`检查相等和类型。这是一种误解。

事实上，==在强制的情况下检查**相等**，而===在不强制的情况下检查相等— **严格相等**。

```
2 == '2'            // True
2 === '2'           // False
undefined == null   // True
undefined === null  // False
```

强制可能很棘手。看一下下面的代码:

你对下面的比较有什么期待？
T0`(1)`

这个比较实际上会返回 True。为什么？
真正发生的是，如果你将一个`boolean`与另一个`boolean`进行比较，JavaScript 会将那个`boolean`强制转换为一个`number`并进行比较。`(2)`

这个比较现在是在一个`number`和一个`string`之间进行。JavaScript 现在将那个`string`强制转换为`number`，并比较两个数字。`(3)`

在这种情况下，最终比较`0 == 0`为真。

```
'0' == false   (1)
'0' == 0       (2)
 0  == 0       (3)
```

为了全面理解这种比较是如何进行的，你可以查看 ES5 文档[这里](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3)。

对于备忘单，可以点击[这里](http://dorey.github.io/JavaScript-Equality-Table/)。

一些需要注意的微妙对比:

```
false == ""  // true
false == []  // true
false == {}  // false
"" == 0      // true
"" == []     // true
"" == {}     // false
0 == []      // true
0 == {}      // false
0 == null    // false
```

### 值与基准电压

简单值(也称为基元)总是由值副本赋值:`null`、`undefined`、`boolean`、`number`、`string`和 ES6 `symbol`。

复合值总是在 assignment: objects 上创建引用的副本，其中包括数组和函数。

```
var a = 2;        // 'a' hold a copy of the value 2.
var b = a;        // 'b' is always a copy of the value in 'a'
b++;
console.log(a);   // 2
console.log(b);   // 3
var c = [1,2,3];
var d = c;        // 'd' is a reference to the shared value
d.push( 4 );      // Mutates the referenced value (object)
console.log(c);   // [1,2,3,4]
console.log(d);   // [1,2,3,4]
/* Compound values are equal by reference */
var e = [1,2,3,4];
console.log(c === d);  // true
console.log(c === e);  // false
```

要按值复制一个复合值，需要用**制作** 的副本。引用没有指向原始值。

### 范围 *e*

范围是指执行上下文。它定义了代码中变量和函数的可访问性。

**全局作用域**是最外层的作用域。在函数外部声明的变量在全局范围内，可以在任何其他范围内访问。在浏览器中，窗口对象是全局范围。

**局部作用域**是嵌套在另一个函数作用域内的作用域。在局部作用域中声明的变量在该作用域和任何内部作用域中都是可访问的。

```
function outer() {
  let a = 1;
  function inner() {
    let b = 2;
    function innermost() {
      let c = 3;
      console.log(a, b, c);   // 1 2 3
    }
    innermost();
    console.log(a, b);        // 1 2 — 'c' is not defined
  }
  inner();
  console.log(a);             // 1 — 'b' and 'c' are not defined
}
outer();
```

您可能会认为示波器是一系列尺寸递减的门(从最大到最小)。一个矮个子能穿过最小的门——最里面的*——*，也能穿过任何更大的门——外面的**。**

例如，一个被卡在第三道门上的高个子可以进入所有之前的门——**外部视野***——*，但不能进入任何其他的门——**内部视野**。

### 提升

在编译阶段将`var`和`function`声明“移动”到各自作用域顶部的行为称为**提升**。

函数声明被完全提升。这意味着声明的函数可以在定义之前被调用。

```
console.log(toSquare(3));  // 9

function toSquare(n){
  return n*n;
}
```

变量被部分吊起。申报单被悬挂，但不是它的转让。

`let`和`const`不吊。

```
{  /* Original code */
  console.log(i);  // undefined
  var i = 10
  console.log(i);  // 10
}

{  /* Compilation phase */
  var i;
  console.log(i);  // undefined
  i = 10
  console.log(i);  // 10
}
// ES6 let & const
{
  console.log(i);  // ReferenceError: i is not defined
  const i = 10
  console.log(i);  // 10
}
{
  console.log(i);  // ReferenceError: i is not defined
  let i = 10
  console.log(i);  // 10
}
```

### 函数表达式与函数声明

*   **函数表达式**
    函数表达式是在执行到它的时候创建的，从那时起就可以使用了——它没有被提升。

```
var sum = function(a, b) {
  return a + b;
}
```

*   **函数声明**
    函数声明在定义之前和之后都可以被调用——它被提升。

```
function sum(a, b) {
  return a + b;
}
```

### 变量:var、let 和 const

在 ES6 之前，只能使用`var`声明变量。在另一个函数中声明的变量和函数不能被任何封闭作用域访问，它们是函数作用域的。

在块范围内声明的变量，比如`if`语句和`for`循环，可以从块的左花括号和右花括号之外访问。

**注**:未声明的变量——没有`var`、`let`或`const`的赋值——在全局范围内创建一个`var`变量。

```
function greeting() {
  console.log(s) // undefined
  if(true) {
    var s = 'Hi';
    undeclaredVar = 'I am automatically created in global scope';
  }
  console.log(s) // 'Hi'
}
console.log(s);  // Error — ReferenceError: s is not defined
greeting();
console.log(undeclaredVar) // 'I am automatically created in global scope'
```

ES6 `let`和`const`是新的。它们不是变量声明的提升和块范围的替代。这意味着一对花括号定义了一个范围，用 let 或 const 声明的变量被限制在这个范围内。

```
let g1 = 'global 1'
let g2 = 'global 2'
{   /* Creating a new block scope */
  g1 = 'new global 1'
  let g2 = 'local global 2'
  console.log(g1)   // 'new global 1'
  console.log(g2)   // 'local global 2'
  console.log(g3)   // ReferenceError: g3 is not defined
  let g3 = 'I am not hoisted';
}
console.log(g1)    // 'new global 1'
console.log(g2)    // 'global 2'
```

一个常见的误解是`const` 是不可变的。它不能被重新分配，但是它的属性可以被**改变**！

```
const tryMe = 'initial assignment';
tryMe = 'this has been reassigned';  // TypeError: Assignment to constant variable.
// You cannot reassign but you can change it…
const array = ['Ted', 'is', 'awesome!'];
array[0] = 'Barney';
array[3] = 'Suit up!';
console.log(array);     // [“Barney”, “is”, “awesome!”, “Suit up!”]
const airplane = {};
airplane.wings = 2;
airplane.passengers = 200;
console.log(airplane);   // {passengers: 200, wings: 2}
```

### 关闭

一个**闭包**是一个函数和声明它的词法环境的组合。闭包允许函数从封闭作用域**环境**中访问变量，即使它离开了声明它的作用域。

```
function sayHi(name){
  var message = `Hi ${name}!`;
  function greeting() {
    console.log(message)
  }
  return greeting
}
var sayHiToJon = sayHi('Jon');
console.log(sayHiToJon)     // ƒ() { console.log(message) }
console.log(sayHiToJon())   // 'Hi Jon!'
```

上面的例子涵盖了关于闭包你需要知道的两件事:

1.  引用外部范围中的变量。
    返回的函数从封闭范围访问`message`变量。
2.  即使在外部函数返回后，它也可以引用外部范围变量。
    `sayHiToJon`是对`greeting`函数的引用，在运行`sayHi`时创建。`greeting`函数维护一个对其外部作用域**环境***—`message`存在于其中的引用。*

*闭包的主要好处之一是它允许**数据封装**。这指的是一些数据不应该直接暴露的想法。下面的例子说明了这一点。*

*当`elementary`被创建时，外部函数已经返回。这意味着`staff`变量只存在于闭包内部，否则无法访问。*

```
*`function SpringfieldSchool() {
  let staff = ['Seymour Skinner', 'Edna Krabappel'];
  return {
    getStaff: function() { console.log(staff) },
    addStaff: function(name) { staff.push(name) }
  }
}

let elementary = SpringfieldSchool()
console.log(elementary)        // { getStaff: ƒ, addStaff: ƒ }
console.log(staff)             // ReferenceError: staff is not defined
/* Closure allows access to the staff variable */
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel"]
elementary.addStaff('Otto Mann')
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel", "Otto Mann"]`*
```

*让我们通过解决这个主题中最常见的一个面试问题来更深入地了解闭包:
下面的代码有什么问题，你将如何修复它？*

```
*`const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i+1) * 1000);
}`*
```

*考虑到上面的代码，控制台将显示四条相同的消息`"The value undefined is at index: 4"`。发生这种情况是因为循环中执行的每个函数将在整个循环完成后执行，引用存储在`i`中的最后一个值，即 4。*

*这个问题可以通过使用 IIFE 来解决，IIFE 为每次迭代创建一个唯一的范围，并将每个值存储在其范围内。*

```
*`const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(`The value ${arr[j]} is at index: ${j}`);
    }, j * 1000);
  })(i)
}`*
```

*另一个解决方案是用`let`声明`i`变量，这将产生相同的结果。*

```
*`const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i) * 1000);
}`*
```

### *立即调用的函数表达式(IIFE)*

*生命是一个函数表达式，在你定义它之后，它会被立即调用。当你想创建一个新的变量作用域时，通常会用到它。*

***(括号括起来)**防止将其视为函数声明。*

***最后一个括号()**正在执行函数表达式。*

*在生活中，当你定义函数的时候，你就在调用它。*

```
*`var result = [];
for (var i=0; i < 5; i++) {
  result.push( function() { return i } );
}
console.log( result[1]() ); // 5
console.log( result[3]() ); // 5
result = [];
for (var i=0; i < 5; i++) {
  (function () {
    var j = i; // copy current value of i
    result.push( function() { return j } );
  })();
}
console.log( result[1]() ); // 1
console.log( result[3]() ); // 3`*
```

*使用生命:*

*   *使您能够将私有数据附加到函数。*
*   *创造新鲜的环境。*
*   *避免污染全局命名空间。*

### *语境*

***上下文**经常和范围混淆。为了理清思路，让我们记住以下几点:
**上下文**通常由函数的调用方式决定。它总是引用代码中特定部分的`this`的值。
**范围**指变量的可见性。*

### *函数调用:调用、应用和绑定*

*这三种方法都是用来将`this` 附加到函数中，区别在于函数调用。*

*立即调用该函数，并要求您以列表形式(一个接一个)传递参数。*

*立即调用该函数，并允许您以数组的形式传递参数。*

*`.call()`和`.apply()`大多是等价的，都是用来借用对象的方法。选择使用哪一个取决于哪一个更容易传递参数。只需决定是传入数组还是逗号分隔的参数列表更容易。*

***快速提示:****A**apply for**A**rray—**C**all for**C**omma。*

```
*`const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);
  }
}
char.knows('something', 'Bran');              // You know something, Bran Stark
char.knows.call(Snow, 'nothing', 'Jon');      // You know nothing, Jon Snow
char.knows.apply(Snow, ['nothing', 'Jon']);   // You know nothing, Jon Snow`*
```

***注意**:如果你把一个数组作为一个调用函数的参数传入，它会把整个数组当作一个单独的元素。
ES6 允许我们用 call 函数将一个数组作为参数展开。*

```
*`char.knows.call(Snow, ...["nothing", "Jon"]);  // You know nothing, Jon Snow`*
```

*`.bind()`返回一个新函数，带有一定的上下文和参数。当您希望某个函数稍后在特定的上下文中被调用时，通常会使用它。*

*这是可能的，因为它能够维护调用原始函数的给定上下文。这对于异步回调和事件很有用。*

*`.bind()`工作原理类似于通话功能。它要求你一个接一个地传递参数，用逗号隔开。*

```
*`const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);}
  }
const whoKnowsNothing = char.knows.bind(Snow, 'nothing');
whoKnowsNothing('Jon');  // You know nothing, Jon Snow`*
```

### *“this”关键字*

*理解 JavaScript 中的关键字`this` ，以及它所指的是什么，有时会非常复杂。*

*`this` 的值通常由函数执行上下文决定。执行上下文仅仅意味着函数是如何被调用的。*

*关键字`this` 作为一个占位符，当实际使用该方法时，将引用调用该方法的任何对象。*

*以下列表是确定这一点的有序规则。在第一个适用的地方停下来:*

*   *`**new**` **绑定** *—* 使用`new`关键字调用函数时，`this` 是新构造的对象。*

```
*`function Person(name, age) {
  this.name = name;
  this.age =age;
  console.log(this);
}
const Rachel = new Person('Rachel', 30);   // { age: 30, name: 'Rachel' }`*
```

*   ***显式绑定** *—* 当使用 call 或 apply 调用函数时，`this` 是作为实参传入的对象。
    **注意** : `.bind()`的工作方式稍有不同。它创建了一个新的函数，这个函数将使用绑定到它的对象来调用原来的函数。*

```
*`function fn() {
  console.log(this);
}
var agent = {id: '007'};
fn.call(agent);    // { id: '007' }
fn.apply(agent);   // { id: '007' }
var boundFn = fn.bind(agent);
boundFn();         // { id: '007' }`*
```

*   ***隐式绑定** *—* 用上下文(包含对象)调用函数时，`this` 是函数所属的对象。
    这意味着一个函数作为一个方法被调用。*

```
*`var building = {
  floors: 5,
  printThis: function() {
    console.log(this);
  }
}
building.printThis();  // { floors: 5, printThis: function() {…} }`*
```

*   ***默认绑定** —如果上述规则都不适用，`this` 为全局对象(在浏览器中，为窗口对象)。
    当一个函数作为独立函数被调用时，会发生这种情况。
    未声明为方法的函数自动成为全局对象的属性。*

```
*`function printWindow() {
  console.log(this)
}
printWindow();  // window object`*
```

***注意**:当从外部函数作用域调用独立函数时，也会发生这种情况。*

```
*`function Dinosaur(name) {
  this.name = name;
  var self = this;
  inner();
  function inner() {
    alert(this);        // window object — the function has overwritten the 'this' context
    console.log(self);  // {name: 'Dino'} — referencing the stored value from the outer context
  }
}
var myDinosaur = new Dinosaur('Dino');`*
```

*   ***词法 this** *—* 当用箭头函数`=>`调用一个函数时，`this`接收它创建时周围作用域的`this`值。`this`保留原始上下文中的值。*

```
*`function Cat(name) {
  this.name = name;
  console.log(this);   // { name: 'Garfield' }
  ( () => console.log(this) )();   // { name: 'Garfield' }
}
var myCat = new Cat('Garfield');`*
```

### *严格模式*

*JavaScript 通过使用`“use strict”`指令在严格模式下执行。严格模式收紧了代码解析和错误处理的规则。*

*它的一些好处是:*

*   ***使调试更容易** —本来会被忽略的代码错误现在会产生错误，比如给不可写的全局或属性赋值。*
*   ***防止意外的全局变量** —给未声明的变量赋值现在会抛出错误。*
*   ***防止无效使用 delete** —尝试删除变量、函数和不可删除的属性现在将抛出错误。*
*   ***防止重复的属性名或参数值** —对象中重复的命名属性或函数中的参数现在将抛出错误。(在 ES6 中不再是这种情况)*
*   ***使 eval()更安全** —在`eval()`语句中声明的变量和函数不会在周围的作用域中创建。*
*   ***“保护”JavaScript，消除这种强制** —引用 null 或 undefined 的`this`值不会强制到全局对象。这意味着在浏览器中，不再可能在函数中使用`this`来引用窗口对象。*

### ***`新'关键字***

*关键字`new`以一种特殊的方式调用一个函数。使用`new`关键字调用的函数被称为**构造函数**。*

*那么`new`关键字实际上是做什么的呢？*

1.  *创建一个新对象。*
2.  *将**对象的**原型设为**构造函数**的原型。*
3.  *以`this`作为新创建的对象执行构造函数。*
4.  *返回创建的对象。如果构造函数返回一个对象，则返回该对象。*

```
*`// In order to better understand what happens under the hood, lets build the new keyword 
function myNew(constructor, ...arguments) {
  var obj = {}
  Object.setPrototypeOf(obj, constructor.prototype);
  return constructor.apply(obj, arguments) || obj
}`*
```

*用`new`关键字调用函数和不用关键字调用函数有什么区别？*

```
*`function Bird() {
  this.wings = 2;
}
/* invoking as a normal function */
let fakeBird = Bird();
console.log(fakeBird);    // undefined
/* invoking as a constructor function */
let realBird= new Bird();
console.log(realBird)     // { wings: 2 }`*
```

### *原型和继承*

*原型是 JavaScript 中最令人困惑的概念之一，其中一个原因是因为在两种不同的上下文中使用了单词**原型**。*

*   ***原型关系**
    每个对象都有一个**原型** 对象，从该对象继承其原型的所有属性。
    `.__proto__`是一个非标准机制(在 ES6 中可用)，用于检索对象 *(*)* 的原型。它指向对象的“父对象”— 对象的原型。
    所有普通对象也继承一个`.constructor`属性，该属性指向对象的构造函数。每当从构造函数创建一个对象时，`.__proto__`属性将该对象链接到用于创建它的构造函数的`.prototype`属性。
    *(*) `Object.getPrototypeOf()`* 是检索对象原型的标准 ES5 函数。*
*   ***原型属性**
    每个函数都有一个`.prototype`属性。
    它引用一个用于附加属性的对象，这些属性将被原型链下游的对象继承。默认情况下，该对象包含一个指向原始构造函数的`.constructor`属性。每个用构造函数创建的对象都继承了一个指向该函数的构造函数属性。*

```
*`function Dog(breed, name){
  this.breed = breed,
  this.name = name
}
Dog.prototype.describe = function() {
  console.log(`${this.name} is a ${this.breed}`)
}
const rusty = new Dog('Beagle', 'Rusty');

/* .prototype property points to an object which has constructor and attached 
properties to be inherited by objects created by this constructor. */
console.log(Dog.prototype)  // { describe: ƒ , constructor: ƒ }

/* Object created from Dog constructor function */
console.log(rusty)   //  { breed: "Beagle", name: "Rusty" }
/* Object inherited properties from constructor function's prototype */
console.log(rusty.describe())   // "Rusty is a Beagle"
/* .__proto__ property points to the .prototype property of the constructor function */ 
console.log(rusty.__proto__)    // { describe: ƒ , constructor: ƒ }
/* .constructor property points to the constructor of the object */
console.log(rusty.constructor)  // ƒ Dog(breed, name) { ... }`*
```

#### ***原型链***

*原型链是相互引用的对象之间的一系列链接。*

*当在对象中查找属性时，JavaScript 引擎将首先尝试访问对象本身的属性。*

*如果没有找到，JavaScript 引擎将在继承其属性的对象上查找该属性，该对象是**对象的原型**。*

*引擎将遍历整个链来查找该属性，并返回找到的第一个属性。*

*链中的最后一个对象是内置的`Object.prototype`，它以`null`作为它的**原型**。一旦引擎到达这个对象，它就返回`undefined`。*

#### *自有属性与继承属性*

*对象有自己的属性和继承的属性。*

*所有者属性是在对象上定义的属性。*

*继承的属性是通过原型链继承的。*

```
*`function Car() { }
Car.prototype.wheels = 4;
Car.prototype.airbags = 1;

var myCar = new Car();
myCar.color = 'black';

/*  Check for Property including Prototype Chain:  */
console.log('airbags' in myCar)  // true
console.log(myCar.wheels)        // 4
console.log(myCar.year)          // undefined

/*  Check for Own Property:  */
console.log(myCar.hasOwnProperty('airbags'))  // false — Inherited
console.log(myCar.hasOwnProperty('color'))    // true`*
```

***object . create(***obj***)**—用指定的**原型** 对象和属性创建一个新对象。*

```
*`var dog = { legs: 4 };
var myDog = Object.create(dog);

console.log(myDog.hasOwnProperty('legs'))  // false
console.log(myDog.legs)                    // 4
console.log(myDog.__proto__ === dog)       // true`*
```

#### ***参照继承***

*继承的属性是对**原型对象的** 属性的引用的副本，它从该属性继承该属性。*

*如果一个对象的属性在原型上发生了变异，继承了该属性的对象将共享相同的变异。但是如果属性被替换，更改将不会被共享。*

```
*`var objProt = { text: 'original' };
var objAttachedToProt = Object.create(objProt);
console.log(objAttachedToProt.text)   // original

objProt.text = 'prototype property changed';
console.log(objAttachedToProt.text)   // prototype property changed

objProt = { text: 'replacing property' };
console.log(objAttachedToProt.text)   // prototype property changed`*
```

#### ***经典遗传与原型遗传***

*在经典继承中，对象从类继承——像一个蓝图或要创建的对象的描述——并创建子类关系。这些对象是通过使用 new 关键字的构造函数创建的。*

*经典继承的缺点是它会导致:*

*   *僵化的等级制度*
*   *紧密耦合问题*
*   *脆弱的基类问题*
*   *重复问题*
*   *还有著名的大猩猩/香蕉问题— *“你想要的是一根香蕉，你得到的是一只拿着香蕉的大猩猩，还有整个丛林。”**

*在原型继承中，对象直接从其他对象继承。对象通常是通过`Object.create()`、对象文字或工厂函数创建的。*

*有三种不同的原型继承:*

*   ***原型委托** —委托原型是用作另一个对象的模型的对象。当从委托原型继承时，新对象将获得对原型及其属性的引用。
    这个过程通常通过使用`Object.create()`来完成。*
*   ***串联继承** —通过复制对象的原型属性将属性从一个对象继承到另一个对象的过程，不保留它们之间的引用。
    这个过程通常通过使用`Object.assign()`来完成。*
*   ***功能继承** —这个过程利用*工厂函数(*)* 创建一个对象，然后直接给创建的对象添加新的属性。
    这个过程的好处是允许通过闭包进行数据封装。
    ***(*)工厂函数*** 是不使用`new`关键字返回对象的函数，不是类或构造函数。*

```
*`const person = function(name) {
  const message = `Hello! My name is ${name}`;
  return { greeting: () => console.log(message) }
}
const will = person("Will");
will.greeting();     // Hello! My name is Will`*
```

*你可以在这里找到埃里克·艾略特 T2[关于这个话题的完整文章。](https://www.freecodecamp.org/news/the-definitive-javascript-handbook-for-a-developer-interview-44ffc6aeb54e/undefined)*

#### *偏好组合而非类继承*

*许多开发人员同意在大多数情况下应该避免类继承。在这种模式中，你根据类型是什么来设计你的类型，这使得它成为一种非常严格的模式。*

*另一方面，组合，你根据它们做什么来设计你的类型，这使得它更加灵活和可重用。*

*这里有一个关于这个话题的很好的视频，由 Mattias Petter Johansson 制作*

 *[https://www.youtube.com/embed/wfMtDGfHWpA?feature=oembed](https://www.youtube.com/embed/wfMtDGfHWpA?feature=oembed)* 

### *异步 JavaScript*

*JavaScript 是一种单线程编程语言。这意味着 JavaScript 引擎一次只能处理一段代码。它的一个主要后果是，当 JavaScript 遇到一段需要长时间处理的代码时，它会阻止之后的所有代码运行。*

*JavaScript 使用一种数据结构来存储名为**调用栈**的活动函数的信息。调用堆栈就像一堆书。放进那堆书里的每本书都放在前一本书的上面。最后进入书堆的书将是第一本从书堆中取出的书，第一本加入书堆的书将是最后一本取出的书。*

*执行大量代码而不阻塞任何东西的解决方案是**异步回调函数**。这些功能稍后被异步执行**。***

***异步进程从放入内存的**堆或**区域的异步回调函数开始。您可以将堆视为一个**事件管理器**。调用堆栈要求事件管理器仅在特定事件发生时执行特定的功能。一旦事件发生，事件管理器将函数移动到回调队列。**注意**:当事件管理器处理一个函数时，之后的代码不会被阻塞，JavaScript 继续执行。***

***事件循环处理多段代码随时间的执行。事件循环监视调用堆栈和回调队列。***

***不断检查调用堆栈是否为空。当它为空时，检查回调队列是否有函数等待调用。当有函数等待时，队列中的第一个函数被推入调用堆栈，调用堆栈将运行它。这个检查过程在事件循环中称为“滴答”。***

***让我们分解以下代码的执行来理解这个过程是如何工作的:***

```
***`const first = function () {
  console.log('First message')
}
const second = function () {
  console.log('Second message')
}
const third = function() {
  console.log('Third message')
}

first();
setTimeout(second, 0);
third();

// Output:
  // First message
  // Third message
  // Second message`***
```

1.  ***最初，浏览器控制台是透明的，调用堆栈和事件管理器是空的。***
2.  ***`first()`被添加到调用堆栈中。***
3.  ***`console.log("First message")`被添加到调用堆栈中。***
4.  ***执行`console.log("First message")`，浏览器控制台显示**【第一条消息】** *。****
5.  ***`console.log("First message")`从调用堆栈中移除。***
6.  ***`first()`从调用堆栈中移除。***
7.  ***`setTimeout(second, 0)`被添加到调用堆栈中。***
8.  ***`setTimeout(second, 0)`由事件管理器执行和处理。0 毫秒后，事件管理器将`second()`移动到回调队列。***
9.  ***`setTimeout(second, 0)`现在已完成并从调用堆栈中移除。***
10.  ***`third()`被添加到调用堆栈中。***
11.  ***`console.log("Third message")`被添加到调用堆栈中。***
12.  ***执行`console.log("Third message")`，浏览器控制台显示**【第三条消息】** *。****
13.  ***`console.log("Third message")`从调用堆栈中移除。***
14.  ***`third()`从调用堆栈中移除。***
15.  ***调用栈现在是空的，`second()`函数在回调队列中等待被调用。***
16.  ***事件循环将`second()`从回调队列移动到调用堆栈。***
17.  ***`console.log("Second message")`被添加到调用堆栈中。***
18.  ***执行`console.log("Second message")`，浏览器控制台显示**“第二条消息”**。***
19.  ***`console.log("Second message")`从调用堆栈中移除。***
20.  ***`second()`从调用堆栈中移除。***

*****注意**:0 ms 后`second()`功能不执行。传递给`setTimeout`函数的**时间**与函数执行的延迟无关。事件管理器将等待给定的时间，然后将该函数移入回调队列。它的执行只会在事件循环中的未来“滴答”时发生。***

***感谢并祝贺你读到这里！如果你对此有任何想法，欢迎发表评论。***

***你可以在 [GitHub](https://github.com/gustavoaz7) 或者 [Twitter](https://twitter.com/intent/follow?screen_name=gustavoaz7_) 上找到我。***