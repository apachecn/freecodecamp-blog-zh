# 如何使用正则表达式匹配表情符号——不和谐表情符号正则表达式教程

> 原文：<https://www.freecodecamp.org/news/how-to-use-regex-to-match-emoji-including-discord-emotes/>

表情符号是表现象形文字的特殊 Unicode 字符。但是这些字符很难用正则表达式(RegEx)来识别。

我最近在开发一个不和谐机器人，它必须检测给定消息中的表情数量。今天我将与你分享我的过程，包括最终解决了我的问题的新的 JavaScript RegEx 特性。

## Unicode 表情符号的工作原理

Unicode 协会为每个表情符号定义了特定的字符代码。他们甚至保留了一张[有用的表情符号图](https://unicode.org/emoji/charts/full-emoji-list.html)作为参考。例如，`U+1F600`对应于😀表情符号。

一些表情符号由多个 Unicode 字符组成。这在国旗表情符号中最为常见，它由构成该国两个字母国家代码的“地区指标”组成。

这意味着美国国旗🇺🇸由两个 Unicode 字符`U+1F1FA`和`U+1F1F8`组成，分别对应于地区指示符`U`和`S`。

> 有趣的是，由操作系统决定**如何**渲染表情符号。例如，如果你在 Windows 上，你不会看到上面有一个标志。你会看到`US`。

## 什么是不和谐表情？

Discord 的众多功能之一是允许社区上传他们自己的自定义表情。这些表情由一个名称标识，并与语法`:emote_name:`一起使用。

然而，它们被客户端/API 识别的方式是不同的。每个表情都有一个唯一的 ID，它们在消息内容中以`<:emote_name:1234567890>`或`<a:emote_name:1234567890>`的形式发送给动画表情。

你可以通过在表情前加一个反斜杠`\`并发送它来看到这种不和谐。它会呈现类似这样的内容:

![A Discord message showing an emote's raw value `<:NaomiGrin:938275644092063784>`](img/fb3929ec7a9cf6154452a2fc50e5e392.png)

## 如何用正则表达式匹配表情符号和表情

我最初的方法有两个不同的正则表达式短语。

我在用`/(<a?)?:\w+:(\d{18}>)?/g`捕捉不和谐的表情。这个正则表达式成功地获得了不和谐的表情，这太棒了！

我将它与`/:[^:\s]*(?:::[^:\s]*)*:/g`配对，以匹配 Unicode 表情符号，它只能部分工作。这里的问题是，我看到一些表情被重复计算了两次——因为 Discord 正则表达式匹配了它们。其他的则完全被忽略了。

因此，由于正则表达式的存在，我试图让它变得更复杂。在匹配内置表情符号方面稍微成功一点，但仍然不够完美。这个正则表达式专门用来匹配 unicode 字符。

在最终放弃并做一些研究之前，我尝试了修剪空白、使用单词 boundary `\b`字符和一些其他的调整。

然后我发现 [Unicode 属性转义](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes)。这个正则表达式特性允许您向正则表达式添加`u`标志，解锁用`\p`字符表示的 Unicode 属性。

通过一些额外的研究，我能够找到[表情符号角色属性](https://unicode.org/reports/tr51/#Emoji_Properties)——确切地说，是`Extended_Pictograph`属性。这使我能够将正则表达式更新为最终的函数值:

```
/<a?:.+?:\d{18}>|\p{Extended_Pictographic}/gu
```

`\p{Extended_Pictographic}`属性似乎匹配 Unicode 表情以及字符修饰符(通常用于表情符号中的肤色)。

## 结论

这个正则表达式目前正在我的生产代码中运行，还没有出现任何问题。

希望这篇文章对你有所帮助。如果您对进一步探索 Unicode 属性转义感兴趣，Unicode Consortium 提供了可用值的完整列表。

编码快乐！