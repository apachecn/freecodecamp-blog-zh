# JavaScript 的“这”是通过组建一个高中乐队来解释的

> 原文：<https://www.freecodecamp.org/news/javascripts-this-explained-by-starting-a-high-school-band-e072c8035eae/>

凯文·科诺年科

![1*sQcxkf5QH-TA29tcMGDHGA](img/33665ed6aa95a0798d9072a1f4972f2b.png)

# JavaScript 的“这”是通过组建一个高中乐队来解释的

如果你曾经参加过乐队，有朋友组建过乐队，或者看过一部关于组建乐队的 80 年代老土电影，那么你就能理解 JavaScript 中“this”的概念。

当您阅读一些 JavaScript 时，如果您遇到了这个关键字，那么您需要采取的步骤似乎是显而易见的。

你可能在想，“我只需要找到包含*这个*的函数，然后我就知道它指的是什么了！”

```
let band= {  name: "myBand",  playGig:function() {    console.log("Please welcome to the stage" + this.name);  }}
```

例如，在上面的例子中， *this.name* 指的是“myBand”这个名字。这似乎很容易！

但是，随着您学习更多的 JavaScript 概念，如闭包和回调，您将很快发现 *this* 并不像您预期的那样运行。

所以，我想创建一个可视化的解释来说明这个在 JavaScript 中是如何工作的。场景是这样的:你回到了高中，和你的朋友们组建了一个乐队(或者你现在还在上高中？)

*   你的乐队有四名成员
*   你参加三种类型的演出:你在酒吧、学校比赛和镇上的公共活动中演出。
*   你的团队可以演奏各种类型的音乐，所以你尽量选择适合听众的歌曲。例如，你不希望在家庭友好活动中出现脏话或性暗示。

正如你将很快看到的，你需要理解的最大概念是*这个*是**执行上下文。**这就是决定*这个*的价值。

在使用本教程之前，你需要了解[对象](https://blog.codeanalogies.com/2017/04/29/javascript-arrays-and-objects-are-just-like-books-and-newspapers/)和[变量](https://blog.codeanalogies.com/2017/12/20/a-visual-guide-to-understanding-the-sign-in-javascript/)。如果你需要复习，可以看看我关于这些主题的教程。

如果您对本教程的技术版本感兴趣，请查看来自 JavaScriptIsSexy 的[指南。](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)

![0*vkmneQ0bjkfUKE4A](img/77d24988eeebe859b345e329258087bb.png)

### 全局执行上下文

假设你的乐队需要在当地公园举办一场适合家庭的演出，作为当地集市的一部分。你需要选择合适的音乐类型，既能让父母开心，又不会冒犯任何人。

比方说，你选择演奏比利·周(著名的美国艺术家)的一些歌曲，尽管这不是你最喜欢的，但你知道这是你需要做的来获得报酬。

这是代码中的样子。

```
//The songs you will play var artist= "Billy Joel"; 
```

```
function playGig(){   //instruments that your band will use   let instruments= ["piano", "microphone", "acousticGuitar", "harmonica"]; 
```

```
 console.log("We are going to be playing music from " + this.artist + "tonight!"); } 
```

```
playGig();
```

![0*nDmTsWSyhway3yRK](img/90413201643419558a1ec904b43c0fab.png)

在上面的例子中，我们有一个 artist **变量**,它指示我们将播放什么类型的音乐。我们有一个装满了*乐器*的数组，它们将被用来在 playGig **函数**中播放音乐。

在最后一行，我们调用 playGig 函数。那么在这种情况下，这个艺术家是什么呢？

首先，我们必须确定这个函数的**执行上下文**。执行上下文由调用函数的**对象决定**。

在这种情况下，没有列出任何对象，这意味着该函数是在*窗口*对象上调用的。也可以这样称呼:

```
window.playGig(); "We are going to be playing music from Billy Joel tonight!"
```

这是全局**执行上下文**。该函数在全局对象层次被调用，*窗口*。此外，变量 *artist* 作为*窗口*对象的属性可用([参见 JavaScript 规范](https://stackoverflow.com/questions/19855823/are-global-variables-just-properties-on-the-window-object)中的注释)。

所以，在上面代码片段的第一行，我们也在说:

```
//old version- let artist = "Billy Joel"; this.artist="Billy Joel";
```

![0*pR8LAtC76p1kvE5s](img/312ab7f12af78116b36f442b725041b3.png)

你的乐队通过演奏吸引每个人的音乐，在全球范围内进行演出(除非那里有任何讨厌比利·周的人)。

![0*19QcrhPgf38OU00_](img/5f11f6f7b63a261adb1bbee51cf19d4a.png)

### 对象级执行上下文

假设你的乐队在当地一家酒吧演出。这太棒了！现在，你不需要演奏让镇上所有人都满意的音乐。你只需要播放人们可以跟着跳舞的音乐。

![0*_djQrnI4NQ5nDzD2](img/9bf5e796fce5d8bd92e9c7c3eec71c84.png)

假设你选择了酷玩乐队，因为他们最近的大多数歌曲都是流行音乐。这场演出需要钢琴、麦克风、架子鼓和吉他。

让我们创建一个酒吧对象，其模式与我们为公园演出创建的模式相同。

```
//The songs you will play in the public park/fair var artist= "Billy Joel"; 
```

```
function playGig(){   //instruments that your band will use   let instruments= ["piano", "microphone", "acousticGuitar", "harmonica"];   console.log("We are going to be playing music from " + this.artist + "tonight!"); } 
```

```
//NEW PART let bar = {  artist:"coldplay",  playGig: function(){     //instruments that your band will use     let instruments= ["piano", "microphone", "guitar", "drumset"];         console.log("We are going to be playing music from " + this.artist + "tonight!");    } }
```

下面是上面代码的示意图:

![0*hfc66VpcvHZZueNz](img/c94cbf3082823618ce7cf2aec6d8a73c.png)

所以，让我们说，我们想写代码，让酒吧的演出开始。我们需要观察我们的**执行上下文**，在本例中是*栏*对象。这看起来是这样的:

```
bar.playGig(); //"We are going to be playing music from coldplay tonight!"
```

而且，我们仍然可以在全局级别上执行 playGig 函数——但是我们将得到不同的输出。这是个好消息，因为我们不想在错误的地点玩比利·周或酷玩乐队…

```
playGig(); //"We are going to be playing music from Billy Joel tonight!"
```

到目前为止，这还算简单。每当我们调用一个函数时，提供**执行上下文**的对象非常简单。但是随着我们变得越来越复杂，这种情况将会改变。

![0*F1hheXRD2SSTmszz](img/3cdb56ee0b398aaa96b906f25cbab4ab.png)

### 使用 jQuery 更改执行上下文

这是上世纪 80 年代以来每一部电影都报道过的大事件:乐队之战！是的，你们高中的每个乐队都要参加比赛，看谁是最好的。

你将演奏一些来自 AC/DC 的歌曲，这是这个星球上最酷的乐队。但是为了做到这一点，你需要一个不同于以前的乐器组合:

*   麦克风
*   电吉他
*   低音吉他
*   一副鼓

姑且称之为战斗**对象**。下面是它在代码中的样子。

```
let battle = {  artist:"acdc",  playGig: function(){     //instruments that your band will use     let instruments= ["microphone", "electricguitar", "bass", "drumset"]; 
```

```
 console.log("We are going to be playing music from " + this.artist + "tonight!");   } }
```

由于这是一年一度的活动，我们将使用来自 jQuery 的 click **事件**来开始您的节目。看起来是这样的:

```
$('#annualBattle').click(battle.playGig);
```

但是如果你真的运行这段代码…它就不会工作。你的乐队会忘记歌词和音符，然后慢慢走下舞台。

为了找出原因，让我们回到执行上下文。我们引用了一个名为 *#annualBattle* 的 DOM 元素，所以让我们看看它在*窗口*对象中的位置。

由于 *#annualBattle* 是 DOM 中的一个元素，所以它是*窗口*对象中*文档*对象的一部分。它没有任何名为 *artist* 的属性。所以如果你运行代码，你会得到:

```
$('#annualBattle').click(battle.playGig); //"We are going to be playing music from undefined tonight!"
```

在这种情况下，**执行上下文**是 DOM 中的一个元素。这就是 click()方法的起源，它使用 playGig 函数作为**回调**。所以，*这个*将以一个未定义的值结束。

在我们的类比中，这意味着你的乐队带着他们所有的乐器出现在比赛现场，就位演奏，然后盯着人群，好像要告诉他们该做什么。这意味着你已经忘记了当初你为什么在那里的背景。

为了解决这个问题，我们需要使用 [bind()方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)来确保 playGig 方法仍然引用 *battle* 对象，即使我们从不同对象的上下文中调用它！看起来是这样的:

```
$('#annualBattle').click(battle.playGig.bind(battle)); //"We are going to be playing music from acdc tonight!"
```

现在，我们得到了正确的输出，尽管上下文是一个 DOM 元素。

### 脱离上下文提取功能

比方说，我们想要编写允许我们为乐队之战活动进行练习的代码。我们将创建一个名为*练习*的独立变量，并从*战斗*对象中分配 playGig **方法**。

```
var artist= "Billy Joel"; 
```

```
function playGig(){  //instruments that your band will use   let instruments= ["piano", "microphone", "acousticGuitar", "harmonica"]; 
```

```
 console.log("We are going to be playing music from " + this.artist + "tonight!"); } 
```

```
let battle = {  artist:"acdc",   playGig: function(){     //instruments that your band will use     let instruments= ["microphone", "electricguitar", "bass", "drumset"]; 
```

```
 console.log("We are going to be playing music from " + this.artist + "tonight!");   } } 
```

```
let practice = battle.playGig; //run a practice practice();
```

所以你可能想知道:最后一行的执行上下文是什么？

嗯，这将遇到与上一个例子类似的问题。当我们创建 *practice* 变量时，我们现在将 playGig 方法的一个实例存储在**全局上下文**中！它不再处于战斗对象的上下文中。

![0*nsh11uuf9M7miGMI](img/3b11eb93c6bfa748a8b50848e399a22f.png)

如果我们运行上面的代码，我们会得到:

```
practice(); 
```

```
//"We are going to be playing music from Billy Joel tonight!"
```

不是我们想要的。我们试着练习 AC/DC，而不是练习比利·周。呀。

相反，我们需要像上面一样使用 bind()方法。这将允许我们绑定*战斗*对象的上下文。

```
let practice = battle.playGig.bind(battle); 
```

```
practice(); //"We are going to be playing music from AC/DC tonight!"
```

### 匿名函数如何影响上下文

假设您的演出即将结束，您想为乐队中的每个人大喊一声，这样观众就可以为每个人鼓掌了。

为此，我们将使用 forEach()方法来遍历 *instruments* 属性的值中的每个元素。(一会儿你就会明白为什么我们把它从变量变成了属性)。它看起来会像这样:

```
let battle = {  artist:"acdc",
```

```
 //instruments that your band will use  instruments: ["microphone", "electricguitar", "bass", "drumset"],
```

```
 shoutout: function(){ 
```

```
 this.instruments.forEach(function(instrument){      console.log("Give a shoutout to my friend for covering the " + instrument + " from " + this.artist + "!");     }   } } 
```

```
battle.shoutout();
```

但是同样，如果我们运行这段代码，它也不会工作。

这一切都围绕着我们声明一个匿名函数来使用在 *instruments* 中的每个元素上。当这个函数被执行时，第一个 *this* 将保留正确的上下文: *battle* 对象。

但是，当我们到达 console.log 语句中的 *this.artist* 时，我们将得到…“比利·周”。这是因为匿名函数在 forEach()方法中用作回调函数。它将范围重置为全局范围。

![1*BACZfoonzMUUwpfNzi9jIw](img/7ca393598bca5066f0f9f57f397aef45.png)

在这种情况下，这意味着我们将声称在最后扮演比利·周…哦！

但我们能做的是。我们可以创建一个名为*的新变量*来在正确的上下文中存储*这个*。然后，当我们引用我们在这个特定演出中演奏的艺术家时，我们可以引用存储的上下文，而不是被迫返回到全球上下文。

```
let battle = {  artist:"acdc",  //instruments that your band will use   instruments: ["microphone", "electricguitar", "bass", "drumset"],   shoutout: function(){
```

```
 //store context of this     let that = this;
```

```
 this.instruments.forEach(function(instrument){      console.log("Give a shoutout to my friend for covering the " + instrument + " from " + that.artist + "!");    }   } }
```

```
battle.shoutout();
```

### 获取最新教程

你喜欢这个教程吗？如果你有，请鼓掌或在这里注册 CodeAnalogies 的最新视觉教程: