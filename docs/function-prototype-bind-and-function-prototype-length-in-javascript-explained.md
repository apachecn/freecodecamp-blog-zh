# 解释了 JavaScript 中的 function.prototype.bind 和 function.prototype.length

> 原文：<https://www.freecodecamp.org/news/function-prototype-bind-and-function-prototype-length-in-javascript-explained/>

## 函数绑定

`bind`是 JavaScript 中所有函数原型上的方法。它允许您从现有函数创建一个新函数，更改新函数的`this`上下文，并提供您希望调用新函数时使用的任何参数。当新函数被调用时，提供给`bind`的参数将在传递给它的任何参数之前。

### **使用`bind`改变函数**中的`this`

提供给`bind`的第一个参数是函数将绑定到的`this`上下文。如果不想改变`this`的值，将`null`作为第一个参数传递。

您的任务是编写代码，在与会者到达会议时更新他们的人数。您创建了一个简单的网页，该网页有一个按钮，单击该按钮会增加会议对象的`numOfAttendees`属性。您使用 jQuery 向按钮添加了一个 click 处理程序，但是在单击按钮之后，confrence 对象并没有改变。您的代码可能如下所示。

```
var nodevember = {
  numOfAttendees: 0,
  incrementNumOfAttendees: function() {
    this.numOfAttendees++;
  }
  // other properties
};

$('.add-attendee-btn').on('click', nodevember.incrementNumOfAttendees);
```

这是使用 jQuery 和 JavaScript 时的一个常见问题。当您单击按钮时，您传递给 jQuery 的`on`方法中的`this`关键字引用按钮，而不是会议对象。你可以结合你的方法的`this`上下文来解决问题。

```
var nodevember = {
  numOfAttendees: 0,
  incrementNumOfAttendees: function() {
    this.numOfAttendees++;
  }
  // other properties
};

$('.add-attendee-btn').on('click', nodevember.incrementNumOfAttendees.bind(nodevember));
```

现在，当点击按钮时，`this`引用了`nodevember`对象。

### **用`bind`** 为函数提供参数

当调用函数时，在第一个参数之后传递给`bind`的每个参数将在任何传递的参数之前。这允许您将参数预先应用到函数中。在下面的例子中，`combineStrings`获取两个字符串并将它们连接在一起。然后使用`bind`创建一个函数，该函数总是将“Cool”作为第一个字符串。

```
function combineStrings(str1, str2) {
  return str1 + " " + str2
}

var makeCool = combineStrings.bind(null, "Cool");

makeCool("trick"); // "Cool trick"
```

关于[这个引用](https://guide.freecodecamp.org/javascript/this-reference)的指南有更多关于`this`关键词引用如何改变的信息。

关于`bind`方法的更多细节可以在 Mozilla 的 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)中找到。

## **功能长度**

function 对象上的`length`属性保存函数被调用时所期望的参数个数。

```
function noArgs() { }

function oneArg(a) { }

console.log(noArgs.length); // 0

console.log(oneArg.length); // 1
```

### **ES2015 语法**

ES2015(通常称为 ES6)引入了 rest 运算符和默认函数参数。这两项添加都改变了`length`属性的工作方式。

如果在函数声明中使用了 rest 操作符或缺省参数,`length`属性将只包含 rest 操作符或缺省参数之前的参数个数。

```
function withRest(...args) { }

function withArgsAndRest(a, b, ...args) { }

function withDefaults(a, b = 'I am the default') { }

console.log(withRest.length); // 0

console.log(withArgsAndRest.length); // 2

console.log(withDefaults.length); // 1
```

关于`Function.length`的更多信息可以在 [Mozilla 的 MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length)中找到。