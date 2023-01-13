# 学习这些 JavaScript 基础知识，成为更好的开发人员

> 原文：<https://www.freecodecamp.org/news/learn-these-javascript-fundamentals-and-become-a-better-developer-2a031a0dc9cf/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

JavaScript 有原语、对象和函数。都是价值观。所有都被视为对象，甚至是原语。

### 基元

数字、布尔、字符串、`undefined`和`null`是原语。

#### 数字

JavaScript 中只有一种数字类型，即 64 位二进制浮点类型。十进制数字的算术不精确。

你可能已经知道了，`0.1 + 0.2`不等于`0.3`。但是对于整数，算术是精确的，所以`1+2 === 3`。

数字从`Number.prototype`对象继承方法。可以在数字上调用方法:

```
(123).toString();  //"123"
(1.23).toFixed(1); //"1.2"
```

将字符串转换为数字的函数有:`Number.parseInt()`、`Number.parseFloat()`、`Number()`:

```
Number.parseInt("1")       //1
Number.parseInt("text")    //NaN
Number.parseFloat("1.234") //1.234
Number("1")                //1
Number("1.234")            //1.234
```

无效的算术运算或无效的转换不会抛出异常，但会导致`NaN`“非数字”值。`Number.isNaN()`可以探测到`NaN`。

`+`操作符可以添加或连接。

```
1 + 1      //2
"1" + "1"  //"11"
1 + "1"    //"11"
```

#### 线

字符串存储一系列 Unicode 字符。文本可以在双引号`""`或单引号`''`中。

字符串从`String.prototype`继承方法。他们的方法有:`substring()`、`indexOf()`和`concat()`。

```
"text".substring(1,3) //"ex"
"text".indexOf('x')   //2
"text".concat(" end") //"text end"
```

像所有的原语一样，字符串是不可变的。例如`concat()`不修改现有的字符串，而是创建一个新的。

#### 布尔代数学体系的

一个布尔值有两个值:`true`和`false`。这种语言有真和假的价值。
`false``null``undefined``''`(空弦)`0` `NaN`都是 falsy。所有其他的价值，包括所有的对象，都是真实的。

在布尔上下文中执行时，真值被评估为`true`。Falsy 值被评估为`false`。看看下一个显示`false`分支的例子。

```
let text = '';
if(text) {
  console.log("This is true");
} else {
  console.log("This is false");
}
```

相等运算符是`===`。不等于运算符是`!==`。

### 变量

可以使用`var`、`let`和`const`定义变量。

声明并可选地初始化一个变量。用`var`声明的变量有一个函数作用域。它们被视为在函数的顶部声明的。这就是所谓的可变提升。

`let`声明有一个块范围。

未初始化变量的值是`undefined`。

不能重新分配用`const`声明的变量。然而，它的值仍然是可变的。`const`冻结变量，`Object.freeze()`冻结对象。`const`声明有一个块范围。

### 目标

对象是属性的动态集合。

属性键是唯一的字符串。当非字符串用作属性键时，它将被转换为字符串。属性值可以是基元、对象或函数。

创建对象的最简单方法是使用对象文字:

```
let obj = {
  message : "A message",
  doSomething : function() {}
}
```

有两种方法可以访问属性:点符号和括号符号。我们可以随时读取、添加、编辑和删除对象的属性。

*   获取:`object.name`，`object[expression]`
*   设置:`object.name = value,` `object[expression] = value`
*   删除:`delete object.name`，`delete object[expression]`

```
let obj = {}; //create empty object
obj.message = "A message"; //add property
obj.message = "A new message"; //edit property
delete obj.message; //delete property
```

对象可以用作地图。可以使用`Object.create(null)`创建一个简单的地图:

```
let french = Object.create(null);
french["yes"] = "oui";
french["no"]  = "non";
french["yes"];//"oui"
```

所有对象的属性都是公共的。`Object.keys()`可用于迭代所有属性。

```
function logProperty(name){
  console.log(name); //property name
  console.log(obj[name]); //property value
}
Object.keys(obj).forEach(logProperty);
```

将所有属性从一个对象复制到另一个对象。可以通过将对象的所有属性复制到空对象来克隆对象:

```
let book = { title: "The good parts" };
let clone = Object.assign({}, book);
```

不可变对象是一旦创建就不能改变的对象。如果你想让对象不可变，使用`Object.freeze()`。

#### 图元与对象

原语(除了`null`和`undefined`)被视为对象，因为它们有方法，但不是对象。

数字、字符串和布尔值都有对象等价包装器。这些是`Number`、`String`和`Boolean`功能。

为了允许访问原语上的属性，JavaScript 创建一个包装对象，然后销毁它。JavaScript 引擎优化了创建和销毁包装对象的过程。

原语是不可变的，对象是可变的。

### 排列

数组是值的索引集合。每个值都是一个元素。元素按其索引号排序和访问。

JavaScript 有类似数组的对象。数组是使用对象实现的。索引被转换为字符串，并用作检索值的名称。

一个类似于`let arr = ['A', 'B', 'C']`的简单数组使用如下所示的对象进行模拟:

```
{
  '0': 'A',
  '1': 'B',
  '2': 'C'
}
```

注意`arr[1]`给出了与`arr['1']` : `arr[1] === arr['1']`相同的值。

用`delete`从数组中删除值会留下漏洞。`splice()`可以用来避免问题，但是可以很慢。

```
let arr = ['A', 'B', 'C'];
delete arr[1];
console.log(arr); // ['A', empty, 'C']
console.log(arr.length); // 3
```

JavaScript 的数组不会抛出“索引超出范围”异常。如果索引不可用，它将返回`undefined`。

使用数组方法可以很容易地实现堆栈和队列:

```
let stack = [];
stack.push(1);           // [1]
stack.push(2);           // [1, 2]
let last = stack.pop();  // [1]
console.log(last);       // 2

let queue = [];
queue.push(1);           // [1]
queue.push(2);           // [1, 2]
let first = queue.shift();//[2]
console.log(first);      // 1
```

## 功能

功能是独立的行为单位。

函数是对象。函数可以赋给变量，存储在对象或数组中，作为参数传递给其他函数，以及从函数返回。

定义函数有三种方式:

*   函数声明(又名函数语句)
*   函数表达式(又名函数文字)
*   箭头功能

## 函数声明

*   `function`是该行的第一个关键字
*   它必须有一个名字
*   可以在定义前使用。函数声明被移动，或者“提升” ****、**、**到它们作用域的顶部。

```
function doSomething(){}
```

函数表达式

*   `function`不是该行的第一个关键字
*   该名称是可选的。可以有匿名函数表达式，也可以有命名函数表达式。
*   它需要被定义，然后才能执行
*   它可以在定义后自动执行(称为“IIFE ”)立即调用函数表达式

```
let doSomething = function() {}
```

## 箭头功能

arrow 函数是一个用于创建匿名函数表达式的 sugar 语法。

```
let doSomething = () => {};
```

箭头函数没有自己的`this`和`arguments`。

## 函数调用

用`function`关键字定义的函数可以以不同的方式调用:

*   函数形式

```
doSomething(arguments)
```

*   方法形式

```
theObject.doSomething(arguments)
theObject["doSomething"](arguments)
```

*   构造函数形式

```
new Constructor(arguments)
```

*   应用表单

```
 doSomething.apply(theObject, [arguments])
 doSomething.call(theObject, arguments)
```

调用函数时可以使用比定义中声明的更多或更少的参数。多余的参数将被忽略，缺失的参数将被设置为`undefined`。

函数(箭头函数除外)有两个伪参数:`this`和`arguments`。

## 这

方法是存储在对象中的函数。功能是独立的。为了让函数知道对哪个对象进行操作，使用了`this`。`this`表示函数的上下文。

用`doSomething()`函数形式调用一个函数时，使用`this` 没有任何意义。在这种情况下，`this`是`undefined`或者是`window`对象，这取决于是否启用了严格模式。

当使用方法表单`theObject.doSomething()`调用函数时，`this`代表对象。

当一个函数被用作构造函数`new Constructor()`，`this`代表新创建的对象。

`this`的值可以用`apply()`或`call()` : `doSomething.apply(theObject)`设定。在这种情况下，`this`是作为第一个参数发送给方法的对象。

`this`的值取决于函数是如何被调用的，而不是函数是在哪里定义的。这当然是混乱的来源。

## 争论

`arguments`伪参数给出了调用时使用的所有参数。它是一个类似数组的对象，但不是数组。它缺少数组方法。

```
function log(message){
  console.log(message);
}

function logAll(){
  let args = Array.prototype.slice.call(arguments);
  return args.forEach(log);
}

logAll("msg1", "msg2", "msg3");
```

另一种方法是新的 rest 参数语法。这次`args`是一个数组对象。

```
function logAll(...args){
  return args.forEach(log);
}
```

## 返回

没有`return`语句的函数返回`undefined`。使用`return`时注意分号的自动插入。下面的函数不会返回一个空对象，而是返回一个`undefined`对象。

```
function getObject(){ 
  return 
  {
  }
}
getObject()
```

为了避免这个问题，在与`return`相同的行上使用`{`:

```
function getObject(){ 
  return {
  }
}
```

## 动态打字

JavaScript 有动态类型。值有类型，变量没有。类型可以在运行时改变。

```
function log(value){
  console.log(value);
}

log(1);
log("text");
log({message : "text"});
```

`typeof()`操作员可以检查变量的类型。

```
let n = 1;
typeof(n);   //number

let s = "text";
typeof(s);   //string

let fn = function() {};
typeof(fn);  //function
```

## 一根线

主 JavaScript 运行时是单线程的。两个功能不能同时运行。运行时包含一个事件队列，该队列存储要处理的消息列表。没有竞争条件，没有死锁。然而，事件队列中的代码需要快速运行。否则，浏览器将变得没有反应，并会要求取消任务。

## 例外

JavaScript 有一个异常处理机制。通过使用`try/catch`语句包装代码，它的工作方式与您预期的一样。该语句有一个处理所有异常的`catch`块。

很高兴知道 JavaScript 有时偏爱无声错误。当我试图修改一个冻结的对象时，下面的代码不会抛出异常:

```
let obj = Object.freeze({});
obj.message = "text";
```

严格模式消除了一些 JavaScript 静默错误。`"use strict";`启用严格模式。

## 原型模式

`Object.create()`、构造函数和`class`在原型系统上构建对象。

考虑下一个例子:

```
let servicePrototype = {
 doSomething : function() {}
}

let service = Object.create(servicePrototype);
console.log(service.__proto__ === servicePrototype); //true
```

`Object.create()`构建一个新的对象`service`，它以`servicePrototype`对象为原型。这意味着`doSomething()`在`service`对象上是可用的。也意味着`service`的`__proto__`属性指向了`servicePrototype`对象。

现在让我们使用`class`构建一个类似的对象。

```
class Service {
  doSomething(){}
}

let service = new Service();
console.log(service.__proto__ === Service.prototype);
```

在`Service`类中定义的所有方法都将被添加到`Service.prototype`对象中。`Service`类的实例将拥有相同的原型(`Service.prototype`)对象。所有实例都将方法调用委托给`Service.prototype`对象。方法在`Service.prototype`上定义一次，然后被所有实例继承。

## 原型链

对象从其他对象继承。每个对象都有一个原型，并从中继承它们的属性。原型可以通过“隐藏”属性`__proto__`获得。

当您请求一个对象不包含的属性时，JavaScript 将向下查找原型链，直到找到请求的属性，或者到达链的末端。

## 功能模式

JavaScript 有一流的函数和闭包。这些概念为 JavaScript 中的函数式编程开辟了道路。因此，高阶函数是可能的。

`filter()`、 `map()`、`reduce()`是在函数风格下处理数组的基本工具箱。

`filter()` 根据决定应该保留哪些值的谓词函数从列表中选择值。

`map()`使用映射函数将一个值列表转换为另一个值列表。

```
let numbers = [1,2,3,4,5,6];

function isEven(number){
  return number % 2 === 0;
}

function doubleNumber(x){
  return x*2;
}

let evenNumbers = numbers.filter(isEven);
//2 4 6
let doubleNumbers = numbers.map(doubleNumber);
//2 4 6 8 10 12
```

`reduce()` 将一列值缩减为一个值。

```
function addNumber(total, value){
  return total + value;
}

function sum(...args){
  return args.reduce(addNumber, 0);
}

sum(1,2,3); //6
```

闭包是一个内部函数，它可以访问父函数的变量，甚至在父函数执行之后。[看下一个例子](https://jsfiddle.net/cristi_salcescu/wxzy52mq/?source=post_page---------------------------):

```
function createCount(){
   let state = 0;
   return function count(){
      state += 1;
      return state;
   }
}

let count = createCount();
console.log(count()); //1
console.log(count()); //2
```

`count()`是嵌套函数。`count()`从父变量`state`中访问变量。它在调用父函数`createCount()`后仍然存在。`count()`是一种封闭。

高阶函数是将另一个函数作为输入、返回一个函数或两者都做的函数。

`filter()`、 `map()`、`reduce()`为高阶函数。

纯函数是仅基于其输入返回值的函数。纯函数不使用外部函数中的变量。纯函数不会引起突变。

在前面的例子中，`isEven()`、`doubleNumber()`、`addNumber()`和`sum()`是纯函数。

## 结论

JavaScript 的强大之处在于它的简单性。

了解 JavaScript 基础知识会让我们更好地理解和使用这种语言。

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** ****[函数式 React](https://www.amazon.com/dp/B088FZQ1XN) 。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)