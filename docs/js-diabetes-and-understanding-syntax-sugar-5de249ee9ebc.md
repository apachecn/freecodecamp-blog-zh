# 语法糖和 JavaScript 糖尿病

> 原文：<https://www.freecodecamp.org/news/js-diabetes-and-understanding-syntax-sugar-5de249ee9ebc/>

瑞安·尤尔卡宁

# 语法糖和 JavaScript 糖尿病

![1*54CJtxnsBXiO8Vlt-edX7w](img/da9a2dfa53c7dfe72f196ac9aaf14e21.png)

Be it stress eating or programming, sugar is always there for me. Photo by Greyson Jaralemon

句法糖是在编程语言中传达更大思想的简写。

我喜欢把它比作自然语言中的首字母缩写词。起初，看到一个新的缩写可能会感到困惑，但一旦你知道它的意思，它的方式更快！

用语法糖——就像用首字母缩写词——你可以 GTFAMLH！(过犹不及，日子更难过)

我刚从大学毕业，和我的朋友在黑客马拉松上制作有趣的应用程序，并开始了新的 JavaScript 惊险之旅。我觉得无法阻挡。我理解所有 Codecademy 的例子，我记住了每个前端面试问题。我看着[“什么……JavaScript？”](https://www.youtube.com/watch?v=2pL28CcEijU)太多次了，如果一只狂暴的猴子尖叫着将随机的代码行扔进控制台，我知道它会评估出什么。

该是我登上 GitHub，与世界分享我的礼物的时候了。我打开我能找到的第一个项目，开始阅读。它看起来像这样:

```
function init(userConfig) {
  const DEFAULT_CONFIG = {
    removeSpaces: false,
    allowHighlighting: true,
    priority: "high",
  }

  const config = { ...DEFAULT_CONFIG, ...userConfig };
}
```

片刻之后…

![1*Fyz7A2P4dj7jsBt0vKltZQ](img/40938833b53aca51b50b041f5ae83798.png)

When you search like you’re desperately trying to not lose a round of [Pictionary](https://en.wikipedia.org/wiki/Pictionary), you’re in trouble.

困惑和失败，我关闭了浏览器标签，退出了这一天。这将开始我做以下一连串的事情:

1.  发现一行当时只是 JavaScript 象形文字的代码。
2.  不知道如何问正确的问题，并且很可能是人类已知的最糟糕的谷歌搜索。
3.  困扰着随机开发人员，直到有人可以“像我 5 岁一样解释”，但最终，仍然不明白为什么有人会写这样的东西。虐待狂，*大概是*。

4.让它点击，了解它为什么有用，了解它解决了什么问题，了解人们过去做了什么来解决问题。这只是一种更简洁的编写代码的方式！只是糖而已！

5.有时，过多地使用 it way 会使我的代码在主观上变得更糟。

6.找到平衡，并为我的 JavaScript 工具箱添加一个很棒的工具。？

5.冲洗，重复 20 次左右。

现在我在这里试着简单地为你分解它！对于每一个甜言蜜语的技巧，我会包括一些背景故事，它可以帮助解决的问题，你如何在语法糖之前实现它，以及你可能不想使用它的情况！？

### 三元运算符

当谈到 JavaScript 中的糖时，三元运算符是我最喜欢开始使用的一个，因为它很容易走得太远。它通常采用`x ? a : b`的形式。这里有一个更现实的例子:

```
const amILazy = true;
const dinnerForTonight = amILazy ? "spaghetti" : "chicken";
```

问题:我有一个依赖于某个条件为真或假的变量。

**饮食解决方案:**这基本上是做`if/else`的一种速记方式！

```
const amILazy = true;
let dinnerForTonight = null;

if(amILazy) { dinnerForTonight = "spaghetti"; }
else { dinnerForTonight = "chicken"; }
```

**何时不用:** Ternaries 是一种非常简单的表示分支路径的方式。在我的主观看来，它们最糟糕的地方就是无限嵌套。所以，如果你喜欢工作保障，你可以写这个大脑熔化器。

```
const canYouFireMe = someCondition1 ?
  (someCondition2 ? false : true) : false
```

JavaScript 糖尿病的经典例子。**代码少并不意味着代码更简洁。**

### 对象扩展

啊，从一开始就让我心碎的例子。在 Javascript 中，当你根据上下文看到`...`、**时，它将是对象/数组展开，或者对象/数组静止。我们一会儿会谈到 Rest，所以先把它放一放。**

展开基本上就是取出一个对象，取出它的所有键/值对，并把它们放入另一个对象。下面是将两个对象扩展成一个新对象的基本示例:

```
const DEFAULT_CONFIG = {
  preserveWhitespace: true,
  noBreaks: false,
  foo: "bar",
};

const USER_CONFIG = {
  noBreaks: true, 
}

const config = { ...DEFAULT_CONFIG, ...USER_CONFIG };
// console.log(config) => {
//   preserveWhitespace: true,
//   noBreaks: true, 
//   foo: "bar",
// }
```

**问题:**我有一个对象，我想创建另一个对象，它有所有相同的键，所有相同的值。也许我想对多个对象这样做，如果有重复的键，选择哪个对象的键胜出。

**减肥方案:**你可以用`Object.assign()`到[达到类似的效果](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)。它接受任意数量的对象作为参数，当涉及到键时，优先考虑最右边的对象，最后对给定的第一个对象进行变异。一个常见的错误是没有将一个空对象作为第一个参数传入，并且无意中改变了一个参数。

如果这很难做到，你会很高兴知道对象扩散使这成为不可能。这里有一个复制语法糖版本的例子。

```
const DEFAULT_CONFIG = {
  preserveWhitespace: true,
  noBreaks: false,
  foo: "bar",
};

const USER_CONFIG = {
  noBreaks: true, 
}

// if we didn't pass in an empty object here, config
// would point to DEFAULT_CONFIG, and default config would be
// mutated
const config = Object.assign({}, DEFAULT_CONFIG, USER_CONFIG);
```

对象传播消除了意外突变的机会。因此，您可以做一些事情，比如更新 Redux 状态，而不用担心意外保留引用会导致浅层比较失败。

**？奖金？Ar** 射线传播的工作原理非常类似！但是由于数组中没有任何键，它只是像调用`Array.Prototype.concat`一样将键添加到新数组中。

```
const arr1 = ['a', 'b', 'c'];
const arr2 = ['c', 'd', 'e'];
const arr3 = [...arr1, ...arr2];
// console.log(arr3) => ['a', 'b', 'c', 'c', 'd', 'e']
```

### 对象析构

这是我在野外经常看到的。现在，我们有了前一个示例中的新配置对象，并希望在我们的代码中使用它。您可能会在代码库中看到类似这样的内容。

```
const { preserveWhiteSpace, noBreaks } = config;

// Now we have two new variables to play around with!
if (preservedWhitespace && noBreaks) { doSomething(); };
```

问题:必须写出一个对象中一个键的完整路径会变得相当沉重，并且阻塞了很多代码。为了更简洁，最好在值之外创建一个变量，以保持代码整洁。

饮食解决方案:你总是可以用老办法来做！看起来会像这样。

```
const preserveWhitespace = config.preserveWhitepsace;
const noBreaks = config.noBreaks;
// Repeat forever until you have all the variables you need

if (preservedWhitespace && noBreaks) { doSomething(); };
```

**什么时候不用:**你其实可以把一个对象析构出一个对象，继续析构越来越深！析构并不是从对象中获取密钥的唯一方法。如果你发现自己只对两三层深的键使用析构，很可能对项目弊大于利。

****？奖金？**** 数组也有析构，但是它们的工作基于离索引。

```
const arr1 = ['a', 'b']
const [x, y] = arr1
// console.log(y) => 'b'
```

### 载物台

对象静止与对象析构密切相关，很容易与对象扩散混淆。我们再次使用`...`操作符，然而上下文是**不同的**。这一次，它在析构时出现，旨在将剩余的键收集到一个对象中。？

```
const { preserveWhiteSpace, noBreaks, ...restOfKeys } = config;

// restOfKeys, is an object containing all the keys from config
// besides preserveWhiteSpace and noBreaks
// console.log(restOfKeys) => { foo: "bar" }
```

**问题:**您希望一个对象拥有另一个对象的键的子集。

**减肥方案:**你可以用我们的老朋友`Object.assign`和[删除](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete)任何你不需要的钥匙！？

**何时不使用:**用它创建一个新的对象，省略掉键是一个常见的用例。请注意，你在析构中省略的键仍然是浮动的，可能会占用内存。如果你不小心，这可能会导致一个错误。？

```
const restOfKeys = Object.assign({}, config);
delete restOfKeys.preserveWhiteSpace
delete restOfKeys.noBreaks
```

****？奖金？**** 猜猜是什么？数组可以做类似的事情，它的工作方式完全一样！

```
const array = ['a', 'b', 'c', 'c', 'd', 'e'];
const [x, y, ...z] = array;
// console.log(z) = ['c', 'c', 'd', 'e']
```

![1*ZeRdmY8ppcSqsW7G66unCw](img/83320fb6601f3f63b4ceae092089fea0.png)

Diabetes or side-effects of reading this whole article?

### 包扎

JavaScript sugar 很棒，了解如何阅读它可以让你输入更多样化的代码库，拓展你作为开发者的思维。请记住，这是一种平衡行为，既要做到简洁，又要让你的代码对别人和你未来的自己都可读。

虽然炫耀你闪亮的新工具感觉很棒，但作为程序员，我们的工作是让代码库比我们进入时更容易维护。

如果你想做进一步的阅读，这里有一些我所涉及的 MDN 文档。？

*   [三元运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
*   [传播语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
*   [销毁作业](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
*   [其余参数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)