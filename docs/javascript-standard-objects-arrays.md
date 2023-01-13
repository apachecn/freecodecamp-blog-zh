# JavaScript 标准对象:数组

> 原文：<https://www.freecodecamp.org/news/javascript-standard-objects-arrays/>

你肯定听说过，在 JavaScript 中，一切都是对象。字符串、数字、函数、数组以及对象都被认为是对象。

在本教程中，我们将深入探讨**数组**“全局”或“标准内置”对象，以及与之相关的方法。

## 什么是数组？

在 JavaScript 中，数组是一个类似列表的对象，存储逗号分隔的值。这些值可以是任何东西——字符串、数字、对象，甚至函数。

数组以左括号(`[`)开始，以右括号(`]`)结束，使用数字作为元素索引。

### 如何创建阵列:

```
const shoppingList = ['Bread', 'Cheese', 'Apples'];
```

### 用括号符号访问数组中的值

```
const shoppingList = ['Bread', 'Cheese', 'Apples'];

console.log(shoppingList[1])
// Cheese
```

array 标准对象有许多有用的方法，下面列出了其中一些。

## Array.prototype.isArray()

如果一个对象是数组，`Array.isArray()`方法返回`true`，否则返回`false`。

### 句法

```
Array.isArray(obj)
```

### **参数**

****obj**** 被检查的对象。

### 的例子。艾萨里()

```
// all following calls return true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// Little known fact: Array.prototype itself is an array:
Array.isArray(Array.prototype); 

// all following calls return false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ __proto__: Array.prototype });
```

## **数组.原型.长度**

`length`是 JavaScript 中数组的属性，返回或设置给定数组中元素的数量。

数组的`length`属性可以这样返回。

```
let desserts = ["Cake", "Pie", "Brownies"];
console.log(desserts.length); // 3
```

赋值操作符，结合`length`属性，可以用来设置数组中元素的数量，如下所示。

```
let cars = ["Saab", "BMW", "Volvo"];
cars.length = 2;
console.log(cars.length); // 2
```

## 数组.原型.推送

`push()`方法用于在数组末尾添加一个或多个新元素。它还返回数组的新长度。如果没有提供参数，它将简单地返回数组的当前长度。

### **语法**

```
arr.push([element1[, ...[, elementN]]])
```

### **参数**

*   ****elementN**** 要添加到数组末尾的元素。

### **返回值**

调用该方法的数组的新长度。

## **举例:**

```
let myStarkFamily = ['John', 'Robb', 'Sansa', 'Bran'];
```

假设你有一组《权力的游戏》中史塔克家族的孩子。然而，其中一名成员 ****艾莉亚**** 却失踪了。知道了上面的代码，您可以通过将`'Arya'`赋给数组最后一个索引之后的索引来添加她，如下所示:

```
myStarkFamily[4] = 'Arya';
```

这种解决方案的问题是它不能处理一般情况。如果你事先不知道数组的长度，你就不能用这种方式添加新元素。这就是`push()`的作用。我们不需要知道数组有多长。我们只需将元素添加到数组的末尾。

```
myStarkFamily.push('Arya');
console.log(myStarkFamily);  // ['John', 'Robb', 'Sansa', 'Bran', 'Arya']

let newLength = myStarkFamily.push('Rickon');  // oops! forgot Rickon
console.log(newLength);  // 6
console.log(myStarkFamily);  // ['John', 'Robb', 'Sansa', 'Bran', 'Arya', 'Rickon']
```

## 数组.原型.反转

JavaScript 数组方法`.reverse()`将反转数组中元素的顺序。

****语法****

```
 let array = [1, 2, 3, 4, 5];
  array.reverse();
```

## **描述**

`.reverse()`反转数组元素的索引。

## **例题**

****用`.reverse()`来反转一个数组的元素****

```
 let array = [1, 2, 3, 4, 5];
  console.log(array);
  // Console will output 1, 2, 3, 4, 5

  array.reverse();

  console.log(array);
  /* Console will output 5, 4, 3, 2, 1 and
  the variable array now contains the set [5, 4, 3, 2, 1] */
```

## Array .原型. indexOf

`indexOf()`方法返回给定元素在数组中的第一个索引。如果元素不存在，则返回-1。

`indexOf()`将您想要搜索的元素作为参数，遍历数组中的元素，并返回可以找到该元素的第一个索引。如果元素不在数组中，`indexOf`返回-1。

**语法**

```
 arr.indexOf(searchElement[, fromIndex])
```

**参数**

*   ****searchElement** :** 您要搜索的元素。
*   ****fromIndex**** (可选):要开始搜索的索引。如果`fromIndex`大于或等于数组的长度，则不搜索数组，方法返回-1。如果 fromIndex 是负数，则认为它是从数组末尾的偏移量(数组仍从该处向前搜索)。默认值为 0，这意味着搜索整个数组。
*   要开始搜索表单的数组索引。默认值为 0，表示从数组的第一个索引开始搜索。如果`fromIndex`大于或等于数组的长度，那么该方法不搜索数组并返回-1。

### 例子

```
var array = [1, 2, 4, 1, 7]

array.indexOf(1); // 0
array.indexOf(7); // 4
array.indexOf(6); // -1
array.indexOf('1'); // -1
array.indexOf('hello'); // -1
array.indexOf(1, 2); // 3
array.indexOf(1, -3); // 3
```

## array . prototype . find 索引

`findIndex()`方法遍历一个数组，并根据作为参数传递的测试函数测试每个元素。它返回数组中第一个元素的索引，该元素根据测试函数返回 true。如果没有元素返回 true，`findIndex()`返回-1。

注意`findIndex()`不会改变它所调用的数组。

**语法**

```
arr.findIndex(callback(element, index, array), [thisArg])
```

##### **参数**

`callback`:对数组中的每个值执行的函数，它有三个参数:

*   `element`:数组中正在处理的当前元素。
*   `index`:数组中正在处理的当前元素的索引。
*   `array`:数组 findIndex()被调用。

`thisArg`(可选):执行回调函数时用作`this`的对象。

### 例子

此示例将在数组中查找相应的项，并从中返回索引。

```
let items = [
    {name: 'books', quantity: 2},
    {name: 'movies', quantity: 1},
    {name: 'games', quantity: 5}
];

function findMovies(item) { 
    return item.name === 'movies';
}

console.log(items.findIndex(findMovies));

// Index of 2nd element in the Array is returned,
// so this will result in '1'
```

以下示例显示了回调函数的每个可选参数的输出。这将返回`-1`,因为回调函数不会返回 true。

```
function showInfo(element, index, array) {
  console.log('element = ' + element + ', index = ' + index + ', array = ' + array);
  return false;
}

console.log('return = ' + [4, 6, 8, 12].findIndex(showInfo));

// Output
//  element = 4, index = 0, array = 4,6,8,12
//  element = 6, index = 1, array = 4,6,8,12
//  element = 8, index = 2, array = 4,6,8,12
//  element = 12, index = 3, array = 4,6,8,12
//  return = -1
```

## 数组.原型.查找

`find()`方法遍历一个数组，并根据作为参数传递的测试函数测试每个元素。它返回数组中第一个元素的值，该值根据测试函数返回 true。如果没有元素返回 true，`find()`返回`undefined`。

注意`find()`不会改变它所调用的数组。

### 句法

```
arr.find(callback(element, index, array), [thisArg])
```

##### **参数**

`callback`:对数组中每个值执行的函数。它需要三个参数:

*   `element`:数组中正在处理的当前元素。
*   `index`:数组中正在处理的当前元素的索引。
*   `array`:数组 find 被调用。

`thisArg`(可选):执行回调函数时用作`this`的对象。

### 例子

此示例将在数组中查找相应的项，并从中返回对象。

```
let items = [
    {name: 'books', quantity: 2},
    {name: 'movies', quantity: 1},
    {name: 'games', quantity: 5}
];

function findMovies(item) { 
    return item.name === 'movies';
}

console.log(items.find(findMovies));

// Output
//  { name: 'movies', quantity: 1 }
```

以下示例显示了回调函数的每个可选参数的输出。这将返回`undefined`,因为回调函数不会返回 true。

```
function showInfo(element, index, array) {
  console.log('element = ' + element + ', index = ' + index + ', array = ' + array);
  return false;
}

console.log('return = ' + [4, 6, 8, 12].find(showInfo));

// Output
//  element = 4, index = 0, array = 4,6,8,12
//  element = 6, index = 1, array = 4,6,8,12
//  element = 8, index = 2, array = 4,6,8,12
//  element = 12, index = 3, array = 4,6,8,12
//  return = undefined
```

## Array.prototype.join

JavaScript 数组方法`.join()`将一个数组的所有元素组合成一个字符串。

****语法****

```
 const array = ["Lorem", "Ipsum", "Dolor", "Sit"];
  const str = array.join([separator]);
```

### 因素

****分隔符**** 可选。指定用于分隔原始数组中每个元素的字符串。如果分隔符不是字符串，它将被转换为字符串。如果未提供 separator 参数，默认情况下，数组元素用逗号分隔。如果分隔符是一个空字符串`""`，所有数组元素之间没有分隔符。

### 描述

将数组中的所有元素连接成一个字符串。如果数组元素中的任何一个是`undefined`或`null`，那么该元素将被转换为空字符串`""`。

### 例子

****利用`.join()`四种不同的方式****

```
const array = ["Lorem", "Ipsum", "Dolor" ,"Sit"];

const join1 = array.join();           /* assigns "Lorem,Ipsum,Dolor,Sit" to join1 variable
                                     (because no separator was provided .join()
                                     defaulted to using a comma) */
const join2 = array.join(", ");       // assigns "Lorem, Ipsum, Dolor, Sit" to join2 variable
const join3 = array.join(" + ");      // assigns "Lorem + Ipsum + Dolor + Sit" to join3 variable
const join4 = array.join("");         // assigns "LoremIpsumDolorSit" to join4 variable
```

## **Array.prototype.concat**

`.concat()`方法返回一个新的数组，该数组由调用它的数组的元素组成，后面是参数的元素(按照它们被传递的顺序)。

您可以向`.concat()`方法传递多个参数。参数可以是数组，也可以是数据类型，如布尔值、字符串和数字。

### **语法**

```
const newArray = array.concat(value1, value2, value3...);
```

### **例题**

#### **连接两个数组**

```
const cold = ['Blue', 'Green', 'Purple'];
const warm = ['Red', 'Orange', 'Yellow'];

const result = cold.concat(warm);

console.log(result);
// results in ['Blue', 'Green', 'Purple', 'Red', 'Orange', 'Yellow'];
```

#### **将值连接到数组**

```
const odd = [1, 3, 5, 7, 9];
const even = [0, 2, 4, 6, 8];

const oddAndEvenAndTen = odd.concat(even, 10);

console.log(oddAndEvenAndTen);
// results in [1, 3, 5, 7, 9, 0, 2, 4, 6, 8, 10];
```

## 数组.原型.切片

JavaScript 数组方法`.slice()`将返回一个新的数组对象，它将是原始数组的一段(一个片段)。原始数组不会被修改。

****语法****

```
 array.slice()
  arr.slice(startIndex)
  arr.slice(startIndex, endIndex) 
```

### 因素

*   ****startIndex**** 切片应该开始的从零开始的索引。如果省略该值，它将从 0 开始。
*   ****endIndex**** 切片将在 这个零基索引之前结束**。负索引用于从数组末尾偏移。如果省略该值，该段将切到数组的末尾。**

### 例子

```
 const array = ['books', 'games', 'cup', 'sandwich', 'bag', 'phone', 'cactus']

  const everything = array.slice()
  // everything = ['books', 'games', 'cup', 'sandwich', 'bag', 'phone', 'cactus']

  const kitchen = array.slice(2, 4)
  // kitchen = ['cup', 'sandwich']

  const random = array.slice(4)
  // random = ['bag', 'phone', 'cactus']

  const noPlants = array.slice(0, -1)
  // noPlats = ['books', 'games', 'cup', 'sandwich', 'bag', 'phone']

  // array will still equal ['books', 'games', 'cup', 'sandwich', 'bag', 'phone', 'cactus']
```

## **数组.原型.拼接**

拼接方法类似于 [Array.prototype.slice](https://guide.freecodecamp.org/javascript/standard-objects/array/array-prototype-slice) ，但与`slice()`不同的是，它改变了被调用的数组。它的不同之处还在于，它可以用来向数组中添加值，也可以用来删除值。

### **参数**

`splice()`可以取下面详述的一个或多个参数。

#### **拼接(开始)**

如果只包含一个参数，那么`splice(start)`将删除从`start`到数组末尾的所有数组元素。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
exampleArray.splice(2);
// exampleArray is now ['first', 'second'];
```

如果`start`为负，它将从数组末尾开始倒数。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
exampleArray.splice(-1);
// exampleArray is now ['first', 'second', 'third'];
```

#### **拼接(开始，删除计数)**

如果包含第二个参数，那么`splice(start, deleteCount)`将从数组中移除`deleteCount`个元素，从`start`开始。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
exampleArray.splice(1, 2);
// exampleArray is now ['first', 'fourth'];
```

#### **splice(start，deleteCount，newElement1，newElement2，…)**

如果包含两个以上的参数，附加参数将是添加到数组中的新元素。这些添加元素的位置将从`start`开始。

通过将`0`作为第二个参数传递，可以在不删除任何元素的情况下添加元素。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
exampleArray.splice(1, 0, 'new 1', 'new 2');
// exampleArray is now ['first', 'new 1', 'new 2', 'second', 'third', 'fourth']
```

元素也可以替换。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
exampleArray.splice(1, 2, 'new second', 'new third');
// exampleArray is now ['first', 'new second', 'new third', 'fourth']
```

### **返回值**

除了改变它被调用的数组，`splice()`还返回一个包含被删除值的数组。这是一种将一个数组切割成两个不同数组的方法。

```
let exampleArray = ['first', 'second', 'third', 'fourth'];
let newArray = exampleArray.splice(1, 2);
// exampleArray is now ['first', 'fourth']
// newArray is ['second', 'third']
```

## **数组.原型.过滤器**

filter 方法将一个数组作为输入。它接受数组中的每个元素，并对其应用条件语句。如果该条件返回 true，则元素被“推”到输出数组。

一旦输入数组中的每个元素被“过滤”后，它将输出一个新数组，其中包含每个返回 true 的元素。

在下面的示例中，有一个数组，其中包含多个对象。通常，要遍历这个数组，可以使用 for 循环。

在这种情况下，我们希望得到所有成绩大于或等于 90 的学生。

```
const students = [
  { name: 'Quincy', grade: 96 },
  { name: 'Jason', grade: 84 },
  { name: 'Alexis', grade: 100 },
  { name: 'Sam', grade: 65 },
  { name: 'Katie', grade: 90 }
];
//Define an array to push student objects to.
let studentsGrades = []
for (var i = 0; i < students.length; i++) {
  //Check if grade is greater than 90
  if (students[i].grade >= 90) {
    //Add a student to the studentsGrades array.
    studentsGrades.push(students[i])
  }
}

console.log(studentsGrades); // [ { name: 'Quincy', grade: 96 }, { name: 'Alexis', grade: 100 }, { name: 'Katie', grade: 90 } ]
```

这种 for 循环是可行的，但是它相当长。为许多需要迭代的数组反复编写 for 循环也会变得很乏味。

这是过滤器的一个很好的用例！

以下是使用过滤器的相同示例:

```
const students = [
  { name: 'Quincy', grade: 96 },
  { name: 'Jason', grade: 84 },
  { name: 'Alexis', grade: 100 },
  { name: 'Sam', grade: 65 },
  { name: 'Katie', grade: 90 }
];

const studentGrades = students.filter(function (student) {
  //This tests if student.grade is greater than or equal to 90\. It returns the "student" object if this conditional is met.
  return student.grade >= 90;
});

console.log(studentGrades); // [ { name: 'Quincy', grade: 96 }, { name: 'Alexis', grade: 100 }, { name: 'Katie', grade: 90 } ]
```

filter 方法写起来更快，读起来更清晰，但仍能完成同样的事情。使用 ES6 语法，我们甚至可以复制带过滤器的 6 行 for 循环:

```
const students = [
  { name: 'Quincy', grade: 96 },
  { name: 'Jason', grade: 84 },
  { name: 'Alexis', grade: 100 },
  { name: 'Sam', grade: 65 },
  { name: 'Katie', grade: 90 }
];

const studentGrades = students.filter(student => student.grade >= 90);
console.log(studentGrades); // [ { name: 'Quincy', grade: 96 }, { name: 'Alexis', grade: 100 }, { name: 'Katie', grade: 90 } ]
```

Filter 非常有用，是针对条件语句过滤数组的 for 循环的最佳选择。

## **Array.prototype.forEach**

`.forEach()` array 方法用于遍历数组中的每一项。在 array 对象上调用该方法，并向其传递一个在数组中的每一项上调用的函数。

```
let arr = [1, 2, 3, 4, 5];

arr.forEach(number => console.log(number * 2));

// 2
// 4
// 6
// 8
// 10
```

回调函数还可以接受索引的第二个参数，以防需要引用数组中当前项的索引。

```
let arr = [1, 2, 3, 4, 5];

arr.forEach((number, i) => console.log(`${number} is at index ${i}`));

// '1 is at index 0'
// '2 is at index 1'
// '3 is at index 2'
// '4 is at index 3'
// '5 is at index 4'
```

## **Array.prototype.reduce**

`reduce()`方法将一组值减少到只有一个值。它被称为瑞士军刀，或多工具，数组变换方法。其他的，比如`map()`和`filter()`，提供更具体的转换，而`reduce()`可以用来将数组转换成你想要的任何输出。

### **语法**

```
arr.reduce(callback[, initialValue])
```

`callback`参数是一个函数，将对数组中的每个项目调用一次。这个函数有四个参数，但通常只使用前两个。

*   *累加器* -前一次迭代的返回值
*   *当前值* -数组中的当前项
*   *索引* -当前项目的索引
*   *数组*——调用 reduce 的原始数组
*   `initialValue`参数是可选的。如果提供了，它将在第一次调用回调函数时用作初始累加器值(参见下面的示例 2)。

### **例 1**

将整数数组转换为数组中所有整数的和。

```
const numbers = [1,2,3]; 
const sum = numbers.reduce(function(total, current){
    return total + current;
});
console.log(sum); 
```

这将输出`6`到控制台。

### **例 2**

将字符串数组转换为单个对象，该对象显示每个字符串在数组中出现的次数。注意这个 reduce 调用传递了一个空对象`{}`作为`initialValue`参数。这将用作传递给回调函数的累加器的初始值(第一个参数)。

```
const pets = ['dog', 'chicken', 'cat', 'dog', 'chicken', 'chicken', 'rabbit'];

const petCounts = pets.reduce(function(obj, pet){
    if (!obj[pet]) {
        obj[pet] = 1;
    } else {
        obj[pet]++;
    }
    return obj;
}, {});

console.log(petCounts); 
```

输出:

```
 { 
    dog: 2, 
    chicken: 3, 
    cat: 1, 
    rabbit: 1 
 }
```

## **数组.原型.排序**

此方法对数组元素进行就地排序，并返回该数组。

`sort()`方法遵循 ****ASCII 顺序**** ！

```
let myArray = ['#', '!'];
let sortedArray = myArray.sort();   // ['!', '#'] because in the ASCII table "!" is before "#"

myArray = ['a', 'c', 'b'];
console.log(myArray.sort()); // ['a', 'b', 'c']
console.log(myArray) // ['a', 'b', 'c']

myArray = ['b', 'a', 'aa'];
console.log(myArray.sort());   // ['a', 'aa', 'b']

myArray = [1, 2, 13, 23];
console.log(myArray.sort());   // [1, 13, 2, 23] numbers are treated like strings!
```

### 高级用法

`sort()`方法也可以接受一个参数:`array.sort(compareFunction)`

### **例如**

```
function compare(a, b){
  if (a < b){return -1;}
  if (a > b){return 1;}
  if (a === b){return 0;}
}

let myArray = [1, 2, 23, 13];
console.log(myArray.sort()); // [ 1, 13, 2, 23 ]
console.log(myArray.sort(compare));   // [ 1, 2, 13, 23 ]

myArray = [3, 4, 1, 2];
sortedArray = myArray.sort(function(a, b){.....});   // it depends from the compareFunction
```

## Array.prototype.some()

JavaScript 数组方法`.some()`将采用回调函数来测试数组中的每个元素；一旦回调返回`true`，那么`.some()`将立即返回 true。

****语法****

```
 var arr = [1, 2, 3, 4];
  arr.some(callback[, thisArg]);
```

## **回调函数**

****语法****

```
 var isEven = function isEven(currentElement, index, array) {
      if(currentElement % 2 === 0) {
          return true;
      } else {
          return false;
      }
  }
```

查看维基上的[算术运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)来查看余数运算符`%`

****有 3 个自变量****

currentelemont

*   这是一个代表传递给回调的元素的变量。

指数

*   这是从 0 开始的当前元素的索引值

排列

*   调用`.some()`的数组。

回调函数应该实现一个测试用例。

### 这个参数

是一个可选参数，更多信息可在[MDN

### 描述

`.some()`将为数组中的每个元素运行回调函数。一旦回调返回 true，`.some()`将返回`true`。如果回调为数组中的每个元素的*返回一个[假值](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)，那么`.some()`返回假。*

不会改变调用它的数组。

### 例子

****传递一个函数给`.some()`****

```
const isEven = function isEven(currentElement, index, array) {
  if(currentElement % 2 === 0) {
      return true;
  } else {
      return false;
  }
}

const arr1 = [1, 2, 3, 4, 5, 6];
arr1.some(isEven);  // returns true
const arr2 = [1, 3, 5, 7];
arr2.some(isEven);  // returns false
```

****匿名功能****

```
const arr3 = ['Free', 'Code', 'Camp', 'The Amazing'];
arr3.some(function(curr, index, arr) {
  if (curr === 'The Amazing') {
      return true;
  } 
}); // returns true

const arr4 = [1, 2, 14, 5, 17, 9];
arr4.some(function(curr, index, arr) {
  return curr > 20;
  });  // returns false

// ES6 arrows functions
arr4.some((curr) => curr >= 14)  // returns true
```

## 数组.原型.每

方法测试数组中的每一个元素是否通过了提供的测试。

****语法****

```
 arr.every(callback[, thisArg])
```

### 因素

1.  **回调**需要三个参数:

*   **(必选)–数组中的当前元素。**
*   ******index**** (可选)–数组中当前元素的索引。**
*   ******数组**** (可选)–调用了`every`方法的数组。**

**2. ****thisArg**** 可选。它是回调中用作`this`的值。**

## ****描述****

**对于每个数组元素，`every`方法调用一次`callback`函数，按照升序排列，直到`callback`函数返回 false。如果找到导致`callback`返回 false 的元素，every 方法立即返回`false`。否则，every 方法返回`true`。**

**对于数组中缺少的元素，不调用回调函数。**

**除了 array 对象之外，every 方法还可用于任何具有 length 属性和数字索引属性名称的对象。不改变调用它的数组。**

## ****例题****

```
 `function isBigEnough(element, index, array) {
    return element >= 10;
  }
  [12, 5, 8, 130, 44].every(isBigEnough);   // false
  [12, 54, 18, 130, 44].every(isBigEnough); // true

  // Define the callback function.
  function CheckIfEven(value, index, ar) {
      document.write(value + " ");

      if (value % 2 == 0)
          return true;
      else
          return false;
  }

  // Create an array.
  var numbers = [2, 4, 5, 6, 8];

  // Check whether the callback function returns true for all of the
  // array values.
  if (numbers.every(CheckIfEven))
      document.write("All are even.");
  else
      document.write("Some are not even.");

  // Output:
  // 2 4 5 Some are not even.`
```

## ****Array.prototype.map****

**`.map()`方法遍历给定的数组，并在每个元素上执行提供的函数。它返回一个新数组，该数组包含对每个元素的函数调用的结果。**

### ****例题****

******ES5******

```
`var arr = [1, 2, 3, 4];
var newArray = arr.map(function(element) { return element * 2});
console.log(newArray); // [2, 4, 6, 8]`
```

******ES6******

```
`const arr = [1, 2, 3, 4];
const newArray = arr.map(element => element * 2);
console.log(newArray);
//[2, 4, 6, 8]`
```

## ****Array.prototype.includes****

**`includes()`方法确定一个数组是否包含一个值。它返回真或假。**

**它需要两个参数:**

1.  **`searchValue` -数组中要搜索的元素。**
2.  **`fromIndex` -数组中开始搜索提供的`searchValue`的位置。如果提供了负值，则从数组长度减去负值开始。**

### ****例子****

```
`const a = [1, 2, 3];
a.includes(2); // true 
a.includes(4); // false`
```

## ****array . prototype . tolocalestring****

**`toLocaleString()`方法返回一个表示数组元素的字符串。所有元素都使用它们的 toLocaleString 方法转换成字符串。调用这个函数的结果是特定于地区的。**

##### ****语法:****

```
`arr.toLocaleString();`
```

##### ****参数****

*   **`locales`(可选)-保存语言标签的字符串或数组的参数 [BCP 47 语言标签](http://tools.ietf.org/html/rfc5646)。**
*   **`options`(可选)-具有配置属性的对象**

##### ****返回值****

**一个字符串，表示由特定于区域设置的字符串(如逗号“，”)分隔的数组元素**

### **例子**

```
`const number = 12345;
const date = new Date();
const myArray = [number, date, 'foo'];
const myString = myArray.toLocaleString(); 

console.log(myString); 
// OUTPUT '12345,10/25/2017, 4:20:02 PM,foo'`
```

**根据语言和地区标识符(地区)，可以显示不同的输出。**

```
`const number = 54321;
const date = new Date();
const myArray = [number, date, 'foo'];
const myJPString = myArray.toLocaleString('ja-JP');

console.log(myJPString);
// OUTPUT '54321,10/26/2017, 5:20:02 PM,foo'`
```

**至此，您应该了解了用 JavaScript 创建和操作数组的所有必要知识。现在去把东西整理好！**

## **关于阵列的更多信息:**

*   **[JavaScript 数组到底是什么？](https://www.freecodecamp.org/news/what-in-the-world-is-a-javascript-array/)**
*   **[JavaScript 数组函数举例说明](https://www.freecodecamp.org/news/javascript-map-reduce-and-filter-explained-with-examples/)**
*   **[终极指南减少](https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-array-methods-reduce/)**
*   **[地图终极指南](https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-array-methods-map/)**
*   **[JavaScript 数组长度解释](https://www.freecodecamp.org/news/javascript-array-length/)**

## **关于回调函数的更多信息**

**您肯定注意到了一件事，那就是许多数组方法都使用回调函数。查看这些文章，了解更多相关信息:**

*   **[JavaScript 中什么是回调函数？](https://www.freecodecamp.org/news/what-is-a-callback-function-in-javascript/)**
*   **[如何避免回调地狱](https://www.freecodecamp.org/news/how-to-deal-with-nested-callbacks-and-avoid-callback-hell-1bc8dc4a2012/)**