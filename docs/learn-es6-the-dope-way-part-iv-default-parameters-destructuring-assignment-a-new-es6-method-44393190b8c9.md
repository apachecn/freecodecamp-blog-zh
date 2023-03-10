# 学习 ES6 的掺杂方式第四部分:默认参数，析构赋值，和一个新的方法！

> 原文：<https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iv-default-parameters-destructuring-assignment-a-new-es6-method-44393190b8c9/>

欢迎来到 **Learn ES6 The Dope Way** 的第四部分，该系列旨在帮助您轻松理解 ES6 (ECMAScript 6)！

今天让我们探索两个新的 ES6 概念并介绍一种新方法！

*   默认函数参数
*   解构分配
*   一种新的 ES6 方法❤

#### 默认函数参数

好处:

*   当您需要函数中的默认值时非常有用。
*   传入 *undefined* 时，仍然会使用默认值代替！

当心:

*   如果您将一个函数设置为另一个函数内部的默认值，它将抛出一个 ReferenceError
*   当您调用函数时，输入值的位置将影响您是否到达具有默认值的参数。例如，如果您有两个参数，并且想要到达第二个参数，那么您只需要在您正在调用的函数中输入一项。因为第二个参数会丢失，所以默认值会出现在那里。更多解释见下面的例子。

如果你曾经想创建一个将默认值作为备份的函数…恭喜你！这光荣的一天终于到来了！

![ZRTmNPBsvuaLTCz8U0fFrX7mzhY7jZxxvMu4](img/cab8ee132c6d9818553247c498e14376.png)

如果没有传递任何值，或者如果传递了*未定义的*，默认函数参数允许您初始化默认值。以前，如果你有这样的东西:

```
function add(x, y) {
  console.log(x+y);
}
add(); // => NaN
```

你会得到 *NaN* ，而不是一个数字。但是现在你可以这样做:

```
function add(x=5, y=7) {
  console.log(x+y);
}
add(); // => 12
```

你得 12 分！这意味着如果您在调用这个函数时没有明确地向它添加值，它将使用默认值。所以你也可以这样做:

```
function add(x=5, y=7) {
  console.log(x+y);
}
add(12, 15); // => 27
add(); // => 12

// AND THIS:
function haveFun(action='burrowing', time=3) {
  console.log(`I will go ${action} with Bunny for ${time} hours.`)
}
haveFun(); // => I will go burrowing with Bunny for 3 hours.
haveFun('swimming', 2); // => I will go swimming with Bunny for 2 hours.
```

当您调用该函数时，将根据您输入输入值的位置来覆盖默认值。例如:

```
function multiply(a, b = 2) {
  return a*b;
}
multiply(3) // => 6 (returns 3 * 2)
multiply(5, 10) // => 50 (returns 5 * 10 since 10 replaces the default value)
```

当传递未定义的值时，仍然选择默认值:

```
// TEST IT HERE: http://goo.gl/f6y1xb
function changeFontColor(elementId, color='blue') {
  document.getElementById(elementId).style.color = color;
}
changeFontColor('title') // => sets title to blue
changeFontColor('title', 'pink') // => sets title to pink
changeFontColor('title', undefined) // => sets title to blue
```

如果没有为参数指定默认值，它将正常返回 undefined:

```
function test(word1='HeyHeyHey', word2) {
  return `${word1} there, ${word2}!`
}
test(); // => HeyHeyHey there, undefined!

// IMPORTANT:
// In order to reach the second parameter and overwrite the default function,
// we need to include the first input as well:
test('Hi', 'Bunny') // => Hi there, Bunny!
```

#### 解构分配

好处:

*   从数组和对象中提取数据，并将它们赋给变量
*   简化所需的击键次数，并提高可读性
*   当需要传入大量具有相同属性的数据(如用户配置文件)时非常有用

当心:

*   开始时理解起来可能有点复杂，但是一旦理解了它的好处，只需回顾提供的示例并进一步研究。你会找到窍门的！:)

让我们后退一步，了解一下析构赋值，以及它是如何与数组、对象，甚至与默认参数结合使用的！

首先，让我们通过创建一个兔子最喜欢的食物的数组来练习数组。我们*可以用传统方式*访问数组中的第一个和第五个项目:

```
var BunnyFavFoods = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(BunnyFavFoods[0]) // => Carrots
console.log(BunnyFavFoods[4]) // => Papaya
```

或者我们可以使用析构赋值！我们通过删除变量名并传入一个括号来实现这一点，当我们调用它时，括号将指向我们希望数组中包含的项:

```
var [firstItem, fifthItem] = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(firstItem) // => Carrots
console.log(fifthItem) // => Carrot Bits
```

哇哇哇！刚刚发生了什么？我们的木瓜呢？

啊哈！抓到你了。

![VhvNuGj9otpoPJB9N1vIctw0Dt7yjLG8SgQg](img/8837238674f33cb1d7daad5a2c397813.png)

看看这个——*第一项*和*第五项*只是单词。这里真正的诀窍是它们被放置在哪里。您放在括号中的单词的位置将对应于您想要的项目在数组中的位置。

这就是为什么括号中的第一个词— *firstItem —* 对应于数组'*胡萝卜*'中的第一个词，第二个词— *fifthItem —* 对应于数组中的第二个词'*胡萝卜位*'。

以下是使用同一个单词访问不同位置的方法:

```
// Every additional comma added will represent the next item in the array.
var [firstItem,,,,fifthItem] = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(firstItem) // => Carrots
console.log(fifthItem) // => Papaya

// Wohoo! Let’s try some more! Which item in the array will this get?
var [firstItem,,guessThisItem,,fifthItem] = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(firstItem) // => Carrots
console.log(guessThisItem) // => Grass
console.log(fifthItem) // => Papaya

// Are you noticing a pattern? One comma separates one word from another and 
// every additional comma before a word represents a place in the array.
// Ok, What would happen if we added a comma to the front?
var [,firstItem,,guessThisItem,,fifthItem] = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(firstItem) // => Carrot Bits
console.log(guessThisItem) // => Berries
console.log(fifthItem) // => Apples

// Everything moves one place over!
// And what if we moved everything back and added a word to the end?
var [firstItem,,guessThisItem,,fifthItem, whichOneAmI] = ['Carrots', 'Carrot Bits', 'Grass', 'Berries', 'Papaya', 'Apples'];
console.log(firstItem) // => Carrots
console.log(guessThisItem) // => Grass
console.log(fifthItem) // => Papaya
console.log(whichOneAmI) // => Apples
```

在你的主机上玩玩这段代码，这样你可以更好地理解这个新概念，并在评论区告诉我们你的发现。:)

好了，我们已经得到了数组，那么现在如何用对象来解构赋值呢？让我们首先看看我们访问对象中的项目的典型方式:

```
var iceCream = {
  cost: 3.99,
  title: 'Ice Cream Flavors',
  type: ['chocolate', 'vanilla', 'caramel', 'strawberry', 'watermelon']
}

console.log(iceCream.cost, iceCream.title, iceCream.type[2]); 
//=> 3.99 ‘Ice Cream Flavors’ ‘caramel’
```

现在，让我们使用与数组类似的方法来析构这个对象。去掉变量名，在它的位置加上花括号——因为这是一个对象——就像我们给数组加括号一样。

在花括号内，传入我们想要访问的对象属性:

```
var {cost, title, type} = {
  cost: 3.99,
  title: 'Ice Cream Flavors',
  type: ['chocolate', 'vanilla', 'caramel', 'strawberry', 'watermelon']
}

// VOILA!
console.log(cost, title, type[2]) 
//=> 3.99 'Ice Cream Flavors' 'caramel'
```

下面是使用析构的一种稍微复杂但有用的方法:

假设您有一个函数，您想访问所有属性相同但值不同的对象。这对于大型数据集(如用户配置文件)尤其有用。但在这个例子中，我们将使用兔子最喜欢的东西来说明这个概念:

```
var iceCream = {
  cost: 3.99,
  name: 'Ice Cream Flavors',
  type: ['chocolate', 'vanilla', 'caramel', 'strawberry', 'watermelon']
}

var sushi = {
  cost: 5.99,
  name: 'Sushi Combinations',
  type: ['Eel Roll', 'Philadelphia Roll', 'Spicy Salmon Handroll', 'Rainbow Roll', 'Special Roll']
}

var fruit = {
  cost: 1.99,
  name: 'Fruits', 
  type: ['cherry', 'watermelon', 'strawberry', 'cantaloupe', 'mangosteen']
}

function favThings({cost, name, type}) {
  var randomNum = Math.floor((Math.random() * 4) + 1);
  console.log(`Bunny loves her ${name}! She especially loves ${type[randomNum]} for only ${cost}!`);
}

// Randomly generated for the type parameter.
// First time:
favThings(iceCream) // => Bunny loves her Ice Cream Flavors! She especially loves caramel for only $3.99!
favThings(sushi) // => Bunny loves her Sushi Combinations! She especially loves Philadelphia Roll for only $5.99!
favThings(fruit) // => Bunny loves her Fruits! She especially loves cantaloupe for only $1.99!

// Second time:
favThings(iceCream) // => Bunny loves her Ice Cream Flavors! She especially loves vanilla for only $3.99!
favThings(sushi) // => Bunny loves her Sushi Combinations! She especially loves Spicy Salmon Handroll for only $5.99!
favThings(fruit) // => Bunny loves her Fruits! She especially loves mangosteen for only $1.99!

// Try it in the console yourself and see what you get!
```

刚刚发生了什么？

当我们传入我们的对象(冰淇淋、寿司、水果)时，favThings 函数解析它并允许我们访问这些属性，因为我们在每个对象中使用了相同的属性名。

#### 将析构赋值与默认参数相结合

研究下面的例子:

```
function profilePage({favColor: favColor} = {favColor: 'vintage pink'}, [name, age] = ['Bunny', 24]) {
  console.log(`My name is ${name}. I am ${age} years old and my favorite color is ${favColor}!`)
}

profilePage(); 
// => My name is Bunny. I am 24 years old and my favorite color is vintage pink!
profilePage({favColor: 'blue'}, ['Ed', 30]) 
// => My name is Ed. I am 30 years old and my favorite color is blue!
```

或者，如果您已经准备好了一个对象和数组进行析构:

```
var aboutEdward = {
  info: ['Edward', 30],
  favColor: 'blue',
  favSushiRoll: 'Squidy squid squid'
}

function profilePage({favColor} = {favColor: 'vintage pink'}, [name, age] = ['Bunny', 24]) {
  console.log(`My name is ${name}. I am ${age} years old and my favorite color is ${favColor}!`)
}
profilePage(); 
// => My name is Bunny. I am 24 years old and my favorite color is vintage pink!
profilePage(aboutEdward, aboutEdward.info); 
// => My name is Edward. I am 30 years old and my favorite color is blue!
```

#### 一种新的 ES6 方法❤

好处:

*   不使用自己的算法重复字符串

当心:

*   负数和无穷大将导致*范围错误*
*   十进制数将被四舍五入为整数

你见过那个算法吗，当你刚开始学习算法的时候，它会让你重复一个单词或字符串几次。

恭喜你！

你重复字符串算法的日子结束了！

![Gmhzyylidg2RgnlYdMijrfnnyy7Z1zV6rrHV](img/d58401a27307c47f4140856b2c456da0.png)

介绍新的*重复。()*ES6 带给你的方法！

它是这样工作的:

```
// The general syntax: str.repeat(count);

// Examples:
'Bunny'.repeat(3); // => BunnyBunnyBunny
'Bunny'.repeat(2.5)// => BunnyBunny
'Bunny'.repeat(10/2) // => BunnyBunnyBunnyBunnyBunny
'Bunny'.repeat(-3) // => RangeError: Invalid count value
'Bunny'.repeat(1/0) // => RangeError: Invalid count value
```

不过，如果你正在读这篇文章，并且正在学习算法或者还没有开始学习，我强烈建议你创建一个函数来重复一个字符串，不要使用这个方法，因为那样会违背学习和解决挑战的目的。一旦你记下来了，就尽情地使用这个方法吧。YIPEE！

恭喜你。你已经通过了第四部分的**学习 ES6，现在你已经掌握了两个超级重要的 ES6 概念:默认函数参数和析构赋值，还学会了一个有趣的重复字符串的新方法！耶！去你的！**

请记住，如果你想使用 ES6，仍然存在浏览器兼容性问题，所以在发布你的代码之前，使用像 *Babel* 这样的编译器或者像 *Webpack* 这样的模块捆绑器。所有这些都将在未来的**中讨论。感谢阅读** **❤**

随着越来越多的**学习 ES6,**即将进入中级，通过喜欢和关注保持你的智慧更新！

**第一部分:const、let &有**

**[第二部分:(箭头)= >功能和](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-ii-arrow-functions-and-the-this-keyword-381ac7a32881/)关键字**

**[第三部分:模板文字，传播操作符&生成器！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iii-template-literals-spread-operators-generators-592765337294/)**

第四部分:默认参数、析构赋值和一个新的 ES6 方法！

**[第五部分:类，翻译 ES6 代码&更多资源！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661/)**

你也可以在 https://github.com/Mashadim 的 github ❤网站上找到我