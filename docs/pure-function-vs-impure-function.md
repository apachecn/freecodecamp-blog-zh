# 函数式编程中的纯函数与不纯函数——有什么区别？

> 原文：<https://www.freecodecamp.org/news/pure-function-vs-impure-function/>

纯函数和不纯函数是你在函数式编程中会经常看到的两个编程术语。

这两种功能的一个核心区别是它们是否有副作用。

在本文中，您将了解什么是副作用，我们将讨论纯函数和不纯函数之间的差异。

事不宜迟，让我们从副作用开始。

## 什么是副作用？

每当你在函数中使用*外部代码*时，就会在程序中产生一个**副作用**——结果影响了函数执行任务的能力。

那么这到底意味着什么呢？让我们看一些例子。

### 副作用示例 1:如何将旧值添加到新值中

```
let oldDigit = 5;

function addNumber(newValue) {
  return oldDigit += newValue;
}
```

在上面的代码片段中，`oldDigit`在函数中的使用给`addNumber()`带来了以下副作用:

#### 第一个副作用:对旧数字的依赖

`addNumber()`依赖于`oldDigit`来成功履行其职责，这意味着每当`oldDigit`不可用(或`undefined`)时，`addNumber()`将返回一个错误。

#### 第二个副作用:修改外部代码

因为`addNumber()`被编程为变异`oldDigit`的[状态](https://www.codesweetly.com/state-in-programming/)，这暗示`addNumber()`有操纵一些外部代码的副作用。

#### 第三:成为一个不确定的函数

在`addNumber()`中使用外部代码使它成为一个不确定的函数——因为你永远无法通过单独读取它来确定它的输出。

换句话说，为了确定`addNumber()`的返回值，您必须考虑其他外部因素——比如`oldDigit`的当前状态。

因此，`addNumber()`不是独立的——它总是有附加条件的。

### 副作用示例 2:如何将文本打印到您的控制台

```
function printName() {
  console.log("My name is Oluwatobi Sofela.");
}
```

在上面的代码片段中，`console.log()`在`printName()`中的用法给了函数副作用。

#### console.log 是如何导致一个函数产生副作用的？

一个`console.log()`导致一个函数有副作用，因为它影响外部代码的状态——也就是说，[控制台对象](https://developer.mozilla.org/en-US/docs/Web/API/console)的状态。

换句话说，`console.log()`指示计算机改变`console`对象的状态。

因此，当您在函数中使用它时，它会导致该函数:

1.  依赖于`console`对象来有效地执行它的工作。
2.  修改外部代码的状态(即`console`对象的状态)。
3.  变得不确定——因为您现在必须考虑`console`的状态以确定函数的输出。

因此，每当你在函数中使用*外部代码*时，那个代码就会引起**副作用**。

那么副作用与纯函数和不纯函数有什么关系呢？

让我们通过看一个纯函数的定义和它的不纯替代来找出答案。

## 什么是不纯函数？

所以现在我们知道了函数中的副作用是什么，我们可以讨论不纯(和纯)函数了。

首先，**不纯函数**是包含一个或多个副作用的函数。

考虑下面的 JavaScript 代码:

```
const myNames = ["Oluwatobi", "Sofela"];

function updateMyName(newName) {
  myNames.push(newName);
  return myNames;
}
```

在上面的代码片段中，`updateMyName()`是一个不纯的函数，因为它包含了使外部[状态](https://www.codesweetly.com/state-in-programming/)变异的代码(`myNames`)——这给了`updateMyName()`一些副作用。

## 什么是纯函数？

一个**纯函数**是没有任何副作用的函数。

考虑下面的 JavaScript 代码:

```
function updateMyName(newName) {
   const myNames = ["Oluwatobi", "Sofela"];
   myNames[myNames.length] = newName;
   return myNames;
}
```

在上面的代码片段中，请注意`updateMyName()`并不依赖任何外部代码来完成它的职责。这使得它成为一个纯函数。

只要有可能，您应该在应用程序中使用纯函数。让我们讨论一下这样做的好处。

## 纯函数的优点

以下是纯函数的一些优点。

### 纯函数是独立的

纯函数不影响任何外部状态，也不受外部代码的影响。

换句话说，pure 函数使用的所有外部数据都作为参数接收——它们不是在内部显式使用的*。*

因此，你所看到的就是你所得到的——绝对没有任何附加条件。

因此，你不需要寻找可能影响你的纯功能有效运行的外部条件(状态),因为所有的活动都发生在内部。

### 纯函数更容易阅读

纯函数比不纯函数更容易阅读和调试。

纯函数之所以如此易读，是因为它们完全依赖于自身——它们既不影响外部状态，也不受外部状态的影响。

## 关于纯函数需要知道的重要事情

无论何时选择使用纯函数，都要记住这三条基本信息。

### 您可以将一个外部状态克隆到一个纯函数中

将外部状态克隆到纯函数中并不会使函数不纯。

状态复制只是一种复制和粘贴操作，不会在源和它的克隆之间留下任何附加条件。

**举例:**

```
const myBio = ["Oluwatobi", "Sofela"];

function updateMyBio(newBio, array) {
  const clonedBio = [...array];
  clonedBio[clonedBio.length] = newBio;
  return clonedBio;
}

console.log(updateMyBio("codesweetly.com", myBio));
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-blhtpi?file=script.js)

在上面的代码片段中，`updateMyBio()`使用[扩展操作符](https://www.codesweetly.com/spread-operator/)将`myBio`的状态复制到`clonedBio`中。但是，它仍然是一个纯函数，因为它既不依赖于`myBio`，也不修改任何外部代码。

相反，它是一个排他的确定性函数，被编程为使用其数组参数的克隆版本。

### 避免纯函数中的代码突变

从技术上讲，你可以在一个纯函数的范围内改变局部定义的变量。但是，最好避免这样做。

例如，考虑下面的代码:

```
const compBio = ["code", "sweetly"];

function updateCompBio(newBio, array) {
  const clonedBio = [...array];
  clonedBio[clonedBio.length] = newBio;
  return clonedBio;
}

console.log(updateCompBio(".com", compBio));
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-dprdlf?file=script.js)

在上面的代码片段中，`updateCompBio()`是一个使用`clonedBio[clonedBio.length] = newBio`来改变其本地状态的纯函数。

虽然这样的操作不会使`updateCompBio()`不纯，但这不是最佳做法。

编写纯函数的推荐方法是让它接收所有的参数值，如下所示:

```
const compBio = ["code", "sweetly"];

function updateCompBio(newBio, array) {
  return [...array, newBio];
}

console.log(updateCompBio(".com", compBio));
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-gyl8sy?file=script.js)

请注意我们的代码现在看起来是多么干净和可移植。这是让您的 pure 函数将所有值作为参数接收的一个优势。通过这样做，您还会发现调试代码变得更加容易。

### 相同的输入将总是返回相同的输出

纯函数的一个重要特点是，不管调用多少次，它们总是用相同的输入集返回相同的值。

## 包装它

如果你的函数不包含任何外部代码，那么它就是纯的。否则，如果它包含一个或多个副作用，它就是**不纯**。

在本文中，我们讨论了什么是纯函数和不纯函数，并且了解了使用纯函数可以给代码带来的好处。

感谢阅读！