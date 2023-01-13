# 递归，递归，递归

> 原文：<https://www.freecodecamp.org/news/recursion-recursion-recursion-4db8890a674d/>

迈克尔·奥洛伦尼索拉

# 递归，递归，递归

![CNzH0PpIqLG0D7x40m-yO442M54rEDL3bzAD](img/f7640b1c64c68350adb07c10c0e6c711.png)

在我告诉你什么是递归之前，你应该先看看这篇文章:

[**递归，递归，递归**](https://medium.freecodecamp.com/recursion-recursion-recursion-4db8890a674d)
[*在我告诉你什么是递归之前，你应该先看看这篇文章:*medium.freecodecamp.com](https://medium.freecodecamp.com/recursion-recursion-recursion-4db8890a674d)

如果你没有上当，给自己一个鼓励。如果你做了，不用担心——你现在知道什么是递归了！

> 人们经常开玩笑说，为了理解递归，你必须首先理解递归。约翰·库克

当我刚开始编码的时候，我一直在考虑递归。我曾经认为它是某种魔法，或者是某种高阶技术，只有最好的开发人员才能用它来解决最困难的问题。

事实证明，这根本不是魔术。但是好的开发者确实理解它。伟大的开发人员知道什么时候使用它最好。

那么到底什么是递归呢？

你有没有一遍又一遍地练习某样东西，直到你“把它记下来”的时候？那么你已经完成了一个递归动作。

简单地说，你反复执行一项任务或一系列步骤，直到你达到某个期望的目标。朋友们，这就是递归的本质。

在代码中，递归函数是调用自身函数。

在开始编写任何代码之前，让我们通过一个基本的例子来理解递归函数的结构。因为钢琴是我的心肝宝贝，我们将举办一个名为 practicePiano 的活动。

每次用一个人调用这个函数，那个人就会练琴。因为我现在没有花足够的时间练习，所以我应该练习一下。

```
practicePiano(person){  practiceScales(person);    practiceChords(person);}
```

```
practicePiano('Michael');
```

我已经调用了上面的函数一次，所以我能够得到一个会话，但我肯定需要一个以上的会话才能真正变得更好。

```
practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');practicePiano('Michael');...
```

这是伟大的，但它打破了编程的最大原则之一:不要重复自己(干)。

我能够得到更多的练习会话，但是每当我想练习更多的时候，我必须添加另一个对 practicePiano 的调用。

解决这个问题的一个方法是调用函数本身，所以每次我练习钢琴的时候，我都练习得更多:

```
practicePiano(person){  practiceScales(person);    practiceChords(person);
```

```
//Recursive magic here!
```

```
 practicePiano(person);}
```

```
//Now we only need one of these!
```

```
practicePiano('Michael');
```

这太棒了。我只需要打一次电话。唯一的问题是，一旦我调用这个函数…我就不会停止练习。我从不停止，直到我真的不可能再练习了。

```
//Our code above would behave as follows:
```

```
 practiceScales('Michael');    practiceChords('Michael');
```

```
//Recursive Call
```

```
 practiceScales('Michael');    practiceChords('Michael');
```

```
//Recursive Call
```

```
 practiceScales('Michael');    practiceChords('Michael');
```

```
//Recursive Call
```

```
//..till I can't physically practice anymore
```

当您的电脑到达无法继续的类似点时，通常会返回以下错误:

```
RangeError: Maximum call stack size exceeded
```

这就相当于你的电脑说，“我没有空间了，不得不关门了。”它将每个函数调用记录在内存堆栈中。但是由于调用从未停止，堆栈被完全填满，计算机被迫停止。(这就是热门网站 Stack Overflow 名字的由来。)

所以回到我那永无止境的钢琴练习，怎样才能让我的手不掉下来呢？

这就是我们看到你以前可能听说过的一个术语的重要性的地方:**基本情况**。

在递归函数中，基本情况是你试图实现的目标或你期望完成的任务。基本用例的工作是告诉你的函数什么时候应该停止。

在我们的类比中，这个目标可以是练习到我累了。

```
practicePiano(person){   if (tired(person)){ //When I am finally tired    console.log("Guess you can take a break now...");    return ;  //This will return out of the function and stop the recursive call  }
```

```
 practiceScales(person);    practiceChords(person);
```

```
//Recursive magic here!
```

```
 practicePiano(person);}
```

```
//Now when we call this here...I'll only practice over and over again until I'm tired
```

```
practicePiano('Michael');
```

这是大多数开发人员遇到问题的地方。虽然我们目前的钢琴练习类比是一种简化，但它揭示了极其重要的一点:如果我从未厌倦练习呢？那么我们编写的基本案例将不会解决我们的“超出最大调用堆栈大小”错误。

在我们直接进入编码之前，重要的是花点时间思考所有可能的情况，这些情况能够——并且应该——停止我们的递归调用。

作为一名开发人员，您将编写复杂的算法，这些算法可能采用变量输入并使用递归来实现某个目标。

您可能会开发一个或多个基本用例，您相信您的递归调用将达到这些基本用例。但情况可能并不总是如此。

考虑一下我的机器人对手的情况:

```
practicePiano(person){   if (tired(person)){    console.log("Guess you can take a break now...");    return ;    }
```

```
 if (handsFallOff(person)){   console.log("Go see a doctor about that");    return ; }
```

```
 practiceScales(person);    practiceChords(person);
```

```
 practicePiano(person);}
```

```
practicePiano('Cyborg-Michael'); 
```

```
//Cyborg-Michael never gets tired//Nor do his hands ever fall off//Back to being stuck practicing forever...
```

考虑到这一点，重要的是要始终确保你在开发你的基本情况时是彻底的，并且确定你的功能的行为方式总是达到基本情况。

在我们的例子中，一个合乎逻辑的基本情况是能够演奏像贝多芬第五交响曲这样的作品。

重构我们的代码，我们现在有:

```
practicePiano(person, song){   if (tired(person)){    console.log("Guess you can take a break now...");    return ;    }
```

```
 if (handsFallOff(person)){   console.log("Go see a doctor about that");    return ; }
```

```
 if (song(person)){   console.log("Great Job! Time to learn this on the guitar!");    return ; }
```

```
 practiceScales(person);    practiceChords(person);
```

```
 practicePiano(person, song);}
```

```
practicePiano('Cyborg Michael', BeethovenFifth); 
```

```
//The Cyborg version of me never gets tired//Nor do my hands ever fall off//But, being a cyborg...I can learn Beethoven's 5th pretty quickly
```

这就是递归解决方案的力量。用几行代码，我就能完成一些任务，但我可能不知道需要多少步骤。我可能需要 100 次练习，而我的半机械人只需要 5 次，但是这个解决方案对我们两个都有效。

概括一下，只要记住以下几点:

1.  递归允许你轻松地重复一个任务来完成某个目标。
2.  基本情况应该足够彻底，以允许您的递归函数实际得出结论(而不是永远运行)。
3.  递归帮助你保持代码干燥(同样，你会经常听到这个首字母缩写词，所以记住它代表“不要重复自己”——哎呀，我就是这么做的！)

### 更多递归即将到来

在我即将发表的关于数据结构的系列文章中，我们将更深入地探讨递归。在这次深入探讨中，我们将了解如何在日常代码中开始使用[时间复杂度分析](https://medium.freecodecamp.com/time-is-complex-but-priceless-f0abd015063c#.huo7yk6wy)和递归。我们还将研究各种循环方法，以理解为什么使用一种方法比使用另一种方法更好。

顺便说一下，我们没有机会在这里讨论的话题之一是[阶乘](https://en.wikipedia.org/wiki/Factorial)问题。阶乘问题是您发现递归解决方案应用最频繁的地方，因为它们需要递归迭代一定的次数。你可以在 SitePoint 的这篇[精彩文章](https://www.sitepoint.com/recursion-functional-javascript/)中找到更多关于解决阶乘问题的细节。

这里有一些额外的资源可以提供帮助:

[**可汗学院关于递归**](https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms/a/recursion)
[*免费学习关于数学、艺术、计算机编程、经济学、物理、化学、生物、医学、金融…*www.khanacademy.org](https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms/a/recursion)[**spark notes:什么是递归？:什么是递归？**](http://www.sparknotes.com/cs/recursion/whatisrecursion/section1.rhtml)
[*概述什么是递归？在什么是递归？。了解本章、场景或……*www.sparknotes.com](http://www.sparknotes.com/cs/recursion/whatisrecursion/section1.rhtml)[**mybrainishuge/recursion-prompts**](https://github.com/mybrainishuge/recursion-prompts)
[*recursion-prompts-使用递归解决的提示库*github.com](https://github.com/mybrainishuge/recursion-prompts)

此外，感谢 [Yara Tercero](https://yctercero.github.io/) 帮助编辑这篇文章。