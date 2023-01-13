# 为什么您应该了解 JavaScript 闭包

> 原文：<https://www.freecodecamp.org/news/javascript-closures/>

完全理解闭包似乎是成为 JavaScript 开发人员的必经之路。

理解闭包很难是有原因的——因为它们经常被倒着教授。你可能学过什么是闭包，但是你可能不明白它们对普通开发人员或者在你自己的代码中是如何有用的。

那么为什么闭包在我们日常的 JavaScript 代码中如此重要呢？

不要把闭包看作是某种突击测验中需要记忆的主题，让我们看看首先是什么样的一系列步骤可以让我们看到闭包。一旦我们明白了它们是什么，我们就会发现为什么闭包值得你去了解并在你的 JavaScript 代码中加以利用。

想看这节课吗？本教程是 2020 JS Bootcamp 的一部分，这是一个 4 个多小时的课程，通过大量实用、严肃的课程向您展示如何成为一名 JavaScript 专家。 **[在这里](https://courses.reedbarger.com/p/2020-js-bootcamp)可以即时进入 JS 训练营。**

## 在野外看到一个终结？

假设我们正在制作博客网站 Medium 的应用克隆，我们希望每个用户能够喜欢不同的帖子。

每当用户点击 like 按钮时，它的值每次都会增加 1。

把它想象成中型拍手按钮:

![react-clap-demo](img/aba2e16add685cc6fc4bf4d6cd11c5c5.png)

将处理每次增加 1 的计数的函数被称为`handleLikePost`，我们使用名为`likeCount`的变量来跟踪喜欢的数量:

```
// global scope
let likeCount = 0;

function handleLikePost() {
  // function scope
  likeCount = likeCount + 1;
}

handleLikePost();
console.log("like count:", likeCount); // like count: 1 
```

每当用户喜欢一个帖子，我们调用`handleLikePost`，它就把我们的`likeCount`加 1。

这是可行的，因为我们知道函数可以访问自身之外的变量。

换句话说，**函数可以访问任何父作用域**中定义的任何变量。

然而，这段代码有一个问题。由于`likeCount`在全局范围内，而不在任何函数中，`likeCount`是一个全局变量。全局变量可以被我们应用程序中的任何其他代码或函数使用(和更改)。

例如，如果在我们的函数之后，我们错误地将我们的`likeCount`设置为 0，该怎么办？

```
let likeCount = 0;

function handleLikePost() {
  likeCount = likeCount + 1;
}

handleLikePost();
likeCount = 0;
console.log("like count:", likeCount); // like count: 0 
```

自然，`likeCount`永远不能从 0 递增。

当只有一个函数需要一段给定的数据时，它只需要存在于本地，也就是说，存在于该函数中。

现在让我们将`likeCount`带入我们的函数:

```
function handleLikePost() {
  // likeCount moved from global scope to function scope
  let likeCount = 0;
  likeCount = likeCount + 1;
} 
```

注意，有一种更短的方法来写我们增加`likeCount`的那一行。我们可以像这样使用+=运算符，而不是说`likeCount`等于`likeCount`的前一个值并加一:

```
function handleLikePost() {
  let likeCount = 0;
  likeCount += 1;
} 
```

为了让它像以前一样工作并获得类似 count 的值，我们还需要将我们的`console.log`带入函数中。

```
function handleLikePost() {
  let likeCount = 0;
  likeCount += 1;
  console.log("like count:", likeCount);
}

handleLikePost(); // like count: 1 
```

而且还和以前一样正常工作。

所以现在用户应该可以随心所欲地喜欢一个帖子了，所以让我们再多调用几次`handleLikePost`:

```
handleLikePost(); // like count: 1
handleLikePost(); // like count: 1
handleLikePost(); // like count: 1 
```

然而，当我们运行这段代码时，有一个问题。

我们期望看到`likeCount`不断增加，但我们每次只看到 1。这是为什么呢？

花点时间，看看我们的代码，试着解释一下为什么我们的`likeCount`不再递增。

让我们看看我们的`handleLikePost`函数及其工作原理:

```
function handleLikePost() {
  let likeCount = 0;
  likeCount += 1;
  console.log("like count:", likeCount);
} 
```

每次我们使用它时，我们都在重新创建这个`likeCount`变量，它的初始值为 0。

难怪我们无法跟踪函数调用之间的计数！它每次都保持设置为 0，然后递增 1，之后该函数运行结束。

所以我们被困在这里了。我们的变量需要存在于`handleLikePost`函数中，但是我们不能保存计数。

我们需要一些东西，允许我们保存或记住函数调用之间的`likeCount`值。

如果我们尝试一些起初看起来有点奇怪的东西—如果我们尝试在我们的函数中加入另一个函数会怎么样:

```
function handleLikePost() {
  let likeCount = 0;
  likeCount += 1;
  function() {

  }
}

handleLikePost(); 
```

这里我们将这个函数命名为`addLike`。原因？因为它现在将负责递增`likeCount`变量。

注意这个内部函数不一定要有名字。它可以是匿名函数。在大多数情况下，的确如此。我们只是给它一个名字，这样我们可以更容易地谈论它和它做什么。

`addLike`现在将负责增加我们的`likeCount`，所以我们将把增加 1 的那一行移到我们的内部函数中。

```
function handleLikePost() {
  let likeCount = 0;
  function addLike() {
    likeCount += 1;
  }
} 
```

如果我们在`handleLikePost`中调用这个`addLike`函数会怎么样？

所有会发生的是，`addLike`将增加我们的`likeCount`，但是`likeCount`变量仍然会被销毁。因此，我们再次失去了我们的价值，结果是 0。

但是，如果我们不在函数内部调用`addLike`，而是在函数外部调用它，会怎么样呢？这似乎更奇怪了。我们该怎么做呢？

我们现在知道函数会返回值。例如，我们可以在`handleLikePost`结束时返回我们的`likeCount`值，以将其传递给程序的其他部分:

```
function handleLikePost() {
  let likeCount = 0;
  function addLike() {
    likeCount += 1;
  }
  addLike();
  return likeCount;
} 
```

但是不要这样做，让我们在`addLike`中返回`likeCount`，然后返回`addLike`函数本身:

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;
    return likeCount;
  };
  // addLike();
}

handleLikePost(); 
```

这可能看起来很奇怪，但是在 JS 中这是允许的。我们可以像使用 JS 中的其他值一样使用函数。这意味着一个函数可以从另一个函数返回。通过返回内部函数，我们可以从其封闭函数的外部调用它。

但是我们该怎么做呢？想一分钟，看看你是否能想出来...

首先，为了更好地了解发生了什么，让我们`console.log(handleLikePost)`当我们调用它时，看看我们得到了什么:

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;
    return likeCount;
  };
}

console.log(handleLikePost()); // ƒ addLike() 
```

不出所料，我们记录了`addLike`函数。为什么？因为毕竟我们要把它退回去。

我们能不能把它放到另一个变量里？正如我们刚才所说，函数可以像 JS 中的任何其他值一样使用。如果我们可以从函数中返回它，我们也可以把它放在变量中。所以让我们把它放在一个叫做`like`的新变量中:

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;
    return likeCount;
  };
}

const like = handleLikePost(); 
```

最后，让我们调用`like`。我们将这样做几次，每个结果是:

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;
    return likeCount;
  };
}

const like = handleLikePost();

console.log(like()); // 1
console.log(like()); // 2
console.log(like()); // 3 
```

我们的`likeCount`终于保存下来了！每当我们调用`like`时，`likeCount`就会从之前的值递增。

这里到底发生了什么？嗯，我们知道了如何在声明函数的范围之外调用`addLike`函数。我们通过从外部函数返回内部函数并存储对它的引用(名为`like`)来调用它。

## 闭包是如何逐行工作的？

当然，这就是我们的实现，但是我们如何在函数调用之间保存`likeCount`的值呢？

```
function handleLikePost() {
  let likeCount = 0;
  return function addLike() {
    likeCount += 1;
    return likeCount;
  };
}

const like = handleLikePost();

console.log(like()); // 1 
```

1.  执行`handleLikePost`外部函数，创建内部函数`addLike`的实例；那个函数*在变量`likeCount`上关闭*，这是上面的一个作用域。
2.  我们从声明函数的作用域之外调用了`addLike`函数。我们通过从外部函数返回内部函数并存储对它的引用(名为`like`)来调用它。
3.  当`like`函数结束运行时，通常我们会期望它的所有变量都被垃圾回收(从内存中移除，这是 JS 编译器执行的一个自动过程)。我们期望每个`likeCount`在函数完成时消失，但是它们没有。

那是什么原因？*关闭*。

**由于内部函数实例仍然活着(分配给`like`)，闭包仍然保存着`countLike`变量。**

你可能会认为用另一个函数编写一个函数，就像用全局作用域编写一个函数一样。但事实并非如此。

这就是闭包让函数如此强大的原因，因为它是一种特殊的属性，是语言中其他任何东西都没有的。

## 变量的生存期

为了更好地理解闭包，我们必须理解 JavaScript 如何处理被创建的变量。您可能想知道当您关闭页面或转到应用程序中的另一个页面时，变量会发生什么变化。变量的寿命有多长？

全局变量会一直存在，直到程序被丢弃，例如当你关闭窗口时。他们存在于项目的生命中。

然而，局部变量的寿命很短。它们是在调用函数时创建的，在函数完成时删除。

之前，当函数运行时，`likeCount`只是一个局部变量。likeCount 变量是在函数开始时创建的，然后在函数执行完毕后销毁。

## 闭包不是快照——它们保持局部变量活着

有时人们说 JavaScript 闭包类似于快照，是我们程序在某个时间点的图像。这是一个误解，我们可以通过在 like 按钮功能中添加另一个特性来消除它。

假设在一些罕见的情况下，我们希望允许用户“双赞”一篇帖子，并且每次将`likeCount`增加 2 而不是 1。

我们将如何添加这个特性？

向函数传递值的另一种方式当然是通过参数，它的操作就像局部变量一样。

让我们向函数传递一个名为 step 的参数，这将允许我们提供一个动态的、可变的值来增加我们的计数，而不是硬编码的值 1。

```
function handleLikePost(step) {
  let likeCount = 0;
  return function addLike() {
    likeCount += step;
    // likeCount += 1;
    return likeCount;
  };
} 
```

接下来，让我们试着创建一个特殊的函数，它将允许我们像我们的帖子一样加倍。我们将传入 2 作为我们的`step`值，然后尝试调用我们的两个函数，`like`和`doubleLike`:

```
function handleLikePost(step) {
  let likeCount = 0;
  return function addLike() {
    likeCount += step;
    return likeCount;
  };
}

const like = handleLikePost(1);
const doubleLike = handleLikePost(2);

like(); // 1
like(); // 2

doubleLike(); // 2 (the count is still being preserved!)
doubleLike(); // 4 
```

我们看到`likeCount`也被保留给`doubleLike`。

这里发生了什么事？

内部`addLike`函数的每个实例在其外部`handleLikePost`函数的作用域内关闭了`likeCount`和`step`变量。`step`随着时间的推移保持不变，但是每次调用内部函数时计数都会更新。因为闭包是在变量上而不仅仅是值的快照上，所以这些更新在函数调用之间被保留。

那么这段代码向我们展示了什么——我们可以传入动态值来改变函数的结果这一事实？他们还活着！闭包让局部变量在很久以前就应该销毁它们的函数中存活下来。

换句话说，它们不是静态不变的，就像封闭变量在某个时间点的快照一样——闭包保存了变量并提供了到它们的活动链接。因此，我们可以使用闭包来观察或更新这些变量。

## 到底什么是终结？

既然您已经看到了闭包的用处，那么有两个标准可以作为闭包，这两个标准您都已经在这里看到了:

1.  闭包是 JavaScript 函数的属性，而且只是函数的属性。没有其他数据类型拥有它们。
2.  要观察一个闭包，您必须在一个不同于函数最初定义的作用域中执行该函数。

## 为什么您应该知道闭包？

让我们回答我们开始回答的原始问题。基于我们所看到的，停下来试着回答这个问题。作为 JS 开发人员，我们为什么要关心闭包？

闭包对你和你的代码很重要，因为它们允许你“记住”值，这是语言中一个非常强大和独特的特性，只有函数才拥有。

我们在这个例子中看到了这一点。毕竟一个不记得喜欢的 like count 变量有什么用？在你的 JS 职业生涯中，你会经常遇到这种情况。你需要以某种方式抓住一些价值，并可能把它与其他价值分开。你用什么？一个功能。为什么？用闭包来跟踪数据。

有了这些，你已经领先其他开发者一步了。

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*