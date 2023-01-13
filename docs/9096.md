# 伪代码在寻找解决方案中的重要性

> 原文：<https://www.freecodecamp.org/news/the-importance-of-pseudo-code-in-searching-for-solutions-f6d5b5d77a83/>

作者戈登·达维德斯库

# 伪代码在寻找解决方案中的重要性

![oCSnIDTjcYSFkpr2UnJ4b82gpqJKrkTn4fs-](img/1d1dca42f9325c74e900b6398434bccc.png)

courtesy of [http://morguefile.com/creative/cheriedurbin](http://morguefile.com/creative/cheriedurbin)

所以你注册了免费代码营，开始像工业割草机一样一课一课地耕耘。一切都很顺利，直到你突然遇到一个大的代码挑战——然后你僵住了。

你不知道从哪里开始。挑战对你的要求太多了。你甚至不确定你是否真的知道如何做它要求你做的所有事情。

就拿[井字游戏挑战](https://www.freecodecamp.com/challenges/build-a-tic-tac-toe-game)来说吧。这是一个游戏，它必须是可玩的，它必须允许你选择一方。当然，它也必须是能赢的(和能输的)。

我认为很多参加自由代码营的人都会遇到这种情况，即使他们之前有学习编码的经验。在我短暂的计算机科学专业学习期间，这种情况在我身上发生过很多次。

总之很容易被淹没。在没有人在场的情况下，你可以问“那我该从哪里开始呢？”你可能会绝望，什么都不做。除了玩最新的*点击这个 cookie* 类的游戏，直到灵感出现。

#### 输入伪代码

与其什么都不做，不如试试用伪代码*。*它会帮你制作一张地图，告诉你从哪里开始，以及如何最好地进行下去。

伪代码是什么？简而言之，它是你用任何编程语言编写的不是真正的代码，但是如果任何人读了它，就会明白发生了什么。

问任何一个孩子井字游戏是怎么玩的，他们都能告诉你。(与帕特里克的游戏*井字游戏相反。只有一个带着五岁儿子看过几遍《海绵宝宝》的人才能解释这一点。*

你有你的冲浪板。一个玩家是 x。一个玩家是 o。他们中的一个先走。然后是另一个。每个人把他们的符号放在空的方格里。他们不断交替轮流，直到其中一人连续三次，或者棋盘已满，双方都没有赢。新的匹配。

现在，让我们看看伪代码是如何实现的。我会这样写:

```
Draw a board on the screen — three squares across by three squares down. If any of the squares are clicked before a new game is started, pop up a warning that their game has not started yet Button : new game.
```

```
When clicked, variable player = x or o depending on which they clicked.
```

```
On the click of a block:If there is an X or an O in the block, do nothing.If there is neither X nor O in the block, change the board space with player.  If the top row or the middle row or the bottom row or the first column or the middle column or either of the diagonals are all player piece --      Announce the player wins!     Ask if the player wants to play again!     If yes, start from the top!   If the player did not win and the board is not full, let the computer take its turn.      Computer turn:      From the first square to the last square, check for two player pieces in either the first, second, or last row or column or diagonal and when found, place a computer piece in the third unoccupied space.      If none of these are found:          From the first square to the last square, check each one to see if any is blank.          As soon as one is found blank, put the computer's piece there.          If the top row or the middle row or the bottom row or the first column or the middle column or either of the diagonals are all computer piece --      Announce the computer wins!     Ask if the player wants to play again!     If yes, start from the top!   If the computer did not win and the board is not full, start from the top with the player's turn.
```

```
If the board is full, game over! New round? Ask the player.
```

就是这样。这就是整个游戏，分为四个基本步骤。现在真正有趣的部分来了——研究。

既然你现在已经有了应用程序如何工作的地图，你可以看看任何一个步骤，做一些关于如何实现它的研究。

首先，你如何在 HTML 中画一个有可点击部分的板子？

显而易见，您可能应该在 HTML 中使用

。有人会说在 Javascript 中使用“onclick”。其他人会使用 jQuery。

计算机怎么知道对手的棋子有两个在一排？嗯，这难道不取决于你如何在视觉场景后面的板上存储 X 和 O 的值吗？

当然了。可以说，一旦你知道了你的应用程序的地图，这是你将要问自己(和我们的朋友*Google 教授)*的许多问题之一。

有许多方法可以为计算机做人工智能——你如何让计算机决定当轮到它的时候它将把它的棋子放在哪里？我选择让它在进攻前主要用于防守(因为它首先看是否能阻挡玩家)。

这只是解决这个问题的一种方法——问问你自己，我将如何选择放置我的作品的空间。

一旦你把所有这些都想通了，就只需要把你的想法从伪代码转换成实际代码了。将你的应用程序分解成可管理的片段后，你可以快速评估需要做什么。

快乐的伪代码(和真正的代码，一旦你记下来)。

看到下面的心脏了吗？点击一下！提前感谢。