# 高阶函数:如何使用 Filter、Map 和 Reduce 获得更易维护的代码

> 原文：<https://www.freecodecamp.org/news/higher-order-functions-in-javascript-d9101f9cf528/>

吉多·施密茨

高阶函数可以让你的代码更具声明性，从而帮助你提升你的 JavaScript 游戏。也就是说，简短，易读。

高阶函数是任何在执行时返回一个函数、将一个函数作为其一个或多个参数或者两者兼有的函数。如果您使用过任何一种`Array`方法，如`map`或`filter`，或者将回调函数传递给 jQuery 的`$.get`，那么您已经使用过高阶函数。

当您使用`Array.map`时，您提供一个函数作为它唯一的参数，它应用于数组中包含的每个元素。

```
var arr = [ 1, 2, 3 ];

var arrDoubled = arr.map(function(num) {
  return num * 2;
});

console.log(arrDoubled); // [ 2, 4, 6 ]
```

高阶函数也可以返回一个函数。例如，您可以创建一个名为`multiplyBy`的函数，它接受一个数字并返回一个将您提供的另一个数字乘以第一个数字的函数。您可以使用这种方法创建一个`multiplyByTwo`函数来传递给`Array.map`。这将会给你和上面看到的一样的结果。

```
function multiplyBy(num1) {
  return function(num2) {
    return num1 * num2;
  }
}

var multiplyByTwo = multiplyBy(2);

var arr = [ 1, 2, 3 ];

var arrDoubled = arr.map(multiplyByTwo);

console.log(arrDoubled); // [ 2, 4, 6 ]
```

知道何时以及如何使用这些功能是至关重要的。它们使你的代码更容易理解和维护。它还可以轻松地将各种功能相互结合。这叫构图，这里就不多赘述了。在本文中，我将介绍 JavaScript 中三个最常用的高阶函数。这些是`.filter()`、`.map()`和`.reduce()`。

## 过滤器

想象一下，编写一段代码，它接受一个人员列表，其中您希望过滤掉 18 岁或以上的人员。

我们的列表如下所示:

```
const people = [ { name: ‘John Doe’, age: 16 }, { name: ‘Thomas Calls’, age: 19 }, { name: ‘Liam Smith’, age: 20 }, { name: ‘Jessy Pinkman’, age: 18 },];
```

让我们看一个一阶函数的例子，它选择 18 岁以上的人。我使用了一个箭头函数,它是 ECMAScript 标准的一部分，简称 ES6。这只是定义函数的一种更短的方式，允许您跳过输入 function 和 return，以及一些括号、大括号和分号。

```
const peopleAbove18 = (collection) => {  const results = [];   for (let i = 0; i < collection.length; i++) {    const person = collection[i];     if (person.age >= 18) {      results.push(person);    }  }
```

```
 return results;};
```

现在，如果我们想选择所有 18 到 20 岁的人呢？我们可以创造另一个功能。

```
const peopleBetween18And20 = (collection) => {  const results = [];   for (let i = 0; i < collection.length; i++) {    const person = collection[i];     if (person.age >= 18 && person.age <= 20) {      results.push(person);    }  }
```

```
 return results;};
```

您可能已经认识到这里有许多重复的代码。这可以抽象成一个更一般化的解决方案。这两个函数有一些共同点。它们都遍历一个列表，并在给定的条件下过滤它。

> "高阶函数是以一个或多个函数作为自变量的函数." *— [关闭桥梁](https://clojurebridge.github.io/community-docs/docs/clojure/higher-order-function/)*

我们可以通过使用更加声明性的方法`.filter()`来改进我们之前的函数。

```
const peopleAbove18 = (collection) => {  return collection    .filter((person) => person.age >= 18);}
```

就是这样！通过使用这个高阶函数，我们可以减少很多额外的代码。这也使得我们代码更容易阅读。我们不关心事情是如何被过滤的，我们只是想让它过滤。我将在本文的后面讨论函数的组合。

## 地图

让我们用同样的人的列表和一组名字来判断这个人是否喜欢喝咖啡。

```
const coffeeLovers = [‘John Doe’, ‘Liam Smith’, ‘Jessy Pinkman’];
```

命令式的方式会是这样的:

```
const addCoffeeLoverValue = (collection) => {  const results = [];   for (let i = 0; i < collection.length; i++) {    const person = collection[i];
```

```
 if (coffeeLovers.includes(person.name)) {      person.coffeeLover = true;    } else {      person.coffeeLover = false;    }     results.push(person);  }   return results;};
```

我们可以使用`.map()`使其更具声明性。

```
const incrementAge = (collection) => {  return collection.map((person) => {    person.coffeeLover = coffeeLovers.includes(person.name);     return person;  });};
```

还是那句话，`.map()`是高阶函数。它允许函数作为参数传递。

## 减少

我敢打赌，当你知道何时和如何使用它时，你会喜欢这个功能的。
关于`.reduce()`很酷的一点是上面的大部分功能都可以用它来做。

先举个简单的例子。我们想总结所有人的年龄。同样，我们将看看如何使用命令式方法来实现这一点。它基本上是在集合中循环，并随着年龄增加一个变量。

```
const sumAge = (collection) => {  let num = 0;   collection.forEach((person) => {    num += person.age;  });   return num;}
```

以及使用`.reduce()`的声明性方法。

```
const sumAge = (collection) => collection.reduce((sum, person) => { return sum + person.age;}, 0);
```

我们甚至可以使用`.reduce()`来创建我们自己的`.map()`和`.filter()`的实现。

```
const map = (collection, fn) => {  return collection.reduce((acc, item) => {    return acc.concat(fn(item));  }, []);}
```

```
const filter = (collection, fn) => {  return collection.reduce((acc, item) => {    if (fn(item)) {      return acc.concat(item);    }     return acc;  }, []);}
```

这一开始可能很难理解。但是`.reduce()`基本上是从一个集合和一个有初始值的变量开始。然后遍历集合，将值追加(或添加)到变量中。

## 结合映射、过滤和减少

很好，这些功能存在。但好的一面是，它们存在于 JavaScript 中的数组原型上。这意味着这些功能可以一起使用！这使得创建可重用的函数变得容易，并减少了编写某些功能所需的代码量。

所以我们讨论了使用`.filter()`来过滤掉 18 岁以下的人。`.map()`添加`coffeeLover`属性，`.reduce()`最终创建每个人年龄总和。
让我们写一些实际上结合了这三个步骤的代码。

```
const people = [ { name: ‘John Doe’, age: 16 }, { name: ‘Thomas Calls’, age: 19 }, { name: ‘Liam Smith’, age: 20 }, { name: ‘Jessy Pinkman’, age: 18 },];
```

```
const coffeeLovers = [‘John Doe’, ‘Liam Smith’, ‘Jessy Pinkman’];
```

```
const ageAbove18 = (person) => person.age >= 18;const addCoffeeLoverProperty = (person) => { person.coffeeLover = coffeeLovers.includes(person.name);  return person;}
```

```
const ageReducer = (sum, person) => { return sum + person.age;}, 0);
```

```
const coffeeLoversAbove18 = people .filter(ageAbove18) .map(addCoffeeLoverProperty);
```

```
const totalAgeOfCoffeeLoversAbove18 = coffeeLoversAbove18 .reduce(ageReducer);
```

```
const totalAge = people .reduce(ageReducer);
```

如果你用命令式的方法，你将会写很多重复的代码。

用`.map()`、`.reduce()`和`.filter()`创建函数的想法将会提高你将要编写的代码的质量。但也增加了很多可读性。你不必考虑函数内部发生了什么。很好理解。

感谢阅读！:)

在[推特](https://twitter.com/guidsen)上打招呼