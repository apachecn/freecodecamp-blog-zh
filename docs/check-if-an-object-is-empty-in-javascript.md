# 如何在 JavaScript 中检查对象是否为空

> 原文：<https://www.freecodecamp.org/news/check-if-an-object-is-empty-in-javascript/>

对象是编程中最常用的数据类型之一。对象是存储为键值对的相关数据的集合。例如:

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14,
} 
```

使用对象时，您可能需要在执行函数之前检查对象是否为空。

在 JavaScript 中，有多种方法可以检查对象是否为空。在本文中，您将了解到实现这一点的各种方法、可以附加的选项以及原因。

**注意:**当一个对象没有键值对时，它被认为是空的。

如果你很着急，这里有一个基本的例子:

```
const myEmptyObj = {};

// Works best with new browsers
Object.keys(myEmptyObj).length === 0 && myEmptyObj.constructor === Object

// Works with all browsers
_.isEmpty(myEmptyObj) 
```

这两个方法都将返回`true`。现在让我们来理解这些以及更多可以用来在 JavaScript 中检查对象是否为空的选项。

## 如何用`Object.keys()`检查对象是否为空

`Object.keys()`方法是 ECMAScript6 (ES6)中引入的静态对象方法，所有现代浏览器都支持。此方法返回一个包含对象键的数组。例如:

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

console.log(Object.keys(userDetails)); // ["name","username","age"] 
```

这样，您现在可以应用`.length`属性了。如果返回零(0)，则对象为空。

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};

console.log(Object.keys(userDetails).length); // 3
console.log(Object.keys(myEmptyObj).length); // 0 
```

现在，您可以使用此方法通过 if 语句检查对象是否为空，或者创建一个检查对象的函数。

```
const isObjectEmpty = (objectName) => {
  return Object.keys(objectName).length === 0
} 
```

这将返回`true`或`false`。如果对象为空，则返回`true`，否则返回`false`。

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};

const isObjectEmpty = (objectName) => {
  return Object.keys(objectName).length === 0
}

console.log(isObjectEmpty(userDetails)); // false
console.log(isObjectEmpty(myEmptyObj)); // true 
```

**注意:**当检查一个对象是否为空或任何数据类型时，只检查长度不是最好的选择。最好是确认数据类型是否正确。

为此，您可以使用构造函数检查:

```
const isObjectEmpty = (objectName) => {
  return Object.keys(objectName).length === 0 && objectName.constructor === Object;
} 
```

这样，你可能会得到更彻底的检查。

到目前为止，一切都运行良好。但是，当变量是`undefined`或者传递的是`null`而不是`{}`时，您可能也想避免抛出`TypeError`。要解决这个问题，您可以添加一个额外的检查:

```
const isObjectEmpty = (objectName) => {
  return (
    objectName &&
    Object.keys(objectName).length === 0 &&
    objectName.constructor === Object
  );
}; 
```

在上面的代码中，添加了一个额外的检查。这意味着如果它不是空对象，它将返回`null`或`undefined`，如下例所示:

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};
let nullObj = null;
let undefinedObj;

const isObjectEmpty = (objectName) => {
  return (
    objectName &&
    Object.keys(objectName).length === 0 &&
    objectName.constructor === Object
  );
};

console.log(isObjectEmpty(userDetails)); // false
console.log(isObjectEmpty(myEmptyObj)); // true
console.log(isObjectEmpty(undefinedObj)); // undefined
console.log(isObjectEmpty(nullObj)); // null 
```

**注意:**这适用于其他对象静态方法，也就是说你可以使用`Object.entries()`或`Object.values()`来代替`Object.keys()`。

## 如何用`for…in`循环检查对象是否为空

您可以使用的另一种方法是 ES6 `for…in`循环。您可以在使用`hasOwnProperty()`方法的同时使用这个循环。

```
const isObjectEmpty = (objectName) => {
  for (let prop in objectName) {
    if (objectName.hasOwnProperty(prop)) {
      return false;
    }
  }
  return true;
}; 
```

上面的方法将遍历每个对象属性。如果找到一个迭代，则该对象不为空。另外，`hasOwnProperty()`将返回一个布尔值，表明对象是否有指定的属性作为它的属性。

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};

const isObjectEmpty = (objectName) => {
  for (let prop in objectName) {
    if (objectName.hasOwnProperty(prop)) {
      return false;
    }
  }
  return true;
};

console.log(isObjectEmpty(userDetails)); // false
console.log(isObjectEmpty(myEmptyObj)); // true 
```

## 如何用`JSON.stringify()`检查对象是否为空

您还可以使用`JSON.stingify()`方法，该方法用于将 JavaScript 值转换为 JSON 字符串。这意味着它将把你的对象值转换成对象的字符串。例如:

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

console.log(JSON.stringify(userDetails)); 

Output:
"{'name':'John Doe','username':'jonnydoe','age':14}" 
```

这意味着当它是一个空对象时，它将返回`"{}"`。您可以利用这一点来检查空对象。

```
const isObjectEmpty = (objectName) => {
  return JSON.stringify(objectName) === "{}";
}; 
```

如果对象为空，这将返回`true`，否则返回`false`:

```
let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};

const isObjectEmpty = (objectName) => {
  return JSON.stringify(objectName) === "{}";
};

console.log(isObjectEmpty(userDetails)); // false
console.log(isObjectEmpty(myEmptyObj)); // true 
```

## 如何用 Lodash 检查一个对象是否为空

最后，我在这里解释的一些方法可能适用于旧版本的浏览器，而其他的可能不适用。如果你关心的是一个既适用于旧版本又适用于新版本浏览器的解决方案，你可以使用 [Lodash](https://lodash.com/) 。

Lodash 是一个现代的 JavaScript 实用程序库，可以用非常基本的语法执行许多 JavaScript 功能。

例如，如果要检查一个对象是否为空，只需使用“is empty”方法。

```
_.isEmpty(objectName); 
```

将 Lodash 安装到您的项目中非常容易。您所要做的就是使用这个命令:

```
$ npm install lodash 
```

现在，您可以初始化下划线方法并使用该方法。

```
const _ = require('lodash');

let userDetails = {
  name: "John Doe",
  username: "jonnydoe",
  age: 14
};

let myEmptyObj = {};

const isObjectEmpty = (objectName) => {
  return _.isEmpty(objectName);
};

console.log(isObjectEmpty(userDetails)); // false
console.log(isObjectEmpty(myEmptyObj)); // true 
```

## 就是这样！💪

我喜欢探索检查对象是否为空的各种方法。请随意使用最适合您的项目或任务的方法。

祝编码愉快！