# 何时将 JavaScript 常量大写

> 原文：<https://www.freecodecamp.org/news/when-to-capitalize-your-javascript-constants-4fabc0a4a4c4/>

许多 JavaScript 风格指南建议将常量名大写。就我个人而言，我很少看到这种约定用在我认为应该用的地方。这是因为我对常数的定义有点偏差。我决定做一些调查，对这个惯例更加熟悉一些。

#### 我们如何定义“常数”这个术语？

在程序设计中，常量是不变的。

> [正常执行](https://en.wikipedia.org/wiki/Constant_(computer_programming))时程序不能更改的值。

那么，JavaScript 是否给了我们一种方法来声明一个不能改变的值呢？在回答这个问题之前，我们先来看看这个约定的根源。

#### 大写约定起源于 C

c 是一种编译语言。这意味着另一个程序在运行之前将你所有的代码转换成机器码。

另一方面，JavaScript 是一种解释型语言。解释器在你的代码运行时一行一行地读取它。

编译和解释之间的差异对我们如何在 c 中声明常量值起着重要作用。

在 C 语言中，我可以这样声明一个变量:

`int hoursInDay = 24;`

或者，这样一个常数:

`#define hoursInDay 24`

第二个例子叫做符号常量。符号常量可以是字符序列、数字常量或字符串。这些也称为原始值。JavaScript 中的原始值是字符串、数字、布尔值、null、未定义、符号(不要与符号常量混淆)和 big int。

现在，让我们重温一下编译。

在编译之前，有一个预编译阶段。这里，预编译器用相应的值替换符号常量的所有实例。编译器永远不知道程序员写了`hoursInDay`。它只看到数字`24`。

大写有助于程序员看到这些真正的常量值。

`#define HOURS_IN_DAY 24`

#### JavaScript 常量不同于符号常量

在 ES6 之前，我们将大多数值存储在变量中，甚至是那些您希望保持不变的值。

大写帮助我们看到我们想要保持不变的价值。

```
var HOURS_IN_DAY = 24;
var hoursRemaining = currentHour - HOURS_IN_DAY;
var MY_NAME = 'Brandon';
MY_NAME = ... // oops don't want to do this.
```

ES6 引入了声明`const` ,从最纯粹的意义上来说，它不是一个“常数”。

ES6 添加了术语`const`和`let`作为创建具有不同意图的变量的方法。

对于这两个术语，您可能认为我们要么:

1.  不需要大写任何东西，因为我们可以清楚地看到哪些变量打算保持不变，或者
2.  我们应该大写我们用`const`声明的一切。

根据定义，`const`创建一个常量，它是一个值的只读引用。这并不意味着它的值是不可变的。它只说变量标识符[不能被重新分配](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)。

换句话说，一些`const`引用可以改变。

```
const firstPerson = {
  favoriteNumber: 10,
};
const secondPerson = firstPerson;
console.log(secondPerson.favoriteNumber); //10
firstPerson.favoriteNumber +=1;
console.log(secondPerson.favoriteNumber); //11
```

上面的例子表明声明`const`不能确保变量是不可变的。

仅阻止我们尝试重新分配变量名。它不会阻止对象属性的更改。记住:对象是按引用传递的。

```
// "TypeError: Assignment to constant variable."secondPerson = 'something else';
const secondPerson = 'Me'
secondPerson = 'something else';
```

因此，对于 JavaScript，我们必须超越仅仅寻找一个`const`声明。我们需要问两个问题来确定一个变量是否是常数:

1.  变量的值是原语吗？
2.  我们打算在整个程序中保持变量名指向同一个值吗？

如果两个问题的答案都是肯定的，我们应该用`const`声明变量，并且可以将名称大写。

注意我说的是"可能"这种约定的精神来自不同的语言，它们有实际的常量。JavaScript 没有。至少在最纯粹的意义上。这可能是为什么你看到这个约定的次数比你想象的要少。Airbnb 的风格指南中有很大一部分介绍了他们的观点。

**关键的一点是**认识到在 JavaScript 中定义一个常量必须包含程序员的意图。

此外，并非一种语言中的每个约定在另一种语言中都有意义。最后，我只能想象在 ide 拥有今天的功能之前，很多约定都被使用了。我确信我的 IDE 乐于告诉我我错了。这种事经常发生。

感谢阅读！

沃兹

在推特上关注我。

#### 笔记

*   你可能想知道为什么我没有在这些例子中使用`PI`。首字母缩写词——尤其是两个字母的首字母缩写词——按照惯例要么总是大写，要么总是小写。