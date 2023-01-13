# JS 复制一个对象——如何在 JavaScript 中克隆一个对象

> 原文：<https://www.freecodecamp.org/news/clone-an-object-in-javascript/>

JavaScript 对象是键值对的集合。它是非原始数据类型，可以包含各种数据类型。例如:

```
const userDetails = {
  name: "John Doe",
  age: 14,
  verified: false
}; 
```

在 JavaScript 中处理对象时，有时可能需要更改对象的值或为其添加新属性。

在某些情况下，在更新或添加新属性之前，您会希望创建一个新对象，并复制或克隆原始对象的值。

例如，如果您想要复制`userDetails`对象的值，然后将名称更改为不同的名称。在一般情况下，您会希望使用等号(=)运算符。

```
const newUser = userDetails;
console.log(newUser); // {name: 'John Doe', age: 14, verified: false} 
```

一切似乎都很好，但是让我们看看如果编辑第二个对象会发生什么:

```
const newUser = userDetails;
newUser.name = "Jane Doe";

console.log(newUser); // {name: 'Jane Doe', age: 14, verified: false} 
```

新对象一切正常，但是如果您尝试检查原始对象的值，您会注意到它受到了影响。为什么？怎么会？

```
console.log(userDetails); // {name: 'Jane Doe', age: 14, verified: false} 
```

这就是问题所在。原始对象受到影响，因为对象是**引用类型**。这意味着您存储在克隆对象或原始对象中的任何值都指向同一个对象。

这不是你想要的。您希望将对象的值存储在新对象中，并在不影响原始数组的情况下操作新对象中的值。

在本文中，您将学习三种方法来实现这一点。您还将了解深层克隆和浅层克隆的含义以及它们是如何工作的。

如果你赶时间，这里有三种方法和一个它们如何工作的例子。

```
// Spread Method
let clone = { ...userDetails }

// Object.assign() Method
let clone = Object.assign({}, userDetails)

// JSON.parse() Method
let clone = JSON.parse(JSON.stringify(userDetails)) 
```

如果你不赶时间，我们就开始吧。🚀

## 如何在 JavaScript 中用 Spread 操作符克隆一个对象

扩展运算符是在 ES6 中引入的，可以将值扩展到前面有三个点的对象中。

```
// Declaring Object
const userDetails = {
  name: "John Doe",
  age: 14,
  verified: false
};

// Cloning the Object with Spread Operator
let cloneUser = { ...userDetails };

console.log(cloneUser); // {name: 'John Doe', age: 14, verified: false} 
```

这不再被引用，这意味着更改对象的值不会影响原始对象。

```
// Cloning the Object with Spread Operator
let cloneUser = { ...userDetails };

// changing the value of cloneUser
cloneUser.name = "Jane Doe"

console.log(cloneUser.name); // 'Jane Doe'
console.log(cloneUser); // {name: 'Jane Doe', age: 14, verified: false} 
```

当您检查原始对象或整个对象中的名称值时，您会注意到它不受影响。

```
console.log(userDetails.name); // 'John Doe'
console.log(userDetails); // {name: 'John Doe', age: 14, verified: false} 
```

**注意:**当引用更深层次的对象时，你只能使用 spread 语法来创建一个对象的浅层次副本。到了本文最后一节你就明白了。

## 如何用`Object.assign()`在 JavaScript 中克隆一个对象

spread 运算符的替代方法是`Object.assign()`方法。您可以使用此方法将值和属性从一个或多个源对象复制到目标对象。

```
// Declaring Object
const userDetails = {
  name: "John Doe",
  age: 14,
  verified: false
};

// Cloning the Object with Object.assign() Method
let cloneUser = Object.assign({}, userDetails);

console.log(cloneUser); // {name: 'John Doe', age: 14, verified: false} 
```

这不再被引用，这意味着更改对象的值不会影响原始对象。

```
// Cloning the Object with Object.assign() Method
let cloneUser = Object.assign({}, userDetails);

// changing the value of cloneUser
cloneUser.name = "Jane Doe"

console.log(cloneUser.name); // 'Jane Doe'
console.log(cloneUser); // {name: 'Jane Doe', age: 14, verified: false} 
```

当您检查原始对象或整个对象中的名称值时，您会注意到它不受影响。

```
console.log(userDetails.name); // 'John Doe'
console.log(userDetails); // {name: 'John Doe', age: 14, verified: false} 
```

**注意:**当引用更深层次的对象时，你只能使用`Object.assign()`方法来制作一个对象的浅层次副本。到了本文最后一节你就明白了。

## 如何用`JSON.parse()`克隆一个对象

最后一个方法是 JSON.parse()。您将在`JSON.stringify()`旁边使用此方法。你可以用它来进行深度克隆，但是它也有一些缺点。首先，我们来看看它是如何工作的。

```
// Declaring Object
const userDetails = {
  name: "John Doe",
  age: 14,
  verified: false
};

// Cloning the Object with JSON.parse() Method
let cloneUser = JSON.parse(JSON.stringify(userDetails));

console.log(cloneUser); // {name: 'John Doe', age: 14, verified: false} 
```

同样，就像前面的方法一样，这里不再引用。这意味着您可以更改新对象中的值，而不会影响原始对象。

```
// Cloning the Object with JSON.parse() Method
let cloneUser = JSON.parse(JSON.stringify(userDetails));

// changing the value of cloneUser
cloneUser.name = "Jane Doe"

console.log(cloneUser.name); // 'Jane Doe'
console.log(cloneUser); // {name: 'Jane Doe', age: 14, verified: false} 
```

当您检查原始对象或整个对象中的名称值时，您会注意到它不受影响。

```
console.log(userDetails.name); // 'John Doe'
console.log(userDetails); // {name: 'John Doe', age: 14, verified: false} 
```

**注意:**这种方法可用于深度克隆，但不是最佳选择，因为它不支持`function`或`symbol`属性。

现在让我们探索一下浅层克隆和深层克隆，以及如何使用`JSON.parse()`方法来执行深层克隆。您还将了解为什么它不是最佳选择。

## 浅层克隆与深层克隆

到目前为止，本文中使用的示例是只有一个级别的基本对象。这意味着我们只执行了浅层克隆。但是，当一个对象有多个级别时，您将需要执行深度克隆。

```
// Shallow object
const userDetails = {
  name: "John Doe",
  age: 14,
  verified: false
};

// Deep object
const userDetails = {
  name: "John Doe",
  age: 14,
  status: {
    verified: false,
  }
}; 
```

请注意，深度对象有多个级别，因为在`userDetails`对象中有另一个对象。深度对象可以有任意多的级别。

**注意:**当你使用扩展操作符或`Object.assign()`方法克隆一个深度对象时，更深的对象将被引用。

```
const userDetails = {
  name: "John Doe",
  age: 14,
  status: {
    verified: false
  }
};

// Cloning the Object with Spread Operator
let cloneUser = { ...userDetails };

// Changing the value of cloneUser
cloneUser.status.verified = true;

console.log(cloneUser); // {name: 'John Doe', age: 14, status: {verified: true}}
console.log(userDetails); // {name: 'John Doe', age: 14, status: {verified: true}} 
```

你会注意到原始对象和新对象都受到影响，因为当你使用扩展操作符或`Object.assign()`方法克隆一个深度对象时，更深的对象将被引用。

### 您如何解决这个问题

你可以用`JSON.parse()`的方法，一切都会好的。

```
const userDetails = {
  name: "John Doe",
  age: 14,
  status: {
    verified: false
  }
};

// Cloning the Object with Spread Operator
let cloneUser = JSON.parse(JSON.stringify(userDetails));

// Changing the value of cloneUser
cloneUser.status.verified = true;

console.log(cloneUser); // {name: 'John Doe', age: 14, status: {verified: true}}
console.log(userDetails); // {name: 'John Doe', age: 14, status: {verified: false}} 
```

但是这种方法有一个问题。问题是你可能会丢失数据。怎么会？

对于像数字、字符串或布尔这样的原始数据类型非常有效，这就是你在前面的例子中看到的。但是有时候，如果你不知道一些值以及它是如何处理它们的，那么`JSON.stringify()`是不可预测的。

例如，它不适用于函数、符号或`undefined`值。它还将其他值如`Nan`和`Infinity`更改为`null`，破坏了您的代码。当你有一个函数、符号或未定义的值时，它将返回一个空的键值对并跳过它。

```
const userDetails = {
  name: "John Doe",
  age: 14,
  status: {
    verified: false,
    method: Symbol(),
    title: undefined
  }
};

// Cloning the Object with Spread Operator
let cloneUser = JSON.parse(JSON.stringify(userDetails)); 
```

一切似乎都很好，但是对于新对象，`JSON.stringify()`不会为未定义的值和符号值返回键值对。

```
console.log(cloneUser); 

// Output
{
  name: "John Doe",
  age: 14,
  status: {
    verified: false
  }
}; 
```

这意味着你需要小心。实现深度克隆的最佳选择是使用 [Lodash](https://lodash.com/docs/#cloneDeep) 。然后，您可以确保您的数据不会丢失。

```
const userDetails = {
  name: "John Doe",
  age: 14,
  status: {
    verified: false,
    method: Symbol(),
    title: undefined
  }
};

console.log(_.cloneDeep(userDetails)); 
```

## 结束了！

本文教您如何使用三种主要方法在 JavaScript 中克隆一个对象。您已经看到了这些方法是如何工作的，以及何时使用每种方法。您还了解了深度克隆。

你可以阅读[这篇文章](https://medium.com/@pmzubar/why-json-parse-json-stringify-is-a-bad-practice-to-clone-an-object-in-javascript-b28ac5e36521)来理解为什么`JSON.parse(JSON.stringify())`在 JavaScript 中克隆一个对象是一个坏习惯。

祝编码愉快！