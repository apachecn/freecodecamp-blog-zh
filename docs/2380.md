# 如何在 JavaScript 和 Node.js 中使您的控制台输出有趣且具有交互性

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-console-log-statements-look-fun-and-interactive/>

在本教程中，您将学习如何在 JavaScript 和 Node.js 中向`console.log`语句添加随机延迟。

![ezgif.com-gif-maker](img/4b41a2f04c971b312992b24f312c7857.png)

## 你为什么想这么做？

首先，编程要有趣。而把`console.log`这种无聊的东西变得好看，是很赏心悦目的。

如果你想快速获得源代码，你可以看看这个 [GitHub 库](https://github.com/AgileNix/funkylog/)。

## 步骤 1:创建一个函数，该函数接受字符串并将其传递给 console.log

为了确保每一步都清晰，我们将从小处着手，创建一个接受字符串作为参数并将其记录到控制台的函数。

```
const log = (s) => {
  console.log(s);
}
```

## 步骤 2:逐个记录字符串中的字符

在我们可以在各个字符的输出之间添加延迟之前，我们需要确保它们实际上是分开的。

让我们添加一个`for`循环，遍历字符串的每个字母，并将其打印到控制台。

```
const log = (s) => {
  for (const c of s) {
    console.log(c);
  }
}
```

## 步骤 3:如何修复换行符问题

现在，每个字母都打印在一个新行上，因为每次调用`console.log`都会增加一个空行。

我们将把`console.log`替换为`***process***.stdout.write`，它本质上做同样的事情，但是不在输出后添加新的一行。

然而，现在我们在输出的最后丢失了换行符，这仍然是可取的。我们将通过显式打印`\n`字符来添加它。

```
const log = (s) => {
  for (const c of s) {
    process.stdout.write(c);
  }
  process.stdout.write('\n');
}
```

## 第四步:实现`sleep`功能

在 JavaScript 中，我们不能简单地停止同步代码的执行一段时间。为了实现这一点，我们需要编写自己的函数。姑且称之为睡眠吧。

它应该接受单个参数`ms`并返回一个承诺，该承诺在延迟`ms`毫秒后解决。

```
const sleep = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms));
};
```

## 第五步:添加延迟

所以，我们准备给输出添加一个延迟！我们需要一些东西:

*   给函数`log`添加一个参数`delay`
*   通过添加关键字`async`使函数`log`异步
*   调用一个`sleep`函数，将下一次循环迭代延迟`delay`毫秒

```
const sleep = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms));
};

const log = async (s, delay) => {
  for (const c of s) {
    process.stdout.write(c);
    await sleep(delay);
  }
  process.stdout.write('\n');
}
```

## 步骤 6:实现随机延迟

如果我们随机选择时间，输出会更好。

让我们给函数`log`添加另一个布尔参数`randomized`。如果为真，那么传递给`sleep`的参数应该在从`0`到`delay`毫秒的范围内。

```
const sleep = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms));
};

const log = async (s, delay, randomized) => {
  for (const c of s) {
    process.stdout.write(c);
    await sleep((randomized ? Math.random() : 1) * delay);
  }
  process.stdout.write('\n');
}
```

我使用了一个三元运算符，但是您可以用一个常规的`if`语句来代替它:

```
if (randomized) {
  sleep(Math.random * delay);
} else {
  sleep(delay);
}
```

## 步骤 7:使日志可配置

现在，我们已经实现了几乎所有我们想要的东西。但是调用它并不是很干净，因为每次我们想打印一些东西到控制台时，我们都必须传递`delay`和随机化标志。

```
log('Hello, world!', 100, true);
log('What\'s up?', 100, true);
log('How are you?', 100, true);
```

如果我们能有一个可配置的日志，可以用一个参数调用，那就太好了——一个我们想要输出的字符串。

为此，我们必须重写代码。计划是这样的:

*   将所有当前功能打包到一个函数`funkylog`中，该函数接受一个具有两个字段`delay`和`randomized`的对象
*   `funkylog`应该返回匿名箭头函数。它的实现应该与我们在步骤 1 到 6 中实现的`log`相同
*   参数`delay`和`randomized`应该从`log`函数中移除，因为现在它们将从`funkylog`中传递

```
const funkylog = ({ delay, randomized }) => {
  const sleep = (ms) => {
    return new Promise(resolve => setTimeout(resolve, ms));
  };

  return async (s) => {
    for (const c of s) {
      process.stdout.write(c);
      await sleep((randomized ? Math.random() : 1) * delay);
    }
    process.stdout.write('\n');
  }
};
```

## 第八步:画龙点睛

让我们来看看我们得到了什么:

```
const log = funkylog({ delay: 100, randomized: true });

log('Hello, world!');
log('What\'s up?');
log('How are you?');
```

*   我们可以使用函数`funkylog`创建一个可配置的记录器
*   我们可以选择任何我们想要的延迟
*   使用记录器并不要求我们每次调用它时都要传递`delay`

我们可以做的另一个改进是为`delay`参数提供一个默认值。

```
const funkylog = ({ delay = 100, randomized }) => {
    ..
    ..
```

所以，现在我们可以创建没有任何参数的`funkylog`,它仍然可以工作！

```
const log = funkylog();

console.log('Hello, world!');
```

## 改进想法

正如我从一开始就说过的，首先，编程应该是有趣的。否则，它会变成一种例行公事，你不会喜欢这样做。

一定要对`funkylog`做进一步的改进，让我知道你的结果是什么样的！例如，您可以通过着色来增加输出的趣味。你可以使用`npm`模块`chalk`来完成它。

然后，一旦实现了不同的颜色，就可以添加另一个标志，在字符串中的单词之间添加额外的延迟。

谢谢你在整个教程中一直陪着我！
我在[learn.coderslang.com](https://learn.coderslang.com)写了一个编程博客，在[T4 建了一个全栈 JS 课程。](https://js.coderslang.com)

### 如果您对本教程有任何反馈或问题，欢迎发微博给我 **@coderslang** 或在 Telegram**@ coder slang _ chat**上参与讨论