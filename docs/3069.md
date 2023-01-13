# 开发人员在 JavaScript 中最常犯的九个错误(以及如何纠正它们)

> 原文：<https://www.freecodecamp.org/news/nine-most-common-mistakes-developers-make-in-javascript/>

JavaScript 是一种[脚本语言](https://en.wikipedia.org/wiki/Scripting_language)，在网页中用于增加功能和交互性。对于一个来自不同编程语言的初学者来说，JavaScript 非常容易理解。通过几个教程，你应该可以马上上手。

然而，有一些许多初学程序员会犯的常见错误。在本文中，我们将解决九个常见错误(或不良实践)及其解决方案，以帮助您成为更好的 JS 开发人员。

## 混淆赋值(=)和相等(==，===)运算符

顾名思义，[赋值操作符](https://www.w3resource.com/javascript/operators/assignment-operator.php#:~:text=Assignment%20Operators,value%20of%20its%20right%20operand.&text=That%20is%2C%20a%20%3D%20b%20assigns,shown%20in%20the%20following%20table.) (=)用于给变量赋值。开发人员经常将它与等式运算符混淆。

这里有一个例子:

```
const name = "javascript";
if ((name = "nodejs")) {
    console.log(name);
}
// output - nodejs
```

在这种情况下，不比较 name 变量和“nodejs”字符串。相反，会将“nodejs”分配给 name，并将“nodejs”打印到控制台。

在 JavaScript 中，双等号(==)和三等号(===)称为比较运算符。

对于上面的代码，这是比较值的适当方式:

```
const name = "javascript";
if (name == "nodejs") {
    console.log(name);
}
// no output
// OR
if (name === "nodejs") {
    console.log(name);
}
// no output
```

这些比较运算符之间的区别在于，双等于执行一个**松散**比较，而三等于执行一个**严格**比较。

在松散比较中，只比较值。但是在严格的比较中，值和数据类型是比较的。

以下代码对此进行了更好的解释:

```
const number = "1";
console.log(number == 1);
// true
console.log(number === 1);
// false
```

变量 number 被赋予字符串值 1。当使用双等于与 1(数字类型)比较时，它返回 true，因为两个值都是 1。

但是当使用三重等于进行比较时，它返回 false，因为每个值都有不同的数据类型。

## 期望回调是同步的

回调是 JavaScript 处理异步操作的一种方式。然而，承诺和 async/await 是处理异步操作的更好方法，因为多次回调会导致[回调地狱](http://callbackhell.com/)。

回调不是 ****同步**** 。它们被用作一个函数，当延迟执行完成时，在一个操作之后调用。

一个例子是全局`setTimeout​`函数，它接收一个回调函数作为第一个参数，接收一个持续时间(以毫秒为单位)作为第二个参数，如下所示:

```
function callback() {
​​    console.log("I am the first");
​​}
​​setTimeout(callback, 300);
​​console.log("I am the last");
​​// output
​​// I am the last
​​// I am the first
```

300 毫秒后，回调函数被调用。但是在它完成之前，其余的代码会运行。这就是为什么首先运行最后一个 console.log 的原因。​​

开发人员常犯的一个错误是将回调误解为同步的。例如，回调会返回一个将用于其他操作的值。

这里有个错误:

```
function addTwoNumbers() {
​​    let firstNumber = 5;
​​    let secondNumber;
​​    setTimeout(function () {
​​        secondNumber = 10;
​​    }, 200);
​​    console.log(firstNumber + secondNumber);
​​}
​​addTwoNumbers();
​​// NaN
```

`NaN`是输出，因为`secondNumber​`未定义。运行`firstNumber + secondNumber`的时候，`secondNumber`还没有定义，因为`setTimeout`函数会在`200ms`之后执行回调。

最好的方法是执行回调函数中的其余代码:

```
function addTwoNumbers() {
​​    let firstNumber = 5;
​​    let secondNumber;
​​    setTimeout(function () {
​​        secondNumber = 10;
​​        console.log(firstNumber + secondNumber);
​​    }, 200);
​​}
​​addTwoNumbers();
​​// 15
```

## 对`this​`的错误引用

`this​`是 JavaScript 中一个普遍被误解的[概念](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)。要在 JavaScript 中使用`this`,你真的需要理解它是如何工作的，因为与其他语言相比，它的操作有点不同。

这里有一个使用`this​`时常见错误的例子:

```
const obj = {
​​    name: "JavaScript",
​​    printName: function () {
​​        console.log(this.name);
​​    },
​​    printNameIn2Secs: function () {
​​        setTimeout(function () {
​​            console.log(this.name);
​​        }, 2000);
​​    },
​​};
​​obj.printName();
​​// JavaScript
​​obj.printNameIn2Secs();
​​// undefined
```

第一个结果是 **`JavaScript`** ，因为`this.name`正确地指向了对象的名称属性。第二个结果是`**undefined**​`，因为`this​`已经丢失了对对象属性(包括名称)的引用。

这是因为`this​`依赖于调用它所在函数的对象。每个函数中都有一个`this`变量，但是它指向的对象是由调用它的对象决定的。

`obj.printName()`中的`this​`直接指向`obj`。`obj.printNameIn2Secs​`中的`this`直接指向`obj​`。但是`setTimeout​`的回调函数中的`this​`没有指向任何对象，因为没有对象调用它。

对于一个调用了`setTimeout​`的对象，类似于`obj.setTimeout...​`的东西将被执行。因为没有对象调用该函数，所以使用默认对象(即`window`)。

窗口上不存在`name`，导致`undefined`。

在`setTimeout`中保留对`this`的引用的最佳方式是使用`bind​`、`call​`、`apply`或箭头函数(在 ES6 中引入)。与普通函数不同，箭头函数不会创建自己的`this`。

因此，以下将保留其对`this​`的引用:

```
​​const obj = {
​​    name: "JavaScript",
​​    printName: function () {
​​        console.log(this.name);
​​    },
​​    printNameIn2Secs: function () {
​​        setTimeout(() => {
​​            console.log(this.name);
​​        }, 2000);
​​    },
​​};
​​obj.printName();
​​// JavaScript
​​obj.printNameIn2Secs();
​​// JavaScript
```

## 忽略对象可变性

与字符串、数字等原始数据类型不同，JavaScript 对象是引用数据类型。例如，在键值对象中:

```
const obj1 = {
​​    name: "JavaScript",
​​};
​​const obj2 = obj1;
​​obj2.name = "programming";
​​console.log(obj1.name);
​​// programming
```

`obj1​`和`obj2`拥有对对象在存储器中存储位置的相同引用。

在数组中:

```
const arr1 = [2, 3, 4];
​​const arr2 = arr1;
​​arr2[0] = "javascript";
​​console.log(arr1);
​​// ['javascript', 3, 4]
```

开发人员常犯的一个错误是他们忽略了 JavaScript 的这一特性，这导致了意想不到的错误。例如，如果 5 个对象对同一个对象有相同的引用，其中一个对象可能会干扰大规模代码库中的属性。

当这种情况发生时，任何访问原始属性的尝试都将返回 undefined 或者可能抛出一个错误。

这方面的最佳实践是，当您想要复制一个对象时，总是为新对象创建新引用。为此，rest 操作符(`...​`在 ES6 中引入)是一个完美的解决方案。

例如，在键值对象中:

```
​​const obj1 = {
​​    name: "JavaScript",
​​};
​​const obj2 = { ...obj1 };
​​console.log(obj2);
​​// {name: 'JavaScript' }
​​obj2.name = "programming";
​​console.log(obj.name);
​​// 'JavaScript'
```

在数组中:

```
const arr1 = [2, 3, 4];
​​const arr2 = [...arr1];
​​console.log(arr2);
​​// [2,3,4]
​​arr2[0] = "javascript";
​​console.log(arr1);
​​// [2, 3, 4]
```

## 将数组和对象保存到浏览器存储中

有时候，在使用 JavaScript 时，开发人员可能想要利用`localStorage`来保存值。但是一个常见的错误是试图将[数组和对象](https://www.tutorialspoint.com/javascript/javascript_arrays_object.htm)原样保存在`localStorage`中。`localStorage`只接受字符串。

为了保存对象，JavaScript 将对象转换为字符串。结果是对象的`[Object Object]`和数组元素的逗号分隔字符串。

例如:

```
​​const obj = { name: "JavaScript" };
​​window.localStorage.setItem("test-object", obj);
​​console.log(window.localStorage.getItem("test-object"));
​​// [Object Object]
​​const arr = ["JavaScript", "programming", 45];
​​window.localStorage.setItem("test-array", arr);
​​console.log(window.localStorage.getItem("test-array"));
​​// JavaScript, programming, 45
```

当对象以这种方式保存时，访问它们就变得很困难。对于对象示例，像`.name​`这样访问对象会导致错误。这是因为`[Object Object]`现在是一个字符串，没有`​name`属性。

在本地存储中保存对象和数组的更好的方法是使用`JSON.stringify​`(用于将对象转换为字符串)和`JSON.parse​`(用于将字符串转换为对象)。这样，访问对象就变得容易了。

上面代码的正确版本应该是:

```
​​const obj = { name: "JavaScript" };
​​window.localStorage.setItem("test-object", JSON.stringify(obj));
​​const objInStorage = window.localStorage.getItem("test-object");
​​console.log(JSON.parse(objInStorage));
​​// {name: 'JavaScript'}
​​const arr = ["JavaScript", "programming", 45];
​​window.localStorage.setItem("test-array", JSON.stringify(arr));
​​const arrInStorage = window.localStorage.getItem("test-array");
​​console.log(JSON.parse(arrInStorage));
​​// JavaScript, programming, 45
```

## 不使用默认值

在动态变量中设置[默认值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)是防止意外错误的一个非常好的做法。下面是一个常见错误的例子:

```
function addTwoNumbers(a, b) {
​​    console.log(a + b);
​​}
​​addTwoNumbers();
​​// NaN
```

结果是`NaN​`，因为`a`是`undefined`，而`b`是`undefined​`。通过使用默认值，可以避免类似的错误。例如:

```
function addTwoNumbers(a, b) {
​​    if (!a) a = 0;
​​    if (!b) b = 0;
​​    console.log(a + b);
​​}
​​addTwoNumbers();
​​// 0
```

或者，ES6 中引入的默认值特性可以这样使用:

```
​​function addTwoNumbers(a = 0, b = 0) {
​​    console.log(a + b);
​​}
​​addTwoNumbers();
​​// 0
```

这个例子虽然很简单，但强调了默认值的重要性。此外，当没有提供预期值时，开发人员可以提供错误或警告消息。

## 变量命名不当

是的，开发者还是会犯这个错误。命名很难，但是开发者真的没有办法。注释是编程中的好习惯，命名[变量](https://en.wikipedia.org/wiki/Variable_(computer_science))也是如此。

例如:

```
function total(discount, p) {
​​    return p * discount
​​}
```

变量`discount`可以，但是`p`或者`total​`呢？总共什么？对于上述情况，更好的做法是:

```
function totalPrice(discount, price) {
​​    return discount * price
​​}
```

正确命名变量是很重要的，因为在特定的时间或将来，开发人员可能永远不会是代码库的唯一开发人员。

恰当地命名变量将使贡献者容易理解项目是如何工作的。

## 检查布尔值

```
const isRaining = false
​​if(isRaining) {
​​    console.log('It is raining')
​​} else {
​​    console.log('It is not raining')
​​}
​​// It is not raining
```

检查布尔值是常见的做法，如上面的代码所示。虽然这没问题，但是在测试一些值时会出现错误。

在 JavaScript 中，`0`和`false`的松散比较返回`true`，`1`和`true​`返回`true`。这意味着如果`isRaining`是`1`，那么`isRaining`就是`true`。

这也是物体经常犯的错误。例如:

```
const obj = {
​​    name: 'JavaScript',
​​    number: 0
​​}
​​if(obj.number) {
​​    console.log('number property exists')
​​} else {
​​    console.log('number property does not exist')
​​}
​​// number property does not exist
```

虽然`number`属性存在，`obj.number`返回`0`，这是一个`falsy`值，因此`else​`块被执行。

因此，除非您确定将要使用的值的范围，否则对象中的布尔值和属性应该像这样进行测试:

```
if(a === false)...
if(object.hasOwnProperty(property))...
```

## 混淆加法和连接

加号`(+)`在 JavaScript 中有两个功能:加法和连接。加法是针对数字的，串联是针对字符串的。一些开发人员经常误用这个运算符。

例如:

```
const num1 = 30;
​​const num2 = "20";
​​const num3 = 30;
​​const word1 = "Java"
​​const word2 = "Script"
​​console.log(num1 + num2);
​​// 3020
​​console.log(num1 + num3);
​​// 60
​​console.log(word1 + word2);
​​// JavaScript
​​
```

当添加字符串和数字时，JavaScript 将数字转换为字符串，并连接所有值。对于数字的相加，执行数学运算。​​

## 结论

当然，除了上面列出的错误之外，还有更多错误(有些是微不足道的，有些是严重的)。所以只要确保你跟上语言的发展就行了。

研究并避免这些错误将有助于您构建更好、更可靠的 web 应用程序和工具。