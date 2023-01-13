# 哭算法泪

> 原文：<https://www.freecodecamp.org/news/bonfire-tears-free-code-camp-edition-d79bbfd3d945/>

蒂芙尼·怀特

# 哭算法泪

![0*reQ5pMmpwq1G06h-](img/3c6e0e5236d5ff2535cfacf5a05c452a.png)

> “笑和泪都是对挫折和疲惫的反应。我自己更喜欢笑，因为之后要做的清洁工作更少。”―库尔特·冯内古特

在每一个新程序员的生活中，当他们遇到一个障碍，一堵墙，一个理解和不理解手头材料之间的门槛时，都会有一个转折点。

我昨天达到了那个门槛。

还有前天。

回想起来，解决方案是如此简单。我有过几次正确的想法。我得到了鼓励，得到了解释，得到了指导，但就像他们的话只是从我的脑壳里弹了出来，而不是被吸收到我的灰质里。

算法挑战是:

> 检查一个字符串(第一个参数)是否以给定的目标字符串(第二个参数)结尾。

> 记得用读-搜-问如果卡住了。编写自己的代码。

> 以下是一些有用的链接:

> String.substr()

代码自由代码营让我开始了:

```
function end(str, target) { 
```

```
// “Never give up and good luck will find you.” 
```

```
// — Falcor 
```

```
return str; 
```

```
}
```

```
end(“Bastian”, “n”); 
```

#### 搞什么鬼？子字符串？

![0*k9MyKxq8P6tLWagt](img/500569888cf8be2fbdd799f92f076672.png)

> 你以前做过，现在也可以。看到积极的可能性。重新引导你挫折的实质能量，把它变成积极的、有效的、不可阻挡的决心。拉尔夫·马斯顿

通过查看失败的测试，我知道我的算法必须处理不同长度的字符串。但是我只对测试中的一个字符串进行了硬编码。

我如何为不同长度的字符串编码呢？我如何得到一个字符串的长度？。长度()对吗？是的。但是*如何*。我把它放在哪里？长度()？

我有这个代码:

```
function end(str, target) { 
```

```
 //”Never give up and good luck will find you.” 
```

```
 // — Falcor
```

```
 //’abcdefghijklmn’.substr(0, 3)
```

```
 // ‘abc’
```

```
 //”grab 3 characters starting with the character at address number 0" ​ 
```

```
 var isEqual = str.substr(6, 1) === target.substr(0, 1); 
```

```
 return isEqual;
```

```
} ​ end(“Bastian”, “n”);
```

我在自由代码营的一个帮助聊天室里发现，你可以通过使用一个负数来到达一个字符串的末尾。没必要一直弹出 Bastian 上 n 前面的所有字母。

但我继续为“Bastian”和“n”进行硬编码。

我需要一个更广泛的方法。

我试过了:

```
function end(str, target) {
```

```
​   var isEqual = str.substr(-1) === target.substr(-1); return isEqual;
```

```
} ​ end(“Bastian”, “n”);
```

但是我没有取得任何进展。除了一个测试之外，所有的测试都通过了，我仍然没有真正利用它。length()来解决字符串长度的差异。

所以我试着这样做:

```
function end(str, target) {
```

```
 var n = target.length;     var z = str.length;     var isEqual = str.substr(-1) === target.substr(-1); return isEqual;
```

```
} ​ end(“Bastian”, “n”);
```

同样的结果。我知道我需要。长度()在那里。但是之后去哪里呢？

#### 啊哈！

![0*bGUwwQpIJYjPywvs](img/1bf0370a13a5c1750113b531482e975c.png)

最后，我不得不被引导到答案。这个女人在英国，我很确定我让她保持清醒。但是我们一起想出了这个解决方案:

```
// You didn't think I'd give it away, did you?
```

最后我明白了。这花了一段时间，但当我们达成解决方案时，我觉得自己像个十足的白痴。我怎么会没有早点明白呢？

我哭了。我真的哭了。一部分是因为我已经很情绪化了。

另一部分是我不想用拳头砸我的 MacBook Pro 的屏幕。

字符串是字符。不是言语。我完全陷进去了。

算法确实撕。

*最初发表于匹兹堡的[代码新手。](http://helloburgh.me/2015/11/26/bonfire-tears-free-code-camp-edition/)*