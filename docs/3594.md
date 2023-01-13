# 如何使用普通 JavaScript 构建钢琴键盘

> 原文：<https://www.freecodecamp.org/news/javascript-piano-keyboard/>

制作一个可弹奏的钢琴键盘是学习编程语言的一个好方法(除了充满乐趣之外)。本教程向您展示了如何使用普通的 JavaScript 编写代码，而不需要任何外部库或框架。

如果你想先看看最终产品，这是我做的 JavaScript 钢琴键盘。

本教程假设您对 JavaScript 有基本的了解，比如函数和事件处理，并且熟悉 HTML 和 CSS。否则，它完全是初学者友好的，面向那些想通过基于项目的学习来提高他们的 JavaScript 技能的人(或者只是想做一个很酷的项目！).

我们为这个项目制作的钢琴键盘是基于 Keith William Horwood 制作的[动态生成合成键盘](https://keithwhor.com/music/)。我们将把可用键的数量扩展到 4 个八度音阶，并设置新的键绑定。

虽然他的键盘可以演奏其他乐器的声音，但我们会保持简单，只坚持用钢琴。

我们将采取以下步骤来解决这个项目:

1.[获取工作文件](#step1)

2.[设置按键绑定](#step2)

3.[生成键盘](#step3)

4.[手柄按键按下](#step4)

我们开始吧！

## 1.获取工作文件

本教程将使用以下文件:

[audiosynth.js](https://keithwhor.github.io/audiosynth/)

[playKeyboard.js](https://github.com/1000mileworld/Piano-Keyboard/blob/master/playKeyboard.js)

如前所述，我们将基于 Keith 的钢琴键盘。自然，我们也会借用他的一些代码，这些代码他已经友好地在 audiosynth.js 上许可了。

我们将 audiosynth.js 合并到 playKeyboard.js(我对 Keith 的一些代码的修改版本)中，它处理我们所有的 JavaScript。本教程在接下来的章节中详细解释了这个文件中的代码如何创建一个完整的钢琴键盘。

我们保留文件 audiosynth.js 不变，因为它单独负责声音生成。

这个文件中的代码通过使用 Javascript 在用户按键时动态生成适当的声音，将这个钢琴键盘与网上找到的其他键盘区分开来。因此，代码不必加载任何外部音频文件。

Keith 已经在他的网站上解释了声音生成是如何工作的，所以我们在这里不再赘述。

简而言之，它涉及使用 JS 中的`Math.sin()`函数来创建正弦波形，并通过一些复杂的数学运算来转换它们，使它们听起来更像真实的乐器。

创建一个索引 HTML 文件，让我们链接到文件头中的 JS 文件:

```
<script src="audiosynth.js"></script>
<script src="playKeyboard.js"></script> 
```

在主体中，我们可以创建一个空的`<div>`元素作为我们的键盘“容器”:

```
<div id= “keyboard”></div>
```

我们给它一个 id 名，以便我们以后使用 JS 创建键盘时可以引用它。我们也可以通过在主体中调用 JS 代码来运行它:

```
<script type="text/javascript">playKeyboard()</script>
```

我们使用 playKeyboard.js 作为一个大函数。浏览器一到达那行代码，它就会运行，并在带有
`id = “keyboard”`的`<div>`元素中生成一个完全可用的键盘。

playKeyboard.js 的前几行设置了移动设备功能(可选)并创建了一个新的`AudioSynth()`对象。我们使用这个对象来调用我们之前链接的 audiosynth.js 的方法。我们在开始时使用这些方法中的一种来设置声音的音量。

在第 11 行，我们将中间 C 的位置设置为第四个八度。

## 2.设置键绑定

在我们生成键盘之前，我们应该设置我们的键绑定，因为它们决定了应该生成多少个键。

我原本想试着弹奏《爱丽丝》的开头音符，所以我选择了 4 个八度音程，总共 48 个黑白键。这几乎需要我的(PC)键盘上的每一个键，你可以随意包含更少的键。

一个警告:我没有最好的键绑定，所以当你实际尝试弹奏时，它们可能会感觉不直观。也许这就是试图创造一个 4 八度键盘的代价。

要设置按键绑定，首先创建一个对象，该对象将使用 keycode 作为其按键，要播放的音符作为其键值(从第 15 行开始):

```
var keyboard = {
	/* ~ */
	192: 'C,-2',
	/* 1 */
	49: 'C#,-2',
	/* 2 */
	50: 'D,-2',
	/* 3 */
	51: 'D#,-2',
    //...and the rest of the keys
} 
```

注释表示用户可以在计算机键盘上按下的键。如果用户按下波浪号键，则对应的键码是 192。您可以使用诸如 keycode.info 之类的工具获取密钥代码。

键值是要以“音符，八度音修饰符”的格式演奏和书写的音符，其中八度音修饰符表示相对于包含中间 C 的八度音的相对八度音位置。例如，“C，-2”是中间 C 下面 2 个八度音的 C 音符。

请注意，没有“平”键。每个音符都用一个“升”来表示。

为了让我们的钢琴键盘发挥作用，我们必须准备一个反向查找表，在那里我们交换`key: value`对，这样要演奏的音符成为键，键码成为值。

我们需要这样一个表，因为我们希望迭代音符来轻松地生成键盘。

现在事情可能变得棘手了:我们实际上需要两个反向查找表。

我们使用一个表来查找我们想要为播放音符所按的计算机键显示的标签(在第 164 行声明为`reverseLookupText`),并使用第二个表来查找实际被按下的键(在第 165 行声明为`reverseLookup`)。

精明的人可能意识到两个查找表都有 keycodes 作为值，那么它们之间有什么区别呢？

结果是(因为我不知道的原因)当你得到一个对应于一个键的键码，并试图对这个键码使用`String.fromCharCode()`方法时，你并不总是得到代表被按下的键的相同字符串。

例如，按下左括号会产生 keycode 219，但是当您实际尝试使用`String.fromCharCode(219)`将 keycode 转换回字符串时，它会返回“è”。要得到“[”，必须使用键码 91。我们从第 168 行开始替换不正确的代码。

获得正确的 keycode 最初需要一些反复试验，但是后来我意识到可以使用另一个函数(第 318 行的`getDispStr()`)来强制显示正确的字符串。

大多数按键都可以正常工作，但是您可以选择从较小的键盘开始，这样您就不必处理不正确的键码。

## 3.生成键盘

我们通过选择第 209 行带有`document.getElementById(‘keyboard’)`的`<div>`元素键盘容器来开始键盘生成过程。

在下一行中，我们声明了`selectSound`对象并将`value`属性设置为零，让 audioSynth.js 加载钢琴的声音配置文件。如果您想尝试其他乐器，您可能希望输入一个不同的值(可以是 0-3)。详见 audioSynth.js 第 233 行`Synth.loadSoundProfile`。

在带有`var notes`的第 216 行，我们从 audioSynth.js 中检索一个八度音阶(C，C#，D…B)的可用音符。

我们通过循环每个八度音阶，然后循环该八度音阶中的每个音符来生成键盘。对于每个音符，我们使用`document.createElement(‘div’)`创建一个`<div>`元素来表示适当的键。

为了区分我们是否需要创建一个黑键或白键，我们看一下音符名称的长度。添加尖号会使字符串的长度大于 1(例如 c#’)，其指示黑键，反之亦然。

对于每个键，我们可以根据键的位置设置宽度、高度和从左边的偏移量。我们还可以设置适当的类，以便以后与 CSS 一起使用。

接下来，我们用我们需要按下来播放其音符的计算机键来标记该键，并将其存储在另一个`<div>`元素中。这就是`reverseLookupText`派上用场的地方。在同一个`<div>`里面，我们也显示了音符名称。我们通过设置标签的 innerHTML 属性并将标签附加到键上来完成所有这些工作(第 240-242 行)。

```
label.innerHTML = '<b class="keyLabel">' + s + '</b>' + '<br /><br />' + n.substr(0,1) + 
'<span name="OCTAVE_LABEL" value="' + i + '">' + (__octave + parseInt(i)) + '</span>' + 
(n.substr(1,1)?n.substr(1,1):''); 
```

类似地，我们向键添加一个事件侦听器来处理鼠标点击(第 244 行):

```
thisKey.addEventListener(evtListener[0], (function(_temp) { return function() { fnPlayKeyboard({keyCode:_temp}); } })(reverseLookup[n + ',' + i]));
```

第一个参数`evtListener[0]`是在第 7 行很早之前声明的一个`mousedown`事件。第二个参数是返回函数的函数。我们需要`reverseLookup`来获取正确的键码，并将该值作为 parameter _temp 传递给内部函数。我们不需要 reverseLookup 来处理实际的`keydown`事件。

该代码是 ES2015 之前的版本(又名 ES6 ),更新后的版本有望更加清晰:

```
const keyCode = reverseLookup[n + ',' + i];
thisKey.addEventListener('mousedown', () => {
  fnPlayKeyboard({ keyCode });
}); 
```

在创建并添加了键盘上所有必要的键之后，我们将需要处理一个音符的实际演奏。

## 4.处理按键

无论用户点击按键还是通过使用第 260 行上的函数`fnPlayKeyboard`按下相应的计算机按键，我们都以相同的方式处理按键。唯一的区别是我们在`addEventListener`中用来检测按键的事件类型。

我们在第 206 行设置了一个名为`keysPressed`的数组来检测什么键被按下/点击了。为了简单起见，我们假设一个键被按下也可以包括它被点击。

我们可以把处理按键的过程分为 3 个步骤:给`keysPressed`添加按键的键码，播放合适的音符，从`keysPressed`移除键码。

添加键码的第一步很简单:

```
keysPressed.push(e.keyCode);
```

其中`e`是由`addEventListener`检测到的事件。

如果添加的键码是我们分配的键绑定之一，那么我们调用第 304 行的`fnPlayNote()`来播放与该键相关的音符。

在`fnPlayNote()`中，我们首先使用 audiosynth.js 的`generate()`方法为我们的笔记创建一个新的`Audio()`元素`container`。当音频加载后，我们就可以播放笔记了。

第 308-313 行是遗留代码，看起来它们可以被替换成`container.play()`，尽管我没有做任何广泛的测试来看有什么不同。

移除按键也非常简单，因为您可以使用第 298 行的`splice`方法从`keysPressed`数组中移除按键。更多详细信息，请参见名为`fnRemoveKeyBinding()`的函数。

我们唯一要注意的是当用户按住一个或多个键时。我们必须确保音符只在一个键被按住时播放一次(第 262-267 行):

```
var i = keysPressed.length;
while(i--) {
	if(keysPressed[i]==e.keyCode) {
		return false;	
    }
} 
```

返回`false`会阻止`fnPlayKeyboard()`的其余部分执行。

## 摘要

我们已经使用普通的 JavaScript 创建了一个全功能的钢琴键盘！

概括来说，我们采取了以下步骤:

1.  我们设置我们的索引 HTML 文件来加载适当的 JS 文件，并执行`<body>`中的
    `playKeyboard()`来生成并使键盘起作用。我们有一个带有`id= "keyboard"`的`<div>`元素，键盘将显示在页面上。

2.  在我们的 JavaScript 文件 playKeyboard.js 中，我们设置了键绑定，将键码作为键，将音符作为值。我们还创建了两个反向查找表，其中一个负责根据注释查找适当的键标签，另一个负责查找正确的键码。

3.  我们通过循环每个八度音域中的每个音符来动态生成键盘。每个键被创建为它自己的`<div>`元素。我们使用反向查找表来生成密钥标签和正确的密钥代码。然后`mousedown`上的一个事件监听器用它来调用`fnPlayKeyboard()`播放这个音符。
    `keydown`事件调用相同的函数，但不需要反向查找表来获取键码。

4.  我们用 3 个步骤来处理鼠标点击或计算机按键引起的按键:将按键的键码添加到一个数组中，播放适当的音符，并从该数组中删除键码。我们必须小心不要在用户持续按住一个键的时候重复弹奏一个音符(从开始)。

键盘现在功能齐全，但看起来可能有点迟钝。我会把 CSS 的部分留给你？

还是那句话，这里是我做的 [JavaScript 钢琴键盘](http://1000mileworld.com/Portfolio/Piano/keyboard.html)供参考。

如果你想了解更多关于 web 开发的知识，并查看一些其他优秀的项目，请访问我的博客，地址是 [1000 英里世界](https://www.1000mileworld.com/)。

感谢阅读和快乐编码！