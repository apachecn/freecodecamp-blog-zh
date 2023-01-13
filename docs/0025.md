# JavaScript 类型检查——如何用 typeof()检查 JS 中的类型

> 原文：<https://www.freecodecamp.org/news/javascript-type-checking-how-to-check-type-in-js-with-typeof/>

JavaScript 是一种动态类型(或松散类型)的编程语言。它允许您在不指定或定义变量类型的情况下声明变量。

您可以在 JavaScript 中创建变量，而无需定义可以存储在变量中的值的类型。这可能会影响您的程序，并在运行时导致错误，因为类型可能会改变。

例如，可以声明一个变量并给它赋一个数字。但是当您编写更多的代码时，值可能会放错位置，您可能会将同一个变量赋值为 string 或 boolean。这会影响您的代码运行:

```
let myVariable = 45; // => number
myVariable = 'John Doe'; // => string
myVariable = false; // => boolean 
```

从上面的例子可以看出，JavaScript 中的变量可以在程序执行过程中改变类型。作为程序员，这可能很难掌握。这是 TypeScript 被认为是 JavaScript 的超集的原因之一。

要在 JavaScript 中通过检查变量的类型来验证变量，可以使用`typeof`操作符。对于非原始数据类型和特定值，JavaScript 中的类型检查并不简单。这就是为什么类型检查会变得很烦人，尤其是对于没有经验的 JS 开发人员。

在本文中，您将学习如何使用`typeof`操作符，不应该使用`typeof`的情况，以及在 JavaScript 中检查这种情况下的类型的最佳方法。

## JavaScript 数据类型

在 JavaScript 中，数据类型分为两组:原始数据类型和非原始数据类型。除了 object 这种非基本数据类型之外，所有其他数据类型都是基本的。

这些数据类型包括:

1.  线
2.  数字
3.  布尔型(真和假)
4.  空
5.  不明确的
6.  标志

此时，您可能认为我省略了数组和函数。但是不，我没有。这是因为它们都是对象。

## 如何在 JavaScript 中用`typeof`操作符检查类型

`typeof`操作符接受单个操作数(一元操作符)并决定操作数的类型。

有两种方法可以使用`typeof`操作符。您可以计算单个值或表达式:

```
typeof(expression);

// Or

typeof value; 
```

`typeof`操作符将以字符串的形式返回类型，意思是“数字”、“字符串”、“布尔”等等。

```
let myVariable = 45;
console.log(typeof myVariable); // returns "number"
console.log(typeof(myVariable)); // returns "number"

console.log(typeof 45); // returns "number"
console.log(typeof(45)); // returns "number" 
```

重要的是要知道，在计算一个[表达式](https://flaviocopes.com/javascript-expressions/)而不是单个值时，应该总是使用表达式方法(以函数的形式)。例如:

```
console.log(typeof(typeof 45)); // returns "string" 
```

上面返回一个字符串是因为`typeof 45`的输出被求值为“number”(作为字符串返回)，那么`typeof("number")`的输出被求值为“string”。

另一个例子是，如果您的号码中有连字符:

```
// Using expression
console.log(typeof(123-4567-890)); // returns "number"

// Using single value
console.log(typeof 123-4567-890); // returns NaN 
```

单值方法将返回`NaN`(不是数字)，因为它将首先计算`typeof 123`，后者将返回一个字符串，“数字”。这意味着你现在只剩下`"number" - 4567-890`，它不能被减去，将返回`NaN`。

### 如何检查数字数据类型

现在让我们来探索返回数字数据类型的可能实例。

JavaScript 认为数字有不同的可能值，如正整数、负整数、零、浮点数和无穷大:

```
console.log(typeof 33); // returns "number"
console.log(typeof -23); // returns "number"
console.log(typeof 0); // returns "number"
console.log(typeof 1.2345); // returns "number"
console.log(typeof Infinity); // returns "number" 
```

同样重要的是要知道，像 NaN 这样的值，即使它意味着不是一个数字，也总是返回一个“数字”类型。此外，数学函数的数据类型为数字:

```
console.log(typeof NaN); // returns "number"
console.log(typeof Math.LOG2E); // returns "number" 
```

最后，当您使用`Number()`构造函数显式地将一个包含一个数字的字符串类型转换为一个数字，或者甚至是一个不能类型转换为整数的实际字符串的值时，它将总是返回一个数字作为其数据类型:

```
// Typecasting value to number
console.log(typeof Number(`123`)); // returns "number"

// Value cannot be typecasted to integer
console.log(typeof Number(`freeCodeCamp`)); // returns "number" 
```

最后，当您使用像 parseInt()和 parseFloat()这样的方法将一个字符串转换成一个数字并对数字进行舍入时，它的数据类型将是 number:

```
console.log(typeof parseInt(`123`)); // returns "number"
console.log(typeof parseFloat(`123.456`)); // returns "number" 
```

### 如何检查字符串数据类型

只有少数情况会返回“字符串”。这些实例是空字符串、字符串(也可以是数字)和多个单词:

```
console.log(typeof ''); // returns "string"
console.log(typeof 'freeCodeCamp'); // returns "string"
console.log(typeof 'freeCodeCamp offers the best free resources'); // returns "string"
console.log(typeof '123'); // returns "string" 
```

此外，当您使用带有任意值的`String()`构造函数时:

```
console.log(typeof String(123)); // returns "string" 
```

### 如何检查布尔数据类型

当您检查`true`和`false`值时，它将总是返回“布尔”类型。同样，当你检查任何使用`Boolean()`构造函数的东西时:

```
console.log(typeof true); // returns "boolean"
console.log(typeof false); // returns "boolean"
console.log(typeof Boolean(0)); // returns "boolean" 
```

此外，当您使用 double not 运算符(`!!`)时，它的工作方式类似于`Boolean()`构造函数，将返回“boolean ”:

```
console.log(typeof !!(0)); // returns "boolean" 
```

### 如何检查符号数据类型

当使用`Symbol()`构造函数时，即使没有传递任何值，也将返回“符号”数据类型。此外，当您传入一个参数或使用`Symbol.iterator`符号时，它指定了对象的默认迭代器:

```
console.log(typeof Symbol()); // returns "symbol"
console.log(typeof Symbol('parameter')); // returns "symbol"
console.log(typeof Symbol.iterator); // returns "symbol" 
```

### 如何检查未定义的数据类型

当你在没有初始化一个值的情况下声明一个变量时，这个变量被称为`undefined`。当您检查 undefined、没有值的已声明变量(undefined)以及未定义变量时，它们将始终返回“undefined”:

```
// Using the undefined keyword
console.log(typeof undefined); // returns "undefined"

//variable is declared but undefined (has no value intentionally)
let a;
console.log(typeof a); // returns "undefined"

// Using undefined variable
console.log(typeof v); // returns "undefined" 
```

到目前为止，您已经学习了如何检查除 null 之外的所有原始数据类型。这有点棘手，我在关于 JavaScript 中的空检查的文章中详细介绍了这一点。

但是我将在本文中简要回顾一下如何检查`null`,这样您就可以理解基本知识了。

### 如何检查对象数据类型

某些实例总是会返回“object ”,虽然`null`的返回是一个[无法修复的历史 bug](https://www.turbinelabs.com/blog/the-odd-history-of-javascripts-null) ,而函数有其技术原因。

```
console.log(typeof null);
console.log(typeof [1, 2, 3, "freeCodeCamp"]);
console.log(typeof { age: 12, name: "John Doe" });
console.log(typeof [1, 2, 3, 4, 5, 6]); 
```

正如你在上面的例子中看到的，当你使用`typeof`操作时，一个数组将总是返回“object”。这可能不太令人愉快，但从技术上讲，数组是一种特殊类型的对象:

```
console.log(typeof [1, 2, 3, 'freeCodeCamp']); 
```

在 ES6 中，引入了`Array.isArray`方法，这使得您可以轻松地检测数组:

```
console.log(Array.isArray([1, 2, 3, "freeCodeCamp"])); // returns true
console.log(Array.isArray({ age: 12, name: "John Doe" })); // returns false 
```

此外，在引入 ES6 之前，`instanceof`操作符用于检测数组:

```
const isArray = (input) => {
    return input instanceof Array;
};

console.log(isArray([1, 2, 3, 'freeCodeCamp'])); // returns true 
```

### 如何检查空数据类型

当您使用`typeof`操作符检查`null`值时，它返回“object ”,因为一个[历史错误](https://www.turbinelabs.com/blog/the-odd-history-of-javascripts-null)无法修复。

**注意:**不要混淆空和未定义。如果一个变量有意包含`null`的值，则该变量被称为`null`。相反，当你声明一个变量而没有初始化一个值时，它就是`undefined`。

检测`null`的一个非常直接的方法是使用严格比较:

```
const isNull = (input) => {
    return input === null;
}

let myVar = null;
console.log(isNull(myVar)); // returns true 
```

你可以阅读这篇关于 JavaScript 解释的空检查的文章，获得更多选项和详细解释。

## JavaScript 中类型检查的通用解决方案

在由 [Tapas Adhikary](https://www.freecodecamp.org/news/author/tapas/) 撰写的关于[如何检查 JS](https://www.freecodecamp.org/news/javascript-typeof-how-to-check-the-type-of-a-variable-or-object-in-js/) 中的变量或对象的类型的早期文章中，他添加并解释了一个通用的解决方案，您可以使用它来更准确地检查类型:

```
const typeCheck = (value) => {
    const return_value = Object.prototype.toString.call(value);
    const type = return_value.substring(
    return_value.indexOf(" ") + 1,
    return_value.indexOf("]")
    );

    return type.toLowerCase();
}; 
```

让我们来测试一下:

```
console.log(typeCheck([])); // returns 'array'
console.log(typeCheck(new Date())); // returns 'date'
console.log(typeCheck(new String("freeCodeCamp"))); // returns 'string'
console.log(typeCheck(new Boolean(true))); // returns 'boolean'
console.log(typeCheck(null)); // returns 'null' 
```

## 结束了！

在本文中，您已经学习了如何用`typeof`操作符检查 JavaScript 中的类型。

您还了解了局限性以及如何使用其他方法来克服这些局限性。记住，对于大多数原始数据类型，您总是可以使用`typeof`操作符。

祝编码愉快！