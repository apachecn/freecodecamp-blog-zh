# JavaScript 中的词法范围——JS 中的范围到底是什么？

> 原文：<https://www.freecodecamp.org/news/javascript-lexical-scope-tutorial/>

术语“**词法范围**”乍一看似乎很难理解。但是理解每个单词的意思是有帮助的。

因此，本文将通过首先检查“词法”和“范围”的含义来解释词法范围。

因此，让我们从理解术语“范围”开始。

## 范围到底是什么？

**范围**是指一个项目(如函数或变量)可见并可被其他[代码](https://www.codesweetly.com/document-vs-data-vs-code/)访问的*区域*。

**注:**

*   **范围**指区域、空间或地域。
*   **全局范围**表示全局空间或公共空间。
*   **局部范围**指局部区域或限制区域。

**这里有一个例子:**

```
// Define a variable in the global scope:
const fullName = "Oluwatobi Sofela";

// Define nested functions:
function profile() {
  function sayName() {
    function writeName() {
      return fullName;
    }
    return writeName();
  }
  return sayName();
}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-fqqxjl?file=script.js)

在上面的代码片段中，我们在全局范围内定义了`fullName`变量。这意味着它对脚本中的所有代码都是可见的和全局可访问的。

但是我们在`sayName()`函数中定义了`writeName()`，所以它的局部范围是`sayName()`。

换句话说，`writeName()`是局部可见的，只有`sayName()`函数中的代码可以访问。

请记住，每当调用`writeName()`函数时，计算机将*而不是*直接进入全局范围调用`fullName`变量。相反，它必须依次通过[范围链](#what-is-a-scope-chain)来寻找`fullName`。

## 什么是范围链？

一个**作用域链**是指从一个变量得到*的作用域*到全局作用域存在的*唯一的*空间。

**这里有一个例子:**

```
// Define a variable in the global scope:
const fullName = "Oluwatobi Sofela";

// Define nested functions:
function profile() {
  function sayName() {
    function writeName() {
      return fullName;
    }
    return writeName();
  }
  return sayName();
}
```

在上面的代码片段中，观察到从`writeName()`函数的作用域调用了`fullName`变量。

因此，从变量调用到全局范围的范围链是:

**writeName()作用域- > sayName()作用域- > profile()作用域- >全局作用域**

换句话说，从`fullName`的调用范围到它的词法范围有四(4)个空格(在这个例子中是*全局范围*)。

**注意:**全局作用域是 [JavaScript](https://www.codesweetly.com/html-css-javascript/) 作用域链的最后一环。

## 范围链是如何工作的？

JavaScript 的作用域链决定了计算机必须经历的位置层次——一个接一个——以找到被调用的特定变量的词法作用域(原点)。

例如，考虑下面的代码:

```
// Define a variable in the global scope:
const fullName = "Oluwatobi Sofela";

// Define nested functions:
function profile() {
  function sayName() {
    function writeName() {
      return fullName;
    }
    return writeName();
  }
  return sayName();
}
```

在上面的代码片段中，每当调用`profile()`函数时，计算机将首先调用`sayName()`函数(这是`profile()`函数中唯一的代码)。

其次，计算机会调用`writeName()`函数(这是`sayName()`函数中唯一的代码)。

此时，由于`writeName()`中的代码指示计算机调用并返回`fullName`变量的内容，计算机将调用`fullName`。但不会直接去全局范围调用`fullName`。

相反，计算机必须通过*作用域链*一步一步地*寻找`fullName`的*词法作用域*。*

因此，下面是计算机定位`fullName`的词法范围必须采取的顺序步骤:

1.  首先，计算机将检查`fullName`是否在`writeName()`函数中被本地定义。但是它在那里找不到`fullName`的定义，所以它移到下一个范围继续寻找。
2.  其次，计算机将在`sayName()`(作用域链中的下一个空格)中搜索`fullName`的定义。尽管如此，它没有在那里找到它，所以它爬上梯子到下一个作用域。
3.  第三，计算机将在`profile()`函数中搜索`fullName`的定义。然而，`fullName`仍然不在那里。所以计算机继续在作用域链的下一个区域中寻找`fullName`的词法作用域。
4.  第四，计算机进入*全局范围*(在`profile()`之后的下一个范围)。幸运的是，它在那里找到了全名的定义！因此，它获取它的内容(`"Oluwatobi Sofela"`)并返回它。

## 练习瞄准镜的时间到了🤸‍♂️🏋️‍♀️🏊‍♀️

考虑下面的脚本。计算机会调用三个`fullName`变量中的哪一个？

```
// First fullName variable defined in the global scope:
const fullName = "Oluwatobi Sofela";

// Nested functions containing two more fullName variables:
function profile() {
  const fullName = "Tobi Sho";
  function sayName() {
    const fullName = "Oluwa Sofe";
    function writeName() {
      return fullName;
    }
    return writeName();
  }
  return sayName();
}
```

计算机会调用第一个、第二个还是第三个`fullName`变量？

注意:如果你自己尝试这个练习，你会从本教程中受益匪浅。

如果你遇到困难，不要气馁。相反，复习功课，再试一次。

一旦你尽了最大努力(如果你不这样做，你只会欺骗自己！)，往下看下面的正确答案。

## 你做对了吗？

在上面脚本中出现的三个`fullName` *定义*中，计算机将调用并返回在`sayName()`函数中定义的定义。

将调用`sayName()`的`fullName`变量，因为`sayName()`是计算机将首先在其中找到`fullName`定义的范围。

因此，当`profile()`被调用时，返回值将是`"Oluwa Sofe"`。

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-9mpvfv?file=script.js)

**需要记住的一些事情:**

*   假设计算机没有在任何作用域中找到`fullName`的定义。在这种情况下，计算机将返回`Uncaught ReferenceError: fullName is not defined`。
*   全局范围总是任何 JavaScript 范围链的最后一个范围。换句话说，全局范围是所有搜索的终点。
*   内部(子)范围可以访问其父(外部)范围，但是外部范围不能访问其子范围。
    例如，在上面的代码片段中，`writeName()`可以访问其任何父作用域内的代码(`sayName()`、`profile()`或*全局作用域*)。
    但是`sayName()`、`profile()`和*全局范围*都不能访问`writeName()`的任何代码。

## 快速回顾到目前为止的范围

JavaScript 作用域完全是关于空间的。

所以下次你的伴侣叫你去他们的私人空间时，记住他们是在邀请你去他们的私人空间😜！

当你到了那里，一定要问他们关于他们最好的词汇游戏...

但是我听到你问，词汇是什么意思？下面就来了解一下。

## 词汇是什么意思？

**词法**指事物的定义。

任何与创造单词、表达或变量相关的东西都被称为词汇的。

例如，[拼字游戏](https://en.wikipedia.org/wiki/Scrabble)是一种词汇活动，因为它与单词的创造有关。

同样，工作与语言学(语言研究)相关的人也有词汇方面的职业。

注意:字典的另一个名字是*词典*。换句话说，词典是一个列出和定义单词的字典。

现在我们知道了范围和词法含义，我们可以讨论词法范围了。

## JavaScript 中的词法作用域是什么？

**词法范围**是一个表达式的*定义*区域。

换句话说，一个条目的词法范围就是这个条目得到*创建*的地方。

**注:**

*   词法范围的另一个名称是*静态范围*。
*   一个项被调用的地方不一定是该项的词法范围。相反，一个条目的*定义空间*是它的词法范围。

### 词法范围的示例

考虑下面的代码:

```
// Define a variable in the global scope:
const myName = "Oluwatobi";

// Call myName variable from a function:
function getName() {
  return myName;
}
```

在上面的代码片段中，注意我们*在全局作用域中定义了*变量`myName`，而*在`getName()`函数中调用了*变量。

**问题:**两个空格中哪一个是`myName`的词法范围？是*的全局作用域*还是`getName()`函数的局部作用域？

**回答:**记住*词法范围*是指*定义空间* —不是*调用空间*。因此，`myName`的词法作用域是*全局作用域*，因为我们在全局环境中定义了`myName`。

### 词法范围的另一个例子

```
function getName() {
  const myName = "Oluwatobi";
  return myName;
}
```

**问题:**`myName`的词法范围在哪里？

**回答:**注意我们在`getName()`中创建和调用了`myName`。因此，`myName`的词法范围是`getName()`的局部环境，因为`getName()`是`myName`的定义空间。

## 词法范围是如何工作的？

JavaScript 表达式的定义环境决定了允许访问它的代码。

换句话说，只有项的词法范围内的代码才能访问它。

例如，考虑下面的代码:

```
// Define a function:
function showLastName() {
  const lastName = "Sofela";
  return lastName;
}

// Define another function:
function displayFullName() {
  const fullName = "Oluwatobi " + lastName;
  return fullName;
}

// Invoke displayFullName():
console.log(displayFullName());

// The invocation above will return:
Uncaught ReferenceError: lastName is not defined
```

注意，上面代码片段中对`displayFullName()`的调用返回了一个`Uncaught ReferenceError`。返回的错误是因为只有项的词法范围内的代码才能访问该项。

因此，`displayFullName()`函数及其内部代码都不能访问`lastName`变量，因为`lastName`是在不同的作用域中定义的。

换句话说，`lastName`的词法范围与`displayFullName()`不同。

`lastName`的定义空间是`showLastName()`，而`displayFullName()`的词汇范围是全局环境。

现在，考虑下面的代码:

```
function showLastName() {
  const lastName = "Sofela";
  return lastName;
}

// Define another function:
function displayFullName() {
  const fullName = "Oluwatobi " + showLastName();
  return fullName;
}

// Invoke displayFullName():
console.log(displayFullName());

// The invocation above will return:
"Oluwatobi Sofela"
```

在上面的代码片段中，`displayFullName()`成功返回了`"Oluwatobi Sofela"`，因为`displayFullName()`和`showLastName()`在同一个词法范围内。

换句话说，`displayFullName()`可以调用`showLastName()`，因为这两个函数都是在全局范围内定义的。

**注:**

*   在上面的例子 2 中，`displayFullName()`不能访问`showLastName()`的`lastName`变量。
    取而代之的是，`displayFullName()`调用`showLastName()`——后者返回其`lastName`变量的内容。
*   词法作用域的替代方法是动态作用域，但是它很少在编程中使用。只有少数语言，比如 bash，使用动态范围。

## 包装它

任何时候你听到词汇，思考定义。

因此，汽车、变量、电话、函数或泳衣的词汇范围指的是它的定义区域。

## 概观

本文讨论了词法范围在 JavaScript 中的含义。我们还研究了为什么它是一个重要的编程概念。

感谢阅读！