# JavaScript 数组方法教程——用例子解释最有用的方法

> 原文：<https://www.freecodecamp.org/news/complete-introduction-to-the-most-useful-javascript-array-methods/>

如果您是一名 JavaScript 开发人员，并且希望改进您的编码，那么您应该熟悉最常用的 ES5 和 ES6+数组方法。

这些方法使编码变得更加容易，也使你的代码看起来简洁易懂。

因此，在本文中，我们将探讨一些最流行和最广泛使用的数组方法。所以让我们开始吧。

## Array.forEach 方法

`Array.forEach`方法的语法如下:

```
Array.forEach(callback(currentValue [, index [, array]])[, thisArg]);
```

`forEach`方法为数组中的每个元素执行一次提供的函数。

看看下面的代码:

```
const months = ['January', 'February', 'March', 'April'];

months.forEach(function(month) {
  console.log(month);
});

/* output

January
February
March
April

*/ 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/bGBqzOw?editors=0012)。

这里，在`forEach`循环回调函数内部，数组的每个元素都作为函数的第一个参数自动传递。

上述示例中循环代码的等效代码如下所示:

```
const months = ['January', 'February', 'March', 'April'];

for(let i = 0; i < months.length; i++) {
  console.log(months[i]);
}

/* output

January
February
March
April

*/ 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/abBJXMR?editors=0012)。

你需要记住的是`forEach`方法不返回任何值。

看看下面的代码:

```
const months = ['January', 'February', 'March', 'April'];
const returnedValue = months.forEach(function (month) {
  return month;
});

console.log('returnedValue: ', returnedValue); // undefined 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/PobpxGb?editors=0012)。

> *注意* `*forEach*` *只是用来循环遍历数组，执行一些处理或者日志记录。它不返回任何值，即使你从回调函数显式返回值(这意味着在上面的例子中返回值是* `undefined` *)。*

在上面所有的例子中，我们只使用了回调函数的第一个参数。但是回调函数还接收两个附加参数，它们是:

*   index -当前正在迭代的元素的索引
*   数组-我们正在循环的原始数组

```
const months = ['January', 'February', 'March', 'April'];

months.forEach(function(month, index, array) {
  console.log(month, index, array);
});

/* output

January 0 ["January", "February", "March", "April"]
February 1 ["January", "February", "March", "April"]
March 2 ["January", "February", "March", "April"]
April 3 ["January", "February", "March", "April"]

*/ 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/OJbpqJR?editors=0012)。

根据需求，您可能会发现使用`index`和`array`参数很有用。

### 使用 forEach 代替 for 循环的优点

*   使用一个`forEach`循环使你的代码更短，更容易理解
*   当使用`forEach`循环时，我们不需要跟踪数组中有多少元素可用。因此它避免了创建额外的计数器变量。
*   使用`forEach`循环使得代码易于调试，因为没有额外的变量来循环数组
*   当数组的所有元素都完成迭代时,`forEach`循环自动停止。

### 浏览器支持

*   所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本
*   Microsoft Edge 版本 12 及以上

## Array.map 方法

在所有其他方法中，阵列映射方法是最有用和最广泛使用的阵列方法。

`Array.map`方法的语法如下:

```
Array.map(function callback(currentValue[, index[, array]]) {
    // Return element for new_array
}[, thisArg])
```

`map`方法为数组中的每个元素执行一次提供的函数，它**返回一个新的转换后的数组。**

看看下面的代码:

```
const months = ['January', 'February', 'March', 'April'];
const transformedArray = months.map(function (month) {
  return month.toUpperCase();
});

console.log(transformedArray); // ["JANUARY", "FEBRUARY", "MARCH", "APRIL"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/ExNWOyr?editors=0012)。

在上面的代码中，在回调函数中，我们将每个元素转换成大写并返回。

上述示例中循环代码的等效代码如下所示:

```
const months = ['January', 'February', 'March', 'April'];
const converted = [];

for(let i = 0; i < months.length; i++) {
 converted.push(months[i].toUpperCase());
};

console.log(converted); // ["JANUARY", "FEBRUARY", "MARCH", "APRIL"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/gOLmyQQ?editors=0012)。

使用`map`有助于避免预先创建一个单独的`converted`数组来存储转换后的元素。所以使用数组`map`可以节省内存空间，代码看起来也更整洁，就像这样:

```
const months = ['January', 'February', 'March', 'April'];

console.log(months.map(function (month) {
  return month.toUpperCase();
})); // ["JANUARY", "FEBRUARY", "MARCH", "APRIL"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/oNYZVoX?editors=0012)。

注意，`map`方法返回一个新数组，它的长度与原始数组完全相同。

`forEach`和`map`方法的区别在于`forEach`只用于循环，不返回任何东西。另一方面，`map`方法返回一个与原始数组长度完全相同的新数组。

另外，注意`map`并没有改变原来的数组，而是返回一个新的数组。

看看下面的代码:

```
const users = [
  {
    first_name: 'Mike',
    last_name: 'Sheridan'
  },
  {
    first_name: 'Tim',
    last_name: 'Lee'
  },
  {
    first_name: 'John',
    last_name: 'Carte'
  }
];

const usersList = users.map(function (user) {
  return user.first_name + ' ' + user.last_name;
});

console.log(usersList); // ["Mike Sheridan", "Tim Lee", "John Carte"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/LYbWaxP?editors=0012)。

这里，通过使用对象数组和`map`方法，我们很容易生成一个名和姓连接在一起的数组。

在上面的代码中，我们使用了`+`操作符来连接两个值。但是更常见的是使用 ES6 模板文字语法，如下所示:

```
const users = [
  {
    first_name: 'Mike',
    last_name: 'Sheridan'
  },
  {
    first_name: 'Tim',
    last_name: 'Lee'
  },
  {
    first_name: 'John',
    last_name: 'Carte'
  }
];

const usersList = users.map(function (user) {
  return `${user.first_name} ${user.last_name}`;
});

console.log(usersList); // ["Mike Sheridan", "Tim Lee", "John Carte"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/abBJMqe?editors=0012)。

如果您只想从数组中提取特定的数据，array `map`方法也很有用，如下所示:

```
const users = [
  {
    first_name: 'Mike',
    last_name: 'Sheridan',
    age: 30
  },
  {
    first_name: 'Tim',
    last_name: 'Lee',
    age: 45
  },
  {
    first_name: 'John',
    last_name: 'Carte',
    age: 25
  }
];

const surnames = users.map(function (user) {
  return user.last_name;
});

console.log(surnames); // ["Sheridan", "Lee", "Carte"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/rNWyRdR?editors=0012)。

在上面的代码中，我们只提取每个用户的姓氏，并将它们存储在一个数组中。

我们甚至可以使用`map`来生成一个包含动态内容的数组，如下所示:

```
const users = [
  {
    first_name: 'Mike',
    location: 'London'
  },
  {
    first_name: 'Tim',
    location: 'US'
  },
  {
    first_name: 'John',
    location: 'Australia'
  }
];

const usersList = users.map(function (user) {
  return `${user.first_name} lives in ${user.location}`;
});

console.log(usersList); // ["Mike lives in London", "Tim lives in US", "John lives in Australia"] 
```

这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/ExNWMOY?editors=0012)。

注意，在上面的代码中，我们没有改变原始的`users`数组。我们正在创建一个包含动态内容的新数组，因为`map`总是返回一个新数组。

### 使用映射方法的优势

*   它有助于在不改变原始数组的情况下快速生成新数组
*   它有助于基于每个元素生成具有动态内容的数组
*   它允许我们快速提取数组中的任何元素
*   它生成一个与原始数组长度完全相同的数组

**浏览器支持:**

*   所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本
*   Microsoft Edge 版本 12 及以上

## Array.find 方法

`Array.find`方法的语法如下:

```
Array.find(callback(element[, index[, array]])[, thisArg])
```

> *`*find*`*方法返回数组中满足给定测试条件的* `*first element*` *的* `*value*` *。**

*`find`方法将回调函数作为第一个参数，并对数组的每个元素执行回调函数。每个数组元素值都作为第一个参数传递给回调函数。*

*假设，我们有这样一个员工列表:*

```
*`const employees = [
 { name: "David Carlson", age: 30 },
 { name: "John Cena", age: 34 },
 { name: "Mike Sheridan", age: 25 },
 { name: "John Carte", age: 50 }
];`*
```

*我们想获得名为`John`的雇员的记录。在这种情况下，我们可以使用如下所示的`find`方法:*

```
*`const employee = employees.find(function (employee) {
  return employee.name.indexOf('John') > -1;
});

console.log(employee); // { name: "John Cena", age: 34 }`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/VwmpVmL?editors=0011)。*

*即使列表中有`"John Carte"`，当找到第一个匹配时,`find`方法也会停止。所以它不会返回名为`"John Carte".`的对象*

*上述示例中循环代码的等效代码如下所示:*

```
*`const employees = [
 { name: "David Carlson", age: 30 },
 { name: "John Cena", age: 34 },
 { name: "Mike Sheridan", age: 25 },
 { name: "John Carte", age: 50 }
];

let user;

for(let i = 0; i < employees.length; i++) {
  if(employees[i].name.indexOf('John') > -1) {
    user = employees[i];
    break;
  }
}

console.log(user); // { name: "John Cena", age: 34 }`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/BaQWbeY?editors=0012)。*

*如您所见，使用普通 for 循环会使代码变得更大，更难理解。但是使用`find`方法，我们可以用一种容易理解的方式编写相同的代码。*

### *使用 find 方法的优点*

*   *它允许我们快速找到任何元素，而无需编写大量代码*
*   *它一找到匹配就停止循环，所以不需要额外的 break 语句*

***浏览器支持:***

*   *除 Internet Explorer (IE)之外的所有现代浏览器*
*   *Microsoft Edge 版本 12 及以上*

## *Array.findIndex 方法*

*`Array.findIndex`方法的语法如下:*

```
*`Array.findIndex(callback(element[, index[, array]])[, thisArg])`*
```

*`findIndex`方法返回满足所提供的测试条件的数组**中第一个元素的**索引**。否则返回`-1`，表示没有元素通过测试。***

```
*`const employees = [
  { name: 'David Carlson', age: 30 },
  { name: 'John Cena', age: 34 },
  { name: 'Mike Sheridan', age: 25 },
  { name: 'John Carte', age: 50 }
];

const index = employees.findIndex(function (employee) {
  return employee.name.indexOf('John') > -1;
});

console.log(index); // 1`*
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/JjbWebQ?editors=0012)。*

*这里我们得到的输出是 **1** ，这是第一个名为`John`的对象的索引。请注意，索引从零开始。*

*上述示例中循环代码的等效代码如下所示:*

```
*`const employees = [
  { name: 'David Carlson', age: 30 },
  { name: 'John Cena', age: 34 },
  { name: 'Mike Sheridan', age: 25 },
  { name: 'John Carte', age: 50 }
];

let index = -1;

for(let i = 0; i < employees.length; i++) {
  if(employees[i].name.indexOf('John') > -1) {
    index = i;
    break;
  }
}

console.log(index); // 1`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/oNYZOgY?editors=0012)。*

### *使用 findIndex 方法的优点*

*   *它允许我们快速找到一个元素的索引，而不需要写很多代码*
*   *它一找到匹配就停止循环，所以不需要额外的 break 语句*
*   *我们也可以使用数组`find`方法来查找索引，但是使用`findIndex`会更容易，并且避免创建额外的变量来存储索引*

***浏览器支持:***

*   *除 Internet Explorer (IE)之外的所有现代浏览器*
*   *Microsoft Edge 版本 12 及以上*

## *数组.过滤器方法*

*`Array.filter`方法的语法如下:*

```
*`Array.filter(callback(element[, index[, array]])[, thisArg])`*
```

*`filter`方法返回包含所有满足所提供测试条件的元素的`a new array`。*

*`filter`方法将回调函数作为第一个参数，并对数组的每个元素执行回调函数。每个数组元素值都作为第一个参数传递给回调函数。*

```
*`const employees = [
  { name: 'David Carlson', age: 30 },
  { name: 'John Cena', age: 34 },
  { name: 'Mike Sheridan', age: 25 },
  { name: 'John Carte', age: 50 }
];

const employee = employees.filter(function (employee) {
  return employee.name.indexOf('John') > -1;
});

console.log(employee); // [ { name: "John Cena", age: 34 }, { name: "John Carte", age: 50 }]`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/yLVMQgE?editors=0011)。*

*从上面的代码中可以看出，使用`filter`有助于从数组中找到所有匹配指定测试条件的元素。*

*因此，使用`filter`不会在找到特定匹配时停止，而是继续检查数组中与条件匹配的其他元素。然后它从数组中返回所有匹配的元素。*

> *`find`和`filter`的主要区别在于`find`只返回数组的第一个匹配元素，而使用`filter`则从数组中返回所有匹配元素。*

*注意，`filter`方法总是返回一个数组。如果没有元素通过测试条件，将返回一个空数组。*

*上述示例中循环代码的等效代码如下所示:*

```
*`const employees = [
  { name: 'David Carlson', age: 30 },
  { name: 'John Cena', age: 34 },
  { name: 'Mike Sheridan', age: 25 },
  { name: 'John Carte', age: 50 }
];

let filtered = [];

for(let i = 0; i < employees.length; i++) {
 if(employees[i].name.indexOf('John') > -1) {
   filtered.push(employees[i]);
 }
}

console.log(filtered); // [ { name: "John Cena", age: 34 }, { name: "John Carte", age: 50 }]`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/qBqrwaq?editors=0011)。*

### *使用过滤方法的优点*

*   *它允许我们从数组中快速找到所有匹配的元素*
*   *即使没有匹配，它也总是返回一个数组，因此它避免了编写额外的`if`条件*
*   *它避免了创建额外的变量来存储被过滤的元素的需要*

***浏览器支持:***

*   *所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本*
*   *Microsoft Edge 版本 12 及以上*

## *Array.every 方法*

*`Array.every`方法的语法如下:*

```
*`Array.every(callback(element[, index[, array]])[, thisArg])`*
```

*`every`方法测试数组中的所有元素是否通过了提供的测试条件，并返回一个布尔值`true`或`false`。*

*假设我们有一个数字数组，我们想检查数组中的每个元素是否都是正数。我们可以用`every`的方法来实现。*

```
*`let numbers = [10, -30, 20, 50];

let allPositive = numbers.every(function (number) {
  return number > 0;
});
console.log(allPositive); // false 

numbers = [10, 30, 20, 50];

allPositive = numbers.every(function (number) {
  return number > 0;
});
console.log(allPositive); // true`* 
```

*假设您有一个注册表单，您想在提交表单之前检查是否所有必填字段都已输入。您可以使用`every`方法轻松检查每个字段值。*

```
*`window.onload = function () {
  const form = document.getElementById('registration_form');
  form.addEventListener('submit', function (event) {
    event.preventDefault();
    const fields = ['first_name', 'last_name', 'email', 'city'];
    const allFieldsEntered = fields.every(function (fieldId) {
      return document.getElementById(fieldId).value.trim() !== '';
    });

    if (allFieldsEntered) {
      console.log('All the fields are entered');
      // All the field values are entered, submit the form
    } else {
      alert('Please, fill out all the field values.');
    }
  });
};`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/rNWyQwo?editors=0011)。*

*这里，在`every`方法的回调函数中，我们检查每个字段值是否不为空，并返回一个布尔值。*

*在上面的代码中，如果对于`fields`数组中的所有元素，回调函数返回一个`true`值，则`every`方法返回`true`。*

*如果回调函数为`fields`数组中的任何元素返回一个`false`值，那么`every`方法将返回`false`作为结果。*

### *使用每种方法的优点*

*   *它允许我们快速检查所有的元素是否符合特定的标准，而无需编写大量的代码*

### *浏览器支持:*

*   *所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本*
*   *Microsoft Edge 版本 12 及以上*

## *Array.some 方法*

*`Array.some`方法的语法如下:*

```
 *`Array.some(callback(element[, index[, array]])[, thisArg]`*
```

*`some`方法测试数组中是否至少有一个元素通过了由提供的函数给出的测试条件，并返回一个布尔值`true`或`false`。*

*一旦找到第一个匹配，就返回`true`，如果没有匹配，就返回`false`。*

*假设我们有一个数字数组，我们想检查数组是否包含至少一个正元素。我们可以用`some`的方法来实现。*

```
*`let numbers = [-30, 40, 20, 50];

let containsPositive = numbers.some(function (number) {
  return number > 0;
});
console.log(containsPositive); // true 

numbers = [-10, -30, -20, -50];

containsPositive = numbers.every(function (number) {
  return number > 0;
});
console.log(containsPositive); // false`* 
```

*使用`some`方法有一些有用的场景。*

### *`Some`方法实施例 1:*

*假设我们有一个雇员列表，我们想检查一个特定的雇员是否在这个数组中。如果找到了该雇员，我们还想获得该雇员的索引位置。*

*因此，我们可以使用`some`方法来完成这两项，而不是分别使用`find`和`findIndex`方法。*

```
*`const employees = [
  { name: 'David Carlson', age: 30 },
  { name: 'John Cena', age: 34 },
  { name: 'Mike Sheridon', age: 25 },
  { name: 'John Carte', age: 50 }
];

let indexValue = -1;
const employee = employees.some(function (employee, index) {
  const isFound = employee.name.indexOf('John') > -1;
  if (isFound) {
    indexValue = index;
  }
  return isFound;
});

console.log(employee, indexValue); // true 1`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/ExNWOvz?editors=0011)。*

### *`Some`方法实施例 2:*

*数组`forEach`、`map`和`filter`方法从头到尾运行，直到处理完数组的所有元素。一旦找到一个特定的元素，就没有办法停止或跳出循环。*

*在这种情况下，我们可以使用数组`some`的方法。`map`、`forEach`和`some`方法在回调函数中采用相同的参数:*

*   *第一个参数是实际值*
*   *第二个参数是索引*
*   *第三个参数是原始数组*

*从上面的例子 1 中可以看出，一旦找到一个特定的匹配，`some`方法就停止遍历数组。*

### *使用 some 方法的优点*

*   *它允许我们快速检查某些元素是否符合特定标准，而无需编写大量代码*
*   *它允许我们快速跳出循环，这是上面提到的其他循环方法所不能做到的*

### *浏览器支持:*

*   *所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本*
*   *Microsoft Edge 版本 12 及以上*

## *Array.reduce 方法*

*`Array.reduce`方法的语法如下:*

```
*`Array.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])`*
```

*`reduce`方法对数组的每个元素执行一个**缩减器**函数(您提供的),产生一个输出值。*

> *注意，`reduce`方法的输出总是一个值。它可以是对象、数字、字符串、数组等等。这取决于您希望`reduce`方法的输出生成什么，但它总是一个值。*

*假设您想求出数组中所有数字的总和。你可以使用`reduce`方法。*

```
*`const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce(function(accumulator, number) {
  return accumulator + number; 
}, 0);

console.log(sum); // 15`*
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/ExNWzmo?editors=0012)。*

*`reduce`方法接受一个回调函数，该函数接收`accumulator`、`number`、`index`和`array`作为值。在上面的代码中，我们只使用了`accumulator`和`number`。*

*`accumulator`将包含用于数组的`initialValue`。`initialValue`决定`reduce`方法返回的数据的返回类型。*

*`number`是回调函数的第二个参数，它将在循环的每次迭代中包含数组元素。*

*在上面的代码中，我们为`accumulator`提供了`0`作为`initialValue`。所以回调函数第一次执行时，`accumulator + number`将是`0 + 1 = 1`，我们将返回值`1`。*

*下次回调函数运行时，`accumulator + number`将会是`1 + 2 = 3`(这里的`1`是上次迭代返回的前一个值，`2`是数组中的下一个元素)。*

*然后，下一次回调函数运行时，`accumulator + number`将会是
`3 + 3 = 6`(这里的第一个`3`是上一次迭代返回的前一个值，下一个`3`是数组中的下一个元素)，它将继续这样，直到`numbers`数组中的所有元素都没有被迭代。*

*所以`accumulator`将像静态变量一样保留上一次操作的值。*

*在上面的代码中，不需要`0`的`initialValue`，因为数组的所有元素都是整数。*

*所以下面的代码也可以工作:*

```
*`const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce(function (accumulator, number) {
  return accumulator + number;
});

console.log(sum); // 15`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/ExNWObz?editors=0012)。*

*这里，`accumulator`将包含数组的第一个元素，`number`将包含数组的下一个元素(第一次迭代中的`1 + 2 = 3`，下一次迭代中的 `3 + 3 = 6`，依此类推)。*

*但是指定`accumulator`的`initialValue`总是好的，因为这使得理解`reduce`方法的返回类型和获得正确类型的数据变得容易。*

*看看下面的代码:*

```
*`const numbers = [1, 2, 3, 4, 5];

const doublesSum = numbers.reduce(function (accumulator, number) {
  return accumulator + number * 2;
}, 10);

console.log(doublesSum); // 40`* 
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/jOVBQYx?editors=0012)。*

*这里，我们将数组的每个元素乘以 2。我们已经为`accumulator`提供了一个 10 的`initialValue`,因此 10 将被添加到求和的最终结果中，如下所示:*

```
*`[1 * 2, 2 * 2, 3 * 2, 4 * 2, 5 * 2] = [2, 4, 6, 8, 10] = 30 + 10 = 40`*
```

*假设，你有一个 x 和 y 坐标的对象数组，你想得到 x 坐标的和。你可以使用`reduce`方法。*

```
*`const coordinates = [
  { x: 1, y: 2 }, 
  { x: 2, y: 3 }, 
  { x: 3, y: 4 }
];

const sum = coordinates.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
}, 0);

console.log(sum); // 6`*
```

*这里有一个[码笔演示](https://codepen.io/myogeshchavan97/pen/OJbpaOg?editors=0012)。*

### *使用 reduce 方法的优势*

*   *使用`reduce`允许我们基于数组生成任何类型的简单或复杂数据*
*   *它会记住先前从循环中返回的数据，这样可以帮助我们避免创建一个全局变量来存储先前的值*

***浏览器支持:***

*   *所有现代浏览器和 Internet Explorer (IE)版本 9 及更高版本*
*   *Microsoft Edge 版本 12 及以上*

### *感谢阅读！*

*想详细了解 ES6+的所有特性，包括`let`和`const`、承诺、各种承诺方法、数组和对象析构、箭头函数、异步/等待、导入和导出等等吗？*

*查看我的《掌握现代 JavaScript》一书。这本书涵盖了学习 React 的所有先决条件，并帮助您更好地掌握 JavaScript 和 React。*

*另外，查看我的免费[React 路由器简介](https://yogeshchavan1.podia.com/react-router-introduction)课程，从头开始学习 React 路由器。*

***想要了解关于 JavaScript、React、Node.js 的最新常规内容吗？[在 LinkedIn 上关注我](https://www.linkedin.com/in/yogesh-chavan97/)。***