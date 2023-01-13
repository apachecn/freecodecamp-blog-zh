# 如何修复类型错误:无法读取 JavaScript 中未定义的属性“push”

> 原文：<https://www.freecodecamp.org/news/fix-typeerror-cannot-read-property-push-of-undefined-in-javascript/>

当使用 JavaScript 数组时，你必须小心不要在一个应该是数组但值为`undefined`的变量上调用`push()`、`pop()`、`shift()`、`unShift()`或`splice()`方法。

如果您错误地这样做，您将得到以下错误:

![s_E70A1833F8285C7F5412DCD9531F13EA1E92CB215392E2D976AD0B6D0DFB9AA0_1665231049694_image](img/4c8d1cc637833d4f9cc0ec6ab9db0b0f.png)

如果你调用`pop()`或任何其他方法而不是 push(如上例所示)，上面的错误将改为‘pop’(或你正在使用的其他方法)。这意味着您将在本文中学习的方法将适用于所有方法。

为了让您正确理解本文和此错误，有必要强调可能触发此问题的各种原因:

*   您对先前设置为`undefined`的变量调用该方法。
*   在用数组初始化变量之前，对变量调用方法。
*   您在数组元素上调用方法，而不是数组本身。
*   您对不存在或具有`undefined`值的对象属性调用该方法。

记住这个方法可以是`push()`、`pop()`、`shift()`、`unShift()`或者`splice()`。现在让我们分析每个场景，并学习如何修复错误。

## 对先前设置为`undefined`的变量调用该方法

当处理变量和数据类型(如字符串)时，我们倾向于在传入原始值之前分配变量值，如`undefined`和`null`。有时我们在调用函数或处理某些动作时会这样做。

对于数组来说，情况并非如此，否则，您将得到错误消息:

```
let myArray = undefined;

myArray.push("John Doe"); // Uncaught TypeError: Cannot read properties of undefined (reading 'push')

console.log(myArray); 
```

要解决这个问题，您必须在数组方法(如`push()`、`pop()`)和其他方法可以处理它之前声明变量是一个数组:

```
let myArray = [];

myArray.push("John Doe");

console.log(myArray); // ["John Doe"] 
```

**注意:**当一个变量被声明时，它不会被识别为一个数组变量，直到使用`Array`构造函数或者使用数组文字符号(`[]`)对它进行初始化。

## 在用数组初始化变量之前，对变量调用方法

正如你刚刚在上面学到的，你可以声明变量的另一种方法是创建它们而不用给它们赋值。

```
let myArray;

myArray.push("John Doe"); // Uncaught TypeError: Cannot read properties of undefined (reading 'push')

console.log(myArray); 
```

这适用于字符串、数字等数据类型，但不适用于数组。必须用`Array`构造函数或数组文字符号(`[]`)初始化数组。

```
let myArray = [];

// Or

let myArray = new Array(); 
```

我们的代码现在将如下所示:

```
let myArray = [];

myArray.push("John Doe");

console.log(myArray); // ["John Doe"] 
```

## 对数组元素而不是数组本身调用方法

数组方法应该在数组本身(意味着数组或用于存储数组的变量)上调用，而不是在数组元素上调用。

```
// Example of arrays
let myArray = [12, 13, 17];
let myArray2 = [];
let myArray3 = new Array();

// Example of array elements
myArray[0];
myArray[1];
myArray[2]; 
```

您可能希望将一个元素推到数组的特定位置，并认为将`push()`或`unShift()`方法直接附加到该元素会解决这个问题。不幸的是，您将得到“无法读取未定义的属性‘push’”错误:

```
let myArray = [12, 13, 17];

myArray[3].push(15); // Uncaught TypeError: Cannot read properties of undefined (reading 'push')

console.log(myArray); 
```

要解决这个问题，您必须对变量本身调用 push 方法，而不是对其元素调用:

```
let myArray = [12, 13, 17];

myArray.push(15);

console.log(myArray); // [12,13,17,15] 
```

## 对不存在或具有`undefined`值的对象属性调用该方法

最后一种情况是，当您试图对一个不存在或其值被设置为`undefined`的对象属性调用该方法时:

```
const user = { name: 'John Doe', scores: undefined };
const user2 = { name: 'John Doe' };

user.scores.push(50);
user2.scores.push(50); 
// Uncaught TypeError: Cannot read properties of undefined (reading 'push') 
```

在上面的场景中，有两个对象:第一个对象有一个键值对`scores`，它的值被设置为`undefined`，但是它意味着接收数组值。而对于第二个对象来说，`scores`并不存在。这两种情况都可能导致错误。

要修复它，您所要做的就是**初始化**键，这样它就可以使用数组文字来期望数组值:

```
const user = { name: "John Doe", scores: [] };

user.scores.push(50);

console.log(user); 
```

## 包扎

在本文中，您已经学习了如何修复“无法读取未定义的属性”错误，当您将这些数组方法附加到未声明或初始化为变量的变量时，会出现这种错误。

祝编码愉快！