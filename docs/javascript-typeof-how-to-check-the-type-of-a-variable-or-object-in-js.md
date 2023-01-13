# JavaScript Type of——如何在 JS 中检查变量或对象的类型

> 原文：<https://www.freecodecamp.org/news/javascript-typeof-how-to-check-the-type-of-a-variable-or-object-in-js/>

数据类型和类型检查是任何编程语言的基本方面。

像 Java 这样的许多编程语言都有严格的类型检查。这意味着，如果一个变量是用特定类型定义的，那么它只能包含该类型的值。

然而，JavaScript 是一种松散类型(或动态类型)的语言。这意味着变量可以包含任何类型的值。JavaScript 代码可以这样执行:

```
let one = 1;
one = 'one';
one = true;
one = Boolean(true);
one = String('It is possible'); 
```

记住这一点，在任何给定的时间知道变量的类型是至关重要的。

变量的类型由赋给它的值的类型决定。JavaScript 有一个名为`typeof`的特殊操作符，它允许您获取任何值的类型。

在本文中，我们将学习如何使用`typeof`，以及一些需要注意的问题。

# **JavaScript 数据类型**

在我们深入研究`typeof`操作符之前，让我们快速看一下 JavaScript 数据类型。

在 JavaScript 中，有七种基本类型。原语是任何不是对象的东西。它们是:

1.  线
2.  数字
3.  比吉斯本
4.  标志
5.  布尔代数学体系的
6.  不明确的
7.  空

其他一切都是一个`object`——甚至包括`array`和`function`。对象是键值对的集合。

# **JavaScript 类型的运算符**

`typeof`操作符只接受一个操作数(一元操作符)。它计算操作数的类型，并以字符串形式返回结果。当你评估一个数的类型，007 时，这是你如何使用它。

```
typeof 007;  // returns 'number' 
```

对于`typeof`操作符有另一种语法，您可以像使用`function`一样使用它:

```
typeof(operand) 
```

当您想要评估表达式而不是单个值时，此语法非常有用。这里有一个例子:

```
typeof(typeof 007); // returns 'string' 
```

在上面的示例中，表达式`typeof 007`的计算结果为 number 类型，并返回字符串‘number’。`typeof('number')`然后产生`'string'`。

让我们看另一个例子来理解带`typeof`操作符的括号的重要性。

```
typeof(999-3223); // returns, "number"
```

如果省略括号，它将返回，`NaN`(不是数字):

```
typeof 999-3223; // returns, NaN
```

这是因为，首先`typeof 999`会产生一个字符串，“数字”。当您在字符串和数字之间执行减法运算时，表达式`"number" - 32223`会产生 NaN。

### **JavaScript 类型的例子**

下面的代码片段显示了使用`typeof`操作符对各种值进行类型检查的结果。

```
typeof 0;  //'number'
typeof +0;  //'number'
typeof -0;  //'number'
typeof Math.sqrt(2);  //'number'
typeof Infinity;  //'number'
typeof NaN;  //'number', even if it is Not a Number
typeof Number('100');  //'number', After successfully coerced to number
typeof Number('freeCodeCamp');  //'number', despite it can not be coerced to a number

typeof true;  //'boolean'
typeof false;  //'boolean'
typeof Boolean(0);  //'boolean'

typeof 12n;  //'bigint'

typeof '';  //'string'
typeof 'freeCodeCamp';  //'string'
typeof `freeCodeCamp is awesome`;  //'string'
typeof '100';  //'string'
typeof String(100); //'string'

typeof Symbol();  //'symbol'
typeof Symbol('freeCodeCamp');  //'symbol'

typeof {blog: 'freeCodeCamp', author: 'Tapas A'};  //'object';
typeof ['This', 'is', 101]; //'object'
typeof new Date();  //'object'
typeof Array(4);  //'object'

typeof new Boolean(true);  //'object'; 
typeof new Number(101);  //'object'; 
typeof new String('freeCodeCamp');  //'object';
typeof new Object;  //'object'

typeof alert;  //'function'
typeof function () {}; //'function'
typeof (() => {});  //'function' - an arrow function so, parenthesis is required
typeof Math.sqrt;  //'function'

let a;
typeof a;  //'undefined'
typeof b;  //'undefined'
typeof undefined;  //'undefined'

typeof null;  //'object' 
```

下表显示了`typeof`的类型检查值:

| **类型** | **返回值类型为** |
| --- | --- |
| 线 | `'string'` |
| 数字 | `'number'` |
| 比吉斯本 | `'bigint'` |
| 标志 | `'symbol'` |
| 布尔代数学体系的 | `'boolean'` |
| 不明确的 | `'undefined'` |
| 功能对象 | `'function'` |
| 空 | `'object'`(见下文！) |
| 还有其他东西吗 | `'object'` |

# **与`typeof`** 的常见问题

有些情况下,`typeof`操作符可能不会返回您期望的类型。这可能会导致混乱和错误。这里有几个案例。

### **NaN 的类型是一个数字**

```
typeof NaN;  //'number', even if it is Not a Number
```

`typeof NaN`就是`'number'`。这很奇怪，因为我们不应该使用`typeof`来检测`NaN`。有更好的处理方式。我们一会儿就能见到他们。

### **`null`的类型是对象**

```
 typeof null;  //'object' 
```

在 JavaScript 中，`typeof null`是一个给人错误印象的对象，`null`是一个原始值的对象。

`typeof null`的这个结果其实是语言中的 bug。过去曾试图修复它，但由于向后兼容问题而被拒绝。

### **未声明变量的类型未定义**

在 ES6 之前，对未声明变量的类型检查用于产生`'undefined'`。但是这并不是一个处理它的安全的方法。

使用 ES6，我们可以用关键字`let`或`const`声明块范围的变量。如果在它们被初始化之前使用`typeof`操作符，它们将抛出一个`ReferenceError`。

```
 typeof cat; // ReferenceError
 let cat = 'brownie'; 
```

### **构造函数的类型是一个对象**

除了`Function`构造函数之外，所有的构造函数都是`typeof`‘object’。

```
typeof new String('freeCodeCamp'); //'object'
```

这可能会导致一些混淆，因为我们期望它是实际的类型(在上面的例子中，是一个`string`类型)。

### **数组的类型是一个对象**

虽然技术上正确，但这可能是最令人失望的一个。我们希望区分数组和对象，即使在 JavaScript 中数组在技术上是一个对象。

```
typeof Array(4);  //'object'
```

幸运的是，有一些方法可以正确地检测数组。我们很快就会看到这一点。

# **超越`typeof`–更好的类型检查**

现在我们已经看到了`typeof`操作符的一些限制，让我们看看如何修复它们并做更好的类型检查。

### **如何检测南**

在 JavaScript 中，NaN 是一个特殊的值。值 NaN 表示实际上无法表示的算术表达式的结果。举个例子，

```
let result = 0/0;
console.log(result);  // returns, NaN 
```

同样，如果我们用`NaN`执行任何算术运算，它将总是产生一个`NaN`。

```
console.log(NaN + 3); // returns, NaN 
```

使用`typeof`操作符对 NaN 进行类型检查没有多大帮助，因为它将类型作为`'number'`返回。JavaScript 有一个名为`isNaN()`的全局函数来检测结果是否为 NaN。

```
isNaN(0/0); // returns, true 
```

但是这里也有一个问题。

```
isNaN(undefined); // returns true for 'undefined' 
```

在 ES6 中，方法`isNaN()`被添加到全局`Number`对象中。这种方法更可靠，因此是首选方法。

```
Number.isNaN(0/0); // returns, true
Number.isNaN(undefined); // returns, false 
```

`NaN`的另一个有趣的方面是，它是唯一一个从不等于任何其他值(包括它自己)的 JavaScript 值。因此，这是在不支持 ES6 的环境中检测 NaN 的另一种方法:

```
function isNaN (input) {
  return input !== input;
} 
```

### **如何检测 JavaScript 中的空值**

我们已经看到，使用`typeof`操作符检测 null 是令人困惑的。检查某个值是否为空的首选方法是使用严格的等式运算符(`===`)。

```
function isNull(input) {
 return input === null;
} 
```

确保不要误使用`==`。用`==`代替`===`会导致错误的类型检测。

### **如何在 JavaScript 中检测数组**

从 ES6 开始，我们可以使用`Array.isArray`方法检测数组。

```
Array.isArray([]); // returns true
Array.isArray({}); // returns false 
```

在 ES6 之前，我们可以使用`instanceof`操作符来确定数组:

```
function isArray(input) {
  return input instanceof Array;
} 
```

# JavaScript 中类型检查的通用解决方案

有一种方法可以创建一个通用的类型检查解决方案。看一下方法，`Object.prototype.toString`。这对于编写类型检查的实用方法来说是非常强大和非常有用的。

当使用`call()`或`apply()`调用`Object.prototype.toString`时，它以`[object Type]`的格式返回对象类型。返回值中的`Type`部分是实际类型。

让我们通过一些例子来看看它是如何工作的:

```
// returns '[object Array]'
Object.prototype.toString.call([]); 

// returns '[object Date]'
Object.prototype.toString.call(new Date()); 

// returns '[object String]'
Object.prototype.toString.call(new String('freeCodeCamp'));

// returns '[object Boolean]'
Object.prototype.toString.call(new Boolean(true));

// returns '[object Null]'
Object.prototype.toString.call(null); 
```

这意味着，如果我们只取返回字符串，去掉`Type`部分，我们将得到实际的类型。下面是一个尝试:

```
function typeCheck(value) {
  const return_value = Object.prototype.toString.call(value);
  // we can also use regex to do this...
  const type = return_value.substring(
           return_value.indexOf(" ") + 1, 
           return_value.indexOf("]"));

  return type.toLowerCase();
} 
```

现在，我们可以使用`typeCheck`函数来检测类型:

```
typeCheck([]); // 'array'
typeCheck(new Date()); // 'date'
typeCheck(new String('freeCodeCamp')); // 'string'
typeCheck(new Boolean(true)); // 'boolean'
typeCheck(null); // 'null' 
```

# **总之**

总结一下我们在本文中学到的内容:

*   JavaScript 类型检查不像其他编程语言那样严格。
*   使用`typeof`运算符检测类型。
*   `typeof`操作符语法有两种变体:`typeof`和`typeof(expression)`。
*   一个`typeof`操作符的结果有时可能会误导人。在那些情况下，我们需要依靠其他可用的方法(`Number.isNaN`、`Array.isArry`等等)。
*   我们可以使用`Object.prototype.toString`创建一个通用类型检测方法。

# 在我们结束之前...

谢谢你读到这里！我们来连线。可以在 [Twitter (@tapasadhikary)](https://twitter.com/tapasadhikary) 上@我，有评论。

您可能也会喜欢这些其他文章:

*   [JavaScript 未定义和 null:最后说一次吧！](https://blog.greenroots.info/javascript-undefined-and-null-lets-talk-about-it-one-last-time-ckh64kmz807v848s15kdkg3dd)
*   [JavaScript:与==，===和 Object.is 的相等比较](https://blog.greenroots.info/javascript-equality-comparison-with-and-objectis-ckdpt2ryk01vel9s186ft8cwl)
*   [为 JS 初学者讲解的 JavaScript `this` Keyword + 5 键绑定规则](https://www.freecodecamp.org/news/javascript-this-keyword-binding-rules/)

目前就这些。很快在我的下一篇文章中再见。在那之前，请好好照顾自己。