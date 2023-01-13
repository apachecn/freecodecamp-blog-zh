# JavaScript 检查是否未定义——如何测试 JS 中的未定义

> 原文：<https://www.freecodecamp.org/news/javascript-check-if-undefined-how-to-test-for-undefined-in-js/>

在 JavaScript 中，未定义的变量或任何没有值的东西总是返回“undefined”。这与 null 不同，尽管两者都意味着空状态。

你通常会在声明变量后给它赋值，但并不总是这样。

当一个变量被声明或初始化但没有赋值时，JavaScript 自动显示“未定义”。看起来是这样的:

```
let myStr;

console.log(myStr); // undefined 
```

同样，当你试图访问一个不存在的数组或对象中的值时，它会抛出`undefined`。

```
let user = {
    name: "John Doe",
    age: 14
};

console.log(user.hobby); // undefined 
```

这是另一个例子:

```
let myArr = [12, 33, 44];

console.log(myArr[7]); // undefined 
```

在本文中，您将学习各种方法和途径来判断 JavaScript 中的变量是否为`undefined`。如果您希望在使用未定义的变量执行操作时避免代码抛出错误，这是必要的。

如果你很急，这里有三个标准方法可以帮助你检查一个变量是否是 JavaScript 中的`undefined`:

```
if(myStr === undefined){}
if(typeof myArr[7] === "undefined"){}
if(user.hobby === void 0){} 
```

现在让我们更详细地解释这些方法。

## 如何用直接比较来检查一个变量在 JavaScript 中是否未定义

首先想到的方法之一是直接比较。这是您比较输出以查看它是否返回`undefined`的地方。您可以通过以下方式轻松做到这一点:

```
let user = {
    name: "John Doe",
    age: 14
};

if (user.hobby === undefined) {
    console.log("This is undefined");
} 
```

这也适用于数组，如下所示:

```
let scores = [12, 34, 66, 78];

if (scores[10] === undefined) {
    console.log("This is undefined");
} 
```

它肯定也适用于其他变量:

```
let name;

if (name === undefined) {
    console.log("This is undefined");
} 
```

## 如何用`typeof`检查一个变量在 JavaScript 中是否未定义

我们也可以使用变量的类型来检查它是否是`undefined`。幸运的是，undefined 是未定义值的数据类型，如下所示:‌

```
let name;

console.log(typeof name); // "undefined" 
```

这样，我们现在可以使用数据类型来检查所有类型的数据的未定义，就像我们上面看到的那样。下面是我们考虑的所有三种情况下的检查结果:

```
if(typeof user.hobby === "undefined"){}
if(typeof scores[10] === "undefined"){}
if(typeof name === "undefined"){} 
```

## 如何用`Void`操作符检查一个变量在 JavaScript 中是否未定义

`void`操作符通常用于获得`undefined`原始值。您可以使用类似于“T3”的“T2”来完成此操作，如下所示:

```
console.log(void 0); // undefined
console.log(void(0)); // undefined 
```

在实际意义上，这类似于直接比较(我们之前看到过)。但是我们会用`void(0)`或`void 0`替换 undefined，如下所示:

```
if(typeof user.hobby === void 0){}
if(typeof scores[10] === void 0){}
if(typeof name === void 0){} 
```

或者像这样:

```
if(typeof user.hobby === void(0)){}
if(typeof scores[10] === void(0)){}
if(typeof name === void(0)){} 
```

## 结论

在本文中，我们学习了如何检查变量是否未定义，以及是什么导致变量未定义。

我们还学习了三种方法，可以用来检查变量是否未定义。所有方法都非常有效。选择你喜欢的方法完全取决于你自己。

祝编码愉快！