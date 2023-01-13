# HTML Canvas 解释:HTML5 Canvas 和 JavaScript 函数介绍

> 原文：<https://www.freecodecamp.org/news/javascript-functions-af6f9186a553/>

作者:亚当·雷夫洛赫

## 表情符号之前，一些背景…

大约 6 个月前，我开始在 web 开发领域工作，此前我的大部分职业生涯是在教育领域度过的。这种转变是巨大的，我非常感谢有机会在现实世界的 web 应用程序上工作。

我很高兴在这个行业工作，但从我的角度来看，还有很多东西需要学习。因此，从我开始成为一名 JavaScript 开发人员的那天起，我就一直坚持每天晚上花时间学习来提升我的技能。

在学习的同时，我最近开始给坦帕湾的青少年讲授“JavaScript 入门课程”(在佛罗里达州圣彼得的铁场)。这是一次很棒的经历，原因有很多。首先，它挑战我去学习更多关于 JavaScript 语言的复杂性和细微差别。

第二，我得到了再次教书的机会，这是我的激情之一。第三，我开始重新审视我是如何学习编程的，以及这与那些甚至不确定自己是否喜欢编程，甚至在某些情况下根本不在乎我要说什么的初学者有什么显著的不同。

你看，我最初认为这门课很棒的课程是三种方式的 JavaScript:DOM 中的 JS、服务器上的 JS 和函数式 JS 编程。

第一天之后，和我的助教好好谈了一次，我意识到我完全错了。这些话题可能会让我感兴趣，但肯定不会让一个只想在浏览器中玩 add 赞助游戏的年轻人开心。我完全重新评估了我要教的东西，在这个过程中，我开始享受乐趣！

下面是我给学生们上的第一堂课，我开始讨论函数，最后创建了一个笑脸表情符号。尽情享受吧！

如果你想跟随我们讨论函数，打开浏览器，进入 [repl.it](http://repl.it) ，在流行语言下选择 NodeJS。REPL (Read Evaluate Print Loop)应该为您打开，您可以跟随代码。

## 什么是功能？

为了理解我们将如何使用 HTML5 画布，我们必须了解一些关于函数的知识。

函数是完成特定任务的独立代码模块。函数通常“接收”数据，处理数据，然后“返回”结果。一个函数一旦写出来，就可以反复使用。”

现在让我给你举几个我们将要处理的函数类型的例子。

## 功能类型

### 正则 Ole 函数

我们使用 JavaScript 关键字*函数*声明一个基本函数。

```
function sayHelloTo(name) {    return ‘Hello ‘ + name;}sayHelloTo(‘Adam’);
```

这个函数有一个名为*的参数，名为*。它是一个传递给 *sayHelloTo* 函数的变量。因此，当程序执行时，它将传入所提供的内容。在我的例子中是*亚当*，所以在控制台中你会看到*你好亚当*。

### 构造器模式

我们也可以使用构造器模式创建一个函数。

```
function Person(name) {    this.name = name;    this.sayHello = function() {        return “Hello, my name is “ + this.name;    }}var me = new Person(“Adam”);me.sayHello();
```

Javascript 关键字*这个*指的是函数。这意味着当我们传入一个像 *name* 这样的变量时，就像我们之前做的那样，我们可以将它赋给函数和该函数的任何实例。为了创建一个实例，我们使用 JavaScript 关键字 *new* 。当函数的这个新实例被创建时，它还具有作为其属性的一个 *this.name* 值和一个 *this.sayHello* 方法。当我们创建方法的实例时，我们传入了自己的名字:*var me = new Person(' Adam ')*。当您查看 *sayHello* 方法时，它使用那个*名称*，它现在是那个实例的一部分，来创建一个句子。如果您在 repl.it 上的 NodeJS REPL 中执行这段代码，您应该会看到它的输出*您好，我是 Adam* 。

既然我们已经解决了无聊的问题，让我们在画布上画画吧。我教下一节的方法是使用 codepen.io。我的建议是，如果你想跟着做，去 codepen.io，创建一个帐户，然后创建一个新的笔，你应该设置好了。如果你想保存你的工作，一定要激活你的帐户。

## 让我们在画布上画画

首先，我们需要创建一个能够在上面绘图的画布。在你的 HTML 中创建一个画布标签。

```
<canvas id=“canvas”></canvas>
```

现在从这里开始就是 JavaScript 了！

我们需要从 DOM 中获取画布元素，并将其声明为一个变量。这将允许我们设置它的上下文。我们对“3d”还不太熟悉，所以我们将坚持使用“2d”场景。

```
var canvas = document.getElementById(“canvas”);var context = canvas.getContext(“2d”);
```

我们还可以赋予画布宽度和高度属性。

```
var canvas = document.getElementById(“canvas”);canvas.width = 800;canvas.height = 800;var context = canvas.getContext(“2d”);
```

我想在这里指出*画布*的行为完全像一个物体。它有属性和方法，就像我们在上面看到的构造函数一样。我们声明它的方式略有不同，但是它的功能非常相似。所以你可以看到它有*高度*和*宽度*属性以及*获取上下文*方法。

现在让我们把所有这些放到一个函数中，这样你就可以对函数式编程有所了解了。

```
function draw() {  var canvas = document.getElementById(“canvas”);  canvas.width = 800;  canvas.height = 800;  var context = canvas.getContext(“2d”);}
```

屏幕上还不会显示任何东西，我们将使用 *fillRect* 方法来帮助我们。

```
function draw() {  var canvas = document.getElementById("canvas");  canvas.width = 800;  canvas.height = 800;  var context = canvas.getContext("2d");  context.fillRect(10,10, 100, 200);}
```

如果你还没猜到的话， *fillRect* 方法有四个参数:x 坐标、y 坐标、宽度和高度。在画布上，x 轴从左边的 0 开始，向右到无穷大。y 轴从顶部的 0 开始，向下到无穷大。因此，当我们从(10，10)开始时，我们将虚拟光标放在点(x = 10，y = 10)上，并从该点向右移动 100，向下移动 200。

您可能已经注意到了，它还没有被添加到页面中。添加一个简单的 *window.onload* 函数，让它等于我们完成的 draw 函数。

```
function draw() {  var canvas = document.getElementById("canvas");  canvas.width = 800;  canvas.height = 800;  var context = canvas.getContext("2d");  context.fillRect(10,10, 100, 200);}window.onload = draw;
```

你可能想知道为什么 draw 函数被执行了，即使我们没有用 parens *()来执行它。*那是因为 *window.onload* 是一个函数。这就相当于说:

```
window.onload = function() {// Do stuff here like what we put in draw();}
```

这意味着 *window.onload* 在窗口被加载时执行一个函数，所以最后发生的是 *window.onload* 用它神奇的力量在 draw 周围放置了不可见的 parens，从而执行它。这涉及到很多魔法。但是现在你知道这个骗局了。

来加点颜色好玩吧！这里我们使用了 *fillStyle* 方法。它需要在*填充*之前出现，否则它不会显示。

```
function draw() {  var canvas = document.getElementById("canvas");  canvas.width = 800;  canvas.height = 800;  var context = canvas.getContext("2d");  context.fillStyle = "blue";  context.fillRect(10,10, 100, 200);}window.onload = draw;
```

这是 codepen 上的一个例子:

## 让我们画一些其他的形状！

这很简单。现在让我们画一些其他的形状。正如我们之前所做的，我们将创建一个函数，并用*宽度*、*高度*和*上下文*实例化我们的画布。

```
function triangle() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;}
```

所以我们别忘了，把 *onload* 函数改成现在取三角函数。

```
window.onload = triangle;
```

现在我们有了画布，让我们开始在画布上画线来创建我们的三角形。

```
function triangle() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;      context.beginPath();  context.moveTo(75, 50);}
```

这里我们开始我们的路径，移动光标到点(x = 75，y = 50)。

现在让我们去镇上画一些线。

```
function triangle() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;      context.beginPath();  context.moveTo(75, 50);  context.lineTo(100, 75);  context.lineTo(100, 25);  context.stroke();}
```

这产生了我们需要的前两行。为了结束它，我们回到我们开始的地方。

```
function triangle() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;      context.beginPath();  context.moveTo(75, 50);  context.lineTo(100, 75);  context.lineTo(100, 25);  context.lineTo(75, 50); // Back to where we started  context.stroke();}
```

要填充三角形，我们可以使用 fill 方法。

```
function triangle() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;      context.beginPath();  context.moveTo(75, 50);  context.lineTo(100, 75);  context.lineTo(100, 25);  context.lineTo(75, 50);  context.stroke();  context.fill();}
```

这是它在野外的样子:

我们现在可以做同样的事情，轻而易举地创造一个巨大的金字塔。

```
function pyramid() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;}
```

记得把 *onload* 函数改成金字塔。

```
window.onload = pyramid;
```

现在让我们将光标移动到我们想要的位置。

```
function pyramid() {  var canvas = document.getElementById(“canvas”);  var context = canvas.getContext(“2d”);  canvas.width = 400;  canvas.height = 400;      context.beginPath();  context.moveTo(200, 0);}
```

我希望我的金字塔占据尽可能多的空间，所以我从画布的最顶端开始，正好在 x 轴的中间。

现在我们可以开始画出我们的形状并填充它。

```
context.lineTo(0, 400);context.lineTo(400, 400);context.lineTo(200, 0);context.stroke();context.fillStyle = “orange”;context.fill();
```

搞定了。你现在应该在你的屏幕上有一个漂亮的橙色金字塔，因为你当然是光明会的一部分。不要撒谎！

这是你可以玩的成品:

## 表情符号！

现在你要的是:表情符号！

就像我们搭起帆布前一样。

```
function smileyFaceEmoji() {    var canvas = document.getElementById(“canvas”);    var context = canvas.getContext(“2d”);    canvas.width = 500;    canvas.height = 500;}
```

记得把 *onload* 改成这个功能。

```
window.onload = smileyFaceEmoji;
```

现在让我们画我们的脸。

```
context.beginPath();context.arc(250, 250, 100,0,Math.PI*2, true);context.stroke();
```

我在这里用*弧*函数做了一些改变。*圆弧*函数带相当多的参数:x 坐标，y 坐标，半径，起点弧度，终点弧度，是否顺时针绘制(我们说的是真)。与矩形从一个点开始移动到下一个点的方式相反，点(x = 250，y = 250)实际上是圆的中间，然后向外延伸 100 个像素。

很酷吧？！接下来是眼睛。

```
context.moveTo(235, 225);context.arc(225, 225, 10, 0, Math.PI*2, true);context.moveTo(285, 225);context.arc(275, 225, 10, 0, Math.PI*2, true);context.stroke();
```

然后是嘴。

```
context.moveTo(250, 275);context.arc(250, 275, 50, 0, Math.PI, false); // Why is this last value false? Why did you just use Math.PI?context.moveTo(250, 275);context.lineTo(200, 275);context.stroke();
```

这是成品的样子:

你做到了，你刚刚做了一个笑脸表情符号！天哪，我真为你骄傲！如果你想让你的画布技巧更上一层楼，试试下面的练习。

### 练习

1.  画个便便表情符号。
2.  用草书写下你名字的首字母。

## 概括起来

在本课中，您学习了函数:如何创建函数、执行函数以及使用函数来构建在画布上画线的小程序。我们了解到函数有多种形式，可以被赋予属性和方法。我希望你喜欢这一课，因为我的目的是向你展示函数的强大功能，而不是用术语让你陷入困境，而是使用视觉刺激来使它们变得生动！

如果你想看这节课的所有代码，去我的代码栏[这里](http://codepen.io/arecvlohe/pen/QNGjBr/)。