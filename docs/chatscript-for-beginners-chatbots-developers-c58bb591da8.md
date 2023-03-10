# 如何使用 ChatScript 构建您的第一个聊天机器人

> 原文：<https://www.freecodecamp.org/news/chatscript-for-beginners-chatbots-developers-c58bb591da8/>

乔治·罗比诺

# 如何使用 ChatScript 构建您的第一个聊天机器人

> 10–10–2018:文章更新为新的 [github repo url](https://github.com/ChatScript/ChatScript) 。

聊天机器人可以帮助你在 Facebook Messenger、Telegram Messenger、Slack 等聊天工具中完成工作。只要说一句话，你的聊天机器人就会部署你的最新版本，或者给你订一份披萨。

有一种特殊的工具可以用来构建聊天机器人，这种工具已经存在很长时间了。它叫做聊天脚本。和 Slack 一样，它最初只是视频游戏的一小部分。

早在 2009 年，[布鲁斯·威尔科克斯](http://brilligunderstanding.com/aboutus.html)是一名游戏开发者和人工智能研究员。他的妻子[苏·威尔科克斯](http://brilligunderstanding.com/aboutus.html)，想要为她的互动小说游戏塑造虚拟角色。他们一起构建了最终成为 ChatScript 的东西。

![sfq-G4yfCxunsEuCSJGq-e5TUIytuUOhDlv2](img/dd5d21359484615cfa02fb2ee9025f6c.png)

Sue Wilcox (source: [http://brilligunderstanding.com/aboutus.html](http://brilligunderstanding.com/aboutus.html))

这个自然语言处理引擎+对话流脚本平台帮助布鲁斯三次获得[罗布纳 AI 奖](http://www.loebner.net/Prizef/loebner-prize.html)。

![QmL7v89jgnPL3uP7-q5vigSEps4Y11hh5mah](img/b8c6d679ae1eeb555d5fe7c71c7b9022.png)

[Watch a talk by Bruce Wilcox](http://www.fundacionareces.tv/watch/50a63327c31997630a020000)

布鲁斯至今仍在开发和维护这个项目。它是用 C 和 C++写的，并且是开源的。事实上，6.8 版本在几周前刚刚推出。

ChatScript 是为数不多的开源聊天机器人 NLProc 引擎之一！

让我们深入了解聊天脚本的基础知识，并认识一个名叫哈里的聊天机器人。

### 安装聊天脚本

根据您使用的操作系统，其中一些步骤可能会有所不同。**我用的是 Linux** 。如果你不想看这篇文章，你实际上不必经历这些步骤。只是跟着读。

#### 步骤 1:在本地计算机上安装系统组件

首先克隆 ChatScript GitHub 存储库:

```
$ git clone https://github.com/ChatScript/ChatScript
```

这将创建一个 ChatScript 目录，其中包含以下子目录:

```
$ cd ChatScript/$ ls -d1 */
```

```
BINARIES/DICT/DOCUMENTATION/LINUX/LIVEDATA/LOEBNERVS2010/LOGS/MAC/RAWDATA/REGRESS/SERVER BATCH FILES/SRC/SUBLIME TEXT EDITOR/TMP/TOPIC/USERS/VERIFY/VS2010/VS2015/WEBINTERFACE/
```

*   文档包含[维基文档文件](https://github.com/ChatScript/ChatScript/tree/master/WIKI)。

> 顺便说一句，我个人致力于更新所有 markdown 格式的原始文档，以便在开发时可以在线和从命令行阅读。❤

*   RAWDATA 包含每个 bot 的子目录。默认情况下，平台附带一个名为 Harry 的默认 bot，它位于 RAWDATA/HARRY。

顺便说一下，请记住设置 LinuxChatScript64 可执行文件:

```
$ chmod +x ChatScript/BINARIES/LinuxChatScript64
```

> 注意:显然在上面我考虑的是 Linux 操作系统环境。
> 点击了解更多关于 Linux、MacOS 或 Windows 安装的信息[。](https://github.com/bwilcox-1234/ChatScript#getting-started)

#### 步骤 2:以独立模式运行 CS 引擎:

```
$ BINARIES/LinuxChatScript64 local
```

运行引擎将启动哈利，然后我们可以与他交谈:

```
ChatScript EVSERVER Version 6.8  64 bit LINUX compiled Aug 16 2016 17:35:43 host=localParams:   dict:720895 fact:800000 text:70000kb hash:50000          buffer:22x80kb cache:1x5000kb userfacts:100WordNet: dict=201350  fact=85710  stext=12703744 Jul26'16-21:58:46Build0:  dict=67726  fact=130289  stext=1145656 Jul31'16-13:45:49 0Build1:  dict=167  fact=15  stext=211304 Jul31'16-13:45:44 harryUsed 53MB: dict 269,244 (23693kb) hashdepth 18/1 fact 216,014 (8640kb) text 14183kb           buffer (1760kb) cache (5000kb) POS: 918 (29kb)Free 79MB: dict 451,651 hash 924 fact 583,986 text 55,816KB
```

```
Enter user name: giorgio
```

```
HARRY:  Welcome backgiorgio: > what is your name ?HARRY:  My name is Harry.giorgio: > who is Bruce Wilcox?HARRY:  Bruce Wilcox (born 1951) is an artificial intelligence programmer.giorgio: > ok, I have to goHARRY:  Indeed.giorgio: > :quitExiting ChatScript via Quit$
```

#### 步骤 3:编写并调试你的聊天机器人脚本

独立模式是一个非常好的 ChatScript 开发环境。它允许你运行交互式对话，然后使用**:命令**与他们互动。这是一组特殊的交互式命令工具，用于在开发和调试阶段测试和调试您的对话框。

以下是一些命令示例:

```
# recompile the bot Harry and reset the status of conversations with user giorgio
```

```
giorgio: &gt; :build Harry giorgio: > :reset
```

```
# request statistics about topic ~pets
```

```
giorgio: > :topicstats ~pets    ~pets     gambits 2 responders 2 rejoinders 5 empties 0Concepts 1860 Topics 1 rules 9 empties 0  gambits 2  responders 2 (?: 2 s: 0  u: 0) rejoinders 5
```

```
# conversation ...
```

```
giorgio: > do you like snakes?HARRY:  I love pythons except ^"Python" (the programming language)
```

```
# ask the reason why a rule fired
```

```
giorgio: > :why~pets.2.0.5.9.0  ?:  ( << you like snake >> ) I love pythons except Python ( the programming language ) via ~control.5.9.0  u:  ( ) $currenttopic = %topic ^if 00m( %response  0 ) 00I{ ^nofail ( TOPIC ^rejoinder ( ) ...
```

注意，您可以运行 **:commands** 来显示可用命令的完整列表。

主题包含在特定的文件中。例如， ***~*** *宠物*主题编码包含在 *pets.top* 文件中，看起来是这样的:

```
topic: ~pets (dog cat pet animal bird fish snake)
```

```
?: ( << you like snake >> ) I love pythons except ^"Python" (the programming language)
```

```
?: ( << you ~like ~animals >> )  I love all animals.
```

```
t: Do you have any pets? #! yes a: ( ~yes ) Great. You like animals.
```

```
#! no a: ( ~no ) You don’t like animals?
```

```
#! I have two parrots a: ( parrots ) Birds are nice.
```

```
#! I have a cat a: ( cat ) I prefer dogs
```

```
#! I have a canary a: ( [parrot bird canary finch swallow] ) Birds are nice.
```

```
t: I have a dog.
```

ChatScript 是一个基于规则的引擎，其中规则是由人类作者通过称为对话流脚本的过程在程序脚本中创建的。这些使用脚本元语言(简称为“脚本”)作为它们的源代码。

以下是 ChatScript 脚本文件的样子:

```
## file: food.top#
```

```
topic: ~food []
```

```
#! I like spinachss: ( I like spinach )    Are you a fan of the Popeye cartoons?
```

```
a: ( ~yes )        I used to watch him as a child. Did you lust after Olive Oyl?     b: ( ~no ) Me neither. She was too skinny.     b: ( ~yes ) You probably like skinny models.
```

```
a: ( ~no ) What cartoons do you watch?     b: ( none ) You lead a deprived life.     b: ( Mickey Mouse ) The Disney icon.
```

```
#! I often eat chickenu: ( ![ not never rarely ] I * ~ingest * ~meat )    You eat meat.
```

```
u: ( !~negativeWords I * ~like * ~meat ) You like meat.
```

```
?: (do you eat _ [ ham eggs bacon])    I eat ‘_0
```

```
?: (do you like _* or _*)    I don’t like ‘_0 so I guess that means I prefer ‘_1.
```

```
s: ( ~like ~fruit ![~animal _bear] )    Vegan, you too...
```

```
?: (do you eat _~meat)    No, I hate _0.
```

```
s: ( I eat _*1 >)   $food = ‘_0   I eat oysters.
```

您可以使用存储为普通文本文件的脚本来定义 bot 的对话流。这比其他聊天工具使用的方法简单得多，后者通常涉及基于浏览器的用户界面、JSON 或 XML。

将脚本写成文本文件可以让你完全控制你的对话流。您可以使用后端脚本和工具轻松处理和升级您的对话代码。

例如，您可以根据数据库中的记录自动更新 ChatScript 对话规则。

你甚至可以使用机器学习工具来挖掘对话日志。这将为你提供各种机会来改进你的对话流程。

但是这些是未来 ChatScript 文章的主题。我会让你自己去玩 ChatScript。

> **请贡献它的开源代码库，并在 [GitHub](https://github.com/ChatScript/ChatScript) 上开始！**？？？？？

[**ChatScript/ChatScript**](https://github.com/ChatScript/ChatScript)
[*通过在 GitHub 上创建一个帐户来为 ChatScript/ChatScript 开发做贡献。*github.com](https://github.com/ChatScript/ChatScript)

![sSgolG-hbTtiw1APxWTmh3CwIkfcWwaIPfBJ](img/e6a498f90f7d73c3d75461d4517a63df.png)

**Please tap or click “︎**❤” to help to promote this piece to others.