# 最好的 JavaScript 例子

> 原文：<https://www.freecodecamp.org/news/javascript-example/>

JavaScript 是世界上使用最广泛的脚本语言。以下是 JavaScript 中一些关键语法模式的例子。

## 参数示例

arguments 对象是一个类似于 ****数组的对象**** *(对象的结构类似于数组；然而，它不应该被认为是一个数组，因为它具有对象的所有功能)*它存储了你传递给一个函数的所有参数，并且是这个函数特有的。

如果你要传递 3 个参数给一个函数，比如说`storeNames()`，那 3 个参数将被存储在一个名为 ****参数**** 的对象中，当我们传递参数`storeNames("Mulder", "Scully", "Alex Krycek")`给我们的函数时，它看起来像这样:

*   首先，我们声明一个函数，并让它返回 arguments 对象。
*   然后，当我们执行带有 n 个参数 、3 的函数时，它会将对象返回给我们，并且 ****看起来像一个数组**** 。我们可以把它转换成一个数组，但是后面会有更多的介绍…

```
function storeNames() { return arguments; }
```

```
// If we execute the following line in the console:
storeNames("Mulder", "Scully", "Alex Kryceck");
// The output will be { '0': 'Mulder', '1': 'Scully', '2': 'Alex Kryceck' }
```

## ****把它当成一个数组****

您可以通过使用`arguments[n]`来调用参数(其中 *n* 是参数在类似数组的对象中的索引)。但是，如果您想将它作为一个数组用于迭代目的或对其应用数组方法，您需要通过声明一个变量并使用 Array.prototype.slice.call 方法将*转换为数组*(因为 *arguments* 不是一个数组):

```
var args = Array.prototype.slice.call(arguments);

// or the es6 way:
var args = Array.from(arguments)
```

自 ****片()**** 有两个(参数 ****结束**** 是可选的)参数。您可以通过指定部分的开始和结束来获取参数的某个部分(使用 *slice.call()* 方法使这两个参数可选，而不仅仅是 *end* )。查看以下代码:

```
function getGrades() {
    var args = Array.prototype.slice.call(arguments, 1, 3);
    return args;
}

// Let's output this!
console.log(getGrades(90, 100, 75, 40, 89, 95));

// OUTPUT SHOULD BE: //
// [100, 75] <- Why? Because it started from index 1 and stopped at index 3
// so, index 3 (40) wasn't taken into consideration.
//
// If we remove the '3' parameter, leaving just (arguments, 1) we'd get
// every argument from index 1: [100, 75, 40, 89, 95].
```

### ****array . slice()****的优化问题

有个小问题:不建议在 arguments 对象中使用 slice(优化原因)…

****重要的**** :你不应该对参数切片，因为它会妨碍 JavaScript 引擎的优化(例如 V8)。相反，尝试通过迭代 arguments 对象来构造一个新数组。

那么，还有什么方法可以将参数*转换成数组呢？我推荐 for 循环(不是 for-in 循环)。你可以这样做:*

```
var args = []; // Empty array, at first.
for (var i = 0; i < arguments.length; i++) {
    args.push(arguments[i])
} // Now 'args' is an array that holds your arguments.
```

[优化杀手:管理参数](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments)

### ****【ES6】rest 参数作为规避自变量对象的方式****

在 ES2015/ES6 中，可以在大多数地方使用 rest 参数(`...`)代替 arguments 对象。假设我们有以下函数(非 ES6):

```
function getIntoAnArgument() {
    var args = arguments.slice();
    args.forEach(function(arg) {
        console.log(arg);
    });
}
```

在 ES6 中，该功能可以替换为:

```
function getIntoAnArgument(...args) {
    args.forEach(arg => console.log(arg));
}
```

注意，我们还使用了一个箭头函数来缩短 forEach 回调！

arguments 对象在 arrow 函数体中不可用。

rest 参数必须始终作为函数定义中的最后一个参数。
`function getIntoAnArgument(arg1, arg2, arg3, ...restOfArgs /*no more arguments allowed here*/) { //function body }`

## 算术运算示例

JavaScript 为用户提供了五种算术运算符:`+`、`-`、`*`、`/`和`%`。运算符分别用于加、减、乘、除和余数。

## ****加法****

****语法****

`a + b`

****用法****

```
2 + 3          // returns 5
true + 2       // interprets true as 1 and returns 3
false + 5      // interprets false as 0 and returns 5
true + "bar"   // concatenates the boolean value and returns "truebar"
5 + "foo"      // concatenates the string and the number and returns "5foo"
"foo" + "bar"  // concatenates the strings and returns "foobar"
```

*提示:*有一个方便的[递增](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment_()运算符，当你把数字加 1 时，这是一个很好的捷径。

## ****减法****

****语法****

`a - b`

****用法****

```
2 - 3      // returns -1
3 - 2      // returns 1
false - 5  // interprets false as 0 and returns -5
true + 3   // interprets true as 1 and returns 4
5 + "foo"  // returns NaN (Not a Number)
```

*提示:*有一个方便的[减量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement_(--)运算符，当你将数字减 1 时，这是一个很好的捷径。

## ****乘法****

****语法****

`a * b`

****用法****

```
2 * 3                // returns 6
3 * -2               // returns -6
false * 5            // interprets false as 0 and returns 0
true * 3             // interprets true as 1 and returns 3
5 * "foo"            // returns NaN (Not a Number)
Infinity * 0         // returns NaN
Infinity * Infinity  // returns Infinity
```

*提示:*在进行计算时，可以使用括号来区分哪些数字应该相乘。

## ****师****

****语法****

`a / b`

****用法****

```
3 / 2                // returns 1.5
3.0 / 2/0            // returns 1.5
3 / 0                // returns Infinity
3.0 / 0.0            // returns Infinity
-3 / 0               // returns -Infinity
false / 5            // interprets false as 0 and returns 0
true / 2             // interprets true a 1 and returns 0.5
5 + "foo"            // returns NaN (Not a Number)
Infinity / Infinity  // returns NaN
```

## ****余数****

****语法****

`a % b`

****用法****

```
3 % 2          // returns 1
true % 5       // interprets true as 1 and returns 1
false % 4      // interprets false as 0 and returns 0
3 % "bar"      // returns NaN
```

## ****增量****

****语法****

`a++ or ++a`

****用法****

```
// Postfix 
x = 3; // declare a variable 
y = x++; // y = 4, x = 3 

// Prefix 
var a = 2; 
b = ++a; // a = 3, b = 3
```

## ****减量****

****语法****

`a-- or --a`

****用法****

```
// Postfix 
x = 3; // declare a variable 
y = x—; // y = 3, x = 3 

// Prefix 
var a = 2; 
b = —a; // a = 1, b = 1
```

*重要:*如你所见，你 ****不能对`Infinity`执行任何种类的操作。****

### 箭头函数示例

箭头函数是一种新的 ES6 语法，用于编写 JavaScript 函数表达式。较短的语法节省了时间，也简化了函数范围。

## ****什么是箭头功能？****

箭头函数表达式是使用“粗箭头”标记(`=>`)编写函数表达式的一种更简洁的语法。

### ****基本语法****

下面是一个箭头函数的基本示例:

```
// ES5 syntax
var multiply = function(x, y) {
  return x * y;
};

// ES6 arrow function
var multiply = (x, y) => { return x * y; };

// Or even simpler
var multiply = (x, y) => x * y; 
```

您不再需要`function`和`return`关键字，甚至不再需要花括号。

### ****一种简化的`this`****

在箭头函数之前，新函数定义了自己的`this`值。要在传统的函数表达式中使用`this`,我们必须像这样写一个变通方法:

```
// ES5 syntax
function Person() {
  // we assign `this` to `self` so we can use it later
  var self = this;
  self.age = 0;

  setInterval(function growUp() {
    // `self` refers to the expected object
    self.age++;
  }, 1000);
}
```

箭头函数不定义自己的`this`值，它从封闭函数继承`this`:

```
// ES6 syntax
function Person(){
  this.age = 0;

  setInterval(() => {
    // `this` now refers to the Person object, brilliant!
    this.age++;
  }, 1000);
}

var p = new Person();
```

## 赋值运算符

## 赋值运算符示例

赋值运算符，顾名思义，就是给一个变量赋值(或重新赋值)。虽然赋值操作符有很多变化，但它们都是建立在基本赋值操作符的基础上的。

语法=**y；descriptionnecessityxavariablerequired =要分配给变量的赋值运算符 required yvalue required**

## ****例句****

```
let initialVar = 5;   // Variable initialization requires the use of an assignment operator

let newVar = 5;
newVar = 6;   // Variable values can be modified using an assignment operator
```

## ****变异****

其他赋值操作符是使用变量(用上面的 x 表示)和值(用上面的 y 表示)执行一些操作的简写，然后将结果赋给变量本身。

例如，下面是加法赋值运算符的语法:

```
x += y;
```

这与应用加法运算符并将总和重新分配给原始变量(即 x)是一样的，可以用以下代码表示:

```
x = x + y;
```

为了使用实际值来说明这一点，下面是使用加法赋值运算符的另一个示例:

```
let myVar = 5;   // value of myVar: 5
myVar += 7;   // value of myVar: 12 = 5 + 7
```

### JavaScript 赋值运算符的完整列表

| 操作员 | 句法 | 长版本 |
| --- | --- | --- |
| 分配 | x = y | x = y |
| 加法赋值 | x += y | x = x + y |
| 减法赋值 | x -= y | x = x - y |
| 乘法赋值 | x *= y | x = x * y |
| 分部分配 | x /= y | x = x / y |
| 余数分配 | x %= y | x = x % y |
| 指数赋值 | x **= y | x = x ** y |
| 左移赋值 | x <<= y | x = x << y |
| 右移位赋值 | x >>= y | x = x >> y |
| 无符号右移位赋值 | x >>>= y | x = x >>> y |
| 按位 AND 赋值 | x &= y | x = x & y |
| 按位异或赋值 | x ^= y | x = x ^ y |
| 按位 OR 赋值 | x &#124;= y | x = x &#124; y |

## **布尔示例**

布尔是计算机编程语言中常用的一种原始数据类型。根据定义，布尔值有两个可能的值:`true`或`false`。

在 JavaScript 中，经常有隐式类型强制转换为 boolean。例如，如果您有一个 If 语句来检查某个表达式，则该表达式将被强制转换为布尔值:

```
var a = 'a string';
if (a) {
  console.log(a); // logs 'a string'
}
```

只有几个值将被强制为假:

*   false(不是真正的强制，因为它已经是 false)
*   空
*   不明确的
*   圆盘烤饼
*   Zero
*   ”(空字符串)

所有其他值将被强制为真。当一个值被强制为布尔值时，我们也称之为“假”或“真”。

使用类型强制的一种方式是使用 or ( `||`)和 and ( `&&`)运算符:

```
var a = 'word';
var b = false;
var c = true;
var d = 0
var e = 1
var f = 2
var g = null

console.log(a || b); // 'word'
console.log(c || a); // true
console.log(b || a); // 'word'
console.log(e || f); // 1
console.log(f || e); // 2
console.log(d || g); // null
console.log(g || d); // 0
console.log(a && c); // true
console.log(c && a); // 'word'
```

如您所见，*或*操作符检查第一个操作数。如果这是真的或真的，它会立即返回(这就是为什么我们在第一种情况下得到“word”而在第二种情况下得到“T2”true)。如果它不是 true 或 truthy，它返回第二个操作数(这就是为什么我们在第三种情况下得到' word ')。

对于 and 运算符，它以类似的方式工作，但是要使“and”为真，两个操作数都必须为真。因此，如果两个操作数都为 true/truthy，它将始终返回第二个操作数，否则将返回 false。这就是为什么在第四种情况下我们得到真实，而在最后一种情况下我们得到单词。

## **布尔对象**

还有一个原生 JavaScript `Boolean`对象，它包装一个值并将第一个参数转换为布尔值。如果值被省略或为 false–0、-0、`null`、`false`、`NaN`、`undefined`或空字符串(`""`)，则对象的值为 false。传递所有其他值，包括字符串`"false"`，对象的值被设置为 true。

注意原始布尔值(`true`和`false`)不同于布尔对象的值。

### 更多细节

请记住，如果在条件语句中使用，任何值不是`undefined`或`null`的对象都将计算为 true。例如，即使这个`Boolean`对象被显式设置为 false，它的计算结果也为 true，并且执行代码:

```
var greeting = new Boolean(false);
if (greeting) {
  console.log("Hello world");
}

// Hello world
```

这不适用于布尔原语:

```
var greeting = false;
if (greeting) {
  console.log("Hello world"); // code will not run
}
```

要将非布尔值转换为布尔值，请将`Boolean`用作函数而非对象:

```
var x = Boolean(expression);     // preferred use as a function
var x = new Boolean(expression); // don't do it this way
```

## 回调函数

本节简要介绍 JavaScript 中回调函数的概念和用法。

### 函数是对象

我们需要知道的第一件事是，在 JavaScript 中，函数是一级对象。因此，我们可以像处理其他对象一样处理它们，比如将它们赋给变量并将它们作为参数传递给其他函数。这很重要，因为后一种技术允许我们在应用程序中扩展功能。

## ****回调函数**示例**

一个 ****回调函数**** 是一个作为参数传递给另一个函数*的函数，在稍后的时间被“回调”。*

接受其他函数作为参数的函数称为 ****高阶函数**** ，它包含当回调函数被执行时*的逻辑。这两者的结合使我们能够扩展我们的功能。*

为了说明回调，让我们从一个简单的例子开始:

```
function createQuote(quote, callback){ 
  var myQuote = "Like I always say, " + quote;
  callback(myQuote); // 2
}

function logQuote(quote){
  console.log(quote);
}

createQuote("eat your vegetables!", logQuote); // 1

// Result in console: 
// Like I always say, eat your vegetables!
```

在上面的例子中，`createQuote`是高阶函数，它接受两个参数，第二个参数是回调。`logQuote`函数被用来作为我们的回调函数传入。当我们执行`createQuote`函数 *(1)* 时，请注意，当我们将其作为参数传入时，*没有将*括号附加到`logQuote`上。这是因为我们不想马上执行回调函数，我们只想将函数定义传递给更高阶的函数，以便稍后执行。

此外，我们需要确保如果我们传入的回调函数需要参数，我们会在执行回调 *(2)* 时提供这些参数。在上面的例子中，那将是`callback(myQuote);`语句，因为我们知道`logQuote`期望传入一个报价。

此外，我们可以将匿名函数作为回调函数传入。下面对`createQuote`的调用会产生与上面例子相同的结果:

```
createQuote("eat your vegetables!", function(quote){ 
  console.log(quote); 
});
```

顺便说一下，你没有*有*来使用“回调”这个词作为你的论点的名称。JavaScript 只需要知道它是正确的参数名。基于上面的例子，下面的函数将以完全相同的方式运行。

```
function createQuote(quote, functionToCall) { 
  var myQuote = "Like I always say, " + quote;
  functionToCall(myQuote);
}
```

## ****为什么要用回调？****

大多数时候，我们都在创建以 ****同步**** 方式运行的程序和应用程序。换句话说，我们的一些操作只有在前面的操作完成之后才开始。

通常，当我们从其他来源(如外部 API)请求数据时，我们并不总是知道*何时*我们的数据会被返回。在这些情况下，我们希望等待响应，但我们不希望在获取数据时整个应用程序陷入停顿。在这些情况下，回调函数就派上了用场。

让我们来看一个模拟向服务器发出请求的示例:

```
function serverRequest(query, callback){
  setTimeout(function(){
    var response = query + "full!";
    callback(response);
  },5000);
}

function getResults(results){
  console.log("Response from the server: " + results);
}

serverRequest("The glass is half ", getResults);

// Result in console after 5 second delay:
// Response from the server: The glass is half full!
```

在上面的例子中，我们向服务器发出一个模拟请求。5 秒钟后，响应被修改，然后我们的回调函数`getResults`被执行。要看到这一点，您可以将上述代码复制/粘贴到浏览器的开发工具中并执行它。

同样，如果你已经熟悉了`setTimeout`，那么你一直都在使用回调函数。传入上面例子的`setTimeout`函数调用的匿名函数参数也是回调！因此，该示例的原始回调实际上是由另一个回调执行的。如果可以的话，注意不要嵌套太多的回调，因为这可能会导致所谓的“回调地狱”！顾名思义，这不是一件愉快的事情。

## **JavaScript 类示例**

JavaScript 本身没有类的概念。

但是我们可以通过利用 JavaScript 的原型性质来模拟一个类的功能。

本节假设您对[原型](https://guide.freecodecamp.org/javascript/prototypes)有基本的了解。

为了清楚起见，让我们假设我们想要创建一个可以执行以下操作的类

```
var p = new Person('James','Bond'); // create a new instance of Person class
	p.log() // Output: 'I am James Bond' // Accessing a function in the class
	// Using setters and getters 
	p.profession = 'spy'
	p.profession // output: James bond is a spy
```

### **使用类关键字**

像在任何其他编程语言中一样，您现在可以使用`class`关键字来创建一个类。

旧版浏览器不支持这一功能，ECMAScript 2015 引入了这一功能。

`class`只是 JavaScript 现有的基于原型的继承模型上的一个语法糖。

一般来说，程序员使用以下方法在 JavaScript 中创建一个类。

### **使用添加到原型的方法:**

在这里，所有的方法都被添加到原型中

```
function Person(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;
}

Person.prototype.log = function() {
    console.log('I am', this._firstName, this._lastName);
}

// This line adds getters and setters for the profession object. Note that in general you could just write your own get and set functions like the 'log' method above.
// Since in this example we are trying the mimic the class above, we try to use the getters and setters property provided by JavaScript
Object.defineProperty(Person.prototype, 'profession', {
    set: function(val) {
        this._profession = val;
    },
    get: function() {
        console.log(this._firstName, this._lastName, 'is a', this._profession);
    }
})
```

您也可以在函数`Person`上编写原型方法，如下所示:

```
Person.prototype = {
    log: function() {
        console.log('I am ', this._firstName, this._lastName);
    }
    set profession(val) {
        this._profession = val;
    }

    get profession() {
        console.log(this._firstName, this._lastName, 'is a', this._profession);
    }

}
```

### **使用内部添加的方法**

这里的方法是在内部添加的，而不是原型:

```
function Person(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;

    this.log = function() {
        console.log('I am ', this._firstName, this._lastName);
    }

    Object.defineProperty(this, 'profession', {
        set: function(val) {
            this._profession = val;
        },
        get: function() {
            console.log(this._firstName, this._lastName, 'is a', this._profession);
        }
    })
}
```

### **用符号隐藏类中的细节**

大多数情况下，一些属性和方法必须隐藏起来，以防止从函数外部访问。

对于类，要获得这种功能，一种方法是使用符号。Symbol 是 JavaScript 的一种新的内置类型，可以调用它来给出一个新的符号值。每个符号都是独一无二的，可以用作对象上的一个键。

因此，符号的一个用例是，你可以向一个你可能不拥有的对象添加一些东西，并且你可能不想与对象的任何其他键发生冲突。因此，创建一个新的并使用 symbol 将其作为属性添加到该对象是最安全的。同样，当符号值被添加到一个对象中时，没有其他人会知道如何获得它。

```
class Person {
    constructor(firstName, lastName) {
        this._firstName = firstName;
        this._lastName = lastName;
    }

    log() {
        console.log('I am', this._firstName, this._lastName);
    }

    // setters
    set profession(val) {
        this._profession = val;
    }
    // getters
    get profession() {
        console.log(this._firstName, this._lastName, 'is a', this._profession);
    }
// With the above code, even though we can access the properties outside the function to change their content what if we don't want that.
// Symbols come to rescue.
let s_firstname  = new Symbol();

class Person {
    constructor(firstName, lastName) {
        this[s_firstName] = firstName;
        this._lastName = lastName;
    }

    log() {
        console.log('I am', this._firstName, this._lastName);
    }

    // setters
    set profession(val) {
        this._profession = val;
    }
    // getters
    get profession() {
        console.log(this[s_firstName], this._lastName, 'is a', this._profession);
    }
```

### JavaScript 闭包示例

闭包是函数和声明该函数的词法环境(作用域)的组合。闭包是 Javascript 的一个基本而强大的属性。本节讨论关于闭包的“如何”和“为什么”:

### **例子**

```
//we have an outer function named walk and an inner function named fly

function walk (){

  var dist = '1780 feet';

  function fly(){
    console.log('At '+dist);
  }

  return fly;
}

var flyFunc = walk(); //calling walk returns the fly function which is being assigned to flyFunc
//you would expect that once the walk function above is run
//you would think that JavaScript has gotten rid of the 'dist' var

flyFunc(); //Logs out 'At 1780 feet'
//but you still can use the function as above 
//this is the power of closures
```

### **另一个例子**

```
function by(propName) {
    return function(a, b) {
        return a[propName] - b[propName];
    }
}

const person1 = {name: 'joe', height: 72};
const person2 = {name: 'rob', height: 70};
const person3 = {name: 'nicholas', height: 66};

const arr_ = [person1, person2, person3];

const arr_sorted = arr_.sort(by('height')); // [ { name: 'nicholas', height: 66 }, { name: 'rob', height: 70 },{ name: 'joe', height: 72 } ]
```

闭包会“记住”它被创建的环境。这个环境由创建闭包时在作用域内的所有局部变量组成。

```
function outside(num) {
  var rememberedVar = num; // In this example, rememberedVar is the lexical environment that the closure 'remembers'
  return function inside() { // This is the function which the closure 'remembers'
    console.log(rememberedVar)
  }
}

var remember1 = outside(7); // remember1 is now a closure which contains rememberedVar = 7 in its lexical environment, and //the function 'inside'
var remember2 = outside(9); // remember2 is now a closure which contains rememberedVar = 9 in its lexical environment, and //the function 'inside'

remember1(); // This now executes the function 'inside' which console.logs(rememberedVar) => 7
remember2(); // This now executes the function 'inside' which console.logs(rememberedVar) => 9 
```

闭包是有用的，因为它让你“记住”数据，然后让你通过返回的函数对数据进行操作。这允许 Javascript 模拟其他编程语言中的私有方法。私有方法对于限制对代码的访问以及管理全局命名空间非常有用。

### **私有变量和方法**

闭包也可以用来封装私有数据/方法。看一下这个例子:

```
const bankAccount = (initialBalance) => {
  const balance = initialBalance;

  return {
    getBalance: function() {
      return balance;
    },
    deposit: function(amount) {
      balance += amount;
      return balance;
    },
  };
};

const account = bankAccount(100);

account.getBalance(); // 100
account.deposit(10); // 110
```

在这个例子中，我们不能从`bankAccount`函数之外的任何地方访问`balance`，这意味着我们刚刚创建了一个私有变量。

封闭在哪里？嗯，想想`bankAccount()`返回的是什么。它实际上返回一个内部有一堆函数的对象，然而当我们调用`account.getBalance()`时，函数能够“记住”它对`balance`的初始引用。

这就是闭包的强大之处，函数“记住”它的词法范围(编译时范围)，即使函数是在词法范围之外执行的。

### 模拟块范围的变量

Javascript 没有块范围变量的概念。这意味着当在 for 循环中定义一个变量时，这个变量在 for 循环之外也是可见的。那么闭包如何帮助我们解决这个问题呢？让我们来看看。

```
 var funcs = [];

    for(var i = 0; i < 3; i++){
        funcs[i] = function(){
            console.log('My value is ' + i);  //creating three different functions with different param values.
        }
    }

    for(var j = 0; j < 3; j++){
        funcs[j]();             // My value is 3
                                // My value is 3
                                // My value is 3
    }
```

因为变量 I 没有块范围，所以它在所有三个函数中的值都用循环计数器更新了，并产生了恶意值。闭包可以帮助我们解决这个问题，它创建了函数创建时所处环境的快照，保存了它的状态。

```
 var funcs = [];

    var createFunction = function(val){
	    return function() {console.log("My value: " + val);};
    }

    for (var i = 0; i < 3; i++) {
        funcs[i] = createFunction(i);
    }
    for (var j = 0; j < 3; j++) {
        funcs[j]();                 // My value is 0
                                    // My value is 1
                                    // My value is 2
    }
```

Javascript 的更高版本(ES6+)有一个名为 let 的新关键字，可以用来给变量一个 blockscope。还有许多函数(forEach)和整个库(lodash.js)致力于解决上述问题。它们当然可以提高你的生产力，但是当你试图创造一些大的东西时，了解所有这些问题仍然是非常重要的。

闭包有许多特殊的应用，在创建大型 Javascript 程序时非常有用。

1.  模拟私有变量或封装
2.  进行异步服务器端调用
3.  创建块范围的变量。

### 模拟私有变量

与许多其他语言不同，Javascript 没有允许您在对象中创建封装的实例变量的机制。在构建大中型程序时，拥有公共实例变量会导致很多问题。然而使用闭包，这个问题可以得到缓解。

与前面的例子非常相似，您可以构建返回对象文字的函数，这些函数的方法可以访问对象的局部变量，而不会暴露它们。从而使它们实际上是私有的。

闭包还可以帮助您管理全局名称空间，以避免与全局共享数据的冲突。通常，所有的全局变量都是在你的项目中的所有脚本之间共享的，这在构建中大型程序时肯定会给你带来很多麻烦。

这就是库和模块作者使用闭包来隐藏整个模块的方法和数据的原因。这被称为模块模式，它使用一个直接调用的函数表达式，该表达式只向外界导出某些功能，从而大大减少了全局引用的数量。

这里有一个模块框架的简短示例。

```
var myModule = (function() = {
    let privateVariable = 'I am a private variable';

    let method1 = function(){ console.log('I am method 1'); };
    let method2 = function(){ console.log('I am method 2, ', privateVariable); };

    return {
        method1: method1,
        method2: method2
    }
}());

myModule.method1(); // I am method 1
myModule.method2(); // I am method 2, I am a private variable
```

闭包对于捕获包含在“记忆”环境中的私有变量的新实例很有用，这些变量只能通过返回的函数或方法来访问。

### JavaScript 注释示例

程序员使用注释给他们的源代码添加提示、注释、建议或警告；它们对代码的实际输出没有影响。注释对于解释代码正在或应该做什么非常有帮助。

开始注释的时候，经常注释总是最好的做法，因为它可以帮助那些阅读你的代码的人理解你的代码到底想要做什么。

JavaScript 有两种在代码中分配注释的方法。

第一种方式是`//`注释；同一行中所有跟在`//`后面的文本都变成一个注释。例如:

```
function hello() {
  // This is a one line JavaScript comment
  console.log("Hello world!");
}
hello();
```

第二种方式是`/* */`注释，单行和多行注释都可以。例如:

```
function hello() {
  /* This is a one line JavaScript comment */
  console.log("Hello world!");
}
hello();
```

```
function hello() {
  /* This comment spans multiple lines. Notice
     that we don't need to end the comment until we're done. */
  console.log("Hello world!");
}
hello();
```

您也可以阻止 Javascript 代码的执行，只需像这样命令代码行:

```
function hello() {
  /*console.log("Hello world!");*/
}
hello();
```

#### **更多信息:**

[如何用 JavaScript 写注释](https://www.digitalocean.com/community/tutorials/how-to-write-comments-in-javascript)

### 许多 ide 都带有键盘快捷键来注释掉行。

1.  突出显示要注释的文本
2.  Mac:推送命令(苹果键)& "/"
3.  Windows:推送控件& "/"
4.  您也可以通过执行相同的步骤来取消对代码的注释

在许多代码编辑器中，注释掉 Javascript 的一部分的快捷方式是突出显示要注释掉的代码行，然后按“Cmd/Ctrl + /”。

注释对于代码测试也很有帮助，因为您可以阻止某个代码行/代码块运行:

```
function hello() {
  // The statement below is not going to get executed
  // console.log('hi')
  }
hello();
```

```
function hello() {
  // The statements below are not going to get executed
  /*
  console.log('hi');
  console.log('code-test');
  */
}
hello();
```

## JavaScript 比较运算符示例

JavaScript 既有 ****严格**** 又有 ****类型——转换**** 比较。

*   严格比较(`===`)仅在两个操作数属于同一类型时计算为 true。
*   抽象比较(`==`)试图在比较之前将两个操作数转换为相同的类型。
*   使用关系抽象比较(`<=`)，两个操作数都被转换成原语，然后在比较前转换成相同的类型。
*   基于标准排序，使用 Unicode 值对字符串进行比较。

## **比较的特征:**

*   当两个字符串具有相同序列和相同长度的字符时，它们被认为是严格相等的。
*   当两个数都是 number 类型并且数值相等时，它们被认为是严格相等的。这意味着`0`和`-0`严格相等，因为它们的值都是`0`。注意`NaN`是一个特殊值，不等于任何东西，包括`NaN`。
*   如果两个布尔操作数都是`true`或`false`，则认为它们严格相等。
*   在严格或抽象比较中，两个对象永远不会被认为是相等的。
*   只有当两个操作数都引用同一个对象实例时，比较对象的表达式才被认为是真的。
*   Null 和 undefined 都被认为严格地等于它们自身(`null === null`)并且抽象地彼此相等(`null == undefined`)

## **相等运算符**

### **相等(==)**

相等运算符首先转换不同类型的操作数，然后对它们进行严格的比较。

#### **语法**

```
 x == y
```

#### **例题**

```
 1   ==  1        // true
"1"  ==  1        // true
 1   == '1'       // true
 0   == false     // true
 0   == null      // false

 0   == undefined   // false
 null  == undefined // true
```

### **不平等(！=)**

如果两个操作数不相等，不等式运算符的计算结果为 true。如果操作数不属于同一类型，它会在进行比较之前尝试将它们转换为同一类型。

#### **语法**

```
x != y
```

#### **例题**

```
1 !=   2     // true
1 !=  "1"    // false
1 !=  '1'    // false
1 !=  true   // false
0 !=  false  // false
```

### **同一性/严格相等(===)**

如果两个操作数在值和类型方面严格相等，则 identity 或严格相等运算符返回 true。与相等运算符(`==`)不同，它不会尝试将操作数转换为相同的类型。

#### **语法**

```
x === y
```

#### **例题**

```
3 === 3   // true
3 === '3' // false
```

### **非同一性/严格不等式(！==)**

如果两个操作数在值或类型上不完全相等，则非同一性或严格不等式运算符返回 true。

#### **语法**

```
x !== y
```

#### **例题**

```
3 !== '3' // true
4 !== 3   // true
```

## **关系运算符**

### **大于运算符(> )**

如果左边的操作数大于右边的操作数，则大于运算符返回 true。

#### **语法**

```
x > y
```

#### **例题**

```
4 > 3 // true
```

### **大于或等于运算符(> =)**

如果左边的操作数大于或等于右边的操作数，则大于或等于运算符返回 true。

#### **语法**

```
x >= y
```

#### **例题**

```
4 >= 3 // true
3 >= 3 // true
```

### **小于运算符(< )**

如果左边的操作数小于右边的操作数，则小于运算符返回 true。

#### **语法**

```
x < y
```

#### **例题**

```
3 < 4 // true
```

### **小于或等于运算符(< =)**

如果左边的操作数小于或等于右边的操作数，则小于或等于运算符返回 true。

#### **语法**

```
x <= y
```

#### **例题**

```
3 <= 4 // true
```

## **JavaScript 表单验证示例**

表单验证通常发生在服务器端，在客户端输入所有必要的数据，然后点击提交按钮之后。如果客户机输入的数据不正确或只是丢失，服务器就必须将所有数据发送回客户机，并请求用正确的信息重新提交表单。这确实是一个漫长的过程，过去会给服务器带来很大的负担。

JavaScript 提供了一种在将表单数据发送到 web 服务器之前，在客户端计算机上验证表单数据的方法。表单验证通常执行两个功能:

### **基本验证**

首先，必须检查表单以确保所有必填字段都已填写。它只需要遍历表单中的每个字段来检查数据。

### **数据格式验证**

其次，必须检查输入的数据的格式和值是否正确。您的代码必须包含适当的逻辑来测试数据的正确性。

#### **举例:**

```
<html>

   <head>
      <title>Form Validation</title>

      <script type="text/javascript">
         <!--
            // Form validation code will come here.
         //-->
      </script>

   </head>

   <body>
      <form action="/cgi-bin/test.cgi" name="myForm" onsubmit="return(validate());">
         <table cellspacing="2" cellpadding="2" border="1">

            <tr>
               <td align="right">Name</td>
               <td><input type="text" name="Name" /></td>
            </tr>

            <tr>
               <td align="right">EMail</td>
               <td><input type="text" name="EMail" /></td>
            </tr>

            <tr>
               <td align="right">Zip Code</td>
               <td><input type="text" name="Zip" /></td>
            </tr>

            <tr>
               <td align="right">Country</td>
               <td>
                  <select name="Country">
                     <option value="-1" selected>[choose yours]</option>
                     <option value="1">USA</option>
                     <option value="2">UK</option>
                     <option value="3">INDIA</option>
                  </select>
               </td>
            </tr>

            <tr>
               <td align="right"></td>
               <td><input type="submit" value="Submit" /></td>
            </tr>

         </table>
      </form>

   </body>
</html>
```

#### **输出**

看一看[这里](https://liveweave.com/LP9eOP)。

### **基本表单验证**

首先让我们看看如何进行基本的表单验证。在上面的表单中，当 onsubmit 事件发生时，我们调用 validate()来验证数据。下面的代码展示了这个`validate()`函数的实现。

```
<script type="text/javascript">
   // Form validation code will come here.
   function validate()
      {

         if( document.myForm.Name.value == "" )
         {
            alert( "Please provide your name!" );
            document.myForm.Name.focus() ;
            return false;
         }

         if( document.myForm.EMail.value == "" )
         {
            alert( "Please provide your Email!" );
            document.myForm.EMail.focus() ;
            return false;
         }

         if( document.myForm.Zip.value == "" ||
         isNaN( document.myForm.Zip.value ) ||
         document.myForm.Zip.value.length != 5 )
         {
            alert( "Please provide a zip in the format #####." );
            document.myForm.Zip.focus() ;
            return false;
         }

         if( document.myForm.Country.value == "-1" )
         {
            alert( "Please provide your country!" );
            return false;
         }
         return( true );
      }
</script>
```

#### **输出**

看一看[这里](https://liveweave.com/pCPTnP)。

### **数据格式验证**

现在，我们将了解如何在提交表单数据到 web 服务器之前验证输入的表单数据。

以下示例显示了如何验证输入的电子邮件地址。电子邮件地址必须至少包含一个“@”符号和一个点(。).此外，“@”不能是电子邮件地址的第一个字符，最后一个点必须至少是“@”符号后面的一个字符。

#### **举例:**

```
<script type="text/javascript">
    function validateEmail()
      {
         var emailID = document.myForm.EMail.value;
         atpos = emailID.indexOf("@");
         dotpos = emailID.lastIndexOf(".");

         if (atpos < 1 || ( dotpos - atpos < 2 )) 
         {
            alert("Please enter correct email ID")
            document.myForm.EMail.focus() ;
            return false;
         }
         return( true );
      }
</script>
```

#### **输出**

看一看[这里](https://liveweave.com/nznVs6)。

### **HTML5 表单约束**

一些常用于`<input>`的 HTML5 约束是`type`属性(例如`type="password"`)、`maxlength`、`required`和`disabled`。一个不太常用的约束是采用 JavaScript 正则表达式的`pattern`属性。

## JavaScript If 语句示例

如果指定的条件为`true`，则`if`语句执行一条语句。如果条件是`false`，可以使用`else`语句执行另一条语句。

****注:****`else`语句可选。

```
if (condition)
    /* do something */
else
    /* do something else */
```

多个`if...else`语句可以链接起来创建一个`else if`子句。这指定了一个要测试的新条件，并且可以重复测试多个条件，一直检查到出现要执行的真语句。

```
if (condition1)
    /* do something */
else if (condition2)
    /* do something else */
else if (condition3)
    /* do something else */
else
    /* final statement */
```

****注意:**** 如果要执行`if`、`else`或`else if`部分中的多条语句，需要用花括号将语句括起来:

```
if (condition) {
    /* do */
    /* something */
    /* with multiple statements */
} else {
    /* do something */
    /* else */
}
```

[MDN 链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) | [MSDN 链接](https://msdn.microsoft.com/en-us/library/85yyde5c.aspx)

## **例题**

****利用**** `if...else`:

```
 // If x=5 z=7 and q=42\. If x is not 5 then z=19.
    if (x == 5) {
      z = 7;
      q = 42
    else
      z = 19;
```

****利用**** `else if`:

```
if (x < 10)
    return "Small number";
else if (x < 50)
    return "Medium number";
else if (x < 100)
    return "Large number";
else {
    flag = 1;
    return "Invalid number";
}
```

## JavaScript 原型示例

JavaScript 是基于原型的语言，因此理解原型对象是 JavaScript 从业者需要了解的最重要的概念之一。

本节将通过各种示例向您简要介绍原型对象。在阅读这一部分之前，你需要对 JavaScript 中的 [`this`引用有一个基本的了解。](https://www.freecodecamp.org/news/the-complete-guide-to-this-in-javascript/)

### **原型物体**

为了清楚起见，让我们来看看下面的例子:

```
function Point2D(x, y) {
  this.x = x;
  this.y = y;
}
```

当声明了`Point2D`函数时，将为它创建一个名为`prototype`的默认属性(注意，在 JavaScript 中，函数也是一个对象)。

`prototype`属性是包含`constructor`属性的对象，其值是`Point2D`函数:`Point2D.prototype.constructor = Point2D`。当你用`new`关键字调用`Point2D`时，*新创建的对象将继承*和`Point2D.prototype`的所有属性。

为了检查这一点，您可以将名为`move`的方法添加到`Point2D.prototype`中，如下所示:

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

`Point2D.prototype`被称为 ****原型对象**** 或`p1`对象的 ****原型**** 以及用`new Point2D(...)`语法创建的任何其他对象。您可以随意添加更多属性到`Point2D.prototype`对象。常见的模式是向`Point2D.prototype`声明方法，其他属性将在构造函数中声明。

JavaScript 中的内置对象也是以类似的方式构造的。例如:

*   用`new Object()`或`{}`语法创建的对象的原型是`Object.prototype`。
*   用`new Array()`或`[]`语法创建的数组的原型是`Array.prototype`。
*   与其他内置对象如`Date`、`RegExp`以此类推。

`Object.prototype`被所有对象继承，它没有原型(它的原型是`null`)。

### **原型链**

原型链机制很简单:当您访问对象`obj`上的属性`p`时，JavaScript 引擎将在`obj`对象中搜索该属性。如果搜索失败，则继续在`obj`对象的原型中搜索，以此类推，直到到达`Object.prototype`。如果搜索结束后，没有发现任何东西，结果将是`undefined`。例如:

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

在上面的代码片段中，语句`var obj2 = Object.create(obj1)`将用原型`obj1`对象创建`obj2`对象。换句话说，`obj1`默认成为`obj2`的原型，而不是`Object.prototype`。如你所见，`b`不是`obj2`的属性；您仍然可以通过原型链访问它。然而，对于`c`属性，您会得到一个`undefined`值，因为在`obj1`和`Object.prototype`中找不到它。

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

## JavaScript 范围示例

如果你用 JavaScript 编程已经有一段时间了，你肯定会遇到一个叫做`scope`的概念。什么是`scope`？你为什么要花时间去学习它呢？

用程序员的话说，`scope`就是 ****当前执行**** 的上下文。迷茫？让我们来看看下面这段代码:

```
var foo = 'Hi, I am foo!';

var baz = function () {
  var bar = 'Hi, I am bar too!';
    console.log(foo);
}

baz(); // Hi, I am foo!
console.log(bar); // ReferenceError...
```

这是一个简单的例子，但是它很好地说明了所谓的*词法范围*。JavaScript 和几乎所有其他编程语言都有一个*词法范围*。还有另一种作用域称为*动态作用域*，但我们不会讨论它。

现在，术语*词法范围*听起来很奇怪，但是正如你将看到的，它在原理上非常简单。在词法作用域中，有两种作用域:全局作用域*和局部作用域*。**

**在您键入程序中的第一行代码之前，会为您创建一个*全局范围*。这包含了你在程序 ****中声明的所有变量，在**** 之外。**

**在上面的例子中，变量`foo`在程序的全局范围内，而变量`bar`在函数内部声明，因此在函数 的局部范围内为 ****。******

**让我们一行一行地分解这个例子。虽然在这一点上你可能会感到困惑，但我保证当你读完这篇文章时，你会有更好的理解。**

**在第 1 行，我们声明了变量`foo`。这里没什么特别的。让我们称之为对`foo`的左侧大小(LHS ),因为我们给`foo`赋值，它在`equal`符号的左侧。**

**在第 3 行，我们声明了一个函数，并将其赋给变量`baz`。这是 LHS 对`baz`的又一次引用。我们给它赋值(记住，函数也是值！).然后在第 8 行调用这个函数。这是一个 RHS，或者说是`baz`的右手边参考。我们正在检索`baz`的值，在本例中是一个函数，然后调用它。**

**对`baz`的另一个 RHS 引用是如果我们将其值赋给另一个变量，例如`foo = baz`。这将是对`foo`的 LHS 引用和对`baz`的 RHS 引用。**

**LHS 和 RHS 的引用可能听起来令人困惑，但是它们对于讨论范围很重要。可以这样想:LHS 引用是给变量赋值，而 RHS 引用是检索变量的值。它们只是“检索值”和“赋值”的一种更简短、更方便的方式。**

**现在让我们来分析一下函数本身内部发生了什么。**

**编译器编译函数内部的代码时，进入函数的 ****局部作用域**** 。**

**在第 4 行，变量`bar`被声明。这是 LHS 对`bar`的引用。在下一行，我们在`console.log()`中有一个对`foo`的 RHS 引用。记住，我们正在检索`foo`的值，然后将其作为参数传递给方法`console.log()`。**

**当我们有一个对`foo`的 RHS 引用时，编译器会寻找变量`foo`的声明。编译器没有在函数本身，或者 ****函数的局部作用域**，**中找到它，所以它 ****上升一级:到全局作用域**** 。**

**此时，您可能认为范围与变量有关。这是正确的。作用域可以被认为是变量的容器。在局部范围内创建的所有变量都只能在该局部范围内访问。但是，所有本地作用域都可以访问全局作用域。(我知道你现在可能更加困惑，但请耐心听我讲几段)。**

**所以编译器会在全局范围内查找对变量`foo`的 LHS 引用。它在第 1 行找到一个，所以它从 LHS 引用中检索值，这是一个字符串:`'Hi, I am foo!'`。这个字符串被发送给`console.log()`方法，并输出到控制台。**

**编译器已经执行完函数内部的代码，所以我们回到第 9 行。在第 9 行，我们有一个变量`bar`的 RHS 引用。**

**现在，`bar`在`baz`的局部作用域中被声明，但是在全局作用域中有一个对`bar`的 RHS 引用。由于在全局范围内没有对`bar`的 LHS 引用，编译器找不到`bar`的值并抛出 ReferenceError。**

**但是，你可能会问，如果函数可以在自身之外寻找变量，或者局部作用域可以窥视全局作用域以找到 LHS 引用，为什么全局作用域不能窥视局部作用域呢？这就是词法范围的工作方式！**

```
**`... // global scope
var baz = function() {
  ... // baz's scope
}
... /// global scope`**
```

**这是上面说明范围的相同代码。这形成了一种上升到全局范围的层次结构:**

**`baz -> global`。**

**因此，如果在`baz`的作用域内有一个变量的 RHS 引用，那么它可以通过在全局作用域内对该变量的 LHS 引用来实现。但与之相反的是 ****而不是真正的**** 。**

**如果我们在`baz`中有另一个函数会怎么样？**

```
**`... // global scope
var baz = function() {
  ... // baz's scope

  var bar = function() {
     ... // bar's scope.
  }

}
... /// global scope`**
```

**在这种情况下，层次结构或 ****范围链**** 将如下所示:**

**`bar -> baz -> global`**

**在`bar`的本地范围内的任何 RHS 引用都可以由全局范围或`baz`的范围内的 LHS 引用来完成，但是在`baz`的范围内的 RHS 引用不能由`bar`的范围内的 LHS 引用来完成。**

******只能向下遍历一个作用域链，不能向上遍历。******

**关于 JavaScript 作用域，您还应该知道另外两件重要的事情。**

1.  **范围是由函数声明的，而不是由块声明的。**
2.  **函数可以被前向引用，但变量不能。**

**观察(每个注释描述了它所在行的范围):**

```
 **`// outer() is in scope here because functions can be forward-referenced

    function outer() {

        // only inner() is in scope here
        // because only functions are forward-referenced

        var a = 1;

        //now 'a' and inner() are in scope

        function inner() {
            var b = 2

            if (a == 1) {
                var c = 3;
            }

            // 'c' is still in scope because JavaScript doesn't care
            // about the end of the 'if' block, only function inner()
        }

        // now b and c are out of scope
        // a and inner() are still in scope

    }

    // here, only outer() is in scope`**
```

## **JavaScript For 循环示例**

### ****语法****

```
**`for ([initialization]); [condition]; [final-expression]) {
   // statement
}`**
```

**javascript `for`语句由三个表达式和一个语句组成:**

*   **初始化——在循环第一次执行之前运行。该表达式通常用于创建计数器。这里创建的变量的作用范围是循环。一旦循环执行完毕，它们就会被销毁。**
*   **条件——在每次迭代执行之前检查的表达式。如果省略，此表达式的计算结果为 true。如果计算结果为 true，则执行循环语句。如果计算结果为 false，则循环停止。**
*   **final-expression -每次迭代后运行的表达式。通常用于递增计数器。但是它也可以用于递减计数器。**
*   **语句-循环中要重复的代码**

**这三个表达式或语句中的任何一个都可以省略。For 循环通常用于计算重复一条语句的迭代次数。在条件表达式计算为 false 之前，使用`break`语句退出循环。**

## ****常见陷阱****

******超出数组界限******

**当多次对数组进行索引时，很容易超出数组的界限(例如尝试引用 3 元素数组的第 4 个元素)。**

```
 **`// This will cause an error.
    // The bounds of the array will be exceeded.
    var arr = [ 1, 2, 3 ];
    for (var i = 0; i <= arr.length; i++) {
       console.log(arr[i]);
    }

    output:
    1
    2
    3
    undefined`**
```

**有两种方法可以修复这个代码。将条件设置为`i < arr.length`或`i <= arr.length - 1`**

### ****例题****

**遍历从 0 到 8 的整数**

```
**`for (var i = 0; i < 9; i++) {
   console.log(i);
}

output:
0
1
2
3
4
5
6
7
8`**
```

**在条件表达式为假之前中断循环**

```
**`for (var elephant = 1; elephant < 10; elephant+=2) {
    if (elephant === 7) {
        break;
    }
    console.info('elephant is ' + elephant);
}

output:
elephant is 1
elephant is 3
elephant is 5`**
```

## **JavaScript Break 语句示例**

******break**** 语句终止当前循环，`switch`或`label`语句，并将程序控制权转移给终止语句后的语句。**

```
**`break;`**
```

**如果在带标签的语句中使用了 ****break**** 语句，语法如下:**

```
**`break labelName;`**
```

## ****例题****

**下面的函数有一个 ****break**** 语句，当 ****i**** 为 3 时终止`while`循环，然后返回值 ****3 * x**** 。**

```
**`function testBreak(x) {
  var i = 0;

  while (i < 6) {
    if (i == 3) {
      break;
    }
    i += 1;
  }

  return i * x;
}`**
```

**在下面的示例中，计数器设置为从 1 计数到 99；然而， ****break**** 语句在 14 次计数后终止循环。**

```
**`for (var i = 1; i < 100; i++) {
  if (i == 15) {
    break;
  }
}`**
```

## **JavaScript Do While 循环示例**

**`do...while`回路与 [`while`](http://forum.freecodecamp.com/t/javascript-while-loop/14668) 回路密切相关。在 do while 循环中，在循环结束时检查条件。**

**下面是 ****的语法**** 用于`do...while`循环:**

## ****语法:****

```
 **`do {

   *Statement(s);*

} while (*condition*);`**
```

******语句:**** 在对条件或布尔表达式求值之前至少执行一次 ****的语句，并且在每次条件求值为真时重新执行。******

******条件:**** 这里，条件是一个。如果布尔表达式的计算结果为 true，则再次执行该语句。当布尔表达式的值为 false 时，循环结束。**

## ****举例:****

```
**`var i = 0;
do {
  i = i + 1;
  console.log(i);
} while (i < 5);

Output:
1
2
3
4
5`**
```

## **JavaScript For In 循环示例**

**`for...in`语句以任意顺序遍历对象的可枚举属性。对于每个不同的属性，可以执行语句。**

```
**`for (variable in object) {
...
}`**
```

**required/OptionalParameterDescriptionRequiredVariable:在每次迭代中为变量分配不同的属性名。OptionalObject:其可枚举属性被迭代的对象。**

## ****例题****

```
**`// Initialize object.
a = { "a": "Athens", "b": "Belgrade", "c": "Cairo" }

// Iterate over the properties.
var s = ""
for (var key in a) {
    s += key + ": " + a[key];
    s += "<br />";
    }
document.write (s);

// Output:
// a: Athens
// b: Belgrade
// c: Cairo

// Initialize the array.
var arr = new Array("zero", "one", "two");

// Add a few expando properties to the array.
arr["orange"] = "fruit";
arr["carrot"] = "vegetable";

// Iterate over the properties and elements.
var s = "";
for (var key in arr) {
    s += key + ": " + arr[key];
    s += "<br />";
}

document.write (s);

// Output:
//   0: zero
//   1: one
//   2: two
//   orange: fruit
//   carrot: vegetable

// Efficient way of getting an object's keys using an expression within the for-in loop's conditions
var myObj = {a: 1, b: 2, c:3}, myKeys = [], i=0;
for (myKeys[i++] in myObj);

document.write(myKeys);

//Output:
//   a
//   b
//   c`**
```

## **JavaScript For Of 循环示例**

**`for...of`语句创建一个遍历可迭代对象(包括数组、映射、集合、参数对象等)的循环，调用一个定制的迭代钩子，为每个不同的属性值执行语句。**

```
 **`for (variable of object) {
        statement
    }`**
```

**描述变量:在每次迭代中，不同属性的值被赋给 variable.object 对象，该对象的可枚举属性被迭代。**

## ****例题****

### ****数组****

```
 **`let arr = [ "fred", "tom", "bob" ];

    for (let i of arr) {
        console.log(i);
    }

    // Output:
    // fred
    // tom
    // bob`**
```

### ****地图****

```
 **`var m = new Map();
    m.set(1, "black");
    m.set(2, "red");

    for (var n of m) {
        console.log(n);
    }

    // Output:
    // 1,black
    // 2,red`**
```

### ****设置****

```
 **`var s = new Set();
    s.add(1);
    s.add("red");

    for (var n of s) {
        console.log(n);
    }

    // Output:
    // 1
    // red`**
```

### ****自变量对象****

```
 **`// your browser must support for..of loop
    // and let-scoped variables in for loops

    function displayArgumentsObject() {
        for (let n of arguments) {
            console.log(n);
        }
    }

    displayArgumentsObject(1, 'red');

    // Output:
    // 1
    // red`**
```

## **JavaScript While 循环示例**

**while 循环从评估条件开始。如果条件为真，则执行语句。如果条件为假，则不执行语句。之后，while 循环结束。**

**下面是 while 循环的 ****语法**** :**

## ****语法:****

```
**`while (condition)

{

  statement(s);

}`**
```

***语句:*只要条件评估为真就执行的语句。**

***条件:*这里，条件是一个布尔表达式，在每次循环之前对其进行评估。如果该条件评估为真，则执行语句。当条件评估为 false 时，将继续执行 while 循环后的语句。**

## ****举例:****

```
 **`var i = 1;
    while (i < 10) 
    {
      console.log(i);
       i++; // i=i+1 same thing
    }

    Output:
    1 
    2 
    3 
    4
    5
    6
    7
    8
    9`**
```