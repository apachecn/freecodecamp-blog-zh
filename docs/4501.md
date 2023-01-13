# 6 分钟学会 JavaScript 闭包

> 原文：<https://www.freecodecamp.org/news/learn-javascript-closures-in-n-minutes/>

众所周知，闭包很难掌握。但是它们对于 JavaScript 开发人员的发展至关重要。

理解闭包可以带来更优雅的代码和更好的工作机会。

我希望这篇文章有助于尽快坚持这个概念。

奖励:闭包不是 JS 特有的！它们是计算机科学的概念——一旦你学会了它们——你就会开始认识到软件开发中的其他地方。

## 函数也是值

首先，理解 JavaScript 支持*一级函数*。

![winnie-1](img/c8d82de0ed0a2f4ae92c0b13add8d479.png)

一个有趣的名字，但它只是意味着函数*被当作任何其他值*一样对待。字符串、数字和对象等值。

你能用价值观做什么？

### 值可以是变量

```
const name = 'Yazeed';
const age = 25;
const fullPerson = {
    name: name,
    age: age
}; 
```

### 值可以在数组中

```
const items = [
    'Yazeed',
    25,
    { name: 'Yazeed', age: 25 }
] 
```

### 值可以从函数返回

```
function getPerson() {
    return [
        'Yazeed',
        25,
        { name: 'Yazeed', age: 25 }
    ];
} 
```

你猜怎么着？函数也可以如此。

![functions-can-do-that-too](img/e16a2f5aa7fccd079979fd0b89dc4caf.png)

### 函数可以是变量

```
const sayHi = function(name) {
    return `Hi, ${name}!`;
} 
```

### 函数可以在数组中

```
const myFunctions = [
    function sayHi(name) {
        return `Hi, ${name}!`;
    },
    function add(x, y) {
        return x + y;
    }
]; 
```

这是最大的一个...

## 函数可以返回*其他函数*

返回另一个函数的函数有一个特殊的名字。它被称为一个高阶函数。

这是闭包存在的基础。这是我们的第一个*高阶*函数的例子。

```
function getGreeter() {
    return function() {
        return 'Hi, Jerome!';
    };
} 
```

`getGreeter`返回函数。要打招呼，叫两下。

```
getGreeter(); // Returns function
getGreeter()(); // Hi, Jerome! 
```

一次调用返回的函数，另一次调用问候语。

您可以将它存储在一个变量中，以便于重用。

```
const greetJerome = getGreeter();

greetJerome(); // Hi, Jerome!
greetJerome(); // Hi, Jerome!
greetJerome(); // Hi, Jerome! 
```

## 做个了结

现在是盛大的揭幕仪式。

我们将通过接受一个名为`name`的参数来使`getGreeter`动态化，而不是硬编码 Jerome。

```
// We can greet anyone now!
function getGreeter(name) {
    return function() {
        return `Hi, ${name}!`;
    };
} 
```

像这样使用它...

```
const greetJerome = getGreeter('Jerome');
const greetYazeed = getGreeter('Yazeed');

greetJerome(); // Hi, Jerome!
greetYazeed(); // Hi, Yazeed! 
```

![fallout-hold-up](img/79ea244dff3c5683a6d608de03a6477a.png)

再看看这段代码。

```
function getGreeter(name) {
    return function() {
        return `Hi, ${name}!`;
    };
} 
```

## 我们用了一个结尾

*外部*函数使用`name`，但是*内部*函数稍后使用它。这就是闭包的力量。

当一个函数返回时，它的生命周期就结束了。它不能再执行任何工作，并且它的局部变量被清除。

除非它返回另一个函数。如果发生这种情况，那么返回的函数仍然可以访问这些外部变量，即使在父变量传递之后。

## 关闭的好处

![why-do-i-care](img/1b9848e6386398e2ddeeeffa9df0e10e.png)

就像我说的，闭包可以提升你的开发者游戏。下面是几个实际用途。

## 1.数据保密

数据隐私对于安全共享代码至关重要。

没有它，任何使用你的函数/库/框架的人都可以恶意操纵它的内部变量。

### 没有隐私的银行

考虑这个管理银行账户的代码。`accountBalance`全球曝光！

```
let accountBalance = 0;
const manageBankAccount = function() {
    return {
        deposit: function(amount) {
            accountBalance += amount;
        },
        withdraw: function(amount) {
            // ... safety logic
            accountBalance -= amount;
        }
    };
} 
```

是什么阻止我膨胀我的平衡或者破坏别人的平衡？

```
// later in the script...

accountBalance = 'Whatever I want, muhahaha >:)'; 
```

![who-reset-my-balance-this-time](img/841f4174151fac8ee588361a9937bbd7.png)

像 Java 和 C++这样的语言允许类拥有私有字段。这些字段不能在类外访问，从而实现了完美的隐私。

JavaScript 不支持私有变量([但](https://github.com/tc39/proposal-class-fields))，但是我们可以使用闭包！

### 有适当隐私的银行

这次`accountBalance`坐在*我们的经理*里面。

```
const manageBankAccount = function(initialBalance) {
    let accountBalance = initialBalance;

    return {
        getBalance: function() { return accountBalance; },
        deposit: function(amount) { accountBalance += amount; },
        withdraw: function(amount) {
            if (amount > accountBalance) {
                return 'You cannot draw that much!';
            }

            accountBalance -= amount;
        }
    };
} 
```

或许像这样使用它...

```
const accountManager = manageBankAccount(0);

accountManager.deposit(1000);
accountManager.withdraw(500);
accountManager.getBalance(); // 500 
```

注意我不能再直接访问`accountBalance`了。我只能通过`getBalance`查看，通过`deposit`和`withdraw`更改。

这怎么可能？闭包！

即使`manageBankAccount`创建了`accountBalance`变量，它返回的三个函数都可以通过闭包访问`accountBalance`。

![i-wish-my-bank-did-that](img/41a822e04e582ee2c873126b430fb6d8.png)

## 2.Currying

在之前，我已经写过关于奉承的文章。当一个函数一次接受一个参数时。

所以代替这个...

```
const add = function(x, y) {
    return x + y;
}

add(2, 4); // 6 
```

你可以利用闭包来*咖喱* `add`...

```
const add = function(x) {
    return function(y) {
        return x + y;
    }
} 
```

你知道返回的函数可以访问`x`和`y`，所以你可以这样做...

```
const add10 = add(10);

add10(10); // 20
add10(20); // 30
add10(30); // 40 
```

如果你想“预装”一个函数的参数以便于重用，Currying 是很好的选择。同样，只有通过 JavaScript 闭包才有可能！

## 3.React 开发人员使用闭包

如果你一直关注 React 新闻，你会听说他们去年发布了《T2》。最令人困惑的钩子`useEffect`依赖于闭包。

本文没有完整的 React 教程，所以我希望这个例子足够简单。

[https://codesandbox.io/embed/react-hooks-closures-example-2kixb?fontsize=14](https://codesandbox.io/embed/react-hooks-closures-example-2kixb?fontsize=14)

### 这是最重要的部分...

```
function App() {
  const username = 'yazeedb';

  React.useEffect(function() {
    fetch(`https://api.github.com/users/${username}`)
      .then(res => res.json())
      .then(user => console.log(user));
  });

  // blah blah blah
} 
```

更改代码中的`username`,注意它将获取用户名并将输出记录到控制台。

这又是一次闭包。`username`被定义在*外部*函数内部，但是`useEffect`的*内部*函数实际上使用了它。

## 摘要

1.  函数也是值。
2.  函数可以返回其他函数。
3.  一个外部函数的变量仍然可以被它的内部函数*访问，甚至在外部函数传递给*之后。
4.  这些变量也被称为状态。
5.  所以闭包也可以叫做*有状态*函数。

## 想要免费辅导？

如果你想安排一个**免费的** 15-30 分钟的电话，讨论关于代码、面试、职业或任何其他方面的前端开发问题[请在 Twitter 上关注我，并给我发短信](https://twitter.com/yazeedBee)。

之后，如果你喜欢我们的第一次会议，我们可以讨论一个持续的教练关系，这将有助于你达到你的前端发展目标！

## 感谢阅读

更多类似的内容，请查看[https://yazeedb.com！](https://yazeedb.com)

下次见！