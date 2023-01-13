# 如何在 JavaScript 中析构对象

> 原文：<https://www.freecodecamp.org/news/how-to-destructure-objects-javascript/>

从 ECMAScript 6(或简称 ES6)开始，可以用 JavaScript 来描述对象。作为一名 JavaScript 开发人员，您可能会将析构对象作为日常工作的一部分。

让我们了解一下为什么使用析构，然后我们将学习如何析构 JavaScript 对象。

# 为什么要析构一个对象？

让我们看一个例子来帮助我们理解为什么我们首先要析构一个对象。让我们创建一个名为`pet`的对象:

```
const pet:  = {
 name: 'Captain',
 food: 'Kibble',
 color: 'Black'
};
```

假设我们想简单地打印出宠物的食物。我们可以这样做:

```
console.log(pet.food);
```

这将输出`kibble`到控制台。但是，每次我们需要食物值的时候都要写出`pet.name`就很繁琐了。所以在 ES6 之前，JavaScript 开发人员会这样做:

```
const food = pet.food;
```

在这里，我们声明了一个名为`food`的变量，并表示将该变量指向存储在`pet.food`内部的值，即`kibble`。我们可以通过添加这行代码来证明我们已经做到了这一点:

```
console.log(food); // output: kibble
```

然而，假设我们想对`pet`对象中的其余属性做同样的事情:

```
const name = pet.name;
const food = pet.food;
const color = pet.color;
```

这开始变得冗长。这就是**析构**的用武之地。

# 如何在 JavaScript 中析构对象

代替写出`const food = pet.food`，我们可以利用析构，简单地写出:

```
const { food } = pet;
```

现在，如果我们再次打印出`food`，我们可以看到我们已经创建了`food`变量:

```
console.log(food); // output: kibble
```

使用析构，我们的代码现在更加简洁和干巴巴。这就是为什么 JavaScript 开发人员在 ES6 中引入析构之后就开始使用它了。

让我们暂停一会儿，再看看这一行:

```
const { food } = pet;
```

这里发生了什么事？

记住，在 JavaScript 对象中，**属性**是一个键/值对。键是一个字符串，值可以是任何数据类型。在`pet`对象中，其中一个键是`food`，它对应的值是`kibble`。

当我们将我们的`food`变量放在括号中时，我们在我们的`pet`对象内部寻找具有相同名称的属性。我们创建一个名为`food`的新变量，并将它的值设置为`kibble`，即该键的对应值。

如果你想从一个对象中析构多个属性和它们的键，你可以这样做:

```
const { name, food, color } = pet;
```

现在如果你打印出这些变量，你会看到它们现在都存在:

```
console.log(name, food, color) // output: captain chewtoy black
```

在这篇文章中，我们学习了如何析构对象以及为什么要这样做。我们还学习了如何从一个对象中析构多个属性。

感谢您的阅读！

如果你喜欢这篇文章，加入我的[编码俱乐部](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，在那里我们每周日一起应对编码挑战，并在学习新技术时相互支持。

如果你对这篇文章有任何反馈或问题，或者在 Twitter 上找到我[@ madisonkana](https://twitter.com/Madisonkanna)。