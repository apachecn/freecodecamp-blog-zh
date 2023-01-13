# ES6 让您的代码更干净、更简短、更易读的提示和技巧！

> 原文：<https://www.freecodecamp.org/news/make-your-code-cleaner-shorter-and-easier-to-read-es6-tips-and-tricks-afd4ce25977c/>

作者:萨姆·威廉姆斯

![AEOLhZTlWk1OgK8hBoQETJ8X-P0dpJSJnkpm](img/ca226d0ba73056515d0bf98bc094558a.png)

# ES6 让您的代码更干净、更简短、更易读的提示和技巧！

### 模板文字

模板文字使得处理字符串比以前容易多了。它们以反勾号开始，可以使用`${variable}`插入变量。比较这两行代码:

```
var fName = 'Peter', sName = 'Smith', age = 43, job= 'photographer';var a = 'Hi, I\'m ' + fName + ' ' + sName + ', I\'m ' + age + ' and work as a ' + job + '.';var b = `Hi, I'm ${ fName } ${ sName }, I'm ${ age } and work as a ${ job }.`;
```

这使得生活更简单，代码更容易阅读。你可以把任何东西放在花括号里:变量、方程或者函数调用。我将在本文的例子中使用这些。

### 语法块范围

JavaScript 的作用域总是由函数确定的，这就是为什么将整个 JavaScript 文件包装在一个空的直接调用函数表达式(IIFE)中变得很常见。这样做是为了隔离文件中的所有变量，因此没有变量冲突。

现在，我们有了块范围和两个绑定到块的新变量声明。

### Let 宣言

这与`var`相似，但有一些显著的不同。因为它是块范围的，所以可以声明同名的新变量，而不会影响外部变量。

```
var a = 'car' ;{    let a = 5;    console.log(a) // 5}console.log(a) // car
```

因为它被绑定到一个 block 范围，所以它解决了这个经典的面试问题:
“什么是输出，你如何让它按照你的期望工作？”

```
for (var i = 1; i < 5; i++){    setTimeout(() => { console.log(i); }, 1000);}
```

在这种情况下，它输出“5 5 5 5 5 ”,因为变量`i` 在每次迭代中都会改变。

如果你把`var`换成`let`，那么一切都会改变。现在，每个循环创建一个新的块范围，I 的值绑定到该循环。虽然你写道:

```
{let i = 1; setTimeout(() => { console.log(i) }, 1000)} {let i = 2; setTimeout(() => { console.log(i) }, 1000)} {let i = 3; setTimeout(() => { console.log(i) }, 1000)} {let i = 4; setTimeout(() => { console.log(i) }, 1000)} {let i = 5; setTimeout(() => { console.log(i) }, 1000)} 
```

`var`和`let`的另一个区别是`let`不像`var`那样被吊起。

```
{     console.log(a); // undefined    console.log(b); // ReferenceError    var a = 'car';    let b = 5;}
```

因为它更严格的范围和更可预测的行为，一些人说你应该使用`let`而不是`var`，除非你特别需要提升或者放宽`var`声明的范围。

### 常数

如果你以前想在 JavaScript 中声明一个常量变量，习惯上是用大写字母命名变量。然而，这不会保护变量——它只是让其他开发人员知道它是一个常量，不应该被更改。

现在我们有了`const`宣言。

```
{    const c = "tree";    console.log(c);  // tree    c = 46;  // TypeError! }
```

不使变量不可变，只是锁定它的赋值。如果你有一个复杂的赋值(对象或数组)，那么这个值仍然可以被修改。

```
{    const d = [1, 2, 3, 4];    const dave = { name: 'David Jones', age: 32};    d.push(5);     dave.job = "salesman";    console.log(d);  // [1, 2, 3, 4, 5]    console.log(dave);  // { age: 32, job: "salesman", name: 'David Jones'}}
```

### 块作用域函数有问题

函数声明现在被指定为绑定到块范围。

```
{    bar(); // works    function bar() { /* do something */ }}bar();  // doesn't work
```

当你在一个`if`语句中声明一个函数时，问题就来了。

考虑一下这个:

```
if ( something) {    function baz() { console.log('I passed') }} else {    function baz() { console.log('I didn\'t pass') } } baz();
```

在 ES6 之前，两个函数声明都会被提升，无论`something`是什么，结果都会是`'I didn\'t pass'`。
现在我们得到`'ReferenceError'`，因为`baz`总是被 block 作用域所约束。

### 传播

ES6 引入了`...`运算符，它被称为“扩展运算符”。它有两个主要用途:将一个数组或对象扩展到一个新的数组或对象中，以及将多个参数连接到一个数组中。

第一个用例可能是您最常遇到的，所以我们先来看看。

```
let a = [3, 4, 5];let b = [1, 2, ...a, 6];console.log(b);  // [1, 2, 3, 4, 5, 6]
```

这对于从数组向函数传递一组变量非常有用。

```
function foo(a, b, c) { console.log(`a=${a}, b=${b}, c=${c}`)} let data = [5, 15, 2];foo( ...data); // a=5, b=15, c=2
```

还可以扩展一个对象，将每个键值对输入到新对象中。(对象传播实际上处于提案的第 4 阶段，将于 ES2018 年正式发布。它仅受 Chrome 60 或更高版本、Firefox 55 或更高版本以及 Node 6.4.0 或更高版本的支持)

```
let car = { type: 'vehicle ', wheels: 4};let fordGt = { make: 'Ford', ...car, model: 'GT'};console.log(fordGt); // {make: 'Ford', model: 'GT', type: 'vehicle', wheels: 4}
```

spread 运算符的另一个特性是它创建一个新的数组或对象。下面的例子为`b`创建了一个新数组，但是`c`只是引用了同一个数组。

```
let a = [1, 2, 3];let b = [ ...a ];let c = a;b.push(4);console.log(a);  // [1, 2, 3]console.log(b);  // [1, 2, 3, 4] referencing different arraysc.push(5);console.log(a);  // [1, 2, 3, 5] console.log(c);  // [1, 2, 3, 5] referencing the same array
```

第二个用例是将变量收集到一个数组中。当你不知道有多少变量被传递给一个函数时，这是非常有用的。

```
function foo(...args) {    console.log(args); } foo( 'car', 54, 'tree');  //  [ 'car', 54, 'tree' ] 
```

### 默认参数

现在可以用默认参数定义函数。缺少或未定义的值用默认值初始化。只是要小心—因为 null 和 false 值被强制为 0。

```
function foo( a = 5, b = 10) {    console.log( a + b);} foo();  // 15foo( 7, 12 );  // 19foo( undefined, 8 ); // 13foo( 8 ); // 18foo( null ); // 10 as null is coerced to 0
```

默认值不仅仅是值，还可以是表达式或函数。

```
function foo( a ) { return a * 4; }function bar( x = 2, y = x + 4, z = foo(x)) {    console.log([ x, y, z ]);}bar();  // [ 2, 6, 8 ]bar( 1, 2, 3 ); //[ 1, 2, 3 ] bar( 10, undefined, 3 );  // [ 10, 14, 3 ]
```

### 解构

析构是将等号左边的数组或对象拆开的过程。数组或对象可以来自变量、函数或方程。

```
let [ a, b, c ] = [ 6, 2, 9];console.log(`a=${a}, b=${b}, c=${c}`); //a=6, b=2, c=9
```

```
function foo() { return ['car', 'dog', 6 ]; } let [ x, y, z ] = foo();console.log(`x=${x}, y=${y}, z=${z}`);  // x=car, y=dog, z=6
```

通过对象析构，对象的键可以在花括号中列出，以提取键值对。

```
function bar() { return {a: 1, b: 2, c: 3}; }let { a, c } = bar();console.log(a); // 1console.log(c); // 3console.log(b); // undefined
```

有时，您希望提取值，但将它们赋给一个新变量。这是通过等号左边的“键:变量”配对来完成的。

```
function baz() {     return {        x: 'car',        y: 'London',        z: { name: 'John', age: 21}    }; }let { x: vehicle, y: city, z: { name: driver } } = baz();
```

```
console.log(    `I'm going to ${city} with ${driver} in their ${vehicle}.`); // I'm going to London with John in their car. 
```

对象析构允许的另一件事是给多个变量赋值。

```
let { x: first, x: second } = { x: 4 };console.log( first, second ); // 4, 4
```

### 对象文字和简明参数

当您从变量创建对象文字时，ES6 允许您忽略与变量名相同的键。

```
let a = 4, b = 7;let c = { a: a, b: b };let concise = { a, b };console.log(c, concise) // {a: 4, b: 7}, {a: 4, b: 7}
```

这也可以和析构结合使用，使你的代码更加简单和整洁。

```
function foo() {    return {        name: 'Anna',         age: 56,       job: { company: 'Tesco', title: 'Manager' }    };} 
```

```
// pre ES6let a = foo(), name = a.name, age = a.age, company = a.job.company;
```

```
// ES6 destructuring and concise parameters let { name, age, job: {company}} = foo();
```

它还可以用来析构传递给函数的对象。方法 1 和 2 是你在 ES6 之前应该做的，方法 3 使用析构和简洁参数。

```
let person = {    name: 'Anna',     age: 56,    job: { company: 'Tesco', title: 'Manager' }};
```

```
// method 1function old1( person) {    var yearOfBirth = 2018 - person.age;    console.log( `${ person.name } works at ${ person.job.company } and was born in ${ yearOfBirth }.`);}
```

```
// method 2function old1( person) {    var age = person.age,        yearOfBirth = 2018 - age,         name = person.name,        company = person.job.company;    console.log( `${ name } works at ${ company } and was born in ${ yearOfBirth }.`);} 
```

```
// method 3function es6({ age, name, job: {company}}) {    var yearOfBirth = 2018 - age;    console.log( `${ name } works at ${ company } and was born in ${ yearOfBirth }.`);} 
```

使用 ES6，我们可以提取`age`、`name`和`company`，而不需要额外的变量声明。

### 动态属性名称

ES6 增加了使用动态分配的键创建或添加属性的能力。

```
let  city= 'sheffield_';let a = {    [ city + 'population' ]: 350000};a[ city + 'county' ] = 'South Yorkshire';console.log(a); // {sheffield_population: 350000, sheffield_county: 'South Yorkshire' }
```

### 箭头功能

箭头函数有两个主要方面:它们的结构和它们的`this`绑定。

它们可以有比传统函数更简单的结构，因为它们不需要关键字`function`，并且它们自动返回箭头后面的任何内容。

```
var foo = function( a, b ) {    return a * b;} 
```

```
let bar = ( a, b ) => a * b;
```

如果函数需要的不仅仅是简单的计算，可以使用花括号，函数返回从块范围返回的任何内容。

```
let baz = ( c, d ) => {    let length = c.length + d.toString().length;    let e = c.join(', ');    return `${e} and there is a total length of  ${length}`;}
```

箭头函数最有用的地方之一是在数组函数中，如`.map`、`.forEach`或`.sort`。

```
let arr = [ 5, 6, 7, 8, 'a' ];let b = arr.map( item => item + 3 );console.log(b); // [ 8, 9, 10, 11, 'a3' ]
```

除了具有更短的语法之外，它还修复了围绕`this`绑定行为经常出现的问题。对 ES6 之前的函数的修正是存储`this`引用，通常作为一个`self`变量。

```
var clickController = {    doSomething: function (..) {        var self = this;        btn.addEventListener(            'click',             function() { self.doSomething(..) },             false       );   } };
```

因为`this`绑定是动态的，所以必须这样做。这意味着事件侦听器中的`this`和`doSomething`中的`this`指的不是同一个东西。

在 arrow 函数内部，`this`绑定是词法的，而不是动态的。这是 arrow 函数的主要设计特征。

虽然词法绑定可能很棒，但有时这并不是我们想要的。

```
let a = {    oneThing: ( a ) => {         let b = a * 2;         this.otherThing(b);    },     otherThing: ( b ) => {....} };
```

```
a.oneThing(6);
```

当我们使用`a.oneThing(6)`时，`this.otherThing( b )`引用失败，因为`this`没有指向`a`对象，而是指向周围的范围。如果您使用 ES6 语法重写遗留代码，这是需要注意的。

### `for … of`循环

ES6 增加了迭代数组中每个值的方法。这不同于现有的在键/索引上循环的`for ... in`循环。

```
let a = ['a', 'b', 'c', 'd' ];// ES6 for ( var val of a ) {    console.log( val );} // "a" "b" "c" "d"// pre-ES6 for ( var idx in a ) {    console.log( idx );}  // 0 1 2 3
```

使用新的`for … of`循环节省了在每个循环中添加一个`let val = a[idx]`的时间。

在标准 JavaScript 中，数组、字符串、生成器和集合都是可迭代的。普通对象通常不能被迭代，除非你为它定义了一个迭代器。

### 数字文字

ES5 代码很好地处理了十进制和十六进制数字格式，但是没有指定八进制格式。事实上，它在严格模式下是被禁止的。

ES6 增加了一种新的格式，在首字母`0`后增加了一个`o`来声明一个八进制数。他们还增加了二进制格式。

```
Number( 29 )  // 29Number( 035 ) // 35 in old octal form. Number( 0o35 ) // 29 in new octal form Number( 0x1d ) // 29 in hexadecimal Number( 0b11101 ) // 29 in binary form
```

### 还有更多…

ES6 为我们提供了更多的东西，让我们的代码更简洁、更易读、更健壮。我的目标是写这篇文章的续篇，涵盖 ES6 不太为人所知的部分。

如果你等不了那么久，可以在 ES6 上读一读凯尔·辛普森的[你不知道的 JS 书，或者看看这个](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch2.md)[辉煌的小网站](http://es6-features.org/#Constants)！

你想成为一名开发人员并得到你的第一份软件工作吗？下载成为开发人员和获得第一份工作的 7 个步骤。

> 接下来-& g[t；如何保住你的理想工作？掌握面试流程](https://medium.com/@samwsoftware/how-to-secure-the-job-of-your-dreams-by-smashing-your-interview-61f38b7cdd0e) ess

如果你喜欢这篇文章并觉得它很有帮助，请鼓掌表示你的支持，并订阅更多这样的文章！

![YteINwvwgVVGxTMs5umIsksyix1ILkn-W0dD](img/5137d78f050a98b5dbc5923326d4e8ee.png)![w12a6mIA8-w7GgPNcl2wD7rHCVlNKVYHpuEB](img/42486a46b70c19ee20825c602b8b0fa1.png)![bs1er3gxQhRoQ1WIfxT6fZLLBUFNIdAR9ER5](img/8b022bd8e2e9b04da60968c364c41886.png)