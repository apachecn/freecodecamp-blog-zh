# 邮寄一个包来解释 JavaScript 闭包

> 原文：<https://www.freecodecamp.org/news/javascript-closures-explained-by-mailing-a-package-4f23e9885039/>

凯文·科诺年科

![1*IJ7H4WWlDuAWxlUSaF-Z6A](img/9696119b59286450031b75068c853782.png)

# 邮寄一个包来解释 JavaScript 闭包

#### 如果您过去邮寄过包裹或信件，那么您可以理解 JavaScript 中的闭包。

在成为中级或高级 JavaScript 开发人员的过程中，您可能会遇到闭包。在阅读了关于这个主题的技术资源后，你可能也朝相反的方向跑了。

闭包有一点很棒:它们允许你编写带有中间步骤的函数，可以在特定时刻从你的站点获取数据。这就像给你的功能添加了一个“暂停”按钮。您可以运行您的函数，并在特定的时间点保存变量的值。然后，当你想在以后恢复这个函数，并使用你的应用程序中已经改变的变量值时，你可以用一个**闭包、**或原始函数中的一个函数**来完成。**

会变得容易的，我保证。

那么，到底什么时候你会使用闭包呢？

假设您正在使用 Google Maps API 构建纽约市旅游地标的交互式地图。你有一个数组，里面有一堆你想添加到地图上的地图标记——自由女神像、帝国大厦、康尼岛，等等。您希望将所有这些标记添加到地图中，但是您还希望向每个标记添加一个单击事件。单击标记时，您想要显示有关该标记的动态信息，包括实时天气数据。

```
var touristPlaces= […];
```

```
for(var i=0; i< touristPlaces.length; i++){  var marker= touristPlaces[i];  $(marker).click(function(){    showToolTip(i)  });}
```

问题来了——如果你这样写的话，[就不行了](http://stackoverflow.com/questions/2622421/what-are-the-use-cases-for-closures-callback-functions-in-javascript)。“for”循环将在 click 事件中的回调注册适当的 I 值之前完成。您需要捕捉这个中间点，以便稍后可以用适当的 I 调用该函数。

你首先需要知道什么？

1.  变量范围
2.  回调的概念(我也为这个写了指南！)

如果你正在寻找闭包的技术解释，MDN 上的指南可能是最好的。

#### 闭包与邮寄包裹的过程相同。

让我们看一些使用闭包来邮寄包裹的基本代码。

addressPackage()函数是一个**闭包**！调用 packBox 函数后，可以随时调用它。它还可以访问最初调用 packBox()时的变量和参数。

注意 console.log 输出直到第 14 行和第 15 行才显示出来。这是极其重要的。如果您在第 11 行之后运行该代码，您将会看到“将球衣放入箱子中”。不会有错误，但是闭包 addressPackage()不会在此时运行。

当你邮寄一个包裹时，你可能会认为直到包裹装满并写上地址，你的工作才算完成。同样，packBox()函数会一直等待，直到闭包也被调用。让我们一行一行地过一遍。

**第 11 行:**您创建了变量 brotherGift，这是 packBox()函数的一个**实例**。你要送一件球衣给你哥哥。

**第 3 行:**您的代码记录了一条关于球衣的语句。

**第 8 行:**packBox()函数返回…另一个函数？啊？

我们就此打住，假设 13 号线还没有运行。下面是正在发生的事情:packBox()函数不会返回“准备发送”行，直到您也用一个参数调用 addressPackage()函数。就像寄一个包裹有两个步骤:第一，填充它，第二，写地址。如果你的包裹里没有东西或者没有地址，那它就一文不值！也就是说，您不一定需要在填充内容后直接寻址包装。你可以等几天再解决这个问题。你可能需要去你的电脑上查找地址。你可能正在等待你的兄弟正式改变他的地址！

不管怎样，如果你不立即处理这个包，这并不意味着这个包会神奇地自动清空。当你返回去处理它的时候，这些内容仍然会在那里！所以，任何时候我们调用 brotherGift，第一个参数，jersey，还是会有。

…等待…等待…现在让我们运行第 13 行。

**第 13 行:**好了，让我们结束这个**实例**！您准备添加地址，因此调用 brotherGift 并提供地址作为参数。请记住第 11 行，brotherGift 是带有“jersey”参数的 packBox 的一个**实例**。因此，当您调用它时，您提供了另一个参数，该参数将被发送到 closure:address package()；

**第 3 行:**将显示 console.log，因为我们现在正在运行第 13 行的代码。

**第 4 行:**我们现在向 addressPackage()提供第二个参数；

**第 6 行:** addressPackage 记录了一条与地址参数相关的语句。

**第 8 行:**return 语句可以为这个实例触发。

同样，闭包允许我们有这样一个中间实例，其中一个参数已经被填充，但是 brotherGift 没有被填充，直到我们添加第二个参数。**如果我们想在一行中做这件事**，我们会写:packBox(' jersey ')(' 123 Main Street，Anywhere USA 01234 ')；

#### 再举一个例子

假设你想给你的每个家庭成员送一份礼物。在给每个盒子添加地址之前，你可以先把它们打包。这是代码中的样子。

闭包的另一个神奇特性！即使我们用 4 个单独的礼物/地址对运行函数，每个实例也能够使用正确的礼物和正确的地址。在传统函数中，没有记忆的概念。如果您想使用一个传统的函数，您需要在第 6–15 行中显式地重述原始的礼物。

#### 你将在哪里使用这个

在 Node.js 中你会经常遇到闭包，如果你只是对前端感兴趣，回想一下我们最初的例子。如果你想写一个在应用程序的两个独立阶段考虑用户输入的函数，你可能要考虑一个闭包！

你喜欢这本指南吗？或者你在另一个 JavaScript 话题上遇到了麻烦？给它一颗心，在评论里让我知道！

寻找其他解释的 JavaScript 概念？查看该系列中过去的文章。

[在赌场赌博解释的 JavaScript 承诺](https://medium.freecodecamp.com/javascript-promises-explained-by-gambling-at-a-casino-28ad4c5b2573#.8e096ciu2)

[模型-视图-控制器(MVC)通过在酒吧点饮料来解释](https://medium.freecodecamp.com/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053#.a2wmxu74b)

[使用 Minions 解释 JavaScript 回调](https://medium.freecodecamp.com/javascript-callbacks-explained-using-minions-da272f4d9bcd#.rf0y0hl26)