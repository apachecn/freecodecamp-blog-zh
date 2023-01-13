# 帮助您节省时间的 5 个 JavaScript 技巧

> 原文：<https://www.freecodecamp.org/news/time-saving-javascript-tips/>

我一直想围绕我的编程爱好创作视频。但是我的母语不是英语，我害怕尝试。

但是几周前，当我准备一些 JavaScript 技巧来开始我的 YouTube 之旅时，我写了这个节省时间的技巧列表。我希望他们能像帮助我一样帮助你。

在这篇文章中，我将与你分享 5 个有用的 JavaScript 技巧(你准备好开始了吗？😀).

现在，你猜怎么着？其中一些建议在我的 YouTube 频道上📹！(这里是[播放列表](https://www.youtube.com/playlist?list=PL-P9AMQZdy2aj3uLhjHagEsfTOrVqF7AR)。

## 对象析构

析构是 ES6 中引入的一个特性。一旦你知道怎么做，这是你每天都会用到的功能之一。

它帮助您处理三个主要问题:

*   **重复。**每当你想提取一个对象属性并创建一个新变量时，你就创建一个新行。

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

// Wow... should we display
// John's password like that?

const firstName = user.firstName;
const lastName = user.lastName;
const password = user.password; 
```

*   **可达性。**每次你想访问一个对象属性时，你应该写下它的路径。(**举例:**`user.firstName``user.family.sister`等等)。
*   **用法。**举个例子，当你创建一个新函数，而你只处理一个对象的一个属性。

现在你已经看到了对象的这三个问题是什么，你认为你可以如何解决它们？

### 如何解决重复问题

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

const { firstName, lastName, password } = user;

console.log(firstName, lastName, password);
// Output: 'John', 'Doe', '123' 
```

析构是通过对象的键从对象中提取属性的过程。通过获取对象中的一个现有键，然后将它放在两个括号(`{ firstName }`)之间，您告诉 JavaScript:

“嘿 JavaScript，我想创建一个与我的属性同名的变量。我想为我的对象的属性`firstName`创建一个变量`firstName`

> 注意:如果你想析构一个对象，你应该总是使用一个现有的键。否则，它将不起作用。

### 如何解决可访问性问题

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
  family: {
    sister: {
      firstName: "Maria",
    },
  },
};

// We access to the nested object `sister`
// and we extract the `firstName` property
const { firstName } = user.family.sister;

console.log(firstName);
// Output: 'Maria' 
```

当您使用嵌套对象时，它会变得非常重复，并且会浪费大量时间多次访问相同的属性。

使用析构，仅在一行中，您就可以将属性路径减少到一个变量。

### 如何解决使用问题

现在你知道了如何去析构一个对象，让我向你展示如何在你的函数参数定义中直接提取属性。

如果你知道 React，你可能已经熟悉它了。

```
function getUserFirstName({ firstName }) {
  return firstName;
}

const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

console.log(getUserFirstName(user));
// Output: 'John' 
```

在上面的例子中，我们有一个`getUserFirstName`函数，我们知道它只会使用我们对象的一个属性`firstName`。

我们可以析构对象的函数参数，而不是传递整个对象或创建一个新变量。

## 如何在 ES6 中合并对象

在编程中，你经常需要处理数据结构的问题。由于在 ES6 中引入了扩展操作符,对象和数组操作变得更加简单。

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

const userJob = {
  jobName: "Developer",
  jobCountry: "France",
  jobTimePerWeekInHour: "35",
}; 
```

假设我们有两个对象:

*   **用户。**定义用户一般信息的对象。
*   用户作业。定义用户的工作信息的对象。

我们想要创建一个只包含这两个对象属性的对象。

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

const userJob = {
  jobName: "Developer",
  jobCountry: "France",
  jobTimePerWeekInHour: "35",
};

const myNewUserObject = {
  ...user,
  ...userJob,
};

console.log(myNewUserObject);
// Output:
//{
//  firstName: 'John',
//  lastName: 'Doe',
//  password: '123',
//  jobName: 'Developer',
//  jobCountry: 'France',
//  jobTimePerWeekInHour: '35'
//} 
```

使用扩展运算符(`...`)，我们可以将一个对象的所有属性提取到另一个对象。

```
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

const myNewUserObject = {
  ...user,
  // We extract:
  // - firstName
  // - lastName
  // - password
  // and send them to
  // a new object `{}`
}; 
```

### 如何合并数组

```
const girlNames = ["Jessica", "Emma", "Amandine"];
const boyNames = ["John", "Terry", "Alexandre"];

const namesWithSpreadSyntax = [...girlNames, ...boyNames];

console.log(namesWithSpreadSyntax);
// Output: ['Jessica', 'Emma', 'Amandine', 'John', 'Terry', 'Alexandre'] 
```

像对象一样，spread 运算符(`...`)将一个数组中的所有元素提取到另一个数组中。

```
const girlNames = ["Jessica", "Emma", "Amandine"];

const newNewArray = [
  ...girlNames,
  // We extract:
  // - 'Jessica'
  // - 'Emma'
  // - 'Amandine'
  // and send them to
  // a new array `[]`
]; 
```

### 如何删除数组重复项

因为数组是列表，所以可以有很多相同值的项。如果您想删除数组中的重复项，可以遵循下面的示例之一。

由于 ES6 的原因，其中一个只有一行，但是我把“旧的”例子放在那里，这样你可以比较。

#### 如何“用老方法”删除数组重复项

```
const animals = ["owl", "frog", "canary", "duck", "duck", "goose", "owl"];

const uniqueAnimalsWithFilter = animals.filter(
  // Parameters example: 'owl', 0, ['owl', 'frog', 'canary', 'duck', 'duck', 'goose', 'owl']
  (animal, index, array) => array.indexOf(animal) == index
);

console.log(uniqueAnimalsWithSet);
// Output: ['owl', 'frog', 'canary', 'duck', 'goose'] 
```

在上面的例子中，我们想要通过删除所有重复的来清理`animals`数组。

我们可以通过使用包含`indexOf`的函数`filter`来实现。

`filter`函数接受`animals`数组的所有元素(`animals.filter`)。那么对于每个事件，它提供:

*   当前值(**例如:** `duck`)
*   索引(**例:** 0)
*   初始数组(**示例:**`animals`数组= > `['owl', 'frog', 'canary', 'duck', 'duck', 'goose', 'owl']`)

我们将对每次出现的原始数组应用`indexOf`,并将`animal`变量(当前值)作为参数给出。

`indexOf`将返回当前值的第一个索引(**示例:**对于‘猫头鹰’该索引为 0)。

然后在过滤器内部，我们将`indexOf`的值与当前索引进行比较。如果相同，我们返回`true`，否则返回`false`。

`filter`将创建一个新数组，只包含返回值为`true`的元素。

所以，在我们的例子中:`['owl', 'frog', 'canary', 'duck', 'goose']`。

### 如何以“新方式”删除数组重复项

嗯，“老办法”理解起来很有意思，但是很长，有点难。现在让我们来看看新的方式:

```
const animals = ["owl", "frog", "canary", "duck", "duck", "goose", "owl"];

const uniqueAnimalsWithSet = [...new Set(animals)];

console.log(uniqueAnimalsWithSet);
// Output: ['owl', 'frog', 'canary', 'duck', 'goose'] 
```

让我们分开不同的步骤:

```
// 1
const animals = ["owl", "frog", "canary", "duck", "duck", "goose", "owl"];

// 2
const animalsSet = new Set(animals);

console.log(animalsSet);
// Output: Set { 'owl', 'frog', 'canary', 'duck', 'goose' }

// 3
const uniqueAnimalsWithSet = [...animalsSet];

console.log(uniqueAnimalsWithSet);
// Output: ['owl', 'frog', 'canary', 'duck', 'goose'] 
```

我们有一个`animals`数组，我们把它转换成一个`Set`，这是 ES6 中一种特殊类型的对象。

它的不同之处在于，它允许你创建一个唯一值的集合。

> **注意:** `Set`是唯一值的集合，但不是`Array`。

一旦我们有了具有唯一值的`Set`对象，我们需要将它转换回数组。

为此，我们使用 spread 操作符来析构它，并将所有属性发送到一个新的`Array`。

因为`Set`对象有唯一的属性，我们的新数组也将只有唯一的值。

## 如何使用三元运算符

你听说过一个只用一行写小条件的方法吗？

如果没有，是时候去掉你很多的`if`和`else`块，转换成小三元运算了。

让我们先看一个带有`console.log`的例子。其思想是检查变量的值，并有条件地显示输出。

```
const colour = "blue";

if (colour === "blue") {
  console.log(`It's blue!`);
} else {
  console.log(`It's not blue!`);
} 
```

这个例子是一个典型的例子，你可以使用[三元运算符](https://herewecode.io/blog/ternary-operator-in-javascript/)将这 5 `if`和`else`行减少到只有一行！

**一行就能统治一切！**

```
const colour = "blue";

colour === "blue" ? console.log(`It's blue!`) : console.log(`It's not blue!`);
// [condition] ? [if] : [else] 
```

三元运算符代替`if`和`else`用于小条件。

> **注意:**不建议用三元运算符创建复杂条件，因为这样会降低可读性。

下面是另一个使用三元运算符的例子，但是这次是在函数的`return`中。

```
function sayHelloToAnne(name) {
  return name === "Anne" ? "Hello, Anne!" : "It's not Anne!";
}

console.log(sayHelloToAnne("Anne"));
// Output: 'Hello, Anne!'

console.log(sayHelloToAnne("Gael"));
// Output: "It's not Anne!" 
```

## 想投稿？以下是方法。

欢迎您对这个 GitHub 库做出贡献。感谢任何贡献，它将帮助我们每个人提高 JavaScript 技能。
[GitHub: JavaScript 牛逼小技巧](https://github.com/gael-thomas/javascript-awesome-tips)

## 结论

我希望你在阅读这篇文章的时候学到了一些关于 JavaScript 的新知识。

如果你想要更多这样的内容，你可以[在 Twitter 上关注我](https://twitter.com/gaelgthomas/),我在 Twitter 上发布了关于 web 开发、自我完善以及我作为全栈开发人员的旅程的信息！