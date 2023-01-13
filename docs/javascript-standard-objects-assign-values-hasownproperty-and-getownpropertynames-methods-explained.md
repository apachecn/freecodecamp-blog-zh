# JavaScript 标准对象:assign、values、hasOwnProperty 和 getOwnPropertyNames 方法解释

> 原文：<https://www.freecodecamp.org/news/javascript-standard-objects-assign-values-hasownproperty-and-getownpropertynames-methods-explained/>

在 JavaScript 中，`Object`数据类型用于存储键值对，并且像`Array`数据类型一样，包含许多有用的方法。这些是您在处理对象时会用到的一些有用的方法。

## 对象分配方法

`Object.assign()`方法用于

1.  向现有对象添加属性和值
2.  制作现有对象的新副本，或者
3.  将多个现有对象合并成一个对象。

`Object.assign()`方法需要一个`targetObject`作为参数，并且可以接受无限个`sourceObjects`作为附加参数。

这里需要注意的是`targetObject`参数总是会被修改。如果该参数指向一个现有的对象，那么该对象将被修改和复制。

如果您希望创建一个对象的副本而不修改原来的对象，您可以传递一个空对象`{}`作为第一个(`targetObject`)参数，传递要复制的对象作为第二个(`sourceObject`)参数。

如果作为参数传入`Object.assign()`的对象共享相同的属性(或键)，参数列表中后面的属性值将覆盖前面的属性值。

****语法****

```
Object.assign(targetObject, ...sourceObject);
```

****返回值****

`Object.assign()`返回`targetObject`。

### 例子

修改和复制`targetObject`:

```
let obj = {name: 'Dave', age: 30};

let objCopy = Object.assign(obj, {coder: true});

console.log(obj); // { name: 'Dave', age: 30, coder: true }
console.log(objCopy); // { name: 'Dave', age: 30, coder: true }
```

无修改复制`targetObject`:

```
let obj = {name: 'Dave', age: 30};

let objCopy = Object.assign({}, obj, {coder: true});

console.log(obj); // { name: 'Dave', age: 30 }
console.log(objCopy); // { name: 'Dave', age: 30, coder: true }
```

具有相同属性的对象 *:*

```
let obj = {name: 'Dave', age: 30, favoriteColor: 'blue'};

let objCopy = Object.assign({}, obj, {coder: true, favoriteColor: 'red'});

console.log(obj); // { name: 'Dave', age: 30, favoriteColor: 'blue' }
console.log(objCopy); // { name: 'Dave', age: 30, favoriteColor: 'red', coder: true }
```

## 目标值方法

`Object.values()`方法将一个对象作为参数，并返回其值的数组。这使得它对于链接常见的`Array`方法很有用，比如`.map()`、`.forEach()`和`.reduce()`。

**语法**

```
Object.values(targetObject);
```

**返回值**

传递的对象(`targetObject`)值的数组。

### 例子

```
const obj = { 
  firstName: 'Quincy',
  lastName: 'Larson' 
}

const values = Object.values(obj);

console.log(values); // ["Quincy", "Larson"]
```

如果你传递的对象有数字作为键，那么`Object.value()`将根据键的数字顺序返回值:

```
const obj1 = { 0: 'first', 1: 'second', 2: 'third' };
const obj2 = { 100: 'apple', 12: 'banana', 29: 'pear' };

console.log(Object.values(obj1)); // ["first", "second", "third"]
console.log(Object.values(obj2)); // ["banana", "pear", "apple"]
```

如果传递给`Object.values()`的不是一个对象，它将在作为数组返回之前被强制转换成一个对象:

```
const str = 'hello';

console.log(Object.values(str)); // ["h", "e", "l", "l", "o"]
```

## **对象拥有自己的属性方法**

`Object.hasOwnProperty()`方法返回一个[布尔值](https://www.freecodecamp.org/news/p/6bce9cb3-38ff-45d1-a56b-322354699b01/www.freecodecamp.org/news/booleans-in-javascript-explained-how-to-use-booleans-in-javascript/)，表明对象是否拥有指定的属性。

这是一种检查对象是否具有指定属性的便捷方法，因为它会相应地返回 true/false。

**语法**

`Object.hasOwnProperty(prop)`

**返回值**

```
true
// or
false
```

### **例题**

使用`Object.hasOwnProperty()`测试给定对象中是否存在属性:

```
const course = {
  name: 'freeCodeCamp',
  feature: 'is awesome',
}

const student = {
  name: 'enthusiastic student',
}

course.hasOwnProperty('name');  // returns true
course.hasOwnProperty('feature');   // returns true

student.hasOwnProperty('name');  // returns true
student.hasOwnProperty('feature'); // returns false
```

## 对象 getOwnPropertyNames 方法

`Object.getOwnPropertyNames()`方法将一个对象作为参数，并返回其所有属性的数组。

**语法**

```
Object.getOwnPropertyNames(obj)
```

**返回值**

传递的对象属性的字符串数组。

### 例子

```
const obj = { firstName: 'Quincy', lastName: 'Larson' }

console.log(Object.getOwnPropertyNames(obj)); // ["firstName", "lastName"]
```

如果传递给`Object.getOwnPropertyNames()`的不是一个对象，它将在作为数组返回之前被强制转换成一个对象:

```
const arr = ['1', '2', '3'];

console.log(Object.getOwnPropertyNames(arr)); // ["0", "1", "2", "length"]
```

## **无极.原型.然后**

一个`Promise.prototype.then`函数接受两个参数并返回一个承诺。

第一个参数是接受一个参数的必需函数。成功履行承诺将触发此功能。

第二个参数是可选函数，它也接受自己的一个参数。一个抛出的错误或对承诺的拒绝将触发该功能。

```
 function onResolved (resolvedValue) {
     /*
     * access to resolved values of promise
     */
   }

  function onRejected(rejectedReason) {
     /*
     * access to rejection reasons of promise
     */
   }

  promiseReturningFunction(paramList)
     .then( // then function
       onResolved,
       [onRejected]
     );
```

`Promise.prototype.then`允许您按顺序执行许多异步活动。您可以通过将一个`then`函数附加到由点运算符分隔的另一个函数来实现这一点。

```
 promiseReturningFunction(paramList)
   .then( // first then function
     function(arg1) {
       // ...
       return someValue;
     }
   )
   ...
   .then( // nth then function
     function(arg2) {
       // ...
       return otherValue;
     }
   )
```

## **Map.prototype.entries**

返回一个新的`Iterator`对象，它包含按插入顺序排列的`Map`对象中每个元素的`[key, value]`对。

## **语法**

```
myMap.entries()
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

var iterator = myMap.entries();

console.log(iterator.next().value); // ['foo', 1]
console.log(iterator.next().value); // ['bar', 2]
console.log(iterator.next().value); // ['baz', 3]
```

## 关于 JavaScript 中对象的更多信息:

*   [如何在 JavaScript 中创建对象](https://www.freecodecamp.org/news/a-complete-guide-to-creating-objects-in-javascript-b0e2450655e8/)
*   [如何在 JavaScript 中遍历对象](https://www.freecodecamp.org/news/how-to-loop-through-objects-in-javascript-a80b7d2478ac/)

## 有关布尔值的更多信息:

*   [JavaScript 中的布尔值](https://www.freecodecamp.org/news/p/6bce9cb3-38ff-45d1-a56b-322354699b01/www.freecodecamp.org/news/booleans-in-javascript-explained-how-to-use-booleans-in-javascript/)