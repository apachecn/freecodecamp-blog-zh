# 找到你的方式。地图()

> 原文：<https://www.freecodecamp.org/news/finding-your-way-with-map-aecb8ca038f6/>

解决方案的冗长和优雅取决于我们用来解决特定问题的工具。虽然问题解决的目标是*解决问题*，但是它的方法应该尽可能向最优雅的方式发展。然而，通向这样一个解决方案的旅程似乎位于一条渐近曲线上。完美越来越近，但永远遥不可及。

#### 问题是

想象一下，有一个数组，需要改变数组中的每个元素。也许，例如，获取一个以英寸为单位的高度数组，并需要将它们转换成厘米。或者可能将摄氏温度转换为华氏温度。如果您是编程新手，您的思维可能会立即进入某种形式的循环。你猜怎么着？我相信你会成功的。

然而，我在这里给你多一个工具——让你更接近优雅的东西:`Array.prototype.map()`。

`map`方法允许我们转换数组的每个元素，而不影响原始数组。它被认为是一个高阶函数和函数式编程技术，因为它将一个函数作为参数，我们在执行计算时不会改变应用程序的状态。

> `Map`是从数组原型继承的属性。原型提供了对象自带的内置方法(在 JavaScript 看来，数组是特殊类型的对象)。虽然`map`可能有点外国，但这个原型与`Array.length`原型没有什么不同。这些只是嵌入到 JavaScript 中的简单方法。数组原型可以被添加和变异:`Array.prototype.<someMethodHere>` =...

本课结束时，我们将了解`map`如何工作，并编写我们自己的数组原型方法。

#### 那么。map()做什么？

假设你有一个摄氏温度数组，你想把它转换成华氏温度。

有许多方法可以解决这个问题。一种方法是编写一个`for`循环，根据给定的摄氏温度创建一个华氏温度数组。

使用`for`循环，我们可以编写:

```
const celciusTemps = [22, 36, 71, 54];
const getFahrenheitTemps = (function(temp) {
   const fahrenheitTemps = [];
   for (let i = 0; i < celciusTemps.length; i += 1) {
      temp = celciusTemps[i] * (9/5) + 32
      fahrenheitTemps.push(temp);
   }
   console.log(fahrenheitTemps); [71.6, 96.8, 159.8, 129.2
})();
```

有几点需要注意:

1.  它工作了。
2.  我们使用一个立即调用的函数表达式(IIFE)来避免调用函数。
3.  有点啰嗦，也不是很优雅。

`Map`允许我们将上面的代码重构如下:

```
const fahrenheitTemps = celciusTemps.map(e => e * (9/5) + 32);
console.log(fahrenheitTemps); // [71.6, 96.8, 159.8, 129.2]
```

#### 那么地图是如何工作的呢？

`Map`接受一个函数，并将该函数应用于数组中的每个元素。我们可以用 ES5 把`map`写得更详细一点，以便看得更清楚一点。

```
const fahrenheitTemps = celciusTemps

   .map(function(elementOfArray) {
      return elementOfArray * (9/5) + 32;
   });
console.log(fahrenheitTemps); // [71.6, 96.8, 159.8, 129.2]
```

如果我们的地图功能可以说出它在做什么，它会说:

“对于数组中的每个元素，我都将其乘以(9/5)，然后加上 32。完成后，我将结果作为一个名为 fahrenheitTemps 的新数组中的元素返回。

让我们看一个更常见的用例。让我们假设我们有一个`people`对象的数组。每个对象都有一个`name`和`age`键值对。我们想创建一个变量，它只是数组中每个人的名字。使用我们的`for`循环方法，我们可以编写:

```
const people = [
   {name: Steve, age: 32},
   {name: Mary, age: 28},
   {name: Bill, age: 41},
];
const getNames = (function(person) {
   const names = [];
   for (let i = 0; i < people.length; i += 1) {
      name = people[i].name;
      names.push(name);
   }
   console.log(names); // [Steve, Mary, Bill];
})();
```

用`map`:

```
const names = people.map(e => e.name);
console.log(names) // [Steve, Mary, Bill];
```

注意这里我们不做任何转换，我们只是返回键值对`name`。

同样，`for`循环起作用。但是，它是冗长的，并且每次我们想要做不同的转换时，我们必须创建一个新的定制函数。编程的一个主要部分是编写枯燥的代码(不要重复)。这些高阶函数(如 map)允许我们用更少的代码行完成更复杂的编程，而没有它们是不行的。

#### 重新发明轮子:

为了更好地理解幕后发生的事情，我们将制作自己的 map 函数，并将它附加到数组原型上。

首先，要将原型方法附加到数组，我们将编写:

`Array.prototype.<yourMethodHere>`

所以对我们来说:

`Array.prototype.myMap = <our code>`

但是，我们的代码会是什么呢？

我们已经从上面的`for`循环中获得了所需的逻辑。我们需要做的就是对它进行一点重构。让我们重构我们写的最后一个函数`getNames()`。

记住，这个函数接受一个人(换句话说，我们数组中的一个元素)，对该元素进行自定义转换(使用`for`循环和一些逻辑)，然后返回一个姓名数组(或一个新数组)。

```
const getNames = (function(person) {
   const names = [];
   for (let i = 0; i < people.length; i += 1) {
      name = people[i].name;
      names.push(name);
   }
   console.log(names); // [Steve, Mary, Bill];
})();
```

首先，让我们更改函数的名称。毕竟，这个新方法并不假设知道它将作用于哪种数组:

```
const myMap = (function(person) { //Changed name
   const names = [];
   for (let i = 0; i < people.length; i += 1) {
      name = people[i].name;
      names.push(name);
   }
   console.log(names); // [Steve, Mary, Bill];
})();
```

第二，我们正在创造我们自己版本的`.map()`。我们知道这将需要用户提供的功能。让我们改变函数的参数:

```
// It is a bit verbose, but a very clear parameter name
const myMap = (function(userProvidedFunction) { 
   const names = [];
   for (let i = 0; i < people.length; i += 1) {
      name = people[i].name;
      names.push(name);
   }
   console.log(names); // [Steve, Mary, Bill];
})();
```

最后，我们不知道这个方法将作用于哪个数组。所以，我们不能指`people.length`但是我们*可以*指`this.length`。`this`，将返回该方法正在作用的数组。此外，让我们清理一些其他的变量名:

```
const myMap = (function(userProvidedFunction) { 
   // change variable name
   const newArr = [];
   // use "this.length"   
   for (let i = 0; i < this.length; i += 1) { 

      // use "this[i]", and change variable name      
      const newElement = this[i];

      // update the array we push into
      newArr.push(newElement); 
   }
   // Return the newly created array
   return newArr; 
})();
```

我们快到了，但是有一件事我们忘记了。我们还没有转换阵列！我们上面所做的就是返回旧的数组。我们必须将用户提供的函数应用于数组的每个元素:

```
const myMap = (function(userProvidedFunction) { 
   const newArr = [];
   for (let i = 0; i < this.length; i += 1) {

      /* Transform the element by passing it into the 
       * user-provided function
       */
      const newElement = userProvidedFunction(this[i]); 

      newArr.push(newElement); 
   }
   return newArr;
})();
```

最后，我们可以将我们的新函数附加到`Array.prototype`上。

`Array.prototype.myMap = myMap;`

最后的健全性检查:

```
const myArray = [1, 2, 3];
// Multiply each element x 2
const myMappedArray = myArray.myMap(e => e * 2)
console.log(myMappedArray) // [2, 4, 6];
```

#### 摘要

`Map`是数组提供的一个原型方法。在幕后，它遍历数组，将用户提供的函数应用于每个元素。最终，它返回一个包含转换后的值的新数组。它在不改变原始数组的情况下做到了这一点。因为它所带的参数是函数，所以被认为是高阶函数。此外，它的使用属于函数式编程范式。

感谢阅读！

沃兹