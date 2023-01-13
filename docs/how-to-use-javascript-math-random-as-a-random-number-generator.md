# 如何使用 JavaScript Math.random()作为随机数生成器

> 原文：<https://www.freecodecamp.org/news/how-to-use-javascript-math-random-as-a-random-number-generator/>

通常在开发项目时，您会发现自己在寻找生成随机数的方法。

生成随机数最常见的用例是像掷骰子、洗牌和旋转轮盘这样的机会游戏。

在本指南中，您将通过构建一个迷你骰子游戏，学习如何使用`Math.random()`方法生成一个随机数。

## Math.random()方法

JavaScript 中的`Math`对象是一个内置对象，它具有用于执行数学计算的属性和方法。

`Math`对象的一个常见用途是使用`random()`方法创建一个随机数。

```
const randomValue = Math.random();
```

但是`Math.random()`方法实际上并不返回一个整数。相反，它返回一个介于 0(含)和 1(不含)之间的浮点值。另外，注意从`Math.random()`返回的值本质上是伪随机的。

由`Math.random()`生成的随机数可能看起来是随机的，但是这些数字会重复，并最终在一段时间内显示出非随机的模式。

这是因为算法随机数生成在本质上永远不可能是真正随机的。这就是我们称之为伪随机数发生器(PRNGs)的原因。

要了解更多关于`Math.random()`方法的信息，你可以查看[这本](https://www.freecodecamp.org/news/javascript-math-random-method-explained/)指南。

## 随机数发生器函数

现在让我们使用`Math.random()`方法创建一个函数，该函数将返回两个值之间的随机整数。

```
const getRandomNumber = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min;
};
```

我们来分解一下这里的逻辑。

`Math.random()`方法将返回一个介于 0 和 1(不含)之间的浮点数。

因此，时间间隔如下:

```
[0 .................................... 1)

[min .................................... max)
```

要计算第二个间隔，从两端减去 min。所以这会给你一个介于 0 和`max-min`之间的区间。

```
[0 .................................... 1)

[0 .................................... max - min)
```

所以现在，要得到一个随机值，你需要做如下的事情:

```
const x = Math.random() * (max - min)
```

这里`x`是随机值。

目前，`max`被排除在间隔之外。要使其具有包容性，请添加 1。此外，您需要将之前减去的`min`加回来，以获得`[min, max)`之间的值。

```
const x = Math.random() * (max - min + 1) + min
```

好了，现在剩下的最后一步是确保`x`总是一个整数。

```
const x = Math.floor(Math.random() * (max - min + 1)) + min
```

您可以使用`Math.round()`方法来代替`floor()`，但是那会给你一个不均匀的分布。这意味着`max`和`min`都有一半的机会出来作为结局。使用`Math.floor()`会给你完美的均匀分布。

现在，您已经对随机生成的工作原理有了相当的了解，让我们使用这个函数来模拟掷骰子。

## 滚动骰子游戏

在这一部分，我们将创建一个非常简单的迷你骰子游戏。两名玩家输入他们的名字，然后掷骰子。骰子点数高的玩家将赢得这一轮。

首先，创建一个函数`rollDice`来模拟掷骰子的动作。

在函数体内，用`1`和`6`作为参数调用`getRandomNumber()`函数。这将给你一个介于 1 和 6(包括 1 和 6)之间的任意随机数，就像真正的骰子是如何工作的一样。

```
const rollDice = () => getRandomNumber(1, 6);
```

接下来，创建两个输入字段和一个按钮，如下所示:

```
<div id="app">
      <div>
        <input id="player1" placeholder="Enter Player 1 Name" />
      </div>
      <div>
        <input id="player2" placeholder="Enter Player 2 Name" />
      </div>
      <button id="roll">Roll Dice</button>
      <div id="results"></div>
    </div>
```

当点击“掷骰子”按钮时，从输入字段中获取玩家姓名，并为每个玩家调用`rollDice()`函数。

```
const getRandomNumber = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

const rollDice = () => getRandomNumber(1, 6);

document.getElementById("roll").addEventListener("click", function () {
  // fetch player names from the input fields
  const player1 = document.getElementById("player1").value;
  const player2 = document.getElementById("player2").value;

  // roll dice for both players
  const player1Score = rollDice();
  const player2Score = rollDice();

  // initialize empty string to store result
  let result = "";

  // determine the result
  if (player1Score > player2Score) {
    result = `${player1} won the round`;
  } else if (player2Score > player1Score) {
    result = `${player2} won the round`;
  } else {
    result = "This round is tied";
  }

  // display the result on the page
  document.getElementById("results").innerHTML = `
  <p>${player1} => ${player1Score}</p>
  <p>${player2} => ${player2Score}</p>
  <p>${result}</p>
  `;
}); 
```

您可以验证玩家的姓名字段，并用一些 CSS 美化标记。为了简洁起见，我在本指南中保持简单。

就是这样。你可以点击查看演示[。](https://5zmdd.csb.app/)

## 结论

所以在 JavaScript 中生成随机数终究不是那么随机的。我们所做的只是接受一些输入数字，使用一些数学方法，然后吐出一个伪随机数。

对于大多数基于浏览器的应用程序和游戏来说，这种随机性就足够了，也符合它的目的。

本指南到此为止。保持安全，像野兽一样继续编码。